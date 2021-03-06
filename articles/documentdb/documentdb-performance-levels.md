---
title: "Niveaux de performances dans DocumentDB | Microsoft Docs"
description: "Découvrez comment les niveaux de performances dans DocumentDB permettent de réserver le débit pour chaque collection."
services: documentdb
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 7dc21c71-47e2-4e06-aa21-e84af52866f4
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2017
ms.author: mimig
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 094729399070a64abc1aa05a9f585a0782142cbf
ms.openlocfilehash: 659d6bd63ea996af7b7b172f998884354e5d5858
ms.lasthandoff: 03/07/2017


---
# <a name="retiring-the-s1-s2-and-s3-performance-levels-in-documentdb"></a>Mise hors service des niveaux de performances S1, S2 et S3 dans DocumentDB

> [!IMPORTANT] 
> Les niveaux de performances S1, S2 et S3 abordés dans cet article vont être mis hors service et ne sont plus disponibles pour les nouvelles collections DocumentDB.
>

Cet article fournit une vue d’ensemble des niveaux de performances S1, S2 et S3 et explique comment les collections qui les utilisent seront migrées vers des collections à partition unique le 1er août 2017. Après avoir lu cet article, vous serez en mesure de répondre aux questions suivantes :

- [Pourquoi les niveaux de performances S1, S2 et S3 vont-ils être mis hors service ?](#why-retired)
- [En quoi les collections à partition unique et les collections partitionnées se différencient-elles des niveaux de performances S1, S2 et S3 ?](#compare)
- [Comment assurer un accès ininterrompu à mes données ?](#uninterrupted-access)
- [Qu’est-ce qui va changer au niveau de ma collection suite à la migration ?](#collection-change)
- [Qu’est-ce qui va changer au niveau de ma facturation suite à la migration vers des collections à partition unique ?](#billing-change)
- [Que se passe-t-il si j’ai besoin de plus de 10 Go de stockage ?](#more-storage-needed)
- [Puis-je modifier les niveaux de performances entre S1, S2 et S3 avant le 1er août 2017 ?](#change-before)
- [Comment serai-je informé de la migration de ma collection ?](#when-migrated)
- [Comment procéder à la migration des niveaux de performances S1, S2 et S3 vers des collections à partition unique par moi-même ?](#migrate-diy)
- [Quelles sont les conséquences pour moi en tant que client Contrat Entreprise ?](#ea-customer)

<a name="why-retired"></a>

## <a name="why-are-the-s1-s2-and-s3-performance-levels-being-retired"></a>Pourquoi les niveaux de performances S1, S2 et S3 vont-ils être mis hors service ?

Les niveaux de performances S1, S2 et S3 n’offrent pas la même flexibilité que les collections DocumentDB à partition unique. Avec les niveaux de performances S1, S2 et S3, le débit et la capacité de stockage étaient prédéfinis. DocumentDB offre désormais la possibilité de personnaliser votre débit et votre stockage, ce qui vous garantit une plus grande flexibilité en matière d’adaptation à l’évolution de vos besoins.

<a name="compare"></a>

## <a name="how-do-single-partition-collections-and-partitioned-collections-compare-to-the-s1-s2-s3-performance-levels"></a>En quoi les collections à partition unique et les collections partitionnées se différencient-elles des niveaux de performances S1, S2 et S3 ?

Le tableau suivant compare les options de débit et de stockage disponibles dans les collections à partition unique, les collections partitionnées et les niveaux de performances S1, S2 et S3. Voici un exemple pour la région Est des États-Unis 2 :

|   |Collection partitionnée|Collection à partition unique|S1|S2|S3|
|---|---|---|---|---|---|
|Débit maximal|Illimité|10 000 RU/s|250 RU/s|1 000 RU/s|2 500 RU/s|
|Débit minimal|2 500 RU/s|400 RU/s|250 RU/s|1 000 RU/s|2 500 RU/s|
|Stockage maximal|Illimité|10 Go|10 Go|10 Go|10 Go|
|Prix|Débit : 6 USD / 100 RU/s<br><br>Stockage :&0;,25 USD/Go|Débit : 6 USD / 100 RU/s<br><br>Stockage :&0;,25 USD/Go|25 USD|50 USD|100 USD|

Vous êtes un client Contrat Entreprise ? Si oui, voir [Quelles sont les conséquences pour moi en tant que client Contrat Entreprise ?](#ea-customer)

<a name="uninterrupted-access"></a>

## <a name="what-do-i-need-to-do-to-ensure-uninterrupted-access-to-my-data"></a>Comment assurer un accès ininterrompu à mes données ?

Vous n’avez rien à faire, DocumentDB se charge de la migration pour vous. Si vous avez une collection S1, S2 ou S3, votre collection actuelle sera migrée vers une collection à partition unique le 31 juillet 2017. 

<a name="collection-change"></a>

## <a name="how-will-my-collection-change-after-the-migration"></a>Qu’est-ce qui va changer au niveau de ma collection suite à la migration ?

Si vous avez une collection S1, elle sera migrée vers une collection à partition unique dotée d’un débit de 400 RU/s, ce qui correspond au débit le plus bas disponible pour les collections à partition unique. Toutefois, le coût d’un débit de 400 RU/s dans une collection à partition unique est approximativement identique à celui que vous payez pour votre collection S1 dotée d’un débit de 250 RU/s : vous ne payez pas pour les 150 RU/s disponibles supplémentaires.

Si vous avez une collection S2, elle sera migrée vers une collection à partition unique dotée d’un débit de 1 000 RU/s. Vous n’observerez aucun changement de niveau de débit.

Si vous avez une collection S3, elle sera migrée vers une collection à partition unique dotée d’un débit de 2 500 RU/s. Vous n’observerez aucun changement de niveau de débit.

Dans chacun de ces cas, une fois la migration de votre collection effectuée, vous serez en mesure de personnaliser votre niveau de débit, ou de le mettre à l’échelle selon vos besoins afin de fournir un accès à faible latence à vos utilisateurs. Pour modifier le niveau de débit une fois votre collection migrée, il vous suffit d’ouvrir votre compte DocumentDB dans le portail Azure, puis de cliquer sur Mettre à l’échelle, de choisir votre collection et enfin d’ajuster le niveau du débit, comme illustré dans la capture d’écran suivante :

![Mise à l’échelle du débit dans le portail Azure](./media/documentdb-performance-levels/azure-documentdb-portal-scale-throughput.png)

<a name="billing-change"></a>

## <a name="how-will-my-billing-change-after-im-migrated-to-the-single-partition-collections"></a>Qu’est-ce qui va changer au niveau de ma facturation suite à la migration vers des collections à partition unique ?

Supposons que vous avez 10 collections S1, d’une capacité de stockage de 1 Go chacune, dans la région Est des États-Unis et que vous migrez ces 10 collections S1 vers 10 collections à partition unique dotées d’un débit de 400 RU/s (niveau minimal). Votre facture se présentera comme suit si vous conservez les 10 collections à partition unique pendant un mois complet :

![Comparaison entre la tarification de 10 collections S1 et de 10 collections utilisant la tarification d’une collection à partition unique](./media/documentdb-performance-levels/documentdb-s1-vs-standard-pricing.png)

<a name="more-storage-needed"></a>

## <a name="what-if-i-need-more-than-10-gb-of-storage"></a>Que se passe-t-il si j’ai besoin de plus de 10 Go de stockage ?

Que vous disposiez d’une collection avec un niveau de performance S1, S2 ou S3 ou d’une collection à partition unique disposant de 10 Go de stockage disponible, vous pouvez utiliser l’outil de migration de données DocumentDB pour migrer vos données vers une collection partitionnée bénéficiant d’un stockage quasi illimité. Pour plus d’informations sur les avantages d’une collection partitionnée, voir [Partitionnement et mise à l’échelle dans Azure DocumentDB](documentdb-partition-data.md). Pour plus d’informations sur la façon de migrer votre collection S1, S2, S3 ou à partition unique vers une collection partitionnée, voir [Migration de collections à partition unique vers des collections partitionnées](documentdb-partition-data.md#migrating-from-single-partition). 

<a name="change-before"></a>

## <a name="can-i-change-between-the-s1-s2-and-s3-performance-levels-before-august-1-2017"></a>Puis-je modifier les niveaux de performances entre S1, S2 et S3 avant le 1er août 2017 ?

Seuls les comptes existants avec des niveaux de performances S1, S2 et S3 seront en mesure de modifier et d’ajuster la hiérarchie des niveaux de performances via le portail ou par programme. À compter du 1er août 2017, les niveaux de performances S1, S2 et S3 ne seront plus disponibles. Si vous remplacez une collection S1, S2 ou S3 par une collection à partition unique, vous ne pouvez plus revenir aux niveaux de performances S1, S2 ou S3.

<a name="when-migrated"></a>

## <a name="how-will-i-know-when-my-collection-has-migrated"></a>Comment serai-je informé de la migration de ma collection ?

La migration aura lieu le 31 juillet 2017. Si vous avez une collection qui utilise les niveaux de performances S1, S2 ou S3, l’équipe DocumentDB vous contactera par e-mail avant de lancer la migration. Une fois la migration terminée, le 1er août 2017, le portail Azure indiquera que votre collection utilise la tarification Standard.

![Confirmation de la migration de votre collection vers le niveau tarifaire Standard](./media/documentdb-performance-levels/documentdb-portal-standard-pricing-applied.png)

<a name="migrate-diy"></a>

## <a name="how-do-i-migrate-from-the-s1-s2-s3-performance-levels-to-single-partition-collections-on-my-own"></a>Comment procéder à la migration des niveaux de performances S1, S2 et S3 vers des collections à partition unique par moi-même ?

Vous pouvez effectuer la migration des niveaux de performance S1, S2 et S3 vers des collections à partition unique via le portail Azure ou par programme. Vous pouvez effectuer cette opération avant le 1er août pour tirer parti des options de débit flexibles disponibles pour les collections à partition unique. Sinon, nous nous chargerons de la migration de vos collections le 31 juillet 2017.

**Migration vers des collections à partition unique à l’aide du portail Azure**

1. Dans le [**Portail Azure**](https://portal.azure.com), cliquez sur **NoSQL (DocumentDB)**, puis sélectionnez le compte DocumentDB à modifier. 
 
    Si **NoSQL (DocumentDB)** ne figure pas dans la barre de lancement, cliquez sur >, accédez à **Bases de données**, sélectionnez **NoSQL (DocumentDB)**, puis sélectionnez le compte DocumentDB.  

2. Dans le menu de la ressource, sous **Collections**, cliquez sur **Mettre à l’échelle**, sélectionnez la collection à modifier dans la liste déroulante, puis cliquez sur **Niveau tarifaire**. Les comptes utilisant un débit prédéfini ont un niveau tarifaire de S1, S2 ou S3.  Dans le panneau **Choisir votre niveau tarifaire**, cliquez sur **Standard** pour basculer vers le débit défini par l’utilisateur, puis cliquez sur **Sélectionner** pour enregistrer vos modifications.

    ![Capture d’écran du panneau Paramètres montrant où modifier la valeur de débit](./media/documentdb-performance-levels/documentdb-change-performance-set-thoughput.png)

3. De retour dans le panneau **Mettre à l’échelle**, le **Niveau tarifaire** est défini sur **Standard** et la zone **Débit (UR/s)** est affichée avec une valeur par défaut de 400. Définissez le débit entre 400 et 10 000 [unités de requête](documentdb-request-units.md)/seconde (unités de requête/s). **L’Estimation de la facture mensuelle** au bas de la page se met à jour automatiquement pour donner une estimation du coût mensuel. 

    >[!IMPORTANT] 
    > Une fois que vous enregistrez vos modifications et que vous passez au niveau tarifaire Standard, vous ne pouvez pas restaurer les niveaux de performances S1, S2 ou S3.

4. Cliquez sur **Enregistrer** pour enregistrer vos modifications.

    Si vous déterminez que vous avez besoin d’un débit supérieur (plus de 10 000 unités de requête/s) ou d’une plus grande capacité de stockage (plus de 10 Go), vous pouvez créer une collection partitionnée. Pour migrer une collection à partition unique vers une collection partitionnée, voir [Migration de collections à partition unique vers des collections partitionnées](documentdb-partition-data.md#migrating-from-single-partition).

    > [!NOTE]
    > Le passage du niveau S1, S2 ou S3 au niveau Standard peut prendre jusqu’à 2 minutes.
    > 
    > 

**Migration vers des collections à partition unique à l’aide du Kit de développement logiciel (SDK) .NET**

Vous pouvez également modifier les niveaux de performances de vos collections via nos Kits SDK. Cette section couvre uniquement la modification du niveau de performances d’une collection à l’aide de notre [Kit de développement logiciel (SDK) .NET](https://msdn.microsoft.com/library/azure/dn948556.aspx), mais le processus est similaire pour nos autres [SDK](https://msdn.microsoft.com/library/azure/dn781482.aspx). Si vous ne connaissez pas notre Kit de développement logiciel (SDK) .NET, consultez notre [didacticiel de mise en route](documentdb-get-started.md).

Voici un extrait de code permettant de remplacer le débit de la collection par 5 000 unités de requête par seconde :
    
```C#
    //Fetch the resource to be updated
    Offer offer = client.CreateOfferQuery()
                      .Where(r => r.ResourceLink == collection.SelfLink)    
                      .AsEnumerable()
                      .SingleOrDefault();

    // Set the throughput to 5000 request units per second
    offer = new OfferV2(offer, 5000);

    //Now persist these changes to the database by replacing the original resource
    await client.ReplaceOfferAsync(offer);
```

Visitez [MSDN](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.aspx) pour afficher des exemples supplémentaires et en savoir plus sur nos méthodes d’offre :

* [**ReadOfferAsync**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.readofferasync.aspx)
* [**ReadOffersFeedAsync**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.readoffersfeedasync.aspx)
* [**ReplaceOfferAsync**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.replaceofferasync.aspx)
* [**CreateOfferQuery**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createofferquery.aspx)

<a name="ea-customer"></a>

## <a name="how-am-i-impacted-if-im-an-ea-customer"></a>Quelles sont les conséquences pour moi en tant que client Contrat Entreprise ?

Les clients Contrat Entreprise bénéficieront d’une protection en matière de tarification jusqu’à la fin de leur contrat actuel.

## <a name="next-steps"></a>Étapes suivantes
Pour en savoir plus sur la tarification et la gestion des données avec Azure DocumentDB, explorez les ressources suivantes :

1.    [Partitionnement des données dans DocumentDB](documentdb-partition-data.md). Découvrez la différence entre les collections à partition unique et les collections partitionnées et bénéficiez de conseils concernant l’implémentation d’une stratégie de partitionnement pour une mise à l’échelle en toute transparence.
2.    [Tarification de DocumentDB](https://azure.microsoft.com/pricing/details/documentdb/). Apprenez-en davantage sur le coût de l’approvisionnement du débit et de la consommation du stockage.
3.    [Unités de requête](documentdb-request-units.md). Découvrez la consommation de débit pour les différents types d’opérations, telles que les opérations de lecture, d’écriture et de requête.
4.    [Modélisation des données dans DocumentDB](documentdb-modeling-data.md). Apprenez à modéliser vos données pour DocumentDB.
