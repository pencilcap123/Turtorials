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
   
   声明一个变量，并自动推断类型

* 指针
1. 值类型的变量直接指向存在的内存中的值
2. 引用类型变量存储的是变量的内存地址
3. 可以使用&identifier来获取指针ptr，而对指针而言是要\*ptr来获取实际指针指向的值

