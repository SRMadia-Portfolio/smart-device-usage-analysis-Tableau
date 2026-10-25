# Smart device usage trend analysis in Tableau

## :hammer_and_wrench: Utilized tools
- Microsoft Excel
- Microsoft SQL Server 
- Tableau

## :memo: Project overview
The project is based on a case study to determine smart device usage trends which could influence Bellabeat products. Furthermore, utilizing the trends to influence marketing campaigns for the various Bellabeat products.

## :mag: Datasets
Two datasets were used in order to answer the business task; Fitbit Fitness Tracker Data and Fitness Tracker Dataset. These datasets were sourced from an open-source platform Kaggle. The datasets were assessed for their credibility, bias, limitation and relevance.

## :clipboard: Data cleaning notes
### FitBit Fitness Tracker Data – Daily activities:
- Cleaned the Daily activity CSV in Excel
- Standardizing metrics - KM
- Formatted all cells to numbers and used 2 decimal places in relevant cells
- Aligned all numbered data to left to ensure consistent formatting
- Number of cells had blanks in various columns

### FitBit Fitness Tracker Data – Weight log:
- Cleaned the Weight log CSV in Excel
- Standardizing metrics – Kg, therefore, deleting the weight measured in pounds
- Deleted the Manual report and Fat column due to relevance
- Extracted the Date from the datetime cells
- Formatted all cells to numbers and used 2 decimal places in relevant cells
- Aligned all numbered data to left to ensure consistent formatting

### Fitness Tracker Dataset:
- Cleaned the dataset CSV in Excel
- Changed all data with numbers from general to numbers
- Removed the decimals in the Age column
- Aligned all numbered data to left to ensure consistent formatting
- Number of cells had blanks in various columns
- Removed text from cells in the Max_BPM column
- Corrected the spelling of the Workout_types in some rows; Strength, Cardio and Yoga

## :notebook: Statistical analysis on SQL
### FitBit Fitness Tracker Data – Daily activities
Performed the following statistical calculations for 35 users based on data collected over 1-2 months period:

Average Total steps per User ID;
```sql
SELECT DISTINCT ID,
AVG(TotalSteps) OVER (PARTITION BY ID) AS StepsAVG
FROM Fitness.dbo.dailyActivity_merged$
```
Average Total distance per User ID;
```sql
SELECT DISTINCT ID,
AVG([TotalDistance-KM]) OVER (PARTITION BY ID) AS DistanceAVG
FROM Fitness.dbo.dailyActivity_merged$
```
Average Very Active Distance per User ID;
```sql
SELECT DISTINCT ID,
AVG([VeryActiveDistance-KM]) OVER (PARTITION BY ID) AS VeryActiveDistanceAVG
FROM Fitness.dbo.dailyActivity_merged$
```
Average Moderately Active Distance per User ID;
```sql
SELECT DISTINCT ID,
AVG([ModeratelyActiveDistance-KM]) OVER (PARTITION BY ID) AS ModeratelyActiveDistanceAVG
FROM Fitness.dbo.dailyActivity_merged$
```
Average Light Active Distance per User ID;
```sql
SELECT DISTINCT ID,
AVG([LightActiveDistance-KM]) OVER (PARTITION BY ID) AS LightActiveDistanceAVG
FROM Fitness.dbo.dailyActivity_merged$
```
Average Very Active Minutes per User ID;
```sql
SELECT DISTINCT ID,
AVG([VeryActiveMinutes]) OVER (PARTITION BY ID) AS VeryActiveMinutesAVG
FROM Fitness.dbo.dailyActivity_merged$
```
Average Fairly Active Minutes per User ID;
```sql
SELECT DISTINCT ID,
AVG([FairlyActiveMinutes]) OVER (PARTITION BY ID) AS FairlyActiveMinutesAVG
FROM Fitness.dbo.dailyActivity_merged$
```
Average Lightly Active Minutes per User ID;
```sql
SELECT DISTINCT ID,
AVG([LightlyActiveMinutes]) OVER (PARTITION BY ID) AS LightlyActiveMinutesAVG
FROM Fitness.dbo.dailyActivity_merged$
```
Number of users with more than the recommended average total daily steps (i.e. 6000 steps); 18 Users

