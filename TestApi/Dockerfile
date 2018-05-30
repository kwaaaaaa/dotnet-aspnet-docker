FROM microsoft/aspnetcore:2.0 AS base
WORKDIR /app
EXPOSE 80

# Copy csproj and restore as distinct layers
FROM microsoft/aspnetcore-build:2.0 AS build
WORKDIR /src
COPY TestApi/TestApi.csproj TestApi/
RUN dotnet restore TestApi/TestApi.csproj

# Copy everything else and build
COPY . .
WORKDIR /src/TestApi
RUN dotnet build TestApi.csproj -c Release -o /app

# Build runtime image
FROM build AS publish
RUN dotnet publish TestApi.csproj -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "TestApi.dll"]
