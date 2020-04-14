## NSString


> NSString 字符串的恒定性

```objective-c
// 使用@"" 创建的字符串存储在常量区
NSString *str1 = @"Jack" ;

// 使用NSString类方法创建的字符串存储在堆区
NSString *str2 = [NSString strignWithFormat:@"Jack"] ;

/*  恒定性  */
// 当在内存中创建字符串对象后，这个字符串对象的内容就无法更改
```


> 常用的方法


```objective-c

// 1. NSString是Foundation框架中的一个类
// NSString *str = [NSString new] ;
// NSString *str = [[NSString alloc] init] ;
// NSString *str = [NSString string] ;

NSString *str = @"我是字符串" ;

// 返回字符串长度  NSUInteger
[str length] ;


// 获取字符串中某个下标的字符
[str characterAtIndex:0] ;


NSString *url = @"http://www.baidu.com" ;
// 判断字符串的开头
[url hasPrefix:@"http://"] ; 
// 判断字符串的结尾
[url hasSuffix:@".png"] ;


// 判断字符串是否相等
int res = [str isEqualToString:@"我是被比较的字符串"] ;


// 打印指针指向的对象
NSLog(@"%@",str) ;

// 打印指针的值
NSLog(@"%p",str) ;



// 拼接OC字符串
NSString *name = @"小明" ;
int age = 18 ;
[NSString stringWithFormat:@"我的名字叫%@,我今年%d",name,age] ;


```

> 比较字符串大小
```objective-c
// 比较字符串大小
NSString *str1 = @"Jack" ;
NSString *str2 = @"Rose" ;

// -- options --
// NSCaseInsensitiveSearch 忽略大小写
// NSLiteralSearch 完全匹配
// NSNumericSearch 两个相同格式字符串中的数字大小
//
// 返回枚举值
NSComparisonResult rs = [str1 compare:str2 options:nil] ;

if(res == NSOrderdAscending){
    NSLog(@" str1 比 str2 小") ;
}
else if(res == NSOrderSame){
    NSLog(@" str1 和 str2 一样大") ;
}
else if(res == NSOrderDescending){
    NSLog(@" str1 比 str2 大") ;
}
```

> 字符串数据转换为其他类型
```objective-c
// 字符串数据转换为其他类型
NSString *numStr = @"123" ;
numStr.intValue ;
numStr.doubleValue ;

// 将C语言的字符串转换为OC字符串
NSString *content = [NSString stringWithUTF8String:"我是c语言字符串"] ;
// 将OC字符串转换为C字符串
char *s = content.UTF8String ;
```


> 字符串截取
```objective-c
// 截取 (从第三个下标截取到最后的子字符串)
[url substringFromIndex:3] ;								// 小标从3到最后
[url substringToIndex:3] ;									// 下标从0到3
[url substringWithRange:NSMakeRange(3,7)] ; // 下标从3到7
```


> 替换
```objective-c
NSString *newStr = [url stringByReplacingOccurrencesOfString:@"baidu" withString:@"google"];
```


> 搜索子字符串
```objective-c
// 搜索子字符串第一次出现的范围（从前往后搜索）
// NSRange{location,length}
NSRange range = [url rangOfString:@"baidu"] ;
// 根据返回值中length是否为0，可以判断是否找到子字符串的位置
```

> 去除字符串中的指定字符集
```objective-c
// 去除字符串前后的空格
NSString *str = @"   338383" ;
str = [str stringByTrimingCharactersInSet:[NSCharacterSet whitespaceCharacterSet]] ;

// 去掉字符串中的大写字母
[NSCharacterSet uppercaseLetterCharacterSet]

// 去掉字符串中的小写字母
[NSCharacterSet lowercaseLetterCharacterSet]

```


> 字符串的读写

```objective-c
// 1. 将字符串内容写入磁盘文件
NSString *str = @"我的名字叫牛牛" ;
NSError *err ;
BOOL res = [str writeToFile:@"/Users/xxx/Desktop/abc.txt" atomically:nil encoding:NSUTF8StringEncoding error:&err] ;
if(res){
   // 写入成功
}
else{
  // 写入失败,打印原因
  NSLog(@"%@",err) ;
}


// 2. 从磁盘文件中读取字符串
NSError *err2 ;
NSString *str2 = [NSString stringWithContentsOfFile:@"/Users/xxx/Desktop/abc.txt" encoding:NSUTF8StringEncoding error:&err2] ;
if(err2 == nil){
  // 读取成功
}
else{
  // 读取失败,打印原因
  NSLog(@"%@",err) ;
}
```

> 字符串大小写转换

```objective-c
NSString *str = @"aBcD" ;
str = [str uppercaseString] ;
str = [str lowercaseString] ;
```



> 使用URL读写字符串数据

```objective-c
// 1.本地磁盘文件  file:///Users/xxx/Desktop/abc.txt
// 2.网络地址.    http://www.baidu.com/a.txt
// 3.Ftp地址     ftp://server.baidu.com/a.txt


NSURL *url = [NSURL URLWithString:@"http://www.baidu.com/"] ;
NSError *err ;
NSString *str = [NSString stringWithContentsOfURL:url encoding:NSUTF8StringEncoding error:&err] ;

BOOL res = [str writeToURL:url atomically:nil encoding:NSUTF8StringEncoding error:&err] ;


```

## NSMutableString:NSString

使用 NSMutableString 来做大批量的字符串拼接（5次以上）

```objective-c
//修改 NSMutableString 不会新创建对象
NSMutableString *str = [NSMutableString string] ;

// 添加字符串
[str appendString:@"Hello"] ;
[str appendString:@" World!"] ;

//
int age = 10 ;
[str appendFormat:@"我今年%d岁了",age] ;

// 
```

---

时间：2020-4-8