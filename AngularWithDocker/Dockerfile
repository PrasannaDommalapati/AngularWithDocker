#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:3.1 AS base
# install NodeJS 13.x
RUN apt-get update -yq 
RUN apt-get install dialog apt-utils -y curl gnupg -yq 
RUN curl -sL https://deb.nodesource.com/setup_14.x | bash -
RUN apt-get install -y nodejs
RUN npm install @angular/cli -g
#end of nodejs set up
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:3.1 AS build
# install NodeJS 13.x
RUN apt-get update -yq 
RUN apt-get install -y curl gnupg -yq 
RUN curl -sL https://deb.nodesource.com/setup_14.x | bash -
RUN apt-get install -y nodejs
#end of nodejs set up
WORKDIR /src
COPY ["AngularWithDocker/AngularWithDocker.csproj", "AngularWithDocker/"]
RUN dotnet restore "AngularWithDocker/AngularWithDocker.csproj"
WORKDIR "/src/AngularWithDocker"
COPY . .
RUN dotnet build "AngularWithDocker.csproj" -c Release -o /app

FROM build AS publish
RUN dotnet publish "AngularWithDocker.csproj" -c Release -o /app --no-restore

FROM base AS final
WORKDIR /app
COPY --from=publish /app .

COPY **/AngularWithDocker/ClientApp/ ClientApp/

WORKDIR /app/ClientApp
RUN npm install -y
RUN npm update
RUN npm update --all --force
RUN npm -v && node -v && ng version
WORKDIR /app
EXPOSE 80
EXPOSE 443

ENTRYPOINT ["dotnet", "AngularWithDocker.dll"]
