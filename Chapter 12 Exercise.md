**Chapter 12:**  
**Virtualizing Your Data Center with Hyper-V**

**Exercise 1: Installation of the Hyper-V server role**

**Task 1: Configuring prerequisites of the Microsoft Hyper-V server role for
nested virtualization**

1.  For this exercise We only need DC1 and SVR2 Shutdown all other machines.

2.  To be able to run the next command on the host you also need to shutdown
    SVR2.

3.  On the physical host run PowerShell and type

*Set-VMProcessor –VMName SVR2 –ExposeVirtualizationExtensions \$True*

Or – if we want to do this for all virtual machines on this host

*Get-VM \| Set-VMProcessor –ExposeVirtualizationExtensions \$True*

1.  In Hyper-V add a NIC to SVR2 and connect it to the Internal network

2.  Change memory allocation on SVR2 to 12.288 MB and disable dynamic memory
    allocation.

3.  Change number of cores on SVR2 to 4

4.  Start SVR2 and sign in as myorg\\Administrator using the password Pa55w.rd

5.  Click Start, and then click Server Manager

6.  In Server Manager, add the Role Hyper-V Manager

7.  Accept the Feature Required include management tolls by clicking Add
    Features

8.  Click Next 3 times

9.  Select “Ethernet 3” in the “Create Virtual Switch”

10. Click Next 3 times and then Install

Results: After completing this part, you have installed the Hyper-V server role
on a server SVR2

**Exercise 2: Configuring Hyper-V networks**

**Task 1: Create an external network**

1.  In Hyper-V Manager (on SVR2), click SVR2, and then in the Actions pane,  
    click Virtual Switch Manager

2.  In the Virtual Switch Manager for SVR2 window,  
    in the left pane, click New virtual network switch

3.  In the Create virtual switch pane, click External, and then click Create
    Virtual Switch

4.  In the Virtual switch properties pane, in the Name text box, type Physical
    Network

5.  In the Connection type area, click External network.  
    Select the Allow management operating system to share this network adapter
    check box,  
    and then click OK

6.  In the Apply Networking Changes dialog box, read the warning that displays,
    and then click Yes

7.  In Server Manager, click Local Server, and then verify that the name of the
    network adapter  
    has changed to vEthernet (xxxxxxxxx)

**Task 2: Create a private network**

1.  On SVR2, in Hyper-V Manager, in the Actions pane, click Virtual Switch
    Manager

2.  In the Virtual Switch Manager for SVR2 window, in the left pane,  
    click New virtual network switch

3.  In the Create virtual switch pane, click Private, and then click Create
    Virtual Switch

4.  In the Virtual Switch Properties pane, in the Name text box, type Isolated
    Network

5.  In the Connection type area, verify that Private network is selected, and
    then click OK

6.  In Server Manager, verify that no new network adapters are visible

**Task 3: Create an internal network**

1.  On SVR2, in Hyper-V Manager, in the Actions pane, click Virtual Switch
    Manager

2.  In the Virtual Switch Manager for SVR2 window, in the left pane,  
    click New virtual network switch

3.  In the Create virtual switch pane, click Internal, and then click Create
    Virtual Switch

4.  In the Virtual Switch Properties pane, in the Name text box, type Host
    Internal Network

5.  In the Connection type area, verify that Internal network is selected, and
    then click OK

6.  In Server Manager, verify that a new network adapter named  
    vEthernet (Host Internal Network) has been created

Results: After completing this part, you have configured an external, internal,
and private network

**Exercise 3: Creating and configuring virtual machines**

**Task 1: Create a Generation 2 virtual machine**

1.  In Hyper-V Manager, in the Actions pane, click New, and then click Virtual
    Machine

2.  In the New Virtual Machine Wizard, on the Before You Begin page, click Next

3.  On the Specify Name type “SVR10” and click next

4.  On the Specify Generation page, click Generation 2, and then click Next

5.  On the Assign Memory page, in the Startup memory box, enter a value of 4096
    MB, select Dynamic memory and then click Next

6.  On the Configure Networking page, click Isolated Network, and then click
    Next

7.  On the Connect Virtual Hard Disk page, click Create a virtual hard disk

8.  In the Size text box, type 20, and then click Next

9.  In the Installation Options select ISO image from “C:\\iso\\Server2019.iso”

10. Click Next and then Finish

11. Connect and start SVR10

12. If you get an error while trying to start the SVR10 Installation you need to
    expand SVR2´s harddisk. **If no error you can jump to step 18**

13. In the Hyper-v Manager in SVR2 click on Edit disk

    ![](media/da85ba7a08d2c10269b506540037e75d.png)

14. Browse and point out SVR2_XXXXX Harddisk and OK.

![](media/6d7742529d3a24ae8302d5ec92687443.png)

1.  Select Next and Next again to Expand the disk

    ![](media/705d37d747fa1f484a7643b93c4bb566.png)

2.  Type in 80 to expand the disk from 41 to 80 GB size and press Finish.

3.  Open Disk Management inside SVR2 Virtual machine and right-click and expand
    C:\\ and Close Disk Management. Now try to start SVR10 one more time.

4.  In the Windows Setup window, Select DK Keyboard and click Next, and then
    click Install now

5.  On the Select the operating system you want to install page,  
    click Windows Server 2019 Datacenter (Desktop Experience), and then click
    Next

6.  On the Applicable notices and license terms page,  
    select the I accept the license terms check box, and then click Next

7.  On the Which type of installation do you want page,  
    click Custom: Install Windows only (advanced)

8.  On the Where do you want to install Windows page, click Drive 0 Unallocated
    Space,  
    and then click Next

Results: After completing this exercise, you have created a Generation 2 virtual
machine
