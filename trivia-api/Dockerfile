#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["trivia-api.csproj", ""]
RUN dotnet restore "trivia-api.csproj"
RUN dotnet user-secrets set Azure:SignalRConnectionString "Endpoint=https://rortzxzsybf4m.service.signalr.net;AccessKey=P1UHZA1XOBGgPEmdOdzY8OlMS6RxgM/jcLjuSx/UmDg=;Version=1.0;"

COPY . .
WORKDIR "/src"
RUN dotnet build "trivia-api.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "trivia-api.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "trivia-api.dll"]