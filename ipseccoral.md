
**Ref:**
-  http://blog.ruanbekker.com/blog/2018/02/11/setup-a-site-to-site-ipsec-vpn-with-strongswan-and-preshared-key-authentication/
- https://manuals.gfi.com/en/kerio/control/content/vpn/configuring-ipsec-vpn-tunnel-kerio-control-and-another-device-1390.html


## Test ENV Prerequisites

**Site 1 (Our Side)**
Server 1: 165.227.255.190[Floating] (10.132.141.8) (ipv4: 142.93.50.180)
Client 1: 10.136.89.134 (138.197.224.51) [Portal API]

**Site 2 (Colina Side)**
Server 2: 65.75.64.21
Client 2: 192.168.15.0/24

=======================================================================

Digital_Ocean_IP = 138.197.50.141
Colina_IP = 65.75.64.21

=======================================================================


## Setup

```
apt update && sudo apt upgrade -y
apt install strongswan -y
```

- To support EAP auth

```
sudo apt install -y strongswan-plugin-eap-mschapv2
```

- Make iptables persistent
```
sudo apt install -y iptables-persistent
```

```
cat >> /etc/sysctl.conf << EOF
net.ipv4.ip_forward = 1
net.ipv4.conf.all.accept_redirects = 0
net.ipv4.conf.all.send_redirects = 0
EOF
```

```
net.ipv4.ip_no_pmtu_disc = 1
```


## Generate Preshared Key:

openssl rand -base64 64

```
Y1q8DB/0DLbVTvynQk8rn31si+GL59/1Gnrn0ZsqQVFnPZLJALlv3vRA8OZCJYnV
Vj7YPB/6douY0LIBD6dLJw==
```

## Configure Site A

- Setup secrets
/etc/ipsec.secrets

```
# source       destination
10.222.111.131 10.222.111.132 : PSK "Y1q8DB/0DLbVTvynQk8rn31si+GL59/1Gnrn0ZsqQVFnPZLJALlv3vRA8OZCJYnVVj7YPB/6douY0LIBD6dLJw=="

10.222.111.131 : RSA "/etc/ipsec.d/private/vpn-server-key.pem"
shivam %any% : EAP "welcome123"
```


- VPN configuration

/etc/ipsec.conf

```
# basic configuration
config setup
        charondebug="all"
        uniqueids=yes
        strictcrlpolicy=no

# connection to site 2 datacenter
conn site1-to-site2
  authby=secret
  left=%defaultroute
  leftid=10.222.111.131
  leftsubnet=172.16.1.0/24
  right=10.222.111.132
  rightsubnet=172.16.2.0/24
  ike=aes256-sha2_256-modp1024!
  esp=aes256-sha2_256!
  keyingtries=0
  ikelifetime=1h
  lifetime=8h
  dpddelay=30
  dpdtimeout=120
  dpdaction=restart
  auto=start
```

```
# connection to site 1 from client
conn client-to-site1
    auto=add
    compress=no
    type=tunnel
    keyexchange=ikev2
    fragmentation=yes
    forceencaps=yes
    ike=aes256-sha1-modp1024,3des-sha1-modp1024!
    esp=aes256-sha1,3des-sha1!
    dpdaction=clear
    dpddelay=300s
    rekey=no
    left=%any
    leftid=10.222.111.131
    leftcert=/etc/ipsec.d/certs/vpn-server-cert.pem
    leftsendcert=always
    leftsubnet=0.0.0.0/0
    right=%any
    rightid=%any
    rightauth=eap-mschapv2
    rightdns=8.8.8.8,8.8.4.4
    rightsourceip=10.10.10.0/24
    rightsendcert=never
    eap_identity=%identity
```

- Firewall Rules:

```
sudo iptables -t nat -A POSTROUTING -s 172.16.2.0/24 -d 172.16.1.0/24 -j MASQUERADE
```

```
sudo iptables -t nat -A POSTROUTING -s 10.222.111.132/32 -d 172.16.1.0/24 -j MASQUERADE
```



## Configure Site B

- Setup secrets
/etc/ipsec.secrets

```
# source       destination
10.222.111.132 10.222.111.131 : PSK "Y1q8DB/0DLbVTvynQk8rn31si+GL59/1Gnrn0ZsqQVFnPZLJALlv3vRA8OZCJYnVVj7YPB/6douY0LIBD6dLJw=="
```

- VPN configuration

/etc/ipsec.conf

```
# basic configuration
config setup
        charondebug="all"
        uniqueids=yes
        strictcrlpolicy=no

# connection to site 1 datacenter
conn site2-to-site1
  authby=secret
  left=%defaultroute
  leftid=10.222.111.132
  leftsubnet=172.16.2.0/24
  right=10.222.111.131
  rightsubnet=172.16.1.0/24
  ike=aes256-sha2_256-modp1024!
  esp=aes256-sha2_256!
  keyingtries=0
  ikelifetime=1h
  lifetime=8h
  dpddelay=30
  dpdtimeout=120
  dpdaction=restart
  auto=start
```

