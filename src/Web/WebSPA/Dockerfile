ARG NODE_IMAGE=node:8.11
FROM mcr.microsoft.com/dotnet/core/aspnet AS base
WORKDIR /app
EXPOSE 80

FROM ${NODE_IMAGE} as node-build
WORKDIR /web
COPY Web/WebSPA/package.json .
COPY Web/WebSPA/package-lock.json .
RUN npm install
COPY Web/WebSPA .
RUN npm run build:prod

FROM mcr.microsoft.com/dotnet/core/sdk AS build
WORKDIR /src

COPY src/scripts scripts/

COPY src/ApiGateways/*/*/*.csproj csproj-files/
COPY src/BuildingBlocks/*/*/*.csproj csproj-files/
COPY src/Services/*/*/*.csproj csproj-files/
COPY src/Web/*/*.csproj csproj-files/

COPY . .
COPY --from=node-build /web/wwwroot /src/Web/WebSPA/wwwroot/
WORKDIR /src/Web/WebSPA
RUN dotnet publish -c Release -o /app

FROM build AS publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "WebSPA.dll"]
