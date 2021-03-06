---
title: "Définir une page d’accueil personnalisée pour les applications publiées à l’aide du proxy d’application Azure AD | Microsoft Docs"
description: "Couvre les bases sur les connecteurs de proxy d’application Azure AD."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/25/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 28f100276511c1ae978466870ff48f885dd53c28
ms.lasthandoff: 04/03/2017


---

# <a name="set-a-custom-home-page-for-published-apps-by-using-azure-ad-application-proxy"></a>Définir une page d’accueil personnalisée pour les applications publiées à l’aide du proxy d’application Azure AD

Cet article explique comment configurer des applications pour diriger les utilisateurs vers une page d’accueil personnalisée lorsqu’ils accèdent aux applications à partir du volet d’accès Azure Active Directory (Azure AD) et du lanceur d’applications Office 365.

>[!NOTE]
>Le proxy d’application est une fonctionnalité qui n’est disponible que si vous effectuez une mise à niveau vers l’édition Premium ou De base d’Azure Active Directory. Pour plus d’informations, consultez la page [Éditions d’Azure Active Directory](active-directory-editions.md).

Avec le module Azure AD Powershell, vous pouvez définir des URL de page d’accueil personnalisées pour des instances lorsque vous souhaitez que les utilisateurs de l’application arrivent sur l’une de ses pages spécifique (par exemple, *https://expenseApp-contoso.msappproxy.net/login/login.aspx*).

>[!NOTE]
>Si vous octroyez aux utilisateurs un accès aux applications publiées, celles-ci sont affichées dans le [volet d’accès Azure AD](active-directory-saas-access-panel-introduction.md) et le [lanceur d’applications Office 365](https://blogs.office.com/2016/09/27/introducing-the-new-office-365-app-launcher).

Lorsque les utilisateurs lancent l’application, ils sont dirigés par défaut vers l’URL du domaine racine de l’application publiée. La page d’arrivée est généralement définie comme URL de page d’accueil. Par exemple, pour l’application principale, http://ExpenseApp, l’URL publiée est la suivante : *https://expenseApp-contoso.msappproxy.net*. Par défaut, l’URL de page d’accueil est définie comme suit : *https://expenseApp-contoso.msappproxy.net*.

## <a name="determine-the-home-page-url"></a>Déterminer l’URL de page d’accueil

Avant de définir l’URL de page d’accueil, n’oubliez pas les éléments suivants :

* Vérifiez que le chemin d’accès que vous spécifiez est un chemin d’accès de sous-domaine de l’URL du domaine racine.

 Par exemple, si l’application publiée est accessible à partir d’une URL de domaine racine (par exemple, https://intranet-contoso.msappproxy.net/), l’URL de page d’accueil que vous configurez doit commencer par https://intranet-contoso.msappproxy.net/.

* Si l’URL de domaine racine est https://apps.contoso.com/app1/ par exemple, l’URL de page d’accueil que vous configurez doit commencer par https://apps.contoso.com/app1/.

* Si vous apportez une modification à l’application publiée, ce changement peut réinitialiser la valeur de l’URL de page d’accueil. Par conséquent, lorsque vous décidez de mettre à jour l’application, vous devez revérifier et, si nécessaire, mettre à jour l’URL de page d’accueil.

Dans la section suivante, vous découvrirez la configuration d’une URL de page d’accueil personnalisée de l’application publiée. 

## <a name="install-the-azure-ad-powershell-module"></a>Installer le module Azure AD PowerShell

Avant de définir une URL de page d’accueil personnalisée à l’aide de PowerShell, installez un package non standard du module Azure AD PowerShell. Vous pouvez télécharger le package à partir de [PowerShell Gallery](https://www.powershellgallery.com/packages/AzureAD/1.1.23.0), qui utilise le point de terminaison de l’API Graph. 

Pour installer le package à l’aide de PowerShell, procédez comme suit :

1. Ouvrez une fenêtre PowerShell standard, puis exécutez la commande suivante :

    ```
     Install-Module -Name AzureAD -RequiredVersion 1.1.23.0
    ```
    Si vous l’exécutez en tant que non-administrateur, utilisez l’option **-scope currentuser**.
2. Pendant l’installation, sélectionnez **Y** pour installer deux packages depuis Nuget.org. Les deux packages sont requis. 

## <a name="set-a-custom-home-page-url-by-using-the-azure-ad-powershell-module"></a>Définir une URL de page d’accueil personnalisée à l’aide du module Azure AD PowerShell

Maintenant que vous avez installé le module Azure AD PowerShell, vous êtes prêt à définir l’URL de page d’accueil. Pour ce faire, suivez les instructions indiquées dans les deux sections ci-dessous.

### <a name="step-1-find-the-objectid-of-the-app"></a>Étape 1 : rechercher l’ObjectID de l’application

Obtenez l’ObjectID de l’application, puis recherchez l’application en fonction de sa page d’accueil.

1. Ouvrez PowerShell, puis importez le module Azure AD à l’aide de la commande suivante :

    ```
    Import-Module AzureAD
    ```

2. Connectez-vous au module Azure AD à l’aide de l’applet de commande suivante, puis suivez les instructions à l’écran. Veillez à vous connecter en tant qu’administrateur client.

    ```
    Connect-AzureAD
    ```
3. Utilisez cette applet de commande pour rechercher l’application en fonction de la page d’accueil contenant *sharepoint-iddemo*. Pour modifier l’application, remplacez la valeur suivante par la valeur qui fonctionne pour l’application.

    ```
    Get-AzureADApplications | where { $_.Homepage -like “*sharepoint-iddemo*” } | fl DisplayName, Homepage, ObjectID
    ```
4. Vous devez obtenir un résultat semblable à celui qui est illustré ici. Copiez le GUID (ObjectID) afin de pouvoir l’utiliser dans « Étape 2 : mettre à jour l’URL de page d’accueil ».

    ```
    DisplayName : SharePoint
    Homepage    : https://sharepoint-iddemo.msappproxy.net/
    ObjectId    : 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4
    ```

### <a name="step-2-update-the-home-page-url"></a>Étape 2 : mettre à jour de l’URL de page d’accueil

Dans le module PowerShell que vous avez utilisé dans « Étape 1 : rechercher l’ObjectID de l’application », effectuez les opérations suivantes :

1. Vérifiez que vous avez la bonne application et remplacez *8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4* par le GUID (ObjectID) que vous avez copié à l’étape précédente.

    ```
    Get-AzureADApplication -AppObjectId 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4.
    ```

 Maintenant que vous avez vérifié l’application, vous êtes prêt à mettre à jour la page d’accueil comme suit.

2. Créez un objet d’application vide pour conserver les modifications que vous souhaitez apporter.  

 >[!NOTE]
 >Il s’agit uniquement d’une variable pour contenir les valeurs que vous souhaitez mettre à jour, donc rien n’a réellement été créé.

    ```
    $appnew = New-Object “Microsoft.Open.AzureAD.Model.Application”
    ```

3. Définissez l’URL de page d’accueil sur la valeur souhaitée. La valeur doit correspondre à un chemin d’accès de sous-domaine de l’application publiée. Par exemple, si vous modifiez l’URL de page d’accueil *https://sharepoint-iddemo.msappproxy.net/* et la remplacez par *https://sharepoint-iddemo.msappproxy.net/hybrid/*, les utilisateurs de l’application accèderont directement à la page d’accueil personnalisée.

    ```
    $appnew.Homepage = “https://sharepoint-iddemo.msappproxy.net/hybrid/”
    ```
4. Procédez à la mise à jour à l’aide du GUID (ObjectID) que vous avez copié dans « Étape 1 : rechercher l’ObjectID de l’application ».

    ```
    Set-AzureADApplication -AppObjectId 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4 - Application $appnew
    ```
5. Pour vérifier la réussite de la modification, redémarrez l’application.

    ```
    Get-AzureADApplication -AppObjectId 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4
    ```

>[!NOTE]
>Les modifications que vous apportez à l’application peuvent réinitialiser l’URL de page d’accueil. Le cas échéant, répétez la procédure indiquée dans « Étape 2 : mettre à jour l’URL de page d’accueil ».

## <a name="next-steps"></a>Étapes suivantes

- [Activer l’accès distant pour SharePoint avec le proxy d’application Azure AD](application-proxy-enable-remote-access-sharepoint.md)
- [Activer le proxy d’application dans le portail Azure](active-directory-application-proxy-enable.md)

