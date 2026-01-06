# On-Premises Active Directory Deployed in Azure

<p align="center">
  <img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory logo"/>
</p>


---



<h2>Deployment and Configuration Steps</h2>

1st step; let's create our resource group in Azure
![1st step lets create our resource group in Azure](https://github.com/user-attachments/assets/02ad0c70-85de-468f-a3dd-bb022f84fe60)


2nd make your virtual network in Azure and put it inside your resource group
![1st step lets create our resource group in Azure](https://github.com/user-attachments/assets/f02d266a-d0c1-4e2d-b55c-6c75355a901e)




3rd this next step is important. Once you have put the VM in your Resource group and created a name. Make sure for the image you select “ Windows server 2022 datacenter “ choose and choose a VM at least 2 CPUs for image size, and then review and create.

![1st step lets create our resource group in Azure](https://github.com/user-attachments/assets/94636678-b530-493e-b08c-e03461ff803d)



4th Now we will create another VM, but this one will be the user's computer. The first one we made will be the domain controller. Everything else is the same as the last VM, except for the image choose windows 11pro. Be sure to set your passwords and check the box at the bottom of the page for the licensing agreement.

![1st step lets create our resource group in Azure](https://github.com/user-attachments/assets/9cec0c15-88eb-4727-a669-43a5ab985ec3)

5th, make sure the client's VM is in the Active Directory v-net

![1st step lets create our resource group in Azure](https://github.com/user-attachments/assets/e7250fbd-b85a-44d1-892c-6bb74ee02a03)


6th now we need to make the IP address of the Domain controller static or set it to never change so that way our client computer doesn’t loose connection to the domain controller/network. So in Azure go to your VM list, select DC-1 on the left, select network > network settings > select the NIC ( network interface card ) or the green box looking icon at the top 

![1st step lets create our resource group in Azure](https://github.com/user-attachments/assets/6386005b-d748-4335-9d3c-8d2a296e9ba4)

7th Below you will see a list that says ipconfig in blue letters select that, then on this pag,e click the static bubble and save.
![1st step lets create our resource group in Azure](https://github.com/user-attachments/assets/6b51dd88-db4c-439f-a116-6c1f73b348cf)


8th ok, go-ahead and login to the DC-1 VM with the public IP address we will then disable the firewall for sake of the lab.
![1st step lets create our resource group in Azure](https://github.com/user-attachments/assets/1b043322-4222-4b0e-8a16-73c669e33ce1)


9th ok you are logged into your VM, right click Start Menu, go to “ run “ and type “ mf.msc.”

![1st step lets create our resource group in Azure](https://github.com/user-attachments/assets/45446f7b-ef59-4794-ba42-d5bb0d5d7a4a)


10th now below “ public profile “ you will see Windows Defender Firewall properties. In the Doman tab , private profile, public profile, and turn the firewall state off on all of these. Once done select apply and ok.
![1st step lets create our resource group in Azure](https://github.com/user-attachments/assets/b82d8464-61ad-4666-a232-c5ca75f77e2d)


11th ok now we are going to set client ones computer to use DC-1 private IP address to so it can connect to the network. Copy dc-1 private IP address.
![1st step lets create our resource group in Azure](https://github.com/user-attachments/assets/fb48fef7-15c8-4ad4-898d-613b5e9d6b00)

12th now go to Client ones Network settings in Azure and go to the NIC page
![1st step lets create our resource group in Azure](https://github.com/user-attachments/assets/31b8d9ab-6a03-47fb-b22d-4c3eb0f1da44)

13th once in the NIC page on the left select DNS settings
![1st step lets create our resource group in Azure](https://github.com/user-attachments/assets/e237ff57-e7ee-4b14-9412-96a516a11b20)


14th from here select custom and paste in the private IP of dc-1 and save. Doing this tells client 1 to use the Internet from DC-1 instead of the internet from the VM connected to the azure DNS server.
![1st step lets create our resource group in Azure](https://github.com/user-attachments/assets/6e8fa1b1-1314-4840-8a45-21f160206f18)


15th go back to client-1 and restart the VM in Azure. So the IP will reconnect
![1st step lets create our resource group in Azure](https://github.com/user-attachments/assets/f36e84e0-a549-4478-9134-5fceb4ce6dc9)

16th log back into client 1’s VM we are going to test the connection
![1st step lets create our resource group in Azure](https://github.com/user-attachments/assets/e03a785f-29d3-4b52-85e4-0b56b909942c)

17th once logged into client-1 VM type in the search bar “PowerShell “, once in PowerShell, type ping type DC-1 private IP and hit enter
![1st step lets create our resource group in Azure](https://github.com/user-attachments/assets/a5b2bbc2-9361-41bb-96b6-3ffd4c88149d)


18th if you have done everything correctly, you should see this traffic communication.
![1st step lets create our resource group in Azure](https://github.com/user-attachments/assets/75805798-5c33-477c-abb2-eb1590f92a0c)


20th, and just to be sure, with PowerShell still open, type ipconfig /all to show the DNS server info and it should be the same IP as DC-1, telling us we have connected.
![1st step lets create our resource group in Azure](https://github.com/user-attachments/assets/5f5c09d0-a7a8-4e5f-b63e-c2f0ac3cc723)


<h2>Part Two <h2>

Ok great, you have created the AD infrastructure. Now, let's actually deploy AD

Step 1 Go to your DC-1 and select Server Manager.
![Part 2 ok 1st ok now go to your DC-1 and select sever manager](https://github.com/user-attachments/assets/e3856017-f456-46e1-ac00-d6466ea88e0e)

2nd select add roles and features > next > next > server selection ( here, there should only be one server “ dc-1 “ ) select the server, then click next

![Part 2 ok 1st ok now go to your DC-1 and select sever manager](https://github.com/user-attachments/assets/94e87781-e8ef-479d-9118-b829e233748a)

3rd in server roles > Active Directory domain services > add features > next

![Part 2 ok 1st ok now go to your DC-1 and select sever manager](https://github.com/user-attachments/assets/c36bf8aa-e8d9-46a2-9329-b3e3f8165c7c)

4th for features and AD DS select next > confirmation ( check the box next to “ restart destination server “ and click next /ok and install once its finished select close and ok.
![Part 2 ok 1st ok now go to your DC-1 and select sever manager](https://github.com/user-attachments/assets/1a968882-6000-409e-99a9-4d47e0a76d27)

5th, now back in server manager on top to the right, there is a flag with a caution icon. Select that, then choose “ promote this server to a domain controller “

![Part 2 ok 1st ok now go to your DC-1 and select sever manager](https://github.com/user-attachments/assets/1e947267-c893-4a1e-836c-239239543fc6)

6th, now select “ add new forest “ and name the domain “ my domain.com “ > next
![Part 2 ok 1st ok now go to your DC-1 and select sever manager](https://github.com/user-attachments/assets/c70e0e3d-4882-4cfb-bf94-7e6d717e988a)

7th in “ domain controller options “, set the password to something you will remember. We will likely never use this > next
![Part 2 ok 1st ok now go to your DC-1 and select sever manager](https://github.com/user-attachments/assets/a2c3629a-b645-45d8-805f-bd0f9f38c7bf)


8th select next for everything, until prerequisite, then install this, then select next on this as well
![Part 2 ok 1st ok now go to your DC-1 and select sever manager](https://github.com/user-attachments/assets/fe67281a-08fe-4a8f-9b53-6375e8e93e2c)

9th once that is finished being installed, you will be signed out. Give the DC a minute, then log back in shouldn’t take long
![Part 2 ok 1st ok now go to your DC-1 and select sever manager](https://github.com/user-attachments/assets/c15b734f-19b0-4fb8-8f5f-ef76d66b248b)


10th once you are logged back into DC-1 click the start menus >windows admin tools > Active Directory users and computers
![Part 2 ok 1st ok now go to your DC-1 and select sever manager](https://github.com/user-attachments/assets/6b12401d-429a-4100-b1bb-9cd54716799c)


11th from here right click “mydomain.com “ > new > organizational unit > and name this unit ( _EMPLOYEE ) > OK

![Part 2 ok 1st ok now go to your DC-1 and select sever manager](https://github.com/user-attachments/assets/cd155918-bd7f-4432-847c-4dff244b880f)

12th repeat the same thing again but name this organizational unit ( _ADMINS )
![Part 2 ok 1st ok now go to your DC-1 and select sever manager](https://github.com/user-attachments/assets/4e3b8450-72d2-46bf-8e65-303c2a4c338e)

13th next we right click admin > new > users > and create the Jane Doe user, and the user name will be jane_admin
![Part 2 ok 1st ok now go to your DC-1 and select sever manager](https://github.com/user-attachments/assets/b0b2972a-c523-4782-8797-5fdefb6bfea2)

14th give the user a password you will remember> next > finish
![Part 2 ok 1st ok now go to your DC-1 and select sever manager](https://github.com/user-attachments/assets/1fa35893-12ae-4b7d-acf8-5ae0051ffe22)

15th now we will make this account a admin > right click Jane doe > properties > member of > add
![Part 2 ok 1st ok now go to your DC-1 and select sever manager](https://github.com/user-attachments/assets/5d4c2881-3488-478d-9224-60e4736f2a82)

16th under ( enter the object name to select ) search domain admins > check names > ok > apply > ok unity
![Part 2 ok 1st ok now go to your DC-1 and select sever manager](https://github.com/user-attachments/assets/600ebbc8-7218-47a3-9cfb-d6e059ebf97e)

17th now we will add client 1 PC to the domain. On your client-1 pc right right-click the Start menu > settings > (rename PC advanced ) or (renaming your device for better security) or ( advanced system settings)

![Part 2 ok 1st ok now go to your DC-1 and select sever manager](https://github.com/user-attachments/assets/9674ffa6-fc46-4837-9935-a01d73a00386)

18th now click on “ computer name tab “ > change > member of ( select Domain) and enter “ mydomain.com” > ok
![Part 2 ok 1st ok now go to your DC-1 and select sever manager](https://github.com/user-attachments/assets/034040c7-6427-4a51-a9ab-5ea8074daf44)

19th, now we will log in to the domain with the jane_admin user
![Part 2 ok 1st ok now go to your DC-1 and select sever manager](https://github.com/user-attachments/assets/804905d6-4f5a-4e7c-baa3-09fb19c4a20d)

20th once you have logged in, the computer will need to restart. Go ahead and do this.
![Part 2 ok 1st ok now go to your DC-1 and select sever manager](https://github.com/user-attachments/assets/aa7f02c1-dbed-4182-ad8b-c7321afadc7c)


21st now back in DC-1 we will click start menu > Active directory user and computer > mydomain.com > computers > and we should see client-1 computer in here.
![Part 2 ok 1st ok now go to your DC-1 and select sever manager](https://github.com/user-attachments/assets/f5313020-e0f8-4fd4-b1b4-6adab115a9af)


22nd, and for the sake of keeping things clean, we will make a new organizational unit > name it _ CLIENTS > and drag our client-1 computer into it.
![Part 2 ok 1st ok now go to your DC-1 and select sever manager](https://github.com/user-attachments/assets/80d6ee86-f5c0-475e-856c-2e4e8354b069)

--------------------------------------------

Part 3


1st step ok great. Now we are going to log in to our Client-1 with Jane's admin account
![Part 3 1st step ok great now we are going to log into our Client-1…](https://github.com/user-attachments/assets/0271caf2-eb27-4b1b-88e0-6791904b3011)

￼
2nd go to your start menu > search “ Remote Desktop “ > then click “select who can remotely use this PC. “
![Part 3 1st step ok great now we are going to log into our Client-1…](https://github.com/user-attachments/assets/69609160-e9b4-4543-beab-edc80a467af0)

￼
3rd click “add “ > then type “ domain users “ in the search box > ok > ok
This allows all the users we will make to be able to log in on the client-1 PC
![Part 3 1st step ok great now we are going to log into our Client-1…](https://github.com/user-attachments/assets/6aa846ea-634f-47ea-8381-a34d9cee4bb3)


￼
4th ok now log into DC-1 PC as an admin > start menu type “PowerShell ISE “ open this as an admin
![Part 3 1st step ok great now we are going to log into our Client-1…](https://github.com/user-attachments/assets/e5b39f86-9865-4797-bdc7-f6e5c587ca50)

￼
5th, ok great now open this GITHUB link ( link here ) and copy this code into the script > you can use control S to save this script on the desktop> and then click the green play button at the top of the page. This is going to create a whole bunch of users for us to select.

![Part 3 1st step ok great now we are going to log into our Client-1…](https://github.com/user-attachments/assets/417539da-049b-4c7a-8e52-73d15438dbd7)

￼
6th, if you go back to Active Directory, and click on the _EMPLOYEES folder, you can see a whole bunch of users being made. Select one of these users, and we will log in with one of them. The password for the account is Password1
![Part 3 1st step ok great now we are going to log into our Client-1…](https://github.com/user-attachments/assets/4bc9defb-0a92-4c72-b32f-7cc3872d46f4)

￼
7th log into client-1 pc with the user of choice

![Part 3 1st step ok great now we are going to log into our Client-1…](https://github.com/user-attachments/assets/e29b8cf4-3ddd-4e7b-a4ec-e9acf190255f)

￼
8th once logged in, you can open power shell and enter “ whoami” to get the computer name and you can also open up the file explorer > this PC > Windows (C:) > users, and you will see all the other client we have logged in with so far.

![Part 3 1st step ok great now we are going to log into our Client-1…](https://github.com/user-attachments/assets/4788ae39-bed1-458c-ae7e-8b36767d4199)

￼----------------------------------
Part 4

1st, we need to setup up the account lock out policy for our accounts on dc-1 so get logged into DC-1, then select start > run > gpmc.msc
￼
![Part 4 step one finally we will set up the accounts in Active Directory…](https://github.com/user-attachments/assets/baa7a63b-b4a4-4f7d-8185-050d2b0d5ba5)

2nd from GPMC > forest : mydomain.com > domains > mydomain.com > right click on Default domain policy > edit
![Part 4 1st we need to setup up the account lock out policy for our…](https://github.com/user-attachments/assets/cfc4c933-7c21-41db-9413-4cc92c8b8a79)

￼
3rd from here click on computer configuration > policies > windows settings > security settings > account policies > account lockout policy
![Part 4 1st we need to setup up the account lock out policy for our…](https://github.com/user-attachments/assets/fd363857-6054-4a75-8ec1-ec9ffe1f1bfd)

￼
4th on the right panel, you will see the policy configurations. Click on account lockout duration > set the lockout duration to 30min > apply > ok
This will set all the other settings automatically

![Part 4 1st we need to setup up the account lock out policy for our…](https://github.com/user-attachments/assets/4aa446a3-aed9-4b55-9dc5-8c7517ca65d7)



￼
5th, now that we have updated the lockout policy, we need to force the computer to update to it. So log in as an admin to client-1

![Part 4 1st we need to setup up the account lock out policy for our…](https://github.com/user-attachments/assets/e8f0ed9a-22d5-47d5-8b17-138ee65fbd73)

￼
6th start menu > run > powershell > gpupdate /force to update the group policy on client 1
![Part 4 1st we need to set up the account lockout policy for our…](https://github.com/user-attachments/assets/36444b6c-16ab-4950-8d1f-9efeab8d7b26)

￼
7th now log out of client 1 and find a random user to log into. We are going to test the policy by locking ourselves out, and after 5 failed attempts, you should see this

![Part 4 1st we need to setup up the account lock out policy for our…](https://github.com/user-attachments/assets/0985368f-2051-4a82-b42a-36c79a8cc4e4)

￼
8th ok now that we have locked ourselves out, lets unlock the account again. Back on DC-1 start > active directory user and computers > right click “my domain.com” > find user > search your user that’s locked out > at the bottom click on that user
![Part 4 1st we need to set up the account lockout policy for our…](https://github.com/user-attachments/assets/c25b648b-6bfd-40d0-92c6-b607b23eefab)

￼
9th on the pop up screen search for account profile > under logon hours you should see a box next to “ unlock account “ check this box and apply it. This will unlock the account.

![Part 4 1st we need to setup up the account lock out policy for our…](https://github.com/user-attachments/assets/c1f9389a-878c-4fb2-b3ec-42dc3778d097)

￼
10th now with the account unlocked, try to log back, and you should be able too.

![Part 4 1st we need to setup up the account lock out policy for our…](https://github.com/user-attachments/assets/c308ae51-1ec3-45b0-8ccb-abb562d04dc1)

￼
11th back in DC-1 just to show you a few more things here. If you right-click on the user, you can see other things we can do to the account, like reset the password, disable the account, add it to a certain group with special access to certain folders on the network, etc
![Part 4 1st we need to set up the account lockout policy for our…](https://github.com/user-attachments/assets/8dbc07d0-d4e9-4956-9eaf-67ceae469644)

￼
12th disabling accounts is something you might find yourself doing as a help desk agent, so let's disable an account by right-clicking on the account > disable it > you will see this pop up on screen
![Part 4 1st we need to set up the account lockout policy for our…](https://github.com/user-attachments/assets/ade8254c-897d-4264-b65f-da1275474865)

￼
13th ok now try to log back in to that account , you should see this pop up. Which means the account is disable
![Part 4 1st we need to setup up the account lock out policy for our…](https://github.com/user-attachments/assets/dd715c49-fe9f-4d9f-91ae-b65e8c7c066b)

￼
14th now back in active directory reenable the account the same way you disabled it.

![Part 4 1st we need to setup up the account lock out policy for our…](https://github.com/user-attachments/assets/29747c74-5e76-4d64-8a70-4ddc45934809)

￼
15th, with the account re-enabled, you should be able to log in now.

![Part 4 1st we need to setup up the account lock out policy for our…](https://github.com/user-attachments/assets/4170588d-4a70-4ab5-93d1-8a394a6cdfdd)

￼
16th next we are going to observe some log like all the failed login attempts from earlier, so DC-1 > start > run > type “ event.vwrmsc “ enter
![16th next we are going to observe some log like all the failed log…](https://github.com/user-attachments/assets/0f7dd46c-9c4b-4af2-90ba-80b8e324832a)

￼
17th, ok great now we will observe some log in events like the all the failed log ins from earlier when we disabled the account. Lets go to DC-1 > start > event viewer

![16th next we are going to observe some log like all the failed log…](https://github.com/user-attachments/assets/74fc44b9-d3f4-4c74-9a60-407a72f8ee3d)

￼
18th once in the event viewer > windows logs > security. Then right click on the security on the left panel > find > search your account you did the failed login attempts with.

![16th next we are going to observe some log like all the failed log…](https://github.com/user-attachments/assets/dd4cc790-1701-4a4b-a91d-571adc2402f3)

￼
19th now as you probably had a hard time finding the failed log in attempts on DC-1 lets search for them on client-1 as they should be there.

Once you are logged into Client-1, after some digging, you will see the key lock icon, those are the failed logged attempts from earlier they say Audit failure. You can also see any other changes made to the account here as well. Congrats you have now installed and configured an entire AD system. These systems are used on college campuses or large corporate buildings where multiple people would use the same or different computers to access their accounts from anywhere on campus without having to carry the computer with them.
![Part 4 1st we need to setup up the account lock out policy for our…](https://github.com/user-attachments/assets/2c6c694a-131d-4309-8c28-34cffbdbb1dc)



￼





