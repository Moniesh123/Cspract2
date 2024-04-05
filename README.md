Part1: configure router

Step1: configure password for vty lines
line vty 0 4
password vtypa55
login

step2: configure secret password on router
enable secret enpa55

step3: configure OSPF on router
router ospf 1
network 192.168.1.0 0.0.0.255 area 0

step4: configure OSPF md5 authentication for all routers in area 0
router ospf 1
area 0 authentication message-digest

step5: configure md5 key for all routers in area 0
int gig0/0
ip ospf message-digest-key 1 md5 md5pa55

step6: Verify md5 authentication configuration
show ip ospf interface

step7: Verify end to end connectivity => PC0 -> R1 and PC1 -> R1

Part2: configure local AAA authentication for console Access on R1

step1: configure local username on R1
username admin secret adminpa55

step2: configure local AAA authentication for console access on R1
aaa new-model
aaa authentication login default local

step3: configure the line console to use the defined AAA authentication method
line console 0
login authentication default

step4: verify the AAA authentication method
exit from R1 hash(#) prompt => R1#exit

Part3: configure local AAA Authentication for vty lines on R1

step1: Configure domain-name and crypto key for use with SSH
ip domain-name ccnasecurity.com
crypto key generate rsa

step2: Configure a named list AAA authentication method for the vty lines on R1
aaa authentication login ssh-login local

step3: configure the vry lines to use the defined AAA authentication method
line vty 0 4
login authentication ssh-login
transport input ssh
end

step4: Verify the AAA authentication method
PC0>
ssh -l admin 92.168.1.1
PC1>
ssh -l admin 92.168.1.1
