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

### Creating a Custom Image

```
docker build . -t apress/exampleapp -f Dockerfile
```

The docker build command creates a new image. The period that follows the build keyword provides
the context, which is the location that is used for commands such as COPY in the Docker file. The -t argument
tags the new image as apress/exampleapp, and the -f argument specifies the Docker file that contains
the instructions for creating the image. (The convention for naming images is to use your name or your
organization’s name, followed by the application name.)

### Creating Containers

```
docker create -p 3000:80 --name exampleApp3000 apress/exampleapp
```

- The docker create command is used to create a new image.
- The -p argument to the docker create command tells Docker how to map port 80 inside the container
  to the host operating system
- The --name argument assigns a name to the container, which makes it easier to work with once it
  has been created. The name in this case is exampleApp3000, indicating that this container will respond to
  requests sent to port 3000 in the host operating system.
- The final argument tells Docker which image to use as the template for the new container. This
  command specifies the apress/exampleapp image

### Starting Containers

```
docker start exampleApp3000
```

### Starting All Containers

```
docker start $(docker ps -aq)
```

The command combines docker start with the output of the docker ps command. The -a argument
includes containers that are not running, and the -q argument returns just the container IDs.

### Stopping Containers

Containers are stopped using the docker stop command, which can stop one or more containers by name
or by ID.

```
docker stop exampleApp3000
```

### Stopping All Containers

```
docker stop $(docker ps -q)
```

### Getting Container Output

```
docker logs exampleApp3000
```

### Following a Container’s Logs

```
docker start exampleApp3000
docker logs -f exampleApp3000
```
