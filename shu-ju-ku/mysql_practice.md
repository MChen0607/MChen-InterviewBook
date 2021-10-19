---
description: >-
  leetcode数据库试题：https://leetcode-cn.com/problemset/database/?sorting=W3sic29ydE9yZGVyIjoiREVTQ0VORElORyIsIm9yZGVyQnkiOiJTT0xVVElPTl9OVU0ifV0%3D
---

# MySQL试题

## **178. 分数排名 \*\*\***

编写一个 SQL 查询来实现分数排名。

如果两个分数相同，则两个分数排名（Rank）相同。请注意，平分后的下一个名次应该是下一个连续的整数值。换句话说，名次之间不应该有“间隔”。

```
+----+-------+
| Id | Score |
+----+-------+
| 1  | 3.50  |
| 2  | 3.65  |
| 3  | 4.00  |
| 4  | 3.85  |
| 5  | 4.00  |
| 6  | 3.65  |
+----+-------+
```

例如，根据上述给定的 `Scores` 表，你的查询应该返回（按分数从高到低排列）：

```
+-------+------+
| Score | Rank |
+-------+------+
| 4.00  | 1    |
| 4.00  | 1    |
| 3.85  | 2    |
| 3.65  | 3    |
| 3.65  | 3    |
| 3.50  | 4    |
+-------+------+
```

**重要提示：**对于 MySQL 解决方案，如果要转义用作列名的保留字，可以在关键字之前和之后使用撇号。例如 **\`Rank\`**

**排名函数的使用**

```
select
    id 
   ,score
   ,rank() over(order by score desc) rank               --按照成绩排名，纯排名
   ,dense_rank() over(order by score desc) dense_rank   --按照成绩排名，相同成绩排名一致
   ,row_number() over(order by score desc) row_number   --按照成绩依次排名
   ,ntile(3) over (order by score desc) ntile         --按照分数划分成绩梯队
from scores;
```

结果

![](<../.gitbook/assets/image (14).png>)

### 解答

```
select
    score as "Score",
    dense_rank() over (order by score desc) as "Rank" 
from
    scores;
```



## **175. 组合两个表**

表1: `Person`

```
+-------------+---------+
| 列名         | 类型     |
+-------------+---------+
| PersonId    | int     |
| FirstName   | varchar |
| LastName    | varchar |
+-------------+---------+
PersonId 是上表主键
```

表2: `Address`

```
+-------------+---------+
| 列名         | 类型    |
+-------------+---------+
| AddressId   | int     |
| PersonId    | int     |
| City        | varchar |
| State       | varchar |
+-------------+---------+
AddressId 是上表主键
```

编写一个 SQL 查询，满足条件：**无论 person 是否有地址信息，都需要基于上述两表提供 person 的以下信息：**

```
FirstName, LastName, City, State
```

### 解答

```
# 需要用join才能，where语句不能查找没有地址信息的人
select FirstName, LastName, City, State
From Person left join Address
on  Person.PersonId = Address.PersonId;
```



## **176. 第二高的薪水 \*\***

编写一个 SQL 查询，获取 `Employee` 表中第二高的薪水（Salary） 。

```
+----+--------+
| Id | Salary |
+----+--------+
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |
+----+--------+
```

例如上述 `Employee` 表，SQL查询应该返回 `200` 作为第二高的薪水。**如果不存在第二高的薪水，那么查询应返回 `null`**。

```
+---------------------+
| SecondHighestSalary |
+---------------------+
| 200                 |
+---------------------+
```

### 解答

```
# 需要DISTINCT来找出真正的第二个。
SELECT
    IFNULL(
      (SELECT DISTINCT Salary
       FROM Employee
       ORDER BY Salary DESC
        LIMIT 1 OFFSET 1),
    NULL) AS SecondHighestSalary;
    
    
SELECT
    (SELECT DISTINCT
            Salary
        FROM
            Employee
        ORDER BY Salary DESC
        LIMIT 1 OFFSET 1) AS SecondHighestSalary;
