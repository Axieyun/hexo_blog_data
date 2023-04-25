---
title: AVL树
tags: 高级数据结构
abbrlink: d609
date: 2022-02-20 19:43:41
password:
---



### AVL

* 平衡二叉搜索树

#### 性质

* 完美继承二叉搜索树的性质
* |H(left) - H(right)| <= 1



#### 优点

* 由于对每个节点的左右子树的树高做了限制，所以整棵树不会退化为一个链表
* 继承了二叉搜索树的优点
* 高度平衡的二叉搜索树，查找效率非常高O(log1.68 h) ~ O(log 2 h)

#### 缺点

* 为了维持高度的平衡，每次插入、删除都要进行树高的调整，比较复杂、耗时。
* 对于频繁的插入删除操作，使用AVL树代价高





#### 操作

* LL、LR、RL、RR型条件的不平衡
* LL与RR型只用进行一次调整
* 其余需要进行两次调整







#### 结构定义

~~~c
//结构定义
typedef struct Node {
	int value, h;
	struct Node *left, *right;
}Node;
~~~









#### 更新树高

~~~c
//更新树高
void update_h(Node *root) {
	if (root == NIL) return ;
	root->h = max(root->left->h, root->right->h) + 1;
	return ;
}
~~~





#### 左旋

~~~c
//左旋
Node *left_rotate(Node *root) {
	//printf("\n left rotate : %d\n", root->value);
	Node *p = root->right;
	root->right = p->left;
	p->left = root;
	update_h(root);
	update_h(p);
	return p;
}
~~~





#### 右旋



~~~c
//右旋
Node *right_rotate(Node *root) {
	//printf("\n right rotate : %d\n", root->value);
	Node *p = root->left;
	root->left = p->right;
	p->right = root;

	update_h(root);
	update_h(p);
	return p;
}
~~~







### 完整代码



~~~c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>


#define max(a, b) (\
	(a) > (b) ? (a) : (b)\
)



//结构定义
typedef struct Node {
	int value, h;
	struct Node *left, *right;
}Node;

Node __NIL;
#define NIL (&__NIL)
__attribute__((constructor))
void init_NIL() {
	NIL->h = 0;
	NIL->value = -1;
	NIL->left = NIL->right = NIL;
}


//结构操作
Node *getNewNode(int target) {
	Node *p = (Node *)malloc(sizeof(Node));
	p->h = 1;
	p->left = p->right = NIL;
	p->value = target;
	return p;
}

void clear(Node *root) {
	if (root == NIL) return ;
	clear(root->left);
	clear(root->right);
	free(root);
	return ;
}

int search(Node *root, int target) {
	if (root == NIL) return 0;
	if (root->value == target) return 1;
	if (root->value > target) {
		return search(root->left, target);
	}
	return search(root->right, target);
}


//更新树高
void update_h(Node *root) {
	if (root == NIL) return ;
	root->h = max(root->left->h, root->right->h) + 1;
	return ;
}

//左旋
Node *left_rotate(Node *root) {
	printf("\n left rotate : %d\n", root->value);
	Node *p = root->right;
	root->right = p->left;
	p->left = root;

	update_h(root);
	update_h(p);
	return p;
}

//右旋
Node *right_rotate(Node *root) {
	printf("\n right rotate : %d\n", root->value);
	Node *p = root->left;
	root->left = p->right;
	p->right = root;

	update_h(root);
	update_h(p);
	return p;
}
const char *maintain_str[] = {"", "LL", "LR", "RL", "RR"};

Node *maintain(Node *root) {
	//判断是否失衡
	if (abs(root->left->h - root->right->h) <= 1) return root;
	
	int maintain_type = 0;
	// 失衡
	if (root->left->h > root->right->h) { //֤左子树高，失衡条件L
		if (root->left->right->h > root->left->left->h) { //LR
			// LR 和 RL 都要旋转两次 
			root->left = left_rotate(root->left); 
			maintain_type = 2;
		} else {
			maintain_type = 1;			
		}
		root = right_rotate(root); 
		
	} else { //右子树高，失衡条件 R 
		
		if (root->right->left->h > root->right->right->h) { 
			root->right = right_rotate(root->right); 
			maintain_type = 3;
		} else {	
			maintain_type = 4;			
		}
		root = left_rotate(root); 
		
	}
	printf("AVL maintain_type : %s\n", maintain_str[maintain_type]);
	return root;
} 

Node *insert(Node *root, int target) {
	if (root == NIL) return getNewNode(target);
	if (root->value == target) return root;
	if (root->value > target) {
		root->left = insert(root->left, target);
	} else {
		root->right = insert(root->right, target);
	}
	update_h(root);
	
	
	
	return maintain(root);
}


