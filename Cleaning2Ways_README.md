# Cleaning 2 Ways Report
Descriptive report on cleaning the data downloaded from the cylcylist_bikeshare in 2 ways
  
## Cleaning Checklists:
1. Column selection
2. Null data cells
3. Duplications
4. Misspelled words
5. Mismatched datypes
6. Inconsistent date formats
7. Confusing variable labels
8. Extra data input

## First Way of Cleaning: Using Excel/Google Sheets
#### Used data set starting with trip_22_01
Because a year's worth of data cannot be imported at once into Excel/Google Sheets due to a large file, indivual monthly datasets need to be cleaned. Or all cleaned in SQL.

### Column Selection
For this project, it was important to select only the necessary columns, to make it easier to skip the cleaning processes. These columns were selected for the project:
  * `ride_id`
  * `rideable_type`
  * `started_at`
  * `ended_at`
  * `start_station_name`
  * `end_station_name`
  * `member_casual`
All other columns were deleted.
Added `duration_min` column for the distance in minutes used. This can be found by using the difference equation of started_at and ended_at columns.

#### Nulls
Nulls were found by using the `Filtering` tool. Multiple null cells found in columns:
  * `start_station_name`
  * `end_station_name`
  * `member_casual`
By filtering out the nulls (or blanks), rows were deleted.

#### Duplications
In order to find duplicates, used the `Remove duplicates` tool under `Data Cleanup`. And also filtered any misspelled words with the `Filtering` tool.

#### Misspelled Words
Mispelled words were checked with the `Spelling` tool, and/or using the ChangeCase Extension
  
#### Mismatched datypes
Dataypes were not needed to be cleaned with Excel/Google Sheets

#### Confusing variable labels
`user_type <- member_casual`  
* `member_casual` was not a defining variable. Variable changed to `user_type`.

#### Extra data input
Used the `MINUTE` function to find the minutes of duration used in the `duration_min` column:
`=MINUTE(D2-C2)`
Then dragged the function down the whole column for results.
Using the `Filtering` tool any data that was less than 1 minute and over 24 hours of usage was taken out. Less than a minute usage could have been an accident, and over 24 hours of usage could have been due to not closing out the ride.

## Second Way of Cleaning: Using SQL (MYSQL 18)
### Used data set starting with trip_22_02

### Column Selection
For this project, it was important to select only the necessary columns, to make it easier for the cleaning processes. These columns were selected for the project:
  * `ride_id`
  * `rideable_type`
  * `started_at`
  * `ended_at`
  * `start_station_name`
  * `end_station_name`
  * `member_casual`

#### Nulls
Used the Not Null function to only select rows that do not have null cells in those columns.
(Could not take out null function during the importing phase, as MYSQL 18 does not allow you to not select the nulls)
Multiple null cells found in columns:
  * `start_station_name`
  * `end_station_name`
  * `member_casual`

#### Duplications
In order to find duplicates, used the quering to find duplicates:
```SQL
SELECT 
	ride_id, 
	COUNT(ride_id), 
	start_station_name, 
	end_station_name
FROM 
	trip_22-02
GROUP BY 
	ride_id, 
	start_station_name, 
	end_station_name
HAVING 
	COUNT(ride_id) > 1
```
`DISTINCT` was then used to filter out the duplicates regarding ride_id:  
`SELECT DISTINCT ride_id`  
 
#### Misspelled Words
Using the 'WATSON TESTING - DIVVY'
Station_name 'WATSON TESTING - DIVVY' appears, wide range of start and end times, as well as duration. All rideable_type listed as `electric_bike` , and all member_casual listed as `casual`. 
  
#### Mismatched datypes
String data inconsistent - 2020 data accepted as `nvarchar(50)`, 2021 data accepted as `nvarchar(MAX)` or `nvarchar (100)` 
`start_station_name`, `end_station_name` altered to `nvarchar(100)`. Remaining string variables are all `nvarchar(50)`
 
Date format consistent to `datetime2`.

#### Misleading variable labels
`user_type <- member_casual`  
* `member_casual` was not a defining variable. Variable changed to `user_type`.  

#### Extra data input
Added an extra column to show the number of minutes of duration usage as `duration_min':
`DATEDIFF(minute, started_at, ended_at) AS duration_min`
Then filtered out any data that was less than 1 minute and over 24 hours of usage. Less than a minute usage could have been an accident, and over 24 hours of usage could have been due to not closing out the ride. The `DATEDIFF` function was used in the `WHERE` clause:
	`DATEDIFF(minute, started_at, ended_at) < 1440
AND 
	DATEDIFF(minute, started_at, ended_at) > 1`

### Current Clean Data Query
#### updated 2022-19-02 -- bike_trip_clean
  
```SQL
SELECT
	DISTINCT ride_id,
	member_casual AS user_type,
	rideable_type AS bike_type,
	start_station_name,
	end_station_name,
	started_at,
	ended_at,
	DATEDIFF(minute, started_at, ended_at) AS duration_min
 FROM
	trip_22-02
 WHERE
		(start_station_name IS NOT NULL 
		AND end_station_name IS NOT NULL)
	AND
		(start_station_name <> 'WATSON TESTING - DIVVY'
		AND end_station_name <> 'WATSON TESTING - DIVVY')
	AND
		DATEDIFF(minute, started_at, ended_at) < 1440
	AND 
		DATEDIFF(minute, started_at, ended_at) > 1
    
