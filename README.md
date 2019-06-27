# Sqoop

## Sqoop简介

> Apache Sqoop(TM) is a tool designed for efficiently transferring bulk data between Apache Hadoop and structured datastores such as relational databases.

## Sqoop安装

## Sqoop的基本使用

### 通用

- 查看数据库列表
```bash
sqoop list-databases --connect jdbc:mysql://localhost:3306 --username root --password 123456
```
- 查看数据库表
```bash
sqoop list-tables --connect jdbc:mysql://localhost:3306/sqoop --username root --password 123456
```
- 指定文件
```bash
sqoop --options-file filename ...agrs
```

### 导入

- 命令格式(`sqoop help import`)
```bash
sqoop import \
--connect \ (连接字符串)
--username \ (用户名)
--password[-P | --password-file] \ (密码)
--query \ (查询字符串)
--warehouse-dir \ (导入的HDFS的目录)
--fields-terminated-by \ (分隔符)
-m [--num-mappers] (map数)
```

### 导出
- 命令格式(`sqoop help export`)
```bash
sqoop import \
--connect \ (连接字符串)
--username \ (用户名)
--password[-P | --password-file] \ (密码)
--export-dir 导出目录，需指定--table或--call其一
--update-key
--updatemod
--input-null-string
--input-null-non-string
```

### sqoop job

```bash
#创建job，会自动保存当前时间作为last-value
sqoop job --create job1 -- import --connect jdbc:mysql://localhost:3306/sqoop --username root --password 123456 --table bigdata --check-column last_mod_ts --incremental lastmodified --last-value "2019-06-26 15:20:22.0" -m 1 --warehouse-dir /sqoop/ --append
```
```bash
#运行job
sqoop job --exec job1
```

```bash
#查看job
sqoop job --show job1
```