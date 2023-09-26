#FROM mcr.microsoft.com/dotnet/sdk:8.0.100-preview.7.23376.3 AS build
#WORKDIR /src
#COPY . .
#RUN dotnet publish "Demo.csproj" -c Release -r linux-arm64 --self-contained=true /p:PublishSingleFile=true /p:PublishTrimmed=true /p:PublishReadyToRun=true -o /publish

#FROM mcr.microsoft.com/dotnet/runtime-deps:8.0.100-preview.7.23376.3 AS final
#WORKDIR /app
#COPY --from=build /publish .
#ENTRYPOINT ["/app/Demo"]

FROM mcr.microsoft.com/dotnet/sdk:8.0-preview-alpine AS build
WORKDIR /src
COPY . .
RUN apk add clang gcc lld musl-dev build-base zlib-dev krb5-dev
RUN dotnet publish "./src/Leonardo.Web/Leonardo.Web.csproj" -c Release -r linux-arm64 /p:PublishTrimmed=true /p:PublishReadyToRun=true -o /publish

FROM docker.io/library/alpine:latest AS base
WORKDIR /app
COPY --from=build /publish .
ENV DOTNET_SYSTEM_GLOBALIZATION_INVARIANT=1
ENTRYPOINT ["/app/Leonardo.Web"]