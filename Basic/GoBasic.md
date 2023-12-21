# GO基础

Go语言是由Google于2006年开源的静态语言

1972：（C语言） --- 1983（C++）---1991（python）---1995（java、PHP、js）---2005（amd双核技术 + web端新技术飞速发展）---2006 （Go）

## Go语言的优势

1. 开发效率高 ---- 语法简单
2. 集各家语言的优势 ---- 出生时间晚，大量参考C和Python
3. 执行性能高 ---- 直接编译成二进制，部署简单（Python、Java都需要各自的环境才行）
4. 并发编程效率高 ---- goroutine
5. 编译速度快 ---- 比C++、Java的编译速度快

## Go语言的使用方向

1. Web开发（Gin、beego）
2. 容器虚拟化（docker、k8s、istio）
3. 中间件（etcd、tidb、influxdb、nsq等）
4. 区块链（以太坊、fabric）
5. 微服务（go-zero、dapr、rpcx、kratos、dubbo-go、kitex）

## 关于包（package）

当我们创建好一个Go语言文件时，会自动创建一行代码：package xxx，这是Go语言区分于动态语言的重要标志，我们在其他包中引入这个包的内容时，我们会使用包名来调用相关内容，而动态语言如python是通过文件名来调用的。

## HelloWorld

这是一个简单的HelloWorld：

```go
package main

import "fmt"

func main() {
	fmt.Println("Hello World")
}
```

注意在这个程序中：package和函数名都必须为main，这标识了这个程序的入口

我们也可以在控制台中进行简单的操作：

```shell
go build xxx.go
./xxx.exe
go run xxx.go
```

上面三条语句，分别是：编译、运行编译后的exe文件、编译并运行

**要注意的问题：不要在一个文件夹内使用两个及以上的main函数，这是不推荐的，我们尽量在多个文件夹中对多个入口函数的情况进行使用**

## 关于变量的定义方法

基础变量定义：GO语言的变量定义出来之后是有一个初值的，int为0，string为''，这一点区别于C和Java

变量的基本定义方法：

```go
func variableZeroValue() {
	var a int
	var s string
	fmt.Println(a, s)
}
```

注意这里的string是不会显示的，因为是一个空串

如果我们一定要打印一下的话，可以使用下面的方法：

```go
func variableZeroValuePrintEmpty() {
	var a int
	var s string
	// 下面这种格式会自动给字符串加一个双引号
	// 这个是%q的功效
	fmt.Printf("%d, %q\n", a, s)
}
```

赋初值的方法：

```go
// 赋初值
func variableInitialValue() {
	var a, b int = 3, 4
	var s string = "abc"
	fmt.Println(a, b, s)
}
```

更进一步的，我们还可以省略类型

在省略类型之后，我们就可以在一行赋很多不同类型的数值

```go
// 更进一步的，我们还可以省略类型
// 在省略类型之后，我们就可以在一行赋很多不同类型的数值
func variableInitialValueDifferent() {
	var a, b, c, s = 5, 6, true, "def"
	fmt.Println(a, b, c, s)
}
```

再进一步，我们可以省略var，以:=代替:来进行变量定义

```go
// 再进一步，我们可以省略var，以:=代替:来进行变量定义
func variableShorter() {
	a, b, c, s := 3, 4, true, "var"
	fmt.Println(a, b, c, s)
}
```

再另外，我们也可以在方法外直接定义变量，但这种变量不叫做全局变量，其属于包内变量（后续理解）

下面是一种集中的定义方式

```go
var (
	aa = 33
	bb = false
	cc = "kiu"
)
```

但注意，我们在函数外定义语言是不能使用  `:= `  的

## 变量的内建类型

- bool、string

- (u)int、(u)int8、(u)int16、(u)int32、(u)int64、uintptr

  GoLang中没有long、short等类型，其使用int + 数字的形式来定义长整型

  若前面带u，就代表他是一个无符号整数、uintptr代表其长度跟随操作系统变化（32位操作系统为32位、64位操作系统为64位）

- byte、rune

  byte指的是一个8子节的数据、rune则是Go语言中的char类型，但不同于普通的char、其有32位，即4字节（UTF-8汉语有3字节），这样更方便其拓展语言

- float32、float64、complex64、complex128

  float也就是浮点数，complex则指的是复数（一半作为虚部、另一半作为实部）

## 显式类型转换

Go语言没有隐式类型转换，我们在进行参数的传递或计算时，不会有隐式的类型转换，所有的类型转换都要显式的进行，否则编译就不会通过：

