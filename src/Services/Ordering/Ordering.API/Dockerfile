FROM mcr.microsoft.com/dotnet/core/aspnet AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/core/sdk AS build
WORKDIR /src

COPY scripts scripts/

COPY Common/*/*.csproj csproj-files/
COPY BuildingBlocks/*/*/*.csproj csproj-files/
COPY Services/*/*/*.csproj csproj-files/
COPY Web/*/*.csproj csproj-files/

COPY . .
WORKDIR /src/Services/Ordering/Ordering.API
RUN dotnet publish -c Release -o /app

FROM build as unittest
WORKDIR /src/Services/Ordering/Ordering.UnitTests

FROM build as functionaltest
WORKDIR /src/Services/Ordering/Ordering.FunctionalTests

FROM build AS publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "Ordering.API.dll"]
