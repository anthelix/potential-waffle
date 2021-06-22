# potential-waffle
Stuff to work around Sql

# Purposee
I discovered and practiced PostgreSQL last year during the 1st covid19 pandemic with [Udacity's nanodegree data enginnering](https://www.udacity.com/course/data-engineer-nanodegree--nd027) and [challenges on Hackerrank](https://www.hackerrank.com/). At the moment I am learning Sql server and I want to improve my practice with other learners. Nothing better than an international community and caring mentors! I would like continue to use Docker too. At home, I work with Solus, a Linux distribution built from scratch,  maintained as "eopkg". At woork, I learn Windows OS. So, I need two Docker with external data and a Makefile...?

# how to do it
[How to start 8 weeks Sql Challenge](https://8weeksqlchallenge.com/getting-started/)  
[Here the ressources ](https://8weeksqlchallenge.com/resources/)

### install docker
- https://hub.docker.com/r/microsoft/mssql-server-windows-developer
- [octopusData](https://octopus.com/blog/running-sql-server-developer-install-with-docker)
- create folder for persist data on windows: C:\Users\Stagiaire\Documents\Docker\Volumes
- [shared volume with linux](https://docs.docker.com/docker-for-windows/#shared-drives)
- [pull immage microsoft/mssql-server-windows-developer](docker pull microsoft/mssql-server-windows-developer)
- [don't forget to Switch to Windows containers.](https://stackoverflow.com/questions/43346580/docker-image-operating-system-windows-cannot-be-used-on-this-platform#comment73937606_43346580)
- [run  this docker comand docker run --name SQLServer -d -p 1433:1433](docker run --name SQLServer -d -p 1433:1433 -e sa_password=Password_01 -e ACCEPT_EULA=Y microsoft/mssql-server-windows-developer)
- create data base with sql server studio
````
CREATE DATABASE [OctopusDeploy]
 CONTAINMENT = NONE
 ON  PRIMARY
( NAME = N'OctopusDeploy', FILENAME = N'C:\SQLData\OctopusDeploy.mdf' , SIZE = 8192KB , FILEGROWTH = 65536KB )
 LOG ON
( NAME = N'OctopusDeploy_log', FILENAME = N'C:\SQLData\OctopusDeploy_log.ldf' , SIZE = 8192KB , FILEGROWTH = 65536KB )
GO
CREATE DATABASE [TeamCity]
 CONTAINMENT = NONE
 ON  PRIMARY
( NAME = N'TeamCity', FILENAME = N'C:\SQLData\TeamCity.mdf' , SIZE = 8192KB , FILEGROWTH = 65536KB )
 LOG ON
( NAME = N'TeamCity_log', FILENAME = N'C:\SQLData\TeamCity_log.ldf' , SIZE = 8192KB , FILEGROWTH = 65536KB )
GO

````
==> create those files .mdf and .ldf in  C:\Users\Stagiaire\Documents\Docker\Volumes\SQLserver.    
and passed to the container using the `attach_dbs` environment variable
``
docker stop SQLServer
docker rm SQLServer
$attachDbs = "[{'dbName':'OctopusDeploy','dbFiles':['C:\\SQLData\\OctopusDeploy.mdf','C:\\SQLData\\OctopusDeploy_log.ldf']},{'dbName':'TeamCity','dbFiles':['C:\\SQLData\\TeamCity.mdf','C:\\SQLData\\TeamCity_log.ldf']}]"
docker run --name SQLServer -d -p 1433:1433 --volume  C:\Users\Stagiaire\Documents\Docker\Volumes\SQLserver:c:\SQLData -e sa_password=Password_01 -e ACCEPT_EULA=Y -e attach_dbs=$attachDbs microsoft/mssql-server-windows-developer
``
- equivalennt  with docker-compose 
``
version: '3.7'
services:
  SQLServer:
   image: microsoft/mssql-server-windows-developer
   environment:
     - ACCEPT_EULA=Y
     - SA_PASSWORD=Password_01   
     - attach_dbs=[{'dbName':'OctopusDeploy','dbFiles':['C:\\SQLData\\OctopusDeploy.mdf','C:\\SQLData\\OctopusDeploy_log.ldf']},{'dbName':'TeamCity','dbFiles':['C:\\SQLData\\TeamCity.mdf','C:\\SQLData\\TeamCity_log.ldf']}]
   ports:
     - '1433:1433'
   volumes:
     - C:\Users\Stagiaire\Documents\Docker\Volumes\SQLserver:c:\SQLData
``
- saved that docker-compose file in theC:\Users\Stagiaire\Documents\Docker folder on my hard drive in docker-composee.yml
- 
- https://www.docker.com/blog/build-your-first-docker-windows-server-container/
- [check this for docker-compose](https://dbafromthecold.com/2020/07/17/sql-server-and-docker-compose/)
# path with with Danny
[Week 1](https://8weeksqlchallenge.com/case-study-1)
[Week 2](https://8weeksqlchallenge.com/case-study-2)
[Week 3](https://8weeksqlchallenge.com/case-study-3)
[Week-4](https://8weeksqlchallenge.com/case-study-4)

# todo

