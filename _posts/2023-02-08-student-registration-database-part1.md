---
layout: post
title: Building Student Registration Database Part 1 - SQL Learning Series
subtitle: Each post also has a subtitle
categories: [database, SQL]
tags: [database, programming, MySQL, SQL, python]
---


<p align="center">
  <img src="/assets/images/banners/srd_part1/mysql.png" width="300">
  <em>Source: wikipedia</em>
</p>

## **Motivation**

After completing Data Analysis course, I still feel somehow that SQL topic hadn’t been explored sufficiently. In short, I’m not satisfied enough and need more exercises. I’ve done one SQL-based project using PostgreSQL in which SQL query was used to answer business questions. That project honestly really helps me to understand some important concepts of RDBMS, such as join and CTE (Common Table Expression). Now, I would like to explore SQL from the scratch, from SQL programming to querying. This project will be series learning projects. My goal is hopefully, at the end of the learning series, I could level up my SQL knowledge.

<p style="margin-bottom:-30px"></p>

## **Overview - Project and Tools**

I did some research what I could use as a starting point and came across [Student Registration Database](https://datamastery.gitlab.io/exercises/registration-database.html). This is very simple database schema that I could modify into little bit more complex one. So, I decided to pick this one with some modifications:

<p style="margin-top:-10px"></p>

* I want to have interface application to interact with the database. However, as I don’t want to spend time building frontend interface, CLI-based application could be a way out. For this purpose, I use python library called Typer as well as Rich.
* I will use MySQL and cloud-based free RDBMS live environment such as [railway.app](https://railway.app/new). As term “free” suggests, it is free to use but only for 24 hours. Afterwards, it will be automatically erased and we have to make a new project environment. It means that we have also to change the connection every 24 hours.
* As there are no business requirements listed, I may add some constraints or check on the tables along the way (see process point 3).
* All data checking and constraint will be in the database side. Though application (python) could also do this heavy lifting task, however, the aim of this project is that me exploring and learning SQL. So, this might not reflect the ideal condition in the real world.

<b>Tech Stack:</b>

<p style="margin-top:-10px"></p>

* [Typer](https://typer.tiangolo.com) - For building great CLIs that are easy to code
* [Rich](https://rich.readthedocs.io/en/stable/introduction.html) - Rich text and beautiful formatting in the terminal
* Cloud-based database environment – [railway.app](https://railway.app/new)
* And of course python

<p style="margin-bottom:-30px"></p>

## **Process**
<p style="margin-bottom:-50px"></p>

### 1.	Installing Typer and Rich

```python
pip install typer rich
```

### 2.	Creating MySQL provision on railway.app

<div class="container" style="display: inline-block;">  
  <figure>
  <div style="float: left; padding: 10px;">
    <img src="/assets/images/banners/srd_part1/railway1.png" width="300" height="350" align="center"/>
    <figcaption align="center"><i>MySQL environment provided by railway.app</i></figcaption>
  </div>

  <div style="float: right; padding: 10px;">
    <img src="/assets/images/banners/srd_part1/connection.png" width="300" height="350" align="center"/>
    <figcaption align="center"><i>Connection details railway.app</i></figcaption>
  </div>
  </figure>
</div>

In case database shell is to be used in IDE (I used VS Code here and MySQL extension), those connection details must be entered.

### 3.	Creating ERD (Entity Relationship Diagram)

I used [dbdiagram.io](https://dbdiagram.io/) for this purpose.

<p align="center">
  <img src="/assets/images/banners/srd_part1/erd.png" width="800">
  <em>Entity Relationship Diagram (ERD)</em>
</p>

So, I made some <b>improvisation and constraints</b> on the schema:

<p style="margin-top:-10px"></p>

*	I added prerequisites table to address mandatory course to take other courses as well as its corresponding minimum grade to pass. For example: students can’t enroll Mathematics 2 when they haven’t taken Mathematics 1 with a minimum grade of 60.
*	Students can only take same course in a year, meaning if they don’t pass a course in year 2022, they can only enroll again in year 2023.
*	Grade is set between 0 – 100.


### 4. Separating files based on its purpose

I managed to divide the entire application into 4 files:

<p style="margin-top:-10px"></p>

<ol>
  <li><b><i>app.py</i></b> - This is the interface of the application. However, as I mentioned, this is CLI-based application. So, interface is not actually interface as you can still see the code.</li>
  <li><b><i>db.py</i></b> - Storing function for database interaction.</li>
  <li><b><i>sql_query.sql</i></b> - Storing SQL schema, trigger and function.</li>
  <li><b><i>db_seed.py</i></b> - Storing initial data to populate tables.</li>
</ol>

### 5.	Let’s roll!

<p style="margin-bottom:-30px"></p>

## **Part 1**
<p style="margin-bottom:-50px"></p>

### Creating schema

```sql
/*File: sql_query.sql*/
DROP DATABASE IF EXISTS railway;
CREATE DATABASE railway;
USE railway;

CREATE TABLE departments(
    id INTEGER PRIMARY KEY AUTO_INCREMENT,
    dept_id VARCHAR(255) NOT NULL UNIQUE,
    dept_name VARCHAR(255) NOT NULL,
    dean_name VARCHAR(255),
    building VARCHAR(255),
    room INTEGER
);

CREATE TABLE courses(
    id INTEGER PRIMARY KEY AUTO_INCREMENT,
    dept_id VARCHAR(255) NOT NULL,
    course_id VARCHAR(10) NOT NULL UNIQUE,
    course_name VARCHAR(255) NOT NULL,  
    hour INTEGER,
    FOREIGN KEY (dept_id) REFERENCES departments(dept_id)
        ON UPDATE CASCADE
);

CREATE TABLE students(
    id INTEGER PRIMARY KEY AUTO_INCREMENT,
    student_id VARCHAR(10) NOT NULL UNIQUE,
    first_name VARCHAR(255) NOT NULL,
    last_name VARCHAR(255) NOT NULL,
    gpa FLOAT NOT NULL DEFAULT 0
);

CREATE TABLE prerequisites(
    id INTEGER PRIMARY KEY AUTO_INCREMENT,
    course_id VARCHAR(10) NOT NULL,
    course_prereq_id VARCHAR(10) NOT NULL,
    min_grade INTEGER NOT NULL,
    FOREIGN KEY (course_id) REFERENCES courses(course_id)
        ON UPDATE CASCADE,
    CHECK (min_grade >= 0 AND min_grade <= 100)
);

CREATE TABLE enrolled(
    id INTEGER PRIMARY KEY AUTO_INCREMENT,
    student_id VARCHAR(10) NOT NULL,
    course_id VARCHAR(10) NOT NULL,
    enrollment_year YEAR NOT NULL,
    grade INTEGER,
    FOREIGN KEY (student_id) REFERENCES students(student_id) ON UPDATE CASCADE,
    FOREIGN KEY (course_id) REFERENCES courses(course_id) ON UPDATE CASCADE,
    UNIQUE (student_id, course_id, enrollment_year),
    CHECK (grade >= 0 AND grade <= 100)
);
```

### Creating MySQL connection

```python
# File: db.py 
def create_server_connection():
    connection = None
    try:
        connection = connect(
            user=env.get("USERNAME"),
            password=env.get("PASSW"),
            host=env.get("HOST"),
            port=env.get("PORT"),
            database=env.get("DB")
        )

        print("Successfully connected to MySQL")
    except Error as err:
        print(f"Error: '{err}'")

    return connection
```

```python
# File: app.py
@app.command()
def sql_connection():
    create_server_connection()
```

As you can see, connection details are saved in the .env file for confidentiality purpose. Later, it can be easily .gitignore-d.

### Creating table and populating database

<b>Creating tables from schema:</b>

```python
# File: db.py 
def resetting_db():
    try:
        with create_server_connection() as con:
            with con.cursor() as cur:
                with open('sql_query.sql', 'r') as file:
                    for statement in cur.execute(file.read(), multi=True):
                        print(f"Executed: {statement.statement}")
    except Error as err:
        print(f"Error: '{err}'")
```

<b>Creating data seed to populate tables:</b>

```python
# File: db_seed.py
seed_departments = [
    ("cs", "computer science", "rubio", "ajax", 100),
    ("math", "mathematics", "carson", "acme", 300),
    ("ee", "electrical engineering", "kasich", "ajax", 200),
    ("ie", "industrial engineering", "cruz", "", 200),
    ("music", "musicology", "costello", "north", 100)
]

seed_courses = [
    ("cs", "cs101", "programming", 4),
    ("cs", "cs201", "algorithms", 3),
    ("cs", "cs202", "systems", 3),
    ("math", "math101", "algebra", 3),
    ("math", "math201", "calculus", 4),
    ("math", "math301", "analysis", 4),
    ("ee", "ee102", "circuits", 3),
    ("ie", "ie101", "probability", 3),
    ("ie", "ie102", "statistics", 3),
    ("music", "music104", "jazz", 3)
]

seed_prerequisites = [
    ("cs202", "cs201", 75),
    ("cs201", "cs101", 75),
    ("math301", "math201", 70),
    ("math201", "math101", 70),
]

seed_students = [
    ("tb01", "tom", "bush"),
    ("ch01", "cruz", "hening"),
    ("cs01", "clinton", "smith"),
    ("es01", "evan", "sanders")
]
```

<b>Populating database with data seed:</b>

```python
# File: db.py 
def populating_db():

    sql_departments = """
      INSERT INTO departments(dept_id, dept_name, dean_name, building, room) 
      VALUES(%s, %s, %s, %s, %s);
    """
    sql_courses = """
      INSERT INTO courses(dept_id, course_id, course_name, hour) 
      VALUES(%s, %s, %s, %s);
    """
    sql_prerequisites = """
      INSERT INTO prerequisites(course_id, course_prereq_id, min_grade) 
      VALUES(%s, %s, %s);
    """
    sql_students = """
      INSERT INTO students(student_id, first_name, last_name) 
      VALUES(%s, %s, %s);
    """

    with create_server_connection() as con:
        query(con, sql_departments, db_seed.seed_departments, many=True)
        query(con, sql_courses, db_seed.seed_courses, many=True)
        query(con, sql_prerequisites, db_seed.seed_prerequisites, many=True)
        query(con, sql_students, db_seed.seed_students, many=True)
        print("Database is successfully initialized.")
```

Here, query function was created to handle CRUD (Create, read, update and delete) operations and thus, also reducing boiler plate code that will be used during interaction with database.

```python
# File: db.py 
def query(connection, query, data=None, many=False, fetch=None):
    cur = connection.cursor()

    try:
        if many:
            cur.executemany(query, data)
        else:
            cur.execute(query, data)

        if fetch:
            return cur.fetchall()
        else:
            connection.commit()

        print(f"Executed successfully: {query}")

        typer.echo(typer.style("Successful!",
                   bg=typer.colors.WHITE, fg=typer.colors.GREEN))

    except (IntegrityError, DatabaseError, Error) as err:
        typer.echo(
            f"Failed. You have an error message: 
              {typer.style(err, bg=typer.colors.WHITE, fg=typer.colors.RED)}")
    finally:
        cur.close()
```

It is also worth mentioning that parameterized statements “%s” was used rather than literal string for security reason, namely preventing SQL injection.

<b>Interface look:</b>

```python
# File: app.py
@app.command()
def reset_db(data_seed: bool = True):
    user_answer = input(
        "Are you sure? This will delete all the data. (y/n): ").strip().lower()

    if user_answer == "y":
        resetting_db()
        typer.echo("Successfully reset database")

    # It will automatically populate database with data seed stored in db_seed.
    # Otherwise user should specify --no-data-seed
        if data_seed:
            populating_db() 
    else:
        typer.echo("Database reset aborted")
```

Then, when we run this function by writing in terminal `python app.py reset-db`.

<p align="center">
  <img src="/assets/images/banners/srd_part1/terminal1.png" width="500">
  <em>Running "reset-db" for resetting and populating database</em>
</p>

<p align="center">
  <img src="/assets/images/banners/srd_part1/terminal2.png" width="500">
  <em>Resetting database successful</em>
</p>

It seems to work, but let’s check our database.

<p align="center">
  <img src="/assets/images/banners/srd_part1/railway2.png" width="500">
  <em>Tables are successfully created</em>
</p>

So, our database is successfully populated with our initial data.

<p align="center">
  <img src="/assets/images/banners/srd_part1/railway3.png" width="500">
  <em>Tables are successfully populated</em>
</p>

### Updating tables

In this step, I added functionalities to add and update all those tables: departments, courses, students, prerequisites and ‚enrolled.

<p style="margin-bottom:-50px"></p>

#### A. Departments

<b>Database function for updating table "departments":</b>

```python
# File: db.py 
def adding_department(dept_details):
    with create_server_connection() as con:
        sql_script = """
          INSERT INTO departments(dept_id, dept_name, dean_name, building, room) 
          VALUES(%s, %s, %s, %s, %s);
        """
        query(con, sql_script, dept_details)
```

<b>Interface function for updating table "departments":</b>

```python
# File: app.py 
@app.command()
def add_department():
    user_answer = input("Do you want to add a department? (y/n): ").strip().lower()

    if user_answer == "y":
        
        dept_id = input("Enter department ID: ").lower()
        dept_name = input("Enter department name: ").lower()
        dean_name = input("Enter dean name of the department (optional): ").lower()
        building = input("Enter building name (optional): ").lower()
        
        try:
            room = int(input("Enter room number (optional): ").strip())
        except:
            room = None

        dept_details = (dept_id, dept_name, dean_name, building, room)

        adding_department(dept_details)
    else:
        typer.echo("Okay. Thank you!")
```

From this process I found out that when user doesn’t input anything (in other words, empty string) for columns “dept_id” and “dept_name”, it is still inserted into database, though I specified “NOT NULL” constraint in the table schema. Eventually, NOT NULL constraint enforces a column to NOT accept NULL values only. Whereas empty string is still interpreted as a value. This of course will be data integrity problem. In order to prevent this from happening, another check for empty string is enforced.

For the other columns: “dean_name”, “building” and “room”, they can hold NULL values which aren’t necessary to be declared explicitly as the default setting are NULL (DEFAULT NULL). However, a problem will arise when converting empty string to integer, so that it was set to be None as default value to be able to insert into the database.

<b>Updated database schema:</b>

```sql
/*File: sql_query.sql*/
DROP DATABASE IF EXISTS railway;
CREATE DATABASE railway;
USE railway;

CREATE TABLE departments(
    id INTEGER PRIMARY KEY AUTO_INCREMENT,
    dept_id VARCHAR(255) NOT NULL UNIQUE CHECK (dept_id != ""),
    dept_name VARCHAR(255) NOT NULL CHECK (dept_name != ""),
    dean_name VARCHAR(255),
    building VARCHAR(255),
    room INTEGER
);
```

#### B. Courses

<b>Database function for updating table "courses":</b>

```python
# File: db.py
 def adding_course(course_details):
    with create_server_connection() as con:
        sql_script = """
          INSERT INTO courses(dept_id, course_id, course_name, hour) 
          VALUES(%s, %s, %s, %s);
        """
        query(con, sql_script, course_details)
```

<b>Interface function for updating table "courses":</b>

```python
# File: app.py
@app.command()
def add_course():
    user_answer = input("Do you want to add a course? (y/n): ").strip().lower()

    if user_answer == "y":
        dept_id = input("Enter department ID: ").lower()
        course_id = input("Enter course ID: ").lower()
        course_name = input("Enter course name: ").lower()

        try:
            hour = int(input("Enter the duration of the course in hour: ").strip())
        except:
            hour = None

        course_details = (dept_id, course_id, course_name, hour)

        adding_course(course_details)
    else:
        typer.echo("Okay. Thank you!")
```

#### C. Students

<b>Database function for updating table "students":</b>

```python
# File: db.py
def adding_student(student_details):
    with create_server_connection() as con:
        sql_script = "INSERT INTO students(student_id, first_name, last_name) VALUES(%s, %s, %s);"
        query(con, sql_script, student_details)
```

<b>Interface function for updating table "students":</b>

```python
# File: app.py
@app.command()
def add_student():
    user_answer = input("Do you want add a student? (y/n): ").strip().lower()

    if user_answer == "y":
        student_id = input("Enter student id: ").lower()
        first_name = input("Enter student's first name: ").lower()
        last_name = input("Enter student's last name: ").lower()

        student_details = (student_id, first_name, last_name)

        adding_student(student_details)
    else:
        typer.echo("Okay. Thank you!")
```

#### D. Prerequisites

<b>Database function for updating table "prerequisites":</b>

```python
# File: db.py
def adding_prereq(prereq_details):
    with create_server_connection() as con:
        sql_script = """
          INSERT INTO prerequisites(course_id, course_prereq_id, min_grade) VALUES(%s, %s, %s);
        """
        query(con, sql_script, prereq_details)
```

<b>Interface function for updating table "prerequisites":</b>

```python
# File: app.py
@app.command()
def add_prereq():
    user_answer = input(
        "Do you want to add prerequisite for a course? (y/n): ").strip().lower()

    if user_answer == "y":
        course_id = input("Enter course ID: ").lower()
        course_prereq_id = input("Enter course prerequisite: ").lower()

        try:
            min_grade = int(input("Enter the minimum grade for the course: ").strip())
        except:
            min_grade = 50  # default minimum grade            

        prereq_details = (course_id, course_prereq_id, min_grade)

        adding_prereq(prereq_details)
    else:
        typer.echo("Okay. Thank you!")
```

#### E. Enrolled

Enrolled table aims to store students and their enrolled course. However, there are constraints in enrolling student for a course, as I mentioned previously in the beginning:

<p style="margin-top:-10px"></p>

*	Mandatory course has to be taken before taking other consecutive courses with its corresponding minimum grade to pass. For example: students can’t enroll Mathematics 2 when they haven’t taken Mathematics 1 with a minimum grade of 60. The prerequisites of the courses are stored in table “prerequisites”.
*	Students can only take same course in a year, meaning if they don’t pass a course in year 2022, they can only enroll again in year 2023. This has been specified as “UNIQUE” constraint in the schema.

For this purpose, I created trigger to check before inserting (enrolling) student for a course. Trigger must ensure that it checks the prerequisite course and minimum grade.

<b>Trigger for requirements checking before inserting into table "enrolled":</b>

```sql
/*File: sql_query.sql
/*Creating a trigger and temporary table*/
DROP TRIGGER IF EXISTS before_enrolled_insert;
CREATE TRIGGER before_student_course_insert
    BEFORE INSERT ON enrolled 
    FOR EACH ROW
BEGIN
    /*Create temporary tables to hold 1. prerequisites of courses and 2. unmet_prereqs*/
    DROP TEMPORARY TABLE IF EXISTS tempo_prereqs;
    DROP TEMPORARY TABLE IF EXISTS unmet_prereqs;
    CREATE TEMPORARY TABLE IF NOT EXISTS tempo_prereqs(
        course_prereq VARCHAR(10) REFERENCES courses(course_id),
        min_grade INTEGER
    );

    CREATE TEMPORARY TABLE IF NOT EXISTS unmet_prereqs(
        course_prereq VARCHAR(10) REFERENCES courses(course_id)
    );
    
    /*Does this course have prereqs? If yes, insert them into temp_prereq -> filter out the course*/
    INSERT INTO tempo_prereqs(course_prereq, min_grade)
    SELECT course_prereq_id, min_grade FROM prerequisites WHERE course_id = NEW.course_id;
    /*New course ID we want to insert*/

    /*Are there any unmet prereqs? -> student-specific*/
    INSERT INTO unmet_prereqs(course_prereq)
    SELECT course_prereq
    FROM tempo_prereqs AS tp
    WHERE tp.course_prereq NOT IN
        (SELECT e.course_id FROM enrolled AS e WHERE e.student_id= NEW.student_id AND e.grade > tp.min_grade);

    /*If there are, insert will fail and message the user*/
    IF EXISTS (SELECT course_prereq FROM unmet_prereqs) THEN
        SET @message_text = CONCAT(
            'Student ', NEW.student_id, ' cannot take course ', NEW.course_id, 
            ' because not all the prerequisites are met.'
            );

        SIGNAL SQLSTATE '45000'
            SET MESSAGE_TEXT = @message_text;
    END IF;
END;
```

<b>Database function for updating table "enrolled":</b>

```python
# File: db.py
def enrolling_student(enrollment_details):
    with create_server_connection() as con:
        sql_script = "INSERT INTO student_course(student_id, course_id, enrollment_year) VALUES(%s, %s, %s);"
        query(con, sql_script, enrollment_details)
```

<b>Interface function for updating table "enrolled":</b>

```python
# File: app.py
@app.command()
def enroll_student():
    user_answer = input(
        "Do you want to enroll a student? (y/n): ").strip().lower()

    if user_answer == "y":
        student_id = input("Enter your student id: ").strip().lower()
        course_id = input("Enter your course id you want to enroll: ").strip().lower()
        year = datetime.now().year

        enrollment_details = (student_id, course_id, year)

        enrolling_student(enrollment_details)
    else:
        typer.echo("Okay. Thank you!")
```

Further, to support this functionality, `update-grade` command is added so that we can set if a student should/should not pass a course.

<b>Database function for updating grade in table "enrolled":</b>

```python
# File: db.py
def updating_grade(student_id, course_id, year, grade):
    with create_server_connection() as con:
        sql_script = """
          UPDATE enrolled SET grade = %s WHERE student_id = %s 
          AND course_id = %s 
          AND enrollment_year = %s;
        """
        data = (grade, student_id, course_id, year)

        return query(con, sql_script, data=data)
```

<b>Interface function for updating grade in table "enrolled":</b>

```python
# File: app.py
@app.command()
def update_grade():
    user_answer = input("Do you want to update course grade? (y/n): ").strip().lower()

    if user_answer == "y":
        student_id = input("Enter your student id: ").strip().lower()
        course_id = input("Enter the course id: ").strip().lower()

        try:
            year = int(input("Enter the year: ").strip())
        except:
            year = datetime.now().year

        grade = int(input("Enter your grade: ").strip())

        updating_grade(student_id, course_id, year, grade)
    else:
        typer.echo("Okay. Thank you!")
```

## **Conclusions**

<b>This is the end of part 1. I will continue in the part 2 for the rest of functionalities.</b>


<p style="margin-bottom:30px"></p>


Find the code for this project [Github](https://github.com/nuki-susanti/Student-Registration-Database)\
Connect with me! [Linkedin](https://www.linkedin.com/in/nukilsusanti/)