```



## **177. 第N高的薪水**

编写一个 SQL 查询，获取 `Employee` 表中第 _n _高的薪水（Salary）。

```
+----+--------+
| Id | Salary |
+----+--------+
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |
+----+--------+
```

例如上述 `Employee` 表，_n = 2 _时，应返回第二高的薪水 `200`。如果不存在第 _n _高的薪水，那么查询应返回 `null`。

```
+------------------------+
| getNthHighestSalary(2) |
+------------------------+
| 200                    |
+------------------------+
```

### 解答

```
# 窗口函数 370ms
CREATE FUNCTION getNthHighestSalary ( N INT ) RETURNS INT BEGIN
	RETURN (
		# Write your MySQL query statement below.
		SELECT
			(
				SELECT DISTINCT 
					Salary 
				FROM ( 
					SELECT 
						Salary, 
						dense_rank() over ( ORDER BY Salary DESC ) AS R 
					FROM Employee 
				) AS t
				WHERE 
				t.R = N 
			) 
		);
END

# GROUP BY  371ms
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
    SET N := N-1;
  RETURN (
      # Write your MySQL query statement below.
      SELECT 
            salary
      FROM 
            employee
      GROUP BY 
            salary
      ORDER BY 
            salary DESC
      LIMIT N, 1
  );
END
# where子句  749ms
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
  RETURN (
      # Write your MySQL query statement below.
      SELECT 
          DISTINCT e.salary
      FROM 
          employee e
      WHERE 
          (SELECT count(DISTINCT salary) FROM employee WHERE salary>e.salary) = N-1
  );
END

#自定义变量 *****
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
  RETURN (
      # Write your MySQL query statement below.
      SELECT 
          DISTINCT salary 
      FROM 
          (SELECT 
                salary, @r:=IF(@p=salary, @r, @r+1) AS rnk,  @p:= salary 
            FROM  
                employee, (SELECT @r:=0, @p:=NULL)init 
            ORDER BY 
                salary DESC) tmp
      WHERE rnk = N
  );
END

作者：luanhz
链接：https://leetcode-cn.com/problems/nth-highest-salary/solution/mysql-zi-ding-yi-bian-liang-by-luanz/
```



## **181. 超过经理收入的员工**

`Employee` 表包含所有员工，他们的经理也属于员工。每个员工都有一个 Id，此外还有一列对应员工的经理的 Id。

```
+----+-------+--------+-----------+
| Id | Name  | Salary | ManagerId |
+----+-------+--------+-----------+
| 1  | Joe   | 70000  | 3         |
| 2  | Henry | 80000  | 4         |
| 3  | Sam   | 60000  | NULL      |
| 4  | Max   | 90000  | NULL      |
+----+-------+--------+-----------+
```

给定 `Employee` 表，编写一个 SQL 查询，该查询可以获取收入超过他们经理的员工的姓名。在上面的表格中，Joe 是唯一一个收入超过他的经理的员工。

```
+----------+
| Employee |
+----------+
| Joe      |
+----------+
```

### 解答

```
# JOIN内部联结
SELECT
     a.NAME AS Employee
FROM Employee AS a JOIN Employee AS b
     ON a.ManagerId = b.Id
     AND a.Salary > b.Salary;

# where子句
Select
    a.Name AS Employee
FROM
    Employee AS a,Employee AS b
where
    a.ManagerId = b.Id
    and
    a.Salary > b.Salary;
```

## **184. 部门工资最高的员工\*\***

`Employee` 表包含所有员工信息，每个员工有其对应的 Id, salary 和 department Id。

```
+----+-------+--------+--------------+
| Id | Name  | Salary | DepartmentId |
+----+-------+--------+--------------+
| 1  | Joe   | 70000  | 1            |
| 2  | Jim   | 90000  | 1            |
| 3  | Henry | 80000  | 2            |
| 4  | Sam   | 60000  | 2            |
| 5  | Max   | 90000  | 1            |
+----+-------+--------+--------------+
```

`Department` 表包含公司所有部门的信息。

```
+----+----------+
| Id | Name     |
+----+----------+
| 1  | IT       |
| 2  | Sales    |
+----+----------+
```

编写一个 SQL 查询，找出每个部门工资最高的员工。对于上述表，您的 SQL 查询应返回以下行（行的顺序无关紧要）。

```
+------------+----------+--------+
| Department | Employee | Salary |
+------------+----------+--------+
| IT         | Max      | 90000  |
| IT         | Jim      | 90000  |
| Sales      | Henry    | 80000  |
+------------+----------+--------+
```

**解释：**

Max 和 Jim 在 IT 部门的工资都是最高的，Henry 在销售部的工资最高。

### 解答

```
# 两个字段使用IN：（A,B）IN ();
Select 
    Department.Name AS Department,
    Employee.Name AS Employee,
    Employee.Salary
FROM
    Employee join Department   
    on Employee.DepartmentId = Department.Id
