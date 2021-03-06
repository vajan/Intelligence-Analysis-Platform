Adding an ELP stage to a live deployment
========================================

After you develop and test a solution for the Intelligence Analysis Platform that involves an ELP stage, the next step is to add an ELP stage to a live deployment.

Before you begin
----------------

These instructions assume that you have access to two instances of the Intelligence Analysis Platform:

-   The development version of the platform, deployed according to the instructions in IBM i2 Intelligence Analysis Platform Developer Essentials. This instance contains an ELP stage.
-   The production version of the platform, deployed according to the instructions in the IBM i2 Intelligence Analysis Platform Deployment Guide. This instance has all of its default settings.

About this task
---------------

The process for adding an ELP stage to a live deployment of the Intelligence Analysis Platform involves editing the configuration files, and then running the deployment scripts.

Procedure
---------

1.  Copy all the configuration elements for the ELP stage from `topology.xml` in the development version of the Deployment Toolkit to the same file in the production version.

    Note: Remember always to start by modifying the Deployment Toolkit on the write-side server.

2.  In the topology file, modify the database settings to match the production deployment.
3.  In the same directory as `topology.xml`, modify the `credentials.properties` file to add credential information for the database that hosts the new ELP stage.
4.  Stop WebSphere Application Server on the write side, and then also stop it on the read side.
5.  Run the following commands on the write-side server:

    ``` {.pre .codeblock}
    deploy.py -s write -t delete-queue-manager
    deploy.py -s write -t provision-storage-and-messaging
    deploy.py -s write -t start-was-server
    deploy.py -s write -t update-application
    deploy.py -s write -t install-http-server-config
    ```

6.  Restart the HTTP server.
7.  Copy the Deployment Toolkit to the read side.
8.  Run the following commands on the read-side server:

    ``` {.pre .codeblock}
    deploy.py -s read -t provision-storage-and-messaging
    deploy.py -s read -t start-was-server
    deploy.py -s read -t update-application
    ```

9.  Run the following command on the write-side server:

    ``` {.pre .codeblock}
    deploy.py -s write -t start-all-applications
    ```

* * * * *

© Copyright IBM Corporation 2014.


