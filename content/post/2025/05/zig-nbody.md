---
date: 2025-05-16T20:40:39-07:00
title: 用Zig语言模拟n体问题
---

[N体问题](https://zh.wikipedia.org/zh-hans/N%E4%BD%93%E9%97%AE%E9%A2%98)是指找出已知初始位置、速度和质量的多个物体在经典力学情况下的后续运动。
这个问题虽然在数学上还没有满意的解答(N≥3时)，却比较容易用数值计算进行模拟。简单来说就是把时间离散化，在每一时刻，根据物体的受力和速度来确定速度和位置的变化量。

这个问题常用来做[基准测试](https://programming-language-benchmarks.vercel.app/problem/nbody)，因为它涉及大量浮点数运算和大量的循环步骤（模拟长时间的运动）。

想起这个测试是因为我正在看[Zig语言的文档](https://ziglang.org/documentation/master/)，发现它原生支持向量运算。例如，我可以定义含有3个分量的向量：
```zig
const Vec3 = @Vector(3, f64);
```
并对它们进行四则运算。

<!--more-->

数乘、点积和模运算可以简单地表示为：
```zig
fn scalarP(s: f64, v: Vec3) Vec3 {
    return @as(Vec3, @splat(s)) * v;
}

fn dotP(u: Vec3, v: Vec3) f64 {
    return @reduce(.Add, u * v);
}

fn norm(v: Vec3) f64 {
    return @sqrt(dotP(v, v));
}
```
这里的`*`直接在3个数上同时进行运算。在支持的平台上，编译器直接生成一条SIMD指令。这就比做3次运算快了。

接下来定义物体，它的动量、动能和两物体的势能都可以用向量运算简洁地表示：
```zig
const Body = struct {
    name: []const u8,
    pos: Vec3,
    veloc: Vec3,
    mass: f64,

    fn momemtum(self: Body) Vec3 {
        return scalarP(self.mass, self.veloc);
    }
    fn kinetic(self: Body) f64 {
        return self.mass * dotP(self.veloc, self.veloc) * 0.5;
    }
    fn potential(b1: Body, b2: Body) f64 {
        return -(b1.mass * b2.mass / norm(b1.pos - b2.pos));
    }
};
```

整个系统的能量是动能和势能之和：
```zig
fn calcSystemEnergy(system: []const Body) f64 {
    var e: f64 = 0;
    for (system, 0..) |b1, i| {
        for (system[0..i]) |b2| {
            e += Body.potential(b1, b2);
        }
        e += b1.kinetic();
    }
    return e;
}
```

模拟的关键在于算出一个时间单位(dt)内，各物体的运动变化：
```zig
fn advanceSystem(system: []Body, dt: f64) void {
    for (system, 0..) |*b1, i| {
        for (system[0..i]) |*b2| {
            const d = b1.pos - b2.pos;
            const dd = dotP(d, d);
            const mag = dt / (dd * @sqrt(dd));
            b1.veloc -= scalarP(b2.mass * mag, d);
            b2.veloc += scalarP(b1.mass * mag, d);
        }
    }
    for (system) |*b| {
        b.pos += scalarP(dt, b.veloc);
    }
}
```
关于这里的计算：两物体 \\(m_1\\), \\(m_2\\) 之间的位矢为 \\(\mathbf r_{12}\\) （由1指向2），则1受到2的万有引力为
\\[\mathbf{F}\_{12} = G\frac{m_1 m_2}{r^2}\hat{\mathbf{r}}\_{12} = G\frac{m_1 m_2}{r^3}\mathbf{r}\_{12},\\]
根据牛顿第二定律，加速度
\\[\mathbf a_{12} = \frac{Gm_2}{r^3}\mathbf r\\]
选取合适的单位制可使\\(G=1\\)，从而省去一次乘法计算。

[这个网站](https://programming-language-benchmarks.vercel.app/problem/nbody)有一组模拟数据（太阳和若干行星）。用这组数据按照dt=0.01，跑五千万次，比较前后的系统能量：
```
-0.169075164
-0.169059907
```
发现能量守恒并不成立😄；其实近似是成立的，毕竟用离散时间进行模拟会带来误差。

在我的笔记本电脑上（Intel Core Ultra 7），跑五千万次用时1.585秒；而[标准C语言的实现](https://github.com/hanabi1224/Programming-Language-Benchmarks/blob/main/bench/algorithm/nbody/2.c)[^1]用时1.712秒。可见Zig的向量运算是相当高效的。

[^1]: clang 20.1.3, `-O3 -march=native -flto`，编译器也会生成优化的SIMD指令。
