---
title: Quotas et limites du service Azure Batch | Microsoft Docs
description: "En savoir plus sur les contraintes, les limites et les quotas par défaut d’Azure Batch, et comment demander une augmentation de quota"
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: 28998df4-8693-431d-b6ad-974c2f8db5fb
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 6ea03adaabc1cd9e62aa91d4237481d8330704a1
ms.openlocfilehash: 4d2ba8d3a5efad1be3395aae732874e7a770f64b
ms.lasthandoff: 04/06/2017


---
# <a name="batch-service-quotas-and-limits"></a>Quotas et limites du service Batch

Comme avec d’autres services Azure, il existe des limites concernant certaines ressources associées au service Batch. La plupart de ces limites représentent des quotas par défaut appliqués par Azure au niveau de l’abonnement ou du compte. Cet article décrit ces paramètres par défaut, et comment vous pouvez demander une augmentation de ces quotas.

Gardez ces quotas à l’esprit lorsque vous concevez et mettez à l’échelle vos charges de travail Batch. Par exemple, si votre pool n’atteint pas le nombre de nœuds de calcul que vous avez spécifiés, il se peut que vous ayez atteint le quota limite de votre compte Batch.

Vous pouvez exécuter plusieurs charges de travail Batch dans un compte Batch ou répartir vos charges de travail entre plusieurs comptes Batch se trouvant dans le même abonnement mais dans différentes régions Azure.

Si vous envisagez d’exécuter des charges de travail de production dans Batch, vous devrez peut-être affecter à un ou plusieurs des quotas une valeur supérieure à la valeur par défaut. Si vous souhaitez augmenter un quota, vous pouvez ouvrir une [demande de service clientèle](#increase-a-quota) en ligne gratuitement.

> [!NOTE]
> Un quota est une limite de crédit, pas une garantie de capacité. Si vous avez des besoins de capacité à grande échelle, contactez le support Azure.
> 
> 

## <a name="resource-quotas"></a>Quotas de ressources
[!INCLUDE [azure-batch-limits](../../includes/azure-batch-limits.md)]

## <a name="other-limits"></a>Autres limites
| **Ressource** | **Limite maximale** |
| --- | --- |
| [Tâches simultanées](batch-parallel-node-tasks.md) par nœud de calcul |4 x nombre de cœurs de nœud |
| [Applications](batch-application-packages.md) par compte Batch |20 |
| Packages d’applications par application |40 |
| Taille de package d’application (individuel) |Environ 195 Go<sup>1</sup> |

<sup>1</sup> Limite Azure Storage pour la taille d’objet blob de blocs maximale

## <a name="view-batch-quotas"></a>Afficher les quotas Batch
Affichez vos quotas de compte Batch dans le [portail Azure][portal].

1. Sélectionnez **Comptes Batch** dans le portail, puis sélectionnez le compte Batch qui vous intéresse.
2. Sélectionnez **Propriétés** dans le panneau de menu du compte Batch
3. Le panneau Propriétés affiche les **quotas** actuellement appliqués au compte Batch
   
    ![Quotas de compte Batch][account_quotas]

## <a name="increase-a-quota"></a>Augmenter un quota
Suivez ces étapes pour demander une augmentation du quota à l’aide du [portail Azure][portal].

1. Sélectionnez la mosaïque **Aide + Support** dans le tableau de bord du portail, ou le point d’interrogation (**?**) dans le coin supérieur droit du portail.
2. Sélectionnez **Nouvelle demande de support** > **De base**.
3. Sur le panneau **De base** :
   
    a. **Type de problème** > **Quota**
   
    b. Sélectionnez votre abonnement.
   
    c. **Type de quota** > **Batch**
   
    d. **Plan de support** > **Support quota - Inclus**
   
    Cliquez sur **Suivant**.
4. Sur le panneau **Problème** :
   
    a. Sélectionnez un **niveau de gravité** en fonction de [l’impact sur votre activité][support_sev].
   
    b. Dans **Détails**, spécifiez chaque quota que vous souhaitez modifier, le nom du compte Batch et la nouvelle limite.
   
    Cliquez sur **Next**.
5. Sur le panneau **Informations de contact** :
   
    a. Sélectionnez une **méthode de contact préférée**.
   
    b. Vérifiez et entrez les informations de contact requises.
   
    Cliquez sur **Créer** pour envoyer la demande de support.

Une fois que vous avez envoyé votre demande de support, le support Azure vous contactera. Notez que le traitement de la demande peut prendre jusqu’à 2 jours ouvrés.

## <a name="related-topics"></a>Rubriques connexes
* [Création et gestion d’un compte Azure Batch dans le portail Azure](batch-account-create-portal.md)
* [Vue d'ensemble des fonctionnalités d'Azure Batch](batch-api-basics.md)
* [Abonnement Azure et limites, quotas et contraintes de service](../azure-subscription-service-limits.md)

[portal]: https://portal.azure.com
[portal_classic_increase]: https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/
[support_sev]: http://aka.ms/supportseverity

[account_quotas]: ./media/batch-quota-limit/accountquota_portal.PNG

