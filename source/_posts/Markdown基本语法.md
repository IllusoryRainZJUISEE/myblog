---
title: Markdown基本语法
date: 2026-02-26
img: /images/鸿雪ep.png
categories: Others
tags:
  - 全部
  - 实验
---
# 一级标题

## 二级标题

### 三级标题

#### 四级标题

##### 五级标题

###### 六级标题

> 这是一段引用 

有序列表：

把大象放进冰箱：
1.打开冰箱
2.把大象塞进去
3.观赏冰箱

无序列表：
- 阿斯蒂芬
- 寒风呼呼
- 埃利奥特
- joker

代码块：
```c
7. 指针相关的3个概念: 野指针、空指针、通用指针
(1)野指针
char a='A', b, *p;
*p = a; // 此处的p就是野指针(wild pointer)
b = *p;
正确的写法应该是:
p = &a; // 引用*p前先对p本身赋值可以防止野指针
b = *p
或
p = &b;
*p = a;

putchar(b);

(2) 空指针(NULL pointer)
当指针变量p=0时，称p是空指针，故0地址就是空指针
C语言规定，0地址对应内存单元中不能存放任何对象(变量、数组、函数)
#include <stdio.h>
int *f(int a[], int n, int x)
{
   int i;
   for(i=0; i<n; i++)
   {
      if(a[i] == x)
         return &a[i];
   }
   return NULL; // 或 return 0;
}

main()
{
   int x[5]={11,22,33,44,55}, d=66, *p;
   p = f(x, 5, d);
   if(p != NULL)
      printf("%d is found at %p", d, p);
   else
      printf("%d is NOT found", d);
}

(3) 通用指针(general pointer)
①void用作函数值的类型
void f(int x) // 这里的void表示函数f()没有返回值
{
   printf("%d", x);
   // return -x; // 语法错误, 因为返回值类型为void的函数
                 // 禁止用return返回一个值
   return; // 正确, 不过这个return可以省略不写
}
main()
{  int y;
   f(5);
   //y = f(5); // 语法错误
}
②void用作函数的形参
int f(void) // 这里的void表示函数f()没有参数
{
   return 123;
}
main()
{
   int y;
   y = f(); // 不能写成y=f(void);
}

void f(void)
{
   puts("Hello");
}
main()
{
   f();
}

③void *用来定义一个通用指针
void a; // 语法错误
void b[10]; // 语法错误
void表示无类型的意思

void *p; // p是通用指针变量, 在dev-c++中, sizeof(p)=4

short int a=0x1234;
char *p;
void *v;
p = &a; // 语法错误
p = (char *)&a; // 正确

v = &a; // 正确, v起到了万能容器的作用
p = v;  // 正确, v起到了万能赋值者的作用
printf("%hhx", *p); // 34
任何时候都不能引用*v
不管p是野指针、空指针、通用指针，我们都不能引用*p

8. 指针作为函数的参数
(1) 参数的传递方法之一: 值传递(call by value)
void f(int p) // 形参p与实参a不是同一个变量
{             // 即使把形参名改成a, 实参与形参仍旧不是同一变量
   printf("&p=%p\n", &p);
   p = -p;
}
main()
{
   int a=123;
   printf("&a=%p\n", &a);
   f(a); // 实参a代表的a的值
   printf("a=%d", a); // a=123
}

(2) 参数的传递方法之一: 地址传递(call by reference)
void f(int *p) // p = &a
{
   *p = -*p;   // a = -a
}
main()
{
   int a=123;
   f(&a); // 实参是地址
   printf("a=%d\n", a); // a=-123
}

void swap(int *p, int *q)
{
   int t;
   t = *p;
   *p = *q;
   *q = t;
}
main()
{
   int a=123, b=456;
   swap(&a, &b);
   printf("a=%d, b=%d\n", a, b); // a=456, b=123
}

int f(int x, int y, int *p)
{
   *p = x - y;
   return x+y;
}
main()
{
   int a=5, b=3, c, d;
   c = f(a, b, &d);
   printf("c=%d, d=%d\n", c, d); // c=8, d=2
}

9. 指针和一维数组的关系
(1) 设a是一个一维数组，则一定有a[i]≡*(a+i)，因为一维数组
    名a可以理解为该数组的首元素的地址，即a≡&a[0]
    char a[4] = "ABC", c;
    c = a[1];   // c = 'B'
    c = *(a+1); // c = 'B'
(2) 设p是一个指针变量，则一定有p[i]≡*(p+i);
    p[0]可看作是p指向的第0个元素即p当前指向的对象,
    p[i]可看作是p指向的第i个元素   
    char a[4] = "ABC", c, d, *p;
    p = &a[1];
    c = p[-1]; // c = 'A'
    c = *(p-1);// c = 'A'
    d = p[1];  // d = 'C'
    d = *(p+1);// d = 'C'
(3) 数组名a与指针变量p的异同
①共性
a[i]≡*(a+i)
p[i]≡*(p+i)
*(a+i)≡a[i]
*(p+i)≡p[i]
②差别
(a) p可以被赋值但a不能被赋值
(b) sizeof(a) ≠ sizeof(p); 
例如: char a[10] = "ABC", *p; // 10 ≠ 4, 
sizeof(a)中的a必须理解为数组a, 不能理解成&a[0]
(c) &a ≠ &p
&a中的a必须理解成数组a，不能理解成&a[0]

(4) 一维数组做函数实参的本质是以首元素的地址做参数
void f(char s[]) //编译器会把char s[]转换成char *s
{//等价于char *s //s=&a[0]
   int i=0;
   while(s[i] != '\0') // s[i]等价于 *(s+i)
   {
      if(s[i] >= 'a' && s[i] <= 'z')
         s[i] -= 'a' - 'A';
      i++;
   }
}
main()
{
   char a[10] = "AxByCz";
   f(a); // 等价于 f(&a[0]);
   puts(a); // AXBYCZ
}

(5) char *指针与字符串的关系
void my_puts(char *p) // 或写成char p[]
{
   while(*p != '\0')
   {
      putchar(*p);
      p++;
   }
   putchar('\n'); // 或printf("\n");
}
int x=123;
char a[4] = "ABC";
char *p = "xyz"; // 等价于char *p = &"xyz"[0];
   先找到一块空的内存来保存"xyz"
   +1000 'x'
   +1001 'y'
   +1002 'z'
   +1003 '\0'
   p = 1000
puts("Hello"); // Hello及回车, 等价于puts(&"Hello"[0]);
puts(a); // ABC及回车, 等价于puts(&a[0]);
puts(p); // xyz及回车

10. 指针与二维数组的关系
二维数组名可以理解成该数组首行的地址，即a≡&a[0]
```
表格:
|表头1|表头2|表头3|
|-|-|-|
|内容1|内容2|内容3|
|内容4|内容5|内容6|

脚注:
一键三连[^三连]

[^三连]:点赞、关注、投币

分隔线:

---
***
___
