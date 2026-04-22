# Dynamic Host Blocking System (DHBS) using Software Defined Networking

## Overview

This project implements a Dynamic Host Blocking System (DHBS) using Software Defined Networking (SDN) principles. The system utilizes the Ryu controller in conjunction with Mininet to dynamically control network traffic by blocking or allowing hosts based on predefined policies.

The controller inspects incoming packets and installs appropriate OpenFlow rules in the switch to enforce access control in real time.

---

## Objectives

- Demonstrate controller–switch interaction in an SDN environment  
- Implement OpenFlow-based match–action flow rules  
- Dynamically control network traffic at the host level  
- Observe and analyze network behavior  

---

## Network Topology

- 1 Switch (s1)  
- 3 Hosts (h1, h2, h3)  
- 1 Controller (Ryu)  
h1 ----

h2 ----- s1 ----- Controller (Ryu)
/
h3 ----/


---

## Technologies Used

- Python  
- Ryu Controller (OpenFlow 1.3)  
- Mininet  
- Open vSwitch  

---

## Project Structure


.
├── firewall.py
└── README.md


---

## Methodology

1. The switch forwards packets without matching rules to the controller (Packet-In event).  
2. The controller inspects packet headers, specifically the source IP address.  
3. If the source IP matches a blocked host, the controller installs a drop rule.  
4. Otherwise, the controller allows normal packet forwarding.  
5. Once flow rules are installed, the switch handles subsequent packets without involving the controller.  

---

## Setup and Execution

### Step 1: Start Ryu Controller

```bash
ryu-manager firewall.py
Step 2: Start Mininet
sudo mn --topo single,3 --controller=remote,ip=127.0.0.1 --switch ovsk,protocols=OpenFlow13
Step 3: Verify Connectivity
pingall
Step 4: Test Host Blocking
h1 ping h2

Expected result: Communication fails (packet loss).

Step 5: Verify Allowed Traffic
h2 ping h3

Expected result: Successful communication.

Step 6: Inspect Flow Table
sudo ovs-ofctl dump-flows s1

Example output:

ipv4_src=10.0.0.1 actions=DROP
Test Scenarios
Scenario 1: Normal Operation

All hosts are able to communicate without restrictions.

Scenario 2: Host Blocking

A specific host (h1) is blocked, preventing it from communicating with other hosts.

Observations
Flow rules are dynamically installed by the controller
Switch handles traffic locally after rule installation
Reduces controller overhead and improves efficiency
Results

The system successfully demonstrates:

Dynamic traffic control using SDN
Real-time enforcement of access policies
Efficient OpenFlow rule management
Conclusion

This project demonstrates how SDN enables centralized control and dynamic network management. The Dynamic Host Blocking System highlights the effectiveness of programmable controllers in enforcing security policies.

Future Enhancements
Dynamic block/unblock via user input
Port-based filtering (TCP/UDP)
Traffic monitoring and logging
Web-based control interface
