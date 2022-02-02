# Site_to_site_VPN_on_Packet_Tracer

1. ACL

2. ISAKMP policy (PHASE1) ISAKMP key

3. IPsec transform-set (PHASE2)

4. Crypto map (tie it together)

5. int g0/0
    - apply the crypto map

#############################################

![Screenshot 2022-02-02 0235462](https://user-images.githubusercontent.com/55639146/152230333-06d01c57-bdb1-4e0c-9c26-eae6a8a31ff5.png)

#############################################


license boot module c1900 technology-package securityk9
####
crypto isakmp policy 10
 encryption aes 256
 authentication pre-share
 group 5
 
 ###
crypto isakmp key secretkey address 209.165.200.1
##
crypto ipsec transform-set R1-R2 esp-aes 256 esp-sha-hmac
##
crypto map IPSEC-MAP 10 ipsec-isakmp 
 set peer 209.165.200.1
 set pfs group5
 set security-association lifetime seconds 86400
 set transform-set R1-R2 
 match address 100
###
interface GigabitEthernet0/0
 crypto map IPSEC-MAP
##
access-list 100 permit ip 192.168.1.0 0.0.0.255 192.168.3.0 0.0.0.255
##
crypto isakmp policy 10
 encryption aes 256
 authentication pre-share
 group 5
###
crypto isakmp key secretkey address 209.165.100.1
##
crypto ipsec transform-set R2-R1 esp-aes 256 esp-sha-hmac
##
crypto map IPSEC-MAP 10 ipsec-isakmp 
 set peer 209.165.100.1
 set pfs group5
 set security-association lifetime seconds 86400
 set transform-set R2-R1 
 match address 100
##
interface GigabitEthernet0/0
 crypto map IPSEC-MAP
####
access-list 100 permit ip 192.168.3.0 0.0.0.255 192.168.1.0 0.0.0.255
