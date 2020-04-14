# NSDictionary

- 以 键值对 存储方式
- 字典数组
- 一旦创建，无法动态新增和删除
- Key: 遵守 NSCoping 协议的 OC 对象
- Value: OC对象
- 键不允许重复(覆盖原有值)

> 创建

```objective-c
NSDictionary *dic = [NSDictionary new] ;
NSDictionary *dic = [NSDictionary dictionary] ;

// 
NSDictionary *dic = [NSDictionary dictionaryWithObjectsAndKeys:@"jack",@"name",@"shanghai",@"city",nil] ;

// 简便方式
NSDictionary *dic = @{
	@"name":@"jack",
	@"city":@"shanghai"
}
```



> 使用

```objective-c

NSDictionary *dic = @{
	@"name":@"jack",
	@"city":@"shanghai"
}

NSString *city = [dic objectForKey:@"city"] ;

```



> 遍历

```objective-c

NSDictionary *dic = @{
	@"name":@"jack",
	@"city":@"shanghai"
} ;

// 方式一  for 
for(NSString *key in dic){
  	NSString *val = [dic objectForKey:key] ;
} ;

//方式二  NSEnumerator 
NSEnumerator *objs = [dic objectEnumerator] ;
id obj ;
while((obj = [objs nextObject]))
{
  
}

// 键 迭代对象
NSEnumerator *keys = [dic keyEnumerator] ;


// 方式二  block
[dic enumerateKeysAndObjectsUsingBlock:^(id  _Nonnull key, id  _Nonnull obj, BOOL * _Nonnull stop) {
        //
    }] ;

```



# NSMutableDictionary

- 可以动态新增和删除键值对

```objective-c
NSMutableDictionary *dic = [NSMutableDictionary dictionaryWithObjectsAndKeys:@"jack",@"name",@"shanghai",@"city",nil] ;

// 增加键值对
[dic setObject:@"18" forKey:@"age"] ;


// 删除指定键值对
[dic removeObjectForKey:@"age"] ;


// 删除所有键值对
[dic removeAllObjects] ;
```



# 持久化

```
// 写入文件
[dic writeToFile:@"/Users/Apple/Desktop/dict.plist" atomically:NO] ;

// 从文件中读取字典
NSDictionary *dic = [NSDictionary dictionaryWithContentsOfFile:@"/Users/Apple/Desktop/dict.plist"] ;
```



---

时间：2020-4-8