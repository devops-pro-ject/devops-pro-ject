FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 5033

ENV ASPNETCORE_URLS=http://*:5033

USER app
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG configuration=Release
WORKDIR /src
COPY ["Newapp.csproj", "./"]
RUN dotnet restore "Newapp.csproj"
COPY . .
WORKDIR "/src/."
RUN dotnet build "Newapp.csproj" -c $configuration -o /app/build

FROM build AS publish
ARG configuration=Release
RUN dotnet publish "Newapp.csproj" -c $configuration -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Newapp.dll"]
