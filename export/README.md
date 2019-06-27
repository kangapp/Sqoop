# export

## 导出到mysql

```bash
#若表没有指定主键，则可以重复导出数据
sqoop export --connect jdbc:mysql://localhost:3306/sqoop --username root --password 123456 --table bigdata2 --export-dir /sqoop/bigdata/
```

- --update-key 更新标识
- --updatemod 
    - updateonly    默认，只允许更新
    - allowinsert   允许插入数据

```bash
#只是根据update-key更新原有的数据，不会插入新的数据
sqoop export --connect jdbc:mysql://localhost:3306/sqoop --username root --password 123456 --table bigdata2 --export-dir /sqoop/bigdata/ --update-key class_id --updatemod updateonly
```

```bash
#若update-key指定的不是主键，则只会插入数据，会形成冗余数据
#若update-key指定的时主键，则会同时执行更新和插入操作
sqoop export --connect jdbc:mysql://localhost:3306/sqoop --username root --password 123456 --table bigdata2 --export-dir /sqoop/bigdata/ --update-key class_id --updatemod allowinsert
```