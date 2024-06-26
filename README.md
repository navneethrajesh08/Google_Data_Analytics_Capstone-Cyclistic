# Google_Data_Analytics_Capstone-Cyclistic
This was done as part of the google certification.

#### A Case Study

## Stakeholders:

- **Lily Moreno:** Director of marketing and my manager.
- **Cyclistic marketing analytics team:** A team of data analysts who are responsible for collecting, analyzing, and reporting data that helps guide Cyclistic marketing strategy. You joined this team six months ago and have been busy learning about Cyclistic's mission and business goals — as well as how you, as a junior data analyst, can help Cyclistic achieve them.
- **Cyclistic executive team:** The notoriously detail-oriented executive team will decide whether to approve the recommended marketing program.

## The Data Analyst Process:

## 1.ASK:

#### Expected Deliverable:

A clear statement of the business task.

#### Guiding Question:

- What is the problem you are trying to solve?
  - I am trying to find, how casual riders differ from annual riders. My objective is to maximize the number of annual riders. I will need to identify, trends and patterns between the two rider types.


- How can your insights drive business decisions?
  - By learning how annual members and casual riders use Cyclistic bikes differently, Cyclistic can then developer marketing strategies around the habits of casual riders and demonstrate different ways in which casula riders can benefit from an annual pass.

### Deliverable: Business Task:

The business task for this study will be to identify the differences between the trends and patterns of casual and Cyclistic members and then present these findings to the executive team.

## 2.Prepare:

