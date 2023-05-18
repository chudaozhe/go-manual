# Json序列化
## json.Marshal / json.Unmarshal
```
	type Article struct {
		Id      int
		Title   string
		Content string
	}
	a := &Article{
		Id:      1,
		Title:   "abc",
		Content: "ccccc",
	}
	data, _ := json.Marshal(a)
	fmt.Printf("%s\n", data)
	var a2 Article
	json.Unmarshal(data, &a2)
	fmt.Printf("%#v\n", a2)
	fmt.Println(a2.Title)
```

## 使用json tag指定字段名
```
	type User struct {
		Id   int    `json:"id"`
		Name string `json:"name"`
	}
	type Article struct {
		Id      int    `json:"id"`
		Title   string `json:"title"`
		Content string `json:"content,omitempty"` //忽略空值字段
		Weight float64 `json:"-"` //指定json序列化时忽略此字段
	    *User   `json:"user,omitempty"` //想要变成嵌套的json串，需要改为具名嵌套或定义字段tag；想要在嵌套的结构体为空值时，忽略该字段，除了添加omitempty，还需要使用嵌套结构体指针
	}
```

## 不修改原结构体忽略空值字段
```
	type Admin struct {
		Id       int    `json:"id"`
		Name     string `json:"name"`
		Password string `json:"password"`
	}
	type PublicAdmin struct {
		*Admin
		Password *struct{} `json:"password,omitempty"`
	}
	a := Admin{
		Id:       1,
		Name:     "abc",
		Password: "abc",
	}
	
	data, _ := json.Marshal(PublicAdmin{
		Admin: &a,
	})
	fmt.Printf("%s\n", data)
```

## 优雅处理字符串格式的数字
```
	type User struct {
		Id    int `json:"id,string"`
		Score int `json:"score,string"` //只需在tag中添加string
	}
	json_str := `{"id":"1", "score":"22"}`
	var u User
	json.Unmarshal([]byte(json_str), &u)
	fmt.Printf("%#v\n", u)
	fmt.Println(u.Score)
```

## 使用匿名结构体添加字段
```
    core.Success(c, 0, struct {
        *models.Admin
        Token string `json:"token"`
    }{
        &admin,
        token,
    })
```

## 使用匿名结构体组合多个结构体
```
	type Comment struct {
		Content string `json:"content"`
	}

	type Image struct {
		Title string `json:"title"`
		URL   string `json:"url"`
	}
	c1 := Comment{Content: "ccc"}
	i1 := Image{
		Title: "iii",
		URL:   "uuuu",
	}
	//序列化
	data, _ := json.Marshal(struct {
		*Comment
		*Image
	}{&c1, &i1})
	fmt.Printf("%s", data)

	//反序列化
	str := `{"content":"ccc","title":"iii","url":"uuuu"}`
	var (
		c2 Comment
		i2 Image
	)
	json.Unmarshal([]byte(str), &struct {
		*Comment
		*Image
	}{&c2, &i2})
	fmt.Printf("c2:%#v i2:%#v\n", c2, i2)
```

## 处理不确定层级的json
```
    //1
	json_str := `{"id":"1", "score":"22", "Address":{"id":3, "content":"北京"}}`
	var data map[string]json.RawMessage

	json.Unmarshal([]byte(json_str), &data)
	fmt.Printf("%s\n", data)
	fmt.Printf("%s", data["Address"])
	
	//2
	type User struct {
		Id int `json:"id"`
		Score   int
		Address json.RawMessage
	}
	json_str := `{"id":"1", "score":"22", "Address":{"id":3, "content":"北京"}}`
	var data User
	
	json.Unmarshal([]byte(json_str), &data)
	fmt.Printf("%#v\n", data)
	fmt.Printf("%s", data.Address)
```
