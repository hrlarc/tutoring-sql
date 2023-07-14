I decided to write my own brief for a project to create a SQL database, write custom queries and produce a Power BI dashboard for analysis. For the topic idea I imagined a simplified version of how the tutoring company that I volunteer for could store and analyse their data.

## Brief
Active Tutoring matches volunteer tutors with students who could benefit from small group sessions in English and Maths. The 2023 programme has just finished. Schools have signed up for the 2024 programme and Active Tutoring wants to assess how many new tutors they will need to hire before then. They also want to evaluate the 2023 programme.

### Part 1
Develop a database to store tutor information, school information, session information and the average results for schools who have already participated in the program.

### Part 2
Answer these questions:
1.	Which tutors scored their satisfaction with the programme as 10/10, and which schools were they at?
2.	Which tutors were least satisfied (bottom 20%) and have they re-enrolled to tutor next year?
3.	Find the names and emails of tutors whose schools achieved more than a 0.8 point increase in their subject to send a congratulations email.

### Part 3
Create a PowerBI dashboard to show:
* Evaluating 2023 Programme
1.	a summary of schools and tutors that participated in 2023.
2.	Improvement in English and Maths results
3.	Tutor satisfaction and its impact on returning tutors
* Planning for 2024 Programme
1.	Number of sessions expected
2.	Current and target numbers of tutors
3.	How many tutors need to be hired for each subject and each day.

## Solution
### Part 1
I created a database schema using dbdiagram.io to show relationships between the primary key columns of different tables.
![alt text](https://github.com/hrlarc/tutoring-sql/blob/main/schema.png "Schema")

Next I imported the script into Postbird, a GUI for PostgreSQL.

Using Excel, I created csv files with headings to match the tables in Postbird. For numeric data I used RANDBETWEEN and RANDINT to create realistic numbers. For name and email data, I used open-source sample data.

Using the command prompt, I imported the data from the csvs into the database on Postbird.
![alt text](https://github.com/hrlarc/tutoring-sql/blob/main/schema.png "Schema")

#### Altering the tables after creation:
* I decided to add an extra table for completed sessions, in order to evaluate the 2023 programme.
* I also added an extra column to Tutors for a satisfaction score.
* I split results into separate English and Maths scores.
* 
After making these changes in the csv files, I ensured new columns could have null values in Postbird, reimported them and deleted any duplicate rows.

![alt text](https://github.com/hrlarc/tutoring-sql/blob/main/schema.png "Schema")

### Part 2
1.	Which tutors scored their satisfaction with the programme as 10/10, and which schools were they at?
```SQL
SELECT tu.satisfaction AS most_satisfied, tu.first_name, s.name
FROM tutors AS tu
LEFT JOIN sessions_2023 AS e
ON e.tutor_id = tu.id
LEFT JOIN schools AS s
ON s.id = e.school_id
WHERE satisfaction =
(SELECT MAX(satisfaction) FROM tutors);
```


Output:
most_satisfied	|first_name|	name
---|---|---
10|	Ronald|	Woodlands
10|	Patricia|	Letchworth
10|	Robert|	St Mary
10|	Linda|	Leftfield
10|	Michael|	Woodlands
10|	Mark|	Greensill 
10|	Betty|	Northbank
10|	William|	Eastbank
10|	Donald|	Letchworth
10|	Elizabeth|	All Saints
10|	Richard|	Woodlands
10|	Charles|	Northbank
10|Steven|	Westbank
10|	Joseph|	Camber High
10|	Dorothy|	Midbrook

2.	Which tutors were least satisfied (bottom 20%) and have they re-enrolled to tutor next year?

``` SQL
SELECT satisfaction, first_name, returning_2024

FROM tutors

ORDER BY satisfaction ASC

LIMIT(50*0.2);
```
Output:
satisfaction|	first_name|	returning_2024
---|---|---
3|	Daniel|	false
3|	Donna|	false
4|	Anthony|	false
5|	Michelle|	false
5|	Sarah|	false
6|	Karen|	false
7|	Jeff|	true
7|	Laura|	true
7|	Kevin|	true
7|	James|	true

3.	Find the names and emails of tutors whose schools achieved more than a 0.8 point increase in their subject to send a congratulations email.


``` SQL
#For Maths
SELECT 

r.ma_results_2023-r.ma_results_2022 AS ma_improve,

s.name,

tu.first_name,

tu.email



FROM results AS r

LEFT JOIN schools AS s

ON s.id = r.school_id

LEFT JOIN sessions_2023 AS e

ON s.id = e.school_id

LEFT JOIN tutors AS tu

ON tu.id = e.tutor_id

WHERE tu.subject = 'Maths' AND r.ma_results_2023-r.ma_results_2022 >= 0.8;
```
Output
ma_improve|	name|	first_name|	email
---|---|---|---
1.1|	Eastbank|	Anthony|	AnthonyLopez@gmail.com
1.6|	St Mary|	Robert|	RobertLewis@hotmail.com
1|	Westbank|	Kimberly|	KimberlyGreen@aol.com
1|	Westbank|	Steven|	StevenEvans@yahoo.com

``` SQL
#For English
SELECT 

r.en_results_2023-r.en_results_2022 AS en_improve,

s.name,

tu.first_name,

tu.email



FROM results AS r

LEFT JOIN schools AS s

ON s.id = r.school_id

LEFT JOIN sessions_2023 AS e

ON s.id = e.school_id

LEFT JOIN tutors AS tu

ON tu.id = e.tutor_id

WHERE tu.subject = 'English' AND r.en_results_2023-r.en_results_2022 >= 0.8;
```
Output
en_improve|	name|	first_name|	email
---|---|---|---
0.9|	All Saints|	Laura|	LauraJackson@hotmail.com
1.8|	St Mary|	Helen|	HelenAdams@aol.com
0.9|	Midbrook|	Jennifer|	JenniferAllen@yahoo.com
0.8|	Woodlands|	Richard|	RichardParker@yahoo.com
1.8|	St Mary|	Margaret|	MargaretEdwards@facebook.com

