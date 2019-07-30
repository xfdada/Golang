# go语言学习笔记
----------------------
## 程序

简介：为了让计算机执行某些操作或解决某个问题而编写的一系列有序的指令的集合

## go语言的特点

    简介：go语言保证了既能达到静态语言的安全和性能，又达到了动态语言开发维护的高效率，使用一个表达式来形容go语言 Go = C + Python


1.从c语言中继承了很多理念，包括表达式，语法控制结构，基础的数据类型，调用参数传值，指针等等

2.引入了包的概念，用于组织结构。go语言的一个文件都要归属于一个包，而不能单独存在

3.垃圾回收机制，内存自动回收

4.天然的并发

    1.从语言层面支持并发，实现简单
    2.goroutine，轻量级线程，可实现大并发处理，高效利用多核
    3.基于CPS并发模型（Communication Sequential Processes）实现
5.吸收了管道机制，形成Go语言特有的管道channel通过管道channel，可以实现不同的goroute之间的相互通信。

6.函数可以返回多个值 如下代码所示：
```
    func getSum(n1 int,n2 int))(int,int){
        sum := n1 + n2

        sub : =n1 - n2
        return sum,sub
    }
```
7.新的创新，比如切片slice，延时执行defer等

---------
## go语言的编译和运行

    1.使用 go run 文件名.go     

    2.使用 go bulid 文件名.go 该该方式会编译成一个可执行文件在Windows下是一个exe文件


-------
## go语言开发事项

    1.Go的源文件意“go”为扩展名。

    2.Go应用程序的入口是main（）函数

    3.Go语言严格区分大小写

    4.语句后面不需要分号结尾

    5.GO的编译器是一行一行进行编译的，一行只能写一条语句，否则会报错

    6.go语言 定义的变量 或 引入的包 没有使用的话会报错

    7.大括号跟在方法名后面 如果在下一行的话编译会报错

***
## go语言中的变量定义方式

    1.先声明后赋值的形式，示例如下代码
```
    var num int
    num = 10
```
    2.使用类型推导的方式来定义，示例如下代码
```
    var num1 = 100
```
    3.省略关键字var定义变量，示例如下代码
