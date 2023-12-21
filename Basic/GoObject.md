# GO语言的面向对象

Go语言仅支持封装，不支持继承和多态，故Go语言没有class的概念

## 对象的基础使用

类的定义：

```go
type treeNode struct {
	value int
	left, right *treeNode
}
```

对象的定义，使用：

```go
func main() {
	var root treeNode
	root.value = 3
	root.left = &treeNode{} // 返回treeNode类型的节点的地址
	root.right = &treeNode{5, nil, nil}
	// 另外，也可以使用new创建对象，注意new返回的是对应的地址
	root.right.left = new(treeNode)

	// 我们还可以进行批量创建对象(使用slice)
	nodes := []treeNode{
		{value: 3},
		{},
		{10, &root, nil},
	}
	fmt.Println(nodes)
}
```

Go语言的对象是没有构造函数的，我们一般定义一个工厂函数来构造对象

```go
func createNode(value int) *treeNode {
	return &treeNode{value, nil, nil}
}
```

使用：

```go
root.left.right = createNode(1)
```

**简单了解Go语言的存储机制：**

- C++：方法的局部变量会创建在栈上，方法退出时清空栈，其他方法无法访问栈上的元素，其他变量存储在堆上，需要进行回收
- Java：方法的全部变量都定义在堆上，由垃圾回收机制进行清除
- Go：方法的变量定义由底层决定，对于&的元素，其为了全局可以使用，会定义在堆上，不会被在之后调用的普通局部变量会定义在堆上。

结构内方法的定义（Java类中方法的定义）

```go
func (node treeNode) print() {
	fmt.Println(node.value)
}
// 调用：
root.print()
```

另外，由于Go语言只有值传递的关系，我们这种形式无法对对象进行修改，只能进行读取和其他操作，如果想要进行修改，就必须传入指针（示例如下）：

```go
func (node *treeNode) setValue(value int) {
	node.value = value
}
```

树的中序遍历

```go
func (node *treeNode) traverse() {
	if node == nil {
		return
	}
	node.left.traverse()
	node.print()
	node.right.traverse()
}
```

## 包的使用

每个目录下只能有同一个包，只有main包标记了程序入口

我们如果想要使用其他包的方法，就必须将他们的属性和方法标记为Public

GO语言中：规定首字母大写为public、小写为private





























































