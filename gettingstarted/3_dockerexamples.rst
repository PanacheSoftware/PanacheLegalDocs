Docker Compose File Examples
============================

The following are examples of complete docker compose files.

Windows with MySQL 
^^^^^^^^^^^^^^^^^^

The following will provide a Panache Legal Platform environment with a MySQL database running in a Linux Docker container::

    version: "3.4"
    services:
        
    sqldata:
        image: mysql:latest
        environment:
        - MYSQL_ROOT_PASSWORD=Passw0rd123!
        volumes:
        - panachesoftware-sqldata:/var/opt/mssql
        
    panachesoftware.identity:
        image: panachesoftware/panachesoftwareidentity:latest
        depends_on:
        - sqldata
        environment:
        - ASPNETCORE_ENVIRONMENT=Development
        - ASPNETCORE_URLS=http://+:80
        - ConnectionStrings__MySQL=server=sqldata;port=3306;database=PanacheSoftware.Identity;user=root;password=Passw0rd123!;GuidFormat=Char36
        - PanacheSoftware__CallMethod__APICallsSecure=False
        - PanacheSoftware__CallMethod__UICallsSecure=False
        - PanacheSoftware__CallMethod__UseAPIGateway=False
        - PanacheSoftware__StartDomain=panachesoftware.com
        - PanacheSoftware__DBProvider=MySQL
        - PanacheSoftware__Url__UIClientURL=http://host.docker.internal:55001
        - PanacheSoftware__Url__UIClientURLSecure=https://host.docker.internal:44301
        - PanacheSoftware__Secret__UIClientSecret=49C1A7E1-0C79-4A89-A3D6-A37998FB86B0
        - PanacheSoftware__Secret__APIGatewaySecret=DDDCB193-213C-43FB-967A-5A911D2EFC04
        - PanacheSoftware__Secret__ClientServiceSecret=1314EF18-40FA-4B16-83DF-B276FF0D92A9
        - PanacheSoftware__Secret__FileServiceSecret=839C649E-4FE3-410C-B43F-69C017A52676
        - PanacheSoftware__Secret__FoundationServiceSecret=70CD8BB9-5256-42CF-8B95-DD61C1051AD0
        - PanacheSoftware__Secret__TaskServiceSecret=AC654B02-E46B-4359-B908-87479CBE1CEB
        - PanacheSoftware__Secret__TeamServiceSecret=5C9BF545-3C20-4448-9EEC-6B3E745B671E
        ports:
        - "55002:80"
        restart: always

    panachesoftware.service.team:
        image: panachesoftware/panachesoftwareserviceteam:latest
        depends_on:
        - sqldata
        - panachesoftware.identity
        environment:
        - ASPNETCORE_ENVIRONMENT=Development
        - ASPNETCORE_URLS=http://+:80
        - ConnectionStrings__MySQL=server=sqldata;port=3306;database=PanacheSoftware.Team;user=root;password=Passw0rd123!;GuidFormat=Char36
        - PanacheSoftware__CallMethod__APICallsSecure=False
        - PanacheSoftware__CallMethod__UICallsSecure=False
        - PanacheSoftware__CallMethod__UseAPIGateway=False
        - PanacheSoftware__StartDomain=panachesoftware.com
        - PanacheSoftware__DBProvider=MySQL
        - PanacheSoftware__Url__IdentityServerURL=http://host.docker.internal:55002
        - PanacheSoftware__Url__IdentityServerURLSecure=https://host.docker.internal:44302
        - PanacheSoftware__Secret__TeamServiceSecret=5C9BF545-3C20-4448-9EEC-6B3E745B671E
        ports:
        - "55006:80"
        restart: always

    panachesoftware.service.task:
        image: panachesoftware/panachesoftwareservicetask:latest
        depends_on:
        - sqldata
        - panachesoftware.identity
        - panachesoftware.service.team
        environment:
        - ASPNETCORE_ENVIRONMENT=Development
        - ASPNETCORE_URLS=http://+:80
        - ConnectionStrings__MySQL=server=sqldata;port=3306;database=PanacheSoftware.Task;user=root;password=Passw0rd123!;GuidFormat=Char36
        - PanacheSoftware__CallMethod__APICallsSecure=False
        - PanacheSoftware__CallMethod__UICallsSecure=False
        - PanacheSoftware__CallMethod__UseAPIGateway=False
        - PanacheSoftware__StartDomain=panachesoftware.com
        - PanacheSoftware__DBProvider=MySQL
        - PanacheSoftware__Url__IdentityServerURL=http://host.docker.internal:55002
        - PanacheSoftware__Url__IdentityServerURLSecure=https://host.docker.internal:44302
        - PanacheSoftware__Url__TeamServiceURL=http://host.docker.internal:55006
        - PanacheSoftware__Url__TeamServiceURLSecure=https://host.docker.internal:44306
        - PanacheSoftware__Secret__TaskServiceSecret=AC654B02-E46B-4359-B908-87479CBE1CEB
        ports:
        - "55007:80"
        restart: always

    panachesoftware.service.foundation:
        image: panachesoftware/panachesoftwareservicefoundation:latest
        depends_on:
        - sqldata
        - panachesoftware.identity
        environment:
        - ASPNETCORE_ENVIRONMENT=Development
        - ASPNETCORE_URLS=http://+:80
        - ConnectionStrings__MySQL=server=sqldata;port=3306;database=PanacheSoftware.Foundation;user=root;password=Passw0rd123!;GuidFormat=Char36
        - PanacheSoftware__CallMethod__APICallsSecure=False
        - PanacheSoftware__CallMethod__UICallsSecure=False
        - PanacheSoftware__CallMethod__UseAPIGateway=False
        - PanacheSoftware__StartDomain=panachesoftware.com
        - PanacheSoftware__DBProvider=MySQL
        - PanacheSoftware__Url__IdentityServerURL=http://host.docker.internal:55002
        - PanacheSoftware__Url__IdentityServerURLSecure=https://host.docker.internal:44302
        - PanacheSoftware__Secret__FoundationServiceSecret=70CD8BB9-5256-42CF-8B95-DD61C1051AD0
        ports:
        - "55004:80"
        restart: always

    panachesoftware.service.file:
        image: panachesoftware/panachesoftwareservicefile:latest
        depends_on:
        - sqldata
        - panachesoftware.identity
        environment:
        - ASPNETCORE_ENVIRONMENT=Development
        - ASPNETCORE_URLS=http://+:80
        - ConnectionStrings__MySQL=server=sqldata;port=3306;database=PanacheSoftware.File;user=root;password=Passw0rd123!;GuidFormat=Char36
        - PanacheSoftware__CallMethod__APICallsSecure=False
        - PanacheSoftware__CallMethod__UICallsSecure=False
        - PanacheSoftware__CallMethod__UseAPIGateway=False
        - PanacheSoftware__StartDomain=panachesoftware.com
        - PanacheSoftware__DBProvider=MySQL
        - PanacheSoftware__Url__IdentityServerURL=http://host.docker.internal:55002
        - PanacheSoftware__Url__IdentityServerURLSecure=https://host.docker.internal:44302
        - PanacheSoftware__Secret__FileServiceSecret=839C649E-4FE3-410C-B43F-69C017A52676
        ports:
        - "55008:80"
        restart: always

    panachesoftware.service.client:
        image: panachesoftware/panachesoftwareserviceclient:latest
        depends_on:
        - sqldata
        - panachesoftware.identity
        environment:
        - ASPNETCORE_ENVIRONMENT=Development
        - ASPNETCORE_URLS=http://+:80
        - ConnectionStrings__MySQL=server=sqldata;port=3306;database=PanacheSoftware.Client;user=root;password=Passw0rd123!;GuidFormat=Char36
        - PanacheSoftware__CallMethod__APICallsSecure=False
        - PanacheSoftware__CallMethod__UICallsSecure=False
        - PanacheSoftware__CallMethod__UseAPIGateway=False
        - PanacheSoftware__StartDomain=panachesoftware.com
        - PanacheSoftware__DBProvider=MySQL
        - PanacheSoftware__Url__IdentityServerURL=http://host.docker.internal:55002
        - PanacheSoftware__Url__IdentityServerURLSecure=https://host.docker.internal:44302
        - PanacheSoftware__Secret__ClientServiceSecret=1314EF18-40FA-4B16-83DF-B276FF0D92A9
        ports:
        - "55005:80"
        restart: always

    panachesoftware.ui.client:
        image: panachesoftware/panachesoftwareuiclient:latest
        depends_on:
        - panachesoftware.identity
        - panachesoftware.service.team
        - panachesoftware.service.task
        - panachesoftware.service.foundation
        - panachesoftware.service.file
        - panachesoftware.service.client
        environment:
        - ASPNETCORE_ENVIRONMENT=Development
        - ASPNETCORE_URLS=http://+:80
        - PanacheSoftware__CallMethod__APICallsSecure=False
        - PanacheSoftware__CallMethod__UICallsSecure=False
        - PanacheSoftware__CallMethod__UseAPIGateway=False
        - PanacheSoftware__StartDomain=panachesoftware.com
        - PanacheSoftware__Url__IdentityServerURL=http://host.docker.internal:55002
        - PanacheSoftware__Url__IdentityServerURLSecure=https://host.docker.internal:44302
        - PanacheSoftware__Url__APIGatewayURL=http://host.docker.internal:55003
        - PanacheSoftware__Url__APIGatewayURLSecure=https://host.docker.internal:44303
        - PanacheSoftware__Url__UIClientURL=http://host.docker.internal:55001
        - PanacheSoftware__Url__UIClientURLSecure=https://host.docker.internal:44301
        - PanacheSoftware__Url__ClientServiceURL=http://host.docker.internal:55005
        - PanacheSoftware__Url__ClientServiceURLSecure=https://host.docker.internal:44305
        - PanacheSoftware__Url__FileServiceURL=http://host.docker.internal:55008
        - PanacheSoftware__Url__FileServiceURLSecure=https://host.docker.internal:44308
        - PanacheSoftware__Url__FoundationServiceURL=http://host.docker.internal:55004
        - PanacheSoftware__Url__FoundationServiceURLSecure=https://host.docker.internal:44304
        - PanacheSoftware__Url__TaskServiceURL=http://host.docker.internal:55007
        - PanacheSoftware__Url__TaskServiceURLSecure=https://host.docker.internal:44307
        - PanacheSoftware__Url__TeamServiceURL=http://host.docker.internal:55006
        - PanacheSoftware__Url__TeamServiceURLSecure=https://host.docker.internal:44306
        - PanacheSoftware__Secret__UIClientSecret=49C1A7E1-0C79-4A89-A3D6-A37998FB86B0
        ports:
        - "55001:80"
        restart: always

    volumes:
    panachesoftware-sqldata:
        external: false

