# NYC_Bike_Dashboard

**Dashboard URL**: [NYC Bike Dashboard](https://public.tableau.com/app/profile/feng.yuan8276/viz/NYC_Bike_Story/NYC_Citys_Bike_Analysis_Story?publish=yes)

## Key Insights:

- The dataset includes a total of **3,247,268** rides from 2022 to 2023.
- Total ridership increased by **17.7%** between 2022 and 2023.
- The proportion of members grew from **78% in 2022** to **81.2% in 2023**, reflecting a **4.1% relative increase**.
- Based on the data, the **peak hours in summer** are from **5 PM to 7 PM**, while in **winter**, the peak hours shift to **4 PM to 6 PM**.
- Casual users have an average ride duration of **29.9 minutes**, compared to members, whose average ride duration is **13.8 minutes**.
- Casual users ride an average distance of **1 mile**, while members average **0.9 miles**, indicating a small difference in ride distance.
- The bike with the longest cumulative usage time is identified as the one most in need of maintenance. The dashboard highlights this bike based on usage data.
- The Top 10 and Bottom 10 start and end stations were analyzed, along with their geographic distribution, to provide additional insights.



## About the Data Source

The data is sourced from Citi Bike: [Citi Bike System Data](https://citibikenyc.com/system-data).

For this analysis, bike usage data from **January 1, 2022** to **December 31, 2023** has been selected to better analyze and showcase the trends over this period.

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

## ETL Process

This dataset consists of 24 months of usage data, with a total of **76 files**, each containing approximately **1 million rows**, resulting in an estimated **75 to 80 million rows** of data overall. To avoid performance issues during data analysis and visualization, especially considering the local machine's configuration, a **5% random sampling** of the monthly data was taken for analysis and visualization. The final sample size was around **3.25 million rows**.

The data merging and sampling processes were handled using Python. For more details, please refer to the accompanying Jupyter Notebook: [Jupyter Notebook](https://github.com/steve-yuan-8276/NYC_Bike_Dashbord/blob/main/bike_etl.ipynb).

## Special Calculated Fields

### Membership Proportion Growth Calculation Method

There are two method：

**Absolute growth rate Formula**:

```
Absolute Growth Rate = 2023 Membership Proportion - 2022 Membership Proportion
```

Based on this dataset, the membership proportion for 2022 was **78%**, and for 2023, it was **81.2%**. Thus, the proportion of **member users** increased by **3.2%** from 2022 to 2023.


**Relative Growth Rate Formula**:

```
Relative Growth Rate = (2023 Membership Proportion - 2022 Membership Proportion) / 2022 Membership Proportion * 100
```

Based on this dataset, the **relative growth rate** of member users compared to 2022 is **4.1%**.

In business analysis, **relative growth rate** is more commonly used because it better reflects the proportional change and growth speed, especially when comparing entities of different scales. Therefore, **relative growth rate** is used in this analysis.

### Station Coordinates

The dataset does not provide the latitude and longitude coordinates for each station. To estimate these coordinates, a **Level of Detail (LOD) expression** was used in Tableau.

- Latitude: `{ FIXED [Start Station Name] : AVG([Start Lat]) }`
- Longitude: `{ FIXED [Start Station Name] : AVG([Start Lng]) }`

### Ride Distance

The dataset does not include the ride distance directly, but it does provide the coordinates (latitude and longitude) for both the start and end points. To estimate the straight-line distance between the two points (also known as the "great circle distance"), the **Haversine formula** was used.

#### Haversine Formula:

```
a = sin²(Δφ/2) + cos(φ₁) * cos(φ₂) * sin²(Δλ/2)

c = 2 * atan2( √a, √(1−a) )

d = R * c
```
Where:
- φ₁ and φ₂ are the latitudes of the two points (in radians).
- Δφ is the difference between the latitudes.
- Δλ is the difference between the longitudes.
- R is the Earth's radius (mean radius = 6,371 km or 3,963 miles).
- d is the distance between the two points.

Since Tableau does not have a built-in Haversine function, a calculated field was created manually:

```tableau
// Define the Earth's radius
3963 *
// Haversine formula
ACOS(SIN(RADIANS([Start Lat])) * SIN(RADIANS([End Lat])) + 
     COS(RADIANS([Start Lat])) * COS(RADIANS([End Lat])) * 
     COS(RADIANS([End Lng]) - RADIANS([Start Lng])))
```

Thanks for your time.