_Note: The datasets have a different name because Cyclistic is a fictional company. For the purposes of this case study, the datasets are appropriate and will enable you to answer the business questions. The data has been made available by Motivate International Inc. under this_ [license](https://www.divvybikes.com/data-license-agreement)_._

#### Expected Deliverable:

A description of all data sources used.

#### Guiding questions:

- Where is your data located?
  - The data was in a database provided by Motivate International. For the purposes of this study the data is Internal and primary data.
- How is the data organized?
  - The data is structured and in a wide format.
- Are there issues with bias or credibility in this data? Does your data ROCCC?
  - Using the ROCCC test:
    - The data is Reliable. It has missing cells for the start\_station\_name, end\_station\_name, end\_lat and end\_lng, but I may not need to use these columns in my final analysis. There are no biases in the database or spelling mistakes. But I will need to clean the data for mistakes, etc.
    - The data is Original: it is first party data collected from the Divvy data logging system.
    - The data is Comprehensive: the data is comprehensive as it contains all that I need to make my analysis.
    - The data is Current: It contains data from the last 12 months i.e., March 2023 to May 2022.
    - The data is Cited: The data source is first party. 
- How are you addressing licensing, privacy, security, and accessibility?
  - The data does not contain any private information i.e., credit card information which could connect the rides to the customer. Also, licensing is covered in https://ride.divvybikes.com/data-license-agreement.
- How did you verify the data's integrity?
  - By checking data schema, and filtering for blanks via BigQuery.
- How does it help you answer your question?
- Are there any problems with the data?

Attributes of the data and transformations done:

1. Ride\_id: No nulls found.
2. Rideable\_type: No nulls found.
3. Started\_at: No nulls found.
4. Ended\_at: No nulls found.
5. Start\_station\_name: Nulls found.
6. Start\_station\_id: Nulls found.
7. End\_station\_name: Nulls found.
8. End\_station\_id: Nulls found.
9. Start\_lat: No nulls found.
10. Start\_lng: No Nulls found.
11. End\_lat: Nulls found.
12. End\_lng: Nulls found.
13. Member\_casual: No nulls found.

- Since I have no direction on what to do with attributes with nulls, I will discard them. I do not need Ride\_id as it is autogenerated string when trips are begun and will not be meaningful in my analysis.

- Start\_lat and start\_lng may be discarded as it will not be meaningful.

- I can use Rideable\_type to compare and contrast the different member types.

- I can use Started\_at and Ended\_at to find out travel time.

- I need to use Member\_casual as it contains what type of membership the customer had.

So, the attributes I'll be using are rideable\_type, started\_at, ended\_at and member\_casual. Will need to rename member\_casual to member\_type.

### Deliverable: Data Sources Used:

The data source used is public [data](https://divvy-tripdata.s3.amazonaws.com/index.html) provided by Motivate International Inc under this[license](https://www.divvybikes.com/data-license-agreement)_._ The files from the database used are:

1. 202204-divvy-tripdata.zip
2. 202205-divvy-tripdata.zip
3. 202206-divvy-tripdata.zip
4. 202207-divvy-tripdata.zip
5. 202208-divvy-tripdata.zip
6. 202209-divvy-tripdata.zip
7. 202210-divvy-tripdata.zip
8. 202211-divvy-tripdata.zip
9. 202212-divvy-tripdata.zip
10. 202301-divvy-tripdata.zip
11. 202302-divvy-tripdata.zip
12. 202303-divvy-tripdata.zip

I loaded the files into BigQuery and combined all the tables into one. Using the aggregated table, I was able to select and find the attributes with and without blank cells. I chose the attributes that were relevant to my analyses which were also complete. The resulting data is anonymized i.e., there is no personal information (credit card, personal details, etc) which also means there will be no personal bias. But, the data will have to be further cleaned and transformed to be made reliable and comprehensive.

## 3.Process:

#### Expected Deliverable:

Documentation of any cleaning or manipulation of data.

#### Guiding Questions:

- What tools are you choosing and why?
  - For cleaning I'll be using Excel. Excel is  easy to use and offers more flexible in performing calculations.
- Have you ensured your data's integrity?
  - Data replication: I've had no issues with data replication. The number of rows of all csv files have been the same.
  - Data Transfer: I was having issues with exporting files to bigquery due to the data type of a column being set to FLOAT instead of TIME. I re-did my data cleaning procedures and was able to export successfully.
  - Data Manipulation: Had to clean data while calculating ride\_length. Turns out, some of the started\_at and ended\_at times were switched, i.e. trips had end times before their start times. This would give an invalid result during calculations. Most of my cleaning efforts were aimed at fixing this.
- How can you verify that your data is clean and ready to analyze?
  - I exported all files to big query and combined them into one table. I selected the columns that I will be using for analysis plus the newly created ones and created a new table for my analysis.

### Deliverables:

#### Steps followed in excel:

1. Unzip the files.
2. Opened each file and saved as .xlsx since I chose to work with excel.
3. Then opened each file and added columns "ride\_length" and "day\_of\_week".
4. For ride\_length column, I found difference between ended\_at and started\_at to give length of ride. I used format cells to change format to h:mm:ss.
5. For day\_of\_week column, I used the weekday function to get the day of the week the ride started on.
6. I then sorted or filtered ride\_length column to look for "######". This indicates an issue in the started\_at and ended\_at fields where the start time is later than the end time.
7. To change more than one row, I sorted ride\_length in ascending order, then switched the started\_at and ended\_at with each other.
8. Repeated steps 6 to 7 for the rest of the files.

Note: The ride\_length field became a means to find dirty data in the started\_at and ended\_at fileds. But the ride\_length fields has a limitation where the data format of time (hh:mm:ss) can only accurately represent ride lengths less than 24 hours.
 For e.g., ride\_id: 7D4CB0DD5137CA9A rented a bike for nearly the whole month of October in 2022. But the ride\_length field only shows a time of 17:47:15.

For this reason, I uploaded the data to BigQuery and calculated ride length using the TIMESTAMP\_DIFF to output the time difference in minutes. Using the above example, its ride length is now accurately shown to be 41387 minutes or 28.7 days.

Because the collated table would be too large for a spreadsheet tool like excel or sheets, naturally SQL or R became the options for handling this large data set. I chose SQL and the following steps detail the process I used to import, create, and transform the collated table in BigQuery.

#### Full Steps followed in BigQuery:

1. Imported all data sets to BigQuery then merged all datasets into one table. 

![image](https://user-images.githubusercontent.com/43974678/236041136-eccee2c4-8a89-4264-b6aa-5f71056f9a9e.png)
        
2. Created new filtered table with only the fields that were relevant to my analysis, renamed member\_casual to member\_type, changed day of week to type string and calculated time difference using TIMESTAMP\_DIFF function.

![image](https://user-images.githubusercontent.com/43974678/236041191-0054d60b-e455-4240-8b76-48ec9f64266a.png)

3. Updated the new table day of the week numbers to actual days of the week.

![image](https://user-images.githubusercontent.com/43974678/236041467-1faa229d-739e-4a50-b78f-77b05cf3a0e5.png)

4. Removed 18 rows of null values found in the table.

![image](https://user-images.githubusercontent.com/43974678/236041482-907d7b00-9906-414d-9c08-d66466922739.png)

5. To extract Month from the table I used

![image](https://user-images.githubusercontent.com/43974678/236041511-ed7e505f-5518-43fa-8c60-dc9011fdf3f5.png)

6. Because the extracted value is in INT64 it will have to be casted as string before I can update the number values to literal months. This will involve creating a new table with the casted data type and then replacing the original table with the casted table.

![image](https://user-images.githubusercontent.com/43974678/236041536-c7ec2cac-8363-45fd-b4a4-7a330a89369c.png)

7. Copying back the table with the modified string back onto the original YearData table.

![image](https://user-images.githubusercontent.com/43974678/236041563-b2556fff-4623-47d1-a5df-0d530a763d58.png)

8. Changing numeric month to literal month.

![image](https://github.com/sf0912/Cyclistic-CaseStudy/assets/43974678/cf4511ec-1acf-4222-8280-b816fd3d9688)

9. To represent yearly quarters, I created a new column of type string and I used this set of queries to update the new column.

![image](https://github.com/sf0912/Cyclistic-CaseStudy/assets/43974678/6ed819f8-34b9-4f74-8b9d-44e82796b1ec)

10. At this stage, the number of rows that I had were 5,803,720. Of these, 633,307 entries (10.7%) were rides that were 3 minutes or less. I decided to filter these rides out as customers could have made mistakes or they could have canceled the ride.

11. The final table has 5,170,413 rows of data.

Schema of the final columns used:

| Column name | Description |
| --- | --- |
| ride\_id | To reference ride\_id with source data |
| member\_type | Type of customer membership |
| rideable\_type | Type of bike used |
| started\_at | Date and time of trip start |
| ended\_at | Date and time of trip end |
| day\_of\_week | Day trip started |
| ride\_length | Excel calculations for ride length |
| ride\_length\_mins | BigQuery calculations for ride length |
| month | Month of the year, extracted from started\_at |
| year\_quarter | Year and quarter based on the month |

## 4.Analyze:

### Expected Deliverable:

A summary of the analysis

#### Guiding questions:

- How should you organize your data to perform analysis on it?
  - I'll be importing my data into BigQuery and compiling all the datasets into one table. Ill be then using aggregate functions to create summary tables, or filter the table for a column to answer specific questions.
- Has your data been properly formatted?
  - Yes, I went through each table and made sure that the data type is of the proper formats and that the table dimensions for all the tables are the same.
- What surprises did you discover in the data?
  - That casual customers may have longer ride lengths, maybe up to a month. We may want to track such behaviours in real time to maybe persuade them to switch over to annual memberships.
- What trends or relationships did you find in the data?
  - Annual members are more consistent with their usage during day hours and during the week. They are also more likely to use the service during colder months, where casual members usage shows a sharp decline.
- How will these insights help answer your business questions?
  - These insights highlight how both member types use the service differently. Hence it helps me create insights for the business question.

### Deliverable: Analysis:

For the purposes of the analysis, I will be referring to our annual customers as members and our casual customers as casuals.

First, I wanted to find the distribution of rides between both member types. I used the following query,

![image](https://user-images.githubusercontent.com/43974678/236041800-8ee96e05-64ec-47c6-9385-27ec759797ec.png)

And got the following result,

| **Row** | **member\_type** | **percentage\_of\_rides** | **count\_of\_rides** |
| --- | --- | --- | --- |
| **1** | casual | 41.86 | 2164550 |
| **2** | member | 58.14 | 3005863 |

We can see that for 2022 Q2-2023 Q1, annual members account for a bit more than half of the total trips. Annual members have 16.28% greater usage than casual members.

Next, I wanted to look at average ride\_length and frequency of rides but at a per quarter level. To do that I first created table showing then number of rides between member types for each quarter.

![image](https://user-images.githubusercontent.com/43974678/236041963-514c504b-3c97-4cb9-8a48-1100cd6eccb8.png)

Then I created a second table with average ride length between member types for each quarter.

![image](https://user-images.githubusercontent.com/43974678/236041988-07fdcc9a-ae0a-440a-8338-d0250b269dd0.png)

Finally, I created the summary table showing number of rides and average ride length.

![image](https://user-images.githubusercontent.com/43974678/236042015-12b2eed1-815c-4be2-b1e1-dbff578df57c.png)

| **year\_quarter** | **member\_type** | **avg\_ride\_length\_mins** | **count\_of\_rides** | **percentage\_num\_rides** | **total\_rides\_per\_quarter** |
| --- | --- | --- | --- | --- | --- |
| **22 Q2** | casual | 33.08 | 730187 | 45.45 | 1606634 |
| **22 Q2** | member | 14.71 | 876447 | 54.55 | 1606634 |
| **22 Q3** | casual | 30.95 | 988103 | 47.16 | 2095416 |
| **22 Q3** | member | 14.82 | 1107313 | 52.84 | 2095416 |
| **22 Q4** | casual | 26.9 | 319351 | 34.15 | 935093 |
| **22 Q4** | member | 13.06 | 615742 | 65.85 | 935093 |
| **23 Q1** | casual | 25.28 | 126909 | 23.8 | 533270 |
| **23 Q1** | member | 12.32 | 406361 | 76.2 | 533270 |

This table gives us more insights into how both member types use the service. By looking at the avg\_ride\_length\_mins column, during Q2 and Q3, we find that casual member on average have a ride length of more than half of the ride length of annual members. This figure decreases to approximately half of that for annual members.

When it comes to frequency of rides taken, members are our more frequent riders throughout the year with them consistently representing more than half of the share of rides. Casuals make up around 46% of the number or rides in Q2 and Q3 (Spring and Summer), with their usage lowering during Q4 and 2023 Q1 (Fall and Winter).

We see this trend even with the total number of rides, Usage peaks at summer and bottoms out during the winter.

Created a table showing quarterly frequency of rides for each day in the week.

![image](https://user-images.githubusercontent.com/43974678/236042056-5f732d1d-8591-4b20-a8b3-50c7bbb4d39c.png)

I'll use visuals for large tables as they are easier to analyze and explain.

![image](https://user-images.githubusercontent.com/43974678/236042087-f5bd540d-e93b-4f91-91de-b27985208e02.png)

I chose to represent the above query as the bar graph above for brevity's sake. The orange are members and blue are casuals, and they follow different trends as the week goes on throughout the year. Casuals peak during the weekend (Saturday, Sunday) and trend downwards during the middle of the week (Tuesday, Wednesday). Members peak during the middle of the week (Wednesday and Thursday) and they don't have as steep a trend downwards like the casuals.

I also wanted to look at the start time between both members, so I created another table from my main table.

![image](https://user-images.githubusercontent.com/43974678/236042148-6efd1f96-e91a-4ca7-ab49-32ba9a147e86.png)

And then, I selected the year\_quarterly, month, member\_type and start\_time fields from the new table.

![image](https://user-images.githubusercontent.com/43974678/236042174-d8cf1ca3-9226-4105-b4c6-01710316cd60.png)

![image](https://user-images.githubusercontent.com/43974678/236042293-bf761c9e-09b9-4378-9e68-9dfec3d9bb6b.png)
In all four seasons, annual members use more of the service throughout the day. Casuals usage rises steadily throughout the day to peak at around five in the evening.

I then looked at bike type usage between members for the year. This is the query used on. 
![image](https://user-images.githubusercontent.com/43974678/236042334-523c41ed-49bf-4769-a2e5-d6189b1bd1b6.png)
![image](https://user-images.githubusercontent.com/43974678/236042361-5a3cbc43-524a-4d23-a77d-0b90b516dd6a.png)
There are differences between the ride choices of casuals and members. I see that casuals are the only ones who used the dock bikes for the year 2022-2023. I see that they prefer electric bikes over classic bikes in any given quarter.

Members change their usage of bike types depending on the seasons. In Q2 and Q3 (around spring or summer), members prefer to use classic bikes over electrics and in Q4 and Q1, they prefer to use electric bikes over classic bikes. Members have not used docked bikes in any of the four quarters for the year. The below pie chart does a better job of highlighting the differences between ride types for each member type. 

![image](https://user-images.githubusercontent.com/43974678/236042459-07ef3ca2-fb33-4fdc-b93b-7c97c62befa9.png)

I also wanted to look at the maximum ride length that taken by each member. This represents how long a customer in any member type is willing to use the service for.

This is the SQL query used:

![image](https://user-images.githubusercontent.com/43974678/236042528-e53f2b2c-10ae-4378-9587-030a1b2aa754.png)

| Row | year\_quarter | member\_type | max\_ride\_length\_hrs |
| --- | --- | --- | --- |
| 1 | 22 Q2 | casual | 604.3 |
| 2 | 22 Q2 | member | 25.0 |
| 3 | 22 Q3 | casual | 570.15 |
| 4 | 22 Q3 | member | 172.55 |
| 5 | 22 Q4 | casual | 689.78 |
| 6 | 22 Q4 | member | 25.0 |
| 7 | 23 Q1 | casual | 560.07 |
| 8 | 23 Q1 | member | 26.0 |

Here the max length is represented in hours. The longest ride was for 689.78 hours or around 28 days from a casual member. In all four quarters, casual customers have long rides of over 500 hours. Meanwhile members have only around 25-hour long rides in three of the quarters, but there was one ride in Q3 that was for 172 hours.

## 5.Share:

#### Expected Deliverable:

Supporting visuals and key findings.

#### Guiding questions:

- Were you able to answer the question of how annual members and casual riders use Cyclistic bikes differently?
  - Yes, I was able to answer how members and casuals use the service differently, based on seasonal, time, ride length and day of the week.
- Who is your audience? What is the best way to communicate with them?
  - The key audience here is the executive team, as they will be choosing the right marketing strategy.
- Can data visualization help you share your findings?
  - Data visualization will definitely help me share my findings, It helps put forward insights in an easy to understand format, and provides clarity for the stakeholders.

#### Deliverable: Visualizations and Key Findings:

##### Supporting visualizations:

![image](https://user-images.githubusercontent.com/43974678/236042589-78f50eec-09e8-44ea-bf6c-8710614f7e1c.png)

![image](https://user-images.githubusercontent.com/43974678/236042602-6481a6cb-c51e-438d-90b2-935fdb3f8033.png)

![image](https://user-images.githubusercontent.com/43974678/236042627-1cce85d3-1dda-4059-b935-64822f217305.png)

![image](https://user-images.githubusercontent.com/43974678/236042645-636ba3e3-28c3-4b7d-b09a-94d45a9808bc.png)

![image](https://user-images.githubusercontent.com/43974678/236042664-bb8fbea3-adc8-4299-871d-7e6746fbba65.png)

![image](https://user-images.githubusercontent.com/43974678/236042681-c1d84e6b-7f80-4747-a6dd-b584c968a52f.png)

##### Key Findings:

Both member types differ in how they use the Cyclistic's services:

1. Members take more rides than Casuals in a given time period.

2. Overall ridership drops during the colder seasons like fall and winter (Q4 & Q1)

3. Annual members use the cycles mostly during weekdays. Conversely, Casuals prefer riding during weekends.

4. During a given day, both member types peak at around 7pm. But Members ride more often throughout the rest of the day.

5. On average Casuals riders have longer trips than member riders.

6. Also, Casuals have done some of the longest trips that can last several days or weeks.

7. When it comes to ride preference, Casual riders prefer electric cycles in any given quarter. Member riders are more balanced in their preferences but have not used docked bikes in the last 12 months.

## 6.Act:

#### Guiding questions:

- How should you organize your data to perform analysis on it?
  - I plan to import my data into BigQuery and consolidate all datasets into a single table. From there, I'll use aggregate functions to generate summary tables or apply filters to the table's columns to address specific inquiries.
- Has your data been properly formatted?
  - I've reviewed each table to ensure that the data types are correctly formatted and that the table dimensions  are the same for all tables.
- What surprises did you discover in the data?
  - Casual customers might exhibit longer ride durations, possibly extending up to a month. It could be beneficial to monitor such behaviors in real-time to potentially encourage them to transition to annual memberships.
- What trends or relationships did you find in the data?
  - Member riders are more consistent with their usage during day hours and during the week. They are also more likely to use the service during colder months, where casual members sharply decrease.
- How will these insights help answer your business questions?
  - These insights highlight how both member-types use the service differently. Hence it does allow me to answer the business question.

Deliverable:
Your top three recommendations based on your analysis.

#### Guiding questions:

- What is your final conclusion based on your analysis?
  - Both the member-types use the service in a significantly different way. Based on start time, during a given day, members have successfully integrated using the service in their everyday life, like traveling to and from work, school, etc. Casuals riders seem to mainly use the service for leisure activities or plan to take very long trips.
- How could your team and business apply your insights?
  - They could look at these differences and craft advertisement, pricing changes, pricing tiers to suit the different needs of the casual rides.
- Is there additional data you could use to expand on your findings?
  - I could use the starting and ending stations. I did not use it here because of its incomplete nature. But it would have given me locations based usage data between both member groups.

### Deliverable: Recommendations:

**Top Three Recommendations:**

- Promote/advertise to casual users the advantages of integrating Cyclistic as a long-term transportation solution for daily activities rather than solely for leisure purposes.
- Monitor casual users who consistently renew trips for multiple days and market to them, the cost-saving benefits of switching to an annual plan for extended trips.
- Maybe start an advertisment campaign that explains the benefits of cold cycling:
  - Combatting holiday weight gain.
  - Fun activity to do with family and friends.
  - Sell the 'experience' and create a narrative comparing it to skiing and other winter activities. 
  - Contributing to savings on gas, expenses, and environmental impact.