Once all containers are running open a web browser and navigate to **http://host.docker.internal:55001** to open the Panache Legal Platform.  You can use the following details to login (assuming you did not change the **PanacheSoftware__StartDomain** environment variable to a different domain).

Username: admin@panachesoftware.com

Password: Passw0rd123!

Windows with MSSQL 
^^^^^^^^^^^^^^^^^^

The following will provide a Panache Legal Platform environment with a Microsoft SQL Server database running in a Linux Docker container::

    version: "3.4"
    services:
        
    sqldata:
        image: mcr.microsoft.com/mssql/server:2017-latest
        environment:
        - SA_PASSWORD=Passw0rd123!
        - ACCEPT_EULA=Y
        ports:
        - "5433:1433"
        volumes:
        - panachesoftware-sqldata:/var/opt/mssql
        
    panachesoftware.identity:
        image: panachesoftware/panachesoftwareidentity:latest
        depends_on:
        - sqldata
        environment:
        - ASPNETCORE_ENVIRONMENT=Development
        - ASPNETCORE_URLS=http://+:80
        - ConnectionStrings__MSSQL=Server=sqldata;Database=PanacheSoftware.Identity.Docker;User Id=sa;Password=Passw0rd123!
        - PanacheSoftware__CallMethod__APICallsSecure=False
        - PanacheSoftware__CallMethod__UICallsSecure=False
        - PanacheSoftware__CallMethod__UseAPIGateway=False
        - PanacheSoftware__StartDomain=panachesoftware.com
        - PanacheSoftware__DBProvider=MSSQL
        - PanacheSoftware__Url__UIClientURL=http://host.docker.internal:55001
        - PanacheSoftware__Url__UIClientURLSecure=https://host.docker.internal:44301
        - PanacheSoftware__Secret__UIClientSecret=49C1A7E1-0C79-4A89-A3D6-A37998FB86B0
        - PanacheSoftware__Secret__APIGatewaySecret=DDDCB193-213C-43FB-967A-5A911D2EFC04
        - PanacheSoftware__Secret__ClientServiceSecret=1314EF18-40FA-4B16-83DF-B276FF0D92A9
        - PanacheSoftware__Secret__FileServiceSecret=839C649E-4FE3-410C-B43F-69C017A52676
        - PanacheSoftware__Secret__FoundationServiceSecret=70CD8BB9-5256-42CF-8B95-DD61C1051AD0
        - PanacheSoftware__Secret__TaskServiceSecret=AC654B02-E46B-4359-B908-87479CBE1CEB
        - PanacheSoftware__Secret__TeamServiceSecret=5C9BF545-3C20-4448-9EEC-6B3E745B671E
        ports:
        - "55002:80"

    panachesoftware.service.team:
        image: panachesoftware/panachesoftwareserviceteam:latest
        depends_on:
        - sqldata
        - panachesoftware.identity
        environment:
        - ASPNETCORE_ENVIRONMENT=Development
        - ASPNETCORE_URLS=http://+:80
        - ConnectionStrings__MSSQL=Server=sqldata;Database=PanacheSoftware.Service.Team.Docker;User Id=sa;Password=Passw0rd123!
        - PanacheSoftware__CallMethod__APICallsSecure=False
        - PanacheSoftware__CallMethod__UICallsSecure=False
        - PanacheSoftware__CallMethod__UseAPIGateway=False
        - PanacheSoftware__StartDomain=panachesoftware.com
        - PanacheSoftware__DBProvider=MSSQL
        - PanacheSoftware__Url__IdentityServerURL=http://host.docker.internal:55002
        - PanacheSoftware__Url__IdentityServerURLSecure=https://host.docker.internal:44302
        - PanacheSoftware__Secret__TeamServiceSecret=5C9BF545-3C20-4448-9EEC-6B3E745B671E
        ports:
        - "55006:80"

    panachesoftware.service.task:
        image: panachesoftware/panachesoftwareservicetask:latest
        depends_on:
        - sqldata
        - panachesoftware.identity
        - panachesoftware.service.team
        environment:
        - ASPNETCORE_ENVIRONMENT=Development
        - ASPNETCORE_URLS=http://+:80
        - ConnectionStrings__MSSQL=Server=sqldata;Database=PanacheSoftware.Service.Task.Docker;User Id=sa;Password=Passw0rd123!
        - PanacheSoftware__CallMethod__APICallsSecure=False
        - PanacheSoftware__CallMethod__UICallsSecure=False
        - PanacheSoftware__CallMethod__UseAPIGateway=False
        - PanacheSoftware__StartDomain=panachesoftware.com
        - PanacheSoftware__DBProvider=MSSQL
        - PanacheSoftware__Url__IdentityServerURL=http://host.docker.internal:55002
        - PanacheSoftware__Url__IdentityServerURLSecure=https://host.docker.internal:44302
        - PanacheSoftware__Url__TeamServiceURL=http://host.docker.internal:55006
        - PanacheSoftware__Url__TeamServiceURLSecure=https://host.docker.internal:44306
        - PanacheSoftware__Secret__TaskServiceSecret=AC654B02-E46B-4359-B908-87479CBE1CEB
        ports:
        - "55007:80"

    panachesoftware.service.foundation:
        image: panachesoftware/panachesoftwareservicefoundation:latest
        depends_on:
        - sqldata
        - panachesoftware.identity
        environment:
        - ASPNETCORE_ENVIRONMENT=Development
        - ASPNETCORE_URLS=http://+:80
        - ConnectionStrings__MSSQL=Server=sqldata;Database=PanacheSoftware.Service.Foundation.Docker;User Id=sa;Password=Passw0rd123!
        - PanacheSoftware__CallMethod__APICallsSecure=False
        - PanacheSoftware__CallMethod__UICallsSecure=False
        - PanacheSoftware__CallMethod__UseAPIGateway=False
        - PanacheSoftware__StartDomain=panachesoftware.com
        - PanacheSoftware__DBProvider=MSSQL
        - PanacheSoftware__Url__IdentityServerURL=http://host.docker.internal:55002
        - PanacheSoftware__Url__IdentityServerURLSecure=https://host.docker.internal:44302
        - PanacheSoftware__Secret__FoundationServiceSecret=70CD8BB9-5256-42CF-8B95-DD61C1051AD0
        ports:
        - "55004:80"

    panachesoftware.service.file:
        image: panachesoftware/panachesoftwareservicefile:latest
        depends_on:
        - sqldata
        - panachesoftware.identity
        environment:
        - ASPNETCORE_ENVIRONMENT=Development
        - ASPNETCORE_URLS=http://+:80
        - ConnectionStrings__MSSQL=Server=sqldata;Database=PanacheSoftware.Service.File.Docker;User Id=sa;Password=Passw0rd123!
        - PanacheSoftware__CallMethod__APICallsSecure=False
        - PanacheSoftware__CallMethod__UICallsSecure=False
        - PanacheSoftware__CallMethod__UseAPIGateway=False
        - PanacheSoftware__StartDomain=panachesoftware.com
        - PanacheSoftware__DBProvider=MSSQL
        - PanacheSoftware__Url__IdentityServerURL=http://host.docker.internal:55002
        - PanacheSoftware__Url__IdentityServerURLSecure=https://host.docker.internal:44302
        - PanacheSoftware__Secret__FileServiceSecret=839C649E-4FE3-410C-B43F-69C017A52676
        ports:
        - "55008:80"

    panachesoftware.service.client:
        image: panachesoftware/panachesoftwareserviceclient:latest
        depends_on:
        - sqldata
        - panachesoftware.identity
        environment:
        - ASPNETCORE_ENVIRONMENT=Development
        - ASPNETCORE_URLS=http://+:80
        - ConnectionStrings__MSSQL=Server=sqldata;Database=PanacheSoftware.Service.Client.Docker;User Id=sa;Password=Passw0rd123!
        - PanacheSoftware__CallMethod__APICallsSecure=False
        - PanacheSoftware__CallMethod__UICallsSecure=False
        - PanacheSoftware__CallMethod__UseAPIGateway=False
        - PanacheSoftware__StartDomain=panachesoftware.com
        - PanacheSoftware__DBProvider=MSSQL
        - PanacheSoftware__Url__IdentityServerURL=http://host.docker.internal:55002
        - PanacheSoftware__Url__IdentityServerURLSecure=https://host.docker.internal:44302
        - PanacheSoftware__Secret__ClientServiceSecret=1314EF18-40FA-4B16-83DF-B276FF0D92A9
        ports:
        - "55005:80"

    panachesoftware.ui.client:
        image: panachesoftware/panachesoftwareuiclient:latest
        depends_on:
        - panachesoftware.identity
        - panachesoftware.service.team
        - panachesoftware.service.task
        - panachesoftware.service.foundation
        - panachesoftware.service.file
        - panachesoftware.service.client
        environment:
        - ASPNETCORE_ENVIRONMENT=Development
        - ASPNETCORE_URLS=http://+:80
        - PanacheSoftware__CallMethod__APICallsSecure=False
        - PanacheSoftware__CallMethod__UICallsSecure=False
        - PanacheSoftware__CallMethod__UseAPIGateway=False
        - PanacheSoftware__StartDomain=panachesoftware.com
        - PanacheSoftware__Url__IdentityServerURL=http://host.docker.internal:55002
        - PanacheSoftware__Url__IdentityServerURLSecure=https://host.docker.internal:44302
        - PanacheSoftware__Url__APIGatewayURL=http://host.docker.internal:55003
        - PanacheSoftware__Url__APIGatewayURLSecure=https://host.docker.internal:44303
        - PanacheSoftware__Url__UIClientURL=http://host.docker.internal:55001
        - PanacheSoftware__Url__UIClientURLSecure=https://host.docker.internal:44301
        - PanacheSoftware__Url__ClientServiceURL=http://host.docker.internal:55005
        - PanacheSoftware__Url__ClientServiceURLSecure=https://host.docker.internal:44305
        - PanacheSoftware__Url__FileServiceURL=http://host.docker.internal:55008
        - PanacheSoftware__Url__FileServiceURLSecure=https://host.docker.internal:44308
        - PanacheSoftware__Url__FoundationServiceURL=http://host.docker.internal:55004
        - PanacheSoftware__Url__FoundationServiceURLSecure=https://host.docker.internal:44304
        - PanacheSoftware__Url__TaskServiceURL=http://host.docker.internal:55007
        - PanacheSoftware__Url__TaskServiceURLSecure=https://host.docker.internal:44307
        - PanacheSoftware__Url__TeamServiceURL=http://host.docker.internal:55006
        - PanacheSoftware__Url__TeamServiceURLSecure=https://host.docker.internal:44306
        - PanacheSoftware__Secret__UIClientSecret=49C1A7E1-0C79-4A89-A3D6-A37998FB86B0
        ports:
        - "55001:80"

    volumes:
    panachesoftware-sqldata:
        external: false

