FROM tomcat:9.0

COPY server.xml /usr/local/tomcat/conf/

# add context to /usr/local/tomcat/webapps
COPY ./sample.war /usr/local/tomcat/webapps/sample.war

EXPOSE 8009
