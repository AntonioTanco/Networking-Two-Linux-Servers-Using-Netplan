## Linux Networking Using Netplan

<h3>Project Overview</h3>
<p>Virtualized two Ubuntu linux servers (24.04) using Oracle VirtualBox. Modified the servers netplan configuration (etc/netplan/50.cloud-init.yaml) in order to assign a static IPv4 Address, Subnet, Gateway Address and Nameserver to both the 'enp0s8' attached network adapters with custom routes.</p>

<h3>Project Objective</h3>
1. Configure Netplan to establish a network</br>
2. Successfully ping the other Linux Server</br>
3. Establish an SSH Connection to either server</br>

<h3> Software/ISO Used:</h3>

- <b>Oracle VM VirtualBox</b>
- <b>Ubuntu Linux v24.04</b>

<h2>Initial Setup - Node 1 (networked-node-1)</h2>

<h3>Node 1 (networked-node-1) - VM No. 1</h3>
Setup Configuration Summary:

```
HOSTNAME: networked-node-1

Administration User: node1admin

[X] Lang: EN
[X] Install OpenSSH Server
[X] Default Network Configuration - NAT Network Adapter - enp0s3
[ ] Ubuntu Pro
[ ] No Packages Installed 

```

<p>Base Installation</p>

![Linix NODE 1 - Startup Config - base installation](https://github.com/user-attachments/assets/ce7a7051-6e6b-4cf6-acb3-40b9efd375a8)

<p>Profile Configuration</p>

![Linix NODE 1 - Startup Config - profile config](https://github.com/user-attachments/assets/d5cbf10a-a726-45e4-bfd3-51362ba150f8)

<p>No Packages Install On First Configuration</p>

![Linix NODE 1 - Startup Config - no pkg installed](https://github.com/user-attachments/assets/10807a99-08e9-445c-ac96-4de62f430046)

<h3>Node 2 (networked-node-2) - VM No. 2</h3>
Setup Configuration Summary:

```
HOSTNAME: networked-node-2

Administration User: node2admin

[X] Lang: EN
[X] Install OpenSSH Server
[X] Default Network Configuration - NAT Network Adapter - enp0s3
[ ] Ubuntu Pro
[ ] No Packages Installed 

```

<p>Base Installation</p>

![Linix NODE 2 - Startup Config - base installation](https://github.com/user-attachments/assets/efb436d1-5dbe-4d14-9ccd-a7421b1f1864)

<p>Profile Configuration</p>

![Linix NODE 2 - Startup Config - profile config](https://github.com/user-attachments/assets/a07d4d3b-6f9d-4819-8350-47c3fed782c1)

<p>No Packages Install On First Configuration</p>

![Linix NODE 2 - Startup Config - no pkg installed](https://github.com/user-attachments/assets/447ad762-ce30-4ddb-86e4-542755970b9d)</br>


<h2>Terminal Commands - Node 1 (networked-node-1)</h2>

```
#Ordered List of Executed Commands

hostname 
#Displays the assigned hostname in console

sudo apt install update 
#Updating the systems local repo DB before upgrading any repositories

sudo apt upgrade 
#Upgrading the locally installed packages to their latest version

sudo apt install net-tools 
#Installing net-tools to use the ifconfig command

ifconfig -a 
#Outputs all the details of the attached network adapters

sudo shutdown -h now 
#Shutting down the linux machine in order to attach a second network adapter (VLAN - networked_node)
```

<p>hostname</p>

![Linix NODE 1 - Terminal - hostname](https://github.com/user-attachments/assets/8fdf92a0-dfa1-4102-9ef5-6141d431a91f)


<p>sudo apt install update</p>


![Linix NODE 1 - Terminal - sudo apt update](https://github.com/user-attachments/assets/d443b7a7-efa7-40d9-b34e-50ac8650570e)


<p>sudo apt upgrade</p>

![Linix NODE 1 - Terminal - sudo apt upgrade - 1](https://github.com/user-attachments/assets/65334818-85d0-437e-84be-5b1515aab16a)

<p>sudo apt install net-tools</p>

![Linix NODE 1 - Terminal - sudo apt intall net-tools](https://github.com/user-attachments/assets/e24e9e02-74b0-4422-877e-f1b77cbafab5)


