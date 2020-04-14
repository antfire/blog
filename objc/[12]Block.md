# Blocks

- 是一种数据类型
- 专门用于存储1段代码（可以有参数 有返回值）

```objective-c
// 返回值类型 (^block变量名称)(参数类型列表)

void (^blockName1)() ;
int (^blockName2)() ;
int (^blockName3)(int,int) ;  



// 代码段格式 (无参数无返回值)
blockName1 = ^void(){
  NSLog(@"我是block") ;
};

// 当代码段没有返回值和参数时可以各自省略
// blockName1 = ^{
//  NSLog(@"我是block") ;
// };

blockName2 = ^int(){
  return 100 ;
};

blockName3 = ^int(int num1,int num2){
  return num1 + num2 ;
};


// 执行
blockName1() ;
blockName2() ;
blockName3() ;
```

> 使用 typedef 简化定义

```objective-c
typedef void (^NewType)()  ; // 代表重新定义了一个无参数无返回值的block类型

//
NewType block1 = ^void(){
  NSLog(@"我是block1") ;
};
//
NewType block2 = ^void(){
  NSLog(@"我是block2") ;
};
```

> 访问外部变量

```objective-c
int n = 100 ;
__block int m = 100 ;
NewType block1 = ^void(){
	n+=1; // error block中不可以修改外部局部变量的值
	m+=1 ; //如果希望局部变量可以在block内部修改，则需要为变量添加 __block 修饰
  NSLog(@"我是 %d",n) ;
};
```

> Block作为函数参数

```objective-c
typedef void (^NewType)()  ;// 代表重新定义了一个无参数无返回值的block类型

void test(NewType block)
{
   block() ;
}

test(^{
  NSLog(@"哈哈哈哈") ;
})
```

> Block作为函数的返回值

```objective-c

```

