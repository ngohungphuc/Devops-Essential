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
