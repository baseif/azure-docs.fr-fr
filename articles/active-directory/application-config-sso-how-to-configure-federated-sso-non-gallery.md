---
title: "Comment configurer l’authentification unique fédérée pour une application non issue de la galerie | Microsoft Docs"
description: "Comment configurer l’authentification unique fédérée pour une application personnalisée non issue de la galerie que vous souhaitez intégrer à Azure AD"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: asteen
translationtype: Human Translation
ms.sourcegitcommit: 0d6f6fb24f1f01d703104f925dcd03ee1ff46062
ms.openlocfilehash: 022a514de4cb36fdce9fc630e4b692f4d2fa42d4
ms.lasthandoff: 04/17/2017


---

# <a name="how-to-configure-federated-single-sign-on-for-a-non-gallery-application"></a>Comment configurer l’authentification unique fédérée pour une application non issue de la galerie

Pour configurer une application non issue de la galerie, vous devez disposer d’Azure AD Premium, et l’application doit prendre en charge SAML 2.0. Pour plus d’informations sur les versions d’Azure AD, consultez [Tarification de Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="overview-of-steps-required"></a>Vue d’ensemble des étapes requises
Vous trouverez ci-dessous une vue d’ensemble des étapes de haut niveau à suivre pour configurer l’authentification unique fédérée pour une application non issue de la galerie (p. ex. une application personnalisée).

-   [Configurer les valeurs de métadonnées de l’application dans Azure AD (URL de connexion, identificateur, URL de réponse)](#_Configuring_single_sign-on)

-   [Sélectionner l’identificateur de l’utilisateur et ajouter les attributs d’utilisateur à envoyer à l’application](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [Récupérer le certificat et les métadonnées Azure AD](#download-the-azure-ad-metadata-or-certificate)

-   [Configurer les valeurs de métadonnées Azure AD dans l’application (URL de connexion, émetteur, URL de déconnexion et certificat)](#_Configuring_single_sign-on)

-   [Affecter des utilisateurs à l’application](#_Assign_users_to_the_application)

## <a name="configuring-single-sign-on-to-non-gallery-applications"></a>Configuration de l’authentification unique pour des applications non issues de la galerie

Pour configurer l’authentification unique pour une application qui n’est pas issue de la galerie Azure AD, effectuez les étapes suivantes :

1.  Ouvrez le [**portail Azure**](https://portal.azure.com/) et connectez-vous en tant **qu’administrateur général** ou en tant que **coadministrateur**.

2.  Ouvrez **l’extension Azure Active Directory** en cliquant sur **Autres services** en bas du menu de navigation principal de gauche.

3.  Tapez « **Azure Active Directory** » dans la zone de recherche du filtre et sélectionnez l’élément **Azure Active Directory**.

4.  Cliquez sur **Applications d’entreprise** dans le menu de navigation de gauche d’Azure Active Directory.

5.  Cliquez sur le bouton **Ajouter** dans le coin supérieur droit du panneau **Applications d’entreprise**.

6.  Cliquez sur **Application ne figurant pas dans la galerie** dans la section **Ajouter votre propre application**.

7.  Entrez le nom de votre application dans la zone de texte **Nom**.

8.  Pour ajouter l’application, cliquez sur le bouton **Ajouter**.

9.  Une fois l’application chargée, cliquez sur **Authentification unique** dans le menu de navigation de gauche de l’application.

10. Sélectionnez **Authentification basée sur SAML** dans la liste déroulante **Mode**.

11. Entrez les valeurs requises dans **Domaine et URL**. Ces valeurs doivent vous être communiquées par le fournisseur de l’application.

   1. Pour configurer l’application comme une application à authentification unique initiée par le fournisseur d’identité, entrez l’URL de réponse et l’identificateur.

   2. **Facultatif** : pour configurer l’application en tant qu’application à authentification unique initiée par le fournisseur de services, une URL de connexion est obligatoire.

12. Dans **Attributs d’utilisateur**, sélectionnez l’identificateur unique de vos utilisateurs dans la liste déroulante **Identificateur de l’utilisateur**.

13. **Facultatif** : pour modifier les attributs à envoyer à l’application dans le jeton SAML lorsque l’utilisateur se connecte, cliquez sur **Afficher et modifier tous les autres attributs utilisateur**.

   Pour ajouter un attribut :

   1. Cliquez sur **Ajouter un attribut**. Entrez le **Nom**, puis sélectionnez la **Valeur** dans la liste déroulante.

   2. Cliquez sur **Enregistrer**. Le nouvel attribut s’affiche dans le tableau.

14. Cliquez sur **Configurer &lt;nom de l’application&gt;** pour accéder à la documentation expliquant comment configurer l’authentification unique pour l’application. En outre, vous disposez des URL et du certificat Azure AD nécessaires pour l’application.

15. [Affectez des utilisateurs à l’application](#assign-users-to-the-application).

## <a name="select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application"></a>Sélectionner l’identificateur de l’utilisateur et ajouter les attributs d’utilisateur à envoyer à l’application

Pour sélectionner l’identificateur de l’utilisateur ou ajouter des attributs d’utilisateur, effectuez les étapes suivantes :

1.  Ouvrez le [**portail Azure**](https://portal.azure.com/) et connectez-vous en tant **qu’administrateur général** ou en tant que **coadministrateur**.

2.  Ouvrez **l’extension Azure Active Directory** en cliquant sur **Autres services** en bas du menu de navigation principal de gauche.

3.  Tapez « **Azure Active Directory** » dans la zone de recherche du filtre et sélectionnez l’élément **Azure Active Directory**.

4.  Cliquez sur **Applications d’entreprise** dans le menu de navigation de gauche d’Azure Active Directory.

5.  Cliquez sur **Toutes les applications** pour afficher une liste de toutes vos applications.

  * Si l’application que vous recherchez ne figure pas dans la liste, utilisez la commande **Filtre** en haut de la **liste de toutes les applications** et définissez l’option **Afficher** sur **Toutes les applications**.

6.  Sélectionnez l’application pour laquelle vous avez configuré l’authentification unique.

7.  Une fois l’application chargée, cliquez sur **Authentification unique** dans le menu de navigation de gauche de l’application.

8.  Dans la section **Attributs d’utilisateur**, sélectionnez l’identificateur unique de vos utilisateurs dans la liste déroulante **Identificateur de l’utilisateur**. L’option sélectionnée doit correspondre à la valeur attendue dans l’application pour authentifier l’utilisateur.

 >[!NOTE} Azure AD sélectionne le format de l’attribut NameID (Identificateur d’utilisateur) en fonction de la valeur sélectionnée ou du format demandé par l’application dans AuthRequest SAML. Pour plus d’informations, consultez l’article [Protocole SAML d’authentification unique](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) dans la section NameIDPolicy.
 >
 >

9.  Pour ajouter des attributs d’utilisateur, cliquez sur **Afficher et modifier tous les autres attributs utilisateur** afin de modifier les attributs à envoyer à l’application dans le jeton SAML lorsque l’utilisateur se connecte.

   Pour ajouter un attribut :

   1. Cliquez sur **Ajouter un attribut**. Entrez le **Nom**, puis sélectionnez la **Valeur** dans la liste déroulante.

   2. Cliquez sur **Enregistrer**. Le nouvel attribut s’affiche dans le tableau.

## <a name="download-the-azure-ad-metadata-or-certificate"></a>Télécharger les métadonnées ou le certificat Azure AD

Pour télécharger les métadonnées ou le certificat de l’application à partir d’Azure AD, effectuez les étapes suivantes :

1.  Ouvrez le [**portail Azure**](https://portal.azure.com/) et connectez-vous en tant **qu’administrateur général** ou en tant que **coadministrateur**.

2.  Ouvrez **l’extension Azure Active Directory** en cliquant sur **Autres services** en bas du menu de navigation principal de gauche.

3.  Tapez « **Azure Active Directory** » dans la zone de recherche du filtre et sélectionnez l’élément **Azure Active Directory**.

4.  Cliquez sur **Applications d’entreprise** dans le menu de navigation de gauche d’Azure Active Directory.

5.  Cliquez sur **Toutes les applications** pour afficher une liste de toutes vos applications.

   * Si l’application que vous recherchez ne figure pas dans la liste, utilisez la commande **Filtre** en haut de la **liste de toutes les applications** et définissez l’option **Afficher** sur **Toutes les applications**.

6.  Sélectionnez l’application pour laquelle vous avez configuré l’authentification unique.

7.  Une fois l’application chargée, cliquez sur **Authentification unique** dans le menu de navigation de gauche de l’application.

8.  Accédez à la section **Certificat de signature SAML**, puis cliquez sur la valeur de colonne **Télécharger**. En fonction de ce dont l’application a besoin pour configurer l’authentification unique, l’option de téléchargement du fichier XML contenant les métadonnées ou celle de téléchargement du certificat s’affiche.

Azure AD ne fournit pas d’URL permettant d’obtenir les métadonnées. Les métadonnées peuvent uniquement être récupérées sous forme de fichier XML.

## <a name="assign-users-to-the-application"></a>Affecter des utilisateurs à l’application

Pour affecter directement un ou plusieurs utilisateurs à une application, effectuez les étapes suivantes :

1.  Ouvrez le [**portail Azure**](https://portal.azure.com/) et connectez-vous en tant **qu’administrateur général**.

2.  Ouvrez **l’extension Azure Active Directory** en cliquant sur **Autres services** en bas du menu de navigation principal de gauche.

3.  Tapez « **Azure Active Directory** » dans la zone de recherche du filtre et sélectionnez l’élément **Azure Active Directory**.

4.  Cliquez sur **Applications d’entreprise** dans le menu de navigation de gauche d’Azure Active Directory.

5.  Cliquez sur **Toutes les applications** pour afficher une liste de toutes vos applications.

  * Si l’application que vous recherchez ne figure pas dans la liste, utilisez la commande **Filtre** en haut de la **liste de toutes les applications** et définissez l’option **Afficher** sur **Toutes les applications**.

6.  Dans la liste qui s’affiche, sélectionnez l’application à laquelle vous souhaitez affecter un utilisateur.

7.  Une fois l’application chargée, cliquez sur **Utilisateurs et groupes** dans le menu de navigation de gauche de l’application.

8.  Cliquez sur le bouton **Ajouter** en haut de la liste **Utilisateurs et groupes** pour ouvrir le panneau **Ajouter une attribution**.

9.  Cliquez sur le sélecteur **Utilisateurs et groupes** à partir du panneau **Ajouter une attribution**.

10. Tapez **le nom complet** ou **l’adresse de messagerie** de l’utilisateur souhaité pour l’attribution dans la zone de recherche **Rechercher par nom ou adresse de messagerie**.

11. Pointez sur **l’utilisateur** dans la liste pour afficher une **case à cocher**. Cliquez sur la case à cocher en regard de la photo de profil ou du logo de l’utilisateur pour ajouter ce dernier à la liste **Sélectionné**.

12. **Facultatif :** si vous souhaitez **ajouter plusieurs utilisateurs**, entrez un autre **nom complet** ou une autre **adresse de messagerie** dans la zone de recherche **Rechercher par nom ou adresse de messagerie**, puis cliquez sur la case à cocher pour ajouter cet utilisateur à la liste **Sélectionné**.

13. Après avoir sélectionné les utilisateurs, cliquez sur le bouton **Sélectionner** pour les ajouter à la liste des utilisateurs et des groupes à affecter à l’application.

14. **Facultatif :** cliquez sur le sélecteur **Sélectionner un rôle** dans le panneau **Ajouter une attribution** pour sélectionner un rôle à affecter aux utilisateurs sélectionnés.

15. Cliquez sur le bouton **Attribuer** pour affecter l’application aux utilisateurs sélectionnés.

Après quelques instants, les utilisateurs que vous avez sélectionnés seront en mesure de démarrer ces applications à l’aide des méthodes décrites dans la section de description des solutions.

## <a name="next-steps"></a>Étapes suivantes
[Fournir une authentification unique à vos applications avec le proxy d’application](active-directory-application-proxy-sso-using-kcd.md)

