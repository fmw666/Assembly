# 汇编语言基础
*对于汇编这门语言来说，了解是很必要的。无论外面的世界高级语言如何变幻，谁强又谁弱，哪个被淘汰哪个又兴起，作为汇编这门语言来说，云淡风轻，因为底层，就是不一样哈~大家都是靠他来完成与硬件的对接，逻辑的实现。所以简单介绍与学习一下这门语言*

## 目录

***一、基础介绍***

  1. [汇编语言简介](#jichu1)
  1. [80×86 计算机组织]()
  1. [80×86 的指令系统和寻址方式]()
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
汇编语言是大学许多基础课程的基础（先行课），并不说明汇编语言很基础，而是它很重要！它作为C语言、计算机组成原理、操作系统等大学课程的基础课，不仅能为我们打下牢固的计算机学习地基，也能让我们建立起一个完整的计算机学习构架。

学来只是为了过渡？of course not！它也可以用作程序开发（虽然语法很不友好），由于偏下底层，所以效率极高，作为高效率程序开发是个不二之选。但是如果选它其实还是蛮二的...不说这个，由于偏向底层硬件，所以在嵌入式开发中仍是有一席之地。总之，汇编语言很重要！

在学习中，我采用的是网上[Masm for Windows](http://www.skycn.com/soft/appid/86368.html)汇编编译器（点击跳到下载页面），语言嘛，工具不重要，学习到其中的东西才是最主要的。当然后续的实际开发中工具合不合适，上不上手才是程序员最应该考虑的，当前学生阶段还是不要太挑了。

后面不罗嗦了，大家一起加油吧！

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
