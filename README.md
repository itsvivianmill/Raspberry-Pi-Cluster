# 
<h1 align="center">
  Raspberry-Pi-Cluster
</h1>

<p align="center"> 
  by Quan Do, Vivian Miller, and Lilian Miller [QVL] 
</p>

<h2>
  Preface: 
</h2>

*This is documentation that records our process of trying to configure and troubleshoot a network security simulation. This is NOT a step-to-step tutorial but rather documents our thought process, changes, problems, and failures that we have encountered when working on this project. 
The overall arching goal of this project is to understand how to build and configure a network— but also with a few extra challenges sprinkled into it (i.e. network security). The other thing to note is that this project encompasses configuring Cisco routers, switches, and firewalls. pfSense was an option for this project; however, we all wanted to get acquainted with Cisco since it is a leader in the networking industry and many competitions we’ve participated in have always had Cisco networking challenges.*

<h3>
  Primary Materials:
</h3>

- Raspberry Pi 400 Series (3)
- NUCs (2)
- Cisco Switches (2)
- Cisco Router
- ASA 5506 Series Cisco Firewall

<h3>
  References:
</h3>

*During the planning process of how we wanted to go about this, we went through different versions. We set up different diagrams and project plans to consider. The important part was that we had a goal and a way to measure our success. The materials remained the same, it just depended on how we wanted to utilize it.*

<h2>
  Project Plans: 
</h2>

<h3>
  Version 1:
</h3>

