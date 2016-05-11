FROM tb4mmaggots/tomcat

#Add the probe to geoserver WAR and move to webapps
#RUN mv /tmp/geofence-probe/* /tmp/geoserver/WEB-INF/lib/
#RUN jar -cvf /tmp/geoserver.war /tmp/geoserver
#RUN mv /tmp/geoserver.war /usr/local/tomcat/webapps
ARG GS_DOWNLOAD_URL
ARG GS_PROBE_DOWNLOAD_URL

LABEL GeoServerURL=$GS_DOWNLOAD_URL
LABEL GeoServerProbeURL=$GS_PROBE_DOWNLOAD_URL

RUN apk update \
    && apk upgrade \
    && apk add curl zip $(apk search -q ttf- | grep -v '\-doc')

#Uncomment below is you have the geoserver war locally
#COPY geoserver.war /tmp/geoserver.war

#Download geoserver WAR & unpack
RUN curl -Ls -o /tmp/geoserver.zip $GS_DOWNLOAD_URL \
    && unzip /tmp/geoserver.zip geoserver.war -d /tmp/ \
    && mkdir /tmp/geoserver \
    && unzip /tmp/geoserver.war -d /tmp/geoserver/

#Uncomment below is you have the probe jars locally
#COPY geofence-model* /tmp/geoserver/WEB-INF/lib/
#COPY geofence-services* /tmp/geoserver/WEB-INF/lib/
#COPY gs-geofence* /tmp/geoserver/WEB-INF/lib/
RUN ls -l /tmp
#Download geofence probe & add probe to geoserver WAR
RUN curl -Ls -o /tmp/geofence-probe.zip $GS_PROBE_DOWNLOAD_URL \
    && mkdir /tmp/geofence-probe \
    && unzip /tmp/geofence-probe.zip -d /tmp/geofence-probe/ \
    && mv /tmp/geofence-probe/*.jar /tmp/geoserver/WEB-INF/lib/ \
    && cd /tmp/geoserver && zip -rv /usr/local/tomcat7/webapps/geoserver.war .

#Check if probe was installed in geoserver WAR
#RUN unzip -l /usr/local/tomcat/webapps/geoserver.war
