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
## 为什么要给数据排序
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
## 使用ORDER BY的规则
1. 使用一列或多列的名称
2. 在每个增加的列名称后加个逗号
3. 能够通过未被检索的列排序
4. 必须是select语句中的最后一部分
## 通过列的位置排序
```sql
ORDER BY 2,3
```
2表示第二列，3表示第三列，等等
## 排序顺序
1. DESC表示降序
2. ASC表示升序
3. 只对在它之后的的一个列名称有用
