---
date: 2025-03-04
author: Fang
---
# 前言
- 本门课程是基于C语言实现的，讲解过程中也会插入一些C++，对于编程基础薄弱的同学来说会有些吃力。
- 在这里，我们会对C语言和C++常用的语法和概念进行一些补充，希望对你有用
# 前置递增和后置递增
## 前置递增
- 先自增，再使用值
```C
#include <stdio.h>
int main() {
    int a = 5;
    int b = ++a; // 先执行 a = a + 1，然后 b = a

    printf("a = %d, b = %d\n", a, b); // 输出：a = 6, b = 6
    return 0;
}
```
## 后置递增
- 先使用值，再自增
```C
#include <stdio.h>
int main() {
    int a = 5;
    int b = a++; // 先使用 a 的值赋给 b，然后 a 自增

    printf("a = %d, b = %d\n", a, b); // 输出：a = 6, b = 5
    return 0;
}
```
# 指针
- 指针是C语言的灵魂，它在C语言中用于访问和修改变量的值，甚至可以动态分配内存，理解指针式学习C语言和数据结构至关重要的一步
## 什么是指针？
- 指针是一个变量，它的值是另一个变量的内存地址，指针存储的地址可以指向任何类型的数据，指针同指向的数据类型相同
- 在C语言中，指针的声明方式如下：
	`type *ptr_name;` 
	其中`type`是指针指向数据类型；`*`表示这个变量是一个指针；`ptr_name`为指针的名字
## 指针的基本操作
1. 获取地址：使用`&`运算符
	```C
	int a = 10;
	int *p = &a; //将a变量的内存地址保存在p上
	```
2. 解引用（获取值，值更改）：使用`*`运算符
```C
int arr[3] = {1,2,3};
int *p = arr; //将arr的地址赋值给指针p
printf("%d\n",*p); //输出1
```
在数组中，指针指向数组的首元素

3. 指针运算
指针支持算术运算，这些指针会给予指针类型的大小来进行；比如`p+1`会让指针`p`指向下一个类型元素的位置，如果是`int`类型，指针会自动后移四个字节
```C
int arr[3] = {1,2,3};
int *p = arr;
p++;
printf("%d\n",*p);//输出2
```

## 指针的高级操作
- 双重指针：指向指针的指针
	双重指针，用于修改指向某个变量的指针，修改它指向的地址
```C
int a = 10;
int *p = &a;
int **pp = &p;
printf("%d\n",**pp); //输出10,**pp解引用pp,再解引用p,最终得到a的值
```

## 指针常见错误
- 野指针：指针指向无效的地址，通常在指针未初始化或者释放内存后发生
- 空指针：指针没有指向任何有效内存区域，通常指向NULL
- 内存泄露：动态分配的内存没有正确释放，导致内存无法在被利用
# 结构体
- 在面对一些特定问题时，如处理学生信息，现有的数据类型处理较为低效，为了提高代码效率，我们可以自己设计抽象数据类型，也就是结构体
## 结构体的定义
- 结构体用于定义多个不同的数据类型的自定义数据类型，它允许将多个不同类型的变量组合在一起，形成一个整体
- 在C语言中，结构体的定义如下：
```C
struct struct_name{
	type member1;
	type mamber2;
	.....
};
```
## typedef
- 关键字`typedef`常常和结构体一起使用，它是C语言中的类型定义关键字，用于为已有类型创建新的别名，提高代码的可读性和可维护性
- typedef的语法格式`typedef 旧类型 新类型名;`
## typedef与结构体
- 在定义结构体时，我们可以使用`typedef` 来简化结构体的声明，省略`struct`关键字
- 不使用typedef：
```C
struct Student {
	char name[50];
	int age;
};
int main() {
	struct Student stu1; // 必须写 struct Student
	struct Student *prt = &stu1;
}
```
- 使用typedef：
```C
typedef struct Student{
	char name[50];
	int age;
} Student,*StuPtr;  
// Student是struct Student的别名,StuPtr是指向结构体指针的别名

int main() {
	Student stu;  // 省略 struct，代码更简洁
	StuPtr prt = &stu; //不用使用*,减少错误，更便捷
}
```


