# C++学习笔记

## Pazzle

- p155文件尾条件
- p246
- p289~296 8.5.5
- p322 new程序

## Chapter4 复合类型

**//p105--**

```c++
int* pt = new int;
short* ps = new short [500];
delete [] pt; //effect is undefined, donnot do it
delete ps;  //same as above
```

- 不要使用`delete`來释放不是`new`分配的内存
- 不要使用`delete`释放同一个内存块两次
- 如果使用`new[]`为数组分配内存，则应使用`delete[]`來释放
- 如果使用`new[]`为一个实体分配内存，则应使用`delete`來释放
- 对空指针应用`delete`是安全的

**//p110--pointer review**

**//p113**

给`cout`提供一个指针，将打印地址；指针类型为`char*`,将打印指向的字符串；若要显示地址，需强制转换`（int*）ps`

数组名是一个指针

**//p115**

```c++
struct inflatable   // structure definition
{//subvar name  };
inflatable *ps = new inflatable;
cin.get(ps->name,20);
cin>>(*ps).name; //two methord for struc member access

//p120--vector and array
std::vector<typename> varname(n_elem); //n_elem -- variables
array<typename,n_elem> varname; //n_elem --not be variables
```

**//p179**
`cctype`字符函数

**// p185**
`switch（case）` ， `case`必须为整数或`char`，无法处理浮点测试；

**//p188**
逻辑运算符中`（while、if..） cin>>xx`被转换为bool类型

**//p204**
函数返回值仅不能为 数组 ！！
但可返回地址。

## Chapter7 编程模块

**//p213**

- 大多情况下，c++将数组视为指针。例外：
- 数组声明使用数组名來标记存储位置；
- 对数组名使用`sizeof`将得到整个数组的长度；
- `&`用于数组名时，返回整个数组的地址。

**//p214--**
函数传递数组实际给的是数组地址，需将数组长度作为独立参数传递，且可被参数欺骗（指定地址）

函数原型，数组：

```c++
void show_array(const double ar[],int n);
//const限定函数内只读，数组最大数需单独声明
//或使用：
void show_array(const double* begin,double* end);
//两个指针确定数组区间
```

**//p221--指针与const**

- 常规变量->常规指针     true；
- 常规变量->const指针   true；
- const变量->常规指针   false；
- cosnt变量->const指针  true；

```c++
const int* ps = &sloth; //不可修改指针指向的值，但可赋新地址给指针
int* const ps = &sloth;//可修改指针指向的值，但不可赋新地址给指针
const int* const ps = &sloth;//都不可修改
```

**//p224--二维数组与函数**

```c++
int data[x][y] = {};
int sum(int ar[][y], int size);
//或
int sum(int (*ar)[y], int size);
//数据引用方法(两次解除)：
ar[x][y] == *(*(ar+x)+y);
```

**//p229--函数与结构**
函数与结构，把结构名当作标准类型名看；
`cin`用于循环的测试表达式，失败返回`false`，但输入值留在输入队列中，用`cin.clear()`重置；
//使用指针节省内存并修改原始对象：

```c++
struct polar
{
  double dis;
  double ang;
};
polar pplace;
show_polar(&pplace);
void show_polar(polar* pda)
{
  cout<<pda->dis;
}
```

**//p243--函数指针**

```c++
typename1 function_name(typename2);   //函数原型
typename1 (*pointer_name)(typename2); //指针声明
pointer_name = function_name;         //地址赋予
typename1 var = pointer_name(var_func); //调用 or (*pointer_name)(var_func);

*pd[3]   // an array of 3 pointers
(*pd)[3] // a pointer to an array of 3 elements
```

## Chapter8 函数探幽

**//p254--内联函数**
在函数声明和定义前加   按值传递，优于C的宏；不能递归；

**//p257--引用**

```c++
int & rodent = rat; // 声明时初始化
```

**//p277--函数重载**
匹配函数时，不区分const/非const；
不同特征标（如`&`）不共存；
强制转换（如`int->double`）若有多个匹配函数，视为错误；

**//p281--函数模板**

1.优先级：非模板函数->显示具体化模板->常规模板；

```c++
template <> void Swap<int>(job &j1, job &j2);//显示具体化模板，注意打头和类型
```



## Chapter9 内存模型和名称空间

**//p302**
头文件引用<>默认位置搜寻，""当前位置搜寻；
同一文件只能包含同一头文件一次；用以下方式避免多次包含：

```c++
#ifndef para
#define para
#endif
```

局部代码块变量名与外部代码块变量被暂时隐藏；离开时，外变量再次可见；

栈LIFO（后进先出），自动变量失效后，栈顶指针指回以前位置，新值没被删除，但不被标记；

