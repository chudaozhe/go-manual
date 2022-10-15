# 文件

## 综合的
```
//读取整个文件
data, err := os.ReadFile("./test.txt")
//写入文件
err := os.WriteFile("./test.txt", []byte("jj2"), 0777)
```

## 打开文件
```
file, err := os.Open("./test.txt")
			 os.Create("./test.txt")
			 os.OpenFile("./test.txt", os.O_WRONLY|os.O_APPEND, 0644) //追加写入
			 os.OpenFile(name, O_RDWR|O_CREATE|O_TRUNC, 0666) //读写模式打开文件。文件存在则清空，不存在则创建，相当于 os.Create("./test.txt")
			 os.OpenFile(name, O_RDONLY, 0) //只读方式打开，相当于os.Open("./test.txt")
//打开后记得关闭
defer file.Close()
```

## 读取数据
打开文件后就可以读取数据了

file.Read()
```
    file, _ := os.Open("./test.txt")
    defer file.Close()
	var tmp = make([]byte, 128)
	byteSize, err := file.Read(tmp)
	if err != nil {
		fmt.Printf("red file err %s", err.Error())
		return
	}
	fmt.Println(byteSize)
	fmt.Printf("%s", tmp[:byteSize])
```

bufio.NewReader()
```
	file, _ := os.Open("./test.txt")
	reader := bufio.NewReader(file)
	line, err := reader.ReadString('\n') //读取一行
	if err != nil {
		return
	}
	fmt.Printf("%s", line)
```

## 写入数据

Write/WriteString
```
	file, _ := os.Create("./test.txt")
	writeByteSize, _ := file.Write([]byte("aaa\n")) //写入字节切片数据
	writeStringSize, _ := file.WriteString("bbb\n") //直接写入字符串数据
```

bufio.NewWriter
```
	file, _ := os.Create("./test.txt")
	defer file.Close()
	writer := bufio.NewWriter(file)
	for i := 0; i < 10; i++ {
		writer.WriteString("hello eric\n") //将数据先写入缓存
	}
	writer.Flush() //将缓存中的内容写入文件
```

io.Copy
```
	srcFile, _ := os.Open("./test.txt")
	dstFile, _ := os.Create("./testCopy2.txt")
	written, _ := io.Copy(dstFile, srcFile) //使用固定的32K缓冲区，因此无论源数据多大，都只会占用32K内存空间。
	fmt.Println(written)
```

## 完整例子
读取整个文件
```
	file, err := os.Open("./test.txt")
	if err != nil {
		fmt.Printf("open file err %s", err.Error())
		return
	}
	defer file.Close()

	content := make([]byte, 0)
	for {
		var tmp = make([]byte, 1280)
		byteSize, err := file.Read(tmp)
		if err == io.EOF {
			fmt.Printf("read file end %s", err.Error())
			break
		}
		if err != nil {
			fmt.Printf("red file err %s", err.Error())
			return
		}
		content = append(content, tmp[:byteSize]...)
	}
	fmt.Printf("%s", content)
```