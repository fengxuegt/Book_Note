# Effective C++

本文用于记录阅读《Effective C++》所得到的收获及知识点。

### 条款1 让自己习惯C++

将C++视为一个由相关语言组成的联邦而非单一语言。其拥有四个次语言，对于某种次语言，各种规则和通例都比较容易理解。

- C语言 
- Object-Oriented C++
- Template C++ 泛型编程部分
- STL template程序库

**总结**

C++高效编程守则视情况变化而变化，取决于你使用C++的哪一部分（次语言）。

### 条款2 尽量以const，enum，inline替换#define

换种说法：尽量用编译器取代预处理器的工作。

由于#define的变量通常在预处理阶段就被移走了，因此当程序报错时你不会找到宏的名字，而是他的值。例如

```cpp
#define RATIO 1.653  (1)
const double ratio = 1.653   (2)
```

第(2)方式

- 会将ratio写入到符号表当中
- 可能会产生较小的代码，因为宏替换的话代码中会产生很多1.653

条款适用情景：

- 定义常量指针，常量定义式通常被放于头文件内，因此有必要将指针声明为const；所以一般在头文件中定义常量如下：

  ```cpp
  const std::string authorName("Linus");
  ```

- class 专属常量。通常static和const变量都是不能在类内进行初始化的，都是类内声明，类外进行初始化。但是当两者合在一起的时候就可以在类内初始化了。例如：

  ```cpp
  class CostEstimate {
      private:
      	static const double FudgeFactor;
      	...
  }; // .h
  
  const double FudgeFactor = 1.35; //.cpp 而且这里也不同再加static关键字了
  ```

  ```cpp
  // 另一种情况，类中需要常量值，但是编译器又不让类内初始化；
  class GamePlayer {
    	private:
      	emun {Num = 5};
      	int scores[Num];
  };
  // 这种方式称为enum hack；主要是为了解决上述问题，很多代码都是这样用的
  ```

- 宏函数一般要使用inline来代替；

  ```cpp
  #define MY_MAX(a,b) f((a) > (b) ? (a) : (b))
  // 括号已经加很多了，但是当如下调用时仍然会出现问题
  MY_MAx(++a, b);
  MY_MAX(++a, b+10);
  // 是因为当三目运算符走不同的分支时，会出现二义性
  // replace version
  template <class T>
  inline void My_Max(const T &a, const T &b)
  {
      f(a > b ? a : b);    // ????????这里的f没看懂
  }
  ```

**总结**

- 对于单纯的常量，最好以const对象或者enum代替#define

- 对于形似函数的宏，最好用inline来代替





### 条款3 尽可能使用const



### 条款4 



### 条款5 了解C++默默编写了并调用了哪些函数

- 拷贝构造函数
- 拷贝赋值函数
- 析构函数
- 构造函数

以上函数就是编译器会自动为你声明的，并且是public和inline的。只有当这些函数具有调用需求的时候，编译器才会帮你去实现他们。

- 对于拷贝构造函数，要考虑类内成员是否具有深拷贝的需求，如果有的话就要自己编写
- 对于copy structor，如果类内有引用成员或者const成员，也需要自己定义拷贝构造函数
- 对于析构函数，如果类有多态需求，要声明为virtual

除了这些特殊的场景之外，如果不是极其简单的类型，请自己编写构造、拷贝、析构、赋值、移动拷贝和移动构造

### 条款6 如果不想使用编译器自动生成的函数，就应该明确拒绝

这条从C11开始的话在声明函数的时候声明成=delete就可以了



### 条款7 为多态基类声明virtual

核心内容为：带有多态性质的基类必须将析构函数声明为虚函数，防止出现内存泄漏。如果析构函数不是virtual的话，那么当父类引用或者指针指向子类对象时，delete父类指针或引用并不会调用子类的析构函数

需要注意的是：

- 普通的基类无需且不应该有虚析构函数，因为虚函数在时间和空间上都有代价

- 如果一个类型没有被设计成基类，但是有误继承的风险，那么在类中声明为final，可以禁止派生

  ```cpp
  class Person final {
    
  };
  ```

- 编译器自动生成的析构函数是非虚的，所以要多态类要自定义析构函数

### 条款8 别让异常逃离析构函数

析构函数一般情况下不应该抛出异常，因为很大可能出现各种未定义的问题。



### 条款9



### 条款10



### 条款11



### 条款12



### 条款13



### 条款14



### 条款15



### 条款16



### 条款17



### 条款18



### 条款19



### 条款20



### 条款21



### 条款22



### 条款23



### 条款24



### 条款25



### 条款26



### 条款27



条款2