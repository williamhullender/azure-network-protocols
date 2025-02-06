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

Next, within the Windows VM, we are going to install <a href="https://www.wireshark.org/">Wireshark</a>.

Wireshark is a network protocol analyzer, also known as a 'packet sniffer.' It captures and inspects incoming and outgoing network traffic, allowing users to analyze and interpret the data for troubleshooting, security analysis, and network performance monitoring.

On the Wireshark webpage, click "Download".

![image](https://github.com/user-attachments/assets/cee42a53-eae0-470a-8e5d-c38ed0779fcb)

Then click, "Windows x64 Installer"

![image](https://github.com/user-attachments/assets/f952e507-04e3-4ab4-a285-662d01b6db86)

Then in downloads in upper right corner, click "open file".

![image](https://github.com/user-attachments/assets/1f4c374e-129c-4641-99dd-123030835b31)

Then click Next -> Noted -> Next -> Next -> Next -> Next. Then ensure "Install Npcap 1.79" is checked. Then click, Next -> Install.

![image](https://github.com/user-attachments/assets/1ffd4dcb-79fb-44ee-8691-a32e791aee43)

Then Npcap should load.

![image](https://github.com/user-attachments/assets/c73cee08-be48-405f-8db6-143e690dd859)

Then click I agree -> Install -> Next -> Finish

Then in Wireshark Setup, click Next -> Finish

![image](https://github.com/user-attachments/assets/aea10544-fd13-40d2-ad21-7017e51d54bb)

Now open Wireshark, and click start packet capture (Blue Fin)

![image](https://github.com/user-attachments/assets/262b24e3-db27-42fb-ad46-a131126b9c8c)

Notice all the network traffic populating.

Now we can filter for ICMP traffic only, in text box input icmp and hit enter.

![image](https://github.com/user-attachments/assets/310a3104-ecad-4340-a44b-4b007392e89b)

Next, obtain the private IP address of the Ubuntu VM (Linux-vm). You can get this from going to Virtual Machines in Azure, click on the VM. Then it should be in the description box for the VM. 

![image](https://github.com/user-attachments/assets/17eafbfb-0887-4aa8-abf8-fe1c384e6071)

Now, go back into the windows VM and open powershell.

![image](https://github.com/user-attachments/assets/14cfee7d-0fdc-472e-9dcc-64cfbc628adc)

Next, ping the Ubuntu VM.

![image](https://github.com/user-attachments/assets/136abbf5-730f-4287-8cd2-3d6cce7a6254)

![image](https://github.com/user-attachments/assets/c6a029d5-6793-4a6b-bdd8-0f130b675995)

Notice the back and forth icmp packets that Wireshark captured.

Next ping a random website.

![image](https://github.com/user-attachments/assets/58c79037-4469-4d43-bb7f-193c36161e29)

![image](https://github.com/user-attachments/assets/e755e3ff-2b76-4b04-9350-eeb74c2790d6)

Observe the traffic between the VM and the website.

Now we are going to configure a Firewall (Network Security Group). Using the windows VM, initiate a continuous ping to the Ubuntu VM.

![image](https://github.com/user-attachments/assets/dd723c85-92ec-4633-b97a-e8bb0ea1dbf2)

Now go to Azure



















