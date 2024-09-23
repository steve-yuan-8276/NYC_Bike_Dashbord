# NYC Bike Data Insight Report

**Dashboard URL**: [NYC Bike Dashboard](https://public.tableau.com/app/profile/feng.yuan8276/viz/NYC_Bike_Story/NYC_Citys_Bike_Analysis_Story?publish=yes)

## Key Insights:

1. **How many trips have been recorded in total during the chosen period?**

The sample dataset includes a total of **3,247,268** rides from 2022 to 2023.

2. **By what percentage has total ridership grown?**

Based on the sample datasets, the total ridership increased by **17.7%** between 2022 and 2023.

3. **How have the proportions of short-term customers and annual subscribers changed?**

The proportion of members grew from **78% in 2022** to **81.2% in 2023**, reflecting a **4.1% relative increase**.

**Note:** There are two methods to calculate growth rates: **Absolute Growth Rate** and **Relative Growth Rate**. Here, the **Relative Growth Rate** is chosen for the following reasons:

- **Absolute Growth Rate Formula:**
        
        
        Absolute Growth Rate = 2023 Membership Proportion - 2022 Membership Proportion
        

Based on this dataset, the membership proportion increased from **78%** in 2022 to **81.2%** in 2023. Thus, the proportion of **member users** increased by an absolute value of **3.2%** from 2022 to 2023.

- **Relative Growth Rate Formula:**
        
        
        Relative Growth Rate = (2023 Membership Proportion - 2022 Membership Proportion) / 2022 Membership Proportion * 100%
        

Using this formula, the **relative growth rate** of member users compared to 2022 is **4.1%**.

In business analysis, the **relative growth rate** is more commonly used because it better reflects proportional changes and growth speed, especially when comparing entities of different scales. Therefore, the **relative growth rate** is used in this analysis.

4. **What are the peak hours when bikes are used during the summer months or winter months?**

Based on the data, the **peak hours in summer** are from **5 PM to 7 PM**, while in **winter**, the peak hours shift to **4 PM to 6 PM**.

5. **How does the average trip duration change by the type of user?**

Casual users have an average ride duration of **29.9 minutes**, compared to members, whose average ride duration is **13.8 minutes**.

6. **What is the average distance in miles for a bike trip?**

Casual users ride an average distance of **1 mile**, while members average **0.9 miles**, indicating a small difference in ride distance.

**Note on Calculating Ride Distance:**

The dataset does not include the ride distance directly, but it does provide the coordinates (latitude and longitude) for both the start and end points. To estimate the straight-line distance between the two points (also known as the "great circle distance"), the **Haversine formula** was used.

**Haversine Formula:**
    
    a = sin²(Δφ/2) + cos(φ₁) * cos(φ₂) * sin²(Δλ/2)
    c = 2 * atan2(√a, √(1−a))
    d = R * c
    

Where:

    - φ₁ and φ₂ are the latitudes of the two points (in radians).
    - Δφ is the difference between the latitudes.
    - Δλ is the difference between the longitudes.
    - R is the Earth's radius (mean radius = 6,371 km or 3,963 miles).
    - d is the distance between the two points.

Since Tableau does not have a built-in Haversine function, a calculated field was created manually:
    
    
    // Define the Earth's radius
    3963 *
    // Haversine formula
    ACOS(SIN(RADIANS([Start Lat])) * SIN(RADIANS([End Lat])) + 
         COS(RADIANS([Start Lat])) * COS(RADIANS([End Lat])) * 
         COS(RADIANS([End Lng]) - RADIANS([Start Lng])))
    

7. **Which bikes (by ID) are most likely due for repair or inspection in the timespan?**

The bike with the longest cumulative usage time is identified as the one most in need of maintenance. Based on the sample dataset, the bike with ID **4517EBE7E10F07F2** has a total riding time of **425,003 minutes**, which is the longest in the sample data. It is reasonable to estimate that this bike is most in need of inspection or repair.

8. **Today, what are the top 10 stations in the city for starting a journey? Based on data, why do you hypothesize these are the top locations?**

**Top 10 Start Stations:**

    - W 21 St & 6 Ave
    - West St & Chambers St
    - Broadway & W 58 St
    - 1 Ave & E 68 St
    - 6 Ave & W 33 St
    - Broadway & W 25 St
    - University Pl & E 14 St
    - W 31 St & 7 Ave
    - 11 Ave & W 41 St
    - Broadway & E 14 St

**Methodology:**

Based on the sample dataset, we used the following steps to identify the busiest start stations:

1. **Calculate the Number of Rides per Station:**

        - Utilized a **Fixed Level of Detail (LOD)** calculation to compute the total number of rides starting from each station.
2. **Create a Bar Chart:**

        - Constructed a bar chart sorted by the number of rides in descending order to determine the Top 10 Start Stations.

In the visualization (Part 2: Start Station), parameter actions were implemented to enable highlighting. When a user selects a station, the corresponding point on the map below is highlighted. Hovering over the point displays the station name, latitude and longitude, number of rides, and its ranking.

**Note on Station Coordinates:**

The dataset does not provide the latitude and longitude coordinates for each station directly. To estimate these coordinates, a **Level of Detail (LOD) expression** was used in Tableau:

    Latitude: { FIXED [Start Station Name] : AVG([Start Lat]) }

    Longitude: { FIXED [Start Station Name] : AVG([Start Lng]) }
    
9. **What are the bottom 10 stations in the city for starting a journey? Based on data, why?**

**Bottom 10 Start Stations:**

    - E 191 St & Bathgate Ave
    - Church Ave & E 45 St
    - Boerum Pl & Pacific St
    - Baldwin at Montgomery
    - Bailey Ave & W 234 St
    - 92 St & 23 Ave
    - 74 St & Woodside Ave
    - 46 Rd & 11 St
    - 12 St & Sinatra Dr N
    - 11 St & Washington St

**Methodology:**

The calculation method is similar to that used for the Top 10 Start Stations, but we sorted the stations by the number of rides in **ascending order** to identify the Bottom 10 Start Stations with the least usage.

In the visualization (Part 2: Start Station), parameter actions were used to enable highlighting, allowing users to interactively explore the data as described in question 8.

10. **Today, what are the top 10 stations in the city for ending a journey? Based on data, why?**

**Top 10 End Stations:**

    - W 21 St & 6 Ave
    - West St & Chambers St
    - Broadway & W 58 St
    - 1 Ave & E 68 St
    - University Pl & E 14 St
    - 6 Ave & W 33 St
    - Broadway & W 25 St
    - W 31 St & 7 Ave
    - 11 Ave & W 41 St
    - Broadway & E 14 St

**Methodology:**

The calculation method is similar to questions 8 and 9. In the visualization (Part 3: End Station), parameter actions were implemented to enable highlighting. When a user selects a station, the corresponding point on the map below is highlighted. Hovering over the point displays the station name, latitude and longitude, number of rides, and its ranking.

11. **Today, what are the bottom 10 stations in the city for ending a journey? Based on data, why?**

**Bottom 10 End Stations:**

    - Hoboken Terminal - River St & Hudson Pl
    - E 6 St & 2 Ave
    - Ditmars Blvd & 31 Dr
    - Corona Ave & 92 St
    - Caton Ave & St. Pauls Pl
    - Bloomfield St & 15 St
    - Baldwin at Montgomery
    - Adams St & 12 St
    - Adams St & 11 St
    - 11 St & 44 Rd

**Methodology:**

The calculation method is the same as in previous questions. In the visualization (Part 3: End Station), parameter actions were used to enable interactive highlighting, as described in question 10.

* * *

### Additional Notes:

#### About the Data Source

The data is sourced from Citi Bike: [Citi Bike System Data](https://citibikenyc.com/system-data).

For this analysis, bike usage data from **January 1, 2022** to **December 31, 2023** was selected to analyze and showcase trends over this period.

The dataset includes the following columns:

- Ride ID
- Rideable Type
- Start Time (Started at)
- End Time (Ended at)
- Start Station Name
- Start Station ID
- End Station Name
- End Station ID
- Start Latitude
- Start Longitude
- End Latitude
- End Longitude
- Member or Casual Rider

#### ETL Process

This dataset consists of 24 months of usage data, with a total of **76 files**, each containing approximately **1 million rows**, resulting in an estimated **75 to 80 million rows** of data overall. To avoid performance issues during data analysis and visualization--especially considering local machine limitations--a **5% random sampling** of the monthly data was taken for analysis and visualization. The final sample size was around **3.25 million rows**.

The data merging and sampling processes were handled using Python. For more details, please refer to the accompanying Jupyter Notebook: [Jupyter Notebook](https://github.com/steve-yuan-8276/NYC_Bike_Dashbord/blob/main/bike_etl.ipynb).