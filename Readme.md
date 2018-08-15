![logo](img/docker.png)

Essential Docker for ASP.Net Core MVC

### Command
The Docker image used to deploy ASP.NET Core applications does not contain the .NET Core compiler,
which means you must use the dotnet publish command to create a directory that contains the compiled
code and all of the support files required to run the application

```
dotnet publish --framework netcoreapp1.1 --configuration Release --output dist
```