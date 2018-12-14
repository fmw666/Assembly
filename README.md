# 汇编语言基础
&ensp;&ensp;&ensp;&ensp;*对于汇编这门语言来说，了解是很必要的。无论外面的世界高级语言如何变幻，谁强又谁弱，哪个被淘汰哪个又兴起，作为汇编这门语言来说，云淡风轻，因为底层，就是不一样哈~大家都是靠他来完成与硬件的对接，逻辑的实现。所以简单介绍与学习一下这门语言。*

## 目录

***一、基础介绍***

  1. [汇编语言简介](#jichu1)
  1. [80×86 计算机组织](#jichu2)
  1. [80×86 的指令系统和寻址方式](#jichu3)
  1. [汇编语言程序格式]()
  1. [子程序结构]()
  
***二、汇编实验***
  1. [打印输出"Hello World!"](#shiyan1)
  1. [双精度数加减法](#shiyan2)
  1. [四则运算](#shiyan3)
  1. [串操作](#shiyan4)
  1. [数组求和](#shiyan5)

***三、例题讲解***

<a name="jichu1"> </a>
## 汇编语言简介
&ensp;&ensp;&ensp;&ensp;汇编语言是大学许多基础课程的基础（先行课），并不说明汇编语言很基础，而是它很重要！它作为C语言、计算机组成原理、操作系统等大学课程的基础课，不仅能为我们打下牢固的计算机学习地基，也能让我们建立起一个完整的计算机学习构架。

&ensp;&ensp;&ensp;&ensp;学来只是为了过渡？of course not！它也可以用作程序开发（虽然语法很不友好），由于偏下底层，所以效率极高，作为高效率程序开发是个不二之选。但是如果选它其实还是蛮二的...不说这个，由于偏向底层硬件，所以在嵌入式开发中仍是有一席之地。总之，汇编语言很重要！

&ensp;&ensp;&ensp;&ensp;在学习中，我采用的是网上[Masm for Windows](http://www.skycn.com/soft/appid/86368.html)汇编编译器（点击跳到下载页面），语言嘛，工具不重要，学习到其中的东西才是最主要的。当然后续的实际开发中工具合不合适，上不上手才是程序员最应该考虑的，当前学生阶段还是不要太挑了。

&ensp;&ensp;&ensp;&ensp;后面不罗嗦了，大家一起加油吧！

[◀返回目录](#目录)

<a name="jichu2"> </a>
## 80×86 计算机组织
&ensp;&ensp;&ensp;&ensp;计算机主要由[CPU](https://baike.baidu.com/item/中央处理器/284033?fromtitle=ＣＰＵ&fromid=368184&fr=aladdin)（运算器和控制器集成在的一个芯片）、[存储器](https://baike.baidu.com/item/存储器)、[输入输出设备](https://baike.baidu.com/item/输入输出设备) 构成，80×86 就是这样一组微处理器系列。

### 80×86寄存器组
**通用寄存器**——AX、BX、CX、DX、SP、BP、DI、SI，8个通用寄存器（只限于8086/8088）。

其中AX、BX、CX、DX可称为数据寄存器，用来暂时存放计算过程中所用到的操作数、结果或其他信息。它们都可以以字（16位）的形式访问，或者也可以以字节（8位）的形式访问。例如，对AX可以分别访问高位字节AH或者低位字节AL。
+ AX（accumulator）作为累加器用，所以它是算术运算的主要寄存器。
+ BX（base）可以作为通用寄存器使用。此外在计算存储器地址时，它经常用作基址寄存器。
+ CX（count）可以作为通用寄存器使用。此外常用来保存计数值。
+ DX（data）可以作为通用寄存器使用。一般在作双字长运算时把DX和AX组合在一起存放一个双字长数，DX用来存放高位字。

其中SP、BP、SI、DI这四个16位寄存器可以像数据寄存器一样在运算过程中存放操作数，但它们只能以字（16位）位单位使用。因为它们更经常的用途是在存储器寻址时，提供偏移地址，所以它们亦称为指针或变址寄存器。
+ SP（stack pointer）称为堆栈指针寄存器，用来指示段顶的偏移地址。
+ BP（base pointer）称为基址指针寄存器。
+ SI（source index）称为源变址寄存器，用来确定段中某一存储单元的地址，有自动增量和自动减量的功能。
+ DI（destination index）称为目的变址寄存器，用来确定段中某一存储单元的地址，有自动增量和自动减量的功能。

**专用寄存器**——IP、SP、FLAGS、（后续为标志寄存器）OF、SF、ZF、CF、AF、PF、等...
+ IP（instruction pointer）为指令指针寄存器，它用来存放代码段中的偏移地址。在程序运行的过程中，它始终指向下一条指令的首地址，与段寄存器CS联用确定下一条指令的物理地址。IP寄存器是计算机中很重要的一个控制寄存器。
+ SP为堆栈指针寄存器。它与堆栈段寄存器联用来确定堆栈段中栈顶的地址。
+ FLAGS为标志寄存器，又称程序状态寄存器（program status word，PSW）。这是一个存放条件码标志、控制标志和系统标志的寄存器。

下面6个条件码标志用来记录程序中运行结果的状态信息，它们是根据有关指令的运行结果由CPU自动设置的。
+ OF（overflow flag）为溢出标志。在运算过程中，操作数超出了机器能表示的范围称为溢出，此时OF位置为1，否则置为0.
+ SF（sign flag）为符号标志。记录运算结果的符号，结果为负时置为1，否则置为0。
+ ZF（zero flag）为零标志。运算结果为0时置为1，否则置为0。
+ CF（carry flag）为进位标志。记录运算时从最高有效位产生的进位值。例如，执行加法指令时，最高有效位有进位时置为1，否则置为0。
+ AF（auxiliary carry flag）为辅助进位标志。记录运算时第3位（半个字节）产生的进位值。进位时置为1，否则置为0。
+ PF（parity flag）为奇偶标志。当结果操作数中1的个数为偶数时置为1，否则置为0。

**段寄存器**——CS、DS、SS、ES，4个，后续还增加了两个FS和GS就不加以说明了。

段寄存器也是一种专用寄存器，它们专用于存储器寻址，用来直接或间接地存放段地址。段寄存器长度为16位。
+ CS（code segment）为代码段。
+ DS（data segment）为数据段。
+ SS（stack segment）为堆栈段。
+ ES（extra segment）为附加段。

[◀返回目录](#目录)

<a name="jichu3"> </a>
## 80×86 的指令系统和寻址方式
&ensp;&ensp;&ensp;&ensp;何为指令系统？指令嘛，执行来使计算机解决实际问题的。每种计算机都有一组指令集供给用户使用，这组指令集就称为计算机的指令系统。计算机中的指令由操作码字段和操作数字段两部分组成，其中操作码字段指示计算机所要执行的操作。用一句来体现80×86指令的格式：`[标号:]指令的助记符[操作数1[,操作数2]][;注释]`




[◀返回目录](#目录)

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

[◀返回目录](#目录)

<a name="shiyan2"> </a>
## 双精度数加减法
***双精度数加法***
```asm
DATAS SEGMENT
    X DW 1,2         ;给双精度X赋值 
    Y DW 3,4         ;给双精度Y赋值 
    Z DW 0,0         ;给双精度Z赋值 
DATAS ENDS

STACKS SEGMENT
    ;此处输入堆栈段代码
STACKS ENDS

CODES SEGMENT
    ASSUME CS:CODES,DS:DATAS,SS:STACKS
START:
    MOV AX,DATAS
    MOV DS,AX
    
    MOV AX,X         ;把X低位放到AX寄存器中
    MOV DX,X+2       ;把X高位放到DX寄存器中
    
    ADD AX,Y         ;把X低位和Y低位相加，存在X中
    ADC DX,Y+2       ;把X高位和Y高位相加，存在DX中
    
    MOV Z,AX         ;把低位相加得到的数放到Z的地位
    MOV Z+2,DX       ;把高位相加得到的数放到Z的高位
    
    MOV DX,Z+2
    MOV DL,DH
    ADD DL,'0'          ;输出Z的高位
    MOV AH,02H        ;调用dos系统的02号功能
    INT 21H             ;中断
    MOV DX,Z+2
    ADD DL,'0'
    MOV AH,02H
    INT 21H
    
    MOV DX,Z
    MOV DL,DH
    ADD DL,'0'          ;输出Z的高位
    MOV AH,02H        ;调用dos系统的02号功能
    INT 21H             ;中断
    MOV DX,Z
    ADD DL,'0'
    MOV AH,02H
    INT 21H
    ;此处输入代码段代码
    MOV AH,4CH
    INT 21H
CODES ENDS
    END START
```

***双精度数减法***
```asm
DATAS SEGMENT
    X DW 4,6                  ;给双精度X赋值 
    Y DW 2,3                  ;给双精度Y赋值
    Z DW 0,0                  ;给双精度Z赋值
DATAS ENDS

STACKS SEGMENT
    ;此处输入堆栈段代码
STACKS ENDS

CODES SEGMENT
    ASSUME CS:CODES,DS:DATAS,SS:STACKS
START:
    MOV AX,DATAS
    MOV DS,AX
    
    MOV AX,X         ;把X的低位放在AX寄存器中
    MOV DX,X+2       ;把X的高位放在DX寄存器中
    
    SUB AX,Y          ;把X与Y的低位相减，存在AX中
    SBB DX,Y+2        ;把X与Y的高位相减，存在DX中
    MOV Z,AX         ;低位相减得到的值放在Z的地位
    MOV Z+2,DX       ;高位相减得到的值放在Z的高位
    
    MOV DX,Z+2
    MOV DL,DH
    ADD DL,'0'          ;输出Z的高位
    MOV AH,02H       ;调用dos系统2号功能
    INT 21H            ;中断
    MOV DX,Z+2
    ADD DL,'0'
    MOV AH,02H
    INT 21H
    
    MOV DX,Z
    MOV DL,DH
    ADD DL,'0'          ;输出Z的低位
    MOV AH,02H       ;调用dos系统2号功能
    INT 21H            ;中断
    MOV DX,Z
    ADD DL,'0'
    MOV AH,02H
    INT 21H
    ;此处输入代码段代码
    MOV AH,4CH
    INT 21H
CODES ENDS
    END START
```

[◀返回目录](#目录)

<a name="shiyan4"> </a>
## 串操作
***串拷贝***
```asm
DATAS SEGMENT
    ;此处输入数据段代码 
    STR1 DB 'fmw2017110110 $'     ;定义一串字符
    
    STR2 DB 20 DUP(?)    ;定义作为拷贝对象
DATAS ENDS

STACKS SEGMENT
    ;此处输入堆栈段代码
STACKS ENDS

CODES SEGMENT
    ASSUME CS:CODES,ES:DATAS
START:
    MOV AX,DATAS    
    MOV ES,AX         
    ;此处输入代码段代码
    
    LEA SI,STR1     ;把STR1存在SI中
    LEA DI,STR2     ;把STR2存在DI中
    MOV CX,20      
    CLD            ;使变址寄存器SI或DI的地址指针自动增加
    REP MOVS ES:BYTE PTR[DI],ES:[SI]   ;表明是操作的是字节
    
    LEA DX,STR2   ;把STR2放入到DX中
    MOV AX,ES        
    MOV DS,AX     ;把AX里的传到DS
    MOV AH,9      ;输出字符串
    INT 21H         
    
    MOV AH,4CH
    INT 21H        
CODES ENDS
    END START
```

***串扫描***
```asm
DATAS SEGMENT
    ;此处输入数据段代码 
    STR1 DB 'fmw2017110110'       ;定义字符串
DATAS ENDS


STACKS SEGMENT
    ;此处输入堆栈段代码
STACKS ENDS

CODES SEGMENT
    ASSUME CS:CODES,ES:DATAS
START:
    MOV AX,DATAS       
    MOV ES,AX
    ;此处输入代码段代码
    
    LEA DI,STR1     ;把STR1的有效地址传送到DI中
    MOV AL,'m'      ;将m的ACSII码送到AL
    MOV CX,20       ;计数
    CLD             ;使变址寄存器SI或DI的地址指针自动增加
    REPNE SCASB     ;串扫描
    
    MOV DL,ES:[DI]   ;若相等就停止比较
    MOV AH,2		 ;输出
    INT 21H     
    
    MOV AH,4CH
    INT 21H     
CODES ENDS
END START
```

***串比较***
```asm
DATAS1 SEGMENT
    ;此处输入数据段代码 
    str1 DB 'fanmaowei'    ;定义字符串1
DATAS1 ENDS

DATAS2 SEGMENT 
	str2 DB 'fammaowei'     ;定义字符串2
DATAS2 ENDS

STACKS SEGMENT
    ;此处输入堆栈段代码
STACKS ENDS

CODES SEGMENT
    ASSUME CS:CODES,DS:DATAS1,ES:DATAS2
START:
    MOV AX,DATAS1    
    MOV DS,AX
    MOV AX,DATAS2
    MOV ES,AX
    ;此处输入代码段代码
    
    LEA SI,str1    ;把STR1的有效地址传送到SI中
    LEA DI,str2    ;把STR1的有效地址传送到SI中
    MOV CX,9       ;计数
    CLD            ;使变址寄存器SI或DI的地址指针自动增加
    REPE CMPSB     ;串比较
    
    MOV DL,ES:[DI]     ;相等则停止
    MOV AH,2      
    INT 21H            ;输出
    
    MOV AH,4CH
    INT 21H        
CODES ENDS
    END START
```

***从串中取元素***
```asm
DATAS SEGMENT
    ;此处输入数据段代码 
    STR1 DB 'fmw2017110110' ;定义字符串,后面只取前三位'fmw'
DATAS ENDS


STACKS SEGMENT
    ;此处输入堆栈段代码
STACKS ENDS

CODES SEGMENT
    ASSUME CS:CODES,DS:DATAS
START:
    MOV AX,DATAS
    MOV DS,AX
    ;此处输入代码段代码
    
    LEA SI,STR1     ;把STR1的有效地址传送到SI中
    CLD             ;使变址寄存器SI或DI的地址指针自动增加
    LODSB           ;从串取
    MOV DL,AL
    MOV AH,2        ;输出
    INT 21H
    
    LODSB           ;从串取
    MOV DL,AL
    MOV AH,2        ;输出
    INT 21H
    
    LODSB           ;从串取
    MOV DL,AL
    MOV AH,2        ;输出
    INT 21H
    
    MOV AH,4CH
    INT 21H        
CODES ENDS
END START
```

***将元素存入串***
```asm
DATAS SEGMENT
    ;此处输入数据段代码 
    STR1 DB 3 dup(?),'$'   ;给定未知内容STR1空间
DATAS ENDS

STACKS SEGMENT
    ;此处输入堆栈段代码
STACKS ENDS

CODES SEGMENT
    ASSUME CS:CODES,ES:DATAS
START:
    MOV AX,DATAS
    MOV ES,AX
    ;此处输入代码段代码
    
    LEA DI,STR1
    MOV AL,'f'    ;将f的ACSII码送到AL
    MOV CX,3      ;与串的大小一致
    CLD           ;使变址寄存器SI或DI的地址指针自动增加
    REP STOSB     ;存放入字节当中
    LEA DX,STR1   ;将STR1放到DX中
    MOV AX,ES
    MOV DS,AX     
    MOV AH,9      ;输出
    INT 21H       
    
    MOV AH,4CH     
    INT 21H        
CODES ENDS
END START
```

[◀返回目录](#目录)

<a name="shiyan5"> </a>
## 数组求和
