# Complex Joins in SQL

##Objectives
1. Know what an outer join is
1. Distinguish an inner join from an outer join
2. Identify different types of outer joins: left, right, and full

##Why is this important?

##Overview
A complex join in SQL is also referred to as an outer join. It is not necessarily more complex than an inner join. It is referred to as "complex" simply because SQL is conducting an inner join in addition to gathering a little more information from one or more tables. What is that extra bit of information? We will discover this by looking at the difference between outer and inner joins.

##Difference between Inner Join and Outer Join
###Inner Join
As you may recall, an inner join is going to return only the rows from the database that match the query. For example, imagine we have the following tables:

```
TEACHERS TABLE             STUDENTS TABLE
teacher_id                 student_id   teacher_id
---------------            ------------------------
1                          1            NULL
2                          2            1
3                          3            NULL
```

Let's look at an inner join.

```sql
SELECT * 
FROM Teachers 
INNER JOIN Students 
ON Teacher.teacher_id = Student.teacher_id;
```

This query returns only the teacher with the `id = 1` because students 1, 2, and 3 are all in the first teacher's class.

```
teacher_id  |  student_id          
--------------------------
1           |  2
```

###Outer Join
Outer Joins, on the other hand, will return all of the matching rows AND all of the additional rows from the specified table. Which table/additional rows are determined by the type of outer join. There are are three types of outer joins: Left Outer Join, Right Outer Join, and Full Outer Join.

####Left Outer Join
This is the most common outer join, and the one you'll use most often. This returns the normal inner join result and also ***returns all of the rows from the left-most (i.e. first mentioned) table***.

```sql
SELECT * 
FROM Teachers 
LEFT OUTER JOIN Students 
ON Teacher.teacher_id = Student.teacher_id;
```

```
teacher_id  |  student_id          
--------------------------
1           |  2
2           |  NULL
3           |  NULL
```
Notice that every row from the teacher's table is returned whether there is a corresponding student or not.

####Right Outer Join
As you might imagine, this is the same as the Left Outer Join with the minor difference being that it ***returns all of the rows from the right-most (i.e. last mentioned) table***. Sticking with our example:

```sql
SELECT * 
FROM Teachers 
RIGHT OUTER JOIN Students 
ON Teacher.teacher_id = Student.teacher_id;
```

```
teacher_id     |  student_id          
--------------------------
NULL           |  1
1              |  2
NULL           |  3
```

###Full Outer Join
The full ***returns all of the rows from the all tables***.

```sql
SELECT * 
FROM Teachers 
FULL OUTER JOIN Students 
ON Teacher.teacher_id = Student.teacher_id;
```

```
teacher_id     |  student_id          
--------------------------
NULL           |  1
1              |  2
NULL           |  3
2              |  NULL
3              |  NULL
```

##Venn Diagrams

It is helpful to think about our queries as a Venn Diagram. Each table can be represented by a circle. 

An Inner Join just returns the overlapping areas of the Venn Diagram.

<image>

A Left Outer Join returns all the the data from the left circle, and it includes the overlapping information from the right circle.

<image>

A Right Outer Join returns all the the data from the right circle, and it includes the overlapping information from the left circle.

<image>

A Full Outer Join returns all the data from all the tables, including the overlapping data.

<image>