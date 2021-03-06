<properties
   pageTitle="Mise en route avec le chiffrement transparent des données (TDE) dans SQL Data Warehouse | Microsoft Azure"
   description="Mise en route avec le chiffrement transparent des données (TDE) dans SQL Data Warehouse"
   services="sql-data-warehouse"
   documentationCenter=""
   authors="ronortloff"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.workload="data-management"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="08/29/2016" 
   ms.author="rortloff;barbkess;sonyama"/>

# Mise en route avec le chiffrement transparent des données (TDE) dans SQL Data Warehouse

> [AZURE.SELECTOR]
- [Présentation de la sécurité](sql-data-warehouse-overview-manage-security.md)
- [Détection de menaces](sql-data-warehouse-security-threat-detection.md)
- [Chiffrement (portail)](sql-data-warehouse-encryption-tde.md)
- [Chiffrement (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)
- [Vue d’ensemble de l’audit](sql-data-warehouse-auditing-overview.md)
- [Audit des clients de niveau inférieur](sql-data-warehouse-auditing-downlevel-clients.md)


La fonction de chiffrement transparent des données (TDE) d’Azure SQL Data Warehouse protège le système contre toute menace d’activité malveillante, en effectuant un chiffrement et un déchiffrement en temps réel de la base de données, des sauvegardes associées et des fichiers journaux de transactions au repos, sans exiger de modification de l’application.

Le chiffrement transparent des données chiffre le stockage d’une base de données entière à l’aide d’une clé symétrique appelée clé de chiffrement de base de données. Dans la base de données SQL, la clé de chiffrement de base de données est protégée par un certificat de serveur intégré. Le certificat de serveur intégré est unique pour chaque serveur de base de données SQL. Microsoft alterne automatiquement ces certificats au moins tous les 90 jours. L’algorithme de chiffrement utilisé par SQL Data Warehouse est AES-256. Pour obtenir une description générale du chiffrement transparent des données, consultez [Chiffrement transparent des données (TDE)].

##Activation du chiffrement

Pour activer le chiffrement transparent des données pour SQL Data Warehouse, procédez comme suit :

1. Ouvrez la base de données dans le [portail Azure](https://portal.azure.com)
2. Dans le panneau de la base de données, cliquez sur le bouton **Paramètres**
3. Sélectionnez l’option **Chiffrement transparent des données** ![][1]
4. Sélectionnez le paramètre **Activé** ![][2]
5. Sélectionnez **Enregistrer** ![][3]

##Désactivation du chiffrement

Pour désactiver le chiffrement transparent des données pour SQL Data Warehouse, procédez comme suit :

1. Ouvrez la base de données dans le [portail Azure](https://portal.azure.com)
2. Dans le panneau de la base de données, cliquez sur le bouton **Paramètres**
3. Sélectionnez l’option **Chiffrement transparent des données** ![][1]
4. Sélectionnez le paramètre **Désactivé** ![][4]
5. Sélectionnez **Enregistrer** ![][5]

##DMV de chiffrement

Le chiffrement peut être vérifié avec les vues DMV suivantes :

- [sys.databases]
- [sys.dm\_pdw\_nodes\_database\_encryption\_keys]

<!--MSDN references-->
[Chiffrement transparent des données (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[sys.databases]: http://msdn.microsoft.com/library/ms178534.aspx
[sys.dm\_pdw\_nodes\_database\_encryption\_keys]: https://msdn.microsoft.com/library/mt203922.aspx

<!--Image references-->
[1]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings.png
[2]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-on.png
[3]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save.png
[4]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-off.png
[5]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save2.png

<!--Link references-->

<!---HONumber=AcomDC_0907_2016-->