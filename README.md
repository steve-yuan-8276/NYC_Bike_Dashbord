# NYC_Bike_Dashboard

**Dashboard URL**: [NYC Bike Dashboard](https://public.tableau.com/app/profile/feng.yuan8276/viz/NYC_Bike_Story/NYC_Citys_Bike_Analysis_Story?publish=yes)

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

This dataset consists of 24 months of usage data, with a total of **76 files**, each containing approximately **1 million rows**, resulting in an estimated **75 to 80 million rows** of data overall. To avoid performance issues during data analysis and visualization, especially considering the local machine's configuration, a **5% random sampling** of the monthly data was taken for analysis and visualization. The final sample size was around **3.2 million rows**.

The data merging and sampling processes were handled using Python. For more details, please refer to the accompanying Jupyter Notebook: [Jupyter Notebook](https://github.com/steve-yuan-8276/NYC_Bike_Dashbord/blob/main/bike_etl.ipynb).

## Special Calculated Fields

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

