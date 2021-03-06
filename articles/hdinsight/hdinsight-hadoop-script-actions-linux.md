---
title: "Développement d’une action de script avec HDInsight basé sur Linux | Microsoft Docs"
description: "Personnalisation de clusters HDInsight basés sur Linux à l’aide d’une action de script. Les actions de script sont un moyen de personnaliser les clusters Azure HDInsight en spécifiant les paramètres de configuration de cluster ou en installant des services, outils ou autres logiciels supplémentaires sur le cluster. "
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: cf4c89cd-f7da-4a10-857f-838004965d3e
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: 9068e0e92e15491d3377a1b8f42071b56373396e
ms.lasthandoff: 04/12/2017


---
# <a name="script-action-development-with-hdinsight"></a>Développement d’actions de script avec HDInsight

Les actions de script sont un moyen de personnaliser les clusters Azure HDInsight en spécifiant les paramètres de configuration de cluster ou en installant des services, outils ou autres logiciels supplémentaires sur le cluster. Vous pouvez utiliser les actions de script lors de la création du cluster ou sur un cluster en cours d'exécution.

> [!IMPORTANT]
> Les étapes décrites dans ce document nécessitent un cluster HDInsight utilisant Linux. Linux est le seul système d’exploitation utilisé sur HDInsight version 3.4 ou supérieure. Pour en savoir plus, consultez le paragraphe [Obsolescence de HDInsight sous Windows](hdinsight-component-versioning.md#hdi-version-33-nearing-deprecation-date).

## <a name="what-are-script-actions"></a>Définition des actions de script

Les actions de script sont des scripts d'interpréteur de commandes qu’Azure exécute sur les nœuds de cluster pour apporter des modifications de configuration ou installer des logiciels. Une action de script est exécutée en tant qu’administrateur et offre des droits d’accès complets aux nœuds du cluster.

Les actions de script peuvent être appliquées selon les méthodes suivantes :

| À utiliser pour appliquer un script... | Pendant la création du cluster... | Sur un cluster en cours d'exécution... |
| --- |:---:|:---:|
| Portail Azure |✓ |✓ |
| Azure PowerShell |✓ |✓ |
| Interface de ligne de commande Azure |&nbsp; |✓ |
| Kit de développement logiciel (SDK) .NET de HDInsight |✓ |✓ |
| Modèle Azure Resource Manager |✓ |&nbsp; |

Pour plus d’informations sur l’utilisation de ces méthodes pour appliquer des actions de script, consultez [Personnalisation de clusters HDInsight à l’aide d’actions de script](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="bestPracticeScripting"></a>Meilleures pratiques relatives au développement de scripts

Quand vous développez un script personnalisé pour un cluster HDInsight, tenez compte des meilleures pratiques suivantes :

* [Rechercher la version Hadoop](#bPS1)
* [Cibler la version du système d’exploitation](#bps10)
* [Fournir des liens stables vers les ressources de script](#bPS2)
* [Utiliser des ressources précompilées](#bPS4)
* [S’assurer que le script de personnalisation du cluster est idempotent](#bPS3)
* [Garantir la haute disponibilité de l’architecture du cluster](#bPS5)
* [Configurer les composants personnalisés pour utiliser le stockage d’objets blob Azure](#bPS6)
* [Écrire des informations sur STDOUT et STDERR](#bPS7)
* [Enregistrer des fichiers au format ASCII avec les fins de ligne LF](#bps8)
* [Utilisation de la logique de nouvelle tentative pour récupérer après une erreur temporaire](#bps9)

> [!IMPORTANT]
> Les actions de script doivent se terminer dans les 60 minutes, faute de quoi elles expirent. Lors de l’approvisionnement du nœud, le script est exécuté en même temps que les autres processus d'installation et de configuration. En raison de cette concurrence pour les ressources, par exemple au niveau du temps processeur ou de la bande passante, l’exécution du script risque de prendre plus de temps que dans votre environnement de développement.

### <a name="bPS1"></a>Rechercher la version Hadoop

Différentes versions de HDInsight sont équipées de différentes versions de services Hadoop et des composants installés. Si votre script attend une version de service ou de composant spécifique, vous devez utiliser uniquement le script avec la version de HDInsight qui inclut les composants requis. Vous trouverez des informations sur les versions de composant incluses dans HDInsight à l’aide du document [Contrôle des versions du composant HDInsight](hdinsight-component-versioning.md) .

### <a name="bps10"></a> Cibler la version du système d’exploitation

HDInsight sous Linux est basé sur la distribution Ubuntu Linux. Différentes versions de HDInsight s’appuient sur des versions différentes d’Ubuntu, ce qui peut avoir une incidence sur le comportement de votre script. Par exemple, HDInsight 3.4 et les versions antérieures sont basées sur des versions Ubuntu qui utilisent Upstart. La version 3.5 est basée sur 16.04 Ubuntu, qui utilise Systemd. Systemd et Upstart reposent sur différentes commandes, donc votre script doit être écrit pour travailler avec les deux.

L’autre différence majeure entre HDInsight 3.4 et 3.5 résident dans le fait que `JAVA_HOME` pointe désormais vers Java 8.

Vous pouvez vérifier la version du système d’exploitation à l’aide de `lsb_release`. Les extraits de code suivants dans le script d’installation Hue montre comment déterminer si le script s’exécute sur Ubuntu 14 ou 16 :

```bash
OS_VERSION=$(lsb_release -sr)
if [[ $OS_VERSION == 14* ]]; then
    echo "OS verion is $OS_VERSION. Using hue-binaries-14-04."
    HUE_TARFILE=hue-binaries-14-04.tgz
elif [[ $OS_VERSION == 16* ]]; then
    echo "OS verion is $OS_VERSION. Using hue-binaries-16-04."
    HUE_TARFILE=hue-binaries-16-04.tgz
fi
...
if [[ $OS_VERSION == 16* ]]; then
    echo "Using systemd configuration"
    systemctl daemon-reload
    systemctl stop webwasb.service    
    systemctl start webwasb.service
else
    echo "Using upstart configuration"
    initctl reload-configuration
    stop webwasb
    start webwasb
fi
...
if [[ $OS_VERSION == 14* ]]; then
    export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64
elif [[ $OS_VERSION == 16* ]]; then
    export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
fi
```

Le script complet contenant ces extraits de code est consultable à l’adresse https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh.

Pour la version d’Ubuntu utilisée par HDInsight, consultez le document [Version du composant HDInsight](hdinsight-component-versioning.md).

Pour comprendre les différences entre Systemd et Upstart, consultez [Systemd pour les utilisateurs Upstart](https://wiki.ubuntu.com/SystemdForUpstartUsers).

### <a name="bPS2"></a>Fournir des liens stables vers les ressources de script

Vous devez vous assurer que les scripts et ressources utilisés par le script sont disponibles tout au long de la durée de vie du cluster et que les versions de ces fichiers ne changent pas sur la durée. Ces ressources sont requises si de nouveaux nœuds sont ajoutés au cluster pendant les opérations de mise à l'échelle.

La meilleure pratique consiste à tout télécharger et à tout archiver dans un compte de stockage Azure de votre abonnement.

> [!IMPORTANT]
> Le compte de stockage utilisé doit être le compte de stockage par défaut du cluster ou un conteneur public en lecture seule d’un autre compte de stockage.

Par exemple, les exemples fournis par Microsoft sont stockés dans le compte de stockage [https://hdiconfigactions.blob.core.windows.net/](https://hdiconfigactions.blob.core.windows.net/) , qui est un conteneur public en lecture seule géré par l’équipe de HDInsight.

### <a name="bPS4"></a>Utiliser des ressources précompilées

Pour réduire le temps nécessaire à l’exécution du script, évitez les opérations qui compilent des ressources depuis le code source. Au lieu de cela, précompilez les ressources et stockez la version binaire dans le stockage d’objets blob Azure afin de pouvoir la télécharger rapidement vers le cluster à partir de votre script.

### <a name="bPS3"></a>S’assurer que le script de personnalisation du cluster est idempotent

Les scripts doivent être conçus de manière idempotente afin que le script, s’il est exécuté plusieurs fois, puisse s’assurer que le cluster est renvoyé dans le même état à chaque fois qu’il est exécuté.

Par exemple, si un script personnalisé a installé une application à l’emplacement /usr/local/bin lors de sa première exécution, alors, à chaque exécution, le script doit vérifier si l’application existe déjà à l’emplacement /usr/local/bin avant de passer aux autres étapes du script.

### <a name="bPS5"></a>Garantir la haute disponibilité de l’architecture du cluster

Les clusters HDInsight basés sur Linux proposent deux fichiers principaux actifs au sein du cluster, et les actions de script sont exécutées depuis les deux nœuds. Si les composants que vous installez n’attendent qu’un seul nœud principal, vous devez créer un script qui installe uniquement le composant sur un des deux nœuds principaux du cluster.

> [!IMPORTANT]
> Les services par défaut installés dans le cadre de HDInsight sont conçus pour basculer d’un nœud principal à l’autre comme nécessaire, cependant, cette fonctionnalité n’est pas étendue aux composants personnalisés installés à l’aide des actions de script. Si les composants installés via une action de script doivent être hautement disponibles, implémentez votre propre mécanisme de basculement qui utilise les deux nœuds principaux disponibles.

### <a name="bPS6"></a>Configurer les composants personnalisés pour utiliser le stockage d’objets blob Azure

Les composants que vous installez sur le cluster peuvent être configurés par défaut pour utiliser le stockage HDFS (Hadoop Distributed File System). HDInsight utilise le stockage d'objets blob Azure (WASB) comme stockage par défaut. Il fournit un système de fichiers compatible HDFS qui rend persistantes les données même en cas de suppression du cluster. Vous devez configurer les composants que vous installez pour pouvoir utiliser WASB au lieu de HDFS.

Par exemple, le texte suivant copie le fichier giraph-examples.jar du système de fichiers local vers WASB :

```bash
hdfs dfs -put /usr/hdp/current/giraph/giraph-examples.jar /example/jars/
```

### <a name="bPS7"></a>Écrire des informations sur STDOUT et STDERR

Les informations écrites dans STDOUT et STDERR pendant l’exécution du script sont consignées et peuvent être affichées à l’aide de l’interface utilisateur web Ambari.

> [!NOTE]
> Ambari n’est disponible que si le cluster a été créé avec succès. Si vous utilisez une action de script lors de la création du cluster et que la création échoue, consultez la section de dépannage de [Personnalisation de clusters HDInsight à l’aide d’une action de script](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting) pour découvrir d’autres façons d’accéder aux informations de journalisation.

La plupart des utilitaires et des packages d’installation ont déjà écrit des informations dans STDOUT et STDERR. Toutefois, vous pouvez ajouter un enregistrement supplémentaire. Pour envoyer du texte à STDOUT, utilisez `echo`. Par exemple :

```bash
echo "Getting ready to install Foo"
```

Par défaut, `echo` envoie la chaîne à STDOUT. Pour la diriger vers STDERR, ajoutez `>&2` avant `echo`. Par exemple :

```bash
>&2 echo "An error occurred installing Foo"
```

Les informations sont redirigées vers STDOUT (1, par défaut et donc, non répertoriées ici) vers STDERR (2). Pour plus d’informations sur la redirection des E/S, consultez [http://www.tldp.org/LDP/abs/html/io-redirection.html](http://www.tldp.org/LDP/abs/html/io-redirection.html).

Pour plus d’informations sur l’affichage des informations consignées par les actions de script, voir [Personnalisation de clusters HDInsight à l’aide d’une action de script](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting)

### <a name="bps8"></a> Enregistrer des fichiers au format ASCII avec les fins de ligne LF

Les scripts d’interpréteur de commandes doivent être stockés au format ASCII, avec des lignes terminées se terminant par LF. Si les fichiers sont stockés au format UTF-8 qui peuvent inclure une marque d’ordre d’octet au début du fichier, ou avec des fins de ligne CRLF, ce qui est courant pour les éditeurs de Windows, le script échoue avec des erreurs ressemblant à ce qui suit :

```
$'\r': command not found
line 1: #!/usr/bin/env: No such file or directory
```

### <a name="bps9"></a> Utilisation de la logique de nouvelle tentative pour récupérer après une erreur temporaire

Lors du téléchargement de fichiers, de l’installation de packages à l’aide d’apt-get ou d’autres actions qui transmettent des données sur Internet, l’action peut échouer en raison d’erreurs réseau temporaires. Par exemple, la ressource distante avec laquelle vous communiquez peut être en train de basculer vers un nœud de secours.

Pour rendre votre script résistant aux erreurs temporaires, vous pouvez implémenter la logique de nouvelle tentative. Voici un exemple de fonction qui exécute toute commande qui lui est transmise et qui, si la commande échoue, réessaye jusqu’à trois fois. Deux secondes s’écoulent entre chaque tentative.

```bash
#retry
MAXATTEMPTS=3

retry() {
    local -r CMD="$@"
    local -i ATTMEPTNUM=1
    local -i RETRYINTERVAL=2

    until $CMD
    do
        if (( ATTMEPTNUM == MAXATTEMPTS ))
        then
                echo "Attempt $ATTMEPTNUM failed. no more attempts left."
                return 1
        else
                echo "Attempt $ATTMEPTNUM failed! Retrying in $RETRYINTERVAL seconds..."
                sleep $(( RETRYINTERVAL ))
                ATTMEPTNUM=$ATTMEPTNUM+1
        fi
    done
}
```

Voici quelques exemples d’utilisation de cette fonction.

```bash
retry ls -ltr foo

retry wget -O ./tmpfile.sh https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh
```

## <a name="helpermethods"></a>Méthodes d'assistance pour les scripts personnalisés

L’action de script fournit des méthodes d’assistance que vous pouvez utiliser lors de l’écriture de scripts personnalisés. Ceux-ci sont définis dans [https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh)et peuvent être inclus dans vos scripts à l’aide des éléments suivants :

```bash
# Import the helper method module.
wget -O /tmp/HDInsightUtilities-v01.sh -q https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh && source /tmp/HDInsightUtilities-v01.sh && rm -f /tmp/HDInsightUtilities-v01.sh
```

Les programmes d’assistance sont ainsi disponibles pour une utilisation dans votre script :

| Utilisation de l’aide | Description |
| --- | --- |
| `download_file SOURCEURL DESTFILEPATH [OVERWRITE]` |Télécharge un fichier de l’URI source vers le chemin d’accès de fichier spécifié. Par défaut, il ne remplacera pas un fichier existant. |
| `untar_file TARFILE DESTDIR` |Extrait un fichier tar (à l’aide de `-xf`,) dans le répertoire de destination. |
| `test_is_headnode` |Lorsqu’il est exécuté sur un nœud principal de cluster, la valeur 1 est renvoyée ; dans le cas contraire, c’est la valeur 0. |
| `test_is_datanode` |Si le nœud actuel est un nœud de données (worker), la valeur 1 est renvoyée ; dans le cas contraire, c’est la valeur 0. |
| `test_is_first_datanode` |Si le nœud actuel est le premier nœud de données (worker) (nommé workernode0,) la valeur 1 est renvoyée ; dans le cas contraire, c’est la valeur 0. |
| `get_headnodes` |Renvoie le nom de domaine complet des nœuds principaux dans le cluster. Les noms sont séparés par des virgules. Une chaîne vide est renvoyée en cas d’erreur. |
| `get_primary_headnode` |Obtient le nom de domaine complet du nœud principal primaire. Une chaîne vide est renvoyée en cas d’erreur. |
| `get_secondary_headnode` |Obtient le nom de domaine complet du nœud principal secondaire. Une chaîne vide est renvoyée en cas d’erreur. |
| `get_primary_headnode_number` |Obtient le suffixe numérique du nœud principal primaire. Une chaîne vide est renvoyée en cas d’erreur. |
| `get_secondary_headnode_number` |Obtient le suffixe numérique du nœud principal secondaire. Une chaîne vide est renvoyée en cas d’erreur. |

## <a name="commonusage"></a>Modes d'utilisation courants

Cette section fournit des conseils sur l'implémentation de certains des modèles d'utilisation courants que vous pouvez rencontrer lors de l'écriture de votre propre script personnalisé.

### <a name="passing-parameters-to-a-script"></a>Transmission de paramètres vers un script

Dans certains cas, votre script peut nécessiter des paramètres. Par exemple, il se peut que vous ayez besoin du mot de passe administrateur afin de récupérer des informations à partir de l’API REST Ambari.

Les paramètres transmis au script sont appelés *paramètres positionnels* et sont affectés à `$1` pour ce qui concerne le premier paramètre, `$2` pour le deuxième et ainsi de suite. `$0` contient le nom du script lui-même.

Les valeurs transmises au script en tant que paramètres doivent être mis entre guillemets simples (’) afin que la valeur transmise soit traitée comme un littéral, et aucun traitement spécial n’est transmis à des caractères inclus tels que « ! ».

### <a name="setting-environment-variables"></a>Définition des variables d'environnement

La définition d’une variable d’environnement est effectuée de la façon suivante :

    VARIABLENAME=value

Où VARIABLENAME est le nom de la variable. Pour accéder à la variable par la suite, utilisez `$VARIABLENAME`. Par exemple, pour affecter une valeur fournie par un paramètre de positionnement tel qu’une variable d’environnement nommée PASSWORD, utilisez ce qui suit :

    PASSWORD=$1

Lors des accès ultérieurs aux informations, il est possible d’utiliser `$PASSWORD`.

Les variables d’environnement définies dans le script existent uniquement dans le cadre du script. Dans certains cas, vous devrez peut-être ajouter des variables d’environnement de niveau système qui persisteront une fois le script terminé. En général, c’est ainsi pour que les utilisateurs qui se connectent au cluster via SSH utilisent les composants installés par votre script. Vous pouvez accomplir cette action en ajoutant la variable d’environnement à `/etc/environment`. Par exemple, ce qui suit ajoute **HADOOP\_CONF\_DIR** :

```bash
echo "HADOOP_CONF_DIR=/etc/hadoop/conf" | sudo tee -a /etc/environment
```

### <a name="access-to-locations-where-the-custom-scripts-are-stored"></a>Accès aux emplacements où sont stockés les scripts personnalisés

Les scripts utilisés pour personnaliser un cluster doivent être stockés dans un des emplacements suivants :

* Le __compte de stockage par défaut__ du cluster.

* Un __compte de stockage supplémentaire__ associé au cluster.

* Une __URI lisible publiquement__, comme OneDrive, Dropbox, etc.

* Un compte __Azure Data Lake Store__ est associé à un groupe de ressources Azure. Pour plus d’informations sur l’utilisation d'Azure Data Lake Store avec HDInsight, voir [Créer un cluster HDInsight avec Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).

    > [!NOTE]
    > Le principal de service que HDInsight utilise pour accéder au Data Lake Store doit avoir accès en lecture au script.

Si votre script accède à des ressources situées ailleurs, celles-ci doivent également se trouver dans un conteneur accessible publiquement (au moins public en lecture seule). Vous pouvez par exemple télécharger un fichier sur le cluster à l’aide de `download_file`.

Stockez le fichier dans un compte de stockage Azure accessible au cluster (par exemple le compte de stockage par défaut), fournissent un accès rapide, ce stockage étant au sein du réseau Azure.

> [!NOTE]
> Le format d’URI utilisé pour référencer le script diffère selon le service utilisé. Pour les comptes de stockage associés au cluster HDInsight, utilisez `wasb://` ou `wasbs://`. Pour des URI lisibles publiquement, utilisez `http://` ou `https://`. Pour Data Lake Store, utilisez `adl://`.

### <a name="checking-the-operating-system-version"></a>Vérification de la version du système d’exploitation

Différentes versions de HDInsight s’appuient sur des versions spécifiques d’Ubuntu. Il peut exister des différences entre les versions de système d’exploitation que vous devez vérifier dans votre script. Par exemple, vous devrez peut-être installer un fichier binaire qui est lié à la version d’Ubuntu.

Pour vérifier la version du système d’exploitation, utilisez `lsb_release`. L’exemple suivant montre comment référencer un fichier tar différent selon la version du système d’exploitation :

```bash
OS_VERSION=$(lsb_release -sr)
if [[ $OS_VERSION == 14* ]]; then
    echo "OS verion is $OS_VERSION. Using hue-binaries-14-04."
    HUE_TARFILE=hue-binaries-14-04.tgz
elif [[ $OS_VERSION == 16* ]]; then
    echo "OS verion is $OS_VERSION. Using hue-binaries-16-04."
    HUE_TARFILE=hue-binaries-16-04.tgz
fi
```

## <a name="deployScript"></a>Liste de vérification pour le déploiement d'une action de script

Voici les étapes à suivre avant de déployer des scripts :

* Placez les fichiers qui contiennent les scripts personnalisés dans un emplacement accessible aux nœuds du cluster lors du déploiement. Il peut s'agir de tout compte de stockage par défaut ou supplémentaire spécifié lors du déploiement du cluster, ou de tout conteneur de stockage accessible au public.
* Ajoutez des contrôles des scripts pour vous assurer qu’ils s’exécutent, ce qui permet d’exécuter le script plusieurs fois sur le même nœud.
* Utilisez un dossier de fichiers temporaires /tmp pour conserver le fichier téléchargé utilisé par les scripts, puis nettoyez-les une fois que les scripts ont été exécutés.
* En cas de modification des paramètres au niveau du système d'exploitation ou des fichiers de configuration du service Hadoop, vous pouvez redémarrer les services HDInsight pour qu'ils récupèrent des paramètres au niveau du système d'exploitation, tels que les variables d'environnement définies dans les scripts.

## <a name="runScriptAction"></a>Comment exécuter une action de script

Vous pouvez utiliser des actions de script pour personnaliser les clusters HDInsight avec les modèles du portail Azure, d’Azure PowerShell ou d’Azure Resource Manager ou le Kit de développement logiciel (SDK) .NET HDInsight. Pour obtenir des instructions, voir [Comment utiliser l’action de script](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="sampleScripts"></a>Exemples de scripts personnalisés

Microsoft fournit des exemples de scripts pour installer des composants sur un cluster HDInsight. Vous trouverez des exemples de scripts et des instructions sur leur utilisation en cliquant sur les liens ci-dessous :

* [Installer et utiliser Hue sur les clusters HDInsight](hdinsight-hadoop-hue-linux.md)
* [Installation et utilisation de R sur des clusters HDInsight Hadoop](hdinsight-hadoop-r-scripts-linux.md)
* [Installer et utiliser Solr sur les clusters HDInsight](hdinsight-hadoop-solr-install-linux.md)
* [Installer et utiliser Giraph sur les clusters HDInsight](hdinsight-hadoop-giraph-install-linux.md)  

> [!NOTE]
> Les documents sont spécifiques aux clusters HDInsight sous Linux. Pour les scripts qui fonctionnent avec HDInsight sous Windows, consultez le [développement d’action de script HDInsight (Windows)](hdinsight-hadoop-script-actions.md) ou utilisez les liens disponibles en haut de chaque article.

## <a name="troubleshooting"></a>Résolution des problèmes

Voici les erreurs que vous pouvez rencontrer lorsque vous utilisez les scripts que vous avez développé :

**Erreur** : `$'\r': command not found`. Parfois suivi par `syntax error: unexpected end of file`.

*Cause*: cette erreur se produit lorsque les lignes d’un script se terminent par CRLF. Les systèmes UNIX attendent seulement LF comme fin de ligne.

Ce problème se produit souvent lorsque le script est créé dans un environnement Windows, car CRLF est une fin de ligne commune à de nombreux éditeurs de texte sous Windows.

*Résolution*: si c’est une option dans votre éditeur de texte, sélectionnez le format Unix ou LF comme fin de ligne. Vous pouvez également utiliser les commandes suivantes sur un système Unix pour changer la séquence CRLF en LF :

> [!NOTE]
> Les commandes suivantes sont à peu près équivalentes dans la mesure où elles doivent changer les fins de ligne CRLF en LF. Sélectionnez-en une basée sur les utilitaires disponibles sur votre système.

| Commande | Remarques |
| --- | --- |
| `unix2dos -b INFILE` |Le fichier d’origine est sauvegardé avec une extension .BAK |
| `tr -d '\r' < INFILE > OUTFILE` |OUTFILE contiendra une version avec des terminaisons LF uniquement |
| `perl -pi -e 's/\r\n/\n/g' INFILE` |Cela modifiera directement le fichier directement sans créer un nouveau fichier |
| ```sed 's/$'"/`echo \\\r`/" INFILE > OUTFILE``` |OUTFILE contiendra une version avec des terminaisons LF uniquement. |

**Erreur** : `line 1: #!/usr/bin/env: No such file or directory`.

*Cause*: cette erreur se produit lorsque le script a été enregistré en tant qu’UTF-8 avec une marque d’ordre d’octet (BOM).

*Résolution*: enregistrer le fichier au format ASCII ou UTF-8 sans marque d’ordre d’octet. Vous pouvez également utiliser la commande suivante sur un système Linux ou Unix pour créer un nouveau fichier sans marque d’ordre d’octet :

    awk 'NR==1{sub(/^\xef\xbb\xbf/,"")}{print}' INFILE > OUTFILE

Pour la commande ci-dessus, remplacez **INFILE** par le fichier contenant la marque d’ordre d’octet. **OUTFILE** doit être un nouveau nom de fichier contenant le script sans marque d’ordre d’octet.

## <a name="seeAlso"></a>Étapes suivantes

* Découvrez comment [Personnaliser des clusters HDInsight à l’aide d’une action de script](hdinsight-hadoop-customize-cluster-linux.md)
* Utilisez la [Référence du Kit de développement logiciel (SDK) .NET HDInsight](https://msdn.microsoft.com/library/mt271028.aspx) pour en savoir plus sur la création d’applications .NET qui gèrent HDInsight
* Utilisez l’ [API REST HDInsight](https://msdn.microsoft.com/library/azure/mt622197.aspx) pour savoir comment utiliser REST pour effectuer des actions de gestion sur des clusters HDInsight.

