---
title: C语言开源uthash哈希表
tags:
  - c语言开源代码
abbrlink: a878
date: 2021-06-22 00:03:59
password:
---



#### 头文件

* #include <uthash.h>



#### 结构定义



~~~
typedef struct Hash_Table {
    int key; //内部标识；/* 我们将使用这个字段作为键 */
    int val; //字符名称
    UT_hash_handle hh;  /* 使这个结构可散列 */
} Hash_Table;

Hash_Table *hash_table = NULL;

Hash_Table *find(int key) { //封装key
    Hash_Table *temp;
    HASH_ADD_INT(hash_table, &key, temp);
    return temp;
}
~~~





#### insert



~~~c
void insert(int key, int val) {
    Hash_Table *it = find(key);
    if (it == NULL) {
        Hash_Table *temp = malloc(sizeof(Hash_Table));
        temp->key = key, temp->val = val;
        HASH_ADD_INT(hashtable, key, temp);
    } else{
        it->val = val;
    }
}
~~~



#### delete



~~~c
void delete_user(struct my_struct *user) {
    HASH_DEL( users, user);
}

~~~

## 待学习



[https://troydhanson.github.io/uthash/]()