where
    (Employee.DepartmentId,Employee.Salary) IN (
        Select
            DepartmentId,
            Max(Employee.Salary) AS Salary
        FROM
            Employee
        Group BY DepartmentId
    );
```

## **185. 部门工资前三高的所有员工\*\*\***

`Employee` 表包含所有员工信息，每个员工有其对应的工号 `Id`，姓名 `Name`，工资 `Salary` 和部门编号 `DepartmentId` 。

```
+----+-------+--------+--------------+
| Id | Name  | Salary | DepartmentId |
+----+-------+--------+--------------+
| 1  | Joe   | 85000  | 1            |
| 2  | Henry | 80000  | 2            |
| 3  | Sam   | 60000  | 2            |
| 4  | Max   | 90000  | 1            |
| 5  | Janet | 69000  | 1            |
| 6  | Randy | 85000  | 1            |
| 7  | Will  | 70000  | 1            |
+----+-------+--------+--------------+
```

`Department` 表包含公司所有部门的信息。

```
+----+----------+
| Id | Name     |
+----+----------+
| 1  | IT       |
| 2  | Sales    |
+----+----------+
```

编写一个 SQL 查询，找出每个部门获得前三高工资的所有员工。例如，根据上述给定的表，查询结果应返回：

```
+------------+----------+--------+
| Department | Employee | Salary |
+------------+----------+--------+
| IT         | Max      | 90000  |
| IT         | Randy    | 85000  |
| IT         | Joe      | 85000  |
| IT         | Will     | 70000  |
| Sales      | Henry    | 80000  |
| Sales      | Sam      | 60000  |
+------------+----------+--------+
```

**解释：**

IT 部门中，Max 获得了最高的工资，Randy 和 Joe 都拿到了第二高的工资，Will 的工资排第三。销售部门（Sales）只有两名员工，Henry 的工资最高，Sam 的工资排第二。

### 解答

```
SELECT
    d.Name AS 'Department', e1.Name AS 'Employee', e1.Salary
FROM
    Employee e1
        JOIN
    Department d ON e1.DepartmentId = d.Id
WHERE
    3 > (SELECT
            COUNT(DISTINCT e2.Salary)
        FROM
            Employee e2
        WHERE
            e2.Salary > e1.Salary
                AND e1.DepartmentId = e2.DepartmentId
        );

# 内库 dense函数  PARTITION BY.
SELECT Department, Name AS Employee, Salary
FROM(
    SELECT D.Name AS Department, E.Name AS Name, Salary,
    DENSE_RANK() OVER (PARTITION BY E.DepartmentId ORDER BY Salary DESC) AS R
    FROM Employee AS E INNER JOIN Department AS D
    ON E.DepartmentId = D.Id) AS C
WHERE C.R <= 3;
```

> rank()内置函数：引入的版本为MySQL8.0 [https://dev.mysql.com/doc/refman/8.0/en/window-function-descriptions.html#function\_dense-rank](https://dev.mysql.com/doc/refman/8.0/en/window-function-descriptions.html#function\_dense-rank)&#x20;

## **182. 查找重复的电子邮箱**

编写一个 SQL 查询，查找 `Person` 表中所有重复的电子邮箱。

**示例：**

```
+----+---------+
| Id | Email   |
+----+---------+
| 1  | a@b.com |
| 2  | c@d.com |
| 3  | a@b.com |
+----+---------+
```

根据以上输入，你的查询应返回以下结果：

```
+---------+
| Email   |
+---------+
| a@b.com |
+---------+
```

**说明：**所有电子邮箱都是小写字母。

### 解答

```
# where子句
Select DISTINCT
    a.Email
FROM
    Person AS a,Person AS b
Where
    a.id != b.id
    and a.Email = b.Email;

# 内部联结
Select DISTINCT
    a.Email
FROM
    Person AS a inner join Person AS b on 
        a.id != b.id
        and a.Email = b.Email;

#分组
select
    email 
from
    person
group by 
    email
having 
    count(email) > 1;
    
# 临时表
SELECT
	Email 
FROM
	( SELECT Email, count( Email ) AS num FROM Person GROUP BY Email ) AS statistic 
WHERE
	num > 1;
