**Chapter 7**  
**Hardening and Security**

**Lab:**  
**Configuring Windows Firewall with Advanced Security**

**Exercise 1: Creating and testing inbound rules**

**Task 1: Test connectivity to the default website on SVR1**

1.  Start DC1, SVR1, SVR2 and CL1

2.  If not installed make sure to install Web service on SVR1 and SVR2

3.  Log on to SVR2 with user myorg\\administrator and password Pa55w.rd

4.  On SVR2, click the Start button, and then click the Windows PowerShell icon

5.  In the Administrator: Windows PowerShell window, type the following command,  
    and then press Enter:

*Test-NetConnection -ComputerName SVR1.myorg.local -CommonTCPPort HTTP*

1.  The results should display "TcpTestSucceeded: True"

2.  In the Administrator: Windows PowerShell window, type the following command,  
    and then press Enter:

*Test-NetConnection -ComputerName SVR1.myorg.local -Port 80*

1.  The results should display "TcpTestSucceeded: True"

**Task 2: Configure IIS on SVR1**

1.  Sign-in to SVR1 as Myorg\\administrator and Pa55w.rd, right-click Start, and
    then click Run

2.  In the Run dialog box, type mmc.exe, and then press Enter

3.  In the Console — [Console Root] window, click File, and then click
    Add/Remove Snap-in

4.  In the **Add or Remove Snap-ins** dialog box, in the Snap-in list,
    double-click **Certificates**

5.  Click the **Computer account**, click Next, click **Finish**, and then click
    **OK**

6.  In the navigation pane, expand Certificates (Local Computer), and then click
    Personal

7.  Right-click Personal, click All Tasks, and then click Request New
    Certificate

8.  In the Certificate Enrollment dialog box, on the Before you Begin page,
    click Next

9.  On the Select Certificate Enrollment Policy page, click Next

10. Select the Computer check box, click Enroll, and then click Finish

11. Click the Start button, and then click Windows Administrative Tools

12. Open Internet Information Services (llS) Manager

13. In Internet Information Services (IIS) Manager, expand SVR1
    (Myorg\\Administrator),  
    expand Sites, and then click Default Web Site

14. In the Actions pane, click Bindings

15. In the Site Bindings dialog box, click http, and then click Edit

16. In the Edit Site Bindings dialog box, in the Port text box, type 8080, and
    then click OK

17. In the Site Bindings dialog box, click Add

18. Select Type: https

19. Select SSL certificate SVR1.myorg.local and press OK

20. In the Site Bindings dialog box, click https, and then click Edit

21. In the Edit Site Bindings dialog box, in the Port text box, type 4443, and
    then click OK

22. In the Site Bindings dialog box, click Close

23. Right-click Default Web Site, point to Manage Website, and then click
    Restart

**Task 3: Test the connectivity to the default website on SVR1**

1.  On SVR2, in the Administrator: Windows PowerShell window, type the following
    command,  
    and then press Enter:

*Test-NetConnection -ComputerName svr1.myorg.local -Port 8080*

1.  The results should display "WARNING: TCP connect to SVR1.myorg.local:8080
    failed”

2.  In the Administrator: Windows PowerShell window, type the following command,  
    and then press Enter:

*Test-NetConnection -ComputerName SVR1.myorg.local -Port 4443*

1.  The results should display "WARNING: TCP connect to svr1.myorg.local:4443
    failed”

**Task 4: Create inbound rules on SVR1**

1.  On SVR1, click Start, expand Windows Administrative Tools,  
    and then click Windows Firewall with Advanced Security

2.  Click Inbound Rules, right-click Inbound Rules, and then click New Rule

3.  In the New Inbound Rule Wizard, on the Rule Type page, click Port, and then
    click Next

4.  On the Protocol and Ports page, in the Specific local ports text box, type
    8080,  
    and then click Next

5.  On the Action page, click Next

6.  On the Profile page, click Next

7.  On the Name page, in the Name text box, type HTTP8080, and then click Finish

8.  Click Inbound Rules, right-click Inbound Rules, and then click New Rule

9.  In the New Inbound Rule Wizard, on the Rule Type page, click Port, and then
    click Next

10. On the Protocol and Ports page, in the Specific local ports text box, type
    4443,  
    and then click Next

11. On the Action page, click Next

12. On the Profile page, click Next

13. On the Name page, in the Name text box, type HTTPS4443, and then click
    Finish

**Task 5: Test connectivity to the default website on SVR1**

1.  On SVR2, in the Administrator: Windows PowerShell window,  
    type the following command, and then press Enter:

*Test-NetConnection -ComputerName svr1.myorg.local -Port 8080*

1.  The results should display "TcpTestSucceeded: True"

2.  In the Administrator: Windows PowerShell Window, type the following command,  
    and then press Enter:

*Test-NetConnection -ComputerName svr1.myorg.local -Port 4443*

1.  The results should display "TcpTestSucceeded: True"

Results: After completing this exercise, you have configured an inbound security
rule  
in Windows Firewall with Advanced Security.

**Exercise 2: Creating and testing outbound rules**

**Task 1: Create an outbound rule**

1.  On CL1, Right-click Start and select Run, type WF.msc, and then press Enter

2.  Click Outbound Rules, right-click Outbound Rules, and then click New Rule

3.  In the New Outbound Rule Wizard, on the Rule Type page, click Port, and then
    click Next

4.  On the Protocol and Ports page, in the Specific remote ports text box, type
    4443,  
    and then click Next

5.  On the Action page, ensure that Block the connection is selected,  
    and then click Next

6.  On the Profile page, click Next

7.  On the Name page, in the Name text box, type Block_HTTPS4443, and then click
    Finish

