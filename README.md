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

<h4>
  Project Goal:
</h4>

1. Set up a networked Raspberry Pi Cluster that load balances and distributes computing resources
2. Set up basic network security measures like setting up a firewall, enabling port forwarding, and implementing MAC address filtering

<h3>
  Version 2:
</h3>

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

<table>
  <tr>
    <th>Devices</th>
    <th>IP Address</th>
  </tr>
  <tr>
    <td>Raspberry Pi 1 (masterbibble)</td>
  </tr>
  <tr>
    <td>Rasberry Pi 2 (workerken)</td>
    <td>Content in the second column</td>
  </tr>
    <tr>
    <td>Rasberry Pi 3 (workerallen)</td>
    <td>Content in the second column</td>
  </tr>
    <tr>
    <td>NUC 1 (Barbie)</td>
    <td>Content in the second column</td>
  </tr>
    </tr>
    <tr>
    <td>NUC 2 (Preminger)</td>
    <td>Content in the second column</td>
  </tr>
</table>
