---
title: "Exemple de script Azure CLI - Créer un Cache Redis Azure Premium avec clustering | Microsoft Docs"
description: "Exemple de script Azure CLI - Créer un Cache Redis Azure Premium avec clustering"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
tags: azure-service-management
ms.assetid: 07bcceae-2521-4fe3-b88f-ed833104ddd2
ms.service: cache-redis
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 04/14/2017
ms.author: sdanie
translationtype: Human Translation
ms.sourcegitcommit: e851a3e1b0598345dc8bfdd4341eb1dfb9f6fb5d
ms.openlocfilehash: 87d0fe4c3eaa8f7b75343a36a069ecdac8241d74
ms.lasthandoff: 04/15/2017

---

# <a name="create-a-premium-azure-redis-cache-with-clustering"></a>Créer un Cache Redis Azure Premium avec clustering

Ce scénario explique comment créer un Cache Redis Azure Premium de 6 Go avec clustering activé et deux partitions.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Exemple de script

[!code-azurecli[main](../../../cli_scripts/redis-cache/create-premium-cache-cluster/create-premium-cache-cluster.sh "Cache Redis Azure")]

[!INCLUDE [cli-script-clean-up](../../../includes/redis-cli-script-clean-up.md)]

## <a name="script-explanation"></a>Explication du script

Ce script utilise les commandes suivantes pour créer un groupe de ressources et un Cache Redis Premium avec clustering activé. Chaque commande du tableau renvoie à une documentation spécifique.

| Commande | Remarques |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#create) | Crée un groupe de ressources dans lequel toutes les ressources sont stockées. |
| [az redis create](https://docs.microsoft.com/cli/azure/redis#create) | Créez une instance de Cache Redis. |


## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur l’interface Azure CLI, consultez la [documentation relative à l’interface Azure CLI](https://docs.microsoft.com/cli/azure/overview).

Pour des exemples supplémentaires de scripts CLI de Cache Redis Azure, voir la [documentation du Cache Redis Azure](../cli-samples.md).