```
    num2 :=20
```
****
## go语言的数据类型

    1.基本数据类型
        1）.整数型 int8 int16 int32 int64 有符号的int数据
                unint8 uint16 uint32 uint64 和 byte相当于uint8

            go语言中没有指定的话默认为int类型数据长度是根据OS来决定的如果是32为系统那默认就是int32 64位的话就是int64
            Golang程序中整型变量的使用，遵循保小不保大的原则，即：在保证程序正确运行的情况下，尽量使用占用空间小的数据类型。
            bit:计算机中最小的存储单位，一个1byte = 8bit

        2）浮点型数据  float32  float64 两种类型
            有两种写法 一种是 0.123 和 .123
            两种结果都是   num = 0.123000
            科学计数法 5.2e2 结果为520  相当于5.2*10的2次方
            浮点数 = 符号位 + 指数位 + 尾数位
            尾数部分可能丢失，造成精度损失的情况
            在程序中应当使用 float64类型的保证精度不至于损失

        3) 布尔值 bool  true false两个
                占用1个字节
                只能有 true 和 false 不能用 0和 非 0数字来表示

        4）string 类型 在go语言中字符串是由字节组成的如 a 其值为97 在官方文档中把其归为基本数据类型
        在程序中存储单个字母一般用byte来保存
        go语言的字符编码使用的是UTF-8 汉字占用3个字节
        字符串可以进行运算的，相当于一个整数，运算时是按照码值运算的。
        字符型 存储到计算机中
            存储：字符->对应码值->二进制->存储
            读取:二进制->码值->字符->读取

        go语言中字符串一旦赋值了就不能修改了，Go中字符串是不可变的

        go语言中字符串有两种标识方式 （1）双引号，会识别转义字符串（2）反引号“ ` ” 以字符串原生形式输出，包括换行和特殊字符，可以实现防止攻击，源代码等效果输出。
        字符串拼接时 加号要在上一行 ，因为go在编译时会在语句末尾加上分号
        所以要保留在上一行如下示例：
            var str5 = "\nhello " + "world "+"hello " + "world"+"hello " + "world"

    2.派生数据类型

        1）.指针(Pointer)

        2）.数组

        3）.结构体(struct)相当于其他语言的类class

        4）.管道(Channel)

        5）.函数

        6）.切片(selice)

        7）.接口（interface）
        
        8）.map ->相当于集合

### 如何在程序中如何查看某个变量的占用字节大小和数据类型

    引入unsafe包使用unsafe包中的Sizeof(V)方法

        package main
        import (
            "fmt"
            "unsafe"
        )
        func main(){
            var n1 int64 = 32
            var n2 int32 = int32(n1)
            fmt.Printf("n2 %T,n2的值为 %v ,n2占用的字节空间大小为%d",n2,n2,unsafe.Sizeof(n2))
        }

### 关于fmt包中 Printf 中占位符的用法

    1.通用占位符：  
        %v     值的默认格式。
        %+v   添加字段名(如结构体)
        %#v　 相应值的Go语法表示 
        %T    相应值的类型的Go语法表示 
        %%    字面上的百分号，并非值的占位符

    2.布尔值：
        %t   true 或 false
    
    3.整数值：
        %b     二进制表示 
        %c     相应Unicode码点所表示的字符 
        %d     十进制表示 
        %o     八进制表示 
        %q     单引号围绕的字符字面值，由Go语法安全地转义 
        %x     十六进制表示，字母形式为小写 a-f 
        %X     十六进制表示，字母形式为大写 A-F 
        %U     Unicode格式：U+1234，等同于 "U+%04X"

    4.浮点数及复数：
        %b     无小数部分的，指数为二的幂的科学计数法，与 strconv.FormatFloat中的 'b' 转换格式一致。例如 -123456p-78 
        %e     科学计数法，例如 -1234.456e+78 
        %E     科学计数法，例如 -1234.456E+78 
        %f     有小数点而无指数，例如 123.456 
        %g     根据情况选择 %e 或 %f 以产生更紧凑的（无末尾的0）输出 
        %G     根据情况选择 %E 或 %f 以产生更紧凑的（无末尾的0）输出

    5.字符串和bytes的slice表示：
        %s     字符串或切片的无解译字节 
        %q     双引号围绕的字符串，由Go语法安全地转义 
        %x     十六进制，小写字母，每字节两个字符 
        %X     十六进制，大写字母，每字节两个字符
### 基本数据类型转 Strin 类型

    方式一：
        fmt.Sprint("%参数",表达式) Sprintf 根据于格式说明符进行格式化并返回其结果字符串。

    方式二：
        使用strconv包
        func FormatBool(b bool) string

        func FormatFloat(f float64, fmt byte, prec, bitSize int) string  基本类型为float64如果没有转成float64会报错
        示例
        var float1 float32 = 2.3456
        str2 := FormatFloat(float64(float1),'f',10,64)
        参数解释：
         fmt 表示转的格式 有 'e', 'E', 'f', 'g'具体查看文档中http://docscn.studygolang.com/pkg/strconv/#FormatFloat
        prec ：表示保留的小数位数
        bitSize：表示转成什么类型 有32 64两种

        func FormatInt(i int64, base int) string   FormatInt 返回给定基数中的i的字符串表示，对于2 <= base <= 36.结果对于数字值> = 10使用小写字母 'a' 到 'z' 。
            示例：
                str :=strconv.FormatInt(12,10)  输出 “12”
                str :=strconv.FormatInt(12,8)   输出 “14“
                str :=strconv.FormatInt(12,16)   输出 “c“
                int64 的值转换成对应进制的值并输出 string 类型
        func FormatUint(i uint64, base int) string
### string类型转成基本数据类型

        1.string转bool类型
            var str = "t"
        ！注意 string转其他基本数据类型的返回值中有两个值，第一个为返回的数据类型值，第二个值为错误
        可以用下划线来忽略该错误值如下示例所示
        ParseBool返回由字符串表示的布尔值。它接受1，t，t，真，真，真，0，f，f，假，假，假。任何其他值都返回错误。
            b ,_:= strconv.ParseBool(str)

        2.strin转float类型
             var str2 = "123.456"
        ParseFloat默认返回的类型是float64 如果要转成float32 需要再做处理
             flat,_ := strconv.ParseFloat(str2,64)
            var money float32 = float32(flat)

        3.string转int类型
            var str3 = "14"
            num,_ :=strconv.ParseInt(str3,10,64)
             ParseInt(s string, base int, bitSize int) 
                s：表示需要转换的字符串
                base:是根据s的类型来的 0代表是默认10进制 2如果s = ”1000“那么输出的值为二进制1000的十进制值为8，以此类推 s = “010” 那么输出值为 8 s =“0x10” 那么输出值为 16
            bitSize： 0,8,16,32,64分别对应 int ，int8 int16 int32 int64 s的值不应大于bitSize中的最大范围值，如果大于只会返回 0 值
***
## 指针 Pointer

### 基本介绍
    1.基本数据类型，变量存的就是值，也叫值类型

    2.获取变量的地址 用 & 符号来获取，如 var num int = 10 获取num的地址为：&num

    3.指针类型，指针变量存储的是一个地址，这个地址指向内存空间存的才是值 如 var pre *int = &num

    4.获取指针指向的值 用 *pre 来获取值

#### 指针使用注意事项

    1.源数据是什么值就得用什么值来接收 如 var num int64 = 10  var ptr float32 = &num 这就编译通不过，因为类型不一样。

    2.值类型， 都有对应的指针类型，形式为 *数据类型， 比如int对应的指针类型就是 *int float 对应的是 *float

    3.值类型包括 ：基本数据类型 int系列 float系列 bool string ，还有数组和结构体（struct）
***
## 值类型和引用类型

    1.值类型 ，基本数据类型 int系列 float系列 bool string ，还有数组和结构体（struct）

    2.引用数据类型，指针，slice切片 map，管道channel，interface接口等

### 值类型和引用类型的使用特点

    1.值类型：变量直接存储值，内存通常在栈中分配

    2.引用数据类型：变量存储的是一个地址，这个地址对应的空间才是真正存储的值，内存通常在堆上分配，当没有任何变量引用这个地址时，该地址对应的数据空间就成为一个垃圾，有GC来回收。

***

## 标识符的命名规范（重点）

    Golang 对各种变量，方法，函数等命名使用的符号序列称为标识符
    凡是可以自己命名的都叫标识符
### 标识符的命名规则
    1.由26个英文字母，和_组成

    2.标识符不能由数字开头

    3.Golang中严格区分大小写

    4.标识符不能包含空格

    5.下划线 “_”本身是一个特殊的标识符，称为空标识符，可以代替任何标识符，但是它对应的值会被忽略掉，所以仅能被作为占位符使用，不能作为标识符使用。

    6.不能用系统保留关键字作为标识符（25）个如 break if等等

### 标识符命名规范

    1.包名：保持package的名字和目录保持一致，尽量采取有意义的包名，剪短，不能好标准库中同名

    2.变量名，函数名，常量名采用驼峰命名法

    3.如果变量名，函数名，常量名首字母大写 ，则可以被其他的包访问，如果首字母小写只能在本包使用（可以这样理解：大写表示公有 其他语言的public 小写表示私有 其他语言的private）


### 包的引入事项

    1.用相对路径进行引入时要注意 环境变量所在的位置GOPATH定义的位置在哪，才能正确的引入包，如我的环境变量中的位置在 C:/goAPP 所以引包时是可以省略 我的文件位置在goApp/src/lesson3/pack下 所以引入时用 import "lesson3/pack"

    2.使用包中的变量，方法，函数，常量等使用 包名.变量或方法如 pack.Nmae
***
## 运算符介绍

    1.算术运算符
        + - * / % ++ --
       / 参与运算的都是int类型 会舍弃小数部分 10 / 4 = 2

       在Golang中没有前++ 和前--操作  只能独立操作
        Golang去掉了 自增和自减容易混淆的写法，让golang更加简洁统一
        如：var num int = 1   num = num++ 这种写法是错误的，编译不通过

    2.赋值运算符

    3.比较运算符 /关系运算符

    4.逻辑运算符

    5位运算符

    6.其他运算符

***

## 获取用户终端输入

### fmt.Scanln

    读取一行内容，回车继续，程序运行后程序在Scanln处等待用户输入数据，输入完成后回车继续下一行代码

    示例：
```
    func main(){
	var age byte
	var name string
	var swl float32
	var isMarray bool
	fmt.Println("我的年龄是")
	fmt.Scanln(&age)
	fmt.Println("我的名字是")
	fmt.Scanln(&name)
	fmt.Println("我的薪水")
	fmt.Scanln(&swl)
	fmt.Println("是否结婚")
	fmt.Scanln(&isMarray)
	fmt.Printf("我的名字是%v \n 我的年龄是%v \n 我的薪水%v \n 是否结婚%v",name,age,swl,isMarray)
    }
