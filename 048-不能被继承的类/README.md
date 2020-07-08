
#2	题意
-------

**题目描述**

>设计一个不能被继承的类



#3	分析
-------

首先想到的是在C++中, 子类的构造函数会自动调用父类的构造函数, 同样, 子类的析构函数也会自动调用父类的析构函数

要想一个类不能被继承, 我们只要把它的构造函数和析构函数都定义为私有函数.

那么当一个类试图从它那继承的时候, 必然会由于试图调用构造函数、析构函数而导致编译错误

```cpp
class SealedClass
{
private :
    SealedClass( ){ }
    ~SealedClass( ){ };
};

```

可是这个类的构造函数和析构函数都是私有函数了, 我们怎样才能得到该类的实例呢


我们有很多种方法来实现类的创建

*	定义静态函数或者静态变量来创建和释放类的实例

*	友元函数可以访问类的私有成员

*	友元类可以访问类的私有成员

#4	静态的创建和释放类的实例
-------


##4.1	静态函数
-------

```cpp
#include <iostream>
using namespace std;


class SealedClass
{
public  :
    static SealedClass* GetInstance( )
    {
        return new SealedClass( );
    }
private :
    SealedClass( ){ }
    ~SealedClass( ){ };

};

class Base : public SealedClass
{
};

int main( )
{
    SealedClass *pb = Base::GetInstance( );
    Base base;

    return 0;
}
```

##4.2	静态变量
-------


```cpp
#include <iostream>

using namespace std;


class SealedClass
{
public  :
    static SealedClass* GetInstance( )
    {
        if(m_sc == NULL)
        {
            m_sc = new SealedClass( );
        }

        return m_sc;
    }
    static void Destroy( )
    {
        if(m_sc != NULL)
        {
            delete m_sc;
        }
    }
private :
    SealedClass( ){ }
    ~SealedClass( ){ }


    static SealedClass  *m_sc;
};


SealedClass* SealedClass::m_sc = NULL;


class Base : public SealedClass
{
};


int main( )
{
    SealedClass *pb = SealedClass::GetInstance( );
    SealedClass::Destroy( );

    pb = NULL;

    //Base b;  //  error
    return 0;
}
```




#5	友元实现
-------

##5.1	友元函数实现
-------



```cpp
#include <iostream>

using namespace std;


class SealedClass
{
private :
    SealedClass( ){ }
    ~SealedClass( ){ }

public  :
    friend SealedClass* GetInstance( );

};


SealedClass* GetInstance( )
{
    return new SealedClass( );
}


class Base : public SealedClass
{
};

int main( )
{
    SealedClass *p = GetInstance( );

    //Base base;    // error

    return 0;
}
```

##5.2	友元类实现
-------

```cpp
#include <iostream>
using namespace std;



class CNoHeritance
{
    friend class SealedClass;
private:
    CNoHeritance(){ }
    ~CNoHeritance(){ }
};



class SealedClass : virtual public CNoHeritance
{
public:
    SealedClass( ){ }

    ~SealedClass( ){ }
};


/*
class Base : public SealedClass
{
public:
    Base():SealedClass( ){ }
    ~Base(){ }
};
*/

int main( )
{
    SealedClass  sc;
    //Base base;

    return 0;
}
```

#6	总结
-------

```cpp
#include <iostream>
using namespace std;

template <typename T>
class CNoHeritance
{
    friend T;
private:
    CNoHeritance(){ }
    ~CNoHeritance(){ }
};


class SealedClass : virtual public CNoHeritance<SealedClass>
{
public:
    SealedClass( ){ }

    ~SealedClass( ){ }
};

/*
class Base : public SealedClass
{
public:
    Base():SealedClass( ){ }
    ~Base(){ }
};
*/

int main( )
{
    SealedClass  sc;
    //Base base;

    return 0;
}
```