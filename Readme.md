![logo](img/docker.png)

Essential Docker for .Net Developer

### Command

##

The Docker image used to deploy ASP.NET Core applications does not contain the .NET Core compiler,
which means you must use the dotnet publish command to create a directory that contains the compiled
code and all of the support files required to run the application

```
dotnet publish --framework netcoreapp1.1 --configuration Release --output dist
```

##

This command publishes the application into a folder called dist, which can be incorporated in the
image using the COPY command in a Docker file. The Docker file is processed to create the image using the
docker build command.

```
docker build . -t apress/exampleapp -f Dockerfile
```

The first argument is a period, indicating the current working directory, which sets the context directory
on which the commands in the Docker file are performed. The -t argument specifies the name of the image,
and the -f argument specifies the Docker file

##

## Essential Commands for Working with Images

| Command       | Description                                                                                                                                                                           |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| docker build  | This command processes a Docker file and creates an image.                                                                                                                            |
| docker images | This command lists the images that are available on the local system. The -q argument returns a list of unique IDs that can be used with the docker rmi command to remove all images. |
| docker pull   | This command downloads an image from a repository.                                                                                                                                    |
| docker push   | This command publishes an image to a repository. You may have to authenticate with the repository using the docker login command.                                                     |
| docker tag    | This command is used to associate a name with an image.                                                                                                                               |
| docker rmi    | This command removes images from the local system. The -f argument can be used to remove images for which containers exist.                                                           |

##

## The Essential Docker File Commands

| Command    | Description                                                                                                                                                                                                                  |
| ---------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| FROM       | This command specifies the base image. For ASP.NET Core MVC projects, this command is generally used to select the microsoft/aspnetcore:1.1.1 (for deployment) or microsoft/aspnetcore-build:1.1.1 (for development) images. |
| WORKDIR    | This command changes the working directory for subsequent commands in the Docker file.                                                                                                                                       |
| COPY       | This command adds files so they will become part of the file system of containers that are created from the image.                                                                                                           |
| RUN        | This command executes a command as the Docker file is processed. It is commonly used to download additional files to include in the image or to run commands that configure the existing files.                              |
| EXPOSE     | This command exposes a port so that containers created from the image can receive network requests.                                                                                                                          |
| ENV        | This command defines environment variables that are used to configure containers created from the image.                                                                                                                     |
| VOLUME     | This command denotes that a Docker volume should be used to provide the contents of a specific directory.                                                                                                                    |
| ENTRYPOINT | This command specifies the application that will be run in containers created from the image.                                                                                                                                |

##

Containers are created using the docker create command, like this:

```
docker create -p 3000:80 --name exampleApp3000 apress/exampleapp
```

Once a container has been created, it can be started using the docker start command.

```
docker start exampleApp3000
```

You can create and start a container in a single step using the docker run command.

```
docker run -p 3000:80 --name exampleApp4000 apress/exampleapp
```

##

## Essential Arguments for the docker create and docker run Commands

| Argument      | Description                                                                                                                 |
| ------------- | --------------------------------------------------------------------------------------------------------------------------- |
| -e, --env     | This argument sets an environment variable                                                                                  |
| --name        | This argument assigns a name to the container.                                                                              |
| --network     | This argument connects a container to a software-defined network.                                                           |
| -p, --publish | This argument maps a host operating system port to one inside the container.                                                |
| --rm          | This argument tells Docker to remove the container when it stops.                                                           |
| -v, --volume  | This argument is used to configure a volume that will provide the contents for a directory in the container’s file system. |

##

## Essential Commands for Working with Containers

| Command       | Description                                                                                                                                                                                                                                                       |
| ------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| docker create | This command creates a new container.                                                                                                                                                                                                                             |
| docker start  | This command starts a container.                                                                                                                                                                                                                                  |
| docker run    | This command creates and starts a container in a single step.                                                                                                                                                                                                     |
| docker stop   | This command stops a container.                                                                                                                                                                                                                                   |
| docker rm     | This command removes a container.                                                                                                                                                                                                                                 |
| docker ps     | This command lists the containers on the local system. The -a argument includes stopped containers. The -q argument returns a list of unique IDs, which can be used to operate on multiple containers with the docker start, docker stop, and docker rm commands. |
| docker logs   | This command inspects the output generated by a container.                                                                                                                                                                                                        |
| docker exec   | This command executes a command in a container or starts an interactive session.                                                                                                                                                                                  |

## Docker Volumes Quick Reference

Volumes allow data files to be stored outside of a container, which means they are not deleted when the
container is deleted or updated.
Volumes are defined using the VOLUME command in Docker files, like this:

