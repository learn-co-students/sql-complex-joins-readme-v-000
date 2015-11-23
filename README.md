# Complex Joins in SQL

##Objectives
1. Know what an outer join is
1. Distinguish an inner join from an outer join
2. Identify different types of outer joins: left, right, and full

##Why is this important?
####Grade Example (Inner Join)
>Imagine you want to get a list of all the students with an "A" in the class. We only want those students in the class with top grades, ignoring the other students in the class. 

####Field Trip Example (Complex/Outer Join)
>Now imagine another scenario where the class is going on a field trip. The cost of the field trip is $10 per student. As a teacher, we want to keep track of which students have paid AND which students still need to pay.

Everything we've done up until this point looks like the Grade Example. This is an inner join. We only want the students with a certain grade. You can imagine a Venn Diagram where one circle is "Grades" and another circle is "Students". We only want the overlapping (or "inner") parts of the two circles.

![](http://readme-pics.s3.amazonaws.com/Grade%20example%20Venn%20Diagram.png)

Complex joins are useful and important when it comes to situations like the Field Trip Example. Sticking with the Venn Diagrams, we can think about "Students" as one circle and "Payments" as another circle. A complex join (or outer join) will return the overlap between the two circles AND the rest (or the "outer" part) of the "Students" circle as well.

![](http://readme-pics.s3.amazonaws.com/Payment%20example%20Venn%20Diagram.png)

We'll elaborate more on visualizing joins in the Venn Diagrams section below.

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
Outer Joins, on the other hand, will return all of the matching rows AND all of the additional rows from the specified table. Which table/additional rows are determined by the type of outer join. There are three types of outer joins: Left Outer Join, Right Outer Join, and Full Outer Join.

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

![](http://readme-pics.s3.amazonaws.com/Inner%20Join%20Venn%20Diagram.png)

A Left Outer Join returns all the the data from the left circle, and it includes the overlapping information from the right circle.

![](http://readme-pics.s3.amazonaws.com/Left%20Outer%20Join%20Venn%20Diagram.png)

A Right Outer Join returns all the the data from the right circle, and it includes the overlapping information from the left circle.

![](http://readme-pics.s3.amazonaws.com/Right%20Outer%20Join%20Venn%20Diagram.png)

A Full Outer Join returns all the data from all the tables, including the overlapping data.

![](http://readme-pics.s3.amazonaws.com/Full%20Outer%20Join%20Venn%20Diagram.png)

##Examples
###Create Our Students Table
```
CREATE TABLE students (
    id INTEGER PRIMARY KEY,
    name TEXT,
    teacher_id INTEGER);
```

###Insert Students
```
INSERT INTO students (name, teacher_id)
    VALUES ("Dave", 1);
INSERT INTO students (name, teacher_id)
    VALUES ("Jessie", 1);
INSERT INTO students (name, teacher_id)
    VALUES ("Bob", 1);
INSERT INTO students (name, teacher_id)
    VALUES ("Sara", 2);
INSERT INTO students (name, teacher_id)
    VALUES ("George",  2);
INSERT INTO students (name, teacher_id)
    VALUES ("Alexis",  NULL);
```

###Students Schema
```
id               name        teacher_id            
---------------  ----------  ----------  
1                Dave           1           
2                Jess           2              
3                Bob            1          
4                Sara           2  
5                Rob            2
6                Alexis        Null             
```

###Create Our Teachers Table
```
CREATE TABLE teachers (
    id INTEGER PRIMARY KEY,
    name TEXT);
```

###Insert Into Teachers

```
INSERT INTO teachers (name)
    VALUES ("Steven");
INSERT INTO teachers (name)
    VALUES ("Joe");
INSERT INTO teachers (name)
    VALUES ("Jeff");
```

###Teachers Schema

```
id               name                         
---------------  ---------
1                Joe                    
2                Steven  
3                Jeff                                  
```


###Left Outer Join

```
SELECT * from teachers
   LEFT OUTER JOIN students on teachers.id = students.teacher_id;
```

This query will return all of the records in the left table (teachers) regardless if any of those records have a match in the right table (students). It will also return any matching records from the right table. So for our example, it first returns all of the teachers followed by any student that has a `teacher_id`. You can see that Alexis was not returned because her `teacher_id` column is `NULL`.

###Results

```
id  teacher_name    id      name     teacher_id               
--- ------------   ----    ------    -----------
1	   Steven		 2       Bob          1
1	   Steven	  	 1       Dave         1	 
1	   Steven	  	 3       Jess         1
2	   Joe	     	 5       Rob          1
2	   Joe	     	 4       Sara         1
3	   Jeff	    	 NULL    NULL         NULL		              
```


##Right Outer Join

```
SELECT * from teachers
   RIGHT OUTER JOIN students on teachers.id = students.teacher_id;
```
This query will return all of the records in the right table (students) regardless if any of those records have a match in the left table (teachers). It will also return any matching records from the left table. You can see that all of the students were returned, but this time Jeff was left out.

###Results

```
id    teacher_name   id      name     teacher_id               
---   ------------  ----    ------    -----------
1	     Steven		 2       Bob          1
1	     Steven	  	 1       Dave         1	 
1	     Steven	  	 3       Jess         1
2	     Joe	     5       Rob          1
2	     Joe	     4       Sara         1
NULL     NULL	     6       Alexis       NULL		              
```

##Full Join

```
SELECT * from teachers
   FULL OUTER JOIN students on teachers.id = students.teacher_id;
```

This Join can be referred to as a FULL OUTER JOIN or a FULL JOIN. This query will return all of the records from both tables, joining records from the left table (teachers) that match records from the right table (students).

```
id    teacher_name   id      name     teacher_id               
---   ------------  ----    ------    -----------
1	     Steven		 2       Bob          1
1	     Steven	  	 1       Dave         1	 
1	     Steven	  	 3       Jess         1
2	     Joe	     5       Rob          1
2	     Joe	     4       Sara         1
NULL     NULL	     NULL    NULL         NULL		              
```
