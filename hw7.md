# Database assignment 

- Save QM7 to SQLAcademy 
- What is SQLAcademy? what is relationship to database?
 
- First looking at regular SQL with khanacademy 

# SQL Basics 
##  What are databases / what is SQL?
- A database stores data and has functionality for adding, modifying and querying data, fast 
- Relational database : stores data in a table. row is an item, column is properties 
- Relational database makes it easy to relates between tables
- Eg for Khan Academy they might have a users table relating names to location and another table relating name to points / badges 
- Can easily relate which users earned which badges by mapping user IDs to badge IDs 
- More efficient than storing all info about user, all info about badge 
- Databases have a query language to interact with the database 
- SQL is designed to access databases and is the most popular 

## Creating a table and inserting data 
- Eg we have a grocery list: 
```
4 Bananas 
1 Peanut butter 
2 Dark chocolate bars
```

- Which we want to turn into a table: 
```
CREATE TABLE groceries (id INTEGER PRIMARY KEY, name TEXT, quantity INTEGER);
```
- Here the columns are in brackets, along with their types
- Also need unique identifier for each row 
- `PRIMARY KEY` identifies the primary identifier
- Now it's empty 

```
INSERT INTO groceries VALUES (1, "Bananas", 4);
INSERT INTO groceries VALUES (2, "Peanut butter", 1);
INSERT INTO groceries VALUES (3, "Dark chocolate bars", 2);
```

- If we want to access some data we can use a `SELECT` statement: 
```
SELECT * FROM groceries;
```

## Querying the table 
- Consider an expanded groceries table with a new column for its aisle in the supermarket: 
```
CREATE TABLE groceries (id INTEGER PRIMARY KEY, name TEXT, quantity INTEGER, aisle INTEGER);
INSERT INTO groceries VALUES (1, "Bananas", 4, 6); 
INSERT INTO groceries VALUES (2, "Peanut butter", 1, 2);
INSERT INTO groceries VALUES (3, "Dark chocolate", 2, 2);
INSERT INTO groceries VALUES (4, "Ice cream", 1, 12); 
INSERT INTO groceries VALUES (5, "Cherries", 6, 2);
INSERT INTO groceries VALUES (6, "Chocolate syrup", 1, 4);
```
### How to retrieve all rows? 
- Any query is `SELECT`
- E.g. `SELECT name FROM groceries;` gives the column `name` 
- If we want ALL the column names, we can `SELECT * FROM groceries;`

- Order by aisle: `SELECT * FROM groceries ORDER BY aisle;`
- Filter results: `SELECT * FROM groceries WHERE aisle > 5 ORDER BY aisle;`

## Aggregating data 
- Want to know total number of items 
- Aggregate function - max/min/sum/average etc 
- `SELECT SUM(quantity) FROM groceries;`
- Group items: `SELECT SUM(quantity) FROM groceries GROUP BY aisle;`
- `SELECT aisle; SUM(quantity) FROM groceries GROUP BY aisle;`

- First `GROUP BY`, then `SUM`, then `SELECT` (i.e. backwards) 
- Shouldn't use something different than what you're grouping by even though it will return something 

# More advanced SQL queries
## More complex queries with AND/OR
- Track exercise 
```
CREATE TABLE exercise_logs
    (id INTEGER PRIMARY KEY AUTOINCREMENT,
    type TEXT,
    minutes INTEGER, 
    calories INTEGER,
    heart_rate INTEGER);


INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("biking", 30, 100, 110);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("biking", 10, 30, 105);
INSERT INTO exercise_logs(type, minutes, calories, heart_rate) VALUES ("dancing", 15, 200, 120);

SELECT * FROM exercise_logs;
```

