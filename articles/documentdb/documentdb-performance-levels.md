<properties
	pageTitle="Niveaux de performances dans DocumentDB | Microsoft Azure"
	description="Découvrez comment les niveaux de performances dans DocumentDB permettent de réserver le débit pour chaque collection."
	services="documentdb"
	authors="mimig1"
	manager="jhubbard"
	editor="monicar"
	documentationCenter=""/>

<tags
	ms.service="documentdb"
	ms.workload="data-services"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/26/2016"
	ms.author="mimig"/>

# Niveaux de performances dans DocumentDB

Cet article fournit une vue d’ensemble des niveaux de performances dans [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/).

Après avoir lu cet article, vous serez en mesure de répondre aux questions suivantes :

-	Qu'est-ce qu'un niveau de performances ?
-	Comment le débit est-il réservé pour un compte de base de données ?
-	Comment puis-je utiliser les niveaux de performances ?
-	Quel est le mode de facturation des niveaux de performances ?

## Introduction aux niveaux de performances

Chaque collection DocumentDB créée sous un compte d'utilisateur Standard est configurée avec un niveau de performances associé. Chaque collection dans une base de données peut avoir un niveau de performances différent, ce qui vous permet de désigner davantage de débit pour les collections fréquemment sollicitées et moins de débit pour les collections rarement sollicitées. DocumentDB prend en charge les niveaux de performances définis par l’utilisateur et les niveaux de performances prédéfinis.

À chaque niveau de performances est associé un taux limite [d’unités de requête](documentdb-request-units.md). Il s'agit du débit qui sera réservé pour une collection en fonction de son niveau de performances et qui sera disponible exclusivement pour cette collection.

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
        	<td valign="top"><p></p></td>
            <td valign="top"><p>Détails</p></td>
            <td valign="top"><p>Limites de débit</p></td>
            <td valign="top"><p>Limites de stockage</p></td>
            <td valign="top"><p>Version</p></td>
 			<td valign="top"><p>API</p></td>            
        </tr>
        <tr>
        	<td valign="top"><p>Performances définies par l’utilisateur</p></td>
            <td valign="top"><p>Stockage mesuré en fonction de l’utilisation en Go.</p><p>Débit en unités avec 100 unités de demande/s</p></td>
            <td valign="top"><p>Illimité. 400 - 250 000 unités de demande/s par défaut (supérieur sur demande)</p></td>
            <td valign="top"><p>Illimité. 250 Go par défaut (supérieur sur demande) </p></td>
            <td valign="top"><p>V2</p></td>
 			<td valign="top"><p>API 2015-12-16 et versions ultérieures</p></td>  
        </tr>
        <tr>
        	<td valign="top"><p>Performances prédéfinies</p></td>
            <td valign="top"><p>10 Go de stockage réservé.</p><p>S1 = 250 unités de demande/s, S2 = 1 000 unités de demande/s, S3 = 2 500 unités de demande/s</p></td>
            <td valign="top"><p>2 500 unités de demande/s</p></td>
            <td valign="top"><p>10 Go</p></td>
            <td valign="top"><p>V1</p></td>
 			<td valign="top"><p>Quelconque</p></td>  
        </tr>        
    </tbody>
</table>                


DocumentDB permet un ensemble complet d'opérations de base de données, notamment des requêtes, des requêtes avec des fonctions définies par l'utilisateur, des procédures stockées et des déclencheurs. Le coût de traitement associé à chacune de ces opérations varie selon le processeur, les E/S et la mémoire nécessaires à l'exécution de l'opération. Plutôt que de vous soucier de la gestion des ressources matérielles, vous pouvez considérer une unité de demande comme une mesure unique des ressources nécessaires à l'exécution des opérations de base de données et à la réponse à une demande de l'application.