```go
func triangle() {
	var a, b int = 3, 4
	var c int
	c = int(math.Sqrt(float64(a * a + b * b)))
}
```

并且，Go语言中的类型转换中，数据类型不用加括号，但其要转换的内容必须加括号

## 常量、枚举的定义

使用const进行常量的定义

```go
func testConst() {
	const fileName = "abc.txt"
	// 注意，在定义数字变量的时候，它的变量类型是不一定的，有可能是float、也有可能是int、我们在使用它时它会自动进行转换
	const a, b = 3, 4

	// 变量的批量定义
	const (
		fileName2 = "def.txt"
		e, f = 8, 9
	)
}
```

在Go语言中，枚举类型也是通过const进行定义的：

```go
func enumTest() {
	const (
		sun  = 1
		moon = 2
		star = 3
	)

	// 另外，我们可以利用iota这个表达式
	// 这个表达式可以对我们的常量进行自增
	const (
		sun1 = iota
		sun2
		sun3
	)
	fmt.Println(sun1, sun2, sun3) //0 1 2

	// 我们可以利用这个表达式进行一些操作
	const (
		b = 1 << (10 * iota)
		kb
		mb
		gb
	)
	fmt.Println(b, kb, mb, gb) //1 1024 1048576 1073741824
}
```

## IF、SWITCH

下面是一个尝试读取文件的程序：

```go
func main() {
	const filename = "abc.txt"
	// 该行允许试图读取一个文件，若该文件未读取到，则返回err，file为nil
	// 若文件读取到，则err为nil，返回一个byte[] 类型的file，该数组应该使用%s进行接收
	file, err := ioutil.ReadFile(filename)
	if err != nil {
		fmt.Println(err)
	} else {
		fmt.Printf("%s\n", file)
	}
}
```

另外，if可以使用类似于for一样进行先执行，再判断

```go
func main() {
	const filename = "abc.txt"
	// 但注意，这种形式就类似于定义了一个局部变量，file、err都只能在if语句中使用
	if file, err := ioutil.ReadFile(filename); err != nil {
		fmt.Println(err)
	} else {
		fmt.Printf("%s\n", file)
	}
}
```

下面是一个Switch语句的示例，要注意的是：Switch后可以不跟任何语句，直接在CASE中进行判断：

```go
func grade(score int) string {
	switch {
	case score < 0 || score > 100:
		// panic代表，中断程序并抛出异常信息，异常信息为后面的内容
		panic(fmt.Sprintf("Wrong Score %d", score))
	case score < 60:
		return "F"
	case score < 80:
		return "C"
	case score < 90:
		return "B"
	case score <= 100:
		return "A"
	default:
		return "?"
	}
}
```

## Loop

Go语言中的循环：

同IF一样，GO语言中的循环也不需要括号，其他的基本与JAVA一致

```go
func convertToBin(n int) string {
	result := ""
	for ; n > 0; n /= 2 {
		lsb := n % 2
		// strconv.Itoa()用来实现将int转string
		result = strconv.Itoa(lsb) + result
	}
	return result
}

func main() {
	fmt.Println(convertToBin(13))
}

```

Go语言中没有while，我们直接在for后面写条件就是传统意义上的while

```go
func printFile(filename string) {
	file, err := os.Open(filename)
	if err != nil {
		panic(err)
	}
	scanner := bufio.NewScanner(file)

	for scanner.Scan() {
		fmt.Println(scanner.Text()) // 输出文件的一行
	}
}
```

如果我们for后面什么也不写，就是一个死循环，相当于while(truer)

```go
func forever () {
	for {
		fmt.Println("abc")
	}
}
```

我们GO语言中的并发编程就是基于GO语言中的死循环的，GO语言对于死循环有非常中意的思路，故其把死循环定义的十分优雅

## 函数

GO语言中的函数示例：

```go
func eval(a, b int, op string) int {
	switch op {
	case "+":
		return a + b
	case "-":
		return a - b
	case "*":
		return a * b
	case "/":
		return a / b
	default:
		panic("unsupported operation:" + op)
	}
}
```

另外，GO语言的另一个显著的特点：其函数可以定义多个返回值：

```go
// 带鱼除法
func div(a, b int) (int, int) {
	return a / b, a % b
}
```

另外，我们可以通过给返回值命名的方式来更灵活的处理函数

