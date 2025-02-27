# Setting up a Web Server with DNS HTTP and HTTPS in CPT

Sure! Here are the commands for each device, along with the IP address table.

### Commands for Router 2911 (R1)
```
interface GigabitEthernet0/0
ip address 192.168.1.1 255.255.255.0
no shutdown

interface Serial0/0/0
ip address 10.0.0.1 255.255.255.252
clock rate 64000
no shutdown
```

### Commands for Router 2911 (R2)
```
interface GigabitEthernet0/0
ip address 192.168.2.1 255.255.255.0
no shutdown

interface Serial0/0/0
ip address 10.0.0.2 255.255.255.252
no shutdown
```

### Commands for Web Server
```
ip address 192.168.1.2 255.255.255.0
ip default-gateway 192.168.1.1
ip dns server
ip host www.example.com 192.168.1.2
ip http server
ip http secure-server
```

### Commands for PC
```
ip address 192.168.2.2 255.255.255.0
ip default-gateway 192.168.2.1
ip dns server 192.168.1.2
```

### IP Address Table
| Device      | Interface          | IP Address      |
|-------------|---------------------|-----------------|
| R1          | GigabitEthernet0/0  | 192.168.1.1     |
| R1          | Serial0/0/0         | 10.0.0.1        |
| R2          | GigabitEthernet0/0  | 192.168.2.1     |
| R2          | Serial0/0/0         | 10.0.0.2        |
| Web Server  | Ethernet            | 192.168.1.2     |
| PC          | Ethernet            | 192.168.2.2     |

These commands should help you set up the devices in Cisco Packet Tracer. If you need further assistance, feel free to ask!
