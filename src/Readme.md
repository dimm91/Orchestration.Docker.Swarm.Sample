
# Docker orchestration Sample

## Docker-Compose

> Compose is a tool for defining and running complex applications with Docker. With Compose, you define a multi-container application in a single file, then spin your application up in a single command which does everything that needs to be done to get it running.

### Build images with docker-compose

To build with docker-compose on an specific file use the following command `docker-compose -f [DockerComposeYamlFile] build`

```
docker-compose -f single.docker-compose.yml build
```

### Run continer with docker-compose
To run containers with docker-compose on an specific file use the following command:
`docker-compose -f [DockerComposeYamlFile] up -d`

```
docker-compose -f single.docker-compose.yml up -d
```

### Stop containers with docker-compose

To stop containers with docker-compose on an specific file use the following command: `docker-compose -f [DockerComposeYamlFile] down` or `docker-compose -f [DockerComposeYamlFile] down --rmi local`

This command will only stop the containers specified on the yaml file
```
docker-compose -f single.docker-compose.yml down
``` 

This command will stop and remove the `local` images specified on the yaml file.
``` 
docker-compose -f single.docker-compose.yml down --rmi local
``` 

## Docker Swarm

> It's a container orchestration tool, allows users to manage multiple containers. It is used by developers to create, deploy, and manage a cluster of Docker nodes. This tool is resourceful because it allows easy deployment.

### Terms
* Node: Kinda like an individual machine or as a Docker host present in the Swarm. There are 2 types
	* Managers: The controlling unit of the swarm
	* Workers: Notifies the Manager the current state of its assigned task so the manager can maintain the desired state of each worker.
* Services: is the application/program that needed to be deployed and/or scaled.
* Stacks: Group of all the services that make up your application and collect them into a single object

> Services & stacks are how we work with our applications and nodes are how we work with individual servers.

### Init Docker Swamr
To begin with, you need to execute the command
```
docker swarm init
```

### Deploy your app into docker swarm

You deploy an appliaction by using the following command: `docker stack deploy -c swarm.docker-compose.yml [StackName]`

```
docker stack deploy -c swarm.docker-compose.yml app
```
* `-c` = Path of the compose file

> If you are using Docker dekstop, then you can go directly to `localhost:[PortNumberOnComposeFile]`, but if you are using WSL2, then you need to go to the IP- address of your WSL2.
 
 To find out your IP on your WSL 2 execute the following command: 
 ```
 ip addr | grep eth0
 ```
The IP will be after inet: `inet XXX.XXX.XXX.XXX`

### Remove one service

```
docker service rm [NameOfTheServiceToRemove]
```

### Remove stack

```
docker stack rm [NameOfStack]
```
### Self-healing approach
> In swarm mode if there's a failure with a container wether the app exits or the container itself gets removed then the service gets repaired by spining up a new container which could be on a different node in the cluster. The container Id will be different from the one that was removed.

### Load-balancing
On the `Docker-Compose` yaml file, to add replicas to a service you will need to add a new section `deploy`.
 >This will get ignore if you try to run the app with `docker-compose` command, but they will get applied if you run the same app using Docker Swarm.

 ```yaml
 ...
 	deploy:
 		replicas: 3# Number of replicas you want on your service
 ...
 ```

 ### Remove everything

 You can reset everything by forcibly leaving the swarm.

 ```
 docker swarm leave -f
 ```
 