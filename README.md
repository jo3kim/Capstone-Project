# Capstone Project: Bike-Share
Analysis of the Bike-Share Case Study

## Context
* Background  
* Marketing Team Questions/Business Task
* Data Analysis
* Key Findings
* Recommendations
* Limitations
* References/Software

### Background
This case study centralizes on working at a bike-sharing company called Cylclistic, to help answer marketing strategies with the marketing team. Cyclistic is located in Chicago that features more than 5,800 bicycles and 600 docking stations with the use of their application. The bikes that are provided includes casual, electric and docked bikes. The company allows people to use their bikes as casual and membership riders. The company was able to gather monthly data and store it through Amazon Web Services in this link [here](https://divvy-tripdata.s3.amazonaws.com/index.html).

### Marketing Team Questions/Business Task
The marketing team want to formulate a business task that answers these 3 questions:
1. How do annual members and causual riders use Cyyclistic bikes differentl?
2. Would casual riders buy Cyclistic annual memberships?
3. How can Cyclistic use digital media to influence casual riders to become members?

With the questions in mind, a business taks formulated to answer this problem: How do we increase the number of annual memberships from the casual riders?

### Data Analysis
After gathering and cleaning 2022 data (process of data cleaning can be found [here](https://github.com/jo3kim/Capstone-Project/blob/main/Cleaning2Ways_README.md), charts were then created to give us these results:

* Member vs Casual Rides Per Month
![Month Count](https://user-images.githubusercontent.com/123437423/221058561-b3af0c42-662e-4ea8-a82f-bc9dacf005ab.png)


* Member vs Casual Rides Per Day
![Daily Count](https://user-images.githubusercontent.com/123437423/221058598-819a0b1e-f54e-4399-85a9-a21af74b1f2e.png)


* Average Ride Time in Minutes
![AVG MIN](https://user-images.githubusercontent.com/123437423/221058619-066aecfe-af73-4c02-90ab-2ff0460d8ec1.png)


* Which Bike is the Most Popular

![Bike Type](https://user-images.githubusercontent.com/123437423/221058643-b26c1a60-2737-4a81-aff6-221abece7dce.png)


* Top 30 Start and End Docking Stations


![Top 30 Start](https://user-images.githubusercontent.com/123437423/221061537-5cd3725d-c430-43f7-b655-3bcb95181072.png)


![Top 30 End](https://user-images.githubusercontent.com/123437423/221061559-473db355-9796-4140-a3f3-21b33286ccd5.png)


### Key Findings
1. The busiest months for users were May through September.
2. Weekend dates showed more usage with classic bikes being the hottest item. The electric would come next, and docked items were not at all used by members.
3. Top 30 stations used to ride and return the bike rides are shown. There are significant number differences between the top 2 stations in both start and end stations that range in 30,000+ usages. 

### Recommendations
1. Promote more advertisements and discounts/promotions for memberships leading up to and in the top 4 months of casual riders. These months are May through August. Technically, when the months become warmer in Chicago, the usage goes up.
2. Increase advertisements and discounts/promotions on the weekend dates rather than the weekday dates.
3. Target marketing advertisements and posters near the top 30 start and end stations.

### Limitations
1. Data provided does not provide how much the bikes cost to use or any plans for membership fees. This could help differentiate if it is more cost effective to become a member. 
2. No age or gender data was provided to determine which group to target or focus more on.
3. No basic income data or reference data was provided to see how pricing can change membership increase or continual usage.

### References/Software
Software Used:
* MySQL (18)
* Excel/Google Sheets
* Tableau (https://public.tableau.com/views/CapstoneProject_16770245020430/MonthCount?:language=en-US&:display_count=n&:origin=viz_share_link)
* Data provided by AWS database

