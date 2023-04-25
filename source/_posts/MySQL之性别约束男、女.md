---
title: MySQL之性别约束男、女
tags: MySQL笔记
abbrlink: 5a6e
date: 2021-11-18 22:03:35
password:
---



### 前言



*对于课本上的check检查约束，在MySQL不支持使用，本人发现使用过程中其不会报错，但查看建表语句发现不存在check,网上查找资料发现*





### 枚举enum字段类型



~~~mysql
create table supplier_contact
(
    supplier_contact_id       varchar(11) not null comment '联系人id'
        primary key,
    supplier_contact_sex enum ('男','女') not null comment '联系人性别',
    supplier_contact_phone     varchar(20) not null comment '联系电话',
    supplier_contact_name varchar(10) not null comment '联系人名字',
    vend_id    int         not null comment '供应商id',
    foreign key (vend_id) references vendors(vend_id)
)
    comment '供应商联系人表' charset = utf8mb4;
~~~





### 不建议使用enum，除非能保证枚举范围只有像男、女这样不要改。



