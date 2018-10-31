# Sample war application using maven 

1. Navigate to the directory and run the below maven command to build the war file as
```
mvn clean verify
```
Once the above is completed the file Example-0.0.1-SNAPSHOT.war will be created under target/ folder
2. Now its time for docker, first build the docker image as below command, make sure you are running from the directory which have Dockerfile, otherwise add the dockerfile with path and name in the command too
```
docker build -t tomcat-sample:1.0 .
```
3. Start the above created image with the below docker command
```
docker run --name tomcat -it --rm -d -p 8080:8080 tomcat-sample:1.0
```
4. Navigate to http://localhost:8080/sample/ to check the started docker image with above created war file.


##You can verify the running docker with below command's
```
$docker ps -l
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                    NAMES
b9f0f0d4eb06        tomcat-sample:1.0   "catalina.sh run"   8 minutes ago       Up 8 minutes        0.0.0.0:8080->8080/tcp   tomcat

$docker exec b9f0f0d4eb06 ls -lrt webapps/
total 28
drwxr-xr-x  5 root root 4096 Mar 31  2015 manager
drwxr-xr-x  5 root root 4096 Mar 31  2015 host-manager
drwxr-xr-x  6 root root 4096 Mar 31  2015 examples
drwxr-xr-x 14 root root 4096 Mar 31  2015 docs
drwxr-xr-x  3 root root 4096 Mar 31  2015 ROOT
-rw-r--r--  1 root root 3114 Oct 31 13:56 sample.war
drwxr-xr-x  4 root root 4096 Oct 31 13:57 sample

```
