---
title: 平衡二叉查找树之AVL树
tags: 高级数据结构
abbrlink: 67c6
date: 2021-08-08 12:43:28
password:
---







#### 性质

* | H (left) - H (right) | <= 1





#### 优点



* 由于对每个节点的左右子树的树高做了限制，故整棵树不会退化为一颗链表
* 继承了二叉查找树所以的性质，相比二叉排序树其查找效率更高



#### 缺点



* AVL树为了维持高度平衡，付出了很大代价，每次插入、删除操作都要调整，就比较耗时、复杂
* 对于频繁的插入、删除操作的数据集合，使用AVL树代价高
* 



#### 高度为H的AVL树，包含节点个数范围？



* low(H - 1) + low(H - 2) + 1 <= size(H) <= 2^H - 1





#### 判断是否失衡



* 判断失衡是在节点插入和删除后
* 若失衡，则进行调整



~~~c
Node *maintain(Node *root) {
	if (abs(root->left->hight - root->right->hight) <= 1) return root;
	if (root->left->hight > root->right->hight) { //左子树高，失衡 ,L 
		if (root->left->right->hight > root->left->left->hight) {
			root->left = left_rotate(root->left);  //LR失衡
		}
		root = right_rotate(root); //   LL失衡
	} else { //右子树高， 失衡 R
		if (root->left->right->hight < root->left->left->hight) { //  RL失衡
				root->right = right_rotate(root->right); 
			}
		root = left_rotate(root);  // RR失衡
	}
	return root;
}
~~~





#### 左旋



~~~c
Node *left_rotate(Node *root) { //左旋操作 
	Node *temp = root->right;
	root->right = temp->left;
	temp->left = root;
	update_h(root);
	update_h(temp);
	return temp;
}
~~~







#### 右旋





~~~C

Node *right_rotate(Node *root) { //右旋操作 
	Node *temp = root->left;
	root->left = temp->right;
	temp->right = root;
	update_h(root);
	update_h(temp);
	return temp; 
}
~~~







#### 代码演示



~~~c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

typedef struct Node {
	int value, h;
	struct Node *left, *right;
} Node;

#define max(a, b) ({\
	__typeof(a) _a = a;\
	__typeof(b) _b = b;\
	_a > _b ? _a : _b;\
})

Node __NIL;
#define NIL (&(__NIL))

__attribute__((constructor))
void init_NIL() {
	NIL->value = -1;
	NIL->h = 0;
	NIL->left = NIL->right = NIL;
	return ;
}

Node *getNewNode(int val) {
	Node *p = (Node *)malloc(sizeof(Node));
	p->value = val;
	p->left = p->right = NIL;
	p->h = 1;
	return p;
}

void clear(Node *root) {
	if (root == NIL) return ;
	clear(root->left);
	clear(root->right);
	free(root);
	return ;
}

void update_h(Node *root) { //计算节点高度 
	root->h =  max(root->left->h, root->right->h) + 1;
	return ;
}

Node *left_rotate(Node *root) { //左旋操作 
	Node *temp = root->right;
	root->right = temp->left;
	temp->left = root;
	update_h(root);
	update_h(temp);
	return temp;
}

Node *right_rotate(Node *root) { //右旋操作 
	Node *temp = root->left;
	root->left = temp->right;
	temp->right = root;
	update_h(root);
	update_h(temp);
	return temp; 
}

Node *maintain(Node *root) {
	if (abs(root->left->h - root->right->h) <= 1) return root;
	if (root->left->h > root->right->h) { //左子树高，失衡 ,L 
		if (root->left->right->h > root->left->left->h) {
			root->left = left_rotate(root->left);  //LR失衡
		}
		root = right_rotate(root); //   LL失衡
	} else { //右子树高， 失衡 R
		if (root->right->right->h < root->right->left->h) { //  RL失衡
				root->right = right_rotate(root->right); 
			}
		root = left_rotate(root);  // RR失衡
	}
	return root;
}


Node *insert(Node *root, int val) {
	if (root == NIL) return getNewNode(val);
	if (root->value == val) return root;
	if (root->value > val) {
		root->left = insert(root->left, val);
	} else {
		root->right = insert(root->right, val);
	}
	update_h(root);
	return maintain(root); //返回调整平衡后的节点
}

Node *get_per(Node *root) { //得到前驱节点 
	if (root == NULL) return NIL;
	Node *p = root->left;
	while (p && p->right != NIL) p = p->right;
	return p;
}

Node *erase(Node *root, int val) {
	if (root == NIL) return NIL;
	if (root->value > val) {
		root->left = erase(root->left, val);
	} else if (root->value < val) {
		root->right = erase(root->right, val);
	} else {
		if (root->left == NIL || root->right == NIL) {
			Node *temp = root->left ? root->left : root->right;
			free(root);
			return temp;
		}
		Node *p = get_per(root);
		root->value = p->value;
		root->left = erase(root->left, p->value); 
	}
	update_h(root);
	return maintain(root); //返回调整平衡后的节点
}

int search(Node *root, int target) {
	if (root == NIL) return 0;
	if (root->value == target) return 1;
	if (root->value > target) {
		return search(root->left, target);
	}
	return search(root->right, target);
	
}

void in_order(Node *root) {
	if (root == NIL) return ;
	in_order(root->left);
	printf("%d , h = %d, left = %d, right = %d\n", root->value, root->h, root->left->value, root->right->value);
	in_order(root->right);
	return ;
}

int main() {
	#define n 300
	srand(time(0));
	Node *root = NIL;
	for (int i = 0; i < n * 2; i++) {
		int val = rand() % 100;
		printf("%d ", val);
		root = insert(root, val);
	}
	printf("\n");
	in_order(root);
	printf("\n");
	
	for (int i = 0; i < n; i++) {
		int val = rand() % 100;
		int flag = search(root, val);
		if (flag) printf("%d            存在\n", val);
		else  printf("%d            不存在\n", val);
		root = erase(root, val);
		printf("root->value = %d\n", root->value);
	}
	
	printf("%d\n", root->value);
	
	printf("\n");
	in_order(root);
	printf("\n");
	
	#undef n
	clear(root);
	return 0;
}
~~~













