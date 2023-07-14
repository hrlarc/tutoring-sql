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
•	Evaluating 2023 Programme
1.	a summary of schools and tutors that participated in 2023.
2.	Improvement in English and Maths results
3.	Tutor satisfaction and its impact on returning tutors
•	Planning for 2024 Programme
1.	Number of sessions expected
2.	Current and target numbers of tutors
3.	How many tutors need to be hired for each subject and each day.

## Solution
### Part 1
I created a database schema using dbdiagram.io to show relationships between the primary key columns of different tables.
 
Next I imported the script into Postbird, a GUI for PostgreSQL.
Using Excel, I created csv files with headings to match the tables in Postbird. For numeric data I used RANDBETWEEN and RANDINT to create realistic numbers. For name and email data, I used open-source sample data.
Using the command prompt, I imported the data from the csvs into the database on Postbird.
