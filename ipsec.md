## Ispec Strong Swan for Connecting Site to site (we have two vpn server each with one client)

### Network Topology 

                                                                   site-to-site-tunnel
    test-server-01-site01----------------VPN-server-01-site-01================================VPN-server-02-site-02-----------------------test-server-01-site02
    public: 167.99.48.248                public: 64.225.10.180                                public: :64.227.145.161                     public: 64.227.136.97
    private: 10.108.16.3                 private:10.108.16.2                                 private : 10.122.0.2                        private : 10.122.0.3

        |-----------------------Route-Add-1-via-VPN-server-01--------------------------------------------|

        |---------------------------------------------------------------------------------Route-Add-2-via-VPN-server-02-----------------------------------|


### Content

1- Setting up the server
2- Installation
3- PSK 
4- Configuration files on both the servers 
5- Adding the Route 

### Setting up the server 

- Configure the kernel to enable packet forwarding for IPv4. Edit the configuration file

```

nano /etc/sysctl.conf

```

- Add the following Lines 

```

net.ipv4.ip_forward = 1
net.ipv6.conf.all.forwarding = 1
net.ipv4.conf.all.accept_redirects = 0
net.ipv4.conf.all.send_redirects = 0

```

- Save and close the file. Then, run the following command to reload the settings:

```

sysctl -p

```

### Installation 

- install the strongSwan IPSec daemon in your system

```

apt-get install strongswan libcharon-extra-plugins strongswan-pki -y

```

- Optional setting include setting up certificate Authority 

### PSK 
- Open the following file on both vpn-server-01 and vpn-server-02

```

vi /etc/ipsec.secrets

```
Edit this file differently on both VPN servers 
  
- On vpn server 1

```

64.225.10.180 64.227.145.161 : PSK "your-ownsecret-text"

```

- On vpn server 2

```

64.227.145.161 64.225.10.180 : PSK "your-ownsecret-text"

```
 
- Save the files

### Configuration of Site to Site 

- Open conf file on both the server

```

vi /etc/ipsec.conf 

```

Edit the file Differently on both the servers 

## On VPN-server-01 

```

config setup
        charondebug="all"
        strictcrlpolicy=no
        uniqueids = yes

conn %default
conn tunnel
        left=64.225.10.180
        leftsubnet=10.108.16.0/24
        right=64.227.145.161
        rightsubnet=10.122.0.0/24
        ike=aes256-sha256-ecp256
        esp=aes256-sha256!
        keyingtries=0
        ikelifetime=1h
        lifetime=8h
        dpddelay=30
        dpdtimeout=120
        dpdaction=clear
        authby=secret
        auto=start
        keyexchange=ikev2
        type=tunnel

```

## On VPN-server-02

```

config setup
        charondebug="all"
        strictcrlpolicy=no
        uniqueids = yes
conn %default
conn tunnel
        left=64.227.145.161
        leftsubnet=10.122.0.0/24
        right=64.225.10.180
        rightsubnet=10.108.16.0/24
        ike=aes256-sha256-ecp256
        esp=aes256-sha256!
        keyingtries=0
        ikelifetime=1h
        lifetime=8h
        dpddelay=30
        dpdtimeout=120
        dpdaction=clear
        authby=secret
        auto=start
        keyexchange=ikev2
        type=tunnel

```

- save the files

- now on both the server run the following command to apply the configuration changes 

```

sudo ipsec restart 

```

- check the status 

```
sudo status

```

### Adding the Route 

#### Now connecting Test servers to the tunnel would require routing the traffic for each testing server .


Step 1 : Route traffic between 
         test-server-01 and VPN-server-02 
         VIA VPN-Server-01 as VPN-server-01 can communicate to both of them 

#### First, add route inside test-server-01 to reach vpn-server-02 using private ip of VPN-server-01 

- go to the test-server-01 and run add route command
 
```

sudo ip route add 10.122.0.2 via 10.108.16.2 dev eth1

```         

 
#### Now add route inside test-server-02 to reach VPN-server-01 using private I.Ps 

- go to the VPN-server-02 and run add route command

```

sudo ip route add 10.108.16.2 via 10.122.0.2 dev eth1

```

### Now we will Add Route on Test-server-02 to reach test-server-1 using VPN-server-02

- Go to the test-server-02 and run add route command

- We will route traffic via vpn-server-02 because VPN server 2 can reach both vpnserver-01 and test-server-01

```
sudo ip route add 10.108.16.3 via 10.122.0.2 dev eth1

```

# Now we will check the conectivity 

