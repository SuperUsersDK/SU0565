**Chapter 4: Certificates in Server 2019**

**Lab: Deploying and configuring a two-tier CA hierarchy**

**Exercise 1: Deploying an offline root CA**

**Task 1: Create file and printer sharing exceptions**

1.  Right-click CA1 and CA2 Virtual Machines and start both.

2.  Sign in to CA1 as Administrator with the password Pa55w.rd

3.  Click Start, and then click Control Panel

4.  In the Control Panel window, click View network status and tasks

5.  In the Network and Sharing Center window, click Change advanced sharing
    settings

6.  Under Guest or Public (current profile), select the Turn on file and printer
    sharing option,  
    and then click Save changes

Switch to CA2

1.  Sign in to CA2 as myorg\\Administrator with the password Pa55w.rd

2.  Click Start, and then click Control Panel

3.  In the Control Panel window, click View network status and tasks

4.  In the Network and Sharing Center window, click Change advanced sharing
    settings

5.  Under Domain (current profile), select the Turn on file and printer sharing
    option,  
    and then click Save changes

**Task 2: Install and configure Active Directory Certificate Services (AD CS) on
CA1**

Switch to CA1

1.  Click Start, and then click Server Manager

2.  In Server Manager, click Add roles and features

3.  On the Before you begin page, click Next

4.  On the Select installation type page, click Next

5.  On the Select destination server page, click Next

6.  On the Select server roles page, select Active Directory Certificate
    Services.  
    When the Add Roles and Features Wizard window displays, click Add Features,  
    and then click Next

7.  On the Select features page, click Next

8.  On the Active Directory Certificate Services page, click Next

9.  On the Select role services page,  
    ensure that Certification Authority is selected, and then click Next

10. On the Confirm installation selections page, click Install

11. On the Installation progress page, after installation completes
    successfully,  
    click the Configure Active Directory Certificate Services on the destination
    server text

12. In the AD CS Configuration Wizard, on the Credentials page, click Next

13. On the Role Services page, select Certification Authority, and then click
    Next

14. On the Setup Type page, ensure that Standalone CA is selected, and then
    click Next

15. On the CA Type page, ensure that Root CA is selected, and then click Next

16. On the Private Key page, ensure that Create a new private key is selected,
    and then click Next

17. On the Cryptography for CA page, keep the default selections for Select a
    cryptographic provider  
    and Select the hash algorithm for signing certificates issued by this CA,
    but set the Key length to  
    4096, and then click Next

18. On the CA Name page, in the Common name for this CA text box, type
    MyorgRootCA,  
    and then click Next

19. On the Validity Period page, click Next

20. On the CA Database page, click Next

21. On the Confirmation page, click Configure

22. On the Results page, click Close

23. On the Installation progress page, click Close

24. On CA1, in Server Manager, click Tools, and then click Certification
    Authority

25. In the certsrv — [Certification Authority (Local)] console, right-click
    MyorgRootCA,  
    and then click Properties

26. In the MyorgRootCA Properties dialog box, click the Extensions tab

27. In the Select extension drop-down list, click CRL Distribution Point (CDP),
    and then click Add

28. In the Location text box, type <http://CA2.myorg.local/CertData/>

29. In the Variable drop-down list, click \<CaName\>, and then click Insert

30. In the Variable drop-down list, click \<CRLNameSuffix\>, and then click
    Insert

31. In the Variable drop-down list, click \<DeltaCRLAllowed\>, and then click
    Insert

32. In the Location text box, position the cursor at the end of the URL, type
    .crl, and then click OK

33. Select the following options, and then click Apply:

Include in the CDP extension of issued certificates

Include in CRLs. Clients use this to find Delta CRL locations

1.  In the Certification Authority pop-up window, click No

2.  In the Select extension drop-down list, click Authority Information Access
    (AIA),  
    and then click Add

3.  In the Location text box, type <http://CA2.myorg.local/CertData/>

4.  In the Variable drop-down list, click \<ServerDNSName\>, and then click
    Insert

5.  In the Location text box, type an underscore (_), in the Variable drop-down
    list,  
    click \<CaName\>, and then click Insert. Position the cursor at the end of
    the URL

6.  In the Variable drop-down list, click \<CertificateName\>, and then click
    Insert

7.  In the Location text box, position the cursor at the end of the URL, type
    .crt, and then click 0K

8.  Select the Include in the AIA extension of issued certificates check box,
    and then click OK

