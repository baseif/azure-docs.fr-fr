---
title: "Azure Storage Service Encryption pour les données au repos | Microsoft Docs"
description: "La fonctionnalité Azure Storage Service Encryption permet de chiffrer votre stockage d’objets blob Azure côté service lors du stockage des données et de le déchiffrer lorsque vous récupérez les données."
services: storage
documentationcenter: .net
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: edabe3ee-688b-41e0-b34f-613ac9c3fdfd
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2016
ms.author: robinsh
translationtype: Human Translation
ms.sourcegitcommit: 988e7fe2ae9f837b661b0c11cf30a90644085e16
ms.openlocfilehash: aaada39283b19015e460768b072d0164b0d22bae
ms.lasthandoff: 04/06/2017


---
# <a name="azure-storage-service-encryption-for-data-at-rest"></a>Azure Storage Service Encryption pour les données au repos
Azure Storage Service Encryption pour les données au repos vous aide à protéger vos données pour répondre aux engagements de votre organisation en matière de sécurité et de conformité. Avec cette fonctionnalité, Azure Storage chiffre automatiquement vos données avant de les rendre persistantes dans le stockage et les déchiffre avant la récupération. La gestion du chiffrement, du déchiffrement et des clés est totalement transparente pour les utilisateurs.

Les sections suivantes fournissent des instructions détaillées sur la façon d’utiliser les fonctionnalités Storage Service Encryption et présentent les scénarios et les expériences utilisateur pris en charge.

## <a name="overview"></a>Vue d'ensemble
Azure Storage propose un ensemble complet de fonctionnalités de sécurité qui, réunies, permettent aux développeurs de créer des applications sécurisées. Les données peuvent être sécurisées en transit entre une application et Azure au moyen du [chiffrement côté client](storage-client-side-encryption.md), de HTTPs ou de SMB 3.0. La fonctionnalité Storage Service Encryption fournit un chiffrement au repos et se charge de la gestion du chiffrement, du déchiffrement et des clés de façon totalement transparente. Toutes les données sont chiffrées à l’aide du [chiffrement AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)256 bits, l’un des algorithmes de chiffrement par blocs les plus puissants disponibles.

SSE fonctionne en chiffrant les données lors leur écriture dans le Stockage Azure et peut être utilisé pour le Stockage Blob Azure et le Stockage Fichier (version préliminaire). Il fonctionne pour les éléments suivants :

* Comptes de stockage généraux et comptes de stockage Blob
* Stockage Premium et stockage Standard 
* Tous les niveaux de redondance (LRS, ZRS, GRS, RA-GRS)
* Comptes de stockage Azure Resource Manager (non Classic) 
* Toutes les régions pour le Stockage Blob. Pour le Stockage Fichier, consultez la section Disponibilité.

Storage Service Encryption : Files Preview SSE est désormais disponible pour le chiffrement des données dans le Stockage Fichier. Actuellement en mode préliminaire. Voici la liste des régions dans lesquelles SSE pour le Stockage Fichier est disponible.

Pour participer à Files Preview SSE, contactez ssediscussions@microsoft.com.

Pour plus d’informations, consultez la FAQ.

### <a name="availability-for-file-storage"></a>Disponibilité pour Stockage Fichier
Le chiffrement du service de stockage pour Stockage Fichier est actuellement disponible dans toutes les régions Azure.


