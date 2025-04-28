FROM okteto/dotnetcore:8 AS base

## Needed for Visual Studio Debugger to work
RUN apt-get update && apt-get install -y --no-install-recommends \
    unzip \
    gdb \
    procps \
    && rm -rf /var/lib/apt/lists/*

# Upload the debugger and place it on the /root/.vs-debugger folder
COPY .vs-debugger /tmp/vs-debugger
RUN unzip -d /tmp/vs-debugger/vs2022 /tmp/vs-debugger/vs2022.zip || true && \
    rm /tmp/vs-debugger/vs2022.zip && \
    mv /tmp/vs-debugger /root/.vs-debugger && \
    ls -l /root/.vs-debugger && \
    chmod +x /root/.vs-debugger/vs2022/vsdbg

# Tells Visual Studio to not download the debugger
ENV VSDEBUGGER_SKIP_INSTALL=1

FROM base AS dev

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
