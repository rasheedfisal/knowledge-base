Setup NGINX and ASP.NET Core on CentOS/RHEL for hosting sites on different ports and ASP.NET Core versions
===============
#### Prerequisites
Update the installed packages.

    yum update
Install the EPEL repository.

    # CentOS/RHEL 6
    rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-6.noarch.rpm
    # CentOS/RHEL 7
    rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
    yum install epel-release
Install the Linux Software Repository for Microsoft Products.

    # CentOS/RHEL 6
    sudo rpm -Uvh https://packages.microsoft.com/config/rhel/6/packages-microsoft-prod.rpm
    # CentOS/RHEL 7
    sudo rpm -Uvh https://packages.microsoft.com/config/rhel/7/packages-microsoft-prod.rpm
Install the REMI repository.

    # CentOS/RHEL 6
    rpm -Uvh https://rpms.remirepo.net/enterprise/remi-release-6.rpm
    # CentOS/RHEL 7
    rpm -Uvh https://rpms.remirepo.net/enterprise/remi-release-7.rpm

Check that the repositories are correctly installed.

    yum repolist
***
#### Step 1 - Install NGINX

    yum install nginx
***
#### Step 2 - Install the ASP.NET Core prerequisites

    yum install krb5-libs libcurl libicu libunwind libuuid lttng-ust zlib
***
#### Step 3 - Setup the .NET SDK
Use the following commands on CentOS.

    # ASP.NET Core 2.0
    sudo yum install dotnet-sdk-2.1.202
    # ASP.NET Core 2.1
    sudo yum install dotnet-sdk-2.1
Use the following commands on RHEL.

    # ASP.NET Core 2.0
    yum install rh-dotnet20 -y
    scl enable rh-dotnet20 bash
    # ASP.NET Core 2.1
    yum install rh-dotnet21 -y
    scl enable rh-dotnet21 bash
***
#### Step 4 - Check the dotnet versions

    dotnet --info
***
#### Step 5 - Configure the sites
Change directory to the default web directory.

    # CentOS/RHEL 6
    cd /usr/share/nginx/www
    # CentOS/RHEL 7
    cd /usr/share/nginx/html
Create the folders each site.

    mkdir dotnet21-example dotnet20-example
Publish and deploy the ASP.NET Core 2.0 and 2.1 projects on their respective folders.

    cd dotnet20-example
    cd dotnet21-example
***
#### Step 6 - Setup the server blocks
Two server blocks will be created, one for each site, on ports 83 and 84 respectively.

    cd /etc/nginx/conf.d
    vim dotnet20-example.conf
    vim dotnet21-example.conf
*dotnet20-example.conf*

    server {
        listen 83;
        server_name _;
        root /usr/share/nginx/html/dotnet20-example;
        location / {
            proxy_cache_bypass $http_upgrade;
            proxy_http_version 1.1;
            proxy_pass http://localhost:5000;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection keep-alive;
            proxy_set_header Host $http_host;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
    }
*dotnet21-example.conf*

    server {
        listen 84;
        server_name _;
        root /usr/share/nginx/html/dotnet21-example;
        location / {
            proxy_cache_bypass $http_upgrade;
            proxy_http_version 1.1;
            proxy_pass http://localhost:5001;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection keep-alive;
            proxy_set_header Host $http_host;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
    }
***
#### Step 7 - Check the NGINX configuration
    nginx -t
***
#### Step 8 - Create the service definition files
Two services will be created, one for each site.

    cd /etc/systemd/system
    vim kestrel-dotnet20-example.conf
    vim kestrel-dotnet21-example.conf
*kestrel-dotnet20-example.service*

    [Unit]
    Description=dotnet20-example

    [Service]
    WorkingDirectory=/usr/share/nginx/html/dotnet20-example
    ExecStart=/usr/bin/dotnet /usr/share/nginx/html/dotnet20-example/dotnet20-example.dll
    Restart=always
    # Restart service after 10 seconds if the dotnet service crashes:
    RestartSec=10
    KillSignal=SIGINT
    SyslogIdentifier=dotnet20-example
    User=nginx
    Environment=ASPNETCORE_ENVIRONMENT=Production
    Environment=ASPNETCORE_URLS=http://localhost:5000
    Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

    [Install]
    WantedBy=multi-user.target
*kestrel-dotnet21-example.service*

    [Unit]
    Description=dotnet21-example

    [Service]
    WorkingDirectory=/usr/share/nginx/html/dotnet21-example
    ExecStart=/usr/bin/dotnet /usr/share/nginx/html/dotnet21-example/dotnet21-example.dll
    Restart=always
    # Restart service after 10 seconds if the dotnet service crashes:
    RestartSec=10
    KillSignal=SIGINT
    SyslogIdentifier=dotnet21-example
    User=nginx
    Environment=ASPNETCORE_ENVIRONMENT=Production
    Environment=ASPNETCORE_URLS=http://localhost:5001
    Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

    [Install]
    WantedBy=multi-user.target
***
#### Step 9 - Configure SELinux
Add the site ports to the whitelist

    # Server
    semanage port -a -t http_port_t -p tcp 83
    semanage port -a -t http_port_t -p tcp 84
    # Socket
    semanage port -m -t http_port_t -p tcp 5000
    semanage port -m -t http_port_t -p tcp 5001
Check the port whitelist

    semanage port -l | grep http_port_t
***
#### Step 10 - Restart NGINX
    systemctl enable nginx
    systemctl restart nginx
***
#### Step 11 - Restart the custom services
    systemctl enable kestrel-dotnet20-example.service kestrel-dotnet21-example.service
    systemctl restart kestrel-dotnet20-example.service kestrel-dotnet21-example.service
***
#### Step 12 - Setup the firewall
    sudo firewall-cmd --permanent --add-port=83/tcp
    sudo firewall-cmd --permanent --add-port=84/tcp
    sudo firewall-cmd --reload
***
#### Troubleshooting
- Better upstream access log format
    - https://stackoverflow.com/a/26299679
- Typical log locations
    - */var/log/nginx/error.log*
***
#### References
- https://blog.tekspace.io/hosting-asp-net-core-2-1-application-on-centos-7-with-nginx/
- https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/linux-nginx?view=aspnetcore-2.2
- https://docs.microsoft.com/en-us/dotnet/core/linux-prerequisites?tabs=netcore2x
- https://docs.microsoft.com/en-us/dotnet/core/versions/selection
- https://dotnet.microsoft.com/download/linux-package-manager/centos/sdk-2.1.202