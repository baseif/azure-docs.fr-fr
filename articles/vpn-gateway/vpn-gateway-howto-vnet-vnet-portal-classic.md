---
title: "Créer une connexion entre des réseaux virtuels (classiques) : portail Azure | Microsoft Docs"
description: "Comment interconnecter des réseaux virtuels Azure à l’aide de PowerShell et du portail Azure Classic."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/11/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: 4448b7209fc777c2aeebe696b7bed87537a4585b
ms.lasthandoff: 04/12/2017


---
# <a name="configure-a-vnet-to-vnet-connection-classic"></a>Configurer une connexion de réseau virtuel à réseau virtuel (classique)

La connexion entre deux réseaux virtuels est semblable à la connexion d’un réseau virtuel à un emplacement de site local. Les deux types de connectivité font appel à une passerelle VPN pour offrir un tunnel sécurisé utilisant Ipsec/IKE. Vous pouvez même combiner une communication de réseau virtuel à réseau virtuel avec des configurations de connexion multi-sites. Vous établissez ainsi des topologies réseau qui combinent une connectivité entre différents locaux et une connectivité entre différents réseaux virtuels.


![Diagramme v2v](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/v2vrmps.png)

Cet article vous guide tout au long de la procédure de création et de connexion de plusieurs réseaux virtuels à l’aide du modèle de déploiement classique. Les étapes suivantes utilisent le portail Azure pour créer des réseaux virtuels et des passerelles, et PowerShell pour configurer la connexion de réseau virtuel à réseau virtuel. Vous ne pouvez pas configurer la connexion dans le portail.

[!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] Si vous souhaitez créer une interconnexion de réseaux virtuels à l’aide d’un modèle de déploiement différent, entre différents modèles de déploiement ou au moyen d’un autre outil de déploiement, vous pouvez sélectionner une option dans la liste déroulante d’articles suivante :