9.  Click Yes to restart the Certification Authority service

10. In the Certification Authority console, expand MyorgRootCA,  
    right-click Revoked Certificates, point to All Tasks, and then click Publish

11. In the Publish CRL window, click OK

12. Right-click MyorgRootCA, and then click Properties

13. In the MyorgRootCA Properties dialog box, click View Certificate

14. In the Certificate dialog box, click the Details tab, and then click Copy to
    File

15. In the Certificate Export Wizard, on the Welcome page, click Next

16. On the Export File Format page, select DER encoded binary X509 (.CER), and
    then click Next

17. On the File to Export page, click Browse, in the File name text box address
    bar in the top,  
    type \\\\CA2\\C\$, and then press Enter

18. In the File name text box, type RootCA, click Save, and then click Next

19. Click Finish, and then click OK three times

20. Open a File Explorer Window, and then browse to
    C:\\Windows\\System32\\CertSrv\\CertEnroll

21. In the Cert Enroll folder, select both files, right-click the highlighted
    files, and then click Copy

22. In the File Explorer address bar, type \\\\CA2\\C\$, and then press Enter

23. Right-click the empty space, and then click Paste

24. Close File Explorer

**Task 3: Create a Domain Name System (DNS) record for an offline root CA**

1.  Sign-in to DC1 Virtual Machine with Myorg\\administrator and Pa55w.rd.

2.  In Server Manager, click Tools, and then click DNS

3.  In the DNS Manager console, expand DC1, expand Forward Lookup Zones, click
    Myorg.local,  
    right-click Myorg.local, and then click New Host (A or AAAA)

4.  In the New Host window, in the Name text box, type CA1

5.  In the IP address Window, type 172.16.0.11, click Add Host, click OK, and
    then click Done

6.  Close DNS Manager

Results: After completing this exercise, you should have successfully installed
and configured the standalone root certification authority (CA) role on the CA1
server. Additionally, you should have created an appropriate DNS record in
Active Directory Domain Services (AD DS) so that other servers can connect to
CA1.

**Exercise 2: Deploying an enterprise subordinate CA**

**Task 1: Install and configure AD CS on CA2**

1.  On CA2, click Start, click Server Manager, and then click Add roles and
    features

2.  On the Before you begin page, click Next

3.  On the Select installation type page, click Next

4.  On the Select destination server page, click Next

5.  On the Select server roles page, select **Active Directory Certificate
    Services**

6.  When the Add Roles and Features Wizard displays, click Add Features, and
    then click Next

7.  On the Select features page, click Next

8.  On the Active Directory Certificate Services page, click Next

9.  On the Select role services page, ensure that Certification Authority is
    selected already,  
    and then select **Certification Authority Web Enrollment**

10. When the Add Roles and Features Wizard displays, click Add Features, and
    then click Next

11. On the Confirm installation selections page, click Install

12. On the Installation progress page, after installation is successful,  
    click the Configure Active Directory Certificate Services on the destination
    server text

13. In the AD CS Configuration wizard, on the Credentials page, click Next

14. On the Role Services page, select both Certification Authority  
    and Certification Authority Web Enrollment, and then click Next

15. On the Setup Type page, select Enterprise CA, and then click Next

16. On the CA Type page, click Subordinate CA, and then click Next

17. On the Private Key page, ensure that Create a new private key is selected,
    and then click Next

18. On the Cryptography for CA page, keep the default selections, and then click
    Next

19. On the CA Name page, in the Common name for this CA text box, type
    Myorg-lssuingCA,  
    and then click Next

20. On the Certificate Request page, ensure that  
    Save a certificate request to file on the target machine is selected, and
    then click Next

21. On the CA Database page, click Next

22. On the Confirmation page, click Configure

23. On the Results page, ignore the warning messages, and then click Close

24. On the Installation progress page, click Close

**Task 2: Install a subordinate CA certificate**

1.  On CA2, open a File Explorer window, and then browse to Local Disk (C:)

2.  Right-click RootCA.cer, and then click Install Certificate

3.  In the Certificate Import wizard, click Local Machine, and then click Next

4.  On the Certificate Store page, click Place all certificates in the following
    store,  
    and then click Browse

5.  Select Trusted Root Certification Authorities, click OK, click Next, and
    then click Finish

6.  When the Certificate Import wizard window appears, click OK

