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
    <td>TBD</td>
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
1. cgroup_memory=1 cgroup_enable=memory ip=10.0.0.10::10.0.0.1:255.255.255.0:rpimaster:eth0:off

We went into the config.txt file and added the following line at the very bottom:
1. arm_64bit=1

In Powershell, we created an ssh folder in the drive so we could access it using our computer by using the following command:
1. new-item ssh

To be able to connect to the Pi, we turned off the WiFi and configured the Ethernet port to an IP address (10.0.0.2) within the network we configured. 
