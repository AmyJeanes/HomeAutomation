FROM mcr.microsoft.com/dotnet/aspnet:11.0@sha256:e0d839005c47866d889283cd66531e11dfeca3c11e32753329241a6a775bb9ec AS base
USER app
WORKDIR /app
EXPOSE 8080

FROM mcr.microsoft.com/dotnet/sdk:11.0@sha256:2d222215f60a3f2e5a7ee2b6f24a4dd5d72e6f0ad1e7a0af954ebdce67377888 AS build
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