#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["Services/ResAPI/Reservation.API/Reservation.API.csproj", "Services/ResAPI/Reservation.API/"]
COPY ["Services/ResAPI/Reservatiron.API.Services/Reservation.API.Services.csproj", "Services/ResAPI/Reservatiron.API.Services/"]
COPY ["Services/ResAPI/Reservation.API.Infrastructure/Reservation.API.Infrastructure.csproj", "Services/ResAPI/Reservation.API.Infrastructure/"]
COPY ["Services/ResAPI/Reservation.API.Models/Reservation.API.Models.csproj", "Services/ResAPI/Reservation.API.Models/"]
RUN dotnet restore "Services/ResAPI/Reservation.API/Reservation.API.csproj"
COPY . .
WORKDIR "/src/Services/ResAPI/Reservation.API"
RUN dotnet build "Reservation.API.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Reservation.API.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Reservation.API.dll"]