Pour activer ou désactiver le chiffrement du service de stockage pour un compte de stockage, connectez-vous au [portail Azure](https://portal.azure.com) , puis sélectionnez un compte de stockage. Dans le panneau Paramètres, recherchez la section Service BLOB, comme illustré dans cette capture d’écran, puis cliquez sur Chiffrement.

![Capture d’écran du portail affichant l’option de chiffrement](./media/storage-service-encryption/image1.png)
<br/>*Figure 1 : Activer SSE pour le service BLOB (étape 1)*

![Capture d’écran du Portail affichant l’option de chiffrement](./media/storage-service-encryption/image3.png)
<br/>*Figure 2 : Activer SSE pour le service Fichier (étape 1)*

Lorsque vous avez cliqué sur le paramètre Chiffrement, vous pouvez activer ou désactiver Storage Service Encryption.

![Capture d’écran du Portail affichant les propriétés de chiffrement](./media/storage-service-encryption/image2.png)
<br/>*Figure 1.1 : Activer SSE pour le service BLOB (étape 2)*

![Capture d’écran du Portail affichant les propriétés de chiffrement](./media/storage-service-encryption/image4.png)
<br/>*Figure 2.1 : Activer SSE pour le service Fichier (étape 2)*
## <a name="encryption-scenarios"></a>Scénarios de chiffrement
Storage Service Encryption peut être activé au niveau d’un compte de stockage. Les scénarios clients suivants sont pris en charge :

* Chiffrement du Stockage Fichier et du Stockage Blob.
* Le chiffrement des comptes de stockage classiques migrés vers des comptes Resource Manager est pris en charge pour le chiffrement du service BLOB, mais pas pour le service Fichier.
* Le chiffrement pour le Stockage Fichier est pris en charge uniquement pour les comptes de stockage nouvellement créés.

SSE présente les limites suivantes :

* Le chiffrement des comptes de stockage classiques n’est pas pris en charge.
* Le chiffrement des comptes de stockage classiques migrés vers des comptes Resource Manager est pris en charge pour le chiffrement du service BLOB, mais pas pour le service Fichier.
* Le chiffrement pour le Stockage Fichier est pris en charge uniquement pour les comptes de stockage nouvellement créés.
* Données existantes : SSE chiffre uniquement les données créées après l’activation du chiffrement. Par exemple, si vous créez un compte de stockage Resource Manager sans activer le chiffrement, puis que vous téléchargez des objets blob ou des disques durs virtuels archivés dans ce compte de stockage avant d’activer SSE, ces objets blob ne sont pas chiffrés, sauf s’ils sont réécrits ou copiés.
* Prise en charge du Marketplace : activez le chiffrement des machines virtuelles créées à partir du Marketplace à l’aide du [portail Azure](https://portal.azure.com), de PowerShell et de l’interface de ligne de commande Azure. L’image de base du disque dur virtuel reste non chiffrée. Toutefois, les écritures effectuées une fois que la machine virtuelle est opérationnelle sont chiffrées.
* Les données des tables et des files d’attente ne sont pas chiffrées.

## <a name="getting-started"></a>Mise en route
### <a name="step-1-create-a-new-storage-accountstorage-create-storage-accountmd"></a>Étape 1 : [Créer un compte de stockage](storage-create-storage-account.md).
### <a name="step-2-enable-encryption"></a>Étape 2 : Activer le chiffrement.
Vous pouvez activer le chiffrement à l’aide du [portail Azure](https://portal.azure.com).

> [!NOTE]
> Si vous souhaitez activer ou désactiver la fonctionnalité Storage Service Encryption par programme sur un compte de stockage, vous pouvez utiliser [l’API REST du fournisseur de ressources de stockage Azure](https://msdn.microsoft.com/library/azure/mt163683.aspx), [la bibliothèque cliente des fournisseurs de ressources de stockage pour .NET](https://msdn.microsoft.com/library/azure/mt131037.aspx), [Azure PowerShell](/powershell/azureps-cmdlets-docs) ou [la CLI Azure](storage-azure-cli.md).
> 
> 

### <a name="step-3-copy-data-to-storage-account"></a>Étape 3 : Copier les données dans le compte de stockage
Si vous activez SSE pour le service BLOB, tous les objets blob écrits dans ce compte de stockage seront chiffrés. Tous les objets blob qui se trouvent déjà dans ce compte de stockage ne seront pas chiffrés jusqu’à ce qu’ils soient réécrits. Vous pouvez copier les données à partir d’un compte de stockage vers un autre avec SSE activé, ou encore activer SSE et copier les objets blob à partir d’un conteneur vers un autre pour vous assurer que les données précédentes sont chiffrées. Vous pouvez utiliser l’un des outils suivants pour effectuer cette opération. Le même comportement s’applique au Stockage Fichier.

#### <a name="using-azcopy"></a>Utilisation d’AzCopy
AzCopy est un utilitaire de ligne de commande Windows conçu pour copier des données depuis et vers un objet blob Microsoft Azure, un fichier ou un stockage de table en utilisant les commandes avec une performance optimale. Vous pouvez l’utiliser pour copier vos objets blob ou vos fichiers d’un compte de stockage à un autre pour lequel SSE est activé. 

Pour plus d’informations, voir [Transfert de données avec l’utilitaire de ligne de commande AzCopy](storage-use-azcopy.md).

#### <a name="using-smb"></a>Utilisation de SMB
Le stockage de fichiers Azure propose des partages de fichiers dans le cloud s’appuyant sur le protocole SMB standard. Vous pouvez monter un partage de fichiers à partir d’un client local ou dans Azure. Une fois le partage monté, des outils tels que Robocopy peuvent être utilisés pour copier les fichiers sur des partages de fichiers Azure. Pour plus d’informations, consultez la page [Guide pratique de montage d’un partage de fichiers Azure sous Windows](https://docs.microsoft.com/en-us/azure/storage/storage-dotnet-how-to-use-files#mount-the-file-share) et [Guide pratique de montage d’un partage de fichiers Azure sous Linux](https://docs.microsoft.com/en-us/azure/storage/storage-how-to-use-files-linux#mount-the-file-share).


#### <a name="using-the-storage-client-libraries"></a>Utilisation des bibliothèques clientes de stockage
Vous pouvez copier des données d’objets blob ou de fichiers vers et à partir du Stockage Blob ou entre comptes de stockage à l’aide de notre vaste choix de bibliothèques clientes de stockage, dont .NET, C++, Java, Android, Node.js, PHP, Python et Ruby.

Pour plus d’informations, consultez la section [Prise en main du stockage d’objets blob Azure à l’aide de .NET](storage-dotnet-how-to-use-blobs.md).

#### <a name="using-a-storage-explorer"></a>Utilisation d’un explorateur de stockage
Vous pouvez utiliser les explorateurs de stockage pour créer des comptes de stockage, charger et télécharger des données, afficher le contenu des objets blob et parcourir les répertoires. Vous pouvez utiliser l’un de ces explorateurs de stockage pour télécharger des objets blob dans votre compte de stockage avec le chiffrement activé. Avec certains explorateurs de stockage, vous pouvez également copier les données du compte de stockage d’objets blob existant vers un conteneur différent dans le compte de stockage ou un nouveau compte de stockage pour lequel SSE est activé.

Pour en savoir plus, consultez [Explorateurs du stockage Azure](storage-explorers.md).

### <a name="step-4-query-the-status-of-the-encrypted-data"></a>Étape 4 : Interroger l’état des données chiffrées
Une version mise à jour des bibliothèques clientes de stockage a été déployée afin que vous puissiez interroger l’état d’un objet pour déterminer s’il est chiffré ou non. Actuellement disponible uniquement pour le Stockage Blob. La prise en charge du Stockage Fichier est prévue. 

En attendant, vous pouvez appeler l’opération [Obtenir les propriétés de compte](https://msdn.microsoft.com/library/azure/mt163553.aspx) afin de vérifier que le chiffrement est activé pour votre compte de stockage ou d’afficher les propriétés du compte de stockage dans le portail Azure.

## <a name="encryption-and-decryption-workflow"></a>Flux de travail de chiffrement et de déchiffrement
Voici une brève description du flux de travail de chiffrement et de déchiffrement :

* Le client active le chiffrement sur le compte de stockage.
* Lorsque le client écrit de nouvelles données (PUT Blob, PUT Block, PUT Page, PUT File, etc.) dans le Stockage Blob ou le Stockage Fichier, chaque écriture est chiffrée à l’aide du [Chiffrement AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) 256 bits, l’un des algorithmes de chiffrement par blocs les plus puissants disponibles.
* Lorsque le client a besoin d’accéder aux données (GET Blob, etc.), celles-ci sont automatiquement déchiffrées avant d’être renvoyées à l’utilisateur.
* Si le chiffrement est désactivé, les nouvelles écritures ne sont plus chiffrées et les données chiffrées existantes restent chiffrées jusqu’à ce qu’elles soient réécrites par l’utilisateur. Lorsque le chiffrement est activé, les écritures dans le Stockage Blob ou le Stockage Fichier sont chiffrées. L’état des données ne change pas lorsque l’utilisateur bascule entre l’activation et la désactivation du chiffrement pour le compte de stockage.
* Toutes les clés de chiffrement sont stockées, chiffrées et gérées par Microsoft.

## <a name="frequently-asked-questions-about-storage-service-encryption-for-data-at-rest"></a>Forum Aux Questions Azure Storage Service Encryption pour les données au repos
**Q : Je possède déjà un compte de stockage classique. Puis-je activer SSE dessus ?**

R : Non. SSE est uniquement pris en charge sur les comptes de stockage Resource Manager.

**Q : Comment chiffrer les données de mon compte de stockage classique ?**

R : Vous pouvez créer un nouveau compte de stockage Resource Manager et copier vos données à l’aide [d’AzCopy](storage-use-azcopy.md) de votre compte de stockage classique existant vers votre nouveau compte de stockage Resource Manager. 

Si vous migrez votre compte de stockage classique vers un compte Resource Manager, les données ne seront pas chiffrées lors de la migration. Toutefois, si vous migrez le compte de stockage puis que vous activez le chiffrement, les nouvelles données écrites sur le compte de stockage seront chiffrées. Pour plus d’informations, consultez [Migration prise en charge par la plateforme de ressources IaaS de l’environnement Classic vers Resource Manager](https://azure.microsoft.com/blog/iaas-migration-classic-resource-manager/). Remarque : cela n’est pris en charge que pour le Stockage Blob. Pour la version préliminaire du Stockage Fichier, les utilisateurs doivent créer de nouveaux comptes de stockage Resource Manager.

**Q : Je possède déjà un compte de stockage Resource Manager. Puis-je activer SSE dessus ?**

R : Oui, mais seuls les objets blob nouvellement écrits seront chiffrés. Il ne revient pas en arrière et chiffre les données qui étaient déjà présentes. Cela n’est pas encore pris en charge pour la version préliminaire du Stockage Fichier.

**Q : Puis-je chiffrer les données actuelles dans un compte de stockage Resource Manager existant ?**

R : Vous pouvez activer SSE à tout moment dans un compte de stockage Resource Manager. Toutefois, les objets blob qui étaient déjà présents ne seront pas chiffrés. Pour chiffrer ces objets blob, vous pouvez les copier sur un autre nom ou un autre conteneur, puis supprimer les versions non chiffrées. Cela n’est pas encore pris en charge pour la version préliminaire du Stockage Fichier.

**Q : J’utilise Stockage Premium. Puis-je utiliser SSE ?**

R : Oui. SSE est pris en charge par le Stockage Standard et Premium. Il n’est pas encore pris en charge pour la version préliminaire du Stockage Fichier.

**Q : Si je crée un compte de stockage et que j’active SSE, puis que je crée une machine virtuelle à l’aide de ce compte de stockage, cela signifie-t-il que ma machine virtuelle est chiffrée ?**

R. : Oui. Les disques créés qui utilisent le nouveau compte de stockage sont chiffrés, à condition qu’ils aient été créés après l’activation de SSE. Si la machine virtuelle a été créée à l’aide d’Azure Marketplace, l’image de base du disque dur virtuel reste non chiffrée. Toutefois, les écritures effectuées une fois que la machine virtuelle est opérationnelle sont chiffrées.

**Q : Puis-je créer des comptes de stockage dans lesquels SSE est activé à l’aide d’Azure PowerShell et de l’interface de ligne de commande Azure ?**

R. : Oui.

**Q : Quel est le coût d’Azure Storage si SSE est activé ?**

R : Aucun coût supplémentaire n’est facturé.

**Q : Qui gère les clés de chiffrement ?**

R : Les clés sont gérées par Microsoft.

**Q : Puis-je utiliser mes propres clés de chiffrement ?**

R : Nous travaillons activement à la mise en place d’une fonctionnalité permettant aux clients d’utiliser leurs propres clés de chiffrement.

**Q : Est-il possible de révoquer l’accès aux clés de chiffrement ?**

R : Pas pour l’instant. Les clés sont entièrement gérées par Microsoft.

**Q : Est-ce que SSE est activé par défaut lorsque je crée un compte de stockage ?**

R : SSE n’étant pas activé par défaut, utilisez le portail Azure pour l’activer. Vous pouvez également l’activer par programme à l’aide de l’API REST du fournisseur de ressources de stockage.

**Q : En quoi est-ce différent d’Azure Disk Encryption ?**

R : Cette fonctionnalité permet de chiffrer les données dans Azure Blob Storage. Azure Disk Encryption permet de chiffrer les disques de système d’exploitation et de données dans les machines virtuelles IaaS. Pour plus d’informations, consultez notre [guide de sécurité sur Storage](storage-security-guide.md).

**Q : Que se passe-t-il si j’active SSE, puis que j’active Azure Disk Encryption sur les disques ?**

R: Cela fonctionne de façon transparente. Vos données sont chiffrées par les deux méthodes.

**Q : Mon compte de stockage est configuré pour être répliqué de manière géo-redondante. Si j’active SSE, ma copie redondante est-elle également chiffrée ?**

R : Oui. Toutes les copies du compte de stockage sont chiffrées et l’ensemble des options de redondance sont prises en charge : stockage localement redondant (LRS), stockage redondant dans une zone (ZRS), stockage géo-redondant (GRS) et stockage géo-redondant avec accès en lecture (RA-GRS).

**Q : Je ne peux pas activer le chiffrement sur mon compte de stockage.**

R : S’agit-il d’un compte de stockage Resource Manager ? Les comptes de stockage classiques ne sont pas pris en charge. 

**Q : SSE est-il autorisé uniquement dans des régions spécifiques ?**

R : SSE est disponible dans toutes les régions pour le Stockage Blob. Pour le Stockage Fichier, consultez la section Disponibilité. 

**Q : Comment obtenir de l’aide si je rencontre des problèmes ou que je souhaite envoyer des commentaires ?**

R : Contactez [ssediscussions@microsoft.com](mailto:ssediscussions@microsoft.com) pour les problèmes de chiffrement Storage Service Encryption.

## <a name="next-steps"></a>Étapes suivantes
Azure Storage propose un ensemble complet de fonctionnalités de sécurité qui, réunies, permettent aux développeurs de créer des applications sécurisées. Pour plus d’informations, consultez notre [guide de sécurité sur Storage](storage-security-guide.md).