```



## **183. 从不订购的客户\***

某网站包含两个表，`Customers` 表和 `Orders` 表。编写一个 SQL 查询，找出所有从不订购任何东西的客户。

`Customers` 表：

```
+----+-------+
| Id | Name  |
+----+-------+
| 1  | Joe   |
| 2  | Henry |
| 3  | Sam   |
| 4  | Max   |
+----+-------+
```

`Orders` 表：

```
+----+------------+
| Id | CustomerId |
+----+------------+
| 1  | 3          |
| 2  | 1          |
+----+------------+
```

例如给定上述表格，你的查询应返回：

```
+-----------+
| Customers |
+-----------+
| Henry     |
| Max       |
+-----------+
```

### 解答

```
# 左连接
SELECT 
	NAME AS Customers 
FROM
	( SELECT Customers.NAME, Orders.Id 
		FROM Customers LEFT JOIN Orders ON 
			Customers.Id = Orders.CustomerId 
	) AS n 
WHERE
	n.Id IS NULL;
	


SELECT 
	NAME AS Customers 
FROM
	Customers 
WHERE
	Customers.Id NOT IN ( SELECT Orders.CustomerId FROM Orders );
```



## **180. 连续出现的数字\*\*\***

表：`Logs`

```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| num         | varchar |
+-------------+---------+
id 是这个表的主键。
```

编写一个 SQL 查询，查找所有至少连续出现三次的数字。

返回的结果表中的数据可以按 **任意顺序** 排列。

查询结果格式如下面的例子所示：

```
Logs 表：
+----+-----+
| Id | Num |
+----+-----+
| 1  | 1   |
| 2  | 1   |
| 3  | 1   |
| 4  | 2   |
| 5  | 1   |
| 6  | 2   |
| 7  | 2   |
+----+-----+

Result 表：
+-----------------+
| ConsecutiveNums |
+-----------------+
| 1               |
+-----------------+
1 是唯一连续出现至少三次的数字。
```

### 解答

```
# 三个表联结
SELECT DISTINCT
	l1.Num AS ConsecutiveNums 
FROM
	LOGS l1,
	LOGS l2,
	LOGS l3 
WHERE
	l1.Id = l2.Id - 1 
	AND l2.Id = l3.Id - 1 
	AND l1.Num = l2.Num 
	AND l2.Num = l3.Num;
```



## **596. 超过5名学生的课**

有一个`courses` 表 ，有: **student (学生) **和 **class (课程)**。

请列出所有超过或等于5名学生的课。

例如，表：

```
+---------+------------+
| student | class      |
+---------+------------+
| A       | Math       |
| B       | English    |
| C       | Math       |
| D       | Biology    |
| E       | Math       |
| F       | Computer   |
| G       | Math       |
| H       | Math       |
| I       | Math       |
+---------+------------+
```

应该输出:

```
+---------+
| class   |
+---------+
| Math    |
+---------+
```

**提示：**

* 学生在每个课中不应被重复计算。

### 解答

```
Select
    class
FROM
    courses
Group by
    class
Having count(DISTINCT student) >= 5;
```



## **197. 上升的温度**

表 `Weather`

```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| recordDate    | date    |
| temperature   | int     |
+---------------+---------+
id 是这个表的主键
该表包含特定日期的温度信息
```

编写一个 SQL 查询，来查找与之前（昨天的）日期相比温度更高的所有日期的 `id` 。

返回结果 **不要求顺序** 。

查询结果格式如下例：

```
Weather
+----+------------+-------------+
| id | recordDate | Temperature |
+----+------------+-------------+
| 1  | 2015-01-01 | 10          |
| 2  | 2015-01-02 | 25          |
| 3  | 2015-01-03 | 20          |
| 4  | 2015-01-04 | 30          |
+----+------------+-------------+

Result table:
+----+
| id |
+----+
| 2  |
| 4  |
+----+
2015-01-02 的温度比前一天高（10 -> 25）
2015-01-04 的温度比前一天高（20 -> 30）
```

### 解答

```
# datediff时间内置函数
SELECT
	W1.id AS Id 
FROM
	Weather W1
	JOIN Weather W2 ON datediff( W1.recordDate, W2.recordDate ) = 1 
	AND W1.temperature > W2.temperature;
```



## **196. 删除重复的电子邮箱\*\*\***

编写一个 SQL 查询，来删除 `Person` 表中所有重复的电子邮箱，重复的邮箱里只保留 **Id **_最小 _的那个。

```
+----+------------------+
| Id | Email            |
+----+------------------+
| 1  | john@example.com |
| 2  | bob@example.com  |
| 3  | john@example.com |
+----+------------------+
Id 是这个表的主键。
```

例如，在运行你的查询语句之后，上面的 `Person` 表应返回以下几行:

```
+----+------------------+
| Id | Email            |
+----+------------------+
| 1  | john@example.com |
| 2  | bob@example.com  |
+----+------------------+
```

**提示：**

* 执行 SQL 之后，输出是整个 `Person` 表。
* 使用 `delete` 语句。

### 解答

```
# where子句****
DELETE p1 
FROM
	Person p1,
	Person p2 