![Screenshot 2024-02-01 100806](https://github.com/itsvivianmill/Raspberry-Pi-Cluster/assets/116047994/b4538444-e00c-4c0b-8405-4e0a736e9f54)

<h4>
  Project Goal:
</h4>

1. Set up a networked Raspberry Pi Cluster that load balances and distributes computing resources
2. Set up basic network security measures like setting up a firewall, enabling port forwarding, and implementing MAC address filtering

<h3>
  Version 2:
</h3>

![Screenshot 2024-02-01 101102](https://github.com/itsvivianmill/Raspberry-Pi-Cluster/assets/116047994/baa9ac39-82ee-4f43-8c90-15975163366c)

<h4>
  Project Goal:
</h4>

1. Simulate a botnet attack by utilizing a Raspberry Pi Cluster to target a client or server
2. Set up basic network security measures like setting up a firewall, enabling port forwarding, and implementing MAC address filtering
3. Demonstrate the importance of security measures when the network is unprotected vs. protected
4. Present the ideas using Wireshark to capture the flow of data 

<h3>
  Version 3:
</h3>

![Screenshot 2024-02-01 101122](https://github.com/itsvivianmill/Raspberry-Pi-Cluster/assets/116047994/3326b414-a99b-4091-a7f6-af914826447e)

<h4>
  Project Goal:
</h4>

1. Simulate a botnet attack by utilizing a (Next Unit of Computing) NUC to target a client or server
2. Set up basic network security measures like setting up a firewall, enabling port forwarding, and implementing MAC address filtering
3. Demonstrate the importance of security measures when the network is unprotected vs. protected
4. Present the ideas using Wireshark to capture the flow of data 

<h3>
  IP Address Table
</h3>

<table align="center">
  <tr>
    <th>Devices</th>
    <th>IP Address</th>
  </tr>
  <tr>
    <td>Default Gateway</td>
    <td>172.30.212.1</td>
  </tr>
  <tr>
    <td>Raspberry Pi 1 (masterbibble) </td>
    <td>172.30.212.201</td>
  </tr>
  <tr>
    <td>Rasberry Pi 2 (workerken)</td>
    <td>172.30.212.202</td>
  </tr>
  <tr>
    <td>Rasberry Pi 3 (workerallen)</td>
    <td>172.30.212.203</td>
  </tr>
  <tr>
    <td>NUC 1 (Barbie)</td>
    <td>172.30.212.200</td>
  </tr>
  </tr>
    <tr>
    <td>NUC 2 (Preminger)</td>
    <td>11.0.0.1</td>
  </tr>
</table>

<h2>1.0 Raspberry Pi 400 Cluster</h2>

<h4>References</h4>
https://www.raspberrypi.com/tutorials/cluster-raspberry-pi-tutorial/ 

<h4>Materials</h4>

- 1 x Switch
- 3 x Raspberry Pi 400 Series

Building Cluster
1. https://www.raspberrypi.com/tutorials/cluster-raspberry-pi-tutorial/ 
2. https://projects.raspberrypi.org/en/projects/build-an-octapi 
3. https://forums.raspberrypi.com/viewtopic.php?t=328584 
4. https://deninet.com/creation/2022/01/30/components-and-rack-pi-cluster 
5. https://www.youtube.com/watch?v=H2rTecSO0gk 

*Connect the Raspberry Pi 400 keyboards with the ethernet cables to the switch. Power each respectively.  We used the references as ideas and not as a definite recipe to follow. A lot of the links were used as a starting point to get to what we have now.*

<h3>1.1: Editing SSH Files</h3>
We are going to be following the https://www.youtube.com/watch?v=X9fSMGkjtug video and start by setting up the Raspberry Pi by putting Raspberry Pi Lite 32-bit OS onto it (later changed to 64-bit).

The device we are using to access and configure the micro ssd in the pi is linked [here](https://www.amazon.com/gp/aw/d/B081VHSB2V/?_encoding=UTF8&pd_rd_plhdr=t&aaxitk=5b290797ca3079a2b9552be521291b69&hsa_cr_id=6748414440801&qid=1704734333&sr=1-1-9e67e56a-6f64-441f-a281-df67fc737124&ref_=sbx_be_s_sparkle_lsi4d_asin_0_img&pd_rd_w=bhBKh&content-id=amzn1.sym.417820b0-80f2-4084-adb3-fb612550f30b%3Aamzn1.sym.417820b0-80f2-4084-adb3-fb612550f30b&pf_rd_p=417820b0-80f2-4084-adb3-fb612550f30b&pf_rd_r=HKWJGDDWGM9EKW4TGN9E&pd_rd_wg=ARnKN&pd_rd_r=2e33343b-fc30-44ce-aeff-e10bac0a08e0&th=1). The OS booted and we named it “masterbibble” (this entire project is Barbie-themed) and set a password. 

Then we went into the cmdline.txt file and added the following line:
<br>
**1. cgroup_memory=1 cgroup_enable=memory ip=10.0.0.10::10.0.0.1:255.255.255.0:rpimaster:eth0:off**

We went into the config.txt file and added the following line at the very bottom:
<br>
**1. arm_64bit=1**

In Powershell, we created an ssh folder in the drive so we could access it using our computer by using the following command:
<br>
**1. new-item ssh**

To be able to connect to the Pi, we turned off the WiFi and configured the Ethernet port to an IP address (10.0.0.2) within the network we configured. 

<h3>1.2: Raspberry Pi Imaging</h3>
We put the micro SSD back into the Raspberry Pi and connected it to a laptop with an Ethernet cable. Using the command prompt, we SSHed into the Pi using the IP address we configured and it worked! However, we did hit an obstacle that occurred— we were unable to use the command iptables, so it will be investigated soon (solution is in iptables section). 
<br>
<br>
<p align="center">The image below shows masterbibble successfully ssh:</p>


We used the Raspberry Pi Imager v1.8.4 to create an image for the two worker Pis. The username of each Pi is workerken whose IP address is 10.0.0.11 and workerallen whose IP address is 10.0.0.12. 

<br>
<p align="center">Image below shows the Raspberry Pi Imager v1.8.4 and the customization settings for workerken and workerallen:</p>

<p align="center">The images below shows workerken and workerallen successfully ssh:</p>

<h3>1.3: iptables issue</h3>
We found out that iptables was not installed, the problem was we weren’t connected to WiFi to be able to download any of the packages because everything had been localized. To solve this problem, we reinstalled the OS for masterbibble to enable wireless LAN connectivity. Another thing we changed was that masterbibble was now a 64-bit OS. 

However, we doubted that having Ethernet and wireless connectivity active at the same time would not allow for a dual connection. There were several ideas like hooking up the ASA firewall to route our internal network while connected to the school network, setting up NAT, using ad hoc, or simply connecting the Pis to the school’s guest WiFi to get over the connectivity issue. Regardless, these solutions were limited by the little authority we had over the school network and we didn’t want our cluster to be public to the guest network. 

So the thing we decided to do was to use leftover IP addresses that were assigned to the classroom that we were occupying during this project. After we have internet access, the Pis will be updated and have installed programs that will be used later on. When that is done, it will be returned to a local network. 

To find the network address of the classroom, we connected an ethernet cable to a laptop, disabled the wireless connectivity, and enabled DHCP.  We went into the command prompt and used ipconfig to look at the IP address that was assigned. The private network address was 172.30.212.0/24 and so we went back into the network adapter setting to statically change it to 172.30.212.200/24. We chose an IP host address that would least likely be occupied by any device on the network. 

<br>
<p align="center">The image below shows the IP address of the laptop after it was configured to be 172.30.212.200/24:</p>

<h3>1.4: Setting up iptables</h3>
With that knowledge, we went back into all three Pis and statically changed their IP addresses to fit with the network of the classroom. We changed masterbibble to 172.30.212.201/24, workerken to 172.30.212.202/24, and workerallen to 172.30.212.203/24

Finally, we tested whether or not the Pi would have internet access. We used a laptop connected to the same network and ssh into ‘masterbibble’ and pinged 1.1.1.1. This successfully pinged!

We continued on and installed iptables, updated, and upgraded it using the following commands for the rest of the Pis:
<br>
**1. sudo apt install iptables**
<br>
**2. sudo apt update**
<br>
**3. sudo apt upgrade**

<br>
<p align="center">Image below shows masterbibble installing iptables:</p>

<p align="center">Image below shows workerken being ssh and successfully ping 1.1.1.1:</p>

The next step was to use the following commands and configure the Raspberry Pi. We used the following commands:
<br>
**1. sudo iptables -F**
<br>
**2. sudo update-alternatives --set iptables /usr/sbin/iptables-legacy**
<br>
**3. sudo update-alternatives --set ip6tables /usr/sbin/ip6tables-legacy**
<br>
**4. sudo reboot**


In the video, we weren’t sure what commands he used because they were copied and pasted, but luckily we found the set of commands to use in the comment section!

<h3>1.5: Setting up kubernetes</h3>

To configure the master to use kubernetes, we used the following commands:
<br>
**1. curl -sfL https://gt.k3s.io | K3S_KUBECONFIG_MOD=”644” sh -s**
<br>
**2. kubectl get nodes**
<br>
**3. cat /var/lib/rancher/k3s/server/node-token**

<br>
<p align="center">Image below shows masterbibble downloading kubernetes:</p>

<p align="center">Image below shows masterbibble gettings the nodes:</p>

<p align="center">Image below shows masterbibble obtaining the node token:</p>

We went back into the worker Pis and started to install and configure kubernetes onto them (ssh using the NUC ). We input our unique token into the “YOURTOKEN” spot, changed the [your server] with the master IP address, and changed “servername” with workerallen. We used the following command:
<br>
**1. curl -sfL https://get.k3s.io | K3S_TOKEN="YOURTOKEN" K3S_URL="https://[your server]:6443" K3S_NODE_NAME="servername" sh -**

<p align="center">⬇️</p>

**2. curl -sfL https://get.k3s.io | K3S_TOKEN="K10578b5c034ffd32e45f0bd5eff4db97249a4abd718c6df93a50beb39be2b162f6::server:ada46499a2b62c634fd4ee538144e3ff" K3S_URL="https://172.30.212.201:6443" K3S_NODE_NAME="workallen" sh -**

However, there was an error that appeared when inputting the command into the CLI, we got an [ERROR] Only https:// URLs are supported for K3S_URL (have “https://172.30.212.201:6443”). 
*Update: It was a semantic error lol. It was the wrong symbol that was copied and pasted. Using a unicode encoder, we can see that although the quotations look similar, they are completely different. The unicodes were not the same :(*

<br>
<p align="center">Image below shows the ERROR:</p>

<p align="center">Image below shows the command working correctly:</p>

We proceeded and waited for the agent to load. Going back to the master, we checked if everything worked correctly by using the following command:
<br>
**1. kubectl get nodes**

<br>
<p align="center">Image below shows the successful connection of workerallen:</p>

The same thing was done on workerken and it was successful. 

<br>
<p align="center">Image below shows the successful connection of workerken:</p>

<h3>1.6: Physical Installment</h3>
Finally, it was time to connect everything together and have access to all of the Raspberry Pis at the same time. 
We connected the Raspberry Pi 400 keyboards with the ethernet cables into the switch, powering each respectively. The Pis were placed on top of each other, going from masterbibble down to workerallen while trying to maintain somewhat of a good cable management (spoiler alert we don't) by zip tying the ethernet cables. It looks a bit messy, but we worked with the space and materials we had. 

