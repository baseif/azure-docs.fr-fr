---
title: "Se connecter à Azure SQL Database à l’aide de Ruby | Microsoft Docs"
description: "Cet article présente un exemple de code Ruby que vous pouvez utiliser pour vous connecter à la base de données Azure SQL Database et pour interroger cette dernière."
services: sql-database
documentationcenter: 
author: ajlam
manager: jhubbard
editor: 
ms.assetid: 94fec528-58ba-4352-ba0d-25ae4b273e90
ms.service: sql-database
ms.custom: quick start connect
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 04/17/2017
ms.author: andrela;sstein;carlrab
translationtype: Human Translation
ms.sourcegitcommit: db7cb109a0131beee9beae4958232e1ec5a1d730
ms.openlocfilehash: 04fc29fe21b77a34094bb89ecb8496f78856f56b
ms.lasthandoff: 04/18/2017


---

# <a name="azure-sql-database-use-ruby-to-connect-and-query-data"></a>Azure SQL Database : utilisez Ruby pour vous connecter et interroger des données

Ce guide de démarrage rapide indique comment utiliser [Ruby](https://Ruby.org) pour se connecter à une base de données SQL Azure, puis utiliser les instructions Transact-SQL pour interroger, insérer, mettre à jour et supprimer des données dans la base de données à partir des plateformes Mac OS et Ubuntu Linux.

Ce guide de démarrage rapide utilise comme point de départ les ressources créées dans l’une de ces instructions de démarrage rapide :

- [Créer une base de données - Portail](sql-database-get-started-portal.md)
- [Créer une base de données - CLI](sql-database-get-started-cli.md)

## <a name="install-ruby-and-database-communication-libraries"></a>Installer les bibliothèques de communication de base de données et Ruby

### <a name="mac-os"></a>**Mac OS**
Ouvrez votre terminal et accédez au répertoire dans lequel vous envisagez de créer votre script Ruby. Entrez les commandes suivantes pour installer **brew**, **FreeTDS** et **TinyTDS**.

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew tap microsoft/msodbcsql https://github.com/Microsoft/homebrew-msodbcsql-preview
brew update
brew install FreeTDS
gem install tiny_tds
```

### <a name="linux-ubuntu"></a>**Linux (Ubuntu)**
Ouvrez votre terminal et accédez au répertoire dans lequel vous envisagez de créer votre script Ruby. Entrez les commandes suivantes pour installer **FreeTDS** et **pymssql**.

```bash
wget ftp://ftp.freetds.org/pub/freetds/stable/freetds-1.00.27.tar.gz
tar -xzf freetds-1.00.27.tar.gz
cd freetds-1.00.27
./configure --prefix=/usr/local --with-tdsver=7.3
make
make install
gem install tiny_tds
```

## <a name="get-connection-information"></a>Obtenir des informations de connexion

Récupérez la chaîne de connexion dans le portail Azure. Cette chaîne de connexion vous permet de vous connecter à la base de données SQL Azure.

1. Connectez-vous au [portail Azure](https://portal.azure.com/).
2. Sélectionnez **Bases de données SQL** dans le menu de gauche, puis cliquez sur votre base de données dans la page **Bases de données SQL**. 
3. Sur la page **Vue d’ensemble** de votre base de données, vérifiez le nom complet du serveur, comme indiqué dans l’image ci-dessous. Vous pouvez pointer sur le nom du serveur pour afficher l’option **Cliquez pour copier**. 

   ![server-name](./media/sql-database-connect-query-dotnet/server-name.png) 

4. Si vous avez oublié vos informations de connexion à votre serveur Azure SQL Database, accédez à la page du serveur SQL Database pour afficher le nom de l’administrateur du serveur et, si nécessaire, réinitialiser le mot de passe.
    

## <a name="select-data"></a>Sélectionner des données
Utilisez le code suivant pour interroger votre base de données SQL Azure à l’aide de la fonction [TinyTDS::Client](https://github.com/rails-sqlserver/tiny_tds) avec une instruction Transact-SQL [SELECT](https://docs.microsoft.com/sql/t-sql/queries/select-transact-sql). La fonction TinyTDS::Client accepte une requête et retourne un jeu de résultats. Une itération est effectuée sur le jeu de résultats en utilisant [result.each do |row|](https://github.com/rails-sqlserver/tiny_tds). Remplacez les paramètres du serveur, de la base de données, du nom d’utilisateur et du mot de passe par les valeurs spécifiées lors de la création de la base de données avec les exemples de données AdventureWorksLT.

```ruby
require 'tiny_tds'
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
client = TinyTds::Client.new username: username, password: password, 
    host: server, port: 1433, database: database, azure: true

puts "Reading data from table"
tsql = "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
        FROM [SalesLT].[ProductCategory] pc
        JOIN [SalesLT].[Product] p
        ON pc.productcategoryid = p.productcategoryid"
result = client.execute(tsql)
result.each do |row|
    puts row
end
```

## <a name="insert-data"></a>Insertion des données
Utilisez le code suivant pour insérer un nouveau produit dans la table SalesLT.Product dans la base de données spécifiée à l’aide de la fonction [TinyTDS::Client](https://github.com/rails-sqlserver/tiny_tds) avec une instruction Transact-SQL [INSERT](https://docs.microsoft.com/sql/t-sql/statements/insert-transact-sql). Remplacez les paramètres du serveur, de la base de données, du nom d’utilisateur et du mot de passe par les valeurs spécifiées lors de la création de la base de données avec les exemples de données AdventureWorksLT.

Dans cet exemple, vous allez découvrir comment exécuter une instruction INSERT en toute sécurité, passer des paramètres pour protéger votre application des vulnérabilités découlant de [l’injection de code SQL](https://technet.microsoft.com/library/ms161953(v=sql.105).aspx) et récupérer la valeur de la [Clé primaire](https://docs.microsoft.com/sql/relational-databases/tables/primary-and-foreign-key-constraints) générée automatiquement.    
  
Pour utiliser TinyTDS avec Azure, il est recommandé d’exécuter plusieurs instructions `SET` pour modifier la façon dont la session en cours gère des informations spécifiques. Les instructions `SET` recommandées sont fournies dans l’exemple de code. Par exemple, `SET ANSI_NULL_DFLT_ON` permet aux nouvelles colonnes créées d’autoriser les valeurs Null même si la possibilité d’utiliser ces valeurs dans ces colonnes n’est pas explicitement définie.  
  
Pour être en harmonie avec le format [datetime](https://docs.microsoft.com/sql/t-sql/data-types/datetime-transact-sql) (date et heure) de Microsoft SQL Server, utilisez la fonction [strftime](http://ruby-doc.org/core-2.2.0/Time.html#method-i-strftime) pour effectuer une conversion au format datetime correspondant.

```ruby
require 'tiny_tds'
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
client = TinyTds::Client.new username: username, password: password, 
    host: server, port: 1433, database: database, azure: true

# settings for Azure
result = client.execute("SET ANSI_NULLS ON")
result = client.execute("SET CURSOR_CLOSE_ON_COMMIT OFF")
result = client.execute("SET ANSI_NULL_DFLT_ON ON")
result = client.execute("SET IMPLICIT_TRANSACTIONS OFF")
result = client.execute("SET ANSI_PADDING ON")
result = client.execute("SET QUOTED_IDENTIFIER ON")
result = client.execute("SET ANSI_WARNINGS ON")
result = client.execute("SET CONCAT_NULL_YIELDS_NULL ON")

def insert(name, productnumber, color, standardcost, listprice, sellstartdate)
    tsql = "INSERT INTO SalesLT.Product (Name, ProductNumber, Color, StandardCost, ListPrice, SellStartDate) 
        VALUES (N'#{name}', N'#{productnumber}',N'#{color}',N'#{standardcost}',N'#{listprice}',N'#{sellstartdate}')"
    result = client.execute(tsql)
    result.each
    puts "#{result.affected_rows} row(s) affected"
end
insert('BrandNewProduct', '200989', 'Blue', 75, 80, '7/1/2016')
```

## <a name="update-data"></a>Mettre à jour des données
Utilisez le code suivant pour mettre à jour les données de votre base de données SQL Azure à l’aide de la fonction [TinyTDS::Client](https://github.com/rails-sqlserver/tiny_tds) avec une instruction Transact-SQL [UPDATE](https://docs.microsoft.com/sql/t-sql/queries/update-transact-sql). Remplacez les paramètres du serveur, de la base de données, du nom d’utilisateur et du mot de passe par les valeurs spécifiées lors de la création de la base de données avec les exemples de données AdventureWorksLT.

```ruby
require 'tiny_tds'
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
client = TinyTds::Client.new username: username, password: password, 
    host: server, port: 1433, database: database, azure: true
    
def update(name, listPrice, client)
    tsql = "UPDATE SalesLT.Product SET ListPrice = N'#{listPrice}' WHERE Name =N'#{name}'";
    result = client.execute(tsql)
    result.each
    puts "#{result.affected_rows} row(s) affected"
end
update('BrandNewProduct', 500, client)
```

## <a name="delete-data"></a>Suppression de données
Utilisez le code suivant pour supprimer des données de votre base de données SQL Azure à l’aide de la fonction [TinyTDS::Client](https://github.com/rails-sqlserver/tiny_tds) avec une instruction Transact-SQL [DELETE](https://docs.microsoft.com/sql/t-sql/statements/delete-transact-sql). Remplacez les paramètres du serveur, de la base de données, du nom d’utilisateur et du mot de passe par les valeurs spécifiées lors de la création de la base de données avec les exemples de données AdventureWorksLT.

```ruby
require 'tiny_tds'
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
client = TinyTds::Client.new username: username, password: password, 
    host: server, port: 1433, database: database, azure: true

# settings for Azure
result = client.execute("SET ANSI_NULLS ON")
result = client.execute("SET CURSOR_CLOSE_ON_COMMIT OFF")
result = client.execute("SET ANSI_NULL_DFLT_ON ON")
result = client.execute("SET IMPLICIT_TRANSACTIONS OFF")
result = client.execute("SET ANSI_PADDING ON")
result = client.execute("SET QUOTED_IDENTIFIER ON")
result = client.execute("SET ANSI_WARNINGS ON")
result = client.execute("SET CONCAT_NULL_YIELDS_NULL ON")

def delete(name, client)
    tsql = "DELETE FROM SalesLT.Product WHERE Name = N'#{name}'"
    result = client.execute(tsql)
    result.each
    puts "#{result.affected_rows} row(s) affected"
end
delete('BrandNewProduct', client)
```

## <a name="next-steps"></a>Étapes suivantes

- Référentiel Github pour [TinyTDS](https://github.com/rails-sqlserver/tiny_tds).
- [Problèmes avec les fichiers/posez des questions](https://github.com/rails-sqlserver/tiny_tds/issues).        
- Plus d’informations sur le [pilote Ruby pour SQL Server](https://docs.microsoft.com/sql/connect/ruby/ruby-driver-for-sql-server/).
- Pour vous connecter et envoyer des requêtes à l’aide de SQL Server Management Studio, consultez [Se connecter à la base de données SQL avec SQL Server Management Studio et exécuter un exemple de requête T-SQL](sql-database-connect-query-ssms.md).
- Pour vous connecter et exécuter des requêtes à l’aide de Visual Studio Code, consultez l’article décrivant comment [se connecter et effectuer des requêtes avec Visual Studio Code](sql-database-connect-query-vscode.md).
- Pour vous connecter et exécuter des requêtes avec .NET, consultez la page [Se connecter et effectuer des requêtes avec .NET](sql-database-connect-query-dotnet.md).
- Pour vous connecter et exécuter des requêtes avec PHP, consultez la page [se connecter et effectuer des requêtes avec PHP](sql-database-connect-query-php.md).
- Pour vous connecter et exécuter des requêtes avec Node.js, consultez la page [se connecter et effectuer des requêtes avec Node.js](sql-database-connect-query-nodejs.md).
- Pour vous connecter et exécuter des requêtes avec Java, consultez la page [se connecter et effectuer des requêtes avec Java](sql-database-connect-query-java.md).
- Pour vous connecter et exécuter des requêtes avec Python, consultez la page [se connecter et effectuer des requêtes avec Python](sql-database-connect-query-python.md).
