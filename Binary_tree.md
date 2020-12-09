二叉树遍历框架：
```c++
/*基本的二叉树节点*/
class TreeNode
{
int val;
TreeNode left, right;
}
void traverse(TreeNode root)
{  //前序
traverse(root->left);
// 中序
traverse(root->right);
// 后序
}
```

⼆叉树框架可以扩展为 N 叉树的遍历框架：
```c++
/*基本的n叉树节点*/
class TreeNode
{
int val;
TreeNode[] children;
}

void traverse(TreeNode root)
{
for (treenode child: root->children)
traverse(child);
}
```

## high frequency
二叉树中序(前序 中序 后序 只是 pushback 位置不同)  遍历代码：
#### 递归
```c++
class Solution {
public:
	vector<int> inorderTraversal(TreeNode* root)
	{
	vector<int> res;
	inorder(root, res);
	return res;
	}

	void inorder(TreeNode* root, vector<int>& res)
	{
		if (!root) return;
		inorder(root->left, res);
		res.push_back(root->val);
		inorder(root->right, res);
	}
};
```
#### 迭代
链接：https://leetcode-cn.com/problems/binary-tree-postorder-traversal/solution/bang-ni-dui-er-cha-shu-bu-zai-mi-mang-che-di-chi-t/
```c++
// 前序遍历 和 后序遍历相似
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        stack<TreeNode*> st;
        vector<int> result;
        if (root == NULL) return result;
        st.push(root);
        while (!st.empty()) {
            TreeNode* node = st.top();                       // 中
            st.pop();
            result.push_back(node->val);
            if (node->right) st.push(node->right);           // 右（空节点不入栈）
            if (node->left) st.push(node->left);             // 左（空节点不入栈）
        }
        return result;
    }
};
// 后续遍历
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        stack<TreeNode*> st;
        vector<int> result;
        if (root == NULL) return result;
        st.push(root);
        while (!st.empty()) {
            TreeNode* node = st.top();
            st.pop();
            result.push_back(node->val);
            if (node->left) st.push(node->left); // 相对于前序遍历，这更改一下入栈顺序 （空节点不入栈）
            if (node->right) st.push(node->right); // 空节点不入栈
        }
        reverse(result.begin(), result.end()); // 将结果反转之后就是左右中的顺序了
        return result;
    }
};

// 中序遍历
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> result;
        stack<TreeNode*> st;
        TreeNode* cur = root;
        while (cur != NULL || !st.empty()) {
            if (cur != NULL) { // 指针来访问节点，访问到最底层
                st.push(cur); // 将访问的节点放进栈
                cur = cur->left;                // 左
            } else {
                cur = st.top(); // 从栈里弹出的数据，就是要处理的数据（放进result数组里的数据）
                st.pop();
                result.push_back(cur->val);     // 中
                cur = cur->right;               // 右
            }
        }
        return result;
    }
};
```
二叉树深度：
```c++
// 递归 
int maxDepth(TreeNode* root)
{
if (root == NULL) return  0;
return max( maxDepth(root->left), maxDepth(root->right)) + 1;
}
```
翻转二叉树
```c
// 命名两个指针 然后改变指针位置
TreeNode* invertTree(TreeNode* root) {
	if(root == NULL) return NULL;
	TreeNode* r = invertTree(root->left);
	TreeNode* l = invertTree(root->right);
	root->left =l;
	root->right =r;
	return root;
}
```
二叉树层序遍历
```c ++
vector<vector<int>> levelOrder(TreeNode* root) {
	// 入栈
	queue<TreeNode*> que;
	if (root != NULL) que.push(root);
	vector<vector<int>> result;
	// 开始遍历
	while (!que.empty()) 
	{
		int size = que.size(); 
		vector<int> vec;
		for (int i = 0; i < size; i++) {
			// 读取队首的值
			TreeNode* node = que.front();
			vec.push_back(node->val);
			// 出队列后，压入左右子树的值
			que.pop();
	        if (node->left) que.push(node->left);
	        if (node->right) que.push(node->right);
		}
		result.push_back(vec);
	}
	
	return result;
}

// 返回每行最后一个值
class Solution {
public:
    vector<int> rightSideView(TreeNode* root) {
        // 返回所有最右边的数值
        //  返回层序遍历每行的最后一个值
        queue<TreeNode*> que;
        if(root) que.push(root); // 教训，这里root 不为空要写作if（root）  
        vector<int> result;
        while(!que.empty())
        {
            int size = que.size(); // size 是不断变化的
            for(int i =0; i<size; i++)
            {
                TreeNode* node = que.front();
                que.pop();
                if(i == (size-1)){
                    result.push_back(node->val); 
                }
                if (node->left) que.push(node->left);
                if (node->right) que.push(node->right);       
            }
        }
        return result;
    }
};
```
合并二叉树
```c++
class Solution {
public:
    TreeNode* mergeTrees(TreeNode* t1, TreeNode* t2) {
    // 这是我写的 看能不能简化
        if(t1 == NULL) return t2;
        if(t2 == NULL) return t1;
        t1->val = t1->val + t2->val;
        TreeNode* l = mergeTrees(t1->left, t2->left);
        TreeNode* r = mergeTrees(t1->right,t2->right);
        t1->left = l;
        t1->right = r;
        return t1;
    }
};
```