Once all containers are running open a web browser and navigate to **http://host.docker.internal:55001** to open the Panache Legal Platform.  You can use the following details to login (assuming you did not change the **PanacheSoftware__StartDomain** environment variable to a different domain).

Username: admin@panachesoftware.com

Password: Passw0rd123!

Raspberry Pi with MySQL 
^^^^^^^^^^^^^^^^^^^^^^^

The following will provide a Panache Legal Platform environment running on a Raspberry Pi with a local MySQL database (Raspberry Pi has a local address of 192.168.86.247)::

    version: "3.4"
    services:

    panachesoftware.identity:
        image: panachesoftware/panachesoftwareidentity-arm32:latest
        environment:
        - ASPNETCORE_ENVIRONMENT=Development
        - ASPNETCORE_URLS=http://+:80      
        - ConnectionStrings__MySQL=server=192.168.86.247;port=3306;database=PanacheSoftware.Identity.Docker;user=pi;password=Passw0rd123!;GuidFormat=Char36
        - PanacheSoftware__CallMethod__APICallsSecure=False
        - PanacheSoftware__CallMethod__UICallsSecure=False
        - PanacheSoftware__CallMethod__UseAPIGateway=False
        - PanacheSoftware__DBProvider=MySQL
        - PanacheSoftware__StartDomain=panachesoftware.com
        - PanacheSoftware__Url__UIClientURL=http://172.17.0.1:55001
        - PanacheSoftware__Url__UIClientURLSecure=https://172.17.0.1:44301
        - PanacheSoftware__Secret__UIClientSecret=49C1A7E1-0C79-4A89-A3D6-A37998FB86B0
        - PanacheSoftware__Secret__APIGatewaySecret=DDDCB193-213C-43FB-967A-5A911D2EFC04
        - PanacheSoftware__Secret__ClientServiceSecret=1314EF18-40FA-4B16-83DF-B276FF0D92A9
        - PanacheSoftware__Secret__FileServiceSecret=839C649E-4FE3-410C-B43F-69C017A52676
        - PanacheSoftware__Secret__FoundationServiceSecret=70CD8BB9-5256-42CF-8B95-DD61C1051AD0
        - PanacheSoftware__Secret__TaskServiceSecret=AC654B02-E46B-4359-B908-87479CBE1CEB
        - PanacheSoftware__Secret__TeamServiceSecret=5C9BF545-3C20-4448-9EEC-6B3E745B671E
        ports:
        - "55002:80"
        restart: always

    panachesoftware.service.team:
        image: panachesoftware/panachesoftwareserviceteam-arm32:latest
        depends_on:
        - panachesoftware.identity
        environment:
        - ASPNETCORE_ENVIRONMENT=Development
        - ASPNETCORE_URLS=http://+:80
        - ConnectionStrings__MySQL=server=192.168.86.247;port=3306;database=PanacheSoftware.Team.Docker;user=pi;password=Passw0rd123!;GuidFormat=Char36
        - PanacheSoftware__CallMethod__APICallsSecure=False
        - PanacheSoftware__CallMethod__UICallsSecure=False
        - PanacheSoftware__CallMethod__UseAPIGateway=False
        - PanacheSoftware__DBProvider=MySQL
        - PanacheSoftware__StartDomain=panachesoftware.com
        - PanacheSoftware__Url__IdentityServerURL=http://172.17.0.1:55002
        - PanacheSoftware__Url__IdentityServerURLSecure=https://172.17.0.1:44302
        - PanacheSoftware__Secret__TeamServiceSecret=5C9BF545-3C20-4448-9EEC-6B3E745B671E
        ports:
        - "55006:80"
        restart: always

    panachesoftware.service.task:
        image: panachesoftware/panachesoftwareservicetask-arm32:latest
        depends_on:
        - panachesoftware.identity
        - panachesoftware.service.team
        environment:
        - ASPNETCORE_ENVIRONMENT=Development
        - ASPNETCORE_URLS=http://+:80
        - ConnectionStrings__MySQL=server=192.168.86.247;port=3306;database=PanacheSoftware.Task.Docker;user=pi;password=Passw0rd123!;GuidFormat=Char36
        - PanacheSoftware__CallMethod__APICallsSecure=False
        - PanacheSoftware__CallMethod__UICallsSecure=False
        - PanacheSoftware__CallMethod__UseAPIGateway=False
        - PanacheSoftware__DBProvider=MySQL
        - PanacheSoftware__StartDomain=panachesoftware.com
        - PanacheSoftware__Url__IdentityServerURL=http://172.17.0.1:55002
        - PanacheSoftware__Url__IdentityServerURLSecure=https://172.17.0.1:44302
        - PanacheSoftware__Url__TeamServiceURL=http://172.17.0.1:55006
        - PanacheSoftware__Url__TeamServiceURLSecure=https://172.17.0.1:44306
        - PanacheSoftware__Secret__TaskServiceSecret=AC654B02-E46B-4359-B908-87479CBE1CEB
        ports:
        - "55007:80"
        restart: always

    panachesoftware.service.foundation:
        image: panachesoftware/panachesoftwareservicefoundation-arm32:latest
        depends_on:
        - panachesoftware.identity
        environment:
        - ASPNETCORE_ENVIRONMENT=Development
        - ASPNETCORE_URLS=http://+:80
        - ConnectionStrings__MySQL=server=192.168.86.247;port=3306;database=PanacheSoftware.Foundation.Docker;user=pi;password=Passw0rd123!;GuidFormat=Char36
        - PanacheSoftware__CallMethod__APICallsSecure=False
        - PanacheSoftware__CallMethod__UICallsSecure=False
        - PanacheSoftware__CallMethod__UseAPIGateway=False
        - PanacheSoftware__DBProvider=MySQL
        - PanacheSoftware__StartDomain=panachesoftware.com
        - PanacheSoftware__Url__IdentityServerURL=http://172.17.0.1:55002
        - PanacheSoftware__Url__IdentityServerURLSecure=https://172.17.0.1:44302
        - PanacheSoftware__Secret__FoundationServiceSecret=70CD8BB9-5256-42CF-8B95-DD61C1051AD0
        ports:
        - "55004:80"
        restart: always

    panachesoftware.service.file:
        image: panachesoftware/panachesoftwareservicefile-arm32:latest
        depends_on:
        - panachesoftware.identity
        environment:
        - ASPNETCORE_ENVIRONMENT=Development
        - ASPNETCORE_URLS=http://+:80
        - ConnectionStrings__MySQL=server=192.168.86.247;port=3306;database=PanacheSoftware.File.Docker;user=pi;password=Passw0rd123!;GuidFormat=Char36
        - PanacheSoftware__CallMethod__APICallsSecure=False
        - PanacheSoftware__CallMethod__UICallsSecure=False
        - PanacheSoftware__CallMethod__UseAPIGateway=False
        - PanacheSoftware__DBProvider=MySQL
        - PanacheSoftware__StartDomain=panachesoftware.com
        - PanacheSoftware__Url__IdentityServerURL=http://172.17.0.1:55002
        - PanacheSoftware__Url__IdentityServerURLSecure=https://172.17.0.1:44302
        - PanacheSoftware__Secret__FileServiceSecret=839C649E-4FE3-410C-B43F-69C017A52676
        ports:
        - "55008:80"
        restart: always

    panachesoftware.service.client:
        image: panachesoftware/panachesoftwareserviceclient-arm32:latest
        depends_on:
        - panachesoftware.identity
        environment:
        - ASPNETCORE_ENVIRONMENT=Development
        - ASPNETCORE_URLS=http://+:80
        - ConnectionStrings__MySQL=server=192.168.86.247;port=3306;database=PanacheSoftware.Client.Docker;user=pi;password=Passw0rd123!;GuidFormat=Char36
        - PanacheSoftware__CallMethod__APICallsSecure=False
        - PanacheSoftware__CallMethod__UICallsSecure=False
        - PanacheSoftware__CallMethod__UseAPIGateway=False
        - PanacheSoftware__DBProvider=MySQL
        - PanacheSoftware__StartDomain=panachesoftware.com
        - PanacheSoftware__Url__IdentityServerURL=http://172.17.0.1:55002
        - PanacheSoftware__Url__IdentityServerURLSecure=https://172.17.0.1:44302
        - PanacheSoftware__Secret__ClientServiceSecret=1314EF18-40FA-4B16-83DF-B276FF0D92A9
        ports:
        - "55005:80"

    panachesoftware.ui.client:
        image: panachesoftware/panachesoftwareuiclient-arm32:latest
        depends_on:
        - panachesoftware.identity
        - panachesoftware.service.team
        - panachesoftware.service.task
        - panachesoftware.service.foundation
        - panachesoftware.service.file
        - panachesoftware.service.client
        environment:
        - ASPNETCORE_ENVIRONMENT=Development
        - ASPNETCORE_URLS=http://+:80
        - PanacheSoftware__CallMethod__APICallsSecure=False
        - PanacheSoftware__CallMethod__UICallsSecure=False
        - PanacheSoftware__CallMethod__UseAPIGateway=False
        - PanacheSoftware__StartDomain=panachesoftware.com
        - PanacheSoftware__Url__IdentityServerURL=http://172.17.0.1:55002
        - PanacheSoftware__Url__IdentityServerURLSecure=https://172.17.0.1:44302
        - PanacheSoftware__Url__APIGatewayURL=http://172.17.0.1:55003
        - PanacheSoftware__Url__APIGatewayURLSecure=https://172.17.0.1:44303
        - PanacheSoftware__Url__UIClientURL=http://172.17.0.1:55001
        - PanacheSoftware__Url__UIClientURLSecure=https://172.17.0.1:44301
        - PanacheSoftware__Url__ClientServiceURL=http://172.17.0.1:55005
        - PanacheSoftware__Url__ClientServiceURLSecure=https://172.17.0.1:44305
        - PanacheSoftware__Url__FileServiceURL=http://172.17.0.1:55008
        - PanacheSoftware__Url__FileServiceURLSecure=https://172.17.0.1:44308
        - PanacheSoftware__Url__FoundationServiceURL=http://172.17.0.1:55004
        - PanacheSoftware__Url__FoundationServiceURLSecure=https://172.17.0.1:44304
        - PanacheSoftware__Url__TaskServiceURL=http://172.17.0.1:55007
        - PanacheSoftware__Url__TaskServiceURLSecure=https://172.17.0.1:44307
        - PanacheSoftware__Url__TeamServiceURL=http://172.17.0.1:55006
        - PanacheSoftware__Url__TeamServiceURLSecure=https://172.17.0.1:44306
        - PanacheSoftware__Secret__UIClientSecret=49C1A7E1-0C79-4A89-A3D6-A37998FB86B0
        ports:
        - "55001:80"
        restart: always

