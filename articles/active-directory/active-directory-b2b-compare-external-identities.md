---
title: "Comparaison des capacités de gestion des identités externes à l’aide d’Azure Active Directory | Microsoft Docs"
description: "Compare les applications Azure Active Directory B2B Collaboration, B2C et l’application mutualisée pour la prise en charge de l’authentification et de l’autorisation associées aux identités externes."
services: active-directory
documentationcenter: 
author: arvindsuthar
manager: cliffdi
editor: 
tags: 
ms.assetid: 749b2e3a-2b8e-45ba-9f2a-6ca56eb1053b
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 02/24/2016
ms.author: asuthar
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: a9a7c0a9adea7fe4fccaa4ec9df0b077cd1e7382


---
# <a name="comparing-capabilities-for-managing-external-identities-using-azure-active-directory"></a>Comparaison des capacités de gestion des identités externes à l’aide d’Azure Active Directory
Outre la gestion de l’accès aux applications mobiles SaaS du personnel, Azure Active Directory (Azure AD) peut aider votre entreprise à partager des ressources avec des partenaires commerciaux et fournir des applications aux entreprises et aux consommateurs.

## <a name="developing-applications-for-businesses"></a>Développement d’applications pour les entreprises
Fournissez-vous aux entreprises un service ou une application, tels qu’un service de paie ? Azure AD propose la plateforme d’identité qui vous permet de développer des applications s’intégrant de manière transparente dans des millions d’organisations pour lesquelles la fonction Azure AD est configurée dans le cadre du déploiement d’Office 365 ou d’autres services d’entreprise.

**Exemple :** une société de distribution pharmaceutique fournit du matériel médical et des systèmes d’information pour le secteur de la santé. Elle veut fournir une application d’analyse aux services médicaux, en souhaitant que les clients gèrent leurs propres identités. Cette société a choisi Azure AD en tant que plateforme d’identité pour son application de gestion de services médicaux, fournissant aux clients des identités Azure AD lorsque nécessaire, lors de leur inscription. Pour plus d’informations, consultez la page [Guide du développeur Azure Active Directory](active-directory-developers-guide.md).

## <a name="enabling-business-partner-access-to-your-corporate-resources"></a>Comment autoriser vos partenaires commerciaux à accéder à vos ressources d’entreprise
Certains partenaires commerciaux ou tiers en dehors de votre entreprise ont-ils besoin d’accéder aux ressources de votre entreprise, comme un site SharePoint ou votre système ERP ? Azure AD permet aux administrateurs d’accorder à des utilisateurs externes (qui existent ou non dans Azure AD) une authentification unique pour l’accès aux applications d’entreprise. Cela améliore la sécurité, car les utilisateurs perdent leur authentification lorsqu’ils quittent l’organisation partenaire, tandis que vous contrôlez les stratégies d’accès au sein de votre organisation. Cela simplifie également l’administration, car vous n’avez pas à gérer le répertoire d’un partenaire externe ou des relations de fédération partenaire.

**Exemple :** une société d’imagerie fournit des services d’imagerie photographique à des détaillants et gère le plus grand réseau de vente au détail de bornes d’impression photo. Ils ont choisi Azure AD, afin de permettre à des milliers d’utilisateurs chez leurs partenaires commerciaux de vente au détail d’utiliser leurs propres informations d’identification pour télécharger les derniers documents marketing du partenaire et réorganiser le matériel de traitement des photos depuis l’extranet du fournisseur de la société. Pour plus d’informations, consultez [Collaboration Azure AD B2B](active-directory-b2b-what-is-azure-ad-b2b.md).

## <a name="developing-applications-for-consumers"></a>Développement d’applications pour les consommateurs
Avez-vous besoin de publier des applications en ligne telles qu’une présentation de votre magasin, de manière économique et en toute sécurité, pour ensuite la rendre visible à des millions d’utilisateurs ? Azure AD fournit une plateforme permettant une connexion à la fois depuis les réseaux sociaux et par nom d’utilisateur/mot de passe, appelée « inscription libre », ainsi que la réinitialisation du mot de passe libre-service pour les détenteurs de votre application. Cela reste plus commode pour vos consommateurs, tout en réduisant la charge sur les développeurs.

**Exemple :** la franchise sportive mondiale \#1 dans le monde avait besoin de communiquer directement avec ses 450 millions de fans. Pour ce faire, cette société a créé une application mobile, à l’aide de Microsoft Azure AD, pour authentifier ses utilisateurs et le stockage de leurs profils. Les fans ont accès à une inscription et une connexion facilitées à partir de réseaux sociaux tels que Facebook, ou peuvent utiliser un nom d’utilisateur/mot de passe traditionnels pour une expérience sans encombre sur iOS, Android et Windows Phone. Le développement de l’application sur la plateforme Azure AD a réduit de manière considérable le code personnalisé, tout en conférant à la plateforme une identité personnalisée et en dissipant les préoccupations en matière de sécurité, les divulgations de données et l’évolutivité. Pour plus d’informations, consultez [Azure AD B2C](https://azure.microsoft.com/documentation/services/active-directory-b2c/).

## <a name="comparison-of-azure-ad-capabilities"></a>Comparaison des fonctionnalités d’Azure AD
Chacun des scénarios déjà abordés dans cet article peut être résolu grâce aux fonctionnalités d’Azure AD. Ce tableau devrait vous aider à déterminer les fonctionnalités les plus pertinentes pour vous :

| **Envisagez de choisir ce produit...** | [Application SaaS Azure AD mutualisée](active-directory-developers-guide.md) | [Azure AD B2B Collaboration](active-directory-b2b-what-is-azure-ad-b2b.md) | [Azure AD B2C](https://azure.microsoft.com/documentation/services/active-directory-b2c/) |
| --- | --- | --- | --- |
| **Si vous devez fournir...** |Un service à une société |Un accès à vos applications à vos partenaires |Un service à vos consommateurs |
| **Si votre activité est similaire à celle...** |D’un distributeur pharmaceutique |D’une société d’imagerie |D’une franchise sportive |
| **Vous déployez une application pour...** |La gestion des services médicaux |L’extranet d’un fournisseur |Des fans de football |
| **Ciblant...** |Les cabinets de médecins |Les partenaires commerciaux approuvés |Toute personne disposant d’une adresse électronique |
| **Accessible lorsque...** |L’administrateur du client donne son approbation |Votre administrateur envoie une invitation |Le client s’inscrit |




<!--HONumber=Dec16_HO4-->


