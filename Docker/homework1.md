(base) hnineikhaing@Hnins-MacBook-Air ~ % docker run -it --entrypoint bash python:3.12.8

Unable to find image 'python:3.12.8' locally
3.12.8: Pulling from library/python
86f9cc2995ad: Download complete 
c80762877ac5: Download complete 
Digest: sha256:2e726959b8df5cd9fd95a4cbd6dcd23d8a89e750e9c2c5dc077ba56365c6a925
Status: Downloaded newer image for python:3.12.8
root@0e12310a4295:/# pip --version
pip 24.3.1 from /usr/local/lib/python3.12/site-packages/pip (python 3.12)
root@0e12310a4295:/# 

2.Hostname: The hostname for connecting to the Postgres database from another container, like pgadmin, is the name of the Postgres service, which is db.
Port: Internally, Postgres is listening on port 5432, but it is mapped to port 5433 on the host machine. When connecting from pgadmin to the Postgres service (within the Docker network), you should use the internal port, which is 5432.
Thus, the hostname and port for pgadmin to connect to the Postgres database would be:

db:5432

So, the correct answer is db:5432.

SELECT 
    SUM(CASE WHEN trip_distance <= 1 THEN 1 ELSE 0 END) AS "Up to 1 mile",
    SUM(CASE WHEN trip_distance > 1 AND trip_distance <= 3 THEN 1 ELSE 0 END) AS "Between 1 and 3 miles",
    SUM(CASE WHEN trip_distance > 3 AND trip_distance <= 7 THEN 1 ELSE 0 END) AS "Between 3 and 7 miles",
    SUM(CASE WHEN trip_distance > 7 AND trip_distance <= 10 THEN 1 ELSE 0 END) AS "Between 7 and 10 miles",
    SUM(CASE WHEN trip_distance > 10 THEN 1 ELSE 0 END) AS "Over 10 miles"
FROM green_tripdata
WHERE lpep_pickup_datetime >= '2019-10-01'
AND lpep_dropoff_datetime < '2019-11-01';

answer - 104802	198924	109603	27678	35189

SELECT lpep_pickup_datetime, trip_distance
FROM green_tripdata
ORDER BY trip_distance DESC
LIMIT 1;

answer - "2019-10-31 23:23:41"	515.89

SELECT t."Zone", SUM(g.total_amount) as total_amount
FROM green_tripdata AS g
LEFT JOIN taxi_zone_lookup AS t
ON g."PULocationID" = t."LocationID"
WHERE DATE(g.lpep_pickup_datetime) = '2019-10-18'
GROUP BY t."Zone"
HAVING SUM(g.total_amount) > 13000;

answer - "East Harlem North"	18686.68000000009
"East Harlem South"	16797.260000000068
"Morningside Heights"	13029.790000000034

SELECT g."DOLocationID", t2."Zone" AS dropoff_zone, MAX(g.tip_amount) AS max_tip
FROM green_tripdata AS g
LEFT JOIN taxi_zone_lookup AS t1 ON g."PULocationID" = t1."LocationID"
LEFT JOIN taxi_zone_lookup AS t2 ON g."DOLocationID" = t2."LocationID"
WHERE DATE(g.lpep_pickup_datetime) BETWEEN '2019-10-01' AND '2019-10-31'
  AND t1."Zone" = 'East Harlem North'
GROUP BY g."DOLocationID", t2."Zone"
ORDER BY max_tip DESC
LIMIT 1;

132	"JFK Airport"	87.3