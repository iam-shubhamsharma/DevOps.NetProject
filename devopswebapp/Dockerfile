FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

ENV ASPNETCORE_URLS=http://+:80

FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
#ARG configuration="Release"
WORKDIR /src
COPY ["devopswebapp.csproj", "./"]
RUN dotnet restore "devopswebapp.csproj"
COPY . .
WORKDIR "/src/."
RUN dotnet build "devopswebapp.csproj" -c Release -o /app/build

FROM build AS publish
#ARG configuration=Release
RUN dotnet publish "devopswebapp.csproj" -c Release -o /app/publish /apps/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "devopswebapp.dll"]
