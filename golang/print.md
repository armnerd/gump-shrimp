# Golang变量与字符串

> 从入门到放弃

### 如何定义变量?

```golang
// 如果没有赋值，默认为0
var a int

// 声明时赋值
var a int = 1

// 自动识别类型
var a = 1

// 自动创建变量
a := 1
```

### 如何定义常量?

```golang
const s string = "hello world"
const pi float32 = 3.1415926
```

### 如何操作字符串?

* 使用双引号""或反引号``, 单引号''只能定义单个字符如: 'a'
* 打印变量: fmt.Println(cup)

```golang
// 获取字符串长度
len("hello")

// 将字符串转换为 []rune 后调用 len 函数进行统计
length := len([]rune(str))

// 字符串查找
strings.Contains("nice", "n")

// 查找字符位置
strings.Index("nice", "i")

// 字符串替换
strings.Replace("foo", "o", "0", -1)

// 字符串截取
str := "cn中文en"
string([]rune(str)[:4])

// 字符串拆分
strings.Split("a-b-c-d-e", "-")

// 字符串拼接
strings.Join([]string{"a", "b"}, "-")

// 转大写
strings.ToUpper("test")

// 转小写
strings.ToLower("TEST")
```

------

# Golang切片与Map

> 没有万能数组, 有的是万能的interface{}

### Array

```golang
// 声明数组
var arr [5]int     // 一维
var arr2 [5][5]int // 二维

// 声明时初始化
var arr = [5]int{1, 2, 3, 4, 5}
arr := [5]int{1, 2, 3, 4, 5}
```

### Slice

```golang
var data = make([]T, length, capacity)
```

* array是固定大小的, 适用性不广泛
* T是指元素的的类型, 如int,string等
* 尽量给一个合适的capacity, 否则扩容时开销比较大

### Map

```golang
// 未初始化无法赋值
var data map[string]interface{}

// 可以赋值
var data = make(map[string]interface{})

// 直接赋值
var data = map[string]interface{}{
			"id":        1
}
```

* 万能的interface{}

##### 如何操作Slice?

```golang
// 追加元素
seq = append(seq, ele)

// 删除元素
index := 2 // 要删除的下标
seq = append(seq[:index], seq[index+1:]...)

// 切片的复制
func copy(dst, src []Type) int
// 被复制的元素个数是dst和src中短的那个, 同时注意一旦复制，对dst的任何修改都不会影响到src

// 大小和容量
len(slice)
cap(slice)
```

##### 如何操作Map?

```golang
// 增加元素
m["key"] = "value"

// 删除元素
delete(cup, "key")
```

##### 如何遍历?

```golang
// range遍历
for index := range res {
	fmt.Println(res[index])
}

// for循环
for i := 0; i<10; i++{
     fmt.Println(i)
}
```

* 复制array是重新生成变量，复制slice是指向了原来的底层array

* 元素类型一致但是容量不同的两个数组不算同一类型

* 操作slice其实是在操作底层的array

* slice在append时，当底层array容量不够的情况下，会将slice指向一个底层array容量两倍的新array变量，开销大，不如在开始时指定容量

* map通过不存在的index去取值，结果为0

* map里各index顺序并不固定

* 与slice一样，map的副本有变更，会影响原来变量里的值，因为操作的其实都是指向其他内存地址的值

------

# Golang函数与异常
> 新瓶装旧酒

### 函数参数与返回

```golang
func joke(num int, word string) (int, string) {
	return num, word
}
num, word := joke(1, "ok")
```

### 变长参数

```golang
func show(nums ...int) {
	for _, num := range nums {
      		fmt.Println(num)
	}
}

// 调用示例
show(1, 2)
show(1, 2, 3)
nums := []int{1, 2, 3, 4}
show(nums...)
```

### 匿名函数

```golang
// 定义函数于变量中
f := func(x, y int) (result int) {
	result = x + y
	return // return 是必须要有的, 如果返回类型里已经声明了变量, 这里可以不写返回值
}
res := f(1, 2)

// 直接创建匿名函数并执行
c, d := func(a, b int) (c, d int) { // 预定义的入参和出参变量名不能一样
	return a, b
}(1, 2)

// 函数当做结果返回 [闭包]
func adder() func(int) int {
	sum := 0
	return func(x int) int {
		sum += x
		return sum
	}
}

// 闭包复制的是原对象指针
handler := adder()
for i := 0; i < 10; i++ {
	fmt.Println(handler(i))
}
```

* 函数外声明的常量，在函数内可以重新声明并覆盖

* 函数返回指针，函数销毁但是指针指向的变量被保留

* 匿名函数作为变量必须在使用前声明

