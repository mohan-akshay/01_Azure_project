# 🔧 Azure Project 1: Core Services Sandbox

## 🧠 Topics Covered

- Azure Resource Groups
- Virtual Machines (VM) - Linux (Ubuntu)
- Azure Storage Account (Blob Storage)
- Network Security Groups (NSG)
- SSH Access to VMs
- Inbound Port Rules
- Basic Networking and Connectivity Testing
- Azure Cleanup Best Practices

---

## 📘 Summary

This project provides hands-on experience with fundamental Azure services. You'll deploy a Linux Virtual Machine (VM), create a Storage Account with Blob Storage, and configure basic networking with security rules. You'll also test network connectivity and perform resource cleanup using the Azure Portal.

---

## 📌 Scenario

You are tasked with setting up a test environment in Microsoft Azure to:
- Host a Linux VM (using free-tier eligible options),
- Create and configure Blob Storage for file uploads,
- Test inbound port rules for controlling access to the VM,
- Connect securely via SSH,
- Ensure everything is cleaned up afterward.

---

## 🛠️ Steps

### 1. Create a Resource Group

1. Go to **Azure Portal** → **Resource groups** → **Create**.
2. **Resource Group Name**: `az900-project1-rg`
3. **Region**: `East US`
4. Click **Review + create** → **Create**

---

### 2. Deploy a Linux Virtual Machine (VM)

1. Go to **Virtual machines** → **Create** → **Azure virtual machine**
2. **Basics**:
   - **Image**: Ubuntu Server 20.04 LTS - Gen2
   - **Size**: `Standard_B1s` (Free-tier eligible)
   - **Authentication**: SSH public key
     - Generate a new key pair and **download the private key** (`.pem`)
3. **Disks**:
   - OS disk type: `Standard SSD`
4. **Networking**:
   - Public inbound ports: **Allow selected ports**
   - Select: `SSH (22)`
   - Create a new **Network Security Group (NSG)**
5. Click **Review + create** → **Create**

---

### 3. Create a Storage Account

1. Go to **Storage accounts** → **Create**
2. **Resource group**: `az900-project1-rg`
3. **Storage account name**: `az900project1sa` (must be globally unique)
4. **Performance**: `Standard`
5. **Redundancy**: `Locally-redundant storage (LRS)`
6. Click **Review + create** → **Create**

---

### 4. Upload a File to Blob Storage

1. Navigate to your new **Storage Account**
2. Go to **Containers** → Click **+ Container**
   - Name: `demo-container`
   - Access level: `Private (no anonymous access)`
3. Open the container → Click **Upload**
4. Choose a small file (e.g., `.txt`) → Click **Upload**

---

### 5. Test Networking (Block HTTP)

1. Go to **VM** → **Networking** → **Add inbound port rule**
2. **Source**: `Any`
3. **Destination port ranges**: `80`
4. **Protocol**: `TCP`
5. **Action**: `Deny`
6. **Priority**: `1000`
7. **Name**: `Block-HTTP`
8. Click **Add**

#### ✅ Verification:

- Open a browser and visit: `http://<Your-VM-Public-IP>`
- You should receive a timeout or connection error (HTTP is blocked).

---

### 6. Connect to Your VM via SSH

Run the following command from your terminal:

```bash
chmod 400 <path-to-private_key.pem>

ssh -i <path-to-downloaded-private-key>.pem azureuser@<VM-Public-IP>
