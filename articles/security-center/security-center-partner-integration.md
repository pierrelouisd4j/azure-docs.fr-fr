---
title: "Intégration des partenaires dans Azure Security Center | Microsoft Docs"
description: "Ce document explique comment Azure Security Center s’intègre avec les partenaires pour améliorer la sécurité globale de vos ressources Azure."
services: security-center
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 6af354da-f27a-467a-8b7e-6cbcf70fdbcb
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/01/2016
ms.author: yurid
translationtype: Human Translation
ms.sourcegitcommit: 5001cd47b6ee51967d1286414ccefedd8e7e7813
ms.openlocfilehash: 095b5c9d1a888a4061450234f80c52c5834fbf53


---
# <a name="partner-integration-in-azure-security-center"></a>Intégration des partenaires dans Azure Security Center
Ce document explique comment Azure Security Center s’intègre avec les partenaires afin d’améliorer la sécurité globale et de fournir une expérience intégrée dans Azure, tout en tirant parti de la Place de marché Microsoft Azure pour la certification des partenaires et la facturation.

## <a name="why-deploy-partners-solutions-from-security-center"></a>Pourquoi déployer des solutions de partenaire à partir de Security Center ?

L’intégration partenaire dans Security Center offre quatre avantages majeurs :

- **Facilité de déploiement** : il est beaucoup plus facile de déployer une solution de partenaire en suivant la recommandation de Security Center. Le processus de déploiement peut être entièrement automatisé par le biais d’une configuration et d’une topologie du réseau par défaut, ou les clients peuvent choisir une option semi-automatique pour une configuration plus flexible et personnalisée.
- **Détections intégrées** : les événements de sécurité des solutions de partenaire sont automatiquement collectés, agrégés et affichés dans le cadre des alertes et des incidents de Security Center. Ces événements sont également fusionnés avec les détections d’autres sources pour fournir des fonctions de détection de menaces avancées.
- **Analyse et gestion unifiées du fonctionnement **: les événements d’analyse intégrés permettent aux clients d’analyser toutes les solutions de partenaire en un coup d’œil. La gestion de base offre un accès facile à la configuration avancée avec la solution de partenaire.
- **Exportation vers un système SIEM** : les clients peuvent désormais exporter toutes les alertes de Security Center et des partenaires au format CEF vers les systèmes SIEM locaux au moyen de l’intégration des journaux Microsoft Azure (version préliminaire)


## <a name="what-partners-are-integrated-with-security-center"></a>Quels partenaires s’intègrent avec Security Center ?
Actuellement, Security Center s’intègre avec les partenaires suivants :

- Endpoint Protection (Trend Micro) ; 
- Pare-feu d’applications web (Barracuda, F5, Imperva et bientôt Microsoft WAF et Fortinet) ; 
- Solutions de pare-feu de nouvelle génération (Check Point, Barracuda, et bientôt Fortinet et Cisco) ; 
- Solutions d’évaluation des vulnérabilités (Qualys - version préliminaire). 

Au fil du temps, Security Center augmentera le nombre de partenaires au sein de ces catégories existantes et ajoutera des catégories. 

## <a name="how-to-deploy-a-partner-solution"></a>Comment déployer une solution de partenaire ?

Selon la configuration de votre environnement Azure et la stratégie de sécurité que vous avez définie, le centre de sécurité peut recommander le déploiement d’une solution de partenaire. La recommandation va vous guider tout au long du processus de sélection et d’installation d’une solution de partenaire. À ce stade, l’expérience de déploiement global peut varier en fonction du type de solution et du partenaire. Consultez les liens ci-après pour plus d'informations :

- [Ajouter un pare-feu d’applications web](security-center-add-web-application-firewall.md)
- [Ajouter un pare-feu de nouvelle génération](security-center-add-next-generation-firewall.md)
- [Installer Endpoint Protection](security-center-install-endpoint-protection.md)
- [Évaluation des vulnérabilités non installée](security-center-vulnerability-assessment-recommendations.md)

## <a name="how-to-manage-partner-solutions"></a>Comment gérer les solutions de partenaires ?

Après le déploiement d’une solution de partenaire, vous pouvez afficher des informations sur l’intégrité de la solution et effectuer les tâches de gestion de base à partir de la mosaïque de solutions de partenaires dans le tableau de bord principal du Security Center. Pour plus d’informations sur la gestion des solutions de partenaire dans Security Center, consultez [Surveillance des solutions de partenaire](security-center-partner-solutions.md) avec Azure Security Center.

![Intégration des partenaires](./media/security-center-partner-integration/security-center-partner-integration-fig1.png)


## <a name="see-also"></a>Voir aussi
Dans ce document, vous avez appris à intégrer une solution de partenaire dans Azure Security Center. Pour plus d’informations sur Security Center, consultez :

* [Guide des opérations et de planification du Centre de sécurité Azure](security-center-planning-and-operations-guide.md)
* [Gestion et résolution des alertes de sécurité dans le Centre de sécurité Azure](security-center-managing-and-responding-alerts.md)
* [Alertes de sécurité par type dans le centre de sécurité Azure](security-center-alerts-type.md)
* [Surveillance de l’intégrité de la sécurité dans Azure Security Center](security-center-monitoring.md) : découvrez comment surveiller l’intégrité de vos ressources Azure.
* [Surveillance des solutions de partenaire avec Azure Security Center](security-center-partner-solutions.md) : découvrez comment surveiller l’état d’intégrité de vos solutions de partenaire.
* [FAQ d’Azure Security Center](security-center-faq.md) : découvrez les réponses aux questions les plus souvent posées à propos de l’utilisation de ce service.
* [Blog sur la sécurité Azure](http://blogs.msdn.com/b/azuresecurity/) : accédez à des billets de blog sur la sécurité et la conformité Azure.



<!--HONumber=Dec16_HO1-->


