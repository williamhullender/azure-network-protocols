<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark and experiment with Network Security Groups. <br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Configure Virtual Machines
- Observe ICMP traffic
- Configure Firewall (Network Security Group)
- Observe various network traffic

<h2>Actions and Observations</h2>

</p>
<br />

![image](https://github.com/user-attachments/assets/23865a66-445d-4b2e-b1cb-ac509f3372d9)

The first thing you want to do is go to https://portal.azure.com/ and create 2 virtual machines. When setting up your first virtual machine use Windows 10 Pro, for both VMs select at least 2 vcpus and 8 GB memory, Then, under the Basics tab, for "Administrator account", choose Password, and remember the username and password used. 

When creating the first VM, create a new resource group and remember the name. Then, under the Networking tab, create a new virtual network and remember the name for when you create the second VM.

For the second VM, use Ubuntu. Then, ensure that both VMs are in the same resource group and virtual network.

![image](https://github.com/user-attachments/assets/819bba7c-825f-48c6-9f70-14a54e8a1d61)

Next, we are going to connect to the Windows VM using Remote Desktop, if using MAC you will need to download the Windows app, if using Windows it is already installed. Once Remote Desktop is loaded, you will need the public IPv4 addresses of the Windows VM to connect. This can be found on the Virtual Machine page in Azure.

![image](https://github.com/user-attachments/assets/183142fa-82b4-444f-9708-0fb545bcbe85)

Input the public IP address into RDC, then input the username and click connect, then input the password and click connect again.

![image](https://github.com/user-attachments/assets/dbd86116-2bf8-492e-aadb-f4425ec05759)

Within the Windows VM, we are going to install Wireshark. 

Wireshark is a network protocol analyzer, also known as a 'packet sniffer.' It captures and inspects incoming and outgoing network traffic, allowing users to analyze and interpret the data for troubleshooting, security analysis, and network performance monitoring.






























































