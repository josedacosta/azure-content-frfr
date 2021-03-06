<properties
	pageTitle="Prendre en main le stockage de tables et les services connectés de Visual Studio (ASP.NET) | Microsoft Azure"
	description="Comment prendre en main le stockage de tables Azure dans un projet ASP.NET dans Visual Studio après s’être connecté à un compte de stockage à l’aide des services connectés de Visual Studio"
	services="storage"
	documentationCenter=""
	authors="TomArcher"
	manager="douge"
	editor=""/>

<tags
	ms.service="storage"
	ms.workload="web"
	ms.tgt_pltfrm="vs-getting-started"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/18/2016"
	ms.author="tarcher"/>

# Prendre en main le stockage de tables et les services connectés de Visual Studio (ASP.NET)

[AZURE.INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## Vue d'ensemble
Cet article explique comment prendre en main Azure Table Storage dans Visual Studio après avoir créé ou référencé un compte de stockage Azure dans un projet ASP.NET via la boîte de dialogue **Ajouter des services connectés** de Visual Studio. Cet article vous montre comment effectuer des tâches courantes dans les tables Azure, y compris la création et la suppression d’une table, ainsi que la manipulation des entités de table. Les exemples sont écrits en code C# et utilisent la [bibliothèque cliente Microsoft Azure Storage pour .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx). Pour obtenir des informations plus générales sur l’utilisation d’Azure Table Storage, consultez la page [Prise en main du stockage de tables Azure à l’aide de .NET](storage-dotnet-how-to-use-tables.md).

Le stockage de tables Azure vous permet de stocker de grandes quantités de données structurées. Il s'agit d'une banque de données NoSQL qui accepte les appels authentifiés provenant de l'intérieur et de l'extérieur du cloud Azure. Les tables Azure sont idéales pour le stockage des données structurées non relationnelles.


## Accès aux tables dans le code

1. Vérifiez que les déclarations d’espace de noms figurant au début du fichier C# incluent ces instructions **using**.

		 using Microsoft.Azure;
		 using Microsoft.WindowsAzure.Storage;
		 using Microsoft.WindowsAzure.Storage.Auth;
		 using Microsoft.WindowsAzure.Storage.Table;

2. Obtenez un objet **CloudStorageAccount** qui représente les informations de votre compte de stockage. Le code suivant permet d'obtenir la chaîne de connexion de stockage et les informations de compte de stockage à partir de la configuration du service Azure.

		 CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
		   CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

    **REMARQUE :** placez tout le code ci-dessus avant celui des exemples suivants.

3. Obtenez un objet **CloudTableClient** pour référencer les objets de table de votre compte de stockage.

	    // Create the table client.
    	CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

4. Obtenez un objet de référence **CloudTable** pour référencer une table et des entités spécifiques.

    	// Get a reference to a table named "peopleTable"
	    CloudTable table = tableClient.GetTableReference("peopleTable");

## Création d'une table dans le code

Pour créer la table Azure, ajoutez simplement un appel à **CreateIfNotExistsAsync()** au code précédent.

	// Create the CloudTable if it does not exist
	await table.CreateIfNotExistsAsync();

## Ajout d'une entité à une table

Pour ajouter une entité à une table, commencez par créer une classe définissant les propriétés de votre entité. Le code suivant définit une classe d'entité nommée **CustomerEntity**, utilisant le prénom du client en tant que clé de ligne et son nom de famille en tant que clé de partition.

	public class CustomerEntity : TableEntity
	{
	    public CustomerEntity(string lastName, string firstName)
	    {
	        this.PartitionKey = lastName;
	        this.RowKey = firstName;
	    }

	    public CustomerEntity() { }

	    public string Email { get; set; }

	    public string PhoneNumber { get; set; }
	}

Les opérations de table impliquant des entités sont effectuées en utilisant l’objet **CloudTable** créé précédemment dans la section « Accès aux tables dans le code ». L'objet **TableOperation** représente l'opération à effectuer. L'exemple de code suivant montre comment créer des objets **CloudTable** et **CustomerEntity**. Pour préparer l’opération, un objet **TableOperation** est créé pour insérer l’entité du client dans la table. Enfin, l'opération est exécutée en appelant CloudTable.ExecuteAsync.

	// Create a new customer entity.
	CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
	customer1.Email = "Walter@contoso.com";
	customer1.PhoneNumber = "425-555-0101";

	// Create the TableOperation that inserts the customer entity.
	TableOperation insertOperation = TableOperation.Insert(customer1);

	// Execute the insert operation.
	await peopleTable.ExecuteAsync(insertOperation);

## Insertion d'un lot d'entités

Vous pouvez insérer plusieurs entités dans une table en une seule opération d'écriture. L’exemple de code suivant crée deux objets d’entité (« Jeff Smith » et « Ben Smith »), les ajoute à un objet **TableBatchOperation** en utilisant la méthode Insert, puis démarre l’opération en appelant **CloudTable.ExecuteBatchAsync**.

	// Create the batch operation.
	TableBatchOperation batchOperation = new TableBatchOperation();

	// Create a customer entity and add it to the table.
	CustomerEntity customer1 = new CustomerEntity("Smith", "Jeff");
	customer1.Email = "Jeff@contoso.com";
	customer1.PhoneNumber = "425-555-0104";

	// Create another customer entity and add it to the table.
	CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
	customer2.Email = "Ben@contoso.com";
	customer2.PhoneNumber = "425-555-0102";

	// Add both customer entities to the batch insert operation.
	batchOperation.Insert(customer1);
	batchOperation.Insert(customer2);

	// Execute the batch operation.
	await peopleTable.ExecuteBatchAsync(batchOperation);

## Recherche de toutes les entités d'une partition
Pour exécuter une requête de table portant sur toutes les entités d'une partition, utilisez un objet **TableQuery**. L’exemple de code suivant indique un filtre pour les entités où ’Smith’ est la clé de partition. Il imprime les champs de chaque entité dans les résultats de requête vers la console.

	// Construct the query operation for all customer entities where PartitionKey="Smith".
    TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>().Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

    // Print the fields for each customer.
    TableContinuationToken token = null;
    do
    {
    	TableQuerySegment<CustomerEntity>
        resultSegment = await peopleTable.ExecuteQuerySegmentedAsync(query, token);
        token = resultSegment.ContinuationToken;

        foreach (CustomerEntity entity in resultSegment.Results)
        {
        Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
        entity.Email, entity.PhoneNumber);
        }
        } while (token != null);

        return View();


