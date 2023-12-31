---
title: SQL语句(五)——SELECT
date: 2023-11-22 20:04:04
description: 
categories: 
tags: 
top_image: 
cover: 
keywords: 
published: false
---
# 数据查询（SELECT）
- 语句格式如下：
	- SELECT \[ALL|DISTINCT] <目标列表达式></br>\[,<目标列表达式>]……</br>FROM <表名或视图名>\[,<表名或视图名]……</br>\[WHERE <条件表达式>]</br>\[GROUP BY <列名1>\[HAVING <条件表达式>]]</br>\[ORDER BY <列名2>\[ASC|DESC]];
- 说明：
	- SELECT：表示选择要显示哪些信息（例如姓名、学号）
	- FROM：表示要从哪张基本表（视图）查找这些信息
	- WHERE：表示条件
	- GROUP BY：表示分组。比如我需要查询一年级各班的整体平均成绩，那我们可以将一年一班、一年级二班、一年级三班进行分组查询，然后再将这些平均成绩加起来求整个年级的平均成绩
	- ORDER BY：表示排序，按哪列排序
- 注意：
	- 语句中的字母不分大小写
	- 语句中的“,;”等标点字符为英文状态下的半角符号
	- \[]中的内容不是语句必须的内容，只是为了实现某些需求时才添加
- <font color = "CC6600">「SELECT」</font>三段论
	1. 首先找出我们需要查询什么，如果是学生的姓名`Sname`，则`Sname`跟在<font color = "CC6600">「SELECT」</font>后面，即`SELECT name`
	2. 然后确定查询涉及到了哪些表，例如在这里涉及了`Student`表，那么在<font color = "CC6600">「FROM」</font>子句后面就跟上表名，即`FROM Student`
	3. 最后根据题目中的查询要求来判断出我们的查询条件是什么，例如查询出信息系的全部学生，则完整的语句就是：`SELECT Sname FROM Student WHERE Sdept='IS';`

## <font color = "886600">单表查询</font>
<strong>功能：对一个表的内容进行查询</strong>
### <font color = "AA7700">选择表中的若干列</font>
- 选择表中的若干列
	- 功能：查询指定列
	- 语句格式：在SELECT后面指定列名，FROM后面跟列所在的表名
	- 例如：查询全体学生的学号和姓名
		- SELECT Sno,Sname </br>FROM Student;
	- 例如：查询全体学生的姓名、学号、所在系
		- SELECT Sname,Sno,Sdept </br>FROM Student;
- 查询全部列
	- 功能：选出表中所有的属性列
	- 语句格式：在SELECT关键字后面列出所有的列名或将<目标列名称>指点为\*
	- 例如：查询全体学生的详细记录
		- SELECT Sno,Sname,Ssex,Sage,Sdept </br>FROM Student;
		- SELECT \* FROM Student;
- 查询经过计算的值
	- 功能：选出表中指定的属性列，并经过计算后输出
	- 格式：SELECT子句的<目标列表达式>可以为：
		- 算术表达式
		- 字符串常量
		- 函数
		- 列别名
	- 例如：查询全体学生的姓名及其出生年份
		- 语句：SELECT Sname,2004-Sage FROM Student;
		- 说明：由于在学生表中没有存储学生的出生年份，因此我们需要利用当前的年份减去学生的年龄，即：2004-Sage来计算学生的出生年份
		- 输出结果如下：
			- ![image.png](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202311222042133.png)
	- 例如：查询全体学生的姓名、出生年份和所有系，要求用小写字母表示所有系名
		- 语句：SELECT Sname,'Year of Birth:',2014-Sage,LOWER(Sdept)</br>FROM Student;
		- 说明：
			- 'Year of Birth:'：由于该字符串在表中没有，因此它是什么那么查询出来的结果就是什么）
			- LOWER()：这是一个函数，用来将查询到结果转换为小写字母，转换为大写字母的是UPPER()函数
		- 输出结果如下图：
			- ![image.png](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202311222032189.png)
	- 例如：使用列别名改变查询结果的列标题
		- 语句：SELECT Sname NAME,'Year of Birth:' BIRTH 2004-Sage BIRTHDAY,LOWER(Sdept) DEPARTMENT FROM Student;
		- 说明：
			- Sname NAME：表示将NAME设置为Sname的别名，那么在查询出的结果集中就会显示NAME名称而不是Sname
			- 'Year of Birth:' BIRTH：表示将BIRTH设置成'Year of Birth:'的别名
			- 2004-Sage BIRTHDAY：表示将BIRTHDAY设置成2004-Sage的别名
			- LOWER(Sdept) DEPARTMENT：表示将DEPARTMENT设置成LOWER(Sdept)的别名
		- 输出结果如下图：
			- ![image.png](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202311222041394.png)
