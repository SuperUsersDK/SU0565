**Chapter 6**  
**Enabling your mobile workforce**

Deploy a Single DirectAccess Server Using the Getting Started Wizard

**Prerequisites**

Before you begin deploying this scenario, review this list for important
requirements:

-   Windows firewall must be enabled on all profiles

-   This scenario is only supported when your client computers are running
    Windows 10, Windows 8.1 or Windows 8.

-   ISATAP in the corporate network is not supported. If you are using ISATAP,
    you should remove it and use native IPv6.

-   A Public Key Infrastructure is not required.

-   Not supported for deploying two factor authentication. Domain credentials
    are required for authentication.

-   Automatically deploys DirectAccess to all mobile computers in the current
    domain.

-   Traffic to Internet does not go over the DirectAccess tunnel. Force tunnel
    configuration is not supported.

-   DirectAccess server is the Network Location Server.

-   Network Access Protection (NAP) is not supported.

-   Changing policies outside of the DirectAccess management console or
    PowerShell cmdlets is not supported.

**Exercise 1:** Deploy a Single DirectAccess Server Using the Getting Started Wizard
====================================================================================

To prepare the virtual machines for a Lab solution with Direct Access, first we
need to create a Direct access Server with two nic´s. One public nic and one
internal nic.

First we create a simulated internet net, from where we test our Direct Access
solution

1.  **Open Hyper-v Manager** on your host machine and click on **Virtual Switch
    Manager…** in the actions sections in the right side.

2.  **Create a Private Virtual switch** and name it **Internet**.

Now we need to create the new Direct Access Virtual machine and attach both the
Internal net and the new Internet.

First we will create a hardisk for the DAServer to attach when creating a new VM
and Second we will create a harddisk for the InternetDNS server.

1.  Select **New** below actions section in the right side of Hyper-V manager
    screen and then select **Harddisk**.

2.  Select **Next two times** and on the **Choose Disk Type** page select
    **Differencing disk** and press **Next**.

3.  On the Specify Name and location page rename the disk **DAServer.vhdx** and
    make sure to place the disk on **C:\\Hyper-V\\Virtual Hard Disks\\** and
    then press **Next**.

4.  On the Configure Disk page type **C:\\Hyper-V\\Base\\Server2019.vhdx** for
    pointing out parent disk which is preconfigured with a server 2019
    installation and press **Next**.. and **Finish**.

5.  Before moving on creating the VM we will also create a disk the same way for
    a new Server2019 which we will configure the Internet DNS/DHCP server.

6.  Go through task 3-6 again with same options but now with

    1.  Harddisk name: **InternetDNS.vhdx.**

**Now we will create the virtual machines and attach the new harddisk´s.**

1.  Select **New** below actions section in the right side of Hyper-V manager
    screen and then select **Virtual Machine…**

2.  Select Next one time and on the Specify Name and location page type
    **DAServer** in the name field and press **Next**.

3.  Select **Generation 2** and **Next** in the specify generation page.

4.  Select **2048 MB Memory** and select to use **dynamic memory** and press
    **Next.**

5.  On the configure Networking page select **Internal Network**.

6.  On the Connect to virtual harddisk select **Use an existing Virtual
    harddisk** and type in the field **C:\\Hyper-V\\Virtual Hard
    Disks\\DAServer.vhdx** and press **Next** and then **Finish**.

7.  Right-click the new **DAServer** virtual machine and select **settings..**.

8.  Select **Add Hardware** in the top and select **Network Adapter**.

9.  Select **Internet** Virtual switch and OK.

10. Now go through task **9-14** again but now with this option:

    1.  Virtual Machine name: **InternetDNS**

    2.  Existing Virtual Harddisk location: **C:\\Hyper-V\\Virtual Hard
        Disks\\InternetDNS.vhdx**

**Configure DAServer**

1.  Start **DAServer** and sign-in with Administrator and **Pa55w.rd**.

