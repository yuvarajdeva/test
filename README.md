

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

### **🔹 Step 6: Create NAT Gateway (Optional)**
1. **Go to AWS Console** → **VPC** → **NAT Gateways**.
2. Click **Create NAT Gateway**.
   - **Subnet:** `SeculboxPublicSubnet`
   - **Connectivity Type:** `Public`
   - **Allocate Elastic IP address**
3. Click **Create NAT Gateway**.

### **🔹 Step 7: Create Route Tables**
1. **Go to AWS Console** → **VPC** → **Route Tables**.
2. Create a **NAT Gateway Route Table**:
   - **Destinations:**
     - `0.0.0.0/0` → `NAT Gateway`
     - `10.0.0.0/16` → `Local`
     - `192.168.1.0/24` → `VGW`
3. Create another **Route Table** for Internet Gateway:
   - **Destinations:**
     - `0.0.0.0/0` → `Internet Gateway`
     - `10.0.0.0/16` → `Local`

---

## **2️⃣ AWS EC2 Instance Setup (Private Instance via VPN)**
### **🔹 Step 1: Create a Private EC2 Instance**
1. **Go to AWS Console** → **EC2** → **Instances** → **Launch Instance**.
2. **Choose an AMI:** Amazon Linux 2 / Ubuntu.
3. **VPC:** `SeculboxPrivateVPC`
4. **Subnet:** `SeculboxPrivateSubnet`
5. **Auto-assign Public IP:** `Disable`
6. **Security Group Settings:**
   - ✅ Allow **SSH (port 22) only from VPN subnet (`192.168.1.0/24`).**
   - ✅ Allow **internal communication (`10.0.0.0/16`).**
   - ✅ Allow **ICMP (ping) and UDP (for VPN).**
7. Click **Launch Instance**.

---

## **3️⃣ StrongSwan VPN Configuration (Ubuntu/macOS)**
### **🔹 Step 1: Download VPN Configuration**
1. **Go to AWS Console** → **VPC** → **Site-to-Site VPN Connections**.
2. **Download Configuration** → Select `StrongSwan`.

### **🔹 Step 2: Install StrongSwan**
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
Paste the following:
```conf
conn aws-vpn-tunnel-1
    auto=start
    left=%defaultroute
    leftid=<Your Public IP>
    right=<AWS Tunnel IP>
    type=tunnel
    leftauth=psk
    rightauth=psk
    keyexchange=ikev1
    ike=aes128-sha1-modp1024
    esp=aes128-sha1-modp1024
    dpddelay=10s
    dpdtimeout=30s
    dpdaction=restart
```
Save & Exit (`CTRL + X`, then `Y`, then `Enter`).

---

### **🔹 Step 4: Start VPN & Verify Connection**
```sh
sudo ipsec restart
sudo ipsec up aws-vpn-tunnel-1
sudo ipsec statusall
```
✅ **If `ESTABLISHED` appears, VPN is working!**

---

## **4️⃣ Multi-PC VPN Access via MacBook**
If you want **multiple PCs** on the same network to **share VPN access** through a MacBook:

### **🔹 Step 1: Enable Packet Forwarding**
```sh
sudo sysctl -w net.inet.ip.forwarding=1
```
Make it **permanent**:
```sh
sudo nano /etc/sysctl.conf
```
Add:
```sh
net.inet.ip.forwarding=1
```
Save & apply:
```sh
sudo sysctl -f /etc/sysctl.conf
```

### **🔹 Step 2: Route Traffic via MacBook**
On **client machines** (other PCs), add:
```sh
sudo route -n add -net 10.0.0.0/16 192.168.5.142
```
(Replace `192.168.5.142` with your **MacBook’s local IP**.)

---

## **🎯 Conclusion**
- ✅ **AWS Site-to-Site VPN with StrongSwan successfully configured.**
- ✅ **Private AWS EC2 instances accessible via VPN.**
- ✅ **MacBook can act as a VPN gateway for multiple devices.**

🚀 **Now, all client machines can securely access AWS private resources via VPN!** 🚀  
🔹 **Contributions & PRs welcome!** 🛠️
```

---

🚀 **This README file is formatted for GitHub and includes all setup steps from the PDF!** 🚀  
Let me know if you need modifications! 🚀
