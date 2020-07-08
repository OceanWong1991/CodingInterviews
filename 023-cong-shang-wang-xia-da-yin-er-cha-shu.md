# README

## ����

**��Ŀ����**

> �������´�ӡ����������ÿ���ڵ㣬ͬ��ڵ�������Ҵ�ӡ��

## ����

��ʵ���ǲ�α����������������֮ǰ��һƪ�������潫�ĺ������

> [�������ı�����⣨ǰ�����������-�ݹ�ͷǵݹ飩](http://blog.csdn.net/gatieme/article/details/51163010#t10)

���е��Ե�ʱ��ʹ����

> [��ָOffer--006-�ع�������](http://blog.csdn.net/gatieme/article/details/51108612)����������ͨ��ǰ���������������������ɶ�����

ֱ�������룬ֻ��Ҫֱ�ӽ����ǲ�α���������̣���ɽ�����ѹ��vector�м���

```cpp
#include <iostream>
#include <vector>
#include <deque>
#include <queue>

using namespace std;

//  ���Կ���
#define __tmain main

#ifdef __tmain

#define debug cout

#else

#define debug 0 && cout

#endif // __tmain


#define undebug 0 && cout


#ifdef __tmain
struct TreeNode
{
    int val;
    struct TreeNode *left;
    struct TreeNode *right;

    TreeNode(int x)
    :val(x), left(NULL), right(NULL)
    {
    }
};
#endif // __tmain



class Solution
{
    vector<int> res;
public:
    vector<int> PrintFromTopToBottom(TreeNode *root)
    {
        ///  ����1  -=>  ѭ�����õݹ��ӡÿһ��
        LevelOrder(root);

        ///  ����2  -=>  ʹ������˫�˶���
        LevelOrderDev(root);

        ///  ����3  -=>  ʹ��˫ָ���ʶ��ǰָ��ͽ���
        LevelOrderUsePoint(root);

        ///  ����4  -=>  ʹ��aprent��childzsizesize��ʶǰһ��͵�ǰ��Ľڵ����
        LevelOrderUseSize(root);

        ///  ����4  -=>  �ڶ����в��������ʶ����ʶ��ǰ�����
        LevelOrderUseEnd(root);
        return this->res;
    }

    /// ��ӡĳһ��Ľڵ㣬�ݹ�ʵ��
    int PrintLevel(TreeNode *root, int level)
    {
        if(root == NULL || level < 0)
        {
            return 0;
        }
        else if(level == 0)
        {
            debug <<root->val;
            res.push_back(root->val);        /// add for result in vector

            return 1;
        }
        else
        {
            return PrintLevel(root->left, level - 1) + PrintLevel(root->right, level - 1);
        }
    }

    ///  ѭ��ÿ�������ڵ㣬���õݹ麯��
    void LevelOrder(TreeNode *root)
    {
        res.clear( );           /// add for result in vector
        if(root == NULL)
        {
            return;
        }
        for(int level = 0; ; level++)
        {
            if(PrintLevel(root, level) == 0)
            {
                break;
            }
            debug <<endl;
        }
    }

    //////////////////////////
    ///  ʹ������˫�˶���
    //////////////////////////
    void LevelOrderDev(TreeNode *root)
    {
        res.clear( );           /// add for result in vector

        /// deque˫�˶��У�
        /// ֧�ֵ���������push_back()������
        /// ��vector��࣬��vector���˸�pop_front,push_front����

        deque<TreeNode *> qFirst, qSecond;
        qFirst.push_back(root);

        while(qFirst.empty( ) != true)
        {
            while (qFirst.empty( ) != true)
            {
                TreeNode *temp = qFirst.front( );
                qFirst.pop_front( );

                debug << temp->val;
                res.push_back(temp->val);        /// add for result in vector


                if (temp->left != NULL)
                {
                    qSecond.push_back(temp->left);
                }
                if (temp->right != NULL)
                {
                    qSecond.push_back(temp->right);
                }
            }
            debug << endl;
            qFirst.swap(qSecond);

        }
    }


    //////////////////////////
    ///  ʹ��˫ָ���ʶ��ǰָ��ͽ���
    //////////////////////////
    void LevelOrderUsePoint(TreeNode *root)
    {
        res.clear( );           /// add for result in vector

        vector<TreeNode*> vec;
        vec.push_back(root);

        int cur = 0;
        int end = 1;

        while (cur < vec.size())
        {
            end = vec.size();       ///  �µ�һ�з��ʿ�ʼ�����¶�λend�ڵ�ǰ�����һ���ڵ����һ��λ��

            while (cur < end)
            {
                debug << vec[cur]->val;  ///  ���ʽڵ�
                res.push_back(vec[cur]->val);        /// add for result in vector

                if (vec[cur]->left != NULL) ///  ѹ����ڵ�
                {
                    vec.push_back(vec[cur]->left);

                }
                if (vec[cur]->right != NULL)    ///  ѹ���ҽڵ�
                {
                    vec.push_back(vec[cur]->right);
                }
                cur++;
            }
            debug << endl;
        }
    }


    //////////////////////////
    ///  ʹ��aprent��childzsizesize��ʶǰһ��͵�ǰ��Ľڵ����
    //////////////////////////
    void LevelOrderUseSize(TreeNode *root)
    {
        res.clear( );           /// add for result in vector

        int parentSize = 1, childSize = 0;
        TreeNode *temp = NULL;

        queue<TreeNode *> q;
        q.push(root);
        while(q.empty( ) != true)
        {
            temp = q.front( );
            debug <<temp->val;
            res.push_back(temp->val);        /// add for result in vector

            q.pop( );

            if (temp->left != NULL)
            {
                q.push(temp->left);

                childSize++;
            }
            if (temp->right != NULL)
            {
                q.push(temp->right);
                childSize++;
            }

            parentSize--;
            if (parentSize == 0)
            {
                parentSize = childSize;
                childSize = 0;
                debug << endl;
            }

        }
    }

    //////////////////////////
    ///  �ڶ����в��������ʶ����ʶ��ǰ�����
    //////////////////////////
    void LevelOrderUseEnd(TreeNode *root)
    {
        res.clear( );           /// add for result in vector

        queue<TreeNode *> q;

        q.push(root);
        q.push(NULL);

        while(q.empty( ) != true)
        {
            TreeNode* node = q.front();
            q.pop();

            if (node)
            {
                debug << node->val;
                res.push_back(node->val);        /// add for result in vector

                if (node->left != NULL)
                {
                    q.push(node->left);
                }
                if (node->right != NULL)
                {
                    q.push(node->right);
                }
            }
            else if (q.empty( ) != true)
            {
                q.push(NULL);
                debug << endl;
            }
        }
    }


    struct TreeNode* reConstructBinaryTree(vector<int> pre,vector<int> in)
    {
        //  ǰ������ĳ��ȸ���������ĳ���Ӧ����ͬ
        if(pre.size( ) != in.size( ))
        {
            undebug <<"the length of PRE and IN should be smae" <<endl;
            return NULL;
        }

        // ���Ȳ���Ϊ0
        int size = pre.size( );
        if(size == 0)
        {
            undebug <<"it's a NULL tree(length = 0)" <<endl;
            return NULL;
        }

        int length = pre.size( );
        undebug <<"the length of your tree = " <<length <<endl;
        int value = pre[0];      //  ǰ������ĵ�һ������Ǹ��ڵ�
        TreeNode *root = new TreeNode(value);

        undebug <<"the root is" <<root->val <<endl;
        //  ����������в��ҵ�����λ��
        int rootIndex = 0;
        for(rootIndex = 0; rootIndex < length; rootIndex++)
        {
            if(in[rootIndex] == value)
            {
                undebug <<"find the root at " <<rootIndex <<" in IN" <<endl;
                break;
            }
        }
        if(rootIndex >= length)
        {
            undebug <<"can't find root (value = " <<value <<") in IN" <<endl;
            return NULL;
        }

        ///  ������������������
        ///  ���������, ����ߵľ���������, �ұߵľ���������
        ///  ǰ�������, ���������ȱ���������, Ȼ����������

        ///  ����ȷ�����������ĳ���, ���������in��ȷ��
        int leftLength = rootIndex;
        int rightLength = length - 1 - rootIndex;
        undebug <<"left length = " <<leftLength <<", rightLength = " <<rightLength <<endl;
        vector<int> preLeft(leftLength), inLeft(leftLength);
        vector<int> preRight(rightLength), inRight(rightLength);
        for(int i = 0; i < length; i++)
        {
            if(i < rootIndex)
            {
                //  ǰ������ĵ�һ���Ǹ��ڵ�, �������(leftLegnth = rootIndex) - 1���ڵ���������, �����i+1
                preLeft[i] = pre[i + 1];
                //  �������ǰ(leftLength = rootIndex) - 1���ڵ���������, ��rootIndex���ڵ��Ǹ�
                inLeft[i] = in[i];
                undebug <<preLeft[i] <<inLeft[i] <<" ";

            }
            else if(i > rootIndex)
            {
                //  ǰ������ĵ�һ���Ǹ��ڵ�, �������(leftLegnth = rootIndex) - 1���ڵ���������, ������������
                preRight[i - rootIndex - 1] = pre[i];
                //  �������ǰ(leftLength = rootIndex) - 1���ڵ���������, ��rootIndex���ڵ��Ǹ�, Ȼ����������
                inRight[i - rootIndex - 1] = in[i];
                undebug <<preRight[i - rootIndex - 1] <<inRight[i - rootIndex - 1] <<" ";

            }
        }
        undebug <<endl <<"the left tree" <<endl;
        for(int i = 0; i < leftLength; i++)
        {
            undebug <<preLeft[i] <<inLeft[i] <<" ";
        }
        undebug <<endl;
        undebug <<"the right tree" <<endl;
        for(int i = 0; i < rightLength; i++)
        {
            undebug <<preRight[i] <<inRight[i] <<" ";
        }
        undebug <<endl;


        root->left = reConstructBinaryTree(preLeft, inLeft);
        root->right = reConstructBinaryTree(preRight, inRight);

        return root;
    }



};

int __tmain( )
{
    int pre[] = { 1, 2, 4, 7, 3, 5, 6, 8 };
    int in[] = { 4, 7, 2, 1, 5, 3, 8, 6 };

    vector<int> preOrder(pre, pre + 8);
    vector<int>  inOrder( in,  in + 8);

    Solution solu;
    TreeNode *root = solu.reConstructBinaryTree(preOrder, inOrder);
    solu.PrintFromTopToBottom(root);

    return 0;
}
```

