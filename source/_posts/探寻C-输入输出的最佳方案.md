---
title: 探寻C++输入输出的最佳方案
date: 2025-11-08 13:32:37
tags:
 - C++
 - io
catagories:
 - exploration
---
## 0.前言
看大佬题解时总是会提到关于输入输出的问题，那就是：
cstdio的输入输出函数(scanf, printf)，总比iostream的输入输出函数快

## 1.输出实验
```
/**
 * 传统标准&文件输出：printf&fprintf
 * C++标准&文件输出：ostream&ofstream
*/

#include <iostream>
#include <fstream>
#include <cstdio>
#include <cstdlib>
#include <ctime>
#include "files.hpp"

clock_t cTest(FILE * out);
clock_t cppTest(std::ostream & out);

int main(void)
{
    // C风格标准&文件输出
    FILE * cStandard = stdout;
    FILE * cFile = fopen(OFILE, "w");
    // CPP风格标准&文件输出
    std::ostream & cppStandard = std::cout;
    std::ofstream cppFile;
    cppFile.open(OFILE);
    clock_t tcS, tcF, tcppS, tcppF;

    // 是否打开成功判断
    if (cFile == NULL || !cppFile.is_open())
        exit(EXIT_FAILURE);

    tcS = cTest(cStandard); // C标准输出用掉的时间
    tcF = cTest(cFile); // C文件输出
    tcppS = cppTest(cppStandard); // CPP标准输出
    tcppF = cppTest(cppFile); // CPP文件输出

    std::cout << "---" << std::endl;
    std::cout << "c standard: " << double(tcS) / CLOCKS_PER_SEC << " seconds" << std::endl;
    std::cout << "c file: " << double(tcF) / CLOCKS_PER_SEC << " seconds" << std::endl;
    std::cout << "cpp standard: " << double(tcppS) / CLOCKS_PER_SEC << " seconds" << std::endl;
    std::cout << "cpp file: " << double(tcppF) / CLOCKS_PER_SEC << " seconds" << std::endl;
    
    fclose(cFile);
    cppFile.close();
}

clock_t cTest(FILE * out)
{
    clock_t start, end;

    start = clock();
    for (int i = 0; i < ITERATE; i++)
        fprintf(out, "%s\n", QUOTE);
    end = clock();

    return end - start;
}

clock_t cppTest(std::ostream & out)
{
    clock_t start, end;

    start = clock();
    for (int i = 0; i < ITERATE; i++)
        out << QUOTE << std::endl;
    end = clock();

    return end - start;
}
```

最后输出：
```
c standard: 27.648 seconds
c file: 0.029 seconds
cpp standard: 3.726 seconds
cpp file: 0.154 seconds
```

结论：
1. 对于标准输出与文件输出，文件输出 远优于 标准输出
2. 对于c文件输出与cpp文件输出，c文件输出 优于 cpp文件输出
3. 对于c标准输出与cpp标准输出，cpp标准输出 远优于 c标准输出

但做到这里我意识到，大数据一般不会写入标准输出里
因此比较的对象应该是重定向输出与文件输出
又进行了一次实验，这次的结果将重定向到CONTAINER.dat里
结果
```
c standard: 0.046 seconds
c file: 0.033 seconds
cpp standard: 0.181 seconds
cpp file: 0.151 seconds
```

于是最后得出结论
1. 对于重定向输出与文件输出，文件输出 远优于 重定向输出
2. 对于文件输出，c文件输出 优于 cpp文件输出
3. 对于重定向输出，c重定向输出 优于 cpp重定向输出

## 2.输入实验
```
/**
 * 传统标准&文件输入：scanf&fscanff
 * C++标准&文件输出：istream&ifstream
*/

#include <iostream>
#include <fstream>
#include <cstdio>
#include <cstdlib>
#include <ctime>
#include "files.hpp"

clock_t cTest(FILE * in);
clock_t cppTest(std::istream & in);

int main(void)
{
    // c标准&文件输入
    FILE * ciFile = fopen(IFILE, "r");
    FILE * ciStandard = stdin;
    // cpp标准&文件输入
    std::ifstream cppiFile;
    cppiFile.open(IFILE);
    std::istream & cppiStandard = std::cin;
    clock_t tcF, tcS, tcppF, tcppS;

    if (ciFile == NULL || !cppiFile.is_open())
        exit(EXIT_FAILURE);
    
    tcF = cTest(ciFile);
    tcS = cTest(ciStandard);
    tcppF = cppTest(cppiFile);
    tcppS = cppTest(cppiStandard);

    std::cout << "---" << std::endl;
    std::cout << "c standard: " << double(tcS) / CLOCKS_PER_SEC << " seconds" << std::endl;
    std::cout << "c file: " << double(tcF) / CLOCKS_PER_SEC << " seconds" << std::endl;
    std::cout << "cpp standard: " << double(tcppS) / CLOCKS_PER_SEC << " seconds" << std::endl;
    std::cout << "cpp file: " << double(tcppF) / CLOCKS_PER_SEC << " seconds" << std::endl;

    fclose(ciFile);
    cppiFile.close();
}

clock_t cTest(FILE * in)
{
    clock_t start, end;
    char buff[1024];
    
    start = clock();
    for (int i = 0; i < ITERATE; i++)
        fscanf(in, "%s", buff);
    end = clock();

    return end - start;
}

clock_t cppTest(std::istream & in)
{
    clock_t start, end;
    char buff[1024];

    start = clock();
    for (int i = 0; i < ITERATE; i++)
        std::cin >> buff;
    end = clock();

    return end - start;
}
```

输出结果:
```
c standard: 0.02 seconds
c file: 0.026 seconds
cpp standard: 0.039 seconds
cpp file: 0.043 seconds
```

结论：
1. 对于重定向输入与文件输入，文件输入 约等于 重定向输入
2. 对于文件输入，c文件输入 优于 cpp文件输入
3. 对于重定向输入，c重定向输入 优于 cpp重定向输入

## 3.结论
c的输入输出效率确实比cpp的快一些
同时，使用文件输入输出的效率是所有情况最快的

这个结论有什么用呢？我们针对算法竞赛来分析

算法竞赛的数据输入输出分为两种类型：
- 核心代码模式
    - 线上算法平台（如leetcode）评判的模式
    - 特点：不需要些输入输出的部分，只需要实现核心代码
- ACM模式
    - 目前主流的模式
    - 特点：需要考虑数据的输入与输出，评判标准是输出是否一致
所以我们针对对ACM模式进行讨论
ACM模式流程：
给定输入文件->输入到程序->得到输出文件->对比标准答案
显然这是一个重定向的过程
于是，对于竞赛来说，确实是使用c的输入输出模式，会节省一些时间