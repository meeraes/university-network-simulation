# University Network Simulation

This project simulates a university network setup using Cisco Packet Tracer. It enables file exchanges between departments via FTP services and hosts academic resources through HTTP. The network includes two routers, four switches, eight PCs, and three servers (FTP, HTTP, DNS).

## Problem Scenario

The university needs a network design that allows seamless access to academic resources and file sharing between departments. The goal is to provide network access for students, faculty, and administrative staff across various departments using FTP and HTTP services, with hostname resolution via DNS.

## Components

- **2 Routers** (Router-PT)
- **4 Switches** (Switch-PT)
- **8 PCs** (PC-PT)
- **1 FTP Server** (192.168.1.20)
- **1 HTTP Server** (192.168.3.20)
- **1 DNS Server** (192.168.2.20)

## Network Overview

### Routers

- **Router 1 (Admin + Library)**:
  - FastEthernet0/0: `192.168.1.1` -> Switch 1 (Admin)
  - FastEthernet1/0: `192.168.2.1` -> Switch 2 (Library)
  - Serial2/0: `192.168.5.1` -> Router 2

- **Router 2 (IT + Research)**:
  - FastEthernet0/0: `192.168.3.1` -> Switch 3 (IT)
  - FastEthernet1/0: `192.168.4.1` -> Switch 4 (Research)
  - Serial2/0: `192.168.5.2` -> Router 1

### Switches

- **Switch 1 (Admin, 192.168.1.0/24)**: Connected to PC1, PC2, FTP Server.
- **Switch 2 (Library, 192.168.2.0/24)**: Connected to PC3, PC4, DNS Server.
- **Switch 3 (IT, 192.168.3.0/24)**: Connected to PC5, PC6, HTTP Server.
- **Switch 4 (Research, 192.168.4.0/24)**: Connected to PC7, PC8.

### PCs

- **PC1 (192.168.1.10)** and **PC2 (192.168.1.11)**: Admin Department.
- **PC3 (192.168.2.10)** and **PC4 (192.168.2.11)**: Library Department.
- **PC5 (192.168.3.10)** and **PC6 (192.168.3.11)**: IT Department.
- **PC7 (192.168.4.10)** and **PC8 (192.168.4.11)**: Research Department.

### Servers

- **FTP Server (192.168.1.20)**:
  - Location: Admin Department.
  - Purpose: File sharing between departments.

- **HTTP Server (192.168.3.20)**:
  - Location: IT Department.
  - Purpose: Hosts academic resources accessible across the network.

- **DNS Server (192.168.2.20)**:
  - Location: Library Department.
  - Purpose: Provides hostname resolution for `ftp.university.edu` and `web.university.edu`.

## Steps to Simulate the Network

### Step 1: Open the .pkt file
- Open **Cisco Packet Tracer**.
- Load the provided `.pkt` file for this project.

### Step 2: Configure IP Addresses for Devices
- PCs in each department should have their respective IP addresses:
  - Admin: PC1 (`192.168.1.10`), PC2 (`192.168.1.11`).
  - Library: PC3 (`192.168.2.10`), PC4 (`192.168.2.11`).
  - IT: PC5 (`192.168.3.10`), PC6 (`192.168.3.11`).
  - Research: PC7 (`192.168.4.10`), PC8 (`192.168.4.11`).

- Configure the **Default Gateway** for each PC:
  - Admin(PC1 and PC2): `192.168.1.1`
  - Library(PC3 and PC4): `192.168.2.1`
  - IT(PC5 and PC6): `192.168.3.1`
  - Research(PC7 and PC8): `192.168.4.1`

### Step 3: Set Up DNS Server
- On the **DNS Server** (`192.168.2.20`), configure hostnames:
  - `ftp.university.edu` -> `192.168.1.20` (FTP Server)
  - `web.university.edu` -> `192.168.3.20` (HTTP Server)

### Step 4: Test FTP and HTTP Connections
- Use **ping** from any PC to check connections:
  - **PC1** (`192.168.1.10`): `ping web.university.edu`
  - **PC5** (`192.168.3.10`): `ping ftp.university.edu`
  
- Verify FTP access:
  - On **PC5**, use the FTP command to log in and exchange files with the FTP Server.

### Step 5: Check Hostname Resolution
- Ensure all PCs are configured to use the **DNS Server** (`192.168.2.20`).
- On any PC, try accessing services using hostnames:
  - For FTP: `ftp ftp.university.edu`
  - For HTTP: Open a web browser and navigate to `web.university.edu`.

### Basic Troubleshooting
- Ensure **PC IP addresses** and **Default Gateways** are correctly configured for cross-department communication.
- The **DNS Server** should resolve both FTP and HTTP hostnames. Every PC must be configured with the IP address of the DNS Server.
- Use **ping** and **tracert** to troubleshoot any connection issues between routers, switches, or PCs.
- It helps to ping from one node to the next if you are trying to do a cross-router communication. This will help to find where exactly the message is getting lost.
- If the message does not pass at once, ping from node to node, and if all of those are successful, try pinging from one end to the other. It may take some time, but it will work.
- If hostname resolution doesn’t work, verify DNS configuration on all PCs.

### Where is everything?

1. PCs:
- For configuring the PCs, click on the PC -> Desktop tab -> IP Configuration -> IPv4 Address, Subnet Mask, Default Gateway and DNS Server fields can be edited
- Desktop tab -> IP Configuration -> Command Prompt -> Can ping any IP address or server from here
- Desktop tab -> IP Configuration -> Web Browser -> HTTP server can be pinged from here as web.university.edu or its IP address
- Default Gateway must be same as the Router IP address it is connected to through the Switch.

2. Switch:
- Nothing needs to be changed in the configuration settings here

3. Router:
- Router -> Config tab -> Select the interface to be configured -> Add IP address and subnet mask
- IP addresses and subnet masks connected to the Switches must be similar to the PCs' IP addresses. So if the IPA of PC1 is 192.168.1.10 and that of PC2 is 192.168.1.11, then the router IP address connected to that Switch must be 192.168.1.1.
- IP address and subnet mask of the interface connecting to the router can be different. So 192.168.5.1 and 255.255.255.252 can be the IPA and SNM of Router 1.
- Make sure the Port statuses of all these interfaces are on.
- To ensure cross-router communication, go to Router1 -> Config tab -> Routing -> Static.
- Network: The Default gateway of Switch3
  Mask: The subnet mask
  Next hop: Router2 IP address
- When the fields have been filled in correctly and added, it should look like this: (192.168.3.0/24 via 192.168.5.2)
- This must be done for the other switch as well
- Vice versa must be done for Router2.

4. Servers:

a) FTP Server:
- Server -> Config tab -> Default gateway
- Server -> Services tab -> Services -> FTP -> Make sure it is on, user can be added with permissions
- Default gateway must be the same as the default gateway of the PCs that are connected to the Switch that the server is connected to.

b) HTTP Server:
- Server -> Config tab -> Default gateway
- Server -> Services tab -> Services -> HTTP -> Make sure it is on, default web page will be set up(can be modified)
- Default gateway must be the same as the default gateway of the PCs that are connected to the Switch that the server is connected to.

c) DNS Server:
- Server -> Config tab -> Default gateway
- Server -> Services tab -> Services -> DNS -> Make sure it is on
- In the DNS Service, Name is the IP address of the server you want to add, Address will be the domain name. Domain names can be set up and added.
- Default gateway must be the same as the default gateway of the PCs that are connected to the Switch that the server is connected to.

