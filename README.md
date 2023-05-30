# MySQL-does-not-support-joint-delete-update-and-select-operations-on-the-same-table
# Table 'table_name' is specified twice, both as a target for 'UPDATE' and as a separate source for data
在写leetcode时，遇见一个问题。（leetcode.196. 删除重复的电子邮箱）
![196  删除重复的电子邮箱](https://github.com/adcodeday/MySQL-does-not-support-joint-delete-update-and-select-operations-on-the-same-table/assets/130963605/86d00cec-762e-44e7-add7-210f49ec947b)

DELETE FROM Person  
WHERE id NOT IN (  
    SELECT MIN(id)  
    FROM Person  
    GROUP BY email  
)  
代码报错：You can't specify target table 'Person' for update in FROM clause.

DELETE FROM Person  
WHERE id NOT IN (  
    SELECT * FROM (  
        SELECT MIN(id)  
        FROM Person  
        GROUP BY email  
    ) AS temp  
)  
成功.

chatgpt给的解释:
是的，MySQL不允许在同一个查询中直接更新（DELETE）正在被查询的表。这是为了确保查询的一致性和可预测性。

当你尝试在一个查询中同时查询和更新同一张表时，MySQL会给出一个错误，提示类似于"Table 'table_name' is specified twice, both as a target for 'UPDATE' and as a separate source for data"。这是因为在同一查询中，你不能在更新（DELETE）操作中引用正在被更新的表。

为了解决这个问题，你可以使用子查询或临时表来分离查询和更新操作。你可以首先执行查询操作，将查询结果存储在一个临时表中或者使用子查询，然后在另一个查询中执行更新（DELETE）操作。

使用子查询可以解决在同一查询中直接更新（DELETE）正在被查询的表的问题，因为子查询可以创建一个临时的结果集，该结果集作为独立的实体进行查询和更新操作，避免了在同一查询中引用正在被更新的表的冲突。

当你执行一个包含子查询的查询语句时，MySQL会首先执行子查询，将其结果作为临时表或虚拟表存储起来，然后在外部查询中使用这个临时表或虚拟表进行查询和更新操作。

通过使用子查询，你可以将查询和更新操作分离，确保了操作的一致性和可预测性。子查询创建了一个临时的结果集，允许你在同一语句中对表进行查询和更新操作，而不会引发冲突或错误。
