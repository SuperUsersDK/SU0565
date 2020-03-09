**Chapter 3:**  
**Core Infrastructure Services**

**Exercise 1: Deploying AD DS**

**Task 1: Install AD DS**

1.  Start the virtual machine DC2.

2.  Change to DC1 virtual machine and signin with myorg\\administrator and
    Pa55w.rd.

3.  On DC1, in Server Manager, click Tools, and then click ´Active Directory
    Module for Windows PowerShell´

4.  At the command prompt in the Windows PowerShell command-line interface, type
    the following  
    command, and then press Enter:

*Install-WindowsFeature -Name AD-Domain-Services -ComputerName DC2*

1.  Type the following command to verify that the AD DS role is installed on
    DC2,  
    and then press Enter:

*Get-WindowsFeature -ComputerName DC2*

1.  In the output of the previous command, scroll up  
    and search for Active Directory Domain Services.

2.  Verify that this check box is selected. Search for Remote Server
    Administration Tools.  
    Look for the Role Administration Tools node below it, and then look for the
    AD DS and AD LDS Tools node

**Note:** Below the AD DS and AD LDS Tools node, only Active Directory module
for Windows PowerShell has been installed and not the graphical tools, such as
the Active Directory Administrative Center. If you centrally manage your
servers, you will not usually need these on each server. If you want to install
them, you need to specify the AD DS tools by running the Install-WindowsFeature
cmdlet with the  
RSAT-ADDS command name and -includeAllSubFeature -IncludeManagementTools.

1.  Type the following command to install the RSAT-ADDS feature on DC2,  
    and then press Enter:

*Install-WindowsFeature -Name RSAT-ADDS -ComputerName DC2*

**Note:** You might need to wait a short time after the installation process
completes before verifying that the AD DS role has installed. If you do not see
the expected results from the Get-WindowsFeature command, you can try again
after a few minutes

**Task 2: Prepare the AD DS installation and promote a remote server**

1.  Add DC2 to Server Manager on DC1

2.  On DC1, in Server Manager, select the All Servers view

3.  On the Manage menu, click Add Servers

4.  In the Add Servers dialog box, maintain the default settings, and then click
    Find Now

5.  In the Active Directory list of servers, select DC2,  
    click the arrow to add it to the Selected list, and then click OK

6.  Remotely configure AD DS by using Server Manager

7.  From DC1, ensure that the installation of the AD DS role on DC2 is complete
    and that the server was added to Server Manager. Then click the
    Notifications flag symbol

8.  Note the post-deployment configuration of DC2,  
    and then click the Promote this server to a domain controller link

9.  In the Active Directory Domain Services Configuration Wizard,  
    on the Deployment Configuration page, under Select the deployment operation,  
    verify that Add a domain controller to an existing domain is selected

10. Ensure that the Myorg.local domain is specified, and then in the Supply the
    credentials to perform this operation section, click Change

11. In the Credentials for deployment operation dialog box, in the User name
    box,  
    type Myorg\\Administrator, and then in the Password box, type Pa55w.rd  
    Click OK, and then click Next

12. On the Domain Controller Options page,  
    clear the selection Global Catalog (GC)  
    Ensure that Read-only domain controller (RODC) is also cleared

13. In the Type the Directory Services Restore Mode (DSRM) password section,  
    type and confirm the password Pa55w.rd, and then click Next

14. On the Additional Options page, click Next

15. On the Paths page, keep the default path settings for the Database folder,  
    Log files folder, and SYSVOL folder, and then click Next

16. On the Review Options page,  
    click **View script** to open the generated Windows PowerShell script

17. In Notepad, edit the generated Windows PowerShell script

18. Delete the comment lines that begin with the number sign (\#)

19. Remove the Import-Module line

20. Remove the grave accents (‘) at the end of each line

21. Remove the line breaks

22. Now the lnstall-ADDSDomainController command and all the parameters are on
    one line.  
    Place the cursor in front of the line, and then press Shift+ End to select
    the whole line.  
    On the menu, click Edit, and then click Copy

23. Switch to the Active Directory Domain Services Configuration Wizard, and
    then click **Cancel**

24. When prompted for confirmation, click Yes to cancel the wizard

25. Switch to Server Manager on DC1, click Tools, and then click ´Active
    Directory Module for Windows PowerShell´

26. At the Windows PowerShell command prompt, type the following command:

*Invoke-Command -ComputerName DC2 { }*

1.  Place the cursor between the braces { },  
    and then paste the content of the copied script line from the clipboard.  
    The whole line should now be as follows:

*Invoke-Command -ComputerName DC2 {Install-ADDSDomainController*  
*-NoGlobalCatalog:\$true -Credential (Get-Credential)
-CriticalReplicationOnly:\$false*  
*-DatabasePath “C:\\Windows\\NTDS" -DomainName “Myorg.local” -InstallDns:\$true*  
*-LogPath “C:\\Windows\\NTDS” -NoRebootonCompletion:\$false -SiteName
“Default-First-Site-Name"*  
*-SysvolPath “C:\\Windows\\SYSVOL" -Force:\$true }*

1.  Press Enter to start the command

2.  In the Windows PowerShell Credential Request dialog box,  
    type Myorg\\Administrator in the User name box, type Pa55w.rd in the
    Password box,  
    and then click OK

3.  When prompted for the password, in the SafeModeAdministratorPassword text
    box,  
    type Pa55w.rd and then press Enter

4.  When prompted for confirmation, in the Confirm password text box,  
    type Pa55w.rd, and then press Enter

5.  Wait until the command runs and the Status Success message is returned  
    There will be some yellow lines with warnings – that is expected  
    The DC2 virtual machine should restart

6.  Close Notepad without saving the file

7.  After DC2 restarts, on DC1, switch to Server Manager, and on the left side,  
    click the AD DS node. Note that DC2 has been added as a server and that the
    warning notification has disappeared. **You might have to click Refresh**.

**Task 3: Run the AD DS Best Practices Analyzer**

1.  On DC1, in Server Manager, go to the AD DS dashboard view

2.  Scroll down to the Best Practices Analyzer section, click the Tasks menu,  
    and then click Start BPA Scan

3.  In the Select Servers dialog box, select DC1.Myorg.local and DC2.Myorg.local

4.  Click Start Scan, and then wait until the Best Practices Analyzer (BPA)
    finishes the scan

5.  Review the results of the BPA

6.  Leave the virtual machines running after this exercise.

Results: After this exercise, you should have successfully created a new domain
controller and reviewed the Active Directory Domain Services (AD DS) Best
Practices Analyzer (BPA) results for that domain controller.
