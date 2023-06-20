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

RUN git clone -b docker https://github.com/yordanlp/LoadBalancer.git

WORKDIR "/src/LoadBalancer"
RUN dotnet restore

RUN dotnet build "./LoadBalancer/LoadBalancer.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "./LoadBalancer/LoadBalancer.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "LoadBalancer.dll"]
