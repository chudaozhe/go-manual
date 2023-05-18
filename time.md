# 时间相关
`time.Time`类型表示时间。我们可以通过time.Now()函数获取当前的时间对象，然后获取时间对象的年月日时分秒等信息。
```
	now := time.Now() //获取当前时间
	fmt.Printf("current time:%v\n", now)

	year := now.Year()     //年
	month := now.Month()   //月
	day := now.Day()       //日
	hour := now.Hour()     //小时
	minute := now.Minute() //分钟
	second := now.Second() //秒
	fmt.Printf("%d-%02d-%02d %02d:%02d:%02d\n", year, month, day, hour, minute, second)
```
## 时间戳
```
	timestamp1 := now.Unix()     //10位时间戳
	timestamp2 := now.UnixNano() //纳秒
	fmt.Printf("current timestamp1:%v\n", timestamp1)
	fmt.Printf("current timestamp2:%v\n", timestamp2)
```
## 日期的字符串 转时间戳
```
	// 加载时区
	loc, _ := time.LoadLocation("Asia/Shanghai") //或者 time.Local
	// 按照指定时区和指定格式解析字符串时间
	timeObj2, _ := time.ParseInLocation("2006/01/02 15:04:05", "2019/08/04 14:15:20", loc)
	fmt.Println(timeObj2.Unix())
```
## 时间戳格式化
```
	timeObj := time.Unix(timestamp1, 0) //将时间戳转为时间对象
	fmt.Println(timeObj)
	fmt.Printf("%d-%02d-%02d %02d:%02d:%02d\n", timeObj.Year(), timeObj.Month(), timeObj.Day(), timeObj.Hour(), timeObj.Minute(), timeObj.Second())
	fmt.Println(timeObj.Format("2006-01-02 15:04:05"))

	fmt.Println(now.Format("2006-01-02 15:04:05"))    //24小时制
	fmt.Println(now.Format("2006-01-02 03:04:05 PM")) //12小时制
	fmt.Println(now.Format("2006/01/02 15:04"))
	fmt.Println(now.Format("15:04 2006/01/02"))
	fmt.Println(now.Format("2006/01/02"))
```
## 定时器
```
	ticker := time.Tick(time.Second)
	for time1 := range ticker { //每秒执行一次
		fmt.Println(time1.Second())
	}
```
## 时间操作
```
	later := now.Add(time.Hour) //1小时后
	//later := now.Add(-time.Hour) //1小时前
	fmt.Println(later)

	//返回两个时间相隔多少
	diff := now.Sub(now.Add(-time.Hour))
	fmt.Println(diff) //1h0m0s

	r := now.Equal(now)
	fmt.Println(r) //true

	//now在u之前返回true
	r = now.Before(now.Add(time.Second))
	r2 := now.Before(now.Add(-time.Second))
	fmt.Println(r)  //true
	fmt.Println(r2) //false

	//now在u之后返回true
	r = now.After(now.Add(time.Second))
	r2 = now.After(now.Add(-time.Second))
	fmt.Println(r)  //false
	fmt.Println(r2) //true
```


