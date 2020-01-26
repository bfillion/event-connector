FROM mcr.microsoft.com/dotnet/core/aspnet:3.0-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:3.0-buster AS build
WORKDIR /src
COPY events.connector.api/events.connector.api.csproj events.connector.api/
RUN dotnet restore "events.connector.api/events.connector.api.csproj"
COPY . .
WORKDIR "/src/events.connector.api"
RUN dotnet build "events.connector.api.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "events.connector.api.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "events.connector.api.dll"]
