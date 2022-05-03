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

以上图片来自于：https://zhuanlan.zhihu.com/p/352104978
