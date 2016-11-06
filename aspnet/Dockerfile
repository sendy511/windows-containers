# Sample Dockerfile

# Indicates that the windowsservercore image will be used as the base image.
FROM yansendou/iis

# Metadata indicating an image maintainer.
MAINTAINER yansen@test.com

# Install .NET 4.5
RUN powershell add-windowsfeature web-asp-net45 

# Copy aspnet application
RUN powershell -Command \
    $ErrorActionPreference = 'Stop'; \
	Invoke-WebRequest -Method Get -Uri https://github.com/sendy511/aspnet/archive/master.zip -OutFile c:\aspnet.zip ; \
	Expand-Archive -Path c:\aspnet.zip -DestinationPath c:\aspnet; \
	Copy-Item -Recurse -Force c:\aspnet\aspnet-master\aspnet c:\inetpub\wwwroot\aspnet; \
	Remove-Item c:\aspnet.zip -Force; \
	Remove-Item -Recurse c:\aspnet -Force

# Configure Web App
RUN icacls C:\inetpub\wwwroot\aspnet /grant Everyone:F /t /q
RUN powershell -Command \
	Import-Module IISAdministration; \
        Import-Module WebAdministration; \
	New-Item 'IIS:\Sites\Default Web Site\aspnet' -Type Application -PhysicalPath 'C:\inetpub\wwwroot\aspnet'

# Overwrite Web.config file so that web service points to db service#
#RUN powershell COPY Web.config C:/inetpub/wwwroot/aspnet
