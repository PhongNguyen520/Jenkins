# Base image cho runtime
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 8080

# Image ?? build ?ng d?ng
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src

# Copy file csproj và restore dependency
COPY ["SwaggerTest/SwaggerTest.csproj", "SwaggerTest/"]
RUN dotnet restore "./SwaggerTest/SwaggerTest.csproj"

# Copy toàn b? source code vào container
COPY . .

# Build ?ng d?ng
WORKDIR "/src/SwaggerTest"
RUN dotnet build "./SwaggerTest.csproj" -c $BUILD_CONFIGURATION -o /app/build

# Publish ?ng d?ng
FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./SwaggerTest.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

# Final runtime image
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .

# Thi?t l?p bi?n môi tr??ng ?úng cách
ENV ASPNETCORE_ENVIRONMENT=Development

# Ch?y ?ng d?ng
ENTRYPOINT ["dotnet", "SwaggerTest.dll"]
