---
title: C陷阱与缺陷笔记
date: 2025-11-10 16:14:03
tags:
 - C/C++
catagories:
 - tutorial
---

#### 2.3 作为语句结束标志的分号
`;`，单独的分号会被视为一个空语句

**错误情况1**
循环或者条件语句后多引号
```
if (x > 0);
    x = -1;
```
当while或者if语句后接了一个引号，只会执行空引号的内容

**错误情况2**
return后缺引号
```
if (n < 3)
    return
lo.date = x[0];
lo.date = x[1];
```
当n >= 3时，lo.date = x[0]永远不会执行，这种错误非常难发现

**错误情况3**
结构后缺引号
```
struct lo {
    int date;
    int time;
    int code;
}

main(){
    ...
}
```
结构后紧接了一个函数定义
那么结构lo会被当成是main函数的返回值

#### 2.4 switch语句
在C中，case会当成一个条件分支处理，因此程序流会按顺序执行case后的语句，需要跳出，就得使用break
这个特性，能够在程序处理多个问题，而问题的处理方式差别不大时，提供帮助，如
```
switch (ch){
    case '\n':
        line++;
        /*此处没有break语句*/
    case '\t':
        /*此处没有break语句*/
    case ' ':
        相同的处理...
        break;
}
```
该程序在遇到三种字符时都进行相同的处理，只不过换行符需要额外执行line++操作
同时，如果case后面没有break，则需要手动用注释写明

#### 2.5 悬挂else引起的问题
```
if (x == 0)
    if (y == 0) error();
else
{
    z = x + y;
    f(&z);
}
```
如果不加大括号明确表明结构的话，if会结合离它最近的else
因此，上面的程序实际上是
```
if (x == 0){
    if (y == 0)
        error();
    else
    {
        z = x + y;
        f(&z);
    }
}
```
所以尽量把括号都加起