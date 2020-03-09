**Chapter 5**  
**Networking with Windows Server 2019**

**Exercise 1: Preparing to deploy Network Controller**

**Task 1: Create the required Active Directory Domain Services security groups**

1.  Switch to DC1

2.  In Server Manager, click Tools, and then click Active Directory Users and
    Computers

3.  In Active Directory Users and Computers, expand **Myorg.local**, and then
    click **Sjælland**

4.  Right-click ´**IT Afdelingen**´, click New, and then click Group

5.  In the New Object — Group dialog box, in the Group name text box,  
    type **Network Controller Admins**, and then click OK

6.  In the details pane, double-click **Network Controller Admins**,  
    and then in the Network Controller Admins Properties dialog box, on the
    Members tab,  
    click Add

7.  In the Select Users, Contacts, Computers, Service Accounts, or Groups dialog
    box,  
    in the Enter the object names to select (examples) text box, type
    **administrator; Ann**,  
    and then click OK twice

8.  Right-click ´**IT Afdelingen**´, click New, and then click Group

9.  In the New Object — Group dialog box, in the Group name text box, type  
    *Network Controller Ops*, and then click OK

10. In the details pane, double-click Network Controller Ops, and then in the  
    **Network Controller Ops** Properties dialog box, on the Members tab, click
    Add

11. In the Select Users, Contacts, Computers, Service Accounts, or Groups dialog
    box,  
    in the Enter the object names to select (examples) text box, type
    **administrator; Ann**,  
    and then click OK twice

12. Close Active Directory Users and Computers

**Task 2: Request a certificate for authenticating Network Controller**

1.  Start SVR2 Virtual Machine.

2.  Sign-in to SVR2 as Myorg\\administrator and Pa55w.rd, right-click Start, and
    then click Run

3.  In the Run dialog box, type mmc.exe, and then press Enter

4.  In the Console — [Console Root] window, click File, and then click
    Add/Remove Snap-in

5.  In the **Add or Remove Snap-ins** dialog box, in the Snap-in list,
    double-click **Certificates**

6.  Click the **Computer account**, click Next, click **Finish**, and then click
    **OK**

7.  In the navigation pane, expand Certificates (Local Computer), and then click
    Personal

8.  Right-click Personal, click All Tasks, and then click Request New
    Certificate

9.  In the Certificate Enrollment dialog box, on the Before you Begin page,
    click Next

10. On the Select Certificate Enrollment Policy page, click Next

11. Select the Computer check box, click Enroll, and then click Finish

12. Close the management console and do not save changes

13. Rename “Ethernet 2” on SVR2 to London_Network

Results: After completing this exercise, you should have successfully prepared
your environment for Network Controller

**Exercise 2: Deploying Network Controller**

**Task 1: Add the Network Controller role**

Note:

The **New-NetworkControllerNodeObject** cmdlet creates a network controller node
object. This cmdlet is used for configuring a network controller for the first
time. The object created from this cmdlet is passed to the
**Install-NetworkControllerCluster** cmdlet to create a network controller
cluster.

The normal steps for configuring a network controller are:

Install the network controller role on all the computers that will be
functioning as a network controller in your deployment.

From one of those computers (or any other remote computer), run the
**New-NetworkControllerNodeObject** cmdlet to enter the details of the node to
be part of the deployment. Repeat this step for all the computers that are part
of the deployment. These node objects will be passed as a parameter to the next
cmdlet.

Run the **Install-NetworkControllerCluster** cmdlet to create a new network
controller cluster.

Run the **Install-NetworkController** cmdlet to create the network controller
application on top of the cluster.

1.  On SVR2, click Start, and then click Server Manager

2.  In Server Manager, in the details pane, click Add roles and features

3.  In the Add Roles and Features Wizard, on the Before you begin page, click
    Next

4.  On the Select installation type page, click Next

5.  On the Select destination server page, click Next

6.  On the Select server roles page, in the Roles list, select the **Network
    Controller** check box,  
    click Add Features, and then click Next

7.  On the Select features page, click Next

8.  On the Network Controller page, click Next

9.  On the Confirm installation selections page, click Install

10. When the role installs, click Close

11. Right-click Start, point to Shut down or sign out, and then click Restart

12. In the Choose a reason that best describes why you want to shut down this
    computer  
    dialog box, click Continue

13. After SVR2 restarts, sign in as Myorg\\Administrator with the password
    Pa55w.rd.

**Task 2: Configure the Network Controller cluster**