# 成员访问运算符
- 以后的学习，我们会经常对结构体和变量进行访问和修改，这里就会用到成员访问运算符；
- 成员访问运算符有`.`和`->`两个，下面来分别介绍
## `.`成员访问运算符
- 用于 **直接** 访问结构体变量的成员
- 语法：`结构体变量.结构体内成员变量`
```C
#include <stdio.h>
struct Person {
    char name[20];
    int age;
};
int main() {
    struct Person p1 = {"Alice", 25};
    
    // 直接使用 . 访问成员
    printf("Name: %s\n", p1.name);
    printf("Age: %d\n", p1.age);  
    return 0;
}
```
## `->`成员访问运算符
- 用**指针** 访问结构体成员时，我们使用`->`
- 语法：结构体指针`->`结构体内成员变量
- `ptr->成员` 等价于 `(*ptr).成员`
```C
#include <stdio.h>
struct Person {
    char name[20];
    int age;
};
int main() {
    struct Person p1 = {"Bob", 30};
    struct Person *ptr = &p1;  // 定义结构体指针
    // 使用 -> 访问成员
    printf("Name: %s\n", ptr->name);
    printf("Age: %d\n", ptr->age);
    return 0;
}
```

# C语言的内存动态分配
- C语言中，内存的动态分配是指再程序运行时申请内存，而不是在编译时就确认大小
- 在语言中，我们有`malloc`,`calloc`,`realloc`,`free`函数进行内存的动态分配和释放
## 动态分配函数

1. **malloc**：分配指定字节数，未初始化（内存中原本存储的数据不进行清洗）
```C
void* malloc(size_t size); //返回分配的内存，失败了就返回NULL
int *ptr = (int*) malloc(5*sizeof(int)); //分配了5个int大小的空间,将其强制转换成int的指针类型,并使指针ptr存储该空间的地址 
```
2. **calloc**：分配指定字节数，并初始化为0
```C
void* calloc(size_t num,size_t size); //分配num*size字节的内存，并初始化为零 
int *ptr = (int*) calloc(5,sizeof(int)); //分配5个int大小的空间，并初始化为零
```
3. **realloc**：调整已经分配的内存大小
```C
void* realloc(void* ptr,size_t new_size);
int *ptr = (int*) malloc(5 * sizeof(int));
ptr = (int*) realloc(ptr,10 * sizeof(int)); //将原来的空间扩展到10个int大小
```

4. **free**：释放动态分配的内存
```C
void free(void* ptr); //将ptr指向的内存释放，避免内存泄露
```
注意：
- `free(ptr)`只能将内存与指针解除绑定，归还到内存池中；但不会将`ptr`置为`NULL`，指针`ptr`仍指向这片内存空间，需要手动`ptr = NULL`，避免后续访问指针`ptr`时发生野指针
- 释放`NULL`指针是安全的，不会产生错误
## 使用案例
```C
//对线性表的定义
typedef struct {
    ElemType *data;  // 指向存储元素的数组的指针
    int length;       // 当前表中元素个数
    int MaxSize;      // 表的最大容量
} Sqlist;
//声明一个线性表
Sqlist L;
//给线性表分配内存
L.data = (ElemType*)malloc(sizeof(ElemType)*MaxSize);
```
这段代码，为顺序表L动态分配了一块连续的内存空间，用于存储元素
- Sqlist 是一个声明的结构体类型
- malloc函数申请堆空间
- 计算内存大小
	`sizeof(ElemType) * MaxSize` 表示需要分配的字节总数；
	`sizeof(ElemType)` 获取单个元素占用的字节数
	`MaxSize`获得最大元素个数
	总大小 = 单个元素大小 × 元素个数
- 强制类型转换
	`(ElemType*)`将malloc返回的`void*` 转换成`(ElemType*)`类型，以便匹配`L.data`的指针类型
