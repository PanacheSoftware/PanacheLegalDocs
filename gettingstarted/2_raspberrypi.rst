Running Panache Legal on a Raspberry Pi
=======================================

One of the core design considerations for the Panache Legal Platform is to build a LegalTech solution that can be used by an individual lawyer, right through to an enterprise, on any environment they may want to use.  Panache Legal is built using `Microsoft .NET Core <https://dotnet.microsoft.com/>`_ which provides cross platform functionality allowing Panache Legal to run on Windows, Mac and Linux (including ARM based variants).  

Although it is unlikely that you would run a production environment on a `Raspberry Pi <https://www.raspberrypi.org/>`_ it is possible to get Panache Legal running on one which means that if you had no available system to test on, you could get up and running with the complete platform for just the cost of a single Raspberry Pi, around £55.

If you want to run Panache Legal on a Raspberry Pi you can grab a copy of the code and build it yourself, but a far simpler method is to run the same way you can on Windows and use our pre-built docker images that you can find on our `Docker Hub <https://hub.docker.com/u/panachesoftware>`_ area, look for the versions ending with '-arm32'.

SQL Server
^^^^^^^^^^

At present Panache Legal only supports SQL Server for its database which doesn't run natively on a Raspberry Pi.  

One of our development milestones is to add support for MYSQL but until then when running Panache Legal on a Raspberry Pi you will need to connect to a SQL Server database hosted externally.  This could be on another local Windows machine but if that isn't available why not sign up to `Microsoft Azure <https://azure.microsoft.com/>`_ for free and use your 30 days of credits to run a Linux based developer version of SQL Server by following this guide: `SQL Server on Linux <https://docs.microsoft.com/en-us/azure/azure-sql/virtual-machines/linux/sql-server-on-linux-vm-what-is-iaas-overview>`_.

0 to LegalTech in 3 minutes
^^^^^^^^^^^^^^^^^^^^^^^^^^^

With a SQL Server database configured and Docker installed on your Raspberry Pi it's possible to get Panache Legal up and running in as little as 3 minutes, just watch `this video <https://youtu.be/pwvgs_HV6Lg>`_ to see an example.

.. note::   Panache Legal is currently in early alpha development and is not suitable for production environments, it should only be used for early testing.  

            Panache Legal containers currently include a development build of the software and do not support features like HTTPS to prevent complications with certificates.

            The current build of Panache Legal supports SQL Server.

TL;DR
^^^^^

What follows is a step by step guide to running Panache Legal on a Raspberry Pi but the TL;DR is.

1. Install Docker and Docker compose

2. Setup a SQL Server environment

3. Download the example `docker-compose.yml <https://github.com/PanacheSoftware/PanacheLegalPlatform/blob/main/support%20files/docker/raspberrypi/docker-compose.yml>`_ file

4. Run the following command::

    docker-compose up -d

Setup your Raspberry Pi
^^^^^^^^^^^^^^^^^^^^^^^

Firstly you need to setup your Raspberry Pi, this example has been tested with a Raspberry Pi 4 model B 4GB.

1. `Purchase <https://thepihut.com/collections/raspberry-pi-kits-and-bundles/products/raspberry-pi-starter-kit>`_ a Raspberry Pi 4 Model B 4GB

2. Wait patiently for delivery....

3. `Setup <https://www.raspberrypi.org/help/>`_ your Raspberry Pi

4. Before you install Panache Legal maybe checkout some of the `cool <https://projects.raspberrypi.org/en>`_ things you can do, don't worry, we'll wait....

5. Install Docker and Docker Compose.  There are lots of guides for this, here's one to get you going: `Guide <https://dev.to/rohansawant/installing-docker-and-docker-compose-on-the-raspberry-pi-in-5-simple-steps-3mgl>`_ 

Setup SQL Server
^^^^^^^^^^^^^^^^

If you don't have a local installation, or a SQL Server instance provided by your organisation, you can stand up a free developer version for testing on Microsoft Azure.

1. Sign up for a free 30 day `Microsoft Azure <Microsoft Azure_>`_ account and claim your £150 worth of credits

2. Follow a guide to get `SQL Server on Linux <SQL Server on Linux_>`_ up and running in a virtual machine and make a note of the IP address, you'll need that below

Download Docker compose file
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

You can download an example docker compose file suitable for a Raspberry Pi (ARM based Linux) via the following link.

GitHub: `docker-compose.yml <docker-compose.yml_>`_

**Steps to run:**

1. Place downloaded **docker-compose.yml** file into a folder of your choosing.

2. Edit the **docker-compose.yml** file changing the following value to the IP address and port of your SQL Server::

    {IP address and port for example 12.345.678.90,1433}

3. In the shell, navigate to the folder where you downloaded the **docker-compose.yml** file and run the following command::

    docker-compose up -d

This will download all the containers from the Docker Hub and start them up.

.. note:: Time to download and start all containers will depend on your internet connection and the performance of your computer.

Once all containers are running open a web browser and navigate to **http://172.17.0.1:55001** to open the Panache Legal Platform.  You can use the following details to login (assuming you did not change the **PanacheSoftware__StartDomain** environment variable to a different domain).

Username: admin@panachesoftware.com

Password: Passw0rd123!
