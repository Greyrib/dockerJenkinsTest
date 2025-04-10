FROM mcr.microsoft.com/dotnet/sdk:8.0 as build
WORKDIR /app

COPY Blazor WASM Test App.sln ./
COPY Blazor WASM Test App.csproj ./

RUN dotnet restore
COPY . ./
RUN dotnet publish -c Release -o MYRELEASEFOLDER

FROM nginx:1.23.0-alpine
WORKDIR /app
EXPOSE 8080
COPY nginx.conf /etc/nginx/nginx.conf
COPY --from=build /app/MYRELEASEFOLDER/wwwroot /usr/share/nginx/html