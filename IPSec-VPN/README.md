## Setting up an IPSec VPN in Cisco Packet Tracer with the given network topology:

### Network Topology
- **ISP Router**: Assumed as the ISP.
- **R1 Router**: Connected to ISP and P1.
- **R2 Router**: Connected to ISP and P2.
- **P1 System**: Connected to R1.
- **P2 System**: Connected to R2.

### Steps to Configure IPSec VPN

#### 1. Configure IP Addresses
- **ISP Router**:
  ```plaintext
  interface GigabitEthernet0/0
  ip address 192.168.1.1 255.255.255.0
  no shutdown
  ```

- **R1 Router**:
  ```plaintext
  interface GigabitEthernet0/0
  ip address 192.168.1.2 255.255.255.0
  no shutdown

  interface GigabitEthernet0/1
  ip address 10.0.0.1 255.255.255.0
  no shutdown
  ```

- **R2 Router**:
  ```plaintext
  interface GigabitEthernet0/0
  ip address 192.168.1.3 255.255.255.0
  no shutdown

  interface GigabitEthernet0/1
  ip address 10.0.1.1 255.255.255.0
  no shutdown
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

#### 2. Configure Routing
- **R1 Router**:
  ```plaintext
  ip route 0.0.0.0 0.0.0.0 192.168.1.1
  ```

- **R2 Router**:
  ```plaintext
  ip route 0.0.0.0 0.0.0.0 192.168.1.1
  ```

#### 3. Configure IPSec VPN
- **R1 Router**:
  ```plaintext
  crypto isakmp policy 10
  encr aes
  authentication pre-share
  group 2

  crypto isakmp key cisco123 address 192.168.1.3

  crypto ipsec transform-set MYSET esp-aes esp-sha-hmac

  crypto map MYMAP 10 ipsec-isakmp
  set peer 192.168.1.3
  set transform-set MYSET
  match address 100

  interface GigabitEthernet0/0
  crypto map MYMAP

  access-list 100 permit ip 10.0.0.0 0.0.0.255 10.0.1.0 0.0.0.255
  ```

- **R2 Router**:
  ```plaintext
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
  ```

#### 4. Verify the Configuration
- **Ping Test**:
  - From P1, ping P2 (10.0.1.2).
  - From P2, ping P1 (10.0.0.2).

If the pings are successful, the IPSec VPN tunnel is working correctly.

This setup ensures that the traffic between P1 and P2 is encrypted and secure as it passes through the IPSec VPN tunnel.
