Running Panache Legal with Docker
=================================

The easiest way to get the Panache Legal Platform up and running is to use `Docker <https://www.docker.com/>`_. Docker provides a way of packaging applications in a container which can be run locally, in an on premise environment, or via a platform like `Microsoft Azure App Service <https://azure.microsoft.com/en-gb/services/app-service/>`_.  When running Panache Legal via docker everything you need is included within the container meaning that as long as you have Docker installed you have everything you need to run Panache Legal.

All Panache Legal Platform containers are distributed via the Panache Software `Docker Hub <https://hub.docker.com/u/panachesoftware>`_, here you can find docker containers for all the components of the Panache Legal platform distributed as Linux containers (that can be run on Windows, Mac or Linux) as well as Linux ARM32 containers suitable for running on ARM platforms like the `Raspberry Pi <https://www.raspberrypi.org/>`_.


0 to LegalTech in 3 minutes
^^^^^^^^^^^^^^^^^^^^^^^^^^^

Once you have Docker installed on your system and have a database available it's possible to get Panache Legal up and running in as little as 3 minutes, just watch `this video <https://youtu.be/pwvgs_HV6Lg>`_ to see an example of getting Panache Legal up and running on a Raspberry Pi.

This guide provides you with details to get you up and running with Panache Legal in a Windows 10 environment.  The principal will be the same in other enviornemnts (like Mac and Linux) so you should be able to adapt these instructions by making adjustments to the docker compose file.

.. note::   Panache Legal is currently in early alpha development and is not suitable for production environments, it should only be used for early testing.  

            Panache Legal containers currently include a development build of the software and do not support features like HTTPS to prevent complications with certificates.

            The current build of Panache Legal supports Microsoft SQL Server and MySQL.

TL;DR
^^^^^

What follows is a step by step guide to running Panache Legal using Docker on Windows but the TL;DR is.

1. Install `Docker Desktop <https://www.docker.com/products/docker-desktop>`_

2. Download the example `docker-compose.yml <https://github.com/PanacheSoftware/PanacheLegalPlatform/blob/main/support%20files/docker/docker-compose.yml>`_ file

3. Run the following command::

    docker-compose up -d

Install Docker
^^^^^^^^^^^^^^

For Windows you will need to install the `Docker Desktop <Docker Desktop_>`_ application.  Under Windows Docker requires that Hyper-V is installed (Available in Windows 10 Pro).

You can enable Hyper-V via Windows 'Apps and Features'.

1. Right click on the Windows button and select 'Apps and Features'.

2. Select **Programs and Features** on the right under related settings.

3. Select **Turn Windows Features on or off**.

4. Select **Hyper-V** and click **OK**.

Setup Database Server
^^^^^^^^^^^^^^^^^^^^^

The Panache Legal Platform requires either a Microsoft SQL Server or MySQL database.  Depending on your situation you may have a local database server installation, an on premise server that you have access to, or a hosted database server provided through a service like Microsoft Azure.  

Usage of Microsoft SQL Server requires a licence and so your use case will be specific to you and/or your organisation.

In this example we will use a MySQL Linux `docker container <https://hub.docker.com/_/mysql>`_.  You could alternatively use a `Microsoft SQL Server <https://hub.docker.com/_/microsoft-mssql-server>`_ Linux container with the free developer licence. This licence includes some limitations on how it can be deployed and you should make sure to read the licence details on the docker hub page, as well as checking which `edition <https://www.microsoft.com/en-us/sql-server/sql-server-2017-editions>`_ will be suitable for your use case.

If you are not using the docker container version of MySQL or Microsoft SQL Server as per this guide and instead will connect to your own database installation you will need to ensure you have a valid connection string to access your database.

Download Docker compose file
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

You can download an example docker compose file via the following link.

GitHub: `docker-compose.yml <docker-compose.yml_>`_

**Steps to run:**

1. Place downloaded **docker-compose.yml** file in a directory of your choosing.

2. Right clieck on the Windows button and select 'Windows PowerShell'.

3. In Windows Powershell navigate to the directory where you stored **docker-compose.yml**.

4. Type the following command::

    docker-compose up -d

This will download all the containers from the Docker Hub and start them up.

.. note:: Time to download and start all containers will depend on your internet connection and the performance of your computer.  

This docker compose file will download and start a Linux based MySQL container to store the Panache Legal Platform databases, because several of the containers require this to be in place before they can run correctly if you open the docker dashboard (via the docker desktop application) after running the command above you may see containers that fail to start.

.. image:: /img/docker-dashboard-fail.png
   :align: center

If this happens simply click on the **Start** button next to any containers that are not running until all have started correctly

.. image:: /img/docker-dashboard.png
   :align: center

Once all containers are running open a web browser and navigate to **http://host.docker.internal:55001** to open the Panache Legal Platform.  You can use the following details to login (assuming you did not change the **PanacheSoftware__StartDomain** environment variable to a different domain).

Username: admin@panachesoftware.com

Password: Passw0rd123!







