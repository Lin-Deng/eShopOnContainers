﻿FROM mcr.microsoft.com/dotnet/core/aspnet AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/core/sdk AS build
WORKDIR /src

COPY scripts scripts/

COPY ApiGateways/*/*.csproj csproj-files/

COPY . .
WORKDIR /src/ApiGateways/ApiGateWays
RUN dotnet publish -c Release -o /app

FROM build AS publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "ApiGateWaysdll"]
