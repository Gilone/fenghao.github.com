---
title: "&#128193 Simple File Transfer Proxy"
layout: post
date: 2020-08-23 22:10
tag: 
- Project
image: https://www.eginnovations.com/blog/wp-content/uploads/2019/12/tuning-tomcat.jpg
headerImage: true
projects: true
hidden: false # don't count this post in blog pagination
description: Deploy the server on a host, which can be accessed by other computers through the HTTP protocol and perform various operations.
category: project
author: Hao Feng
---

[GitHub](https://github.com/Gilone/jersey_tomcat_file_transfer_restful_api_with_guide)
Deploy the server on a host, which can be accessed by other computers through the HTTP protocol and perform various operations.

  - With one token, and bypass other tedious verification
  - Restful, and easy to deploy
  - Magic
  
![][version-badge]
![][author-badge]

## Getting Started
This section will show how to prepare an environment for debugging and deployment.

### Dependency
- Java JDK 11+
- [Maven]
- [Tomcat] 8.x/9.x
- [Jersey] 2.x
- VS Code extensions: [Tomcat for Java], [Java Extension Pack] and [SonarLint] (optional)

### Initialization
- *Install Java JDK 11+*, set system environment variable `JAVA_HOME: C:\java`, `CLASSPATH: %JAVA_HOME%\lib` (optional) and `PATH: %JAVA_HOME%\bin`

- *Install VS Code extensions*, [Tomcat for Java] and [Java Extension Pack]

- *Install Maven*, set system environment variable `MAVEN_HOME: C:\Maven` and `PATH: C:\Maven\bin`

  Also, set the repo for Maven in `C:\Maven\conf\settings.xml`:

```XML
<!-- localRepository
    | The path to the local repository maven will use to store artifacts.
    |
    | Default: ${user.home}/.m2/repository
  <localRepository>/path/to/local/repo</localRepository>
  -->
  <localRepository>C:\MavenRepoFileYouWant</localRepository>
```
- *Install Tomcat 8/9*, choose "32-bit/64-bit Windows Service Installer" and set system environment variable `CATALINA_HOME: C:\tomcat` 
  
  Also, set service connector port and service shutdown port, 8800 and 8801, for example, in the c:\tomcat\conf\server.xml

```XML
<!-- A "Connector" represents an endpoint by which requests are received
      and responses are returned. Documentation at :
      Java HTTP Connector: /docs/config/http.html
      Java AJP  Connector: /docs/config/ajp.html
      APR (HTTP/AJP) Connector: /docs/apr.html
      Define a non-SSL/TLS HTTP/1.1 Connector on port 8800
  -->
  <Connector port="8800" protocol="HTTP/1.1"
              connectionTimeout="20000"
              redirectPort="8443" />
  <!-- A "Connector" using the shared thread pool-->
  <!--
  <Connector executor="tomcatThreadPool"
              port="8800" protocol="HTTP/1.1"
              connectionTimeout="20000"
              redirectPort="8443" />
  -->  
```
```XML
<!-- Note:  A "Server" is not itself a "Container", so you may not
     define subcomponents such as "Valves" at this level.
     Documentation at /docs/config/server.html
 -->
<Server port="8801" shutdown="SHUTDOWN">
```
  Do not forget to setup Tomcat extensions in VS Code: 
  
  ![setup tomcat](https://raw.githubusercontent.com/Gilone/jersey_tomcat_file_transfer_restful_api_with_guide/main/Image/tomcat_extensions.png.jpg)

  Note that the tomcat in VS Code has a configuration file for **itself**, please right-click on the tomcat in VS Code and select *open server configuration* to set up ports.

### Clone
```shell
 git clone git@gitlabe2.ext.net.nokia.com:fehao/java_file_sever.git
 cd .\helloweb\
```

## Development
This section will show how to debug this project on VS Code and build the .war package.

### Introduction to the file structure
This repository contains the following file structure:

- *helloweb*  :the main project folder

  - *src\main\java\com\toolsof5g*  :several packages of the web project

    - *auth*  :the filter annotation implement of token authentication

    - *fileserver*  :one of the servlet entry point, main body of the api

    - *fileutils*  :files operation package

    - *interfaces*  :interface definition of filter annotation

  - *src\main\webapp\WEB-INF*  :required folders for web projects

    - *web.xml*  :configure critical parameters of web project such as servlet mapping

  - *pom.xml*  :Maven's configuration file of package dependency management using to import Jersey, please make sure the network works well and wait for a while to get ready

- *filepathinfo.csv*  :the file that maps the required file name to the real file path

### Building
Use maven command in the terminal to clean up history build files and get new .war package.   
```shell
C:\JavaProject\helloweb> mvn clean package
```
![mvn package](https://raw.githubusercontent.com/Gilone/jersey_tomcat_file_transfer_restful_api_with_guide/main/Image/terminal_mvn_package.PNG)

You will find .war package in helloweb\target\

### Debugging
Click the right mouse button in the VS Code on .war package, and select "Debug on Tomcat Server"

![debugging](https://raw.githubusercontent.com/Gilone/jersey_tomcat_file_transfer_restful_api_with_guide/main/Image/debug.png.jpg)

Then the Tomcat server is started in debugging mode.

If you select "Run on Tomcat Server", the server will be started in normal mode inside the VS Code.

## Usage
You will see the way to deploy the .war package on a tomcat server and use this Restful service in this section.

### Deployment
For deployment, you **do not need so many dependencies**, only `Tomcat`, `.war Package` and the `Mapping File.csv`.

Firstly, move the .war package to C:\tomcat\webapps.

Then, put the mapping file "filepathinfo.csv" on the C:\tomcat which is the default root directory.

In the end, one way to start the server is C:\tomcat\bin\Tomcat8w.exe.

![run on tomcat](https://raw.githubusercontent.com/Gilone/jersey_tomcat_file_transfer_restful_api_with_guide/main/Image/run.png.jpg)

### Test
Remember to add token in Headers before sending HTTP request. 

There is a repository which is an example of Python script sending GET request to API. https://github.com/Gilone/restful_api_client_sample_with_python_requests

Also, using [Postman] is another good choice to test this API.

![postman](https://raw.githubusercontent.com/Gilone/jersey_tomcat_file_transfer_restful_api_with_guide/main/Image/postman.png.jpg)

Now let's do a test:

Firstly, create a new microsoft word document named *tests.docx* at *C:\download_files*, which looks like this:

![new file](https://raw.githubusercontent.com/Gilone/jersey_tomcat_file_transfer_restful_api_with_guide/main/Image/new_word_doc.png.jpg)

Secondly, deploy the .war package and edit the mapping file on *C:\tomcat*.

![mapping](https://raw.githubusercontent.com/Gilone/jersey_tomcat_file_transfer_restful_api_with_guide/main/Image/set_mapping.png.jpg)

Then, start the Tomcat server, and send get requests through Postman or Python scripts. You will see the results:

![results](https://raw.githubusercontent.com/Gilone/jersey_tomcat_file_transfer_restful_api_with_guide/main/Image/test_result.png.jpg)

## Frequently Asked Questions

### Why my VS Code extension *Project Manager for Java* does not work?
After opening the project folder in VS Code, you can try to right-click on *pom.xml* and select *update project configuration*.

### Why there is an error indicating *Failed to read artifact descriptor for XX* in pom.xml?
Switch your network environment from Nokia to a personal hotspot, and clean up the path to the local repository of maven, *C:\MavenRepoFileYouWant* or *C:\Users\YourName\\.m2\repository*, for example, then run following commands in the terminal.
```shell
C:\JavaProject\helloweb> mvn clean
C:\JavaProject\helloweb> mvn install
```
### Why tomcat does not work? Where could I find logs?
For logs in tomcat, you could check *C:\tomcat\logs*.

For logs in tomcat of vscode, check here:
![results](https://raw.githubusercontent.com/Gilone/jersey_tomcat_file_transfer_restful_api_with_guide/main/Image/log_in_vscode.png.jpg)

### Why Maven in VS Code could not resolve all dependencies? How to make local repo of maven works in VS Code?
You could try cleaning up VS Code's workspace directory in *C:\Users\your_username\AppData\Roaming\Code\User\workspaceStorage*,

Also, try to add `"java.configuration.maven.userSettings": "C:\\Maven\\conf\\settings.xml"` in *C:\Users\fehao\AppData\Roaming\Code\User\settings.json*, connect to your personnal hotspot, restart VS Code, and have a cup of coffee.

### Is there a convenient way to deploy or configure my JDK environment and VS Code?
Yes! There is a special tools you could try when you encounter problem.
 
https://aka.ms/vscode-java-installer-win

It will automatically detect and deploy VS Code and JDK environment.

### Got an error:"The compiler compliance specified is 1.8 but a JRE 11 is used vscode". How to set JDK version of Maven?
In pom.xml:
```xml
  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
  </properties>
```
In maven's conf/setting.xml:
```xml
       <profile>
		    <id>jdk-1.8</id>  
	        <activation>  
	          <activeByDefault>true</activeByDefault>  
	          <jdk>1.8</jdk>  
	        </activation>  
			<properties>  
			<maven.compiler.source>1.8</maven.compiler.source>  
			<maven.compiler.target>1.8</maven.compiler.target>  
			<maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>  
			</properties> 
        </profile>
```
### Got an error in Tomcat logs:'java.lang.ClassNotFoundException: javax.servlet.Filter'
Check your jersey's and tomcat's version.

Jersey version 2.x is based off JAX-RS API version 2.x which in turn is part of Java EE version 8.

Tomcat version 10.0 is based off Servlet API version 5.0 which in turn is part of Java/Jakarta EE version 9.

More info: http://tomcat.apache.org/whichversion.html

### Why I got a 403 error? What is the root path of tomcat in VS Code?
When you get http 403 error, you may see messages like *"{}"* or *"{tests.txt:c:\\test\\tests.txt}"* showing on the response page, which are contents of the mapping file.

This error means that the server faild to found the mapping file, faild to reach the file from its real path or faild to found the mapping in the mapping file.

The mapping file is placed in the root path of the tomcat serve by default. In tomcat, this path is *C:\tomcat*, and in tomcat in VS Code, this directory should be a temporary folder. You could set a breakpoint, and watch the *WRITE_CSV_FILE_PATH* in *APIUtils.java*.


### Why I cannot fetch files from OneDrive? Why do I still get a 403 error after checking the above questions?
First of all, due to some unknown reasons, tomcat cannot reach *C:\Users* outside of VS Code, which will return an "access denied" error. In order to solve this problem, you should ensure that the file to be copied is not under the *C:\Users* directory, although this is the default path of the OneDrive folder.

Second, if you need to fetch a OneDrive file, please right-click the file and select "Always keep on this device", so that tomcat could access this file outside of VS Code.

**Enjoy!**



<!--lint ignore no-html-->

<table>
<tr valign="middle">
<td width="20%" align="center" colspan="2">
  <a href="https://tomcat.apache.org/">Apache Tomcat</a> 🥇<br><br>
  <a href="https://tomcat.apache.org/"><img src="https://tomcat.apache.org/res/images/tomcat.png" width="128"></a>
</td>
<td width="20%" align="center" colspan="2">
  <a href="https://eclipse-ee4j.github.io/jersey/">Jersey</a> 🥇<br><br>
  <a href="https://eclipse-ee4j.github.io/jersey/"><img src="https://pbs.twimg.com/profile_images/628552740399095808/E6ajYzZW.png" width="128"></a>
</td>
<td width="20%" align="center" colspan="2">
  <a href="https://maven.apache.org/index.html">Apache Maven</a><br><br>
  <!--OC has a sharper image-->
  <a href="https://maven.apache.org/index.html"><img src="https://maven.apache.org/images/maven-logo-black-on-white.png" width="128"></a>
</td>
</tr>
</table>


<!-- Definitions -->

[version-badge]: https://img.shields.io/badge/Version-3.1-blue

[author-badge]: https://img.shields.io/badge/Author-Team%20Mushroom-green

[Jersey]: https://eclipse-ee4j.github.io/jersey/

[Maven]: https://maven.apache.org/index.html

[Tomcat]: https://tomcat.apache.org/

[Tomcat for Java]: https://marketplace.visualstudio.com/items?itemName=adashen.vscode-tomcat

[SonarLint]: https://marketplace.visualstudio.com/items?itemName=SonarSource.sonarlint-vscode

[Java Extension Pack]: https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-pack

[Postman]: https://www.postman.com/