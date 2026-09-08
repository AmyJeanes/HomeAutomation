FROM mcr.microsoft.com/dotnet/aspnet:10.0@sha256:3a494b8a73ec3248c237c9438592ff1e04587edf65863212bc1027e54dcb6f36 AS base
USER app
WORKDIR /app
EXPOSE 8080

FROM mcr.microsoft.com/dotnet/sdk:10.0@sha256:096ca98743616ccfa14dccaa2a86615fd7345bd0c4535445226e98dbd46af2b5 AS build
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