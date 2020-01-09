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
      - Management: 10.1.1.4
        Internal: 10.1.10.10
        External: 10.1.20.10
      - admin / admin
    * - BIG-IP02
      - Management: 10.1.1.1
        Internal: 10.1.10.11
        External: 10.1.20.11
      - admin / admin
    * - Windows Server
      - Management: 10.1.1.9
        Internal: 10.1.10.41
      - user / user
    * - LAMP Server
      - Management: 10.1.1.6
        Internal: 10.1.10.30
      - None


Starting the Lab
================

**Connect to the Windows host system via RDP**

#. Click on the `Components` tab in your UDF deployment
    .. image:: /_static/components.jpg

#. Under ''Systems'' find the Windows Server 2019 Base and click ''Access'',then click ''RDP''.  When prompted, select option to "Save" RDP file.  RDP file will be downloaded to your local machine.
    .. image:: /_static/Win2019_RDP_Access.png

#. Open the RDP file downloaded in the previous step and click ''Continue'' when prompted.
    .. image:: /_static/Win2019_RDP_Continue.png

#. When prompted for login credentials, select ''More Choices''.

#. Select ''Use a different account'' under ''More Choices'' option.
    .. image:: /_static/Win2019_RDP_DiffAccount.png

#. Enter credentials: Username: user / Password: user, then click ''OK''.
    .. image:: /_static/Win2019_RDP_Login.png

#. When prompted, click ''Yes'' to connection, and session will be established to Windows host.
    .. image:: /_static/RDP_Connect.jpg

#. Congratulations! You are now connected to your windows jump host.