```go
// 带鱼除法
func div(a, b int) (q, r int) {
	q = a / b
	r = a % b
	return
}

func main() {
	q, r := div(8, 3)
	fmt.Println(q, r)
}
```

但是，如果我们只想使用其中一个参数，就需要我们使用下划线在取值时忽略掉另一个参数：

```go
// 带鱼除法
func div(a, b int) (q, r int) {
	q = a / b
	r = a % b
	return
}

func main() {
	q, _ := div(8, 3)
	fmt.Println(q)
}
```

同时，我们可以利用Go语言中提供的多返回值机制优化四则运算中错的问题：

```go
func eval(a, b int, op string) (int, error) {
	switch op {
	case "+":
		return a + b, nil
	case "-":
		return a - b, nil
	case "*":
		return a * b, nil
	case "/":
		q, _ := div(a, b)
		return q, nil
	default:
		return 0, fmt.Errorf("Unsupported Operation: %s ", op)
	}
}

func main() {
	if result, err := eval(15, 4, "C"); err != nil {
		fmt.Println("Error:", err)
	} else {
		fmt.Println(result)
	}
}
```

另外，我们也可以在函数的形参中定义函数，这也被我们叫做函数式编程，在后面详细讲

对于重载等Java中的技术，在Go语言中都不存在，其存在的有一个可变形参列表：

```go
func sumArgs(values ...int) int {
	sum := 0
	for i := range values {
		sum += values[i]
	}
	return sum
}

// main: fmt.Println(sumArgs(1, 2, 3, 4, 5, 6, 7, 8, 9))
```

## 指针

指针指的是通过操作数据的存储地址从而进行对数据操作的工具，不同于C中的指针，Go中的指针没有运算操作，这一点大大简化了学习指针的难度。

### 值传递和引用传递

在此之前，我们需要理清楚值传递和引用传递的区别：

```c++
void pass_by_val(int a) {a++;}
void pass_by_ref(int& a) {a++;}
```

在上面的C++代码片段中，没有加&的就属于值传递，其会在空间中开辟一片新的区域用来存储一个传进来的形参a的复制，这就叫做值传递，而在下面的代码中，添加了&，这就属于引用传递，其会在传入的形参原值上进行操作

### 典例：交换两个元素的内容

值传递的经典例子：

```go
func swap(a, b int) {
	a, b = b, a
}
```

这种写法不会交换任何元素，这是在元素的复制上进行操作，没有效果

下面是利用指针的写法：

```go
func swap(a, b *int) {
	*a, *b = *b, *a
}
func main() {
  	a, b := 3, 4
	swap(&a, &b)
	fmt.Println(a, b)  
}
```

但问题就是：我们这种写法对观感来讲非常差，不利于代码维护，故我们一般不使用指针，而是将我们需要的值返回出去并让其他变量接收，进而解决值传递的问题。

通常解法：

```go
func swap(a, b int) (int, int) {
	return b, a
}

func main() {
    a, b := 3, 4
	a, b = swap(a, b)
	fmt.Println(a, b)

}
```

## 数组

### 数组的定义方式

```go
func main() {
	// 定义数组的常规方式，这样定义会给他们赋初值都是0
	var arr1 [5]int
	// 以这种方式定义的数组必须赋初值
	arr2 := [3]int{1, 2, 3}
	// 这种方式可以定义数量不确定的数组，但同时，使用[...]的话就也必须赋初值
	arr3 := [...]int{5, 6, 7}
	// 多维数组的定义方式
	var grid [3][4]int
	fmt.Println(arr1, arr2, arr3)
	fmt.Println(grid)
}
```

### 数组的遍历

```go
	// 遍历的方法
	// 最基础的遍历方法
	for i := 0; i < len(arr3); i++ {
		fmt.Println(arr3[i])
	}

	// 利用range进行遍历
	for index, value := range arr3 {
		fmt.Println(index, value)
	}

	// 若我们不想使用下标或值，我们需要使用下划线_代替（因为Go语言不支持定义未使用的变量）
	for _, v := range arr3 {
		fmt.Print(v)
	}
```

### 数组是值类型

我们调用函数的时候和Java中是一样的，我们的函数在对形参进行修改的时候，不会修改到传入的参数，而是会修改其传入的形参的复制，也就是说，数组的调用是值传递（Java中会直接传入数组的地址，所以看似是引用传递，但实际上是值传递，实现了引用传递的功能）。

另外：a [10]int和[11]int是不同的类型，再进行形参传递时甚至无法通过编译

