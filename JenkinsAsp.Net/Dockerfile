# Base image for running the app
FROM mcr.microsoft.com/dotnet/aspnet:8.0-nanoserver-1809 AS base
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

# Build image for building the app
FROM mcr.microsoft.com/dotnet/sdk:8.0-nanoserver-1809 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src

# Correct the path to the JenkinsAsp.Net.csproj file
COPY ["JenkinsAsp.Net/JenkinsAsp.Net.csproj", "./"]
RUN dotnet restore "./JenkinsAsp.Net.csproj"

# Copy the rest of the source code
COPY . .
WORKDIR "/src"
RUN dotnet build "JenkinsAsp.Net.csproj" -c %BUILD_CONFIGURATION% -o /app/build

# Publish the app to prepare for release
FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "JenkinsAsp.Net.csproj" -c %BUILD_CONFIGURATION% -o /app/publish /p:UseAppHost=false

# Final image for running the app
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "JenkinsAsp.Net.dll"]
