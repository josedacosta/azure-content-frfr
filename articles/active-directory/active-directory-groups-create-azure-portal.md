<properties
	pageTitle="Créer un groupe dans la version préliminaire d’Azure Active Directory | Microsoft Azure"
	description="Comment créer un groupe dans Active Directory Azure et ajouter des utilisateurs (membres) au groupe"
	services="active-directory"
	documentationCenter=""
	authors="curtand"
	manager="femila"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/12/2016"
	ms.author="curtand"/>


# Créez un groupe dans la version préliminaire d’Azure Active Directory

Cet article explique comment créer un remplir un nouveau groupe dans la version préliminaire d’Azure Active Directory (Azure AD). [Nouveautés de la version préliminaire](active-directory-preview-explainer.md) Un groupe vous permet d’effectuer différentes tâches de gestion, par exemple l’attribution de licences ou autorisations à plusieurs utilisateurs ou appareils simultanément.

## Comment créer un groupe ?

1. Connectez-vous au [portail Azure](https://portal.azure.com) en utilisant un compte d’administrateur général pour le répertoire.

2. Sélectionnez **Plus de services**, saisissez **Utilisateurs et groupes** dans la zone de texte, puis sélectionnez **Entrée**.

  ![Ouvrir la gestion des utilisateurs](./media/active-directory-groups-create-azure-portal/search-user-management.png)

3. Dans le panneau **Utilisateurs et groupes**, sélectionnez **Tous les groupes**.

  ![Ouvrir le panneau de groupes](./media/active-directory-groups-create-azure-portal/view-groups-blade.png)

4. Dans le panneau **Utilisateurs et groupes - Tous les groupes**, sélectionnez la commande **Ajouter**.

  ![Sélection de la commande Ajouter](./media/active-directory-groups-create-azure-portal/add-group-command.png)

5. Dans le panneau **Groupe**, ajoutez un nom et une description pour le groupe.

6. Pour sélectionner les membres à ajouter au groupe, sélectionnez **Affecté** dans la zone **Type d’appartenance**, puis sélectionnez **Membres**. Pour plus d’informations sur la façon de gérer l’appartenance à un groupe dynamique, consultez la page [Gérer les règles dynamiques pour les utilisateurs dans un groupe](active-directory-groups-dynamic-users-azure-portal.md) ou [Gérer les règles dynamiques pour les appareils dans un groupe](active-directory-groups-dynamic-devices-azure-portal.md).

  ![Sélection de membres à ajouter](./media/active-directory-groups-create-azure-portal/select-members.png)

5. Dans le panneau **Membres**, sélectionnez un ou plusieurs utilisateurs ou appareils à ajouter au groupe, puis cliquez sur le **Sélectionner** en bas du panneau pour les ajouter au groupe. La zone **utilisateur** filtre l’affichage en fonction de la correspondance de votre entrée avec une partie ou l’intégralité d’un nom d’utilisateur ou d’appareil. Dans cette zone aucun caractère générique n’est accepté.

6. Lorsque vous avez terminé l’ajout de membres au groupe, sélectionnez **Créer** dans le panneau **Groupe**.

  ![Confirmation de création du groupe](./media/active-directory-groups-create-azure-portal/create-group-confirmation.png)




## Informations supplémentaires

Ces articles fournissent des informations supplémentaires sur Azure Active Directory.

* [Consulter les groupes existants](active-directory-groups-view-azure-portal.md)
* [Gérer les paramètres d’un groupe](active-directory-groups-settings-azure-portal.md)
* [Gérer les membres d’un groupe](active-directory-groups-members-azure-portal.md)
* [Gérer l’appartenance à un groupe](active-directory-groups-membership-azure-portal.md)
* [Gérer les règles dynamiques pour les utilisateurs dans un groupe](active-directory-groups-dynamic-membership-azure-portal.md)

<!---HONumber=AcomDC_0914_2016-->