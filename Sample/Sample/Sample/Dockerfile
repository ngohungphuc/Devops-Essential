FROM microsoft/dotnet:2.1-aspnetcore-runtime-nanoserver-1709 AS base
WORKDIR /app
EXPOSE 80

FROM microsoft/dotnet:2.1-sdk-nanoserver-1709 AS build
WORKDIR /src
COPY Sample/Sample.csproj Sample/
RUN dotnet restore Sample/Sample.csproj
COPY . .
WORKDIR /src/Sample
RUN dotnet build Sample.csproj -c Release -o /app

FROM build AS publish
RUN dotnet publish Sample.csproj -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "Sample.dll"]
