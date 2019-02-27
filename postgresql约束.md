## 来个前言

目前添加的东西有：

- [x] 0、五个约束，及其添加/删除语句
- [x] 1、添点注释
- [x] 2、修改表名
- [x] 3、修改字段名
- [ ] 4、添加行/删除行
- [x] 5、添加列/删除列
- [ ] 6、修改字段类型
- [x] 7、修改约束类型（算是可以勾了吧）
- [ ] 8、时间类型
- [ ] 9、日期计算（会有吗？）

注：

- 对命令行语句大小写不敏感，但表名字段名大小写敏感
- 字段名大写加" "
- insert 语句字符串加 ' '
- （虽然）建议命令行语句大写
- 以下key名字在命令行可通过查看表内容` \d table_name` 得知，在图形化工具可在Dependents查看
- COLUMN关键字据说是多余的，可省略
- 删除列DROP COLUMN时，只是在数据库中简单标记为不可见，插入或更新时该列位置为NULL

##约束

约束可以在创建表时规定（通过 CREATE TABLE 语句），或者在表创建之后规定（通过 ALTER TABLE 语句）。

以下为建完表之后对约束值的添加/删除


|   关键字    | 约束类型 |
| :---------: | :------: |
| PRIMARY KEY |   主键   |
| FOREIGN KEY |   外键   |
|   UNIQUE    |   唯一   |
|   DEFAULT   |  默认值  |
|  NOT NULL   |   非空   |

#### 主键 pkey

`PRIMARY KEY`: NOT NULL 和 UNIQUE 的结合。约束表中的一行，作为这一行的唯一标识符，不能有重复记录且不能为空

```
#添加主键
ALTER TABLE table_name ADD PRIMARY KEY(column_name);

#删除主键
ALTER TABLE table_name DROP CONSTRAINT table_pkey;
```



#### 外键 fkey

`FOREIGN KEY`: 一个表可以有多个外键，每个外键必须 REFERENCES (参考) 另一个表的主键，被外键约束的列，取值必须在它参考的列中有对应值
```
#添加外键
ALTER TABLE table_name ADD FOREIGN KEY(column_name) REFERENCES other_table_name(other_column_name) ON UPDATE CASCADE  ON DELETE CASCADE;

ON UPDATE CASCADE: 被引用行更新时，引用行自动更新； 
ON DELETE CASCADE: 被引用行删除时，引用行也一起删除；

#删除外键
ALTER TABLE table_name DROP CONSTRAINT table_fkey;
```



#### 唯一约束

`UNIQUE`: 每个值都是唯一的，插入有重复时失败
```
#添加
ALTER TABLE table_name ADD UNIQUE(column_name);
或
ALTER TABLE table_name
ADD CONSTRAINT table_ukey UNIQUE(column1, column2...);

CONSTRAINT table_ukey：为自定义键名，去掉也行，主键、外键同

#删除（主键、外键同，以删除索引值为方法）
ALTER TABLE table_name DROP CONSTRAINT table_ukey;
```

#### 默认值（缺省值）

`DEFAULT`: 插入数据为空时，使用默认值
```
##栗子
people_num INT(10)DEFAULT'10',
#people_num有DEFAULT约束，默认值为10

#正常插入数据
INSERT INTO department(dpt_name,people_num) VALUES('dpt1',11);

#插入新的数据，people_num 为空，使用默认值
INSERT INTO department(dpt_name) VALUES('dpt2');
```
```
#添加默认值
ALTER TABLE table_name ALTER COLUMN column_name SET DEFAULT default_value;

#删除默认值
ALTER TABLE table_name ALTER COLUMN column_name DROP DEFAULT;
```

#### 非空约束

`NOT NULL`: 某列不能储存NULL值，即不能为空
```
#允许NULL值
ALTER TABLE table_name ALTER COLUMN column_name SET NOT NULL;

#拒绝NULL值
ALTER TABLE table_name ALTER COLUMN column_name DROP NOT NULL;
```

~~ALTER TABLE table_name MODIFY column_name datatype NOT NULL;~~


##更新表名
```
ALTER TABLE table_oldname RENAME TO table_newname;
```
##修改字段名字
```
ALTER TABLE table_name RENAME column old_name TO new_name;
```
##添加字段
```
ALTER TABLE table_name ADD column_name datatype （约束）;
```
##删除字段
```
ALTER TABLE table_name DROP column_name;
```
##  ~~修改数据类型及约束~~

```
ALTER TABLE table_name MODIFY column_name datatype 约束;

ALTER TABLE table_name ALTER COLUMN column_name TYPE datatype;
```

以上两条暂时无效