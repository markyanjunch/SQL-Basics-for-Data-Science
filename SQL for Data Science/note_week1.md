# 第一周
## 目标
1. 定义并讨论SQL与其他计算机语言的不同
2. 讨论如何在数据库中使用SQL
3. 对比数据库管理员与数据科学家职责的差别
4. 解释数据库中的数据关系
5. 使用SELECT命令
6. 学习语法
7. 写注释的好处
  
## What is SQL Anyway?
### 什么是SQL
1. Structured Query Language - "sequel"
2. 查询，插入，更新和修改数据
3. 与数据库通信
4. 命令由描述性语句组成
5. SQL并非程序性语言：不能用来写一个完整的应用

### 如何使用SQL
*SQL is all about data!*
1. 阅读/检索数据
2. 写入数据
3. 更新数据

### 数据库管理员与数据科学家
- **Database Admin**
    + 管理整个数据库
    + 提供用户许可
    + 决定数据的权限
    + 管理与创建列表
    + 使用SQL查询与检索数据
- **Data Scientist**
    + 数据库客户端用户
    + 使用SQL查询与检索数据

### 数据科学家如何使用SQL
1. 检索数据
2. 创建自己的表格或测试环境
3. 组合多个source
4. 为分析编写复杂的查询语句

### SQL与数据库管理系统(DBMS)
1. 语法取决于你使用的DBMS
2. 每种DBMS都有自己的特殊语句
3. SQL能够翻译这些特殊语句
4. 有时需要根据DBMS的特殊语句修改你的语句

### 关系数据库管理系统
- Microsoft SQL Server
- MySQL
- IBM DB2
- Oracle
- Apache Open Office Base
- Sybase ASE
- SQLite
- PostgreSQL

## Data Models Part 1: Thinking about Your Data
### 理解你的数据
1. 理解数据所建模的业务流程或主体
2. 知道商业规则
3. 理解表格中数据的结构

### 这么做的好处
1. 得到更准确地结果
2. 加速你的工作
3. 减少返工

### 数据库与表格
- **Database**
    + 用来储存整理好的数据的容器
    + 一组相关的信息

- **Table**
    + 一个有结构的表格，其中是数据或特定类型

### 行和列
1. 列：表格中的一个field
2. 行：表格中的一条记录

## Data Models Part 2： The Evolution of Data Models
### 什么是数据建模？
1. 整理与结构化信息，并放入多个关系表格中
2. 能够表示一个业务流程或业务流程间的关系
3. 要尽可能代表真实世界

### 数据模型的类型
1. 数据科学家建立的预测模型
2. 数据库中所表现和整理的表格形式的数据模型

### 大数据世界中的SQL
1. NoSQL - Not Only SQL
2. 一种储存和检索不使用关系数据库中的表格关系来建模的非结构化数据的机制

## Data Models Part 3: Relational vs. Transactional Models
### 关系模型与事务模型
- **Relational Model**: 允许以一种简单、符合逻辑与直觉的方式来进行简单的查询和数据操作
- **Transactional Model**: 操作型数据库——医疗保健系统中的保险索赔

### 数据模型构件
1. 主体(Entity)：人，地点，事物或事件；可区分，唯一且界限清晰
2. 属性(Attribute)：主体的特征
3. 关系(Relationship)：描述主体间的联系
    + One-to-many: 客户与发票
    + Many-to-many：学生与班级
    + One-to-one：经理与商店

### 实体-联系图（ER Diagrams）
1. ER 模型：由主体类型组成，列举主体类型的实例间存在的联系
2. 作用
    + 展示联系
    + 业务流程
    + 可视化
    + 展示连接（primary keys）

### 主键和外键
1. 主键（primary key）：一列（或一组列），他们的值能够唯一确定表格中的每一行
2. 外键（foreign key）：一列或几列，能够一起用来确定另一张表格中的一行

### ER图标记法
<div align=center><img src="https://github.com/markyanjunch/SQL-Basics-for-Data-Science/blob/main/SQL%20for%20Data%20Science/Figures/ERDiagramNotation.JPG" width = "700" height = "450" alt=""/></div>
