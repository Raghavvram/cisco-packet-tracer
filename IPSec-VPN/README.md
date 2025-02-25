Sure, let's create a complete documentation with all the required commands for the ISP router, R1, R2, and the two PCs, along with a table summarizing the interfaces and IP addresses.

### Network Topology and Configuration

**Router Used: Cisco 2911**

#### Network Topology
- **ISP Router**:
  - **Interface GigabitEthernet0/0** connected to **R1 GigabitEthernet0/0**
  - **Interface GigabitEthernet0/1** connected to **R2 GigabitEthernet0/0**
- **R1 Router**:
  - **Interface GigabitEthernet0/0** connected to **ISP GigabitEthernet0/0**
  - **Interface GigabitEthernet0/1** connected to **P1**
- **R2 Router**:
  - **Interface GigabitEthernet0/0** connected to **ISP GigabitEthernet0/1**
  - **Interface GigabitEthernet0/1** connected to **P2**

#### IP Address Assignments
| Device | Interface | IP Address         | Subnet Mask       |
|--------|------------|--------------------|-------------------|
| ISP    | G0/0       | 192.168.1.1        | 255.255.255.0     |
| ISP    | G0/1       | 192.168.2.1        | 255.255.255.0     |
| R1     | G0/0       | 192.168.1.2        | 255.255.255.0     |
| R1     | G0/1       | 10.0.0.1           | 255.255.255.0     |
| R2     | G0/0       | 192.168.2.2        | 255.255.255.0     |
| R2     | G0/1       | 10.0.1.1           | 255.255.255.0     |
| P1     | NIC        | 10.0.0.2           | 255.255.255.0     |
| P1     | Gateway    | 10.0.0.1           |                   |
| P2     | NIC        | 10.0.1.2           | 255.255.255.0     |
| P2     | Gateway    | 10.0.1.1           |                   |

### Steps to Configure IPSec VPN

#### 1. Configure IP Addresses

- **ISP Router**:
  ```plaintext
  enable
  configure terminal
  interface GigabitEthernet0/0
  ip address 192.168.1.1 255.255.255.0
  no shutdown
  exit
  interface GigabitEthernet0/1
  ip address 192.168.2.1 255.255.255.0
  no shutdown
  end
  write memory
  ```

- **R1 Router**:
  ```plaintext
  enable
  configure terminal
  interface GigabitEthernet0/0
  ip address 192.168.1.2 255.255.255.0
  no shutdown
  exit
  interface GigabitEthernet0/1
  ip address 10.0.0.1 255.255.255.0
  no shutdown
  exit
  ip route 0.0.0.0 0.0.0.0 192.168.1.1
  end
  write memory
  ```

- **R2 Router**:
  ```plaintext
  enable
  configure terminal
  interface GigabitEthernet0/0
  ip address 192.168.2.2 255.255.255.0
  no shutdown
  exit
  interface GigabitEthernet0/1
  ip address 10.0.1.1 255.255.255.0
  no shutdown
  exit
  ip route 0.0.0.0 0.0.0.0 192.168.2.1
  end
  write memory
  ```

- **P1 System**:
  ```plaintext
  IP Address: 10.0.0.2
  Subnet Mask: 255.255.255.0
  Default Gateway: 10.0.0.1
  ```

- **P2 System**:
  ```plaintext
  IP Address: 10.0.1.2
  Subnet Mask: 255.255.255.0
  Default Gateway: 10.0.1.1
  ```

#### 2. Configure IPSec VPN

- **R1 Router**:
  ```plaintext
  enable
  configure terminal
  crypto isakmp policy 10
  encr aes
  authentication pre-share
  group 2

  crypto isakmp key cisco123 address 192.168.2.2

  crypto ipsec transform-set MYSET esp-aes esp-sha-hmac

  crypto map MYMAP 10 ipsec-isakmp
  set peer 192.168.2.2
  set transform-set MYSET
  match address 100

  interface GigabitEthernet0/0
  crypto map MYMAP

  access-list 100 permit ip 10.0.0.0 0.0.0.255 10.0.1.0 0.0.0.255
  end
  write memory
  ```

- **R2 Router**:
  ```plaintext
  enable
  configure terminal
  crypto isakmp policy 10
  encr aes
  authentication pre-share
  group 2

  crypto isakmp key cisco123 address 192.168.1.2

  crypto ipsec transform-set MYSET esp-aes esp-sha-hmac

  crypto map MYMAP 10 ipsec-isakmp
  set peer 192.168.1.2
  set transform-set MYSET
  match address 100

  interface GigabitEthernet0/0
  crypto map MYMAP

  access-list 100 permit ip 10.0.1.0 0.0.0.255 10.0.0.0 0.0.0.255
  end
  write memory
  ```

### 3. Verify the Configuration
- **Ping Test**:
  - From P1, ping P2 (10.0.1.2).
  - From P2, ping P1 (10.0.0.2).

If the pings are successful, the IPSec VPN tunnel is working correctly. This setup ensures that the traffic between P1 and P2 is encrypted and secure as it passes through the IPSec VPN tunnel.

Let me know if you need further assistance or have any other questions!
