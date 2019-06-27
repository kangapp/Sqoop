# import

## 导入到HDFS

- 导入的目录不能已存在
- 没指定-m或--split-by时，表必须存在主键，默认启动4个任务
- 默认分隔符是 ,  

### 导入全表

```bash
sqoop import --connect jdbc:mysql://localhost:3306/sqoop --username root --password 123456 --table bigdata --split-by class_id -m 2 --warehouse-dir /sqoop/ --fields-terminated-by "|"
```

### 导入部分数据
- --query 必须指定--target-dir
- --query 必须使用 \\$CONDITIONS
```bash
sqoop import --connect jdbc:mysql://localhost:3306/sqoop --username root --password 123456 --query "select * from bigdata where class_id <=5 and \$CONDITIONS" -m 1 --target-dir /sqoop/bigdata
```

- --where 指定限制条件
- --columns 指定查询的列
```bash
sqoop import --connect jdbc:mysql://localhost:3306/sqoop --username root --password 123456 --table bigdata -m 1 --warehouse-dir /sqoop/ --columns "class_id,class_name" --where "class_id<5"
```

### 增量导入

- --check-column (col)  指定在确定要导入的行时要检查的列
- --incremental (mode)  指定Sqoop如何确定哪些行是新的
    - append
    - lastmodified
- --last-value (value) 指定上一个导入的检查列的最大值

```bash
#会在原来的目录生成追加生成新的目录文件
sqoop import --connect jdbc:mysql://localhost:3306/sqoop --username root --password 123456 --table bigdata --check-column class_id --incremental append --last-value 9 -m 1 --warehouse-dir /sqoop/
```

```bash
#注意mysql和本地的时区是否一致
#追加时间大于和等于 las-value的时间
sqoop import --connect jdbc:mysql://localhost:3306/sqoop --username root --password 123456 --table bigdata --check-column last_mod_ts --incremental lastmodified --last-value "2019-06-26 04:05:22.0" -m 1 --warehouse-dir /sqoop/ --append
```

```bash
#适用数据有更新，不会产生冗余数据，根据merge-key进行合并
sqoop import --connect jdbc:mysql://localhost:3306/sqoop --username root --password 123456 --table bigdata --check-column last_mod_ts --incremental lastmodified --last-value "2019-06-26 04:05:22.0" -m 1 --warehouse-dir /sqoop/ --merge-key class_id
```