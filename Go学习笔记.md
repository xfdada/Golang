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
## 字符遍历

    for-range   按字符的方式来遍历的 示例如下：

    func main(){
        var str string = "abc123全南中学"

        for index,val := range str {
            fmt.Printf("index=%d,val = %c \n",index,val)
        }
    }
    输出结果为
    index=0,val = a
    index=1,val = b
    index=2,val = c
    index=3,val = 1
    index=4,val = 2
    index=5,val = 3
    index=6,val = 全
    index=9,val = 南
    index=12,val = 中
    index=15,val = 学
  

## 产生随机数
    import "math/rand"  包
    rand.Seed(time.Now.UnixNano())：随机因子 每次随机的时间，当前时间

    rand.Intn(int) int为随机范围 [0，int）

## 包

### 包的概念
    go的文件都是属于一个包的，也就是说go是以包的形式来管理文件和项目目录结构的
### 包的三大作用

    1.区分相同名字的函数、变量等标识符
    2.当程序文件很多时可以很好的管理项目
    3.控制函数、变量等访问范围，即作用域

### 引入包的注意事项
    1.给一个文件打包时，该包对应一个文件夹，包名和文件名应该一致
    2.在引入包时，路径从$GOPATH的src下开始
    3.为了让其他包的文件能够访问到夲包的函数和方法，其首字母应该大写
    4.访问其他包的函数时，应该用  包名.函数名
    5.给包取别名  tool "lesson5/tools" 用法是在包名前面加一个标识符，原来的包名无法引用其本身的方法，应该用别名来访问 如 tool.help（）

## 函数

### return 语句

    Go函数支持多个返回值
    func 函数名（形参列表）（返回值类型列表）{
        语句。。。
        return 返回值列表
    }

    1.如果返回多个值是，在接收时如果不想要其返回值可以用 _ 符号忽略
    2.如果只返回一个值可以省略 （）

### 递归调用

    解释：一个函数在函数体内又调用了本身，称其为递归调用。
    示例：求阶乘
    func DiGui(num int)(int){
        if((num-1)==0){
            return 1;
        }
        sum := num * DiGui(num-1)
        return sum
    }
    递归遵从的原则：
    1.执行一个函数时，就会创建一个新的独立的空间
    2.函数的局部变量是独立的，不会相互影响
    3.递归必须向退出的条件逼近，否则就是无限递归
    4.当一个函数执行完毕时，或者遇到return，就会返回，当函数执行完毕时函数本身会被销毁。

### 函数的使用细节
    1.函数的形参可以是多个，返回值也可以是多个

    2.形参列表和返回值列表的数据类型可以是值类型和引用类型

    3.函数的命名规范遵循标识符的命名规范，其他包引用时首字母大写

    4.函数中的变量是局部变量

    5.基本数据类型和数组都是值传递，即进行值拷贝，在函数内修改不会影响原值

    6.如果想要进行引用传递，可以用指针的方式来实现，即传入变量的地址，&num在函数内以指针的方式来操作变量。

    7.Go函数不支持重载
    
    8.在Go中函数也是一种数据类型，可以赋值给一个变量，则该变量就是一个函数类型的变量，通过该变量对函数进行调用

    9.函数是一种数据类型，因此在Go中，函数可以作为形参，并且调用

    10.为了简化类型的定义，Go支持自定义数据类型
    基本语法：type 自定义数据类型名 数据类型 理解相当于一个别名
    type myInt int //这时myInt你就相当于int来使用
    虽然myInt和int都是int类型但是 go认为是两种数据类型，所以需要进行类型转换才能 赋值给 int类型的变量

    11.支持对函数返回值命名
        func getSumAndSub（n1 int,n2 int）（sum int,sub int）{
            sum = n1 + n2
            sun = n1 - n2
            return
        }

    12.Go支持可变参数
        支持0到n个参数
        func sum（args...int）sun int{
        }
        支持1到n个参数
        func sum（n1 int,args...int）sum int{
        }
        args 是slice切片，通过args[index]可以访问到各个值
        如果一个函数的形参列表中有可变参数时，则可变参数需要放在形参列表最后
### init函数
    介绍：每一个源文件都可以包含一个init()函数，该函数会在main（）函数之前执行 init函数的主要作用为王城一些初始化的工作

    如果一个文件中同时包含全局变量定义，init函数和main函数，则执行流程为 全局变量定义-》init函数-》main函数

    main.go                 utils.go
    import "utils"          变量定义5
    变量定义       2         init（）函数6
    init（）函数3
    main（）函数4

    上述代码执行顺序为 5-6-2-3-4

### 匿名函数

    介绍：Go支持匿名函数，匿名函数没有函数名，如果某个函数只执行一次，可以考虑使用匿名函数，匿名函数也可以实现多次调用

    调用方式一：直接调用

        func main(){
            res := func(n1 int,n2 int) int{
                return n1+n2
            }(10,20)
        } 
    调用方式二：将匿名函数赋给一个变量再通过变量来调用匿名函数

   
        func main(){
            a :=func(n1 int, n2 int)int{
                    return n1*n2
                }
            res := a(10,20)
        }
    全局匿名函数：
    如果将匿名函数赋值给一个全局变量，那么这个匿名函数就成为了一个全局匿名函数，可以在程序有效范围内
### 闭包
    返回的是一个匿名函数，但是这个匿名函数引用到函数外的n，因此这个匿名函数就和n形成一个整体，这就构成了闭包，如下示例代码

    func Addup()func (int)int{
        var n int = 10
        return func (x int) int{
            n += x
            return n
        }
    }

## 常用字符串函数
    以下函数需要引入import “strings”包

    1.统计字符串长度，按字节  len（str） 是一个内置函数

    2.字符串转整数
	n,rr := strconv.Atoi("123")
	if err !=nil{
		fmt.Println("转换错误",err)
	}else{
		fmt.Println("转换的结果是",n)
	}

	3.整数转字符串
	str4 := strconv.Itoa(2322)
	fmt.Printf("str的类型为%T,值为%v \n",str4,str4)

	4.字符串转byte
	var list = []rune("xiang")
	fmt.Printf("list = %c \n",list)

	5.[]byte 转字符串 
	str5 := string([]byte{65,66,67})
	fmt.Printf("str5 = %v \n",str5)

	6.10进制转2,8,16进制
	str5 = strconv.FormatInt(123,16)
	fmt.Printf("str5 = %v \n",str5)

	7.查找字符是否在字符串中
	b  := strings.Contains("hello","o")
	fmt.Printf("b = %v \n",b)

	8.统计一个字符串有几个指定字符串
	c := strings.Count("hello","le")
	fmt.Printf("c = %v \n",c)

	9.不区分大小写的字符串比较（==是区分大小写）
	b = strings.EqualFold("abc","ABc")
	fmt.Printf("b = %v \n",b)

	10.返回子串在字符串第一次出现的位置，如果不存在的话，返回-1
	c = strings.Index("小白兔白又白","白")
	fmt.Printf("c = %v \n",c)    //3 一个汉字占用三个字节

	11.返回子串在字符串最后一次出现的位置，如果不存在的话，返回-1
	c = strings.LastIndex("小白兔白又白","白")
	fmt.Printf("c = %v \n",c)    //15 一个汉字占用三个字节

	12.将指定的子串 替换成另一个子串 strings.Replace(b1,"白","黑",n) 
	// n可以指定你希望替换几个，如果n=-1时表示全部替换
	b1 := "小白兔白又白"
	str6 :=strings.Replace(b1,"白","黑",-1)
	fmt.Printf("str6 = %v \n",str6)

	13按照指定的某个字符。为分隔标识，将一个字符串拆分成数组
	strArr :=strings.Split("小白兔白又白","白")
	fmt.Printf("strArr = %v \n",strArr)


    14.将字符串的字母进行大小写转换 strings.ToLower（）strings.ToUpper()
	str4 = "abcdEFG"

	str2 = strings.ToLower(str4)
	str2 = strings.ToUpper(str4)
	fmt.Printf("str2 = %v \n",str2)

	15.去掉字符串两端的空格 strings.TrimSpace()
	str4 = " aaabbb "
	str4 = strings.TrimSpace(str4)
	fmt.Printf("str4 = %v \n",str4)

	16.去掉字符串两端的指定字符 strings.Trim()
	str4 = "* 刘大哥，诶，我滴郎诶 *"
	str4 = strings.Trim(str4," *")
	fmt.Printf("str4 = %v \n",str4)

	17.判断字符串是否以指定字符串开头 strings.HasPrefix(str,need)

	str4 = "abcdEFG"
	s :=strings.HasPrefix(str4,"a")
	fmt.Printf("s = %v \n",s)

	18.判断字符串是否以特定字符结尾 HasSuffix(str,need)

	file :="123.jpg"
	s = strings.HasSuffix(file,".jpg")
	fmt.Printf("s = %v \n",s)

### 时间函数
    介绍：使用时间日期相关函数时需要引入 import "time"包

    time.Now().Format("2006-01-02 15:04:05")
    Format（“2006-01-02 15:04:05”）里面时间为固定的，数字不能修改
    time.Now().Format("2006-01-02 15:04:05")返回的是当前时间

    时间常量{
        Nanosecond Duration = 1  //纳秒
        Microsecond         =1000*Nanosecond //微秒
        Millisecond         =1000*Microsecond//毫秒
        Second              =1000*Millisecond//秒
        Minute              =60*Second
        Hour                =60*Minute
    }

    Unix和UnixNano

    time.Now().Unix()为当前的秒数
    time.Now().UnixNano()为当前的纳秒数

### 错误处理
    Go中会抛出一个panic的异常 然后在defer中通过recover捕获这个异常
    recover()是一个内置函数 
    func test() int{
        defer func(){
            err := recover() //该函数会自动捕获错误
            if err !=nil{
                fmt.Println("发生错误——",err)
            } 
        }()
        num :=10
        num2 :=0
        res := num/num2
        return res
    }
    func main(){
        test()
        fmt.Println("main********")
    }

    上面的代码错误会打印出来，并且还会继续执行


    自定义错误：
    需要引入 import “errors”
    Go程序中，支持自定义错误，使用errors.New()和panic内置函数
    具体流程是：
    1.errors.New(“错误说明”),会返回一个error类型的值，表示一个错误
    2.panic内置函数，接收一个interface{}类型的值（也就是任何值）作为参数。可以接收error类型的变量，输出错误信息，并且退出程序

    func config(name string)(err error){
        if name=="config.ini"{
            return nil
        }else{
            return errors.New("读取文件错误。。。。")
        }
    }

    func test01(){
        err := config("config1.ini")
        if err != nil{
            panic(err)
        }
        fmt.Println("继续执行下面代码......")
        }
    func main(){
        test01()
        fmt.Println("main********")
    }
    上面代码示例解读 test01（）调用config并且传入config1.ini 由于config.ini不等于config1.ini所以创建一个自定义错误，由于panic会捕获错误并且会中断程序的执行。
## 数组和切片

### 数组
    数组可以存放多个同一类型的数据。数组也是一种数据类型，在Go中，数组是值类型

    数组的定义 var 数组名[数组大小]数据类型

    定义：var a[5]int  赋值： a[0] = 1,a[2]=20
    数组的地址可以通过数组名来获取
    数组的第一个元素的地址就是数组的首地址
    数组的地址是连续的如int64 则第二个元素的地址为首地址加上8
    &a[0] =>0x21525500   &a[1]=>0x21525508
    四种初始化数组的方法
    第一种：
        var array[3]int = [3]int{1,2,3}
    第二种：
        var array1 = [3]int{4,5,6}
    第三种:
        var array3 = [...]int{7,8,9}
    第四种：
        array4 := [...]int{10,11,12}

### 数组的遍历
    1.常规遍历 
        for i:=0;i<len(array);i++{
            fmt.Println(array[i])
        }
    2.for-range
        for index,value:=range array01{

        }
    for- range说明
     for index,value:=range array01{
    }
    1.index是数组的下标
    2.value 是该下标的值
    3.他们都是在for循环内部的变量
    4.遍历数组到时候，如果不想使用下标index，可以用下划线_
    5.index和value不是固定的，可以随便取名字
### 数组使用细节
    1.数组是多个相同类型的数据组合，一个数组一旦声明了，他的长度是固定的，不能动态变化 示例：
        var arr[3]int = [3]int{1,2,3}
        arr[2] = 1.1 //报错 ，原因是数据类型不是整数

        arr[3] = 10	//报错 原因是超出数组长度

    2.数组中的元素可以是任何数据，包括值类型和引用类型，但是不能混用

    3.数组创建后如果没有赋值会有默认值 数值型为 0 字符串为空字符串 bool为 false

    4.数组下标必须在指定范围内使用，否则包panic：数组越界

    5.Go的数组属于值类型，在默认情况下属于值传递，因此数组间不会互相影响

    6.如果想要在函数中修改原来的数组，可以引用传递的方式来（指针）
        示例：
        func arr(arr*[3]string) {
            fmt.Printf("arr[1]的地址为%v \n",&arr[1])
            fmt.Printf("arr[1]的值为%v \n",arr[1])
            arr[2] = "小红"
        }
        name :=[...]string{"小明","小玲","小猪"}
        arr(&name)

    7.长度是数组类型的一部分，在传递函数参数时需要考虑数组的长度

### 切片 slice
    介绍：
        1.切片是数组的一个引用，因此切片是引用类型，在进行传递时，需要遵守引用传递的机制
        2.切片和数组类似，遍历切片和长度计算与数组一致
        3.切片的长度是可变的，是一个动态变化的数组
        4.基本语法： var 切片名[]数据类型 如 var name[]string

    slice的使用：
        方法一：定义一个切片，然后让slice去引用创建好的数组，如下列代码所示：
            var arr[]int = [...]int{1,2,3,4}
            var slice = arr[1:3]
            slice是将arr数组中下标为1到下标为2的值存储到slice中

        方法二：通过make来创建slice  用法 var a[]type = make([]type,len,[cap]) 参数解析 []type为切片数据类型，len为切片长度，cap为容量，可选 如果分配了cap，那么cap>=len
            1.通过make方式创建slice可以指定切片的大小和容量
            2.如果没有给切片各个元素赋初值那么就会使用默认值
            3.通过make方式创建切片对应的数组是由make底层维护，对外不可见，只能通过slice去访问各个元素

        方法三：定义一个slice直接就指定具体数组，使用原理类似make的方式
        示例：
	    var movies[]string = []string{"罗小黑战记","大鱼海棠","齐天大圣"}

    切片的遍历和数组一致：for和for-range两种方式
### slice切片的使用注意事项
    1.切片初始化时 var slice = arr[startIndex:endIndex]
    说明：从arr数组下标为startInde，取到下标为endIndex的元素（不包含arr[endIndex]
    2.切片初始化时，仍然不能越界。范围在[0-len(arr)]之间，但是可以动态增长
    var slice = arr[0:end]可以简写成 var slice = arr[:end]
    var slice = arr[start:len(arr)]可以简写成 var slice = arr[start:]
     var slice = arr[0:len(arr)]可以简写成 var slice = arr[:]

     3.cap是一个内置函数，用于统计切片的容量，即最大可以存放多少个元素

     4.切片定义完后还不能使用，因为本身是一个空的，需要让其引用到一个数组，或者make一个空间

     5.切片可以继续切片 继续切片后的数据指向的是同一个数据地址

     6.使用append内置函数可以对切片进行动态追加
        	var movies[]string = []string{"罗小黑战记","大鱼海棠","齐天大圣"}
	        movies = append(movies,"复仇者联盟4")       
        解释上述代码：append本质上是创建了一个新的数组，把原来数组的值拷贝了一份到新的数组上 movies重新引用到新的数组上 新的数组是在底层操作，程序员是不可见的
    7.切片的拷贝操作
        使用copy内置函数完成拷贝
        copy(new,old) new 表示需要拷贝到是切片，old是被拷贝的切片
        new 和old的数据空间是独立的，互不影响
        当new的长度小于old的长度时 只会拷贝到new长度的数据

## map
    介绍：map是key-value数据结构，有称为字段或者关联数组。类似于集合

    声明：var map 变量名     map[keytype]valuetype

    key的类型：bool，数字，string，指针，channel还可以是接口，结构体，数组。 通常key为int和string
    slice，map和function不能用来作为key

    map在使用前一定要make
    map的key是不能重复的，如果重复了则为最后这个key-value为准
    map的value是可以相同的
    map的key-value是无序的

### map的三种定义形式
    第一种：
    var a map[string]string
    a = make(map[string]string,10)
    第二种：
    citys :=make(map[string]string,10)
    第三种：
    student := map[string]string{
        "stu1":"xiaoming",
        "stu2":"lihua",
    }
### map的crud
    map更新 map[key] = value 如果key存在那么就更新其value
    如果key不存在的话，那么就是增加

    map删除：delete（map,"key"），key是一个内置函数，如果key存在，就删除该key-value如果key不存在的话不操作，但是也不会报错

    注意如果要删除map所有的key，没有一个专门的方法一次性删除，可以遍历可以逐个删除，或者map = make(...),make一个新的。让原来的成为来及，被gc回收


## 面向对象

### Golang面向对象编程说明

    1.Golang 也支持面向对象编程（OOP），但是和传统的面向对象编程有区别，并不是纯粹的面向对象语言

    2.Golang没有类（class），Go语言的结构体（struct）和其他语言的类是同等地位的

    3.Golang面向对象编程非常简洁，去掉了传统的OOP语言的继承，方法重载，构造函数和析构函数，隐藏的this指针等等

    4.Golang仍然有面向对象的继承，封装和多态的特性，只是实现方式和其他语言不一样，Golang没有extends关键字，继承是通过匿名字段来实现的

    5.Golang面向对象很优雅，OOP本身就是语言系统的一部分，通过接口关联，耦合性低，也非常灵活。
### 声明struct
    type 结构体名称 struct{
        field type
        field2 type
    }

    type Cat struct{
        Name string
        Age  int
    }

    字段都有默认值

    int-> 0 ，string-> “” bool->false  指针，slice，map->nil 即没有分配空间

#### 创建struct变量和访问struct字段
    方式一：直接声明
        var person Person
        person.name = "天明"

    方式二：{}
        var person Person = Person{}

    方式三： -&
    var person *Person = new（Person）
    这是person是一个指针，可以通过(*person).nam=""来访问，但是也可以直接通过person.name=“”来访问 因为底层会对person.name进行处理，编译器会自动加上取值运算（*person）.name = “”
    方式四：
        var p *Person = &Person{}
        该方式是通过指针访问的，标准的访问字段的方法是（*p）.name=""
        但是为了程序员的使用方便也可以通过p.name=“”来访问原因是底层会对p.name进行处理，编译器会自动加上取值运算（*p）.name = “”

    struct是值类型，默认是值拷贝。当通过指针来访问赋值变量时如下示例：
        type Dog struct{
            name string
        }

        var d = Dog{name:"小黑"}
        var dd *Dog = &d
        (*dd).name
        只能用小括号把对象包起来用，因为.的运算符优先级高于*

#### struct使用注意事项和细节
    1.结构体的所有字段在内存中是连续的

    2.结构体是用户单独定义的类型，和其他类型进行装换时需要有完全相同的字段

    3.结构体进行type重新定义时，Golang认为是新的数据类型，但是相互间可以强转

    4.struct的每一个字段上都可以写上一个tag，该tag可以通过反射机制获取，常见的使用场景就是序列化和反序列化

#### struct方法的定义和声明
    type A struct{
        Num int
    }

    func (a A)test(){
        fmt.Println(a.Num)
    }
    (a A)体现 test方法是和A类型绑定的


    声明：

    func (recevier type)menthodName(参数列表)(返回值列表){
        方法体
        return 返回值
    }
    recevier type:表示这个方法和type这个类型进行绑定，或者说该方法作用于type类型
    type可以是struct 也可以是其他自定义类型

    方法的使用注意事项和细节

    1.struct是值类型，在方法中调用遵循传递机制，是值拷贝的传递方式
    2.如果希望修改struct变量的值，可以通过结构体指针的方式来处理
    3.Golang中方法用在指定的数据类型上，因此自定义类型都可以有方法
    4.方法的访问控制范围和函数一致
     

### 工厂模式
    使用工厂模式实现跨包创建结构体实例，如果model包的结构体变量首字母大写，在引入后其他包可以直接使用，但是结构体实小写时就无法引入。所以可以用工厂模式解决

    工厂模式如下示例所示：

    type student struct{
        Name string
        Score int
    }

    func NewStu(n string,s int)*student{
        return &student{
            Name:n,
            Score:s,
        }
    }

    代码解释：工厂模式的原理是通过有个对外访问的方法，在其方法中对该结构体实现创建实例，返回该实例的地址。
