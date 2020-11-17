Docker Compose File Examples
============================

The following are examples of complete docker compose files.

.. note::   In the examples below whenever a GUID is shown i.e. 9C32BBD1-9C08-40FA-96C3-2195F61661F1, this is used as an example.  You should replace these with your own GUIDS when creating your environment.


Raspberry Pi (or Linux) with MySQL 
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The following will provide a Panache Legal Platform environment running on a Raspberry Pi using a local MySQL (MariaDb) database.

In this example the Raspberry Pi hostname is 'raspberrypi' and a local `installation of MySQL <https://pimylifeup.com/raspberry-pi-mysql/>`_ has been performed.  A user called 'pluser' has been setup with a password of 'Passw0rd123!'.

Once all containers are running open a web browser on the Raspberry Pi, or from any device connected to the same local network, and navigate to **http://raspberrypi:55001** to open the Panache Legal Platform.  You can use the following details to login (assuming you did not change the **PanacheSoftware__StartDomain** environment variable to a different domain).

Username: admin@panachesoftware.com

Password: Passw0rd123!

::

    version: "3.4"
    services:

    panachesoftware.identity:
        image: panachesoftware/panachesoftwareidentity-arm32:latest
        environment:
            - ASPNETCORE_ENVIRONMENT=Development
            - ASPNETCORE_URLS=http://+:55002      
            - ConnectionStrings__MySQL=server=raspberrypi;port=3306;database=PanacheSoftware.Identity.Pi;user=pluser;password=Passw0rd123!;GuidFormat=Char36
            - PanacheSoftware__CallMethod__APICallsSecure=False
            - PanacheSoftware__CallMethod__UICallsSecure=False
            - PanacheSoftware__CallMethod__UseAPIGateway=False
            - PanacheSoftware__DBProvider=MySQL
            - PanacheSoftware__StartDomain=panachesoftware.com
            - PanacheSoftware__Url__UIClientURL=http://raspberrypi:55001
            - PanacheSoftware__Url__UIClientURLSecure=https://raspberrypi:44301
            - PanacheSoftware__Secret__UIClientSecret=9C32BBD1-9C08-40FA-96C3-2195F61661F1
            - PanacheSoftware__Secret__APIGatewaySecret=D32CAA87-2F1F-4D35-B2DF-C712C2AFF6C3
            - PanacheSoftware__Secret__ClientServiceSecret=AA04416A-A87B-4D88-956B-27CBFFCC2802
            - PanacheSoftware__Secret__FileServiceSecret=6D20E27B-18EE-4694-AFB7-929343F51D43
            - PanacheSoftware__Secret__FoundationServiceSecret=A81F774A-6E87-4C37-BEA0-2F62CC2F374A
            - PanacheSoftware__Secret__TaskServiceSecret=8CDD9C0B-EAD3-4377-A42E-AEEAC9909C0D
            - PanacheSoftware__Secret__TeamServiceSecret=819E2211-EAD4-49D0-84E5-29E6F45587BB
        network_mode: "host"
        restart: always

    panachesoftware.service.team:
        image: panachesoftware/panachesoftwareserviceteam-arm32:latest
        depends_on:
            - panachesoftware.identity
        environment:
            - ASPNETCORE_ENVIRONMENT=Development
            - ASPNETCORE_URLS=http://+:55006
            - ConnectionStrings__MySQL=server=raspberrypi;port=3306;database=PanacheSoftware.Team.Pi;user=pluser;password=Passw0rd123!;GuidFormat=Char36
            - PanacheSoftware__CallMethod__APICallsSecure=False
            - PanacheSoftware__CallMethod__UICallsSecure=False
            - PanacheSoftware__CallMethod__UseAPIGateway=False
            - PanacheSoftware__DBProvider=MySQL
            - PanacheSoftware__StartDomain=panachesoftware.com
            - PanacheSoftware__Url__IdentityServerURL=http://raspberrypi:55002
            - PanacheSoftware__Url__IdentityServerURLSecure=https://raspberrypi:44302
            - PanacheSoftware__Secret__TeamServiceSecret=819E2211-EAD4-49D0-84E5-29E6F45587BB
        network_mode: "host"
        restart: always

    panachesoftware.service.task:
        image: panachesoftware/panachesoftwareservicetask-arm32:latest
        depends_on:
            - panachesoftware.identity
            - panachesoftware.service.team
        environment:
            - ASPNETCORE_ENVIRONMENT=Development
            - ASPNETCORE_URLS=http://+:55007
            - ConnectionStrings__MySQL=server=raspberrypi;port=3306;database=PanacheSoftware.Task.Pi;user=pluser;password=Passw0rd123!;GuidFormat=Char36
            - PanacheSoftware__CallMethod__APICallsSecure=False
            - PanacheSoftware__CallMethod__UICallsSecure=False
            - PanacheSoftware__CallMethod__UseAPIGateway=False
            - PanacheSoftware__DBProvider=MySQL
            - PanacheSoftware__StartDomain=panachesoftware.com
            - PanacheSoftware__Url__IdentityServerURL=http://raspberrypi:55002
            - PanacheSoftware__Url__IdentityServerURLSecure=https://raspberrypi:44302
            - PanacheSoftware__Url__TeamServiceURL=http://raspberrypi:55006
            - PanacheSoftware__Url__TeamServiceURLSecure=https://raspberrypi:44306
            - PanacheSoftware__Secret__TaskServiceSecret=8CDD9C0B-EAD3-4377-A42E-AEEAC9909C0D
        network_mode: "host"
        restart: always

    panachesoftware.service.foundation:
        image: panachesoftware/panachesoftwareservicefoundation-arm32:latest
        depends_on:
            - panachesoftware.identity
        environment:
            - ASPNETCORE_ENVIRONMENT=Development
            - ASPNETCORE_URLS=http://+:55004
            - ConnectionStrings__MySQL=server=raspberrypi;port=3306;database=PanacheSoftware.Foundation.Pi;user=pluser;password=Passw0rd123!;GuidFormat=Char36
            - PanacheSoftware__CallMethod__APICallsSecure=False
            - PanacheSoftware__CallMethod__UICallsSecure=False
            - PanacheSoftware__CallMethod__UseAPIGateway=False
            - PanacheSoftware__DBProvider=MySQL
            - PanacheSoftware__StartDomain=panachesoftware.com
            - PanacheSoftware__Url__IdentityServerURL=http://raspberrypi:55002
            - PanacheSoftware__Url__IdentityServerURLSecure=https://raspberrypi:44302
            - PanacheSoftware__Secret__FoundationServiceSecret=A81F774A-6E87-4C37-BEA0-2F62CC2F374A
        network_mode: "host"
        restart: always

    panachesoftware.service.file:
        image: panachesoftware/panachesoftwareservicefile-arm32:latest
        depends_on:
            - panachesoftware.identity
        environment:
            - ASPNETCORE_ENVIRONMENT=Development
            - ASPNETCORE_URLS=http://+:55008
            - ConnectionStrings__MySQL=server=raspberrypi;port=3306;database=PanacheSoftware.File.Pi;user=pluser;password=Passw0rd123!;GuidFormat=Char36
            - PanacheSoftware__CallMethod__APICallsSecure=False
            - PanacheSoftware__CallMethod__UICallsSecure=False
            - PanacheSoftware__CallMethod__UseAPIGateway=False
            - PanacheSoftware__DBProvider=MySQL
            - PanacheSoftware__StartDomain=panachesoftware.com
            - PanacheSoftware__Url__IdentityServerURL=http://raspberrypi:55002
            - PanacheSoftware__Url__IdentityServerURLSecure=https://raspberrypi:44302
            - PanacheSoftware__Secret__FileServiceSecret=6D20E27B-18EE-4694-AFB7-929343F51D43
        network_mode: "host"
        restart: always

    panachesoftware.service.client:
        image: panachesoftware/panachesoftwareserviceclient-arm32:latest
        depends_on:
            - panachesoftware.identity
        environment:
            - ASPNETCORE_ENVIRONMENT=Development
            - ASPNETCORE_URLS=http://+:55005
            - ConnectionStrings__MySQL=server=raspberrypi;port=3306;database=PanacheSoftware.Client.Pi;user=pluser;password=Passw0rd123!;GuidFormat=Char36
            - PanacheSoftware__CallMethod__APICallsSecure=False
            - PanacheSoftware__CallMethod__UICallsSecure=False
            - PanacheSoftware__CallMethod__UseAPIGateway=False
            - PanacheSoftware__DBProvider=MySQL
            - PanacheSoftware__StartDomain=panachesoftware.com
            - PanacheSoftware__Url__IdentityServerURL=http://raspberrypi:55002
            - PanacheSoftware__Url__IdentityServerURLSecure=https://raspberrypi:44302
            - PanacheSoftware__Secret__ClientServiceSecret=AA04416A-A87B-4D88-956B-27CBFFCC2802
        network_mode: "host"
        restart: always

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
            - ASPNETCORE_URLS=http://+:55001
            - PanacheSoftware__CallMethod__APICallsSecure=False
            - PanacheSoftware__CallMethod__UICallsSecure=False
            - PanacheSoftware__CallMethod__UseAPIGateway=False
            - PanacheSoftware__StartDomain=panachesoftware.com
            - PanacheSoftware__Url__IdentityServerURL=http://raspberrypi:55002
            - PanacheSoftware__Url__IdentityServerURLSecure=https://raspberrypi:44302
            - PanacheSoftware__Url__APIGatewayURL=http://raspberrypi:55003
            - PanacheSoftware__Url__APIGatewayURLSecure=https://raspberrypi:44303
            - PanacheSoftware__Url__UIClientURL=http://raspberrypi:55001
            - PanacheSoftware__Url__UIClientURLSecure=https://raspberrypi:44301
            - PanacheSoftware__Url__ClientServiceURL=http://raspberrypi:55005
            - PanacheSoftware__Url__ClientServiceURLSecure=https://raspberrypi:44305
            - PanacheSoftware__Url__FileServiceURL=http://raspberrypi:55008
            - PanacheSoftware__Url__FileServiceURLSecure=https://raspberrypi:44308
            - PanacheSoftware__Url__FoundationServiceURL=http://raspberrypi:55004
            - PanacheSoftware__Url__FoundationServiceURLSecure=https://raspberrypi:44304
            - PanacheSoftware__Url__TaskServiceURL=http://raspberrypi:55007
            - PanacheSoftware__Url__TaskServiceURLSecure=https://raspberrypi:44307
            - PanacheSoftware__Url__TeamServiceURL=http://raspberrypi:55006
            - PanacheSoftware__Url__TeamServiceURLSecure=https://raspberrypi:44306
            - PanacheSoftware__Secret__UIClientSecret=9C32BBD1-9C08-40FA-96C3-2195F61661F1
        network_mode: "host"
        restart: always

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
        - ASPNETCORE_URLS=http://+:55002
        - ConnectionStrings__MySQL=server=sqldata;port=3306;database=PanacheSoftware.Identity;user=root;password=Passw0rd123!;GuidFormat=Char36
        - PanacheSoftware__CallMethod__APICallsSecure=False
        - PanacheSoftware__CallMethod__UICallsSecure=False
        - PanacheSoftware__CallMethod__UseAPIGateway=False
        - PanacheSoftware__StartDomain=panachesoftware.com
        - PanacheSoftware__DBProvider=MySQL
        - PanacheSoftware__Url__UIClientURL=http://localhost:55001
        - PanacheSoftware__Url__UIClientURLSecure=https://localhost:44301
        - PanacheSoftware__Secret__UIClientSecret=49C1A7E1-0C79-4A89-A3D6-A37998FB86B0
        - PanacheSoftware__Secret__APIGatewaySecret=DDDCB193-213C-43FB-967A-5A911D2EFC04
        - PanacheSoftware__Secret__ClientServiceSecret=1314EF18-40FA-4B16-83DF-B276FF0D92A9
        - PanacheSoftware__Secret__FileServiceSecret=839C649E-4FE3-410C-B43F-69C017A52676
        - PanacheSoftware__Secret__FoundationServiceSecret=70CD8BB9-5256-42CF-8B95-DD61C1051AD0
        - PanacheSoftware__Secret__TaskServiceSecret=AC654B02-E46B-4359-B908-87479CBE1CEB
        - PanacheSoftware__Secret__TeamServiceSecret=5C9BF545-3C20-4448-9EEC-6B3E745B671E
        network_mode: "host"
        restart: always

    panachesoftware.service.team:
        image: panachesoftware/panachesoftwareserviceteam:latest
        depends_on:
        - sqldata
        - panachesoftware.identity
        environment:
        - ASPNETCORE_ENVIRONMENT=Development
        - ASPNETCORE_URLS=http://+:55006
        - ConnectionStrings__MySQL=server=sqldata;port=3306;database=PanacheSoftware.Team;user=root;password=Passw0rd123!;GuidFormat=Char36
        - PanacheSoftware__CallMethod__APICallsSecure=False
        - PanacheSoftware__CallMethod__UICallsSecure=False
        - PanacheSoftware__CallMethod__UseAPIGateway=False
        - PanacheSoftware__StartDomain=panachesoftware.com
        - PanacheSoftware__DBProvider=MySQL
        - PanacheSoftware__Url__IdentityServerURL=http://localhost:55002
        - PanacheSoftware__Url__IdentityServerURLSecure=https://localhost:44302
        - PanacheSoftware__Secret__TeamServiceSecret=5C9BF545-3C20-4448-9EEC-6B3E745B671E
        network_mode: "host"
        restart: always

    panachesoftware.service.task:
        image: panachesoftware/panachesoftwareservicetask:latest
        depends_on:
        - sqldata
        - panachesoftware.identity
        - panachesoftware.service.team
        environment:
        - ASPNETCORE_ENVIRONMENT=Development
        - ASPNETCORE_URLS=http://+:55007
        - ConnectionStrings__MySQL=server=sqldata;port=3306;database=PanacheSoftware.Task;user=root;password=Passw0rd123!;GuidFormat=Char36
        - PanacheSoftware__CallMethod__APICallsSecure=False
        - PanacheSoftware__CallMethod__UICallsSecure=False
        - PanacheSoftware__CallMethod__UseAPIGateway=False
        - PanacheSoftware__StartDomain=panachesoftware.com
        - PanacheSoftware__DBProvider=MySQL
        - PanacheSoftware__Url__IdentityServerURL=http://localhost:55002
        - PanacheSoftware__Url__IdentityServerURLSecure=https://localhost:44302
        - PanacheSoftware__Url__TeamServiceURL=http://localhost:55006
        - PanacheSoftware__Url__TeamServiceURLSecure=https://localhost:44306
        - PanacheSoftware__Secret__TaskServiceSecret=AC654B02-E46B-4359-B908-87479CBE1CEB
        network_mode: "host"
        restart: always

    panachesoftware.service.foundation:
        image: panachesoftware/panachesoftwareservicefoundation:latest
        depends_on:
        - sqldata
        - panachesoftware.identity
        environment:
        - ASPNETCORE_ENVIRONMENT=Development
        - ASPNETCORE_URLS=http://+:55004
        - ConnectionStrings__MySQL=server=sqldata;port=3306;database=PanacheSoftware.Foundation;user=root;password=Passw0rd123!;GuidFormat=Char36
        - PanacheSoftware__CallMethod__APICallsSecure=False
        - PanacheSoftware__CallMethod__UICallsSecure=False
        - PanacheSoftware__CallMethod__UseAPIGateway=False
        - PanacheSoftware__StartDomain=panachesoftware.com
        - PanacheSoftware__DBProvider=MySQL
        - PanacheSoftware__Url__IdentityServerURL=http://localhost:55002
        - PanacheSoftware__Url__IdentityServerURLSecure=https://localhost:44302
        - PanacheSoftware__Secret__FoundationServiceSecret=70CD8BB9-5256-42CF-8B95-DD61C1051AD0
        network_mode: "host"
        restart: always

    panachesoftware.service.file:
        image: panachesoftware/panachesoftwareservicefile:latest
        depends_on:
        - sqldata
        - panachesoftware.identity
        environment:
        - ASPNETCORE_ENVIRONMENT=Development
        - ASPNETCORE_URLS=http://+:55008
        - ConnectionStrings__MySQL=server=sqldata;port=3306;database=PanacheSoftware.File;user=root;password=Passw0rd123!;GuidFormat=Char36
        - PanacheSoftware__CallMethod__APICallsSecure=False
        - PanacheSoftware__CallMethod__UICallsSecure=False
        - PanacheSoftware__CallMethod__UseAPIGateway=False
        - PanacheSoftware__StartDomain=panachesoftware.com
        - PanacheSoftware__DBProvider=MySQL
        - PanacheSoftware__Url__IdentityServerURL=http://localhost:55002
        - PanacheSoftware__Url__IdentityServerURLSecure=https://localhost:44302
        - PanacheSoftware__Secret__FileServiceSecret=839C649E-4FE3-410C-B43F-69C017A52676
        network_mode: "host"
        restart: always

    panachesoftware.service.client:
        image: panachesoftware/panachesoftwareserviceclient:latest
        depends_on:
        - sqldata
        - panachesoftware.identity
        environment:
        - ASPNETCORE_ENVIRONMENT=Development
        - ASPNETCORE_URLS=http://+:55005
        - ConnectionStrings__MySQL=server=sqldata;port=3306;database=PanacheSoftware.Client;user=root;password=Passw0rd123!;GuidFormat=Char36
        - PanacheSoftware__CallMethod__APICallsSecure=False
        - PanacheSoftware__CallMethod__UICallsSecure=False
        - PanacheSoftware__CallMethod__UseAPIGateway=False
        - PanacheSoftware__StartDomain=panachesoftware.com
        - PanacheSoftware__DBProvider=MySQL
        - PanacheSoftware__Url__IdentityServerURL=http://localhost:55002
        - PanacheSoftware__Url__IdentityServerURLSecure=https://localhost:44302
        - PanacheSoftware__Secret__ClientServiceSecret=1314EF18-40FA-4B16-83DF-B276FF0D92A9
        network_mode: "host"
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
        - ASPNETCORE_URLS=http://+:55001
        - PanacheSoftware__CallMethod__APICallsSecure=False
        - PanacheSoftware__CallMethod__UICallsSecure=False
        - PanacheSoftware__CallMethod__UseAPIGateway=False
        - PanacheSoftware__StartDomain=panachesoftware.com
        - PanacheSoftware__Url__IdentityServerURL=http://localhost:55002
        - PanacheSoftware__Url__IdentityServerURLSecure=https://localhost:44302
        - PanacheSoftware__Url__APIGatewayURL=http://localhost:55003
        - PanacheSoftware__Url__APIGatewayURLSecure=https://localhost:44303
        - PanacheSoftware__Url__UIClientURL=http://localhost:55001
        - PanacheSoftware__Url__UIClientURLSecure=https://localhost:44301
        - PanacheSoftware__Url__ClientServiceURL=http://localhost:55005
        - PanacheSoftware__Url__ClientServiceURLSecure=https://localhost:44305
        - PanacheSoftware__Url__FileServiceURL=http://localhost:55008
        - PanacheSoftware__Url__FileServiceURLSecure=https://localhost:44308
        - PanacheSoftware__Url__FoundationServiceURL=http://localhost:55004
        - PanacheSoftware__Url__FoundationServiceURLSecure=https://localhost:44304
        - PanacheSoftware__Url__TaskServiceURL=http://localhost:55007
        - PanacheSoftware__Url__TaskServiceURLSecure=https://localhost:44307
        - PanacheSoftware__Url__TeamServiceURL=http://localhost:55006
        - PanacheSoftware__Url__TeamServiceURLSecure=https://localhost:44306
        - PanacheSoftware__Secret__UIClientSecret=49C1A7E1-0C79-4A89-A3D6-A37998FB86B0
        network_mode: "host"
        restart: always

    volumes:
    panachesoftware-sqldata:
        external: false

