---
title: "Didacticiel : intégration d’Azure Active Directory à Tableau Server | Microsoft Docs"
description: "Découvrez comment configurer l’authentification unique entre Azure Active Directory et Tableau Server."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: c1917375-08aa-445c-a444-e22e23fa19e0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: d8845ecb86acef651fdc8d6a64ebcae82b54dcd8
ms.lasthandoff: 04/03/2017

---

# <a name="tutorial-azure-active-directory-integration-with-tableau-server"></a>Didacticiel : Intégration d’Azure Active Directory à Tableau Server
L’objectif de ce didacticiel est de vous montrer comment intégrer Tableau Server à Azure AD (Azure Active Directory).

L’intégration de Tableau Server à Azure AD vous offre les avantages suivants :

* Dans Azure AD, vous pouvez contrôler qui a accès à Tableau Server.
* Vous pouvez autoriser les utilisateurs à se connecter automatiquement à Tableau Server (via l’authentification unique) avec leur compte Azure AD
* Vous pouvez gérer vos comptes à un emplacement central avec le portail Azure Classic

Pour en savoir plus sur l’intégration des applications SaaS avec Azure AD, consultez [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Composants requis
Pour configurer l’intégration d’Azure AD à Tableau Server, vous avez besoin des éléments suivants :

* Un abonnement Azure AD
* Un abonnement Tableau Server pour lequel l’authentification unique est activée

>[!NOTE]
>Pour tester les étapes de ce didacticiel, nous déconseillons l’utilisation d’un environnement de production.
>
>

Vous devez en outre suivre les recommandations ci-dessous :

* Vous ne devez pas utiliser votre environnement de production, sauf si cela est nécessaire.
* Si vous n’avez pas d’environnement d’essai Azure AD, vous pouvez [obtenir un essai d’un mois](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Description du scénario
Ce didacticiel vise à vous permettre de tester l’authentification unique Azure AD dans un environnement de test. 

Le scénario décrit dans ce didacticiel se compose des deux sections principales suivantes :

1. Ajout de Tableau Server à partir de la galerie
2. Configuration et test de l’authentification unique Azure AD

## <a name="add-tableau-server-from-the-gallery"></a>Ajouter Tableau Server à partir de la galerie
Pour configurer l’intégration de Tableau Server à Azure AD, vous devez ajouter Tableau Server, disponible dans la galerie, à votre liste d’applications SaaS gérées.

**Pour ajouter Tableau Server à partir de la galerie, procédez comme suit :**

1. Dans le volet de navigation gauche du **portail Azure Classic**, cliquez sur **Active Directory**. 
   
    ![Active Directory][1]
2. Dans la liste **Annuaire** , sélectionnez l'annuaire pour lequel vous voulez activer l'intégration d'annuaire.
3. Pour ouvrir la vue des applications, dans la vue d'annuaire, cliquez sur **Applications** dans le menu du haut.
   
    ![Applications][2]
4. Cliquez sur **Ajouter** en bas de la page.
   
    ![Applications][3]
5. Dans la boîte de dialogue **Que voulez-vous faire ?**, cliquez sur **Ajouter une application à partir de la galerie**.
   
    ![Applications][4]
6. Dans la zone de recherche, tapez **Tableau Server**.
   
    ![Création d’un utilisateur de test Azure AD](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_01.png)
7. Dans le volet de résultats, sélectionnez **Tableau Server**, puis cliquez sur **Terminer** pour ajouter l’application.
   
    ![Sélection de l’application dans la galerie](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_02.png)

## <a name="configure-and-test-azure-ad-sso"></a>Configurer et tester l’authentification unique Azure AD
L’objectif de cette section est de vous montrer comment configurer et tester l’authentification unique Azure AD avec Tableau Server avec un utilisateur de test appelé « Britta Simon ».

Pour que l’authentification unique fonctionne, Azure AD doit savoir qui est l’utilisateur Tableau Server équivalent dans Azure AD. En d’autres termes, une relation entre un utilisateur Azure AD et l’utilisateur Tableau Server associé doit être établie.

Pour ce faire, affectez la valeur de **nom d’utilisateur** dans Azure AD comme valeur de **nom d’utilisateur** dans Tableau Server.

Pour configurer et tester l’authentification unique Azure AD avec Tableau Server, vous devez suivre les indications des sections suivantes :

1. **[Configuration de l’authentification unique Azure AD](#configuring-azure-ad-single-single-sign-on)** pour permettre à vos utilisateurs d’utiliser cette fonctionnalité.
2. **[Création d’un utilisateur de test Azure AD](#creating-an-azure-ad-test-user)** pour tester l’authentification unique Azure AD avec Britta Simon.
3. **[Création d’un utilisateur de test Tableau Server](#creating-a-tableauserver-test-user)** pour avoir un équivalent de Britta Simon dans Tableau Server lié à la représentation Azure AD associée.
4. **[Affectation de l’utilisateur de test Azure AD](#assigning-the-azure-ad-test-user)** pour permettre à Britta Simon d'utiliser l'authentification unique Azure AD.
5. **[Test de l’authentification unique](#testing-single-sign-on)** pour vérifier si la configuration fonctionne.

### <a name="configure-azure-ad-single-sign-on"></a>Configurer l’authentification unique Azure AD
L’objectif de cette section est d’activer l’authentification unique Azure AD dans le portail Azure Classic et de la configurer dans votre application Tableau Server.

L’application Tableau Server attend les assertions SAML dans un format spécifique. La capture d’écran suivante montre un exemple : 

![Configurer l’authentification unique](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_51.png) 

**Pour configurer l’authentification unique Azure AD avec Tableau Server, procédez comme suit :**

1. Dans le portail Azure Classic, dans la page d’intégration d’application **Tableau Server**, dans le menu situé en haut, cliquez sur **Attributs**.
   
    ![Configurer l’authentification unique](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_81.png) 
2. Dans la boîte de dialogue **Attributs du jeton SAML** , procédez comme suit :

   1. Cliquez sur **ajouter un attribut utilisateur** pour ouvrir la boîte de dialogue **Ajouter un attribut utilisateur**.

    ![Configurer l’authentification unique](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_82.png) 
   2. Dans la zone de texte **Nom d’attribut**, tapez **username**.
   3. Dans la liste **Valeur d’attribut**, sélectionnez **user.displayname**.
   4. Cliquez sur **Terminé**.    

3. Dans le menu situé en haut, cliquez sur **Démarrage rapide**.
   
    ![Configurer l’authentification unique](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_83.png)  
4. Cliquez sur **Configurer l’authentification unique** pour ouvrir la boîte de dialogue **Configurer l’authentification unique**.
   
    ![Configurer l’authentification unique][6] 
5. Dans la page **Comment voulez-vous que les utilisateurs se connectent à Tableau Server**, sélectionnez **Authentification unique Azure AD**, puis cliquez sur **Suivant**.
   
    ![Configurer l’authentification unique](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_03.png) 
6. Dans la page de boîte de dialogue **Configurer les paramètres d’application**, procédez comme suit et cliquez sur **Suivant** :
   
    ![Configurer l’authentification unique](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_04.png) 

   1. Dans la zone de texte **URL de connexion** , tapez l’URL de votre locataire Tableau Server. 
   2. Dans la **zone Identificateur**, copiez l’URL.
   3. Cliquez sur **Suivant**.

7. Dans la page **Configurer l’authentification unique sur Tableau Server**, procédez comme suit et cliquez sur **Suivant** :
   
    ![Configurer l’authentification unique](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_05.png) 

   1. Cliquez sur **Télécharger les métadonnées**, puis enregistrez le fichier sur votre ordinateur.
   2. Cliquez sur **Suivant**.

8. Pour que l’authentification unique soit configurée pour votre application, vous devez vous connecter à votre locataire Tableau Server en tant qu’administrateur.
   
   1. Dans Tableau Server configuration (Configuration de Tableau Server), cliquez sur l’onglet **SAML** .
  
    ![Configurer l’authentification unique](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_001.png) 
   2. Cochez la case **Use SAML for single sign-on**(Utiliser SAML pour l’authentification unique).
   3. Recherchez le fichier de métadonnées de fédération téléchargé à partir du portail Azure Classic, puis chargez-le sur **SAML Idp metadata file**(Fichier de métadonnées de l’Idp SAML).
   4. Tableau Server return URL (URL de retour Tableau Server) : URL à laquelle accèdent les utilisateurs Tableau Server, telle que http://tableau_server. L’utilisation de l’URL http://localhost n’est pas recommandée. L’utilisation d’une URL avec une barre oblique finale (par exemple, http://tableau_server/) n’est pas prise en charge. Copiez **Tableau Server return URL** (URL de retour Tableau Server) et collez-la dans la zone de texte **URL de connexion** Azure AD, comme indiqué à l’étape 3.
   5. SAML entity ID (ID d’entité SAML) : l’ID d’entité identifie de façon unique votre installation Tableau Server auprès du fournisseur d’identité. Vous pouvez à nouveau entrer l’URL Tableau Server ici, si vous le souhaitez, mais ce n’est pas obligatoire. Copiez **SAML entity ID** (ID d’entité SAML) et collez-le dans la zone de texte **IDENTIFER** (IDENTIFICATEUR) Azure AD, comme indiqué à l’étape 3.
   6. Cliquez sur **Export Metadata File** (Exporter le fichier de métadonnées) et ouvrez-le dans l’application de l’éditeur de texte. Recherchez l’URL Assertion Consumer Service avec HTTP POST et Index 0, puis copiez l’URL. Collez-la maintenant dans la zone de texte **URL de réponse** Azure AD, comme indiqué à l’étape 3. 
   7. Cliquez sur le bouton **OK** dans la page Tableau Server configuration (Configuration de Tableau Server).
   
    >[!NOTE] 
    >Si vous avez besoin d’aide pour la configuration de SAML dans Tableau Server, consultez l’article [Configurer SAML](http://onlinehelp.tableau.com/current/server/en-us/config_saml.htm).
    >

9. Dans le portail Azure Classic, sélectionnez la confirmation de la configuration de l’authentification unique, puis cliquez sur **Suivant**.
   
    ![Authentification unique Azure AD][10]
10. Sur la page **Confirmation de l’authentification unique**, cliquez sur **Terminer**. 
   
    ![Authentification unique Azure AD][11]

### <a name="create-an-azure-ad-test-user"></a>Créer un utilisateur de test Azure AD
L’objectif de cette section est de créer un utilisateur de test appelé Britta Simon dans le portail Azure Classic.

* Dans la liste Utilisateurs, sélectionnez **Britta Simon**.

![Créer un utilisateur Azure AD][20]

**Pour créer un utilisateur de test dans Azure AD, procédez comme suit :**

1. Dans le volet de navigation gauche du **portail Azure Classic**, cliquez sur **Active Directory**.
   
    ![Création d’un utilisateur de test Azure AD](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_09.png) 
2. Dans la liste **Annuaire** , sélectionnez l'annuaire pour lequel vous voulez activer l'intégration d'annuaire.
3. Pour afficher la liste des utilisateurs, dans le menu situé en haut, cliquez sur **Utilisateurs**.
   
    ![Création d’un utilisateur de test Azure AD](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_03.png) 
4. Pour ouvrir la boîte de dialogue **Ajouter un utilisateur**, cliquez sur l’option **Ajouter un utilisateur** figurant dans la barre d’outils du bas.
   
    ![Création d’un utilisateur de test Azure AD](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_04.png)
5. Sur la page de boîte de dialogue **Dites-nous en plus sur cet utilisateur** , procédez comme suit :
   
    ![Création d’un utilisateur de test Azure AD](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_05.png) 
   
   1. Dans **Type d’utilisateur**, sélectionnez **Nouvel utilisateur dans votre organisation**.
   2. Dans la zone de texte **Nom d’utilisateur**, entrez **BrittaSimon**.
   3. Cliquez sur **Next**.
6. Sur la page de boîte de dialogue **Profil utilisateur** , procédez comme suit :
   
   ![Création d’un utilisateur de test Azure AD](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_06.png) 
   
   1. Dans la zone de texte **First Name**, tapez **Britta**.  
   2. Dans la zone de texte **Last Name**, tapez **Simon**.
   3. Dans la zone de texte **Nom d’affichage**, entrez **Britta Simon**.
   4. Dans la liste **Rôle**, sélectionnez **Utilisateur**.
   5. Cliquez sur **Next**.
7. Sur la page de boîte de dialogue **Obtenir un mot de passe temporaire**, cliquez sur **créer**.
   
    ![Création d’un utilisateur de test Azure AD](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_07.png) 
8. Sur la page de boîte de dialogue **Obtenir un mot de passe temporaire** , procédez comme suit :
   
    ![Création d’un utilisateur de test Azure AD](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_08.png) 
   
   1. Notez la valeur du **Nouveau mot de passe**.
   2. Cliquez sur **Terminé**.   

### <a name="create-a-tableau-server-test-user"></a>Créer un utilisateur de test Tableau Server
L’objectif de cette section est de créer un utilisateur appelé Britta Simon dans Tableau Server. Vous devez approvisionner tous les utilisateurs dans Tableau Server. 

Le nom de l’utilisateur doit correspondre à la valeur que vous avez configurée dans l’attribut personnalisé Azure AD **username**. Avec le mappage correct, l’intégration doit fonctionner. [Configuration de l’authentification unique Azure AD](#configuring-azure-ad-single-single-sign-on).

>[!NOTE]
>Si vous avez besoin de créer un utilisateur manuellement, vous devez contacter l’administrateur Tableau Server de votre organisation.
> 
> 

### <a name="assign-the-azure-ad-test-user"></a>Affecter l’utilisateur de test Azure AD
L’objectif de cette section est de permettre à Britta Simon d’utiliser l’authentification unique (SSO) Azure en lui accordant l’accès à Tableau Server.

![Affecter des utilisateurs][200] 

**Pour affecter Britta Simon à Tableau Server, procédez comme suit :**

1. Pour ouvrir la vue des applications dans le portail Azure Classic, dans la vue de répertoire, cliquez sur **Applications** dans le menu du haut.
   
    ![Affecter des utilisateurs][201] 
2. Dans la liste des applications, sélectionnez **Tableau Server**.
   
    ![Configurer l’authentification unique](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_50.png) 
3. Dans le menu situé en haut, cliquez sur **Utilisateurs**.
   
    ![Affecter des utilisateurs][203]
4. Dans la liste Utilisateurs, sélectionnez **Britta Simon**.
5. Dans la barre d’outils située en bas, cliquez sur **Attribuer**.

![Affecter des utilisateurs][205]

### <a name="test-single-sign-on"></a>Tester l’authentification unique
L’objectif de cette section est de tester la configuration de l’authentification unique Azure AD à l’aide du volet d’accès.

Quand vous cliquez sur la vignette Tableau Server dans le volet d’accès, vous devez être connecté automatiquement à votre application Tableau Server.

## <a name="additional-resources"></a>Ressources supplémentaires
* [Liste de didacticiels sur l’intégration d’applications SaaS avec Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_205.png

