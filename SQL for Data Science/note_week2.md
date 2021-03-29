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
