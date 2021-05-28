# Golang
## 语言结构
```go
package main

import "fmt"

func main() { // { 无法单独成行！！
   /* 这是我的第一个简单的程序 */
   fmt.Println("Hello, World!")
}
```
* 包声明
1. 文件名、文件夹名与包名没有直接关系，文件名、文件夹名与包名称可以不一致。
2. 同一个文件夹下的文件只能有一个包名，否则编译报错
* 行分隔符
1. 不需要;结尾，编译器自动处理
* main函数
1. main函数是每个可执行程序必须，且是程序启动后第一个执行的函数（若有init函数，则会先执行init）
* 注释
1. // 单行注释 /* */ 多行注释
* 标识符
1. 常量、变量、类型、函数名、结构字段等等，若大写字母开头，则为外部包代码可访问，类似public，如小写字母开头，则包外不可见，但则包内部可见，类似protected
2. 字母、数字、下划线组合，但开头必须为字母或者下划线
* 关键字
![image](https://user-images.githubusercontent.com/41630875/119616940-d02aaf80-be33-11eb-96e5-9fe8dcd50fff.png)

## 数据类型
1. 布尔型
true | false.  var b bool = true
2. 数字类型
* 整型：unit8、unit16、uint32、uint64、int8、int16、int32、int64
* 浮点型：float32、float64、complex64、complex128
* 其他：byte（int8）、rune（int32）、uint（32或64）、int（32或者64）、uintptr
3. 字符串类型string
4. 派生类型
指针Pointer、数组、结构体struct、Channel类型、函数类型、切片类型、接口类型、Map类型

## 变量
* 变量申明
1. `var identifier[,identifier2] type`
   
   指定类型声明，若没有初始化，会自动初始化为零值，整型就是0，布尔就是false，字符串为""，其他为nil
2. `var identifier[,identifier2] = value[,value2]`
   
   声明一个变量，并根据值自行判断类型
3. `identifier[,identifier2] := value[,value2] `
   
   声明一个变量，并自动推断类型。可以使用_替换掉不需要的变量，代表忽略改变量

* 指针
1. 值类型的变量直接指向存在的内存中的值
2. 引用类型变量存储的是变量的内存地址
3. 可以使用&identifier来获取指针ptr，而对指针而言是要\*ptr来获取实际指针指向的值

## 常量
1. 常量声明 
`const c_name1[,c_name2] [type] = value1[, value2]`
2. 常量枚举 
```go
const (
   Unknown = 0
   Female = 1
   Male = 2
)
```
3. iota，特殊常量，在const关键字出现时将被重置为0，const中每新增一行常量iota计数一次，中间若出现其他赋值语句，则直到重新出现iota赋值语句才恢复iota的数据输出
```golang
package main

import "fmt"

func main() {
    const (
            a = iota   //0
            b          //1
            c          //2
            d = "ha"   //独立值，iota += 1
            e          //"ha"   iota += 1
            f = 100    //iota +=1
            g          //100  iota +=1
            h = iota   //7,恢复计数
            i          //8
    )
    fmt.Println(a,b,c,d,e,f,g,h,i)
    // 0 1 2 ha ha 100 100 7 8
}
```

## 运算符
* 算数运算符：+ - * %、 % ++ --
* 关系运算符：== != > < >= <= 
* 逻辑运算符：&& || !
* 位运算符：& | ^ >> <<
* 赋值运算符：= += -= \*= /= %= <<= >>= &= ^= |=
* 其他：&a返回变量地址、\*a指针变量

## 条件语句
* if
```go
if 布尔表达式 {
   /* do something while true */
}

if 布尔表达式 {
   /* 在布尔表达式为 true 时执行 */
} else {
  /* 在布尔表达式为 false 时执行 */
}
```

* switch
```go
switch var1 {
    case val1:
        ...
    case val2:
        ...
    case val3,val4,val5;
        ...
    case val6:
        fallthrough // 不加fallthrough默认直接break
    default:
        ...
}
```

* select
select常用于通信控制，每个case必须是一个通信操作，发送或者接口。select随机执行一个可运行的case，若无case可运行，将阻塞直到有case可运行。默认子句总是可运行的，默认字句将在无case可运行的时候执行。
```go
select {
    case communication clause  :
       statement(s);      
    case communication clause  :
       statement(s);
    /* 你可以定义任意数量的 case */
    default : /* 可选 */
       statement(s);
}
```
1. 每个case都必须为chan
2. 所有chan都会被求值
3. 所有被发送的表达式都会被求值
4. 每次只会随机执行一个可执行的通信，其他的被忽略。如果一个都没有，会执行default，如果连default都没有，会阻塞。

## 循环语句
* for
for循环存在3中写法，init为赋值表达式，condition为逻辑表达式，post为后置表达式
`for init; condition; post {}`
`for condition {}`
`for {}`

样例1，对map就行迭代
```go
for key,value := range oldMap {
    newMap[key] = value
}
```

* break

break支持中断当前循环，并直接中断到label标记的循环
```go
	fmt.Println("----break without label----")
	for i:= 1; i <= 3; i++ {
		fmt.Printf("i: %d\n", i)
		for i2 := 11; i2<= 13; i2++ {
			fmt.Printf("i2: %d\n", i2)
		}
	}

	fmt.Println("----break WITH label----")
	re:
		for i := 1; i <= 3; i++ {
			fmt.Printf("i: %d\n", i)
			for i2 := 11; i2 <= 13; i2++ {
				fmt.Printf("i2: %d\n", i2)
				break re
			}
		}
```

* continue

continue支持跳过当次循环，并直接跳转到label标记的循环，继续开始
```go
	// 不使用标记
	fmt.Println("---- continue ---- ")
	for i := 1; i <= 3; i++ {
		fmt.Printf("i: %d\n", i)
		for i2 := 11; i2 <= 13; i2++ {
			fmt.Printf("i2: %d\n", i2)
			continue
		}
	}

	// 使用标记
	fmt.Println("---- continue label ----")
	re2:
		for i := 1; i <= 3; i++ {
			fmt.Printf("i: %d\n", i)
			for i2 := 11; i2 <= 13; i2++ {
				fmt.Printf("i2: %d\n", i2)
				continue re2
			}
		}
```

* goto

goto支持跳过本次循环并回到循环开始语句loop处
```go
	/* 定义局部变量 */
	var a int = 10

	/* 循环 */
	LOOP: for a < 20 {
		if a == 15 {
			/* 跳过迭代 */
			a = a + 1
			goto LOOP
		}
		fmt.Printf("a的值为 : %d\n", a)
		a++
	}
```

## 函数
```go
func function_name( [parameter list] ) [return_types] {
    function body
}
```

* 值传递与引用传递
```go
func main() {
	var1 := 1
	var2 := 2
	fmt.Println("swap init", var1, var2)
	swap1(var1, var2)
	fmt.Println("swap result", var1, var2)
	swap2(&var1, &var2)
	fmt.Println("swap result", var1, var2)
}

func swap1(x int, y int) {
	var tmp int
	tmp = x
	x = y
	y = tmp
}

func swap2(x *int, y *int)  {
	var tmp int
	tmp = *x
	*x = *y
	*y = tmp
}
```

## 变量作用域
三种变量：
1. 函数内定义 -> 局部变量
2. 函数外定义 -> 全局变量
3. 函数定义中的 -> 形式参数

局部变量、形参若与全局变量重名，则局部变量、形参优先
```go
package main

import "fmt"

/* 声明全局变量 */
var a int = 20;

func main() {
   /* main 函数中声明局部变量 */
   var a int = 10
   var b int = 20
   var c int = 0

   fmt.Printf("main()函数中 a = %d\n",  a);
   c = sum( a, b);
   fmt.Printf("main()函数中 c = %d\n",  c);
}

/* 函数定义-两数相加 */
func sum(a, b int) int {
   fmt.Printf("sum() 函数中 a = %d\n",  a);
   fmt.Printf("sum() 函数中 b = %d\n",  b);

   return a + b;
}
```

## 数组
go语言的数组索引编号也是从零开始的
`var variable_name [size] variable_type`

```go
var arr = [2]int{1,2}
arr := [2]int{1,2}

arr := [...]int[1,2] // 编译器自动推断数组长度

arr := [3]int{0:1,2:5} // 可以通过索引指定元素值
```

## 指针
指针变量初始值为nil
```go
	var a = 10
	fmt.Println("address for var is ", &a)

	var aPtr *int
	aPtr = &a
	fmt.Println("prt aPtr is ", aPtr, "value is ", *aPtr)
```

指针数组
`var ptr [size]*int`

## 结构体
结构体定义
```go
type struct_variable_type struct {
    member definition
    member definition
    member definition
    ...
    member definition
}
```

结构体变量声明

`variable_name := structure_variable_type {value1, value2...valuen}`

`variable_name := structure_variable_type { key1: value1, key2: value2..., keyn: valuen}` 

结构体构造&结构体指针
```
type Books struct {
	title   string
	author  string
	subject string
	book_id int
}

/**
结构体内部方法定义
*/
func (book Books) price() float32 {
	return float32(book.book_id) / 10
}

func structTest() {
	fmt.Println(Books{"Go语言", "xuzhe", "基础教程", 123})
	fmt.Println(Books{
		title:   "Java语言",
		author:  "xuzhe",
		subject: "从入门到放弃",
		book_id: 1234,
	})
	fmt.Println(Books{title: "Python语言", author: "xuzhe"})

	var book1 Books
	book1.title = "C语言"
	book1.author = "xuzhe2"
	book1.subject = "字典"
	book1.book_id = 123
	fmt.Println(book1, "price", book1.price())

	bookPtr := &book1
	bookPtr.subject = "字典2" // 对于结构体指针，直接使用.即可访问成员
	fmt.Println(*bookPtr)
}
```

## 切片
```go
func sliceTest() {
	arr := [5]int{1,2,3,4,5}
	s1 := arr[:]  // 从数组获取切片
	fmt.Println(s1, len(s1), cap(s1), s1[:2], s1[2:], s1[2:4]) // 切片的截取
	s1 = append(s1, 6,7,8) // 切片追加
	fmt.Println(s1, len(s1), cap(s1)) // 获取切片长度、当前容量

	var s4 []int
	copy(s1, s4) // 拷贝切片，拷贝长度为s1、s4的len的最小值，这里的代码拷贝长度为0，因为s4长度为0.
	fmt.Println(s4, len(s4), cap(s4))

	s2 := make([]int, 4, 6) // 创建切片，非nil
	fmt.Println(s2, len(s2), cap(s2))
	if s2 == nil {
		fmt.Printf("切片是空的")
	}

	var s3 []int  // 未指定长度的切片是nil，但nil的切片可以append元素
	fmt.Println(s3, len(s3), cap(s3))
	if s3 == nil {
		fmt.Printf("切片是空的")
	}
}
```

## range关键字
```go
func rangeTest() {
	nums := []int{2,3,4}
	sum := 0
	for _, num := range nums {
		sum += num
	}
	fmt.Println("sum is", sum)

	for idx, num := range nums {
		if num == 3 {
			fmt.Println("index:", idx)
		}
	}

	kvs := map[string]string{"a":"apple", "b":"banana"}
	for k, v := range kvs {
		fmt.Println("k:", k, "v:", v)
	}

	for idx, ch := range "go lang" {
		fmt.Println("idx:", idx, "ch:", ch)
	}
}
```

## map
* 定义
1. 声明变量，默认值为nil
> `var map_variable map[key_type]value_type`
2. 使用make，默认非nil
> `map_variable := make(map[key_type]value_type)`

```go
func mapTest() {
	var countryMap map[string]string     // 声明
	countryMap = make(map[string]string) // 初始化，未初始化的map不能使用

	countryMap["France"] = "巴黎"
	countryMap["Italy"] = "罗马"
	countryMap["Japan"] = "东京"
	countryMap["India"] = "新德里"

	for country := range countryMap {
		fmt.Println("Capital of", country, "is", countryMap[country])
	}

	test := "American"
	capital, ok := countryMap[test]
	if ok {
		fmt.Println("Capital of", test, "is", capital)
	} else {
		fmt.Println(test, "not exist")
	}

	delete(countryMap, "France")
	fmt.Println(countryMap)
}
```

## 类型转化
```go
func typeConversion() {
	sum := 17
	count := 5
	var mean float32

	mean = float32(sum) / float32(count) // 操作符两侧必须类型一致
	fmt.Println("mean值为: ", mean)
}
```

## 接口interface
接口中定义一组方法签名，任何其他类型只要实现了这些方法就是实现了这个接口
```go
type interface_name interface {
	method_name1 [return_type]
	method_name2 [return_type]
	method....
}

type struct_name struct {
	// some definitions
}

func (struct_name_variable struct_name) method_name1() [return_type] {
}

func (struct_name_variable struct_name) method_name2() [return_type] {
}
```

样例代码
```go
type Phone interface {
	call()
}

type Nokia struct {
}

func (phone Nokia) call() {
	fmt.Println("Nokia call")
}

type IPhone struct {
}

func (phone IPhone) call() {
	fmt.Println("IPhone call")
}

func interfaceTest() {
	var phone Phone

	phone = new(Nokia)
	phone.call() //Nokia call

	nokia := phone

	phone = new(IPhone)
	phone.call() //IPhone call

	iPhone := phone

	nokia = iPhone
	nokia.call() //IPhone call
}
```

## 异常处理
go内置的error类型是个interface，定义如下：
```go
type error interface {
	Error() string
}
```

样例代码
```go
type DivideError struct {
	dividee int
	divider int
}

func (de *DivideError) Error() string {// 自己创建异常
	strFormat := `
			Cannot proceed, the divider is zero.
			dividee: %d
			divider: 0
		`

	return fmt.Sprintf(strFormat, de.dividee)
}

func Divide(dividee int, divider int) (result int, errorMsg string) {
	if divider == 0 {
		errorInfo := DivideError{
			dividee: dividee,
			divider: divider,
		}
		errorMsg = errorInfo.Error()
	} else {
		result = dividee / divider
	}

	return
}

func createError() error {// 使用errors包创建异常
	return errors.New("I create an error")
}

func errorTest() {
	if result, errorMsg := Divide(100, 10); errorMsg == "" {
		fmt.Println("divide result is", result)
	}

	if _, errorMsg := Divide(100, 0); errorMsg != "" {
		fmt.Println("error is", errorMsg)
	}

	fmt.Println("error is", createError())
}

```

## 并发
### goroutine
goroutine是轻量级线程，由Golang运行时管理，使用语法如下：

`go func_name(vars)`

在函数前添加go关键字即可使用goroutine运行该方法。同一个程序中的所有goroutine共享同一个地址空间。
```go
func saySomething(words string) {
	for i := 0; i < 5; i++ {
		time.Sleep(100 * time.Millisecond)
		fmt.Println(words)
	}
}

func goroutineTest() {
	go saySomething("world")
	saySomething("hello")
}
```

### channel
* 定义：
chan适用于传递数据的数据结构，可用于多个goroutine见传递指定类型的数据进行同步与通讯，操作符<-用于指定方向，发送或接受，未指定防线则为双向通道。
```go
ch <- v // 发送v到ch
v:= <- ch // 从ch读取数据，赋值给v
```
* 声明
`ch := make(chan type)` // 默认通道不带缓冲区，发送端会阻塞直到接收端获取了数据，才能再次发送数据
`ch := make(chan type, buffer_size)` // 带有缓冲区，缓冲区满，则发送端无法再发送数据
