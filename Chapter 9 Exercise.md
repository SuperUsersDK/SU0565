**Chapter 9 Redundancy in Windows Server 2019**

**Lab: Implementing failover clustering**

**Exercise 1: Creating a failover cluster**

*Virtual machines: DC1, SVR1, SVR2, SVR3*

*Pre-steps to go through before the exercise*

1.  *SVR3 is a workgroup server and needs to be domain joined for this
    exercise.*

2.  *Start SVR3 and sign in with administrator and Pa55w.rd.*

3.  *From Server Manager – Local Server, click on Workgroup link and Domain join
    the server to Myorg.local with myorg.local\\administrator and Pa55w.rd.*

4.  *After restart, sign in with myorg\\administrator and Pa55w.rd*

**Task 1: Connect cluster nodes to iSCSI shared storage**

1.  Start and Sign in to SVR1 as myorg\\administrator with Pa55w.rd, Open
    PowerShell and and write;

*Install-WindowsFeature FS-iSCSITarget-Server -IncludeManagementTools*

1.  In Server Manager, go to File and Storage Services, and then go to iSCSI

2.  Create an iSCSI virtual disk with the following values:

Storage location: C:

Disk name: iSCSIDisk1

Size: 5 GB

1.  Create a new iSCSI target with the following values:

Target name: SVR1

Two iSCSI initiators with the following IP addresses:

IP Address: 172.16.0.22

IP Address: 172.16.0.23

1.  Repeat step 3 and 4 to create two more iSCSI virtual disks  
    with disk names iSCSIDisk2 and iSCSIDisk3

2.  On SVR2, open Server Manager, start the iSCSI Initiator, select yes to the
    prompt,  
    and then configure Discover Portal with the IP address 172.16.0.21

3.  In the Targets list, connect to the discovered target

4.  Repeat steps 6 and 7 on SVR3

5.  Shut down SVR3

6.  On SVR2, open Disk Management

7.  Bring the three new disks online, and then initialize them. These are disks
    1 through 3

8.  Create a simple volume on each disk, and format it with NTFS.  
    Label the disks Data1, Data2, and Data3 respectively

9.  Shut down SVR2 and start up SVR3

10. On SVR3, open Disk Management, and then bring the three new disks online

11. Start up SVR2 again

12. On SVR2, install the Failover Clustering feature by using Server Manager

13. On SVR3, install the Failover Clustering feature by using Server Manager

  
**Task 2: Validate the servers for failover clustering**

1.  On SVR2, open the Failover Cluster Manager console

2.  Before running the Validation Configuration Wizard make sure that all
    services have started on SVR2 and SVR3. Refresh Server Manager until no
    service warnings are there.

3.  Open the Validate Configuration Wizard

4.  Use SVR2 and SVR3 as nodes for test

5.  Run all tests

6.  There should be no errors, but you will receive some warning messages

**Task 3: Create the failover cluster**

1.  Create a cluster with the following parameters:

Servers: SVR2 and SVR3

Cluster Name: Cluster1

Address: 172.16.0.125

**Task 4: Add the file-server application to the failover cluster**

1.  On SVR2, open the Failover Cluster Manager console

2.  In the Storage node, click Disks, and verify that three cluster disks are
    online

3.  Add File Server as a cluster role. Select the File Server for general use
    option

4.  Specify MyOrgFS as Client Access Name, use 172.16.0.130 as the address,  
    and use Cluster Disk 2 as the storage

5.  Close the Failover Cluster window.

**Task 5: Add a shared folder to a highly available file server**

1.  On SVR3, from Server Manager, open the Failover Cluster Manager console

2.  Open the New Share Wizard, and add a new shared folder to the MyOrgFS
    cluster role

3.  Specify the File share profile as SMB Share – Quick

4.  Accept the default values on the Select the server and the path for this
    share page

5.  Name the shared folder Docs

6.  Accept the default values on the Configure share settings  
    and Specify permissions to control access pages

7.  At the end of the New Share Wizard, create the share

**Task 6: Configure failover and failback settings**

1.  On SVR3, in the Failover Cluster Manager console,  
    open the Properties for the MyOrgFS cluster role

2.  Enable failback between 4 and 5 hours

3.  Select both SVR2 and SVR3 as the preferred owners

4.  Move SVR3 to the first space in the preferred owners list

**Task 7: Validate the highly available file-server deployment**

1.  On DC1, open File Explorer, and then attempt to access the \\\\MyOrgFS\\
    location.  
    Verify that you can access the Docs folder

2.  Create a text document inside this folder named test.txt

3.  Verify the current owner of MyOrgFS

Note: The owner will be SVR2 or SVR3.

1.  On SVR2, in the Failover Cluster Manager console, move MyOrgFS to the second
    node

2.  On DC1, in File Explorer, verify that you can still access the \\\\MyOrgFS\\
    location

**Task 8: Validate the failover and quorum configuration for the File Server
role**

1.  On SVR2, determine the current owner for the MyOrgFS role

2.  Stop the Cluster service on the node that is the current owner of the
    MyOrgFS role

3.  Try to access \\\\MyOrgFS\\ from DC1 to verify that MyOrgFS has moved to
    another node  
    and that the \\\\MyOrgFS\\ location is still available

4.  Start the Cluster service on the node in which you stopped it in step 2

5.  Browse to the Disks node, and take the disk marked as Disk Witness in Quorum
    offline

6.  Verify that MyOrgFS is still available by trying to access it from DC1

7.  Bring the disk witness online

8.  Open Cluster Quorum Settings

9.  Choose to perform advanced configuration

10. Change the witness disk to Cluster Disk 3. Do not make any other changes

11. After this exercise leave the virtual machines running.

Results: After completing this exercise, you should have created a failover
cluster successfully, configured a highly available file server, and tested the
failover scenarios.
