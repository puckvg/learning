# Database assignment 

- Save QM7 to SQLAcademy 
- What is SQLAcademy? what is relationship to database?

## What are databases / what is SQL?
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

# Why query ? 
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

