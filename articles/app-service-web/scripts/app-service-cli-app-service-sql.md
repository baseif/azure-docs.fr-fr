---
title: "Exemple de script Azure CLI - Connecter une application web à une instance SQL Database | Microsoft Docs"
description: "Exemple de script Azure CLI - Connecter une application web à une instance SQL Database"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 7c2efdd0-f553-4038-a77a-e953021b3f77
ms.service: app-service
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 03/20/2017
ms.author: cfowler
translationtype: Human Translation
ms.sourcegitcommit: 26d460a699e31f6c19e3b282fa589ed07ce4a068
ms.openlocfilehash: c576157f2c641a916f7b595fe0d2d1c0af2c0066
ms.lasthandoff: 04/04/2017

---

# <a name="connect-a-web-app-to-a-sql-database"></a>Connecter une application web à une instance SQL Database

Dans ce scénario, vous allez apprendre à créer une instance Azure SQL Database et une application web Azure. Ensuite, vous allez lier l’instance SQL Database à l’application web par le biais des paramètres de l’application.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Exemple de script

[!code-azurecli[principal](../../../cli_scripts/app-service/connect-to-sql/connect-to-sql.sh?highlight=9-10 "SQL Database")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Explication du script

Ce script utilise les commandes suivantes pour créer un groupe de ressources, une application web, une instance SQL Database et toutes les ressources associées. Chaque commande du tableau renvoie à une documentation spécifique.

| Commande | Remarques |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#create) | Crée un groupe de ressources dans lequel toutes les ressources sont stockées. |
| [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#create) | Crée un plan App Service. Cela équivaut à une batterie de serveurs pour votre application web Azure. |
| [az appservice web create](https://docs.microsoft.com/cli/azure/appservice/web#create) | Crée une application web Azure dans le plan App Service. |
| [az sql server create](https://docs.microsoft.com/cli/azure/sql/server#create) | Crée un serveur SQL Database.  |
| [az sql db create](https://docs.microsoft.com/cli/azure/sql/db#create) | Crée une base de données avec le serveur SQL Database. |
| [az appservice web config appsetings update](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) | Crée ou met à jour un paramètre d’application pour une application web Azure. Les paramètres d’application sont exposés en tant que variables d’environnement pour votre application. |

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur l’interface Azure CLI, consultez la [documentation relative à l’interface Azure CLI](https://docs.microsoft.com/cli/azure/overview).

Vous trouverez des exemples supplémentaires de scripts CLI App Service dans la [documentation relative à Azure App Service](../app-service-cli-samples.md).
