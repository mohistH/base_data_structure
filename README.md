
##欢迎指正 
# base_data_structure
	记录常用的数据结构:
		排序
		单向链表
		双向链表
		栈
		队列
		二叉树
		平衡二叉树（AVL树）
		红黑树
		
		c++实现
`欢迎指正`




### 更新说明:
#### 1.9 更新时间：3-22-2019 增加二叉树的高度获取方法（递归） 

	https://www.cnblogs.com/pandamohist/p/10581532.html

#### 1.8 更新时间：3-22-2019 21:55 补充平衡二叉树的 层次遍历（队列 与 数组实现）
#### 1.7 更新时间：3-21-2019 22:38 重新提交平衡二叉树的完整算法（c++实现），修复：
###### 1.7.1 左旋和右旋返回值值应该为旋转后的根结点
##### 1.7.2 删除函数del_node2中，增加树的平衡判断
##### 1.7.3 更新删除算法，当删除结点同时含有左右孩子时，应该先判断当前结点左右子树高度情况，从高度较高的子树中找替代当前结点的值
#### 1.6 更新时间：2019-3-20 23:28 修正平衡二叉树插入结点时，新建结点的高度应该为1。
#### 1.5 更新时间：2019-3-20 23:19 修正平衡二叉树删除RL型与RR型判断条件写反的情况
#### 1.4 更新时间：2019-3-20 07:58 添加平衡二叉树(初版)，详见 下文
#### 1.3 更新时间：2019-3-14 21:48，详见 下文 1.3 二叉树 
		·补充并完善删除某个结点函数 remove_node(int del_data);
		·参考《算法导论》实现





#### 自己实现vector动态数组（c++）
	##仅实现了部分功能， 后续更新##

```c++
#ifndef _my_vector
#define _my_vector

#include <iostream>
#include <cstdlib>
#include <cstdio>

using namespace std;



// 定义了异常

class UnderflowException { };  
class IllegalArgumentException { };  
class ArrayIndexOutOfBoundsException { };  
class IteratorOutOfBoundsException { };  
class IteratorMismatchException { };  
class IteratorUninitializedException { };  
class NullItemException{};


template <typename T> 


class myVector
{

public:
    typedef T* iterator;

private:
    T *data;
    int len;
    int size;

    int cur_index;

public:
    // 无参数构造函数
    myVector():data(NULL), len(0), size(0)
    {
        cur_index = 0;
    }

    virtual ~myVector()
    {
        if (NULL != data)
        {
            delete []data;
            data = NULL;
        }
    }

    // 拷贝构造函数
    myVector(const myVector& tmp)
    {
        if (this == &tmp)
            return;
        
        data = new(std::nothrow) T[tmp.len];
        if (NULL == data)
            return;

        for (int i = 0; i < tmp.size; i++)
            data[i] = tmp.data[i];
        
        len = tmp.len;
        size = tmp.size;
    }

    // 重载 []
    T & operator[](int index)
    {
        if ( (0 > index) || (size < index) )
            throw IllegalArgumentException();

        if (NULL    == data)
            throw NullItemException();

        return data[index];
    }

    // 重载 = 
    const myVector & operator = (const myVector &tmp)
    {
        if (this != &tmp)
        {   
            if (NULL != data)
                delete []data;

            size    = tmp.size;
            len     = tmp.len;
            data = new(std::nothrow) T[len];
            memcpy(data, tmp.data, len);
        }

        return *this;
    }

    //  
    const myVector& push_back(const T tmp)
    {
        // 若当前缓冲区无法存下，则需要新开辟
        if (len <= size)
        {
            T *new_data = new(std::nothrow) T[len * 2 + 1];
            memcpy(new_data, data, len * sizeof(T));
            delete []data;
            data = new_data;
            len = 2 * len + 1;
        }

        data[size++] = tmp;
        return *this;
    }

    void resize(int new_size)
    {
        if (new_size > size)
            reverse((2 * new_size) + 1);
        
        size = new_size;
    }

    void reverse(int new_size)
    {
        T *old_data = data;

        int num2copy = (new_size < size) ?  new_size : size;
        len = new_size;

        data = new(std::nothrow)T [len];
        if (NULL != data)
        {
            memcpy(data, old_data, num2copy);
            size = num2copy;

            delete []old_data;
            old_data = NULL;
        }
    }

    

    void pop_back()
    {
        if (0 == size)
            throw UnderflowException();
        
        size--;
    }

    iterator begin()
    {
        cur_index = 0;
        return &data[cur_index];
    }

    iterator end()
    {
        return &data[size-1];
    }

    iterator next()
    {
        cur_index++;
        if ( (0 > cur_index) || (cur_index > size) )
            throw ArrayIndexOutOfBoundsException();
        return &data[cur_index];
    }



    // 
    int get_size()
    {
        return size;
    }

};


struct A
{
    int data;
    void zero()
    {
        data = 0;
    }

    A()
    {
        zero();
    }

    A(int val) :data(val)
    {
        
    }
};


int main()
{
    myVector<A> aaa;

    aaa.push_back(A(1));
    aaa.push_back(A(2));
    aaa.push_back(A(3));
    aaa.push_back(A(4));
    aaa.push_back(A(5));

    for (int i = 0; i< 5; i++)
    {
        cout << "i = " << i  << ", val = " << aaa[i].data << endl;
    }

    cout << "size = " << aaa.get_size() << endl;

    return 0;
}



#endif //_my_vector
```