2.  In the Server manager select Local server and configure the following
    network settings :

    1.  Configure the Network

        1.  Rename the NIC connected to myorg.local to **Corp_Net**

        2.  Rename the other NIC to **Internet**

        3.  Configure **Corp_Net** with the following configuration:

            1.  IP-Adress: 172.16.0.99

            2.  Subnet Mask: 255.255.255.0

            3.  Default Gateway: 172.16.0.1

            4.  DNS Server: 172.16.0.10

        4.  Configure **Internet** NIC with the following configuration

            1.  IP-Adress: 131.107.0.100

            2.  Subnet Mask: 255.255.255.0

    2.  Rename the server **DAServer** and domain join **to Myorg.local** domain
        with **myorg.local\\administrator** and **Pa55w.rd**

    3.  After restart Sign-in with **myorg.local\\administrator** and
        **Pa55w.rd.**

**Configure Internetdns server**

1.  Start **InternetDNS** Virtual Machine and sign-in with Administrator and
    **Pa55w.rd**.

2.  In the Server manager select Local server and configure the following
    network settings :

    1.  Configure the Network

        1.  Rename the NIC to Internet

        2.  Configure **Internet** with the following configuration:

            1.  IP-Adress: 131.107.0.101

            2.  Subnet Mask: 255.255.255.0

3.  **Now install DNS role.**

4.  Open DNS from tools menu in the server manager

5.  Right-click INTERNETDNS and select **New Zone**… and **Next**.

6.  Select P**rimary** zone and **Next**.

7.  Select **Forward lookup zone** and **Next**.

8.  Type Zone Name: **Myorg.local** and **Next** two times.

9.  On the Dynamic Update page select “**Allow both nonsecure and secure dynamic
    updates**” and **Next**.

10. Select Finish and close DNS console

11. Now install the DHCP role with default settings.

12. Configure DHCP with a new scope called **Internet.**

13. Start IP-address: 131.107.0.1

14. End IP-Address: 131.107.0.99

15. Length: 24

16. Configure DHCP options

    1.  Add Router (Default Gateway) 131.107.0.100

    2.  Add DNS Server 131.107.0.101

    3.  Other setting is default

We will now Install Direct access server on DAServer and use CL1 as a Direct
access client for test. First we will create a Domain security group called
DAClients on DC that contains CL1. Next we will install the Remote Access role
on DAServer and use the “Gettings Started wizard” to configure DAClients as
Direct Access Clients. After this configuration you will “move” CL1 to the
Internet by changing from Internal net to internet virtual switch. We will then
test to see if we can still access the internal net.

**Task 1: Configuring Direct Access**

1.  Switch to DC1

2.  In Server Manager, click Tools, and then click Active Directory Users and
    Computers

3.  In Active Directory Users and Computers, expand **Myorg.local**, and select
    Users container.

4.  click New, and then click Group

5.  In the New Object — Group dialog box, in the Group name text box,  
    type **DAClients**, and then click OK

6.  In the details pane, double-click **DAClients**, and then in the DAClients
    Properties dialog box select **Object types** and select **Computers** and
    **OK**, on the Members tab, click **Add**

7.  In the Select Users, Contacts, Computers, Service Accounts, or Groups dialog
    box,  
    in the Enter the object names to select (examples) text box, type **CL1**,  
    and then click OK twice.

8.  Switch to DAServer virtual machine

To deploy Remote Access, you must install the Remote Access role on a server in
your organization that will act as the Remote Access server.

#### To install the Remote Access role

1.  On the DAServer, in the Server Manager console, in the **Dashboard**,
    click **Add roles and features**.

2.  Click **Next** three times to get to the server role selection screen.

3.  On the **Select server roles** dialog, select **Remote Access**, and then
    click **Next**.

4.  On the **Select features** dialog, click **Next**.

5.  Click **Next**, and then on the **Select role services** dialog, click
    the **DirectAccess and VPN (RAS)** check box.

6.  Click **Add Features**, click **Next**, and then click **Install**.

7.  On the **Installation progress** dialog, verify that the installation was
    successful, and then click **Close**.

#### To configure DirectAccess using the Getting Started Wizard

1.  In Server Manager click **Tools**, and then click **Remote Access
    Management**.

2.  In the Remote Access Management console, select the role service to
    configure in the left navigation pane, and then click **Run the Getting
    Started Wizard**.

3.  Click **Deploy DirectAccess only**.

