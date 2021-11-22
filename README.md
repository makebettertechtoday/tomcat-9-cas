# Tomcat 9 with CAS

## Introduction
There is quite a bit of documentation on setting up a Apereo (Jasig) CAS server and client. This doesn't repeat that; it extends it to show how to configure a Tomcat CAS client. For myself to learn I needed more than references to Maven and code snippets in isolation.

For each branch the commits start from scratch and build up to a working client. The main branch is intended to be the simplest route to achieve a working CAS client. Later branches use different features to make the Java app and client more production ready and possibly different frameworks.

If there is sufficient interest, I can do the same for configuring a CAS server on Tomcat. For the moment, this is only focused on configuring a CAS client. Also, I'm assuming a single Tomcat server and not addressing scaling and high availablity here.

## Prerequisites
- [Java 8](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html)
- [Maven](https://maven.apache.org/download.cgi)
- [Tomcat 9](https://tomcat.apache.org/download-90.cgi)
- [Apereo (Jasig) CAS Client](https://github.com/apereo/java-cas-client)
### Java 8 Installed
You can install Java on virtually anything and there are probably over a billion websites with instructions on how to intall it. Note that newer versions of Java will create an error because the Jave EE modules have been removed in version 11 and newer (deprecated in 9 and 10). You can solve this with [Jakarta EE 9](https://stackoverflow.com/questions/52502189/java-11-package-javax-xml-bind-does-not-exist), but is beyond where I want to keep this, as in basic.
#### Installation Links
- [How to Install Java on Centos 7](https://linuxize.com/post/install-java-on-centos-7/) - shows installing and switching between 11 and 8
- [Download and Install Java 8 on Windows](https://www.codejava.net/java-se/download-and-install-java-8-on-windows) - note the guide at the bottom about setting the PATH. To change versions of Java you can [set an environment variable](https://bryantson.medium.com/changing-default-java-version-in-microsoft-windows-674f2d6d0955); if you are just getting started it might be easier to uninstall and reinstall Java.
- [How to Install Java 8 on Mac](https://java.tutorials24x7.com/blog/how-to-install-java-8-on-mac) - One thing it doesn't show is switching between versions of Java. You may need a little more flexibility in development than being locked into Java 8. [Stack Overflow](https://stackoverflow.com/questions/21964709/how-to-set-or-change-the-default-java-jdk-version-on-os-x) to the rescue.
- [Docker](https://hub.docker.com/r/bitnami/java/) and [VirtualBox](https://app.vagrantup.com/boxes/search?order=desc&page=1&provider=&q=java+8&sort=created&utf8=%E2%9C%93) - If you already have command of these, you can likely skip the installation and just browse the commits.

## Resources
There is no lack of information about about CAS clients. Here are three resources that I found helpful.
- [CAS Installation on Tomcat over HTTPS](https://ktree.com/blog/cas-installation-on-tomcat-over-https.html)
- [CAS-ify a Java Application](https://cuit.columbia.edu/cas-authentication/java)
- [Tomcat Container Authentication](https://apereo.atlassian.net/wiki/spaces/CASC/pages/103252615/Tomcat+Container+Authentication?showComments=true&showCommentArea=true)

## Check Java
You can check that Java is installed correctly with the following lines
```
$ java -version 
java version "1.8.0_271"
Java(TM) SE Runtime Environment (build 1.8.0_271-b09)
Java HotSpot(TM) 64-Bit Server VM (build 25.271-b09, mixed mode)
```

## Install Maven
Maven is a build tool for Java. It is similar to composer in PHP except it not only downloads dependencies it builds them, too.
### Install Maven with yum on Centos
You should be able to install Maven with yum without enabling the EPEL repo. Read all the answers on this [Stack Overflow](https://stackoverflow.com/questions/7532928/how-do-i-install-maven-with-yum). It is customary to shorten it to mvn.
```
$ yum install maven
$ mvn -version
```
### Install Maven on everything else
For other platforms it is best to follow [Maven's install directions](https://maven.apache.org/install.html). Note setting the PATH will make it easier to execute. You can do that with:
```
export PATH=/opt/apache-maven-3.8.4/bin:$PATH
```
but that is only temporary. It is best to add this to ~/.bashrc (or .zshrc) so it is permanent.

## Java Apereo CAS Client
Once Java is installed you need to download the [Java Apereo CAS Client](https://github.com/apereo/java-cas-client). You may see it mentioned as the JASIG CAS client as well. The below line will clone (download) the CAS client into your current directory.
```
$ git clone git@github.com:apereo/java-cas-client.git .
```
Change to the directory and have Maven build the client.
```
$ cd java-cas-client
$ mvn clean package
```

## Install Tomcat

## Move JAR files to Tomcat Lib
Move cas-client-core-3.6.3-SNAPSHOT.jar, cas-client-integration-tomcat-common-3.6.3-SNAPSHOT.jar, and cas-client-integration-tomcat-v90-3.6.3-SNAPSHOT.jar to Tomcat's lib directory.
They are in /path/to/java-cas-client-master/target and /path/to/java-cas-client-master/cas-client-integration-tomcat-v90/target

Best practice would be to add a POM file and have Maven move these; I didn't do that.

## Edit web.xml
Add the below to the filter section. About line 591.
```
                                <!-- https://cuit.columbia.edu/cas-authentication/java -->
                                 <filter>
                                    <filter-name>CAS Authentication Filter</filter-name>
                                    <filter-class>org.jasig.cas.client.authentication.AuthenticationFilter</filter-class>
                                    <init-param>
                                    <param-name>casServerLoginUrl</param-name>
                                    <param-value>https://prime.uindy.edu/cas/login</param-value>
                                    </init-param>
                                    <init-param>
                                    <param-name>serverName</param-name>
                                    <param-value>http://localhost:8080</param-value>
                                    </init-param>
                                </filter>
                                <filter>
                                    <filter-name>CAS Validation Filter</filter-name>
                                    <filter-class>org.jasig.cas.client.validation.Cas20ProxyReceivingTicketValidationFilter</filter-class>
                                    <init-param>
                                    <param-name>casServerUrlPrefix</param-name>
                                    <param-value>https://prime.uindy.edu/cas/</param-value>
                                    </init-param>
                                    <init-param>
                                    <param-name>serverName</param-name>
                                    <param-value>http://localhost:8080</param-value>
                                    </init-param>
                                    <init-param>
                                    <param-name>artifactParameterName</param-name>
                                    <param-value>ticket</param-value>
                                    </init-param>
                                    <init-param>
                                    <param-name>redirectAfterValidation</param-name>
                                    <param-value>true</param-value>
                                    </init-param>
                                </filter>
                                <filter>
                                    <filter-name>CAS HttpServletRequest Wrapper Filter</filter-name>
                                    <filter-class>org.jasig.cas.client.util.HttpServletRequestWrapperFilter</filter-class>
                                </filter> 
```

Add the below to the filter-mapping section
```
                <!-- https://cuit.columbia.edu/cas-authentication/java -->
                <filter-mapping> 
                    <filter-name>CAS Authentication Filter</filter-name> 
                    <url-pattern>/*</url-pattern> 
                    </filter-mapping> 
                <filter-mapping> 
                    <filter-name>CAS Validation Filter</filter-name> 
                    <url-pattern>/*</url-pattern> 
                </filter-mapping> 
                <filter-mapping> 
                    <filter-name>CAS HttpServletRequest Wrapper Filter</filter-name> 
                    <url-pattern>/*</url-pattern> 
                </filter-mapping>
```

## Start Tomcat

## Try to go to localhost:8080