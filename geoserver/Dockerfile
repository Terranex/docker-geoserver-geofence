FROM tomcat

#Add the probe to geoserver WAR and move to webapps
#RUN mv /tmp/geofence-probe/* /tmp/geoserver/WEB-INF/lib/
#RUN jar -cvf /tmp/geoserver.war /tmp/geoserver
#RUN mv /tmp/geoserver.war /usr/local/tomcat/webapps
ARG GS_DOWNLOAD_URL=http://sourceforge.net/projects/geoserver/files/GeoServer/2.8.3/geoserver-2.8.3-war.zip
ARG GS_PROBE_DOWNLOAD_URL=http://ares.boundlessgeo.com/geoserver/2.8.x/community-latest/geoserver-2.8-SNAPSHOT-geofence-plugin.zip

LABEL GeoServerURL=$GS_DOWNLOAD_URL
LABEL GeoServerProbeURL=$GS_PROBE_DOWNLOAD_URL

#Uncomment below is you have the geoserver war locally
#COPY geoserver.war /tmp/geoserver.war

#Download geoserver WAR & unpack
RUN curl -Ls -o /tmp/geoserver.zip $GS_DOWNLOAD_URL
RUN unzip /tmp/geoserver.zip geoserver.war -d /tmp/
RUN unzip /tmp/geoserver.war -d /tmp/geoserver/

#Uncomment below is you have the probe jars locally
#COPY geofence-model* /tmp/geoserver/WEB-INF/lib/
#COPY geofence-services* /tmp/geoserver/WEB-INF/lib/
#COPY gs-geofence* /tmp/geoserver/WEB-INF/lib/

#Download geofence probe & add probe to geoserver WAR
RUN curl -Ls -o /tmp/geofence-probe.zip $GS_PROBE_DOWNLOAD_URL
RUN unzip /tmp/geofence-probe.zip -d /tmp/geofence-probe/ &&  mv /tmp/geofence-probe/*.jar /tmp/geoserver/WEB-INF/lib/

#Install ZIP and recreate geoserver WAR
RUN apt-get update && apt-get install zip
RUN cd /tmp/geoserver && zip -rv /usr/local/tomcat/webapps/geoserver.war .

#Check if probe was installed in geoserver WAR
#RUN unzip -l /usr/local/tomcat/webapps/geoserver.war
