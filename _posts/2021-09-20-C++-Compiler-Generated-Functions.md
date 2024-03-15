---
title: C++ Compiler Generated Functions
author: Richard Hernandez
date: 2021-09-20 14:10:00 +1200
categories: [C++, C++ Generated Functions]
tags: [C++, Advance, Proggramming, methods, functions, generated]
---

## What are generated functions:

Reference: [Video](https://www.youtube.com/watch?v=KMSYmY74AEs&ab_channel=BoQian)

This are functions that the compiler adds when you don't explicitly declare them(only if is required):
1. Copy constructor
2. Copy Assigment Operator
3. Destructor
4. Default constructor (only if there is not contructor declared)


```cpp 

class Dog{};

// similar to

class Dog{

    dog(const dog& rhs) {...} //Member by Member initialization
    dog& operator=(const dog& rhs ) {...}; // Member by Member copying
    dog() {...}; // 1- Call base class's default constructor
                 // 2- Call data member's default constructor. 
    ~dog() {...};// 1- Call data class's destructor.
                 // 2- Call data member's destructor. 
}

```

Additional Information:
1. These are public and inline.
2. They are generatedd only if they are needed
3. A `default constructor` is a constructor that can work without any parameters.

