## Golang 学习

#### 面向对象

- Golang 也支持面向对象编程(OOP)，但是和传统的面向对象编程有区别，并不是纯粹的面向对象语言。
- Golang 没有类(class)，Go 语言的结构体(struct)和其它编程语言的类(class)有同等的地位，Golang 是基于 struct 来实现 OOP 特性的，去掉了传统 OOP 语言的继承、方法重载、构造函数和析构函数、隐藏的 this 指针等等
- Golang 仍然有面向对象编程的继承，封装和多态的特性，只是实现的方式和其它 OOP 语言不一样，比如继承 ：Golang 没有 extends 关键字，继承是通过匿名字段来实现。

- Golang 面向对象(OOP)很优雅，OOP 本身就是语言类型系统(type system)的一部分，通过接口(interface)关联，耦合性低，也非常灵活。

### 一、基础知识

#### 数据类型

![在这里插入图片描述](https://gitee.com/xu_zuyun/picgo/raw/master/img/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zNzkxMDQ1Mw==,size_16,color_FFFFFF,t_70-20240123203857482.png)

![在这里插入图片描述](https://gitee.com/xu_zuyun/picgo/raw/master/img/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zNzkxMDQ1Mw==,size_16,color_FFFFFF,t_70.png)

```markdown
## golang字符类型
字符类型的本质是一个整数，占8个字节（Go 的字符串是由字节组成的，根据utf-8编码）
字符型 存储到 计算机中，需要将字符对应的码值（整数）找出来
存储：字符—>对应码值---->二进制–>存储
读取：二进制----> 码值 ----> 字符 --> 读取
字符和码值的对应关系是通过字符编码表决定的(是规定好)
```

```markdown
## golang字符串类型
两种表现形式：
(1) 双引号, 会识别转义字符
(2) 反引号，以字符串的原生形式输出，包括换行和特殊字符，可以实现防止攻击、输出源代码等效果
```

```markdown
## golang int 类型
在Go语言中，int类型的大小取决于具体的平台，它会根据不同的系统而变化。在32位系统上，int类型通常占4个字节（32位），而在64位系统上，int类型通常占8个字节
```

溢出问题 

```
	a := int8(127)
	b := int8(1)
	fmt.Println(a + b) // 输出-128，不会报错
	
	a := uint8(255)
	b := uint8(1)
	fmt.Println(a + b) //输出0，不会报错
```

rune 类型：相当int32，由于golang中的字符串底层实现是通过byte数组的，中文字符在unicode下占2个字节，在utf-8编码下占3个字节

- byte 等同于int8，常用来处理ascii字符
- rune 等同于int32,常用来处理unicode或utf-8字符

##### 数组和切片

数组：

- 数组的地址可以通过数组名来获取 &intArr
- 数组的第一个元素的地址，就是数组的首地址
- 数组的各个元素的地址间隔是依据数组的类型决定，比如 int64 -> 8 int32->4…

切片：

slice 底层数据结构是由一个 array 指针指向底层数组，len 表示切片长度，cap 表示切片容量

当扩容时：

- 假如 slice 容量够用，则追加新元素进去，slice.len++，返回原来的 slice。
- 当原容量不够，则 slice 先扩容，扩容之后 slice 得到新的 slice，将元素追加进新的 slice，slice.len++，返回新的 slice。

```markdown
扩容规则：
	当切片比较小时（容量小于 1024），则采用较大的扩容倍速进行扩容（新的扩容会是原来的 2 倍），避免频繁扩容，从而减少内存分配的次数和数据拷贝的代价。当切片较大的时（原来的 slice 的容量大于或者等于 1024），采用较小的扩容倍速（新的扩容将扩大大于或者等于原来 1.25 倍），主要避免空间浪费
```

和切片的区别：

- 1）数组是定长，访问和复制不能超过数组定义的长度，否则就会下标越界，切片长度和容量可以自动扩容
- 2）数组是值类型，切片是引用类型，每个切片都引用了一个底层数组，切片本身不能存储任何数据，都是这底层数组存储数据，所以修改切片的时候修改的是底层数组中的数据。切片一旦扩容，指向一个新的底层数组，内存地址也就随之改变

##### Channel

go中的channel是一个队列，遵循先进先出的原则，负责协程之间的通信，channel 是 goroutine 之间数据通信桥梁，而且是线程安全的，写入，读出数据都会加锁。

三种类型：只读 channel、只写 channel（意义在于在参数传递时候指明管道可读还是可写，即使当前管道是可读写的）、可读可写 channel

```
channel 中只能存放指定的数据类型
channle 的数据放满后，就不能再放入了
在没有使用协程的情况下，如果 channel 数据取完了，再取，就会报 dead lock
goroutine 中使用 recover，解决协程中出现 panic，导致程序崩溃问题
```

```
## channel的三种状态：
未初始化的状态：只进行了声明，或者手动赋值为nil。

active：正常的channel，可读或者可写。

closed：已关闭，channel的值不是nil，关闭的状态的channel仍然可以读值（取值），但不能写值（会报panic: send on closed channel），nil状态的channel是不能close（panic: close of nil channel）的。
如果关闭后的 channel 没有数据可读取时，将得到零值，即对应类型的默认值。
```

应用场景：

- （停止）信号监听

  ```go
  package main
  
  import (
  	"fmt"
  	"sync"
  )
  // 控制顺序的打印i
  func printNumber(i int, in <-chan struct{}, out chan<- struct{}, wg *sync.WaitGroup) {
  	defer wg.Done()
  	<-in        // 等待信号
  	fmt.Println(i)
  	out <- struct{}{} // 发送信号给下一个goroutine
  }
  
  func main() {
  	const n = 10                               // 打印数字从1到n
  	var wg sync.WaitGroup                      // 用于等待所有goroutine完成
  	chans := make([]chan struct{}, n+1)        // 创建足够的通道
  
  	for i := range chans {
  		chans[i] = make(chan struct{})         // 初始化通道
  	}
  
  	wg.Add(n)
  	for i := 1; i <= n; i++ {
  		go printNumber(i, chans[i-1], chans[i], &wg)  // 启动goroutine
  	}
  
  	chans[0] <- struct{}{}  // 触发第一个goroutine开始打印
  	wg.Wait()               // 等待所有的goroutine完成
  	close(chans[n])         // 关闭最后一个通道
  }
  ```

- 定时任务

  ```go
  package main
  
  import (
  	"fmt"
  	"time"
  )
  
  func main() {
  	// 创建一个打点器，每隔1秒钟触发一次
  	ticker := time.NewTicker(1 * time.Second)
  
  	// 创建一个退出信号channel
  	done := make(chan bool)
  
  	// 启动一个goroutine，模拟定时执行的任务
  	go func() {
  		for {
  			select {
  			case <-done:
  				// 收到退出信号
  				return
  			case t := <-ticker.C:
  				// 定时任务代码
  				fmt.Println("Tick at", t)
  			}
  		}
  	}()
  
  	// 主线程等待5秒钟后，发送退出信号停止定时任务
  	time.Sleep(5 * time.Second)
  	ticker.Stop()
  	done <- true
  }
  ```

- 生产方和消费方解耦

  ```go
  package main
  
  import (
  	"fmt"
  	"sync"
  	"time"
  )
  
  // Producer 函数生成数据，并将其发送到channel
  func Producer(ch chan<- int, wg *sync.WaitGroup) {
  	defer wg.Done() // 在函数结束时通知WaitGroup该goroutine已完成
  	for i := 0; i < 5; i++ {
  		fmt.Printf("Produced: %d\n", i)
  		ch <- i // 将数据发送到channel
  		time.Sleep(time.Second) // 等待1秒模拟耗时操作
  	}
  	close(ch) // 生产完数据后关闭channel
  }
  
  // Consumer 函数从channel读取数据并消费它
  func Consumer(ch <-chan int, wg *sync.WaitGroup) {
  	defer wg.Done() // 在函数结束时通知WaitGroup该goroutine已完成
  	for {
  		i, ok := <-ch // 接收数据，如果channel关闭，则ok为false
  		if !ok {
  			fmt.Println("Channel closed!")
  			return
  		}
  		fmt.Printf("Consumed: %d\n", i)
  	}
  }
  
  func main() {
  	var wg sync.WaitGroup
  	ch := make(chan int) // 创建一个无缓冲的channel
  
  	wg.Add(1)
  	go Producer(ch, &wg) // 启动生产者goroutine
  
  	wg.Add(1)
  	go Consumer(ch, &wg) // 启动消费者goroutine
  
  	wg.Wait() // 等待所有goroutine执行完毕
  }
  ```

- 控制并发数（使用channel作为信号量）

  ```go
  package main
  
  import (
  	"fmt"
  	"sync"
  	"time"
  )
  
  func worker(id int, semaphore chan struct{}, wg *sync.WaitGroup) {
  	defer wg.Done()
  
  	// 获取信号量资源
  	semaphore <- struct{}{}
  	fmt.Printf("Worker %d started\n", id)
  	time.Sleep(2 * time.Second) // 模拟耗时操作
  	fmt.Printf("Worker %d finished\n", id)
  	// 释放信号量资源
  	<-semaphore
  }
  
  func main() {
  	workers := 10        // 工作goroutine的总数
  	concurrency := 3     // 最大并发数
  	semaphore := make(chan struct{}, concurrency)
  
  	var wg sync.WaitGroup
  	wg.Add(workers)
  
  	for i := 1; i <= workers; i++ {
  		go worker(i, semaphore, &wg)
  	}
  
  	// 等待所有的工作goroutine完成
  	wg.Wait()
  	fmt.Println("All workers completed")
  }
  ```

底层原理：

![img](https://gitee.com/xu_zuyun/picgo/raw/master/img/479081a1737c475dbc77285f30bc3828.png)

有缓冲的channel使用ring buffer（环形缓冲区）来缓存写入的数据，本质是循环数组（为啥用循环数组？普通数组容量固定、更适合指定的空间，且弹出元素时，元素需要全部前移）

```
通过var声明或者make函数创建的channel变量是一个存储在函数栈帧上的指针，占用8个字节，指向堆上的hchan结构体
buf指向一个底层的循环数组，只有设置为有缓存的channel才会有buf
sendq和recvq分别表示发送数据的被阻塞的goroutine和读取数据的goroutine，这两个都是一个双向链表结构
sendq和recvq 的结构为等待队列类型，sudog是对goroutine的一种封装 是双向链表，包含一个头结点和一个尾结点，每个节点是一个sudog结构体变量，记录哪个协程在等待，等待的是哪个channel，等待发送/接收的数据在哪里
```

流程：

```markdown
## 写数据
    读等待队列 -> buf -> 排队
如果channel的读等队列存在接受者goroutine
将数据直接发送给第一个等待的goroutine,唤醒接收的goroutine
如果channel的读等队列不存在接受者goroutine
    如果循环数组的buf未满，那么将数据发送到循环数组的队尾
    如果循环数组的buf已满，将当前的goroutine加入写等待对列，并挂起等待唤醒接收
## 读数据
	buf -> 写等待队列 -> 排队
如果channel的写等待队列存在发送者goroutine
    如果是无缓冲channel，直接从第一个发送者goroutine那里把数据拷贝给接收变量，唤醒发送的gorontine
    如果是有缓冲channel(已满),将循环数组buf的队首元素拷贝给接受变量，将第一个发送者goroutine的数据拷贝到循环数组队尾，唤醒发送端goroutine

如果channel的写等待队列不存在发送者goroutine
    如果循环数组buf非空，将循环数据buf的队首元素拷贝给接受变量
    如果循环数组buf为空，这个时候就会走阻塞接收的流程，将当前goroutine加入读等队列，并挂起等待唤醒
```

```markdown
## 相比较共享内存
    共享内存访问需要加锁，若持锁失败，要么忙等重试，要么待会儿再来。
    降低耦合：channel以消息传递通信，消息发出后就不用管了，除非它希望得到回馈，完全异步。

```

##### Struct{}

1. **无内存占用**：空结构体不包含任何字段，因此它的大小为零。这意味着创建一个`struct{}`类型的值并不会分配内存。当你在channel中传递`struct{}`时，实际上并没有内存复制的开销。
2. **信号传递**：当channel被用于信号传递而不是数据交换时，你通常不关心传递的数据，而只是想利用channel的同步特性。`struct{}`非常适合这种情况，因为它只表示一个事件的发生而没有其他语义负担。
3. **清晰的意图**：使用`struct{}`可以清楚地表明该channel仅用于同步和信号发送。任何读取channel的代码都知道不需要处理具体的数据值，而只关注信号的接收。

##### interface{}

`interface{}`是一个特殊的类型，称为"空接口"。它对实现没有任何方法的要求，因此任何类型都至少实现了空接口。这意味着`interface{}`可以存储任意类型的值。原因：

1. **通用容器**：当你需要一个函数能够接受任何类型的参数时，或者你想要一个集合能够保存不同类型的值，这时候通常会用到`interface{}`。例如，内置的`fmt.Println`函数可以接受任意类型的变量。
2. **动态类型**：在某些情况下，你可能需要编写能够处理不确定类型的代码。使用`interface{}`可以创建动态类型的变量，在运行时确定具体的底层类型。

然而使用`interface{}`也有其缺点，包括：

- **性能开销**：使用`interface{}`可能涉及到额外的内存分配和接口转换，这可能会影响性能。

#### select {}

保持程序运行，直到程序终止

##### Map

原理：底层使用 hash  table，每个 map 的底层结构是 hmap，是有若干个结构为 bmap（链表） 的 bucket 组成的数组。用链表来解决冲突 ，出现冲突时，不是每一个 key 都申请一个结构通过链表串起来，而是以 bmap 为最小粒度挂载，一个 bmap  可以放 8 个 kv。在哈希函数的选择上，会在程序启动时，检测 cpu 是否支持 aes，如果支持，则使用 aes hash，否则使用  memhash。

 key 可以是很多种类型，比如 bool, 数字，string, 指针, channel ,，接口, 结构体, 数组，**slice， map 还有 function 不可以，因为这几个没法用 == 来判断** 

声明是不会分配内存的，初始化需要 make ，分配内存后才能赋值和使用

map对象不是线程安全的，并发读写的时候运行时会有检查，遇到并发问题就会导致panic

```markdown
## 内存回收
删除键值对：使用delete函数从map中删除键值对只是移除了键和值的关联，并不直接释放内存。但是，在没有其他引用指向已删除键或值的情况下，这些对象就会在未来某个不确定时间点由GC回收。

回收整个map：如果要回收整个map占用的内存，可以将map的引用设置为nil或让引用超出作用域，这会使得map及其所有键值对不可达，因此都可以被GC回收。

ps：
1. go 底层map 是由若干个bmap（桶）构成的，桶只会扩容，不会缩容 ，所以 map中占用的内存不会被释放，
具体来说，虽然 delete 可以从 map 中删除键，但它不会缩小或重新分配 map 的底层存储。也就是说，即使你删除了许多键，map 本身占用的内存也不会立即减小。这是因为 map 的设计目的是为了优化访问速度，而不是空间效率。
以上只针对值类型的数据结构 例如：基本类型 int string slice struct 等
2. 如果key为 指针变量 删除后这个指针变量内存不会释放，但是这个指针指向的对象，引用计数会 -1 如果引用计数为0 在gc的时候就会被释放！

## 元素有序性
map 因扩张⽽重新哈希时，各键值项存储位置都可能会发生改变，顺序自然也没法保证了，所以官方避免大家依赖顺序，直接打乱处理，每次遍历，得到的输出 可能不一样。
for range map 在开始处理循环逻辑的时候，就做了随机播种（要想有序遍历，可以先将 key 进行排序，然后根据 key 值遍历）

## 线程安全
map对象不是线程安全的，并发读写的时候运行时会有检查，遇到并发问题就会导致panic
解决方法：使用sync.Map、使用读写锁
```

##### 结构体

```go
type Person struct {
	Name string `json:name-field`
	Age int
}
```

- 结构体指针访问字段的标准方式应该是：(*结构体指针).字段名 ，但 go 做了一个简化，也支持 结构体指针.字段名, 更加符合程序员使用的习惯，go 编译器底层 对 person.Name 做了转化 (*person).Name。
- 结构体的所有字段在内存中是连续的
- 结构体进行 type 重新定义(相当于取别名)，Golang 认为是新的数据类型，但是相互间可以强转（和其它类型进行转换时需要有完全相同的字段(名字、个数和类型）
- struct 的每个字段上，可以写上一个 tag, 该 tag 可以通过反射机制获取，常见的使用场景就是序
   列化和反序列化。

##### 函数与方法

```go
//函数
func getArea(R int) float64 {
	return math.Pi * math.Pow(R, 2)
}
//方法
func (c Circle)getArea() float64 {
	return math.Pi * math.Pow(c.R, 2)
}
```

方法的调用和传参机制和函数基本一样，不一样的地方是方法调用时，会将调用方法的变量，当做实参也传递给方法，体现了封装性。函数则是无状态的代码块。

Go 的函数参数传递都是值传递：调用函数时将实际参数复制一份传递到函数中，这样在函数中如果对参数进行修改，将不会影响到实际参数。

#### 对象

##### make和new

- 1）作用变量类型不同，new给string,int和数组分配内存，make给切片，map，channel分配内存；
- 2）返回类型不一样，new返回指向变量的指针，make返回变量本身；
- 3）new 分配的空间被清零。make 分配空间后，会进行初始化；
- 4）make一般是这样的形式make(type, cap)，后者表示预分配的容量（切片、map可扩容，channel则是固定大小）

```go
还可以通过字面量初始化：
字面量语法提供了一种快捷方便的方式来创建和初始化数据结构，它能够让代码可读性更高，既清晰又简洁。

a := [3]int{1, 2, 3}
s := []int{1, 2, 3}
m := map[string]int{
    "alice": 23,
    "bob":   34,
}
```

![在这里插入图片描述](https://gitee.com/xu_zuyun/picgo/raw/master/img/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zNzkxMDQ1Mw==,size_16,color_FFFFFF,t_70-20240123212053047.png)

##### 继承

```go
type Person struct {
	id int
	name string
	age  int
}

type Student struct {
	Person
	id int
	score     int
	className string
}
```

1. 使用匿名属性，来实现继承：即将父类作为子类的匿名属性
2. 如果父类和子类中有重复字段，则优先使用子类自身的属性
3. 方法的重写（方法名，参数，返回值类型都必须一样）此时调用方法绑定的对象不在时父类而是子类本身

##### 接口

1. 空接口

```go
// fmt包中的方法 Println底层
func Println(a ...interface{}) (n int, err error) {
	return Fprintln(os.Stdout, a...)
}
// 接纳任意对象
var i interface{} = 45
i=[...]int{1,2,3}
```

可以接纳任意对象，类似java中的Object

2. 接口

可以定义一些通用的方法，将被继承和实现的接口以匿名属性传入即可，但不必将所有的方法都实现

```go
type annimal interface {
	eat()
	sleep()
	run()
}
type cat interface {
	annimal
	Climb()
}
```

3. 多态

可以在调用方法时会因传入对象的不同而得到不同的效果

```go
// 使用 对象.(指定的类型) 判断改对象是否时指定的类型
if data,ok :=v.(cat);ok{
			data.eat()
			fmt.Println("this is HelloKitty : ")
		}
```

实现接口中的方法可以通过指针和结构体绑定

```go
type animal interface {
	eat()
}
type Dog struct {
	Name string
	Age  int
}
//func (d Dog) eat() { 结构体绑定
//}
func (d *Dog) eat() { 指针绑定
}

func main() {
	var a animal
	dPoint := &Dog{
		Name: "susan",
		Age:  12,
	}
	dStruct := Dog{
		Name: "susan",
		Age:  12,
	}
	a = dPoint
	// 使用指针接收者实现接口不能存结构体类型变量
	// a = dStruct
}
```

区别：使用值接受者实现接口，结构体类型和结构体指针类型的变量都能存，指针接收者实现接口只能存指针类型的变量

#### 字符串

go中的string类型采用UTF-8编码，这是一种可变长的 Unicode 字符编码方案，几乎可以表示世界任何字符。

##### string、rune、byte

- rune对应于int32，也就是这个unicode码点，几乎在所有方面等同于int32
- byte对应于原始字节（8位所以0~255），也就是原始字节

![image-20240428131739289](https://gitee.com/xu_zuyun/picgo/raw/master/img/image-20240428131739289.png)

```go
a := "Go语言"

fmt.Println("字节数：", len(a))
//utf8计算unicode字符数
fmt.Println("Unicode字符数：", utf8.RuneCountInString(a))
fmt.Println("[]Rune：", len([]rune(a)))

// 遍历字符串中的字符 char为rune类型
fmt.Println("遍历字符串中的字符：")
for _, char := range a {
  //测试当前rune类型
  fmt.Printf("%T %c\n", char, char)
}
fmt.Println()

输出结果：
字节数： 8
Unicode字符数： 4
[]Rune： 4
遍历字符串中的字符：
int32 G
int32 o
int32 语
int32 言
```

##### for range 字符串

- 从string中得到所有的rune字符，它能够自动根据 UTF-8 规则识别 Unicode 编码的字符。
- 每个 rune 字符和索引在 for range 循环中是一一对应的。

```go
package main
import (
	"fmt"
)
func main() {
	var str = "hello 你好"
	for key, value := range str {
		fmt.Printf("key:%d value:0x%x\n", key, value)
	}
}
```

![1.png](https://img.php.cn/upload/image/590/535/397/1673596126274551.png)

##### strings.Map()

对一个 字符串 中的每一个 字符 都做相对应的处理

```go
package main
import (
	"fmt"
	"strings"
)
func strEncry(r rune)rune{
	return r+1
}
func main() {
	//使用 strings.Map() 函数，实现将一个字符串中的每一个字符都后移一位
	strHaiCoder := "HaiCoder"
	mapStr := strings.Map(strEncry, strHaiCoder)
	fmt.Println("mapStr =", mapStr)
}
```

#### 异常

1. 编译时异常：在编译时抛出的异常，编译不通过，语法使用错误，符号填写错误等等。。。
2. 运行时异常：在程序运行时抛出的异常，这个才是我们将要说的，程序运行时，有很多状况发生，例如：让用户输入一个数字，可用户偏偏输入一个字符串，导致的异常，数组的下标越界，空指针等等。。。。

编译时异常很容易找到，而运行时异常不容易提前发现，通过`if err != nil`判断，但是依然会漏掉很多异常，因此我们需要在运行过程中动态的捕获异常

##### defer和recover

*defer*：延时执行，即在方法执行结束（出现异常而结束或正常结束）时执行
 *recover*：恢复的意思，如果是异常结束程序不会中断，返回异常信息，可以根据异常来做出相应的处理
 recover必须放在defer的函数中才能生效

```go
func test(a int, b int) int {
	defer func() {
		err := recover()
		fmt.Println("err:",err)
	}()
	a = b / a
	return a
}
func main() {
	i := test(0, 1)
	fmt.Println("====main方法正常结束！！====",i)
}

//结果：
err: runtime error: integer divide by zero
====main方法正常结束！！==== 0
```

##### 手动抛出异常——panic

有些异常是不应该恢复的，应该抛出异常，可以让这个异常一层层的返回给调用方的程序，使其不能继续执行，从而起到保护后面业务的目的

```go
func test(a int) int {
	i:=100 - a
	if i<0{
		panic(errors.New("账户金额不足！！！！"))
	}
	fmt.Println("=======账户扣款=====")
	return i
}
```

### 二、进阶使用

#### 性能提升——协程

##### GoRoutine

```go
go f();
```

- 一个 Go 线程上，可以起多个协程（有独立的栈空间、共享程序堆空间、调度由用户控制）
- 主线程是一个物理线程，直接作用在 cpu 上的。是重量级的，非常耗费 cpu 资源。
- 协程从主线程开启的，是轻量级的线程，是逻辑态。对资源消耗相对小。

![在这里插入图片描述](https://gitee.com/xu_zuyun/picgo/raw/master/img/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zNzkxMDQ1Mw==,size_16,color_FFFFFF,t_70-20240123220705956.png)

```markdown
## 优点：
空间大小：一般情况下一个thread的创建大小默认占用栈空间1M，而goroutine默认则是2kb
切换开销：thread的切换是需要通过用户态达到内核态，会设计到上下文的切换开销，对应资源的耗费会很大
线程通信：常见的线程通信有三种方式(C++)，使用起来相对比较复杂，最棘手的问题就是共享内存，一般会使用锁，使用到锁，就会引申各种锁的问题，比如死锁。
资源回收：创建线程相对简单，回收线程资源会麻烦很多
```

##### 底层原理

每个Goroutine都包含一个g结构体，其中包含了该Goroutine的唯一ID、调度上下文

- **G**，协程。通常在代码里用  `go`  关键字执行一个方法，那么就等于起了一个`G`。
- **M**，**内核**线程，操作系统内核其实看不见`G`和`P`，只知道自己在执行一个线程。`G`和`P`都是在**用户层**上的实现。

##### CSP并发模型

Java、C++、或者Python，他们线程间通信都是通过共享内存的方式来进行的。非常典型的方式就是，在访问共享数据（例如数组、Map、或者某个结构体或对象）的时候，通过锁来访问

Go：*不要以共享内存的方式来通信，相反，要通过通信来共享内存*

- **goroutine** 是Go语言中并发的执行单位
- **channel**是Go语言中各个并发结构体(goroutine)之前的通信机制

底层原理——GMP模型（Go 实现的一个用户态的调度器）：

- M指的是Machine，代表OS线程。
- P代表着处理器(processor)，它的主要用途就是用来执行goroutine的，一个P代表执行一个go代码片段的基础(上下文环境)，所以它也维护了一个可运行的goroutine队列，和自由的goroutine队列，**只有将 P 和 M 绑定，才能让 P 的 runq 中的 G 真正运行起来**
- G指的是Goroutine，代表一个goroutine。它包括堆栈，指令指针和其他对调度goroutine很重要的信息。
- Sched代表着一个调度器，它维护有存储空闲的M队列和空闲的P队列，可运行的G队列，自由的G队列（全局runqueue）以及调度器的一些状态信息等。

```
操作系统会在物理处理器上调度线程（M）来运行，而 Go 语言的运行时会在逻辑处理器（P）上调度goroutine（G）来运行。
p默认cpu内核数，M与P的数量没有绝对关系，一个M阻塞，P就会去创建或者切换另一个M，
```

![img](https://gitee.com/xu_zuyun/picgo/raw/master/img/webp)

流程：

**步骤 1：**创建 G，关键字 go func() 创建 G
**步骤 2：**保存 G，创建的 G 优先保存到本地队列 P，如果 P 满了，则则将本地队列中的 G 拿出一半，放到全局队列中。
**步骤 3：**唤醒或者新建M执行任务，进入调度循环（步骤4,5,6)
**步骤 4：**M 获取 G，M首先从P的本地队列获取 G，如果 P为空，则从全局队列获取 G，如果全局队列也为空，则从另一个本地队列偷取一半数量的 G（负载均衡）。
**步骤 5：**M 调度和执行 G，M调用 G.func() 函数执行 G，分下面两种情况：

- 如果 M在执行 G 的过程发生系统调用阻塞（同步），会阻塞G和M（操作系统限制），此时`P会和当前M解绑`，并寻找新的M，如果`没有空闲的M就会新建一个M` ，接管正在阻塞G所属的P，接着继续执行 P中其余的G；当系统调用结束后，这个G会尝试获取一个空闲的P执行，优先获取之前绑定的P，并放入到这个P的本地队列，如果获取不到P，那么这个线程M变成休眠状态，加入到空闲线程中，然后这个G会被放入到全局队列中。
- 如果M在执行G的过程发生网络IO等操作阻塞时（异步），阻塞G，`G主动让出M`，不会阻塞M。M会寻找P中其它可执行的G继续执行，G会被`网络轮询器network poller `接手，当阻塞的G恢复后，G1从network poller 被`移回到P的 LRQ `中，重新进入可执行状态。
- 理论上，在正常的运行过程中，每个 P 都会关联一个 M 来执行 G。

```markdown
## 核心思想：
1、work stealing机制
当本线程无可运行的G时，尝试从其他线程绑定的P偷取G，而不是销毁线程。
2、hand off 机制
当本线程因为G进行系统调用阻塞时，线程释放绑定的P，把P转移给其他空闲的线程执行。
```

```markdown
⚠️注意：
go的协程是`非抢占式的`，由协程主动交出控制权，也就是说，上面在发生IO操作时，并不是调度器强制切换执行其他的协程，- 而是当前协程交出了控制权，调度器才去执行其他协程。
```

```markdown
## 问题：
1. 为什么要用P的本地队列？
没有的话就会大家都要访问全局队列，有并发问题，需要加锁，影响性能，有了本地队列后，优先从本地队列拿，本地没有再从全局队列拿
2. 为什么P的逻辑不直接加在M上
主要还是因为M其实是内核线程，内核只知道自己在跑线程，而golang的运行时（包括调度，垃圾回收等）其实都是用户空间里的逻辑。

3. gmp当一个g堵塞时，g、m、p会发生什么?
M 会阻塞，如果当前有 G 在执行，调度器会将 MP 进行分离，如果有空闲的M就用或者是从线程池中取，如果都没有就创建一个新的M 来服务于这个 P，当M进行系统调用结束的时候，这个G会尝试获取一个空闲的 P 执行，并放入到这个 P 的本地队列。如果获取不到 P，那么这个`线程 M 变成休眠状态`， 加入到空闲线程中，然后这个 `G 会被放入全局队列`中。
```

##### 协程之间的通信方式

- 1、通道（Channels）

  ```go
  ch := make(chan int) // 创建一个无缓冲的通道
  
  // 发送者 goroutine
  go func() {
      ch <- 42 // 发送值到通道
  }()
  
  // 接收者 goroutine
  go func() {
      value := <-ch // 从通道接收值
      fmt.Println(value)
  }()
  ```

- 2、同步原语（Mutex、WaitGroup）

  ```go
  var mutex sync.Mutex
  var sharedResource map[string]int
  
  // 在访问共享资源前加锁
  mutex.Lock()
  sharedResource["key"] = 42
  mutex.Unlock()
  
  // 其他 goroutines 将等待锁释放才能访问 sharedResource
  ```

- 3、原子操作

  ```go
  var count int32
  
  // 原子地增加 count 的值
  atomic.AddInt32(&count, 1)
  
  // 原子地读取 count 的值
  value := atomic.LoadInt32(&count)
  ```

- 4、context语句

  ```go
  ctx, cancel := context.WithCancel(context.Background())
  
  // 工作 goroutine
  go func() {
      for {
          select {
          case <-ctx.Done():
              return
          default:
              // 执行工作...
          }
      }
  }()
  
  // 当需要停止 goroutine 时
  cancel()
  ```

##### WaitGroup

底层原理：

- **计数器（counter）**：`sync.WaitGroup` 内部维护了一个计数器，用于跟踪要等待的 Goroutine 的数量。当这个计数器的值为 0 时，`Wait()` 方法将不再阻塞。
- **互斥锁（mutex）**：用于保护计数器的读写操作。在调用 `Add()`、`Done()` 或 `Wait()` 方法时，会涉及对计数器的修改，互斥锁确保了这些操作的原子性，防止多个 Goroutine 并发修改计数器时出现竞态条件。
- **条件变量（condition variable）**：用于在计数器不为 0 时阻塞 Goroutine，并在计数器为 0 时唤醒它们。条件变量允许 Goroutine 在等待某个条件为真时进入休眠状态，并在条件变为真时唤醒它们。

`sync.WaitGroup` 的 `Add()` 方法用于增加计数器的值，`Done()` 方法用于减少计数器的值，而 `Wait()` 方法会阻塞当前 Goroutine，直到计数器的值变为 0。

```go
type WaitGroup struct {
    counter int32
    mutex   sync.Mutex
    cond    sync.Cond
}

func (wg *WaitGroup) Add(delta int) {
    wg.mutex.Lock()
    defer wg.mutex.Unlock()
    wg.counter += delta
    if wg.counter < 0 {
        panic("sync: negative WaitGroup counter")
    }
    if wg.counter == 0 {
        wg.cond.Broadcast()
    }
}

func (wg *WaitGroup) Done() {
    wg.Add(-1)
}

func (wg *WaitGroup) Wait() {
    wg.mutex.Lock()
    defer wg.mutex.Unlock()
    for wg.counter > 0 {
        wg.cond.Wait()
    }
}
```

#### 动态获取信息——反射

- 反射可以在运行时动态获取变量的各种信息, 比如变量的类型(type)，类别(kind)
- 如果是结构体变量，还可以获取到结构体本身的信息(包括结构体的字段、方法)
- 通过反射，可以修改变量的值，可以调用关联的方法。

Type和Value：Kind是一个大的分类，比如定义了一个Person类，它的Kind就是struct 而Type的名称是Person，其中**Value：** 为go值提供了反射接口。

```go
package main

import (
	"fmt"
	"reflect"
)

type Student struct {
	Name string
	Age int
}

func test(i interface{}){
	//获取指针指向的真正的数值Value
	valueOfI := reflect.ValueOf(i).Elem()
	//获取对应的Type这个是用来获取属性方法的
	typeOfI := valueOfI.Type()
	//判断是否是struct
	if typeOfI.Kind()!=reflect.Struct{
		fmt.Println("except struct")
		return
	}
	//获取属性的数量
	numField := typeOfI.NumField()
	//遍历属性，找到特定的属性进行操作
	for i:=0;i< numField;i++{
		//获得属性的StructField，次方法不同于Value中的Filed（这个返回的是Field）
		field := typeOfI.Field(i)
		//获取属性名称
		fieldName := field.Name
		fmt.Println(fieldName)
		//找到名为Name的属性进行修改值
		if fieldName=="Name"{
			//改变他的值为jack
			valueOfI.Field(i).SetString("jack")
		}
	}
}

func main() {
	stu:=Student{Name:"susan",Age:58}
	test(&stu)
	fmt.Println(stu.Name)
}
```

#### IO多路复用——select机制

```go
select {
    case <-chan1:
        fmt.Println("chan1 ready.")
    case <-chan2:
        fmt.Println("chan2 ready.")
    default:
        fmt.Println("default")
    }
```

每个线程或者进程都先到注册到相应的可接受 channel，然后阻塞，当注册的线程和进程准备好数据后，channel会得到相应的数据。

- 2）如果某个case中的channel已经ready，则执行相应的语句并退出select流程，否则：有default会走default然后退出select，没有default，select将阻塞直至channel ready；
- 3）每个 case 语句仅能处理一个管道，要么读要么写。
- 4）多个 case 语句的执行顺序是随机的。
- 5）存在 default 语句，select 将不会阻塞，但是存在 default 会影响性能。
- case后面不一定是读channel，也可以写channel，只要是对channel的操作就可以；空的select语句将被阻塞，直至panic；

使用场景：

```go
2.1 超时控制
func (n *node) waitForConnectPkt() {
	select {
	case <-n.connected:
		log.Println("接收到连接包")
	case <-time.After(time.Second * 5):
		log.Println("接收连接包超时")
		n.conn.Close()
	}
}
2.2 无阻塞获取值
func (w *wantConn) waiting() bool {
	select {
	case <-w.ready:
		return false
	default:
		return true
	}
}
2.3 类事件驱动循环
func (n *node) heartbeatDetect() {
	for {
		select {
		case <-n.heartbeat:
			// 收到心跳信号则退出select等待下一次心跳
			break
		case <-time.After(time.Second*3):
			// 心跳超时，关闭连接
			n.conn.Close()
			return
		}
	}
}
```

#### 延迟函数——defer

每个 defer 语句都对应一个_defer 实例，多个实例使用指针连接起来形成一个单连表，保存在 gotoutine 数据结构中，每次插入_defer 实例，均插入到链表的头部，函数结束再一次从头部取出，从而形成后进先出的效果。

- 延迟函数执行按照后进先出的顺序执行，即先出现的 defer 最后执行。
- 延迟函数可能操作主函数的返回值。
- 申请资源后立即使用 defer 关闭资源是个好习惯。

#### 上下文控制——Context

Go 的 Context 的数据结构包含 Deadline，Done，Err，Value，Deadline

- Deadline 方法返回一个 time.Time，表示当前 Context 应该结束的时间
- Done 方法当 Context 被取消或者超时时候返回的一个 close 的 channel，告诉给 context 相关的函数要停止当前工作然后返回了
- Err 表示 context 被取消的原因
- Value 方法表示 context 实现共享数据存储的地方，是协程安全的

应用：

- 在网络服务中管理请求生命周期：确保取消操作能够及时传播，防止资源泄露。
- 传递请求特定的元数据：比如追踪 ID 或认证令牌。
- 控制并发任务的截止日期或执行时间。

```go
func operation(ctx context.Context) {
	select {
	case <-time.After(500 * time.Millisecond):
		// 假设这里是该操作实际所需的时间
		fmt.Println("operation done")
	case <-ctx.Done():
		// 如果 context 被取消，我们就停止操作
		fmt.Println("operation canceled", ctx.Err())
	}
}

func main() {
	ctx, cancel := context.WithTimeout(context.Background(), 2000*time.Millisecond) // 100时输出 main: time up
	defer cancel()                                                                  // 不要忘记取消 context 以避免资源泄露

	go operation(ctx)
	//cancel() //直接输出 operation canceled

	// 做其他工作...

	select {
	case <-ctx.Done(): //超时或者手动调用cancel()
		switch ctx.Err() {
		case context.DeadlineExceeded:
			fmt.Println("main: times up")
		case context.Canceled:
			fmt.Println("main: context was canceled")
		}
	}
}
```

#### 互斥锁——Mutex

##### 两种模式

```
根据不同的情况自动切换到不同的模式以保持性能和公平性之间的平衡
```

**1）正常模式**

当 `Mutex` 被解锁时:

- 当 `Mutex` 被解锁时，它将首先检查是否有等待的 goroutines 要获取锁。如果没有，它就直接变为可用状态，任何后来尝试加锁的 goroutine 都能够立即获得锁。如果有一个或多个等待的 goroutines，则按照先到先得的原则唤醒一个 goroutine 来获取锁。
- 问题是：mutex解锁瞬间，新的goruntine进来，将参与竞争。新来的 goroutine 有先天的优势，它们正在 CPU 中运行，可能它们的数量还不少，所以，在高并发情况下，被唤醒的 waiter  可能比较悲剧地获取不到锁，这时，它会被插入到队列的前面。**如果 waiter 获取不到锁的时间超过阈值 1 毫秒，那么，这个 Mutex  就进入到了饥饿模式。**

**2）饥饿模式**

在饥饿模式下，Mutex 的拥有者将直接把锁交给队列最前面的 waiter。新来的 goroutine 不会尝试获取锁，而是加入到等待队列的尾部。 如果拥有 Mutex 的 waiter 发现下面两种情况的其中之一，它就会把这个 Mutex 转换成正常模式:

- 此 waiter 已经是队列中的最后一个 waiter 了，没有其它的等待锁的 goroutine 了；
- 此 waiter 的等待时间小于 1 毫秒。

```markdown
⚠️：
- 始终配对 Lock 和 Unlock：确保每次调用 Lock() 后都调用 Unlock()，以防止死锁。
- 释放锁的顺序：如果您需要按顺序获取多个锁，请始终按同一顺序释放它们，以避免死锁。
- 限制锁的范围：尽可能减小锁的范围，只保护必要的代码区域。
- 使用 defer：在函数开始处使用 defer mu.Unlock() 可以确保即使存在 panic 或早期返回，互斥锁也会被释放。
```

##### 两种类型

互斥锁：任何时候只能有一个goroutine持有这种锁

```go
var mu sync.Mutex
mu.Lock()   // 获取锁
// 临界区代码：只有一个goroutine可以执行此处代码
mu.Unlock() // 释放锁
```

读写锁：

- **读锁（RLock）：** 当一个goroutine获得读锁后，其他goroutine仍然可以获取读锁，但不能获取写锁。读锁之间是非排他的。
- **写锁（Lock）：** 获取写锁时，其他goroutine不得获取读锁或写锁。写锁是完全排他的。

```go
var rwMu sync.RWMutex
rwMu.RLock()   // 获取读锁
// 可以同时有多个goroutine执行读取操作
rwMu.RUnlock() // 释放读锁

rwMu.Lock()    // 获取写锁
// 只有一个goroutine可以执行写入操作
rwMu.Unlock()  // 释放写锁
```

#### 垃圾回收

1. **并发标记清除算法**：Go 使用了并发的标记清除（Concurrent Mark-Sweep）算法来实现垃圾回收。这意味着在应用程序执行的同时，垃圾回收进程也能够并行地运行。
2. **三色标记法**：在并发标记阶段，它采用了所谓的“三色”标记算法，通过对对象进行灰色、黑色和白色标记来表示对象是否在使用中、已经扫描过或未被访问。
   - **白色**: 初始状态，表示对象尚未被访问。
   - **灰色**: 表示对象已被标记为在使用中，但其引用的对象尚未全部检查。
   - **黑色**: 表示对象及其所有引用的对象都已被检查并标记。
3. **写屏障（Write Barrier）**：由于垃圾回收是并发进行的，因此需要确保在标记阶段期间没有错过任何对象。写屏障是一种同步机制，用于记录在垃圾回收过程中应用程序对内存的修改。当应用程序在并发标记阶段写入一个指针时，写屏障会将相关对象从白色变为灰色，以确保它在接下来的清除阶段不会被错误地清除掉。
4. **STW（Stop-The-World）**：尽管大部分垃圾回收过程是并发进行的，但仍有某些阶段需要暂停应用程序的执行。比如在垃圾回收开始前的初始化阶段和最终的清扫阶段可能会出现短暂的 STW 暂停。
5. **垃圾回收调优**：Go 提供了一些环境变量来控制垃圾回收的行为。例如，`GOGC` 环境变量可以用来设置触发垃圾回收的堆内存增长百分比。默认值是 100，意味着每当堆大小增长一倍时，垃圾回收就会被触发。
6. **逃逸分析**：Go 编译器会进行逃逸分析，来决定变量应该分配在栈上还是堆上。栈上的内存分配和回收通常更高效，因为当函数返回时，整个栈帧都会被移除。而堆上的内存分配则需要依赖垃圾回收器来释放。

#### 问题

1. 是否可以对Golang中的map元素取地址？

   不可以，因为`map`的元素可能会因为新元素的添加或者`map`的扩容而被移动，所以直接获取`map`元素的地址可能会引用到错误的元素。

2. Golang 调用函数传入结构体时，应该传值还是指针？

- 结构体的大小：如果结构体非常大，使用指针传递会更有效率，因为这样只会复制指针值（一般是8字节），而不是复制整个结构体。如果结构体小，值传递和指针传递的性能差异可能可以忽略不计。
- **是否需要修改原始结构体**：如果你需要在函数中修改原始结构体，你应该使用指针传递。如果你使用值传递，函数会接收结构体的一个副本，你在函数中对结构体的修改不会影响到原始的结构体。

3. 单引号，双引号，反引号的区别？

   单引号，表示byte类型或rune类型，对应 uint8和int32类型，默认是 rune 类型。byte用来强调数据是raw data，而不是数字；而rune用来表示Unicode的code point。双引号，才是字符串，实际上是字符数组。可以用索引号访问某字节，也可以用len()函数来获取字符串所占的字节长度。反引号，表示字符串字面量，但不支持任何转义序列。字面量 raw literal string 的意思是，你定义时写的啥样，它就啥样，你有换行，它就换行。你写转义字符，它也就展示转义字符。

4. 怎么控制并发数量？

   有缓冲通道

   ```go
   func main() {
   	count := 10 // 最大支持并发
   	sum := 100 // 任务总数
   	wg := sync.WaitGroup{} //控制主协程等待所有子协程执行完之后再退出。
   
   	c := make(chan struct{}, count) // 控制任务并发的chan
   	defer close(c)
   
   	for i:=0; i<sum;i++{
   		wg.Add(1)
   		c <- struct{}{} // 作用类似于waitgroup.Add(1)
   		go func(j int) {
   			defer wg.Done()
   			fmt.Println(j)
   			<- c // 执行完毕，释放资源
   		}(i)
   	}
   	wg.Wait()
   }
   ```

   第三方协程池

   ```go
   import (
   	"log"
   	"time"
   
   	"github.com/Jeffail/tunny"
   )
   func main() {
   	pool := tunny.NewFunc(10, func(i interface{}) interface{} {
   		log.Println(i)
   		time.Sleep(time.Second)
   		return nil
   	})
   	defer pool.Close()
   
   	for i := 0; i < 500; i++ {
   		go pool.Process(i)
   	}
   	time.Sleep(time.Second * 4)
   }
   ```


5. 结构体可以比较吗？

   结构体（struct）可以进行比较，但有一定的限制。只有当结构体中的所有字段都是可比较的类型时，结构体才是可比较的。可比较的类型包括布尔型、数值型、字符串、指针、通道以及任何包含这些类型的数组。如果一个结构体包含不可比较的类型，如切片、映射（map）、函数等，那么这个结构体就不能直接使用`==`和`!=`运算符进行比较。

6. golang 通过new创建的channel、map可以读吗？

   使用`new`操作符创建的通道（channel）是指向通道类型零值的指针。通道类型的零值是`nil`，试图从一个`nil`通道进行读取或者向其写入将会永久阻塞，因为对`nil`通道的操作永远不会成功。

7. go是值传递还是引用传递？

   Go语言中的参数传递总是值传递。这意味着无论你传递一个变量给函数的时候，实际上传递的是该变量的副本（值的拷贝），而不是它的引用。因此，在被调用的函数内部对参数所做的任何修改都不会影响到原始数据。

   但当传递的是指针、切片、映射（map）、通道（channel）或接口等复合类型的变量时，他们指针虽然被复制了一份，但是指向的内存地址还是同一份。

8. golang中copy是深拷贝还是浅拷贝？

   在Go语言中，`copy`函数的行为可以被视作浅拷贝（shallow copy）。这意味着它只复制切片的元素值到目标切片，而不会递归地复制那些元素指向的对象。如果切片包含的是基本数据类型（如int、float等），这个操作表现得就像深拷贝；如果切片包含的是引用类型（如指针、切片、映射、通道或接口），`copy`函数仅复制引用，而不是它们指向的实际数据。这意味着新切片和原切片中对应的元素将指向同一个底层数据结构。

   切片、Map实际上并不是指针。它们是复合类型，有自己的内部结构，但当你将它们作为函数参数传递时，它们的行为表现得好像通过引用传递一样

9. golang中哪些类型不可以作为map的key？

   不可比较的类型都不可以作为key，会动态变化的类型都不可以作为键

   1. **切片类型**：因为切片的底层元素可能会变化，所以它们是不可比较的。试图使用切片作为`map`的键将在编译时出错。
   2. **映射类型**：就像切片一样，映射是不可比较的，因此不能作为`map`的键。
   3. **函数类型**：函数也是不可比较的，所以不能用作`map`的键。
   4. **包含上述不可比较类型的复合类型**

10. golang中内存泄漏有哪些现象？

    - 如果 goroutine 因为某种原因无法退出，例如它在等待一个永远不会发生的事件，那么与其相关的内存将无法被回收。

    - 如果你打开了文件、数据库连接或者其他类型的资源而没有关闭它们，这些资源占用的内存也不会被回收。

    - 两个对象互相引用，并且这两个对象都不再被任何外部变量引用，这在理论上形成了一个不可达的循环，但是 Go 的垃圾回收器可以处理这种情况。然而，在一些复杂的数据结构中，循环引用可能会导致内存泄漏。

    - 如果你的应用程序使用全局变量来存储大量数据，或者保持对长生命周期对象的引用，这些内存不会被释放，即使它们已经没有用处。

    - 在使用诸如 map 或 slice 等数据结构时，如果不删除不再需要的元素，这些元素仍然会占用内存。

    本质都是gc无法清理到的东西：对象可达、需手动关闭的资源

11. golang绑定方法时使用结构体或指针有什么区别？

    1. **改变状态**: 如果需要在方法内修改结构体的字段，通常使用指针接收器，以确保所有修改都会反映在原始的结构体上。

    2. **性能**: 如果结构体很大，使用指针接收器可能更有效，因为它避免了复制整个结构体的值，只需传递一个指针。

    3. **一致性**: 如果结构体的某些方法必须使用指针接收器（例如，它们需要修改结构体的状态），通常建议，为了一致性，将该结构体的所有方法都定义为指针接收器。

    4. **值类型的方法集与指针类型的方法集**: 在Go语言中，值类型（比如 `MyStruct`）的变量可以调用绑定到值类型和指针类型的方法，但指针类型（比如 `*MyStruct`）的变量只能调用绑定到指针类型的方法。这是因为Go会自动取地址或解引用，但仅限于方法调用。

       ```go
       type MyStruct struct {
           Field int
       }
       
       func (s MyStruct) SetValue(val int) {
           s.Field = val // 任何对 Field 的修改都不会影响调用该方法的原始 MyStruct 实例
       }
       
       func (s *MyStruct) SetValue(val int) {
           s.Field = val // 会反映在原始 MyStruct 实例上
       }
       ```

    
    12. golang查看json中某个key是否存在？
    
        ```java
        func recursiveSearchKey(data interface{}, keyToFind string) bool {
        	switch v := data.(type) {
        	case map[string]interface{}:
        		// 如果是 map 类型，遍历键值对。
        		for k, value := range v {
        			if k == keyToFind {
        				return true // 找到了 key。
        			}
        			// 如果值是对象或数组，则递归搜索。
        			if recursiveSearchKey(value, keyToFind) {
        				return true
        			}
        		}
        	case []interface{}:
        		// 如果是数组类型，遍历数组。
        		for _, item := range v {
        			if recursiveSearchKey(item, keyToFind) {
        				return true // 在数组某个元素中找到了 key。
        			}
        		}
        	}
        	return false // 没有找到 key。
        }
        ```

13. Golang结构体字段为什么要大写？

    ```go
    type MyStruct struct {
        PublicField  string // 可以被其他包访问
        privateField string // 只能在定义它的包内访问
    }
    ```

1. **封装**：通过控制哪些字段是对外部包可见的，Go 语言提供了一种封装机制。你可以隐藏内部状态和实现细节，只暴露那些需要被外部访问的接口。
2. **接口实现**：如果一个结构体需要实现某个接口，接口内定义的方法通常都是大写开头的，以确保它们是可导出的。因此，结构体本身也会有匹配的大写方法。
3. **反射**：在使用反射（reflect）这个强大的特性时，只有可导出的字段才能被完全操作。比如，你可能想要遍历一个结构体的所有字段并读取或修改它们的值，如果字段不是大写开头，反射在处理时将受到限制。
4. **JSON序列化与反序列化**：当使用标准库中的 `encoding/json` 包进行JSON序列化和反序列化时，只有可导出的字段会被处理。私有字段（小写开头）将不会被包含在JSON输出中，并且也不会被填充数据。

### 三、Gin框架

#### Gin的中间件

中间件实际上是一个符合 `gin.HandlerFunc` 类型签名的函数，即接收一个 `*gin.Context` 参数的函数：

```go
func MyMiddleware(c *gin.Context) {
    // 前置处理：在调用具体业务逻辑前执行
    // ...
    c.Next()  // 调用下一个中间件或最终的处理函数
    // 后置处理：可以在响应返回客户端之前执行
    // ...
}
```

- `*gin.Context` 是 Gin 提供的一个上下文对象，它存储着当前请求的所有信息，并且提供了操作请求和响应的方法。
- 中间件可以通过调用 `c.Next()` 来将控制权传递给下一个处理程序，这可能是另一个中间件或最终的请求处理函数。如果中间件没有显式地调用 `c.Next()`，则处理流程会停止，后面的中间件或请求处理函数都不会被执行。

#### 使用方式

```go
// 单个路由中间件：
r.GET("/someRoute", MyMiddleware, someHandler)
// 路由组中间件：
group := r.Group("/group")
group.Use(MyMiddleware)
```

