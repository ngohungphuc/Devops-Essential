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

| Argument      | Description                                                                                                                |
| ------------- | -------------------------------------------------------------------------------------------------------------------------- |
| -e, --env     | This argument sets an environment variable                                                                                 |
| --name        | This argument assigns a name to the container.                                                                             |
| --network     | This argument connects a container to a software-defined network.                                                          |
| -p, --publish | This argument maps a host operating system port to one inside the container.                                               |
| --rm          | This argument tells Docker to remove the container when it stops.                                                          |
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
