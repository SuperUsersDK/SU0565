**Chapter 10: PowerShell**

**Exercise 1: Using Windows PowerShell to Manage Servers**

**Task 1: Use Windows PowerShell to connect remotely to servers and view
information**

1.  Sign in to DC1 with the Myorg\\Administrator account and the password
    Pa55w.rd

2.  In the Server Manager console, click LAB-2

3.  Right-click SVR2, and then click Windows PowerShell

4.  At the command prompt, type the following, and then press Enter:

*Import-Module ServerManager*

1.  To review the roles and features installed on SVR2,  
    at the command prompt, type the following, and then press Enter:

*Get-WindowsFeature*

1.  To review the running services on SVR2,  
    at the command prompt, type the following, and then press Enter:

*Get-service \| where-object {\$_.status -eq "Running"}*

1.  To view a list of processes on SVR2,  
    at the command prompt, type the following, and then press Enter:

*Get-process*

1.  To review the lP addresses assigned to the server, at the command prompt,  
    type the following, and then press Enter:

*Get-NetIPAddress \| Format-table*

1.  To review the most recent 10 items in the security log,  
    at the command prompt, type the following, and then press Enter:

*Get-EventLog Security -Newest 10*

1.  Close Windows PowerShell

**Task 2: Use Windows PowerShell to remotely install new features**

1.  On DC1, start Windows PowerShell

2.  To verify that the “Containers” feature has not been installed on SVR2,  
    type the following command, and then press Enter:

*Get-WindowsFeature -ComputerName SVR2*

1.  To deploy the Containers feature on SVR2,  
    type the following command, and then press Enter:

*Install-WindowsFeature Containers -ComputerName SVR2*

1.  To verify that the Containers feature has now been deployed on SVR2,  
    type the following command, and then press Enter:

*Get-WindowsFeature —ComputerName SVR2*

1.  In the Server Manager console, from the Tools drop-down menu, click Windows
    PowerShell ISE.

2.  In the Windows PowerShell ISE window, in the Untitledl.psl script pane,  
    type the following, pressing Enter after each line:

*Import-Module ServerManager*

*Install-WindowsFeature WINS -ComputerName SVR1*

*Install-WindowsFeature WINS -ComputerName SVR2*  


1.  Click the Save icon

2.  Select the root of Local Disk (C:)

3.  Create a new folder named Scripts, and then save the script in that folder
    as InstallWins.ps1

4.  To run the script, press the F5 key

5.  After this exercise leave the virtual machines running.

Results: After you complete this exercise, you should have used Windows
PowerShell to perform a remote installation of features on multiple servers.