**//p310--变量存储**
静态变量，即使代码块不在运行中，变量及其值仍保留，且除启动时，不会再被初始化；
自动；寄存；静态（在代码块中，无链static）；

- 静态（不在任何函数中，外部链接）；
- 静态（不在任何函数中，内部链接static）；
- 静态（外部链接）在其他文件中使用需：

```c++
extern int para;
```

作用域解析运算符`::para`，表示变量的全局版本；

`const` 全局变量为内部链接性；添加 `extern const` 覆盖默认内部链接性；

静态函数：

```c++
static function_name; static function_name 
// 原型定义都需要加，只在函数内使用，外部文件可定义同名函数；
```

new 的初始化

```c++
int* pin = new int[4] {1,2,3,5};
```

**//p330 名称空间**
名称空间可嵌套：

```c++
using namespace space1::space2;
```

别名：

```c++
namespace short_name = full_name:sub::namespace;//嵌套简化
```

未命名空间，等效内部静态变量；
老式头文件（.h）没有使用名称空间；新式使用了；

## Chapter10 对象和类

**//p348**
类声明中的函数自动成为内联函数；或使用：

```c++
class Stock{... void func()};
inline void Stock::func(){}
```

类的一个初衷：数据隐藏

**//p354构造函数**

```c++
Stock food = Stock("string", , ); //显示调用
Stock food("string", , );//隐式
Stock food; // 隐式调用默认，没有圆括号
Stock * pointer = new food();//
Stock::Stock(){  }//默认构造函数
```

作用域为类的常量；

```c++
private:
  enum {Months = 12}; //枚举
  static const int Months = 12;//与其他静态变量存储在一起，而非存储在对象中；
                               //无论创建多少对象，只创建一个副本；共享；
  static classname::Months = 12;//可在类外初始化静态变量；初始化应在方法文件中，避免重复初始化；
//枚举避免冲突：
enum class size{small, medium, large};//可指定类型 enum class : short size...
int egg = int(size::small); // 需显式转换，隐式非法
```

## Chapter11 使用类

**//p385 运算符重载**

```c++
Time Time::operator+(const Time & t) const//t为引用，不要返回指向临时变量的地址
    {}
```

下述均为Time对象；

```c++
total = coding.operator+(fixing);
//等效于
total = coding + fixing;

t4 = t1 + t2 +t3 + t4;
//等效于
t4 = t1.operator+(t2+t3);
```

**//p387 重载限制**

**//p387 重载限制**

```c++
friend Time operator*();//原型前加friend，定义不需要加；
```

{成员函数通过this指针传递操作数：

```c++
return *this；
//等效于 
return this->total_val;
```

非成员函数应为友元函数，显示传递；}

**//p406**

```c++
namespace xx
{ class zz
  {
   private:
   public:
   //friend//友元函数不在类作用域内，需使用限定名zz::val
  };
}

rand()；//返回0～某个值，为伪随机，本次随机值用作下次随机的种子；
srand(time(0));//time返回当前时间，更随机；

operator typename();//转换函数，1.必须是类方法，2.不能指定返回类型，3.不能有参数，4.但必须返回转换后的值；
//前加explicit，显示调用，最好使用显示调用，避免自动转换不需要的转换；
```

**//p420**
符号运算定义为友元，可更容易适应自动类型转换；

## Chapter12 类和动态内存分配

```c++
TableTennisPlayer::TableTennisPlayer (const string & fn,
    const string & ln, bool ht) : firstname(fn),
	    lastname(ln), hasTable(ht) {}// 初始化，减少一步；
```



## Chapter13 类继承

```c++
derived_class::derived_class(typename r, const string & fn,
     const string & ln, bool ht) : base_class(fn, ln, ht)
{rating = r;}//显式传递参数，否则将使用默认基类；

derived_class derived();
base_class base(derived);
//基类初始化为派生类对象；因为存在隐式复制构造函数：
base_class(const base_class &);
或：
base_class_name = derived_class_name;//隐式重载赋值运算符；

virtual ~base_class_name(){}//虚方法，为基类声明一个虚析构函数是惯例;
                            //避免使用指针数组时，只调用基类虚构函数；
```

派生类方法中，需使用作用域解析运算符來调用基类方法（有虚函数的）；//否则为派生类方法；

创建指向`base_class_name`的指针数组（需基类指针），则指针既可指向基类也可指向派生类；->多态性

类的调用（作为参数），按值传递只将派生类的基类部分传递，指针与引用则隐式向上强制转换；

{重新定义继承，应确保与原来的原型完全相同，除非返回类型是基类引用或指针，则派生类做相应修改；
 如果基类声明被重载了，则应在派生类中重新定义 所有 的基类版本；}

*****
 **类设计回顾p523**
*****

派生类使用基类友元函数，通过强制类型转换将派生类引用或指针转换为基类引用或指针；   
