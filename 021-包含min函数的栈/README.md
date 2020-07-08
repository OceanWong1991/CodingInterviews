

#题意
-------


**题目描述**


>定义栈的数据结构，请在该类型中实现一个能够得到栈最小元素的min函数。


#分析
-------

思路很简单，我们维持两个栈，

*    数据栈data，存储栈的数据用于常规的栈操作

*    最小栈min，保存每次push和pop时候的最小值，

在push-data栈的时候，将当前最小数据压入，

在pop-data栈的时候，将min栈栈顶的最小数据弹出

这样保证min栈中存储着当前现场的最小值，并随着数据栈的更新而更新

```cpp

class Solution
{
public:
    void push(int value)
    {
        this->m_data.push(value);
//        if(this->m_min.size( ) <= 1)
//        {
//            this->m_min.push(value);
//        }
//        int min_data = MIN(value, this->m_min.top( ));
//        this->m_min.push(min_data);

        if(this->m_min.size( ) == 0 || value < this->m_min.top( ))
        {
            this->m_min.push(value);
        }
        else
        {
            this->m_min.push(this->m_min.top( ));
        }
    }

    void pop()
    {
        assert(this->m_data.size( ) > 0 && this->m_min.size( ) > 0);

        this->m_data.pop( );
        this->m_min.pop( );
    }

    int top()
    {
        assert(this->m_data.size( ) > 0 && this->m_min.size( ) > 0);


        return this->m_data.top( );
    }

    int min()
    {
        if(this->m_data.empty( ) == true)
        {
            return 0;
        }

        return this->m_min.top( );
    }
protected:
    stack<int>  m_data;     //  数据栈
    stack<int>  m_min;      //  存储每次栈中最小值的栈信息
};
```
