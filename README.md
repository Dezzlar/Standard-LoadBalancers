# Azure Standard Load Balancer Setup

This project demonstrates the configuration of a Standard Azure Load Balancer to distribute HTTP traffic across multiple Windows Virtual Machines running IIS web servers. It includes health probes, backend pools, load balancing rules, and network security group configurations to ensure high availability and fault tolerance.

---

## 🚀 Project Overview

The goal of this project is to create a highly available web service environment in Azure by leveraging a Standard Load Balancer. It balances incoming traffic between two IIS web servers hosted on separate VMs within a Virtual Network, using health probes to monitor backend VM health and route traffic accordingly.

---

## 🗺 Architecture Diagram

![Load Balancer Overview](docs/LBS.png)
![Frontend connectivity](docs/VMS.png)

---

## 📋 Prerequisites

- Azure subscription with permissions to create resources
- Two Windows Server VMs in the same Virtual Network, each with IIS installed
- Network Security Groups (NSGs) configured to allow HTTP traffic (port 80)
- Basic familiarity with Azure Portal and PowerShell

---

## 🛠️ Deployment Steps

### Step 1: Create a Standard Azure Load Balancer

- Navigate to **Azure Portal > Load Balancer > Create**
- Set:
  - SKU: **Standard**
  - Type: **Public**
  - Assign a new **Static Public IP** for frontend configuration
- Complete creation

### Step 2: Configure Backend Pool

- Within the Load Balancer, go to **Backend Pools**
- Add both VM network interfaces from the same Virtual Network

### Step 3: Setup Health Probe

- Go to **Health Probes** and add a new probe:
  - Protocol: **HTTP**
  - Port: **80**
  - Path: `/iisstart.htm` (or `/`)
  - Interval: 5 seconds
  - Unhealthy threshold: 2

### Step 4: Define Load Balancing Rule

- Add a load balancing rule:
  - Frontend Port: 80
  - Backend Port: 80
  - Backend Pool: Use the one configured above
  - Health Probe: Use the probe created above
  - Session Persistence: None
  - Floating IP: Disabled

---

## 🔧 VM IIS Setup

On each VM, run the following PowerShell to install IIS and configure a simple HTML page:

```powershell
Install-WindowsFeature -Name Web-Server -IncludeManagementTools
echo "Hello from $(hostname)" > C:\inetpub\wwwroot\iisstart.htm
