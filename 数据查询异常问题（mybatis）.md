---
date: "2025-02-12"
title: "数据查询异常问题（mybatis）"
summary: ""
tags: ["mybatis", "mybatis-plus", "SQL"]
categories: []
---

### 问题描述 

#### 问题 SQL

```mybatis
SELECT DISTINCT router_name
        FROM b${businessId} _menu menu
                 INNER JOIN (SELECT DISTINCT menu_id
                             FROM b${businessId} _role_menu role_menu
                             WHERE del_flag = 1
                               AND role_id IN (#{roleIds})) role_menu ON (role_menu.menu_id = menu.menu_id)
        WHERE menu.del_flag = 1
          AND router_name IS NOT NULL
          AND menu_type in ('Button', 'Fragment')
```

#### 问题描述

使用 mybatis plus 查询和直接在数据库中查询数据不一致

#### 问题定位

1. 直接在数据库中查询是将 roleIds 条件直接写在 SQL 中，而在 mybatis plus 中使用的是 ? 占位符方式
2. mybaits plus 中占位符方式为防止 SQL 注入，会将内容认为是字符串，添加单引号，如下

```sql
SELECT DISTINCT role_id
        FROM b1_role_menu
        WHERE del_flag = 1
          AND role_id IN ('1, 2, 7')
```

3. SQL 查询 IN 条件遇到字符串可能会进行类型转换，将'1,2,7'转为1。导致查询错误

#### 解决方案

1. 将 # 替换为 $（如果确认不会有 SQL 注入问题，此解决方案最简单）
2. 使用 FIND_IN_SET 函数修改 SQL 为

```sql
SELECT DISTINCT role_id
        FROM b1_role_menu
        WHERE del_flag = 1
          AND FIND_IN_SET(role_id, '1,2,7') > 0
```

#### 参考链接

[mybatis 中 #{} 和 ${} 的区别](https://www.cnblogs.com/haiyangwu/p/10265325.html)

[解决SQL查询，in条件参数为带逗号的字符串而导致查询结果错误](https://blog.csdn.net/H_233/article/details/87814533)