**Task 2: Test the rule**

1.  On CL1, click Start, type IExplore.exe, and then press Enter

2.  In the Internet Explorer address bar, type HTTP://SVR1.Myorg.local:8080,  
    and then press Enter

3.  The default Microsoft Internet Information Services (IIS) webpage appears

4.  In the Internet Explorer address bar, click the Home button

5.  In the Internet Explorer address bar, type HTTPS://SVR1.Myorg.local:4443,  
    and then press Enter

6.  The “Can't reach this page” error appears in the browser

Results: After completing this exercise, you should have configured an outbound
security rule in Windows Defender Firewall with Advanced Security.

**Exercise 3:**  
**Creating and testing connection security rules**

**Task 1: Configure a secure server GPO**

1.  On DC1, in Server Manager, click Tools, and then click Group Policy
    Management

2.  Expand Forest:Myorg.local, expand Domains, and then expand Myorg.local

3.  Right-click Myorg.local, and then click New Organizational Unit

4.  In the New Organizational Unit dialog box, type Secure Servers, and then
    click OK

5.  Right-click Secure Servers, and then click Create a GPO in this domain, and
    Link it here

6.  In the New GPO dialog box, type Require IPsec, and then click OK

7.  Expand Secure Servers, right-click Require IPsec, and then click Edit

8.  Under Computer Configuration, expand Policies, expand Windows Settings,  
    expand Security Settings, expand Windows Firewall with Advanced Security,  
    and then expand the next Windows Firewall with Advanced Security node

9.  Click and then right-click Connection Security Rules, and then click New
    Rule

10. In the New Connection Security Rule Wizard, click Isolation, and then click
    Next

11. On the Requirements page, click Require authentication for inbound
    connections  
    and request authentication for outbound connections, and then click Next

12. On the Authentication Method page, click Computer (Kerberos V5), and then
    click Next

13. On the Profile page, click Next

14. On the Name page, in the Name box, type Require Security, and then click
    Finish

15. Close the Group Policy Management Editor

**Task 2: Configure a secure client GPO**

1.  On DC1, in the Group Policy Management Console, right-click Myorg.local,  
    and then click New Organizational Unit

2.  In the New Organizational Unit dialog box, type Secure Clients, and then
    click OK

3.  Right-click Secure Clients, and then click Create a GPO in this domain, and
    Link it here

4.  In the New GPO dialog box, type Request IPsec, and then click OK

5.  Expand Secure Clients, right-click Request IPsec, and then click Edit

6.  Under Computer Configuration, expand Policies, expand Windows Settings,  
    expand Security Settings, expand Windows Firewall with Advanced Security,  
    and then expand the next Windows Firewall with Advanced Security node

7.  Click and then right-click Connection Security Rules, and then click New
    Rule

8.  In the New Connection Security Rule Wizard, click Isolation, and then click
    Next

9.  On the Requirements page, click  
    Request authentication for inbound and outbound connections, and then click
    Next

10. On the Authentication Method page, click Computer (Kerberos V5), and then
    click Next

11. On the Profile page, click Next

12. On the Name page, in the Name box, type Request Security, and then click
    Finish

13. Close the Group Policy Management Editor

**Task 3: Test network connectivity**

1.  On DC1, in Server Manager, click Tools,  
    and then click Active Directory Users and Computers

2.  Expand Myorg.local, and then click Computers

3.  Drag SVR1 into the Secure Servers OU. In the warning dialog box, click Yes

4.  Drag CL1 into the Secure Clients OU. In the warning dialog box, click Yes

5.  On SVR1, click Start, and then click Windows PowerShell

6.  At the PowerShell prompt, type gpupdate /force, and then press Enter

7.  Wait for the user and computer policies to be applied, type
    Restart-Computer,  
    and then press Enter

8.  On SVR2 sign in with myorg.local and Pa55w.rd, click Start, and then click
    Control Panel

9.  Click System and Security, and then click Allow an app through Windows
    Firewall

10. Under Allowed apps and features, verify that the File and Printer Sharing
    check box is selected,  
    and then click OK

11. On CL1, click Start, type PowerShell, and then click Windows PowerShell

12. At the PowerSheII prompt, type gpupdate /force, and then press Enter

13. Wait for the user and computer policies to be applied, type
    Restart-Computer,  
    and then press Enter

14. On SVR2, click Start, and then click Control Panel

15. Click System and Security, and then click Allow an app through Windows
    Firewall

16. Under Allowed apps and features, verify that the File and Printer Sharing
    check box is selected,  
    and then click OK

17. On SVR1, sign in as Myorg\\Administrator, using the password Pa55w.rd

18. On CL1, sign in as Myorg\\Administrator, using the password Pa55w.rd

19. Open File Explorer, and then try to connect to \\\\SVR1\\C\$. Verify that
    you can connect

20. Click Start, type Firewall, and then click Windows Defender Firewall with
    Advanced Security

21. Expand Monitoring, expand Security Associations, and then click Main Mode

22. In the Main Mode pane, double-click the listed item

23. View the information in Main Mode, and then click OK

24. Click Quick Mode

25. In the Quick Mode pane, double-click the listed item

26. View the information in Quick Mode, and then click OK

27. In File Explorer, try to connect to \\\\SVR2\\C\$, and verify that you can
    connect

28. In Windows Firewall with Advanced Security,  
    verify that no new security associations (SAs) were created

29. On SVR2, open File Explorer, and then try to connect to \\\\SVR1\\C\$  
    Verify that you cannot connect

30. On SVR1, open File Explorer, and then try to connect to \\\\SVR2\\C\$  
    Verify that you can connect

Results: After completing this exercise, you have configured and tested  
server isolation zones to secure network access
