---
date: 2021-10-03T16:21:53-07:00
title: 斑马问题的求解
---

[斑马问题](https://en.wikipedia.org/wiki/Zebra_Puzzle)是一个著名的逻辑推理题。流传甚广的“爱因斯坦谜题”的形式与之类似。据维基百科考证，出处为1962年的《生活》杂志。题目内容如下：

1. 有五栋房子。
2. 英国人住在红房子里。
3. 西班牙人养狗。
4. 绿房子里的人喝咖啡。
5. 乌克兰人喝茶。
6. 绿房子紧挨在象牙色房子的右边。
7. 抽Old Gold牌香烟的人养蜗牛。
8. 黄房子里的人抽Kools牌香烟。
9. 中间的房子里的人喝牛奶。
10. 挪威人住在第一间房子。
11. 抽Chesterfields牌香烟的人住在养狐狸的人的隔壁。
12. 抽Kools牌香烟的人住在养马的人的隔壁。
13. 抽Lucky Strike牌香烟的人喝橙汁。
14. 日本人抽Parliaments牌香烟。
15. 挪威人住在蓝色房子的隔壁。

问题是：谁喝水？谁养斑马？

<!--more-->

原问题的澄清内容包括：每栋房子的颜色、主人国籍、所喝饮料、所抽香烟的牌子、所养宠物都不相同。

题干中未明确说明，但一般默认的隐含条件是：

* “第一间”指左起第一间。
* 有人喝水以及有人养斑马。

此类问题的一般解法是，画一个表格，每行代表房子的一个属性，每列代表一栋房子。对已知条件进行推理，可以在表中填上合适的取值。当没有更多单元格可以得到唯一取值时，则需要使用回溯法：选择一种可能的取值，并继续推理。如果推理出了矛盾，则回溯到上一个做出选择的状态。

通过推理可以得到唯一解：

|          | 1 | 2 | 3 | 4 | 5 |
|----------|---|---|---|---|---|
| **颜色** | 黄 | 蓝 | 红 | 象牙 | 绿 |
| **主人** | 挪威人 | 乌克兰人 | 英国人 | 西班牙人 | 日本人 |
| **饮料** | 水 | 茶 | 牛奶 | 橙汁 | 咖啡 |
| **宠物** | 狐狸 | 马 | 蜗牛 | 狗 | 斑马 |
| **香烟** | Kools | Chesterfields | Old Gold | Lucky Strike | Parliaments |
---

对于计算机而言，推理有时不依赖于对逻辑演绎。穷举所有可能的解，一一验证是否符合给出的约束条件，并给出答案，也算是一种“推理”。

表格中的每一行都是该行所有可能的取值的全排列。5种取值的全排列共有\\(5! = 120\\)种。总共有5行，因此搜索空间的大小为\\(120^5 = 24883200000\\)。这个数字对于现在的计算机而言，并不算大，一天之内足以跑完。

优化的办法是“剪枝”：在枚举每个全排列的时候，就检查是否满足约束条件。如果不满足，则不需要继续枚举其他的全排列。经过合适的剪枝，搜索时间大幅减少（特别是对于此类约束条件较多，可行解唯一的问题）。

怎么穷举？对于命令式的编程范式，5种不同的属性，需要5层嵌套循环。每层循环枚举一行的全排列，最后检查约束条件，并留下可行解。枚举全排列是一个经典的算法问题，这里不再讨论。

```python
for 颜色 in permutations('红', '绿', '象牙', '黄', '蓝'):
   # 剪枝
   for 主人 in permutations('英国人', '西班牙人', '乌克兰人', '挪威人', '日本人'):
      # 剪枝
      for 饮料 in permutations('咖啡', '茶', '牛奶', '橙汁', '水'):
          # 剪枝
          for 宠物 in permutations('狗', '蜗牛', '狐狸', '马', '斑马'):
              # 剪枝
              for 香烟 in permutations('Old Gold', 'Kools', 'Chesterfield', 'Lucky Strike', 'Parliaments'):
                  if 满足约束条件(颜色, 主人, 饮料, 宠物, 香烟):
                      输出可行解(...)
```

调整循环顺序不影响结果，但是影响剪枝条件。合适的循环顺序可以提升剪枝效果。枚举全排列的过程中也可以进行剪枝。

这就是计算机的“推理”方法。

命令式的编程，优点是容易和算法建立直接对应，缺点是过分拘泥于细节(把上述代码中的约束条件补全后即可看出）。与之对应，可以采用声明式编程，即代码本身更注重于约束条件本身的描述，而不是求解过程的描述。

下面是用[Picolisp](https://picolisp.com)求解的代码。Picolisp是一个LISP语言的实现，它附带许多实用功能，其中之一为Pilog，即Picolisp实现的[Prolog](https://en.wikipedia.org/wiki/Prolog)，一种声明式的逻辑编程语言。在Prolog中，代码基本上就是题干的逐行翻译。

```plain
(be constraints (@House @Person @Drink @Pet @Cigarettes)
   (permute (red green ivory yellow blue) @House)  #1; colors are implied
   (left-of @House ivory @House green)  #6

   (permute (Englishman Spaniard Ukrainian Norwegian Japanese) @Person)  # implied
   (equal @Person (Norwegian @ @ @ @))  #10
   (related @Person Englishman @House red)  #2
   (next-to @Person Norwegian @House blue)  #15

   (permute (coffee tea milk juice water) @Drink)  # implied
   (equal @Drink (@ @ milk @ @))  #9
   (related @Drink coffee @House green)  #4
   (related @Person Ukrainian @Drink tea)  #5

   (permute (OldGold Kools Chesterfields LuckyStrike Parliaments) @Cigarettes)  # implied
   (related @Cigarettes Kools @House yellow)  #8
   (related @Cigarettes LuckyStrike @Drink juice)  #13
   (related @Person Japanese @Cigarettes Parliaments)  #14

   (permute (dog snails fox horse zebra) @Pet)  # implied
   (related @Person Spaniard @Pet dog)  #3
   (related @Cigarettes OldGold @Pet snails)  #7
   (next-to @Cigarettes Chesterfields @Pet fox)  #11
   (next-to @Cigarettes Kools @Pet horse) ) #12
```

这里 `permute` 和 `equal` 是Pilog自带的关系。

- `permute (1 2 3 4 5) @X` 表示 `@X` 是 `(1 2 3 4 5)` 的一个全排列。
- `equal` 表示相等。 `equal @Person (Norwegian @ @ @ @ @)` 表示 `@Person` 的第一个元素是 `Norwegian`， 其他元素为任意取值（`@`）。

`related`、`left-of` 和 `next-to` 则需要自己给出定义：

```plain
# @A and @B are at the same location
(be related ((@A . @) @A (@B . @) @B))
(be related ((@ . @X) @A (@ . @Y) @B) (related @X @A @Y @B) )

# @A is left of @B
(be left-of (@X @A (@ . @Y) @B) (related @X @A @Y @B))

# @A is left/right of @B
(be next-to (@X @A @Y @B) (left-of @X @A @Y @B))
(be next-to (@X @A @Y @B) (left-of @Y @B @X @A))
```

这些都是利用递归关系表达的非常抽象的定义：

1. 如果 A 是 X 的第一个，B 是 Y 的第一个，那么 A 和 B 就在同一位置 (`related`)。
2. 否则， 用 X 和 Y 分别去掉第一个剩下的列表检查是否满足 `related` 关系。
3. 把 Y 去掉第一个，如果满足 `related` 关系，则 A 在 B 的左边。
3. A 在 B 左边或者 B 在 A 左边， 则 A 和 B 有相邻 (`next-to`) 关系。

这样便足够 Pilog 进行求解：

```plain
: (? (constraints @House @Person @Drink @Pet @Cigarettes)
     (related @Person @ZebraOwner @Pet zebra)
     (related @Person @WaterDrinker @Drink water) )
 @House=(yellow blue red ivory green)
 @Person=(Norwegian Ukrainian Englishman Spaniard Japanese)
 @Drink=(water tea milk juice coffee)
 @Pet=(fox horse snails dog zebra)
 @Cigarettes=(Kools Chesterfields OldGold LuckyStrike Parliaments)
 @ZebraOwner=Japanese
 @WaterDrinker=Norwegian
-> NIL
```

尽管Pilog可以自己进行求解，但其求解的过程仍然是非常确定的：Pilog按照代码中给定的顺序做代换。因此，约束条件的顺序也影响了运行时间。这个程序在我的机器上只须不到0.5秒即可运行结束：但如果调换宠物和香烟的顺序，则运行时间将增加到3秒以上。

等价的 Prolog 语法表述为如下，可以在[SWISH](https://swish.swi-prolog.org)上运行。

```prolog
constraints(House, Person, Drink, Pet, Cigarettes) :-
   permutation([red, green, ivory, yellow, blue], House),  %1; colors are implied
   left_of(House, ivory, House, green),  %6

   permutation(['Englishman', 'Spaniard', 'Ukrainian', 'Norwegian', 'Japanese'], Person),  % implied
   Person = ['Norwegian', _, _, _, _],  %10
   related(Person, 'Englishman', House, red),  %2
   next_to(Person, 'Norwegian', House, blue), %15

   permutation([coffee, tea, milk, juice, water], Drink),  % implied
   Drink = [_, _, milk, _, _],  %9
   related(Drink, coffee, House, green),  %4
   related(Person, 'Ukrainian', Drink, tea),  %5

   permutation(['OldGold', 'Kools', 'Chesterfields', 'LuckyStrike', 'Parliaments'], Cigarettes),  % implied
   related(Cigarettes, 'Kools', House, yellow),  %8
   related(Cigarettes, 'LuckyStrike', Drink, juice),  %13
   related(Person, 'Japanese', Cigarettes, 'Parliaments'),  %14

   permutation([dog, snails, fox, horse, zebra], Pet),  % implied
   related(Person, 'Spaniard', Pet, dog),  %3
   related(Cigarettes, 'OldGold', Pet, snails),  %7
   next_to(Cigarettes, 'Chesterfields', Pet, fox),  %11
   next_to(Cigarettes, 'Kools', Pet, horse). %12


related([A|_], A, [B|_], B).
related([_|X], A, [_|Y], B) :- related(X, A, Y, B).

left_of(X, A, [_|Y], B) :- related(X, A, Y, B).

next_to(X, A, Y, B) :- left_of(X, A, Y, B).
next_to(X, A, Y, B) :- left_of(Y, B, X, A).
```
