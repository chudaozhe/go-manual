# 反射
反射是指在程序运行期对程序本身进行访问和修改的能力，程序在编译时变量被转换为内存地址，变量名不会被编译器写入到可执行部分，在运行程序时程序无法获取自身的信息。

支持反射的语言可以在程序编译期将变量的反射信息，如字段名称、类型信息、结构体信息等整合到可执行文件中，并给程序提供接口访问反射信息，这样就可以在程序运行期获取类型的反射信息，并且有能力修改它们。

## 反射示例
```
    func reflectType(x interface{}) {
        v := reflect.TypeOf(x)
        fmt.Printf("type:%v\n", v)
        fmt.Println(v.Name(), v.Kind())
    }

	type User struct {
	}
	var a float32 = 3.14
	var b User
	reflectType(a) // type:float32 name:float32 kind:float32
	reflectType(b) // type:main.User name:User kind:struct
```

## 结构体反射示例

示例1
```
    type Student struct {
        Name  string `json:"name"`
        Score int    `json:"score"`
    }
	stu1 := Student{
		Name:  "eric",
		Score: 90,
	}
	t := reflect.TypeOf(stu1)
	v := reflect.ValueOf(stu1)      //值
	fmt.Println(t.Name(), t.Kind()) //Student struct
	if v.Kind() == reflect.Struct { //取结构体字段的值
		name := v.FieldByName("Name").String() //eric
		score := v.FieldByName("Score").Int()  //90
		fmt.Println(name, score)
	}

	// 通过for循环遍历结构体的所有字段信息
	for i := 0; i < t.NumField(); i++ {
		field := t.Field(i)
		fmt.Printf("name:%s, IsExported:%v, index:%d, type:%v, json_tag:%v\n", field.Name, field.IsExported(), field.Index, field.Type, field.Tag.Get("json"))
	}

	// 通过字段名获取指定结构体字段信息
	if scoreField, ok := t.FieldByName("Score"); ok {
		fmt.Printf("name:%s index:%d type:%v json_tag:%v\n", scoreField.Name, scoreField.Index, scoreField.Type, scoreField.Tag.Get("json"))
	}

```

示例2
```
type Student struct {
	Name  string `json:"name"`
	Score int    `json:"score"`
}

// 给student添加两个方法 Study和Sleep(注意瘦子米大写)
func (s Student) Study() string {
	msg := "好好学习，天天向上。"
	fmt.Println(msg)
	return msg
}

func (s Student) Sleep() string {
	msg := "好好睡觉，快快长大。"
	fmt.Println(msg)
	return msg
}

func printMethod(x interface{}) {
	t := reflect.TypeOf(x)
	v := reflect.ValueOf(x)

	fmt.Println(t.NumMethod())
	for i := 0; i < v.NumMethod(); i++ {
		methodType := v.Method(i).Type()
		fmt.Printf("method name:%s\n", t.Method(i).Name)
		fmt.Printf("method:%s\n", methodType)
		// 通过反射调用方法传递的参数必须是 []reflect.Value 类型
		var args = []reflect.Value{}
		v.Method(i).Call(args)
	}
}

//遍历s包含的方法
printMethod(Student{Name: "zs", Score: 11})
```