- 赋值给`L.data`
	将内存地址赋值给`L.data` ,此时`L.data`指向这块内存的起始位置。可以像数组一样使用

# C语言的参数传递
- C语言的函数传递主要有值传递和指针传递（间接传递）两种方式
## 值传递
- 实际上是将原变量的值复制一份传递给形参，并**不会影响原变量**
```C
#include <stdio.h>
void increment(int x) {
    x += 10; // 修改的是x的副本，不影响外部的a
    printf("Inside function: x = %d\n", x);
}
int main() {
    int a = 5;
    printf("Before function call: a = %d\n", a);
    increment(a);
    printf("After function call: a = %d\n", a);
    return 0;
}
```
## 指针传递
- 传递的是变量的地址，函数内部可以直**接修改原变量**
- 适用于修改原数据，返回多个值，**传递大数据结构**
```C
#include <stdio.h>

void modifyValue(int *x) {  // x 存储 a 的地址
	*x = 20;  // 通过指针修改原值
	printf("Inside function: x = %d\n", *x);
}

int main() {
	int a = 10;
	printf("Before function call: a = %d\n", a);
	modifyValue(&a);  // 传递 a 的地址
	printf("After function call: a = %d\n", a);
	return 0;
}
```

## 数组参数传递
- **数组名本质上是指向数组第一个元素的指针**，所以数组总是以指针方式传递
```C
#include <stdio.h>
void modifyArray(int arr[], int size) {  // 等价于 int *arr
	for (int i = 0; i < size; i++) {
	    arr[i] += 10;  // 修改原数组
	}
}

int main() {
	int nums[] = {1, 2, 3, 4, 5};
	modifyArray(nums, 5);  // 传递数组（传递的是首地址）
	for (int i = 0; i < 5; i++) {
	    printf("%d ", nums[i]);  // 输出: 11 12 13 14 15
	}
	return 0;
}
```
- `arr`只是`nums`的首地址，`modifyArray`内部修改的是`nums`的内容
- 数组的大小不会自动传递，所以需要手动传递`size`

## 结构体参数传递
- 结构体可以使用值传递或者指针传递
- 值传递：传递整个结构体，只是**对数据拷贝，不改变原数据**，且性能较为低下
```C
#include <stdio.h>
typedef struct {
	int id;
	char name[20];
} Student;
	
void modifyStudent(Student s) {  // 传递结构体（数据拷贝）
	s.id = 100;  // 仅修改副本，不影响原变量
}
	
int main() {
	Student stu = {1, "Alice"};
	modifyStudent(stu);
	printf("ID = %d\n", stu.id);  // 输出: ID = 1
	return 0;
}
```

- 传递原结构体地址，避免拷贝，适用于大结构体
```C
#include <stdio.h>
typedef struct {
	int id;
	char name[20];
} Student;
	
void modifyStudent(Student *s) {  // 传递地址
	s->id = 100;  // 直接修改原变量
}
	
int main() {
	Student stu = {1, "Alice"};
	modifyStudent(&stu); //传递stu的地址
	printf("ID = %d\n", stu.id);  // 输出: ID = 100
	return 0;
}
```

## 函数指针作为参数
- 函数指针可以作为参数，实现**回调函数**
```C
#include <stdio.h>
void sayHello() {
	printf("Hello, World!\n");
}
	
void executeFunction(void (*func)()) {  // 接受函数指针作为参数
	func();  // 调用函数
}
	
int main() {
	executeFunction(sayHello);  // 传递函数地址
	return 0;
}
```
- `executeFunction(sayHello)`传递`sayHello`的地址，在函数里实现回调

## 总结

| 传递方式    | 特点          | 是否修改原值 | 使用场景              |
| ------- | ----------- | ------ | ----------------- |
| 值传递     | 传递变量的拷贝     | ❌否     | 传递简单数据类型          |
| 指针传递    | 传递变量地址      | ✅是     | 需要修改变量，返回多个值，大型数据 |
| 数组传递    | 传递数组第一个元素地址 | ✅是     | 传递数组，修改原数组        |
| 结构体传递   | 传递整个结构体的拷贝  | ❌否     | 小结构体，不修改内容        |
| 结构体指针传递 | 传递结构体地址     | ✅是     | 传递大结构体，修改内容       |
| 函数指针传递  | 传递函数地址      | ✅是     | 实现回调函数            |



