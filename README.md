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

