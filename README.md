# Sample war application using maven 

1. Navigate to the directory and run the below maven command to build the war file as
```
$mvn clean verify

[INFO] Scanning for projects...
[INFO]                                                                         
[INFO] ------------------------------------------------------------------------
[INFO] Building Example 0.0.1-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] 
[INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ Example ---
[INFO] 
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ Example ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory /Users/prem/Desktop/codebase/java-maven-sample-war/src/main/resources
[INFO] 
[INFO] --- maven-compiler-plugin:3.2:compile (default-compile) @ Example ---
[INFO] Changes detected - recompiling the module!
[INFO] Compiling 1 source file to /Users/prem/Desktop/codebase/java-maven-sample-war/target/classes
[INFO] 
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ Example ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory /Users/prem/Desktop/codebase/java-maven-sample-war/src/test/resources
[INFO] 
[INFO] --- maven-compiler-plugin:3.2:testCompile (default-testCompile) @ Example ---
[INFO] No sources to compile
[INFO] 
[INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ Example ---
[INFO] No tests to run.
[INFO] 
[INFO] --- maven-war-plugin:2.5:war (default-war) @ Example ---
[INFO] Packaging webapp
[INFO] Assembling webapp [Example] in [/Users/prem/Desktop/codebase/java-maven-sample-war/target/Example-0.0.1-SNAPSHOT]
[INFO] Processing war project
[INFO] Copying webapp resources [/Users/prem/Desktop/codebase/java-maven-sample-war/src/main/webapp]
[INFO] Webapp assembled in [69 msecs]
[INFO] Building war: /Users/prem/Desktop/codebase/java-maven-sample-war/target/Example-0.0.1-SNAPSHOT.war
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 3.698 s
[INFO] Finished at: 2018-10-31T20:12:34+05:30
[INFO] Final Memory: 16M/139M
[INFO] ------------------------------------------------------------------------
```
Once the above is completed the file Example-0.0.1-SNAPSHOT.war will be created under target/ folder

2. Now its time for docker, first build the docker image as below command, make sure you are running from the directory which have Dockerfile, otherwise add the dockerfile with path and name in the command too
```
$docker build -t tomcat-sample:1.0 .

Sending build context to Docker daemon  271.9kB
Step 1/2 : FROM tomcat:8.0.20-jre8
 ---> e88a065848be
Step 2/2 : COPY target/Example-0.0.1-SNAPSHOT.war /usr/local/tomcat/webapps/sample.war
 ---> 68da124851e4
Successfully built 68da124851e4
Successfully tagged tomcat-sample:1.0
```
3. Start the above created image with the below docker command
```
$docker run --name tomcat -it --rm -d -p 8080:8080 tomcat-sample:1.0
```
4. Navigate to http://localhost:8080/sample/ to check the started docker image with above created war file.

#### The java code under src/main is of no use, its just as placeholder right now.

## You can verify the running docker with below command's
```
$docker ps -l
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                    NAMES
bfd111dae312        tomcat-sample:1.0   "catalina.sh run"   5 minutes ago       Up 5 minutes        0.0.0.0:8080->8080/tcp   tomcat


$docker exec bfd111dae312 ps -eaf | grep -i tomcat
root         1     0  1 14:43 pts/0    00:00:05 /usr/bin/java -Djava.util.logging.config.file=/usr/local/tomcat/conf/logging.properties -Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager -Djava.endorsed.dirs=/usr/local/tomcat/endorsed -classpath /usr/local/tomcat/bin/bootstrap.jar:/usr/local/tomcat/bin/tomcat-juli.jar -Dcatalina.base=/usr/local/tomcat -Dcatalina.home=/usr/local/tomcat -Djava.io.tmpdir=/usr/local/tomcat/temp org.apache.catalina.startup.Bootstrap start

```
