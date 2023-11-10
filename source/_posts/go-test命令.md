---
title: Go test命令
date: 2023-11-07 15:43:29
tags:
  - golang
categories:
---

Go 语言拥有一套单元测试和性能测试系统，仅需要添加很少的代码就可以快速测试一段需求代码。

<!-- more -->

go test 命令，会自动读取源码目录下面名为 `*_test.go` 的文件，生成并运行测试用的可执行文件。

##### 提示

单元测试（unit testing），是指对软件中的最小可测试单元进行检查和验证。对于单元测试中单元的含义，一般要根据实际情况去判定其具体含义，如 C 语言中单元指一个函数，Java 里单元指一个类，图形化的软件中可以指一个窗口或一个菜单等。总的来说，单元就是人为规定的最小的被测功能模块。

单元测试是在软件开发过程中要进行的最低级别的测试活动，软件的独立单元将在与程序的其他部分相隔离的情况下进行测试。

## 单元测试

要开始一个单元测试，需要准备一个 go 源码文件，在命名文件时需要让文件必须以\_test 结尾。默认的情况下，go test 命令不需要任何的参数，它会自动把你源码包下面所有 test 文件测试完毕，当然你也可以带上参数。

常用参数：

- `-bench regexp`, 执行相应的 benchmarks, 例如 `-bench=.` ;
- `-cover`, 开启测试覆盖率
- `-run regexp`, 只运行 `regexp` 匹配的函数，例如 `-run=Array` 那么就执行包含有 `Array` 开头的函数；
- `-v`, 显示测试的详细命令

单元测试源码文件可以由多个测试用例组成，每个测试用例函数需要以`Test`为前缀，例如：

```go
func TestXXX( t *testing.T )
```

- 测试用例文件不会参与正常源码编译，不会被包含到可执行文件中。
- 测试用例文件使用`go test`指令来执行，没有也不需要 `main()` 作为函数入口。所有在以`_test`结尾的源码内以`Test`开头的函数会自动被执行。
- \_测试用例可以不传入 `*testing.T` 参数。

示例代码

```go
import "testing"

func TestHelloWorld(t *testing.T){
  t.Log("hello world")
}
```

- 单元测试文件 (*\_test.go) 里的测试入口必须以 Test 开始，参数为 *testing.T 的函数。一个单元测试文件可以有多个测试入口。

#### 1) 单元测试命令行

单元测试使用`go test`命令启动，如：

```bash
$ go test src/gopl.io/ch2/testecho/main_test.go
ok          command-line-arguments        0.001s

$ go test -v src/gopl.io/ch2/testecho/main_test.go
=== RUN   TestHelloWorld
    main_test.go:6: hello world
--- PASS: TestHelloWorld (0.00s)
PASS
ok      command-line-arguments  0.001s
```

代码说明：

- ok 表示测试通过，`command-line-arguments` 是测试用例需要用到的一个包名，`0.001s` 表示测试花费的时间。
- 附加参数 `-v` 可以在测试时显示详细的流程。

#### 2) 运行指定单元的测试用例

```go
import "testing"

func TestA(t *testing.T) {
    t.Log("A")
}

func TestAK(t *testing.T) {
    t.Log("AK")
}

func TestB(t *testing.T) {
    t.Log("B")
}

func TestC(t *testing.T) {
    t.Log("C")
}

```

指定 TestA 进行测试：

```bash
$ go test -v -run TestA src/gopl.io/ch2/testecho/select_test.go

=== RUN   TestA
    select_test.go:6: A
--- PASS: TestA (0.00s)
=== RUN   TestAK
    select_test.go:10: AK
--- PASS: TestAK (0.00s)
PASS
ok      command-line-arguments  0.001s

```

TestA 和 TestAK 的测试用例都被执行，原因是`-run`跟随的测试用例的名称支持正则表达式，使用`-run TestA$`即可只执行 TestA 测试用例。

#### 3) 标记单元测试结果

当需要终止当前测试用例时，可以使用`FailNow`,参考一下代码：

```go
func TestFailNow(t *testing.T) {
	fmt.Println("11111")
	t.FailNow()
	fmt.Println("22222")
}
```

运行结果：

```bash
Running tool: /usr/local/go/bin/go test -timeout 30s -run ^TestFailNow$ gopl.io/ch2/testecho

11111
--- FAIL: TestFailNow (0.00s)
FAIL
FAIL	gopl.io/ch2/testecho	0.001s
FAIL
```

还有一种只标记错误不终止测试的方法，使用`Fail`：

```go
func TestFail(t *testing.T) {
	fmt.Println("11111")
	t.Fail()
	fmt.Println("22222")
}
```

运行结果：

```bash
Running tool: /usr/local/go/bin/go test -timeout 30s -run ^TestFail$ gopl.io/ch2/testecho

11111
22222
--- FAIL: TestFail (0.00s)
FAIL
FAIL	gopl.io/ch2/testecho	0.001s
FAIL
```

#### 4) 单元测试日志

每个测试用例可能并发执行，使用 testing.T 提供的日志输出可以保证日志跟随这个测试上下文一起打印输出。testing.T 提供了几种日志输出方法，详见下表所示。