#### 1.二叉树
##### 1.1 初步实现二叉树的基本功能，包括：添加结点、查找结点、3中遍历结点、释放树
#### 1.2 更新时间:2019-3-13 22:21 更新部分功能：
		·查找某个结点的父节点
		·查找最小值与最大值
#### 1.3 更新时间：2019-3-14 21:48， 
		·补充并完善删除某个结点函数 remove_node(int del_data);
		·参考《算法导论》实现
```c++
	
#include <iostream>
using namespace std;

struct node 
{
    // 数据域
    int data;

    // 左节点
    node *lc;

    // 右结点
    node *rc;

    // 构造函数
    node()
        : data(0)
        , lc(NULL)
        , rc(NULL)
    {
    }
};


// bst
class bstree
{
public:
    enum
    {
        hmax_size_32767     = 32767,
        hmin_size_0         = 0,
    };

public:

    // 构造函数
    bstree()
        : root(NULL)
        , size(0)
    {
    }

    // 析构函数
    virtual ~bstree(){}
    
    int get_size()
    {
        return size;
    }

    // 插入结点
    void insert_node(int data)
    {
        int cur_size = get_size();
        if (hmax_size_32767 == cur_size)
        {
            cout << "insert node error, the size of the tree is max" << endl;
            return ;
        }
        root = insert(root, data);
    }

    // 先序遍历（前序遍历）
    void pre_order()
    {
        pre_order_traverse(root);
    }

    // 中序遍历
    void in_order()
    {
        in_order_traverse(root);
    }

    // 后序遍历
    void last_order()
    {
        last_order_traverse(root);
    }

    /*
        查找某个结点
        int key - 查找结果

        返回值：
            NULL : 可能为root为空 或者 没有找到
            != NULL, 找到结点
    */
    node* query(int key)
    {
        if (NULL == root)
        {
            cout << "query error, root = null" << endl;
            return NULL;
        }

        return query_node(root, key);
    }

    // 删除树
    void remove_all()
    {
        if (NULL == root)
        {
            cout << "remove all failed, root = null" << endl;
            return;
        }
        
        remove_all(root);

        int cur_size = get_size();
        if (0 == cur_size)
            root = NULL;
    }

    // 删除某个结点
    void remove_node(int del_data)
    {
        if (NULL == root)
        {
            cout << "remove node error, root = null" << endl;
            return;
        }

        node *parent_node   = NULL;
        node *del_node      = root;

        // 找到删除结点的父节点与删除结点
        while (del_node)
        {
            if (del_data == del_node->data)
                break;
            else if (del_data > del_node->data)
            {
                parent_node = del_node;
                del_node    = del_node->rc;
            }
            else if (del_data < del_node->data)
            {
                parent_node = del_node;
                del_node = del_node->lc;
            }
        }

        // 若没有找到要删除的结点
        if (NULL == del_node)
        {
            cout << "remove node error, " << del_data << " was not find" << endl;
            return;
        }

        // 1、若删除的结点没有左子树和右子树
        if ( (NULL == del_node->lc) && (NULL == del_node->rc)  )
        {
            // 为什么要先判断根结点，因为根结点的父节点找不到，结果为NULL，
            // 1.1 可能只有一个根结点， 将root释放值为空
            if (del_node == root)
            {
                root = NULL;
                delete del_node;
                del_node = NULL;

                dec_size();
                return;
            }

            // 1.2 非根结点，那就是叶子结点了， 将父节点指向删除结点的分支指向NULL
            if  (del_node == parent_node->lc)
                parent_node->lc = NULL;
            else if (del_node == parent_node->rc)
                parent_node->rc  = NULL;

            // 释放结点
            delete del_node;
            del_node = NULL;
            dec_size();
        }

        // 2、若删除结点只有左孩子，没有右孩子
        else if ( (NULL != del_node->lc) && (NULL == del_node->rc) )
        {
            // 2.1 删除结点为根结点，则将删除结点的左孩子替代当前删除结点
            if (del_node == root)
            {
                root = root->lc;
            }
            // 2.2 其他结点，将删除结点的左孩子作为父节点的左孩子
            else
            {
                if (parent_node->lc == del_node)
                    parent_node->lc = del_node->lc;
                else if (parent_node->rc == del_node)
                    parent_node->rc = del_node->lc;
            }

            delete del_node;
            del_node = NULL;

            dec_size();
        }

        // 3、若删除结点只有右孩子
        else if ( (NULL == del_node->lc) && (NULL != del_node->rc) )
        {
            // 3.1 若为根结点
            if (root == del_node)
            {
                root = root->rc;
            }
            else
            {
                if (del_node == parent_node->lc)
                    parent_node->lc = del_node->rc;
                else if (del_node == parent_node->rc)
                    parent_node->rc = del_node->rc;
            }

            delete del_node;
            del_node = NULL;

            dec_size();
        }

        // 4、若删除结点既有左孩子，又有右孩子,需要找到删除结点的后继结点作为根结点
        else if ( (NULL != del_node->lc) && (NULL != del_node->rc) )
        {
            node *successor_node = del_node->rc;
            parent_node = del_node;

            while (successor_node->lc)
            {
                parent_node = successor_node;
                successor_node = successor_node->lc;
            }

            // 交换后继结点与当前删除结点的数据域
            del_node->data = successor_node->data;
            // 将指向后继结点的父节点的孩子设置后继结点的右子树
            if (successor_node == parent_node->lc)
                parent_node->lc = successor_node->rc;
            else if (successor_node == parent_node->rc)
                parent_node->rc = successor_node->rc;

            // 删除后继结点
            del_node = successor_node;
            delete del_node;
            del_node = NULL;

            dec_size();
        }
    }

    // 返回以proot为根结点的最小结点
    node *get_min_node(node *proot)
    {
        if (NULL == proot->lc)
            return proot;

        return get_min_node(proot->lc);
    }

    // 返回以proo为根节点的最大结点
    node *get_max_node(node *proot)
    {
        if (NULL == proot->rc)
            return proot;
        
        return get_max_node(proot->rc);
    }

    // 返回根节点
    node *get_root_node()
    {
        return root;
    }

    // 返回proot结点的父节点
    node *get_parent_node(int key)
    {
        // 当前结点
        node *cur_node = NULL;
        // 父节点
        node *parent_node = NULL;

        cur_node = root;

        // 标记是否找到
        bool is_find = false;
        while (cur_node)
        {
            if (key == cur_node->data)
            {
                is_find = true;
                break;
            }

            // 因为比当前结点的值还要小，所以需要查找当前结点的左子树
            else if (key < cur_node->data)
            {
                parent_node = cur_node;
                cur_node = cur_node->lc;
            }
            // 同上， 查找当前结点的右子树
            else if (key > cur_node->data)
            {
                parent_node = cur_node;
                cur_node    = cur_node->rc;
            }
        }

        return (true == is_find)?  parent_node :  NULL; 
    }

	// 3-22-2019 新增
    int get_tree_deepth()
    {
        return get_tree_deepth(root);
    }


private:
	// 3-22-2019 新增
    // 返回树的高度，采用 后序遍历的思想
    int get_tree_deepth(node *pnode)
    {
        if (NULL == pnode)
            return 0;

        int left_h = get_tree_deepth(pnode->lc) ;
        int right_h = get_tree_deepth(pnode->rc);

        return (left_h > right_h) ? (1 + left_h) : (1 + right_h);
    }

private:
    //查找某个值
    node *query_node(node *proot, int key)
    {
        if (NULL == proot)
        {
            return proot;
        }

        if (proot->data == key)
            return proot;
        else if (proot->data > key)
        {
            return query_node(proot->lc, key);
        }
        else if (proot->data < key)
        {
            return query_node(proot->rc, key);
        }
        
        return NULL;
    }

    // 后序遍历删除所有结点
    void remove_all(node *proot)
    {
        if (NULL != proot)
        {
            remove_all(proot->lc);
            remove_all(proot->rc);
            delete proot;

            dec_size();
        }
    }

    // 先序遍历
    void pre_order_traverse(node *proot)
    {
        if (NULL != proot)
        {
            cout << proot->data << ",   "; 
            pre_order_traverse(proot->lc);
            pre_order_traverse(proot->rc);
        }
    }

    // 中序遍历
    void in_order_traverse(node *proot)
    {
        if (NULL != proot)
        {
            in_order_traverse(proot->lc);
            cout << proot->data << ",   "; 
            in_order_traverse(proot->rc);
        }
    }

    // 后续遍历
    void last_order_traverse(node *proot)
    {
        if (NULL != proot)
        {
            last_order_traverse(proot->lc);
            last_order_traverse(proot->rc);
            cout << proot->data << ",   ";
        }
    }

    // 插入结点
    node *insert(node *proot, int data)
    {
        // 结点不存在， 则创建
        if (NULL == proot)
        {
            node *new_node = new(std::nothrow) node;
            if (NULL != new_node)
            {
                new_node->data = data;
                proot = new_node;
                
                // 结点+1；
                add_size();
            }

            return proot;
        }

        //  插入值比当前结点值还要小， 则应该插入到当前节点的左边
        if (proot->data > data)
        {
            proot->lc = insert(proot->lc, data);
        }
        // 插入之比当前结点值还要打，则应该插入到当前结点的右边
        else if (proot->data < data)
        {
            proot->rc = insert(proot->rc, data);
        }

        // 相等，则不插入结点。

        return proot;
    }

    // size + 1
    void add_size()
    {
        if (hmax_size_32767 == size)
            return ;
        size++;
    }

    // size - 1
    void dec_size()
    {
        if ( hmin_size_0 == size)
        {
            return ;
        }

        size--;
    }

private:
    // 根结点
    node *root;

    // 当前树的结点个数
    int size;
};



int main()
{
    // 构造测试数据
    bstree tree;
    tree.insert_node(5);

    tree.insert_node(3);
    tree.insert_node(1);
    tree.insert_node(4);

    tree.insert_node(7);
    tree.insert_node(10);
    tree.insert_node(6);
    tree.insert_node(11);


    // 输出结点个数
    cout << "cur size = " << tree.get_size() << endl;

    // 前序遍历
    cout << "pre order" << endl;
    tree.pre_order();
    cout << endl;

    // 中序遍历
    cout << endl << "in order " << endl;
    tree.in_order();
    cout << endl;

    // 后序遍历
    cout << endl << "last order" << endl;
    tree.last_order();
    cout << endl;

    // 输出最大结点
    node *node_max = tree.get_max_node(tree.get_root_node());
    if (NULL != node_max)
    {
        cout << "tree's max value is = " <<   node_max->data << endl;
    }
    else
    {
        cout << "tree is null" << endl;
    }

    // 输出最小结点
    node *node_min = tree.get_min_node(tree.get_root_node());
    if (NULL != node_max)
    {
        cout << "tree's max value is = " <<   node_min->data << endl;
    }
    else
    {
        cout << "tree is null" << endl;
    }

    
    // 查找某个节点
    int find_key = 0;
    cout << "input find key = ";
    cin >> find_key;

    node *find_node = tree.query(find_key);
    if (NULL != find_node)
    {
        cout << find_key << " has existed, data =  " << find_node->data << endl;
    }
    else
    {
        cout << find_key << "was not find" << endl;
    }

    // 查找某个结点的父节点
    cout << endl << endl << "请输入要查找的结点的:";
    int find_parent = 0;
    cin >> find_parent;
    node *parent = tree.get_parent_node(find_parent);
    if (NULL != parent)
        cout << endl << find_parent << " parent node is " << parent->data  << endl;
    else
        cout << endl << "not find parent node" << endl;

    // 释放所有结点
    cout << endl << "destroy" << endl;
    tree.remove_all();

    cout << "cur size = " << tree.get_size() << endl;

    return 0;
}




```

