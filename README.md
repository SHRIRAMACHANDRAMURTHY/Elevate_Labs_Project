# Elevate_Labs_Project
Sports Tournament Tracker and Movie Review and Recommendation Engine

Sports Tournament Tracker


Introduction
The Sports Tournament Tracker project provides a database-driven solution for managing and analyzing sports
tournament data. It records player statistics, match results, and team performances using SQL. The system
enables efficient querying and reporting through MySQL, helping organizers track results and generate
leaderboards seamlessly.
Abstract
This project creates a relational database to monitor tournament activities. The database captures details of
teams, players, and matches while allowing dynamic analysis through SQL queries, CTEs, and views. The tracker
can display player performance, calculate averages, and export team standings. It ensures data consistency and
makes sports management easier and more transparent.
Tools Used
- Database System: MySQL
- Modeling Tool: MySQL Workbench / phpMyAdmin
- Export Utility: MySQL Workbench CSV Export
- Environment: Localhost (XAMPP / WAMP)
Steps Involved in Building the Project
Create Schema
CREATE TABLE Teams (
 TeamID INT PRIMARY KEY AUTO_INCREMENT,
 TeamName VARCHAR(50),
 CoachName VARCHAR(50)
);
CREATE TABLE Players (
 PlayerID INT PRIMARY KEY AUTO_INCREMENT,
 Name VARCHAR(50),
 TeamID INT,
 Role VARCHAR(30),
 FOREIGN KEY (TeamID) REFERENCES Teams(TeamID)
);
CREATE TABLE Matches (
 MatchID INT PRIMARY KEY AUTO_INCREMENT,
 Team1ID INT,
 Team2ID INT,
 MatchDate DATE,
 WinnerID INT,
 FOREIGN KEY (Team1ID) REFERENCES Teams(TeamID),
 FOREIGN KEY (Team2ID) REFERENCES Teams(TeamID)
);
CREATE TABLE Stats (
 StatID INT PRIMARY KEY AUTO_INCREMENT,
 PlayerID INT,
 MatchID INT,
 Runs INT,
 Wickets INT,
 Points INT,
 FOREIGN KEY (PlayerID) REFERENCES Players(PlayerID),
 FOREIGN KEY (MatchID) REFERENCES Matches(MatchID)
);
Insert Sample Data
INSERT INTO Teams (TeamName, CoachName)
VALUES ('Warriors', 'John Doe'), ('Titans', 'Alex Smith');
INSERT INTO Players (Name, TeamID, Role)
VALUES ('Mike', 1, 'Batsman'), ('Ryan', 2, 'Bowler');
Write Queries for Match Results and Player Scores
SELECT M.MatchID, T1.TeamName AS Team1, T2.TeamName AS Team2, T3.TeamName AS Winner
FROM Matches M
JOIN Teams T1 ON M.Team1ID = T1.TeamID
JOIN Teams T2 ON M.Team2ID = T2.TeamID
JOIN Teams T3 ON M.WinnerID = T3.TeamID;
SELECT P.Name, SUM(S.Runs) AS TotalRuns, SUM(S.Wickets) AS TotalWickets
FROM Stats S
JOIN Players P ON S.PlayerID = P.PlayerID
GROUP BY P.Name;
Create Views for Leaderboards and Points Tables
CREATE VIEW TeamLeaderboard AS
SELECT T.TeamName, SUM(S.Points) AS TotalPoints
FROM Stats S
JOIN Players P ON S.PlayerID = P.PlayerID
JOIN Teams T ON P.TeamID = T.TeamID
GROUP BY T.TeamName
ORDER BY TotalPoints DESC;
Use CTE for Average Player Performance
WITH PlayerAverages AS (
 SELECT PlayerID, AVG(Runs) AS AvgRuns, AVG(Wickets) AS AvgWickets
 FROM Stats
 GROUP BY PlayerID
)
SELECT P.Name, PA.AvgRuns, PA.AvgWickets
FROM PlayerAverages PA
JOIN Players P ON PA.PlayerID = P.PlayerID;
Export Team Performance Reports
SELECT * FROM TeamLeaderboard;
The results can be exported as CSV or Excel reports from MySQL Workbench for analysis and record-keeping.
Conclusion
The Sports Tournament Tracker demonstrates effective use of SQL for managing and analyzing sports data.
Through schemas, queries, views, and CTEs, it provides a comprehensive solution for monitoring matches and
performance. This scalable system can be expanded into a full-stack application with live score tracking and web
dashboards.





