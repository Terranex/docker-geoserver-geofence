# docker-geoserver-geofence
Implementation of GeoServer/Geofence ready for deployment in a containerized environment.

## Installation
The default installation assumes you either have or is going to use a PostGIS database for GeoFence data storage. 
If you want to use the default H2 database, comment out the postgis service in the docker-compose.yml file before you start the container ecosystem

1. Pull the repository:
`git clone https://github.com/Terranex/docker-geoserver-geofence.git

2. Start the postgis service:
`docker-compose up -d postgis