```
VOLUME /var/lib/mysql
```

This tells Docker that the files in the /var/lib/mysql folder should be stored in a volume. This is useful
only when a named volume is created and applied when configuring the container. Volumes are created
using the docker volume create command, like this:
docker volume create --name productdata
The --name argument is used to specify a name for the volume, which is then used with the -v argument
to the docker create or docker run command like this:

```
docker run --name mysql -v productdata:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=mysecret -e
bind-address=0.0.0.0 mysql:8.0.0
```

This command tells Docker that the productdata volume will be used to provide
the contents of the /var/lib/mysql directory in the container’s file system. Removing the container won’t
remove the volume, which means that any files that are created in the /var/lib/mysql directory won’t be
deleted, allowing the result of user actions to be stored persistently.

## Essential Commands for Working with Volumes

| Command              | Description                                                                                                                                                                           |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| docker volume create | This command creates a new volume.                                                                                                                                                    |
| docker volume ls     | This command lists the volumes that have been created. The -q argument returns a list of unique IDs, which can be used to delete multiple volumes using the docker volume rm command. |
| docker volume rm     | This command removes one or more volumes.                                                                                                                                             |

## 
Software-defined networks are used to connect containers together, using networks that are created and managed using Docker

Software-defined networks are created using the docker network create command, like this:
```
docker network create backend
```

This command creates a software-defined network called backend. Containers can be connected to the
network using the --network argument to the docker create or docker start command, like this:
```
docker run -d --name mysql -v productdata:/var/lib/mysql --network=backend -e
MYSQL_ROOT_PASSWORD=mysecret -e bind-address=0.0.0.0 mysql:8.0.0
```

Containers can also be connected to software-defined networks using the docker network connect
command, like this:
```
docker network connect frontend productapp1
```
This command connects the container called productapp1 to the software-defined network called
frontend.

## Essential Commands for Working with Software-Defined Networks

| Command                | Description                                                                                                                                                                                                                                                  |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| docker network create  | This command creates a new software-defined network.                                                                                                                                                                                                         |
| docker network connect | This command connects a container to a software-defined network.                                                                                                                                                                                             |
| docker network ls      | This command lists the software-defined networks that have been created, including the ones that Docker uses automatically. The -q argument returns a list of unique IDs, which can be used to delete multiple networks using the docker network rm command. |
| docker network rm      | This command removes a software-defined network. There are some built-in networks that Docker creates and that cannot be removed.                                                                                                                            |

## Docker Compose
Docker Compose is used to describe complex applications that require multiple containers, volumes,
and software-defined networks.

| Keyword     | Description                                                                                                                                                                                                                     |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| version     | This keyword specifies the version of the compose file schema. At the time of writing the latest version is version 3.                                                                                                          |
| volume      | This keyword is used to list the volumes that are used by the containers defined in the compose file.                                                                                                                           |
| networks    | This keyword is used to list the volumes that are used by the containers defined in the compose file. The same keyword is used to list the networks that individual containers will be connected to.                            |
| services    | This keyword is used to denote the section of the compose file that describes containers.                                                                                                                                       |
| image       | This keyword is used to specify the image that should be used to create a container.                                                                                                                                            |
| build       | This keyword is used to denote the section that specifies how the image for a container will be created.                                                                                                                        |
| context     | This keyword specifies the context directory that will be used when building the image for a container.                                                                                                                         |
| dockerfile  | This keyword specifies the Docker file that will be used when building the image for a container.                                                                                                                               |
| environment | This keyword is used to define an environment variable that will be applied to a container.                                                                                                                                     |
| depends_on  | This keyword is used to specify dependencies between services. Docker doesn’t have insight into when applications in containers are ready, so additional steps must be taken to control the startup sequence of an application |

## 
Docker files are processed using the docker-compose build command like this:
```
docker-compose -f docker-compose.yml build
```

The containers, networks, and volumes in a compose file are created and starting using the
docker-compose up command.
```
docker-compose up
```

## Essential Commands for Docker Compose
| Command              | Description                                                                                                                                                            |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| docker-compose build | This command processes the contents of the compose file and creates the images required for the services it contains.                                                  |
| docker-compose up    | This command creates the containers, networks, and volumes defined in the compose file and starts the containers.                                                      |
| docker-compose stop  | This command stops the containers created from the services in the compose file. The containers, networks, and volumes are left in place so they can be started again. |
| docker-compose down  | This command stops the containers created from the services in the compose file and removes them, along with the networks and volumes.                                 |
| docker-compose scale | This command changes the number of containers that are running for a service.                                                                                          |
| docker-compose ps    | This command lists the containers that have been created for the services defined in the compose file.                                                                 |