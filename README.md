
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
```c++
	
	#include <iostream>
	using namespace std;
	struct node
	{
		// 数据区
		int data;

    	// 左节点
    	node *lc;

    	// 右节点
    	node *rc;

    	node ():data(0), lc(nullptr), rc(nullptr){}
	};


	// 二叉树
	class btree	
	{
	public:
		// 构造函数
		btree():root(nullptr), size(0){}
		virtual ~ btree(){}
		
		// 插入节点。 data 为插入值
		void insert(int data)
		{
			root = insert_node(root, data);
		}

		// 删除整棵树
		void remove()
		{
			cout << "remove " << endl;
			remove_node(root);
		}
		
		// 前序遍历(先序遍历)
		void pre_order_traverse()
		{
			cout << "pre_order_traverse" << endl;
			pre_order_traverse_node(root);
			cout << endl << endl;
		}

		// 中序遍历
		void in_order_traverse()
		{
			cout << "in_order_traverse" << endl;
			in_order_traverse_node(root);
			cout << endl << endl;
		}	
		
		// 后续遍历
		void post_order_traverse()
		{
			cout << "post_order_traverse" << endl;
			post_order_traverse_node(root);
			cout << endl << endl;
		}
		
		// 获取当前树的节点数
		int get_size()
		{
			return size;
		}

		// 递归查找大于data的最小节点
		node * get_min_ceiling(int data)
		{
			return get_min_ceiling_node(root, data);
		}

		// 非递归查找大于data的最小节点
		node *find_min_ceiling(int key) const
		{
			if (nullptr == root)
			{
				cout << "find ceiling is error, root is null" << endl;
				return nullptr;
			}
			node *cur_node = root;
			node *parent_node = nullptr;
			
			while (cur_node)
			{
				if (cur_node->data <= key)
					cur_node = cur_node->rc;
				else
				{
					parent_node = cur_node;
					cur_node = cur_node->lc;
				}
			}
		
			return parent_node;
		}
		
	private:
		void add_size()
		{
			if (32767 < size)
				return;
			size++;
		}
		
		void dec_size()
		{
			if (0 == size)
				return;
			size--;
		}

		node* insert_node(node *proot, int data)
		{
			if ( (nullptr == proot) || (NULL == proot))
			{
				proot = new node;
				if ( (nullptr != proot ) && (NULL != proot))
				{
					proot->data = data;
					add_size();
				}
			}

			// 插入的值比当前节点值大
			else if (data > proot->data)
				proot->rc = insert_node(proot->rc, data);
			else if (data < proot->data)
				proot->lc = insert_node(proot->lc, data);

			return proot;
		}

		void remove_node(node *proot)
		{
			if ( (nullptr == proot) || (NULL == proot))
				return;
				
			remove_node(proot->lc);
			remove_node(proot->rc);
			delete proot;
			dec_size();

			if ( 0 == get_size())		
				root = nullptr;
		}
		
		void pre_order_traverse_node(node *proot)
		{
			if ((nullptr == proot) || (NULL == proot))
				return;
			
			cout << proot->data << ",  ";
			pre_order_traverse_node(proot->lc);
			pre_order_traverse_node(proot->rc);
		}

		void in_order_traverse_node(node *proot)
		{
			if ((nullptr == proot) || (NULL == proot))
				return;
			
			in_order_traverse_node(proot->lc);
			cout << proot->data << ",   ";
			in_order_traverse_node(proot->rc);
		}
		
		void post_order_traverse_node(node *proot)
		{
			if ((nullptr == proot) || (NULL == proot))
				return;
			
			post_order_traverse_node(proot->lc);
			post_order_traverse_node(proot->rc);
			cout << proot->data << ",   ";

		}
		
		node* get_min_ceiling_node(node *proot, int data)
		{
			if(nullptr == proot)
				return nullptr;

			if ( proot->data <= data)
				return get_min_ceiling_node(proot->rc, data); 
			else 
			{
				node *tmp_node = get_min_ceiling_node(proot->lc, data);
				return tmp_node ? tmp_node : proot;
			}            
		}

	private:
		node *root;
		int size;
	};

	int main()
	{
		btree tree;
		tree.insert(8);
		tree.insert(6);
		tree.insert(4);
		tree.insert(2);
		tree.insert(0);
		tree.insert(-2);
		
		tree.pre_order_traverse();
		tree.in_order_traverse();
		tree.post_order_traverse();

		int ceil = 0;
		cout << "input ceiling num = ";
		cin >> ceil;
		node *tmp   = nullptr;
		tmp         = tree.find_min_ceiling(ceil);
		cout << "get ceiling " << ceil << " is = ";
	
		if (nullptr == tmp)
			cout << "null" << endl;
		else
			cout << tmp->data << endl;
		
		tree.remove();

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
```c++