####2.归并排序
```c++

#include<iostream>
#include <vector>
using namespace std;

// 重命名数组
typedef vector<int> pvArr;


// c++ int数组实现, 递归实现
class solution
{
public:

    /*
    src - 排序数组
    tmp - 临时数组
    left - 数组起始下标，从0开始数
    right - 数组边界下标，从0开始数
    */ 
    void merge_sort(int src[], int tmp[], int left, int right)
    {
        // 非法检测
        if ( (0 > left) || (0 > right) )
            return;

        if ( (nullptr == src) || (NULL == src) )
            return;
        
        if ( (nullptr == tmp) || (NULL == tmp) )
            return;
        
        // 
        if (left < right)
        {
            int mid = (left + right ) / 2;
            merge_sort(src, tmp, left, mid);
            merge_sort(src, tmp, mid + 1, right);
            merge_array(src, tmp, left, right);
        }
    }

private:
    /*
    src - 排序数组
    tmp - 临时数组
    left - 数组起始下标，从0开始数
    right - 数组边界下标，从0开始数
    */ 
    void merge_array(int src[], int tmp[], int left,  int right)
    {
        // 计算得到中间的下标位置
        int mid = (left + right ) / 2;

        // 从左边开始的索引， 便于从数组左边开始取数
        int left_index = left;

        // 从右边开始的索引， 便于从数组中间开始取数
        int right_index = mid + 1;

        // 临时数组存放数据的有效个数
        int min_index = 0;
        while ( (left_index <= mid) && (right_index <= right))
        {
            // 比左边还大，取左边的数放入临时缓冲
            if (src[left_index] < src[right_index])
                tmp[min_index++] = src[left_index++];
            else
                tmp[min_index++]  = src[right_index++];
        }

        // 继续遍历数组的左边，直到中间结束，
        while (left_index <= mid)
            tmp[min_index++] = src[left_index++];
        
        // 继续遍历数组右边剩下的部分元素 
        while (right_index <= right)
            tmp[min_index++] = src[right_index++];
        
        // 将排序后的结果放入源数组中
        for (int i = 0; i < min_index; i++)
            src[left + i] = tmp[i];
    }
};


class solution2
{
public:
    /*
    src - 待排序数组
    left - 数组的起始下标，从0 开始数
    right - 数组的右边界下标，从0开始数
    */
    void merge_sort(pvArr &src, int left, int right)
    {
        if ( (0 > left) || (0 > right) )
            return;
        
        if (left < right)
        {
            int mid = (left + right) / 2;
            merge_sort(src, left, mid);
            merge_sort(src, mid+1, right);
            merge(src, left, right);
        }
    }

private:
    /*
    src - 待排序数组
    left - 数组的起始下标，从0 开始数
    right - 数组的右边界下标，从0开始数
    */
    void merge(pvArr &src, int left, int right)
    {
        int mid = (left + right ) / 2;
        int left_index = left;
        int right_index = mid + 1;
        pvArr tmparr;

        int min_index = 0;
        while ( (left_index <= mid) && (right_index <= right) )
        {
            min_index = (src[left_index] < src[right_index]) ? left_index++ : right_index++;
            tmparr.push_back(src[min_index]);
        }

        while (left_index <= mid)
            tmparr.push_back(src[left_index++]);

        while (right_index <= right)
            tmparr.push_back(src[right_index++]);

        for (int i = 0; i < tmparr.size(); i++)
        {
            src[left + i] = tmparr[i];
        }
    }
};

int main()
{
    // solution ssssssss;
    // int int_arr[]   = {8, 4, 5, 7,    1, 3, 6, 2};
    // int int_tmp[8] = {0};

    // 构造测试数据
    pvArr arr;
    arr.push_back(8);
    arr.push_back(4);
    arr.push_back(5);
    arr.push_back(2);
    arr.push_back(1);
    arr.push_back(3);
    arr.push_back(6);
    arr.push_back(2);

    cout << "-------------------------" << endl;

    // 执行归并排序
    solution2 sl2;
    sl2.merge_sort(arr, 0, arr.size() - 1);

    // 输出
    int size = arr.size();
    for (int i = 0; i < size; i++)
        cout << "i = " << i + 1 << ", num = " << arr[i] << endl;

    return 0;
}
```
####3.平衡二叉树 2019-3-21 22:37
```c++
#include <iostream>

using namespace std;



struct node 
{
    int data;
    int height;
    node *lc;
    node *rc;
    node()
        : data(0)
        , height(0)
        , lc(0)
        , rc(0)
    {

    }
};



// 平衡二叉树
class avltree2
{
public:

    // 构造函数 
    avltree2()
        : root(NULL)
    {

    }

    virtual ~avltree2(){}
//---------------------------------------------------------

    // 返回结点的高度
    int height(node *pnode)
    {
        return pnode == NULL ? 0 : pnode->height;
    }

    // 计算两个的最大值
    int max(int a, int b)
    {
        return a > b ? a : b;
    }

//------------------------------------------------------------
/*
    旋转代码:
        LL型
        RR型
        LR型
        RL型
*/ 

    // 左旋转， 函数命名方式是以二叉树插入结点的形状命名的，这里的左旋，
    // 对应的形状是 RR型，即在结点右孩子插入结点。
    node *rotate_rr(node *pnode)
    {
        node *ret_node = NULL;

        if (NULL != pnode)
        {
            // 保存失衡结点的右孩子
            node *loss_balance_node_rc = pnode->rc;

            // 将失衡结点的右孩子的左孩子赋值给失衡结点的右孩子
            pnode->rc   = loss_balance_node_rc->lc;

            // 将失衡结点作为失衡结点的左孩子
            loss_balance_node_rc->lc    = pnode;

            // 更新失衡结点与新根结点的高度
            pnode->height = max(height(pnode->lc), height(pnode->rc)) + 1;
            loss_balance_node_rc->height = max( height( loss_balance_node_rc->lc), 
                                                height(loss_balance_node_rc->rc)) + 1;

            ret_node = loss_balance_node_rc;
        }

        return ret_node;
    }

    // 右旋, 函数名是以插入结点后的形式命名的，这里是
    // 将新结点插入后，形成LL形状， 
    node *rotate_ll(node *pnode)
    {
        node *ret_node = NULL;

        if  (NULL != pnode)
        {
            node *loss_balance_node_lc    = pnode->lc;

            if (NULL != loss_balance_node_lc)
            {
                // 将失衡结点的左孩子的右孩子为失衡结点的左孩子
                pnode->lc = loss_balance_node_lc->rc;

                // 将失衡结点作为失衡结点左孩子的右孩子
                loss_balance_node_lc->rc    = pnode;

                // 更新高度
                pnode->height   = max(height(pnode->lc), height(pnode->rc)) + 1;

                loss_balance_node_lc->height = max( height(loss_balance_node_lc->lc),   
                                                    height(loss_balance_node_lc->rc)) + 1;
            }

            ret_node = loss_balance_node_lc;
        }

        return ret_node;
    }


    // LR 型
    // 函数命名方式同上, 需要先左旋在右旋
    node* rotate_lr(node *pnode)
    {
        if (NULL != pnode)
        {
            pnode->lc = rotate_rr(pnode->lc);
            
            pnode = rotate_ll(pnode);
        }

        return pnode;
    }

    // RL型
    // 同上，需要先右旋，再左旋
    node *rotate_rl(node *pnode)
    {
        if (NULL != pnode)
        {
            // 先右旋再左旋
            pnode->rc = rotate_ll(pnode->rc);

            pnode = rotate_rr(pnode);
        }
        
        return pnode;
    }

//------------------------------------------------------------

    

    // 计算pnode结点的平衡因子
    int get_balance_factor(node *pnode)
    {
        // 平衡因子= 左孩子 - 右孩子
        return (NULL == pnode) ? 0 : height(pnode->lc) - height(pnode->rc);
    }


    // 找到以pnode为根结点的最小结点
    node *get_min(node *pnode)
    {
        if (NULL == pnode)
            return pnode;

        while (pnode->lc)
            pnode = pnode->lc;
        
        return pnode;
    }

    // 找到以pnode为根结点的最大结点
    node *get_max(node *pnode)
    {
        if (NULL == pnode)
        {
            return pnode;
        }

        while (pnode->rc)
            pnode = pnode->rc;

        return pnode;
    }

    // 插入结点
    void insert(int key)
    {
        root = insert(root, key);
    }

    // 删除结点值为key的结点
    void remove_node(int key)
    {
        if (NULL == root)
            return;

        // del_node(root, key);

        root = del_node2(root, key);
    }


    // 中序遍历
    void in_order_traverse()
    {
        if (NULL == root)
            return;

        in_order(root);
    }

    // 删除整棵树
    void remove()
    {
        if (NULL == root)
            return;
        
        remove(root);
    }

    // 前序遍历
    void pre_order_traverse()
    {
        if (NULL == root)
            return;

        pre_order(root);
    }

	// ----------- 3-22-2019 -------- 新增------	// 层次遍历树 （使用队列）
    void layer_order()
    {

        cout << endl << endl << "层次遍历,是用队列完成" << endl;
        queue<node*> vq;

        if (NULL != root)
            vq.push(root);
        
        // 队列不为空，继续遍历
        while (false == vq.empty())
        {
            cout << vq.front()->data << " -> ";

            // 若左孩子不为空，则继续加入队列
            if ( NULL != vq.front()->lc )
            {
                vq.push(vq.front()->lc);
            }

            // 若右孩子不为空，则继续入队
            if (NULL  != vq.front()->rc)
                vq.push(vq.front()->rc);
        
            // 已经遍历输出的元素，出队
            vq.pop();
        }

        cout << endl << endl;
    }
 
	// ----------- 3-22-2019 -------- 新增------
    // 层次遍历，不是用队列，使用数组完成
    void layer_order_arr()
    {
        cout << endl << endl << " 层次遍历，不是用队列，使用数组完成 " << endl;

        // 定义指针数组的大小
        const int arr_len = 100;
        // 保存结点指针
        node *arr[arr_len] = {0};

        // 添加元素索引，添加到数组时使用
        int in_index = 0;

        // 输出元素索引，输出元素使用
        int out_index = 0;

        // 添加结点
        if (NULL != root)
            arr[in_index++] = root;

        // 若 添加元素索引大于输出元素索引，说明数组中还有没有输出的元素
        while ( in_index > out_index)
        {
            // 输出
            cout << arr[out_index]->data << " -> ";

            // 若左子树不为空，将其添加到数组
            if (NULL != arr[out_index]->lc)
                arr[in_index++] = arr[out_index]->lc;
            
            // 若右子树不为空，将其添加到数组
            if (NULL != arr[out_index]->rc)
                arr[in_index++] = arr[out_index]->rc;
            
            // 输出索引指向数组的下一个元素
            out_index++;
        }
    }


private:
    // 删除所有结点
    void remove(node *pnode)
    {
        if (pnode)
        {
            remove(pnode->lc);
            remove(pnode->rc);
            delete pnode;
        }

        pnode = NULL;
    }

    // 前序遍历
    void pre_order(node *pnode)
    {
        if (NULL != pnode)
        {
            cout << pnode->data << ", height = " << pnode->height << "  ";
            pre_order(pnode->lc);
            pre_order(pnode->rc);
        }
    } 

    // 中序遍历
    void in_order(node *pnode)
    {
        if (NULL != pnode)
        {
            in_order(pnode->lc);
            cout << pnode->data << ", height = " << pnode->height << "  ";
            in_order(pnode->rc);    
        }
    }
    
    // 找到参数结点的父节点
    node *get_parent_node(node *pnode)
    {
        if (NULL == pnode)
        {
            return pnode;
        }

        node *parent_node = NULL;
        int key = pnode->data;
        
        node *cur_node = root;
        while (cur_node)
        {
            if (key == cur_node->data)
                break;
            else if (key < cur_node->data)
            {
                parent_node = cur_node;
                cur_node = cur_node->lc;
            }
            else if (key > cur_node->data)
            {
                parent_node = cur_node;
                cur_node = cur_node->rc;
            }
        }

        return parent_node;
    }


    // 插入
    node* insert(node *pnode, int key)
    {

        // 为空，则创建结点
        if (NULL == pnode)
        {
            pnode = new(std::nothrow) node;
            if (NULL != pnode)
            {
                pnode->data = key;

                pnode->height = 1;
                return pnode;
            }
        }

        // 1、插入值比当前结点的值还要小,应该继续找左子树
        if (key < pnode->data)
        {
            pnode->lc = insert(pnode->lc, key);
        }
        // 2、比当前值大。应该继续找当前结点的右子树
        else if (key > pnode->data)
        {
            pnode->rc = insert(pnode->rc, key);
        }
        else 
        {
            // 3、相等，树中已经存在插入的结点了，无法继续插入相同结点
            return pnode;
        }

        // 4、更新结点的高度
        pnode->height = max(height(pnode->lc), height(pnode->rc)) + 1;

        // 5、获取插入后的平衡情况的平衡因子
        int pnode_bf     = get_balance_factor(pnode);

        // 6、下面开始判断新插入节点的平衡因子，当前插入结果是否需要旋转结点
        // 6.1 若为LL型,右旋
        if ( (1 < pnode_bf) && (key < pnode->data) )
            return rotate_ll(pnode);
        // 6.2 若为RR型，左旋
        if ( (-1 > pnode_bf) && (key > pnode->data) )
            return rotate_rr(pnode);
        // 6.3 若为LR型
        if ( (1 < pnode_bf) && (key > pnode->data) )
            return rotate_lr(pnode);
        // 6.4 若为RL型
        if ( (-1 > pnode_bf) && (key < pnode->data) )
            return rotate_rl(pnode);

        return pnode;
    }


    // 删除
    node *del_node2(node *pnode, int key)
    {
        if (NULL == pnode)
        {
            return pnode;
        }

        // 找 删除结点的位置
        if (key < pnode->data)
        {
            pnode->lc = del_node2(pnode->lc, key);
        }
        else if (key > pnode->data)
        {
            pnode->rc = del_node2(pnode->rc, key);
        }
        else
        {
            // 找到删除结点，

            // 3.1 若删除结点同时存在左右孩子
            if ( (NULL != pnode->lc) && (NULL != pnode->rc) )
            {
                // 需要从左右子树中最高的那棵树开始删除

                // 从左子树中开始删除
                if (height(pnode->lc) > height(pnode->rc))
                {
                    // 找到左子树中最大的结点，替换当前结点
                    node *pre_node = get_max(pnode->lc);

                    // 将当前结点的值替换为前驱结点的值
                    pnode->data = pre_node->data;

                    // 删除左子树中最大的结点
                    pnode->lc = del_node2(pnode->lc, pnode->data);
                }

                // 删除的结点从右子树中开始找
                else
                {
                    // 找到后继结点
                    node *successor_node = get_min(pnode->rc);

                    // 将删除结点的值赋值给当前结点
                    pnode->data = successor_node->data;

                    // 删除后继结点中指定结点的值
                    pnode->rc = del_node2(pnode->rc, pnode->data);
                }
                
            }

            // 3.2 若只有左孩子
            else if ((NULL != pnode->lc) && (NULL == pnode->rc) )
            {
                // 指向需要删除的结点
                node *del_node_tmp = pnode->lc;

                // 将孩子的值赋值给删除结点
                pnode->data = del_node_tmp->data;
                // 将父节点指向孩子结点的分支设置为NULL
                pnode->lc = NULL;

                delete del_node_tmp;
                del_node_tmp = NULL;
            }
            // 3.3 若只有右孩子
            else if ( (NULL == pnode->lc) && (NULL != pnode->rc))
            {
                // 指向需要删除的结点
                node *del_node_tmp = pnode->rc;
                pnode->data = del_node_tmp->data;
                // 将分支设置为NULL
                del_node_tmp->rc = NULL;

                delete del_node_tmp;
                del_node_tmp = NULL;
            }
            // 3.4 删除的是叶子结点
            else
            {

                node *pnode_pa = get_parent_node(pnode);
                if (NULL != pnode_pa)
                {
                    if (pnode == pnode_pa->lc)
                        pnode_pa->lc = NULL;
                    else if (pnode == pnode_pa->rc)
                        pnode_pa->rc = NULL;
                }
                // 说明只有根结点
                else
                    root = NULL;
                    

                delete pnode;
                pnode = NULL;
            }
        }

        // 为了保证树的平衡
        if (NULL == pnode)
            return pnode;

        // ------- 删除节点后，需要做平衡
        pnode->height = max(height(pnode->lc), height(pnode->rc)) + 1;

        // 当前结点的平衡因子
        int pnode_bf    = get_balance_factor(pnode);

        // LL型
        if ( (1 < pnode_bf) && (0 <= get_balance_factor(pnode->lc)) )
        {
            return rotate_ll(pnode);
        }
        // LR型
        if ( (1 < pnode_bf) && (0 > get_balance_factor(pnode->lc)) )
        {
            return rotate_lr(pnode);
        }
        // RR型
        if ( (-1 > pnode_bf) && (0 >= get_balance_factor(pnode->rc)) )
        {
            return rotate_rr(pnode);
        }
        
        // RL型
        if ( (-1 > pnode_bf) && (0 < get_balance_factor(pnode->rc)) )
        {
            return rotate_rl(pnode);
        }

        return pnode;
    }


private:
    node *root;
};




int main()
{
    avltree2 tree;
    int insert_key = 0;
    cout << "插入开始，结束插入 输入-1" << endl << endl << endl;
    while (1)
    {
        cout << "请输入插入值:";
        cin >> insert_key;
        if (-1 == insert_key)
            break;
        tree.insert(insert_key);    
        cout << endl <<  "插入后，中序遍历结果:";
        tree.in_order_traverse();

        cout << endl << endl;
    }

    cout << "前序遍历:";
    tree.pre_order_traverse();
    cout << endl;

    cout << "========================================" << endl;
    cout << "开始删除, 请输入删除元素的值, 结束输入-1" << endl << endl;
    int del_key = 0;
    while (1)
    {
        cout << "请输入删除结点值: ";
        cin >> del_key;
        if (-1 == del_key)
            break;
    
        tree.remove_node(del_key);
        cout << "删除后，中序遍历:";
        tree.in_order_traverse();  
        cout << endl <<  "删除后，前序遍历:";
        tree.pre_order_traverse();
        cout << endl << endl;
    }


    cout << endl << endl << endl << "----------------------------" << endl << endl;
    cout << "删除树" << endl;
    tree.remove();


    return 0;
}

```



