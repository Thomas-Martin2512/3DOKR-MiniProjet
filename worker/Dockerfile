FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src

COPY Worker.csproj .
RUN dotnet restore

COPY . .
RUN dotnet publish -c Release -o /app/publish --no-restore

FROM mcr.microsoft.com/dotnet/runtime:7.0 AS runtime
WORKDIR /app

RUN useradd --system --no-create-home appuser

COPY --from=build /app/publish .
USER appuser

HEALTHCHECK --interval=15s --timeout=3s --start-period=30s --retries=3 \
    CMD find /tmp/worker_alive -mmin -1 | grep -q . || exit 1

ENTRYPOINT ["dotnet", "Worker.dll"]