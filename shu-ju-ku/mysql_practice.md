# MySQL试题

## **178. 分数排名 \*\*\***

编写一个 SQL 查询来实现分数排名。

如果两个分数相同，则两个分数排名（Rank）相同。请注意，平分后的下一个名次应该是下一个连续的整数值。换句话说，名次之间不应该有“间隔”。

```text
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

```text
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

```text
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

![](../.gitbook/assets/image%20%2814%29.png)

### 解答

```text
select
    score as "Score",dense_rank() over (order by score desc) as "Rank" 
from
    scores;
```



## **175. 组合两个表**

表1: `Person`

```text
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

```text
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

```text
FirstName, LastName, City, State
```

### 解答

```text
# 需要用join才能，where语句不能查找没有地址信息的人
select FirstName, LastName, City, State
From Person left join Address
on  Person.PersonId = Address.PersonId;
```



## **176. 第二高的薪水 \*\***

编写一个 SQL 查询，获取 `Employee` 表中第二高的薪水（Salary） 。

```text
+----+--------+
| Id | Salary |
+----+--------+
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |
+----+--------+
```

例如上述 `Employee` 表，SQL查询应该返回 `200` 作为第二高的薪水。**如果不存在第二高的薪水，那么查询应返回 `null`**。

```text
+---------------------+
| SecondHighestSalary |
+---------------------+
| 200                 |
+---------------------+
```

### 解答

```text
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

编写一个 SQL 查询，获取 `Employee` 表中第 _n_ 高的薪水（Salary）。

```text
+----+--------+
| Id | Salary |
+----+--------+
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |
+----+--------+
```

例如上述 `Employee` 表，_n = 2_ 时，应返回第二高的薪水 `200`。如果不存在第 _n_ 高的薪水，那么查询应返回 `null`。

```text
+------------------------+
| getNthHighestSalary(2) |
+------------------------+
| 200                    |
+------------------------+
```

### 解答

```text
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

```text
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

```text
+----------+
| Employee |
+----------+
| Joe      |
+----------+
```

### 解答

```text
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

```text
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

```text
+----+----------+
| Id | Name     |
+----+----------+
| 1  | IT       |
| 2  | Sales    |
+----+----------+
```

编写一个 SQL 查询，找出每个部门工资最高的员工。对于上述表，您的 SQL 查询应返回以下行（行的顺序无关紧要）。

```text
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

```text
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

```text
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

```text
+----+----------+
| Id | Name     |
+----+----------+
| 1  | IT       |
| 2  | Sales    |
+----+----------+
```

编写一个 SQL 查询，找出每个部门获得前三高工资的所有员工。例如，根据上述给定的表，查询结果应返回：

```text
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

```text
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

> rank\(\)内置函数：引入的版本为MySQL8.0 [https://dev.mysql.com/doc/refman/8.0/en/window-function-descriptions.html\#function\_dense-rank](https://dev.mysql.com/doc/refman/8.0/en/window-function-descriptions.html#function_dense-rank)

## **182. 查找重复的电子邮箱**

编写一个 SQL 查询，查找 `Person` 表中所有重复的电子邮箱。

**示例：**

```text
+----+---------+
| Id | Email   |
+----+---------+
| 1  | a@b.com |
| 2  | c@d.com |
| 3  | a@b.com |
+----+---------+
```

根据以上输入，你的查询应返回以下结果：

```text
+---------+
| Email   |
+---------+
| a@b.com |
+---------+
```

**说明：**所有电子邮箱都是小写字母。

### 解答

```text
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

```text
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

```text
+----+------------+
| Id | CustomerId |
+----+------------+
| 1  | 3          |
| 2  | 1          |
+----+------------+
```

例如给定上述表格，你的查询应返回：

```text
+-----------+
| Customers |
+-----------+
| Henry     |
| Max       |
+-----------+
```

### 解答

```text
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

```text
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

```text
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

```text
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

有一个`courses` 表 ，有: **student \(学生\)** 和 **class \(课程\)**。

请列出所有超过或等于5名学生的课。

例如，表：

```text
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

```text
+---------+
| class   |
+---------+
| Math    |
+---------+
```

**提示：**

* 学生在每个课中不应被重复计算。

### 解答

```text
Select
    class
FROM
    courses
Group by
    class
Having count(DISTINCT student) >= 5;
```



