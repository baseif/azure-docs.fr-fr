---
title: "Qu’est-ce qu’une machine virtuelle Science des données ? | Microsoft Docs"
description: "Découvrez les scénarios et fonctionnalités clés, et familiarisez-vous avec les machines virtuelles Science des données, un environnement et les outils prêts pour l’analyse."
keywords: "outils de science des données, machine virtuelle science des données, outils pour la science des données, science des données linux"
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: d4f91270-dbd2-4290-ab2b-b7bfad0b2703
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/13/2017
ms.author: gokuma
translationtype: Human Translation
ms.sourcegitcommit: e851a3e1b0598345dc8bfdd4341eb1dfb9f6fb5d
ms.openlocfilehash: 77b4f634c2a30d0cbec92d34a7f83866541d7d84
ms.lasthandoff: 04/15/2017


---
# <a name="introduction-to-the-cloud-based-data-science-virtual-machine-for-linux-and-windows"></a>Présentation de la machine virtuelle Science des données basée sur le cloud pour Linux et Windows
La machine virtuelle Science des données (DSVM) est une image de machine virtuelle personnalisée sur le cloud Microsoft Azure spécialement conçue pour la science des données. Elle inclut de nombreux outils de science des données populaires et d’autres outils sont préinstallés et préconfigurés afin d’accélérer la création d’applications intelligentes à des fins d’analyse avancée. Elle est disponible sur Windows Server 2012 et sur Linux. Nous vous proposons l’édition Linux de la DSVM sous Ubuntu 16.04 LTS ou sous des versions Linux basées sur OpenLogic 7.2 CentOS. 

Cette rubrique explique ce que vous pouvez faire avec la machine virtuelle Science des données, présente les principaux scénarios d’utilisation de la machine Virtuelle, détaille les principales fonctionnalités disponibles dans les versions de Windows et Linux, et fournit des instructions sur la façon de commencer à les utiliser.

## <a name="what-can-i-do-with-the-data-science-virtual-machine"></a>Que puis-je faire avec la machine virtuelle Science des données ?
L’objectif de la machine virtuelle Science des données est de fournir aux professionnels des données, tous niveaux de compétence et rôles confondus, un environnement de science des données sans erreur. Cette machine virtuelle vous fait gagner le temps considérable nécessaire qu’il vous faudrait pour déployer vous-même un environnement comparable. Commencez immédiatement votre projet de science des données dans une nouvelle instance de machine virtuelle. 

La machine virtuelle Science des données a été conçue et configurée pour fonctionner dans un grand nombre de scénarios. Vous pouvez faire évoluer ou réduire votre environnement en fonction des besoins de votre projet. Vous pouvez utiliser votre langage par défaut pour programmer les tâches de science des données. Vous pouvez installer d’autres outils et personnaliser le système en fonction de vos besoins exacts.

## <a name="key-scenarios"></a>Principaux scénarios
Cette section propose des scénarios principaux dans lesquels la machine virtuelle Science des données peut être déployée.

### <a name="preconfigured-analytics-desktop-in-the-cloud"></a>Bureau d’analyse préconfiguré dans le cloud
La machine virtuelle Science des données fournit une configuration de base pour les équipes de science des données cherchant à remplacer leurs postes de travail locaux par un bureau de cloud géré. Cette base garantit que tous les scientifiques de données de l’équipe disposent d’une configuration cohérente permettant de vérifier les expériences et encourageant la collaboration. Cela réduit également les coûts en réduisant les efforts pour l’administrateur système et en permettant de réduire le temps nécessaire à l’évaluation, à l’installation et la à gestion des divers packages logiciels nécessaires à l’analyse avancée.  

### <a name="data-science-training-and-education"></a>Formation et éducation de la science des données
Les formateurs d’entreprise et les instructeurs qui enseignent la science des données fournissent généralement une image de machine virtuelle pour s’assurer que leurs étudiants disposent d’une installation cohérente et que les exemples fonctionnent comme prévu. La machine virtuelle Science des données crée un environnement à la demande avec une configuration cohérente qui simplifie le support et les défis liés à la compatibilité. Ceci est d’autant plus bénéfique dans les cas où ces environnements doivent être créés fréquemment, en particulier lors de formations plus courtes.

### <a name="on-demand-elastic-capacity-for-large-scale-projects"></a>Flexibilité à la demande pour les projets d’envergure
Les concours de science des données ou la modélisation et l’exploration de données à grande échelle nécessitent une augmentation de la capacité matérielle, généralement sur une courte durée. La machine virtuelle Science des données permet de répliquer rapidement et à la demande l’environnement de science des données, sur des serveurs montés offrant ainsi des expériences d’exécution des ressources informatique hautes performances.

