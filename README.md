# apitest
## SETTING UP!

1. Clone the repo

## Activate Virtual Environment

2. Open Command Prompt and cd to cloned folder, cd into scripts.
3. run activate. - this will activate the virtual environment.

## To install the Requirements:

4. pip freeze > requirements.txt
5. pip install requirements.txt

## To setup server

6. Open PostgreSQL (PG ADMIN), create a database named => locinfo
7. change postgres database password in script.py and serversetup.py files.
8. on command prompt cd back to the root directory (apitest) of virtual env and run "python serversetup.py"
9. In PG ADMIN Query tool, run
```
    CREATE EXTENSION "cube";
    CREATE EXTENSION "earthdistance";
```
## Run the script

10. on command prompt run "python script.py".

## Working

There are 4 API's:

### 1. /post_location :

This API can be used to enter a new entry in the geo_info table of the database.
This API checks if the PINCODE is already present in the database or if there is any nearby latitude+longitude location already available in database. if both conditions are not present then a new entry in made in the database.

### To Run:
```
curl -H "Content-type:text/plain" -X POST http://127.0.0.1:8000/post_location --data-ascii 28.699+77.111+IN/110085+Rohini+Delhi
```
## Output
### When the location is not present in database:
![post_location1](https://i.imgur.com/EnRU4N0.png)

### When the pincode is alreday present in database:
![post_location2](https://i.imgur.com/1PAgocC.png)

### When a nearby location present in database:
![post_location3](https://i.imgur.com/E4CXuXQ.png)


### 2. /get_using_postgres :

This API can be used to all the locations present in the database within 5km radius of the given point. This API Uses postgres's earthdistance for calculating all the points which are present within 5Km radius of given point.

### To Run:
```
curl -H "Content-type:text/plain" -X GET http://127.0.0.1:8000/get_using_postgres --data-ascii 28.610+77.223
```
### Output

![get_using_postgres](https://i.imgur.com/E3myPg9.png)

### 3. /get_using_self :

This API can be used to all the locations present in the database within 5km radius of the given point. This API Uses a self made function named getDistanceFromLatLonInKm() for calculating all the points which are present within 5Km radius of given point.

### To Run:
```
curl -H "Content-type:text/plain" -X GET http://127.0.0.1:8000/get_using_self --data-ascii 28.610+77.223
```
### Output

![get_using_self](https://i.imgur.com/UzMEhGm.png)

### 4. /get_city_name :

This api uses geojson which is used to define shapes of locations. In the provided geojson there are boundaries coordinates provided of different cities, with this API we can check if the given point is present inside the boundaries of any location provided in the geojson and return the name of the location of which the point is inside.

### To Run:
```
curl -H "Content-type:text/plain" -X GET http://127.0.0.1:8000/get_city_name --data-ascii 28.610+77.223
```
### Output

![get_city_name](https://i.imgur.com/YPO71Bt.png)

## To Test

Simply run
```
py.test test_script.py
```
### Output

![pyTest](https://i.imgur.com/ATdyXkb.png)
