
### Network Topology:

1. **Router1 (R1):**
    - **Serial 0/0/0 Interface:** Connected to Router2 (R2) via a serial connection.
    - **GigabitEthernet 0/0 Interface:** Connected to Switch S1. This interface is used to provide connectivity to the Web Server.

2. **Router2 (R2):**
    - **Serial 0/0/1 Interface:** Connected to Router1 (R1) via a serial connection.
    - **GigabitEthernet 0/1 Interface:** Connected to Switch S2. This interface is used to provide connectivity to the PC.

3. **Switch S1:**
    - **Interface:** Connected to Router1 (R1) via GigabitEthernet 0/0 interface.
    - **Interface:** Connected to the Web Server via FastEthernet 0 interface.

4. **Switch S2:**
    - **Interface:** Connected to Router2 (R2) via GigabitEthernet 0/1 interface.
    - **Interface:** Connected to the PC via FastEthernet 0 interface.

5. **Web Server:**
    - **FastEthernet 0 Interface:** Connected to Switch S1. The Web Server is assigned an IP address within the 192.168.10.0/24 network.

6. **PC:**
    - **FastEthernet 0 Interface:** Connected to Switch S2. The PC is assigned an IP address within the 192.168.20.0/24 network.


### Table of Devices with Interfaces and IP Addresses:
| Device      | Interface        | IP Address    |
|-------------|------------------|---------------|
| Router1     | Serial 0/0/0     | 192.168.1.1   |
| Router2     | Serial 0/0/1     | 192.168.1.2   |
| Router1     | Gigabit 0/0      | 192.168.10.1  |
| Router2     | Gigabit 0/1      | 192.168.20.1  |
| Switch S1   | N/A              | N/A           |
| Switch S2   | N/A              | N/A           |
| Web Server  | FastEthernet 0   | 192.168.10.2  |
| PC          | FastEthernet 0   | 192.168.20.2  |

### Configuration Commands:
#### 1. **Router1 Configuration:**
```plaintext
enable
configure terminal
interface serial 0/0/0
ip address 192.168.1.1 255.255.255.0
no shutdown
interface gigabitEthernet 0/0
ip address 192.168.10.1 255.255.255.0
no shutdown
exit
ip route 192.168.20.0 255.255.255.0 192.168.1.2
end
```

#### 2. **Router2 Configuration:**
```plaintext
enable
configure terminal
interface serial 0/0/1
ip address 192.168.1.2 255.255.255.0
no shutdown
interface gigabitEthernet 0/1
ip address 192.168.20.1 255.255.255.0
no shutdown
exit
ip route 192.168.10.0 255.255.255.0 192.168.1.1
end
```

#### 3. **Web Server Configuration:**
- **Set IP Address:**
    ```plaintext
    IP Address: 192.168.10.2
    Subnet Mask: 255.255.255.0
    Default Gateway: 192.168.10.1
    ```
- **Configure DNS Server:**
    ```plaintext
    DNS Server: 192.168.10.2
    ```
- **Configure HTTP/HTTPS:**
    - Enable HTTP and HTTPS services on the web server through its settings.

#### 4. **PC Configuration:**
- **Set IP Address:**
    ```plaintext
    IP Address: 192.168.20.2
    Subnet Mask: 255.255.255.0
    Default Gateway: 192.168.20.1
    DNS Server: 192.168.10.2
    ```

### Setting Up DNS, HTTPS, and HTTP:

#### **DNS Server:**
1. Open the Web Server services.
2. Go to DNS service and turn it on.
3. Add A records to map the domain name to the IP address, for example:
    ```plaintext
    Name: www.example.com
    Address: 192.168.10.2
    ```

#### **HTTP/HTTPS:**
1. Enable HTTP and HTTPS services in the Web Server settings.
2. Verify that both services are running.

### Accessing the Web Server:

1. **Open a web browser on the PC.**
2. For **HTTP**, type `http://www.example.com` or `http://192.168.10.2`.
3. For **HTTPS**, type `https://www.example.com` or `https://192.168.10.2`.

That covers the complete documentation for setting up the network, web server, DNS, HTTP, and HTTPS in Cisco Packet Tracer.
