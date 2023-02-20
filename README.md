# Cleaning 2 Ways Report
Descriptive report on cleaning the data downloaded from the cylcylist_bikeshare in 2 ways
  
## Cleaning Checklists:
* Column Selection
* Null data cells
* Duplications
* Misspelled words
* Mismatched datypes
* Messy/Inconsistent date formats
* Misleading variable labels

## First Way of Cleaning: Using Excel/Google Sheets
### Used data set starting with trip_22_01
Because a year's worth of data cannot be imported at once into Excel/Google Sheets due to a large file, indivual monthly datasets need to be cleaned. Or all cleaned in SQL.

### Column Selection
For this project, it was important to select only the necessary columns, to make it easier to skip cleaning processes. These columns were selected for the project:
  * `ride_id`
  * `rideable_type`
  * `started_at`
  * `ended_at`
  * `start_station_name`
  * `end_station_name`
  * `member_casual`
All other columns were deleted.

#### Nulls
Nulls were found by using the `Filtering` Tool. Multiple null cells found in columns:
  * `start_station_name`
  * `end_station_name`
  * `member_casual`
By filtering out the nulls (or blanks), rows were deleted.

#### Duplications
In order to find duplicates, used the quering to find duplicates:
```Excel/Google Sheets

```
Data was removed from table through use of `DISTINCT` regaring ride_id  
`SELECT DISTINCT ride_id`  
 
#### Misspelled Words

  
#### Mismatched datypes
Dataypes were not needed to be cleaned with Excel/Google Sheets

#### Misleading variable labels
`user_type <- member_casual`  
* `member_casual` was not a defining variable. Variable changed to `user_type`.


## Second Way of Cleaning: Using SQL (MYSQL 18)
### Used data set starting with trip_22_02

### Column Selection
For this project, it was important to select only the necessary columns, to make it easier to skip cleaning processes. These columns were selected for the project:
  * `ride_id`
  * `rideable_type`
  * `started_at`
  * `ended_at`
  * `start_station_name`
  * `end_station_name`
  * `member_casual`

#### Nulls
Multiple null cells found in columns:
  * `start_station_name`
  * `end_station_name`
  * `member_casual`
Used the Not Null function to only select rows that do not have null cells in those columns.
(Could not take out null function during the importing phase, as MYSQL 18 does not allow you to not select the nulls)

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
Data was removed from table through use of `DISTINCT` regaring ride_id  
`SELECT DISTINCT ride_id`  
 
#### Misspelled Words

Using the 'WATSON TESTING - DIVVY'
Station_name 'WATSON TESTING - DIVVY' appears, wide range of start and end times, as well as duration. All rideable_type listed as `electric_bike` , and all member_casual listed as `casual`. 
  
#### Mismatched datypes
String data inconsistent - 2020 data accepted as `nvarchar(50)`, 2021 data accepted as `nvarchar(MAX)`  
`start_station_name`, `end_station_name` altered to `nvarchar(100)`. Remaining string variables all `nvarchar(50)`
 
Date format consistent `datetime`.

#### Misleading variable labels
`user_type <- member_casual`  
* `member_casual` was not a defining variable. Variable changed to `user_type`.  
  
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
	DATEDIFF(minute, started_at, ended_at) AS duration
 FROM
	trip_22-02
 WHERE
		(start_station_name IS NOT NULL 
		AND end_station_name IS NOT NULL)
	AND
		(start_station_name <> 'WATSON TESTING - DIVVY'
		AND end_station_name <> 'WATSON TESTING - DIVVY')
	AND
		DATEDIFF(second, started_at, ended_at) < 86400
	AND 
		DATEDIFF(second, started_at, ended_at) > 60
    
