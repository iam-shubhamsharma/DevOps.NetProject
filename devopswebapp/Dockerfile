FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

ENV ASPNETCORE_URLS=http://+:80

FROM --platform=linux mcr.microsoft.com/dotnet/sdk:9.0 AS build
ARG configuration="Release"
WORKDIR /src
COPY ["devopswebapp.csproj", "devopswebapp/"]
RUN dotnet restore "devopswebapp/devopswebapp.csproj"
COPY . .
WORKDIR "/src/devopswebapp"
RUN dotnet build "devopswebapp.csproj" -c $configuration -o /app/build

FROM build AS publish
ARG configuration=Release
RUN dotnet publish "devopswebapp.csproj" -c $configuration -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "devopswebapp.dll"]