二叉树直径 
```c++
// 就是将二叉树拉直了看看多宽
class Solution {
public:
    int maxdepth =0;
    int diameterOfBinaryTree(TreeNode* temp) {
        if (temp == NULL) return 0;
        // 就是将二叉树拉直了看看多宽 = 左子树最大深度 + 右子树最大深度 
        //  会出现 最大深度在同一子树的情况，所以要进行迭代
        deepnode(temp);
        return maxdepth ;

    }
//这个题教训就是一定尽量储存递归计算的变量，防止多次重复计算
    int deepnode(TreeNode* root){
        if (root == NULL) return 0;
        int l = deepnode(root->left);
        int r = deepnode(root->right);
        maxdepth = max(l+r,maxdepth);
        return max(l,r)+1;
    }
};
```

二叉树展开为链表 （画图思考）
https://leetcode-cn.com/problems/flatten-binary-tree-to-linked-list/solution/xiang-xi-tong-su-de-si-lu-fen-xi-duo-jie-fa-by--26/
```c++
class Solution {
public:
    void flatten(TreeNode* root) {
        if (root == nullptr) return;
        TreeNode * temp = root->right;//save
        root->right = root->left; //move
        root->left = nullptr;// 没通过是因为这里没设置地址？
        flatten(root->right); // 循环
        while(root->right != nullptr) // conncect
        {
            root = root->right;
        } // 遍历到结尾
        root->right = temp;
        flatten(temp);
    }
};  
```

从前序与中序遍历序列构造二叉树
```c++
```
判断路径总和
```c++
class Solution {
public:
    bool hasPathSum(TreeNode* root, int sum) {
        //根节点到叶子节点的路径
        // 经过一个节点 - val
        // 到叶子结点时，判断0，相等则返回；没有则返回fasle
        if(!root) return false;
        int temp = sum - root->val;
        if(root->left==NULL && root->right==NULL){
            if(temp==0) return true;
            else return false;
        }
        return (hasPathSum(root->left, temp) || hasPathSum(root->right, temp));  
    }
};
```
二叉树所有路径
```c
// 命名两个指针 然后改变指针位置
class Solution {
public:
    vector<string> str;
    string path = "->";
    vector<string> binaryTreePaths(TreeNode* root) {
        // 深度优先遍历 要改变形式
        // 从根出发，遇到节点便记录，变成一个string  push back
        // 教训  先写思路
        string temp;
        preorder(root, temp);
        return str;
    }
    void preorder(TreeNode* root, string temp){  //添加参数思路：如果遇到一个随着递归不断变换的变量，则当做参数传递
        if(!root) return ;
        if((!root->left) && (!root->right)) {
            temp = temp+to_string(root->val) ;  // 遇到叶子结点 只+节点值,
            str.push_back(temp);
        }
        else temp = temp + to_string(root->val)+ path;    // 储存当前路线   

        preorder(root->left, temp);
        preorder(root->right,temp);
    }
};
```

递归三部曲
确定递归函数的参数和返回值：
确定哪些参数是递归的过程中需要处理的，那么就在递归函数里加上这个参数， 并且还要明确每次递归的返回值是什么进而确定递归函数的返回类型。

确定终止条件：
写完了递归算法, 运行的时候，经常会遇到栈溢出的错误，就是没写终止条件或者终止条件写的不对，操作系统也是用一个栈的结构来保存每一层递归的信息，如果递归没有终止，操作系统的内存栈必然就会溢出。

确定单层递归的逻辑：
确定每一层递归需要处理的信息。在这里也就会重复调用自己来实现递归的过程。

```c++

```

## low frequency
叶子结点个数
```c++
 int diameterOfBinaryTree(TreeNode* temp) {
        // 有几个叶子结点  
        int i = leafnode(temp);
        return i>>1;
    }
   int leafnode(TreeNode* root){
       if (root == NULL) return 1;
       int l = leafnode(root->left);
       int r = leafnode(root->right);
       int pa = (l+r);
       return pa;
   }
```
 [https://mp.weixin.qq.com/s/-ZJn3jJVdF683ap90yIj4Q](https://mp.weixin.qq.com/s/-ZJn3jJVdF683ap90yIj4Q)