- New insert statements: specifying column names in parentheses after table name (don't have to specify every column value) 
- Missing ID column - now set to autoincrement 

- Max exercise: 
`SELECT * FROM exercise_logs WHERE calories > 50 ORDER BY calories;`
- Burn > 50 calories but took < 30 mins: 
`SELECT * FROM exercise_logs WHERE calories > 50 AND minutes < 30;`

- Could also use OR operator 
- AND has precedence over OR if both in the same query 
- But can also use brackets 

## Querying IN subqueries 
- We could have a list of OR statements: 
`SELECT * FROM exercise_logs WHERE type == "biking" or type == "hiking" OR type == "tree climbing" OR type =="rowing";`
- Instead can use IN statement: 
`SELECT * FROM exercise_logs WHERE type IN ("biking", "hiking", "tree climbing", "rowing");`
- Can also use NOT IN 

- Can compare two tables eg a new one for Drs Favourites 
- Query can always be up to date with what is in another query 

`SELECT * FROM exercise_logs WHERE type IN (SELECT type FROM drs_favorites);`

- This is a subquery 
- What if we only want favourites for cardiovascular reasons? 

`SELECT * FROM exercise_logs WHERE type IN (SELECT type FROM drs_favorites WHERE reason == "Increases cardiovascular health");`

- Here we look for an exact match which is too strong 
- Use `LIKE` operator with WILDCARD: 
`SELECT * FROM exercise_logs WHERE type IN (SELECT type FROM drs_favorites WHERE reason LIKE "%cardiovascular%");`

## Querying HAVING 
- `SELECT type, SUM(calories) FROM exercise_logs GROUP BY type;`
- Gives type w total calories 
- Column shows `SUM(calories)` 
- Give new name using AS: `SELECT type, SUM(calories) AS total_calories FROM exercise_logs GROUP BY type;`
- Filter results where burned > 150 calories total: 
`SELECT type, SUM(calories) AS total_calories FROM exercise_logs GROUP BY type HAVING total_calories > 150;`
- `HAVING` applies condition to grouped values unlike `WHERE` which is on single values 
- Average calories for exercises where we burned > 100 average: 
`SELECT type, AVG(calories) AS avg_calories FROM exercise_logs GROUP BY type HAVING avg_calories > 70;`

- COUNT operator tells us how often we did activity: `SELECT type FROM exercise_logs GROUP BY type HAVING COUNT(*) >= 2;`

## Why query ? 
- Imagine we have an exercise app with thousands of users storing exercise logs 
- App lets users enter daily logs and view progress on a personal dashboard 
- Software engineers will build the backend (server side logic) and front end (HTML/CSS/JS that renders data and forms) 
- Software engineers would use SQL to communicate on server-side with database that is storing user data 
- They would need to know how to do any queries needed by frontend 
- For example, if users saw a dashboard when they logged the exercise they did that day, the engineer would need to `SELECT` filter by date and user 
- They also need to insert data and update it (onto that later) 
- Data scientists will analyse the data, learning more about users, understanding how to help them exercise more 
- They will use `SELECT` statements to query the data to do the analysis 
- They might use a `SELECT` to analyse what percentage of users were more likely to exercise if they started in the morning maybe using `CASE` and `GROUP BY`
- Product managers are the decision makers at a company - look at the data, talk to users, look at the market, and understand how to improve a product to get more users/ make users happy / make more money 
- Use SQL queries to look at usage statistics and understand which products are used most 
- Might use `SELECT` to look at how many users use the `heart_rate` field at all, and whether to get rid of it 

## Calculating results with CASE 
- Logged heart_rate for cases 
- Max heart_rate is 220 - age 
- Query logs to see if heart rate goes above max - something wrong with device 
- `SELECT COUNT(*) FROM exercise_logs WHERE heart_rate > 220 - 30;` 
- Most maths operators work 
- Does heart go into target (50-75% of max)?
- `SELECT COUNT(*) FROM exercise_logs WHERE heart_rate > ROUND(0.5 * (220-30)) AND heart_rate <= ROUND(0.9 * (220-30))`
- Want a summary of logs and how many were in each zone - use CASE statement to "create" column 

- Similar to an `if` 
```
SELECT COUNT(*), 
    CASE 
        WHEN heart_rate > 220-30 THEN "above max" 
        WHEN heart_rate > ROUND(0.9*(220-30)) THEN "above target"
        WHEN heart_rate > ROUND(0.5*(220-30)) THEN "within target"
        ELSE "below target"
    END as "hr_zone"

FROM exercise_logs
GROUP BY hr_zone; 
```

# Relational Queries with SQL  
## Splitting data into related tables 
- A single table is good when data is <b>one-to-one</b>. Multiple tables are better otherwise. 
- Most of the time, we have data in multiple tables, and those tables are related in some way
- Need to understand how to deal with data that is split up across multiple related tables, and bring data back together across tables when you need it 
- Use `join`s

## JOINing related tables 
- Consider two tables: students names/emails and student_grades 
```
CREATE TABLE students (id INTEGER PRIMARY KEY,
    first_name TEXT,
    last_name TEXT,
    email TEXT,
    phone TEXT,
    birthdate TEXT);

INSERT INTO students (first_name, last_name, email, phone, birthdate)
    VALUES ("Peter", "Rabbit", "peter@rabbit.com", "555-6666", "2002-06-24");
INSERT INTO students (first_name, last_name, email, phone, birthdate)
    VALUES ("Alice", "Wonderland", "alice@wonderland.com", "555-4444", "2002-07-04");
    
CREATE TABLE student_grades (id INTEGER PRIMARY KEY,
    student_id INTEGER,
    test TEXT,
    grade INTEGER);

INSERT INTO student_grades (student_id, test, grade)
    VALUES (1, "Nutrition", 95);
INSERT INTO student_grades (student_id, test, grade)
    VALUES (2, "Nutrition", 92);
INSERT INTO student_grades (student_id, test, grade)
    VALUES (1, "Chemistry", 85);
INSERT INTO student_grades (student_id, test, grade)
    VALUES (2, "Chemistry", 95);
```
- The id's are consistent across the databases 
- Query to output student name and email across test grade
- Across 2 tables : join them 

### Cross join 
- `SELECT * FROM student_grades, students;` creates rows for each 
- Each row in first table it adds a row in each row for second table (2 x 4) 
- Don't want all rows matched with all other rows - but only want match if student ID matches 

### Inner join 
- Implicit inner join: 
- `SELECT * FROM student_grades, students WHERE student_grades.student.id = students.id;`

- Explicit inner join JOIN : 
- `SELECT first_name, last_name, email, test, grade FROM students JOIN student_grades ON students.id = students_grades.student_id;`

- Can also filter after `WHERE grade > 90;`

- What about tables with same column names but different meanings? Eg if grade is in two tables with different meanings 
- Should prefix columns with table name they are from 
- `SELECT students.first_name, students.last_name, students.email, student_grades.test, student_grades.grade FROM students JOIN student_grades ON students.id = student_grades.student_id;`

## Joining related tables with left outer joins 
Status https://www.khanacademy.org/computing/computer-programming/sql/relational-queries-in-sql/pt/joining-related-tables-with-left-outer-joins




# Comments 
- Can you not add all data at once??? Why is it one by one? 




