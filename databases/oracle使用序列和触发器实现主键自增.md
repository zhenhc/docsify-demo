### oracle实现自增主键（创建序列和触发器）

Oracle没有这个”auto_increment”属性，所以它没法像MySQL般在表内定义自增主键。

但是，Oracle里的序列（SEQUENCE），可间接实现自增主键的作用。

 

序列（Sequence），又叫序列生成器，用于提供一系列的数字，开发人员使用序列生成唯一键。每次访问序列，序列按照一定的规律增加或者减少。

序列的定义存储在SYSTEM表空间中，序列不像表，它不会占用磁盘空间。

序列独立于事务，每次事务的提交和回滚都不会影响序列。

- 创建序列的方法及参数说明如下

```
CREATE SEQUENCE SEQNAME    //序列名字
INCREMENT BY 1                    //每次自增1， 也可写非0的任何整数，表示自增，或自减
START WITH 1                       //以该值开始自增或自减
MAXVALUE 1.0E20                   //最大值；设置NOMAXVALUE表示无最大值
MINVALUE 1                           //最小值；设置NOMINVALUE表示无最大值
CYCLE or NOCYCLE                  //设置到最大值后是否循环；
CACHE 20                              //指定可以缓存 20 个值在内存里；如果设置不缓存序列，则写NOCACHE
ORDER or NOORDER                  //设置是否按照请求的顺序产生序列
```

- 创建表结构

```
CREATE TABLE Demo  
(  
    id INT NOT NULL PRIMARY KEY,  
    key1 VARCHAR2(40) NULL,  
    key2 VARCHAR2(40) NULL  
);  
```

- 插入时：

```
insert into Demo(id, key1, key2) value(SEQ.NEXTVAL,"k1","k2") 
```

> 然而，上述方法不适用于insert的另一种使用方式；即

```sql
insert into table(key1, key2) select k1, k2 from anotherTable; 
```

所以真正设计时，应该用触发器保证自增主键的实现，如下过程：

- 创建触发器

```sql
create or replace trigger dectuser_tb_tri
before insert on dectuser  /*触发条件：当向表dectuser执行插入操作时触发此触发器*/
 for each row     /*对每一行都检测是否触发*/
begin   /*触发器开始*/
select  dectuser_tb_seq.nextval into :new.userid from dual;
/*触发器主题内容，即触发后执行的动作，在此是取得序列dectuser_tb_seq的下一个值插入到表dectuser中的userid字段中*/
end;
/
```

- 检测

```
insert into dectuser(name,sex) values ('feng','男);    
     commit;  /*提交*/   
```





### 参考链接

- [oracle 实现 自增主键功能](https://www.cnblogs.com/sharpest/p/10160370.html)