### <font color = "AA7700">选择表中的若干元组</font>
- 消除取值重复的行：如果没有指定<font color = "CC6600">「DISTINCT」</font>关键词，则缺省为<font color = "CC6600">「ALL」</font>
	- 例如：查询选修了课程的学生学号
		- 语句：SELECT Sno FROM SC; => SELECT ALL Sno FROM SC;
		- 输出结果：
			- |    Sno    |
			  |:---------:|
			  | 200215121 |
			  | 200215121 |
			  | 200215121 |
			  | 200215122 |
			  | 200215122 |
	- 例如：指定<font color = "CC6600">「DISTINCT」</font>关键词，去掉表中重复的行
		- 语句：SELECT DISTINCT Sno FROM SC;
		- 输出结果：
			- |    Sno    |
			  |:---------:|
			  | 200215121 |
			  | 200215122 |
- 查询满足条件的元组
	- <strong>查询满足条件的元组可以通过<font color = "CC6600">「WHERE」</font>子句实现，<font color = "CC6600">「WHERE」</font>子句常用的查询条件如下表所示：</strong>
	- ![image.png](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202311231949712.png)
		- <>：这个与!=等价
		- NOT：表示取反
	- 应用：比较大小(能够确定大致的区间)
		- 例如：查询计算机科学系全体学生的姓名
			- 语句：SELECT Sname FROM Student</br>&emsp;&emsp;&emsp;WHERE Sdept='CS';
		- 例如：查询所有年龄在20岁以下的学生姓名及其年龄
			- 语句：SELECT Sname,Sage FROM Student</br>&emsp;&emsp;&emsp;WHERE Sage<20;
		- 例如：查询考试成绩有不及格的学生的学号
			- 语句：SELECT DISTINCT Sno FROM SC</br>&emsp;&emsp;&emsp;WHERE Grade<60; 
			- 说明：因为一个学生可能有多门课程不及格，因此需要用到<font color = "CC6600">「DISTINCT」</font>
	- 应用：确定范围
		- <strong>谓词有：<font color = "CC6600">「BETWEEN……AND……」</font>和<font color = "CC6600">「NOT BETWEEN……AND……」</font></strong>
		- 例如：查询年龄在20~23岁（包括20岁和23岁）之间的学生的姓名、系别和年龄
			- 语句：SELECT Sname,Sdept,Sage FROM Student</br>&emsp;&emsp;&emsp;WHERE Sage BETWEEN 20 AND 23;
		- 例如：查询年龄年龄不在20~23岁之间的学生姓名、系别和年龄
			- 语句：SELECT Sname,Sdept,Sage FROM Student</br>&emsp;&emsp;&emsp;WHERE Sage NOT BETWEEN 20 ADD 23;
	- 应用：确定集合
		- <strong>谓词有：<font color = "CC6600">「IN</值表>」</font>,<font color = "CC6600">「NOT IN</值表>」</font></strong>
		- 例如：查询信息系（IS）、数学系（MA）和计算机科学系（CS）学生的姓名和性别
			- 语句：SELECT Sname,Ssex FROM Student</br>&emsp;&emsp;&emsp;WHERE Sdept IN('IS','MA','CS');
		- 例如：查询既不是信息系、数学系，也不是计算机科学系学生的姓名和性别
			- 语句：SELECT Sname,Ssex FROM Student</br>&emsp;&emsp;&emsp;WHERE Sdept NOT IN('IS','MA','CS');
	- 应用：字符匹配
		- 谓词有：\[NOT] LIKE '<匹配串>' \[ESCAPE '<换码字符>']
		- 场景一：匹配串为固定字符串
			- 例如：查询学号为200215121的学生的详细情况
				- 语句：SELECT \* FROM Student</br>&emsp;&emsp;&emsp;WHERE Sno LIKE '200215121';
				- 上述语句等价于：SELECT \* FROM Student</br>&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;WHERE Sno='200215121';
				- 按照编程习惯，因为我们已经知道了确切的查询条件信息，因此我们更常用第二种查询语句
		- 场景二：匹配串为含通配符的字符串
			- 例如：查询所有姓刘学生的姓名，学号和性别
				- 语句：SELECT Sname,Sno,Ssex FROM Student</br>&emsp;&emsp;&emsp;WHERE Sname LIKE '刘%';
				- 说明：%表示任意长度的字符串，但是开头的字符必须是'刘'
			- 例如：查询姓'欧阳'且全名为三个汉字的学生的姓名
				- 语句：SELECT Sname FROM Student</br>&emsp;&emsp;&emsp;WHERE Sname LIKE '欧阳_';
				- 说明：'\_'表示”欧阳“后面只能跟一个字符，如果需要跟两个字符则是'\_\_'（其实'\_'和'\_\_'相当于占位符）
			- 例如：查询名字中第二个字为“阳”字的学生的姓名和学号
				- 语句：SELECT Sname,Sno FROM Student</br>&emsp;&emsp;&emsp;WHERE Sname LIKE '\_阳%';
			- 例如：查询所有不姓刘的学生姓名、学号和性别
				- 语句：SELECT Sname,Sno,Ssex FROM Student</br>&emsp;&emsp;&emsp;WHERE Sname NOT LIKE '刘%';
			- 例如：查询所有不姓刘的学生姓名、学号和性别
				- 语句：SELECT Sname,Sno,Ssex FROM Student</br>&emsp;&emsp;&emsp;WHERE NOT LIKE '刘%';
		- 场景三：使用换码字符将通配符转义为普通字符
			- 例如：查询DB\_Design课程的课程号和学分
				- 语句：SELECT Cno,Ccredit FROM Course</br>&emsp;&emsp;&emsp;WHERE Cname LIKE 'DB\_Design' ESCAPE '\\';
				- 说明：在'DB\_Design'中，它里面包含的'\_'是我们实际需要查询的条件并不代表任意字符，因此我们需要使用<font color = "CC6600">「ESCAPE」</font>来进行转义，则`ESCAPE \`这里表示的意思就是：告诉DBMS，\\后面的字符'\_'具有实际意义，不是任意字符
			- 例如：查询以“DB\_”开头，且倒数第三个字符为i的课程的详细信息情况
				- 语句：SELECT \* FROM Course</br>&emsp;&emsp;&emsp;WHERE Cname LIKE 'DE\_%i\_\_' ESCAPE '\\'
				- 说明：ESCAPE '\\'表示“\”为转义字符。因为倒数第三个字符是i，因此i前面任意长度的字符，因此使用%，，后面为两个占位符'\_\_'
		- 场景四：涉及空值的查询
			- <strong>谓词：<font color = "CC6600">「IS NULL」</font>或<font color = "CC6600">「IS NOT NULL」</font></strong><font color = "BA8448">【注意，IS不能用=替代】</font>
			- 例如：某些学生选修课程后没有参加考试，所以有选课记录但是没有考试成绩。查询缺少成绩学生得学号和相应得课程号
				- 语句：SELECT Sno,Cno FROM SC</br>&emsp;&emsp;&emsp;WHERE Grade IS NULL;
			- 例如：查询所有有成绩得学生学号和课程号
				- 语句：SELECT Sno,Cno FROM SC</br>&emsp;&emsp;&emsp;WHERE Grade IS NOT NULL;
		- 场景五：多重条件查询
			- <strong>用逻辑运算符<font color = "CC6600">「AND」</font>和<font color = "CC6600">「OR」</font>来联结多个查询条件，<font color = "CC6600">「AND」</font>的优先级高于<font color = "CC6600">「OR」</font>，可以用括号改变优先级</strong>。联结查询可以用来实现多种其他谓词：\[NOT] IN，\[NOT] BETWEEN……AND……
			- 例如：查询计算机系年龄在20岁以下的学生姓名
				- 语句：SELECT Sname FROM Student</br>&emsp;&emsp;&emsp;WHERE Sage<20 AND Sdept='IS';
			- 例如：查询信息系（IS）、数学系（MA）和计算机科学系（CS）的学生的姓名和性别
				- 语句：SELECT Sname,Ssex FROM Student</br>&emsp;&emsp;&emsp;WHERE Sdept='IS' OR Sdept='MA' OR Sdept='CS';
### <font color = "AA7700">ORDER BY子句</font>
<strong>ORDER BY子句可以按一个或多个属性列排序：</strong>
- 升序：ASC;（默认为升序排序）
- 降序：DESC;
- 当排序列含空值时：（空值默认为最大值）
	- ASC：排序列为空值的元组最后显示
	- DESC：排序列为空值的元组最先显示
- 应用：
	- 例如：查询选修了3号课程的学生的学号及其成绩，查询结果按分数降序排列
		- 语句：SELECT Sno,Grade FROM SC WHERE Cno = '3'</br>&emsp;&emsp;&emsp;ORDER BY Grade DESC;
	- 例如：查询全体学生情况，查询结果按所在系的系号升序排列，同一系中的学生按年龄降序排列
		- 语句：SELECT \* FROM Student</br>&emsp;&emsp;&emsp;ORDER BY Sdept,Sage DESC;

### <font color = "AA7700">聚集函数</font>
| 函数 | 说明 |
|:----:|:---:|
| COUNT (\[DISTINCT \| ALL ] \*) | 统计元组个数 |
| COUNT (\[DISTINCT \| ALL] \<列名>) | 统计一列中值的个数 |
| SUM (\[DISTINCT \| ALL] \<列名>) | 计算一列值的总和（注意，此列必须是数值列）|
| AVG (\[DISTINCT \| ALL] \<列名>) | 计算一列值的平均值（注意，此列必须是数值列）|
| MAX (\[DISTINCT \| ALL] \<列名>) | 求一列值的最大值 |
| MIN (\[DISTINCT \| ALL] \<列名>) | 求一列值中的最小值|
注意：默认是ALL；
- 例：查询学生总人数
	- 语句：SELECT COUNT (\*) FROM Student;
	- 说明：从Student表中查询一共有多少个元组（记录），有多少个元组就代表有多少学生
- 例：查询选修了课程的学生人数
	- 语句：SELECT COUNT (DISTINCT Sno) FROM SC;
	- 说明：从SC表中统计一共有多少个学生学号（Sno），因为一个学生可能会选修多门课程，因此需要去重
- 例：查询学生200215012选修课程的总学分数
	- 语句：SELECT SUM (Ccredit) FROM SC,Course</br>&emsp;&emsp;&emsp;WHERE Sno = '200215012'</br>&emsp;&emsp;&emsp;AND SC.Cno = Course.Cno
	- 说明：此查询涉及到联合查询
- 例：计算1号课程的学生平均成绩
	- 语句：SELECT AVG (Grade) FROM SC</br>&emsp;&emsp;&emsp;WHERE Cno = '1';
- 例：查询选修1号课程的学生最高分数
	- 语句：SELECT MAX (Grade) FROM SC</br>&emsp;&emsp;&emsp;WHERE Cno = '1';

<font color = "CC6600">「注意：WHERE子句中是不能用聚集函数作为条件表达式的。聚集函数只能用于SELECT子句和GROUP BY中的HAVING子句」</font>

### <font color = "AA7700">GROUP BY</font>
<strong>GROUP BY子句的作用是：按指定的一列或多列值分组，值相等的为一组，来细化聚集函数的作用对象</strong>
- 说明：
	- 未对查询结果分组，聚集函数将作用于整个查询结果
	- 对查询结果分组后，聚集函数将分别作用于每个组
- 例：求各个课程号及相应的选课人数
	- 语句：SELECT Cno,COUNT (Sno) FROM SC </br>&emsp;&emsp;&emsp;GROUP BY Cno;
	- 说明：如果有GROUP BY，则SELECT后面只能跟两列信息，第一列为GROUP BY后面的列，第二列就是聚集函数所计算出来的值
	- 查询结果：
		- ![](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo%2F%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202023-12-10%20165317.png)
- <font color = "CC6600">「注意：GROUP BY子句进行分组之后，可以使用HAVING短语指定筛选条件」</font>
	- 例如：查询选修了3门以上课程的学生学号
		- 语句：SELECT Sno FROM SC </br>&emsp;&emsp;&emsp;GROUP BY Sno</br>&emsp;&emsp;&emsp;HAVING COUNT (\*) > 3;
		- <font color = "CC6600">「注意：GROUP BY后面不能接WHERE，只能接HAVING，利用聚集函数进行筛选」</font>

## <font color = "886600">多表查询</font>
### <font color = "AA7700">连接查询</font>
1. 等值与非等值连接查询
	连接查询的`WHERE`子句中用来连接两个表的条件称为<font color = "CC6600">「连接条件」</font>或<font color = "CC6600">「连接谓词」</font>，其一般格式如下：
	- 格式一：\[\<表名1>.]\<列名1> \<比较运算符> \[\<表明2>.]\<列名2>;</br>其中，比较运算符有：=、>、<、>=、<=、!=
	- 格式二：\[\<表名1>.]\<列名1> BETWEEN \[\<表名2>.]\<列名2> AND \[\<表名2>.]\<列名3>;
	- 注意：
		- 当连接运算符为“=”时称为<font color = "CC6600">「等值连接」</font>，其他运算符称为<font color = "CC6600">「非等值连接」</font>
		- 连接谓词中的列名称为<font color = "CC6600">「连接字段」</font>，并且各连接字段必须是可比的，但名字不必是相同的
	- 例如：查询每个学生及其选修课程的情况
		- 语句：SELECT Student.\*,SC.\*</br>&emsp;&emsp;&emsp;FROM Student,SC</br>&emsp;&emsp;&emsp;WHERE Student.Sno=SC.Sno
2. 自然连接
	若在<font color = "CC6600">「等值连接」</font>中把目标列中重复的属性列去掉则为自然连接
	- 例如：对上例用自然连接完成
	- 语句：SELECT Student.Sno,Sname,Ssex,Sage,Sdept,Cno,Grade</br>&emsp;&emsp;&emsp;FROM Student,SC</br>&emsp;&emsp;&emsp;WHERE Student.Sno = SC.Sno
	- 说明：为什么只有Sno列需要使用表名而后面几列不需要使用表名，因为Sno列是Student和SC共有的列，如果只用列名来表示那么DBMS将无法知道是显示Student表中的Sno还是显示SC表中的Sno，因此需要使用表名来区分，而Sname之类的列是每个表独有的，DBMS可以很容易的区分，因此不需要使用表名。<font color = "CC6600">「但是为了规范以及区分，还是建议加上表名进行区分提高可阅读性」</font>
3. 自身连接
	一个表与其自己进行连接
	- 说明：
		- 需要给表<font color = "CC6600">「起表名以示区别」</font>
		- 由于所有属性名都是同名属性，因此<font color = "CC6600">「必须使用别名前缀」</font>
	- 例如：查询每一门课的间接先修课（即先修课的先修课）
		- 语句：SELECT FIRST.Cno,SECOND.Cpno</br>&emsp;&emsp;&emsp;FROM Course FIRST,Course SECOND</br>&emsp;&emsp;&emsp;WHERE FIRST.Cpno = SECOND.C no
		- 说明：设置表的别名在`FROM`子句中进行设置
4. 外连接
	- 普通连接操作只输出满足连接条件的元组
	- <font color = "CC6600">「外连接」</font>操作以指定表为连接主体，将主体表中不满足连接条件的元组一并输出
	- 外连接又分为：<font color = "CC6600">「左外连接」</font>和<font color = "CC6600">「右外连接」</font>
		- 左外连接：列出左边关系中所有的元组
			- 语句：LEFT OUT JOIN \<表名> ON
		- 右外连接：列出右边关系中所有的元组
			- 语句：RIGHT OUT JOIN \<表名> ON
	- 例如：查询每个学生及其选修课程的情况
		- 语句：SELECT Student.Sno,Sname,Ssex,Sage,Sdept,Cno,Grade</br>&emsp;&emsp;&emsp;FROM Student,SC</br>&emsp;&emsp;&emsp;WHERE Student.Sno = SC.Sno
		- 上述语句可改为：SELECT Student.Sno,Sname,Ssex,Sage,Sdept,Cno,Grade</br>FROM Student <font color = "CC6600">LEFT OUT JOIN SC ON</font> (Student.Sno = SC.Sno)
		- 说明：LEFT OUT JOIN SC ON语句表示“Student表与SC表建立左外连接”
5. 多表连接
	连接操作时两个以上的表进行连接
	- 例如：查询每个学生的学号、姓名、选修的课程名及其成绩
		- 语句：SELECT Student.Sno,Sname,Cname,Grade</br>&emsp;&emsp;&emsp;FROM Student,SC,Course</br>&emsp;&emsp;&emsp;WHERE Student.Sno = SC.Sno AND SC.Cno = Course.Cno;

## <font color = "886600">嵌套查询</font>
一个`SELECT-FROM-WHERE`语句称为一个<font color = "CC6600">「查询块」</font>
嵌套查询定义：<strong>是指将一个查询块嵌套在另一个查询块的WHERE子句或HAVING短语的条件中的查询</strong>
- 说明：
	- 子查询中不能使用`ORDER BY`子句
	- 层层嵌套方式反映了SQL语言的结构化
	- 有些嵌套查询可以用连接运算替代
	- 外层查询（父查询）、内层查询（子查询）
- 例：SELECT Sname FROM Student（外层查询/父查询）</br>&emsp;&emsp;WHERE Sno IN (SELECT Sno FROM SC WHERE Cno = '2');（内层查询/子查询）
1. 带有`IN`谓词的子查询
	在嵌套查询中，子查询的结果往往是个集合，用`IN`谓词表示父查询的条件在子查询结果的集合中
	- 例：查询与“刘晨”在同一个系学习的学生
		- 方法一：此查询可以分步来完成
			- 第一步：确定“刘晨”所在系名
				- 语句：SELECT Sdept FROM Student WHERE Sname = '刘晨'
				- 结果为：CS
			- 第二步：根据上一步的查询结果查询所有在CS系学习的学生
				- 语句：SELECT Sno,Sname,Sdept</br>&emsp;&emsp;&emsp;FROM Student</br>&emsp;&emsp;&emsp;WHERE Sdept = 'CS'
			- 从上述两步中可以看出，第二步中的CS是由第一步的查询得到的结果，因此第二步中的'CS'可以使用第一步的语句替代
		- 方法二：将第一步查询嵌套到第二步查询的条件中
			- 语句：SELECT Sno,Sname,Sdept</br>&emsp;&emsp;&emsp;FROM Student</br>&emsp;&emsp;&emsp;WHERE Sdept <font color = "CC6600">IN (SELECT Sdept FROM Student WHERE Sname = '刘晨')</font>;
	- 说明
		- 不相关子查询：子查询的查询条件不依赖于父查询
		- 相关子查询：子查询的查询条件依赖于父查询，整个查询语句称为<font color = "CC6600">「嵌套查询」</font>
2. 带有比较运算符的子查询
	当能确切知道内层查询返回单值时，可用比较运算符（>，<，=，>=，<=，!=或<>）
	- 例如：查询与“刘晨”在同一个系学习的
		- 语句：SELECT Sno,Sname,Sdept</br>&emsp;&emsp;&emsp;FROM Student</br>&emsp;&emsp;&emsp;WHERE Sdept <font color = "CC6600">= (SELECT Sdept FROM Student WHERE Sname = '刘晨')</font>;
	- 例如：找出每个学生超过他选修课程平均成绩的课程号
		- 语句：SELECT Sno,Cno</br>&emsp;&emsp;&emsp;FROM SC x</br>&emsp;&emsp;&emsp;WHERE Grade >= (SELECT AVG(Grade) FROM SC y WHERE x.Sno = y.Sno);
3. 带有`ANY(SOME)`或`ALL`谓词的子查询
	- 谓词语义：
		- ANY：任意一个值
		- ALL：所有值
	- 在使用`ANY`或`ALL`的时候需要配合比较运算符一起使用：
		- ![773a3cb976fe1e35c1830b0b349b5d21.png](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202401061555356.png)
		- ![image.png](https://arturia-blog-1316646580.cos.ap-shanghai.myqcloud.com/ArturiaBlogPicGo/202401061555122.png)
	- 例：查询非计算机科学系中比计算机科学任意一个学生年龄小的学生姓名和年龄
		- 语句：SELECT Sname,Sage</br>&emsp;&emsp;&emsp;FROM Student</br>&emsp;&emsp;&emsp;WHERE Sage \<ANY (SELECT Sage FROM Student WHERE Sdept = 'CS')</br>&emsp;&emsp;&emsp;AND Sdept != 'CS';
	- 例：查询非计算机科学系中比计算机科学系所有学生年龄都小的学生姓名及年龄(使用`ALL`谓词)
		- 语句：SELECT Sname,Sage</br>&emsp;&emsp;&emsp;FROM Student</br>&emsp;&emsp;&emsp;WHERE Sage \<ALL (SELECT Sage FROM Student WHERE Sdept = 'CS')</br>&emsp;&emsp;&emsp;AND sdept != 'CS';
	- 说明：<font color = "BA8448">【用聚集函数实现子查询要比直接用ANY、ALL效率更高。】</font>
4. 带有`EXISTS`谓词的子查询
	- EXISTS谓词：EXISTS谓词代表存在量词∃，带有EXISTS谓词的子查询只返回逻辑真(True)或逻辑假(False)
	- 例：查询所有选修了1号课程的学生姓名
		- 思路分析：
			- 本查询涉及Student和SC关系
			- 在student中依次取每个Sno值，用此值取检查SC关系
			- 若SC中存在这样的元组，其Sno值等于此Student.Sno且Cno='1'，则取此Student.Sname送入结果关系中
		- 使用连接查询：
			- 语句：SELECT s1.Sname</br>&emsp;&emsp;&emsp;FROM Student AS s1,SC AS s2</br>&emsp;&emsp;&emsp;WHERE s1.Sno = s2.Sno AND s2.Cno = '1';
		- 使用嵌套查询：
			- 语句：SELECT Sname</br>&emsp;&emsp;&emsp;FROM Student</br>&emsp;&emsp;&emsp;WHERE EXISTS (SELECT \* FROM SC WHERE Sno = Student.Sno AND Cno = '1')
		- 说明：
			- 使用存在量词EXISTS后，若内层查询结果为空，则外层的WHERE子句返回真值；否则返回假值
			- 由EXISTS引出的子查询目标列都用\*，因为带EXISTS的子查询只返回真值或假值，给出列名无实际意义
5. `NOT EXISTS`谓词
	- 说明：
		- 若内层查询结果为非空，则外层的WHERE子句返回假值（查询到结果返回False）
		- 若内层查询结果为空，则外层的WHERE子句返回真值（没有查询到结果返回True）
	- 例：查询没有选修1号课程的学生姓名
		- 语句：SELECT Sname</br>&emsp;&emsp;&emsp;FROM Student</br>&emsp;&emsp;&emsp;WHERE NOT EXISTS (SELECT * FROM SC WHERE Sno = Student.Sno AND Cno = '1');