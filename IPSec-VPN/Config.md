Here's the documentation with comments explaining each line. I'll include everything in a single markdown file for you.

---

# Network Configuration Documentation

## Router Configurations

### Configuration for R1
```bash
hostname R1                             # Sets the hostname of the router to R1
interface g0/1                          # Selects interface g0/1
ip address 192.168.1.1 255.255.255.0    # Assigns IP address to interface g0/1
no shut                                 # Enables the interface g0/1
interface g0/0                          # Selects interface g0/0
ip address 209.165.100.1 255.255.255.0  # Assigns IP address to interface g0/0
no shut                                 # Enables the interface g0/0
exit                                    # Exits interface configuration mode
ip route 0.0.0.0 0.0.0.0 209.165.100.2  # Sets default route to 209.165.100.2
```

### Configuration for ISP
```bash
hostname ISP                            # Sets the hostname of the router to ISP
interface g0/1                          # Selects interface g0/1
ip address 209.165.200.2 255.255.255.0  # Assigns IP address to interface g0/1
no shut                                 # Enables the interface g0/1
interface g0/0                          # Selects interface g0/0
ip address 209.165.100.2 255.255.255.0  # Assigns IP address to interface g0/0
no shut                                 # Enables the interface g0/0
exit                                    # Exits interface configuration mode
```

### Configuration for R3
```bash
hostname R3                             # Sets the hostname of the router to R3
interface g0/1                          # Selects interface g0/1
ip address 192.168.3.1 255.255.255.0    # Assigns IP address to interface g0/1
no shut                                 # Enables the interface g0/1
interface g0/0                          # Selects interface g0/0
ip address 209.165.200.1 255.255.255.0  # Assigns IP address to interface g0/0
no shut                                 # Enables the interface g0/0
exit                                    # Exits interface configuration mode
ip route 0.0.0.0 0.0.0.0 209.165.200.2  # Sets default route to 209.165.200.2
```

## Enabling Security License

```bash
license boot module c1900 technology-package securityk9  # Enables security features on the router
```

## IPsec Configuration

### IPsec on R1
```bash
crypto isakmp policy 10                # Creates ISAKMP policy 10
 encryption aes 256                    # Sets encryption method to AES 256
 authentication pre-share              # Sets authentication method to pre-shared key
 group 5                               # Sets Diffie-Hellman group to 5
!
crypto isakmp key secretkey address 209.165.200.1  # Defines pre-shared key and peer address
!
crypto ipsec transform-set R1-R3 esp-aes 256 esp-sha-hmac  # Creates transform-set for IPsec
!
crypto map IPSEC-MAP 10 ipsec-isakmp  # Creates crypto map IPSEC-MAP
 set peer 209.165.200.1               # Sets peer for IPsec
 set pfs group5                       # Enables Perfect Forward Secrecy with group 5
 set security-association lifetime seconds 86400  # Sets SA lifetime to 86400 seconds
 set transform-set R1-R3              # Applies transform-set R1-R3
 match address 100                    # Matches traffic defined by access-list 100
!
interface GigabitEthernet0/0
 crypto map IPSEC-MAP                 # Applies crypto map IPSEC-MAP to interface g0/0
!
access-list 100 permit ip 192.168.1.0 0.0.0.255 192.168.3.0 0.0.0.255  # Defines traffic to be protected by IPsec
```

### IPsec on R3
```bash
crypto isakmp policy 10                # Creates ISAKMP policy 10
 encryption aes 256                    # Sets encryption method to AES 256
 authentication pre-share              # Sets authentication method to pre-shared key
 group 5                               # Sets Diffie-Hellman group to 5
!
crypto isakmp key secretkey address 209.165.100.1  # Defines pre-shared key and peer address
!
crypto ipsec transform-set R3-R1 esp-aes 256 esp-sha-hmac  # Creates transform-set for IPsec
!
crypto map IPSEC-MAP 10 ipsec-isakmp  # Creates crypto map IPSEC-MAP
 set peer 209.165.100.1               # Sets peer for IPsec
 set pfs group5                       # Enables Perfect Forward Secrecy with group 5
 set security-association lifetime seconds 86400  # Sets SA lifetime to 86400 seconds
 set transform-set R3-R1              # Applies transform-set R3-R1
 match address 100                    # Matches traffic defined by access-list 100
!
interface GigabitEthernet0/0
 crypto map IPSEC-MAP                 # Applies crypto map IPSEC-MAP to interface g0/0
!
access-list 100 permit ip 192.168.3.0 0.0.0.255 192.168.1.0 0.0.0.255  # Defines traffic to be protected by IPsec
```

---

I hope this helps! If you need any more adjustments or further explanations, feel free to ask.
