FROM mcr.microsoft.com/dotnet/core/aspnet AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/core/sdk AS build
WORKDIR /src

COPY src/scripts scripts/

COPY src/ApiGateways/*/*/*.csproj csproj-files/
COPY src/BuildingBlocks/*/*/*.csproj csproj-files/
COPY src/Services/*/*/*.csproj csproj-files/
COPY src/Web/*/*.csproj csproj-files/

COPY . .
WORKDIR /src/Services/Basket/Basket.API
RUN dotnet publish -c Release -o /app

FROM build as unittest
WORKDIR /src/Services/Basket/Basket.UnitTests

FROM build as functionaltest
WORKDIR /src/Services/Basket/Basket.FunctionalTests

FROM build AS publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "Basket.API.dll"]
