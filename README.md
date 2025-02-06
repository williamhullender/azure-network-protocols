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

Now we are going to configure a Firewall (Network Security Group). Using the Windows VM, initiate a continuous ping to the Ubuntu VM.

![image](https://github.com/user-attachments/assets/dd723c85-92ec-4633-b97a-e8bb0ea1dbf2)

Now go to Azure, go to virtual machines, click on the Linux-vm, then settings, click Networking, then click Linux-vm-nsg.

![image](https://github.com/user-attachments/assets/f718a976-fa45-44f3-865c-05790152e6d7)

Then click settings -> inbound security rules -> add.

![image](https://github.com/user-attachments/assets/eaae609a-c7ff-4649-bdcb-8f1e8cd06ef2)

Then input the following, and then click add.

![image](https://github.com/user-attachments/assets/15535650-16fc-4070-9fe4-82233c979b18)

![image](https://github.com/user-attachments/assets/86e4fde1-63fc-4761-b99b-a202e2d4f9df)

You can observe that the rule was added.

![image](https://github.com/user-attachments/assets/62a932e9-7b47-4c2a-8273-16e5bd0d1747)

Now go back to the windows VM powershell.

![image](https://github.com/user-attachments/assets/b184661e-8122-482a-a8bf-8bb625c90db9)

You can see the rule has taken effect because the pings are unsuccessful.

If you go to Wireshark, you can see the new icmp packets are unsuccessful.

![image](https://github.com/user-attachments/assets/331655b3-2b5a-4115-85dd-7c2c0dc515bb)

Now you can go back into the Firewall (Network Security Group) settings for the Linux VM, and re-enable icmp traffic by deleting the rule we made.

Now go back into the Windows vm and observe Wireshark and powershell. Notice how once the security rule has taken efect, the icmp packets are now successful.

Now we can stop the ping by typing [ctrl + c] in powershell.

Next we can observe SSH traffic in Wireshark. In Wireshark, click the red square followed by the blue fin again to restart the packet capture, theres no need to save.

Next filter only for SSH traffic.

![image](https://github.com/user-attachments/assets/8a464778-6cc0-4380-96ea-f8c1cd81583d)

Now in the Windows VM, we can SSH into the Ubuntu VM through powershell. In powershell, run the command ssh (labuser@10.0.0.5). Replace "labuser" with the username you used for your Ubuntu VM when setting it up, and replace "10.0.0.5" with your Ubuntu VM's private IP address.

After typing the command above hit enter, then type yes and hit enter, then type in the password associated with your VM and hit enter.

If done successfully you will notice the user has changed.

![image](https://github.com/user-attachments/assets/595b79e5-d035-496e-b765-d901fc020098)

Now observe the traffic populated in Wireshark.

Now type commands in powershell and observe the traffic populated in Wireshark.

![image](https://github.com/user-attachments/assets/26f5c741-b724-4f4f-8f06-728476cb6642)

Now you can exit the ssh connection by typing exit in powershell and hitting enter.

![image](https://github.com/user-attachments/assets/8f6251aa-4a8f-4d33-9186-8490fa3d2077)

Now we can observe DHCP traffic. In Wireshark filter only for dhcp.

![image](https://github.com/user-attachments/assets/90bac843-174b-4288-b672-ff38b5313d6c)

Now, run powershell as a administrator and enter the command 

![image](https://github.com/user-attachments/assets/720110ff-6175-4e67-be06-95708c13ea5f)

Observe the DHCP traffic populated in Wireshark.

![image](https://github.com/user-attachments/assets/cf3120f5-99db-49d2-b952-56146831496d)

Now we can observe DNS traffic. In Wireshark filter only for DNS traffic.

![image](https://github.com/user-attachments/assets/238f5129-b6c7-4615-b446-b553b268b7ab)

Now in powershell type the command nslookup www.google.com

![image](https://github.com/user-attachments/assets/72a640fb-9aad-41d4-b0bd-efaae9a7cfea)

Notice the traffic populated in Wireshark.

![image](https://github.com/user-attachments/assets/a6a1cc09-13e7-4314-b3ee-b4efac0bc5ac)

Now we can look at RDP traffic. In Wireshark filter for "tcp.port == 3389"

![image](https://github.com/user-attachments/assets/4bea01ea-7d48-4fdf-a4d5-d6205beba4b9)

Notice how it is constantly populating, that is because Remote Desktop Protocol is constantly showing you a live stream from one computer to another, this results in constant traffic being transmitted.

Congratulations! We have configured 2 VMs, setup Wireshark, observed network traffic, and configured a firewall (Network Security Group).
