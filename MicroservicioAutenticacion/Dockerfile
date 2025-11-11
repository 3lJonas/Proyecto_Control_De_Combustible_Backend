# Consulte https://aka.ms/customizecontainer para aprender a personalizar su contenedor de depuraciÃ³n y cÃ³mo Visual Studio usa este Dockerfile para compilar sus imÃ¡genes para una depuraciÃ³n mÃ¡s rÃ¡pida.

# Esta fase se usa cuando se ejecuta desde VS en modo rÃ¡pido (valor predeterminado para la configuraciÃ³n de depuraciÃ³n)
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081


# Esta fase se usa para compilar el proyecto de servicio
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["MicroservicioAutenticacion.csproj", "."]
RUN dotnet restore "./MicroservicioAutenticacion.csproj"
COPY . .
WORKDIR "/src/."
RUN dotnet build "./MicroservicioAutenticacion.csproj" -c $BUILD_CONFIGURATION -o /app/build

# Esta fase se usa para publicar el proyecto de servicio que se copiarÃ¡ en la fase final.
FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./MicroservicioAutenticacion.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

# Esta fase se usa en producciÃ³n o cuando se ejecuta desde VS en modo normal (valor predeterminado cuando no se usa la configuraciÃ³n de depuraciÃ³n)
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "MicroservicioAutenticacion.dll"]

COPY certs certs