> [!div class="op_single_selector"]
> * [Resource Manager - Portail Azure](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
> * [Resource Manager - PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
> * [Classic - Portail Azure](vpn-gateway-howto-vnet-vnet-portal-classic.md)
> * [Connexions entre différents modèles de déploiement - Portail Azure](vpn-gateway-connect-different-deployment-models-portal.md)
> * [Connexions entre différents modèles de déploiement - PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)
>

[!INCLUDE [vpn-gateway-vnetpeeringlink](../../includes/vpn-gateway-vnetpeeringlink-include.md)]

![Schéma de connectivité de réseau virtuel à réseau virtuel](./media/vpn-gateway-howto-vnet-vnet-portal-classic/v2vclassic.png)


## <a name="about-vnet-to-vnet-connections"></a>À propos des connexions de réseau virtuel à réseau virtuel
Dans le modèle de déploiement classique qui utilise une passerelle VPN, la connexion entre deux réseaux virtuels est similaire à la connexion d’un réseau virtuel à un emplacement de site local. Les deux types de connectivité font appel à une passerelle VPN pour offrir un tunnel sécurisé utilisant Ipsec/IKE. 

Les réseaux virtuels que vous connectez peuvent être situés dans différents abonnements et différentes régions. Vous pouvez combiner une communication de réseau virtuel à réseau virtuel avec des configurations multisites. Vous établissez ainsi des topologies réseau qui combinent une connectivité entre différents locaux et une connectivité entre différents réseaux virtuels.

### <a name="why-connect-virtual-networks"></a>Pourquoi connecter des réseaux virtuels ?
Vous pouvez décider de connecter des réseaux virtuels pour les raisons suivantes :

* **Géo-redondance et présence géographique dans plusieurs régions**
  
  * Vous pouvez configurer la géo-réplication ou la synchronisation avec une connectivité sécurisée sans passer par les points de terminaison accessibles sur Internet.
  * Avec l’Azure Load Balancer et les technologies de clustering Microsoft ou tierces, vous pouvez configurer une charge de travail hautement disponible avec la géoredondance dans plusieurs régions Azure. Vous pouvez par exemple configurer SQL Always On avec des groupes de disponibilité répartis dans différentes régions Azure.
* **Applications multiniveaux régionales avec une forte limite d’isolement**
  
  * Dans la même région, vous pouvez configurer des applications multiniveaux avec plusieurs réseaux virtuels interconnectés avec une isolation renforcée et une communication sécurisée entre les niveaux.
* **Communication interorganisationnelle entre plusieurs abonnements dans Azure**
  
  * Si vous avez plusieurs abonnements Azure, vous pouvez désormais interconnecter des charges de travail de différents abonnements en toute sécurité entre des réseaux virtuels.
  * Pour les entreprises ou prestataires de services, il est désormais possible d’activer la communication interorganisationnelle avec une technologie VPN sécurisée au sein d’Azure.

Pour plus d’informations sur les connexions de réseau virtuel à réseau virtuel, consultez la partie [Interconnexion de réseaux virtuels](#faq) à la fin de cet article.

## <a name="step1"></a>Étape 1 : planifier vos plages d’adresses IP
Il est important de choisir les plages que vous utiliserez pour configurer vos réseaux virtuels. Pour cette configuration, vous devez vous assurer que vos plages de réseau virtuel ne se chevauchent pas entre elles ou avec un réseau local auquel elles se connectent.

Le tableau suivant présente un exemple de la définition de vos réseaux virtuels. Utilisez les plages comme indication uniquement. Consignez les plages pour vos réseaux virtuels. Vous aurez besoin de ces informations pour les étapes ultérieures.

**Exemple**

| Réseau virtuel | Espace d'adressage | Région | Se connecte au site de réseau local |
|:--- |:--- |:--- |:--- |
| TestVNet1 |TestVNet1<br>(10.11.0.0/16)<br>(10.12.0.0/16) |Est des États-Unis |VNet4Local<br>(10.41.0.0/16)<br>(10.42.0.0/16) |
| TestVNet4 |TestVNet4<br>(10.41.0.0/16)<br>(10.42.0.0/16) |Ouest des États-Unis |VNet1Local<br>(10.11.0.0/16)<br>(10.12.0.0/16) |

## <a name="vnetvalues"></a>Étape 2 : créer les réseaux virtuels
Créez deux réseaux virtuels dans le [portail Azure](https://portal.azure.com). Pour découvrir comment créer des réseaux virtuels classiques, consultez [Créer un réseau virtuel (classique)](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). Si vous utilisez cet article dans le cadre d’un exercice, vous pouvez vous servir des valeurs d’exemple suivantes :

**Valeurs pour TestVNet1**

Nom : TestVNet1<br>
Espace d’adressage : 10.11.0.0/16, 10.12.0.0/16 (facultatif)<br>
Nom du sous-réseau : par défaut<br>
Plage d’adresses de sous-réseau : 10.11.0.1/24<br>
Groupe de ressources : ClassicRG<br>
Emplacement : Est des États-Unis<br>
Sous-réseau de passerelle : 10.11.1.0/27

**Valeurs pour TestVNet4**

Nom : TestVNet4<br>
Espace d’adressage : 10.41.0.0/16, 10.42.0.0/16 (facultatif)<br>
Nom du sous-réseau : par défaut<br>
Plage d’adresses de sous-réseau : 10.41.0.1/24<br>
Groupe de ressources : ClassicRG<br>
Emplacement : États-Unis de l’Ouest<br>
Sous-réseau de passerelle : 10.41.1.0/27

**Lorsque vous créez vos réseaux virtuels, tenez compte des paramètres suivants :**

* **Espaces d’adresses du réseau virtuel** : sur la page Espaces d’adresses du réseau virtuel, indiquez la plage d’adresses que vous voulez utiliser pour votre réseau virtuel. Il s’agit des adresses IP dynamiques qui seront affectées aux machines virtuelles et aux autres instances de rôle que vous déployez dans ce réseau virtuel.<br>Les espaces d’adressage que vous sélectionnez ne peuvent pas chevaucher les espaces d’adressage des autres réseaux virtuels ou les emplacements locaux auxquels ce réseau virtuel se connectera.

* **Emplacement** : lorsque vous créez un réseau virtuel, vous l’associez à un emplacement Azure (région). Par exemple, si vous souhaitez que vos machines virtuelles déployées sur votre réseau virtuel soient physiquement situées dans la région Ouest des États-Unis, sélectionnez cet emplacement. Vous ne pouvez pas modifier l’emplacement associé à votre réseau virtuel après sa création.

**Une fois vos réseaux virtuels créés, vous pouvez ajouter les paramètres suivants :**

* **Espace d’adressage** : aucun espace d’adressage supplémentaire n’est nécessaire pour cette configuration, mais vous pouvez ajouter un espace d’adressage supplémentaire après avoir créé votre réseau virtuel.

* **Sous-réseaux** : les sous-réseaux supplémentaires ne sont pas requis pour cette configuration, mais vous souhaiterez peut-être placer vos machines virtuelles dans un sous-réseau distinct de vos autres instances de rôle.

* **Serveurs DNS** : entrez le nom du serveur DNS et son adresse IP. Ce paramètre n’entraîne pas la création de serveur DNS. Il vous permet de spécifier le serveur DNS que vous souhaitez utiliser pour la résolution de noms pour ce réseau virtuel.


Dans cette section, vous allez configurer le type de connexion et le site local, puis créer la passerelle. 

## <a name="localsite"></a>Étape 3 : configurer le site local

Azure utilise les paramètres spécifiés dans chaque site de réseau local pour déterminer comment router le trafic entre les réseaux virtuels. Chaque réseau virtuel doit pointer vers le réseau local vers lequel vous souhaitez acheminer le trafic. Vous déterminez le nom à utiliser pour faire référence à chaque site de réseau local. Il est préférable d’utiliser un nom descriptif.

Par exemple, TestVNet1 se connecte à un site de réseau local que vous créez, nommé « VNet4Local ». Les paramètres de VNet4Local contiennent les préfixes d’adresse pour TestVNet4. 

Le site local de chaque réseau virtuel correspond à l’autre réseau virtuel. Notre configuration utilise les valeurs d’exemple suivantes :

| Réseau virtuel | Espace d'adressage | Région | Se connecte au site de réseau local |
|:--- |:--- |:--- |:--- |
| TestVNet1 |TestVNet1<br>(10.11.0.0/16)<br>(10.12.0.0/16) |Est des États-Unis |VNet4Local<br>(10.41.0.0/16)<br>(10.42.0.0/16) |
| TestVNet4 |TestVNet4<br>(10.41.0.0/16)<br>(10.42.0.0/16) |Ouest des États-Unis |VNet1Local<br>(10.11.0.0/16)<br>(10.12.0.0/16) |

1. Recherchez TestVNet1 dans le portail Azure. Dans la section **Connexions VPN** du panneau, cliquez sur **Passerelle**.
 
    ![Aucune passerelle](./media/vpn-gateway-howto-vnet-vnet-portal-classic/nogateway.png)
2. Sur la page **Nouvelle connexion VPN**, sélectionnez **De site à site**.
3. Cliquez sur **Site local** pour ouvrir la page Site local et configurer les paramètres.
4. Sur la page **Site local**, nommez votre site local. Dans notre exemple, nous allons nommer le site local « VNet4Local ». 
5. Comme **Adresse IP de la passerelle VPN**, vous pouvez utiliser l’adresse IP de votre choix, pourvu qu’elle soit dans un format valide. En général, vous utilisez la véritable adresse IP externe d’un périphérique VPN. Pour une configuration de réseau virtuel à réseau virtuel classique, cependant, vous allez utiliser l’adresse IP publique affectée à la passerelle de votre réseau virtuel. Étant donné que vous n’avez pas encore créé la passerelle du réseau virtuel, vous devez spécifier une adresse IP publique valide en tant qu’espace réservé.<br>Ne laissez pas ce champ vide. Il n’est pas facultatif pour cette configuration. Lors d’une étape ultérieure, vous reviendrez à ces paramètres et les configurerez avec les adresses IP de passerelle de réseau virtuel correspondantes une fois celles-ci générées par Azure. 
6. Comme **Espace d’adressage du client**, utilisez l’espace d’adressage de l’autre réseau virtuel. Reportez-vous à votre exemple de planification. Cliquez sur **OK** pour enregistrer vos paramètres et revenir au panneau **Nouvelle connexion VPN**.

    ![site local](./media/vpn-gateway-howto-vnet-vnet-portal-classic/localsite.png)

## <a name="gw"></a>Étape 4 : créer la passerelle de réseau virtuel

Chaque réseau virtuel doit disposer d’une passerelle de réseau virtuel. La passerelle de réseau virtuel achemine et chiffre le trafic.

1. Dans le panneau **Nouvelle connexion VPN**, cochez la case **Créer une passerelle immédiatement**.
2. Cliquez sur **Sous-réseau, taille et type de routage**. Sur le panneau **Configuration de la passerelle**, cliquez sur **Sous-réseau**.
3. Le nom du sous-réseau de passerelle est automatiquement renseigné avec le nom requis « GatewaySubnet ». La **Plage d’adresses** contient les adresses IP qui sont allouées aux services de passerelle VPN. Certaines configurations autorisent un sous-réseau de passerelle de /29, mais il est préférable d’utiliser un sous-réseau /28 ou /27 pour prendre en charge les configurations futures qui pourront nécessiter davantage d’adresses IP pour les services de passerelle. Dans les paramètres de notre exemple, nous utilisons 10.11.1.0/27. Ajustez l’espace d’adressage, puis cliquez sur **OK**.
4. Configurez la **Taille de la passerelle**. Ce paramètre fait référence à la [SKU de passerelle](vpn-gateway-about-vpngateways.md#gateway-skus).
5. Configurez le **Type de routage**. Le type de routage pour cette configuration doit être **dynamique**. Vous ne pourrez pas modifier le type de routage par la suite, sauf si vous désactivez la passerelle et si vous en créez une nouvelle.
6. Cliquez sur **OK**. 
7. Dans le panneau **Nouvelle connexion VPN**, cliquez sur **OK** pour commencer à créer votre passerelle de réseau virtuel. La création d’une passerelle nécessite généralement au moins 45 minutes, selon la référence SKU de passerelle sélectionnée.

## <a name="step-5---configure-testvnet4-settings"></a>Étape 5 : configurer les paramètres de TestVNet4

Répétez les étapes jusqu’à la [Création d’un site local](#localsite) et la [Création de la passerelle de réseau virtuel](#gw) pour configurer TestVNet4, en remplaçant les valeurs si cela est nécessaire. Si vous effectuez cette opération en guise d’exercice, utilisez les [valeurs d’exemple](#vnetvalues).

## <a name="step-6---update-the-local-sites"></a>Étape 6 : mettre à jour les sites locaux

Une fois vos passerelles de réseau virtuel créées pour les deux réseaux virtuels, vous devez ajuster les valeurs **d’adresse IP de passerelle VPN** des sites locaux. 

|Nom du réseau virtuel|Site connecté|Adresse IP de la passerelle|
|:--- |:--- |:--- |
|TestVNet1|VNet4Local|Adresse IP de passerelle VPN pour TestVNet4|
|TestVNet4|VNet1Local|Adresse IP de passerelle VPN pour TestVNet1|

### <a name="part-1---get-the-virtual-network-gateway-public-ip-address"></a>Partie 1 : obtenir l’adresse IP publique de passerelle de réseau virtuel

1. Recherchez votre réseau virtuel dans le portail Azure.
2. Cliquez pour ouvrir le panneau **Présentation** du réseau virtuel. Dans le panneau, sous **connexions VPN**, vous pouvez afficher l’adresse IP de votre passerelle de réseau virtuel.

    ![Adresse IP publique](./media/vpn-gateway-howto-vnet-vnet-portal-classic/publicIP.png)
3. Copiez l’adresse IP. Vous en aurez besoin dans la prochaine section.
4. Répétez ces étapes pour TestVNet4

### <a name="part-2---modify-the-local-sites"></a>Partie 2 : modifier les sites locaux

1. Recherchez votre réseau virtuel dans le portail Azure.
2. Sur le panneau **Présentation** du réseau virtuel, cliquez sur le site local.

    ![Site local créé](./media/vpn-gateway-howto-vnet-vnet-portal-classic/local.png)
3. Dans le panneau **Connexions VPN de site à site**, cliquez sur le nom du site local que vous souhaitez modifier.

    ![Ouvrir le site local](./media/vpn-gateway-howto-vnet-vnet-portal-classic/openlocal.png)
4. Cliquez sur le **site local** que vous souhaitez modifier.

    ![modifier le site](./media/vpn-gateway-howto-vnet-vnet-portal-classic/connections.png)
5. Mettez à jour **l’adresse IP de passerelle VPN** et cliquez sur **OK** pour enregistrer les paramètres.

    ![IP de la passerelle](./media/vpn-gateway-howto-vnet-vnet-portal-classic/gwupdate.png)
6. Fermez les autres panneaux.
7. Répétez ces étapes pour TestVNet4.


## <a name="step-7---retrieve-values-from-the-network-configuration-file"></a>Étape 7 : récupérer des valeurs à partir du fichier de configuration réseau

Lorsque vous créez des réseaux virtuels classiques dans le portail Azure, le nom que vous voyez ne correspond pas au nom complet que vous utilisez pour PowerShell. Par exemple, un réseau virtuel qui semble être nommé **TestVNet1** dans le portail Azure peut avoir un nom beaucoup plus long dans le fichier de configuration réseau. Le nom peut ressembler à ceci : **Group ClassicRG TestVNet1**. Lorsque vous créez vos connexions, il est important d’utiliser les valeurs que vous voyez dans le fichier de configuration réseau. 

Dans les étapes suivantes, vous allez vous connecter à votre compte Azure et télécharger et afficher le fichier de configuration réseau pour obtenir les valeurs nécessaire pour établir les connexions.

1. Téléchargez et installez la dernière version des applets de commande PowerShell Azure Service Management (SM). Pour plus d’informations, consultez la rubrique [Installation et configuration d’Azure PowerShell](/powershell/azureps-cmdlets-docs).

2. Ouvrez la console PowerShell avec des droits élevés et connectez-vous à votre compte. Utilisez l’exemple suivant pour faciliter votre connexion :

        Login-AzureRmAccount

    Vérifiez les abonnements associés au compte.

        Get-AzureRmSubscription

    Si vous avez plusieurs abonnements, sélectionnez celui que vous souhaitez utiliser.

        Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

    Utilisez l’applet de commande suivante pour ajouter votre abonnement Azure à PowerShell pour le modèle de déploiement classique.

        Add-AzureAccount

3. Exportez et affichez le fichier de configuration réseau. Créez un répertoire sur votre ordinateur, puis exportez le fichier de configuration réseau dans ce répertoire. Dans cet exemple, le fichier de configuration réseau est exporté vers **C:\AzureNet**.

         Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
4. Dans un éditeur de texte, ouvrez le fichier, puis affichez les noms de vos réseaux virtuels et de vos sites. Ces noms sont ceux que vous utiliserez lorsque vous allez créer vos connexions.<br>Les noms des réseaux virtuels sont répertoriés comme suit : **VirtualNetworkSite name =**<br>Les noms des sites sont répertoriés comme suit : **LocalNetworkSiteRef name =**

## <a name="step-8---create-the-vpn-gateway-connections"></a>Étape 8 : créer les connexions de passerelle VPN

Lorsque vous avez terminé les étapes précédentes, vous pouvez définir les clés prépartagées IPsec/IKE et créer la connexion. Cet ensemble d’étapes utilise PowerShell. Les connexions de réseau virtuel à réseau virtuel pour le modèle de déploiement classique ne peuvent pas être configurées dans le portail Azure.

Dans les exemples ci-dessous, notez que la clé partagée est exactement identique. La clé partagée doit toujours correspondre. Veillez à remplacer les valeurs de ces exemples par les noms exacts de vos réseaux virtuels et de vos sites de réseau local.

1. Créez la connexion de TestVNet1 à TestVNet4.

        Set-AzureVNetGatewayKey -VNetName 'Group ClassicRG TestVNet1' `
        -LocalNetworkSiteName '17BE5E2C_VNet4Local' -SharedKey A1b2C3D4

2. Créez la connexion de TestVNet4 à TestVNet1.

        Set-AzureVNetGatewayKey -VNetName 'Group ClassicRG TestVNet4' `
        -LocalNetworkSiteName 'F7F7BFC7_VNet1Local' -SharedKey A1b2C3D4

3. Attendez l’initialisation des connexions. Une fois la passerelle initialisée, le système affiche l’état « Réussite ».


        Error          :
        HttpStatusCode : OK
        Id             : 
        Status         : Successful
        RequestId      : 
        StatusCode     : OK

## <a name="faq"></a>Interconnexion de réseaux virtuels pour les réseaux virtuels classiques
* Les réseaux virtuels peuvent être situés dans des abonnements identiques ou différents.
* Les réseaux virtuels peuvent être situés dans des régions (emplacements) identiques ou différentes.
* Un service cloud ou un point de terminaison d’équilibrage de charge ne peut pas s’étendre sur différents réseaux virtuels, même si ces derniers sont interconnectés.
* L’interconnexion de plusieurs réseaux virtuels ne nécessite pas d’appareils VPN.
* La connexion de réseau virtuel à réseau virtuel prend en charge la connexion de réseaux virtuels Azure. Elle ne prend pas en charge la connexion de machines virtuelles ou de services cloud qui ne sont pas déployés dans un réseau virtuel.
* La connectivité de réseau virtuel à réseau virtuel nécessite des passerelles de routage dynamiques. Les passerelles de routage statique Azure ne sont pas prises en charge.
* Une connectivité réseau virtuelle peut être utilisée en même temps que des VPN multisites. Il peut y avoir au maximum 10 tunnels VPN par passerelle VPN de réseau virtuel se connectant à d’autres réseaux virtuels ou à des sites locaux.
* Les espaces d’adressage des réseaux virtuels et sur les sites de réseau locaux ne doivent pas se chevaucher. Le chevauchement des espaces d’adressage entraîne l’échec de la création de réseaux virtuels ou du téléchargement de fichiers de configuration netcfg.
* Les tunnels redondants entre deux réseaux virtuels ne sont pas pris en charge.
* Tous les tunnels VPN du réseau virtuel, y compris les VPN de point à site, partagent la bande passante disponible pour la passerelle VPN, ainsi que le même contrat SLA concernant le temps d’activité des passerelles VPN dans Azure.
* Le trafic de réseau virtuel à réseau virtuel circule sur la dorsale principale d’Azure.




## <a name="next-steps"></a>Étapes suivantes
Vérifiez vos connexions. Voir [Vérifier une connexion de passerelle VPN](vpn-gateway-verify-connection-resource-manager.md).


