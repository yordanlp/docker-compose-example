# Base ASP.NET Core runtime
FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

# SDK for building the project
FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /src

RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y git

RUN git clone -b features/docker https://github.com/yordanlp/CoffeeShopApi.git

WORKDIR "/src/CoffeeShopApi"
RUN dotnet restore

RUN dotnet build "./CoffeeShopApi/CoffeeShopApi.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "./CoffeeShopApi/CoffeeShopApi.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "CoffeeShopApi.dll"]
