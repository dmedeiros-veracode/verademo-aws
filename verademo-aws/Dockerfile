#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["verademo-aws/verademo-aws.csproj", "verademo-aws/"]
RUN dotnet restore "verademo-aws/verademo-aws.csproj"
COPY . .
WORKDIR "/src/verademo-aws"
RUN dotnet build "verademo-aws.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "verademo-aws.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "verademo-aws.dll"]