
# 自己实现vector动态数组（c++）
	##仅实现了部分功能， 后续更新##
	## 欢迎指正##	

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

#### 1.二叉树
##### 1.1 初步实现二叉树的基本功能，包括：添加结点、查找结点、3中遍历结点、释放树
#### 1.2 更新时间:2019-3-13 22:21 更新部分功能：
		·查找某个结点的父节点
		·查找最小值与最大值
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

        remove_node(root, del_data);
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

    // 查找某个结点为根节点的最结点

private:

    void remove_node(node *proot, int del_data)
    {
        node *find_node = query(del_data);
        node *del_node = NULL;

        if(NULL == find_node)
        {
            cout << "delete failed, may root is null or " << del_data << " not exist" << endl;
            return;
        }

        // 1、若删除结点是叶子结点, 直接删除叶子结点，并置为空
        if ( (NULL == proot->lc) && (NULL == proot->rc) )
            del_node = proot;

        // 2、当前删除结点仅包含一个子节点（左子结点或者右子结点）
        else if (    (( NULL != proot->lc) && (NULL == proot->rc)) || 
                    (( NULL == proot->lc) && (NULL == proot->rc)) 
                )
        {
            // 2.1 仅有左子结点
            if (( NULL != proot->lc) && (NULL == proot->rc))
            {

            }

            // 2.2 仅有右子结点
            else if (( NULL == proot->lc) && (NULL == proot->rc))
            {

            }
        }

    }


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



