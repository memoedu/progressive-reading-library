今天刚好把数据库原理的题目做完，正好写篇博客来纪念一下。反正以后大概率用不到那些复杂的查询语句。

## Ex1. 建立基本表

### 建立教学数据库的三个基本表S（学生）、SC（选课）、C（课程）

| 表名 | 属性                         | 说明                         |
| ---- | ---------------------------- | ---------------------------- |
| S    | Sno,Sname,Sgender,Sage,Sdept | 学号，姓名，性别，年龄，系   |
| SC   | Sno,Cno,Grade                | 学号，课程号，成绩           |
| C    | Cno,Cname,Cpno,Ccredit       | 课程号，课程名，先行课，学分 |

代码：

```sql
CREATE TABLE S (
    SNO     VARCHAR2(9),
    SNAME   VARCHAR2(4),
    SGENDER VARCHAR2(2),
    SAGE    NUMBER(3),
    SDEPT   VARCHAR2(20)
);

CREATE TABLE SC (
    SNO     VARCHAR2(9),
    CNO     VARCHAR2(4),
    GRADE   NUMBER(3)
);

CREATE TABLE C (
    CNO     VARCHAR2(4),
    CNAME   VARCHAR2(20),
    CPNO    VARCHAR2(4),
    CCREDIT NUMBER(2, 1)
);
```



### 通过insert语句插入数据

数据自行编造：

```sql
insert into S values('201215121','李勇','男',20,'CS');
insert into S values('201215122','刘晨','女',19,'CS');
insert into S values('201215123','王敏','女',18,'MA');
insert into S values('201215124','李四','男',18,'MA');
insert into S values('201215125','张立','男',19,'IS');
insert into S values('201215126','张三','男',21,'IS');
insert into S values('201215127','唯一','男',21,'IS');

insert into SC values('201215121','1',92);
insert into SC values('201215121','2',85);
insert into SC values('201215121','3',88);

insert into SC values('201215122','1',82);
insert into SC values('201215122','2',90);
insert into SC values('201215122','3',95);
insert into SC values('201215122','4',86);
insert into SC values('201215122','5',85);
insert into SC values('201215122','6',95);
insert into SC values('201215122','7',75);

insert into SC values('201215123','1',92);
insert into SC values('201215123','2',70);
insert into SC values('201215123','3',56);

insert into SC values('201215124','4',70);
insert into SC values('201215124','5',90);
insert into SC values('201215124','6',88);

insert into SC values('201215125','2',94);
insert into SC values('201215125','6',97);
insert into SC values('201215125','7',93);

insert into SC values('201215126','1',88);
insert into SC values('201215126','4',95);
insert into SC values('201215126','5',88);
insert into SC values('201215126','7',90);

insert into SC values('201215127','1',null);
insert into SC values('201215127','2',null);
insert into SC values('201215127','7',100);


insert into C values('1','数据库原理','5',4);
insert into C values('2','高等数学',null,3);
insert into C values('3','信息系统','1',3);
insert into C values('4','操作系统','6',3);
insert into C values('5','数据结构','7',4);
insert into C values('6','数据处理',null,3);
insert into C values('7','PASCAL语言','6',4);
```

---

## Ex2. 查询

### 查询选修1号课程的学生学号与姓名。

```sql
-- 做法一：嵌套查询
SELECT SNO, SNAME FROM S
WHERE SNO IN (
    SELECT SNO FROM SC 
    WHERE CNO='1'
);

-- 做法二：自然连接
SELECT SNO, SNAME FROM S NATURAL JOIN SC
WHERE SC.CNO='1';
```

### 查询选修课程名为数据库原理的学生学号与姓名。

```sql
SELECT SNO, SNAME FROM S
WHERE SNO IN (
    SELECT SNO FROM SC
    WHERE CNO=(
        SELECT CNO FROM C
        WHERE CNAME='数据库原理'
    )
);
```

### 查询所有除了选修1号课程外的学生中，所有成绩均及格的学生的学号和平均成绩，其结果按平均成绩的降序排列。

基本逻辑为先找出成绩不及格或者选修了1号课程的学号，然后通过`not in`将它们排除后，再进行分组聚合。

```sql
SELECT SNO, AVG(GRADE) FROM (
    SELECT * FROM SC SCX
    WHERE SCX.SNO NOT IN (
        SELECT SCY.SNO FROM SC SCY
        WHERE SCY.GRADE < 60 OR SCY.CNO='1'
    ) AND SCX.GRADE IS NOT NULL
)
GROUP BY SNO
ORDER BY AVG(GRADE) DESC;
```

