#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER app
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["src/TeduBlog.WebApp/TeduBlog.WebApp.csproj", "src/TeduBlog.WebApp/"]
COPY ["src/TeduBlog.Core/TeduBlog.Core.csproj", "src/TeduBlog.Core/"]
COPY ["src/TeduBlog.Data/TeduBlog.Data.csproj", "src/TeduBlog.Data/"]
RUN dotnet restore "./src/TeduBlog.WebApp/./TeduBlog.WebApp.csproj"
COPY . .
WORKDIR "/src/src/TeduBlog.WebApp"
RUN dotnet build "./TeduBlog.WebApp.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./TeduBlog.WebApp.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "TeduBlog.WebApp.dll"]