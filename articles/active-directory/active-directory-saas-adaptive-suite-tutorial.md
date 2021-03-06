---
title: "Didacticiel : Intégration d’Azure Active Directory à Adaptative Suite | Microsoft Docs"
description: "Apprenez à utiliser Adaptive Suite avec Azure Active Directory pour activer l’authentification unique, l’approvisionnement automatisé et bien plus encore !"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 13af9d00-116a-41b8-8ca0-4870b31e224c
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/12/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 9e83f60701158f791811ce368ee3541358215071
ms.openlocfilehash: aecd4ebb1e905c7341bd02d8db451eb82d64604b
ms.lasthandoff: 02/17/2017


---
# <a name="tutorial-azure-active-directory-integration-with-adaptive-suite"></a>Didacticiel : Intégration d’Azure Active Directory à Adaptative Suite
L’objectif de ce didacticiel est de montrer comment intégrer Azure et Adaptive Suite.  

Le scénario décrit dans ce didacticiel part du principe que vous disposez des éléments suivants :

* Un abonnement Azure valide
* Un locataire Adaptive Suite

À l’issue de ce didacticiel, les utilisateurs Azure AD que vous avez affectés à Adaptive Suite pourront s’authentifier de manière unique dans l’application sur votre site d’entreprise Adaptive Suite (connexion initiée par le fournisseur du service) ou en s’appuyant sur la [Présentation du volet d’accès](active-directory-saas-access-panel-introduction.md).

Le scénario décrit dans ce didacticiel se compose des blocs de construction suivants :

* Activation de l’intégration d’application pour Adaptive Suite
* Configuration de l’authentification unique (SSO)
* Configuration de l'approvisionnement des utilisateurs
* Affectation d’utilisateurs

![Scénario](./media/active-directory-saas-adaptive-suite-tutorial/IC805637.png "Scénario")

## <a name="enable-the-application-integration-for-adaptive-suite"></a>Activer l’intégration d’applications pour Adaptive Suite
Cette section décrit l’activation de l’intégration d’application pour Adaptive Suite.

**Pour activer l’intégration d’applications pour Adaptive Suite, suivez les étapes ci-dessous :**

1. Dans le volet de navigation gauche du portail Azure Classic, cliquez sur **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-adaptive-suite-tutorial/IC700993.png "Active Directory")
2. Dans la liste **Annuaire** , sélectionnez l'annuaire pour lequel vous voulez activer l'intégration d'annuaire.
3. Pour ouvrir la vue des applications, dans la vue d'annuaire, cliquez sur **Applications** dans le menu du haut.
   
   ![Applications](./media/active-directory-saas-adaptive-suite-tutorial/IC700994.png "Applications")
4. Cliquez sur **Ajouter** en bas de la page.
   
   ![Ajouter une application](./media/active-directory-saas-adaptive-suite-tutorial/IC749321.png "Ajouter une application")
5. Dans la boîte de dialogue **Que voulez-vous faire ?**, cliquez sur **Ajouter une application à partir de la galerie**.
   
   ![Ajouter une application à partir de la galerie](./media/active-directory-saas-adaptive-suite-tutorial/IC749322.png "Ajouter une application à partir de la galerie")
6. Dans la **zone de recherche**, tapez **Adaptive Suite**.
   
   ![Galerie d’applications](./media/active-directory-saas-adaptive-suite-tutorial/IC805638.png "Galerie d’applications")
7. Dans le volet de résultats, sélectionnez **Adaptive Suite**, puis cliquez sur **Terminer** pour ajouter l’application.
   
   ![Adaptive Suite](./media/active-directory-saas-adaptive-suite-tutorial/IC805639.png "Adaptive Suite")
   
## <a name="configure-single-sign-on"></a>Configurer l’authentification unique

Cette section explique comment permettre aux utilisateurs de s’authentifier sur Adaptive Suite avec leur compte Azure AD en utilisant la fédération basée sur le protocole SAML.

