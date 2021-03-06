<properties
   pageTitle="Pilotes pour SQL Data Warehouse | Microsoft Azure"
   description="Chaînes de connexion et pilotes pour SQL Data Warehouse"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/01/2016"
   ms.author="sonyama;barbkess"/>


# Pilotes pour Azure SQL Data Warehouse

> [AZURE.SELECTOR]
- [Vue d'ensemble](sql-data-warehouse-connect-overview.md)
- [Authentification](sql-data-warehouse-authentication.md)
- [Pilotes](sql-data-warehouse-connection-strings.md)


Vous pouvez vous connecter à SQL Data Warehouse avec plusieurs protocoles d’application différents tels que [ADO.NET][], [ODBC][], [PHP][] et [JDBC][]. Voici quelques exemples de chaînes de connexion pour chaque protocole. Vous pouvez également utiliser le portail Azure pour créer votre chaîne de connexion. Pour créer votre chaîne de connexion à l’aide du portail Azure, accédez au panneau de votre base de données, sous *Essentials*, cliquez sur *Afficher les chaînes de connexion de la base de données*.

## Exemple de chaîne de connexion ADO.NET

```C#
Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};User ID={your_user_name};Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
```

## Exemple de chaîne de connexion ODBC

```C#
Driver={SQL Server Native Client 11.0};Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};Uid={your_user_name};Pwd={your_password_here};Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;
```

## Exemple de chaîne de connexion PHP

```PHP
Server: {your_server}.database.windows.net,1433 \r\nSQL Database: {your_database}\r\nUser Name: {your_user_name}\r\n\r\nPHP Data Objects(PDO) Sample Code:\r\n\r\ntry {\r\n   $conn = new PDO ( "sqlsrv:server = tcp:{your_server}.database.windows.net,1433; Database = {your_database}", "{your_user_name}", "{your_password_here}");\r\n    $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );\r\n}\r\ncatch ( PDOException $e ) {\r\n   print( "Error connecting to SQL Server." );\r\n   die(print_r($e));\r\n}\r\n\rSQL Server Extension Sample Code:\r\n\r\n$connectionInfo = array("UID" => "{your_user_name}", "pwd" => "{your_password_here}", "Database" => "{your_database}", "LoginTimeout" => 30, "Encrypt" => 1, "TrustServerCertificate" => 0);\r\n$serverName = "tcp:{your_server}.database.windows.net,1433";\r\n$conn = sqlsrv_connect($serverName, $connectionInfo);
```

## Exemple de chaîne de connexion JDBC

```Java
jdbc:sqlserver://yourserver.database.windows.net:1433;database=yourdatabase;user={your_user_name};password={your_password_here};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
```

> [AZURE.NOTE] Vous pouvez définir le délai de connexion à 300 secondes pour permettre à la connexion de résister à des courtes périodes d’indisponibilité.

## Étapes suivantes

Pour commencer à interroger votre entrepôt de données avec Visual Studio et d’autres applications, consultez [Query with Visual Studio (Soumettre des requêtes avec Visual Studio)][].

<!--Image references-->

<!--Azure.com references-->
 [Query with Visual Studio (Soumettre des requêtes avec Visual Studio)]: ./sql-data-warehouse-query-visual-studio.md
 
<!--MSDN references-->
[ADO.NET]: https://msdn.microsoft.com/library/e80y5yhx(v=vs.110).aspx
[ODBC]: https://msdn.microsoft.com/library/jj730314.aspx
[PHP]: https://msdn.microsoft.com/library/cc296172.aspx?f=255&MSPPError=-2147217396
[JDBC]: https://msdn.microsoft.com/library/mt484311(v=sql.110).aspx

<!--Other references-->

<!---HONumber=AcomDC_0803_2016-->