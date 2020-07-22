Docker Compose File
===================

You can download an example docker compose file via the following link.

GitHub: `docker-compose.yml <https://github.com/PanacheSoftware/PanacheLegalPlatform/blob/main/support%20files/docker/docker-compose.yml>`_

This file has been configured to download and start up everything you need to run the Panache Legal Platform and its currently available services.

This example also includes a SQL Server `docker container <https://hub.docker.com/_/microsoft-mssql-server>`_ using the free developer licence.  This developer licence includes some limitations on how it can be deployed and you should make sure to read the licence details on the docker hub page, as well as checking which `edition <https://www.microsoft.com/en-us/sql-server/sql-server-2017-editions>`_ will be suitable for your use case.

Environment Variables
^^^^^^^^^^^^^^^^^^^^^

The Panache Legal Platform docker images require several environment variables to be configured, these are listed below.

.. note:: Not all images require the same variables.

``ASPNETCORE_ENVIRONMENT``

Set's the environment that this container uses.  This should be set as **Development**

``ASPNETCORE_URLS``

The http ports to be spun up by the webserver.  This should be set as **http://+:80**

``ConnectionStrings__MySQL``

If the image requires a database use this to specify a connection string to a MySQL database.

``ConnectionStrings__MSSQL``

If the image requires a database use this to specify a connection string to a MSSQL database.

``PanacheSoftware__CallMethod__APICallsSecure``

Specifies if API calls should be made using **http** (value of false) or **https** (value of true).  Currently Panache Legal docker images only support a value of **False**

``PanacheSoftware__CallMethod__UICallsSecure``

Specifies if UI calls should be made using **http** (value of false) or **https** (value of true).  Currently Panache Legal docker images only support a value of **False**

``PanacheSoftware__CallMethod__UseAPIGateway``

Specifies if API calls should be handled via the Panache Legal API Gateway service.  Currently unsupported when using Panache Legal docker images so this should have a value of **False**

``PanacheSoftware__StartDomain``

The underlying foundations of the Panache Legal Platform support running as `multitenant <https://en.wikipedia.org/wiki/Multitenancy>`_. This variable should be used to specify the start domain (tenant) and all users created in system will require a username set with an email address that matches this domain.  A default user will be created on startup based on this value so, for example, if this is set with a value of **panachesoftware.com** then on start a default user will be created with the following details.

Username: admin@panachesoftware.com

Password: Passw0rd123!

``PanacheSoftware__DBProvider``

Used to specify if the service should use Microsoft SQL server or MySQL for its database.  This should have a value of either **MSSQL** or **MySQL** and you should make sure the corresponding connection string is configured.

``PanacheSoftware__Url__IdentityServerURL`` and ``PanacheSoftware__Url__IdentityServerURLSecure``

Specifies the URL for the Panache Legal Identity service.  Currently Panache Legal docker images do not support the **Secure** URL.

``PanacheSoftware__Url__APIGatewayURL`` and ``PanacheSoftware__Url__APIGatewayURLSecure``

Specifies the URL for the Panache Legal API Gateway.  Currently not supported by Panache Legal docker images, although this should still be included.

``PanacheSoftware__Url__UIClientURL`` and ``PanacheSoftware__Url__UIClientURLSecure``

Specifies the URL for the Panache Legal UI.  Currently Panache Legal docker images do not support the **Secure** URL.

``PanacheSoftware__Url__ClientServiceURL`` and ``PanacheSoftware__Url__ClientServiceURLSecure``

Specifies the URL for the Panache Legal Client service.  Currently Panache Legal docker images do not support the **Secure** URL.

``PanacheSoftware__Url__FileServiceURL`` and ``PanacheSoftware__Url__FileServiceURLSecure``

Specifies the URL for the Panache Legal File service.  Currently Panache Legal docker images do not support the **Secure** URL.

``PanacheSoftware__Url__FoundationServiceURL`` and ``PanacheSoftware__Url__FoundationServiceURLSecure``

Specifies the URL for the Panache Legal Foundation service.  Currently Panache Legal docker images do not support the **Secure** URL.

``PanacheSoftware__Url__TaskServiceURL`` and ``PanacheSoftware__Url__TaskServiceURLSecure``

