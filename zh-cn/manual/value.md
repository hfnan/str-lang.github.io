# 名字，值和解析值

STR语言中的数据元素都有两个属性——名字和值。有些还有解析值。

## REPL

> 悲惨的是，STR语言最终退化成了一门解释型语言。
> 
> 但它想保留它的尊严。（制造混乱之心不死）

STR的REPL将会求prompt后*无论什么东西*的值（当然，可能没值，那就仅仅执行它），但由于很多时候我们输入的都是 **凡串**， 所以会将输入原封不动地输出。

## 面子串

显式显示的字符串称为面子串。

面子串具有 **名值相同** 的特点，即你看到它是什么样子，它的值就是什么样子。（~~表里如一了属于是~~）

如：

```str
> hello, world
hello, world
> 5 + 3 * 2
5 + 3 * 2
```

## 凡串

解析无意义的串称为凡串。

## 解析式

用`{}`括起来的面子串称为解析式。

解析式的值是其中面子串evaluate后的值。

如：

```str
> {hello, world} // 凡串没有解析值
> {5 + 3 * 2} 
11
```

## 变量

变量是一个 **串的引用** 。

如：

```str
> var five = 5 + 3 * 2
> five
5 + 3 * 2
> {five}
11
```

变量`five`的名字是`five`，值是`5 + 3 * 2`,解析值是`11`。

所以，什么是解析值？

!> 解析值就是**对值解析**得到的。

## 函数

函数就是**带参变量**。

如：

```str
> var square(x) = x * x
> square(5)
5 * 5
> {square(5)}
25
```

参数是字面意义上的**传值调用**，也就是说，传的是值。

如果是表达式，会写成用括号括起来不可分割的形式。（避免毫无道理的歧义）

```str
> var exp = 5 + 3 * 2
> var square(x) = x * x
> square(exp)
(5 + 3 * 2) * (5 + 3 * 2)
> {square(exp)}
121
```

## 赋值

将右部串的 **值** 赋给左部变量。

如：

```str
> var value = 5 + 3 * 2
> value
5 + 3 * 2
> value = {5 + 3 * 2}
> value
11
```

!> 对未定义变量求解析是非法的,但求值是合法的

```str
> var value = 5 + x * y
> value
5 + x * y
> value = {5 + x * y}
ERROR
```

如果变量已定义：

```str
> var x = 3
> var y = 5
> var prod = {x * y}
> prod
15
```

## 特殊表达

直接看代码：

```str
> var std is interesting  //所有不符合STR语法的都是面子串
var std is interesting
>
> var hello = world
> hello world //面子串
hello world
>
> $hello world //替换，还没有实装，看个乐呵吧
world world
```

## 块结构

遇到了实现上的难点，我只能说想法很丰满，现实很骨感

```str
var del(x, y) = 
    var sum = x + y
    var dif = x - y
    return sum * dif //这里显然不能返回解析式，为什么？
```

## 条件控制

我还没想好，怎么保持一致性。

初步设想大概是这个样子

```str
variable =
    test pred : expr
    test pred : expr
    else expr
```

