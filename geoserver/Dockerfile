FROM tb4mmaggots/tomcat

#Add the probe to geoserver WAR and move to webapps
#RUN mv /tmp/geofence-probe/* /tmp/geoserver/WEB-INF/lib/
#RUN jar -cvf /tmp/geoserver.war /tmp/geoserver
#RUN mv /tmp/geoserver.war /usr/local/tomcat/webapps
ARG GS_URL
ARG GS_GF_PROBE_URL
ARG GS_CAS_URL

LABEL GeoServerURL=$GS_URL
LABEL GeoServerProbeURL=$GS_GF_PROBE_URL
LABEL GeoServerCas=$GS_CAS_URL

RUN apk update \
    && apk upgrade \
    && apk add --no-cache curl zip $(apk search -q ttf- | grep -v '\-doc')

#Download geoserver & required plugins
RUN curl -Ls -o /tmp/geoserver.zip $GS_URL \
    && curl -Ls -o /tmp/geofence.zip $GS_GF_PROBE_URL \
    && curl -Ls -o /tmp/cas.zip $GS_CAS_URL

#Repackage geoserver with requirements
RUN unzip /tmp/geoserver.zip geoserver.war -d /tmp/ \
    && mkdir /tmp/geoserver && mkdir /tmp/geofence && mkdir /tmp/cas \
    && unzip /tmp/geoserver.war -d /tmp/geoserver/ \
    && unzip /tmp/geofence.zip -d /tmp/geofence/ \
    && unzip /tmp/cas.zip -d /tmp/cas/ \
    && mv /tmp/geofence/*.jar /tmp/geoserver/WEB-INF/lib/ \
    && mv /tmp/cas/*.jar /tmp/geoserver/WEB-INF/lib/ \
    && cd /tmp/geoserver && zip -rv /usr/local/tomcat7/webapps/geoserver.war .

#Cleanup
RUN rm -rf /tmp/*
