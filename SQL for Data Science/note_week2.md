# 第二周
## 目标
1. SQL中心的语句与运算符：WHERE, BETWEEN, IN, OR, NOT, LIKE, ORDER BY, GROUP BY
2. 通配符
    - 通配符使搜索更精确
    - 讨论使用通配符的好处和坏处
    - 列举使用通配符的最佳方式
3. 数学运算符
    - 使用数学计算符号
    - 使用汇总数学函数: AVERAGE, COUNT, MAX, MIN

## Basics of Filtering with SQL
### 为什么要筛选？
1. 确定你想要检索的数据
2. 减少你要检索的记录数目
3. 提升查询性能
4. 减少用户应用的压力
5. 管理限制

### WHERE语句运算符
```sql
SELECT column_name,column_name
FROM table_name
WHERE column_name operator value;
```
|Operator|Description|
|:------:|-----------|
|=       |Equal|
|<>      |Not equal. Note: In some versions of SQL this operator may be written as !=|
|>       |Greater than|
|<       |Less than|
|>=      |Greater than or equal|
|<=      |Less than or equal|
|BETWEEN |Between an inclusive range|
|IS NULL |Is a null value|

### 单一条件筛选
```sql
SELECT ProductName
,UnitPrice
,SupplierID
FROM Products
WHERE UnitPrice = ‘Tofu’;
```
### 单一值筛选
```sql
SELECT ProductName
,UnitPrice
,SupplierID
FROM Products
WHERE UnitPrice >= 75;
```
### 检查不匹配项
```sql
SELECT ProductName
,UnitPrice
,SupplierID
FROM Products
WHERE ProductName <> 'Alice Mutton';
```
### 用一个范围内的值来筛选
```sql
SELECT ProductName
,UnitPrice
,SupplierID
，UnitsInStock
FROM Products
WHERE UnitsInStock BETWEEN 15 AND 80;
```
### 用空值来筛选
```sql
SELECT ProductName
,UnitPrice
,SupplierID
，UnitsInStock
FROM Products
WHERE ProductName IS NULL;
```

## Advanced Filtering: IN, OR, and NOT
### IN运算符
1. 指出一个范围内的条件
2. 逗号分隔的一列值
3. 包含在()内
e.g.
```sql
SELECT ProductID
,UnitPrice
,SupplierID
FROM Products
WHERE SupplierID IN (9,10,11);
```
### OR运算符
1. 如果WHERE语句中的第一个条件被满足的话，DBMS不会计算第二个条件
2. 用在任何满足一定条件的列上
```sql
SELECT ProductID
,UnitPrice
,SupplierID
,ProductName
FROM Products
WHERE ProductName = 'Tofu' OR 'Konbu';
```
### IN vs. OR
1. IN与OR工作机制相同
2. IN的好处
    - 一长列选择
    - IN运行速度比OR快
    - IN不用考虑顺序
    - 能包含另一个SELECT

### OR与AND
```sql
SELECT ProductID
,UnitPrice
,SupplierID
FROM Products
WHERE SupplierID=9 OR
SupplierID=11
AND UnitPrice>15;
```
```sql
SELECT ProductID
,UnitPrice
,SupplierID
FROM Products
WHERE (SupplierID=9 OR
SupplierID=11)
AND UnitPrice>15;
```
1. SQL先计算AND再计算OR
2. 使用()
3. 不要依赖于默认运算顺序，最好习惯性地加上()

### NOT运算符
```sql
SELECT *
FROM Employees
WHERE NOT City='London' AND
NOT City='Seattle';
```

## Using Wildcards in SQL
### 什么是通配符？
1. 用来匹配一个值的某些部分的特殊字符
2. 搜索来自文本，通配符或它们的组合的模式
3. 使用LIKE作为运算符（尽管严格地说是一个谓词）
4. 只能用于字符串
5. 不能用于非文本数据模型
6. 当数据科学家研究字符串变量时很有用

### 使用%通配符
|Wildcards|Action|
|-|-|
|'%Pizza'|抓取所有以pizza结尾的事物|
|'Pizza%'|抓取所有pizza之后的事物|
|'%Pizza%'|抓取所有pizza之前和之后的事物|
|'S%E'|抓取以"S"开头以"E"结尾的事物（比如Sadie）|
|'t%@gmail.com'|抓取以"t"开头的gmail地址（希望找到Tom）|
1. %通配符不会匹配NULL
2. NULL表示一列中没有值

### 下划线( _ )通配符
1. 匹配一个单字符
2. DB2不支持

```sql
WHERE size LIKE '_pizza'
    Output:
    spizza
    mpizza
```
### 方括号[]通配符
1. 用来明确在一个特定位置的一系列字符
2. 不是所有DBMS都能用
3. SQLite不能用

### 通配符的弊端
1. 运行时间更长
2. 最好使用其他运算符（如果可能的话）: =, <, =>等等
3. 如果含有通配符的语句位于搜索模式的最后，运行时间将会更长
4. 通配符的位置很重要

