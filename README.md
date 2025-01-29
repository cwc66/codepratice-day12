# codepratice-day12
代码随想录第12次

二叉树第二次练习。

二叉树的递归、迭代。

[翻转二叉树](https://leetcode.cn/problems/invert-binary-tree/description/)
翻转一颗二叉树
将每一个节点的左右孩子交换。
前序或者后序遍历可行，而中序遍历会将某些节点的左右孩子翻转两次，可以画图理解。

递归法
1.确定递归函数的参数和返回值
参数为传入节点的指针，不需要其它。返回值也不需要，但需要返回根节点。

```CPP
TreeNode* invertTree(TreeNode* root)
```

2.确定终止条件
节点为空时，返回
```CPP
if(root==nullptr) return root;
```

3.确定单层递归的逻辑
前序遍历，先进行交换左右孩子节点，翻转左子树，翻转右子树
```CPP
swap(root->left,root->right);
invertTree(root->left);
invertTree(root->right);
```

递归三步法完成，对应代码为：
```CPP
class Solution{
public:
    TreeNode* invertTree(TreeNode* root)
    {
        if(root==nullptr) return root;
        swap(root->left,root->right);
        invertTree(root->left);
        invertTree(root->right);
        return root;
    }
};
```

迭代法(前序遍历)：
```CPP
class Solution{
public:
    TreeNode* invertTree(TreeNode* root)
    {
        if(root==nullptr) return root;
        stack<TreeNode*> st;
        st.push(root);
        while(!st.empty())
        {
            TreeNode* node=st.top();
            st.pop();
            swap(node->left,node->right);
            if(node->right) st.push(node->right);//中右左，因为是栈存储
            if(node->left) st.push(node->left);
        }
        return root;
    }
}
```

迭代模板的统一写法（同样是前序遍历）,空指针标记法：
```CPP
class Solution
{
public:
    TreeNode* invertTree(TreeNode* root)
    {
        stack<TreeNode*> st;
        if(root!=nullptr) st.push(root);
        while(!st.empty())
        {
            TreeNode* node=st.top();
            if(node!=nullptr)
            {
                st.pop();
                if(node->right) st.push(node->right);
                if(node->left) st.push(node->left);
                st.push(node);
                st.push(nullptr);
            }
            else
            {
                st.pop()l
                node=st.top();
                st.pop();
                swap(node->left,node->right);
            }
        }
        return root;
    }
}
```

广度优先遍历：
```CPP
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        queue<TreeNode*> que;
        if (root != NULL) que.push(root);
        while (!que.empty()) {
            int size = que.size();
            for (int i = 0; i < size; i++) {
                TreeNode* node = que.front();
                que.pop();
                swap(node->left, node->right); // 节点处理
                if (node->left) que.push(node->left);
                if (node->right) que.push(node->right);
            }
        }
        return root;
    }
};
```

[对称二叉树](https://leetcode.cn/problems/symmetric-tree/description/)
判断二叉树是否镜像对称的。
比较根节点的左右子树是不是相互翻转的。递归遍历时，同时遍历两棵树，比较两棵树。两个子树的里侧和外侧是否相等。
使用后序遍历。中左右，因为要用递归函数的返回值判断 两棵树的内侧和外侧节点。一棵树的遍历顺序是左右中，另一个是右左中。

递归法
1.确定递归函数的参数和返回值
比较的是两个树，因此参数应该是左子树和右子树节点，返回bool
```CPP
bool compare(TreeNode* left,TreeNode* right)
```

2.确定终止条件
要比较两个节点数值是否相同，首先要把两个节点为空的情况弄清楚。
左为空，右不为空 -> 不对称
左不为空，右为空 -> 不对称
左右都空 -> 对称

以上情况排除节点为空，之后是节点不空
不为空时，比较节点数值。

最后为节点不空，且数值相同。

3.确定单层递归的逻辑。
单层递归的逻辑是处理 左右节点都不为空，且数值相同的情况。

比较外侧是否对称->传入左节点的左孩子，右节点的右孩子
比较内侧是否对称->传入左节点的右孩子，右节点的左孩子
如果左右都对称 返回true， 有不对称的一侧，返回false


```CPP
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    bool compare(TreeNode* left,TreeNode* right)
    {
        if(left==NULL&&right!=NULL) return false;
        else if(left!=NULL&&right==NULL) return false;
        else if(left==NULL&&right==NULL) return true;
        //排除空节点，然后排除数值不相同的情况.
        else if(left->val!=right->val) return false;

        //此时，左右节点都不为空，且数值相同的情况
        //此时做递归，做下一层的判断
        bool outside=compare(left->left,right->right);
        bool inside=compare(left->right,right->left);
        bool isSame=outside&&inside;
        return isSame;
    }
    bool isSymmetric(TreeNode* root) {
        if(root==NULL) return true;
        return compare(root->left,root->right);
    }
};
```

简洁
```CPP
class Solution {
public:
    bool compare(TreeNode* left, TreeNode* right) {
        if (left == NULL && right != NULL) return false;
        else if (left != NULL && right == NULL) return false;
        else if (left == NULL && right == NULL) return true;
        else if (left->val != right->val) return false;
        else return compare(left->left, right->right) && compare(left->right, right->left);

    }
    bool isSymmetric(TreeNode* root) {
        if (root == NULL) return true;
        return compare(root->left, root->right);
    }
};
```


迭代法
不是前中后序，用队列比较两个树是否相互翻转

```CPP
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        if(root==nullptr) return true;
        queue<TreeNode*> que;
        que.push(root->left);//将左子树头节点加入队列
        que.push(root->right);//将右子树头节点加入队列

        while(!que.empty())
        {
            //接下来就要判断这两个树是否相互翻转
            TreeNode* leftNode=que.front();que.pop();
            TreeNode* rightNode=que.front();que.pop();
            if(!leftNode && !rightNode)
            {
                //左节点为空、右节点为空，说明此时是对称的
                continue;
            }

            //左右一个节点不为空，或者都不为空但是数值不相同，返回false
            if((!leftNode||!rightNode||(leftNode->val!=rightNode->val)))
            {
                return false;
            }
            que.push(leftNode->left);//加入左节点左孩子
            que.push(rightNode->right);//加入右节点右孩子
            que.push(leftNode->right);//加入左节点右孩子
            que.push(rightNode->left);//加入右节点左孩子

        }
        return true;
    }
};  
```


对称的树这道题，修改一下，可以用在以下两题。

[相同的树](https://leetcode.cn/problems/same-tree/)
遍历时，比较左右孩子节点迭代的结果是否相等。
```CPP
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    bool compare(TreeNode* p,TreeNode* q)
    {
        if(p==nullptr&&q!=nullptr) return false;
        else if(p!=nullptr&&q==nullptr) return false;
        else if(p==nullptr&&q==nullptr) return true;
        else if(p->val!=q->val) return false;
        //剩下就是p,q不为空且数值相等情况。
        else return compare(p->left,q->left)&&compare(p->right,q->right);
        
    }
    bool isSameTree(TreeNode* p, TreeNode* q) {
        return compare(p,q);
    }
};

```

[另一棵树的子树](https://leetcode.cn/problems/subtree-of-another-tree/)
这道题不一样，在于它需要在每个节点去判断，是否以当前节点的子树，是否和目标树相同，以一个check判断两棵树，以dfs函数进入树和左右孩子迭代判断。

深度优先搜索枚举s中的每一个节点，判断这个点的子树是否和t相等。
```CPP
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    bool check(TreeNode* o,TreeNode* t)
    {
        if(!o&&!t)
        {
            return true;
        }
        if((o&&!t)||(!o&&t)||(o->val!=t->val))
        {
            return false;
        }
        return check(o->left,t->left) && check(o->right,t->right);

    }
    bool dfs(TreeNode* o,TreeNode* t)
    {
        if(!o)
        {
            return false;
        }
        return check(o,t)||dfs(o->left,t)||dfs(o->right,t);
    }
    bool isSubtree(TreeNode* root, TreeNode* subRoot) {
        return dfs(root,subRoot);
    }
};
```

[二叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-binary-tree/description/)
最大深度是指 从根节点到最远叶子节点的最长路径上的节点数。

递归
前序求深度、后序求高度

深度：指从根节点到该节点的最长简单路径边的条数或节点数
高度：指从该节点到叶子节点的最长简单路径边的条数或者节点数。

根节点的高度==二叉树的最大深度 

后序遍历
1.确定递归函数的参数和返回值
参数就是传入树的根节点，返回就返回这棵树的深度，所以返回值为int类型。

2.确定终止条件
如果为空节点的话，就返回0，表示高度为0。

3.确定单层递归的逻辑
先求它的左子树的深度，再求右子树的深度，最后取左右深度最大的数值 再+1 （加1是因为算上当前中间节点）就是目前节点为根节点的树的深度。


```CPP
class Solution {
public:
    int getdepth(TreeNode* node) {
        if (node == NULL) return 0;
        int leftdepth = getdepth(node->left);       // 左
        int rightdepth = getdepth(node->right);     // 右
        int depth = 1 + max(leftdepth, rightdepth); // 中
        return depth;
    }
    int maxDepth(TreeNode* root) {
        return getdepth(root);
    }
};
```

精简
```CPP
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if (root == null) return 0;
        return 1 + max(maxDepth(root->left), maxDepth(root->right));
    }
};

```
前序遍历方式
```CPP
class Solution {
public:
    int result;
    void getdepth(TreeNode* node, int depth) {
        result = depth > result ? depth : result; // 中

        if (node->left == NULL && node->right == NULL) return ;

        if (node->left) { // 左
            depth++;    // 深度+1
            getdepth(node->left, depth);
            depth--;    // 回溯，深度-1
        }
        if (node->right) { // 右
            depth++;    // 深度+1
            getdepth(node->right, depth);
            depth--;    // 回溯，深度-1
        }
        return ;
    }
    int maxDepth(TreeNode* root) {
        result = 0;
        if (root == NULL) return result;
        getdepth(root, 1);
        return result;
    }
};
```
简化
```CPP
class Solution {
public:
    int result;
    void getdepth(TreeNode* node, int depth) {
        result = depth > result ? depth : result; // 中
        if (node->left == NULL && node->right == NULL) return ;
        if (node->left) { // 左
            getdepth(node->left, depth + 1);
        }
        if (node->right) { // 右
            getdepth(node->right, depth + 1);
        }
        return ;
    }
    int maxDepth(TreeNode* root) {
        result = 0;
        if (root == 0) return result;
        getdepth(root, 1);
        return result;
    }
};
```


迭代做法（模板修改）：
```CPP
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if (root == NULL) return 0;
        int depth = 0;
        queue<TreeNode*> que;
        que.push(root);
        while(!que.empty()) {
            int size = que.size();
            depth++; // 记录深度
            for (int i = 0; i < size; i++) {
                TreeNode* node = que.front();
                que.pop();
                if (node->left) que.push(node->left);
                if (node->right) que.push(node->right);
            }
        }
        return depth;
    }
};
```

[N叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-n-ary-tree/description/)
思路与上题相同

递归法
```CPP
class Solution {
public:
    int maxDepth(Node* root) {
        if (root == 0) return 0;
        int depth = 0;
        for (int i = 0; i < root->children.size(); i++) {
            depth = max (depth, maxDepth(root->children[i]));
        }
        return depth + 1;
    }
};
```

迭代法
```CPP
class Solution {
public:
    int maxDepth(Node* root) {
        queue<Node*> que;
        if (root != NULL) que.push(root);
        int depth = 0;
        while (!que.empty()) {
            int size = que.size();
            depth++; // 记录深度
            for (int i = 0; i < size; i++) {
                Node* node = que.front();
                que.pop();
                for (int j = 0; j < node->children.size(); j++) {
                    if (node->children[j]) que.push(node->children[j]);
                }
            }
        }
        return depth;
    }
};
```

[二叉树的最小深度](https://leetcode.cn/problems/maximum-depth-of-binary-tree/description/)
最小深度 从根节点到最近节点的最短路径上的节点数量。

后序遍历求高度，前序遍历求深度
后序求高度，最小距离同样是最小深度。
最小深度是从根节点到最近叶子节点的最短路径上的节点数量。

递归
1.确定递归函数的参数和返回值
参数为要传入的二叉树根节点，返回的是int类 型的深度。

2.确定终止条件
终止条件也是遇到空节点返回0，表示当前节点的高度为0。

3.确定单层递归的逻辑
这里的逻辑和最大深度不一样。
如果左子树为空，右子树不为空，说明最小深度是 1 + 右子树的深度。
右子树为空，左子树不为空，最小深度是 1 + 左子树的深度。
最后如果左右子树都不为空，返回左右子树深度最小值 + 1 。
```CPP
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int getDepth(TreeNode* node)
    {
        if(node==NULL) return 0;
        int leftDepth=getDepth(node->left);
        int rightDepth=getDepth(node->right);

        //求二叉树的最小深度和求二叉树的最大深度
        //差别在于处理左右孩子不为空的逻辑
        //当一个左子树为空，右不为空，这时不是最低点
        if(node->left==NULL&&node->right!=NULL)
        {
            return 1+rightDepth;
        }

        //当一个右子树为空，左不为空，这时并不是最低点
        if(node->left!=NULL&&node->right==NULL)
        {
            return 1+leftDepth;
        }

        int result=1+min(leftDepth,rightDepth);
        return result;
    }
    int minDepth(TreeNode* root) {
        return getDepth(root);
    }
};
```

简化
```CPP
class Solution {
public:
    int minDepth(TreeNode* root) {
        if (root == NULL) return 0;
        if (root->left == NULL && root->right != NULL) {
            return 1 + minDepth(root->right);
        }
        if (root->left != NULL && root->right == NULL) {
            return 1 + minDepth(root->left);
        }
        return 1 + min(minDepth(root->left), minDepth(root->right));
    }
};
```

前序遍历方式
```CPP
class Solution {
private:
    int result;
    void getdepth(TreeNode* node, int depth) {
        // 函数递归终止条件
        if (node == nullptr) {
            return;
        }
        // 中，处理逻辑：判断是不是叶子结点
        if (node -> left == nullptr && node->right == nullptr) {
            result = min(result, depth);
        }
        if (node->left) { // 左
            getdepth(node->left, depth + 1);
        }
        if (node->right) { // 右
            getdepth(node->right, depth + 1);
        }
        return ;
    }

public:
    int minDepth(TreeNode* root) {
        if (root == nullptr) {
            return 0;
        }
        result = INT_MAX;
        getdepth(root, 1);
        return result;
    }
};
```

迭代法
需要注意的是，只有当左右孩子都为空的时候，才说明遍历到最低点了。如果其中一个孩子不为空则不是最低点
```CPP
class Solution {
public:

    int minDepth(TreeNode* root) {
        if (root == NULL) return 0;
        int depth = 0;
        queue<TreeNode*> que;
        que.push(root);
        while(!que.empty()) {
            int size = que.size();
            depth++; // 记录最小深度
            for (int i = 0; i < size; i++) {
                TreeNode* node = que.front();
                que.pop();
                if (node->left) que.push(node->left);
                if (node->right) que.push(node->right);
                if (!node->left && !node->right) { // 当左右孩子都为空的时候，说明是最低点的一层了，退出
                    return depth;
                }
            }
        }
        return depth;
    }
};
```

这部分的内容很多，同一道题的解法多，需要多复习。