WHERE
	p1.Email = p2.Email 
	AND p1.Id > p2.Id;

# IN语句
DELETE 
FROM
	Person 
WHERE
	Id NOT IN ( 
		SELECT 
			t.Id 
		FROM ( 
			SELECT 
				Email, min( Id ) AS Id 
			FROM 
				Person 
			GROUP BY Email 
		) AS t 
	);
```

###

## **601. 体育馆的人流量 \*\*\*\***

```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| visit_date    | date    |
| people        | int     |
+---------------+---------+
visit_date 是表的主键
每日人流量信息被记录在这三列信息中：序号 (id)、日期 (visit_date)、 人流量 (people)
每天只有一行记录，日期随着 id 的增加而增加
```

编写一个 SQL 查询以找出每行的人数大于或等于 `100` 且 `id` 连续的三行或更多行记录。

返回按 `visit_date` 升序排列的结果表。

查询结果格式如下所示。

```
Stadium table:
+------+------------+-----------+
| id   | visit_date | people    |
+------+------------+-----------+
| 1    | 2017-01-01 | 10        |
| 2    | 2017-01-02 | 109       |
| 3    | 2017-01-03 | 150       |
| 4    | 2017-01-04 | 99        |
| 5    | 2017-01-05 | 145       |
| 6    | 2017-01-06 | 1455      |
| 7    | 2017-01-07 | 199       |
| 8    | 2017-01-09 | 188       |
+------+------------+-----------+

Result table:
+------+------------+-----------+
| id   | visit_date | people    |
+------+------------+-----------+
| 5    | 2017-01-05 | 145       |
| 6    | 2017-01-06 | 1455      |
| 7    | 2017-01-07 | 199       |
| 8    | 2017-01-09 | 188       |
+------+------------+-----------+
id 为 5、6、7、8 的四行 id 连续，并且每行都有 >= 100 的人数记录。
请注意，即使第 7 行和第 8 行的 visit_date 不是连续的，输出也应当包含第 8 行，因为我们只需要考虑 id 连续的记录。
不输出 id 为 2 和 3 的行，因为至少需要三条 id 连续的记录。
```

### 解答

```
-- 查出下标重复次数大于等于3的数据
select id, visit_date, people from
(
-- 计算id减去下标后出现的重复次数
select 
	*,
	count(*) over (PARTITION by templete.oder) as oder2
from 
		(
		-- 用id减去数据下标值，如果id连续中断id减下标的值会增大否则都相同
		select 
			*,
			row_number() over(order by id asc) as `rank`,
			id - row_number() over(order by id asc) as `oder`
		from (
					-- 查出人数大于等于100的数据
					select * 
					from stadium
					where people >= 100
					) as s1) as templete
) templete2
-- 下标重复次数大于等于3的数据
where templete2.oder2 >= 3


# Write your MySQL query statement below
SELECT DISTINCT
	t1.*
FROM
	stadium t1,
	stadium t2,
	stadium t3 
WHERE
	t1.people >= 100 
	AND t2.people >= 100 
	AND t3.people >= 100 
	AND (        
		( t1.id - t2.id = 1 AND t1.id - t3.id = 2 AND t2.id - t3.id = 1 ) -- t1, t2, t3		
		OR ( t2.id - t1.id = 1 AND t2.id - t3.id = 2 AND t1.id - t3.id = 1 ) -- t2, t1, t3	
		OR ( t3.id - t2.id = 1 AND t2.id - t1.id = 1 AND t3.id - t1.id = 2 ) -- t3, t2, t1		
	) 
ORDER BY
	t1.id;
