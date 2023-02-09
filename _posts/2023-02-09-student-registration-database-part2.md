---
layout: post
title: Building Student Registration Database Part 2 - SQL Learning Series
subtitle: Each post also has a subtitle
categories: [database, SQL]
tags: [database, programming, MySQL, SQL, python]
---


<p align="center">
  <img src="/assets/images/banners/srd_part1/mysql.png" width="300">
  <em>Source: wikipedia</em>
</p>


## **Part 2**
<p style="margin-bottom:-50px"></p>

### Showing tables

In this step, I added functionalities to see all created and updated tables based on designed keyword such as student ID, student name, etc.

<p style="margin-bottom:-50px"></p>

#### A. Departments

<b>Database function for reading table "departments":</b>

```python
# File: db.py 
def showing_department(department):
    with create_server_connection() as con:
        
        if department != None:
            sql_script = """
                SELECT dept_id, dept_name, dean_name, building, room 
                FROM departments WHERE dept_name = %s;
            """
            return query(con, sql_script, data=(department, ), fetch=True)
        else:
            sql_script = "SELECT * FROM departments;"
            return query(con, sql_script, fetch=True)
```

<b>Interface function for showing table "departments":</b>

```python
# File: app.py 
@app.command()
def show_department():
    user_answer = input("Do you want to see one particular department?. Enter department name: ").strip().lower()
    
    if user_answer:
        pretty_table(["Department ID", "Department Name", "Dean Name", "Building", "Room"],
                    data=showing_department(user_answer), in_color="green")
    else:
        department = None
        typer.echo(typer.style("Showing all departments...", bg=typer.colors.WHITE, fg=typer.colors.GREEN))
        pretty_table(["No", "Department ID", "Department Name", "Dean Name", "Building", "Room"],
                    data=showing_department(department), in_color="green")
```

Here, user can search for a particular department by inputting department name. However, when user doesn't specify anything, it would show all content of the table. Then, when we run this function by writing in terminal `python app.py show-department`.

<p align="center">
  <img src="/assets/images/banners/srd_part1/show_department1.png" width="500">
  <em>Running "show-department" without any user input</em>
</p>

<p align="center">
  <img src="/assets/images/banners/srd_part1/show_department2.png" width="500">
  <em>Running "show-department" with user input</em>
</p>


#### B. Courses

<b>Database function for reading table "courses":</b>

```python
# File: db.py
def showing_course(department):
    with create_server_connection() as con:
        
        if department != None:
            sql_script = "SELECT dept_id, course_id, course_name, hour FROM courses WHERE dept_id = %s;"
            return query(con, sql_script, data=(department, ), fetch=True)
        else:
            sql_script = "SELECT * FROM courses;"
            return query(con, sql_script, fetch=True)
```

<b>Interface function for showing table "courses":</b>

```python
# File: app.py
@app.command()
def show_course():
    user_answer = input("Enter department ID: ").strip().lower()
    
    if user_answer:
        pretty_table(["Department ID", "Course ID", "Course Name", "Duration (hr)"],
                    data=showing_course(user_answer), in_color="green")
    else:
        department = None
        typer.echo(typer.style("Showing all courses...", bg=typer.colors.WHITE, fg=typer.colors.GREEN))
        pretty_table(["No", "Department ID", "Course ID", "Course Name", "Duration (hr)"],
                    data=showing_course(department), in_color="green")
```

The same principle also applies here.

#### C. Students

<b>Database function for reading table "students":</b>

```python
# File: db.py
def showing_student(student_name):
    with create_server_connection() as con:

        if student_name != None:     
            student = ('%' + student_name + '%', '%' + student_name + '%',) # Regex
            sql_script = """
                SELECT student_id, first_name, last_name, gpa FROM students WHERE first_name LIKE %s OR last_name LIKE %s
            """
            return query(con, sql_script, data=(student), fetch=True)
        else:
            sql_script = "SELECT student_id, first_name, last_name, gpa FROM students;"
            return query(con, sql_script, fetch=True)
```

I used regular expression or REGEX to filter base on student's name, both first and last name.

<b>Interface function for showing table "students":</b>

