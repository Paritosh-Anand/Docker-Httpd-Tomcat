## Docker: Apache Web & Application server

### How things work currently
Most of the application that are built with on-premise datacenter ideology have a setup which involves Apache Tomcat as an application server where the web application is deployed. Along with this there is an Apache HTTPD web server that redirects requests landing on port 80 to the application server which is running on port 8080.

Prime benefits for using HTTPD as web server rather than utilising Tomcat web server capabilities:
* Better clutering for handling failures of Tomcat instances.
* Better perfromance of HTTPD in serving static content.
* Security point of view.

### In Terms of Docker
Talking in terms of Docker: the very best practise is to run one-container per service that is run one container for web server and another for application server. This is little different from a setup that has a VM machine which in which we are running both web server and application server. We configure web server using mod_jk that enables us to redirect our traffic to the application server and life goes on like this.

Apache HTTPD with mod_jk provides load balancing features that can help in load balancing request on multiple backends. Though we have a single backend in most of the cases that is our Apache Tomcat.

While docker-ising a web application mod_jk configurations are very crucial to setup an environment such that web server and application server in two different containers. Also by using mod_jk we can create a setup which have a single container that is running web server and multiple containers running application servers accepting requests from a single web server. This web server is also responsible for load balancing and stuff.


Make clone of this project and run below command. 
```
$ docker-compose up -d
```

This will start invoke two Docker containers 
- An Apache Tomcat container that will host a sample web application.
- An Apache HTTPD container that will run the httpd web server with mod_jk configuration. 

Mod_jk configurations will be pointing the backend to the tomcat container which will have it's AJP connector port exposed. As per Docker compose version 2 by default every container is reachable via other containers, hence the backend host i.e. Apache Tomcat will be already reachable from HTTPD. 

Apache HTTPD's port 80 will be mapped to base machine's port via which the user requests will land on our web server. Run below command to find out the base machine's port:
```
$ docker-compose ps
          Name                         Command               State               Ports
---------------------------------------------------------------------------------------------------
dockerhttpdtomcat_httpd_1   apachectl -k start -DFOREG ...   Up      443/tcp, 0.0.0.0:32776->80/tcp
dockerhttpdtomcat_web_1     catalina.sh run                  Up      8009/tcp, 8080/tcp
```

Do note: don't make hard-mappings of HTTPD's container port to base machine. It will kill the possiblities of scaling  any of the services.

Test the setup from browser on http://localhost:*mapped_httpd_port*. It will open Apache Tomcat's welcome page. URL in this case would be:
```
http://localhost:32776/
```