### 查询选修数据库原理成绩第2名的学生姓名。

经典的第二名问题，最稳妥的做法是先找出成绩为第一个的学号的集合（第一名可能不只一个），然后把这些学号全部排除即可，剩下的里面最大的就是第二名了。

```sql
SELECT SNAME FROM S
WHERE SNO IN (
    SELECT SNO FROM SC NATURAL JOIN C
    WHERE GRADE = (
        SELECT MAX(GRADE) FROM SC NATURAL JOIN C
        WHERE CNAME='数据库原理' AND GRADE NOT IN (
            SELECT MAX(GRADE) FROM SC NATURAL JOIN C
            WHERE CNAME='数据库原理'
        )
    ) AND C.CNAME='数据库原理'
);
```

### 查询所有3个学分课程中有3门以上（含3门）课程获80分以上（含80分）的学生的姓名。

```sql
SELECT SNAME FROM S
WHERE SNO IN (
    SELECT SNO FROM (
        SELECT SNO, GRADE 
        FROM SC NATURAL JOIN C
        WHERE CCREDIT = 3  
    )
    WHERE GRADE IS NOT NULL AND GRADE >= 80
    GROUP BY SNO
    HAVING COUNT(*) >= 3
);
```

### 查询选课门数唯一的学生的学号。

首先找出选课门数这个属性唯一的数量为多少，然后通过聚合统计找出符合要求的学号。

```sql
SELECT SNO FROM SC
GROUP BY SNO
HAVING COUNT(SNO) IN (
    SELECT count FROM (
        SELECT SNO, COUNT(*) as count FROM SC
        GROUP BY SNO
    )
    GROUP BY count
    HAVING COUNT(count)=1
);
```

---

## Ex3. 修改与删除

### 把1号课程的非空成绩提高10％。

这个很简单，但是需要注意一下，提高的分数不能超过100（这是常识）。`Oracle`中的三元表达式可以通过`case when <condition> then <expression1> else <expression2> end`来表示。

```sql
UPDATE SC
SET GRADE = CASE WHEN GRADE * 1.1 > 100 
                THEN 100
                ELSE GRADE * 1.1
                END
WHERE GRADE IS NOT NULL
AND CNO='1';
```

### 在SC表中删除课程名为数据库原理的元组。

```sql
DELETE FROM SC 
WHERE CNO IN (
    SELECT CNO FROM C
    WHERE CNAME='数据库原理'
);

SELECT * FROM SC NATURAL JOIN C;
```

### 在S和SC表中删除学号为201215121的所有数据。

```sql
DELETE FROM S WHERE SNO='201215121';
DELETE FROM SC WHERE SNO='201215121';
```

---

## Ex4. 建立视图

### 建立男学生的视图，属性包括学号、姓名、选修课程名和成绩。

```sql
CREATE VIEW BOY
AS
SELECT SNO, SNAME, CNAME, GRADE FROM C NATURAL JOIN (
    SELECT * FROM SC NATURAL JOIN S
    WHERE SGENDER='男' 
);
```

### 在男学生视图中查询平均成绩大于80分的学生学号与姓名。

视图中查找的方法和基本表基本一致。

```sql
SELECT SNO, SNAME FROM BOY
GROUP BY SNO, SNAME
HAVING AVG(GRADE) > 80;
```

---

## Ex5. 授权与撤销

### 计算每个学生有成绩的课程门数、平均成绩。

通过`is null`来判断某个元组的某个分量是否为空。

```sql
SELECT SNO, COUNT(*), AVG(GRADE) FROM SC
WHERE GRADE IS NOT NULL
GROUP BY SNO;
```

### 使用GRANT语句，把对基本表S、SC、C的使用权限授给其它用户。

记住`grant`可以赋予的角色。基本的所有权限直接用`all privileges`。

```sql
CREATE USER K IDENTIFIED BY 123;
GRANT ALL PRIVILEGES ON S TO K;
GRANT ALL PRIVILEGES ON SC TO K;
GRANT ALL PRIVILEGES ON C TO K;
```

### 实验完成后，撤消建立的基本表和视图。

通过`drop`来撤销表格，需要注意，这种方式并不会彻底消除基本表。

```sql
DROP TABLE S;
DROP TABLE SC;
DROP TABLE C;
DROP VIEW BOY;
```