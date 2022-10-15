# Map
Go语言中提供的映射关系容器为map，其内部使用散列表(hash)实现。

map是一种无序的基于key-value的数据结构，Go语言中的map是引用类型，必须初始化才能使用。

Go语言中map的定义语法如下：
```
map[KeyType]ValueType
```
- KeyType：表示键的类型。
- ValueType：表示键对应的值的类型。

map类型的变量默认初始值为nil，需要使用make()函数来分配内存。
```
make(map[KeyType]ValueType, [cap])
```
cap表示map的容量，该参数虽然不是必须的，但是我们应该在初始化map的时候就为其指定一个合适的容量。

## map基本使用
```
	scoreMap := make(map[string]int, 8)
	scoreMap["张三"] = 90
	scoreMap["小明"] = 100
	fmt.Println(scoreMap)
	fmt.Println(scoreMap["小明"])
	fmt.Printf("type of a:%T\n", scoreMap)
```

map也支持在声明的时候填充元素，例：
```
userInfo := map[string]string{
    "username": "沙河小王子",
    "password": "123456",
}
fmt.Println(userInfo) //
```

## 判断某个键是否存在

Go语言中有个判断map中键是否存在的特殊写法，格式：
```
value, ok := map[key]
```

例
```
    scoreMap := make(map[string]int)
    scoreMap["张三"] = 90
    scoreMap["小明"] = 100
    // 如果key存在ok为true,v为对应的值；不存在ok为false,v为值类型的零值
    v, ok := scoreMap["张三"]
    if ok {
        fmt.Println(v)
    } else {
        fmt.Println("查无此人")
    }
```