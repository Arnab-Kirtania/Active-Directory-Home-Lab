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
<h1>Creating Users</h1>
<h1>Setting Up Client Machine</h1>
<h1>Finishing Up</h1>