4.  Select the Edge topology of your network configuration and type the public
    IP address 131.107.0.100 to which remote access clients will connect.
    Click **Next**.

>   ** Note**

>   By default, the Getting Started Wizard deploys DirectAccess to all laptops
>   and notebook computers in the domain by applying a WMI filter to the client
>   settings GPO.

1.  We will change this default so that it only configures DAClients security
    group and not Domain computer (Labtops)

2.  Edit remote clients and remove **Domain computers** and add **DAClients**
    group. **Remember to remove the checkmark “**Enable DirectAccess for mobile
    computers only” for this lab.

    ![](media/eb57c2d42d1270197ca7e66bbca8436b.png)

3.  Click Next and select allow DirectAccess clients to use local name
    resolution

![](media/3bf6fe4fd9ad431ad421199ebbd4f6ab.png)

1.  Click **Finish**.

2.  Since there is no PKI used in this deployment, if certificates are not
    found, the wizard will automatically provision self-signed certificates for
    IP-HTTPS and the Network Location Server, and will automatically enable
    Kerberos proxy. The wizard will also enable NAT64 and DNS64 for protocol
    translation in the IPv4-only environment. After the wizard successfully
    completes applying the configuration, click **Close**.

3.  In the console tree of the Remote Access Management console,
    select **Operations Status**. Wait until the status of all monitors display
    as "Working". In the Tasks pane under Monitoring,
    click **Refresh** periodically to update the display.

**Task 2 Update clients with the DirectAccess configuration**
=============================================================

#### To update DirectAccess clients

1.  Sign-in to CL1 as myorg\\administrator and Pa55w.rd.

2.  Open PowerShell as an administrator.

3.  In the PowerShell window, type **gpupdate** and then press **ENTER**.

4.  Wait for the computer policy update to complete successfully.

5.  Type **Get-DnsClientNrptPolicy** and then press **ENTER**

>   The Name Resolution Policy Table (NRPT) entries for DirectAccess are
>   displayed. Note that the NLS server exemption is displayed. The Getting
>   Started wizard automatically created this DNS entry for the DirectAccess
>   server, and provisioned an associated self-signed certificate so that the
>   DirectAccess server can function as the Network Location Server.

1.  Type **Get-NCSIPolicyConfiguration** and then press **ENTER**. The network
    connectivity status indicator settings deployed by the wizard are displayed.
    Note the value of DomainLocationDeterminationURL. Whenever this network
    location server URL is accessible, the client will determine that it is
    inside the corporate network, and NRPT settings will not be applied.

2.  Type **Get-DAConnectionStatus** and then press **ENTER**. Since the client
    can reach the network location server URL, the status will display
    as **ConnectedLocally**.

**Task 3 Verify Deployments**
=============================

### To verify access to internal resources through DirectAccess

1.  Connect a DirectAccess client computer CL1 to the corporate network and
    obtain the group policy.

2.  Click the **Network connections** icon in the notification area to access
    the DA Media Manager.

3.  Click the **DirectAccess Connection**, and you will see that the status
    is **Connected Locally**.

4.  Connect the client computer **CL1** to the **Internet** network and attempt
    to access internal resources.

5.  Right-click and select setting on **CL1** Virtual Machine.

6.  Select Network adapter and choose Internet Virtual switch and press ok.

7.  Restart CL1 and sign-in with myorg\\administrator and Pa55w.rd.

>   You should be able to access all corporate resources.

1.  Try to run a gpupdate /force in powershell.

2.  Try to access a shared folder on one of the servers.

3.  You can also monitor on the DAServer DA console

![](media/9cd7ca39bcc92b08973664e9e9fbc930.png)

![](media/1433a4befe5a868ab9c06c0fd0445d13.png)

>   When finished with this exercise clean up the Lab the following way

1.  On CL1 virtual machine settings, change CL1 client net back to Internal
    Network and restart the client. Sign-in to CL1 and run from powershell a
    GPUpdate /Force to make sure that the client is no more configured by Direct
    Access. After the command restart the client one last time.

2.  Shutdown the Virtual machines DAServer and InternetDNS

3.  On DC1, In Group Policy Management Tool, Disable the two policies
    “DirectAccess Client Settings” and “DirectAccess Server” settings.
