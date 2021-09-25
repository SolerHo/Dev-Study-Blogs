<h1 align="center">📔 Effective C++ 读书笔记</h1>

## 第一章 自己习惯C++
### T1. 视C++为一个语言联邦
四个语言层次的切换：
- C：C++继承了C语言，但是C语言没有面向对象，没有模板（templates）、没有异常（exception）、没有重载（overloading）。
- Object-Oriented C++：C++中类编程（包括构造函数和析构函数）部分，C++的主要特性：封装（encapsulation）、继承（inheritance）、多态（polymorphism）、虚函数（动态绑定）等。
- Template C++：泛型编程。也衍生出了模板原编程（template metaprogramming，TMP）。
- STL：template程序库，对容器（container）、迭代器（iterators）、算法（Algorithms）以及函数对象（function objects）的规约。也可以通过其他的方式构建出和STL一样需求的程序库。

工程间切换语言层次时，遵守该语言的规约会让了解更容易。

**总结**：C++高效编程守则视状况而变化，取决于使用C++哪一部分。

### T2. 尽量用 `const、enum、inline` 来替代 `#define`
另一种解释：`宁可编译器来替换预处理器`。
#### 使用`const`替代`#define`
在 `#define` 定义下的标记是不会经过编译器处理，在编译器开始处理源码之前就被预处理器移走。
**隐患**：编译会难查 `#define` 引入的问题，导致追踪而浪费时间。
**使用const的好处**：编译器会协助检查如类型错误等导致的问题。
const 可以将变量的作用域限制在class内，#define定义的宏在包含了该头文件的文件里都可以使用。
```cpp
class GamePlayer{
private:  
	static const int NumTurns = 5;	//常量声明式 
	int scores[NumTurns]; //使用该常量
	...
};

class CostEstimate{
private:
	static const double FudgeFactor;	//static class 常量声明
	...                                 //位于头文件内
};
const double ConstEstimate::FudgeFactor = 1.35;	// 在实现文件内定义
```

**注意**：
不能用 `#define` 来创建一个class专属常量。**原因**：`#define`不重视作用域，一旦宏被定义，则其后的编译过程中有效（除非某处被underf）。会造成不能提供封装性。

#### 使用`enum`替代`#define`
旧式编译器不支持类的static成员在声明式上获得初值。
但如果在类的编译期间需要一个常量值（例如用于确定数组大小），可使用enum。
取一个 `const` 的地址合法，但取 `enum	 `和`#define`的地址不合法，则 `enum` 可用于保护常量。

`enum` 不会造成不必要的内存申请（避免他人对该常量取地址取指针）

#### 使用`inline`替代`#define`
可以使用 `inline` 函数来定义内联函数，例如：
```cpp
template<typename T>
inline void callwithMax(const T& a,const T& b){
	f(a>b?a:b);
}
```
此时不需要给函数本体中为参数加上括号，也不需要操心参数被核算等。
#### 宏定义带来的麻烦

```cpp
#define CALL_WITH_MAX(a,b) f((a)>(b)?(a):(b))
int a = 5,b = 0;
CALL_WITH_MAX(++a,b) ; //a被累加2次
CALL_WITH_MAX(++a,b+10); //a被累加1次
```

**总结**
- 对于单纯常量，最好以 `const对象` 或 `enums` 来替代 `#defines`。
- 对于形似函数的宏（macros），最好改用`inline函数` 替换 `#defines`。

### T3. 尽可能使用const
#### 3.1 const修饰指针和用法
>当const在星号左侧时，指针指向的为常值
>当const在星号右侧时，指针本身为常值
>当const星号两侧都有时，指针本身及其指向均为常值
>当指针指向常值时，const在类型名前后意思相同
```cpp
char greeting[] = "Hello";
char* p = greeting;				// non-const pointer, non-const data
const char* p = greeting;		// non-const pointer, const data
char* const p = greeting;		// const pointer, non-const data
const char* const p = greeting;	// const pointer, const data
void f1(const Widget *pw);
void f1(Widget const *pw);	// 两个意思相同
```
在一个函数声明式内，const可以和函数返回值、各参数、函数自身（如果是成员函数）产生关联。
#### 3.2 STL迭代器的const
>`const iterator` 相当于 `T* const`
>`const_iterator` 相当于 `const T*`
```cpp
std::vector<int> vec;
...
const std::vector<int>::iterator iter = vec.begin();	//const iterator 相当于T* const
*iter = 10;	// 正确,改变iter的指向
++iter;		//错误。iter是const，不可更改
std::vector<int>::const_iterator cIter = vec.begin();	//const_iterator 相当于const T*
*cIter = 10;// 错误，cIter是const，不可更改
++cIter;	// 正确，改变cIter
```
#### 3.3 const修饰函数返回值
令函数返回一个常量值，会降低因客户错误而造成的意外。但又不至于放弃安全性和高效性。
```cpp
class Rational { ... };
const Rational operator*(const Rational & lhs , const rational rhs);
```
如果不加const来实现，则会难以发现如下类型的错误
```cpp
if((a*b)=c) // === 用成 =
```

