<properties
    pageTitle="Different ways to create a Linux VM | Microsoft Azure"
    description="Learn the different ways to create a Linux virtual machine on Azure, including links to tools and tutorials for each method."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="09/27/2016"
    ms.author="iainfou"/>

# <a name="different-ways-to-create-a-linux-virtual-machine-in-azure"></a>Different ways to create a Linux virtual machine in Azure

You have the flexibility in Azure to create a Linux virtual machine (VM) using tools and workflows comfortable to you. This article summarizes these differences and examples for creating your Linux VMs.


## <a name="azure-cli"></a>Azure CLI 

The Azure CLI is available across platforms via an npm package, distro-provided packages, or Docker container. You can read more about [how to install and configure the Azure CLI](../xplat-cli-install.md). The following tutorials provide examples on using the Azure CLI. Read each article for more details on the CLI quick-start commands shown:

- [Create a Linux VM from the Azure CLI for dev and test](virtual-machines-linux-quick-create-cli.md)
    - The following example creates a CoreOS VM using a public key named `azure_id_rsa.pub`:

    ```bash
    azure vm quick-create -ssh-publickey-file ~/.ssh/azure_id_rsa.pub \
        --image-urn CoreOS
    ```

- [Create a secured Linux VM using an Azure template](virtual-machines-linux-create-ssh-secured-vm-from-template.md)
    - The following example creates a VM using a template stored on GitHub:

    ```bash
    azure group create --name TestRG --location WestUS 
        --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
    ```

- [Create a complete Linux environment using the Azure CLI](virtual-machines-linux-create-cli-complete.md)
    - Includes creating a load balancer and multiple VMs in an availability set.

- [Add a disk to a Linux VM](virtual-machines-linux-add-disk.md)
    - The following example adds a 5Gb disk to an existing VM named `TestVM`:

    ```bash
    azure vm disk attach-new --resource-group TestRG --vm-name TestVM \
        --size-in-GB 5
    ```

## <a name="azure-portal"></a>Azure portal

The [Azure portal](https://portal.azure.com) allows you to quickly create a VM since there is nothing to install on your system. Use the Azure portal to create the VM:

- [Create a Linux VM using the Azure portal](virtual-machines-linux-quick-create-portal.md) 
- [Attach a disk using the Azure portal](virtual-machines-linux-attach-disk-portal.md)


## <a name="operating-system-and-image-choices"></a>Operating system and image choices
When creating a VM, you choose an image based on the operating system you want to run. Azure and its partners offer many images, some of which include applications and tools pre-installed. Or, upload one of your own images (see [the following section](#use-your-own-image)).

### <a name="azure-images"></a>Azure images
Use the `azure vm image` CLI commands to see what's available by publisher, distro release, and builds.

List available publishers as follows:

```bash
azure vm image list-publishers --location WestUS
```

List available products (offers) for a given publisher as follows:

```bash
azure vm image list-offers --location WestUS --publisher Canonical
```

List available SKUs (distro releases) of a given offer as follows:

```bash
azure vm image list-skus --location WestUS --publisher Canonical --offer UbuntuServer
```

List all available images for a given release follows:

```bash
azure vm image list --location WestUS --publisher Canonical --offer UbuntuServer --sku 16.04.0-LTS
```

For more examples on browsing and using available images, see [Navigate and select Azure virtual machine images with the Azure CLI](virtual-machines-linux-cli-ps-findimage.md).

The `azure vm quick-create` and `azure vm create` commands have aliases you can use to quickly access the more common distros and their latest releases. Using aliases is often quicker than specifying the publisher, offer, SKU, and version each time you create a VM:

| Alias     | Publisher | Offer        | SKU         | Version |
|:----------|:----------|:-------------|:------------|:--------|
| CentOS    | OpenLogic | Centos       | 7.2         | latest  |
| CoreOS    | CoreOS    | CoreOS       | Stable      | latest  |
| Debian    | credativ  | Debian       | 8           | latest  |
| openSUSE  | SUSE      | openSUSE     | 13.2        | latest  |
| RHEL      | Redhat    | RHEL         | 7.2         | latest  |
| SLES      | SLES      | SLES         | 12-SP1      | latest  |
| UbuntuLTS | Canonical | UbuntuServer | 14.04.4-LTS | latest  |

### <a name="use-your-own-image"></a>Use your own image

If you require specific customizations, you can use an image based on an existing Azure VM by *capturing* that VM. You can also upload an image created on-premises. For more information on supported distros and how to use your own images, see the following articles:

- [Azure endorsed distributions](virtual-machines-linux-endorsed-distros.md)

- [Information for non-endorsed distributions](virtual-machines-linux-create-upload-generic.md)

- [How to capture a Linux virtual machine as a Resource Manager template](virtual-machines-linux-capture-image.md).
    - Quick-start example commands to capture an existing VM:

    ```bash
    azure vm deallocate --resource-group TestRG --vm-name TestVM
    azure vm generalize --resource-group TestRG --vm-name TestVM
    azure vm capture --resource-group TestRG --vm-name TestVM --vhd-name-prefix CapturedVM
    ```

## <a name="next-steps"></a>Next steps

- Create a Linux VM from the [portal](virtual-machines-linux-quick-create-portal.md), with the [CLI](virtual-machines-linux-quick-create-cli.md), or using an [Azure Resource Manager template](virtual-machines-linux-cli-deploy-templates.md).

- After creating a Linux VM, [add a data disk](virtual-machines-linux-add-disk.md).

- Quick steps to [reset a password or SSH keys and manage users](virtual-machines-linux-using-vmaccess-extension.md)