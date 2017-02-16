# lagom-poc

Building Reactive Java 8 application with Lagom framework. This is a classic CRUD application which persist events in Cassandra Db. Here we are using external Cassandra to persist events and external kafka for publishing and subscribing between microservices.

## Prerequisites
1. Java 1.8
2. Maven 4.0

## Getting the Project
#### To clone the project follow the command
`git clone git@github.com:knoldus/lagom-poc.git`

#### Command to start the project
`mvn lagom:runAll`

#### Application runs on port 9000 by default  `http://localhost:9000`

## Json Formats for different Rest services are mentioned below :

#### 1. Create User:

Route(Method - POST) : `localhost:9000/api/user`

Rawdata(json): 
    {
	"id": "1",
	"name": "User 1",
	"age": 24
    }


#### 2. Update User:

Route(Method - PUT) : `localhost:9000/api/user`

Rawdata(json): 
    {
	"id": "1",
	"name": "User 1",
	"age": 30
    }
    

#### 3. Delete User:

Route(Method - DELETE) : `localhost:9000/api/user/:id`
    

#### 4. Get User details:

Route(Method - GET) : `localhost:9000/api/user/:id`

### Publish message to kafka topic `greetings`
`curl -H "Content-Type: application/json" -X POST -d '{"message":"Hi"}' http://localhost:9000/api/hello/Bob`

### Consume message from kafka topic
The application subscribes to the topic `greetings` and dump out messages to standard output.

## Tools Integrated

### >> Flyway-maven
Flyway is an open-source database migration tool. This project integrates H2 datbase and flyway tool and migrations written in this project are in SQL.</br>
The migrations are created in the directory `src/main/resources/db/migration/`</br>
These migrations can be executed using `mvn flyway:migrate`</br>
To check if the migrations have been successfully applied, issue `mvn flyway:info`</br>

### >> SonarQube (version 6.2)
SonarQube (formerly known as Sonar) is an open-source platform for continuous inspection of code quality. Sonar is a web based code quality analysis tool for MAVEN based JAVA projects. It covers a wide area of code quality checkpoints which include: Architecture & Design, Complexity, Duplications, Coding Rules, Potential Bugs, Unit test etc.

#### Command to start SonarQube server
`YOUR_DIR_PATH/sonarqube/bin/[OS]/sonar.sh console`

or use Docker's `docker run -d --name sonarqube -p 9000:9000 -p 9092:9092 sonarqube:6.2-alpine` for quick local testing.

#### SonarQube runs on port 9000 by default  `http://localhost:9000`

#### Default username & password for administrator
`username - admin`</br>
`password - admin`

#### Command to generate report on SonarQube server
`mvn clean - to clean the existing resources`</br>
`mvn install`</br>
`mvn sonar:sonar`

### >> Cobertura
Cobertura is a free Java tool that calculates the percentage of code accessed by tests. It can be used to identify which parts of your Java program are lacking test coverage. It is based on jcoverage.

#### Command to run test cases and generate code coverage report
`mvn cobertura:cobertura`

### >> Checkstyle
Checkstyle is a development tool to help programmers write Java code that adheres to a coding standard. The Checkstyle Plugin generates a report regarding the code style used by the developers.

#### Checktyle Plugin has some goals which are as follows
`checkstyle:checkstyle` is a reporting goal that performs Checkstyle analysis and generates a report on violations.<br>
`checkstyle:check` is a goal that performs Checkstyle analysis and outputs violations or a count of violations to the console, potentially failing the build. It can also be configured to re-use an earlier analysis.

#### Command to run checkstyle
`mvn checkstyle:checkstyle`<br>
`mvn checkstyle:check`

### >> Findbugs
FindBugs looks for bugs in Java programs. It is based on the concept of bug patterns.FindBugs uses static analysis to inspect Java bytecode for occurrences of bug patterns.FindBugs is a static code analysis tool which identifies problems found from Java code.

#### Command to generate Findbugs report
`mvn findbugs:findbugs`

### >> PMD
A Maven plugin for the PMD toolkit, that produces a report on both code rule violations and detected copy and paste fragments, as well as being able to fail the build based on these metrics.
PMD has additional rules to check for cyclomatic complexity, Npath complexity, etc which allows you write healthy code.Another advantage of using PMD is CPD (Copy/Paste Detector).It finds out code duplication across projects and is not constrained to JAVA

#### The plugin has following goals
`pmd:pmd` creates a PMD site report based on the rulesets and configuration set in the plugin. It can also generate a pmd output file aside from the site report in any of the following formats: xml, csv or txt. <br>
`pmd:cpd` generates a report for PMD's Copy/Paste Detector (CPD) tool. It can also generate a cpd results file in any of these formats: xml, csv or txt.

#### Command to generate PMD reports
`mvn pmd:pmd`

#### Steps to use Kafka-Docker
Install Docker
<p>Install docker-compose
<p>Update docker-compose.yml with your docker host IP
(KAFKA_ADVERTISED_HOST_NAME)
<p>Start the cluster
  <p>`$ docker-compose up`
<p>To start a cluster with two broker
  <p>`$ docker-compose scale kafka=2`
<p>NOTE:This will start a single zookeeper instance and two Kafka instances. You can use docker-compose ps to show the running instances. If you want to add more Kafka brokers simply increase the value passed to docker-compose scale kafka=n
<p>Starting the KAFKA SHELL
   <p>$ `start-kafka-shell.sh [DOCKER_HOST_IP] [ZK_HOST:ZK_PORT]`
   <p>for eg.- start-kafka-shell.sh 172.17.0.1 172.17.0.1:2181
<p>To test your setup, start a shell, create a topic and start a producer:
   <p>`$ $KAFKA_HOME/bin/kafka-topics.sh --create --topic [topic_name] --partitions 4 --zookeeper $ZK --replication-factor 2`
<p>`$ $KAFKA_HOME/bin/kafka-topics.sh --describe --topic topic --zookeeper $ZK`
<p>`$ $KAFKA_HOME/bin/kafka-console-producer.sh --topic=topic --broker-list=`broker-list.sh``
<p>Start another shell and start a consumer:
<p>`$ $KAFKA_HOME/bin/kafka-console-consumer.sh --topic=topic --zookeeper=$ZK`