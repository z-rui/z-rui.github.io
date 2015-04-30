---
date: 2015-04-30T10:07:00+08:00
title: "SQLite3类型系统"
---

SQLite3非常[与众不同](https://www.sqlite.org/different.html)。它的类型系统也不同于大部分的数据库管理系统(DBMS)。

通常，DBMS使用静态类型(static typing)，即数据表的每一列中只能存放同一种类型的数据，这个类型在创建表的时候就已经确定。而SQLite3使用称为manifest typing的类型系统，它允许每一列中存放不同类型的元素，实际上很像动态类型。Manifest typing的准确译法我不知道。按照字面意思可以翻译成“显式类型”，可是这又无法体现它动态类型的特点。

注：有的教材把行称为“记录”，把列称为“字段”。我认为这些名字并不形象，对理解数据表帮助不大。

首先，数据表的每一列并没有严格的数据类型，只有具体的数据有类型。SQLite3的数据类型有5种，如下面的例子所示。

```
SELECT
  TYPEOF(NULL),
  TYPEOF(-9223372036854775808),
  TYPEOF(1e-5),
  TYPEOF('hello, world'),
  TYPEOF(X'DEADBEEF');
--> null|integer|real|text|blob
```

虽然和静态类型有很大的不同，但是实际使用的时候，SQLite3和使用静态类型的DBMS是兼容的。

```
CREATE TABLE BOOKS(
  ID INTEGER PRIMARY KEY,
  NAME VARCHAR(255),
  PRICE REAL
);
INSERT INTO BOOKS(NAME, PRICE) VALUES
  ('The Art of Computer Programming', 259.99),
  ('Concrete Mathematics', 57.57);
```

你照样可以在创建表格的时候指定各个列的类型，不过它们只是一个摆设，并不会被SQLite3严格执行（有一些例外，见下文）。事实上，SQLite3中根本没有```VARCHAR(255)```这个数据类型，你可以指定任意名称的数据类型。这样做实际上提高了SQLite3和其他DBMS的兼容性。

<!--more-->

在这个例子里，数据表各个列的意义明确，数据类型一般来说也都是一样的：主键一定是整数，书籍的名称一定是字符串而不会是实数或者别的什么东西，等等。

事实上，SQLite3的manifest typing中有一个例外情况，那就是被声明成```INTEGER PRIMARY KEY```的列只能存放整数值，而不能是其他的值。

对于其他的列而言，如果插入的数据没有遵从声明的类型，由于manifest typing的特点，SQLite3允许这样的操作，而不会向你报告错误。例如：

```
INSERT INTO BOOKS(NAME, PRICE) VALUES
  (1984, '6.00');
```

这样的插入操作也是被允许的。在这个例子里，被插入的数据实际上是有意义的（本意是插入```('1984', 6.00)```）。但是，实际操作数据的时候，会不会产生意外的情况？

看看下面这个查询：

```
SELECT * FROM BOOKS WHERE PRICE < 100;
```

本意是选出价格小于100的书籍。可是，对于 *1984* 这本书而言，它的```PRICE```列存放的值是字符串```'6.00'```而不是一个数值（实际上并非如此，见下文）。字符串和数值比较大小，怎么比较呢？

可能的做法有：

  1. 抛出错误，拒绝比较。
  2. 把```'6.00'```转换为数值，和100比较。这样有```'6.00' < 100```。
  3. 把100转换为字符串，和```'6.00'```比较。字符串的比较，就是逐个字符比较大小直到分出结果为止。这样有```'6.00' > 100```。

可以看得出来，manifest typing可能会带来一些复杂的情况。以上3中做法，并没有哪一种是特别显然的，这样会给使用者带来麻烦。事实上，SQLite3的做法**并不是以上3种的任何一种**。它的规则是：

  1. 比较不同类型的时候，首先有NULL < (INTEGER 或 REAL) < TEXT < BLOB
  2. 比较INTEGER和REAL，比较实际数值。
  3. 比较两个TEXT或者两个BLOB的时候，使用和```memcmp```类似的规则。

所以实际上是根据第1条规则可以确定```'6.00' > 100```。

如果不是必要，我**强烈建议**不要依赖上述规则，因为这些规则比较复杂，容易搞错，也不便于移植。

如果做一个实验，又会发现新的“意外”情况：

```
SELECT * FROM BOOKS WHERE PRICE < 100;
--> 2|Concrete Mathematics|57.57
--> 3|1984|6.0
```

我们发现事实上 *1984* 被选了出来。这样的结果，符合直觉和查询者的本意，但和上文进行的理性的分析又不一样。

出现这种结果的原因是，为了减少意外的结果，进一步提高SQLite3和其他DBMS的兼容性，前者又引入了type affinity的概念。Type affinity的准确译法我不知道。有的人译成类型亲和性，有的人译成类型近似。这些译法都是字面翻译。它的作用是提供一种类型的偏向，以便SQLite3在恰当的时候做一些隐式的类型转换。

SQLite3的type affinity也有5种：

  1. INTEGER。声明的类型中含有字符串```INT```的，归结到它。
  2. TEXT。声明的类型中含有字符串```CHAR```, ```CLOB```或者```TEXT```的，归结到它。例如，```VARCHAR(255)```就具有TEXT affinity。
  3. NONE。声明的类型是```BLOB```或者没有声明类型的，归结到它。
  4. REAL。声明的类型中含有字符串```REAL```, ```FLOA```或者```DOUB```的，归结到它。
  5. NUMERIC。以上各种情形之外的，归结到它。例如```DECIMAL```的type affinity就是NUMERIC。

注：有些单词，例如INTEGER和REAL，既是数据类型的名称，又是type affinity的名称。但它们表示不同的概念，不应混淆。

根据以上规则，我们知道，表中的```NAME```和```PRICE```字段分别具有TEXT和REAL的type affinity。这对数据库操作有什么影响呢？

最重要的影响就是，当插入数据的时候，SQLite3会尝试做类型转换。转换的规则同样很复杂：

1. NULL和BLOB类型的数据永远不会被自动地转换。
2. 如果列的type affinity是：
  1. NONE，则待插入的数据不会被转换类型。
  2. TEXT，则待插入的数据会被转换为TEXT类型。
  3. INTEGER或者NUMERIC，则待插入的数据会被转换为INTEGER或者REAL类型，如果相应的转换不会引起精度损失的话。
  4. REAL，则待插入的数据会被转换为REAL类型，如果相应的转换不会引起精度损失的话。

根据这些规则，我们知道，在插入```(1984, '6.00')```的时候，两个数据值就已经被自动地转换成为了```'1984'```和```6.0```。所以，之后的查询自然可以给出符合直觉的结果。

话说回来，一切的一切，都是因为一条错误的插入语句引起的，如果之前把它正确地写成

```
INSERT INTO BOOKS(NAME, PRICE) VALUES
  ('1984', 6.00);
```

那么就不需要依靠type affinity这些复杂的规则来“修正”。

然而type affinity的规则还不止于此。当比较两个数据的时候：

1. 如果表达式是运算符```IN```和```NOT IN```的右操作数：
  1. 表达式是一个列表，则它的type affinity是NONE。
  2. 表达式是一个SELECT，则它的type affinity和结果集的type affinity一样。
2. 如果表达式是列的名称，则它type affinity是这个列的type affinity。
3. 如果表达式具有```CAST(expr AS type)```的形式，则它的type affinity和数据类型```type```对应的type affinity一样。
4. 其他情况的表达式的type affinity是NONE。

这样，在比较之前，事先还有可能进行隐式的类型转换：

1. 如果一边的type affinity是INTEGER, REAL或者NUMERIC，而另一边是TEXT或者NONE，则另一边被转换为NUMERIC。
2. 如果一边的type affinity是TEXT，而另一边是NONE，则另一边被转换为TEXT。
3. 其他情况下不做类型转换。

下面是来自SQLite3官方文档的一个综合的例子。

```
CREATE TABLE t1(
    a TEXT,      -- text affinity
    b NUMERIC,   -- numeric affinity
    c BLOB,      -- no affinity
    d            -- no affinity
);

-- Values will be stored as TEXT, INTEGER, TEXT, and INTEGER respectively
INSERT INTO t1 VALUES('500', '500', '500', 500);
SELECT typeof(a), typeof(b), typeof(c), typeof(d) FROM t1;
text|integer|text|integer

-- Because column "a" has text affinity, numeric values on the
-- right-hand side of the comparisons are converted to text before
-- the comparison occurs.
SELECT a < 40,   a < 60,   a < 600 FROM t1;
0|1|1

-- Text affinity is applied to the right-hand operands but since
-- they are already TEXT this is a no-op; no conversions occur.
SELECT a < '40', a < '60', a < '600' FROM t1;
0|1|1

-- Column "b" has numeric affinity and so numeric affinity is applied
-- to the operands on the right.  Since the operands are already numeric,
-- the application of affinity is a no-op; no conversions occur.  All
-- values are compared numerically.
SELECT b < 40,   b < 60,   b < 600 FROM t1;
0|0|1

-- Numeric affinity is applied to operands on the right, converting them
-- from text to integers.  Then a numeric comparison occurs.
SELECT b < '40', b < '60', b < '600' FROM t1;
0|0|1

-- No affinity conversions occur.  Right-hand side values all have
-- storage class INTEGER which are always less than the TEXT values
-- on the left.
SELECT c < 40,   c < 60,   c < 600 FROM t1;
0|0|0

-- No affinity conversions occur.  Values are compared as TEXT.
SELECT c < '40', c < '60', c < '600' FROM t1;
0|1|1

-- No affinity conversions occur.  Right-hand side values all have
-- storage class INTEGER which compare numerically with the INTEGER
-- values on the left.
SELECT d < 40,   d < 60,   d < 600 FROM t1;
0|0|1

-- No affinity conversions occur.  INTEGER values on the left are
-- always less than TEXT values on the right.
SELECT d < '40', d < '60', d < '600' FROM t1;
1|1|1
```

可以说在SQLite3中，type affinity是一个相当复杂的概念，用它来出期末考试题，恐怕是再好也没有了。实际上，SQLite3之所以设计这样一个相当复杂的机制，初衷是为了尽量避免意外情况，提高它和其他DBMS的兼容性。所以，我**强烈建议**用户不要依赖这些具体的特性。这就好像是住宅楼上，窗户外边修的紧急逃生梯，是一种后备选项。只有楼房失火的时候，才应该借助这个梯子逃生，而不是让住户天天从这个逃生梯进出房屋。有趣的是，中国的楼房没有一个在窗外修建逃生梯的（而是修建在楼房内部），因为凡是提供一种机制，就有被滥用的可能性。

> “在窗边修逃生梯？那岂不是太方便了那些小偷们从楼下钻进来了吗。”

注：我并不认为把逃生梯修在窗外是一个好主意。实际上，这是美国的房地产商为了节约成本同时满足消防要求而想出来的点子。在我看来，这样做确实很不妥当。我只是为了说明(逃生)机制会被(小偷)滥用而做了一个比喻。此外我还觉得，一种琐碎而无关紧要的东西，总是被拿来做考试题，也是一种滥用。

最后，还有一类隐式类型转换，和type affinity无关，那就是数学运算符。数学运算符天生只能对数字进行运算，所以它两边的操作数都会按照NUMERIC type affinity的规则进行类型转换。如果一个操作数是NULL，则数学运算的结果是NULL。

注：这里，SQLite3的[官方文档](http://www.sqlite.org/datatype3.html)似乎有误。原文说的是

> ... cast both operands to the NUMERIC **storage class** prior to being carried out.

但事实上并不存在名为NUMERIC的storage class（即本文所说的数据类型）。看来即便是SQLite3的原作者，也容易被这些复杂的概念搞晕。(当然，他并非分不清这些概念——否则也没法写出实现了——只是容易产生笔误而已。)