- Firewall Rules:

```
sudo iptables -t nat -A POSTROUTING -s 172.16.1.0/24 -d 172.16.2.0/24 -j MASQUERADE
```

```
sudo iptables -t nat -A POSTROUTING -s 10.222.111.131/32 -d 172.16.2.0/24 -j MASQUERADE
```

## Start the VPN

- Start the VPN on both ends:

```
$ sudo ipsec restart
```


## Check logs

```
tail -f /var/log/syslog
```




## Creating a Certificate Authority


- Create directory for VPN

```
mkdir certs-ipsec
cd certs-ipsec
```

- generate and secure the key

```
ipsec pki --gen --type rsa --size 4096 --outform pem > server-root-key.pem
chmod 600 server-root-key.pem
```

- create root certificate authority, using the key to sign the root certificate

```
ipsec pki --self --ca --lifetime 3650 \
--in server-root-key.pem \
--type rsa --dn "C=US, O=Coral VPN Server, CN=VPN Server Root CA" \
--outform pem > server-root-ca.pem
```

## Generating a Certificate for the VPN Server

- Create a private key for the VPN server with the following command:

```
ipsec pki --gen --type rsa --size 4096 --outform pem > vpn-server-key.pem
```

- Create and sign the VPN server certificate with the certificate authority's key we created

```
ipsec pki --pub --in vpn-server-key.pem \
--type rsa | ipsec pki --issue --lifetime 1825 \
--cacert server-root-ca.pem \
--cakey server-root-key.pem \
--dn "C=US, O=Coral Media, CN=server_name_or_ip" \
--san server_name_or_ip \
--flag serverAuth --flag ikeIntermediate \
--outform pem > vpn-server-cert.pem
```

```
ipsec pki --pub --in vpn-server-key.pem \
--type rsa | ipsec pki --issue --lifetime 1825 \
--cacert server-root-ca.pem \
--cakey server-root-key.pem \
--dn "C=US, O=Coral Media, CN=167.99.226.107" \
--san 167.99.226.107 \
--flag serverAuth --flag ikeIntermediate \
--outform pem > vpn-server-cert.pem
```

```
ipsec pki --pub --in vpn-server-key.pem \
--type rsa | ipsec pki --issue --lifetime 1825 \
--cacert server-root-ca.pem \
--cakey server-root-key.pem \
--dn "C=US, O=Coral Media, CN=10.132.25.45" \
--san 10.132.25.45 \
--flag serverAuth --flag ikeIntermediate \
--outform pem > vpn-server-cert.pem
```


- Copy the certificates to a path which would allow StrongSwan to read the certificates

```
sudo cp ./vpn-server-cert.pem /etc/ipsec.d/certs/vpn-server-cert.pem
sudo cp ./vpn-server-key.pem /etc/ipsec.d/private/vpn-server-key.pem
```

- secure the keys so they can only be read by the root user

```
sudo chown root /etc/ipsec.d/private/vpn-server-key.pem
sudo chgrp root /etc/ipsec.d/private/vpn-server-key.pem
sudo chmod 600 /etc/ipsec.d/private/vpn-server-key.pem
```



## Allow VPN clients


sudo ufw disable


iptables -P INPUT ACCEPT
iptables -P FORWARD ACCEPT
iptables -F
iptables -Z


sudo iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT

sudo iptables -A INPUT -i lo -j ACCEPT


sudo iptables -A INPUT -p udp --dport  500 -j ACCEPT
sudo iptables -A INPUT -p udp --dport 4500 -j ACCEPT


**May not be needed**
```
sudo iptables -A FORWARD --match policy --pol ipsec --dir in  --proto esp -s 172.16.2.0/24 -j ACCEPT
sudo iptables -A FORWARD --match policy --pol ipsec --dir out --proto esp -d 172.16.2.0/24 -j ACCEPT
```
```
sudo iptables -A FORWARD --match policy --pol ipsec --dir in  --proto esp -s 192.168.0.0/24 -j ACCEPT
sudo iptables -A FORWARD --match policy --pol ipsec --dir out --proto esp -d 192.168.0.0/24 -j ACCEPT
```


sudo iptables -t nat -A POSTROUTING -s 172.16.2.0/24 -o eth0 -m policy --pol ipsec --dir out -j ACCEPT
sudo iptables -t nat -A POSTROUTING -s 172.16.2.0/24 -o eth0 -j MASQUERADE



sudo iptables -t mangle -A FORWARD --match policy --pol ipsec --dir in -s 192.168.0.0/24 -o ens18 -p tcp -m tcp --tcp-flags SYN,RST SYN -m tcpmss --mss 1361:1536 -j TCPMSS --set-mss 1360