#### 3.4 const修饰成员函数
**目的**：
>使class接口更容易被理解。
>让“操作const对象”成为可能。
例子：
```cpp
class TextBlock{
public:
	...
    const char& operator[](std::size_t position) const
    {
	    return text[position];  //返回const对象
	}
    char& operator[](std::size_t position)
    {
	    return text[position]; //返回non-const对象
	}	
private:
    std::string text;
};

// usage
TextBlock tb("Hello");
TextBlock ctb("Hello");

std::cout<<tb[0];	// 正确，读non-const
tb[0]='x';			// 正确，写non-const
std::cout<<ctb[0];	// 正确，读const
ctb[0]='x';	 //错误，const不可更改
```

在成员函数中的const，有两个概念：
- ***bitwise constness（又称physical constness）***：成员函数只有在不更改对象的任何成员变量（static除外）时才可说是const。
	- *const成员函数不可更改对象内任何non-static成员变量（任何一个bits）*。
- ***logical constness***：一个const成员函数可以修改它所处理对象内的某些bits，但只有在客户端检测不出来的情况下才满足const。
	- 使用关键字 `mutable` , mutable会释放掉non-static成员变量的bitwise constness约束。

```cpp
class CTextBlock{
public:
	...
	std::size_t length() const;
private:
	char* pText;
	// muatbel会释放，故可更改成员变量
	mutable std::size_t textLength;
	mutable bool lengthIsValid; 
};// const成员函数内
std::size_t CTextBlock::length() const
{
	if(!lengthIsValid){
		textLength = std::strlen(pText); 
		lengthIsValid = true;
	}
	return textLength;
}
```
#### 3.5 在 `const` 和 `non-const` 中避免重复
尝试使用强制类型转换。
例子：
```cpp
class TextBlock{
public:
    ...
    const char& operator[](std::size_t position) const
    {
         ...   //边界检验
		 ...   //日志记录访问记录
		 ...   //检验数据完整性
         return text[position];
    }
    char& operator[](std::size_t position)
    {
        return const_cast<char&>(static_cast<const TextBlock&>(*this)[position]); //static_cast加const强制安全转换（防止出现递归调用）
    }
    ...
};
```
*`不推荐使用non const函数调用const函数的方法来避免代码重复`*。

### T4. 确定对象被使用前已先被初始化
> - 对于内置类型，永远在使用对象之前先将其初始化。对于无任何成员的内置类型，必须手工完成初始化。
> ```
> int x = 0;	// 手工初始化int
>const char* text = "A C-style string";	// 对指针进行手工初始化
>double d;
>std::cin>>d;	//读取输入流以初始化
> ```

**注意**：~~不能混淆赋值和初始化两个概念~~

> - 对于非内置类型，初始化责任落在构造函数上。确保每一个构造函数都将对象的每一个成员初始化。

例子：表示通讯录的class
```cpp
class PhoneNumber{...};
class ABEntry{ //ABDEntry = "Address Book Entry"
public:
	ABEntry(const std::string& name,
		const std::string& address,
		const std::list<PhoneNumber>& phones);
private:
	std::string theName;
	std::string theAddress;
	std::list<PhoneNumber> thePhones;
	int numTimesConsulted;
};
/*
ABEntry::ABEntry(const std::string& name,
		 const std::string& address,
		 const std::list<PhoneNumber>& phones)
		{
			//均是赋值，非初始化
			theName = name;
			theAddress = address;
			thePhones = phones;
			numTimesConsulted = 0;
		}	// 赋值
*/
ABEntry::ABEntry(const std::string& name,
		 const std::string& address, 
		 const std::list<PhoneNumber>& phones)
		//成员变量初始化
    :theName(name),
    theAddress(address),
    thePhones(phones),
    numTimesConsulted(0)
	{ }	//构造函数本体无需任何操作
```
**赋值 VS 初始化**
- 赋值：首先调用默认构造函数为成员变量赋初值，然后立刻再对它们赋新值
- 使用成员初值列表避免赋值的问题，效率较高

**没有在成员初值列表中指定初值的成员变量**
- 对于用户自定义类型的成员变量，将会自动调用其自身的默认构造函数
- 对于内置类型的成员变量，则可能出现随机结果，带来问题。

#### 4.1 初始化次序问题
static对象：其寿命从被构造出来直到程序结束为止。

**编译单元（translation unit）**：指产出单一目标文件的源码（`单一源码文件`加上其所包含的 `头文件#include`）。

如果一个编译单元的non-local static对象的初始化用到另外一个不同的编译单元中的non-local static对象，则这个被用到的对象可能未被初始化。
>定义于不同编译单元内的 `non-local static对象`的初始化次序并无明确定义。

