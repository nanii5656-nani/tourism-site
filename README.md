                                      DEPLOYING A STATIC WEBSITE USING AZURE LOAD BALANCER 


# DEPLOYING A STATIC WEBSITE USING AZURE LOAD BALANCER

## 📖 Introduction
A **Load Balancer** is a networking solution that distributes incoming traffic across multiple servers to ensure no single server becomes a bottleneck.  
In Azure, a Load Balancer helps improve **availability**, **reliability**, and **performance** by routing requests to healthy Virtual Machines (VMs) in different zones.


## Load Balancer Architecture
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/637dd129-5632-453f-ab0a-f1d733360f43" />


---

## 🛠️ Project Overview
This project demonstrates how to deploy a **highly available static website** using:
- Azure CLI for resource creation
- Virtual Network with **two subnets** using ARM templates
- Two Linux Ubuntu Virtual Machines in **different availability zones**
- Azure Load Balancer to route traffic between them

---

## 📌 Steps to Implement

### 1️⃣ Create a Resource Group using Azure CLI
```bash
# Variables
RESOURCE_GROUP="StaticWebLB-RG"
LOCATION="eastus"

# Create Resource Group
az group create \
  --name $RESOURCE_GROUP \
  --location $LOCATION

---

### 1️⃣ Create a Resource Group using Azure CLI
