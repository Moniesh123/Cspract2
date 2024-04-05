enable secret enpa55

line console 0
password conpa55
login

ip domain-name ccnasecurity.com
username admin secret adminpa55
line  vty 0 4
login local
crypto key generate rsa

int loopback 0
ip address 192.168.2.1 255.255.255.0
no shut

r1
ip route 192.168.3.0 255.255.255.0 10.1.1.2
ip route 10.2.2.0 255.255.255.252 10.1.1.2
ip route 192.168.2.0 255.255.255.0 10.1.1.2

r2
ip route 192.168.1.0 255.255.255.0 10.1.1.1
ip route 192.168.2.0 255.255.255.0 10.2.2.1

access-list 10 permit host 192.168.3.3

line vty 0 4
access-class 10 in

ssh -l admin 192.168.2.1

access-list 120 permit udp any host 192.168.1.3 eq domain
access-list 120 permit tcp any host 192.168.1.3 eq smtp
access-list 120 permit tcp any host 192.168.1.3 eq ftp
access-list 120 deny tcp any host 192.168.1.3 eq 443
access-list 120 permit tcp host 192.168.3.3 host 10.1.1.1 eq 22


int se0/1/0
ip access-group 120 in

access-list 120 permit icmp any any echo-reply
access-list 120 permit icmp any any unreachable
access-list 120 deny icmp any any
access-list 120 permit ip any any 

access-list 110 permit ip 192.168.3.0 0.0.0.255 any

int gig0/0
ip access-group 110 in

access-list 100 permit tcp 10.0.0.0 0.255.255.255 host 192.168.3.3 eq 22
access-list 100 deny ip 10.0.0.0 0.255.255.255 any
access-list 100 deny ip 172.16.0.0 0.15.255.255 any
access-list 100 deny ip 192.168.0.0 0.0.255.255 any
access-list 100 deny ip 127.0.0.0 0.255.255.255 any
access-list 100 deny ip 224.0.0.0 15.255.255.255 any
access-list 100 permit ip any any

int se0/1/0
ip access-group 100 in 
