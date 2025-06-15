# Azure VM Deployment & NSG Configuration (CLI-based)

This project demonstrates how to deploy a virtual machine in Microsoft Azure using the Azure CLI, configure a virtual network and subnet, and set up a Network Security Group (NSG) with specific inbound rules. Finally, all resources are cleaned up to avoid unwanted charges.

---

## 🔧 Technologies Used
- Azure CLI
- PowerShell (for command execution)
- Microsoft Azure (Free Tier)

---

## 📌 Project Steps

1. **Login to Azure CLI**
   ```bash
   az login
   ```

2. **Create a Resource Group**
   ```bash
   az group create --name RG-LAB --location westeurope
   ```

3. **Create Virtual Network and Subnet**
   ```bash
   az network vnet create --resource-group RG-LAB --name VNET-LAB --address-prefix 10.0.0.0/16 \
     --subnet-name Subnet-LAB --subnet-prefix 10.0.1.0/24
   ```

4. **Create a Network Security Group and Inbound Rule for RDP**
   ```bash
   az network nsg create --resource-group RG-LAB --name NSG-LAB
   az network nsg rule create --resource-group RG-LAB --nsg-name NSG-LAB \
     --name Allow-RDP --protocol Tcp --direction Inbound --priority 1000 \
     --source-address-prefixes '*' --source-port-ranges '*' --destination-port-ranges 3389 \
     --access Allow
   ```

5. **Create a Virtual Machine**
   ```bash
   az vm create --resource-group RG-LAB --name VM-LAB --image Win2019Datacenter \
     --admin-username azureuser --admin-password 'YourStrongP@ssw0rd!' \
     --vnet-name VNET-LAB --subnet Subnet-LAB --nsg NSG-LAB --public-ip-sku Basic
   ```

6. **Remote Desktop Connection to the VM**
   - Connect to the VM using the assigned public IP and port 3389.

7. **Clean Up Resources**
   ```bash
   az group delete --name RG-LAB --yes --no-wait
   ```

---

## 🧼 Resource Management

To avoid unexpected billing, all created resources were deleted after testing using `az group delete`.

---

## 💡 Notes

- This project was executed in the Azure free tier, and all configurations were managed using CLI.
- No sensitive information (e.g. real IPs or credentials) is shared in this repository.
