**Chapter 11:**  
**Containers and Nano Server**

**Exercise 1: Deploying Docker in Windows Server 2019**

**Task 1: Installing Docker for Windows Server 2019**

1.  Start NAT and wait for it to fully start.

2.  On SVR1, click Start, and then click Windows PowerShell

3.  At the Windows PowerShell command prompt,

4.  Type ping -t 8.8.8.8 and wait for it to succeed before you continue.

5.  Press Ctrl + C to stop the ping.  
    type the following command to install Docker, and then press Enter:

*Install-Module DockerProvider -Force*

1.  At the notification  
    “Do you want PowerShellGet to install and import the NuGet provider now?”  
    type Y, and then press Enter

2.  At the Windows PowerShell command prompt, type the following command,  
    and then press Enter:

*Install-Package Docker -ProviderName DockerProvider -Force*

1.  At the notification  
    “Are you sure you want to install software from DockerDefault”  
    type Y, and then press Enter:

Note: This takes 2-3 minutes

1.  Type the following command to restart the computer, and then press Enter:

*Restart-Computer -Force*

**Task 2: Download an image**

1.  After the VM restarts, sign in to SVR1 as Myorg\\Administrator  
    with the password Pa55w.rd

2.  Wait for all services to start.

3.  On SVR1, click Start, and then click Windows PowerShell

4.  At the Windows PowerShell command prompt,  
    type the following command to search the Docker  
    Hub for Windows container images, and then press Enter:

*docker search microsoft*

Note: If this command fails – then look at Docker in Services – start the
service if not started correctly

1.  To download the IIS image, type the following command, and then press Enter:

*docker pull microsoft/iis:windowsservercore*

Note: During this step, you are downloading and extracting several gigabytes of
data  
Depending on your internet connection and CPU, this might take about 10 minutes

Results: After completing this exercise, you have installed Docker

**Exercise 2: Installing and configuring an IIS container**

**Task 1: Deploy a new container**

1.  If you haven't already done so, sign in to SVR1 as Myorg\\Administrator  
    with a password of Pa55w.rd

2.  At the Windows PowerShell command prompt, type the following command  
    to display the downloaded container image(s), and then press Enter:

*docker images*

Note: Note the image REPOSITORY of microsoft/iis with a TAG of windowsservercore  
You will use this to run the container

1.  At the Windows PowerShell command prompt,  
    type the following command to deploy the IIS container, and then press
    Enter:

*docker run -d -p 80:80 microsoft/iis:windowsservercore cmd*

Note: This command runs the IIS image as a background service (-d) and
configures networking such that port 80 of the container host maps to port 80 of
the container

If this produces an error (File is being used by another process) then change
the default website to bind to another port than 80 and try again

1.  Type the following command to retrieve the IP address information of the
    container host,  
    and then press Enter:

*ipconfig*

Note: Note the IPv4 address of the Ethernet adapter named vEthernet (nat)  
This is the address of the new container. Make a note of the IPv4 address of the
Ethernet adapter  
named Ethernet. This is the IP address of the container host.

1.  On CL1, open Internet Explorer.

2.  In the address bar, type the following command, and then press Enter:

3.  http://\< Containerhost\>

Note: Replace \<Containerhost\> with the IP address of the container host,  
which is the IP address of SVR1

1.  Observe the default IIS page

**Task 2: Manage the container**

1.  On SVR1, in the Windows PowerShell command prompt window,  
    type the following command to view the running containers, and then press
    Enter:

*docker ps*

1.  Make a note of the container ID

2.  Type the following command to stop the container, and then press Enter:

*docker stop \<ContainerlD\>*

Note: Replace \<ContainerID\> with the actual container ID

1.  On CL1, open Internet Explorer

2.  In the Internet Explorer Address Bar, type the following command, and then
    press Enter:

http://\<Containerhost\>

Notice that the default IIS page is no longer accessible  
This is because the container is not running

1.  On SVR1, in the Windows PowerShell command prompt window, type the following  
    command to delete the container, and then press Enter:

*docker rm \<Container\>*

Note: Replace \<ContainerID\> with the container actual ID

1.  After this exercise leave the virtual machines running.

Results: After completing this exercise, you should have deployed and managed a
container
