---
date: 2020-05-14T18:08:56-07:00
title: ABC记法
---

<link href="/css/abcjs-midi.css" type="text/css" rel="stylesheet" />
<script src="/scripts/abcjs_plugin-midi_5.11.0-min.js"></script>
<script type="text/javascript">
	window.ABCJS.plugin.render_options = {
		responsive: "resize",
		inlineControls: {
			loopToggle: true,
		},
	};
</script>

[ABC](https://en.wikipedia.org/wiki/ABC_notation)是一种文本记谱法，
既方便人工录入，也方便计算机读取。

<!--more-->

一些基本的格式：
* 开始的数行， 每行以`字母:`开头，描述该谱的一个属性，例如，
`M:`指定节拍，`L:`指定记录单位，`K:`指定调性。
* 在`K:`之后的各行记录各音符。
* 字母CDEFGHAB分别表示音C4-B4，而cdefghab表示音C5-B5。
* 在字母后加`'`（或`,`）可以提升（或降低）一个八度。
* 在字母后加`x/y`将音符长度调整为原来的`x/y`。默认x=1, y=2。例如，当`L: 1/4`时，`C2`得到二分音符，`C4`得到全音符，而`C/2`得到八分音符。
* `>`表示附点，它左边的音延长一半，右边的音缩短一半。
* `()`添加连音线。
* `|` 表示小节线。

详见[ABC标准](http://abcnotation.com/wiki/abc:standard:v2.1#repeat_bar_symbols)。

有现有的计算机软件可以对ABC进行排版，或者转换为音频。下面的例子使用了[abcjs](https://abcjs.net/)。
```
X: 1
Q: 120
M: 4/4
L: 1/4
K: G
   B>B d>e | d4       | B>A G>A | B4     |
   d>d g>g | (f3e)    | e>B e>d | d4     |
   B>B d g | f>f   e2 | B>B B G | B>A A2 |
   d>d e f | g>a   b2 | a>e f d | g4     |
```
