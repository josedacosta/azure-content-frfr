<properties
	pageTitle="Ajouter le connecteur Dynamics CRM Online à vos applications logiques | Microsoft Azure"
	description="Créez des applications logiques avec Azure App Service. Le fournisseur de connexion Dynamics CRM Online fournit une API permettant de travailler avec les entités sur Dynamics CRM Online."
	services="logic-apps"    
	documentationCenter=""     
	authors="MandiOhlinger"    
	manager="erikre"    
	editor="" 
	tags="connectors" />

<tags
ms.service="logic-apps"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="08/15/2016"
ms.author="mandia"/>

# Prise en main du connecteur Dynamics CRM Online
Connectez-vous à Dynamics CRM Online pour créer un enregistrement, mettre à jour un élément et bien plus encore. Avec CRM Online, vous pouvez effectuer les opérations suivantes :

- Créer votre flux d’activité en fonction des données que vous obtenez de CRM Online.
- Utiliser des actions pour supprimer un enregistrement, obtenir des entités et bien plus encore. Ces actions obtiennent une réponse, puis mettent la sortie à la disposition d’autres actions. Par exemple, quand un élément est mis à jour dans CRM, vous pouvez envoyer un message électronique à l’aide d’Office 365.

Cette rubrique décrit comment utiliser le connecteur Dynamics CRM Online dans une application logique et répertorie également les déclencheurs et les actions.

>[AZURE.NOTE] Cette version de l’article s’applique à la disponibilité générale des applications logiques.

Pour plus d’informations sur Logic Apps, voir [Qu’est-ce qu’une application logique ?](../app-service-logic/app-service-logic-what-are-logic-apps.md) et [Créer une application logique](../app-service-logic/app-service-logic-create-a-logic-app.md).

## Se connecter à Dynamics CRM Online

Pour que votre application logique puisse accéder à un service, vous devez d’abord créer une *connexion* à celui-ci. Une connexion permet d’assurer la connectivité entre une application logique et un autre service. Par exemple, pour vous connecter à Dynamics, vous devez disposer d’une *connexion* Dynamics CRM Online. Pour créer une connexion, entrez les informations d’identification que vous utilisez généralement pour accéder au service auquel vous souhaitez vous connecter. Ensuite, dans Dynamics, entrez les informations d’identification à votre compte Dynamics CRM Online pour créer la connexion.


### Créer la connexion

>[AZURE.INCLUDE [Étapes de création d’une connexion à un fournisseur de connexion Dynamics CRM Online](../../includes/connectors-create-api-crmonline.md)]

## Utilisation d’un déclencheur

