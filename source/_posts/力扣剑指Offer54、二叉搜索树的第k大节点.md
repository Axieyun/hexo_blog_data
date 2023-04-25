---
title: 力扣剑指Offer54、二叉搜索树的第k大节点
tags:
  - 力扣题解
abbrlink: 1b5b
date: 2021-08-05 19:23:56
password:
---



#### 一般解法

~~~c
int ans = 0;

//二叉排序树的中序遍历是有序的
//将二叉排序树中序遍历倒过来输出即可找到第k大值
void in_order(struct TreeNode *root, int *k) { //取k的地址，保证k是唯一
    if (root->right) in_order(root->right, k); //递归找到最大值
    (*k) -= 1;
    if (*k == 0) {
        ans = root->val;
        return ;
    }
    if (root->left && k) in_order(root->left, k);
}

int kthLargest(struct TreeNode* root, int k){
    if(root == NULL) return 0;
    in_order(root, &k);
    return ans;
}
~~~





#### 解法二



~~~c
int count(struct TreeNode *root) {
    if (root == NULL) return 0;
    return count(root->left) + count(root->right) + 1;
}

int kthLargest(struct TreeNode* root, int k) {
    int r_size = count(root->right); //求右子树节点个数
    if (r_size >= k) return kthLargest(root->right, k); //右子树节点数量大于k，则所求数在该节点右子树上
    if (r_size + 1 == k) return root->val; //自己加右子树节点个数为k，则该节点即为所求；
    return kthLargest(root->left, k - r_size - 1); //否则所求数在该节点左子树上
}
~~~

