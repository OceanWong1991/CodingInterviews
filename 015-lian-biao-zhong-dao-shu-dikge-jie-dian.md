# README

## 题意

**题目描述**

> 输入一个链表，输出该链表中倒数第k个结点。

## 分析

这道题我想大多数人都会有思路，因为我们已经见的很多了 最暴力的方式,两趟遍历，第一趟先求出list的长度length，然后进而length - k得到倒数第k个节点的位置 当然我们大多数都会知道另外一个更加高效的方法，**双指针法** 其实就是第一个指针right先向前走K步，然后left和right一起走，此时两个指针差别K步，那么当right走到链表尾部的时候，left指向的就是倒数第K个节点

期间要注意的问题有

* 链表可能为NULL
* 链表长度可能没有K个

```cpp
/// 1 -> 2 -> 3 -> 4 -> 5
/// 比如要走倒数第3个节点
/// 那么right先走到第3 - 1个节点&[2]
/// 那么right指针向前走到其下一个节点为NULL时, left节点既是倒数第K个节点
///  此时两个指针相差为K - 1


/// 1 -> 2 -> 3 -> 4 -> 5
/// 比如要走倒数第3个节点
/// 那么right先走到第3个节点&[2]
/// 那么right指针向前走到链表尾部为NULL时, left节点既是倒数第K个节点
/// 此时两个指针相差为K
class Solution
{
public:
    ListNode* FindKthToTail(ListNode* pListHead, unsigned int k)
    {
        if(pListHead == NULL)
        {
            return NULL;
        }
        unsigned int i = 0;
        ListNode *right = pListHead;

        //  left指针先向前走K步
        while(i < k && right != NULL)
        {
            debug <<"index  = " <<i <<", value = " <<right->val <<endl;
            right = right->next;
            i++;
        }

        if(right == NULL && i < k)
        {
            cout <<"the list length = " <<i <<" < " <<k <<endl;
            return NULL;
        }

        ListNode *left = pListHead;
        while(right != NULL)
        {
            debug <<"index  = " <<i++ <<", value = " <<right->val <<endl;

            left = left->next;
            right = right->next;
        }

        return left;

    }
};
```

当然也可以第一个指针right先向前走K-1步，然后left和right一起走，此时两个指针差别K-1步，那么当right走到链表尾部的前一个结点时候的，left指向的就是倒数第K个节点

```cpp
/// 1 -> 2 -> 3 -> 4 -> 5
/// 比如要走倒数第3个节点
/// 那么right先走到第3 - 1个节点&[2]
/// 那么right指针向前走到其下一个节点为NULL时, left节点既是倒数第K个节点
/// 此时两个指针相差为K - 1
/// 因此right需要走到链表尾部前一个结点


/// 1 -> 2 -> 3 -> 4 -> 5
/// 比如要走倒数第3个节点
/// 那么right先走到第3个节点&[2]
/// 那么right指针向前走到链表尾部为NULL时, left节点既是倒数第K个节点
/// 此时两个指针相差为K
/// 因此right需要走到链表尾部前

class Solution
{
public:
    ListNode* FindKthToTail(ListNode* pListHead, unsigned int k)
    {
        if(pListHead == NULL)
        {
            return NULL;
        }
        unsigned int i = 0;
        ListNode *right = pListHead;

        //  left指针先向前走K - 1步
        while(i < k - 1 && right != NULL)
        {
            debug <<"index  = " <<i <<", value = " <<right->val <<endl;
            right = right->next;
            i++;
        }

        if(right == NULL)
        {
            cout <<"the list length = " <<i <<" < " <<k <<endl;
            return NULL;
        }

        ListNode *left = pListHead;
        while(right->next != NULL)
        {
            debug <<"index  = " <<i++ <<", value = " <<right->val <<endl;

            left = left->next;
            right = right->next;
        }

        return left;

    }
};
```

