FROM mcr.microsoft.com/dotnet/sdk:8.0 AS buildstage

WORKDIR /src/Api

COPY src/Api/*.csproj .
RUN dotnet restore

COPY src/Api/. .
RUN dotnet publish -c Release -o /app/publish

# Runtime stage
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS final

WORKDIR /app

#copy files form  build stage ---> host/app/publish  to runtime stage app
COPY --from=buildstage /app/publish .

ENTRYPOINT ["dotnet", "DockerPracticeApi.dll"]