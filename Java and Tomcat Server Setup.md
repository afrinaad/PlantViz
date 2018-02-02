

## Java and Tomcat Server Setup

***In order to use Tomcat in Windows, you must install Java JDK beforehand.***
1.	Go to http://www.oracle.com/technetwork/java/javase/downloads/index.html
2.	Download the latest Java JDK stable version. For this system, it is recommended to use Java JDK 8.
3.	Run the downloaded Java JDK installer.
4.	Follow the instructions provided by the installation wizard.
<br><br>
#### STEP 1 : Download Tomcat 8 package
1.	Go to http://tomcat.apache.org/
2.	Under **Downloads**, click **Tomcat 8**
3.	Under **8.5.27** (latest version at this time) **Binary Distributions**, download 32-bit/64-bit Windows zip package depending on your Windows. (e.g., “*apache-tomcat-8.5.27-windows-x86.zip*”)
<br><br>
#### STEP 2 : Install Tomcat 8 package
1.	Unzip downloaded file into a folder in C: directory.
2.	Rename unzipped file for ease of use in the future. (e.g., “*C:/tomcat8*”)
<br><br>
#### STEP 3 : Create Environment Variable JAVA_HOME and CATALINA_HOME
1.	Find your JDK installation directory.
The default directory is in “*C:\Program Files\Java\jdk1.8.0_{xx}*” where *{xx}* is the upgrade number.
2.	Find Tomcat installation directory created in **STEP 2 : Install Tomcat 8 package**.
3.	Go to **Control Panel** → **System and Security** → **System** → **Advanced system settings**.
4.	Click **Environment Variables**.
5.	Under **System Variables**, click **New**.
6.	For **JAVA_HOME**<br>
a.	In **Variable Name**, enter **JAVA_HOME**.<br>
b.	In **Variable Value**, enter your JDK installed directory in step 1, then click **OK**.<br>
7.	For **CATALINA_HOME**<br>
a.	In **Variable Name**, enter **CATALINA_HOME**.<br>
b.	In **Variable Value**, enter the top level of Tomcat installation folder in step 2, then click **OK**.
<br><br>
#### STEP 4 : Launch Tomcat Server
1.	Go to **bin** folder in Tomcat’s installation directory. (e.g, “*C:/tomcat8/bin*”)
2.	Run **startup.bat** to start Tomcat server
3.	Run **shutdown.bat** to stop Tomcat server
<br><br><br>
## Installing Tomcat version 8 in Linux

Before installing Tomcat, make sure Java JDK is installed in your Linux. If there is no Java installed, follow steps below.
<br><br>
1.	Create a new directory to install Java. It is recommended to install it in **opt** directory. (e.g., “*/opt/java*")
2.	Download the latest stable Java JDK source tar package. It is recommended to use **Java JDK version 8u45**. 
3.	Move the downloaded tar into **/opt/java** directory and extract the tar package.
<br>`tar -zxvf jdk-8u45-linux-i586.tar.gz`
4.	Set up Environment Variables **JAVA_HOME**. You may change the java version accordingly.
```
export JAVA_HOME=/opt/java/jdk1.8.0_45/	
export JRE_HOME=/opt/java/jdk1.8.0._45/jre 	
export PATH=$PATH:/opt/java/jdk1.8.0_45/bin:/opt/java/jdk1.8.0_45/jre/bin
```
5.	Verify Java version to confirm that installation is complete.
<br>`java -version`
<br><br>
#### STEP 1 : Download Tomcat 8 package
1.	Go to http://tomcat.apache.org/
2.	Under **Downloads**, click **Tomcat 8**
3.	Under **8.5.27** (latest version at this time) **Binary Distributions**, download 32-bit/64-bit Linux zip package depending on your Linux.
<br><br>
#### STEP 2 : Extract downloaded Tomcat 8 package
1.	Make sure the package is in the directory that you want to install it in
2.	Extract the package
`tar zxvf apache-tomcat-8.5.27.tar.gz`
<br><br>
#### STEP 3 : Start Tomcat
1.	Go to the **bin** folder of your Tomcat installation directory
2.	Run **startup.bat** OR start **catalina.sh** to start Tomcat
`startup` OR `./catalina.sh start`
4.	Run **shutdown.bat** OR stop **catalina.sh** to stop Tomcat
5.	`shutdown` OR `./catalina.sh stop`

