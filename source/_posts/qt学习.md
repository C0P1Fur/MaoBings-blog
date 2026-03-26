---
title: qt学习
date: 2026-03-26 19:40:58
categories:
 - qt
 - C/C++
tags:
 - tutorial
---

### 0.qt是什么
**一句话总结**:用C/C++语言，来构建图形化gui用户操作界面的库

### 1.初识qt
##### 1.1 qt新建项目文件结构
使用qt creator新建一个QT widgets项目，项目文件介绍如下：

**widget.ui**
.ui 文件是 Qt Widgets 模块中使用的 XML 格式的界面设计文件，由 Qt Designer 可视化设计工具生成。
使用“设计”功能打开.ui文件，可对文件进行编辑
左侧为控件工具栏，可自定义设计你的窗口，后可直接运行查看效果

**widget.h**
widget类的声明文件

**widget.cpp**
widget类的实现文件

**main.cpp**
程序主函数入口

##### 1.2 设计第一个qt窗口
可以直接使用自定义组件，编辑widget.ui，最后点击运行即可
具体设计基础操作详见：[qt ui窗口设计基础教程](https://www.bilibili.com/video/BV1N34y1H7x7?p=3)

##### 2. 信号与槽
信号：用户操作
槽：对应函数

有两种给控件添加槽的方式
**1.在ui布局界面添加**
右键控件，然后选择添加槽，可直接将控件连接到对应的槽
widget.h与widget.cpp文件里都会有新内容
widget.h:
```
// 槽函数的声明
private slots:
    void on_pushButton_clicked();
```
widget.cpp
```
// 槽函数
void Widget::on_pushButton_clicked()
{
    // 获取信息
    /**
     * 其中，
     * QString：qt的字符串对象
     * ui：控件对象都存在ui对象里，可通过ui访问控件对象
     * lineText：对应的控件对象（可自定义名称），使用text()方法可获取到它的输入文本
      */
    QString userInput = ui->lineText->text();

    /* 对信息的处理代码 */
}
```
**2.使用connect()函数**
connect()函数用于连接信号与槽
此函数的参数列表：谁发出信号 发出什么信号 谁处理信号 怎么处理
widget.cpp
```
    /**
     * ui->lineText：链接的对象
     * SIGNAL()：SIGNAL为宏函数，表示这个函数是信号
     * returnPressed()：为pressButton的信号函数，表示要处理的信号
     * this：当前Widget对象的指针，表示处理这个信号的对象
     * SLOT()：SLOT为宏函数，表示这个函数是槽
     * on_buttonEnter_clicked()：对应的槽函数
     */
    connect(ui->lineText, SIGNAL(returnPressed()), this, SLOT(on_buttonEnter_clicked()));
```