# Stage: base image for runtime
FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

# Stage: build the application
FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src

# Copy project file and restore dependencies
COPY ["ProjetoNetCore.csproj", "./"]
RUN dotnet restore "ProjetoNetCore.csproj"

# Copy all source files and build
COPY . .
RUN dotnet build "ProjetoNetCore.csproj" -c $BUILD_CONFIGURATION -o /app/build

# Stage: publish the application
FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "ProjetoNetCore.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

# Stage: final image for production/runtime
FROM base AS final
WORKDIR /app

# Copy the published output
COPY --from=publish /app/publish .

# Configure ASP.NET Core to listen on port 80
ENV ASPNETCORE_URLS=http://+:80

# Entry point
ENTRYPOINT ["dotnet", "ProjetoNetCore.dll"]
