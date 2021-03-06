---
title: "Démarrage rapide avec Azure - PowerShell pour la création d’une machine virtuelle Windows | Microsoft Docs"
description: "Apprenez à créer rapidement des machines virtuelles Windows avec PowerShell"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 04/03/2017
ms.author: nepeters
translationtype: Human Translation
ms.sourcegitcommit: 0d9afb1554158a4d88b7f161c62fa51c1bf61a7d
ms.openlocfilehash: 91ef7f432d0954cc8456e5d98c48943aa0ad72a7
ms.lasthandoff: 04/12/2017


---

# <a name="create-a-windows-virtual-machine-with-powershell"></a>Créer une machine virtuelle Windows avec PowerShell

Le module Azure PowerShell est utilisé pour créer et gérer des ressources Azure à partir de la ligne de commande PowerShell ou dans des scripts. Ce guide détaille l’utilisation de PowerShell pour la création d’une machine virtuelle Azure exécutant Windows Server 2016.  Une fois le déploiement terminé, connectez-vous au serveur et installez IIS.  

Si vous n’avez pas d’abonnement Azure, créez un [compte gratuit](https://azure.microsoft.com/en-us/free/?WT.mc_id=A261C142F) avant de commencer.

Assurez-vous également que la dernière version du module Azure PowerShell est installée. Pour plus d’informations, consultez la rubrique [Installation et configuration d’Azure PowerShell](/powershell/azureps-cmdlets-docs).

## <a name="log-in-to-azure"></a>Connexion à Azure

Connectez-vous à votre abonnement Azure avec la commande `Login-AzureRmAccount` et suivez les instructions à l’écran.

```powershell
Login-AzureRmAccount
```

## <a name="create-resource-group"></a>Créer un groupe de ressources

Création d’un groupe de ressources Azure. Un groupe de ressources est un conteneur logique dans lequel les ressources Azure sont déployées et gérées. 

```powershell
New-AzureRmResourceGroup -Name myResourceGroup -Location westeurope
```

## <a name="create-networking-resources"></a>Création de ressources de mise en réseau

### <a name="create-a-virtual-network-subnet-and-a-public-ip-address"></a>Créez un réseau virtuel, un sous-réseau et une adresse IP publique. 
Ces ressources sont utilisées pour fournir une connectivité réseau à la machine virtuelle et la connecter à Internet.

```powershell
# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork -ResourceGroupName myResourceGroup -Location westeurope `
-Name MYvNET -AddressPrefix 192.168.0.0/16 -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$pip = New-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup -Location westeurope `
-AllocationMethod Static -IdleTimeoutInMinutes 4 -Name "mypublicdns$(Get-Random)"
```

### <a name="create-a-network-security-group-and-a-network-security-group-rule"></a>Créez un groupe de sécurité réseau et une règle de groupe de sécurité réseau. 
Le groupe de sécurité réseau permet sécurise la machine virtuelle avec des règles entrantes et sortantes. Dans ce cas, une règle entrante est créée pour le port 3389, qui autorise les connexions Bureau à distance entrantes.  Nous souhaitons également créer une règle entrante pour le port 80, qui autorise le trafic web entrant.

```powershell
# Create an inbound network security group rule for port 3389
$nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleRDP  -Protocol Tcp `
-Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 3389 -Access Allow

# Create an inbound network security group rule for port 80
$nsgRuleWeb = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleWWW  -Protocol Tcp `
-Direction Inbound -Priority 1001 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 80 -Access Allow

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName myResourceGroup -Location westeurope `
-Name myNetworkSecurityGroup -SecurityRules $nsgRuleRDP,$nsgRuleWeb
```

### <a name="create-a-network-card-for-the-virtual-machine"></a>Créez une carte réseau pour la machine virtuelle. 
La carte réseau connecte la machine virtuelle à un sous-réseau, un groupe de sécurité réseau et une adresse IP publique.

```powershell
# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface -Name myNic -ResourceGroupName myResourceGroup -Location westeurope `
-SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id
```

## <a name="create-virtual-machine"></a>Create virtual machine

Créez une configuration de machine virtuelle. Cette configuration inclut les paramètres qui sont utilisés lors du déploiement de la machine virtuelle, comme une image de machine virtuelle, la taille et la configuration de l’authentification. Lors de l’exécution de cette étape, vous êtes invité à saisir vos informations d’identification. Les valeurs que vous saisissez sont configurées comme le nom d’utilisateur et le mot de passe pour la machine virtuelle.

```powershell
# Define a credential object
$cred = Get-Credential

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName myVM -VMSize Standard_DS2 | `
Set-AzureRmVMOperatingSystem -Windows -ComputerName myVM -Credential $cred | `
Set-AzureRmVMSourceImage -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2016-Datacenter -Version latest | `
Add-AzureRmVMNetworkInterface -Id $nic.Id
```

Créez la machine virtuelle.

```powershell
New-AzureRmVM -ResourceGroupName myResourceGroup -Location westeurope -VM $vmConfig
```

## <a name="connect-to-virtual-machine"></a>Connexion à la machine virtuelle

Une fois le déploiement terminé, créez une connexion Bureau à distance avec la machine virtuelle.

Exécutez les commandes suivantes pour renvoyer l’adresse IP publique de la machine virtuelle.  Prenez note de cette adresse IP, afin de pouvoir vous y connecter ultérieurement avec votre navigateur de manière à tester la connectivité web.

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup | Select IpAddress
```

Utilisez la commande suivante pour créer une session Bureau à distance avec la machine virtuelle. Remplacez l’adresse IP par la valeur `publicIPAddress` de votre machine virtuelle. À l'invite, saisissez les informations d’identification que vous avez utilisées lors de la création de la machine virtuelle.

```bash 
mstsc /v:<publicIpAddress>
```

## <a name="install-iis-via-powershell"></a>Installer IIS à l’aide de PowerShell

Maintenant que vous êtes connecté à la machine virtuelle Azure, vous pouvez utiliser une seule ligne de PowerShell pour installer IIS et activer la règle de pare-feu local pour autoriser le trafic web.  Ouvrez une invite PowerShell et exécutez la commande suivante :

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

## <a name="view-the-iis-welcome-page"></a>Afficher la page d’accueil IIS

Une fois IIS installé et le port 80, ouvert sur votre machine virtuelle à partir d’Internet, vous pouvez utiliser un navigateur web pour afficher la page d’accueil IIS par défaut. Veillez à utiliser la valeur `publicIpAddress` indiquée ci-dessus pour accéder à la page par défaut. 

![Site IIS par défaut](./media/quick-create-powershell/default-iis-website.png) 

## <a name="delete-virtual-machine"></a>Suppression d'une machine virtuelle

Lorsque vous n’en avez plus besoin, la commande suivante permet de supprimer le groupe de ressources, la machine virtuelle et toutes les ressources associées.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Étapes suivantes

[Didacticiel d’installation de rôle et de configuration de pare-feu](hero-role.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[Découvrir les exemples de déploiement de machine virtuelle avec PowerShell](powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
