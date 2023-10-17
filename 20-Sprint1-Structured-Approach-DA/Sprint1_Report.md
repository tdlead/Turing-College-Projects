

## Foreword
This report outlines the methodology I employed to analyze data using the six steps of data analysis. 
Specifically, the data being analyzed is the steps data from my Apple Watch for the period of August 2022 to February 2023.

## <Phase 1> Ask 

In this phase, my main objective was to clearly define the problem at hand and formulate questions that I aimed to answer throughout the analysis.

**Define the problem:** 
As a proponent of healthy lifestyle,
I'm willing to know if I meet my daily activity goal as 10000 steps
And identify patterns and deviations in my physical activity,
So that I can reach my healthy goal.

**Questions I would like to answer:** 
-   What is my average daily step count since I started tracking it? Have I been able to meet my goal of 10,000 steps per day? If not, what is the deviation from the goal and how long will it take to achieve it?
-   How can I increase my daily step count? Can I identify days with maximum step count and determine what activities I was doing on those days to replicate them?
-   Do I tend to be more active on weekends or weekdays? Are there specific days of the week when I am less active, indicating any patterns? How can I incorporate more physical activity into those days? (Timeframe: February 2023)
-   During which hours of the day am I more active or less active? How can I make better use of my time to improve my physical activity during the day?


## <Phase 2> Prepare

During this phase, resources were allocated to identify the required data for analysis and the potential data sources

**Identify data needed:**
- Date and time
- Steps count

**Detect datasources:**
Primary datasource of daily activity : Iphone's Health data 
The datasource that could provide me data about count of steps is Apple Watch (device automatically tracks steps per/day) and is also connected to Apple Health application. From there, I was able to export data. 

***Issues that I encountered:***
Iphone's Health data is exported in xml format in one single format. It requires to be processed, to transform xml in csv format, I used available site https://www.ericwolter.com/projects/apple-health-export/ (it transforms xml data in separate by topic csv files). I downloaded only file related to steps count: HKQuantityTypeIdentifierStepCount (raw data).

**Organize and Secure data:**
Data will be organized on my Google Drive, in Turring College/ Sprint 1/ StepsAnalisys with specific  defined structure :
- raw
- processed
- analysised
- final

## <Phase 3> Process

During this phase, data cleaning and transformation techniques were applied to prepare the data for analysis. This involved making a copy of the raw data and creating a new file with cleaned and transformed data. Various steps were taken to clean and transform the data, including renaming fields, deleting duplicates and irrelevant records, renaming column headers, and extracting date, hour, weekday, and month information from the data. Incomplete months were also deleted to ensure more relevant data for analysis.

**Transformation techniques I applied:**
-   A copy of the raw data was created and transformed to a clean, usable format.
-   Fields were renamed to be more intuitive (e.g., "value" -> "step count").
-   Duplicate data was deleted, specifically records from the Apple iPhone that also calculates steps, creating redundant data. Only data from the Apple Watch was used for this analysis.
-   Records not relevant to the analysis were deleted, specifically columns like "type," "sourcename," "sourceversion," "device," "unit," "creationdate," and "enddate." These columns were redundant and didn't provide any value to this specific analysis.
-   The column header was renamed to a more intuitive name, specifically "value" -> "step count."
-   Dates were extracted from string format using a Google Sheets function: `=SPLIT(B2," ")`
-   The date column was formatted to be more easily readable.
-   Hours were extracted and adjusted by adding +01 hour to the basic value to adjust to Amsterdam time: `=If(HOUR(E2)=23,0,HOUR(E2)+1)`
-   A column for the weekday was extracted and created using the function `=TEXT(D2,"ddd")`
-   A column for the month was extracted and created using the function `=TEXT(D2,"MMM")`
-   Incomplete months were deleted to ensure only relevant data was included.

## <Phase 4> Analyze

During the analysis phase, a variety of activities were performed to gain insights into the individual's physical activity levels. This included:

1.  Aggregating the available data to gain an overall view of the individual's daily step counts.
2.  Creating pivot tables and charts to examine the data in different ways and identify trends or patterns.
3.  Analyzing the data on a monthly basis to determine whether there were any seasonal variations in physical activity levels.
4.  Analyzing the data on a weekday basis to identify any differences in step counts between workdays and weekends.
5.  Analyzing the data on an hourly basis to determine whether there were any specific times of day when the individual was more or less active.

By performing these activities, it was possible to detect specific behavior patterns that may be hindering the individual's physical activity levels. This information can be used to develop solutions to improve overall physical activity and lead to a healthier lifestyle.
  
## <Phase 5> Share

During the share phase, a presentation was created to summarize the main takeaways and recommendations from the analysis phase. The purpose of this presentation was to communicate the insights gained and provide recommendations for improving physical activity levels.

The presentation included the following:

1.  Overview of the individual's average daily step counts over a period of 7 months and comparison to the recommended goal of 10000 steps per day.
2.  Analysis of the daily step counts on a weekday and weekend basis, identifying the most and least active days and recommendations for increasing activity levels.
3.  Examination of the daily step counts on an hourly basis, identifying specific times of day when the individual was more or less active and recommendations for increasing activity during the day.
4.  Recommendations for increasing physical activity levels, including suggestions for incorporating more walking into daily routines, such as walking to the supermarket or hiking, and increasing activity during lunch breaks and evenings on weekends.

By sharing the main takeaways and recommendations through a presentation, it was possible to communicate the insights gained from the analysis phase and provide actionable steps for improving physical activity levels.

## <Phase 6> Act

In the Act phase, I will actively work on implementing the recommendations that were made based on my analysis. This will involve adjusting my daily habits and routines to increase my daily step count and reach my activity goals.


## Left questions, problems:
- Year is not complete, data colection starts from 4th July, We are missing complete March (21 March 23 ), April, May, June.
- To see patterns in month and seasonality is good to have more years as base for analysis
- We don't know the reasons why certain day has so little steps done:
  - it can be sickness (bed regime)
  - The phone or watch was forgotten at home
  - Weather (does weather effects the steps?)
- What are the factors that influences spikes in numbers of days? Track also events, trips, sport activities per day.