```python
# File: app.py
@app.command()
def show_student():
    user_answer = input("Search for student's name: ").strip().lower()
    
    if user_answer:
        pretty_table(["Student ID", "First Name", "Last Name", "GPA"],
                 data=showing_student(user_answer), in_color="green")
    else:
        student_name = None
        typer.echo(typer.style("Showing all students...", bg=typer.colors.WHITE, fg=typer.colors.GREEN))
        pretty_table(["No", "Student ID", "First Name", "Last Name", "GPA"],
                 data=showing_student(student_name), in_color="green")
```

If we run this function by writing in terminal `python app.py show-student`. For example, "in" is entered, it will get all student's name containing "in".

<p align="center">
  <img src="/assets/images/banners/srd_part1/show_student1.png" width="500">
  <em>Running "show-student" without any user input</em>
</p>

<p align="center">
  <img src="/assets/images/banners/srd_part1/show_student2.png" width="500">
  <em>Running "show-student" with user input</em>
</p>

Another thing is the GPA here. The default is 0. It will be updated when student has taken a course and obtained a grade. This functionaty will be addressed in the "enrolled" section below.

#### D. Prerequisites

<b>Database function for reading table "prerequisites":</b>

```python
# File: db.py
def showing_prereq(course_id):
    with create_server_connection() as con:
        data = (course_id,)

        if course_id != None:
            sql_script = "SELECT course_id, course_prereq_id, min_grade FROM prerequisites WHERE course_id = %s"
            return query(con, sql_script, data=data, fetch=True)
        else:
            sql_script = "SELECT * FROM prerequisites;"
            return query(con, sql_script, fetch=True)
```

<b>Interface function for showing table "prerequisites":</b>

```python
# File: app.py
@app.command()
def show_prereq():
    user_answer = input("Enter course ID: ").strip().lower()
    
    if user_answer:
        pretty_table(["Course ID", "Course Prerequisites", "Minimum Grade"],
                 data=showing_prereq(user_answer), in_color="green")
    else:
        course_id = None
        typer.echo(typer.style("Showing all course prerequisites...", bg=typer.colors.WHITE, fg=typer.colors.GREEN))
        pretty_table(["No", "Course ID", "Course Prerequisites", "Minimum Grade"],
                    data=showing_prereq(course_id), in_color="green")
```

The same principle also applies here.

#### E. Enrolled

<b>Function for reading table "enrolled":</b>

```python
# File: db.py
def showing_enrolled(student_id):
    with create_server_connection() as con:
        data = (student_id,)

        if student_id != None:
            sql_script = "SELECT student_id, course_id, enrollment_year, grade FROM enrolled WHERE student_id = %s"
            return query(con, sql_script, data=data, fetch=True)
        else:
            sql_script = "SELECT * FROM enrolled;"
            return query(con, sql_script, fetch=True)
```

<b>Interface function for showing table "enrolled":</b>

```python
# File: app.py
@app.command()
def show_enrolled():
    user_answer = input("Enter student ID: ").strip().lower()
    
    if user_answer:
        pretty_table(["Student ID", "Course ID", "Enrollment year", "Grade"],
                 data=showing_enrolled(user_answer), in_color="green")
    else:
        course_id = None
        typer.echo(typer.style("Showing all enrolled students...", bg=typer.colors.WHITE, fg=typer.colors.GREEN))
        pretty_table(["No", "Student ID", "Course ID", "Enrollment year", "Grade"],
                    data=showing_enrolled(course_id), in_color="green")
```

If we run this function by writing in terminal `python app.py show-enrolled`, it will show empty table because we haven't entered anything. In order to fill out table "enrolled", we should run `python app.py enroll-student`. Please note that student can enroll a course if the prerequisites are met OR if the course doesn't have any prerequisites. Example below "cs101" does not have any course prerequisite.

<p align="center">
  <img src="/assets/images/banners/srd_part1/enroll_student1.png" width="500">
  <em>Running "enroll-student" for course without prerequisites</em>
</p>

<p align="center">
  <img src="/assets/images/banners/srd_part1/enroll_student2.png" width="500">
  <em>Running "enroll-student" for course with prerequisites</em>