<center>
单元测试框架提供的日志方法

| 方 法  | 备 注                            |
| :----: | -------------------------------- |
|  Log   | 打印日志，同时结束测试           |
|  Logf  | 格式化打印日志，同时结束测试     |
| Error  | 打印错误日志，同时结束测试       |
| Errorf | 格式化打印错误日志，同时结束测试 |
| Fatal  | 打印致命日志，同时结束测试       |
| Fatalf | 格式化打印致命日志，同时结束测试 |

</center>

### 基准测试-获得代码内存占用和运行效率的性能数据

基准测试可以测试一段程序的运行性能及耗费 CPU 的程度。Go 语言中提供了基准测试框架，使用方法类似于单元测试，使用者无须准备高精度的计时器和各种分析工具，基准测试本身即可以打印出非常标准的测试报告。

#### 1) 基准测试基本使用

基准测试：

```go
import "testing"

func Benchmark_Add(b *testing.B) {
    var n int
    for i := 0; i < b.N; i++ {
        n++
    }
}
```

这段代码使用基准测试框架测试加法性能。第 7 行中的 b.N 由基准测试框架提供。测试代码需要保证函数可重入性及无状态，也就是说，测试代码不使用全局变量等带有记忆性质的数据结构。避免多次运行同一段代码时的环境不一致，不能假设 N 值范围。

使用如下命令行开启基准测试：

```bash
$ go test -v -bench=. src/gopl.io/ch2/testecho/benchmark_test.go
goos: linux
goarch: amd64
cpu: AMD Ryzen 7 3700X 8-Core Processor
Benchmark_Add
Benchmark_Add-16        1000000000               0.2411 ns/op
PASS
ok      command-line-arguments  0.271s
```

代码说明：

- `-bench=.`表示运行 benchmark_test.go 文件里的所有基准测试，和单元测试中的`-run`类似
- 第 6 行中显示基准测试名称，1000000000 表示测试的次数，也就是 testing.B 结构中提供给程序使用的 `N`。“0.2411 ns/op”表示每一个操作耗费多少时间（纳秒）。

注意：Windows 下使用 go test 命令行时，`-bench=.`应写为`-bench="."`。

#### 2) 基准测试原理

基准测试框架对一个测试用例的默认测试时间是 1 秒。开始测试时，当以 `Benchmark` 开头的基准测试用例函数返回时还不到 1 秒，那么 testing.B 中的 N 值将按 1、2、5、10、20、50……递增，同时以递增后的值重新调用基准测试用例函数。

#### 3) 自定义测试时间

通过`-benchtime`参数可以自定义测试时间，例如：

```bash
$ go test -v -bench=. -benchtime=5s  src/gopl.io/ch2/testecho/benchmark_test.go
goos: linux
goarch: amd64
cpu: AMD Ryzen 7 3700X 8-Core Processor
Benchmark_Add
Benchmark_Add-16        1000000000               0.2409 ns/op
PASS
ok      command-line-arguments  0.271s
```

- 使用 `-benchtime=5s` 来设置每个基准测试函数的运行时间限制为 5 秒。这意味着基准测试工具会运行 Benchmark_Add 函数，直到满足 5 秒的条件，然后提供相应的性能测量结果。

#### 4) 测试内存

基准测试可以对一段代码可能存在的内存分配进行统计，下面是一段使用字符串格式化的函数，内部会进行一些分配操作。

```go
func Benchmark_Alloc(b *testing.B) {
    for i := 0; i < b.N; i++ {
        fmt.Sprintf("%d", i)
    }
}
```

在命令行中添加`-benchmem`参数以显示内存分配情况，参见下面的指令：

```bash
$ go test -v -bench=Alloc  -benchmem  src/gopl.io/ch2/testecho/benchmark_test.go
goos: linux
goarch: amd64
cpu: AMD Ryzen 7 3700X 8-Core Processor
Benchmark_Alloc
Benchmark_Alloc-16      11972752        101.0 ns/op      16 B/op     1 allocs/op
PASS
ok      command-line-arguments  1.315s
```

- “16 B/op”表示每一次调用需要分配 16 个字节，“2 allocs/op”表示每一次调用有两次分配。

#### 5) 控制计时器

有些测试需要一定的启动和初始化时间，如果从 Benchmark() 函数开始计时会很大程度上影响测试结果的精准性。testing.B 提供了一系列的方法可以方便地控制计时器，从而让计时器只在需要的区间进行测试。我们通过下面的代码来了解计时器的控制。

```go
func Benchmark_Add_TimerControl(b *testing.B) {

    // 重置计时器
    b.ResetTimer()

    // 停止计时器
    b.StopTimer()

    // 开始计时器
    b.StartTimer()

    var n int
    for i := 0; i < b.N; i++ {
        n++
    }
}
```

从 Benchmark() 函数开始，Timer 就开始计数。StopTimer() 可以停止这个计数过程，做一些耗时的操作，通过 StartTimer() 重新开始计时。ResetTimer() 可以重置计数器的数据。

计数器内部不仅包含耗时数据，还包括内存分配的数据。
