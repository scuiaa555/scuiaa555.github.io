---
layout: post
title: "C++ Basics"
description: "Notes on C++ reference books"
date: 2017-02-17
tags: [programming, c++, notes]
comments: true
share: true
---

### 《Effective C++》

---
1. const的好处
    - 只要（某值保持不变）是事实，就应该申明是const，因为说出来可以获得编译器的帮助，确保这条约束不被违反。
    - const成员函数可以作用于const对象。
    - 使用const速度更快，例如pass by reference-to-const方式传递对象。
2. 令non-const函数调用其const兄弟是一个避免代码重复的安全做法。 (p. 24)

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
3. - 内置类型必须手动完成初始化。 (p. 27)
   - 使用member initialization list 通常效率更高。因为采用赋值初始化的话，default constructor的作用就浪费了。 (p. 28)