### 不恰当的类比

| Golang            | PHP           |
| ----------------- | ------------- |
| func init() {}    | __construct() |
| defer func() {}() | __destruct()  |

### Errors

```golang
// 引入多个包
import (
	"errors"
	"fmt"
)

// 返回错误或 nil
func tryout() error {
	return errors.New("I don't know either")
	// return nil
}

// 通常的简洁写法
if err := tryout(); err != nil {
	fmt.Println(err)
}
```

### 异常处理

> 没有try catch肿么办?

```golang
// 兜底
defer func() {
  if err := recover(); err != nil {
		fmt.Printf("recover:%v\n", err)
	}
}()

// 抛出
panic("oops")
```

* 多个 defer 倒序执行

* defer 在 panic 前执行

### 位运算

```golang
^：XOR
&^ : AND NOT
&^ : a &^ b  =  a & (a^b)
```

* 比较运算符左右，不同为左值，相同为0
* 如果右侧是0，则左侧数保持不变
* 如果右侧是1，则左侧数一定清零

------

# Golang结构与接口

### 结构

>  裤衩外穿的 class

1. 就是各种类型数据的自定义组合
2. 方法不是必须要有的而且还写在了外边

```golang
type Car struct {
	brand       string
	hoursepower int
}

func (car *Car) drive(destination string) string {
	return fmt.Sprintf("hello %s, drive me to %s", car.brand, destination)
}

func main() {
	// 创建实例
	car1 := &Car{
		brand: "BMW",
	}
	fmt.Println(car1.drive("Beijing"))
    	
	// 使用 new 实例化
	car2 := new(Car)
	car2.brand = "VW"
	fmt.Println(car2.drive("Shanghai"))
}
```

* struct 复制是新变量，互不影响

* struct 中成员是其他 struct 时，类似于 php 中的 trait

* struct中 成员类型后可配置 tag，利用 reflect 可以获取这些信息做验证

* new(T) 为一个类型为 T 的新项目分配了值为零的存储空间并返回其地址，也就是一个类型为 *T 的值。用 Go  的术语来说，就是它返回了一个指向新分配的类型为 T 的零值的指针

* make(T, args) 函数的目的与 new(T) 不同。它仅用于创建切片、map 和 chan（消息管道），并返回类型 T（ 不是*T ）的一个被初始化了的（不是零）实例

* 结构体里的方法在声明时，参数应为结构指针变量，否则在方法里操作的结构成员变量是副本

* 如果你想让一个方法可以被别的包访问的话，你需要把这个方法的第一个字母大写。这是一种约定

### 接口

> 会我的功夫你就是我门下的人了

```golang
// 定义接口
type Car interface {
	drive() string
}

// 定义结构实现方法
type Bmw struct {
	hoursepower string
}

func (b *Bmw) drive() string {
	return fmt.Sprintf("You have %s hoursepower to use, have fun~", b.hoursepower)
}

func main() {
	// 注意类型是接口, 如果具体结构没有全部实现接口里的方法会报错
	var c Car = &Bmw{
		hoursepower: "300",
	}
	fmt.Println(c.drive())
}
```

### 类型转换

> 悟空, 你戴上这个紧箍咒之后, 世上的情缘将与你无缘了

```golang
// 当一个类型的数据被 interface{} 类型的变量接收后, 原类型的方法就用不了
var mask interface{}
mask = "hello"

// 找回自我的方法
face, err := mask.(string)
if !err {
	fmt.Println(err)
} else {
	fmt.Println(face)
}
```

* 结构储存数据，接口储存行为

* 接口不继承，结构内定义接口中同名方法。声明变量为接口的类型，值却是结构的实例，然后使用那个相同的方法

* 只有一个方法的接口的命名规则，例如方法 write，接口名 writer。

* 接口的实现如果是结构体的 value，结构方法的接收器必须都是 value。如果是结构体的 pointer，结构方法的接收器是 value 或是 pointer 都可以

* 使用很多很小的接口

* 单方法接口比较强大和灵活，如 io.Writer, io.Reader, interface{}

### 新奇特

* Golang 没有三元运算和 while

* switch 默认 break，也可在 case 里中断，且有 fallthrough 可以贯穿 case

------

# Golang协程与channel

### 协程

> go go go ~

```golang
func f(msg string) {
    fmt.Println(msg)
}

func main(){
    // 定义好的函数
    go f("gogogo")
    
    // 匿名函数
    go func(msg string) {
        fmt.Println(msg)
    }("gogogo")
}
```

### Mutex

* var mutex sync.Mutex / 定义全局变量

* mutex.Lock() / 锁

