Sure, I'll provide the network topology and configuration commands for your Cisco Packet Tracer setup. 

**Network Topology Description:**

- **Router1** <-> **Router2** (Serial connection)
- **Router1** <-> **Switch S1**
- **Router2** <-> **Switch S2**
- **Switch S1** <-> **Web Server**
- **Switch S2** <-> **PC**

**Table of Devices with Interfaces and IP Addresses:**

| Device      | Interface     | IP Address    |
|-------------|---------------|---------------|
| Router1     | Serial 0/0/0  | 192.168.1.1   |
| Router2     | Serial 0/0/1  | 192.168.1.2   |
| Router1     | Gigabit 0/0   | 192.168.10.1  |
| Router2     | Gigabit 0/1   | 192.168.20.1  |
| Switch S1   | N/A           | N/A           |
| Switch S2   | N/A           | N/A           |
| Web Server  | FastEthernet0 | 192.168.10.2  |
| PC          | FastEthernet0 | 192.168.20.2  |

**Configuration Commands:**

1. **Router1 Configuration:**
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

2. **Router2 Configuration:**
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

3. **Web Server Configuration:**
    - Set IP Address:
        ```plaintext
        IP Address: 192.168.10.2
        Subnet Mask: 255.255.255.0
        Default Gateway: 192.168.10.1
        ```
    - Configure DNS Server:
        ```plaintext
        DNS Server: 192.168.10.2
        ```
    - Configure HTTP/HTTPS:
        - Enable HTTP and HTTPS services on the web server through its settings.

4. **PC Configuration:**
    - Set IP Address:
        ```plaintext
        IP Address: 192.168.20.2
        Subnet Mask: 255.255.255.0
        Default Gateway: 192.168.20.1
        DNS Server: 192.168.10.2
        ```

**Setting Up DNS, HTTPS, and HTTP:**

1. **DNS Server:**
    - Open the Web Server services.
    - Go to DNS service and turn it on.
    - Add A records to map the domain name to the IP address, for example:
        ```plaintext
        Name: www.example.com
        Address: 192.168.10.2
        ```

2. **HTTP/HTTPS:**
    - Enable HTTP and HTTPS services in the Web Server settings.
    - Verify that both services are running.

**Accessing the Web Server:**

- Open a web browser on the PC.
- For HTTP, type `http://www.example.com` or `http://192.168.10.2`.
- For HTTPS, type `https://www.example.com
