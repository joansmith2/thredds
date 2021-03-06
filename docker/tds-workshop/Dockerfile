FROM tomcat:8
MAINTAINER Sean Arms <sarms@ucar.edu>
#
# Expose ports 8080 (tomcat) and 8443 (tomcat ssl)
#
EXPOSE 8080 8443

#
# Install nano and vim so that we have text editors
#
RUN apt-get update && apt-get install -y \
  less \
  vim 

#
# Create the user and group tomcat and change ownershiup of the tomcat
#   directory to user and group tomcat
#
RUN groupadd -r tomcat -g 1000 && \
useradd -u 1000 -r -g tomcat -d /usr/local/tomcat -s /bin/bash -c "User tomcat for Docker image" tomcat

#
# Create a data directory and let tomcat own it
#
RUN mkdir /data 

#
# Copy over modified tomcat files
#
COPY tomcat-files/bin/setenv.sh /usr/local/tomcat/bin/setenv.sh
COPY tomcat-files/conf/server.xml /usr/local/tomcat/conf/server.xml
COPY tomcat-files/conf/tomcat-users.xml /usr/local/tomcat/conf/tomcat-users.xml
COPY tomcat-files/conf/keystore /usr/local/tomcat/conf/keystore

#
# Copy over TDM files
#
RUN mkdir -p /usr/local/tomcat/content/tdm
COPY tdm-files/runTdm.sh /usr/local/tomcat/content/tdm/runTdm.sh

#
# Copy over script that starts up tomcat and the TDM
#
COPY tomcat-files/startItAll.sh /usr/local/tomcat/startItAll.sh

#
# Change owner of tomcat directory to user and group tomcat
#
RUN chown -R tomcat:tomcat /usr/local/tomcat 

#
# Switch to user tomcat
#
USER tomcat

#
# get the latest stable THREDDS Data Server (TDS)
#
#RUN wget -P /usr/local/tomcat/webapps ftp://ftp.unidata.ucar.edu/pub/thredds/4.6/current/thredds.war
#
# Need the latest build of the TDS for the workshop
#
COPY tds-files/tds-4.6.3-SNAPSHOT.war /usr/local/tomcat/webapps/thredds##04.06.03-SNAPSHOT.war

#
# get the latest THREDDS Data Monitor (TDM)
#
#RUN wget -P /usr/local/tomcat/content/tdm ftp://ftp.unidata.ucar.edu/pub/thredds/4.6/current/tdm-4.6.jar
#
# Need the latest build of the TDM for the workshop
#
COPY tdm-files/tdmFat-4.6.3-SNAPSHOT.jar /usr/local/tomcat/content/tdm/tdmFat-4.6.3-SNAPSHOT.jar

#
# Start the TDM
#
CMD ["/bin/bash", "/usr/local/tomcat/startItAll.sh"]
