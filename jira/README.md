#NGINX as SSL proxy to Atlassian Jira

This is a simple guide with configuration to run Jira behind nginx web server.

The nginx server here accomplishes the following 

* terminate SSL connection
* redirect HTTP to HTTPS protocol
* improved SSL configuration

In this configuration, connection to http://jira.example.com is redirected to https://jira.example.com on nginx. nginx then makes a http only connection to the backend jira service running on tomcat at the default port 8080.

It is advised that nginx server runs on the same host as Jira and is _dedicated_ for Jira application only.

As of this writing, it is tested on the following at their default locations,

* nginx 1.4.7
* jira 6.3
* fedora 20 x86_64 

_jira.example.com_ is used as a server name and the URL that will be used to access Jira.

Replace the default nginx.conf with the one from this repository. Edit ssl key and certificate locations as well as the servername.

Edit your Jira server.xml and replace the following block,

        <Connector port="8080"
                   maxThreads="150"
                   minSpareThreads="25"
                   connectionTimeout="20000"
                   enableLookups="false"
                   maxHttpHeaderSize="8192"
                   protocol="HTTP/1.1"
                   useBodyEncodingForURI="true"
                   redirectPort="8443"
                   acceptCount="100"
                   disableUploadTimeout="true"/>


with 

        <Connector port="8080"
                   maxThreads="150"
                   minSpareThreads="25"
                   connectionTimeout="20000"
                   enableLookups="false"
                   maxHttpHeaderSize="8192"
                   protocol="HTTP/1.1"
                   useBodyEncodingForURI="true"
                   redirectPort="8443"
                   acceptCount="100"
                   disableUploadTimeout="true"
                   scheme="https"
                   proxyName="jira.example.com"
                   proxyPort="443"/>


Restart or start nginx and jira services.