如果我们需要对数组的内容进行修改，就需要使用到指针：

```go
func printArray(arr *[3]int) {
	arr[0] = 100
	for i, v := range arr {
		fmt.Println(i, v)
	}
}

printArray(&arr3)
```

这样使用数组其实不太方便，我们在实际使用中一般使用切片来代替数组的功能

## 切片（slice）

在我们以后的开发过程中，使用切片就像我们使用Java中的数组一样，我们更多使用Slice进行实际的开发

### 切片的定义

切片的定义：

```go
	arr := [...]int{0, 1, 2, 3, 4, 5, 6, 7, 8, 9}
	s1 := arr[2:6]
	fmt.Println(s1) // [2 3 4 5]
```

切片的灵活定义：

```go
	s2 := arr[:6]
	s3 := arr[2:]
	s4 := arr[:]
	fmt.Println(s2) // [0 1 2 3 4 5]
	
	fmt.Println(s3) //[2 3 4 5 6 7 8 9]

	fmt.Println(s4)//[0 1 2 3 4 5 6 7 8 9]
```

### 切片的基本原理

切片是对数组的视图，对这个视图的修改会对原数据造成修改：

```go
func updateSlice(slice []int) {
	slice[0] = 100
}

	s5 := arr[2:6]
	fmt.Println(s5) //[2 3 4 5]

	updateSlice(s5)
	fmt.Println(s5) // [100 3 4 5]

	fmt.Println(arr) // [0 1 100 3 4 5 6 7 8 9]
```

另外的：我们甚至可以在slice上再slice，但我们最终操作的都是第一个slice基于的数组

#### Slice的扩展

我们还可以注意一个问题：Slice是可以向后自动拓展的

![image-20231207150641944](C:\Users\M_Bai\AppData\Roaming\Typora\typora-user-images\image-20231207150641944.png)

举例：

```go
	s6 := arr[3:5]
	fmt.Println(s6)
	s7 := s6[1:4]   //[3 4]
	fmt.Println(s7) //[4 5 6]
	s8 := s6[3:4]   // [6]
	fmt.Println(s8)
```

按理说s6中只有两个元素，我们不可能取出第3、4个元素，但我们却实实在在的取出来了，这是因为我们的slice会存储一个cap元素用来记录从slice的起始位置到arr的最后位置，这样我们向后延展的时候就可以取出来了，并且我们甚至可以从超出slice长度的后面开始取，但基于这种原理，我们无法向前延展。

同理，由于s[index]不属于slice切片的赋值范畴，所以我们也不能用这种方式拓展

### 切片的操作

我们可以使用append来向slice中添加元素，这个添加可以超过原数组的长度，并且如果没有超过长度时，会修改对应原数组对应索引位置的数据，如果超过了长度，我们的Slice就不再作为原数组的索引，而是会被系统自动映射一个新的更长的索引，这个索引会比Append以后的切片更长（其实是拥有一个扩容机制，这个扩容机制也是由0、1、2、4、8、16、32、64、128。。。这样进行扩容的，在append时发现cap长度不足时会触发扩容

```go
	s9 := arr[8:9]
	fmt.Println(s9) // [8]

	s10 := append(s9, 100)
	s11 := append(s10, 101)
	fmt.Println(s11) // [8 100 101]

	fmt.Println(s11[0:4])	// [8 100 101 0]
```

另外的，append添加元素后的新切片必须被接收

Slice如果以基础的方式被声明，其内容为nil，len=0、cap=0

#### 直接声明Slice

以下面这种方式可以直接声明一个切片

使用make方法可以创建一个切片，并声明它的len和cap

```go
	var s12 []int

	fmt.Println(s12) // []
	s13 := make([]int, 15, 16)
	fmt.Println(s13) // [0 0 0 0 0 0 0 0 0 0 0 0 0 0 0]

	fmt.Println(len(s13)) // 15

	fmt.Println(cap(s13))	// 16
```

#### Slice的复制与删除

Slice的Copy操作：

```go
	s14 := []int{2, 4, 8, 10}
	copy(s13, s14) // s14 复制给 s13、不会改变s13的len和cap，只从前往后改变，后面的也不便
	// s13:[0 0 0 0 0 0 0 0 0 0 0 0 0 0 0]
	// s14:[2 4 8 10]
	fmt.Println(s13) // [2 4 8 10 0 0 0 0 0 0 0 0 0 0 0]
```

