<properties
	pageTitle="Contrôle d’accès en fonction du rôle | Microsoft Azure"
	description="Découvrez la gestion de l’accès avec le contrôle d’accès en fonction du rôle Azure dans le portail Azure. Utilisez des attributions de rôle pour attribuer des autorisations dans votre répertoire."
	services="active-directory"
	documentationCenter=""
	authors="kgremban"
	manager="femila"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.devlang="na"
	ms.topic="article"
	ms.tgt_pltfrm="na"
	ms.workload="identity"
	ms.date="08/03/2016"
	ms.author="kgremban"/>

# Prise en main de la gestion des accès dans le portail Azure

Les entreprises orientées sécurité doivent s’efforcer de donner aux employés les autorisations exactes dont ils ont besoin. Un trop grand nombre d’autorisations expose un compte aux attaquants. Si le nombre d’autorisations est trop faible, les employés ne peuvent pas effectuer leur travail efficacement. Le contrôle d’accès en fonction du rôle (RBAC) Azure permet de résoudre ce problème en offrant une gestion précise de l’accès pour Azure.

Avec le contrôle d’accès en fonction du rôle, vous pouvez séparer les tâches au sein de votre équipe et accorder aux utilisateurs uniquement les accès nécessaires pour accomplir leur travail. Plutôt que de donner à tous des autorisations illimitées au sein de votre abonnement ou de vos ressources Azure, vous pouvez autoriser uniquement certaines actions. Par exemple, vous pouvez utiliser le RBAC pour permettre à un employé donné de gérer les machines virtuelles dans un abonnement, tandis qu’un autre pourra gérer les bases de données SQL au sein du même abonnement.

## Concepts de base de la gestion d’accès dans Azure
Chaque abonnement Azure est associé à un répertoire Azure Active Directory (AD). Les utilisateurs, groupes et applications de ce répertoire peuvent gérer les ressources de l’abonnement Azure. Affectez ces droits d’accès à l’aide du portail Azure, des outils en ligne de commande Azure ou des API de gestion Azure.

Accordez les accès en affectant le rôle RBAC approprié aux utilisateurs, groupes et applications, dans une étendue donnée. L’étendue d’une attribution de rôle peut être une seule ressource, un groupe de ressources ou un abonnement. Un rôle attribué à une étendue parent accorde également l’accès aux enfants qu’elle contient. Par exemple, un utilisateur ayant accès à un groupe de ressources peut gérer toutes les ressources qu’il contient, telles que les sites Web, les machines virtuelles et les sous-réseaux.

![Relation entre les éléments d’Azure Active Directory - diagramme](./media/role-based-access-control-what-is/rbac_aad.png)

Le rôle RBAC que vous attribuez détermine les ressources que l’utilisateur, le groupe ou l’application peut gérer au sein de l’étendue.

## Rôles intégrés
Le contrôle d’accès en fonction du rôle Azure comporte trois rôles de base qui s’appliquent à tous les types de ressources :

- Le **Propriétaire** dispose d’un accès total à toutes les ressources, ainsi que le droit de déléguer l’accès à d’autres personnes.
- Le **Contributeur** peut créer et gérer tous les types de ressources Azure, mais ne peut pas accorder l’accès à d’autres personnes.
- Le **Lecteur** peut consulter les ressources Azure existantes.

Les autres rôles RBAC dans Azure permettent de gérer des ressources Azure spécifiques. Par exemple, le rôle de contributeur de machine virtuelle permet à l’utilisateur de créer et gérer des machines virtuelles. Il ne lui donne pas accès au réseau virtuel ou au sous-réseau auquel la machine virtuelle se connecte.

L’article [Rôles RBAC intégrés](role-based-access-built-in-roles.md) répertorie les rôles disponibles dans Azure. Elle indique les opérations et l’étendue que chaque rôle intégré accorde aux utilisateurs. Si vous avez besoin de définir vos propres rôles pour un meilleur contrôle, découvrez comment créer [des rôles personnalisés dans RBAC Azure](role-based-access-control-custom-roles.md).

## Hiérarchie des ressources et héritage d’accès
- Chaque **abonnement** Azure appartient à un seul répertoire.
- Chaque **groupe de ressources** appartient à un seul abonnement.
- Chaque **ressource** appartient à un seul groupe de ressources.

L’accès que vous accordez à des étendues parentes est hérité par les étendues enfant. Par exemple :

- Vous attribuez le rôle de lecteur à un groupe Azure AD dans l’étendue de l’abonnement. Les membres de ce groupe peuvent consulter tous les groupes de ressources et les ressources de l’abonnement.
- Vous attribuez le rôle de contributeur à une application dans l’étendue du groupe de ressources. Il peut gérer tous les types de ressources dans ce groupe de ressources, mais pas dans les autres groupes de ressources de l’abonnement.

## Comparaison entre RBAC Azure et Administrateur d’abonnement classiques
Les administrateurs et coadministrateurs d’abonnement classiques ont un accès total à l’abonnement Azure. Ils peuvent gérer des ressources à l’aide du [portail Azure](https://portal.azure.com) avec des API Azure Resource Manager ou du [portail Azure Classic](https://manage.windowsazure.com) et du modèle de déploiement Azure Classic. Dans le modèle RBAC, les administrateurs classiques se voient attribuer le rôle de propriétaire dans l’étendue de l’abonnement.

RBAC Azure est pris en charge uniquement par le portail Azure et les nouvelles API Azure Resource Manager. Les utilisateurs et les applications qui se voient affecter des rôles RBAC ne peuvent pas utiliser le portail de gestion classique et le modèle de déploiement Azure Classic.

## Autorisation pour les opérations de gestion et les opérations de données
RBAC Azure prend uniquement en charge les opérations de gestion des ressources Azure dans le portail Azure et les API Azure Resource Manager. Il ne peut pas autoriser toutes les opérations au niveau des données pour les ressources Azure. Par exemple, vous pouvez autoriser une personne à gérer les comptes de stockage, mais non les objets blob ou les tables dans un compte de stockage. De même, une base de données SQL peut être gérée, mais pas les tables qu’elle contient.

## Étapes suivantes
- Découvrez le [contrôle d’accès en fonction du rôle dans le portail Azure](role-based-access-control-configure.md).
- Consultez les [rôles RBAC intégrés](role-based-access-built-in-roles.md)
- Définissez vos propres [rôles personnalisés dans Azure RBAC](role-based-access-control-custom-roles.md)

<!---HONumber=AcomDC_0810_2016-->