

## Deploy plantviz.war in Tomcat Manager

WAR (Web Archive) file is a file that has a collection of JAR files, JSP, Servlets and other resource components that are necessary to build a web application.

***NOTE : Make sure the tomcat server is launched first.***

**STEP 1 : Download plantviz.war**
1.	Go to https://goo.gl/b7Mc5G
2.	Download the file and unzip

**STEP 2 : Configure roles in Tomcat server**
1.	Go to **conf** folder in Tomcat’s installation directory and open **tomcat-users.xml**
2.	Add code below before the final line in the file

```
    <role rolename="manager-gui"/>
    <role rolename="manager-script"/>
    <user username="admin" password="password" roles="manager-gui, manager-script"/>
```

**STEP 3 : Upload WAR file in Tomcat Manager**
1.	Go to http://localhost:8080/manager
2.	Under **Deploy** section, find another section WAR file to deploy
3.	Choose WAR file downloaded in **STEP 1 : Download plantviz.war** and click **Deploy**
4.	The page will reload and there should be a message stating that the deployment is successful.
The deployed WAR file should appear in the **Applications** section

PlantViz now should be accessible from http://localhost:8080/plantviz <br/>
To access PlantViz’s homepage, go to http://localhost:8080/plantviz/home.jsp

