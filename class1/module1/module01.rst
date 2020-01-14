===============
Getting Started
===============

Lab Components
==============

The following table lists the virtual appliances in the lab along with their networks and credentials to use.

.. list-table::
    :widths: 20 40 40
    :header-rows: 1
    :stub-columns: 1

    * - **System Type**
      - **Networks**
      - **Credentials**

    * - BIG-IP01
      - Management: 10.1.1.4,
        Internal: 10.1.10.10,
        External: 10.1.20.10
      - admin / admin
    * - BIG-IP02
      - Management: 10.1.1.1,
        Internal: 10.1.10.11,
        External: 10.1.20.11
      - admin / admin
    * - Windows Server
      - Management: 10.1.1.9,
        Internal: 10.1.10.41,
        External: 10.1.20.41
      - user / user
    * - LAMP Server
      - Management: 10.1.1.6,
        Internal: 10.1.10.30
      - None


Starting the Lab
================

**Exercise 1 - Login to Windows jump host**

#. Click on the ``Components`` tab in your UDF deployment
    .. image:: /_static/components.jpg

#. Under ``Systems`` find the Windows Server 2019 Base and click ``Access``,then click ``RDP``.  When prompted, select option to ``Save`` RDP file.  RDP file will be downloaded to your local machine.
    .. image:: /_static/Win2019_RDP_Access.JPG

#. Open the RDP file downloaded in the previous step and click ``Continue`` when prompted.
    .. image:: /_static/Win2019_RDP_Connect.JPG

#. When prompted for login credentials, select ``More Choices``.

#. Select ``Use a different account`` under ``More Choices`` option.
    .. image:: /_static/Win2019_RDP_DiffAccount.JPG

#. Enter credentials: user / user

#. Click ``OK``.
    .. image:: /_static/Win2019_RDP_Login.JPG

#. When prompted, click ``Yes`` to connection, and session will be established to Windows host.
    .. image:: /_static/Win2019_RDP_YesConnect.JPG

#. Congratulations! You are now connected to your Windows jump host.

**Exercise 2 - Launch and configure Postman**

#. Right click and open Postman

    .. image:: /_static/postman.jpg

#. Once Postman has launched, you need load the Postman environment variables and import the collections we will be using for the lab.

#. First, let's import the collections, which are located on a github repo. On the top left, click the ``Import`` button.

    .. image:: /_static/import.jpg

#. Select ``Import From Link`` and paste the link into the text box, ``https://raw.githubusercontent.com/Larsende/Agility2020-AS3/master/_static/Postman-AS3-lab.json``, then click ``Import``:

    .. image:: /_static/import_from_link.jpg
    
#. You should now see a new folder under the ``Collections`` tab to the left of the Postman application screen.

#. Navigate to File -> Settings and confirm the ``SSL Certificate Verification`` option is turned OFF. If it is on, please turn it OFF. Once finished, exit the settings menu.

#. Now let's import the Postman environment. An environment is a set of variables that allow you to switch the context of your requests. Repeat the same steps we did to import our collections. On the top left, click the ``Import`` button.

    .. image:: /_static/import.jpg

#. Select ``Import From Link`` and paste the following link into the text box, then click ``Import``:

    .. image:: /_static/import_from_link.jpg

#. Navigate to the top right of Postman where you will see a dropdown menu to choose your environment, ``AS3-Lab``. Click on the environment we just imported. You can then view the variables and their values by clicking the eye button.

    .. image:: /_static/environment.jpg

    .. image:: /_static/eye.jpg


