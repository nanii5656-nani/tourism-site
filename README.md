
---

# DEPLOYING A STATIC WEBSITE USING AZURE LOAD BALANCER

## 📖 Introduction

A **Load Balancer** is a networking solution that distributes incoming traffic across multiple servers to ensure no single server becomes a bottleneck.
In Azure, a Load Balancer helps improve **availability**, **reliability**, and **performance** by routing requests to healthy Virtual Machines (VMs) in different availability zones.

---

## 🖼 Load Balancer Architecture

<img width="1536" height="1024" alt="Azure Load Balancer Architecture" src="https://github.com/user-attachments/assets/637dd129-5632-453f-ab0a-f1d733360f43" />

---

## 🛠 Project Overview

This project demonstrates how to deploy a **highly available static website** using:

* **Azure CLI** for resource creation
* **Virtual Network** with two subnets using **ARM templates**
* **Two Linux Ubuntu Virtual Machines** in different availability zones
* **Azure Load Balancer** to route traffic between them

---

## 📌 Steps to Implement

### 1️⃣ Create a Resource Group using Azure CLI

The Resource Group acts as a logical container for all Azure resources in this project.

```bash
# Variables
RESOURCE_GROUP="StaticWebLB-RG"
LOCATION="eastus"

# Create Resource Group
az group create \
  --name $RESOURCE_GROUP \
  --location $LOCATION
```

---

### 2️⃣ Create Virtual Network with Two Subnets using ARM Template

In this step, we set up a **Virtual Network (VNet)** that will host our two subnets — each subnet will be used to place one Virtual Machine in a separate availability zone.
This ensures **high availability** and **fault tolerance** in case one zone experiences downtime.

#### Purpose

* The **Virtual Network** acts as the backbone for infrastructure, enabling secure communication between VMs and the Load Balancer.
* **Subnets** divide the network into smaller segments for better isolation, security, and IP management.

#### ARM Template Structure

The ARM template will:

* Create a VNet named `StaticWebVNet`
* Define two subnets: `subnet1` and `subnet2`
* Assign unique IP address ranges (CIDR blocks) to each subnet

#### Key Points

* CIDR blocks must not overlap.
* Availability Zones are assigned at VM deployment, not in subnet creation.
* ARM templates enable **Infrastructure as Code**, making deployments **repeatable** and **consistent**.

**Resource Flow:**

```
Azure CLI → Deploy ARM Template → Creates VNet → Creates subnet1 & subnet2
```

---

### 3️⃣ Deploy Virtual Machines in Different Availability Zones

We will deploy **two Ubuntu Linux VMs** — one in **Zone 1** and the other in **Zone 2** — to host our static website.

#### Purpose

* Multiple zones increase **resiliency** and **uptime**.
* Each VM will automatically pull the static website code from a GitHub repository during provisioning.

#### VM Deployment Process

1. Use Azure Portal or ARM Template to define VM configurations.
2. Attach each VM to the correct subnet:

   * **VM1** → **subnet1** → **Zone 1**
   * **VM2** → **subnet2** → **Zone 2**
3. Provide a **Custom Script Extension** to:

   * Install a web server (Nginx/Apache)
   * Pull website files from GitHub into `/var/www/html`

#### VM Configuration Highlights

* **OS**: Ubuntu Server LTS
* **Size**: Standard\_B1s (for testing; scale as needed)
* **Networking**: Connected to Load Balancer backend pool
* **Bootstrap Script**: Automates deployment of the static site

---

### 4️⃣ Configure Azure Load Balancer

#### Purpose

The Load Balancer:

* Accepts incoming HTTP requests
* Distributes them evenly across VMs
* Monitors VM health using **probes**

#### Steps

1. Create a **Public Load Balancer** in Azure Portal
2. Assign a **Frontend Public IP**
3. Configure a **Backend Pool** and add both VMs
4. Set up **Health Probes** (HTTP, port 80)
5. Create a **Load Balancing Rule** to map HTTP requests to backend pool

**Traffic Flow:**

```
User Request → Public Load Balancer → Healthy VM in Zone 1 or Zone 2 → Static Website Served
```

---

## ✅ Benefits of This Setup

* **High Availability**: Survives zone-level failures
* **Scalability**: Add more VMs to backend pool easily
* **Cost-Effective**: Pay for what you use
* **Automated Deployment**: ARM templates + scripts enable quick redeployments

---
