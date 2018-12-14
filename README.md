# 汇编语言基础
*对于汇编这门语言来说，了解是很必要的。无论外面的世界高级语言如何变幻，谁强又谁弱，哪个被淘汰哪个又兴起，作为汇编这门语言来说，云淡风轻，因为底层，就是不一样哈~大家都是靠他来完成与硬件的对接，逻辑的实现。所以简单介绍与学习一下这门语言*

## 目录

***基础介绍***

  1. [汇编语言简介]()
  1. [80×86 计算机组织]()
  1. [80×86 的指令系统和寻址方式]()
  1. [汇编语言程序格式]()
  1. [子程序结构]()
  
***汇编实验***
  1. [打印输出"Hello World!"](#shiyan1)
  1. [双精度数加减法]()
  1. [四则运算]()
  1. [串操作]()
  1. [数组求和]()

***例题讲解***







<a name="shiyan1"> </a>
## 打印输出"Hello World!"
```asm
DATAS  SEGMENT
     STR1  DB  'Hello World!',13,10,'$'
DATAS  ENDS

CODES  SEGMENT
     ASSUME    CS:CODES,DS:DATAS
START:
     MOV  AX,DATAS      ;把立即数赋给AX寄存器
     MOV  DS,AX          ;段地址寄存器赋给AX ，再移到DS
     LEA  DX,STR1        ;调用字符串开始地址
     MOV  AH,9           ;调用dos系统9号功能
     INT  21H             ;中断
   
     MOV  AH,4CH
     INT  21H
CODES  ENDS
END   START
```
