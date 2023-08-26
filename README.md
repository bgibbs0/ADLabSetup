<h1>Description</h1>
In this lab I am setting up a Active Directory (AD) environment in Azure. This environment will be the foundation of future Active Directory labs I will be diving into. The AD environment will have two Domain Controllers that will replicate each other, enabling redundancy and high availbility incase one were to go offline. 

<h2>Utilities Used</h2>
- Azure: Used to host virtual machines and create a virtual network. </br>
- Windows Server 2022: Operating system that the Domain Controllers will be running. </br>
- Active Directory: Directory service used to create users and groups. </br>
- Group Policy: Used to allow Remote Desktop connections for users created in lab environment. 


<h2>Glossary</h2>
Active Directory - Active Directory (AD) is a directory service developed by Microsoft for Windows domain networks </br>
Azure - Microsoft's cloud computing platform. </br>
Group Policy - Group Policy is an infrastructure that allows administrators to specify managed configurations for users and computers through Group Policy settings and Group Policy Preferences. </br>

<h2>Lab Walkthrough</h2>

I first started off by creating a resource group named `ADLab`. This resource group will contain all the components of this lab. 
<p align="center">
Azure Resource Group: </br>
<img src="https://i.imgur.com/fa5YtQy.png"
  </p>
</br>

I then deployed a virtual network. This virutal network will allow our virtual machines to be in the same subnet and communicate with each other. </br>
<p align="center">
Azure Virtual Network: </br>
<img src="https://i.imgur.com/azihxlX.png"
  </p>
</br>
<p align="center">
Azure Virtual Network: </br>
<img src="https://i.imgur.com/hWDmWpl.png"
  </p>
</br>

I provisioned the virtual network a 10.0.0.0/22 address space. This space contains 1,024 IP addresses. Out of the 10.0.0.0/22, I subnetted the 10.0.0.0/24 network that contains 256 addresses. The /24 subnet will be where my Active Directory environment lives. </br>

<p align="center">
Azure Virtual Network: </br>
<img src="https://i.imgur.com/KnooqX9.png"
  </p>
</br>

I named the virtual network `ADPrem`. </br>

<p align="center">
Azure Virtual Network: </br>
<img src="https://i.imgur.com/FQVu3ue.png"
  </p>
</br>

With the virtual network created, I moved on to spinning up the virtual machines that will be the Domain Controllers. The first virtual machine I created was named `DC01` and used a `Windows Server 2022` image. </br>
<p align="center">
DC01 Virutal Machine: </br>
<img src="https://i.imgur.com/T7ftsMV.png"
  </p>
</br>

During the creation of `DC01`, I attached a seperate disk volume that will be used to host Active Directory data seperatley from the rest of the VM encase of corruption. I provisioned `DC01_DataDisk_0` 10 Gigabytes. </br>
<p align="center">
DC01 Virutal Machine: </br>
<img src="https://i.imgur.com/q5xdaO4.png"
  </p>
</br>

I then made sure that `DC01` would be deployed in the 10.0.0.0/24 subnet. </br>
<p align="center">
DC01 Virutal Machine: </br>
<img src="https://i.imgur.com/hxeufaV.png"
  </p>
</br>

`DC01` was provisioned a public IP address of `20.163.204.78`. This IP address will let me connect through a Remote Desktop session with my admin credentials created during the initial set up. On the `ADPrem` virtual network, `DC01` has a private IP address of `10.0.0.4`.
<p align="center">
DC01 Virutal Machine: </br>
<img src="https://i.imgur.com/Te64UMN.png"
  </p>
</br>

After `DC01` was successfully deployed, I repeated the same set up process for `DC02` which will be the second Domain Controller. `DC02` will replicate `DC01` to ensure there is a backup of Active Directory. </br>
<p align="center">
DC02 Virutal Machine: </br>
<img src="https://i.imgur.com/mVezImY.png"
  </p>
</br>

`DC02` was provisioned a public IP address of `172.178.48.39` and a private IP address of `10.0.0.5` on the virtual network. </br>

<p align="center">
DC02 Virutal Machine: </br>
<img src="https://i.imgur.com/DsLXhdY.png"
  </p>
</br>

After the virtual machines were created, I initiated a Remote Desktop session to `DC01` at IP address `20.163.204.78`. I logged in with my administrator credentials. </br>
<p align="center">
DC01 Virutal Machine: </br>
<img src="https://i.imgur.com/J9HPTY8.png"
  </p>
</br>

Once I was connected, I first went into Disk Management to initialize the `DC01_DataDisk_0` disk I allocated to the VM. I assigned it a drive letter of `F:` </br>
<p align="center">
DC01 Virutal Machine: </br>
<img src="https://i.imgur.com/MArXz2e.png"
  </p>
</br>
<p align="center">
DC01 Virutal Machine: </br>
<img src="https://i.imgur.com/jHGLbXX.png"
  </p>
</br>

I then added the `Active Directory Domain Services` role to the VM. </br>

<p align="center">
DC01 Virutal Machine: </br>
<img src="https://i.imgur.com/xtELxYP.png"
  </p>
</br>
<p align="center">
DC01 Virutal Machine: </br>
<img src="https://i.imgur.com/WZMTAoE.png"
  </p>
</br>
<p align="center">
DC01 Virutal Machine: </br>
<img src="https://i.imgur.com/UEl7YKF.png"
  </p>
</br>
<p align="center">
DC01 Virutal Machine: </br>
<img src="https://i.imgur.com/Udt00zM.png"
  </p>
</br>
<p align="center">
DC01 Virutal Machine: </br>
<img src="https://i.imgur.com/trr9qv5.png"
  </p>