Vous pouvez créer des collections via le [portail Microsoft Azure](https://portal.azure.com), l’[API REST](https://msdn.microsoft.com/library/azure/mt489078.aspx) ou l’un des [Kits de développement logiciel (SDK) DocumentDB](https://msdn.microsoft.com/library/azure/dn781482.aspx). Les API DocumentDB vous permettent de spécifier le niveau de performances d'une collection.

> [AZURE.NOTE] Le niveau de performances d’une collection peut être ajusté via les API ou le [portail Microsoft Azure](https://portal.azure.com/). Les modifications au niveau des performances sont censées se terminer en 3 minutes.

## Définition des niveaux de performances pour les collections
Lorsqu'une collection est créée, l'affectation complète des unités de demande est réservée pour la collection, en fonction du niveau de performances désigné.

Notez qu’avec les niveaux de performances prédéfinis et définis par l’utilisateur, DocumentDB fonctionne selon la réservation de débit. En créant une collection, une application a réservé et est facturée pour le débit réservé, quelle que soit la quantité de débit utilisée activement. Avec les niveaux de performances définis par l’utilisateur, le stockage est mesuré en fonction de la consommation, mais avec les niveaux de performances prédéfinis, 10 Go de stockage est réservé au moment de la création de la collection.

Après la création de collections, vous pouvez modifier le niveau de performances via les Kits de développement logiciel (SDK) DocumentDB ou via le portail Azure Classic.

> [AZURE.IMPORTANT] Les collections DocumentDB standard sont facturées au tarif horaire et chaque collection que vous créez sera facturée pour une heure d'utilisation au minimum.

Si vous ajustez le niveau de performances d'une collection pendant une période d'une heure, vous serez facturé pour le plus haut niveau de performances défini pendant cette période. Par exemple, si vous augmentez votre niveau de performances pour une collection à 8 h 53, vous serez facturé pour le nouveau niveau à partir de 8 h 00. De même, si vous diminuez le niveau de performances à 8 h 53, le nouveau taux s'appliquera à partir de 9 h 00.

Les unités de demande sont réservées pour chaque collection sur la base du niveau de performances défini. La consommation d'unités de demande est évaluée sous la forme d'un taux par seconde. Les applications qui dépassent le taux d'unités de demande (ou le niveau de performances) configuré sur une collection sont limitées jusqu'à ce que le taux tombe sous le niveau réservé pour cette collection. Si votre application requiert un niveau de débit plus élevé, vous pouvez augmenter le niveau de performances pour chaque collection.

> [AZURE.NOTE] Lorsque votre application dépasse les niveaux de performances d'une ou plusieurs collections, les demandes sont limitées en fonction de chaque collection. Cela signifie que certaines demandes d'application peuvent réussir tandis que d'autres peuvent être limitées. Il est recommandé d’ajouter un petit nombre de nouvelles tentatives en cas de limitation, pour gérer les hausses du trafic des demandes.

## Utilisation des niveaux de performances
Les collections DocumentDB vous permettent de regrouper vos données selon les modèles de requête et les besoins de performances de votre application. Avec l'indexation automatique et la prise en charge des requêtes dans DocumentDB, il est assez courant colocaliser des documents hétérogènes au sein de la même collection. Les points clés de la décision quant à l’utilisation de collections distinctes sont les suivants :

- Requêtes - Une collection est l'étendue pour l'exécution des requêtes. Si vous avez besoin d'interroger un ensemble de documents, la colocalisation des documents dans une collection unique donne les modèles de lecture les plus efficaces.
- Transactions : toutes les transactions sont limitées à une collection unique. Si vous avez des documents qui doivent être mis à jour au sein d’une seule procédure stockée ou d’un déclencheur, ils doivent être stockés dans la même collection. Plus précisément, une clé de partition dans une collection constitue la limite de transaction. Consultez [Partitionnement dans DocumentDB](documentdb-partition-data.md) pour plus de détails.
- Isolation des performances : à une collection est associé un niveau de performances. Cela garantit que chaque collection a des performances prévisibles via des unités de demande réservées. Les données peuvent être affectées à différentes collections, avec des niveaux de performances différents, selon la fréquence des accès.

> [AZURE.IMPORTANT] Il est important de comprendre que vous serez facturé au tarif standard en fonction du nombre de collections créées par votre application.

Il est recommandé que votre application utilise un petit nombre de collections à moins que vos besoins en termes de stockage ou de débit ne soient importants. Vérifiez que vous avez bien compris les modèles d’application pour la création de collections. Vous pouvez choisir de réserver la création de la collection comme une action de gestion gérée en dehors de votre application. De même, l'ajustement du niveau de performances pour une collection modifiera le taux horaire de facturation de la collection. Vous devez surveiller les niveaux de performances d'une collection si votre application les ajuste de manière dynamique.

## <a id="changing-performance-levels-using-the-azure-portal"></a>Remplacer S1, S2, S3 par les performances définies par l’utilisateur

Procédez comme suit pour passer de niveaux de débit prédéfinis à des niveaux de débit définis par l’utilisateur dans le portail Azure. Les niveaux de débit définis par l’utilisateur vous permettent d’adapter le débit à vos besoins. Et si vous utilisez encore un compte S1, vous pouvez augmenter le débit par défaut de 250 à 400 unités de requête/s en seulement quelques clics.

Pour plus d’informations sur la modification de la tarification pour les débits définis par l’utilisateur ou prédéfinis, consultez l’article de blog [DocumentDB : tout ce que vous devez savoir sur l’utilisation des nouvelles options de tarification](https://azure.microsoft.com/blog/documentdb-use-the-new-pricing-options-on-your-existing-collections/).

> [AZURE.VIDEO changedocumentdbcollectionperformance]

1. Dans votre navigateur, accédez au [**portail Azure**](https://portal.azure.com).
2. Cliquez sur **Parcourir** -> **Comptes DocumentDB**, puis sélectionnez le compte DocumentDB à modifier.
3. Dans le filtre **Bases de données**, sélectionnez la base de données à modifier, puis, dans le panneau **Base de données**, sélectionnez la collection à modifier. Les comptes utilisant un débit prédéfini ont un niveau tarifaire de S1, S2 ou S3.

      ![Capture d’écran de la collection S1 dans le panneau Base de données](./media/documentdb-performance-levels/documentdb-change-performance-S1.png)

4. Dans le panneau **Collections**, cliquez sur **Plus**, puis sur **Paramètres** dans la barre supérieure.
5. Dans le panneau **Paramètres**, cliquez sur **Niveau tarifaire** et notez que l’estimation des coûts mensuels pour chaque plan est affichée dans le panneau **Choisir votre niveau tarifaire**. Pour passer à un débit défini par l’utilisateur, cliquez sur **Standard**, puis cliquez sur **Sélectionner** pour enregistrer vos modifications.

      ![Capture d’écran des panneaux Paramètres DocumentDB et Choisir votre niveau tarifaire](./media/documentdb-performance-levels/documentdb-change-performance.png)

6. De retour dans le panneau **Paramètres**, le **Niveau tarifaire** est défini sur **Standard** et la zone **Débit (unités de requête/s)** est affichée avec une valeur par défaut de 400. Définissez le débit entre 400 et 10 000 [unités de requête](documentdb-request-units.md)/seconde (unités de requête/s). Le **Résumé de la tarification** au bas de la page se met à jour automatiquement pour donner une estimation du coût mensuel. Cliquez sur **OK** pour enregistrer vos modifications.

	![Capture d’écran du panneau Paramètres montrant où modifier la valeur de débit](./media/documentdb-performance-levels/documentdb-change-performance-set-thoughput.png)

7. Sur le panneau **Base de données**, vous pouvez vérifier le nouveau débit de la collection.

	![Capture d’écran d’une collection modifiée dans le panneau Base de données](./media/documentdb-performance-levels/documentdb-change-performance-confirmation.png)

Si vous déterminez que vous avez besoin d’un débit supérieur (plus de 10 000 unités de requête/s) ou d’une plus grande capacité de stockage (plus de 10 Go), vous pouvez créer une collection partitionnée. Pour créer une collection partitionnée, consultez [Créer une collection](documentdb-create-collection.md).

>[AZURE.NOTE] La modification des niveaux de performances d’une collection peut prendre jusqu’à 2 minutes.

## Modification des niveaux de performances à l’aide du Kit SDK .NET

Vous pouvez également modifier les niveaux de performances de vos collections via nos Kits SDK. Cette section couvre uniquement la modification du niveau de performances d’une collection à l’aide de notre [Kit de développement logiciel (SDK) .NET](https://msdn.microsoft.com/library/azure/dn948556.aspx), mais le processus est similaire pour nos autres [SDK](https://msdn.microsoft.com/library/azure/dn781482.aspx). Si vous ne connaissez pas notre Kit de développement logiciel (SDK) .NET, visitez notre [didacticiel de mise en route](documentdb-get-started.md).

Voici un extrait de code pour modifier le débit de l’offre sur 50 000 unités de demande par seconde :

	//Fetch the resource to be updated
	Offer offer = client.CreateOfferQuery()
		              .Where(r => r.ResourceLink == collection.SelfLink)    
		              .AsEnumerable()
		              .SingleOrDefault();

	// Set the throughput to 5000 request units per second
	offer = new OfferV2(offer, 5000);

	//Now persist these changes to the database by replacing the original resource
	await client.ReplaceOfferAsync(offer);

	// Set the throughput to S2
	offer = new Offer(offer);
	offer.OfferType = "S2";

	//Now persist these changes to the database by replacing the original resource
	await client.ReplaceOfferAsync(offer);



> [AZURE.NOTE] Les collections dotées de moins de 10 000 unités de demande par seconde peuvent être migrées entre les offres avec un débit défini par l’utilisateur et un débit prédéfini (S1, S2, S3) à tout moment. Les collections dotées de plus de 10 000 unités de demande par seconde ne peuvent pas être converties en niveaux de débit prédéfinis.

Visitez [MSDN](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.aspx) pour afficher des exemples supplémentaires et en savoir plus sur nos méthodes d’offre :

- [**ReadOfferAsync**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.readofferasync.aspx)
- [**ReadOffersFeedAsync**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.readoffersfeedasync.aspx)
- [**ReplaceOfferAsync**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.replaceofferasync.aspx)
- [**CreateOfferQuery**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createofferquery.aspx)

## <a id="change-throughput"></a>Modification du débit d’une collection

Si vous utilisez déjà les performances définies par l’utilisateur, vous pouvez modifier le débit de votre collection en procédant comme suit. Si vous avez besoin de passer d’un niveau de performance S1, S2 ou S3 (performances prédéfinies) aux performances définies par l’utilisateur, consultez [Remplacer S1, S2, S3 par les performances définies par l’utilisateur](#changing-performance-levels-using-the-azure-portal).

1. Dans votre navigateur, accédez au [**portail Azure**](https://portal.azure.com).
2. Cliquez sur **Parcourir** -> **Comptes DocumentDB**, puis sélectionnez le compte DocumentDB à modifier.
3. Dans le panneau **Compte DocumentDB**, dans le filtre **Bases de données**, sélectionnez la base de données à modifier, puis, dans le panneau **Base de données**, sélectionnez la collection à modifier.
4. Dans le panneau **Collections**, cliquez sur **Paramètres** dans la barre supérieure.
5. Dans le panneau **Paramètres**, augmentez la valeur de la zone **Débit (RU/s)**, puis cliquez sur **OK** pour enregistrer vos modifications. Le **Résumé de la tarification** en bas du panneau est mis à jour afin de vous présenter la nouvelle estimation du coût mensuel de cette collection dans une seule région.

    ![Capture d’écran du panneau Paramètres, avec mise en surbrillance de la zone Débit et du Résumé de tarification](./media/documentdb-performance-levels/documentdb-change-performance-set-thoughput.png)

Si vous n’êtes pas sûr de l’augmentation à appliquer au débit, consultez [Estimation des besoins de débit](documentdb-request-units.md#estimating-throughput-needs) et [Calculatrice d’unités de demande](https://www.documentdb.com/capacityplanner).

## Étapes suivantes

Pour en savoir plus sur la tarification et la gestion des données avec Azure DocumentDB, explorez les ressources suivantes :

- [Tarification de DocumentDB](https://azure.microsoft.com/pricing/details/documentdb/)
- [Gestion de la capacité de DocumentDB](documentdb-manage.md)
- [Modélisation des données dans DocumentDB](documentdb-modeling-data.md)
- [Partitionnement des données dans DocumentDB](documentdb-partition-data.md)
- [Unités de demande](http://go.microsoft.com/fwlink/?LinkId=735027)

Pour en savoir plus sur DocumentDB, consultez la [documentation](https://azure.microsoft.com/documentation/services/documentdb/) Azure DocumentDB.

Pour commencer avec le test des performances et de la mise à l’échelle avec DocumentDB, consultez [Test des performances et de la mise à l’échelle avec Azure DocumentDB](documentdb-performance-testing.md).

[1]: ./media/documentdb-performance-levels/documentdb-change-collection-performance7-9.png
[2]: ./media/documentdb-performance-levels/documentdb-change-collection-performance10-11.png

<!---HONumber=AcomDC_0831_2016-->