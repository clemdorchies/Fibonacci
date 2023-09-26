FROM mcr.microsoft.com/dotnet/sdk:8.0-preview-alpine AS build
WORKDIR /src
COPY . .
RUN apk add clang gcc lld musl-dev build-base zlib-dev krb5-dev
RUN dotnet publish "./src/Leonardo.Web/Leonardo.Web.csproj" -c Release -r linux-musl-arm64 /p:PublishTrimmed=true /p:PublishReadyToRun=true -o /publish
RUN echo $(ls)
RUN ls -alh
RUN ls -alh /publish

FROM docker.io/library/alpine:latest AS base
WORKDIR /app
COPY --from=build /publish .
RUN echo $(ls)
RUN ls -alh
RUN ls -alh /app
ENV DOTNET_SYSTEM_GLOBALIZATION_INVARIANT=1
ENTRYPOINT ["/app/Leonardo.Web"]