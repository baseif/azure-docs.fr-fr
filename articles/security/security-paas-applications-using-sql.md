---
title: "Sécurisation des applications mobiles et web PaaS à l’aide d’Azure SQL Database et de SQL Data Warehouse | Microsoft Docs"
description: " Découvrez les bonnes pratiques de sécurité Azure SQL Database et SQL Data Warehouse pour protéger vos applications mobiles et web PaaS. "
services: security
documentationcenter: na
author: techlake
manager: MBaldwin
editor: 
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/21/2017
ms.author: terrylan
translationtype: Human Translation
ms.sourcegitcommit: 1429bf0d06843da4743bd299e65ed2e818be199d
ms.openlocfilehash: be00c1427d57b96506ec8b0ac881b7c1bd09e4de
ms.lasthandoff: 03/22/2017


---
# <a name="securing-paas-web-and-mobile-applications-using-sql-database-and-sql-data-warehouse"></a>Sécurisation des applications mobiles et web PaaS à l’aide de SQL Database et de SQL Data Warehouse

Dans cet article, nous abordons un ensemble de bonnes pratiques de sécurité [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) et [SQL Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/) pour protéger vos applications mobiles et web PaaS. Ces bonnes pratiques sont issues de notre expérience avec Azure, mais également de celle de nos clients, comme vous.

## <a name="azure-sql-database-and-sql-data-warehouse"></a>Azure SQL Database et SQL Data Warehouse
[Azure SQL Database](../sql-database/sql-database-technical-overview.md) et [SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) fournissent un service de base de données relationnelle pour vos applications basées sur Internet. Examinons les services qui protègent vos applications et vos données lors de l’utilisation de SQL Database et de SQL Data Warehouse dans un déploiement PaaS :

