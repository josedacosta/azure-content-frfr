<properties
   pageTitle="Interroger Azure SQL Data Warehouse (Visual Studio) | Microsoft Azure"
   description="Interrogez SQL Data Warehouse avec Visual Studio."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/16/2016"
   ms.author="sonyama;barbkess"/>

# Interroger Azure SQL Data Warehouse (sqlcmd) (Visual Studio)

> [AZURE.SELECTOR]
- [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
- [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
- [Visual Studio](sql-data-warehouse-query-visual-studio.md)
- [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 

Utilisez Visual Studio pour interroger Azure SQL Data Warehouse en seulement quelques minutes. Cette méthode utilise l’extension de SQL Server Data Tools (SSDT) dans Visual Studio.

## Composants requis

Pour utiliser ce didacticiel, vous avez besoin des éléments suivants :

+ Un entrepôt de données SQL existant. Pour en créer un, consultez la page [Créer un entrepôt de données SQL][].
+ SSDT pour Visual Studio. Si vous avez Visual Studio, vous en disposez probablement déjà. Pour obtenir des instructions et des options d’installation, consultez [Installation de Visual Studio et/ou SSDT][].
+ Le nom complet du serveur SQL. Pour le trouver, consultez la page [Connect to SQL Data Warehouse][] \(Connexion à SQL Data Warehouse).

## 1\. Connexion à votre SQL Data Warehouse

1. Ouvrir Visual Studio 2013 ou 2015
2. Ouvrez l’Explorateur d’objets SQL Server. Pour ce faire, sélectionnez **Affichage** > **Explorateur d’objets SQL Server**.

    ![Explorateur d’objets SQL Server][1]

3. Cliquez sur l’icône **Ajouter SQL Server**.

    ![Ajouter SQL Server][2]

4. Renseignez les champs dans la fenêtre Se connecter au serveur.

    ![Se connecter au serveur][3]

    - **Nom du serveur**. Saisissez le **nom du serveur** précédemment identifié.
    - **Authentification**. Sélectionnez **Authentification SQL Server** ou **Authentification intégrée Active Directory**.
    - **Nom d’utilisateur** et **Mot de passe**. Entrez un nom d’utilisateur et mot de passe si l’authentification SQL Server a été sélectionnée plus haut.
    - Cliquez sur **Connecter**.

5. Pour voir plus d’informations, développez votre serveur SQL Azure. Vous pouvez afficher les bases de données associées au serveur. Développez AdventureWorksDW pour voir les tables de votre exemple de base de données.

    ![Explorer AdventureWorksDW][4]

## 2\. Exécuter un exemple de requête

Maintenant qu’une connexion à votre base de données a été établie, passons à l’écriture d’une requête.

1. Cliquez avec le bouton droit sur votre base de données dans l’Explorateur d’objets SQL Server.

2. Sélectionnez **Nouvelle requête**. Une nouvelle fenêtre de requête s’ouvre.

    ![Nouvelle requête][5]

3. Copiez la requête TSQL suivante dans la fenêtre de requête :

    ```sql
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```

4. Exécutez la requête. Pour ce faire, cliquez sur la flèche verte ou utilisez le raccourci `CTRL`+`SHIFT`+`E`.

    ![Exécuter une requête][6]

5. Passez en revue les résultats de la requête. Dans cet exemple, la table FactInternetSales a 60 398 lignes.

    ![Résultats de la requête][7]

## Étapes suivantes

Comme vous pouvez à présent vous connecter et exécuter des requêtes, essayez de [visualiser les données avec Power BI][].

Pour configurer votre environnement pour l’authentification Azure Active Directory, voir [Authentification vers SQL Data Warehouse][].

<!--Arcticles-->
[Connect to SQL Data Warehouse]: sql-data-warehouse-connect-overview.md
[Créer un entrepôt de données SQL]: sql-data-warehouse-get-started-provision.md
[Installation de Visual Studio et/ou SSDT]: sql-data-warehouse-install-visual-studio.md
[Authentification vers SQL Data Warehouse]: sql-data-warehouse-authentication.md
[visualiser les données avec Power BI]: sql-data-warehouse-get-started-visualize-with-power-bi.md

<!--Other-->
[Azure portal]: https://portal.azure.com

<!--Image references-->

[1]: media/sql-data-warehouse-query-visual-studio/open-ssdt.png
[2]: media/sql-data-warehouse-query-visual-studio/add-server.png
[3]: media/sql-data-warehouse-query-visual-studio/connection-dialog.png
[4]: media/sql-data-warehouse-query-visual-studio/explore-sample.png
[5]: media/sql-data-warehouse-query-visual-studio/new-query2.png
[6]: media/sql-data-warehouse-query-visual-studio/run-query.png
[7]: media/sql-data-warehouse-query-visual-studio/query-results.png

<!---HONumber=AcomDC_0622_2016-->