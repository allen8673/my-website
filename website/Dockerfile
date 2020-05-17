FROM mcr.microsoft.com/dotnet/core/aspnet:2.1 AS base
WORKDIR /app
# EXPOSE 5000
# EXPOSE 443
ENV ASPNETCORE_URLS=http://*:5000

FROM mcr.microsoft.com/dotnet/core/sdk:2.1 AS build
WORKDIR /src
COPY ["my-website.csproj", "./"]
RUN dotnet restore "./my-website.csproj"
COPY . .
WORKDIR "/src/."
RUN apt-get update -yq && apt-get upgrade -yq && apt-get install -yq curl git nano
RUN curl -sL https://deb.nodesource.com/setup_8.x | bash - && apt-get install -yq nodejs build-essential
RUN npm install -g npm
RUN npm install
RUN dotnet build "my-website.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "my-website.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
EXPOSE 5000
ENTRYPOINT ["dotnet", "my-website.dll"]