## Sorting with ORDER BY
### 为什么要给数据排序
1. 显示的数据以潜在的表格的顺序出现
2. 更新或删除数据会改变这个顺序
3. 如果没有明确指出，不能假定检索的数据序列
4. 有逻辑地给数据排序能使你想要的数据在最上层出现
5. ORDER BY语句让用户能通过特定的列给数据排序
```sql
SELECT something
FROM database
ORDER BY characteristic
```
### 使用ORDER BY的规则
1. 使用一列或多列的名称
2. 在每个增加的列名称后加个逗号
3. 能够通过未被检索的列排序
4. 必须是select语句中的最后一部分
### 通过列的位置排序
```sql
ORDER BY 2,3
```
2表示第二列，3表示第三列，等等
### 排序顺序
1. DESC表示降序
2. ASC表示升序
3. 只对在它之后的的一个列名称有用

## Math Operations
### 数学运算符
|Operator|Description|
|-|-|
|+|加|
|-|减|
|* |乘|
|/|除|
### 乘法实例
```sql
SELECT ProductID
,UnitsOnOrder
,UnitPrice
,UnitsOnOrder*UnitPrice AS Total_Order_Cost
FROM Products;
```
### 运算符顺序
括号>乘方>乘法>除法>加法>减法  
*"Please excuse my dear Aunt Sally"*
### 混合数学运算
```sql
SELECT ProductID
,Quantity
,UnitPrice
,Discount
,(UnitPrice-Discount)/Quantity AS Total_Cost
FROM OrderDetails;
```

## Aggregate Functions
### 什么是汇总函数
1. 用来总结数据
2. 寻找最大值和最小值
3. 寻找总行数
4. 寻找平均数

### 汇总函数
|Function|Description|
|-|-|
|AVG()|求一列值的平均|
|COUNT()|计算值的个数|
|MIN()|寻找最小值|
|MAX()|寻找最大值|
|SUM()|对列的值求和|

### AVERAGE函数  
包含NULL值的行会被AVERAGE函数忽略
```sql
SELECT AVG(UnitPrice) AS avg_price
FROM products
```
### COUNT函数
1. COUNT(* )-计算一张表格中所有的行数，包括NULL值
```sql
SELECT COUNT(*) AS total_customers
FROM Customer;
```
2. COUNT(column)-计算一张表格中特定列的行数，忽略NULL值
```sql
SELECT COUNT(CustomerID) AS total_customers
FROM Customer;
```
### MAX与MIN函数  
NULL值会被MIN和MAX函数忽略
```sql
SELECT MAX(UnitPrice) AS max_prod_price
FROM Products
```
```sql
SELECT MAX(UnitPrice) AS max_prpod_price
,MIN(UnitPrice) AS min_prod_price
FROM Products
```
### SUM汇总函数
```sql
SELECT SUM(UnitPrice) AS total_price
FROM Products
WHERE SupplierID=23;
```
### 在汇总函数中使用DISTINCT
1. 如果没用DISTINCT，假定是ALL
2. COUNT(* )不能用DISTINCT
3. MIN和MAX函数中没必要使用
```sql
SELECT COUNT(DISTINCT CustomerID)
FROM Customers
```

## Grouping Data with SQL
### 数据分类
1. 学习如何给数据分类以总结数据的子集
2. 新语句GROUP BY; HAVING
3. 如何对一个特殊的值作汇总
```sql
SELECT
Region
,COUNT(CustomerID) AS total_customers
FROM Customers
GROUP BY Region;
```
### 其他关于GROUP BY的信息
1. GROUP BY语句能包含多列
2. SELECT语句中的每一列都必须出现在GROUP BY中，汇总计算除外
3. 如果GROUP BY的列包含NULL，NULL值会被分类到一起
### HAVING语句-筛选分类
1. WHERE对分类没用
2. WHERE筛选的是行
3. 转而我们用HAVING语句来筛选分类
```sql
SELECT
CustomerID
,COUNT(*) AS orders
FROM Orders
GROUP BY CustomerID
HAVING COUNT (*) >=2;
```
### WHERE vs. HAVING
1. WHERE在数据被分类前筛选
2. HAVING在数据被分类后筛选
3. 被WHERE语句去除的行不会被包含在分类里
### ORDER BY 与 GROUP BY
    - ORDER BY 会给数据排序
    - GROUP BY 不会给数据排序

## Putting It All Together
### 核心SQL语句
|Clause|Description|Required|
|-|-|-|
|SELECT|要返回的列或表达式|Yes|
|FROM|要检索数据的表格|只有当从一个表格中选取数据时需要|
|WHERE|行间的筛选|No|
|GROUP BY|明确类别|只有当通过类别计算汇总时需要|
|HAVING|类别间的筛选|No|
|ORDER BY|输出排序|No|
