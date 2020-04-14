# NSArray

> 创建

```objective-c

// 1. 只能存储OC对象
// 2. 无法修改

NSArray *arr = [NSArray new] ;
NSArray *arr = [NSArray array] ;
NSArray *arr = [[NSArray alloc] init] ;

// 要以nil结尾
NSArray *arr = [NSArray arrayWithObjects:@"Jack",@"rose",@"lili",nil] ;

// 快捷方式
NSArray *arr = @[@"Jack",@"rose",@"lili"] ;

```



> 使用

```objective-c
NSArray *arr = @[@"Jack",@"rose",@"lili"] ;

// 使用下标获取值
arr[0]
  
// 使用对象方法获取值 
[arr objectAtIndex:1] ;

// 数组长度
arr.count

// 是否包含元素
[arr containsObject:@"lili"]
  
// 数组中首个元素
arr.firstObject

// 数组中最后一个元素
arr.lastObject
  
// 查找元素下标
NSUInteger idx = [arr indexOfObject:@"lili"] ;
```


> 遍历

```objective-c
NSArray *arr = @[@"Jack",@"rose",@"lili"] ;

// 方式一
for (int i = 0 ;i<arr.count;i++){
    NSLog(@"%d = %@",i,arr[i]) ;
}

// 方式二  (item 称为迭代变量)
for(NSString *item in arr)
{
  NSLog(@"%@",item) ;
}

//方式三  NSEnumerator 
NSEnumerator *objs = [arr objectEnumerator] ;
id obj ;
// 
while((obj = [objs nextObject]))
{
  NSLog(@"I found %@",obj) ;
}

// 方式四
[arr enumerateObjectsUsingBlock:^(id _Nonnull obj,NSUInteger idx,BOOL * _Nonnull stop){
   NSLog(@"%@",obj) ;
  // 如果想停止遍历 将 *stop = YES
}];
```

> 数组和字符串

```objective-c
NSArray *arr = @[@"Jack",@"rose",@"lili"] ;

// 链接元素
NSString *str = [arr componentsJoinedByString:@"#"] ;

// 分割
NSString *str2 = @"Jack,rose,lili" ;
NSArray *arr2 = [str2 componentsSeparatedByString:@","] ; 
```



# NSMutableArray:NSArray

```objective-c
// 元素可以动态新增和删除

NSMutableArray *arr = [NSMutableArray new] ;
NSMutableArray *arr = [NSMutableArray array] ;

//
NSMutableArray *arr = [NSMutableArray arrayWithObjects:@"Jack",@"rose",@"lili",nil] ;

// 增加元素
[arr addObject:@"fraser"] ;
[arr addObjectsFromArray:@[@"julian",@"vivian",@"miller"]]

// 在制定位置插入元素
[arr insertObject:@"Simon" atIndex:1] ;

// 移除元素
[arr removeObjectAtIndex:1] ;
[arr removeObject:@"Jack"] ;
[arr removeLastObject] ;
[arr removeAllObjects] ;

```





---

时间：2020-4-8