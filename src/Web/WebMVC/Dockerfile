FROM mcr.microsoft.com/dotnet/core/aspnet as base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/core/sdk as build
WORKDIR /src

COPY scripts scripts/

COPY Common/*/*.csproj csproj-files/
COPY BuildingBlocks/*/*/*.csproj csproj-files/
COPY Services/*/*/*.csproj csproj-files/
COPY Web/*/*.csproj csproj-files/

COPY . .
WORKDIR /src/Web/WebMVC
RUN dotnet publish -c Release -o /app

FROM build AS publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "WebMVC.dll"]
