#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:5.0-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:5.0-buster-slim AS build
WORKDIR /src
COPY ["src/services/VirtualStore.Identidade.API/VirtualStore.Identidade.API.csproj", "src/services/VirtualStore.Identidade.API/"]
RUN dotnet restore "src/services/VirtualStore.Identidade.API/VirtualStore.Identidade.API.csproj"
COPY . .
WORKDIR "/src/src/services/VirtualStore.Identidade.API"
RUN dotnet build "VirtualStore.Identidade.API.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "VirtualStore.Identidade.API.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "VirtualStore.Identidade.API.dll"]