```

### fmt.Scanf()
    获取一行数据内容 
```
	fmt.Println("年龄，姓名，薪水，已婚")
	fmt.Scanf("%d %s %f %t",&age,&name,&swl,&isMarray)
```

***
## 进制转换

### 二进制转其他进制
    1.二进制转8进制 
        如：110 101 转8进制为 为065 每三位的值为一位8进制的值

    2.二进制转16进制
        如：110110 转16进制为 0x36 每四位的值为一位16进制的值

### 其他进制转二进制
    1.8进制转二进制
        8进制每一位值对应三位二进制的值，如：010 对应二进制为1000 
    2.16进制转二进制
        16进制每一位值对应4位二进制的值，如：0x10对应二进制位10000

***
### 循环

### if else

    if 表达式 {
        语句块
    }else{
        语句块
    }

    注意事项：Golang语言中else不能换行写否则编译通不过
### switch 
    注意事项：case 后方表达式可以有多个表达式，用逗号隔开
            case 下的语句块后方不需要 break
    使用细节：
        1.表达式可以是 常量 变量 表达式 甚至是运算都可以
        2.case 表达式的值和switch后面的数据类型需要保持一致
        3.后方表达式可以有多个表达式，用逗号隔开
        4.case 后边的表达式是一个常量（字面量）不能重复
        5.case后面不需要带break，程序匹配到一个case执行完语句块后，然后退出switch ，如果一个都匹配不到则会走default
        6.default 是不必须的
    switch 表达式{
        case 表达式1，表达式2：
            语句块
        case 表达式1，表达式2，...：
            语句块
        default：
            语句块
    }
