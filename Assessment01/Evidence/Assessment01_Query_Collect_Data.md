# Assessment01 - Collect Data

## Query List

---

CREATE_VIEW - collected all weather from 2012-2020

```sql
CREATE VIEW `uhi-project-414216.12005304_assessment.weather_2012_to_2020` AS
SELECT DATE(CAST(year as INT64), CAST(mo as INT64), CAST(da as INT64)) as date, year, mo, da, temp, dewp, slp, visib, wdsp, mxpsd, gust, max, min, prcp, sndp, fog
FROM `bigquery-public-data.noaa_gsod.gsod2012` WHERE stn='725060'
UNION ALL
SELECT DATE(CAST(year as INT64), CAST(mo as INT64), CAST(da as INT64)) as date, year, mo, da, temp, dewp, slp, visib, wdsp, mxpsd, gust, max, min, prcp, sndp, fog
FROM `bigquery-public-data.noaa_gsod.gsod2013` WHERE stn='725060'
UNION ALL
SELECT DATE(CAST(year as INT64), CAST(mo as INT64), CAST(da as INT64)) as date, year, mo, da, temp, dewp, slp, visib, wdsp, mxpsd, gust, max, min, prcp, sndp, fog
FROM `bigquery-public-data.noaa_gsod.gsod2014` WHERE stn='725060' AND wban='14756'
UNION ALL
SELECT DATE(CAST(year as INT64), CAST(mo as INT64), CAST(da as INT64)) as date, year, mo, da, temp, dewp, slp, visib, wdsp, mxpsd, gust, max, min, prcp, sndp, fog
FROM `bigquery-public-data.noaa_gsod.gsod2015` WHERE stn='725060'
UNION ALL
SELECT DATE(CAST(year as INT64), CAST(mo as INT64), CAST(da as INT64)) as date, year, mo, da, temp, dewp, slp, visib, wdsp, mxpsd, gust, max, min, prcp, sndp, fog
FROM `bigquery-public-data.noaa_gsod.gsod2016` WHERE stn='725060'
UNION ALL
SELECT DATE(CAST(year as INT64), CAST(mo as INT64), CAST(da as INT64)) as date, year, mo, da, temp, dewp, slp, visib, wdsp, mxpsd, gust, max, min, prcp, sndp, fog
FROM `bigquery-public-data.noaa_gsod.gsod2017` WHERE stn='725060'
UNION ALL
SELECT DATE(CAST(year as INT64), CAST(mo as INT64), CAST(da as INT64)) as date, year, mo, da, temp, dewp, slp, visib, wdsp, mxpsd, gust, max, min, prcp, sndp, fog
FROM `bigquery-public-data.noaa_gsod.gsod2018` WHERE stn='725060'
UNION ALL
SELECT DATE(CAST(year as INT64), CAST(mo as INT64), CAST(da as INT64)) as date, year, mo, da, temp, dewp, slp, visib, wdsp, mxpsd, gust, max, min, prcp, sndp, fog
FROM `bigquery-public-data.noaa_gsod.gsod2019` WHERE stn='725060'
UNION ALL
SELECT DATE(CAST(year as INT64), CAST(mo as INT64), CAST(da as INT64)) as date, year, mo, da, temp, dewp, slp, visib, wdsp, mxpsd, gust, max, min, prcp, sndp, fog
FROM `bigquery-public-data.noaa_gsod.gsod2020` WHERE stn='725060'
ORDER BY year, mo, da
```

SELECT_ALL - view contents of weather.

```sql
SELECT * FROM `uhi-project-414216.12005304_assessment.weather_2012_to_2020` LIMIT 1000
```

---

COUNT - Check the number of records in weather data

```sql
SELECT COUNT(*) FROM `uhi-project-414216.12005304_assessment.weather_2012_to_2020`
```

CREATE_VIEW - count number of collisions

```sql
CREATE VIEW `uhi-project-414216.12005304_assessment.collision_data_count`
AS SELECT CAST(timestamp as DATE) as collision_date, COUNT(CAST(timestamp as DATE)) AS NUM_COLLISIONS
FROM `bigquery-public-data.new_york_mv_collisions.nypd_mv_collisions`
GROUP BY collision_date;
```

SELECT_ALL - View contents

```sql
SELECT * FROM `uhi-project-414216.12005304_assessment.collision_data_count` LIMIT 1000
```

---

CREATE_VIEW - final - set day of the week, collision_date, NUM_COLLISIONS

```sql
CREATE VIEW `uhi-project-414216.12005304_assessment.collision_data_count_final`
AS SELECT FORMAT_DATE("%u", collision_date) as day, collision_date, NUM_COLLISIONS
FROM `uhi-project-414216.12005304_assessment.collision_data_count`
```

SELECT_ALL - View contents

```sql
SELECT * FROM `uhi-project-414216.12005304_assessment.collision_data_count_final` LIMIT 1000
```

---

CREATE TABLE - Add all required contents to one table.

```sql
CREATE TABLE `uhi-project-414216.12005304_assessment.collated_collision_data`
AS SELECT day, year, mo, da, collision_date, temp, dewp, slp, visib, wdsp, mxpsd, gust, max, min, prcp, sndp, fog, NUM_COLLISIONS
FROM `uhi-project-414216.12005304_assessment.weather_2012_to_2020` as weather,
`uhi-project-414216.12005304_assessment.collision_data_count_final` as complaints
WHERE complaints.collision_date = weather.date
```

SELECT_ALL - View contents

```sql
SELECT * FROM `uhi-project-414216.12005304_assessment.collated_collision_data` LIMIT 1000
```
