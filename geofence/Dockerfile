FROM tb4mmaggots/tomcat

ARG GF_DOWNLOAD_URL=http://ares.boundlessgeo.com/geofence/master/geofence-master-latest-war.zip
ARG PG_SPATIAL_DOWNLOAD_URL=http://www.hibernatespatial.org/repository/org/hibernatespatial/hibernate-spatial-postgis/1.1.1/hibernate-spatial-postgis-1.1.1.jar

LABEL GeoFenceDownloadURL=$GF_DOWNLOAD_URL
LABEL PostGISHibernateSpatial=$PG_SPATIAL_DOWNLOAD_URL

RUN apk update \
 && apk upgrade \
 && apk add --no-cache curl

#Uncomment if geofence WAR is local
#COPY geofence.war /usr/local/tomcat7/webapps/

#Download & unpack GeoFence
RUN curl -Ls -o /tmp/geofence.zip $GF_DOWNLOAD_URL

#Do initial run & sleep until done
RUN unzip /tmp/geofence.zip -d /tmp/ \
 && mv /tmp/gui/web/target/geofence.war /usr/local/tomcat7/webapps/ \
 && startup.sh && sleep 60 \
 && shutdown.sh

#Add Hibernate spatial lib for PostGIS to geofence
RUN wget -P /usr/local/tomcat7/webapps/geofence/WEB-INF/lib/ $PG_SPATIAL_DOWNLOAD_URL

#Cleanup
RUN rm -rf /tmp/*
