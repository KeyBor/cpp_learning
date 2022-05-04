以下图片来自于：https://zhuanlan.zhihu.com/p/352104978
### 转换函数 conversion function
``` c++
//non-explicit-one-argument ctor
class Fraction
{
public:
	//non-explicit-one-argument ctor
	//one-argument:只要一个实参就够了，给两个也可以。two-parameter
	//non-explicit:没有添加explicit修饰
	//可以把int隐式的转换为Fraction
	Fraction(int num, int den = 1)//这种默认是符合数学上的规定
		:m_numerator(num), m_denominator(den) {}
	//转出去，把Fraction转换为double
    operator double() const {
	    return (double)m_numerator / (double)m_denominator;
	}
	Fraction operator+(const Fraction& f) {
		return Fraction(1,2);
	}
 
private:
	int m_numerator;	//分子
	int m_denominator;	//分母
};
 
Fraction f(3, 5);
Fraction d = f + 4;
```

当转换函数和non-explicit的单参构造函数并存时，由于其先找+号的操作符重载，发现可以直接将4转为fraction，但是f又可以直接通过转换函数转为double造成了歧义，所以编译器会报错，所以此时需要将non-explicit的单参构造函数，通过explicit表明，让编译器不会在类型转换时主动调用单参构造函数

### pointer-like classes
![img](https://github.com/KeyBor/cpp_learning/blob/master/pic/Pasted%20image%2020220503194245.png)
将类设计成类似于指针的形式，为什么呢，因为既希望拥有指针的特性又想完成更多的事情。
这样子的类需要完成所有指针的行为，操作符的重载包括解引用和箭头操作符。
在上述sp->method()调用时，其实，通过箭头操作符重载之后会先返回px指针，按理来说，箭头操作符将失效，但是cpp语法规定该情况下箭头操作符可以重复使用

### 迭代器其实也是一种pointer-like class

### function-like class 仿函数
顾名思义，对括号操作符进行重载，模仿函数的行为，完成类似函数的行为

### 模板主要分为三类：类模板，函数模板和成员模板
![img](https://github.com/KeyBor/cpp_learning/blob/master/pic/Pasted%20image%2020220503195154.png)
成员模板是模板的一部分，但是自己也是模板，虽然我还不知道具体的用途
其余的模板都比较简单，可能主要为了完成面向对象中up-cast的特性吧，即父类对象可以接受子类对象进行初始化。

### 模板特化和模板偏特化
![img](https://github.com/KeyBor/cpp_learning/blob/master/pic/Pasted%20image%2020220503195250.png)
模板特化后，注意是用特定类型绑定，所以尖括号内不需要进行泛化的设定
![img](https://github.com/KeyBor/cpp_learning/blob/master/pic/Pasted%20image%2020220503195357.png)
![img](https://github.com/KeyBor/cpp_learning/blob/master/pic/Pasted%20image%2020220503195426.png)

### c++11新特性
#### variadic templates 
![img](https://github.com/KeyBor/cpp_learning/blob/master/pic/Pasted%20image%2020220503195511.png)
递归循环调用解包的过程，需要注意，最后包长度为0时，需要调用无参函数

### Reference
引用其实就是对内存地址取别名，但是这个取别名在底层的实现还是使用指针实现的，在对引用进行取地址和计算大小时，都与其引用的原来对象一样，这都是假象，是cpp在底层实现的。在底层，引用还是占据了另外的内存地址，并非狭隘的认为是在其引用对象的地址上起别名，最终由指针来实现，引用对象即内存地址别名的效果。
另外，定义函数形参时，引用传递和值传递，由于在传入实参是编译器无法区分，所以，不能定义两个同名函数，形参类型为值和引用。
此外，在函数名后添加const的函数签名是不一样的。

### 复合和继承关系下的构造和析构
只要记住一个原则，那么就是构造的时候由内而外，我想象成先把内部零件搭好，才能拼装成整体，析构函数就是由外到内，你需要先将外部拆解开，才能把内部的零件再拆解掉。

### 关于虚表和虚指针
虚指针是指向虚表的，虚表内存储所有虚函数的地址，每一个子类会有一个虚表，这个虚表是类共享，但是虚指针是每一个对象有一个虚指针。
虚函数的调用，需要有以下三个条件：<br>
1、由指针调用（直接由对象调用，向上转型会丢失子类的信息）<br>
2、父类指针指向子类对象，即可以向上转型，这样子就通过虚函数表查询获取真正的虚函数的地址，动态绑定完成了子类重写的虚函数方法。<br>
3、为虚函数，不必多说这个了<br>
关于成员函数，会传入一个this指针，也可以用来完成动态绑定.<br>
比如父类的定义的一个非虚函数中调用了虚函数，那么子类继承父类，并调用非虚函数时，会传入子类对象的this指针，那么在运行到非虚函数中虚函数的调用时，就会传入this指针，从而完成动态绑定，运行子类重写的虚函数，输出属于子类的结果。



 
