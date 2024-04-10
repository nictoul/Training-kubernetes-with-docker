# -- Build Image --
FROM mcr.microsoft.com/dotnet/sdk:8.0-bookworm-slim AS build
ARG build_configuration=Release
ARG build_version=1.0.0

WORKDIR /src

# Copy the project files, regardless of project structure
COPY /*.csproj ./
RUN for file in $(ls *.csproj); do mkdir -p ${file%.*}/ && mv "$file" ${file%.*}/; done

# NuGet restore
RUN dotnet restore "WebApplication2\WebApplication2.csproj"
# Copy all source code
COPY . .

# Build the app
#
RUN dotnet publish "WebApplication2.csproj" \
    -c ${build_configuration} \
    -o /app/publish \
    # -p:Version=${build_version} \
    -p:UseAppHost=false

# -- Final Image --
FROM mcr.microsoft.com/dotnet/aspnet:8.0-alpine3.18

# Set env vars for new relic and our service
ENV ASPNETCORE_URLS=http://+:8080
EXPOSE 8080

WORKDIR /app
COPY --from=build /app/publish .

ENTRYPOINT ["dotnet", "WebApplication2.dll"]