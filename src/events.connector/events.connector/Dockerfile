FROM mcr.microsoft.com/dotnet/core/runtime:3.1-alpine AS base
WORKDIR /app

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-alpine AS build
WORKDIR /src
COPY events.connector/events.connector.csproj events.connector/
COPY events.connector.worker/events.connector.worker.csproj events.connector.worker/
RUN dotnet restore "events.connector/events.connector.csproj"
COPY . .
WORKDIR "/src/events.connector"
RUN dotnet build "events.connector.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "events.connector.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "events.connector.dll"]
