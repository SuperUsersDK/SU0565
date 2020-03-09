**Chapter 2:**  
**Installing and Managing Windows Server 2019**

**Exercise 1: Deploying Windows Server 2019**

**Task 1: Install the Windows Server 2019 server**

**(Make sure that DC1 is running – if not then right click on DC1 and select
“Start”)**

1.  Open the Hyper-V Manager console

2.  Click SVR4

3.  In the Actions pane, click Settings

4.  Under Hardware, click DVD Drive

5.  Click Image file, and then click Browse

6.  Browse to C\\iso and then click “Server2019.iso”

7.  Click Open, and then click OK

8.  In the Actions pane, click Settings

9.  Under Firmware Select DVD Drive and move Up to first place and click OK.

10. In the Hyper-V Manager console, double-click SVR4

11. In the Virtual Machine Connection Window, in the Action menu, click Start

12. Click inside the window and press Space within 10 seconds

13. In the Windows Setup Wizard, on the Windows Server 2019 page,  
    verify the following settings:

14. Language to install: English (United States)

15. Time and currency format: Danish (Denmark)

16. Keyboard or input method: Danish

17. Click Next

18. On the Windows Server 2019 page, click Install now

19. On the Select the operating system you want to install page,  
    select Windows Server 2019 Datacenter (Desktop Experience)

20. On the License terms page, review the operating system license terms,  
    select the l accept the license terms check box, and then click Next

21. On the Which type of installation do you want? page,  
    click Custom: Install Windows only (advanced)

22. On the Where do you want to install Windows? page,  
    verify that Drive 0 Unallocated Space has enough space for  
    the Windows Server 2019 operating system, and then click Next.

*Note:* Depending on the speed of the equipment, the installation takes about 5
minutes.  
The virtual machine will restart during this process.

1.  On the Settings page, in both the Password and Reenter password boxes,  
    enter the password Pa55w.rd, and then click Finish.

**Task 2: Change the server name**

1.  Sign in to SVR4 as Administrator with the password Pa55w.rd

2.  In Server Manager, click Local Server

3.  Click the randomly generated name next to Computer name

4.  In the System Properties dialog box, on the Computer Name tab, click Change

5.  In the Computer Name/Domain Changes dialog box, in the Computer name text
    box,  
    enter the name SVR4, and then click OK.

6.  In the Computer Name/Domain Changes dialog box, click OK

7.  Close the System Properties dialog box

8.  In the Microsoft Windows dialog box, click Restart Now

**Task 3: Change the date and time**

1.  Sign in to server SVR4 as Administrator with the password Pa55w.rd

2.  On the taskbar, click the time display.  
    A pop-up window with a calendar and a clock appears

3.  In the pop-up window, click “Date and time”

4.  In the Time Zone Settings, verify or set the time zone to your current time
    zone

5.  Verify or set “Set time automatically” to on

6.  Close the Date and Time settings

**Task 4: Configure the network**

1.  On SVR6, in the Server Manager console, click Local Server

2.  In the Server Manager console, next to Ethernet,  
    click “IPv4 address assigned by DHCP, IPv6 Enabled”

3.  In the Network Connections dialog box, right-click Ethernet, and then click
    Properties

4.  In the Ethernet Properties dialog box,  
    click Internet Protocol Version 4 (TCP/IPv4),  
    and then click Properties

5.  In the Internet Protocol Version 4 (TCP/IPv4) Properties dialog box,  
    click “Use the following IP address”,  
    enter the following IP address information

6.  IP address: 172.16.0.26

7.  Subnet Mask: 255.255.255.0

8.  Default Gateway: 172.16.0.1

9.  Preferred DNS server: 172.16.0.10

10. then click OK to “Internet Protocol Version 4 (TCP/IPv4) Properties” dialog
    box

11. then click OK to “Ethernet Properties” dialog box

**Task 5: Add the server to the domain**

1.  On SVR4, in the Server Manager console, click Local Server

2.  Next to Workgroup, click WORKGROUP

3.  In the System Properties dialog box, on the Computer Name tab, click Change

4.  In the Computer Name/Domain Changes dialog box, in the Member Of area,  
    click the Domain option

5.  In the Domain box, type myorg.local, and then click OK

6.  In the Windows Security dialog box, enter the following details, and then
    click OK:  
    Username: Administrator  
    Password: Pa55w.rd

7.  In the Computer Name/Domain Changes dialog box, click OK

8.  When informed that you must restart the computer to apply the changes, click
    OK

9.  In the System Properties dialog box, click Close

10. In the Microsoft Windows dialog box, click Restart Now

11. After SVR4 restarts, sign in as myorg\\Administrator with the password
    Pa55w.rd

    1.  If prompted for password change, choose the password Pa55w.rd1 as a new
        password.

12. After this exercise leave the virtual machines running.

Results: After completing this exercise, you should have deployed Windows Server
2019 on SVR4  
You also should have configured SVR4, including name change, date and time,
Domain and networking
