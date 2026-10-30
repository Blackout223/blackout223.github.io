---
layout: default
title: Day 3 - Create VM using Azure CLI
---

# Overview

This time there was no portal access, only the Azure CLI on the `azure-client` host. The task:

1. Create a VM named `xfusion-vm` with the Azure CLI
2. Image `Ubuntu2204`, size `Standard_B2s`
3. Admin username `azureuser`, with SSH keys generated
4. `Standard_LRS` storage, 30 GB disk, and the VM must be running afterwards

# Solution

Doing this from the CLI is quicker than clicking through the portal, and the command can be saved and reused or put in a script.

First I found the resource group the lab had already created:

```sh
az group list --output table
```

![az group list](/KodeKloud/100%20Days/images/azure-day3/01.png)

Then created the VM in one command:

```sh
az vm create \
  --resource-group kml_rg_main-320a36a631214a44 \
  --name xfusion-vm \
  --image Ubuntu2204 \
  --size Standard_B2s \
  --admin-username azureuser \
  --generate-ssh-keys \
  --storage-sku Standard_LRS \
  --os-disk-size-gb 30 \
  --public-ip-sku Standard \
  --nsg-rule SSH
```

`--generate-ssh-keys` creates a key pair in `~/.ssh` if there isn't one already, and puts the public key on the VM.

Once it finished the VM was running.

![VM running](/KodeKloud/100%20Days/images/azure-day3/02.png)

To SSH in, the private key needs the right permissions:

```sh
chmod 600 /root/.ssh/id_rsa
```

Then I got the VM's public IP:

```sh
az vm show \
  --resource-group kml_rg_main-320a36a631214a44 \
  --name xfusion-vm \
  --show-details \
  --query publicIps \
  --output tsv
```

![Public IP](/KodeKloud/100%20Days/images/azure-day3/03.png)

The IP was `172.185.157.5`, so:

```sh
ssh -i /root/.ssh/id_rsa azureuser@172.185.157.5
```

And I was logged in.

![SSH login](/KodeKloud/100%20Days/images/azure-day3/04.png)
