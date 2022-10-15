## Print
Print系列函数会将内容输出到系统的标准输出，区别在于Print函数直接输出内容，printf函数支持格式化输出字符串，Println函数会在输出内容的结尾添加一个换行符。

Printf
```
    var n = 100
    // 查看类型
    fmt.Printf("%T\n", n)  // 查看数据类型
    fmt.Printf("%v\n", n)  // 查看变量值
    fmt.Printf("%b\n", n)  // 二进制
    fmt.Printf("%d\n", n)  // 十进制
    fmt.Printf("%o\n", n)  // 八进制
    fmt.Printf("%x\n", n)  // 十六进制

    var s = "word"
    fmt.Printf("%s\n", s)  // 字符串 word  不会体现类型
    fmt.Printf("%v\n", s)  // 字符串 word  不会体现类型
    fmt.Printf("%#v\n", s)  // 字符串 "word"  体现出了具类型
```

## Fprint
Fprint系列函数会将内容输出到一个io.Writer接口类型的变量w中，我们通常使用这个函数往文件中写入内容。

```
// 向标准输出写入内容
fmt.Fprint(os.Stdout, "向标准输出写入内容")

// 向打开的文件句柄中写入内容
file, _ := os.Create("./test.txt")
fmt.Fprintf(file, "往文件中写入信息：%s", "haha")
```

## Sprint
Sprint系列函数会把传入的数据生成并返回一个字符串。

```
s1 := fmt.Sprint("Eric")
fmt.Println(s1)
```

## Errorf