```



## **262. 行程和用户\*\*\***

表：`Trips`

```
+-------------+----------+
| Column Name | Type     |
+-------------+----------+
| Id          | int      |
| Client_Id   | int      |
| Driver_Id   | int      |
| City_Id     | int      |
| Status      | enum     |
| Request_at  | date     |     
+-------------+----------+
Id 是这张表的主键。
这张表中存所有出租车的行程信息。每段行程有唯一 Id ，其中 Client_Id 和 Driver_Id 是 Users 表中 Users_Id 的外键。
Status 是一个表示行程状态的枚举类型，枚举成员为(‘completed’, ‘cancelled_by_driver’, ‘cancelled_by_client’) 。
```

表：`Users`

```
+-------------+----------+
| Column Name | Type     |
+-------------+----------+
| Users_Id    | int      |
| Banned      | enum     |
| Role        | enum     |
+-------------+----------+
Users_Id 是这张表的主键。
这张表中存所有用户，每个用户都有一个唯一的 Users_Id ，Role 是一个表示用户身份的枚举类型，枚举成员为 (‘client’, ‘driver’, ‘partner’) 。
Banned 是一个表示用户是否被禁止的枚举类型，枚举成员为 (‘Yes’, ‘No’) 。
```

写一段 SQL 语句查出 `"2013-10-01"`** **至 `"2013-10-03"`** **期间非禁止用户（**乘客和司机都必须未被禁止**）的取消率。非禁止用户即 Banned 为 No 的用户，禁止用户即 Banned 为 Yes 的用户。

**取消率** 的计算方式如下：(被司机或乘客取消的非禁止用户生成的订单数量) / (非禁止用户生成的订单总数)。

返回结果表中的数据可以按任意顺序组织。其中取消率 `Cancellation Rate` 需要四舍五入保留 **两位小数** 。

查询结果格式如下例所示：

```
Trips 表：
+----+-----------+-----------+---------+---------------------+------------+
| Id | Client_Id | Driver_Id | City_Id | Status              | Request_at |
+----+-----------+-----------+---------+---------------------+------------+
| 1  | 1         | 10        | 1       | completed           | 2013-10-01 |
| 2  | 2         | 11        | 1       | cancelled_by_driver | 2013-10-01 |
| 3  | 3         | 12        | 6       | completed           | 2013-10-01 |
| 4  | 4         | 13        | 6       | cancelled_by_client | 2013-10-01 |
| 5  | 1         | 10        | 1       | completed           | 2013-10-02 |
| 6  | 2         | 11        | 6       | completed           | 2013-10-02 |
| 7  | 3         | 12        | 6       | completed           | 2013-10-02 |
| 8  | 2         | 12        | 12      | completed           | 2013-10-03 |
| 9  | 3         | 10        | 12      | completed           | 2013-10-03 |
| 10 | 4         | 13        | 12      | cancelled_by_driver | 2013-10-03 |
+----+-----------+-----------+---------+---------------------+------------+

Users 表：
+----------+--------+--------+
| Users_Id | Banned | Role   |
+----------+--------+--------+
| 1        | No     | client |
| 2        | Yes    | client |
| 3        | No     | client |
| 4        | No     | client |
| 10       | No     | driver |
| 11       | No     | driver |
| 12       | No     | driver |
| 13       | No     | driver |
+----------+--------+--------+

Result 表：
+------------+-------------------+
| Day        | Cancellation Rate |
+------------+-------------------+
| 2013-10-01 | 0.33              |
| 2013-10-02 | 0.00              |
| 2013-10-03 | 0.50              |
+------------+-------------------+

2013-10-01：
  - 共有 4 条请求，其中 2 条取消。
  - 然而，Id=2 的请求是由禁止用户（User_Id=2）发出的，所以计算时应当忽略它。
  - 因此，总共有 3 条非禁止请求参与计算，其中 1 条取消。
  - 取消率为 (1 / 3) = 0.33
2013-10-02：
  - 共有 3 条请求，其中 0 条取消。
  - 然而，Id=6 的请求是由禁止用户发出的，所以计算时应当忽略它。
  - 因此，总共有 2 条非禁止请求参与计算，其中 0 条取消。
  - 取消率为 (0 / 2) = 0.00
2013-10-03：
  - 共有 3 条请求，其中 1 条取消。
  - 然而，Id=8 的请求是由禁止用户发出的，所以计算时应当忽略它。
  - 因此，总共有 2 条非禁止请求参与计算，其中 1 条取消。
  - 取消率为 (1 / 2) = 0.50
```

### 解答

```
SELECT
	T.request_at AS `Day`,
	ROUND( SUM( IF ( T.STATUS = 'completed', 0, 1 ) ) / COUNT( T.STATUS ), 2 ) AS `Cancellation Rate` 
FROM
	trips AS T 
WHERE
	T.Client_Id NOT IN ( SELECT users_id FROM users WHERE banned = 'Yes' ) 
	AND T.Driver_Id NOT IN ( SELECT users_id FROM users WHERE banned = 'Yes' ) 
	AND T.request_at BETWEEN '2013-10-01' 
	AND '2013-10-03' 
