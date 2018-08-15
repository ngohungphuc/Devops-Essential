![logo](img/docker.png)

Essential Docker for ASP.Net Core MVC

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