# C++的内存动态分配
- 在C++中，内存的动态分配主要依赖于`new`和`delete`运算符
- 与C语言的`malloc`和`free`不同，C++提供了更安全，面向对象的方式来管理动态内存
## 动态分配方法的使用
- 其用法和C语言大同小异，都是基于指针实现的
### 分配和释放单个变量
```cpp
#include <iostream>
using namespace std;

int main() {
    int *p = new int;   // 在堆上分配一个 int 变量
    *p = 10;            // 赋值
    cout << "Value: " << *p << endl;

    delete p;  // 释放内存
    return 0;
}
```

### 分配和释放数组
```cpp
#include <iostream>
using namespace std;

int main() {
    int *arr = new int[5];  // 在堆上分配一个包含 5 个整数的数组
    for (int i = 0; i < 5; i++) {
        arr[i] = i * 10;
    }
    for (int i = 0; i < 5; i++) {
        cout << arr[i] << " ";  // 输出：0 10 20 30 40
    }

    delete[] arr;  // 释放数组,必须加[]，否则可能只释放第一个元素，导致内存泄漏
    return 0;
}
```



# C++的引用
- 引用类型是C++特有的数据类型，C++中经常使用引用类型作为参数
- 什么是引用？它可以给变量起一个别名，比指针更安全，更易用
```cpp
#include<iostream>
using namespace std;
void main(){
	int i = 5;
	int &j = i;
	i = 7;
	cout<<"i" = <<i<<"j" = <<j<<endl;
}
```
输出结果：
```cpp
i = 7 j = 7
```


使用引用对两个值进行调换：
```cpp
#inclide<iostream>
void swap(float &m,float &n){
	float temp = m;
	m = n;
	n = temp;
}
int main(){
	float a,b;
	cin>>a>>b;
	swap(a,b);
	cout<<a<<endl<<b<<endl;
	return 0;
}
```

- 对引用参数的说明：
	1. 传递引用与传递指针的效果是一样的，形参变化实参也发生变化
	2. 引用类型作为形参，在内存中并没有产生实参的副本，它**对实参直接操作**；而对于一般变量做参数，形参变量实际上就是实参的副本
	3. 当数据量较大时，引用参数的时间和空间效率都比较好
	4. C语言中，指针参数虽然也能达到引用的效果，但在重复使用指针变量名进行运算时，容易产生错误。另外一方面，在主函数调用点处，必须使用变量的地址作为实参
```C
void modify(int *ptr) {
	*ptr = 100;
}
int main() {
	int a = 5;
	modify(&a); // 必须传递变量的地址,否则会产生错误
	printf("a = %d\n", a); // 输出 100
	return 0;
}
```

# 补充：操作算法中用到的预定义常量和类型
在以后的笔记中，你会看到下面的代码块：
```C
Status func() {
    if (some_condition) {
        return OK;
    } else {
        return ERROR;
    }
}
```
这里的Status类型函数是干什么的呢？

其实，完整代码是这样的：
```C
#define TRUE 1
#define FALSE 0
#define OK 1
#define ERROR 0
#define INFEASIBLE -1
#define OVERFLOW -2
//这些是函数结果状态码,在这里我们提前定义了一些异常情况的返回值
typedef int Status; //这行代码定义了一个新的类型Status,它是int的别名
Status func() {
    if (some_condition) {
        return OK;
    } else {
        return ERROR;
    }
}
```
- `Status` 类型函数用于表示函数的执行状态，其值通常是上述预定义常量之一；
- 通过定义 `Status` 类型，返回特定异常情况对应的值，使代码的可读性更高，也更容易维护
- 在以后的代码中，为了便利，我们通常省去前面的代码，只写出Status函数