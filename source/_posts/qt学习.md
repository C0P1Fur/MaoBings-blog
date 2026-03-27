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

### 2. 信号与槽
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
至于这种方法信号与槽是如何连接的，豆包给出的解释是
自动连接完全不依赖.ui文件中的信号槽配置，而是依赖于：
1. 控件的objectName（存储在.ui文件中）
2. 槽函数的命名规范（在代码中）
3. uic生成的connectSlotsByName调用（在生成的ui_*.h中）

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
    // 三种不同的connect语法：1.旧式传递函数语法 2.新式传递函数指针语法 3.传递lambda函数语法
    connect(ui->lineText, SIGNAL(returnPressed()), this, SLOT(on_buttonEnter_clicked()));
    connect(ui->buttonCancel, &QPushButton::clicked, this, &Widget::on_buttonCancel_clicked);
    connect(ui->buttonDelete, &QPushButton::clicked, [this]() {
    QMessageBox::information(this, "删除", "删除成功");
    });
```

**信号与槽程序示例：使用qt写一个四则运算器**
widget.h:
```
#ifndef WIDGET_H
#define WIDGET_H

#include <QWidget>
#include <stack>

QT_BEGIN_NAMESPACE
namespace Ui {
class Widget;
}
QT_END_NAMESPACE

class Widget : public QWidget
{
    Q_OBJECT

public:
    explicit Widget(QWidget *parent = nullptr);
    ~Widget() override;

private slots:
    void on_buttonOne_clicked();

    void on_buttonTwo_clicked();

    void on_buttonThree_clicked();

    void on_buttonFour_clicked();

    void on_buttonFive_clicked();

    void on_buttonSix_clicked();

    void on_buttonSeven_clicked();

    void on_buttonEight_clicked();

    void on_buttonNine_clicked();

    void on_buttonLeft_clicked();

    void on_buttonZero_clicked();

    void on_buttonRight_clicked();

    void on_buttonAdd_clicked();

    void on_buttonMinus_clicked();

    void on_buttonDivide_clicked();

    void on_buttonMulti_clicked();

    void on_buttonEqual_clicked();

    void on_buttonClear_clicked();

    void on_buttonBack_clicked();

private:
    Ui::Widget *ui;
    QString expression;

private:
    void expressionEval(std::stack<int> &, std::stack<char> &);
};
#endif // WIDGET_H
```

widget.cpp:
```
#include "widget.h"
#include "./ui_widget.h"
#include <unordered_map>
#include <stack>
#include <string>
#include <ctype.h>

Widget::Widget(QWidget *parent)
    : QWidget(parent)
    , ui(new Ui::Widget)
{
    ui->setupUi(this);

    // 设置窗口最大与最小大小
    this->setMaximumSize(1920, 1080);
    this->setMinimumSize(200, 280);

    // 设置窗口标题
    this->setWindowTitle("Hansen's 计算器");
    // 设置窗口
    QIcon * windowIcon = new QIcon("C:/Users/29955/Documents/qt_tutorial_projects/calculator/icon.png");
    this->setWindowIcon(*windowIcon);

    // 设置显示字体
    QFont * lineFont = new QFont("仿宋", 12);
    ui->lineExpression->setFont(*lineFont);
}

Widget::~Widget()
{
    delete ui;
}

void Widget::on_buttonOne_clicked()
{
    expression += "1";
    ui->lineExpression->setText(expression);
}


void Widget::on_buttonTwo_clicked()
{
    expression += "2";
    ui->lineExpression->setText(expression);
}


void Widget::on_buttonThree_clicked()
{
    expression += "3";
    ui->lineExpression->setText(expression);
}


void Widget::on_buttonFour_clicked()
{
    expression += "4";
    ui->lineExpression->setText(expression);
}


void Widget::on_buttonFive_clicked()
{
    expression += "5";
    ui->lineExpression->setText(expression);
}


void Widget::on_buttonSix_clicked()
{
    expression += "6";
    ui->lineExpression->setText(expression);
}


void Widget::on_buttonSeven_clicked()
{
    expression += "7";
    ui->lineExpression->setText(expression);
}


void Widget::on_buttonEight_clicked()
{
    expression += "8";
    ui->lineExpression->setText(expression);
}


void Widget::on_buttonNine_clicked()
{
    expression += "9";
    ui->lineExpression->setText(expression);
}


void Widget::on_buttonLeft_clicked()
{
    expression += "(";
    ui->lineExpression->setText(expression);
}


void Widget::on_buttonZero_clicked()
{
    expression += "0";
    ui->lineExpression->setText(expression);
}


void Widget::on_buttonRight_clicked()
{
    expression += ")";
    ui->lineExpression->setText(expression);
}


void Widget::on_buttonAdd_clicked()
{
    expression += "+";
    ui->lineExpression->setText(expression);
}


void Widget::on_buttonMinus_clicked()
{
    expression += "-";
    ui->lineExpression->setText(expression);
}


void Widget::on_buttonDivide_clicked()
{
    expression += "/";
    ui->lineExpression->setText(expression);
}


void Widget::on_buttonMulti_clicked()
{
    expression += "*";
    ui->lineExpression->setText(expression);
}


void Widget::on_buttonClear_clicked()
{
    expression.clear();
    ui->lineExpression->clear();
}


void Widget::on_buttonBack_clicked()
{
    expression.chop(1);
    ui->lineExpression->setText(expression);
}

void Widget::expressionEval(std::stack<int> & num, std::stack<char> & op) // 计算栈顶
{
    int num1 = num.top(); num.pop();
    int num2 = num.top(); num.pop();
    char opX = op.top(); op.pop();
    int r;

    if (opX == '-') r = num2 - num1;
    if (opX == '*') r = num2 * num1;
    if (opX == '+') r = num2 + num1;
    if (opX == '/') r = num2 / num1;

    num.push(r);
}

void Widget::on_buttonEqual_clicked()
{
    //这里我就直接套用我算法课上学的堆栈计算表达式的算法了
    // 大型项目里就不能直接用using namespace std了
    std::stack<int> num;
    std::stack<char> op;
    std::unordered_map<char, int> l{{'-', 1}, {'+', 1}, {'*', 2}, {'/', 2}};
    std::string s = expression.toStdString();
    if (s[0] == '-')
        num.push(0); // 加上前导0方便计算负数

    for (int i = 0; i < s.size(); i++)
    {
        if (isdigit(s[i]))
        {
            int x = 0, j = i;
            while (j < s.size() && isdigit(s[j])) // 计算多位数字
            {
                x = x * 10 + s[j] - '0';
                j++;
            }

            num.push(x); // 数字入栈
            i = j - 1; // 调整下标
        }
        else if (s[i] == '(') // 优先度低直接压入栈
        {
            op.push(s[i]);
        }
        else if (s[i] == ')') // 计算括号内的所有元素
        {
            while (op.top() != '(') // 如果没有到左括号，就一直计算
                expressionEval(num, op);
            op.pop();
        }
        else
        {
            while (!op.empty() && l[op.top()] >= l[s[i]])
                expressionEval(num, op);

            op.push(s[i]);
        }
    }

    while (!op.empty())
        expressionEval(num, op);

    int expressionResult = num.top();
    expression = QString::number(expressionResult);
    ui->lineExpression->setText(expression);
}
```