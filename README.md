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

</p>
<br />

The first thing you want to do is go to https://portal.azure.com/ and create 2 virtual machines. When setting up your first virtual machine use Windows 10 Pro, for both VMs select at least 2 vcpus and 8 GB memory, Then, under the Basics tab, for "Administrator account", choose Password, and remember the username and password used. 

When creating the first VM, create a new resource group and remember the name. Then, under the Networking tab, create a new virtual network and remember the name for when you create the second VM.

For the second VM, use Ubuntu. Then, ensure both VMs are in the same resource group and virtual network.

</p>
<br />

![image](https://github.com/user-attachments/assets/819bba7c-825f-48c6-9f70-14a54e8a1d61)

</p>
<br />

Next, we will connect to the Windows VM using Remote Desktop, if using MAC you will need to download the Windows app, if using Windows it is already installed. Once Remote Desktop is loaded, you will need the public IPv4 addresses of the Windows VM to connect. This can be found on the Virtual Machine page in Azure.

</p>
<br />

![image](https://github.com/user-attachments/assets/183142fa-82b4-444f-9708-0fb545bcbe85)

</p>
<br />

Input the public IP address into RDC, then input the username and click connect, then input the password and click connect again.

</p>
<br />

![image](https://github.com/user-attachments/assets/dbd86116-2bf8-492e-aadb-f4425ec05759)

</p>
<br />

Next, within the Windows VM, we are going to install <a href="https://www.wireshark.org/">Wireshark</a>.

Wireshark is a network protocol analyzer, also known as a 'packet sniffer.' It captures and inspects incoming and outgoing network traffic, allowing users to analyze and interpret the data for troubleshooting, security analysis, and network performance monitoring.

On the Wireshark webpage, click "Download".

</p>
<br />

![image](https://github.com/user-attachments/assets/cee42a53-eae0-470a-8e5d-c38ed0779fcb)

</p>
<br />

Then click, "Windows x64 Installer"

</p>
<br />

![image](https://github.com/user-attachments/assets/f952e507-04e3-4ab4-a285-662d01b6db86)

</p>
<br />

Then in downloads in the upper right corner, click "open file".

</p>
<br />

![image](https://github.com/user-attachments/assets/1f4c374e-129c-4641-99dd-123030835b31)

</p>
<br />

Then click Next -> Noted -> Next -> Next -> Next -> Next. Then ensure "Install Npcap 1.79" is checked. Then click Next -> Install.

</p>
<br />

![image](https://github.com/user-attachments/assets/1ffd4dcb-79fb-44ee-8691-a32e791aee43)

</p>
<br />

Then Npcap should load.

</p>
<br />

![image](https://github.com/user-attachments/assets/c73cee08-be48-405f-8db6-143e690dd859)

</p>
<br />

Then click I agree -> Install -> Next -> Finish

Then in Wireshark Setup, click Next -> Finish

</p>
<br />

![image](https://github.com/user-attachments/assets/aea10544-fd13-40d2-ad21-7017e51d54bb)

</p>
<br />

Now open Wireshark, and click start packet capture (Blue Fin)

</p>
<br />

![image](https://github.com/user-attachments/assets/262b24e3-db27-42fb-ad46-a131126b9c8c)

</p>
<br />

Notice all the network traffic populating.

Now we can filter for ICMP (Internet Control Message Protocol) traffic only, in the text box input icmp and hit enter.

</p>
<br />

![image](https://github.com/user-attachments/assets/310a3104-ecad-4340-a44b-4b007392e89b)

</p>
<br />

Next, obtain the private IP address of the Ubuntu VM (Linux-vm). You can get this by going to Virtual Machines in Azure and clicking on the VM. Then it should be in the description box for the VM. 

</p>
<br />

![image](https://github.com/user-attachments/assets/17eafbfb-0887-4aa8-abf8-fe1c384e6071)

</p>
<br />

Now, go back into the Windows VM and open Powershell as an administrator.

</p>
<br />

![image](https://github.com/user-attachments/assets/14cfee7d-0fdc-472e-9dcc-64cfbc628adc)

</p>
<br />

Next, ping the Ubuntu VM.

</p>
<br />

![image](https://github.com/user-attachments/assets/136abbf5-730f-4287-8cd2-3d6cce7a6254)

</p>
<br />

![image](https://github.com/user-attachments/assets/c6a029d5-6793-4a6b-bdd8-0f130b675995)

</p>
<br />

Notice the back-and-forth ICMP packets that Wireshark captured.

Next, ping a random website.

</p>
<br />

![image](https://github.com/user-attachments/assets/58c79037-4469-4d43-bb7f-193c36161e29)

</p>
<br />

![image](https://github.com/user-attachments/assets/e755e3ff-2b76-4b04-9350-eeb74c2790d6)

</p>
<br />

Observe the traffic between the VM and the website.

Now we are going to configure a Firewall (Network Security Group). Using the Windows VM, initiate a continuous ping to the Ubuntu VM.

</p>
<br />

![image](https://github.com/user-attachments/assets/dd723c85-92ec-4633-b97a-e8bb0ea1dbf2)

</p>
<br />

Now go to Azure, go to virtual machines, click on the Linux-vm, then settings, click Networking, then click Linux-vm-nsg.

</p>
<br />

![image](https://github.com/user-attachments/assets/f718a976-fa45-44f3-865c-05790152e6d7)

</p>
<br />

Then click settings -> inbound security rules -> add.

</p>
<br />

![image](https://github.com/user-attachments/assets/eaae609a-c7ff-4649-bdcb-8f1e8cd06ef2)

</p>
<br />

Then input the following, and then click add.

</p>
<br />

![image](https://github.com/user-attachments/assets/15535650-16fc-4070-9fe4-82233c979b18)

</p>
<br />

![image](https://github.com/user-attachments/assets/86e4fde1-63fc-4761-b99b-a202e2d4f9df)

</p>
<br />

You can observe that the rule was added.

</p>
<br />

![image](https://github.com/user-attachments/assets/62a932e9-7b47-4c2a-8273-16e5bd0d1747)

</p>
<br />

Now go back to the Windows VM powershell.

</p>
<br />

![image](https://github.com/user-attachments/assets/b184661e-8122-482a-a8bf-8bb625c90db9)

</p>
<br />

You can see the rule has taken effect because the pings are unsuccessful.

If you go to Wireshark, you can see the new ICMP packets are unsuccessful.

</p>
<br />

![image](https://github.com/user-attachments/assets/331655b3-2b5a-4115-85dd-7c2c0dc515bb)

</p>
<br />

Now you can go back into the Firewall (Network Security Group) settings for the Linux VM, and re-enable ICMP traffic by deleting the rule we made.

Now go back into the Windows VM and observe Wireshark and Powershell. Notice how once the security rule has taken effect, the ICMP packets are now successful.

Now we can stop the ping by typing [ctrl + c] in PowerShell.

Next, we can observe SSH traffic in Wireshark. In Wireshark, click the red square icon followed by the blue fin icon again to restart the packet capture, there's no need to save.

Next filter only for SSH (Secure Shell) traffic.

</p>
<br />

![image](https://github.com/user-attachments/assets/8a464778-6cc0-4380-96ea-f8c1cd81583d)

</p>
<br />

Now in the Windows VM, we can SSH into the Ubuntu VM through Powershell. In Powershell, run the command ssh (labuser@10.0.0.5). Replace "labuser" with the username you used for your Ubuntu VM when setting it up and replace "10.0.0.5" with your Ubuntu VM's private IP address.

After typing the command above hit enter, then type yes and hit enter, then type in the password associated with your VM and hit enter.

If done successfully you will notice the user has changed.

</p>
<br />

![image](https://github.com/user-attachments/assets/595b79e5-d035-496e-b765-d901fc020098)

</p>
<br />

Now observe the traffic populated in Wireshark.

Now type commands in Powershell and observe the traffic populated in Wireshark.

</p>
<br />

![image](https://github.com/user-attachments/assets/26f5c741-b724-4f4f-8f06-728476cb6642)

</p>
<br />

Now you can exit the SSH connection by typing exit in Powershell and hitting enter.

</p>
<br />

![image](https://github.com/user-attachments/assets/8f6251aa-4a8f-4d33-9186-8490fa3d2077)

</p>
<br />

Now we can observe DHCP (Dynamic Host Configuration Protocol) traffic. In Wireshark filter only for dhcp.

</p>
<br />

![image](https://github.com/user-attachments/assets/90bac843-174b-4288-b672-ff38b5313d6c)

</p>
<br />

Now, run Powershell as an administrator and enter the command 

</p>
<br />

![image](https://github.com/user-attachments/assets/720110ff-6175-4e67-be06-95708c13ea5f)

</p>
<br />

Observe the DHCP traffic populated in Wireshark.

</p>
<br />

![image](https://github.com/user-attachments/assets/cf3120f5-99db-49d2-b952-56146831496d)

</p>
<br />

Now we can observe DNS (Domain Name System) traffic. In Wireshark filter only for DNS traffic.

</p>
<br />

![image](https://github.com/user-attachments/assets/238f5129-b6c7-4615-b446-b553b268b7ab)

</p>
<br />

Now in Powershell type the command nslookup www.google.com

</p>
<br />

![image](https://github.com/user-attachments/assets/72a640fb-9aad-41d4-b0bd-efaae9a7cfea)

</p>
<br />

Notice the traffic populated in Wireshark.

</p>
<br />

![image](https://github.com/user-attachments/assets/a6a1cc09-13e7-4314-b3ee-b4efac0bc5ac)

</p>
<br />

Now we can look at RDP (Remote Desktop Protocol) traffic. In Wireshark filter for "tcp.port == 3389"

</p>
<br />

![image](https://github.com/user-attachments/assets/4bea01ea-7d48-4fdf-a4d5-d6205beba4b9)

</p>
<br />

Notice how it is constantly populating, that is because Remote Desktop Protocol is constantly showing you a live stream from one computer to another, this results in constant traffic being transmitted.

Congratulations! We have configured 2 VMs, set up Wireshark, observed network traffic, and configured a firewall (Network Security Group).
