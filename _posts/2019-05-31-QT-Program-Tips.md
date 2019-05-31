---
layout:     post
title:      "QT编程贴士"
subtitle:   "QT编程接口的使用"
date:       2019-05-31
author:     "Jannon"
header-img: "images/homepage-bg.jpg"
tags:
    - QT
---

## QFile接口类的使用
QFile类是QT中的文件操作接口类，可以实现对文件进行相应的读取和各种操作。例如文档中xml节点都在一行表示，没有回车和换行，不利于阅读，需要
对节点结束符后面添加换行符，优化文件显示结构。优化之前的文件内容如下：
``` html
<svg><g><circle><d="M 12,33"></circle></g></svg>
```
优化实现代码如下：
``` c++
   QFile inFile("/home/Pictures/test.svg");
   QFile outFile("/home/Pictures/fix.svg");

   if(inFile.open(QFile::ReadOnly) && outFile.open(QFile::WriteOnly))
   {
       QTextStream in(&inFile),out(&outFile);
       QString fileStr = in.readAll();

       fileStr.replace(QString(">"),QString(">\n"));
       out<<fileStr;

       inFile.close();
       outFile.close();
   }
```
优化之后的文件如下：
``` html
<svg>
<g>
<circle>
<d="M 12,33">
</circle>
</g>
</svg>
```
也可以不使用输入输出流，直接使QFile接口实现：
``` c++
QFile txtFile("/home/Pictures/test.txt");
QFile newTxtFile("/home/Pictures/new.txt");

if(txtFile.open(QFile::ReadWrite) && newTxtFile.open(QFile::WriteOnly))
{
    QByteArray fileData = txtFile.readAll();
    QString fileText(fileData);

    fileText.replace(QString(">"),QString(">\n"));
    newTxtFile.write(fileText.toUtf8());

    txtFile.close();
    newTxtFile.close();
}
```

修改替换文件内容：
``` c++
QFile txtFile("/home/Pictures/test.txt");
   QFile newTxtFile("/home/Pictures/new.txt");
   if(txtFile.open(QFile::ReadWrite) && newTxtFile.open(QFile::WriteOnly))
   {
       QString fileText = txtFile.readAll();
       QRegularExpression rep("b");
       QString replacementText("a");

       QRegularExpressionMatchIterator it = rep.globalMatch(fileText);

       while(it.hasNext())
       {
           QRegularExpressionMatch match = it.next();
           fileText.replace(match.capturedStart(0), match.capturedLength(0), replacementText);
       }

       newTxtFile.write(fileText.toUtf8());
       txtFile.close();
       newTxtFile.close();
   }
```
test.txt中的文件内容：
```
hbppy
bpple
```
new.txt中的文件内容:
```
happy
apple
```
## QByteArray接口类的使用
使用一个字节保存十进制或者十六进制整型数到QByteArray。代码实现如下：
``` C++
    QByteArray arr;
    arr.append(28);
    arr.append(0x16);
    bool ok;
    // "\x1C\x16" Size: 2 element 7190 true    
    qDebug()<<arr<<"Size:"<<arr.size()<<"element"<<arr.toHex().toInt(&ok,16)<<ok;

    static const char va[] = {0x22,0x2f,0x08};
    QByteArray rawDataArray = QByteArray::fromRawData(va,sizeof (va));
    // 3 "222f08"
    qDebug()<<rawDataArray.size()<<rawDataArray.toHex();

    QByteArray ba;
    ba.resize(5);
    ba[0] = 0x3c;
    ba[1] = 0x04;
    ba[2] = 0x64;
    ba[3] = 0x18;
    ba[4] = 0x08;
    // < 5
    qDebug()<<ba[0]<<ba.size();

    QByteArray array;
    array.insert(0, 0x22);
    array.append(0x2d);
    array[2] = 0x01;
    array.insert(3, 0x05);
    QByteArray kk = array.left(1);
    int result = kk.toHex().toInt(nullptr,16);
    //34
    qDebug()<<result;
    QString mm(kk.toHex());
    //0
    qDebug()<<mm.compare("22");
    // "22" 4 "222d0105" 2
    qDebug()<<mm<<array.size()<<array.toHex()<<kk.toHex().size();

```
