---
layout: 力扣剑指Offer26.树的子结构
title: 
      - 力扣剑指Offer26.树的子结构
date: 2021-08-09 13:20:37
tags:
      - 力扣题解
---





#### 思路

* 遍历A的所有节点，找到与B树根节点值相同的节点，判断从该节点出发的树是否和B树相同



#### 自己写的



~~~c
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     struct TreeNode *left;
 *     struct TreeNode *right;
 * };
 */

bool flag = false;

bool func1(struct TreeNode *A, struct TreeNode *B) {
    if (B == NULL) return true;
    if (A == NULL) return false;
    if (A->val != B->val) return false;
    return flag = (func1(A->left, B->left) && func1(A->right, B->right));
}

void func(struct TreeNode *A, struct TreeNode *B) {
    if (A == NULL) return ;
    if (flag) return ; 说明在A树中找到B树了
    if (A->val == B->val) {
        func1(A, B);
    }
    func(A->left, B);//遍历A树
    func(A->right, B);//遍历A树
    return ;
}

bool isSubStructure(struct TreeNode* A, struct TreeNode* B) {
    if (B == NULL || A == NULL) return false;
    flag = false;
    func(A, B);
    return flag;
}
~~~





#### 精简版



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

bool is_match(struct TreeNode *A, struct TreeNode *B) { 判断从子节点A出发的子树和B树是否相同
    if (B == NULL) return true;
    if (A == NULL) return false;
    if (A->val != B->val) return false;
    return is_match(A->left, B->left) && is_match(A->right, B->right);
}

bool isSubStructure(struct TreeNode* A, struct TreeNode* B) {
    if (B == NULL || A == NULL) return false;
    if (A->val == B->val && is_match(A, B)) return true; //
    return isSubStructure(A->left, B) || isSubStructure(A->right, B); //遍历A树
}
~~~

