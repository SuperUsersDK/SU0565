**Chapter 8: Server Core**

**Exercise 1: Configuring Windows Server 2019 Server Core**

**Task 1: Set computer name**

1.  Sign in to SRVCORE1 as Administrator with the password Pa55w.rd

2.  At the command prompt, type sconfig and press Enter

3.  To select Computer Name, type 2, and then press Enter

4.  Enter the computer name SVRCORE4, and then press Enter

5.  In the Restart dialog box, click Yes

6.  Sign in to server SVRCORE4(SVRCORE1 vm) using the Administrator account with
    the password Pa55w.rd

7.  At the command prompt, type hostname, and then press Enter to verify the
    computer's name

**Task 2: Change the computer's date and time**

1.  Ensure you are signed in to server SVRCORE4(SVRCORE1 vm) as Administrator
    with the password Pa55w.rd

2.  At the command prompt, type sconfig, and then press Enter

3.  To select Date and Time, type 9, and then press Enter

4.  In the Date and Time dialog box, Verify or Change time zone.  
    Set the time zone to the same time zone that your classroom uses

5.  In the Date and Time dialog box, verify that the date and time match those
    in your location  
    To dismiss the dialog boxes, click OK two times

6.  In the Command Prompt window, type 15, and then press Enter to exit Server
    Configuration

**Task 3: Configure the network**

1.  Ensure that you are signed in to server SVRCORE4(SVRCORE1 vm)  
    using the account Administrator and the password Pa55w.rd

2.  At the command prompt, type sconfig, and then press Enter

3.  To configure Network Settings, type 8, and then press Enter

4.  Type the index number of the network adapter that you want to configure,  
    and then press Enter

5.  On the Network Adapter Settings page, type 1,  
    and then press Enter. This sets the Network Adapter Address

6.  To select static IP address configuration, type S, and then press Enter

7.  At the Enter static IP address: prompt, type 172.16.0.39, and then press
    Enter

8.  At the Enter subnet mask prompt, type 255.255.255.0, and then press Enter

9.  At the Enter default gateway prompt, type 172.16.0.1, and then press Enter

10. On the Network Adapter Settings page, type 2, and then press Enter  
    This configures the DNS server address

11. At the Enter new preferred DNS server prompt, type 172.16.0.10, and then
    press Enter

12. In the Network Settings dialog box, click OK

13. To choose **not** to configure an alternate DNS server address, press Enter

14. Type 4, and then press Enter to return to the main menu

15. Type 15, and then press Enter to exit sconfig

16. At the command prompt, type ping dc.myorg.local  
    to verify connectivity to the domain controller from SVRCORE4(SVRCORE1 vm)

**Task 4: Add the server to the domain**

1.  Ensure that you are signed in to server SVRCORE4(SVRCORE1 vm)  
    using the account Administrator with the password Pa55w.rd

2.  At the command prompt, type sconfig, and then press Enter

3.  To switch to configure Domain/Workgroup, type 1, and then press Enter

4.  To join a domain, type D, and then press Enter

5.  At the Name of domain to join prompt, type myorg.local, and press Enter

6.  At the Specify an authorized domain\\user prompt, type Myorg\\Administrator,  
    and then press Enter

7.  At the Type the password associated with the domain user prompt, type
    Pa55w.rd,  
    and then press Enter

8.  At the Change Computer Name prompt, click No

9.  In the Restart dialog box, click Yes

10. Sign in to server SVRCORE4(SVRCORE1 vm) with the **Myorg\\**Administrator
    account and the password Pa55w.rd

Results: After you complete this exercise, you should have configured a Windows
Server 2019 Server Core deployment, verified the server's name and made it
domain member

**Exercise 2: Managing Servers**

**Task 1: Create a server group**

1.  Start SVR1 (Right click on SVR1 and select Start)

2.  Sign in to DC1 with the Administrator account and the password Pa55w.rd

3.  In the Server Manager console, click Dashboard, and then click Create a
    server group

4.  In the Server group name box, type LAB-2

5.  In the Create Server Group dialog box, click the Active Directory tab, and
    then click Find Now

6.  Use the arrow to add SVR1 and SVR2 to the server group.  
    Click OK to close the Create Server Group dialog box

7.  In the Server Manager console, click LAB-2  
    Press and hold the Ctrl key, and then select both SVR1 and SVR2

8.  Scroll down, and under the Performance section, select both SVR1 and SVR2

9.  Right-click and then click Start Performance Counters

**Task 2: Deploy features and roles to both servers**

1.  In Server Manager on DC1, click LAB-2

2.  Scroll to the top of the pane, right-click SVR1, and then click Add Roles
    and Features

3.  In the Add Roles and Features Wizard, click Next

4.  On the Select installation type page,  
    click Role-based or feature-based installation, and then click Next

5.  On the Select destination server page, verify that SVR1.Myorg.local is
    selected,  
    and then click Next

6.  On the Select server roles page, select Windows Deployment Services,  
    Accept the “Add features that are required for Windows Deployment Services”
    dialog  
    and then click Next

7.  On the Features page, select Windows Server Backup, and then click Next

8.  On the Select role services page, click Next

9.  On the Confirm installation selections page,  
    select the Restart the destination server automatically if required check
    box,  
    and then click Install

10. Click Close to close the Add Roles and Features Wizard

11. In Server Manager, right-click SVR2, and then click Add Roles and Features

12. In the Add Roles and Features Wizard, on the Before you begin page, Click
    Next

13. On the Select installation type page, click Role-based or feature-based
    installation. Click Next

14. On the Select destination server page, verify that SVR2.Myorg.local is
    selected,  
    and then click Next

15. On the Server Roles page, click Next

16. On the Select features page, click Windows Server Backup, and then click
    Next

17. On the Confirm installation selections page,  
    select the Restart the destination server automatically if required check
    box,  
    and then click Install

18. Once the install commences, click Close

19. In Server Manager, refresh the view, click the IIS node,  
    and then verify that SVR2 is listed.

**Task 3: Review services and change a service setting**

1.  Sign in to SVR2 with the Myorg\\Administrator account and the password
    Pa55w.rd

2.  In the Command Prompt window, type the following two commands,  
    and press Enter after each one:

*netsh.exe advfirewall firewall set rule group="remote desktop" new enable=yes*

*netsh.exe advfirewall firewall set rule group="remote event log management" new
enable=yes*

1.  Sign in to DC1 with the Myorg\\Administrator account and the password
    Pa55w.rd

2.  In Server Manager, click LAB-2

3.  Right-click SVR2, and then click Computer Management

4.  In the Computer Management console, expand Services and Applications,  
    and then click Services (can take a while)

5.  Right-click the World Wide Web Publishing service, and then click
    Properties.  
    Verify that the Startup type is set to Automatic

6.  In the World Wide Web Publishing Service dialog box, on the Log On tab,  
    verify that the service is configured to use the Local System account

7.  On the Recovery tab, configure the following settings

First failure: Restart the Service

Second failure: Restart the Service

Subsequent failures: Restart the Computer

Reset fail count after: 1 days

Restart service after: 1 minute

1.  Click the Restart Computer Options button

2.  In the Restart Computer Options dialog box,  
    in the Restart Computer After box, type 2, and then click OK

3.  Click OK to close the World Wide Web Publishing Services Properties dialog
    box

4.  Close the Computer Management console

5.  After this exercise leave the virtual machines running.

Results: After you complete this exercise, you should have created a server
group,  
deployed roles and features, and configured the properties of a service.
