---
layout: post
title: "C++ Basics"
description: "Notes on C++ reference books"
date: 2017-02-17
tags: [programming, c++, notes]
comments: true
share: true
---

This notes also serve as a pratice of formatting, inserting images, links in markdown.

### 《Effective C++》

---
1. const的好处
    - 只要（某值保持不变）是事实，就应该申明是const，因为说出来可以获得编译器的帮助，确保这条约束不被违反。
    - const成员函数可以作用于const对象。
    - 使用const速度更快，例如pass by reference-to-const方式传递对象。
2. 令non-const函数调用其const兄弟是一个避免代码重复的安全做法。 (p.24)

    ``` c++
    class TextBlock {
    public:
        ...
        const char& operator[](std::size_t position) const;
        {
            ...
            return text[position];
         }
         char& operator[](std::size_t position)
         {
             return const_cast<char&>(static_cast<const TextBlock&>(*this)[position]);
         }   
    };
    ```
3. - 内置类型必须手动完成初始化。 (p.27)
   - 使用member initialization list 通常效率更高。因为采用赋值初始化的话，default constructor的作用就浪费了。 (p.28)

4. 如果没有自己申明，编译器就会自己申明a copy constructor, a copy assignment and a destructor. 
编译器产出的析构函数是non-virtual，除非这个class的base class自身申明有virtual 析构函数（这种情况下这个函数的虚属性主要来自base class）。

5. 别让异常逃逸析构函数

    原因：当有两个异常发生，程序即停止。 (p.44)

6. 绝不再构造和析构过程中调用virtual函数。 (p.48)

   Derived class的构造函数被调用时首先调用base class的构造函数，这时候调用virtual function的话调用的是base class里面的virtual function 而非derived class里的。
所以，在base class构造期间，virtual函数不是virtual函数。
在derived class对象的base class构造期间，对象的类型是base class而不是derived class。

7. shared_ptr 提供一个get成员函数，用来执行显式转换，它会返回智能指针内部的原始指针的复件！ (p.70)

8. 以独立语句将newed对象存储于（置入）智能指针内。如果不这样做，一旦异常被抛出，有可能导致难以察觉的资源泄漏。 (p.77; 例子在 p.76)

9. shared_ptr 提供的某个构造函数接受两个实参：一个是被管理的指针，另一个是引用次数变成0时被调用的“删除器“ （deleter）。 (p.82)

10. 如果窥探C++编译器的底层，你会发现，references往往以指针实现出来，因此pass by reference通常意味真正传递的是指针。因此如果你有个对象属于内置类型（例如int），pass by value往往比pass by reference的效率高些。一般而言，你可以合理假设“pass by value并不昂贵”的唯一对象就是内置类型，STL的迭代器（iterator）和函数对象。 (p.90)

11. 只要类型T支持copying（通过copy构造函数和copy assignment操作符完成），缺省的swap实现代码就会帮你置换类型为T的对象，你不需要为此另外再做任何工作了。 (p.106)

12. C++只允许对class templates偏特化，在function templates身上偏特化是行不通的。客户可以全特化std内的templates，但不可以添加新的templates（或classes或functions或其他任何东西）到std里头。

13. 转型动作

    - old-style casts：
      - (T)expression
      - T(expression)
   
    - new-style casts:
      - const_cast<T>(expression)：常量性转移
      - dynamic_cast<T>(expression)：用来决定某对象是否归属继承体系中的某个类型
      - reinterpret_cast<T>(expression)：e.g. 将pointer to int 转换为 int
      - static_cast<T>(expression)：强迫隐式转换，但无法常量性转移

14. 只有在public继承时：接受pointer-to-BASE或reference-to-BASE的地方也可以接受pointer-to-DERIVED或reference-to-DERIVED。 (p.151)

15. C++的名称遮掩规则（name-hiding rules）所做的唯一事情是：遮掩名称。至于名称是否应和相同或不同的类型，并不重要。本例中一个名为`x`的`double`遮掩了一个名为`x`的`int`：(p.156)

    ``` c++
    int x;
    void someFunc() {
        double x;
        std::cout>>x;
    }
    ```

    对于函数，也是一样，无论函数的parameter arguments一样还是不一样，对于成员来说，也无论是virtual还是non-virtual，只要函数名称一样，同样全部遮掩。

    ``` c++
    class Base {
    public:
        virtual void mf1()=0;
        virtual void mf1(int);
    };

    class Derived: public Base {
    public:
        virtual void mf1();
    };
  
    Derived d;
    d.mf1(5);       // wrong; Derived::mf1() hides both Base::mf1() and Base::mf1(int)
    ```

    如果想让原来的base class中的同名成员函数在Derived中可见，可在Derived class中加入

    `Using Base::mf1;`

    而且
  `d.mf1();`
    依旧调用Derived class中的版本。

    ``` c++
    Base b;
    b.mf(5);   //legal!
    ```
    **有时候你并不想继承base classes的所有函数，这在public继承是不可以理解的，但是在private继承之下它却是有意义的！！  (p.160)
	
16. 借由function wrapper来完成Strategy模式  (p.175)

    ![Alt Text](https://scuiaa555.github.io/assets/images/2017-02-17-function.png "a")

    **typical examples can be found here <http://en.cppreference.com/w/cpp/utility/functional/function>.
    
    An example (pp.173-175) shows how great is the combination of using `std::function` and `std::bind` together.
    
    ``` c++
    class GameCharacter {
    public:
        //HeathCalcFunc 可以是任何“可调用物”（callable entity）,可被调用并接受
        //任何兼容于GameCharacter之物，返回任何兼容于int的东西。
        typedef std::function<int(const GameCharacter&)> HealthCalcFunc;
        int healthValue() const {
            return healthFunc(*this);
        }
    private:
        HealthCalcFunc healthFunc;
    }
    
    short calcHealth(const GameCharacter&);  //其返回值为non-int，可调用物的参数可被隐式转换为const GameCharacter&，
                                             //其返回类型可被隐式转换为int。
    
    struct HealthCalculator {                //函数对象同样ok。
        int operator()(const GameCharacter&) const {...}
    };
    
    class GameLevel {                        //成员函数同样ok。
    public:
        float health(const GameCharacter&) const;
    };
    
    GameCharacter ebg1(calcHealth);
    GameCharacter ecc1(HealthCalculator());
    
    GameLevel currentLevel;
    GameCharacter ebg2(std::bind(&GameLevel::health,currentLevel,_1)); //成员函数需要指定对象，所以需要std::bind。
    ```
    
    
    