- Authentification Azure Active Directory (au lieu de l'authentification SQL Server)
- Pare-feu SQL Azure
- Chiffrement transparent des données (TDE)

## <a name="best-practices"></a>Meilleures pratiques

### <a name="use-a-centralized-identity-repository-for-authentication-and-authorization"></a>Utiliser un référentiel d’identités centralisé pour l’authentification et l’autorisation

Des bases de données SQL Azure peuvent être configurées pour utiliser l'un des deux types d’authentification :

- **L’authentification SQL** utilise un nom d’utilisateur et un mot de passe. Lorsque vous avez créé un serveur logique pour votre base de données, vous avez spécifié un compte de connexion « Admin serveur », associé à un nom d’utilisateur et à un mot de passe. À l’aide de ces informations d’identification, vous pouvez vous authentifier auprès de n’importe quelle base de données sur ce serveur en tant que propriétaire de la base de données.

- **L’authentification Azure Active Directory** utilise des identités gérées par Azure Active Directory et est prise en charge pour les domaines gérés et intégrés. Pour utiliser l’authentification Azure Active Directory, vous devez créer un autre administrateur de serveur appelé « administrateur Azure AD », autorisé à gérer les groupes et utilisateurs Active Directory Azure. Cet administrateur peut également effectuer toutes les opérations d’un administrateur de serveur ordinaire.

[L’authentification Azure Active Directory](../active-directory/develop/active-directory-authentication-scenarios.md) est un mécanisme servant à se connecter aux services Azure SQL Database et SQL Data Warehouse à l’aide d’identités dans Azure Active Directory (Azure AD). Azure AD fournit une alternative à l’authentification SQL Server. Vous pouvez donc arrêter la prolifération des identités d’utilisateur sur les serveurs de base de données. L’authentification Azure AD vous permet de gérer de manière centralisée les identités des utilisateurs de base de données et d’autres services Microsoft dans un emplacement centralisé. La gestion centralisée des ID fournit un emplacement unique pour gérer les utilisateurs de la base de données et simplifie la gestion des autorisations.  

Avantages de l’utilisation de l’authentification Azure AD au lieu de l’authentification SQL sont les suivants :

- Permet une rotation du mot de passe dans un emplacement unique.
- Gère des autorisations de base de données à l'aide de groupes Azure AD externes.
- Élimine le stockage des mots de passe en activant l’authentification Windows intégrée et d'autres formes d’authentification prises en charge par Azure AD.
- Utilise les utilisateurs de base de données à relation contenant-contenu pour authentifier les identités au niveau de la base de données.
- Prend en charge l’authentification basée sur les jetons pour les applications se connectant à SQL Database.
- Prend en charge ADFS (fédération de domaine) ou l’authentification utilisateur/mot de passe native pour un répertoire Azure AD local sans synchronisation du domaine.
- Prend en charge les connexions à partir de SQL Server Management Studio qui utilisent l’authentification universelle Active Directory, notamment [Multi-Factor Authentication (MFA)](../multi-factor-authentication/multi-factor-authentication.md). MFA comprend une authentification forte avec une gamme d’options de vérification simples (appel téléphonique, SMS, cartes à puce avec code PIN ou notification d’application mobile). Pour plus d’informations, voir [Prise en charge de SSMS pour Azure AD MFA avec la base de données SQL et SQL Data Warehouse](../sql-database/sql-database-ssms-mfa-authentication.md).

Pour en savoir plus sur l'authentification Azure AD, consultez :

- [Connexion au service Base de données SQL ou SQL Data Warehouse avec l’authentification Azure Active Directory](../sql-database/sql-database-aad-authentication.md)
- [Authentification sur Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-authentication.md)
- [L’authentification basée sur le jeton prend en charge la base de données SQL Azure à l’aide de l’authentification Azure AD](https://blogs.msdn.microsoft.com/sqlsecurity/2016/02/09/token-based-authentication-support-for-azure-sql-db-using-azure-ad-auth/)

> [!NOTE]
> Pour vous assurer qu’Azure Active Directory est adapté à votre environnement, consultez [Limitations et fonctionnalités Azure AD](../sql-database/sql-database-aad-authentication.md#azure-ad-features-and-limitations), en particulier les considérations supplémentaires.
>
>

### <a name="restrict-access-based-on-ip-address"></a>Restreindre l’accès en fonction de l’adresse IP
Vous pouvez créer des règles de pare-feu qui spécifient des plages d’adresses IP acceptables. Ces règles peuvent être ciblées au niveau du serveur et de la base de données. Nous recommandons d’utiliser, dans la mesure du possible, des règles de pare-feu au niveau de la base de données pour améliorer la sécurité et renforcer la portabilité de la base de données. L'utilisation de règles de pare-feu pour les administrateurs est recommandée au niveau du serveur quand plusieurs bases de données ont les mêmes exigences d’accès alors que vous ne souhaitez pas les configurer une à une.

Les restrictions d'adresse IP source de SQL Database par défaut autorisent l’accès à partir de n’importe quelle adresse Azure (notamment d'autres abonnements et clients). Vous pouvez limiter cette option pour autoriser uniquement vos adresses IP pour accéder à l’instance. Même avec votre pare-feu SQL et les restrictions d’adresse IP, l’authentification forte est toujours nécessaire. Consultez les recommandations faites plus haut dans cet article.

Pour en savoir plus sur le pare-feu SQL Azure et les restrictions d'adresse IP, consultez :

- [Contrôle d’accès à Azure SQL Database](../sql-database/sql-database-control-access.md)
- [Configuration des règles de pare-feu de la base de données SQL Azure - Vue d’ensemble](../sql-database/sql-database-firewall-configure.md)
- [Configurer une règle de pare-feu au niveau du serveur sur une base de données SQL Azure à l’aide du portail Azure](../sql-database/sql-database-configure-firewall-settings.md)

### <a name="encryption-of-data-at-rest"></a>Chiffrement des données au repos
[Transparent Data Encryption (TDE)](https://msdn.microsoft.com/library/azure/bb934049), également appelé chiffrement des données au repos, chiffre les fichiers de données SQL Server, Azure SQL Database et Azure SQL Data Warehouse. Vous pouvez prendre plusieurs précautions pour sécuriser la base de données telles que la conception d’un système sécurisé, le chiffrement de ressources confidentielles et la création d'un pare-feu autour des serveurs de base de données. Toutefois, dans un scénario où le support physique (par exemple, les lecteurs ou les bandes de sauvegarde) est volé, une personne malveillante peut restaurer ou attacher la base de données et consulter les données. Une solution consiste à chiffrer les données sensibles dans la base de données et à protéger les clés qui sont utilisées pour chiffrer les données avec un certificat. Cela empêche toute personne sans les clés d'utiliser les données, mais ce type de protection doit être planifié à l’avance.

TDE protège les données au repos, ce qui signifie les données et les fichiers journaux. Cette fonctionnalité permet de se conformer aux nombreuses lois, réglementations et directives établies dans de nombreux secteurs. Cela permet aux développeurs de logiciels de chiffrer les données à l’aide d’algorithmes de chiffrement standard dans l'industrie sans modifier les applications existantes.

TDE doit être utilisée si des réglementations spécifient explicitement ce type de chiffrement. N’oubliez pas, cependant, que cela n’arrêtera pas un pirate informatique d’utiliser le chemin d’accès normal. Vous pouvez éventuellement utiliser TDE dans le cas peu probable où vous devez éventuellement utiliser un chiffrement supplémentaire au niveau de l'application, par le chiffrement des lignes et des colonnes Azure SQL ou par le chiffrement au niveau de l'application.

Le chiffrement au niveau de l’application doit également être utilisé pour des données sélectionnées. Les problèmes de souveraineté de données peuvent être limités par le chiffrement des données avec une clé conservée dans le pays correct. Cela empêche même un transfert de données accidentel de causer un problème, car il est impossible de déchiffrer les données sans la clé, en supposant qu'un algorithme fort est utilisé (par exemple, AES 256).

Le chiffrement des lignes et colonnes fourni par Azure SQL peut être utilisé pour permettre un accès uniquement aux utilisateurs autorisés ([RBAC](../active-directory/role-based-access-built-in-roles.md)) et empêcher les utilisateurs disposant de privilèges inférieurs d'afficher des colonnes ou des lignes.

## <a name="next-steps"></a>Étapes suivantes
Dans cet article, nous avons abordé un ensemble de bonnes pratiques de sécurité SQL Database et SQL Data Warehouse pour protéger vos applications PaaS mobiles et web. Pour en savoir plus sur la sécurisation de vos déploiements PaaS, consultez :

- [Sécurisation des déploiements PaaS](security-paas-deployments.md)
- [Sécurisation des applications mobiles et web PaaS à l’aide d’Azure App Services](security-paas-applications-using-app-services.md)

