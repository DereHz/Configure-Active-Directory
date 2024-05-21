<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-Premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Step 1. Setup Resources in Azure
- Step 2. Ensure Connectivity between the client and Domain Controller
- Step 3. Install Active Directory
- Step 4. Create an Admin and Normal User Account in AD
- Step 5. Join Client-1 to your domain (mydomain.com)
- Step 6. Setup Remote Desktop for non-administrative users on Client-1
- Step 7. Create a bunch of additional users and attempt to log into client-1 with one of the users

<img width="1312" alt="Screen Shot 2024-05-21 at 1 40 49 PM" src="https://github.com/DereHz/Configure-Active-Directory/assets/169094076/06357105-55c3-4617-b83d-b1cd15ec9328">
<img width="1315" alt="Screen Shot 2024-05-21 at 1 42 56 PM" src="https://github.com/DereHz/Configure-Active-Directory/assets/169094076/24f360e7-a0eb-4226-8fa9-dbf1d6927bfc">

1. Setup Resourses in Azure 
 - Create a resourse group
 - Create the Domain Controller VM (Windows Server 2022) named “DC-1”
Take note of the Resource Group and Virtual Network (Vnet) that get created at this time

<img width="1315" alt="Screen Shot 2024-05-21 at 1 54 26 PM" src="https://github.com/DereHz/Configure-Active-Directory/assets/169094076/2a9b36c6-45e5-4a46-b984-a5a589702e9b">

 - Set Domain Controller’s NIC Private IP address to be static
 - Virtual Machines -> DC-1 -> Networking -> Network Interface -> IP configurations -> ipconfig1 -> change from Dynamic to Static -> Save

<img width="1315" alt="Screen Shot 2024-05-21 at 1 49 30 PM" src="https://github.com/DereHz/Configure-Active-Directory/assets/169094076/2272e0ae-e626-4255-bb5b-0aa7e48a401d">
 - Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet that was created in DC-1
 - Ensure that both VMs are in the same Vnet (you can check the topology with Network Watcher

Note: make sure you let DC-1 VM finish deploying, otherwise, when creating the second VM, it wont show up.

<img width="2088" alt="Screen Shot 2024-05-21 at 2 01 17 PM" src="https://github.com/DereHz/Configure-Active-Directory/assets/169094076/45fb36f2-d70f-4dc0-970a-409c18f7a6dd">



2. Ensure Connectivity between the client and Domain Controller
 - Login to Client-1 with Remote Desktop and ping DC-1’s private IP address

<img width="999" alt="Screen Shot 2024-05-21 at 2 46 07 PM" src="https://github.com/DereHz/Configure-Active-Directory/assets/169094076/78e06dd2-2cbb-4a88-ae8f-d1506a6d53d2">

 - Open Command Line -> type in pinng -t (perpetual ping) and the private IP address -> enter.
 - Observe how the comman did not work

<img width="1083" alt="Screen Shot 2024-05-21 at 2 54 38 PM" src="https://github.com/DereHz/Configure-Active-Directory/assets/169094076/3f9325da-b199-4e44-a97e-ff4718641648">

 - Login to the Domain Controller and enable ICMPv4 in on the local windows Firewall
 - Minimize Client-1 VM window and go to DC-1 VM
 - Once inside DC-1 VM go to finder and type in "Windows Defender firewall with advance security" and open it and expand it

<img width="1592" alt="Screen Shot 2024-05-21 at 2 58 17 PM" src="https://github.com/DereHz/Configure-Active-Directory/assets/169094076/9049022c-ad87-4a4e-8046-f86a79f7d963">
<img width="1592" alt="Screen Shot 2024-05-21 at 2 59 19 PM" src="https://github.com/DereHz/Configure-Active-Directory/assets/169094076/812bd19d-e32c-4062-ac49-39e8b2640c53">


 - Click on Inbound Rules -> Protocol -> look for "Core Networking - Destination unreachable Fragmentation Needed (ICMPv4)
 - Right click on the 2 rules directly under it and click "enable rule"

<img width="998" alt="Screen Shot 2024-05-21 at 3 04 31 PM" src="https://github.com/DereHz/Configure-Active-Directory/assets/169094076/113a205c-bb35-49f4-bfa3-c7b913d45f4e">

 - Minimize DC-1 VM and go back to Client-1 VM
 - Observer how the ping started working again after enabling the ICPv4 request on the windows firewall.
 - Press Control C and close the command line window.

<img width="1488" alt="Screen Shot 2024-05-21 at 2 51 27 PM" src="https://github.com/DereHz/Configure-Active-Directory/assets/169094076/1d40b455-fcdb-4fd1-b121-ca57d385b865">

<img width="1489" alt="Screen Shot 2024-05-21 at 3 16 20 PM" src="https://github.com/DereHz/Configure-Active-Directory/assets/169094076/6ecbf084-8031-4593-a1ec-eecf8fb31a50">

3. Install Active Directory
 -  Minimize Client 1 VM and Go to DC-1
 -  Open up Server manager
 -  Click Add roles and features -> click next until you get to Server Roles -> Click on "Active Directory Domain Services" -> Add feature -> click next until it says install -> install -> click close after its done

<img width="1486" alt="Screen Shot 2024-05-21 at 3 20 50 PM" src="https://github.com/DereHz/Configure-Active-Directory/assets/169094076/6dcb1009-c5f8-4ab6-a415-b9b997e52af5">

 -  Click on the Yellow exclamation point icon next to the flag on top
 -  Click on "Promote thi server to a domain controller"

<img width="1487" alt="Screen Shot 2024-05-21 at 3 24 11 PM" src="https://github.com/DereHz/Configure-Active-Directory/assets/169094076/6a7d45f6-0ceb-4937-929a-b2b838061573">


 -  Click on Add a new forest
 -  Type in "mydomain.com"
 -  click next to type in a password and put which ever password you choose
 -  click next to everything and install

<img width="1587" alt="Screen Shot 2024-05-21 at 3 29 26 PM copy" src="https://github.com/DereHz/Configure-Active-Directory/assets/169094076/1ffab6a0-609e-4a0c-b1f6-3bbf377b0583">

 - After it finishes installing, it be automatically sign you out

<img width="771" alt="Screen Shot 2024-05-21 at 3 34 17 PM" src="https://github.com/DereHz/Configure-Active-Directory/assets/169094076/c6a89255-ff7a-4fb3-9239-93447b076208">

 - Open it back up but instead of using the username you originally put in for the VM, type in the username you put in the root domain name \ your VM username
 - In this case, it is mydomain.com\Derek15. Reason for this is because we turned this VM into a Domain controller


4. 



































