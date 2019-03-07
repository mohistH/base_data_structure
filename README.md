# base_data_structure
	记录常用的数据结构:
		单向链表
		双向链表
		栈
		队列
		二叉树
		平衡二叉树（AVL树）
		红黑树
		
		c++实现
###`欢迎指正`

#### 1.二叉树
`
	
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
`



