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

> [!NOTE]
> The same commands executed on Node 1 have also been executed on Node 2 in the exact same order, you may reference the screenshots provided in this git repo located inside of 'Linux Screenshot - Initial Configuration' for the outputs of those commands executed on either linux node. 

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

<p>ifconfig -a</p>

![Linix NODE 1 - Terminal - ifconfig -a ](https://github.com/user-attachments/assets/c1a061c0-a76d-46f1-88f7-17f7378025a6)

<p>sudo shutdown -h now</p>
<p>At this point in the project, I've confirmed that the first network adapter was recongized and I will shutdown this VM to attach another network adapter in VirtualBox</p>

![Linix NODE 1 - Terminal - sudo shutdown -h now](https://github.com/user-attachments/assets/62ad8ccf-b931-4708-9302-65505900317a)

<h2>VM Network Configuration - Adding a second NIC</h2>

> [!NOTE]
> The same internal network adapter has been assigned to _networked-node-2_ as well. Please reference the 'Virtual-VM-Configuration' folder for screenshots of the network adapter being attached.

<H3>VM Network Configuration - Node 1 (networked-node-1)</H3>

![VM- NODE 1 - Second Network Adapter ](https://github.com/user-attachments/assets/acb170cf-0b4b-4873-9b28-63281cefec14)

<H3>VM Network Configuration - Node 2 (networked-node-2)</H3>

![VM- NODE 2 - Second Network Adapter ](https://github.com/user-attachments/assets/b7eebcdf-504d-4843-b1e3-19169538ac32)

<h2>Linux Networking</h2>
<p>I used <i>ifconfig -a</i> to confirm that the new network adapter was successfully recongnized within both linux servers. Taking note of their the NIC name's: <i>enp0s8</i>, I began editing and appending a new network configuration in the local Netplan for both linux machines (/etc/Netplan/50-cloud-init.yaml).</p>


```
#General Network Configuration Using Netplan

#This is the IP Address where all of the traffic originates and routes from for both servers
Gateway Address for both Linux Servers: 192.168.1.1

networked-node-1:

appended the following settings to /etc/Netplan/50-cloud-init.yaml

enp0s8:
  dhcp4: false
  addresses:
    - 192.168.1.10/24
  routes:
    # This allows for Node 1 to communicate with Node 2 via the 'default gateway: 192.168.1.1' 
    - to: 192.168.1.20/24
      via: 192.168.1.1
  nameservers:
    addresses:
      - 8.8.8.8
      - 8.8.4.4

networked-node-2:

appended the following settings to /etc/Netplan/

enp0s8:
  dhcp4: false
  addresses:
    - 192.168.1.20/24
  routes:
    # This allows for Node 2 to communicate with Node 1 via the 'default gateway: 192.168.1.1' 
    - to: 192.168.1.10/24
      via: 192.168.1.1
  nameservers:
    addresses:
      - 8.8.8.8
      - 8.8.4.4


```

<h3>Terminal Commands - ifconfig -a (After second NIC was attached two both VMs)</h3>

<p>
#After internal network adapter was assigned in VirtualBox</p></br>
NODE 1: ifconfig -a </p>

![NODE 1 - ifconfig -a after second NIC](https://github.com/user-attachments/assets/7d1a5bd9-3c6e-4ffd-9bf0-418a0549fa05)


<p>NODE 2: ifconfig -a </p>

![NODE 2 - ifconfig -a after second NIC](https://github.com/user-attachments/assets/6f1cbfb8-9894-4458-b326-d415c8982d29)


<h3>Linux Networking - Editing /etc/Netplan/</h3>

```
#Commands executed in order for both Servers

cd /etc/Netplan/
#Change the target directory to the directory where the network config for both machines live

ls -a
#Listing the contents of the current working directory and all hidden files

sudo nano 50-cloud-init.yaml
#Appending the Configuration of enp0s8 and setting a static IPv4 Address, Gateway Address, Subnet and route
```

<h2>Appending enp0s8 configuration to Node 1's Netplan</h2>

![NODE 1 - sudo nano - Editing 50-cloud-init yaml](https://github.com/user-attachments/assets/63c59b49-ce9c-4b88-8107-ef20f37da2b8)

<p>50-cloud-init.yaml</p>

![NODE 1 - Appending enp0s8 to 50-cloud-init yaml](https://github.com/user-attachments/assets/8d2ca75c-4671-4006-9b2a-9a6674f2d264)

<p>ifconfig -a | after sudo netplan apply</p>

```
The Static Assignment, Subnet, Gateway Address has been applied.

IPv4 Address: 192.168.1.10/24

Gateway: 192.168.1.1
```

![NODE 1 - ifconfig -a after sudo netplan apply](https://github.com/user-attachments/assets/89894683-e957-4fec-b90f-ca17561e3278)

<h2>Appending enp0s8 configuration to Node 2's Netplan</h2>

<p>50-cloud-init.yaml</p>

![NODE 2 - Appending enp0s8 to 50-cloud-init yaml](https://github.com/user-attachments/assets/d19e2f43-e173-4c11-9e71-c87f93198d7b)

<p>ifconfig -a | after sudo netplan apply</p>

```
The Static Assignment, Subnet, Gateway Address has been applied.

IPv4 Address: 192.168.1.20/24

Gateway: 192.168.1.1
```

![NODE 2 - ifconfig -a after sudo netplan apply](https://github.com/user-attachments/assets/4355bdfd-1658-45e8-ac74-0696caa63b72)

<h2>Network Connectivity Test</h2>

<p>Node 1 and Node 2 can successfully ping each other, they are now networked together. I established an ssh connection via Node 2 to Node 1 as a test and was able to establish a new session without a hitch.</p>

<h3>ping 192.168.1.10 from Node 2</h3>

![NODE 2 - ping 192 168 1 10 (NODE 1)](https://github.com/user-attachments/assets/b59eeca0-bd50-417e-b499-b7417ac3eaaf)

<h3>ping 192.168.1.20 from Node 1</h3>

![NODE 1 - ping 192 168 1 20 (NODE 2)](https://github.com/user-attachments/assets/b7c5a37c-22cc-4c93-bb5d-34dddce09b66)


<h3>SSH Connection from Node 2 to Node 1</h3>

<p>ssh node1admin@192.168.1.10</p>

![NODE 2 - ssh session to node 1 - connection test](https://github.com/user-attachments/assets/88fe59a9-ed52-4173-b5d0-aa30e161a87d)

<p>Running 'whoami' and 'sudo systemctl status ssh' within the SSH Session</p>

![NODE 2 - ssh session to node 1 - sudo systemctl status ssh - connection verified ](https://github.com/user-attachments/assets/5e64c233-e689-4d65-ada1-77aaf5b254dc)


