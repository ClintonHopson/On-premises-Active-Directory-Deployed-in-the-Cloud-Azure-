# On-premises-Active-Directory-Deployed-in-the-Cloud-Azure-

<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Video Demonstration</h2>

- ### [YouTube: How to Deploy on-premises Active Directory within Azure Compute](https://www.youtube.com)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Prepare the AD infrastructure 
- Deploy AD 
- Create the users with PowerShell 
- Configure Group Policy 

<h2>Deployment and Configuration Steps</h2>

1st step; lets create our resource group in Azure
![1st step lets create our resource group in Azure](https://github.com/user-attachments/assets/02ad0c70-85de-468f-a3dd-bb022f84fe60)


2nd make your virtual network in Azure and put it inside your resource group 
![1st step lets create our resource group in Azure](https://github.com/user-attachments/assets/f02d266a-d0c1-4e2d-b55c-6c75355a901e)




3rd this next step is important. Once you have put the VM in your Resource group and created the name. Make sure for image you select “ Windows server 2022 datacenter “ choose and choose a VM at least 2 cpu’s for image size and then review and create. 

![1st step lets create our resource group in Azure](https://github.com/user-attachments/assets/94636678-b530-493e-b08c-e03461ff803d)



4th Now we will create another VM, but this one will be the user's computer. The first one we made will be the domain controller. Everything else is the same as the last VM, except for image choose windows 11pro be sure to set your passwords and check the box at the bottom of the page for the licensing agreement. 

![1st step lets create our resource group in Azure](https://github.com/user-attachments/assets/9cec0c15-88eb-4727-a669-43a5ab985ec3)

5th make sure the clients VM is in the Active directory v-net

![1st step lets create our resource group in Azure](https://github.com/user-attachments/assets/e7250fbd-b85a-44d1-892c-6bb74ee02a03)


6th now we need to make the IP address of the Domain controller static or set it to never change so that way our client computer doesn’t loose connection to the domain controller / network. So in azure go to your VM list select DC-1 on the left select network > network settings > select the NIC ( network interface card ) or the green box looking this at the 

![1st step lets create our resource group in Azure](https://github.com/user-attachments/assets/6386005b-d748-4335-9d3c-8d2a296e9ba4)

7th Below you will see a list that said ipconfig in blue letter select that then on this page click the static bubble and save.
![1st step lets create our resource group in Azure](https://github.com/user-attachments/assets/6b51dd88-db4c-439f-a116-6c1f73b348cf)


8th ok go-ahead and login the DC-1 VM with the public IP address we will then disable the firewall for sake of the lab.
![1st step lets create our resource group in Azure](https://github.com/user-attachments/assets/1b043322-4222-4b0e-8a16-73c669e33ce1)


9th ok you are logged into your VM, right click start menu go  to “ run “ and type “ mf.msc” 

![1st step lets create our resource group in Azure](https://github.com/user-attachments/assets/45446f7b-ef59-4794-ba42-d5bb0d5d7a4a)


10th now below “ public profile  “ you will see windows defender firewall properties. In the Doman tab , private profile , public profile and turn the fire wall state off on all of these. Once done select apply and ok. 
![1st step lets create our resource group in Azure](https://github.com/user-attachments/assets/b82d8464-61ad-4666-a232-c5ca75f77e2d)


11th ok now we are going to set client ones computer to use DC-1 private IP address to so it can connect to the network. Copy dc-1 private IP address.
![1st step lets create our resource group in Azure](https://github.com/user-attachments/assets/fb48fef7-15c8-4ad4-898d-613b5e9d6b00)

12th now go to Client ones Network settings in Azure and go to the NIC page
![1st step lets create our resource group in Azure](https://github.com/user-attachments/assets/31b8d9ab-6a03-47fb-b22d-4c3eb0f1da44)

13th once in the NIC page on the left select DNS settings
![1st step lets create our resource group in Azure](https://github.com/user-attachments/assets/e237ff57-e7ee-4b14-9412-96a516a11b20)


14th from here select custom and paste in the private IP of dc-1  and save. Doing this tells client 1 to use the Internet from DC-1 instead of the internet from the VM connected to the azure DNS server. 
![1st step lets create our resource group in Azure](https://github.com/user-attachments/assets/6e8fa1b1-1314-4840-8a45-21f160206f18)


15th go back to client-1 and restart the VM in Azure. So the IP will reconnect
![1st step lets create our resource group in Azure](https://github.com/user-attachments/assets/f36e84e0-a549-4478-9134-5fceb4ce6dc9)

16th log back into client 1’s VM we are going to test the connection
![1st step lets create our resource group in Azure](https://github.com/user-attachments/assets/e03a785f-29d3-4b52-85e4-0b56b909942c)

17th once logged into client 1 vm type in the search bar “Powershell “, once in PowerShell, type ping type DC-1 private IP and hit enter
![1st step lets create our resource group in Azure](https://github.com/user-attachments/assets/a5b2bbc2-9361-41bb-96b6-3ffd4c88149d)


18th if you have done everything correctly, you should see this traffic communication. 
![1st step lets create our resource group in Azure](https://github.com/user-attachments/assets/75805798-5c33-477c-abb2-eb1590f92a0c)


20th and just to be sure with power shell still open type ipconfig /all to show the DNS server info and it should be the same IP as DC-1 telling us we have connected. 
![1st step lets create our resource group in Azure](https://github.com/user-attachments/assets/5f5c09d0-a7a8-4e5f-b63e-c2f0ac3cc723)