7.  In the File Explorer window, Select the MyorgRootCA.crl and
    CA1_MyorgRootCA.crt files,  
    right-click the files, and then click Copy

8.  Double-click inetpub

9.  Double-click wwwroot

10. Create a new folder, and then name it CertData

11. Paste the two copied files into that folder

>   Switch to the CA1 server

1.  Switch to Local Disk (C:)

2.  In the File Explorer address bar, type \\\\CA2\\C\$, and then press Enter

3.  Right-click the CA2.Myorg.local_Myorg-CA2-CA.req file, and then click Copy

4.  In the File Explorer window select This PC, right-click an empty space on
    C:\\, and then click Paste

5.  Make sure that the request file copies to CA1

6.  ln the Certificate Authority console, right-click MyorgRootCA, point to All
    Tasks,  
    and then click Submit new request

7.  In the Open Request File window, navigate to Local Disk (C:),  
    click the CA2.Myorg.local_Myorg- CA2-CA.req file, and then click Open

8.  In the Certification Authority console, click the Pending Requests
    container.  
    Right-click Pending Requests, and then click Refresh

9.  In the details pane, right-click the request (with ID 2), point to All
    Tasks, and then click Issue

10. In the Certification Authority console, click the Issued Certificates
    container

11. In the details pane, double-click the certificate, click the Details tab,
    and then click Copy to File

12. In the Certificate Export wizard, on the Welcome page, click Next

13. On the Export File Format page,  
    click Cryptographic Message Syntax Standard — PKCS \#7 Certificates (.P7B),  
    click Include all certificates in the certification path if possible,  
    and then click Next

14. On the File to Export page, click Browse

15. In the File name text box, type \\\\CA2\\c\$\\SubCA, click Save, click Next,
    click Finish, and then click OK twice

Switch to the CA2 server

1.  In Server Manager, click Tools, and then click Certification Authority

2.  In the Certification Authority console, right-click Myorg-IssuingCA, point
    to All Tasks,  
    and then click Install CA Certificate

3.  Go to Local Disk (c:), click the SubCA.p7b file, and then click Open

4.  Wait for 15-20 seconds, and then on the toolbar, click the green icon to
    start the CA service

5.  Ensure that the CA successfully starts

Switch to the CA1 server

1.  Shut down the server

Note: From this point, you can safely take the root CA offline  
and use just the enterprise subordinate CA

**Task 3: Publish a root CA certificate through Group Policy**

1.  On DC1, in Server Manager, click Tools, and then click Group Policy
    Management

2.  In the Group Policy Management Console, expand Forest: Myorg.local, expand
    Domains,  
    expand Myorg.local, right-click Default Domain Policy, and then click Edit

3.  In the Computer Configuration node, expand Policies, expand Windows
    Settings,  
    expand Security Settings, expand Public Key Policies, right-click Trusted
    Root Certification Authorities, click Import, and then click Next

4.  On the File to Import page, click Browse

5.  In the file name text box, type \\\\CA2\\C\$, and then press Enter

6.  Click file RootCA.cer, and then click Open

7.  Click Next two times, and then click Finish and the WAIT until certificate
    import has finished.

8.  When the Certificate Import Wizard Window appears, click OK

Note: It might take 15-20 seconds for this window to appear.

1.  Close the Group Policy Management Editor and the Group Policy Management
    Console

2.  Switch to SVR4 and sign in with Myorg\\administrator and Pa55w.rd to verify
    that the Root Certificate is send out to domain computers.

3.  Open a PowerShell console and type the command Gpupdate /force and press
    Enter, to make sure that the Root Certificate is send out.

4.  In the open PowerShell window type MMC and press Enter.

5.  In the open console window select File – Add/Remove Snap-in…

6.  Select **Certificates** Snap-in and press **Add\>** and Computer account and
    select next and Finish.

7.  Select OK and in the open Console Windows navigate **Certificates (Local
    Computer) – Trusted Root Certification Authorities – Certificates**.

8.  Now make sure that you can see the Root Certificate **MyorgRootCA** on the
    list.

9.  Close all windows in **SVR4** and shutdown the Virtual Machine. Leave the
    other Virtual machines running.

Results: After completing this exercise, you should have successfully deployed
and configured an enterprise subordinate CA. You also should have a subordinate
CA certificate issued by a root CA installed on CA2. To establish trust between
the root CA and domain member clients, you have verified that you can use Group
Policy to deploy a root CA certificate.