Once all containers are running open a web browser on the Raspberry Pi and navigate to **http://172.17.0.1:55001** to open the Panache Legal Platform.  You can use the following details to login (assuming you did not change the **PanacheSoftware__StartDomain** environment variable to a different domain).

Username: admin@panachesoftware.com

Password: Passw0rd123!

Raspberry Pi with MySQL (remote access) 
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The following will provide a Panache Legal Platform environment running on a Raspberry Pi with a local MySQL database (Raspberry Pi has a local address of 192.168.86.247), the Docker containers will have there network ports exposed to on the host so you can access from another machine on the same network::

    version: "3.4"
    services:

    panachesoftware.identity:
        image: panachesoftware/panachesoftwareidentity-arm32:latest
        environment:
        - ASPNETCORE_ENVIRONMENT=Development
        - ASPNETCORE_URLS=http://+:80      
        - ConnectionStrings__MySQL=server=192.168.86.247;port=3306;database=PanacheSoftware.Identity.Docker;user=pi;password=Passw0rd123!;GuidFormat=Char36
        - PanacheSoftware__CallMethod__APICallsSecure=False
        - PanacheSoftware__CallMethod__UICallsSecure=False
        - PanacheSoftware__CallMethod__UseAPIGateway=False
        - PanacheSoftware__DBProvider=MySQL
        - PanacheSoftware__StartDomain=panachesoftware.com
        - PanacheSoftware__Url__UIClientURL=http://192.168.86.247:55001
        - PanacheSoftware__Url__UIClientURLSecure=https://192.168.86.247:44301
        - PanacheSoftware__Secret__UIClientSecret=49C1A7E1-0C79-4A89-A3D6-A37998FB86B0
        - PanacheSoftware__Secret__APIGatewaySecret=DDDCB193-213C-43FB-967A-5A911D2EFC04
        - PanacheSoftware__Secret__ClientServiceSecret=1314EF18-40FA-4B16-83DF-B276FF0D92A9
        - PanacheSoftware__Secret__FileServiceSecret=839C649E-4FE3-410C-B43F-69C017A52676
        - PanacheSoftware__Secret__FoundationServiceSecret=70CD8BB9-5256-42CF-8B95-DD61C1051AD0
        - PanacheSoftware__Secret__TaskServiceSecret=AC654B02-E46B-4359-B908-87479CBE1CEB
        - PanacheSoftware__Secret__TeamServiceSecret=5C9BF545-3C20-4448-9EEC-6B3E745B671E
        ports:
        - "192.168.86.247:55002:80"
        restart: always

    panachesoftware.service.team:
        image: panachesoftware/panachesoftwareserviceteam-arm32:latest
        depends_on:
        - panachesoftware.identity
        environment:
        - ASPNETCORE_ENVIRONMENT=Development
        - ASPNETCORE_URLS=http://+:80
        - ConnectionStrings__MySQL=server=192.168.86.247;port=3306;database=PanacheSoftware.Team.Docker;user=pi;password=Passw0rd123!;GuidFormat=Char36
        - PanacheSoftware__CallMethod__APICallsSecure=False
        - PanacheSoftware__CallMethod__UICallsSecure=False
        - PanacheSoftware__CallMethod__UseAPIGateway=False
        - PanacheSoftware__DBProvider=MySQL
        - PanacheSoftware__StartDomain=panachesoftware.com
        - PanacheSoftware__Url__IdentityServerURL=http://192.168.86.247:55002
        - PanacheSoftware__Url__IdentityServerURLSecure=https://192.168.86.247:44302
        - PanacheSoftware__Secret__TeamServiceSecret=5C9BF545-3C20-4448-9EEC-6B3E745B671E
        ports:
        - "192.168.86.247:55006:80"
        restart: always

    panachesoftware.service.task:
        image: panachesoftware/panachesoftwareservicetask-arm32:latest
        depends_on:
        - panachesoftware.identity
        - panachesoftware.service.team
        environment:
        - ASPNETCORE_ENVIRONMENT=Development
        - ASPNETCORE_URLS=http://+:80
        - ConnectionStrings__MySQL=server=192.168.86.247;port=3306;database=PanacheSoftware.Task.Docker;user=pi;password=Passw0rd123!;GuidFormat=Char36
        - PanacheSoftware__CallMethod__APICallsSecure=False
        - PanacheSoftware__CallMethod__UICallsSecure=False
        - PanacheSoftware__CallMethod__UseAPIGateway=False
        - PanacheSoftware__DBProvider=MySQL
        - PanacheSoftware__StartDomain=panachesoftware.com
        - PanacheSoftware__Url__IdentityServerURL=http://192.168.86.247:55002
        - PanacheSoftware__Url__IdentityServerURLSecure=https://192.168.86.247:44302
        - PanacheSoftware__Url__TeamServiceURL=http://192.168.86.247:55006
        - PanacheSoftware__Url__TeamServiceURLSecure=https://192.168.86.247:44306
        - PanacheSoftware__Secret__TaskServiceSecret=AC654B02-E46B-4359-B908-87479CBE1CEB
        ports:
        - "192.168.86.247:55007:80"
        restart: always

    panachesoftware.service.foundation:
        image: panachesoftware/panachesoftwareservicefoundation-arm32:latest
        depends_on:
        - panachesoftware.identity
        environment:
        - ASPNETCORE_ENVIRONMENT=Development
        - ASPNETCORE_URLS=http://+:80
        - ConnectionStrings__MySQL=server=192.168.86.247;port=3306;database=PanacheSoftware.Foundation.Docker;user=pi;password=Passw0rd123!;GuidFormat=Char36
        - PanacheSoftware__CallMethod__APICallsSecure=False
        - PanacheSoftware__CallMethod__UICallsSecure=False
        - PanacheSoftware__CallMethod__UseAPIGateway=False
        - PanacheSoftware__DBProvider=MySQL
        - PanacheSoftware__StartDomain=panachesoftware.com
        - PanacheSoftware__Url__IdentityServerURL=http://192.168.86.247:55002
        - PanacheSoftware__Url__IdentityServerURLSecure=https://192.168.86.247:44302
        - PanacheSoftware__Secret__FoundationServiceSecret=70CD8BB9-5256-42CF-8B95-DD61C1051AD0
        ports:
        - "192.168.86.247:55004:80"
        restart: always

    panachesoftware.service.file:
        image: panachesoftware/panachesoftwareservicefile-arm32:latest
        depends_on:
        - panachesoftware.identity
        environment:
        - ASPNETCORE_ENVIRONMENT=Development
        - ASPNETCORE_URLS=http://+:80
        - ConnectionStrings__MySQL=server=192.168.86.247;port=3306;database=PanacheSoftware.File.Docker;user=pi;password=Passw0rd123!;GuidFormat=Char36
        - PanacheSoftware__CallMethod__APICallsSecure=False
        - PanacheSoftware__CallMethod__UICallsSecure=False
        - PanacheSoftware__CallMethod__UseAPIGateway=False
        - PanacheSoftware__DBProvider=MySQL
        - PanacheSoftware__StartDomain=panachesoftware.com
        - PanacheSoftware__Url__IdentityServerURL=http://192.168.86.247:55002
        - PanacheSoftware__Url__IdentityServerURLSecure=https://192.168.86.247:44302
        - PanacheSoftware__Secret__FileServiceSecret=839C649E-4FE3-410C-B43F-69C017A52676
        ports:
        - "192.168.86.247:55008:80"
        restart: always

    panachesoftware.service.client:
        image: panachesoftware/panachesoftwareserviceclient-arm32:latest
        depends_on:
        - panachesoftware.identity
        environment:
        - ASPNETCORE_ENVIRONMENT=Development
        - ASPNETCORE_URLS=http://+:80
        - ConnectionStrings__MySQL=server=192.168.86.247;port=3306;database=PanacheSoftware.Client.Docker;user=pi;password=Passw0rd123!;GuidFormat=Char36
        - PanacheSoftware__CallMethod__APICallsSecure=False
        - PanacheSoftware__CallMethod__UICallsSecure=False
        - PanacheSoftware__CallMethod__UseAPIGateway=False
        - PanacheSoftware__DBProvider=MySQL
        - PanacheSoftware__StartDomain=panachesoftware.com
        - PanacheSoftware__Url__IdentityServerURL=http://192.168.86.247:55002
        - PanacheSoftware__Url__IdentityServerURLSecure=https://192.168.86.247:44302
        - PanacheSoftware__Secret__ClientServiceSecret=1314EF18-40FA-4B16-83DF-B276FF0D92A9
        ports:
        - "192.168.86.247:55005:80"

    panachesoftware.ui.client:
        image: panachesoftware/panachesoftwareuiclient-arm32:latest
        depends_on:
        - panachesoftware.identity
        - panachesoftware.service.team
        - panachesoftware.service.task
        - panachesoftware.service.foundation
        - panachesoftware.service.file
        - panachesoftware.service.client
        environment:
        - ASPNETCORE_ENVIRONMENT=Development
        - ASPNETCORE_URLS=http://+:80
        - PanacheSoftware__CallMethod__APICallsSecure=False
        - PanacheSoftware__CallMethod__UICallsSecure=False
        - PanacheSoftware__CallMethod__UseAPIGateway=False
        - PanacheSoftware__StartDomain=panachesoftware.com
        - PanacheSoftware__Url__IdentityServerURL=http://192.168.86.247:55002
        - PanacheSoftware__Url__IdentityServerURLSecure=https://192.168.86.247:44302
        - PanacheSoftware__Url__APIGatewayURL=http://192.168.86.247:55003
        - PanacheSoftware__Url__APIGatewayURLSecure=https://192.168.86.247:44303
        - PanacheSoftware__Url__UIClientURL=http://192.168.86.247:55001
        - PanacheSoftware__Url__UIClientURLSecure=https://192.168.86.247:44301
        - PanacheSoftware__Url__ClientServiceURL=http://192.168.86.247:55005
        - PanacheSoftware__Url__ClientServiceURLSecure=https://192.168.86.247:44305
        - PanacheSoftware__Url__FileServiceURL=http://192.168.86.247:55008
        - PanacheSoftware__Url__FileServiceURLSecure=https://192.168.86.247:44308
        - PanacheSoftware__Url__FoundationServiceURL=http://192.168.86.247:55004
        - PanacheSoftware__Url__FoundationServiceURLSecure=https://192.168.86.247:44304
        - PanacheSoftware__Url__TaskServiceURL=http://192.168.86.247:55007
        - PanacheSoftware__Url__TaskServiceURLSecure=https://192.168.86.247:44307
        - PanacheSoftware__Url__TeamServiceURL=http://192.168.86.247:55006
        - PanacheSoftware__Url__TeamServiceURLSecure=https://192.168.86.247:44306
        - PanacheSoftware__Secret__UIClientSecret=49C1A7E1-0C79-4A89-A3D6-A37998FB86B0
        ports:
        - "192.168.86.247:55001:80"
        restart: always

Once all containers are running navigate to **http://192.168.86.247:55001** on any machine connected to the same network to open the Panache Legal Platform.  You can use the following details to login (assuming you did not change the **PanacheSoftware__StartDomain** environment variable to a different domain).

Username: admin@panachesoftware.com

Password: Passw0rd123!