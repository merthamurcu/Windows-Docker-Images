ARG VERSION="2025.2.15055"
ARG WINDOWS_VERSION=ltsc2022
FROM mcr.microsoft.com/windows/servercore:${WINDOWS_VERSION} as install
ARG VERSION
SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]
WORKDIR C:/seq
RUN iwr https://datalust.co/download/begin?version=${Env:VERSION} -o seq.msi
RUN Start-Process -FilePath msiexec.exe -Wait -ArgumentList @('/package', 'seq.msi', '/quiet', '/log', 'seq.txt')

EXPOSE 5341
ENTRYPOINT ["C:\\Program Files\\Seq\\seq.exe", "run", "--storage=C:\\seq\\data", "--listen=http://0.0.0.0:5341"]
