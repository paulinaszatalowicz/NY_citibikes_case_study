###According to public data from BigQuery there are 1584 citibike stations in New York.###
###Managing such a project can be quite challenging. Data can help and answer questions in few seconds eg.:###
*How many bikes are available?*
*What is the average number of citibikes available in NY?*
```SQL
SELECT 
    station_id,
    num_bikes_available,
    (SELECT 
        AVG(num_bikes_available)
    FROM 
        `bigquery-public-data.new_york_citibike.citibike_stations`) AS avg_num_bikes_available
FROM 
        `bigquery-public-data.new_york_citibike.citibike_stations`
```

*How many trips start at a certain station?* 
```SQL
SELECT 
    station_id,
    name,
    number_of_rides AS number_of_rides_starting_at_station.   #using alias to recognize new feature easily
FROM     #using subquery to define an exact query
    (
        SELECT 
            start_station_id,
            COUNT(*) number_of_rides
        FROM 
            `bigquery-public-data.new_york_citibike.citibike_trips` #using second table in NY_citibike dataset
        GROUP BY 
            start_station_id

    )
    AS station_num_trips
    INNER JOIN 
    bigquery-public-data.new_york_citibike.citibike_stations ON station_id = start_station_id
    ORDER BY 
        number_of_rides DESC 
 ```
 *How about checking how many subscribers(type of customer) start a trip at a certain station and which station is the most popular to start their trip from?*
 ```SQL
 SELECT 
    station_id,
    name
FROM 
    `bigquery-public-data.new_york_citibike.citibike_stations`
WHERE
    station_id IN 
(
    SELECT 
        start_station_id
    FROM 
        `bigquery-public-data.new_york_citibike.citibike_trips`
    WHERE 
        usertype = 'Subscriber'
 ```
