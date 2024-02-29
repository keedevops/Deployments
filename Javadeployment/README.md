# JAVA APPLICATION DEPLOYMENT USING GRADLE TOMCAT AND DOCKER 

This is a project where a java app is built using gradle, containerised using docker and hosted using tomcat server


1. Create an instance with ubuntu with inbound rules 8080 port open
![Screenshot Capture - 2024-02-29 - 15-29-13](https://github.com/keedevops/Deployments/assets/155215036/1c77bf70-937e-4960-8532-38521bcc8f7c)

2.Clone a github repository with the source code

```
git clone https://github.com/DeekshithSN/CICD_Java_gradle_application.git

```
3.Update the packages and install java

```
apt-get update
apt install openjdk-11-jdk
java -version
nano ~/.bashrc
source ~/.bashrc
echo $JAVA_HOME

```
4. Dowloading Gradle for building the code
```
 wget https://services.gradle.org/distributions/gradle-7.6.4-bin.zip
 mkdir /opt/gradle
 cd /opt/gradle
 unzip -d /opt/gradle gradle-7.6.4-bin.zip

```
5.While unzipping the gradle package it asks you to install unzip application

```
apt install unzip
cd ..
cd /home/ubuntu
unzip -d /opt/gradle gradle-7.6.4-bin.zip 
cd /opt/gradle
export PATH=$PATH:/opt/gradle/gradle-7.6.4/bin
echo "export PATH=$PATH:/opt/gradle/gradle-7.6.4/bin" >> ~/.bashrc
gradle --version

```
6.We are now going to build the code using gradle

```
 cd /home/ubuntu
 cd CICD_Java_gradle_application/
 gradle build
 cd build
 cd libs
 ls
 cd ../..

```
7. Now we are creating a dockerfile instead of using the existing dockerfile we have created a new dockerfile

```
nano Dockerfile123.sh

```
```
# Use a base image with Java 11 and Tomcat
FROM tomcat:9-jdk11

# Set the working directory in the container
WORKDIR /usr/local/tomcat/webapps

# Copy the WAR file into the container
COPY /build/libs/sampleWeb-0.0.1-SNAPSHOT.war .


# Expose the port your application will run on
EXPOSE 5000

# Command to run your application
CMD ["catalina.sh", "run"]

RUN rm -rf ROOT && mv sampleWeb-0.0.1-SNAPSHOT.war ROOT.war

```
8. Run the Dockerfile12.sh
```
sh Docker.sh
```
9.Creating a docker image and running it as a container 

```
docker build -t demo .
docker images
docker run -d -p 8080:8080 --name demoapp 27c
docker ps

```
**docker build -t demo .**: This command builds a Docker image using the Dockerfile in the current directory (.) and tags (-t) the image with the name demo.

**docker images**: This command lists all the Docker images that are currently available on your system. After running the previous docker build command, you should see an entry for the demo image.

**docker run -d -p 8080:8080 --name demoapp 27c**: This command runs a Docker container in detached mode (-d), maps port 8080 of the host to port 8080 of the container (-p 8080:8080), gives the container the name demoapp (--name demoapp), and specifies the Docker image to use (27c). The 27c is the truncated ID of the Docker image you built earlier with the docker build command.

**docker ps**: This command lists all the Docker containers that are currently running on your system. After running the docker run command, you should see an entry for the demoapp container.

10.Now we can view our application using the public ip of your instance followed with the port 8080