La configuration de l’authentification unique pour Adaptive Suite nécessite de récupérer une valeur d’empreinte numérique dans un certificat. Si cette procédure ne vous est pas familière, consultez [Comment récupérer la valeur d’empreinte numérique d’un certificat](http://youtu.be/YKQF266SAxI).

**Pour configurer l’authentification unique, procédez comme suit :**

1. Sur la page d’intégration d’applications **Adaptive Suite** du Portail Azure Classic, cliquez sur **Configurer l’authentification unique** pour ouvrir la boîte de dialogue **Configurer l’authentification unique**.
   
   ![Configurer l’authentification unique](./media/active-directory-saas-adaptive-suite-tutorial/IC805640.png "Configurer l’authentification unique")
2. Dans la page **Comment voulez-vous que les utilisateurs se connectent à Adaptive Suite ?**, sélectionnez **Authentification unique Microsoft Azure AD**, puis cliquez sur **Suivant**.
   
   ![Configurer l’authentification unique](./media/active-directory-saas-adaptive-suite-tutorial/IC805641.png "Configurer l’authentification unique")
3. Dans la page **Configurer l’URL de l’application**, dans la zone de texte **URL de réponse**, tapez votre URL au format « *https://login.adaptiveinsights.com:443/samlsso/RlJFRVRSSUFMMTI3MTE=* », puis cliquez sur **Suivant**.
   
>[!NOTE]
> Vous pouvez obtenir cette valeur dans la page **SAML SSO Settings** d’Adaptive Suite.
>  
   
  ![Configurer les paramètres d’application](./media/active-directory-saas-adaptive-suite-tutorial/IC805642.png "Configurer les paramètres d’application")
4. Dans la page **Configurer l’authentification unique sur Adaptive Suite**, cliquez sur **Télécharger le certificat**, puis enregistrez le fichier de certificat localement sur votre ordinateur.
   
  ![Configurer l’authentification unique](./media/active-directory-saas-adaptive-suite-tutorial/IC805643.png "Configurer l’authentification unique")
5. Dans une autre fenêtre de navigateur web, connectez-vous à votre site d’entreprise Adaptive Suite en tant qu’administrateur.
6. Accédez à **Admin**.
   
  ![Administrateur](./media/active-directory-saas-adaptive-suite-tutorial/IC805644.png "Administrateur")
7. Dans la section **Users and Roles**, cliquez sur **Manage SAML SSO Settings**.
   
  ![Gérer les paramètres d’authentification unique de SAML](./media/active-directory-saas-adaptive-suite-tutorial/IC805645.png "Gérer les paramètres d’authentification unique de SAML")
8. Dans la page **SAML SSO Settings** , procédez comme suit :
   
  ![Paramètres d’authentification unique de SAML](./media/active-directory-saas-adaptive-suite-tutorial/IC805646.png "Paramètres d’authentification unique de SAML")
   
  1. Dans la zone de texte **Identity provider name** , attribuez un nom à votre configuration.
  2. Dans le portail Azure Classic, dans la page **Configurer l’authentification unique sur Adaptive Suite** de la boîte de dialogue, copiez la valeur **ID d’identité**, puis collez-la dans la zone de texte **Identity provider Entity ID**.
  3. Dans le portail Azure Classic, dans la page **Configurer l’authentification unique sur Adaptive Suite**, copiez la valeur **URL SSO SAML**, puis collez-la dans la zone de texte **Identity provider SSO URL**.
  4. Dans le portail Azure Classic, dans la page **Configurer l’authentification unique sur Adaptive Suite**, copiez la valeur **URL SSO SAML**, puis collez-la dans la zone de texte **Custom logout URL**.
  5. Pour charger votre certificat téléchargé, cliquez sur **Choisir un fichier**.
  6. Sélectionnez les options suivantes :
    * Pour **SAML user id**, sélectionnez **User’s Adaptive Insights user name**.
    * Pour **SAML user id location**, sélectionnez **User id in NameID of Subject**.
    * Pour **SAML NameID format**, sélectionnez **Email address**.
    * Pour **Enable SAML**, sélectionnez **Allow SAML SSO and direct Adaptive Insights login**.
   7. Cliquez sur **Save**.
9. Dans le portail Azure Classic, sélectionnez la confirmation de la configuration de l’authentification unique, puis cliquez sur **Terminer** pour fermer la boîte de dialogue **Configurer l’authentification unique**.
   
   ![Configurer l’authentification unique](./media/active-directory-saas-adaptive-suite-tutorial/IC805647.png "Configurer l’authentification unique")
   
## <a name="configure-user-provisioning"></a>Configurer l'approvisionnement de l'utilisateur

Pour permettre aux utilisateurs Azure AD de se connecter à Adaptive Suite, vous devez les approvisionner dans Adaptive Suite.  

* Dans le cas d’Adaptive Suite, cet approvisionnement est une tâche manuelle.

**Pour configurer l’approvisionnement de l’utilisateur, suivez les étapes ci-dessous :** 

1. Connectez-vous à votre site d’entreprise **Adaptive Suite** en tant qu’administrateur.
2. Accédez à **Admin**.
   
   ![Administrateur](./media/active-directory-saas-adaptive-suite-tutorial/IC805644.png "Administrateur")
3. Dans la section **Users and Roles**, cliquez sur **Add User**.
   
   ![Ajouter un utilisateur](./media/active-directory-saas-adaptive-suite-tutorial/IC805648.png "Ajouter un utilisateur")
4. Dans la section **New User** , procédez comme suit :
   
   ![Envoyer](./media/active-directory-saas-adaptive-suite-tutorial/IC805649.png "Envoyer")   
  1. Tapez le nom, l’identifiant de connexion, l’adresse de messagerie et le mot de passe de l’utilisateur Azure Active Directory valide que vous souhaitez renseigner dans les zones de texte correspondantes, à savoir, **Name**, **Login**, **Email** et **Password**.
  2. Sélectionnez un **rôle**.
  3. Cliquez sur **Submit**.

>[!NOTE]
>Vous pouvez utiliser n’importe quel autre outil ou API de création de compte d’utilisateur Adaptive Suite fourni par ce service pour approvisionner des comptes d’utilisateur Azure Active Directory.
>  

## <a name="assign-users"></a>Affecter des utilisateurs
Pour tester votre configuration, vous devez autoriser les utilisateurs d’Azure AD concernés à accéder à votre application.

**Pour affecter des utilisateurs à Adaptive Suite, suivez les étapes ci-dessous :**

1. Dans le portail Azure Classic, créez un compte de test.
2. Sur la page d’intégration d’applications **Adaptive Suite**, cliquez sur **Affecter des utilisateurs**.
   
   ![Affecter des utilisateurs](./media/active-directory-saas-adaptive-suite-tutorial/IC805650.png "Affecter des utilisateurs")
3. Sélectionnez votre utilisateur de test, cliquez sur **Affecter**, puis sur **Oui** pour confirmer votre affectation.
   
   ![Oui](./media/active-directory-saas-adaptive-suite-tutorial/IC767830.png "Oui")

Si vous souhaitez tester vos paramètres d’authentification unique, ouvrez le volet d’accès. Pour plus d'informations sur le panneau d'accès, consultez [Présentation du panneau d’accès](active-directory-saas-access-panel-introduction.md).


