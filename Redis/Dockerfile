#escape=`
FROM mcr.microsoft.com/windows/servercore:ltsc2022 as download
ARG redisVersion="3.2.100"
SHELL ["powershell", "-Command"]
WORKDIR C:/Redis

RUN Invoke-WebRequest -Uri https://github.com/MSOpenTech/redis/releases/download/win-${env:redisVersion}/Redis-x64-${env:redisVersion}.zip -OutFile Redis.zip ; `
    Expand-Archive Redis.zip -DestinationPath . ; `
    Remove-Item Redis.zip -Force

# Setup redis for unprotected mode to allow connections from outside
RUN Get-Content redis.windows.conf | Where { $_ -notmatch 'bind 127.0.0.1' } | Set-Content redis.openport.conf ; `
    Get-Content redis.openport.conf | Where { $_ -notmatch 'protected-mode yes' } | Set-Content redis.unprotected.conf ; `
    Add-Content redis.unprotected.conf 'protected-mode no' ; `
    Add-Content redis.unprotected.conf 'bind 0.0.0.0' ; `
    Get-Content redis.unprotected.conf

FROM mcr.microsoft.com/windows/nanoserver:ltsc2022 as final
WORKDIR C:/Redis
COPY --from=download C:/Redis .

EXPOSE 6379
ENTRYPOINT .\redis-server.exe .\redis.unprotected.conf
