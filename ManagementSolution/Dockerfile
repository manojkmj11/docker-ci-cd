FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["ManagementSolution/ManagementDNApi/ManagementDNApi.csproj", "ManagementDNApi/"]
COPY ["ManagementSolution/ManagementBusinessLogicLayer/ManagementBusinessLogicLayer.csproj", "ManagementBusinessLogicLayer/"]
COPY ["ManagementSolution/ManagementDataLayer/ManagementDataLayer.csproj", "ManagementDataLayer/"]
COPY ["ManagementSolution/ManagementNurseModels/ManagementNurseModels.csproj", "ManagementNurseModels/"]
RUN dotnet restore "ManagementDNApi/ManagementDNApi.csproj"
COPY . .
WORKDIR "/src/ManagementSolution/ManagementDNApi"
RUN dotnet build "ManagementSolution/ManagementDNApi.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "ManagementSolution/ManagementDNApi.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "ManagementDNApi.dll"]