Once all containers are running open a web browser and navigate to **http://localhost:55001** to open the Panache Legal Platform.  You can use the following details to login (assuming you did not change the **PanacheSoftware__StartDomain** environment variable to a different domain).

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
        - ASPNETCORE_URLS=http://+:55002
        - ConnectionStrings__MSSQL=Server=sqldata;Database=PanacheSoftware.Identity.Docker;User Id=sa;Password=Passw0rd123!
        - PanacheSoftware__CallMethod__APICallsSecure=False
        - PanacheSoftware__CallMethod__UICallsSecure=False
        - PanacheSoftware__CallMethod__UseAPIGateway=False
        - PanacheSoftware__StartDomain=panachesoftware.com
        - PanacheSoftware__DBProvider=MSSQL
        - PanacheSoftware__Url__UIClientURL=http://localhost:55001
        - PanacheSoftware__Url__UIClientURLSecure=https://localhost:44301
        - PanacheSoftware__Secret__UIClientSecret=49C1A7E1-0C79-4A89-A3D6-A37998FB86B0
        - PanacheSoftware__Secret__APIGatewaySecret=DDDCB193-213C-43FB-967A-5A911D2EFC04
        - PanacheSoftware__Secret__ClientServiceSecret=1314EF18-40FA-4B16-83DF-B276FF0D92A9
        - PanacheSoftware__Secret__FileServiceSecret=839C649E-4FE3-410C-B43F-69C017A52676
        - PanacheSoftware__Secret__FoundationServiceSecret=70CD8BB9-5256-42CF-8B95-DD61C1051AD0
        - PanacheSoftware__Secret__TaskServiceSecret=AC654B02-E46B-4359-B908-87479CBE1CEB
        - PanacheSoftware__Secret__TeamServiceSecret=5C9BF545-3C20-4448-9EEC-6B3E745B671E
        network_mode: "host"
        restart: always

    panachesoftware.service.team:
        image: panachesoftware/panachesoftwareserviceteam:latest
        depends_on:
        - sqldata
        - panachesoftware.identity
        environment:
        - ASPNETCORE_ENVIRONMENT=Development
        - ASPNETCORE_URLS=http://+:55006
        - ConnectionStrings__MSSQL=Server=sqldata;Database=PanacheSoftware.Service.Team.Docker;User Id=sa;Password=Passw0rd123!
        - PanacheSoftware__CallMethod__APICallsSecure=False
        - PanacheSoftware__CallMethod__UICallsSecure=False
        - PanacheSoftware__CallMethod__UseAPIGateway=False
        - PanacheSoftware__StartDomain=panachesoftware.com
        - PanacheSoftware__DBProvider=MSSQL
        - PanacheSoftware__Url__IdentityServerURL=http://localhost:55002
        - PanacheSoftware__Url__IdentityServerURLSecure=https://localhost:44302
        - PanacheSoftware__Secret__TeamServiceSecret=5C9BF545-3C20-4448-9EEC-6B3E745B671E
        network_mode: "host"
        restart: always

    panachesoftware.service.task:
        image: panachesoftware/panachesoftwareservicetask:latest
        depends_on:
        - sqldata
        - panachesoftware.identity
        - panachesoftware.service.team
        environment:
        - ASPNETCORE_ENVIRONMENT=Development
        - ASPNETCORE_URLS=http://+:55007
        - ConnectionStrings__MSSQL=Server=sqldata;Database=PanacheSoftware.Service.Task.Docker;User Id=sa;Password=Passw0rd123!
        - PanacheSoftware__CallMethod__APICallsSecure=False
        - PanacheSoftware__CallMethod__UICallsSecure=False
        - PanacheSoftware__CallMethod__UseAPIGateway=False
        - PanacheSoftware__StartDomain=panachesoftware.com
        - PanacheSoftware__DBProvider=MSSQL
        - PanacheSoftware__Url__IdentityServerURL=http://localhost:55002
        - PanacheSoftware__Url__IdentityServerURLSecure=https://localhost:44302
        - PanacheSoftware__Url__TeamServiceURL=http://localhost:55006
        - PanacheSoftware__Url__TeamServiceURLSecure=https://localhost:44306
        - PanacheSoftware__Secret__TaskServiceSecret=AC654B02-E46B-4359-B908-87479CBE1CEB
        network_mode: "host"
        restart: always

    panachesoftware.service.foundation:
        image: panachesoftware/panachesoftwareservicefoundation:latest
        depends_on:
        - sqldata
        - panachesoftware.identity
        environment:
        - ASPNETCORE_ENVIRONMENT=Development
        - ASPNETCORE_URLS=http://+:55004
        - ConnectionStrings__MSSQL=Server=sqldata;Database=PanacheSoftware.Service.Foundation.Docker;User Id=sa;Password=Passw0rd123!
        - PanacheSoftware__CallMethod__APICallsSecure=False
        - PanacheSoftware__CallMethod__UICallsSecure=False
        - PanacheSoftware__CallMethod__UseAPIGateway=False
        - PanacheSoftware__StartDomain=panachesoftware.com
        - PanacheSoftware__DBProvider=MSSQL
        - PanacheSoftware__Url__IdentityServerURL=http://localhost:55002
        - PanacheSoftware__Url__IdentityServerURLSecure=https://localhost:44302
        - PanacheSoftware__Secret__FoundationServiceSecret=70CD8BB9-5256-42CF-8B95-DD61C1051AD0
        network_mode: "host"
        restart: always

    panachesoftware.service.file:
        image: panachesoftware/panachesoftwareservicefile:latest
        depends_on:
        - sqldata
        - panachesoftware.identity
        environment:
        - ASPNETCORE_ENVIRONMENT=Development
        - ASPNETCORE_URLS=http://+:55008
        - ConnectionStrings__MSSQL=Server=sqldata;Database=PanacheSoftware.Service.File.Docker;User Id=sa;Password=Passw0rd123!
        - PanacheSoftware__CallMethod__APICallsSecure=False
        - PanacheSoftware__CallMethod__UICallsSecure=False
        - PanacheSoftware__CallMethod__UseAPIGateway=False
        - PanacheSoftware__StartDomain=panachesoftware.com
        - PanacheSoftware__DBProvider=MSSQL
        - PanacheSoftware__Url__IdentityServerURL=http://localhost:55002
        - PanacheSoftware__Url__IdentityServerURLSecure=https://localhost:44302
        - PanacheSoftware__Secret__FileServiceSecret=839C649E-4FE3-410C-B43F-69C017A52676
        network_mode: "host"
        restart: always

    panachesoftware.service.client:
        image: panachesoftware/panachesoftwareserviceclient:latest
        depends_on:
        - sqldata
        - panachesoftware.identity
        environment:
        - ASPNETCORE_ENVIRONMENT=Development
        - ASPNETCORE_URLS=http://+:55005
        - ConnectionStrings__MSSQL=Server=sqldata;Database=PanacheSoftware.Service.Client.Docker;User Id=sa;Password=Passw0rd123!
        - PanacheSoftware__CallMethod__APICallsSecure=False
        - PanacheSoftware__CallMethod__UICallsSecure=False
        - PanacheSoftware__CallMethod__UseAPIGateway=False
        - PanacheSoftware__StartDomain=panachesoftware.com
        - PanacheSoftware__DBProvider=MSSQL
        - PanacheSoftware__Url__IdentityServerURL=http://localhost:55002
        - PanacheSoftware__Url__IdentityServerURLSecure=https://localhost:44302
        - PanacheSoftware__Secret__ClientServiceSecret=1314EF18-40FA-4B16-83DF-B276FF0D92A9
        network_mode: "host"
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
        - ASPNETCORE_URLS=http://+:55001
        - PanacheSoftware__CallMethod__APICallsSecure=False
        - PanacheSoftware__CallMethod__UICallsSecure=False
        - PanacheSoftware__CallMethod__UseAPIGateway=False
        - PanacheSoftware__StartDomain=panachesoftware.com
        - PanacheSoftware__Url__IdentityServerURL=http://localhost:55002
        - PanacheSoftware__Url__IdentityServerURLSecure=https://localhost:44302
        - PanacheSoftware__Url__APIGatewayURL=http://localhost:55003
        - PanacheSoftware__Url__APIGatewayURLSecure=https://localhost:44303
        - PanacheSoftware__Url__UIClientURL=http://localhost:55001
        - PanacheSoftware__Url__UIClientURLSecure=https://localhost:44301
        - PanacheSoftware__Url__ClientServiceURL=http://localhost:55005
        - PanacheSoftware__Url__ClientServiceURLSecure=https://localhost:44305
        - PanacheSoftware__Url__FileServiceURL=http://localhost:55008
        - PanacheSoftware__Url__FileServiceURLSecure=https://localhost:44308
        - PanacheSoftware__Url__FoundationServiceURL=http://localhost:55004
        - PanacheSoftware__Url__FoundationServiceURLSecure=https://localhost:44304
        - PanacheSoftware__Url__TaskServiceURL=http://localhost:55007
        - PanacheSoftware__Url__TaskServiceURLSecure=https://localhost:44307
        - PanacheSoftware__Url__TeamServiceURL=http://localhost:55006
        - PanacheSoftware__Url__TeamServiceURLSecure=https://localhost:44306
        - PanacheSoftware__Secret__UIClientSecret=49C1A7E1-0C79-4A89-A3D6-A37998FB86B0
        network_mode: "host"
        restart: always

    volumes:
    panachesoftware-sqldata:
        external: false

Once all containers are running open a web browser and navigate to **http://localhost:55001** to open the Panache Legal Platform.  You can use the following details to login (assuming you did not change the **PanacheSoftware__StartDomain** environment variable to a different domain).

Username: admin@panachesoftware.com

Password: Passw0rd123!