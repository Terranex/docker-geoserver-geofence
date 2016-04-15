# docker-geoserver-geofence
Implementation of GeoServer/Geofence ready for deployment in a containerized environment.

## Dependancies
### This repository requires the following to run:
- docker
- docker-compose

## Installation
The default installation assumes you either have or is going to use a PostGIS database for GeoFence data storage. 
If you want to use the default H2 database, comment out the postgis service in the docker-compose.yml file before you start the container ecosystem.

The installation follows the following steps:

1. Pull the repository.
2. Start the postgis service.
3. Create a PostGIS enabled database with the associated user.
4. Change your geofence service environment to match you database setup.
5. Launch the remaining geoserver & geofence services.

### 1. Pull the repository:
Choose a suitable directory and clone the repository:
```
$ git clone https://github.com/Terranex/docker-geoserver-geofence.git
```

### 2. Start the postgis service:
`$ docker-compose up -d postgis`

### 3. Create a PostGIS enabled database with the associated user:
##### First connect to you postgis service:
`$ docker exec -it geofencegeoserver_postgis_1 bash`
##### Next create your user:
```
$ su postgres
$ createuser --createdb --login --no-superuser --no-createrole --pwprompt geofence
$ exit
```
##### Next create your database:
`$ psql -U postgres -c "CREATE DATABASE geofence OWNER geofence ENCODING 'UTF8';"`
##### Lastly enable postgis extentions:
`$ psql -U postgres -d geofence -c "CREATE EXTENSION postgis;"`

Your database is now set up and ready. 
Try logging in as `geofence`:
```
psql -U geofence
```
Now run `SELECT PostGIS_Version();` to validate postgis extentions where successfully installed.
For additional help customizing this service visit [PostgreSQL and PostGIS in a Docker container](https://hub.docker.com/r/cheewai/postgis/)

### 4. Change your geofence service environment to match you database setup.
 - Open the GeoFence configuration override file *./geofence/geofence-datasource-ovr.properties*
 - Adjust the required database settings to match your postgis service
 - For help go to [Geofence configuration help](https://github.com/geoserver/geofence/wiki/GeoFence-configuration)

### 5. Launch the remaining geoserver & geofence services.
`$ docker-compose up -d`


## Other
Environment variables per service are stored in a ENV file in each of the service directories, i.e.
- ./geofence
- ./geoserver
- ./postgis

This should run out of the box, but can be ammended should the need arise.
This will automatically download the required software, i.e. GeoServer, GeoFence along with the dependencies based on the download URLs provided in the docker-compose.yml `ags` subsection per service.

##### Additional help:
- [Geoserver](http://docs.geoserver.org/)
- [Geofence](https://github.com/geoserver/geofence/wiki)
