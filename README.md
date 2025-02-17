

# 🚀 AWS Site-to-Site VPN Setup & StrongSwan Configuration

This guide provides a **step-by-step setup** for configuring **AWS Site-to-Site VPN** and **StrongSwan VPN** on Ubuntu/macOS for secure access to private AWS resources.

## **📌 Overview**
- ✅ Create **AWS VPC, Subnets, Route Tables, and VPN Connections**
- ✅ Configure **StrongSwan VPN Client on Ubuntu/macOS**
- ✅ Enable **Private AWS EC2 Access via VPN**
- ✅ Support **Multiple Client Devices on the Same Network**

---

## **1️⃣ AWS Setup: Create a VPC, Subnets, and Site-to-Site VPN**
### **🔹 Step 1: Create a VPC**
1. **Go to AWS Console** → **VPC** → **Your VPCs**.
2. Click **Create VPC**.
   - **Name:** `SeculboxPrivateVPC`
   - **IPv4 CIDR Block:** `10.0.0.0/16`
3. Click **Create VPC**.

### **🔹 Step 2: Create Subnets**
1. **Go to AWS Console** → **VPC** → **Subnets**.
2. Click **Create Public Subnet** (for internet access).
   - **Name:** `SeculboxPublicSubnet`
   - **IPv4 CIDR Block:** `10.0.2.0/24`
3. Add a **Private Subnet** (for EC2 instances).
   - **Name:** `SeculboxPrivateSubnet`
   - **IPv4 CIDR Block:** `10.0.1.0/24`
4. Click **Create Subnet**.

### **🔹 Step 3: Create a Customer Gateway**
1. **Go to AWS Console** → **VPC** → **Customer Gateways**.
2. Click **Create Customer Gateway**.
   - **Name:** `My-CGW`
   - **Routing:** `Static`
   - **IP Address:** `<Your Public IP>` (e.g., `217.160.11.18`)
3. Click **Create Customer Gateway**.

### **🔹 Step 4: Create a Virtual Private Gateway (VGW)**
1. **Go to AWS Console** → **VPC** → **Virtual Private Gateways**.
2. Click **Create Virtual Private Gateway**.
   - **Name:** `My-VGW`
   - **Autonomous System Number (ASN):** `65000` (default)
3. Click **Create Virtual Private Gateway**.
4. Attach **VGW** to your VPC:
   - Go to **VPC:** → **Your VPCs:** (SeculboxPrivateVPC)
   - Select your VPC → Click **Attach to virtual Private Gateway**.

### **🔹 Step 5: Create Site-to-Site VPN Connection**
1. **Go to AWS Console** → **VPC** → **Site-to-Site VPN Connections**.
2. Click **Create VPN Connection**.
   - **Name:** `Green-Enclave-VPN`
   - **Virtual Private Gateway:** `My-VGW`
   - **Customer Gateway:** `My-CGW`
   - **Routing Option:** `Static`
   - **Static Routes:** `192.168.1.0/24` (or your on-prem subnet)
3. Click **Create VPN Connection**.

### **🔹 Step 6: Create NAT Gateway**
1. **Go to AWS Console** → **VPC** → **NAT Gateways**.
2. Click **Create NAT Gateway**.
   - **Name:** `Secublox-Nategateway`
   - **Subnet:** Select PublicSubnet (`SeculboxPublicSubnet`)
   - **Connectivity Type:** `Public`
   - **Allocate Elastic IP address**
   - **Create NAT Gateway**.

### **🔹 Step 7: Create Route Tables**
1. **Go to AWS Console** → **VPC** → **Route Tables**.
2. Click create route table
3. **Name:** NateGateway Route Table
4. Select your VPC and Create Route Table
   - Select subnets and add routes
   - Destinations: 0.0.0.0/0, Target NetGateway, its for NateGateway for internet
      - Its`Secublox-Nategateway created in Step 6`
   - Destination: 10.0.0.0/16, Target Local, its default VPC route
   - Destination: 192.168.1.0/24, Target Virtual Private Gateway : its for local Customer gateway traffic route back 
5. Follow same to create another Route table for internet Gateway
   - **Name:** RouteTableNateGateway
   - Select public subnet
   - Routes
     - `0.0.0.0/0` → `Internet Gateway`(for internet)
     - `10.0.0.0/16` → `Local`(Default VPC route)

---

## **2️⃣ AWS EC2 Instance Setup (Private Instance via VPN)**
### **🔹 Step 1: Create a Private EC2 Instance**
1. **Go to AWS Console** → **EC2** → **Instances** → **Launch Instance**.
2. **Choose an AMI:** Amazon Linux 2 / Ubuntu.
3. Choose an Instance Type: `t2.micro` (or as needed).
4. **Choose the VPC and Subnet:**
   - **VPC:** `SeculboxPrivateVPC`
   - **Subnet:** `SeculboxPrivateSubnet`
   - **Auto-assign Public IP:** `Disable`
5. **Security Group Settings:**
   - ✅ Allow **SSH (port 22) only from VPN subnet (`192.168.1.0/24`).**
   - ✅ Allow **internal communication (`10.0.0.0/16`).**
   - ✅ Allow outbound traffic to All ports 
   - ✅ Allow **ICMP (ping) and UDP (for VPN).**
6. Other settings as per Needs and Launch the Instance with Your SSH Key.
7. Click **Launch Instance**.

---

## **3️⃣ StrongSwan VPN Configuration (Ubuntu/macOS)**
### **🔹 Step 1: Download VPN Configuration from AWS Site to Site VPN **
1. **Go to **VPC** → **Site-to-Site VPN Connections**.
2. **Select Site to Site VPN and Download Configuration** → Select `StrongSwan`.

