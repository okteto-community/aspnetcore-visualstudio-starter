FROM okteto/dotnetcore:8 AS dev

## The Visual Studio debugger needs this folder created
RUN mkdir /root/.vs-debugger

## Needed for Visual Studio Debugger to work
RUN apt-get update && apt-get install -y --no-install-recommends \
    gdb \
    procps \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /src

COPY *.csproj ./
RUN dotnet restore

COPY . ./
RUN dotnet build -c Release -o /app
RUN dotnet publish  -c Release -o /app


####################################

FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS prod

WORKDIR /app
COPY --from=dev /app .
EXPOSE 5000
ENTRYPOINT ["dotnet", "helloworld.dll"]
