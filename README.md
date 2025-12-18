# On-premises-Active-Directory-Deployed-in-the-Cloud-Azure-

<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
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


<h2>Part Two <h2> 
Ok great, you have created the AD infrastructure Now let's actually deploy AD 

Step 1 Go to your DC-1 and select Server Manager. 
![Part 2 ok 1st ok now go to your DC-1 and select sever manager](https://github.com/user-attachments/assets/e3856017-f456-46e1-ac00-d6466ea88e0e)

2nd select add roles and features > next > next > server selection ( here there should only be on server “ dc-1 “ ) select the server then click next
![Part 2 ok 1st ok now go to your DC-1 and select sever manager](https://github.com/user-attachments/assets/94e87781-e8ef-479d-9118-b829e233748a)

3rd in server roles > Active Directory domain services > add features > next
![Part 2 ok 1st ok now go to your DC-1 and select sever manager](https://github.com/user-attachments/assets/c36bf8aa-e8d9-46a2-9329-b3e3f8165c7c)

4th for features and AD DS select next > confirmation ( check the box next to “ restart destination server “ and click next /ok and install once its finished select close and ok. 
![Part 2 ok 1st ok now go to your DC-1 and select sever manager](https://github.com/user-attachments/assets/1a968882-6000-409e-99a9-4d47e0a76d27)

5th now back in server manager on top to the right there is a flag with a caution icon. Select that, then choose “ promote this server to a domain controller “ 

![Part 2 ok 1st ok now go to your DC-1 and select sever manager](https://github.com/user-attachments/assets/1e947267-c893-4a1e-836c-239239543fc6)

6th now select “ add new forrest “ and name the domain “ my domain.com “ > next
![Part 2 ok 1st ok now go to your DC-1 and select sever manager](https://github.com/user-attachments/assets/c70e0e3d-4882-4cfb-bf94-7e6d717e988a)

7th in “ domain controller options “ set the password to something you will remember. We will likely never use this > next 
![Part 2 ok 1st ok now go to your DC-1 and select sever manager](https://github.com/user-attachments/assets/a2c3629a-b645-45d8-805f-bd0f9f38c7bf)


8th select next for everything , until prerequisite then install this then select next on this as well
![Part 2 ok 1st ok now go to your DC-1 and select sever manager](https://github.com/user-attachments/assets/fe67281a-08fe-4a8f-9b53-6375e8e93e2c)

9th once that is finished being install you will be signed out give the DC a min then log back in shouldn’t take long
![Part 2 ok 1st ok now go to your DC-1 and select sever manager](https://github.com/user-attachments/assets/c15b734f-19b0-4fb8-8f5f-ef76d66b248b)


10th once you are logged back into DC-1 click the start menus >windows  admin tools > Active Directory users and computers 
![Part 2 ok 1st ok now go to your DC-1 and select sever manager](https://github.com/user-attachments/assets/6b12401d-429a-4100-b1bb-9cd54716799c)


11th from here right click “mydomain.com “ > new > organizational unit > and name this unit ( _EMPLOYEE ) > OK 

![Part 2 ok 1st ok now go to your DC-1 and select sever manager](https://github.com/user-attachments/assets/cd155918-bd7f-4432-847c-4dff244b880f)

12th repeat the same thing again but name this organizational unit ( _ADMINS ) 
![Part 2 ok 1st ok now go to your DC-1 and select sever manager](https://github.com/user-attachments/assets/4e3b8450-72d2-46bf-8e65-303c2a4c338e)

13th next we right click admin > new > users > and create the Jane Doe user and the user name will be jane_admin 
![Part 2 ok 1st ok now go to your DC-1 and select sever manager](https://github.com/user-attachments/assets/b0b2972a-c523-4782-8797-5fdefb6bfea2)

14th give the user a password you will remember> next > finish 
![Part 2 ok 1st ok now go to your DC-1 and select sever manager](https://github.com/user-attachments/assets/1fa35893-12ae-4b7d-acf8-5ae0051ffe22)

15th now we will make this account a admin > right click Jane doe > properties > member of > add 
![Part 2 ok 1st ok now go to your DC-1 and select sever manager](https://github.com/user-attachments/assets/5d4c2881-3488-478d-9224-60e4736f2a82)

16th under ( enter the object name to select ) search domain admins > check names > ok > apply > ok unity 
![Part 2 ok 1st ok now go to your DC-1 and select sever manager](https://github.com/user-attachments/assets/600ebbc8-7218-47a3-9cfb-d6e059ebf97e)

17th now we will add client 1 PC to the domain. On your client-1 pc right click the start  menu > settings > (rename PC advanced ) or (renaming your device for better security)  or ( advanced system setting )  

![Part 2 ok 1st ok now go to your DC-1 and select sever manager](https://github.com/user-attachments/assets/9674ffa6-fc46-4837-9935-a01d73a00386)

18th now click on “ computer name tab “ > change > member of ( select Domian ) and enter “ mydomain.com” > ok  
![Part 2 ok 1st ok now go to your DC-1 and select sever manager](https://github.com/user-attachments/assets/034040c7-6427-4a51-a9ab-5ea8074daf44)

19th now we will login to the domain with the jane_admin user
![Part 2 ok 1st ok now go to your DC-1 and select sever manager](https://github.com/user-attachments/assets/804905d6-4f5a-4e7c-baa3-09fb19c4a20d)

20th once you have logged in the computer will need to restart. Go ahead and do this. 
![Part 2 ok 1st ok now go to your DC-1 and select sever manager](https://github.com/user-attachments/assets/aa7f02c1-dbed-4182-ad8b-c7321afadc7c)


21st now back in DC-1 we will click start menu > Active directory user and computer > mydomain.com > computers > and we should see client-1 computer in here. 
![Part 2 ok 1st ok now go to your DC-1 and select sever manager](https://github.com/user-attachments/assets/f5313020-e0f8-4fd4-b1b4-6adab115a9af)


22nd and for the sake of keeping things clean we will make a new organizational unit > name it _ CLIENTS > and drag our client-1 computer into it. 
![Part 2 ok 1st ok now go to your DC-1 and select sever manager](https://github.com/user-attachments/assets/80d6ee86-f5c0-475e-856c-2e4e8354b069)







