FROM mcr.microsoft.com/dotnet/aspnet:9.0-bookworm-slim@sha256:2b70ef1451cb01c2118969ad15fd0ae40fdbac158f09699fefae0dc32c4ca352 AS base
USER app
WORKDIR /app
EXPOSE 8080

FROM mcr.microsoft.com/dotnet/sdk:9.0@sha256:840f3b62b9742dde4461a3c31e38ffd34d41d7d33afd39c378cfcfd5dcb82bd5 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["HomeAutomation.Web/HomeAutomation.Web.csproj", "HomeAutomation.Web/"]
RUN dotnet restore "./HomeAutomation.Web/HomeAutomation.Web.csproj"
COPY . .
WORKDIR "/src/HomeAutomation.Web"
RUN dotnet build "./HomeAutomation.Web.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./HomeAutomation.Web.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "HomeAutomation.Web.dll"]