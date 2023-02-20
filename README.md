# Cleaning 2 Ways Report
Descriptive report on cleaning the data downloaded from the cylcylist_bikeshare in 2 ways
  
## Cleaning Checklists:
* Null data cells
* Duplications
* Misspelled words
* Mistyped numbers
* Mismatched datypes
* Messy/Inconsistent strings
* Messy/Inconsistent date formats
* Misleading variable labels

## First Way of Cleaning: Using Excel/Google Sheets

## Second Way of Cleaning: Using SQL (MYSQL 18)
### Used data set starting with trip_22_02

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
	22-02
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
7 total station_names found with (\*) as a suffix. Appears to be intentional rather than separate inconsistent format. Will not exclude rows.  
   
Station_name 'WATSON TESTING - DIVVY' appears, wide range of start and end times, as well as duration. All rideable_type listed as `electric_bike` , and all member_casual listed as `casual`. Account for less than 1% of data. Will remove rows.  
  
#### Misspelled Numbers
Only geographical coordinates are listed as number datatypes.  
Coordinates are irrelevant to analysis and will be excluded from the cleaned table.  
  
 
#### Mismatched datypes
String data inconsistent - 2020 data accepted as `nvarchar(50)`, 2021 data accepted as `nvarchar(MAX)`  
`start_station_name`, `end_station_name` altered to `nvarchar(100)`. Remaining string variables all `nvarchar(50)`
 
Date format consistent `datetime`.

#### Misleading variable labels
`user_type <- member_casual`  
* `member_casual` narrow in it's description. Variable changed to `user_type`.  
  
`bike_type <- rideable_type`  
* Only bikes are offered to ride. Makes variable more clear.  
  
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
	22-01
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
    