1.  On SVR2, right-click Start, and then click Windows PowerShell (Admin)

2.  At the Windows PowerShell (Admin) command prompt, type the following
    command,  
    and then press Enter:

\$node=New-NetworkControllerNodeObject -Name "Node1" -Server "SVR2.Myorg.local"
-FaultDomain "fd:/rack1/host1" -RestInterface “London_Network”

The first command creates a network controller node object, and then stores it
in the \$node variable. The fully qualified domain name of the computer is
SVR2.contoso.com and the interface on the computer listening to REST requests is
named London_Network.

1.  At the Windows PowerShell (Admin) command prompt, type the following
    command,  
    and then press Enter:

\$Certificate = Get-Item Cert:\\LocalMachine\\My \| Get-ChildItem \| where
{\$_.Subject -imatch "SVR2" }

The second command gets the certificate named SVR2, and then stores it in the
\$Certificate variable.

1.  At the Windows PowerShell (Admin) command prompt, type the following
    command,  
    and then press Enter:

Install-NetworkControllerCluster -Node \$node -ClusterAuthentication Kerberos
-ManagementSecurityGroup "Myorg\\Network Controller Admins"
-CredentialEncryptionCertificate \$Certificate

This command installs a Network Controller cluster in a domain-joined
environment. The authentication that is used between the cluster nodes is
Kerberos. To encrypt the credentials used to store Network Controller binaries
on disk, the user provides a certificate with subject name as SVR2.myorg.local.

**Task 3: Configure the Network Controller application**

1.  At the Windows PowerShell (Admin) command prompt, type the following
    command,  
    and then press Enter:

>   Install-NetworkController -Node \$node -ClientAuthentication Kerberos
>   -ClientSecurityGroup “Myorg\\Network Controller Ops“ -RestIpAddress
>   “172.16.0.240/24" -ServerCertificate \$Certificate

The final command Install-NetworkController deploys the network controller in a
test environment. Since single node is used in the deployment, there is no high
availability support. Network controller uses Kerberos authentication between
the cluster nodes, and between the REST clients and network controller. Only
clients that are part of the Network Controller Ops security group can
communicate with the network controller. The certificate in \$Certificate is
used to encrypt traffic between the REST clients and network controller.

**Task 4: Verify the deployment**

1.  (We will disable Kerberos authentication not to have any issues with
    authentication in the test Lab and server 2019 security – for more info see
    <https://docs.microsoft.com/en-us/windows-server/networking/sdn/security/kerberos-with-spn>
    )

Set-NetworkController -ClientAuthentication none

1.  At the Windows PowerShell (Admin) command prompt, type the following
    command,  
    and then press Enter:

\$cred=New-Object Microsoft.Windows.Networkcontroller.credentialproperties

1.  At the Windows PowerShell (Admin) command prompt, type the following
    command,  
    and then press Enter:

\$cred.type="usernamepassword"

1.  At the Windows PowerShell (Admin) command prompt, type the following
    command,  
    and then press Enter:

\$cred.username="admin"

1.  At the Windows PowerShell (Admin) command prompt, type the following
    command,  
    and then press Enter:

\$cred.value="abcd"

1.  At the Windows PowerShell (Admin) command prompt, type the following
    command,  
    and then press Enter:

New-NetworkControllerCredential -ConnectionUri https://SVR2.Myorg.local
-Properties \$cred -ResourceID cred1

Description. The **New**-**NetworkControllerCredential** cmdlet adds a **new**
device **credential** to the **Network Controller**. If the **credential** is
already present, this cmdlet will modify the properties of the **credential**.
**Network Controller** uses the device **credential** to connect to a southbound
device for managing the device.

1.  Press Y, and then press Enter when prompted

2.  At the Windows PowerShell (Admin) command prompt, type the following
    command,  
    and then press Enter:

Get-NetworkControllerCredential -ConnectionUri https://SVR2.Myorg.local
-ResourceID cred1

You should receive output that looks similar to the output below:

Tags :

ResourceRef : /credentials/cred1

CreatedTime : 1/1/0001 12:00:00 AM

InstanceId : e16ffe62—a701—4d31—915e—7234d4bc5a18

Etag : W/"lec5963l—607f—4d3e-ac78-94b0822f3a9d"

ResourceMetadata :

ResourceID : cred1

Properties : Microsoft.Windows.NetworkContro1ier.CredentiaiProperties

Results: After completing this exercise, you have successfully deployed Network
Controller

1.  Leave the virtual machines running after this exercise.