另外的，我们若把长的Copy给短的，短的的长度也不会改变，只会从前往后把元素复制过来

如果我们要进行删除操作：

我们可以使用slice的切片机制以及append机制进行拼接

```go
	s15 := s14[0:2]
	s16 := s14[3:4]
	s17 := append(s15, s16...) // 这个...用来将slice中的元素全部以逗号分隔的形式取出
	fmt.Println(s17)           // 这样就删除了第3个元素
```

另外的，我们可以使用s[1:]和s[:len(s)- 1]来掐头去尾

## Map

Map，键值对

### Map的定义

Map的定义方式：有初值的定义方式

```go
	// 有初值的定义方式
	m := map[string]string{
		"name":    "ccmouse",
		"course":  "golang",
		"site":    "imooc",
		"quality": "notbad",
	}
	fmt.Println(m)
```

使用Make和var进行定义的方式：

```go
	m2 := make(map[string]int)
	fmt.Println(m2)
	var m3 map[string]int
	fmt.Println(m3)
```

要注意的是：使用make定义出来的map是一个空map、而使用var定义的map是nil

### map的操作

Map的遍历

```go
	for k, v := range m {
		fmt.Println(k, v)
	}
```

同样的：我们的k，v也可以使用下划线进行省略

这里要注意的是：map底层是一个HashMap：其内部是无序的，故我们每次遍历其顺序都不同

Map的取值：

```go
	courseName := m["name"]
	fmt.Println(courseName)
```

要注意的是：

我们就算取了一个不存在的Key值，也可以通过编译（会取到Zero Value），我们可以使用第二个返回值来判断是否取到元素

```go
	courseName, ok := m["name"]
	fmt.Println(courseName, ok)

	c, ok := m["c"]
	fmt.Println(c, ok)
```

这就自然而然的延伸出了一种防止取到空值的方法：

```go
	if courseName, ok := m["name"]; ok {
		fmt.Println(courseName)
	}
```

Map的值的删除：

```go
	delete(m, "name")
```

### 例题

一个简单的判断子串问题：

```go
func length(s string) int {
	lastOccurred := make(map[byte]int) // 每个byte为键、int为值
	start := 0
	maxLength := 0
	// 遍历字符串
	for i, ch := range []byte(s) {
		if lastI, ok := lastOccurred[ch]; ok && lastI >= start {
			start = lastI + 1
		}
		if i-start+1 > maxLength {
			maxLength = i - start + 1
		}
		lastOccurred[ch] = i
	}
	return maxLength
}

func main() {
	fmt.Println(length("abcabcdbb"))
}  
```

思路是：遍历每一个字符，对于每一个字符：若其在空map中不存在，则认为其在之前的位置都没有出现过，一定可以新加入进来，若其在map中不为空，则证明其在之前出现过，但我们仍然需要判断其最后是否在start的位置之前出现的，若是的话，也可以新加入进来，若不是的话，则证明这个字符串到头了，可以作为一个不重复子串了

## 字符和字符串的处理

一个Demo：

```go
func main() {
	str := "Luckin瑞幸咖啡"
	// 将字符串转成字节流就是这个样子（16进制数）
	// 每一个中文字符都是3个字节
	for index, value := range []byte(str) {
		fmt.Printf("(%d %X)", index, value)
	}
	fmt.Println()
	// 若转成char型呢：
	// 将每一个字符串联在一起，并且其下标是按照字符标注的，遇到中文会跳跃两个
	for index, value := range str {
		fmt.Printf("(%d %X)", index, value)
	}
	fmt.Println()

	// 同时，我们可以使用utf8的标准库进行一些操作
	count := utf8.RuneCountInString(str) // 求字符串字符数
	fmt.Printf("%d", count)
	bytes := []byte(str)
	for len(bytes) > 0 {
		ch, size := utf8.DecodeRune(bytes) // 可以将字节流转换成字符
		bytes = bytes[size:]
		fmt.Printf("%c ", ch) // 输出每一个字符
	}
	fmt.Println()
}
```

要注意的是，我们在直接使用i, v := string的时候，我们的下标是跳跃的，很容易出现乱码的情况。

我们可以使用rune进行操作。(rune会将原先的东西另开一片空间，将他们都存储在新的空间中)

```go
	// 使用rune，这样它的下标是连续的
	for index, ch := range []rune(str) {
		fmt.Printf("(%d %c)", index, ch)
	}
	fmt.Println()
```

%d   整数、%c   字符、%X   字节



























































































































































