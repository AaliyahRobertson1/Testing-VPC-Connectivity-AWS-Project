# 📌 **Testing VPC Connectivity – AWS Project**

This project demonstrates the setup and testing of a Virtual Private Cloud (VPC) in AWS, including public and private subnets, internet connectivity, security group configurations, and EC2 communication.

---

## 🛠 **Environments and Technologies Used**
- AWS VPC
- Amazon EC2
- Security Groups & Network ACLs
- Route Tables & Internet Gateway
- Linux CLI (Terminal, Ping, SSH)

---

## 📌 **VPC Architecture**

Before starting, let's look at the overall architecture of the VPC.

![Architecture](assets/architecture-Vpc.png)

---

## 📌 **Step 1: Creating a VPC**

The first step is creating a Virtual Private Cloud (VPC) in AWS.

![My VPC](assets/my-vpc.png)

---

## 📌 **Step 2: Public and Private Subnet Creation**

Two subnets are created:

- ✅ **Public Subnet** – For public-facing resources like a web server.
- ✅ **Private Subnet** – For internal services that shouldn’t be directly accessible from the internet.

![Public Subnet Creation](assets/public-subnet-creation.png)

<img width="1338" alt="Private-subnet-creation" src="https://github.com/user-attachments/assets/f418df49-6201-4c57-95e0-47edd1812f84" />

---

## 📌 **Step 3: Internet Gateway (IGW) & Route Tables**

To allow the public subnet to access the internet, an Internet Gateway (IGW) is attached, and the Public Route Table is configured.

- **Attaching an Internet Gateway**
  ![IGW](assets/IGW.png)

- **Public Route Table Configuration**
  ![Public Route Table](assets/public-route-tables-1.png)

- **Private Route Table Configuration**
  ![Private Route Table](assets/private-route-table-1.png)

---

## 📌 **Step 4: Configuring Security Groups**

Security Groups are configured to control inbound and outbound traffic.

- **Public Security Group – Inbound Rules**
<img width="1660" alt="public security group inbound update" src="https://github.com/user-attachments/assets/b851d9af-74a5-4aec-8af3-6a7f79da1893" />

- **Private Security Group – Inbound Rules**
![Private Security Group Inbound](assets/private-security-inbound-update.png)

---

## 📌 **Step 5: Configuring Network ACLs**

Network ACLs (Access Control Lists) are configured to provide an additional layer of security.

- **Public Network ACL Rules**
<img width="1411" alt="Public-network-ACL" src="https://github.com/user-attachments/assets/f5a102d7-5179-4fe8-ae89-d55717db296c" />
  
- **Private Network ACL Rules**
<img width="1446" alt="inbound-acl" src="https://github.com/user-attachments/assets/90072bf5-4422-4623-b57d-7dc91ba4e947" />

  ---

## 📌 **Step 6: Launching EC2 Instances**

Two EC2 instances are launched:

- ✅ **Public EC2 Instance** – Accessible via SSH from the internet.
- ✅ **Private EC2 Instance** – Only accessible from the Public EC2 Instance.

- **Public Server Instance**
  ![Public Server](assets/public-server.png)

- **Private Server Instance**
  ![Private Server](assets/private-server.png)

---

## 📌 Step 7: Troubleshooting Public Server Connection
If the public server fails to connect, the issue is likely with the Public Security Group settings or inbound rule misconfigurations.

✅ Check Public Security Group Inbound Rules

Ensure the Public Security Group allows SSH (port 22) and HTTP (port 80) traffic.

✅ Public Security Group - Inbound Rules Example:
<img width="1660" alt="public security group inbound update" src="https://github.com/user-attachments/assets/b851d9af-74a5-4aec-8af3-6a7f79da1893" />
✅ Ensure the Instance is Connected to the Public Security Group

If the instance is configured with any other security group or port settings, update it to use the Public Security Group with the correct rules.

✅ Successful Public Linux Connection

![Public Linux Connection](assets/Public-linux-connection.png)

## 📌 Step 8: Troubleshooting Private Server Connection (Failed Ping)
After successfully connecting to the public server, the next step is to ping the private server to test connectivity.

💻 Attempt to Ping the Private Server
Run the following command from the Public EC2 instance:

ping <Private-EC2-IP>
✅ Example Command:
ping <Private-EC2-IP>

![Ping Private EC2](assets/ping-private-EC2.png)

⚠️If the Ping  Fails, Check These Settings:
✅ Check Private Route Table
Verify that the private subnet is associated with the correct route table.

![Private Ro<img width="1680" alt="ssh" src="https://github.com/user-attachments/assets/2afbc2b9-94ba-4b31-91cc-b069416ef602" />
ute Table](assets/private-route-table-1.png)

Fix Private Network ACL Rules
The issue might be restrictive Network ACL (NACL) settings.

1️⃣ Correct Private NACL Inbound Rules:

Rule #	Type	Protocol	Port Range	Source	Description
100	All ICMP - IPv4	ICMP	ALL	10.0.0.0/24	Allows ping traffic
        
![Private Network ACL Update](assets/private-network-ACL-update.png)
2️⃣ Correct Private NACL Outbound Rules and make it the same

3️⃣ If NACLs look fine, check the Private Security Group inbound rules.
✅ Ensure the following rule exists:

Rule #	Type	Protocol	Port Range	Source	Description
100	All ICMP - IPv4	ICMP	ALL	10.0.0.0/24	Allows ping traffic


✅ Final Successful Ping Test

![Success](assets/success.png)
🔎 Summary of Fixes for Failed Ping Issues
1️⃣ Ensure the Private Subnet is correctly set up.
2️⃣ Verify the Private Route Table.
3️⃣ Fix Private Network ACL Rules (Allow ICMP).
4️⃣ Check Private Security Group Inbound Rules.
