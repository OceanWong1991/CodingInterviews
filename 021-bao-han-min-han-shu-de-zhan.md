# 021

## ����

**��Ŀ����**

> ����ջ�����ݽṹ�����ڸ�������ʵ��һ���ܹ��õ�ջ��СԪ�ص�min������

## ����

˼·�ܼ򵥣�����ά������ջ��

* ����ջdata���洢ջ���������ڳ����ջ����
* ��Сջmin������ÿ��push��popʱ�����Сֵ��

��push-dataջ��ʱ�򣬽���ǰ��С����ѹ�룬

��pop-dataջ��ʱ�򣬽�minջջ������С���ݵ���

������֤minջ�д洢�ŵ�ǰ�ֳ�����Сֵ������������ջ�ĸ��¶�����

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
    stack<int>  m_data;     //  ����ջ
    stack<int>  m_min;      //  �洢ÿ��ջ����Сֵ��ջ��Ϣ
};
```

