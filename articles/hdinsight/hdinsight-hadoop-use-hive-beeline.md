---
title: Utiliser Beeline avec Hive sur HDInsight (Hadoop) | Microsoft Docs
description: "Découvrez comment utiliser SSH pour vous connecter à un cluster Hadoop dans HDInsight, puis envoyer de manière interactive des requêtes Hive à l’aide de Beeline. Beeline est un utilitaire qui permet d’utiliser HiveServer2 sur JDBC."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 3adfb1ba-8924-4a13-98db-10a67ab24fca
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/05/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: 7a1757a7ef2881b9e09389745a8e5aeaea2abf9d
ms.lasthandoff: 04/12/2017


---
# <a name="use-hive-with-hadoop-in-hdinsight-with-beeline"></a>Utilisation de Hive avec Hadoop dans HDInsight via Beeline
[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

Découvrez comment utiliser [Beeline](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients#HiveServer2Clients-Beeline–NewCommandLineShell) pour exécuter des requêtes Hive sur HDInsight.

Beeline est un outil en ligne de commande inclus dans les nœuds principaux de votre cluster HDInsight. Il utilise JDBC pour se connecter à HiveServer2, un service hébergé sur votre cluster HDInsight. Le tableau suivant fournit des chaînes de connexion utilisables avec Beeline :

| Emplacement d’exécution de Beeline | Chaîne de connexion | Autres paramètres |
| --- | --- | --- |
| Une connexion SSH à un nœud principal | `jdbc:hive2://localhost:10001/;transportMode=http` | `-n admin` |
| Un nœud de périmètre | `jdbc:hive2://headnodehost:10001/;transportMode=http` | `-n admin` |
| En dehors du cluster | `jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2` | `-n admin -p password` |

> [!NOTE]
> Remplacez « admin » par le compte de connexion de votre cluster.
>
> Remplacez « password » par le mot de passe du compte de connexion du cluster.
>
> Remplacez `clustername` par le nom de votre cluster HDInsight :

## <a id="prereq"></a>Configuration requise

* Un cluster Hadoop Linux sur HDInsight.

  > [!IMPORTANT]
  > Linux est le seul système d’exploitation utilisé sur HDInsight version 3.4 ou supérieure. Pour plus d’informations, consultez la page [Dépréciation d’HDInsight versions 3.2 et 3.3](hdinsight-component-versioning.md#hdi-version-33-nearing-deprecation-date).

* Un client SSH. Pour plus d’informations sur l’utilisation de SSH, voir [Utilisation de SSH avec HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

## <a id="ssh"></a>Connexion avec SSH

Connectez-vous à votre cluster via SSH avec la commande suivante :

```bash
ssh sshuser@myhdinsight-ssh.azurehdinsight.net
```

Remplacez `sshuser` par le compte SSH de votre cluster. Remplacez `myhdinsight` par le nom de votre cluster HDInsight :

**Si vous avez fourni un mot de passe pour l’authentification SSH** lorsque vous avez créé le cluster HDInsight, vous devez fournir le mot de passe lorsque vous y êtes invité.

**Si vous avez fourni une clé de certificat pour l’authentification SSH** lorsque vous avez créé le cluster HDInsight, vous devez spécifier l’emplacement de la clé privée sur votre système client :

    ssh -i ~/.ssh/mykey.key ssh@myhdinsight-ssh.azurehdinsight.net

Pour en savoir plus sur l’utilisation de SSH avec HDInsight, voir [Utilisation de SSH avec HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

## <a id="beeline"></a>Utilisez la commande Beeline

1. Dans la session SSH, utilisez la commande suivante pour démarrer Beeline :

    ```bash
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin
    ```

    Cette commande démarre le client Beeline et se connecte à HiveServer2 sur le nœud principal du cluster. Le paramètre `-n` est utilisé pour fournir le compte de connexion du cluster. La connexion par défaut est `admin`. Si vous avez utilisé un autre nom lors de la création du cluster, utilisez-le à la place de `admin`.

    Une fois la commande terminée, vous arrivez à une invite `jdbc:hive2://localhost:10001/>`.

2. Les commandes Beeline commencent par un caractère `!`. Par exemple, `!help` affiche l’aide. Toutefois, `!` peut être omis pour certaines commandes. Par exemple, `help` fonctionne également.

    `!sql` est utilisé pour exécuter des instructions HiveQL. Cependant, HiveQL est si souvent utilisé que vous pouvez omettre le `!sql`qui précède. Les deux instructions suivantes sont équivalentes :

    ```hiveql
    !sql show tables;
    show tables;
    ```

    Sur un nouveau cluster, une seule table est listée : **hivesampletable**.

3. Utilisez la commande suivante pour afficher le schéma correspondant à hivesampletable :

    ```bash
    describe hivesampletable;
    ```

    Cette commande renvoie les informations suivantes :

        +-----------------------+------------+----------+--+
        |       col_name        | data_type  | comment  |
        +-----------------------+------------+----------+--+
        | clientid              | string     |          |
        | querytime             | string     |          |
        | market                | string     |          |
        | deviceplatform        | string     |          |
        | devicemake            | string     |          |
        | devicemodel           | string     |          |
        | state                 | string     |          |
        | country               | string     |          |
        | querydwelltime        | double     |          |
        | sessionid             | bigint     |          |
        | sessionpagevieworder  | bigint     |          |
        +-----------------------+------------+----------+--+

    Ces informations décrivent les colonnes de la table. Nous pourrions exécuter des requêtes sur ces données, mais nous allons créer à la place une toute nouvelle table pour montrer comment charger des données dans Hive et appliquer un schéma.

4. Saisissez les instructions suivantes pour créer une table nommée **log4jLogs** avec les exemples de données fournies avec le cluster HDInsight :

    ```hiveql
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
    SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;
    ```

    Ces instructions effectuent les opérations suivantes :

    * **DROP TABLE** : si la table existe, elle est supprimée.

    * **CREATE EXTERNAL TABLE** : crée une table **externe** dans Hive. Les tables externes stockent uniquement la définition de table dans Hive. Les données restent à l'emplacement d'origine.

    * **ROW FORMAT** : le mode de formatage des données. Dans ce cas, les champs de chaque journal sont séparés par un espace.

    * **STORED AS TEXTFILE LOCATION** : emplacement où sont stockées les données, et format de fichier.

    * **SELECT** : sélectionne toutes les lignes dont la colonne **t4** contient la valeur **[ERROR]**. Cette requête renvoie la valeur **3**, car trois lignes contiennent cette valeur.

    * **INPUT__FILE__NAME LIKE ’%.log’** : l’exemple de fichier de données est stocké avec d’autres fichiers. Cette instruction limite la requête aux données stockées dans des fichiers qui se terminent par .log.

  > [!NOTE]
  > Les tables externes doivent être utilisées lorsque vous vous attendez à ce que les données sous-jacentes soient mises à jour par une source externe, Par exemple, un processus de chargement de données automatisé ou une opération MapReduce.
  >
  > La suppression d'une table externe ne supprime **pas** les données, mais seulement la définition de la table.

    Le résultat de la commande se présente ainsi :

        INFO  : Tez session hasn't been created yet. Opening session
        INFO  :

        INFO  : Status: Running (Executing on YARN cluster with App id application_1443698635933_0001)

        INFO  : Map 1: -/-      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0(+1)/1  Reducer 2: 0/1
        INFO  : Map 1: 0(+1)/1  Reducer 2: 0/1
        INFO  : Map 1: 1/1      Reducer 2: 0/1
        INFO  : Map 1: 1/1      Reducer 2: 0(+1)/1
        INFO  : Map 1: 1/1      Reducer 2: 1/1
        +----------+--------+--+
        |   sev    | count  |
        +----------+--------+--+
        | [ERROR]  | 3      |
        +----------+--------+--+
        1 row selected (47.351 seconds)

5. Pour quitter Beeline, utilisez `!exit`.

## <a id="file"></a>Exécuter un fichier HiveQL

Utilisez les étapes suivantes pour créer un fichier, puis exécutez-le à l’aide de Beeline.

1. Utilisez la commande suivante pour créer un fichier nommé **query.hql**:

    ```bash
    nano query.hql
    ```

2. Utilisez le texte ci-après comme contenu du fichier. Cette requête crée une table « interne » nommée **errorLogs** :

    ```hiveql
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';
    ```

    Ces instructions effectuent les opérations suivantes :

    * **CREATE TABLE IF NOT EXISTS** : si la table n’existe pas, elle est créée. Étant donné que le mot-clé **EXTERNAL** n’est pas utilisé, cette instruction crée une table interne. Les tables internes sont stockées dans l’entrepôt de données Hive et entièrement gérées par Hive.
    * **STORED AS ORC** : stocke les données au format ORC (Optimized Row Columnar). Le format ORC est particulièrement efficace et optimisé pour le stockage de données Hive.
    * **INSERT OVERWRITE ... SELECT** : sélectionne des lignes de la table **log4jLogs** qui contiennent **[ERROR]**, puis insère les données dans la table **errorLogs**.

    > [!NOTE]
    > Contrairement aux tables externes, la suppression d’une table interne entraîne également la suppression des données sous-jacentes.

3. Pour enregistrer le fichier, utilisez **Ctrl**+_+**X**, saisissez ensuite **Y**, puis appuyez sur **Entrée**.

4. Pour exécuter le fichier à l’aide de Beeline, utilisez les éléments suivants. Remplacez **HOSTNAME** par le nom obtenu précédemment pour le nœud principal, et **PASSWORD** par le mot de passe du compte d’administration :

    ```bash
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin -i query.hql
    ```

    > [!NOTE]
    > Le paramètre `-i` démarre Beeline et exécute les instructions dans le fichier query.hql. Une fois la requête terminée, vous accédez à l’invite `jdbc:hive2://localhost:10001/>`. Vous pouvez également exécuter un fichier avec le paramètre `-f`, qui ferme Beeline une fois la requête terminée.

5. Pour vérifier que la table **errorLogs** a été créée, utilisez l’instruction suivante pour renvoyer toutes les lignes à partir **d’errorLogs** :

    ```hiveql
    SELECT * from errorLogs;
    ```

    Trois lignes de données doivent normalement être renvoyées. Elles contiennent toutes **[ERROR]** dans la colonne t4 :

        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | errorlogs.t1  | errorlogs.t2  | errorlogs.t3  | errorlogs.t4  | errorlogs.t5  | errorlogs.t6  | errorlogs.t7  |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | 2012-02-03    | 18:35:34      | SampleClass0  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 18:55:54      | SampleClass1  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 19:25:27      | SampleClass4  | [ERROR]       | incorrect     | id            |               |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        3 rows selected (1.538 seconds)

## <a name="edge-nodes"></a>Nœuds de périmètre

Si votre cluster possède un nœud de périmètre, nous vous recommandons de toujours l’utiliser, plutôt que le nœud principal, lors de la connexion via SSH. Pour lancer Beeline à partir d’une connexion SSH à un nœud de périmètre, utilisez la commande suivante :

```bash
beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http' -n admin
```

## <a name="remote-clients"></a>Clients distants

Si Beeline est installé localement, ou que vous l’utilisez dans une image Docker comme [sutoiku/beeline](https://hub.docker.com/r/sutoiku/beeline/), vous devez utiliser les paramètres suivants :

* __Chaîne de connexion__ : `-u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2'`

* __Nom de connexion du cluster__ : `-n admin`

* __Mot de passe de connexion du cluster__ : `-p 'password'`

Remplacez le `clustername` de la chaîne de connexion par le nom de votre cluster HDInsight.

Remplacez `admin` par le nom de connexion de votre cluster, puis remplacez `password` par le mot de passe de votre cluster.

## <a id="summary"></a><a id="nextsteps"></a>Étapes suivantes

Pour plus d’informations générales sur Hive sur HDInsight, consultez le document suivant :

* [Utilisation de Hive avec Hadoop sur HDInsight](hdinsight-use-hive.md)

Pour plus d’informations sur les autres façons de travailler avec Hadoop sur HDInsight, consultez les documents suivants :

* [Utilisation de Pig avec Hadoop sur HDInsight](hdinsight-use-pig.md)
* [Utilisation de MapReduce avec Hadoop sur HDInsight](hdinsight-use-mapreduce.md)

Si vous utilisez Tez avec Hive, consultez les documents suivants :

* [Utiliser l'interface utilisateur Tez sur HDInsight Windows](hdinsight-debug-tez-ui.md)
* [Utilisez la vue Tez Ambari sur HDInsight Linux](hdinsight-debug-ambari-tez-view.md)

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md

[putty]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html


[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md


[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