===============================================================================================================================================================================================================






Movie Review and Recommendation Engine



Introduction
The Movie Review and Recommendation Engine project aims to analyze user preferences and movie ratings to
generate personalized recommendations. Using PostgreSQL, it efficiently manages movie data, user feedback,
and computed rankings. The project helps understand viewer behavior and trends while automating the
recommendation process through SQL queries and analytical views.
Abstract
This project designs a relational database for storing and analyzing movie-related information. It includes data
about users, movies, ratings, and reviews while applying SQL techniques to calculate averages, identify top-rated
titles, and recommend similar movies. The use of PostgreSQL’s analytical and window functions ensures powerful
data handling, making the system suitable for streaming platforms and entertainment analytics.
Tools Used
- Database System: PostgreSQL
- Data Modeling Tool: pgAdmin / DBeaver
- Data Export: CSV or Query Output from PostgreSQL
- Environment: Localhost (PostgreSQL Server)
Steps Involved in Building the Project
Define Schema
CREATE TABLE Movies (
 MovieID SERIAL PRIMARY KEY,
 Title VARCHAR(100),
 Genre VARCHAR(50),
 ReleaseYear INT
);
CREATE TABLE Users (
 UserID SERIAL PRIMARY KEY,
 UserName VARCHAR(50),
 Age INT,
 Country VARCHAR(50)
);
CREATE TABLE Ratings (
 RatingID SERIAL PRIMARY KEY,
 MovieID INT,
 UserID INT,
 Rating DECIMAL(2,1),
 FOREIGN KEY (MovieID) REFERENCES Movies(MovieID),
 FOREIGN KEY (UserID) REFERENCES Users(UserID)
);
CREATE TABLE Reviews (
 ReviewID SERIAL PRIMARY KEY,
 MovieID INT,
 UserID INT,
 ReviewText TEXT,
 FOREIGN KEY (MovieID) REFERENCES Movies(MovieID),
 FOREIGN KEY (UserID) REFERENCES Users(UserID)
);
Insert Sample IMDb-Style Data
INSERT INTO Movies (Title, Genre, ReleaseYear)
VALUES ('Inception', 'Sci-Fi', 2010),
 ('The Dark Knight', 'Action', 2008),
 ('Interstellar', 'Adventure', 2014);
INSERT INTO Users (UserName, Age, Country)
VALUES ('Alice', 25, 'USA'),
 ('Bob', 30, 'UK'),
 ('Charlie', 28, 'Canada');
INSERT INTO Ratings (MovieID, UserID, Rating)
VALUES (1, 1, 9.0), (1, 2, 8.5), (2, 3, 9.5), (3, 1, 8.0);
Write Average Rating and Ranking Queries
SELECT M.Title, AVG(R.Rating) AS AverageRating
FROM Movies M
JOIN Ratings R ON M.MovieID = R.MovieID
GROUP BY M.Title
ORDER BY AverageRating DESC;
Create Views for Recommended Movies
CREATE VIEW RecommendedMovies AS
SELECT M.Title, AVG(R.Rating) AS AvgRating
FROM Movies M
JOIN Ratings R ON M.MovieID = R.MovieID
GROUP BY M.MovieID, M.Title
HAVING AVG(R.Rating) > 8.0
ORDER BY AvgRating DESC;
Use Window Functions to Track Top-Rated Content
SELECT M.Title,
 AVG(R.Rating) AS AvgRating,
 RANK() OVER (ORDER BY AVG(R.Rating) DESC) AS Rank
FROM Movies M
JOIN Ratings R ON M.MovieID = R.MovieID
GROUP BY M.Title;
Export Movie Recommendation Results
SELECT * FROM RecommendedMovies;
The results can be exported as CSV files or integrated into a dashboard for users to view top-recommended
movies.
Conclusion
The Movie Review and Recommendation Engine showcases how PostgreSQL can be used to analyze user
feedback and generate intelligent content suggestions. Through structured schemas, views, and window
functions, it delivers a scalable recommendation system ideal for entertainment platforms. The project
demonstrates the power of SQL in combining data analysis with personalization.
