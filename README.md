#  Azure VM Creation using Azure CLI — DevOps Lab

## Overview
This project documents how to create an **Azure Virtual Machine** using the **Azure CLI**

##  VM Specifications For Example 

| Property         | Value                          |
|------------------|-------------------------------|
| VM Name          | `devops-vm`                   |
| OS Image         | `Ubuntu2204`                  |
| VM Size          | `Standard_B2s`                |
| Admin Username   | `azureuser`                   |
| Authentication   | SSH Keys (auto-generated)     |
| Storage SKU      | `Standard_LRS`                |
| OS Disk Size     | `30 GB`                       |
| State            | `VM running`                  |

---

##  Prerequisites

- Azure CLI installed (`az --version`)
- Logged in to Azure (`az login`)
- An existing resource group

---

##  Step-by-Step Commands

### Step 1 — Check your resource group
```bash
az group list --output table
```

### Step 2 — Create the VM
```bash
az vm create \
  --resource-group <your-resource-group> \
  --name devops-vm \
  --image Ubuntu2204 \
  --size Standard_B2s \
  --admin-username azureuser \
  --generate-ssh-keys \
  --storage-sku Standard_LRS \
  --os-disk-size-gb 30
```

### Step 3 — Verify VM is running
```bash
az vm show \
  --resource-group <your-resource-group> \
  --name devops-vm \
  --show-details \
  --query powerState \
  --output tsv
```
> ✅ Expected output: `VM running`

### Step 4 — Start VM if not running (optional)
```bash
az vm start \
  --resource-group <your-resource-group> \
  --name devops-vm
```

### Step 5 — SSH into the VM
```bash
ssh azureuser@<PUBLIC_IP>
```
> The public IP is shown in the JSON output after Step 2 completes.

---

##  Project Structure

```
azure-vm-devops/
│
├── README.md          # This file
└── create-vm.sh       # Shell script to automate VM creation
```

---

##  Automation Script

Save the following as `create-vm.sh` and run it directly:

```bash
#!/bin/bash

RESOURCE_GROUP="<your-resource-group>"
VM_NAME="devops-vm"
IMAGE="Ubuntu2204"
SIZE="Standard_B2s"
ADMIN_USER="azureuser"
STORAGE_SKU="Standard_LRS"
DISK_SIZE=30

echo "Creating Azure VM: $VM_NAME ..."

az vm create \
  --resource-group $RESOURCE_GROUP \
  --name $VM_NAME \
  --image $IMAGE \
  --size $SIZE \
  --admin-username $ADMIN_USER \
  --generate-ssh-keys \
  --storage-sku $STORAGE_SKU \
  --os-disk-size-gb $DISK_SIZE

echo "Verifying VM state..."
az vm show \
  --resource-group $RESOURCE_GROUP \
  --name $VM_NAME \
  --show-details \
  --query powerState \
  --output tsv
```

---

## 🧠 What I Learned

- How to create Azure VMs entirely from the CLI without portal access
- How to use `--generate-ssh-keys` for secure, passwordless access
- How to verify VM power state using `az vm show`
- How Azure resource groups organize cloud resources

---

## 🔗 References

- [Azure CLI Documentation](https://learn.microsoft.com/en-us/cli/azure/)
- [az vm create reference](https://learn.microsoft.com/en-us/cli/azure/vm#az-vm-create)
- [Ubuntu on Azure](https://learn.microsoft.com/en-us/azure/virtual-machines/linux/quick-create-cli)

---

## 👤 BasavaBM

Built in public as part of my **#DevOps learning journey** 🚀

---

⭐ If this helped you, give it a star!