Initially created a CTE which translates into the main query;
```sql
WITH TotalStepsAVG AS
(SELECT DISTINCT ID,
AVG(TotalSteps) OVER (PARTITION BY ID) AS StepsAVG
FROM Fitness.dbo.dailyActivity_merged$)

SELECT COUNT(DISTINCT ID) AS Users
FROM TotalStepsAVG
WHERE StepsAVG > 6000

WITH LowActivityUsers AS
(SELECT DISTINCT ID,
AVG(LightlyActiveMinutes) OVER (PARTITION BY ID) AS LightlyActiveMinutesAVG
FROM Fitness.dbo.dailyActivity_merged$)

SELECT COUNT(DISTINCT ID) AS Users
FROM LowActivityUsers
WHERE LightlyActiveMinutesAVG >= 100 AND LightlyActiveMinutesAVG <= 200
```
Number of users with >200 minutes of Lightly active minutes; 15 Users

Initially created a CTE which translates into the main query;
```sql
WITH ModerateActivityUsers AS
(SELECT DISTINCT ID,
AVG(LightlyActiveMinutes) OVER (PARTITION BY ID) AS LightlyActiveMinutesAVG
FROM Fitness.dbo.dailyActivity_merged$)

SELECT
COUNT(DISTINCT ID) AS Users
FROM ModerateActivityUsers
WHERE LightlyActiveMinutesAVG >=200
```
Fitness Tracker Dataset:
Gender count; Male - 857 and Female - 872
```sql
SELECT Gender, COUNT(*) AS Gender_count
FROM Fitness.dbo.gym_members_exercise_tracking_s$
GROUP BY Gender
```
Gender count per workout type;
```sql
SELECT Workout_Type, Gender, COUNT(*) AS Gender_count
FROM Fitness.dbo.gym_members_exercise_tracking_s$
GROUP BY Workout_Type, Gender
ORDER BY Workout_Type, Gender
```
Average calories burnt per workout type;
```sql
SELECT Workout_Type,
    AVG(Calories_Burned) AS Average_Calories
FROM Fitness.dbo.gym_members_exercise_tracking_s$
GROUP BY Workout_Type
```

## :bar_chart: Data Visualization - Tableau 
Chose olive (#BAB86C) and violet (#CF9FFF) due to colour psychology whereby men preferred olive and women preferred colours such as violet. Bar graphs were used to showcase the difference between user usage per dataset or preferred workout types. Lastly, the tree map gave the most distinct calculation variation when determining the average calorie burn per workout type. 
![pdf alt](https://github.com/SRMadia-Portfolio/smart-device-usage-analysis-Tableau/blob/d7137654fe13078cdb73a6332e410f5142a3041e/Bellabeat%20dashboard.pdf)
![pdf alt](https://github.com/SRMadia-Portfolio/smart-device-usage-analysis-Tableau/blob/d7137654fe13078cdb73a6332e410f5142a3041e/Bellabeat%20dashboard%20-%202.pdf)

## :pushpin: Actions and limitations
Based on the two datasets which were selected to analyze smart devices usage trends in relation to physical activities or health awareness, Bellabeat can focus on two types of people who are on their health journey. The first type; are professional athletes who are stats driven and seek to gather a holistic data view of their health, whether active or inactive. The second type; would be the everyday person who has started or is currently on health journey. The aforementioned type, would need to simplest type of data collection and view of their journey. 

Limiting factors regarding these datasets were; datasets being dated or not collected during the same period. Secondly, not all users had a consistent collection of data for the two – three months and blanks in between the datasets likely led to the calculations having an undesirable margin of error.  

The conclusion would be to determine the marketing campaign’s budget and whether to focus on either type or both but have a staggered approach in order not to confuse the target market.
