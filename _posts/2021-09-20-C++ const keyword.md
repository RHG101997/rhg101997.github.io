---
title: C++ Const Keyword
author: Richard Hernandez
date: 2021-08-08 14:10:00 +0800
categories: [C++, C++ const]
tags: [C++, Advance, Proggramming, const, const functions, functions, methods]
---



## `const` with functions

Reference: [Video](https://www.youtube.com/watch?v=RC7uE_wl1Uc&ab_channel=BoQian)

Using const in methods examples, and can be used for more efficiency.

```cpp

class Dog {
    // Class Memebers
    int age;
    string name;

    public:
    Dog() {age =3; name = "dummy";}
    
    // Const parameter
    void setAge (const int& a ) {age = a;} // Correct[ pass by reference and can't be changed]
    void setAge (const int a ) {age = a;} // const is not useful here
    void setAge (int a ) {age = a;} // similar

    //  Const return value
    const string& getName() {return name;}; // pass by reference "more efficient"

    //const function [Means it will not modify any members of the class]
    void printDogName() const {
        cout << getName() << "const" << endl; // const functions can only called const functions
    }
    // Overload const function (will be called if instance of the class is const)
    void printDogName() {
        cout << name << "non-const" << endl; // const functions can only called const functions
    } 
     
}

int main(){
    // Call regular
    Dog d;
    d.printDogName();

    // Call overloaded 
    const Dog d2;
    d2.printDogName();
    }
}

```


