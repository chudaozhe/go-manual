# 字符串
Go语言里的字符串的内部实现使用UTF-8编码。字符串的值为双引号(")中的内容

拼接字符串的方法：`+号拼接`、`fmt.Sprint`、`strings.Join`、`bytes.Buffer`、`strings.Builder`。其中`strings.Join`是最快的，`+号拼接`是最慢的
## 多行字符串
Go语言中要定义一个多行字符串时，就必须使用反引号字符：
```
asd := `第一行
第二行
第三行`
fmt.Println(asd)
```
所有的转义字符均无效，文本将会原样输出

## byte和rune类型

组成每个字符串的元素叫做“字符“，可以通过遍历或者单个获取字符串元素获得字符。字符用单引号（''）包裹起来，像：
```
var a = '汉'
var b = 'x'
```
## 修改字符串
要修改字符串，需要先将其转换成[]rune或[]byte，完成后再转换为string。无论哪种转换，都会重新分配内存，并复制字节数组。

## 字符串的常用操作
### 字符串比较
```
    // Compare 函数，用于比较两个字符串的大小，如果两个字符串相等，返回为 0。如果 a 小于 b ，返回 -1 ，反之返回 1 。不推荐使用这个函数，直接使用 == != > < >= <= 等一系列运算符更加直观。
   func Compare(a, b string) int 
   //   EqualFold 函数，计算 s 与 t 忽略字母大小写后是否相等。
   func EqualFold(s, t string) bool

```
### 是否存在某个字符或子串
有三个函数做这件事：
```
// 子串 substr 在 s 中，返回 true
func Contains(s, substr string) bool
// chars 中任何一个 Unicode 代码点在 s 中，返回 true
func ContainsAny(s, chars string) bool
// Unicode 代码点 r 在 s 中，返回 true
func ContainsRune(s string, r rune) bool
```

### 子串出现次数 ( 字符串匹配 )
在 Count 的实现中，处理了几种特殊情况，属于字符匹配预处理的一部分。这里要特别说明一下的是当 sep 为空时，Count 的返回值是：`utf8.RuneCountInString(s) + 1`
```
fmt.Println(strings.Count("cheese", "e")) //3
fmt.Println(len("谷歌中国")) //12
fmt.Println(strings.Count("谷歌中国", "")) //5
```

### 字符串分割为[]string

该包提供了六个三组分割函数：Fields 和 FieldsFunc、Split 和 SplitAfter、SplitN 和 SplitAfterN。
```
    s:=strings.Fields("  foo bar  baz   ")
    fmt.Printf("Fields are: %q", s) //Fields are: ["foo" "bar" "baz"]

	s := "C:\\Windows\\System32\\FileName"
	ss := strings.FieldsFunc(s, func(r rune) bool {
		return r == '\\' || r == '/'
	})
	fmt.Printf("%q", ss) // ["C:" "Windows" "System32" "FileName"]

```

### Split 和 SplitAfter、 SplitN 和 SplitAfterN
```
	s := strings.Split("foo,bar,baz", ",")           //["foo" "bar" "baz"]
	s2 := strings.SplitAfter("foo,bar,baz", ",")     //["foo," "bar," "baz"]
	s3 := strings.SplitN("foo,bar,baz", ",", 2)      //["foo" "bar,baz"]
	s4 := strings.SplitAfterN("foo,bar,baz", ",", 2) //["foo," "bar,baz"]
```

### 字符串是否有某个前缀或后缀
```
	s := strings.HasPrefix("Gopher", "")   //true
	s2 := strings.HasSuffix("Amigo", "go") //true
```

### 字符或子串在字符串中出现的位置
```
	// 在 s 中查找 sep 的第一次出现，返回第一次出现的索引
	s := strings.Index("abcd", "b") //1
	// 在 s 中查找字节 c 的第一次出现，返回第一次出现的索引
	s1 := strings.IndexByte("abcd", 'b') //1
	// chars 中任何一个 Unicode 代码点在 s 中首次出现的位置
	s2 := strings.IndexAny("abcd", "cd") //2
	// 查找字符 c 在 s 中第一次出现的位置，其中 c 满足 f(c) 返回 true
	s3 := strings.IndexFunc("abcd", func(r rune) bool {
		return unicode.Is(unicode.Han, r) // 汉字
	}) //-1
	// Unicode 代码点 r 在 s 中第一次出现的位置
	s4 := strings.IndexRune("abcd", 'b') //1
	
	// 有三个对应的查找最后一次出现的位置
    func LastIndex(s, sep string) int
    func LastIndexByte(s string, c byte) int
    func LastIndexAny(s, chars string) int
    func LastIndexFunc(s string, f func(rune) bool) int
```

### 字符串 JOIN 操作
```
	//strs := []string{"Monday", "Tuesday", "Wednesday"}
	//str1 := strings.Join(strs, ",")
	//println(str1)
```

### 字符串重复几次
```
	s := strings.Repeat("na", 2) //重复两次, nana
```

### 字符替换
```
func Map(mapping func(rune) rune, s string) string
```

### 字符串子串替换
```
	// 用 new 替换 s 中的 old，一共替换 n 个。
	// 如果 n < 0，则不限制替换次数，即全部替换
	// 等于0不替换
	s := strings.Replace("abc", "a", "A", -1)
	// 该函数内部直接调用了函数 Replace(s, old, new , -1)
	s2 := strings.ReplaceAll("abc", "a", "A")
	fmt.Println(s)
	fmt.Println(s2)
```

### 大小写转换
```
	s := strings.ToLower("HELLO WORLD")
	s2 := strings.ToLowerSpecial(unicode.TurkishCase, "Ā Á Ǎ À")
	s3 := strings.ToUpper("hello")
	s4 := strings.ToUpperSpecial(unicode.TurkishCase, "ā á ǎ à")
	fmt.Println(s)
	fmt.Println(s2)
	fmt.Println(s3)
	fmt.Println(s4)
```

### 标题处理
```
fmt.Println(strings.Title("hElLo wOrLd")) // 首字母大写，HElLo WOrLd
fmt.Println(strings.ToTitle("hElLo wOrLd")) // 全部大写，HELLO WORLD
fmt.Println(strings.ToTitleSpecial(unicode.TurkishCase, "hElLo wOrLd")) //全部大写，包括一些特殊字符
```

### 修剪
```
	// 将 s 左侧和右侧中匹配 cutset 中的任一字符的字符去掉
	s := strings.Trim("% aaba #", "#%") // aaba
	// 将 s 左侧的匹配 cutset 中的任一字符的字符去掉
	s2 := strings.TrimLeft("% aaba ", "%") // aaba
	// 将 s 右侧的匹配 cutset 中的任一字符的字符去掉
	s3 := strings.TrimRight(" aaba #", "#") // aaba
	// 如果 s 的前缀为 prefix 则返回去掉前缀后的 string , 否则 s 没有变化。
	s4 := strings.TrimPrefix(" aaba ", " a") //aba
	// 如果 s 的后缀为 suffix 则返回去掉后缀后的 string , 否则 s 没有变化。
	s5 := strings.TrimSuffix(" aaba ", "a ") // aab
	// 将 s 左侧和右侧的间隔符去掉。常见间隔符包括：'\t', '\n', '\v', '\f', '\r', ' ', U+0085 (NEL)
	s6 := strings.TrimSpace(" aaba ") //aaba
	// 将 s 左侧和右侧的匹配 f 的字符去掉
	s7 := strings.TrimFunc("% aaba #", func(r rune) bool {
		return r == '%' || r == '#'
	}) // aaba 
	// 将 s 左侧的匹配 f 的字符去掉
	s8 := strings.TrimLeftFunc("%@ aaba #", func(r rune) bool {
		return r == '%'
	}) //@ aaba #
	// 将 s 右侧的匹配 f 的字符去掉
	s9 := strings.TrimRightFunc("% aaba $#", func(r rune) bool {
		return r == '#'
	}) //% aaba $
```

### Replacer 类型
进行多个替换。如果 oldnew 长度与奇数，会导致 panic
```
	r := strings.NewReplacer("{name}", "zs", "{email}", "11@qq.com")
	fmt.Println(r.Replace("{name}你好，我的email是{email}")) //zs你好，我的email是11@qq.com
```