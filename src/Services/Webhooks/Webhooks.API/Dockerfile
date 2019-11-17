FROM mcr.microsoft.com/dotnet/core/aspnet AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/core/sdk AS build
WORKDIR /src

COPY src/scripts scripts/

COPY src/ApiGateways/*/*/*.csproj .
COPY src/BuildingBlocks/*/*/*.csproj .
COPY src/Services/*/*/*.csproj .
COPY src/Web/*/*.csproj .

COPY . .
WORKDIR /src/Services/Webhooks/Webhooks.API
RUN dotnet publish -c Release -o /app

FROM build AS publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "Webhooks.API.dll"]