`non-local对象`：指的是global或位于namespace作用域内，抑或在class内或file作用域内被声明为static。

```cpp
class FileSystem
{
public:
std::size_t numDisks() const;
}
extern FileSystem tfs;
```

另一个文件：
```cpp
class Director{ // 由程序库客户建立
public:
	Directory(params);
    ...
}
Director::Director(params)
{
	...
	std::size_t disks = tfs.numDisks(); //使用tfs对象
	...
}
```
假设客户端决定创建一个Directory对象，用来存放临时文件：
```cpp
Directory tempDir(params);
```
此时初始化次序的重要性显现出来了：除非tfs在tempDir之前先被初始化，否则会用到尚未初始化的tfs。
> 多个编译单元的 non-local static对象经过模板隐式具体化形成。

**解决方案**：
>将每个 non-local static对象 移至专属函数内（对象在该函数内声明为 `static`），函数返回一个引用指向它所含的对象，***用户调用函数时不直接使用对象***，使得 `non-local static对象`被转换为` local static对象`。（类似设计模式中的 `单例模式`）

**基础原理**：
>C++保证函数内的 `local static对象` 会在 “函数被调用期间” 或 "首次遇上该对象的定义式"时被初始化。

```cpp
class FileSystem{...};

FileSystem& tfs()
{
    static FileSystem fs; //定义并初始化 local static对象
    return fs; //返回指向对象的引用
}

class Directory{...};

Directory::Directory(params)
{
    ...
    std::size_t disks = tfs().numDisks();
    ...
}

Directory& tempDir() //函数替换tempDir对象
{
    static Directory td; //定义并初始化local static对象
    return td;
}
```
>在函数内含 static对象，在多线程系统中带有不确定性。
>**解决方法**：在程序的单线程启动阶段手工调用所有返回引用的函数，可消除与初始化有关的竞争。
#### 4.2 初始化必做三件事
避免在对象初始化之前过早地使用。
- 手动初始化 `内置型non-member对象`。
- 使用 `成员初值列表(member initialization lists)`处理成员对象初始化.
- 针对初始化次序不确定性加强设计。

#### 4.3 总结
>- 为内置对象进行手工初始化，C++是不会保证初始化。
>- 构造函数最好使用 `成员初值列（member initialization list）`，而不是在构造函数本体内使用赋值操作。初值列列出的成员变量，其排列次序应该和它在class中的声明次序相同。
>- 为免除 “跨编译单元之初始化次序”问题，推荐使用 `local static 对象`来替换 `non-local static对象`。


## 第二章 构造、析构、赋值
### T5. Know what functions C++ silently writes and calls

如果未声明任何构造函数，编译器会为其声明一个 `default构造函数`。

>一般会自动生成 `copy构造函数`、`copy赋值操作符`、`析构函数`。

***所有的函数都是 public 且 inline***。
例如：
```cpp
class Empty{...}
```
等价于
```cpp
class Empty{
public:
	Empty(){...} //default构造函数
	Empty(const Empty& rhs){...} //copy构造函数
	~Empty(){...} //析构函数
	
	Empty& operator = (const Empty& rhs){...} //copy赋值操作符
};
```
当函数被调用时，三个函数才会被编译器创建。
>编译器产生的析构函数是 `non-virtual`，除非这个`类（class）`的`基类（base class）`自身声明中有 `virtual 析构函数`。

对于编译器生成的 `copy构造函数` 和 `copy 赋值操作符`，只是简单地将源对象中的每一个non-static成员变量拷贝到目标对象中。



#### 5.1 总结
编译器会暗自为 `class` 创建 ***`default构造函数、copy构造函数、copy assignment操作符以及析构函数`***。

### T6. 若不想使用编译器自动生成的函数，则明确拒绝

在T5中介绍，如果不声明 `copy构造函数`和 `copy赋值操作符`函数，则编译器会在被调用时自动生成。

> 为了**解决自动生成的方法**：主动对函数进行声明为private。【做法不是绝对安全】
>
> ```c++
> class HomeForSale{
>     public:
>     	...
>     private:
>     	...
>         HomeForSale(const HomeForSale&);
>     	HomeForSale& operator=(const HomeForSale&);
> };
> ```

**另外一种方法**：通过**设计一个基类来继承**，从而减少代码的重复。

```c++
class Uncopyable{
protected:
    Uncopyable(){}
    ~Uncopyable(){}

private:
    Uncopyable(const Uncopyable&);
    Uncopyable& operator=(const Uncopyable&);
};
```

> 缺点：会出现多重继承。

在boost库中提供 `noncopyable` 函数。

**总结**：为驳回编译器自动（暗自）提供的机能，将相应的 **成员函数声明为 private 并且不予实现**。




