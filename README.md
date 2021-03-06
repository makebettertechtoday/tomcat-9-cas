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
- [How to Install Java on Centos 7](https://linuxize.com/post/install-java-on-centos-7/) - shows installing a switching between 11 and 8
- [Download and Install Java 8 on Windows](https://www.codejava.net/java-se/download-and-install-java-8-on-windows) - note the guide at the bottom about setting the PATH. To change versions of Java you can [set an environment variable](https://bryantson.medium.com/changing-default-java-version-in-microsoft-windows-674f2d6d0955); if you are just getting started it might be easier to uninstall and reinstall Java.
- [How to Install Java 8 on Mac](https://java.tutorials24x7.com/blog/how-to-install-java-8-on-mac) - One thing it doesn't show is switching between versions of Java. You may need a little more flexibility in development than being locked into Java 8. [Stack Overflow](https://stackoverflow.com/questions/21964709/how-to-set-or-change-the-default-java-jdk-version-on-os-x) to the rescue.
- [Docker](https://hub.docker.com/r/bitnami/java/) and [VirtualBox](https://app.vagrantup.com/boxes/search?order=desc&page=1&provider=&q=java+8&sort=created&utf8=%E2%9C%93) - If you already have command of these, you can likely skip the installation and just browse the commits.

## Resources
There is no lack of information about about CAS clients. Here are three resources that I found helpful.
- [CAS Installation on Tomcat over HTTPS](https://ktree.com/blog/cas-installation-on-tomcat-over-https.html)
- [CAS-ify a Java Application](https://cuit.columbia.edu/cas-authentication/java)
- [Tomcat Container Authentication](https://apereo.atlassian.net/wiki/spaces/CASC/pages/103252615/Tomcat+Container+Authentication?showComments=true&showCommentArea=true)