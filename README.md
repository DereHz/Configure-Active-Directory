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
 - Login to the Domain Controller and enable ICMPv4 in on the local windows Firewall









