### <a name="short-term-experimentation-and-evaluation"></a>Expérimentation et évaluation à court terme
La machine virtuelle Science des données peut être utilisée pour évaluer ou découvrir des outils tels que Microsoft R Server, SQL Server, Visual Studio, Jupyter, kits de ressources de formation approfondie / ML et de nouveaux outils populaires dans la communauté, avec un minimum d’efforts d’installation. Étant donné que la machine virtuelle Science des données peut être configurée rapidement, elle peut être appliquée dans d’autres scénarios d’utilisation à court terme comme la réplication d’expériences publiées, l’exécution de démonstrations, le suivi de procédures pas-à-pas dans des sessions en ligne ou des didacticiels de conférence.

### <a name="deep-learning"></a>Apprentissage approfondi
La machine virtuelle de Science des données peut être utilisée pour le modèle d’apprentissage, grâce à des algorithmes d’apprentissage approfondis sur le matériel basé sur les processeurs graphiques (GPU). La DSVM vous aide à utiliser le matériel basé sur GPU sur le cloud uniquement, selon les besoins, lorsque vous devez former des modèles importants ou que vous avez besoin de calculs haut débit qui peuvent profiter de la puissance du processeur graphique.  Sous Windows, nous proposons actuellement un [Kit d’apprentissage approfondi pour machine virtuelle de Science des données](http://aka.ms/dsvm/deeplearning), comme module complémentaire à la DSVM. Ce module complémentaire installe automatiquement les pilotes du processeur graphique (GPU), les infrastructures et la version du processeur graphique des algorithmes d’apprentissage approfondis, tout en créant votre instance de machine virtuelle. Sous Linux, l’apprentissage approfondi sur le processeur graphique n’est activé que sur la [machine virtuelle des sciences de données pour l’édition Linux (Ubuntu)](http://aka.ms/dsvm/ubuntu). Vous pouvez déployer l’édition Ubuntu de la machine virtuelle de science des données sur une machine virtuelle Azure non basée sur le processeur graphique (GPU). Dans ce cas, toutes les infrastructures d’apprentissage approfondi passent en mode UC. L’édition Linux basée sur CentOS de la machine virtuelle de Science des données contient uniquement les versions CPU de certains des outils d’apprentissage approfondi (CNTK, Tensorflow, MXNet), mais les infrastructures et les pilotes du processeur graphique GPU ne sont pas préinstallés. 

## <a name="whats-included-in-the-data-science-vm"></a>Qu’est-ce qui est inclus dans la machine virtuelle Science des données ?
De nombreux outils de science des données et d’apprentissage approfondi populaires sont déjà installés et configurés sur la machine virtuelle Science des données. Elle inclut également des outils simplifiant l’utilisation de différents produits de données et d’analyse Azure. Vous pouvez explorer et créer des modèles prédictifs sur des jeux de données d’envergure à l’aide de Microsoft R Server ou de SQL Server 2016. D’autres outils encore de la communauté open source et de Microsoft sont également inclus, ainsi qu’un exemple de code et des notebooks. Le tableau suivant détaille et compare les principaux composants inclus dans les éditions Windows et Linux de la machine virtuelle Science des données.


| **Outil**                                                           | **Édition Windows** | **Édition Linux** |
| :------------------------------------------------------------------ |:-------------------:|:------------------:|
| [Microsoft R Open](https://mran.microsoft.com/open/) avec packages courants préinstallés   |O                      | O             |
| [Microsoft R Server](https://msdn.microsoft.com/microsoft-r/) Developer Edition inclut : <br />  L’infrastructure R haute performance distribuée et parallèle &nbsp;&nbsp;&nbsp;&nbsp;* [ScaleR](https://msdn.microsoft.com/microsoft-r/scaler-getting-started)<br />  &nbsp;&nbsp;&nbsp;&nbsp;* [MicrosoftML](https://msdn.microsoft.com/microsoft-r/microsoftml-introduction) – nouveaux algorithmes ML de pointe de Microsoft <br />  &nbsp;&nbsp;&nbsp;&nbsp;* [Opérationnalisation de R](https://msdn.microsoft.com/microsoft-r/operationalize/about)                                            |O                      | O <br/> (MicrosoftML n’est pas encore disponible)|
| [Anaconda Python](https://www.continuum.io/) 2.7, 3.5 avec packages populaires préinstallés    |O                      |O              |
| [JuliaPro](https://juliacomputing.com/products/juliapro.html) avec packages populaires préinstallés                         |O                      |O              |
| Bases de données relationnelles                                                            | [SQL Server 2016 SP1](https://www.microsoft.com/sql-server/sql-server-2016) <br/> Developer Edition| [PostgreSQL](https://www.postgresql.org/) |
| Outils de base de données                                                       | * SQL Server Management Studio <br/>* SQL Server Integration Services<br/>* [bcp, sqlcmd](https://docs.microsoft.com/sql/tools/command-prompt-utility-reference-database-engine)<br /> * Pilotes ODBC/JDBC| * [SQuirreL SQL](http://squirrel-sql.sourceforge.net/) (outil de requête), <br /> * bcp, sqlcmd <br /> * Pilotes ODBC/JDBC|
| Analytiques en base de données évolutives avec services SQL Server R | O     |N              |
| **[Serveur Bloc-notes Jupyter](http://jupyter.org/) avec noyaux suivants,**                                  | O     | O |
|     &nbsp;&nbsp;&nbsp;&nbsp;* R | O | O |
|     &nbsp;&nbsp;&nbsp;&nbsp;* Python 2.7 et 3.5 | O | O |
|     &nbsp;&nbsp;&nbsp;&nbsp;* Julia | O | O |
|     &nbsp;&nbsp;&nbsp;&nbsp;* PySpark | N | O |
|     &nbsp;&nbsp;&nbsp;&nbsp;* [Sparkmagic](https://github.com/jupyter-incubator/sparkmagic) | N | O (Ubuntu uniquement) |
|     &nbsp;&nbsp;&nbsp;&nbsp;* SparkR     | N | O |
| JupyterHub (serveur de bloc-notes multi-utilisateur)| N | O |
| **Outils de développement, éditeurs de code et EDI**| | |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Visual Studio 2015 (Community Edition)](https://www.visualstudio.com/community/) > avec plug-in Git, Azure HDInsight (Hadoop), Data Lake, SQL Server Data Tools, [Node.js](https://github.com/Microsoft/nodejstools), [Python](http://aka.ms/ptvs), et [R Tools for Visual Studio (RTVS)](http://microsoft.github.io/RTVS-docs/) | O | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Visual Studio Code](https://code.visualstudio.com/) | O | O |
| &nbsp;&nbsp;&nbsp;&nbsp;* [RStudio Desktop](https://www.rstudio.com/products/rstudio/#Desktop) | O | O |
| &nbsp;&nbsp;&nbsp;&nbsp;* [RStudio Server](https://www.rstudio.com/products/rstudio/#Server) | N | O |
| &nbsp;&nbsp;&nbsp;&nbsp;* [PyCharm](https://www.jetbrains.com/pycharm/) | N | O |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Atom](https://atom.io/) | N | O |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Juno (Julia IDE)](http://junolab.org/)| O | O |
| &nbsp;&nbsp;&nbsp;&nbsp;* Vim et Emacs | O | O |
| &nbsp;&nbsp;&nbsp;&nbsp;* Git et GitBash | O | O |
| &nbsp;&nbsp;&nbsp;&nbsp;* OpenJDK | O | O |
| &nbsp;&nbsp;&nbsp;&nbsp;* .Net Framework | O | N |
| PowerBI Desktop | O | N |
| Kits de développement logiciel (SDK) pour accéder à Azure et à la suite de services Cortana Intelligence | O | O |
| **Outils de gestion et de déplacement de données** | | |
| &nbsp;&nbsp;&nbsp;&nbsp;* Explorateur de stockage Microsoft Azure | O | O |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Interface de ligne de commande Azure](https://docs.microsoft.com/cli/azure/overview) | O | O |
| &nbsp;&nbsp;&nbsp;&nbsp;* Azure Powershell | O | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Azcopy](https://docs.microsoft.com/azure/storage/storage-use-azcopy) | O | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Adlcopy (Azure Data Lake Storage)](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-copy-data-azure-storage-blob) | O | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Outil de migration de données DocDB](https://docs.microsoft.com/azure/documentdb/documentdb-import-data) | O | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Passerelle de gestion de données Microsoft](https://msdn.microsoft.com/library/dn879362.aspx) : déplace les données entre OnPrem et le cloud | O | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* Utilitaires de ligne de commande Unix/Linux | O | O |
| [Apache Drill](http://drill.apache.org) pour l’exploration des données | O | O |
| **Outils Machine Learning** |||
| &nbsp;&nbsp;&nbsp;&nbsp;* Intégration avec [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) (R, Python) | O | O |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Xgboost](https://github.com/dmlc/xgboost) | O | O |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit) | O | O |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Weka](http://www.cs.waikato.ac.nz/ml/weka/) | O | O |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Rattle](http://rattle.togaware.com/) | O | O |
| &nbsp;&nbsp;&nbsp;&nbsp;* [LightGBM](https://github.com/Microsoft/LightGBM) | N | O (Ubuntu uniquement) |
| &nbsp;&nbsp;&nbsp;&nbsp;* [H2O](https://www.h2o.ai/h2o/) | N | O (Ubuntu uniquement) |
| **Outils d’apprentissage approfondi basés sur processeur graphique (GPU)** |Utilisez le [kit d’apprentissage approfondi pour machine virtuelle de science des données](http://aka.ms/dsvm/deeplearning) |Édition Ubuntu uniquement|
| &nbsp;&nbsp;&nbsp;&nbsp;* [Microsoft Cognitive Toolkit (CNTK)](http://cntk.ai) | O | O |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Tensorflow](https://www.tensorflow.org/) | O | O |
| &nbsp;&nbsp;&nbsp;&nbsp;* [MXNet](http://mxnet.io/) | O | O|
| &nbsp;&nbsp;&nbsp;&nbsp;* [Caffe et Caffe2](https://github.com/caffe2/caffe2) | N | O |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Torch](http://torch.ch/) | N | O |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Theano](https://github.com/Theano/Theano) | N | O |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Keras](https://keras.io/)| N | O |
| &nbsp;&nbsp;&nbsp;&nbsp;* [NVidia Digits](https://github.com/NVIDIA/DIGITS) | N | O |
| &nbsp;&nbsp;&nbsp;&nbsp;* [CUDA, CUDNN, Nvidia Driver](https://developer.nvidia.com/cuda-toolkit) | O | O |
| **Plateforme Big Data (Devtest uniquement)**|||
| &nbsp;&nbsp;&nbsp;&nbsp;* [Spark](http://spark.apache.org/) autonome localité | N | O |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Hadoop](http://hadoop.apache.org/) local (HDFS, YARN) | N | O |



## <a name="how-to-get-started-with-the-windows-data-science-vm"></a>Prise en main de la machine virtuelle Science des données Windows
* Créez une instance de la machine virtuelle sur Windows en accédant à [cette page](https://azure.microsoft.com/marketplace/partners/microsoft-ads/standard-data-science-vm/) et en sélectionnant le bouton vert **Créer une machine virtuelle**.
* Connectez-vous à la machine virtuelle à partir de votre Bureau à distance en utilisant les informations d’identification que vous avez spécifiées lorsque vous avez créé la machine virtuelle.
* Pour découvrir et lancer les outils disponibles, cliquez sur le menu **Démarrer**.

## <a name="get-started-with-the-linux-data-science-vm"></a>Prise en main de la machine virtuelle Science des données Linux
* Créer une instance de la machine virtuelle sous Linux
  * Pour l’édition basée sur OpenLogic CentOS, accédez à [cette page](http://aka.ms/dsvm/centos) et cliquez sur **Télécharger maintenant**.
  * Pour l’édition Ubuntu, accédez à [cette page](http://aka.ms/dsvm/ubuntu) et cliquez sur **Télécharger maintenant**.
* Connectez-vous à la machine virtuelle à partir d’un client SSH, comme une commande Putty ou SSH, en utilisant les informations d’identification que vous avez spécifiées lorsque vous avez créé la machine virtuelle.
* Dans l’invite de commandes, entrez dsvm-plus-info.
* Pour un ordinateur de bureau graphique, téléchargez le client X2Go pour votre plateforme cliente [ici](http://wiki.x2go.org/doku.php/doc:installation:x2goclient) et suivez les instructions dans le document de la machine virtuelle Science des données Linux [Approvisionnement d’une machine virtuelle de science des données Linux](machine-learning-data-science-linux-dsvm-intro.md#installing-and-configuring-x2go-client).

## <a name="next-steps"></a>Étapes suivantes
### <a name="for-the-windows-data-science-vm"></a>Pour la machine virtuelle Science des données Windows
* Pour plus d’informations sur l’exécution des outils spécifiques disponibles sur la version Windows, consultez [Approvisionnement d’une machine virtuelle pour la science des données](machine-learning-data-science-provision-vm.md) et
* Pour plus d’informations sur l’exécution de diverses tâches nécessaires à votre projet de science des données sur la machine virtuelle Windows, consultez [Dix choses que vous pouvez effectuer sur la machine virtuelle pour la science des données](machine-learning-data-science-vm-do-ten-things.md).

### <a name="for-the-linux-data-science-vm"></a>Pour la machine virtuelle Science des données Linux
* Pour plus d’informations sur l’exécution des outils spécifiques disponibles sur la version Linux, consultez [Approvisionnement d’une machine virtuelle pour la science des données Linux](machine-learning-data-science-linux-dsvm-intro.md).
* Pour une procédure montrant comment effectuer plusieurs tâches courantes relatives à la science des données avec la machine virtuelle Linux, consultez [Science des données sur la machine virtuelle de science des données Linux](machine-learning-data-science-linux-dsvm-walkthrough.md).


