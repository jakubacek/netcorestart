#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["Web.Mvc/Web.Mvc.csproj", "Web.Mvc/"]
RUN dotnet restore "Web.Mvc/Web.Mvc.csproj"
COPY . .
WORKDIR "/src/Web.Mvc"
RUN dotnet build "Web.Mvc.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Web.Mvc.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Web.Mvc.dll"]