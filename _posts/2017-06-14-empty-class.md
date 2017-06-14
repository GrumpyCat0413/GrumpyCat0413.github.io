---
layout: post
title: C++空类，编译器为其自动生成6个函数
---
###空类使用时编译器为其自动生成

  构造函数、
  析构函数、
  拷贝构造函数、
  赋值操作符、
  取值操作符、
  取值操作符const
  
  
'''
class Empty  
{  
};  
空类占用一个字节的大小，用来占位（在内存中占位）
'''
'''
class Empty  
{  
public:  
    Empty();    // 默认构造函数  
    Empty(const Empty &);   // 默认拷贝构造函数  
    ~Empty();   // 默认析构函数  
    Empty& operator=(const Empty &);    // 赋值运算符  
    Empty* operator&(); //  取值运算符  
    const Empty* operator&() const; // 取值运算符 const  
};  

'''

使用时调用：
'''
Empty *e = new Empty();    //缺省构造函数  
delete e;                  //析构函数  
Empty e1;                  //缺省构造函数                                 
Empty e2(e1);              //拷贝构造函数  
e2 = e1;                   //赋值运算符  
Empty *pe1 = &e1;          //取址运算符(非const)  
const Empty *pe2 = &e2;    //取址运算符(const)  
'''

c++编译器对函数的实现：
'''
inline Empty::Empty()                          //缺省构造函数  
{  
}  
inline Empty::~Empty()                         //析构函数  
{  
}  
inline Empty *Empty::operator&()               //取址运算符(非const)  
{  
  return this;   
}             
inline const Empty *Empty::operator&() const    //取址运算符(const)  
{  
  return this;  
}  

inline Empty::Empty(const Empty &rhs)           //拷贝构造函数  
{  
  //对类的非静态数据成员进行以"成员为单位"逐一拷贝构造  
  //固定类型的对象拷贝构造是从源对象到目标对象的"逐位"拷贝  
}  
  
inline Empty& Empty::operator=(const Empty &rhs) //赋值运算符  
{  
  //对类的非静态数据成员进行以"成员为单位"逐一赋值  
  //固定类型的对象赋值是从源对象到目标对象的"逐位"赋值。  
}  


'''

