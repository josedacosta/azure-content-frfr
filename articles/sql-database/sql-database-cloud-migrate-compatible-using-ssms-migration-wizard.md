<properties
   pageTitle="Migration d’une base de données SQL Server vers une base de données SQL à l’aide de l’Assistant de déploiement de base de données vers une base de données Microsoft Azure | Microsoft Azure"
   description="Base de données SQL Microsoft Azure, migration de base de données, Assistant de base de données Microsoft Azure"
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sqldb-migrate"
   ms.date="08/24/2016"
   ms.author="carlrab"/>

# Migration d’une base de données SQL Server vers Base de données SQL à l’aide de l’assistant de déploiement de base de données vers Microsoft Azure Database


> [AZURE.SELECTOR]
- [Assistant Migration SSMS](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md)
- [Exporter vers un fichier BACPAC](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [Importer depuis un fichier BACPAC](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [Réplication transactionnelle](sql-database-cloud-migrate-compatible-using-transactional-replication.md)

Cet assistant, disponible dans SQL Server Management Studio, permet de migrer une [base de données SQL Server compatible](sql-database-cloud-migrate.md) directement dans votre serveur Base de données SQL Microsoft Azure.

## Utiliser l’assistant de déploiement de base de données dans une base de données Microsoft Azure

> [AZURE.NOTE] Les étapes suivantes supposent que vous avez un [serveur Base de données SQL approvisionné](https://azure.microsoft.com/documentation/learning-paths/sql-database-training-learn-sql-database/).

1. Assurez-vous de disposer de la dernière version de SQL Server Management Studio. Les nouvelles versions de Management Studio sont mises à jour tous les mois afin de refléter les mises à jour publiées sur le portail Azure.

    > [AZURE.IMPORTANT] Nous vous recommandons d’utiliser systématiquement la dernière version de Management Studio afin de rester en cohérence avec les mises à jour de Microsoft Azure et Base de données SQL. [Mettre à jour SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).

2. Ouvrez Management Studio et connectez-vous à votre base de données SQL Server pour migrer dans l’Explorateur d’objets.
3. Dans l’Explorateur d’objets, cliquez avec le bouton droit de la souris sur la base de données, pointez sur **Tâches**, puis cliquez sur **Déployer une base de données sur Base de données SQL Microsoft Azure…**

	![Déployer vers Azure à partir du menu Tâches](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard01.png)

4.	Dans l’assistant de déploiement, cliquez sur **Suivant**, puis cliquez sur **Connecter** pour configurer la connexion à votre serveur Base de données SQL.

	![Déployer vers Azure à partir du menu Tâches](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard002.png)

5. Dans la boîte de dialogue Se connecter au serveur, entrez vos informations de connexion à votre serveur Base de données SQL.

	![Déployer vers Azure à partir du menu Tâches](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard00.png)

5.	Fournissez les informations suivantes pour le fichier [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) que cet Assistant crée pendant le processus de migration :

 - Le **Nouveau nom de la base de données**
 - **L’Édition de Microsoft Azure SQL Database** ([niveau de service](sql-database-service-tiers.md))
 - La **Taille maximale de base de données**
 - **L’objectif du service** (niveau de performances)
 - Le **nom du fichier temporaire**

    ![Paramètres d’exportation](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard02.png)

6.	Terminez l’Assistant. Selon la taille et la complexité de la base de données, le déploiement peut durer de quelques minutes à plusieurs heures. Si cet assistant détecte des problèmes de compatibilité, des erreurs seront affichées à l’écran et la migration s’arrête. Pour obtenir des instructions sur la résolution des problèmes de compatibilité de base de données, consultez la page [Résoudre les problèmes de compatibilité de base de données](sql-database-cloud-migrate-fix-compatibility-issues.md).

7.	À l’aide de l’Explorateur d’objets, connectez-vous à la base de données que vous venez de déployer sur votre serveur de base de données SQL Azure.
8.	Dans le portail Azure, affichez votre base de données et ses propriétés.

## Étapes suivantes

- [Version la plus récente de SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Version la plus récente de SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)

## Ressources supplémentaires

- [Base de données SQL V12](sql-database-v12-whats-new.md)
- [Fonctions partiellement ou non prises en charge de Transact-SQL](sql-database-transact-sql-information.md)
- [Migration de bases de données non-SQL Server avec l’Assistant Migration SQL Server](http://blogs.msdn.com/b/ssma/)

<!---HONumber=AcomDC_0831_2016-->