GROUP BY
	T.request_at;
	

SELECT T.request_at AS `Day`, 
	ROUND(
			SUM(
				IF(T.STATUS = 'completed',0,1)
			)
			/ 
			COUNT(T.STATUS),
			2
	) AS `Cancellation Rate`
FROM Trips AS T
JOIN Users AS U1 ON (T.client_id = U1.users_id AND U1.banned ='No')
JOIN Users AS U2 ON (T.driver_id = U2.users_id AND U2.banned ='No')
WHERE T.request_at BETWEEN '2013-10-01' AND '2013-10-03'
GROUP BY T.request_at;
```

###

## **627. 变更性别**

给定一个 `salary` 表，如下所示，有 m = 男性 和 f = 女性 的值。交换所有的 f 和 m 值（例如，将所有 f 值更改为 m，反之亦然）。要求只使用一个更新（Update）语句，并且没有中间的临时表。

注意，您必只能写一个 Update 语句，请不要编写任何 Select 语句。

**例如：**

```
| id | name | sex | salary |
|----|------|-----|--------|
| 1  | A    | m   | 2500   |
| 2  | B    | f   | 1500   |
| 3  | C    | m   | 5500   |
| 4  | D    | f   | 500    |
```

运行你所编写的更新语句之后，将会得到以下表:

```
| id | name | sex | salary |
|----|------|-----|--------|
| 1  | A    | f   | 2500   |
| 2  | B    | m   | 1500   |
| 3  | C    | f   | 5500   |
| 4  | D    | m   | 500    |
```

### 解答

```
# IF
UPDATE salary 
SET sex = IF( sex = 'm', 'f', 'm' );

# CASE THEN
UPDATE salary
SET
    sex = 
        CASE sex
            WHEN 'm' THEN 'f'
            ELSE 'm'
        END;
```



## **620. 有趣的电影**

某城市开了一家新的电影院，吸引了很多人过来看电影。该电影院特别注意用户体验，专门有个 LED显示板做电影推荐，上面公布着影评和相关电影描述。

作为该电影院的信息部主管，您需要编写一个 SQL查询，找出所有影片描述为**非** `boring` (不无聊) 的并且** id 为奇数 **的影片，结果请按等级 `rating` 排列。

例如，下表 `cinema`:

```
+---------+-----------+--------------+-----------+
|   id    | movie     |  description |  rating   |
+---------+-----------+--------------+-----------+
|   1     | War       |   great 3D   |   8.9     |
|   2     | Science   |   fiction    |   8.5     |
|   3     | irish     |   boring     |   6.2     |
|   4     | Ice song  |   Fantacy    |   8.6     |
|   5     | House card|   Interesting|   9.1     |
+---------+-----------+--------------+-----------+
```

对于上面的例子，则正确的输出是为：

```
+---------+-----------+--------------+-----------+
|   id    | movie     |  description |  rating   |
+---------+-----------+--------------+-----------+
|   5     | House card|   Interesting|   9.1     |
|   1     | War       |   great 3D   |   8.9     |
+---------+-----------+--------------+-----------+
```

### 解答

```
select
    id,
    movie,
    description,
    rating
FROM
    cinema
where
    id % 2 = 1
    and description != 'boring'
order by
    rating DESC;

# mod()
select
    id,
    movie,
    description,
    rating
FROM
    cinema
where
    mod(id, 2) = 1
    and description != 'boring'
order by
    rating DESC;
```



## **595. 大的国家**

这里有张 `World` 表

```
+-----------------+------------+------------+--------------+---------------+
| name            | continent  | area       | population   | gdp           |
+-----------------+------------+------------+--------------+---------------+
| Afghanistan     | Asia       | 652230     | 25500100     | 20343000      |
| Albania         | Europe     | 28748      | 2831741      | 12960000      |
| Algeria         | Africa     | 2381741    | 37100000     | 188681000     |
| Andorra         | Europe     | 468        | 78115        | 3712000       |
| Angola          | Africa     | 1246700    | 20609294     | 100990000     |
+-----------------+------------+------------+--------------+---------------+
```

如果一个国家的面积超过 300 万平方公里，或者人口超过 2500 万，那么这个国家就是大国家。

编写一个 SQL 查询，输出表中所有大国家的名称、人口和面积。

例如，根据上表，我们应该输出:

```
+--------------+-------------+--------------+
| name         | population  | area         |
+--------------+-------------+--------------+
| Afghanistan  | 25500100    | 652230       |
| Algeria      | 37100000    | 2381741      |
+--------------+-------------+--------------+
```

### 解答

```
Select
    name,
    population,
    area
