<properties
   pageTitle="Gestion des configurations de service et des profils | Microsoft Azure"
   description="Découvrez comment utiliser les configurations de service et les fichiers de configuration de profils | qui stockent les paramètres pour les environnements de déploiement et comment publier les paramètres pour les services cloud."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# Gestion des configurations de service et des profils

## Vue d'ensemble

Lorsque vous publiez un service cloud, Visual Studio stocke les informations de configuration dans deux types de fichiers de configuration : les configurations de service et les profils. Les configurations de service (fichiers .cscfg) stockent les paramètres des environnements de déploiement pour un service cloud Azure. Azure utilise ces fichiers de configuration pour gérer vos services cloud. D'autre part, les profils (fichiers .azurePubxml) stockent les paramètres de publication des services cloud. Ces paramètres correspondent à l'enregistrement de ce que vous sélectionnez lorsque vous utilisez l'Assistant Publication. Ils sont utilisés localement par Visual Studio. Cette rubrique explique comment utiliser les deux types de fichiers de configuration.

## Configurations de service

Vous pouvez créer plusieurs configurations de service à utiliser pour chacun de vos environnements de déploiement. Par exemple, vous pouvez créer une configuration de service pour l'environnement local que vous utilisez pour exécuter et tester une application Azure et une autre configuration de service pour votre environnement de production.

Vous pouvez ajouter, supprimer, renommer et modifier ces configurations de service selon vos besoins. Vous pouvez gérer ces configurations de service à partir de Visual Studio, comme indiqué dans l'illustration suivante.

![Gérer les configurations de service](./media/vs-azure-tools-service-configurations-and-profiles-how-to-manage/manage-service-config.png)

Vous pouvez également ouvrir la boîte de dialogue **Gérer les configurations** de la page de propriétés du rôle. Pour ouvrir les propriétés d'un rôle dans votre projet Azure, ouvrez le menu contextuel de ce rôle, puis sélectionnez **Propriétés**. Dans l’onglet **Paramètres**, développez la liste **Configuration de service**, puis sélectionnez **Gérer** pour ouvrir la boîte de dialogue **Gérer les configurations**.

### Pour ajouter une configuration de service

1. Dans l’Explorateur de solutions, ouvrez le menu contextuel du projet Azure, puis sélectionnez **Gérer les configurations**.

    La boîte de dialogue **Gérer les configurations de service** s'affiche.

1. Pour ajouter une configuration de service, vous devez créer une copie d'une configuration existante. Pour ce faire, sélectionnez la configuration que vous souhaitez copier dans la liste Nom, puis sélectionnez **Créer une copie**.

1. (Facultatif) Pour donner un nom différent à la configuration de service, sélectionnez la nouvelle configuration de service dans la liste Nom, puis sélectionnez **Renommer**. Dans la zone de texte **Nom**, tapez le nom que vous voulez utiliser pour cette configuration de service, puis sélectionnez **OK**.

    Un nouveau fichier de configuration de service nommé ServiceConfiguration.[Nouveau nom].cscfg est ajouté au projet Azure dans l’Explorateur de solutions.


### Pour supprimer une configuration de service

1. Dans l’Explorateur de solutions, ouvrez le menu contextuel du projet Azure, puis sélectionnez **Gérer les configurations**.

    La boîte de dialogue **Gérer les configurations de service** s’affiche.

1. Pour supprimer une configuration de service, sélectionnez la configuration que vous voulez supprimer dans la liste **Nom**, puis sélectionnez **Supprimer**. Une boîte de dialogue vous invite à confirmer que vous souhaitez supprimer cette configuration.

1. Sélectionnez **Supprimer**.

     Le fichier de configuration de service est supprimé du projet Azure dans l'Explorateur de solutions.


### Pour renommer une configuration de service

1. Dans l’Explorateur de solutions, ouvrez le menu contextuel du projet Azure, puis sélectionnez **Gérer les configurations**.

    La boîte de dialogue **Gérer les configurations de service** s’affiche.

1. Pour renommer une configuration de service, sélectionnez la nouvelle configuration de service dans la liste **Nom**, puis sélectionnez **Renommer**. Dans la zone de texte **Nom**, tapez le nom que vous voulez utiliser pour cette configuration de service, puis sélectionnez **OK**.

    Le nom du fichier de configuration de service est modifié dans le projet Azure dans l'Explorateur de solutions.

### Pour modifier une configuration de service