Un déclencheur est un événement qui peut être utilisé pour lancer le flux de travail défini dans une application logique. Les déclencheurs « interrogent » le service à l’intervalle et à la fréquence de votre choix. [En savoir plus sur les déclencheurs](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

1. Dans l’application logique, saisissez « dynamics » pour obtenir la liste des déclencheurs :

	![](./media/connectors-create-api-crmonline/dynamics-triggers.png)

2. Sélectionnez **Dynamics CRM Online - Lorsqu’un enregistrement est créé**. Si une connexion existe déjà, sélectionnez une organisation et une entité dans la liste déroulante.

	![](./media/connectors-create-api-crmonline/select-organization.png)

	Si vous êtes invité à vous connecter, entrez les informations de connexion pour créer la connexion. La section [Créer la connexion](connectors-create-api-crmonline.md#create-the-connection) figurant dans cette rubrique répertorie les étapes.

	> [AZURE.NOTE] Dans cet exemple, l’application logique s’exécute lorsqu’un enregistrement est créé. Pour consulter les résultats de ce déclencheur, ajoutez une autre action qui vous envoie un courrier électronique. Par exemple, ajoutez l’action Office 365 *Envoyer un courrier électronique* qui vous avertit par e-mail lorsqu’un enregistrement est ajouté.

3. Sélectionnez le bouton **Modifier**, puis renseignez les champs **Fréquence** et **Intervalle**. Par exemple, si vous souhaitez que le déclencheur interroge le service toutes les 15 minutes, définissez le champ **Fréquence** sur **Minute**, et le champ **Intervalle** sur **15**.

	![](./media/connectors-create-api-crmonline/edit-properties.png)

4. **Enregistrez** vos modifications (dans le coin supérieur gauche de la barre d’outils). Votre application logique est enregistrée et peut être activée automatiquement.


## Utilisation d’une action

Une action est une opération effectuée par le flux de travail défini dans une application logique. [En savoir plus sur les actions](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

1. Sélectionnez le signe plus. Vous disposez de plusieurs options : **Ajouter une action**, **Ajouter une condition** ou l’une des options **Plus**.

	![](./media/connectors-create-api-crmonline/add-action.png)

2. Choisissez **Ajouter une action**.

3. Dans la zone de texte, saisissez « dynamics » pour obtenir la liste de toutes les actions disponibles.

	![](./media/connectors-create-api-crmonline/dynamics-actions.png)

4. Dans notre exemple, choisissez **Dynamics CRM Online - Mettre à jour un enregistrement**. Si une connexion existe déjà, choisissez le **Nom de l’organisation**, le **Nom de l’entité** et d’autres propriétés :

	![](./media/connectors-create-api-crmonline/sample-action.png)

	Si vous êtes invité à saisir les informations de connexion, entrez les informations requises pour créer la connexion. La section [Créer la connexion](connectors-create-api-crmonline.md#create-the-connection) figurant dans cette rubrique décrit ces propriétés.

	> [AZURE.NOTE] Dans cet exemple, nous mettons à jour un enregistrement existant dans CRM Online. Vous pouvez utiliser la sortie d’un autre déclencheur pour mettre à jour l’enregistrement. Par exemple, ajoutez le déclencheur SharePoint *Lorsqu’un élément existant est modifié*. Ensuite, ajoutez l’action CRM Online *Mettre à jour un enregistrement* qui utilise les champs SharePoint pour mettre à jour l’enregistrement existant dans CRM Online.

5. **Enregistrez** vos modifications (dans le coin supérieur gauche de la barre d’outils). Votre application logique est enregistrée et peut être activée automatiquement.


## Détails techniques

## Déclencheurs

|Déclencheur | Description|
|--- | ---|
|[Lorsqu’un enregistrement est créé](connectors-create-api-crmonline.md#when-a-record-is-created)|Déclenche un flux quand un objet est créé dans CRM.|
|[Lorsqu’un enregistrement est mis à jour](connectors-create-api-crmonline.md#when-a-record-is-updated)|Déclenche un flux quand un objet est modifié dans CRM.|
|[Lorsqu’un enregistrement est supprimé](connectors-create-api-crmonline.md#when-a-record-is-deleted)|Déclenche un flux quand un objet est supprimé dans CRM.|


## Actions

|Action|Description|
|--- | ---|
|[Répertorier les enregistrements](connectors-create-api-crmonline.md#list-records)|Cette opération récupère les enregistrements d’une entité.|
|[Créer un enregistrement](connectors-create-api-crmonline.md#create-a-new-record)|Cette opération crée un enregistrement pour une entité.|
|[Obtenir l’enregistrement](connectors-create-api-crmonline.md#get-record)|Cette opération récupère l’enregistrement spécifié pour une entité.|
|[Supprimer un enregistrement](connectors-create-api-crmonline.md#delete-a-record)|Cette opération supprime un enregistrement à partir d’une collection d’entités.|
|[Mise à jour d’un enregistrement](connectors-create-api-crmonline.md#update-a-record)|Cette opération met à jour un enregistrement existant pour une entité.|

### Détail des déclencheurs et des actions

Dans cette section, consultez les détails relatifs à chacun des déclencheurs et actions, y compris toutes les propriétés d’entrée requises ou facultatives et toute sortie correspondante associée au connecteur.

#### Lorsqu’un enregistrement est créé
Déclenche un flux quand un objet est créé dans CRM.

|Nom de la propriété| Nom complet|Description|
| ---|---|---|
|dataset*|Nom de l’organisation|Nom de l’organisation CRM, comme Contoso|
|table*|Nom de l’entité|Nom de l’entité|
|$skip|Nombre à ignorer|Nombre d’entrées à ignorer (valeur par défaut : 0)|
|$top|Nombre maximal à récupérer|Nombre maximal d’entrées à récupérer (par défaut : 256)|
|$filter|Requête de filtre|Requête de filtre ODATA pour limiter le nombre d’entrées renvoyées|
|$orderby|Trier par|Requête orderBy ODATA pour spécifier l’ordre des entrées|

Un astérisque (*) signifie que la propriété est requise.

##### Détails des résultats
ItemsList

| Nom de la propriété | Type de données |
|---|---|
|value|array|


#### Lorsqu’un enregistrement est mis à jour
Déclenche un flux quand un objet est modifié dans CRM.

|Nom de la propriété| Nom complet|Description|
| ---|---|---|
|dataset*|Nom de l’organisation|Nom de l’organisation CRM, comme Contoso|
|table*|Nom de l’entité|Nom de l’entité|
|$skip|Nombre à ignorer|Nombre d’entrées à ignorer (valeur par défaut : 0)|
|$top|Nombre maximal à récupérer|Nombre maximal d’entrées à récupérer (par défaut : 256)|
|$filter|Requête de filtre|Requête de filtre ODATA pour limiter le nombre d’entrées renvoyées|
|$orderby|Trier par|Requête orderBy ODATA pour spécifier l’ordre des entrées|

Un astérisque (*) signifie que la propriété est requise.

##### Détails des résultats
ItemsList

| Nom de la propriété | Type de données |
|---|---|
|value|array|


#### Lorsqu’un enregistrement est supprimé
Déclenche un flux quand un objet est supprimé dans CRM.

|Nom de la propriété| Nom complet|Description|
| ---|---|---|
|dataset*|Nom de l’organisation|Nom de l’organisation CRM, comme Contoso|
|table*|Nom de l’entité|Nom de l’entité|
|$skip|Nombre à ignorer|Nombre d’entrées à ignorer (valeur par défaut : 0)|
|$top|Nombre maximal à récupérer|Nombre maximal d’entrées à récupérer (par défaut : 256)|
|$filter|Requête de filtre|Requête de filtre ODATA pour limiter le nombre d’entrées renvoyées|
|$orderby|Trier par|Requête orderBy ODATA pour spécifier l’ordre des entrées|

Un astérisque (*) signifie que la propriété est requise.

##### Détails des résultats
ItemsList

| Nom de la propriété | Type de données |
|---|---|
|value|array|


#### Répertorier les enregistrements
Cette opération récupère les enregistrements d’une entité.

|Nom de la propriété| Nom complet|Description|
| ---|---|---|
|dataset*|Nom de l’organisation|Nom de l’organisation CRM, comme Contoso|
|table*|Nom de l’entité|Nom de l’entité|
|$skip|Nombre à ignorer|Nombre d’entrées à ignorer (valeur par défaut : 0)|
|$top|Nombre maximal à récupérer|Nombre maximal d’entrées à récupérer (par défaut : 256)|
|$filter|Requête de filtre|Requête de filtre ODATA pour limiter le nombre d’entrées renvoyées|
|$orderby|Trier par|Requête orderBy ODATA pour spécifier l’ordre des entrées|

Un astérisque (*) signifie que la propriété est requise.

##### Détails des résultats
ItemsList

| Nom de la propriété | Type de données |
|---|---|
|value|array|


#### Créer un enregistrement
Cette opération crée un enregistrement pour une entité.

|Nom de la propriété| Nom complet|Description|
| ---|---|---|
|dataset*|Nom de l’organisation|Nom de l’organisation CRM, comme Contoso|
|table*|Nom de l’entité|Nom de l’entité|

Un astérisque (*) signifie que la propriété est requise.

##### Détails des résultats
Aucune.


#### Obtenir l’enregistrement
Cette opération récupère l’enregistrement spécifié pour une entité.

|Nom de la propriété| Nom complet|Description|
| ---|---|---|
|dataset*|Nom de l’organisation|Nom de l’organisation CRM, comme Contoso|
|table*|Nom de l’entité|Nom de l’entité|
|id*|Identificateur d’élément|Indiquez l’identificateur de l’enregistrement|

Un astérisque (*) signifie que la propriété est requise.

##### Détails des résultats
Aucune.


#### Supprimer un enregistrement
Cette opération supprime un enregistrement à partir d’une collection d’entités.

|Nom de la propriété| Nom complet|Description|
| ---|---|---|
|dataset*|Nom de l’organisation|Nom de l’organisation CRM, comme Contoso|
|table*|Nom de l’entité|Nom de l’entité|
|id*|Identificateur d’élément|Indiquez l’identificateur de l’enregistrement|

Un astérisque (*) signifie que la propriété est requise.


#### Mise à jour d’un enregistrement
Cette opération met à jour un enregistrement existant pour une entité.

|Nom de la propriété| Nom complet|Description|
| ---|---|---|
|dataset*|Nom de l’organisation|Nom de l’organisation CRM, comme Contoso|
|table*|Nom de l’entité|Nom de l’entité|
|id*|Identificateur de l’enregistrement|Indiquez l’identificateur de l’enregistrement|

Un astérisque (*) signifie que la propriété est requise.

##### Détails des résultats
Aucune.


## Réponses HTTP

Les actions et déclencheurs peuvent renvoyer un ou plusieurs des codes d’état HTTP suivants :

|Name|Description|
|---|---|
|200|OK|
|202|Acceptée|
|400|Demande incorrecte|
|401|Non autorisé|
|403|Interdit|
|404|Introuvable|
|500|Erreur interne du serveur. Une erreur inconnue s’est produite.|
|default|L’opération a échoué.|


## Étapes suivantes

[Créez une application logique](../app-service-logic/app-service-logic-create-a-logic-app.md). Explorez les autres connecteurs disponibles dans les applications logiques en consultant notre [liste d’API](apis-list.md).

<!---HONumber=AcomDC_0817_2016-->