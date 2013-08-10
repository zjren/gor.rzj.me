---
date: 2013-03-21
layout: post
title: Mysql基本sql语句优化的10个原则
categories:
- Mysql
tags:
- Mysql
---

Mysql基本sql语句优化的10个原则

1. 尽量避免在列上进行运算，这样会导致索引失效。

    > 例如原句为：  
    > select * from t where YEAR(d)>=2011;  
    > 优化为：  
    > select * from t where d>='2011-01-01';
    
2. 使用JOIN时，因该用小结果集驱动大结果集，同时把复杂的JOIN拆分成多个Query。
3. 注意LIKE模糊查询的使用，避免%%

    > 例如原句为：  
    > select * from t where name like '%de%'  
    > 优化位：  
    > select * from t where name>='de' and name<'df'
    
4. 仅列出需要查询的字段，这里主要考虑节省内存
5. 使用批量插入语句节省交互
6. Limit的基数比较大时使用Between
7. 不要使用rand函数获取多条随机记录
8. 避免使用NULL
9. 不要使用count(id),而应该是count(*)

    > * 任何情况下SELECT COUNT(*) FROM tablename是最优选择；
    > * 尽量减少SELECT COUNT(*) FROM tablename WHERE COL = 'value’ 这种查询；
    > * 杜绝SELECT COUNT(COL) FROM tablename的出现。 

10. 不要做无谓的排序操作，尽可能在索引中完成排序

--摘自[《PHP核心技术与最佳实践》](http://book.douban.com/subject/20370984/)
