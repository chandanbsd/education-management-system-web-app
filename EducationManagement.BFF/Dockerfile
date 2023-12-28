FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["EducationManagement.BFF/EducationManagement.BFF.csproj", "EducationManagement.BFF/"]
RUN dotnet restore "EducationManagement.BFF/EducationManagement.BFF.csproj"
COPY . .
WORKDIR "/src/EducationManagement.BFF"
RUN dotnet build "EducationManagement.BFF.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "EducationManagement.BFF.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "EducationManagement.BFF.dll"]
