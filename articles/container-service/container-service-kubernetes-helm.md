---
title: "Déployer des conteneurs avec Helm dans Azure Kubernetes | Microsoft Docs"
description: "Utiliser l’outil d’empaquetage Helm pour déployer des conteneurs sur un cluster Kubernetes dans Azure Container Service"
services: container-service
documentationcenter: 
author: sauryadas
manager: madhana
editor: 
tags: acs, azure-container-service
keywords: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/10/2017
ms.author: saudas
ms.custom: 
translationtype: Human Translation
ms.sourcegitcommit: 7f469fb309f92b86dbf289d3a0462ba9042af48a
ms.openlocfilehash: 98d6d9ba03c3b69c17a50cff6ab57d39bd943f1a
ms.lasthandoff: 04/13/2017


---
# <a name="use-helm-to-deploy-containers-on-a-kubernetes-cluster"></a>Utilisez Helm pour déployer des conteneurs sur un cluster Kubernetes 

[Helm](https://github.com/kubernetes/helm/) est un outil d’empaquetage open source qui vous aide à installer et à gérer le cycle de vie d’applications Kubernetes. À l’instar de gestionnaires de package Linux tels que Apt-get et Yum, Helm sert à gérer les graphiques Kubernetes, qui sont des packages de ressources Kubernetes préconfigurés. Cet article montre comment utiliser Helm sur un cluster Kubernetes déployé dans Azure Container Service.

Helm comprend deux composants : 
* **Helm CLI** est un client qui s’exécute sur votre ordinateur en local ou dans le cloud  

* **Tiller** est un serveur qui s’exécute sur le cluster Kubernetes et gère le cycle de vie de vos applications Kubernetes 
 
## <a name="prerequisites"></a>Conditions préalables

* [Déployer un cluster Kubernetes](container-service-kubernetes-walkthrough.md) dans Azure Container Service

* [Installer et configurer `kubectl`](container-service-connect.md) sur un ordinateur local

* [Installer Helm](https://github.com/kubernetes/helm/blob/master/docs/install.md) sur un ordinateur local

## <a name="helm-basics"></a>Principes de base Helm 

Pour afficher des informations concernant le cluster Kubernetes sur lequel vous installez Tiller et déployez vos applications, tapez la commande suivante :

```bash
kubectl cluster-info 
```
![kubectl cluster-info](media/container-service-kubernetes-helm/clusterinfo.png)
 
Après avoir installé Helm, installez Tiller sur votre cluster Kubernetes en tapant la commande suivante :

```bash
helm init --upgrade
```
Une fois celle-ci exécutée avec succès, vous pouvez voir une sortie telle que la suivante :

![Installation de Tiller](media/container-service-kubernetes-helm/tiller-install.png)
 
 
 
 
Pour afficher tous les graphiques Helm disponibles dans le référentiel, tapez la commande suivante :

```bash 
helm search 
```

Vous pouvez voir une sortie telle que la suivante :

![Recherche dans Helm](media/container-service-kubernetes-helm/helm-search.png)
 
Pour mettre à jour les graphiques afin d’obtenir les dernières versions, tapez :

```bash 
helm repo update 
```
## <a name="deploy-an-nginx-ingress-controller-chart"></a>Déployer un graphique de contrôleur d’entrée Nginx 
 
Pour déployer un graphique de contrôleur d’entrée Nginx, tapez une seule commande :

```bash
helm install stable/nginx-ingress 
```
![Déployer un contrôleur d’entrée](media/container-service-kubernetes-helm/nginx-ingress.png)

Si vous tapez `kubectl get svc` pour afficher tous les services en cours d’exécution sur le cluster, vous voyez qu’une adresse IP est attribuée au contrôleur d’entrée. (Bien que l’affectation soit en cours, vous voyez `<pending>`. Quelques minutes sont nécessaires pour achever l’opération.) 

Une fois l’adresse IP adresse attribuée, accédez à la valeur de l’adresse IP externe pour voir le serveur principal Nginx en cours d’exécution. 
 
![Adresse IP d’entrée](media/container-service-kubernetes-helm/ingress-ip-address.png)


Pour afficher la liste des graphiques installés sur votre cluster, tapez :

```bash
helm list 
```

Vous pouvez abréger la commande en `helm ls`.
 
 
 
 
## <a name="deploy-a-mariadb-chart-and-client"></a>Déployer un graphique et un client MariaDB

Déployez à présent un graphique et un client MariaDB pour vous connecter à la base de données.

Pour déployer le graphique MariaDB, tapez la commande suivante :

```bash
helm install --name v1 stable/mariadb
```

où `--name` est une balise utilisée pour les publications.

> [!TIP]
> Si le déploiement échoue, exécutez `helm repo update` puis réessayez.
>
 
 
Pour afficher tous les graphiques sur votre cluster, tapez :

```bash 
helm list
```
 
Pour afficher tous les déploiements en cours d’exécution sur votre cluster, tapez :

```bash
kubectl get deployments 
``` 
 
 
Enfin, pour exécuter un pod pour accéder au client, tapez :

```bash
kubectl run v1-mariadb-client --rm --tty -i --image bitnami/mariadb --command -- bash  
``` 
 
 
Pour vous connecter au client, tapez la commande suivante en remplaçant `v1-mariadb` par le nom de votre déploiement :

```bash
sudo mysql –h v1-mariadb
```
 
 
Vous pouvez à présent utiliser des commandes SQL standard pour créer des bases de données, tables, etc. Par exemple, `Create DATABASE testdb1;` crée une base de données vide. 
 
 
 
## <a name="next-steps"></a>Étapes suivantes

* Pour plus d’informations sur la gestion des graphiques Kubernetes, consultez la [documentation de Helm](https://github.com/kubernetes/helm/blob/master/docs/index.md). 


