# NSKeyedArchiver （数据归档）

- 所谓的**归档**，就是将数据写到一个文件里面去。一般我们的应用的变量常量之类的数据都是在内存里面的，只要APP关闭，这些数据都会丢失。但是把数据存储到文件里面去，就能将数据保存到本地磁盘里面（目前iOS基本就是在沙盒里面操作了），不管是APP关闭还是重启设备，下次启动APP的时候都能够读出来。

- 所谓**解档**(别人也叫**反归档**)，就是将数据从文件里面读取出来。在程序里面使用。



> #### 普通数组的归档和解档

```objective-c
//沙盒home目录
NSString *docPath = NSHomeDirectory();
//完整的文件路径
NSString *path = [docPath stringByAppendingPathComponent:@"Documents/Archive/numbers.plist"];
    
//获取文件夹路径
NSString *directory = [path stringByDeletingLastPathComponent];

//判断文件夹是否存在
BOOL fileExists = [[NSFileManager defaultManager] fileExistsAtPath:directory isDirectory:nil];
//如果不存在则创建文件夹
if (!fileExists) {
        NSLog(@"文件夹不存在");
        //创建文件夹
        NSError *error = nil;
        [[NSFileManager defaultManager] createDirectoryAtPath:directory withIntermediateDirectories:YES attributes:nil error:&error];
        if (error) {
            NSLog(@"error=%@",error.description);
        } else {
            NSLog(@"文件夹创建成功");
        }
}

if (![[NSFileManager defaultManager] isWritableFileAtPath:directory]) {
        NSLog(@"目录无写入权限");
}

NSArray *numbers = @[@"one",@"two"];
    
//将数据归档到path文件路径里面
BOOL success = [NSKeyedArchiver archiveRootObject:numbers toFile:path];    
if (success) {
        NSLog(@"文件归档成功");
}else {
        NSLog(@"归档失败");
}

//将path文件路径的数据解档出来
NSArray *numbers = [NSKeyedUnarchiver unarchiveObjectWithFile:path];
NSLog(@"numbers=%@",numbers);
```



一般因为没有权限导致归档失败，都是在两个目录：

1. 安装包目录`[[NSBundle mainBundle] bundlePath]`。这个很明显。我们一般只操作沙盒目录，对安装包目录是没有权限的（模拟器不算）。
2. 沙盒home目录的根目录。这个比较容易出错了。我们明明可以操作沙盒的，为什么没有权限呢？
    因为你是在沙盒的根目录操作。
    `NSHomeDirectory`这个就是沙盒的根目录。直接在这个目录下归档也是会导致归档失败的。一般我们都是在沙盒的Documents目录里面玩。



> #### 普通字典的归档和解档

```objective-c
//沙盒ducument目录
NSString *docPath = [NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES) objectAtIndex:0];
//完整的文件路径
// 这里的后缀名用了.archive。实际上对于归档操作来说，用什么后缀名都无所谓的。只是我们一般习惯用.archive。
// 之前数组的时候用.plist，是为了方便演示。
NSString *path = [docPath stringByAppendingPathComponent:@"personDict.archive"];
NSDictionary *personDict = @{@"name":@"shixueqian",@"age":@(27)};
//将数据归档到path文件路径里面
BOOL success = [NSKeyedArchiver archiveRootObject:personDict toFile:path];
if (success) {
        NSLog(@"归档成功");
}else {
        NSLog(@"归档失败");
}

//将path文件路径的数据解档出来
NSDictionary *personDict = [NSKeyedUnarchiver unarchiveObjectWithFile:path];
NSLog(@"personDict=%@",personDict);
```

> #### 多个普通对象同时归档和解档

```objective-c
//沙盒ducument目录
NSString *docPath = [NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES) objectAtIndex:0];
//完整的文件路径
NSString *path = [docPath stringByAppendingPathComponent:@"manyData.plist"];
    
NSInteger age = 27;
NSString *name = @"shixueqian";
NSArray *array = @[@"one",@"two"];
    
NSMutableData *data = [[NSMutableData alloc] init];
NSKeyedArchiver *archiver = [[NSKeyedArchiver alloc] initForWritingWithMutableData:data];
[archiver encodeObject:name forKey:@"name"];
[archiver encodeInteger:age forKey:@"age"];
[archiver encodeObject:array forKey:@"numbers"];
    
//完成归档
[archiver finishEncoding];
    
//将归档好的数据写到文件里面
[data writeToFile:path atomically:YES];


// 【解档】
NSMutableData *data = [NSMutableData dataWithContentsOfFile:path];
NSKeyedUnarchiver *unarchiver = [[NSKeyedUnarchiver alloc] initForReadingWithData:data];
NSString *name = [unarchiver decodeObjectForKey:@"name"];
NSInteger age = [unarchiver decodeIntegerForKey:@"age"];
NSArray *numbers = [unarchiver decodeObjectForKey:@"numbers"];
[unarchiver finishDecoding];
//
NSLog(@"name=%@，age=%zd,numbers=%@",name,age,numbers);
```

> #### 自定义对象的归档和解档

- 如果想要对自定义的类创建出来的对象进行归档，我们需要先实现NSCoding协议。
```objective-c
#import <UIKit/UIKit.h>

@interface Person : NSObject<NSCoding>

  @property (nonatomic,copy)NSString *name;

  @property (nonatomic,assign)NSInteger age;

  //有些属性可以不进行归档
  @property (nonatomic,assign)CGFloat height;

@end


#import "Person.h"

@implementation Person

//NSCoding协议方法：将需要归档的属性进行归档
- (void)encodeWithCoder:(NSCoder *)aCoder {    
    [aCoder encodeObject:self.name forKey:@"name"];
    [aCoder encodeInteger:self.age forKey:@"age"];
}

//NSCoding协议方法：将需要解档的属性进行解档
- (instancetype)initWithCoder:(NSCoder *)aDecoder {    
    if (self = [super init]) {
        self.name = [aDecoder decodeObjectForKey:@"name"];
        self.age = [aDecoder decodeIntegerForKey:@"age"];
    }
    
    return self;
}

//重写description方法，方便直接打印对象
- (NSString *)description {
    return [NSString stringWithFormat:@"person.name=%@,person.age=%zd,person.height=%f",self.name,self.age,self.height];
}

@end
  
  
// 使用 
//沙盒ducument目录
NSString *docPath = [NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES) objectAtIndex:0];
//完整的文件路径
NSString *path = [docPath stringByAppendingPathComponent:@"person.archive"];
    
Person *person = [[Person alloc] init];
person.name = @"谦言忘语";
person.age = 27;
    
//将数据归档到path文件路径里面
BOOL success = [NSKeyedArchiver archiveRootObject:person toFile:path];
    
if (success) {
        NSLog(@"归档成功");
}else {
        NSLog(@"归档失败");
}

Person *person = [NSKeyedUnarchiver unarchiveObjectWithFile:path];
NSLog(@"person=%@",person);
```