## Obtention d'une seule entité
Vous pouvez écrire une requête pour obtenir une seule entité. Le code suivant utilise un objet **TableOperation** pour spécifier le client « Ben Smith ». Cette méthode renvoie une seule entité (et non une collection). De plus, la valeur renvoyée dans **TableResult.Result** est un objet **CustomerEntity**. La méthode la plus rapide pour extraire une seule entité à partir du service de Table consiste à spécifier une clé de partition et une clé de ligne.

	// Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

	// Execute the retrieve operation.
	TableResult retrievedResult = await peopleTable.ExecuteAsync(retrieveOperation);
	
	// Print the phone number of the result.
	if (retrievedResult.Result != null)
		Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
	else
	   Console.WriteLine("The phone number could not be retrieved.");

## Suppression d'une entité
Une fois l'entité trouvée, vous pouvez la supprimer. Le code suivant recherche l'entité de client « Ben Smith », puis la supprime.

	// Create a retrieve operation that expects a customer entity.
	TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

	// Execute the operation.
	TableResult retrievedResult = peopleTable.Execute(retrieveOperation);

	// Assign the result to a CustomerEntity object.
	CustomerEntity deleteEntity = (CustomerEntity)retrievedResult.Result;

	// Create the Delete TableOperation and then execute it.
	if (deleteEntity != null)
	{
	   TableOperation deleteOperation = TableOperation.Delete(deleteEntity);

	   // Execute the operation.
	   await peopleTable.ExecuteAsync(deleteOperation);

	   Console.WriteLine("Entity deleted.");
	}

	else
	   Console.WriteLine("Couldn't delete the entity.");

## Étapes suivantes

[AZURE.INCLUDE [vs-storage-dotnet-tables-next-steps](../../includes/vs-storage-dotnet-tables-next-steps.md)]

<!---HONumber=AcomDC_0727_2016-->