### **🔹 Step 2: Install StrongSwan in your local machine**
#### **On Ubuntu:**
```sh
sudo apt update
sudo apt install -y strongswan strongswan-pki libcharon-extra-plugins libcharon-extauth-plugins
```
#### **On macOS:**
```sh
brew install strongswan
```

### **🔹 Step 3: Configure StrongSwan**
Create or modify **`/etc/ipsec.conf`** (Ubuntu) or **`/opt/homebrew/etc/ipsec.conf`** (macOS).
```sh
sudo nano /etc/ipsec.conf
```
**Paste the Following Configuration: (this configuration is based on download configuration file instruction in previous steps)**
```config setup
    uniqueids=no
# ======================= TUNNEL 1 =======================
conn aws-vpn-tunnel-1
	auto=start
	left=%defaultroute
	leftid=106.219.86.74 #this is local machine IP
	right=18.158.20.117 # this site to site VPN tunnel IP
	type=tunnel
	leftauth=psk
	rightauth=psk
	keyexchange=ikev1
	ike=aes128-sha1-modp1024
	ikelifetime=8h
	esp=aes128-sha1-modp1024
	lifetime=1h
	keyingtries=%forever
	leftsubnet=0.0.0.0/0
	rightsubnet=0.0.0.0/0
	dpddelay=10s
	dpdtimeout=30s
	dpdaction=restart
	## Please note the following line assumes you only have two tunnels in your Strongswan configuration file. This "mark" value must be unique and may need to be changed based on other entries in your configuration file.
	mark=100
	## Uncomment the following line to utilize the script from the "Automated Tunnel Healhcheck and Failover" section. Ensure that the integer after "-m" matches the "mark" value above, and <VPC CIDR> is replaced with the CIDR of your VPC
	## (e.g. 192.168.1.0/24)
	leftupdown="/etc/ipsec.d/aws-updown.sh -ln aws-vpn-tunnel-1 -ll 169.254.133.102/30 -lr 169.254.133.101/30 -m 100 -r 10.0.0.0/16"
# ======================= TUNNEL 2 =======================
conn aws-vpn-tunnel-2
	auto=start
	left=%defaultroute
	leftid=106.219.86.74
	right=52.57.188.15
	type=tunnel
	leftauth=psk
	rightauth=psk
	keyexchange=ikev1
	ike=aes128-sha1-modp1024
	ikelifetime=8h
	esp=aes128-sha1-modp1024
	lifetime=1h
	keyingtries=%forever
	leftsubnet=0.0.0.0/0
	rightsubnet=0.0.0.0/0
	dpddelay=10s
	dpdtimeout=30s
	dpdaction=restart
	## Please note the following line assumes you only have two tunnels in your Strongswan configuration file. This "mark" value must be unique and may need to be changed based on other entries in your configuration file.
	mark=200
	## Uncomment the following line to utilize the script from the "Automated Tunnel Healhcheck and Failover" section. Ensure that the integer after "-m" matches the "mark" value above, and <VPC CIDR> is replaced with the CIDR of your VPC
	## (e.g. 192.168.1.0/24)
	leftupdown="/etc/ipsec.d/aws-updown.sh -ln Tunnel2 -ll 169.254.197.54/30 -lr 169.254.197.53/30 -m 200 -r 10.0.0.0/16"

```
✅ Save & Exit (`CTRL + X`, then `Y`, then `Enter`).

```sh
sudo nano /etc/ipsec.secret or /opt/homebrew/etc/ipsec.secret
```

**Paste the Following Configuration: (this configuration is based on download configuration file instruction in previous steps)**

```
# This file holds shared secrets or RSA private keys for authentication.
# RSA private key for this host, authenticating it to any other host
# which knows the public part.
106.219.86.74 18.158.20.117 : PSK "nqjiAWDDmguj25fYqRcY0M4GC.hWf71b"
106.219.86.74 52.57.188.15 : PSK "Blc9WmHnH58YnENf8iVF9gjV9xyzvh_B"

```
✅ Save & Exit (`CTRL + X`, then `Y`, then `Enter`).

sudo nano /etc/ipsec.d/aws-updown.sh or  /opt/homebrew/etc/ipsec.d/aws-updown.sh 

This file is for **Automated Tunnel Healthcheck and Failover** which you can find in the downloaded configuration file. Past the bash code starting from

```
#!/bin/bash To esac
```
✅ Save & Exit (`CTRL + X`, then `Y`, then `Enter`).

---

### **🔹 Step 4: Restart StrongSwan & Connect to VPN**
```sh
sudo ipsec restart
sudo ipsec up aws-vpn-tunnel-1
sudo ipsec up aws-vpn-tunnel-2
sudo ipsec statusall
Check VPN status: sudo ipsec statusall
```
✅ **If `ESTABLISHED` appears, VPN is working!**

---

## ** Step 4: Test Private Instance Access via VPN**
1. **Ping the private EC2 instance:**
   - ping 10.0.1.106  # Replace with EC2 private IP
2. SSH into the EC2 instance via VPN:
   - ssh -i your-key.pem ec2-user@10.0.1.106
3. ✅ If you can SSH, your private EC2 instance is successfully accessible **only through VPN!**

## **🔹 Step 5: Allow others PC to access AWS through single PC where we configure / Connected VPN**
1. Other PC 
   - Set default gateway IP
   - Restart networking and ping to aws
2. Same we can apply in all PCs which are in same network 
3. On Main server PC where we configured VPN
   - Enable Packet Forwarding
      - sudo sysctl -w net.inet.ip.forwarding=1
      - Make it permanent:
         - sudo nano /etc/sysctl.conf
         - Add: 
            - net.inet.ip.forwarding=1
         - Save and apply :
            - sudo sysctl -f /etc/sysctl.conf
