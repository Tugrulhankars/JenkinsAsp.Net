# Build stage
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src
EXPOSE 8081
COPY ["JenkinsAsp.Net.csproj", "./"]
RUN dotnet restore "./JenkinsAsp.Net.csproj"
COPY . .
RUN dotnet publish "JenkinsAsp.Net.csproj" -c Release -o /app/publish

# Runtime stage
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS final
WORKDIR /app
COPY --from=build /app/publish .
ENTRYPOINT ["dotnet", "JenkinsAsp.Net.dll"]