FROM
    World
where
    area >= 3000000
    or population >= 25000000;

# UNION
SELECT
    name, population, area
FROM
    world
WHERE
    area > 3000000

UNION

SELECT
    name, population, area
FROM
    world
WHERE
    population > 25000000;
```



## **1179. 重新格式化部门表\***

部门表 `Department`：

```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| revenue       | int     |
| month         | varchar |
+---------------+---------+
(id, month) 是表的联合主键。
这个表格有关于每个部门每月收入的信息。
月份（month）可以取下列值 ["Jan","Feb","Mar","Apr","May","Jun","Jul","Aug","Sep","Oct","Nov","Dec"]。
```

编写一个 SQL 查询来重新格式化表，使得新的表中有一个部门 id 列和一些对应 **每个月 **的收入（revenue）列。

查询结果格式如下面的示例所示：

```
Department 表：
+------+---------+-------+
| id   | revenue | month |
+------+---------+-------+
| 1    | 8000    | Jan   |
| 2    | 9000    | Jan   |
| 3    | 10000   | Feb   |
| 1    | 7000    | Feb   |
| 1    | 6000    | Mar   |
+------+---------+-------+

查询得到的结果表：
+------+-------------+-------------+-------------+-----+-------------+
| id   | Jan_Revenue | Feb_Revenue | Mar_Revenue | ... | Dec_Revenue |
+------+-------------+-------------+-------------+-----+-------------+
| 1    | 8000        | 7000        | 6000        | ... | null        |
| 2    | 9000        | null        | null        | ... | null        |
| 3    | null        | 10000       | null        | ... | null        |
+------+-------------+-------------+-------------+-----+-------------+

注意，结果表有 13 列 (1个部门 id 列 + 12个月份的收入列)。
```

### 解答

```
select id,
    sum(case month when 'Jan' then revenue end) as Jan_Revenue,
    sum(case month when 'Feb' then revenue end) as Feb_Revenue,
    sum(case month when 'Mar' then revenue end) as Mar_Revenue,
    sum(case month when 'Apr' then revenue end) as Apr_Revenue,
    sum(case month when 'May' then revenue end) as May_Revenue,
    sum(case month when 'Jun' then revenue end) as Jun_Revenue,
    sum(case month when 'Jul' then revenue end) as Jul_Revenue,
    sum(case month when 'Aug' then revenue end) as Aug_Revenue,
    sum(case month when 'Sep' then revenue end) as Sep_Revenue,
    sum(case month when 'Oct' then revenue end) as Oct_Revenue,
    sum(case month when 'Nov' then revenue end) as Nov_Revenue,
    sum(case month when 'Dec' then revenue end) as Dec_Revenue
from Department
group by id
order by id;
```

## **626. 换座位\***

小美是一所中学的信息科技老师，她有一张 `seat` 座位表，平时用来储存学生名字和与他们相对应的座位 id。

其中纵列的 **id **是连续递增的

小美想改变相邻俩学生的座位。

你能不能帮她写一个 SQL query 来输出小美想要的结果呢？

**示例：**

```
+---------+---------+
|    id   | student |
+---------+---------+
|    1    | Abbot   |
|    2    | Doris   |
|    3    | Emerson |
|    4    | Green   |
|    5    | Jeames  |
+---------+---------+
```

假如数据输入的是上表，则输出结果如下：

```
+---------+---------+
|    id   | student |
+---------+---------+
|    1    | Doris   |
|    2    | Abbot   |
|    3    | Green   |
|    4    | Emerson |
|    5    | Jeames  |
+---------+---------+
```

**注意：**

如果学生人数是奇数，则不需要改变最后一个同学的座位。

### 解答

```
SELECT
    (CASE
        WHEN MOD(id, 2) != 0 AND counts != id THEN id + 1
        WHEN MOD(id, 2) != 0 AND counts = id THEN id
        ELSE id - 1
    END) AS id,
    student
FROM
    seat,
    (SELECT
        COUNT(*) AS counts
    FROM
        seat) AS seat_counts
ORDER BY id ;

SELECT
	IF
		(
			id % 2 = 1 AND counts != id,
			id + 1,
			IF ( id % 2 = 1 AND counts = id, id, id - 1 ) 
		) AS id,
	student 
FROM
		seat,
		( SELECT COUNT(*) AS counts FROM seat ) AS seat_counts 
ORDER BY
	id;
```