</br>
<p align="center">
DC01 Virutal Machine: </br>
<img src="https://i.imgur.com/0fDgPRQ.png"
  </p>
</br>

In this stage, I promoted `DC01` to a Domain Controller. Since there isn't a domain yet, I chose the `Add a new forest` option. This creates a Active Directory Forest, a Forest is the top most layer of an Active Directory environment. The Forest is able to host multiple domains an organization might maintain. I gave a root domain name of `BrianTestLabCorp.com`. And a NetBIOS domain name of `BRIANTESTLABCOR` (I missed out on the P because there is a 15 character limit). </br>

<p align="center">
DC01 Virutal Machine: </br>
<img src="https://i.imgur.com/8PKKoJW.png"
  </p>
</br>

<p align="center">
DC01 Virutal Machine: </br>
<img src="https://i.imgur.com/e2LH1sJ.png"
  </p>
</br>

During this set up process, I defined the location that the AD DS database, log files and SYSVOL will be stored. I chose the `F:` drive so that the AD data is on it's own volume. This will reduce the chances of the AD data being accidentally altered. </br>

<p align="center">
DC01 Virutal Machine: </br>
<img src="https://i.imgur.com/aaUzs8M.png"
  </p>
</br>

<p align="center">
DC01 Virutal Machine: </br>
<img src="https://i.imgur.com/5tzEdhf.png"
  </p>
</br>

I then navigated back to Azure. From here I changed the network interface of `DC01` from a dynamic IP address to a static one. `DC01` will also act as a DNS server and with a static IP address, it can provide reliable DNS services. </br>

<p align="center">
DC01 Virutal Machine: </br>
<img src="https://i.imgur.com/Yeey5m8.png"
  </p>
</br>
<p align="center">
DC01 Virutal Machine: </br>
<img src="https://i.imgur.com/6n2wO6K.png"
  </p>
</br>

I then went back to `Active Directory Users and Computers` and can see that `DC01` has been added into the `Domain Controllers` OU. This means that `DC01` has successfully been promoted to a Domain Controller. </br>

<p align="center">
DC01 Virutal Machine: </br>
<img src="https://i.imgur.com/tbHprj3.png"
  </p>
</br>

I repeated the same process with `DC02` by initiating a Remote Desktop session to IP address `172.178.48.39`. </br>

<p align="center">
DC02 Virutal Machine: </br>
<img src="https://i.imgur.com/ERdx536.png"
  </p>
</br>

During the process of promoting `DC02` to a Domain Controller, instead of adding a new forest, I chose `Add a domain controller to an existing domain` since I had already done so in the `DC01` promotion. I specified the `BrianTestLabCorp.com` domain to joined. I also set up `DC02` to replicate `DC01`, this will ensure that the Domain Controllers can be maintenanced without effecting Active Directory Domain Services. </br>

<p align="center">
DC02 Virutal Machine: </br>
<img src="https://i.imgur.com/E591NaX.png"
  </p>
</br>
<p align="center">
DC02 Virutal Machine: </br>
<img src="https://i.imgur.com/X3c5ixp.png"
  </p>
</br>

Now that `DC02` is a part of the `BrianTestLabCorp.com` domain, I can connect through Remote Desktop with domain admin credentials. </br>

<p align="center">
DC02 Virutal Machine: </br>
<img src="https://i.imgur.com/0y3a0w3.png"
  </p>
</br>

I verified that `DC01` and `DC02` were both in the same domain and showed up in Active Directory. </br>

<p align="center">
DC02 Virutal Machine: </br>
<img src="https://i.imgur.com/xvWITK8.png"
  </p>
</br>


With the Domain Controllers set up, I connected back to `DC01` and tested out creating a Organizational Unit and a few users. An Organizationl Unit is a container within a Microsoft Active Directory domain which can hold users, groups and computers. I created a `Information Technology` OU and created three users to reside in the OU. </br>

<p align="center">
Active Directory Users and Computers: </br>
<img src="https://i.imgur.com/69l92x9.png"
  </p>
</br>

I wanted to enable one of the users to connect to the Domain Controllers through Remote Desktop. I added `Steve Jobs` to the `Remote Desktop Users` security group. Security groups are a way to collect user accounts, computer accounts, and other groups into manageable units. This way I can specify what users should have Remote Desktop privileges. </br>

<p align="center">
Active Directory Users and Computers: </br>
<img src="https://i.imgur.com/EavmC3m.png"
  </p>
</br>

<p align="center">
Active Directory Users and Computers: </br>
<img src="https://i.imgur.com/eCvrxfw.png"
  </p>
</br>

Before I could remote in with `Steve Jobs`, I needed to allow the `Remote Desktop Users` security group the ability to log in through Remote Desktop Services using Group Policy. Group Policy is an infrastructure that allows you to specify managed configurations for users and computers through Group Policy settings. This signficantly improves the efficiency of managing user permissions. </br>

<p align="center">
Group Policy Management Editor: </br>
<img src="https://i.imgur.com/ZXwotCG.png"
  </p>
</br>
<p align="center">
Group Policy Management Editor: </br>
<img src="https://i.imgur.com/i7MuoeE.png"
  </p>
</br>

With this Group Policy setting configured for `Remote Desktop Users`, I was able to log into `DC01` with `Steve Jobs`. I ran a `whoami` command once logged into the VM as a proof of concept. </br>

<p align="center">
DC01 Virtual Machine: </br>
<img src="https://i.imgur.com/ibMbdVl.png"
  </p>
</br>
<p align="center">
DC01 Virtual Machine: </br>
<img src="https://i.imgur.com/dkXXvkh.png"
  </p>
</br>

<h2>Conclusion</h2>
