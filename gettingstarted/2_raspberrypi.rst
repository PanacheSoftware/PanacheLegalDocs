Running Panache Legal on a Raspberry Pi
=======================================

.. image:: /img/raspi-logo.png
   :align: center

One of the core design considerations for the Panache Legal Platform is to build a LegalTech solution that can be used by an individual lawyer, right through to an enterprise, on any environment they may want to use.  Panache Legal is built using `Microsoft .NET Core <https://dotnet.microsoft.com/>`_ which provides cross platform functionality allowing Panache Legal to run on Windows, Mac and Linux (including ARM based variants).  

Although it is unlikely that you would run a production environment on a `Raspberry Pi <https://www.raspberrypi.org/>`_ it is possible to get Panache Legal running on one which means that if you had no available system to test on, you could get up and running with the complete platform for just the cost of a single Raspberry Pi, around £55.

If you want to run Panache Legal on a Raspberry Pi you can grab a copy of the code and build it yourself, but a far simpler method is to run the same way you can on Windows and use our pre-built docker images that you can find on our `Docker Hub <https://hub.docker.com/u/panachesoftware>`_ area, look for the versions ending with '-arm32'.

0 to LegalTech in 3 minutes
^^^^^^^^^^^^^^^^^^^^^^^^^^^

With a SQL Server database configured and Docker installed on your Raspberry Pi it's possible to get Panache Legal up and running in as little as 3 minutes, just watch `this video <https://youtu.be/pwvgs_HV6Lg>`_ to see an example.

.. note::   Panache Legal is currently in early alpha development and is not suitable for production environments, it should only be used for early testing.  

            Panache Legal containers currently include a development build of the software and do not support features like HTTPS to prevent complications with certificates.

            The current build of Panache Legal supports Microsoft SQL Server and MySQL.

TL;DR
^^^^^

What follows is a step by step guide to running Panache Legal on a Raspberry Pi but the TL;DR is.

1. Install Docker and Docker compose

2. Setup a MySQL database

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

Setup MySQL
^^^^^^^^^^^

In this example we will use MySQL installed locally on the Raspberry Pi.  The easiest way to set this up is to use the following guides:

1. Install MySQL on your Raspberry Pi using MariaDB: `MySQL Install <https://pimylifeup.com/raspberry-pi-mysql/>`_

2. Although not required you should install phpMyAdmin to make Database administration easy: `phpMyAdmin Install <https://pimylifeup.com/raspberry-pi-phpmyadmin/>`_

Make sure to note down the MySQL username and password you setup.

Download Docker compose file
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

You can download an example docker compose file suitable for a Raspberry Pi (ARM based Linux) via the following link.

GitHub: `docker-compose.yml <docker-compose.yml_>`_

**Steps to run:**

1. Place downloaded **docker-compose.yml** file into a folder of your choosing.

2. Edit the **docker-compose.yml** file changing the following value to the IP address of your MySQL installation (or 127.0.0.1 if installed locally)::

    {ip address of MySQL server}

3. Edit the **docker-compose.yml** file changing the following to 172.17.0.1, or the IP address of your Raspberry Pi if running remotely::

    {ip address of host or 172.17.0.1 if only accessing locally}

4. Edit the **docker-compose.yml** to delete the below, or set it to the the IP address of your Raspberry Pi if running remotely::

    {delete this if running locally, otherwise ip address of host i.e. 123.456.78.900:}

5. In the shell, navigate to the folder where you downloaded the **docker-compose.yml** file and run the following command::

    docker-compose up -d

This will download all the containers from the Docker Hub and start them up.

.. note:: Time to download and start all containers will depend on your internet connection and the performance of your computer.

Once all containers are running open a web browser and navigate to **http://172.17.0.1:55001** to open the Panache Legal Platform.  You can use the following details to login (assuming you did not change the **PanacheSoftware__StartDomain** environment variable to a different domain).

Username: admin@panachesoftware.com

Password: Passw0rd123!
