# 第三周
## 目标
1. 子查询
    - 它们如何工作
    - 优点与缺点
    - 使用子查询的最佳实践
2. Joins
    - 重温键字段
    - 使用joins把数据连接在一起
    -不同类型的joins的特点
3. 让代码更简洁和有效  
    - 使用别名和前限定词

## Using Subqueries
### 什么是子查询？
1. 嵌入其它查询中的查询语句
2. 关系数据库将数据存储在多张表格中
3. 子查询将多个来源的数据合并到一起
4. 加入其它筛选条件（来自其它的表格）

### 问题设置：从子查询到筛选  
需要知道有订单运费超过100的顾客来自的地区
1. 检索所有订单运费超过100的顾客的ID
2. 检索顾客信息
3. 将两个查询结合
```sql
SELECT DISTINCT customerID
FROM Orders
WHERE Freight > 100;

SELECT customerID
	,CompanyName
	,Region
FROM Customers
WHERE customerID IN (
		'RICSV'
		,'ERNSH'
		,'FRANK'...
		);
```

### 结合为子查询
```sql
SELECT CustomerID
	,CompanyName
	,Region
FROM Customers
WHERE CustomerID IN (
		SELECT customerID
		FROM Orders
		WHERE Freight > 100
		);
```

### 使用子查询语句
1. 总是先运行最内层的SELECT部分
2. DBMS做了两个操作：
    - 得到所选产品的订单号
    - 将其放入WHERE语句中，然后处理整个SELECT语句

## Subquery Best Practices and Considerations
### 子查询的最佳实践
1. 你能有的子查询没有数量上的限制
2. 当你嵌套得太深是运行速度会变慢
3. 子查询中的SELECT只能检索一列

### 子查询中的子查询
1. 牙刷的订单号
2. 这些订单的客户ID
3. 这些订单的客户信息
```sql
SELECT Customer_name
	,Customer_contact
FROM Customers
WHERE cust_id IN (
		SELECT customer_id
		FROM Orders
		WHERE prod_name = 'Toothbrush'
		);
```
### PoorSQL网站  
www.poorsql.com
1. 网站会预格式化代码
2. 正确缩进
3. 代码易于阅读和排错

### 在计算中使用子查询  
每个客户下的总订单数
```sql
SELECT COUNT(*) AS orders
FROM Orders
WHERE cutomer_id = '143569';
SELECT customer_name
        ,customer_state
        (SELECT COUNT(*) AS orders
        FROM Orders
        WHERE Orders.customer_id = Customer.customer_id) AS orders
FROM customers
ORDER BY Customer_name
```
|Customer_name|Customer_state|Orders|
|-|-|-|
|Becky|IA|5|
|Nita|CA|6|
|Raj|OH|0|
|Steve|AZ|1|

### 子查询的力量
1. 子查询是非常强大的工具
2. 关系到运行速度，并非总是最好的选择

## Joining Tables: An Introduction
### 将数据分成不同表格的好处
1. 高效的存储
2. 更易于操作
3. 更高的扩展性
4. 逻辑上建模了一个过程
5. 表格间通过共有的值（键）来相互关联
### 连接(Joins)
1. 动态地关联每张表格中的正确记录
2. 在一条请求中完成从多张表格检索信息
3. Joins不是物理上的——它们只在请求执行时有效

## Catesian(Cross) Joins
### 什么是笛卡尔连接（交叉连接）？  
CROSS JOINs: 来自第一张表格中的每一行都与另一张表格中的所有行相连
<div align=center><img src="https://github.com/markyanjunch/SQL-Basics-for-Data-Science/blob/main/SQL%20for%20Data%20Science/Figures/CartesianCrossJoin.JPG?raw=ture" width = "533" height = "292" alt=""/></div>

### 笛卡尔（交叉）连接实例
```sql
SELECT product_name
	,unit_price
	,company_name
FROM suppliers
CROSS JOIN products;
```
### 笛卡尔（交叉）连接的缺点
1. 不是很常用
2. 计算上负担大
3. 会为产品得到错误的或根本不存在的供应商

## Inner Joins
### 什么是内连接  
INNER JOIN关键词选择在两张表格中有相匹配的值的记录
<div align=center><img src="https://github.com/markyanjunch/SQL-Basics-for-Data-Science/blob/main/SQL%20for%20Data%20Science/Figures/InnerJoin.JPG?raw=ture" width = "446" height = "285" alt=""/></div>

### 内连接实例
```sql
SELECT suppliers INNER JOIN Products
ON Suppliers.supplierid = Products.supplierid
```

### 内连接语法
1. 指出连接类型(INNER JOIN)
2. 连接条件需要在FROM语句中使用ON语句
3. 连接越多的表格会影响数据库的整体表现
4. 你可以连接多张表格，没有限制
5. 列出所有的表格，然后定义条件

### 内连接与多张表格
```sql
SELECT o.OrderID, c.CompanyName, e.LastName
FROM ((Orders o INNER JOIN Customers c ON
o.CustomerID = c.CustomerID)
INNER JOIN Employee e ON o.EmployeeID = e.EmployeeID);
```
### 使用内连接的最佳做法
1. 提前限定(pre-qualify)名称
2. 不要建立无用的连接
3. 思考建立的连接类型
4. 你是如何连接记录的？

## Aliases and Self Joins
### 什么是别名
1. SQL别名会赋予一张表格或一列一个暂时的名称
2. 使列名可读性更强
3. 别名只会在一次查询的期间有效
```sql
SELECT column_name
FROM table_name AS alias_name
```

### 使用别名的查询实例
```sql
SELECT vendor_name
,product_name
,product_price
FROM Vendors, Products
WHERE Vendors.vendor_id = Products.vendor_id;
```

使用别名：
```sql
SELECT vendor_name
,product_name
,product_price
FROM Vendors As v, Products As p
WHERE v.vendor_id = p.vendor_id;
```

### 自连接
1. 匹配来自同一城市的客户
2. 将表格视作两张不同的表格
3. 将原始表格与它自己连接
```sql
SELECT column_name(s)
FROM table1 T1, table2 T2
WHERE condition;
```

### 自连接实例  
以下的SQL语句将来自同一城市的客户匹配到了一起：
```sql
SELECT A.CustomerName as CustomerName1
,B.CustomerName AS CustomerName2
,A.City
FROM Customers A, Customers B
WHERE A.CustoemrID = B.CustomerID
AND A.City=B.City
ORDER BY A.City;
```