Node *get_pre(Node *root) {
	Node *p = root;
	while (p->right != NIL) {
		p = p->right;
	}
	return p;
}

Node *erase(Node *root, int target) {
	if (root == NIL) return NIL;
	if (root->value > target) {
		root->left = erase(root->left, target);
	} else if (root->value < target) {
		root->right = erase(root->right, target);
	} else {
		if (root->left == NIL || root->right == NIL) {
			Node *tmp = (root->left != NIL ? root->left : root->right);
			free(root);
			return tmp;
		} else {
			Node *pre = get_pre(root->left);
			root->value = pre->value;
			root->left = erase(root->left, pre->value); 
		}
	}
	update_h(root);  
	 
	return maintain(root);
}

void in_order(Node *root) {
	if (root == NIL) return ;
	in_order(root->left);
	printf("%d ", root->value);
	in_order(root->right);
	return ;
}

void outputTree(Node *root) {
	if (root == NIL) return ;
	printf("(%d(%d) | %d, %d)\n", root->value, root->h, root->left->value, root->right->value);
	outputTree(root->left);
	outputTree(root->right);
	return ;
}


int main() {
	srand(time(NULL));
	Node *root = NIL;
	int n;
	scanf("%d", &n);
	for (int i = 0; i < n; ++i) {
		int val = rand() % 100;
		printf("\ninsert %d to tree: \n", val);
		root = insert(root, val);
		outputTree(root);
	}
	
	
	while (scanf("%d", &n) != EOF) {
		root = erase(root, n);
		//in_order(root);
		outputTree(root);
	}
	
	clear(root);
	
	return 0;
}

~~~









#### [剑指 Offer 26. 树的子结构](https://leetcode-cn.com/problems/shu-de-zi-jie-gou-lcof/)



输入两棵二叉树A和B，判断B是不是A的子结构。(约定空树不是任意一个树的子结构)

B是A的子结构， 即 A中有出现和B相同的结构和节点值。





~~~c
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     struct TreeNode *left;
 *     struct TreeNode *right;
 * };
 */

// bool flag = false;

// bool func1(struct TreeNode *A, struct TreeNode *B) {
//     if (B == NULL) return true;
//     if (A == NULL) return false;
//     if (A->val != B->val) return false;
//     return flag = (func1(A->left, B->left) && func1(A->right, B->right));
// }

// void func(struct TreeNode *A, struct TreeNode *B) {
//     if (A == NULL) return ;
//     if (flag) return ;
//     if (A->val == B->val) {
//         func1(A, B);
//     }
//     func(A->left, B);
//     func(A->right, B);
//     return ;
// }

bool is_match(struct TreeNode *A, struct TreeNode *B) {
    if (B == NULL) return true;
    if (A == NULL) return false;
    if (A->val != B->val) return false;
    return is_match(A->left, B->left) & is_match(A->right, B->right);
}

bool isSubStructure(struct TreeNode* A, struct TreeNode* B) {
    if (B == NULL || A == NULL) return false;
    if (is_match(A, B)) return true;
    return isSubStructure(A->left, B) || isSubStructure(A->right, B);
}
~~~







#### [108. 将有序数组转换为二叉搜索树](https://leetcode-cn.com/problems/convert-sorted-array-to-binary-search-tree/)





~~~c
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     struct TreeNode *left;
 *     struct TreeNode *right;
 * };
 */

// struct TreeNode *func(int *nums, int left, int right) {
    
//     if (left >= right) {
//         return NULL;
//     }
//     int ind = (left + right) >> 1;
//     struct TreeNode *root = (struct TreeNode *)malloc(sizeof(struct TreeNode));
//     root->val = nums[ind];
//     root->left = func(nums, left, ind);
//     root->right = func(nums, ind + 1, right);
//     return root;
// }

// struct TreeNode* sortedArrayToBST(int* nums, int numsSize){
//     return numsSize == 0 ? NULL : func(nums, 0, numsSize);
// }
#define max(a, b) ((a) > (b) ? (a) : (b))

int getHeight(struct TreeNode *root) {
    if (root == NULL) {
        return 0;
    }
    int l = getHeight(root->left);
    int r = getHeight(root->right);
    return max(l, r) + 1;
}

struct TreeNode *left_rotate(struct TreeNode *root) {
    struct TreeNode *p = root->right;
    root->right = p->left;
    p->left = root;

    return p;
}

struct TreeNode *right_rotate(struct TreeNode *root) {
    struct TreeNode *p = root->left;
    root->left = p->right;
    p->right = root;       
    return p;
}