</p>

<p align="center">
  <img src="/assets/images/banners/srd_part1/show_enrolled1.png" width="500">
  <em>Running "show-enrolled" for reading enrolled students</em>
</p>


It is assumed that student has not finished the course and not yet obtained any grade. In order to update this grade, we should run `python update-grade`.

<p align="center">
  <img src="/assets/images/banners/srd_part1/show_enrolled2.png" width="500">
  <em>Running "show-enrolled" for reading enrolled students after updating course's grade</em>
</p>

By doing this, we have enough information for student's GPA. For this purpose, I created another trigger after updating grade in the "enrolled" table.

```sql
/*File: sql_query.sql*/
DROP TRIGGER IF EXISTS after_enrolled_update;
CREATE TRIGGER after_enrolled_update
    AFTER UPDATE ON enrolled 
    FOR EACH ROW
BEGIN
    UPDATE students
    SET gpa = (SELECT AVG(grade) FROM enrolled AS e WHERE e.student_id = NEW.student_id AND e.grade IS NOT NULL)
    WHERE student_id = NEW.student_id;
END;
```
<p align="center">
  <img src="/assets/images/banners/srd_part1/show_student3.png" width="500">
  <em>Running "show-student" for querying student's GPA</em>
</p>

#### F. Student's transcript

In this step, I added functionality for converting student's GPA into letter grade (A, B, C, D, E) by creating function in SQL.

```sql
/*File: sql_query.sql*/
DROP FUNCTION IF EXISTS calculate_letter_grade;
CREATE FUNCTION calculate_letter_grade(gpa FLOAT)
  RETURNS VARCHAR(10) DETERMINISTIC
  BEGIN
    DECLARE letter_grade VARCHAR(10);

    IF gpa >= 90 THEN SET letter_grade = 'A';
    ELSEIF gpa >= 80 THEN SET letter_grade = 'B';
    ELSEIF gpa >= 70 THEN SET letter_grade = 'C';
    ELSEIF gpa >= 60 THEN SET letter_grade = 'D';
    ELSE SET letter_grade = 'E';
    END IF;
    RETURN letter_grade;
  END;
```

<b>Function for querying student's GPA:</b>

```python
# File: db.py
def showing_transcript(student_id):
    with create_server_connection() as con:
        sql_script = """
            SELECT student_id, first_name, last_name, gpa, calculate_letter_grade(gpa) AS letter_grade
            FROM students WHERE student_id = %s;
            """

        return query(con, sql_script, (student_id, ), fetch=True)
```

<b>Interface function for querying student's GPA:</b>

```python
# File: app.py
@app.command()
def show_transcript():
    student_id = input("Enter your student id: ").strip().lower()

    if student_id:
        data = showing_transcript(student_id)
        pretty_table(["Student ID", "First Name", "Last Name", "GPA", "Letter Grade"], data=data, in_color="green")
    else:
        typer.echo("Okay. Thank you!")
```

If we run this function by writing in terminal `python app.py show-transcript`, it will show student's transcript.

<p align="center">
  <img src="/assets/images/banners/srd_part1/show_transcript.png" width="500">
  <em>Running "show-student" for querying student's transcript</em>
</p>

<b>So, this is the end of Student Registration Database project, during which I had fun exploring and learning more about SQL :)</b>

## **Conclusions**

During this project, I learned these following points:

<p style="margin-top:-10px"></p>

<ol>
  <li>Creating ERD (Entity Relationship Diagram)</li>
  <li>Creating DDL (Data Definition Language: CREATE), DML (Data Manipulation Language: SELECT, INSERT, UPDATE), TCL (Transaction Control Language: COMMIT)</li>
  <li>Applying constraints on MySQL schema</li>
  <li>Applying parameterized statements</li>
  <li>Creating trigger, function and temporary table</li>
  <li>Building CLI-based application</li>
</ol>

<p style="margin-bottom:30px"></p>


Find the code for this project [Github](https://github.com/nuki-susanti/Student-Registration-Database)\
Connect with me! [Linkedin](https://www.linkedin.com/in/nukilsusanti/)
