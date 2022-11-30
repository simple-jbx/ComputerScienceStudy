# 基础

## 数据类型

### 基本数据类型

- 数值型
    - 整数类型（int 、int8、int16、int32、int64、uint、uint8、uint16、uint32、uint64、byte）
    - 浮点类型（float32、float64）

- 字符型（没有单独的字符型，使用byte保存单个字母字符）

- 布尔型（bool）

- 字符串（string）

### 派生/复杂数据类型

- 指针
- 数组
- 结构体
- 管道
- 函数
- 切片
- 接口
- map

## 函数

```go
func 函数名 (形参列表) (返回值类型列表) ｛
	//执行语句
	return + 返回值列表
｝
```

- 函数不支持重载

- 支持可变参数

- 形参默认值传递

- 要想在函数中修改形参的值，可以使用指针（类似引用传递）

- 函数也是一种数据类型，可以赋值给一个变量，并通过变量进行函数调用 

    - ```go
        package main
        
        import "fmt"
        
        func test(num int) {
        	fmt.Println(num)
        }
        
        func main() {
        	a := test
        	fmt.Printf("type a: %T, type test(): %T", a, test)
        	fmt.Println()
        	a(10)
        }
        ```

- 函数可以作为形参，并被调用

- 支持自定义数据类型

    - 基本语法：type 自定义数据类型名 数据类型（与c一样，相当于起了个别名）

- 支持对函数返回值命名

    - ```go
        package main
        
        import "fmt"
        
        func cal1(num1 int, num2 int) (int, int) {
        	result1 := num1 + num2
        	result2 := num1 - num2
        	return result1, result2
        }
        
        func cal2(num1 int, num2 int) (result2 int, result1 int) {
        	result1 = num1 + num2
        	result2 = num1 - num2
        	return
        }
        
        func main() {
        	fmt.Println(cal1(10, 20))
        	fmt.Println(cal2(10, 20))
        }
        ```

### 包