Specifies the URL for the Panache Legal Task service.  Currently Panache Legal docker images do not support the **Secure** URL.

``PanacheSoftware__Url__TeamServiceURL`` and ``PanacheSoftware__Url__TeamServiceURLSecure``

Specifies the URL for the Panache Legal Team service.  Currently Panache Legal docker images do not support the **Secure** URL.

``PanacheSoftware__Secret__UIClientSecret``

The secret used to identify the Panache Legal UI to Panache Legal Identity.  Should be set as a unique GUID.

``PanacheSoftware__Secret__APIGatewaySecret``

The secret used to identify the Panache Legal API Gateway to Panache Legal Identity.  Should be set as a unique GUID.

``PanacheSoftware__Secret__ClientServiceSecret``

The secret used to identify the Panache Legal Client service to Panache Legal Identity.  Should be set as a unique GUID.

``PanacheSoftware__Secret__FileServiceSecret``

The secret used to identify the Panache Legal File service to Panache Legal Identity.  Should be set as a unique GUID.

``PanacheSoftware__Secret__FoundationServiceSecret``

The secret used to identify the Panache Legal Foundation service to Panache Legal Identity.  Should be set as a unique GUID.

``PanacheSoftware__Secret__TaskServiceSecret``

The secret used to identify the Panache Legal Task service to Panache Legal Identity.  Should be set as a unique GUID.

``PanacheSoftware__Secret__TeamServiceSecret``

The secret used to identify the Panache Legal Team service to Panache Legal Identity.  Should be set as a unique GUID.

MySQL or MSSQL Docker
^^^^^^^^^^^^^^^^^^^^^

MySQL::

    sqldata:
        image: mysql:latest
    environment:
      - MYSQL_ROOT_PASSWORD=Passw0rd123!
    volumes:
      - panachesoftware-sqldata:/var/opt/mssql


Microsoft SQL Server::

    sqldata:
        image: mcr.microsoft.com/mssql/server:2017-latest
    environment:
        - SA_PASSWORD=Passw0rd123!
        - ACCEPT_EULA=Y
    ports:
        - "5433:1433"
    volumes:
        - panachesoftware-sqldata:/var/opt/mssql

::

    volumes:
        panachesoftware-sqldata:
            external: false

This downloads and starts up a Linux based SQL Server container or a MySQL container in docker.  

For Microsoft SQL server the password for the **sa** user will be set to 'Passw0rd123!' and the EULA will be automatically accepted.  On MySQL the password for the **root** user will be set to 'Passw0rd123!'.

For Microsoft SQL Server it is assumed that you will be running this SQL Server image using the free developer licence, but you should confirm that this licence applies to your organisation and use case or whether you require a seperate licence.

.. note:: This is not required if you are connecting to an existing database installation.

Panache Software Identity
^^^^^^^^^^^^^^^^^^^^^^^^^

::

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

The Identity Service requires a database (which will be created on start-up) as well as the secrets to identify all of the other Panache Legal services, along with the address of the Panache Legal UI.

Panache Software Team Service
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

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

The Team Service requires a database (which will be created on start-up) as well as a secret that can be used to identify it and the location of the Panache Legal Identity service to allow it to perform authorisation against requests.

Panache Software Task Service
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

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

The Task Service requires a database (which will be created on start-up) as well as a secret that can be used to identify it and the location of the Panache Legal Identity service to allow it to perform authorisation against requests.  This service also needs to call the Team service for data control.

Panache Software Foundation Service
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

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

The Foundation Service requires a database (which will be created on start-up) as well as a secret that can be used to identify it and the location of the Panache Legal Identity service to allow it to perform authorisation against requests.

Panache Software File Service
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

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

The File Service requires a database (which will be created on start-up) as well as a secret that can be used to identify it and the location of the Panache Legal Identity service to allow it to perform authorisation against requests.

Panache Software Client Service
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

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

The Client Service requires a database (which will be created on start-up) as well as a secret that can be used to identify it and the location of the Panache Legal Identity service to allow it to perform authorisation against requests.

Panache Software UI
^^^^^^^^^^^^^^^^^^^

::

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

The Panache Legal UI requires a secret that can be used to identify it and the location of all other Panache Legal services so that it can make appropriate API calls.
