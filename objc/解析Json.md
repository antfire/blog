# **Objective-c json**

通过使用NSJSONSerialization 可以Json与Foundation的相互转换。下面具体介绍 Objective-c json 的使用。



### **Json To Fundation**

```objective-c
NSString* items = @"{"items":["item0","item1","item2"]}"; 
NSData *data= [items dataUsingEncoding:NSUTF8StringEncoding];
NSError *error = nil;
id jsonObject = [NSJSONSerialization JSONObjectWithData:data
                    options:NSJSONReadingAllowFragments
                    error:&amp;error];

// Json的顶层可以是{} 或 []因此可以有 NSDictionary 和 NSArray 两种格式
if ([jsonObject isKindOfClass:[NSDictionary class]]){
 
    NSDictionary *dictionary = (NSDictionary *)jsonObject;
 
    NSLog(@"Dersialized JSON Dictionary = %@", dictionary);
 
}else if ([jsonObject isKindOfClass:[NSArray class]]){
 
    NSArray *nsArray = (NSArray *)jsonObject;
 
    NSLog(@"Dersialized JSON Array = %@", nsArray);
 
} else {
 
    NSLog(@"An error happened while deserializing the JSON data.");
 
}
 
NSDictionary *dict = (NSDictionary *)jsonObject;
 
NSArray* arr = [dict objectForKey:@"items"];
NSLog(@"list is %@",arr);
```



### **Fundation To Json**

```objective-c
NSArray *myProduct = response.products;
NSDictionary *myDict;
NSMutableDictionary *dict = [NSMutableDictionary                               dictionaryWithCapacity: 4];
 
for(int i  = 0;i&lt;myProduct.count;++i)
{
 
    //NSLog(@"----------------------");
    //NSLog(@"Product title: %@" ,[myProduct[i] localizedTitle]);
    //NSLog(@"Product description: %@" ,[myProduct[i] localizedDescription]);
    //NSLog(@"Product price: %@" ,[myProduct[i] price]);
    //NSLog(@"Product id: %@" ,[myProduct[i] productIdentifier]);
 
    myDict = [NSDictionary dictionaryWithObjectsAndKeys:
                    [myProduct[i] localizedTitle], @"title",
                    [myProduct[i] localizedDescription], @"desc",
                    [myProduct[i] price], @"price",
                    [myProduct[i] productIdentifier], @"product", nil];
 
    [dict setValue: myDict forKey: [myProduct[i] productIdentifier]];
}

//
if([NSJSONSerialization isValidJSONObject:dict])
{
    NSError* error;
    NSData *str = [NSJSONSerialization dataWithJSONObject:dict
                        options:kNilOptions error:&amp;error];
    NSLog(@"Result: %@",[[NSString alloc]initWithData:str
                            encoding:NSUTF8StringEncoding]);
}
else
{
    NSLog(@"An error happened while serializing the JSON data.");
}      
```



