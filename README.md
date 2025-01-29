# Active-Directory-Home-Lab
How I created a home lab that implements an active directory with around 1000 users via PowerShell.

<h1>Files and links you'll need for this</h1>

Please download the following for this project. Also, download the extension pack for Oracle Virtualbox. <br>

[Oracle VirtualBox](https://www.virtualbox.org/wiki/Downloads) <br>
[Server 2019 ISO](https://www.microsoft.com/en-us/evalcenter/download-windows-server-2019) <br>
[Windows 10 ISO](https://www.microsoft.com/en-us/software-download/windows10) <br>
[Create Accounts Script](https://github.com/joshmadakor1/AD_PS...) <br>

<h1>Setting Up the Domain Controller</h1>
Set the version to Other Windows (64-bit), give the VM 2 GB of RAM, and a single core. <br>

Do note that you can increase the specs if the machine you're running the VM on can handle it. <br>

![alt text][logo]

[logo]: https://github.com/Arnab-Kirtania/Active-Directory-Home-Lab/blob/main/1.png "Creating DC"

Enabling bidirectional for both of these options simply makes it easier to do things between the VM and my actual machine. <br>

![alt text][logo1]

[logo1]: https://github.com/Arnab-Kirtania/Active-Directory-Home-Lab/blob/main/2.png "Settings change"

Adpater 1 should already attached to NAT, but if it isn't set it to that. <br>
Then add  a second adapter. This will act as the internal NIC on the domain controller that will allow the client machine (will be created later) to access the internet despite being on an internal network. <br>

![alt text][logo2]

[logo2]: https://github.com/Arnab-Kirtania/Active-Directory-Home-Lab/blob/main/3.png "Adapter settings"

Next boot up the domain controller. Here you'll need to add the Windows Server ISO. FOr me it gave me an issue with a DVD booter, so I selected the other file option and selected the server ISO. From there I let it install. <br>

![alt text][logo3]

[logo3]: https://github.com/Arnab-Kirtania/Active-Directory-Home-Lab/blob/main/4.png "Server setup"

From here I selected the standard desktop experience so I would have access to a GUI and a CLI. <br>

![alt text][logo4]

[logo4]: https://github.com/Arnab-Kirtania/Active-Directory-Home-Lab/blob/main/5.png "Server selection"

Next hit custom install and speed through the other things. Then it will take a bit for everything to properly install. <br>

![alt text][logo5]

[logo5]: https://github.com/Arnab-Kirtania/Active-Directory-Home-Lab/blob/main/6.png "Custom Install"

Once here we can set the password of the built-in admin account. For the sake of simplicity, I'll be setting the password to Password1. In a real scenario I would not do this because that password is extremely insecure. <br>

![alt text][logo6]

[logo6]: https://github.com/Arnab-Kirtania/Active-Directory-Home-Lab/blob/main/7.png "Admin account settings"

Next login to the admin account. Because this is a VM you will likely need to use a special key combo to login. However, it's a lot easier to just go to input and clcik the combination you need. <br>

![alt text][logo7]

[logo7]: https://github.com/Arnab-Kirtania/Active-Directory-Home-Lab/blob/main/8.png "Admin account log in"

Once inside, to make our VM experience a lot smoother we need to go to devices and insert guest additions. <br>

![alt text][logo8]

[logo8]: https://github.com/Arnab-Kirtania/Active-Directory-Home-Lab/blob/main/9.png "Adding guest additions"

Then run this application, run through everything, and then shutdown the VM manually. <br>

![alt text][logo9]

[logo9]: https://github.com/Arnab-Kirtania/Active-Directory-Home-Lab/blob/main/10.png "Installing guest additions"

Once you log back in, find your way to this panel and determine which ethernet is your internet NIC and which one is your internal NIC. Then label them accordingly. <br>

![alt text][logo10]

[logo10]: https://github.com/Arnab-Kirtania/Active-Directory-Home-Lab/blob/main/11.png "Labelling networks"

Then we need to go into the properties of the internal network and assign an IP to it. This will allow the DC to act as its own DNS server. We also don't need a default gateway because the DC will serve as one. <br>

![alt text][logo11]

[logo11]: https://github.com/Arnab-Kirtania/Active-Directory-Home-Lab/blob/main/12.png "IP Assignment"

After that we'll rename the PC to DC because it is the domain controller. Then let the VM restart and go through the login process again.

![alt text][logo12]

[logo12]: https://github.com/Arnab-Kirtania/Active-Directory-Home-Lab/blob/main/13.png "Renaming the PC"

<h1>Setting Up Active Directory Domain Services</h1>

Once back in we will click add roles and features and then add Active Directory Domain Services (AD DS). Then go through the process and install. Do note that it may take a while. <br>

![alt text][logo13]

[logo13]: https://github.com/Arnab-Kirtania/Active-Directory-Home-Lab/blob/main/14.png "Adding AD DS"

Once it is installed we need to click the warning symbol and promote the server to a domain controller. Then we're going to add a new forest and for this example we will name the domain, mydomain.com. <br>

![alt text][logo14]

[logo14]: https://github.com/Arnab-Kirtania/Active-Directory-Home-Lab/blob/main/15.png "Deploying domain"

The rest of the process is straightforward so go through the installation process. The VM will restart at the end to properly promote the DC. <br>

![alt text][logo15]

[logo15]: https://github.com/Arnab-Kirtania/Active-Directory-Home-Lab/blob/main/16.png "Promoting DC"

Once restarted, log in again. Note that the admin account now has the domain in front of it now!

![alt text][logo16]

[logo16]: https://github.com/Arnab-Kirtania/Active-Directory-Home-Lab/blob/main/17.png "Logging in"

Once logged back in we'll go to Active Directory Users and Computers. <br>

![alt text][logo17]

[logo17]: https://github.com/Arnab-Kirtania/Active-Directory-Home-Lab/blob/main/18.png "Navigating to Active Directory"

Once here we'll create a new organizational unit (OU) called _ADMINS. They are essentially folders that are used to organize and manage user accounts, computer accounts, printers, and other things. <br>

![alt text][logo18]

[logo18]: https://github.com/Arnab-Kirtania/Active-Directory-Home-Lab/blob/main/19.png "Adding Organizational Unit"

Now create a new user in this folder. Just give it your name. For the logon name I'm adding a "a-" in front to signify that this is an admin account. <br>

![alt text][logo19]

[logo19]: https://github.com/Arnab-Kirtania/Active-Directory-Home-Lab/blob/main/20.png "Adding a user"

However, even though the user is in the _ADMINS OU, that does not mean the user is an admin. We need to go into the properties of our new account and make them a member of the Domain Admins. Once done, hit apply then ok. <br>

Now that we have this, sign out of the domain controller. <br>

![alt text][logo20]

[logo20]: https://github.com/Arnab-Kirtania/Active-Directory-Home-Lab/blob/main/21.png "Promoting a user"

Use your new domain admin account and log in with that. <br>

![alt text][logo21]

[logo21]: https://github.com/Arnab-Kirtania/Active-Directory-Home-Lab/blob/main/22.png "Logging in"

<h1>Setting Up RAS & NAT</h1>

Now that we are back in we need to install Remote Access Server and Network Address Translation (RAS/NAT). This will allow the future Windows 10 client we'll make to be on a private virtual network but still access the internet through the DC. <br>

We'll do this by going to add roles and then remote access. After that install routing as well. It should show up while going through the installation process. <br>

![alt text][logo22]

[logo22]: https://github.com/Arnab-Kirtania/Active-Directory-Home-Lab/blob/main/23.png "Adding RAS"

Once that is done, go back to the server manager, go to tools, then routing and remote access, and then configure the DC. Then click NAT and choose the internet interface. Then finish. <br>

If the internet interface we set up earlier doesn't show up initially just close out of the configure window and repeat the steps to get there again. <br>

![alt text][logo23]

[logo23]: https://github.com/Arnab-Kirtania/Active-Directory-Home-Lab/blob/main/24.png "Configuring DC"

<h1>Setting Up DHCP Server</h1>

With that done now we need to set up a Dynamic Host Configuration Protocol DHCP Server on our DC. The DHCP will allow the Windows 10 client users to get an IP address to get on the internet despite being on a private network. <br>

As per usual, go back to the server manager, add roles, and select DHCP Server. <br>

![alt text][logo24]

[logo24]: https://github.com/Arnab-Kirtania/Active-Directory-Home-Lab/blob/main/25.png "Adding DHCP Server"

Next go to tools, dhcp, open the dop down menu for our domain, then right-click IPv4 and select new scope. <br>

![alt text][logo25]

[logo25]: https://github.com/Arnab-Kirtania/Active-Directory-Home-Lab/blob/main/26.png "Setting Up Scope"

Set the range of IPs to these, and make sure the subnet mask matches the one on our internal NIC for the DC. We're not going to be excluding any IPs. <br>

![alt text][logo26]

[logo26]: https://github.com/Arnab-Kirtania/Active-Directory-Home-Lab/blob/main/27.png "Setting IP range"

This just sets how long a computer can have this IP address before it needs to be refreshed. For example if you were a cafe and someone joins your network for half and hour and leaves and you had a lease time of 8 days, that IP address would not be usable for 8 days until it refreshed. So use your best judgment on how long leases should last. For this example we'll be keeping it to 1 day. <br>

![alt text][logo27]

[logo27]: https://github.com/Arnab-Kirtania/Active-Directory-Home-Lab/blob/main/28.png "Setting lease duration"

Tell the DHCP that we want to configure stuff now and go to the next panel. Here we need to add our DC's IP address. Remember to hit Add otherwise it will break things later on in the process. <br>

Then you can go through everything else and finish. <br>

![alt text][logo28]

[logo28]: https://github.com/Arnab-Kirtania/Active-Directory-Home-Lab/blob/main/29.png "Adding IP addresses"

Then go to the DHCP server and hit authorize to activate everything. Right-click it again and refresh it if you don't see any changes. <br>

![alt text][logo29]

[logo29]: https://github.com/Arnab-Kirtania/Active-Directory-Home-Lab/blob/main/30.png "Authorizing DHCP Server"

<h1>Creating Users</h1>

With the DHCP now set up we need to tackle adding users to the DC. It would take forever adding over 1000 accounts manually, so we're just going to use a powershell script. <br>

But before we can do that we need to set up a local server and disable IE Advanced security so we can download the file to our desktop without too much trouble. You wouldn't want to do this in a live production environment, however this is a lab. <br>

![alt text][logo30]

[logo30]: https://github.com/Arnab-Kirtania/Active-Directory-Home-Lab/blob/main/31.png "Disabling IE Advanced Security"

Now just copy and paste the link to creating accounts into the browser and download this file. It contains the powershell script and a .txt file of roughly 1000 names. Save it to the desktop and you should be good to go.

![alt text][logo31]

[logo31]: https://github.com/Arnab-Kirtania/Active-Directory-Home-Lab/blob/main/32.png "Downloading Powershell Script"

You can also open the txt file to add your name into the mix as well to make  a non-admin account for yourself. <br>

![alt text][logo32]

[logo32]: https://github.com/Arnab-Kirtania/Active-Directory-Home-Lab/blob/main/33.png "names.txt file"

Next we need to boot up powershell so we can run our script properly. <br>

![alt text][logo33]

[logo33]: https://github.com/Arnab-Kirtania/Active-Directory-Home-Lab/blob/main/34.png "Opening Powershell"

Once powershell is running, clcik the folder to open the create users script. <br>

![alt text][logo34]

[logo34]: https://github.com/Arnab-Kirtania/Active-Directory-Home-Lab/blob/main/35.png "Opening the Powershell script"

The way this script works is by taking in the names file, breaking each name into the components it needs to fill out the new user info in active directory and generating a username for the person as well. <br>

In a more realistic scenario there would likely be some numbers added as well to ensure people don't have the same username, but this is a lab so we're not going to worry about that. <br>

![alt text][logo35]

[logo35]: https://github.com/Arnab-Kirtania/Active-Directory-Home-Lab/blob/main/36.png "Powershell script code"

Before we can fully run the script we do need to set the execution policy to unrestricted so that we can actually run our script. Just say yes to all. I wouldn't reccomend doing this in an actual environment but for the sake of our lab we'll do it this way. <br>

![alt text][logo36]

[logo36]: https://github.com/Arnab-Kirtania/Active-Directory-Home-Lab/blob/main/37.png "Setting execution policy"

We also need to be in the proper directory to run the script so we need to use cd to move into the right area. <br>

![alt text][logo37]

[logo37]: https://github.com/Arnab-Kirtania/Active-Directory-Home-Lab/blob/main/38.png "Changing directories"

All that's left to do now is hit the green play button and watch as 1000 accounts get added into the active directory. <br>

![alt text][logo38]

[logo38]: https://github.com/Arnab-Kirtania/Active-Directory-Home-Lab/blob/main/39.png "Adding Users"

You can double check this by opening active directory once more and checking _USERS. You can even locate specific users, such as your own non-admin account! <br>

![alt text][logo39]

[logo39]: https://github.com/Arnab-Kirtania/Active-Directory-Home-Lab/blob/main/40.png "Checking Users"

<h1>Setting Up Client Machine</h1>

With that done we're in the home stretch. What we need to do now is create our Windows 10 client. We can give it similar specs to the DC. Also do things such as enabling bidirectional in the settings, and set Adapter 1 to internal network. <br>

![alt text][logo40]

[logo40]: https://github.com/Arnab-Kirtania/Active-Directory-Home-Lab/blob/main/41.png "Creating client machine"

Now if you try to boot this up as is it won't work. You need to grab a Windows 10 64-bit ISO file. Microsoft gives us a downloadable exe that if we run we can create an ISO file. <br>

![alt text][logo41]

[logo41]: https://github.com/Arnab-Kirtania/Active-Directory-Home-Lab/blob/main/42.png "Creating ISO file"

Finally when you're here, boot up with the windows iso you now have Windows 10 running.

![alt text][logo42]

[logo42]: https://github.com/Arnab-Kirtania/Active-Directory-Home-Lab/blob/main/43.png "Booting Windows 10"

Click I don't have a product key. <br>

![alt text][logo43]

[logo43]: https://github.com/Arnab-Kirtania/Active-Directory-Home-Lab/blob/main/44.png "Choosing no poduct key"

Select windows 10 Pro, then go with a custom install. Then just speed through the process and go with no internet and a limited set up. <br>

![alt text][logo44]

[logo44]: https://github.com/Arnab-Kirtania/Active-Directory-Home-Lab/blob/main/45.png "Choosing Windows version"

After that create a new user and it doesn't need a password. Then you can disable all the unnecessary stuff. <br>

![alt text][logo45]

[logo45]: https://github.com/Arnab-Kirtania/Active-Directory-Home-Lab/blob/main/46.png "Adding a new user"

Once you log into the client account open up the command line and check if everything is set up properly. If your default gateway matches the DC you should be good. <br>

![alt text][logo46]

[logo46]: https://github.com/Arnab-Kirtania/Active-Directory-Home-Lab/blob/main/47.png "Checking IP"

You can also go back to the DC to check if the client has a lease! <br>

![alt text][logo47]

[logo47]: https://github.com/Arnab-Kirtania/Active-Directory-Home-Lab/blob/main/48.png "Checking lease"

Now do a quick test ping to any website. If it works that means that entire infrastructure we have been creating has worked! <br>

![alt text][logo48]

[logo48]: https://github.com/Arnab-Kirtania/Active-Directory-Home-Lab/blob/main/49.png "Test ping"

Now go to settings, then rename this PC advanced, then change (do not type stuff in the computer description box). <br>

Now we need to add this client to be part of the actual domain. So set the domain to be the same as the DC. Then just log in with your domain admin account and it should be added. <br>

You can also change the PC name here to be something more normal. Then restart to apply the changes. <br>

![alt text][logo49]

[logo49]: https://github.com/Arnab-Kirtania/Active-Directory-Home-Lab/blob/main/50.png "Changing PC name and adding it to the domain"

Once you do that you can go and check the DHCP to see if the changes were applied. <br>

![alt text][logo50]

[logo50]: https://github.com/Arnab-Kirtania/Active-Directory-Home-Lab/blob/main/51.png "Checking DHCP"

<h1>Finishing Up</h1>

Now go back to Client1 and see if you can login with one of the user accounts in the active directory.

![alt text][logo51]

[logo51]: https://github.com/Arnab-Kirtania/Active-Directory-Home-Lab/blob/main/52.png "Logging in with one of the active diectory accounts we made"

Once you log in, do one last check in the command line and that should be it. You've created a VMware network with a domain controller that has around 1000 users and a Windows 10 computer on a private internal network that can access the internet with any one of the accounts in active directory. <br>

![alt text][logo52]

[logo52]: https://github.com/Arnab-Kirtania/Active-Directory-Home-Lab/blob/main/53.png "Final ping check"

