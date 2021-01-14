#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:5.0-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:5.0-buster-slim AS build
WORKDIR /src
COPY ["src/web/VirtualStore.WebApp.MVC/VirtualStore.WebApp.MVC.csproj", "src/web/VirtualStore.WebApp.MVC/"]
RUN dotnet restore "src/web/VirtualStore.WebApp.MVC/VirtualStore.WebApp.MVC.csproj"
COPY . .
WORKDIR "/src/src/web/VirtualStore.WebApp.MVC"
RUN dotnet build "VirtualStore.WebApp.MVC.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "VirtualStore.WebApp.MVC.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "VirtualStore.WebApp.MVC.dll"]