- Pour modifier une configuration de service, ouvrez le menu contextuel du rôle que vous souhaitez modifier dans le projet Azure, puis sélectionnez **Propriétés**. Pour plus d’informations, consultez [Procédure : configuration des rôles pour un service cloud Azure avec Visual Studio](https://msdn.microsoft.com/library/azure/hh369931.aspx).

## Réaliser différentes combinaisons de paramètres à l'aide des profils

En utilisant un profil, vous pouvez renseigner automatiquement l’**Assistant Publication** avec une combinaison de paramètres différente pour chaque objectif. Par exemple, vous pouvez créer un profil pour le débogage et un autre pour les versions finales. Dans ce cas, votre profil **Débogage** aurait **IntelliTrace** activé et la configuration **Débogage** sélectionnée, tandis que votre profil **Version** aurait **IntelliTrace** désactivé et la configuration **Version** sélectionnée. Vous pourriez également utiliser des profils différents pour déployer un service à l'aide de comptes de stockage différents.

Lorsque vous exécutez l'Assistant pour la première fois, un profil par défaut est créé. Visual Studio stocke le profil dans un fichier portant l’extension .azurePubXml, qui est ajouté à votre projet Azure dans le dossier **Profils**. Si vous spécifiez manuellement d’autres options lors d’une exécution ultérieure de l'Assistant, le fichier se met à jour automatiquement. Avant de suivre la procédure suivante, vous devez avoir déjà publié votre service cloud au moins une fois.

### Pour ajouter un profil

1. Ouvrez le menu contextuel de votre projet Azure, puis sélectionnez **Publier**.

1. En regard de la liste **Profil cible** sélectionnez le bouton **Enregistrer le profil**, comme le montre l’illustration suivante. Cette action vous crée un profil.

    ![Créer un profil](./media/vs-azure-tools-service-configurations-and-profiles-how-to-manage/create-new-profile.png)

1. Une fois le profil créé, sélectionnez **<Gérer...>** dans la liste **Profil cible**.

    La boîte de dialogue **Gérer les profils** s’affiche, comme le montre l’illustration suivante.

    ![Boîte de dialogue Gérer les profils](./media/vs-azure-tools-service-configurations-and-profiles-how-to-manage/manage-profiles.png)

1. Dans la liste **Nom**, sélectionnez un profil, puis sélectionnez **Créer une copie**.

1. Sélectionnez le bouton **Fermer**.

    Le nouveau profil apparaît dans la liste des profils cibles.

1. Dans la liste **Profil cible**, sélectionnez le profil que vous venez de créer. Les paramètres de l'Assistant Publication sont renseignés avec les options de profil que vous avez sélectionnées.

1. Sélectionnez les boutons **Précédent** et **Suivant** pour afficher chaque page de l’Assistant Publication, puis personnalisez les paramètres de ce profil. Pour plus d’informations, consultez [Assistant Publication d’application Azure](http://go.microsoft.com/fwlink/p/?LinkID=623085).

1. Une fois la personnalisation des paramètres terminée, sélectionnez **Suivant** pour revenir à la page Paramètres. Le profil est enregistré quand vous publiez le service avec ces paramètres ou si vous sélectionnez **Enregistrer** en regard de la liste de profils.

### Pour renommer ou supprimer un profil

1. Ouvrez le menu contextuel de votre projet Azure, puis sélectionnez **Publier**.

1. Dans la liste **Profil cible**, sélectionnez **Gérer**.

1. Dans la boîte de dialogue **Gérer les profils**, sélectionnez le profil que vous voulez supprimer, puis sélectionnez **Supprimer**.

1. Dans la boîte de dialogue de confirmation qui s’affiche, sélectionnez **OK**.

1. Sélectionnez **Fermer**.

### Pour modifier un profil

1. Ouvrez le menu contextuel de votre projet Azure, puis sélectionnez **Publier**.

1. Dans la liste **Profil cible**, sélectionnez le profil que vous voulez modifier.

1. Sélectionnez les boutons **Précédent** et **Suivant** pour afficher chaque page de l’Assistant Publication, puis modifiez les paramètres de votre choix. Pour plus d’informations, consultez [Assistant Publication d’application Azure](http://go.microsoft.com/fwlink/p/?LinkID=623085).

1. Une fois la modification des paramètres terminée, sélectionnez **Suivant** pour revenir à la page **Paramètres**.

1. (Facultatif) Sélectionnez **Publier** pour publier le service cloud avec les nouveaux paramètres. Si vous ne souhaitez pas publier votre service cloud à ce stade et que vous fermez l'Assistant Publication, Visual Studio vous demandera si vous souhaitez enregistrer les modifications apportées au profil.

## Étapes suivantes

Pour en savoir plus sur la configuration des autres parties de votre projet Azure à partir de Visual Studio, consultez [Configuration d’un projet Azure](http://go.microsoft.com/fwlink/p/?LinkID=623075)

<!---HONumber=AcomDC_0817_2016-->