* mutex.Unlock() / 释放

### WaitGroup

> 协程间不需要通信

* var wg sync.WaitGroup / 定义全局变量

* wg.Add(1)为 / wg 添加一个计数

* wg.Done() / 减去一个计数

* wg.Wait() / 等待所有的协程执行结束

### channel

> 协程间需要通信

```golang
// 创建 channel
channel := make(chan string, cacheSize)
// cacheSize 是channel 缓存的容量, 没有就是无缓存的
 
// 创建协程向 channel 里发送字符串
go func() { channel <- "hello" }()
msg := <- channel

// 关闭
close(channel)

// range 读取
for msg := range channel{
    fmt.Println(msg)
}

// for 读取
for {
    msg, ok := <- channel
    if !ok {
        break
    }  
    fmt.Println(msg)
}

// select 读取
for {
	select {
	case msg := <-channel1:
		fmt.Println(msg)
	case msg := <-channel2:
		fmt.Println(msg)
	default:
		...
	}
}
```

* 没有缓存的 channel 读写都会阻塞，有缓存的未满时还可以读写
* 向 channel 中压入变量时，压入的是副本，对变量的改变不会影响 channel 中的值
* 不要向一个已关闭的 channel 中发送消息或者重复关闭一个 channel

### 死锁

> all goroutines are asleep - deadlock!

* go 编译器认为不管是发送数据还是接收数据应该有对应的协程来处理
* 没有缓存的 channel 发送数据前需要有协程接收, 有缓存的 channel 没有关系
* 读取一个 channel, 然而 channel 已经没有协程会写入数据时就会报错
* 发送数据到 channel 后记得及时关闭 channel

------

# Golang 常用操作

### 字符串与整型

```golang
// int/int64 转字符串
string := strconv.Itoa(int)
string := strconv.FormatInt(int64,10)

// 字符串转 int/int64
int, err := strconv.Atoi(string)
int64, err := strconv.ParseInt(string, 10, 64)

// int 和 int64 互转
m := int64(n)
n := int(m)
```

### 类型转换

```golang
// string 与 []byte 互转
[]byte(str)
string(byteSlice)

// interface 断言
var raw interface{}
origin,ok := raw.(map[string]interface{})
```

### 时间

* time.Parse()的默认时区是UTC，time.Format()的时区默认是本地
* 如果时间字符串中带了时区信息才去使用 time.Parse，否则使用 time.ParseInLocation

```golang
// 当前 time
time.Now()

// 字符串解析 time
time.RFC3339 == "2006-01-02T15:04:05Z07:00"
timeTime, _ = time.Parse("2006-01-02T15:04:05-07:00", timeStr)
timeTime, _ = time.ParseInLocation("2006-01-02 15:04:05", timeStr, time.Local)

// 时区
loc, _ := time.LoadLocation("Local")
loc, _:= time.LoadLocation("Asia/Shanghai")

// time 格式化字符串
timeStr := timeTime.In(loc).Format("2006-01-02 15:04:05")

// time 转换时间戳
timestamp := timeTime.Unix()

// 时间戳转换 time
timeTime := time.Unix(timestamp, 0)

// 毫秒
秒乘以或除以1000

// n 分钟前
time.Now().Add(-time.Minute * n)

// n 小时前
h, _ := time.ParseDuration("-1h")
hn := now.Add(n * h)

// n 天前
timeTime.AddDate(0, 0, -n)
```

### Json

```golang
// encode
data := make(map[string]interface{})
msg, err := json.Marshal(data)

// decode
str := "{}"
var result map[string]interface{}
err := json.Unmarshal(str, &result)
```

### 文件操作

```golang
// 获取当前路径
dir, _ := os.Getwd()

// 读取全部
f, err := ioutil.ReadFile(path)
string(f)

// 逐行读取
fs, err := os.Open(path)
defer fs.Close()
r := bufio.NewReader(fs)
for {
    a, _, c := r.ReadLine()
    if c == io.EOF {
        break
    }
    line := string(a)
}

// 追加写
f, err = os.OpenFile(path, os.O_APPEND, 0666)
defer f.Close()
io.WriteString(f, content)

// 覆盖写
f, err := os.OpenFile(path, os.O_TRUNC|os.O_WRONLY, 0644)
io.WriteString(f, content)
```

### 定时

```go
// 延时
time.Sleep(time.Second)

// 周期
ticker := time.NewTicker(time.Second)
for t := range ticker.C {
    fmt.Println(t)
    break
}

// 超时控制
ch := make(chan int, 1)
select {
    case <-ch:
    // do something
    case <-time.After(time.Second * 5):
    // call timed out
    break
}
```