struct TreeNode *maintain(struct TreeNode *root) {
    if (abs(getHeight(root->left) - getHeight(root->right)) <= 1) return root;
    if (getHeight(root->left) > getHeight(root->right)) {
        if (root->left && root->right && getHeight(root->left->right) > getHeight(root->left->left)) {
            root->left = left_rotate(root->left);
        }
        root = right_rotate(root);
    } else {
        if (root->left && root->right && getHeight(root->right->left) > getHeight(root->right->right)) {
            root->right = right_rotate(root->right);
        }
        root = left_rotate(root);
    }
    return root;
}
struct TreeNode *getNewNode(int target) {
    struct TreeNode *p = (struct TreeNode *)malloc(sizeof(struct TreeNode));
    p->val = target;
    p->left = p->right = NULL;
    return p;
}
struct TreeNode *insert(struct TreeNode *root, int target) {
    if (root == NULL) return getNewNode(target);
    
    if (root->val > target) {
        root->left = insert(root->left, target);
    } else {
        root->right = insert(root->right, target);
    }
    return maintain(root);
}

struct TreeNode* sortedArrayToBST(int* nums, int numsSize){
    struct TreeNode *root = NULL;
    for (int i = 0; i < numsSize; ++i) {
        root = insert(root, nums[i]);
    }
    return root;
}
~~~





#### [112. 路径总和](https://leetcode-cn.com/problems/path-sum/)





~~~c
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     struct TreeNode *left;
 *     struct TreeNode *right;
 * };
 */


/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     struct TreeNode *left;
 *     struct TreeNode *right;
 * };
 */
void hash_insert(struct TreeNode *A, int *num, int sum) {
    if (A->left == NULL && A->right == NULL) {
        if (sum > 1000 || sum < -1000) return ;
        if (sum < 0) num[1000 - sum] = 1;
        else num[sum] = 1;
        return ;
    } 
    if (A->left) hash_insert(A->left, num, sum + A->left->val);
    if (A->right) hash_insert(A->right, num, sum + A->right->val);
    return ;
}

//哈希

bool hasPathSum(struct TreeNode* root, int targetSum) {
    if (root == NULL) return false;
    int num[2001] = {0};
    //memset(num, 0, sizeof(num));
    hash_insert(root, num, root->val);
    if (targetSum >= 0) return num[targetSum];
    return num[1000 - targetSum];
}
~~~





~~~c
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

    bool hasPathSum(TreeNode* root, int targetSum) {
        if (root == nullptr) return false;
        if (root->left == nullptr && root->right == nullptr) {
            return root->val == targetSum;
        }
        return hasPathSum(root->left, targetSum - root->val) | hasPathSum(root->right, targetSum - root->val);;
    }
};
~~~







#### [103. 二叉树的锯齿形层序遍历](https://leetcode-cn.com/problems/binary-tree-zigzag-level-order-traversal/)

* BFS



~~~c
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     struct TreeNode *left;
 *     struct TreeNode *right;
 * };
 */


/**
 * Return an array of arrays of size *returnSize.
 * The sizes of the arrays are returned as *returnColumnSizes array.
 * Note: Both returned array and *columnSizes array must be malloced, assume caller calls free().
 */

#define N (1024)
int** zigzagLevelOrder(struct TreeNode* root, int* returnSize, int** returnColumnSizes){
    *returnSize = 0;
    if (root == NULL) return NULL;

    int **ans = (int **)malloc(sizeof(int *) * N );
    
    *returnColumnSizes = (int *)malloc(sizeof(int) * N );

    struct TreeNode *queue[N], *p = NULL;
    int l = 0, r = 0;
    queue[r++] = root;
    //循环队列，反向搜索
    int flag = 0;//flag = 0从左往右，1从右往左
    while (l != r) {
        //printf("flag = %d, l = %d, r = %d\n", flag, l, r);
        int n = r - l;
        (*returnColumnSizes)[*returnSize] = n;
        int *s = (int *)malloc(sizeof(int) * n);
        int rr = r - 1;
        l = r;
        if (flag == 0) { //从左往右
            for (int i = 0; i < n; ++i) {
                p = queue[rr];
                rr = (--rr) % N;

                s[i] = p->val;
                if (p->left != NULL) {
                    queue[r] = p->left;
                    r = (++r) % N;
                }
                if (p->right != NULL) {
                    queue[r] = p->right;
                    r = (++r) % N;                    
                }


            }
            flag = 1;
        } else { //从右往左
            for (int i = 0; i < n; ++i) {
                p = queue[rr];
                rr = (--rr) % N;

                s[i] = p->val;

                if (p->right != NULL) {
                    queue[r] = p->right;
                    r = (++r) % N;                    
                }
                if (p->left != NULL) {
                    queue[r] = p->left;
                    r = (++r) % N;
                }

            }
            
            flag = 0;
        }
        ans[*returnSize] = s;
        ++(*returnSize);

    }
    return ans;
}
~~~

