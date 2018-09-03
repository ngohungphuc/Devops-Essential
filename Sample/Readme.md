Run

```
docker pull alpine
```

- Delete image by id

```
docker rmi -f id
```

The -f argument is used to remove images even when they are being
used by containers

- Deleting All Images

```
docker rmi -f $(docker images -q)
```

The -q argument specifies the oddly named quiet mode, which returns only the IMAGE ID values from
the docker images command, which are processed by the docker rmi command, removing all the images
in the list.

### Example

```
FROM microsoft/aspnetcore:1.1.1
COPY dist /app
WORKDIR /app
EXPOSE 80/tcp
ENTRYPOINT ["dotnet", "ExampleApp.dll"]
```

#### Copying the Application Files

When you containerize an ASP.NET Core application, all the compiled class files, NuGet packages,
configuration files, and the Razor views are added to the image. The COPY command copies files or folders
into the container.

```
COPY dist /app
```

This command copies the files from a folder called dist into a folder called /app in the container. The
dist folder doesn’t exist at the moment, but I’ll create it when I prepare the MVC project for use with the
container.

#### Setting the Working Directory

The WORKDIR command sets the working directory for the container, which is useful if you need to run
commands or use files without having to specify the full path each time. The command in the Docker file
sets the path to the /app folder that the COPY command created and that contains the application files.

#### Exposing the HTTP Port

Processes inside a container can open network ports without any special measures, but Docker won’t allow
the outside world to access them unless the Docker file contains an EXPOSE command that specifies the port
number, like this:

```
EXPOSE 80/tcp
```

This command tells Docker that it can make port 80 available for TCP traffic from outside the container.
For the example application, this is required so that the ASP.NET Core Kestrel server can receive HTTP
requests.

#### Running the Application

The final step in the Docker file is the ENTRYPOINT command, which tells Docker what to do when the
container starts.

```
ENTRYPOINT ["dotnet", "ExampleApp.dll"]
```

This command tells Docker to run the dotnet command-line tool to execute the ExampleApp.dll file,
which I will create in the next section. The path to the ExampleApp.dll file doesn’t have to be specified
because it is assumed to be within the directory specified by the WORKDIR command, which will contain all
the application’s files.

#### Preparing the Application for the Image

```
dotnet publish --framework netcoreapp1.1 --configuration Release --output dist
```

```
docker cp ./Views/Home/Index.cshtml exampleApp3000:/app/Views/Home/
```

The docker cp command is used to copy files in and out of containers. This command copies the
modified Index.cshtml file from the project folder in the host operating system into the /app/Views folder
in the exampleApp3000 folder, which is the folder from which the MVC application inside the container gets
its views.

#### Examining Changes to a Container

```
docker diff exampleApp3000
```

The docker diff command shows the differences between the files in the container and the image that
was used to create it

#### Executing Commands in Containers

```
docker exec exampleApp3000 cat /app/Views/Home/Index.cshtml
```

The docker exec command is used to execute commands inside the container. The name of the
container is followed by the command and any arguments that it requires

#### Starting a Shell in a Container

```
docker exec -it exampleApp3000 /bin/bash
```

The -it argument to the docker exec command tells Docker that this is an interactive command
that requires terminal support.

#### Putting Docker Volumes and Software-Defined Networks in Context

| Plugin                      | README                                                                                                                                                                                                                                                                                                                                               |
| --------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| What are they?              | Volumes allow important data to exist outside of the container, which means you can replace a container without losing the data that it created. Software-defined networks are Internet Protocol networks created by Docker that allow the applications in containers to communicate.                                                                |
| Why are they useful?        | Volumes make it possible to delete a container without also deleting the data it contains, which allows containers to be changed or upgraded without losing user data. Software-defined networks make it possible to create more complex applications that span multiple containers, making it easier to introduce common components like databases. |
| How are they used?          | These features are managed through the docker volume and docker network commands. Volumes and software-defined networks must be prepared before the containers that use them are created.                                                                                                                                                            |
| Are there any pitfalls or   |
| limitations?                | Working out which volumes are required by base images can be a difficult process. Software-defined networks only connect containers on a single server unless a Docker cluster is used                                                                                                                                                               |
| Are there any alternatives? | There are no alternatives to these features, but you require them for every project. Volumes are not required if a containerized application doesn’t generate data that you need to save when the container is removed. Software-defined networks are not required if your containers do not need to communicate.                                   |


#### Managing Data with a Docker Volume
Docker volumes solve the data file problem by keeping data files outside the container while still making
them accessible to the application that runs inside it.

Step 1: Updating the Docker File
```
VOLUME /data
WORKDIR /data
ENTRYPOINT (test -e message.txt && echo "File Exists" \
|| (echo "Creating File..." \
&& echo Hello, Docker $(date '+%X') > message.txt)) && cat message.txt
```

The VOLUME command tells Docker that any files stored in /data should be stored in a volume, putting
them outside the regular container file system. The important point to note is that the application running
in the container won’t know that the files in the /data directory are special: they will be read and written just
like any other file in the container’s file system.

Updating the Image to Use a Volume
```
docker build . -t apress/vtest -f Dockerfile.volumes
```

Step 2: Creating the Volume

The second step is to create the volume that will hold the data files.

Creating a Volume
```
docker volume create --name testdata
```
The docker volume create command is used to create a new volume and assign it a name, which
is testdata in this case. The volume is a self-contained file system that will provide the contents for one
directory in the container’s file system. Since the volume isn’t part of the container, the files it contains are
not deleted when the container is destroyed

Step 3: Creating the Container
The third and final step is to tell Docker which volume should be used by the container
```
docker run --name vtest2 -v testdata:/data apress/vtest
```
The -v argument tells Docker that any data the container creates in the /data directory should be
stored in the testdata volume.