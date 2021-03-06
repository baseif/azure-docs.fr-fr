---
title: "Personnalisation des solutions préconfigurées | Microsoft Docs"
description: "Fournit des conseils sur la personnalisation des solutions préconfigurées Azure IoT Suite."
services: 
suite: iot-suite
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 4653ae53-4110-4a10-bd6c-7dc034c293a8
ms.service: iot-suite
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/15/2017
ms.author: corywink
translationtype: Human Translation
ms.sourcegitcommit: 4c2de5227388a1f23af84048a83564816ae329bd
ms.openlocfilehash: 6f7e787f18a9ffa77430c86931196c638f000cc8
ms.lasthandoff: 02/27/2017


---
# <a name="customize-a-preconfigured-solution"></a>Personnaliser une solution préconfigurée
Les solutions préconfigurées fournies avec Azure IoT Suite présentent les services de la suite qui collaborent pour fournir une solution de bout en bout. À partir de ce point de départ, il existe différents endroits où vous pouvez étendre et personnaliser la solution pour des scénarios spécifiques. Les sections suivantes décrivent ces points de personnalisation courants.

## <a name="find-the-source-code"></a>Recherche du code source
Le code source pour les solutions préconfigurées est disponible sur GitHub dans les dépôts suivants :

* Surveillance à distance : [https://www.github.com/Azure/azure-iot-remote-monitoring](https://github.com/Azure/azure-iot-remote-monitoring)
* Maintenance prédictive : [https://github.com/Azure/azure-iot-predictive-maintenance](https://github.com/Azure/azure-iot-predictive-maintenance)

Le code source pour les solutions préconfigurées est fourni dans le but d’illustrer les modèles et pratiques utilisés pour implémenter la fonctionnalité de bout en bout d’une solution IoT utilisant Azure IoT Suite. Pour plus d’informations sur la création et le déploiement des solutions, consultez les dépôts GitHub.

## <a name="change-the-preconfigured-rules"></a>Modification des règles préconfigurées
La solution de surveillance à distance inclut trois travaux [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) destinés à gérer la logique d’informations, de règles et de télémétrie dans la solution.

Les trois travaux Stream Analytics et leur syntaxe sont décrits en détail dans [Présentation de la solution préconfigurée de surveillance à distance](iot-suite-remote-monitoring-sample-walkthrough.md). 

Vous pouvez modifier ces tâches directement pour en modifier la logique, ou ajouter une logique spécifique à votre scénario. Pour rechercher les tâches Stream Analytics, procédez comme suit :

1. Accédez au [portail Azure](https://portal.azure.com).
2. Accédez au groupe de ressources portant le même nom que votre solution IoT. 
3. Sélectionnez la tâche Azure Stream Analytics à modifier. 
4. Arrêtez le travail en sélectionnant **Arrêter**dans l’ensemble de commandes. 
5. Modifiez les entrées, la requête et les sorties.
   
    Une simple modification consiste à changer la requête pour la tâche **Règles** afin d’utiliser le signe **« < »** au lieu du signe **« > »**. Le portail de solution affiche toujours **« > »** quand vous modifiez une règle, mais le comportement est inversé en raison de la modification du travail sous-jacent.
6. Démarrage du travail

> [!NOTE]
> Le tableau de bord de surveillance à distance dépend de données spécifiques. Ainsi, modifier les tâches peut provoquer l’échec du tableau de bord.
> 
> 

## <a name="add-your-own-rules"></a>Ajout de vos propres règles
En plus de modifier les tâches Azure Stream Analytics préconfigurées, vous pouvez utiliser le portail Azure pour ajouter de nouvelles tâches ou ajouter de nouvelles requêtes aux tâches existantes.

## <a name="customize-devices"></a>Personnalisation des appareils
L’une des activités d’extension les plus courantes consiste à utiliser des appareils spécifiques à votre scénario. Il existe plusieurs méthodes pour travailler avec des appareils, Notamment, la modification d’un appareil simulé pour qu’il corresponde à votre scénario ou l’utilisation du [Kit SDK d’appareils IoT][IoT Device SDK] pour connecter votre appareil physique à la solution.

Pour obtenir un guide étape par étape sur l’ajout d’appareils, consultez les articles sur la [connexion des appareils dans IoT Suite](iot-suite-connecting-devices.md) et [l’exemple de Kit de développement logiciel (SDK) C de surveillance à distance](https://github.com/Azure/azure-iot-sdk-c/tree/master/serializer/samples/remote_monitoring). Ces appareils sont conçus pour fonctionner avec la solution préconfigurée de surveillance à distance.

### <a name="create-your-own-simulated-device"></a>Création de votre propre appareil simulé
Le [code source de la solution de surveillance à distance](https://github.com/Azure/azure-iot-remote-monitoring) contient un simulateur .NET. C’est lui qui est configuré dans le cadre de la solution et qui peut être modifié pour envoyer des données de télémétrie ou des métadonnées différentes, ou pour répondre à des commandes et méthodes différentes.

Le simulateur préconfiguré dans la solution préconfigurée de surveillance à distance simule un appareil de refroidissement qui émet des données de télémétrie de température et d’humidité. Vous pouvez modifier le simulateur dans le projet [Simulator.WebJob](https://github.com/Azure/azure-iot-remote-monitoring/tree/master/Simulator/Simulator.WebJob) une fois le référentiel GitHub dupliqué.

### <a name="available-locations-for-simulated-devices"></a>Emplacements disponibles pour les appareils simulés
L’ensemble des emplacements par défaut se trouve à Seattle/Redmond, Washington, États-Unis. Vous pouvez modifier ces emplacements dans [SampleDeviceFactory.cs][lnk-sample-device-factory].

### <a name="add-a-desired-property-update-handler-to-the-simulator"></a>Ajout du gestionnaire de mise à jour des propriétés souhaitées au simulateur
Vous pouvez définir la valeur d’une propriété souhaitée pour un appareil dans le portail de solution. C’est l’appareil qui gère la demande de modification de propriété lorsqu’il récupère la valeur de propriété souhaitée. Pour ajouter la prise en charge d’une modification de valeur de propriété par une propriété souhaitée, vous devez ajouter un gestionnaire au simulateur.

Le simulateur contient des gestionnaires pour les propriétés **SetPointTemp** et **TelemetryInterval** que vous pouvez mettre à jour en définissant les valeurs souhaitées dans le portail de solution.

L’exemple suivant montre le gestionnaire pour la propriété souhaitée **SetPointTemp** dans la classe **CoolerDevice** :

```csharp
protected async Task OnSetPointTempUpdate(object value)
{
    var telemetry = _telemetryController as ITelemetryWithSetPointTemperature;
    telemetry.SetPointTemperature = Convert.ToDouble(value);

    await SetReportedPropertyAsync(SetPointTempPropertyName, telemetry.SetPointTemperature);
}
```

Cette méthode met à jour la température du point de télémétrie, puis signale la modification à IoT Hub en définissant une propriété signalée.

Vous pouvez ajouter vos propres gestionnaires pour vos propres propriétés en suivant le modèle de l’exemple précédent.

Vous devez également lier la propriété souhaitée au gestionnaire comme indiqué dans l’exemple suivant issu du constructeur **CoolerDevice** :

```csharp
_desiredPropertyUpdateHandlers.Add(SetPointTempPropertyName, OnSetPointTempUpdate);
```

Notez que **SetPointTempPropertyName** est une constante définie en tant que « Config.SetPointTemp ».

### <a name="add-support-for-a-new-method-to-the-simulator"></a>Ajout de la prise en charge d’une nouvelle méthode au simulateur
Vous pouvez personnaliser le simulateur pour ajouter la prise en charge d’une nouvelle [méthode (méthode directe)][lnk-direct-methods]. Les deux principales étapes requises sont les suivantes :

- Le simulateur doit fournir à IoT hub des informations sur la méthode dans la solution préconfigurée.
- Le simulateur doit inclure le code pour gérer l’appel de méthode lorsque vous l’appelez à partir du volet **Informations sur l’appareil** dans l’Explorateur de solutions ou via un travail.

La solution préconfigurée de surveillance à distance utilise les *propriétés signalées* pour envoyer des informations sur les méthodes prises en charge à IoT hub. Le serveur principal de solution gère une liste de toutes les méthodes prises en charge par chaque appareil, ainsi que l’historique des appels de méthode. Vous pouvez afficher ces informations sur les appareils et appeler des méthodes dans le portail de solution.

Pour informer IoT Hub qu’un appareil prend en charge une méthode, l’appareil doit ajouter des informations sur la méthode au nœud **SupportedMethods** dans les propriétés signalées :

```json
"SupportedMethods": {
  "<method signature>": "<method description>",
  "<method signature>": "<method description>"
}
```

La signature de méthode a le format suivant : `<method name>--<parameter #0 name>-<parameter #1 type>-...-<parameter #n name>-<parameter #n type>`. Par exemple, pour spécifier que la méthode **InitiateFirmwareUpdate** attend un paramètre de chaîne nommé **FwPackageURI**, utilisez la signature de méthode suivante :

```
InitiateFirmwareUpate--FwPackageURI-string: "description of method"
```

Pour obtenir la liste des types de paramètres pris en charge, consultez la classe **CommandTypes** dans le projet Infrastructure.

Pour supprimer une méthode, définissez la signature de méthode `null` dans les propriétés signalées.

> [!NOTE]
> Le serveur principal de solution met uniquement à jour les informations sur les méthodes prises en charge lorsqu’il reçoit un message *d’informations sur l’appareil* de la part de l’appareil.
> 
> 

L’exemple de code suivant issu de la classe **SampleDeviceFactory** du projet Common montre comment ajouter une méthode à la liste des **SupportedMethods** dans les propriétés signalées envoyées par l’appareil :

```csharp
device.Commands.Add(new Command(
    "InitiateFirmwareUpdate",
    DeliveryType.Method,
    "Updates device Firmware. Use parameter 'FwPackageUri' to specifiy the URI of the firmware file, e.g. https://iotrmassets.blob.core.windows.net/firmwares/FW20.bin",
    new[] { new Parameter("FwPackageUri", "string") }
));
```

Cet extrait de code ajoute les informations sur la méthode **InitiateFirmwareUpdate**, y compris le texte à afficher dans le portail de solution et les informations sur les paramètres de méthode requis.

Le simulateur envoie les propriétés signalées, y compris la liste des méthodes prises en charge, à IoT Hub au démarrage du simulateur.

Ajoutez un gestionnaire au code du simulateur pour chaque méthode qu'il prend en charge. Vous pouvez voir les gestionnaires existants dans la classe **CoolerDevice** du projet Simulator.WebJob. L’exemple suivant montre le gestionnaire de la méthode **InitiateFirmwareUpdate** :

```csharp
public async Task<MethodResponse> OnInitiateFirmwareUpdate(MethodRequest methodRequest, object userContext)
{
    if (_deviceManagementTask != null && !_deviceManagementTask.IsCompleted)
    {
        return await Task.FromResult(BuildMethodRespose(new
        {
            Message = "Device is busy"
        }, 409));
    }

    try
    {
        var operation = new FirmwareUpdate(methodRequest);
        _deviceManagementTask = operation.Run(Transport).ContinueWith(async task =>
        {
            // after firmware completed, we reset telemetry
            var telemetry = _telemetryController as ITelemetryWithTemperatureMeanValue;
            if (telemetry != null)
            {
                telemetry.TemperatureMeanValue = 34.5;
            }

            await UpdateReportedTemperatureMeanValue();
        });

        return await Task.FromResult(BuildMethodRespose(new
        {
            Message = "FirmwareUpdate accepted",
            Uri = operation.Uri
        }));
    }
    catch (Exception ex)
    {
        return await Task.FromResult(BuildMethodRespose(new
        {
            Message = ex.Message
        }, 400));
    }
}
```

Les noms de gestionnaires de méthode doivent commencer par `On` suivi du nom de la méthode. Le paramètre **methodRequest** contient tous les paramètres transmis avec l’appel de méthode à partir du serveur principal de solution. La valeur de retour doit être de type **Task&lt;MethodResponse&gt;**. La méthode d’utilitaire **BuildMethodResponse** vous permet de créer la valeur de retour.

Dans le gestionnaire de méthode, vous pouvez :

- Démarrer une tâche asynchrone.
- Récupérer les propriétés souhaitées depuis le *jumeau d’appareil* dans IoT Hub.
- Mettre à jour une seule propriété signalée à l’aide de la méthode **SetReportedPropertyAsync** dans la classe **CoolerDevice**.
- Mettre à jour plusieurs propriétés signalées en créant une instance **TwinCollection** et en appelant la méthode **Transport.UpdateReportedPropertiesAsync**.

L’exemple précédent de mise à jour du microprogramme effectue les étapes suivantes :

- Vérifie que l’appareil est en mesure d’accepter la demande de mise à jour du microprogramme.
- Lance l’opération de mise à jour du microprogramme de façon asynchrone et réinitialise les données de télémétrie lorsque l’opération est terminée.
- Retourne immédiatement le message « FirmwareUpdate accepted » (Mise à jour du microprogramme acceptée) pour indiquer que la demande a été acceptée par l’appareil.

### <a name="build-and-use-your-own-physical-device"></a>Création et utilisation de votre propre appareil (physique)
Les [Kits de développement logiciel (SDK) Azure IoT](https://github.com/Azure/azure-iot-sdks) fournissent des bibliothèques pour la connexion de nombreux types d’appareils (langages et systèmes d’exploitation) à des solutions IoT.

## <a name="modify-dashboard-limits"></a>Modification des limites de tableau de bord
### <a name="number-of-devices-displayed-in-dashboard-dropdown"></a>Nombre d’appareils affichés dans la liste déroulante du tableau de bord
Par défaut, la valeur est 200. Vous pouvez modifier cette valeur dans [DashboardController.cs][lnk-dashboard-controller].

### <a name="number-of-pins-to-display-in-bing-map-control"></a>Nombre d’épingles à afficher dans le contrôle de carte Bing
Par défaut, la valeur est 200. Vous pouvez modifier cette valeur dans [TelemetryApiController.cs][lnk-telemetry-api-controller-01].

### <a name="time-period-of-telemetry-graph"></a>Période de temps du graphique de télémétrie
La valeur par défaut est 10 minutes. Vous pouvez modifier cette valeur dans [TelemetryApiController.cs][lnk-telemetry-api-controller-02].

## <a name="manually-set-up-application-roles"></a>Configuration manuelle des rôles d’application
La procédure suivante décrit comment ajouter les rôles d’application **Admin** et **ReadOnly** à une solution préconfigurée. Les solutions préconfigurées provisionnées à partir du site azureiotsuite.com incluent les rôles **Admin** et **ReadOnly**.

Les membres du rôle **ReadOnly** peuvent afficher le tableau de bord et la liste des appareils, mais ils ne sont pas autorisés à ajouter des appareils, modifier les attributs de l’appareil ou envoyer des commandes.  Les membres du rôle **Admin** ont un accès complet à toutes les fonctionnalités de la solution.

1. Connectez-vous au [Portail Azure Classic][lnk-classic-portal].
2. Sélectionnez **Active Directory**.
3. Cliquez sur le nom du client AAD que vous avez utilisé lorsque vous avez provisionné votre solution.
4. Cliquez sur **Applications**.
5. Cliquez sur le nom de l’application qui correspond au nom de votre solution préconfigurée. Si vous ne voyez pas votre application dans la liste, sélectionnez **Applications que ma société possède** dans le menu déroulant **Afficher** et cliquez sur la coche.
6. En bas de la page, cliquez sur **Gérer le manifeste** et **Télécharger le manifeste**.
7. Cette procédure télécharge un fichier .json sur votre ordinateur local. Ouvrez ce fichier pour le modifier dans un éditeur de texte de votre choix.
8. Sur la troisième ligne du fichier .json, vous trouverez :
   
   ```
   "appRoles" : [],
   ```
   Remplacez cette ligne par le code suivant :
   
   ```
   "appRoles": [
   {
   "allowedMemberTypes": [
   "User"
   ],
   "description": "Administrator access to the application",
   "displayName": "Admin",
   "id": "a400a00b-f67c-42b7-ba9a-f73d8c67e433",
   "isEnabled": true,
   "value": "Admin"
   },
   {
   "allowedMemberTypes": [
   "User"
   ],
   "description": "Read only access to device information",
   "displayName": "Read Only",
   "id": "e5bbd0f5-128e-4362-9dd1-8f253c6082d7",
   "isEnabled": true,
   "value": "ReadOnly"
   } ],
   ```
9. Enregistrez le fichier .json mis à jour (vous pouvez remplacer le fichier existant).
10. Au bas de la page du portail Azure Classic, sélectionnez **Gérer le manifeste**, puis **Télécharger le manifeste** pour télécharger le fichier .json que vous avez enregistré à l’étape précédente.
11. Vous avez maintenant ajouté les rôles **Admin** et **ReadOnly** à votre application.
12. Pour affecter un de ces rôles à un utilisateur de votre répertoire, consultez [Autorisations sur le site azureiotsuite.com][lnk-permissions].

## <a name="feedback"></a>Commentaires
Vous avez une personnalisation que vous aimeriez voir couverte dans ce document ? Ajoutez des suggestions de fonctionnalités à [User Voice](https://feedback.azure.com/forums/321918-azure-iot) ou commentez cet article. 

## <a name="next-steps"></a>Étapes suivantes
Pour en savoir plus sur les options permettant de personnaliser les solutions préconfigurées, consultez :

* [Connecter Logic App à la solution préconfigurée de surveillance à distance Azure IoT Suite][lnk-logicapp]
* [Utilisation de la télémétrie dynamique avec la solution préconfigurée de surveillance à distance][lnk-dynamic]
* [Métadonnées relatives aux informations d’appareil dans la solution préconfigurée de surveillance à distance][lnk-devinfo]

[lnk-logicapp]: iot-suite-logic-apps-tutorial.md
[lnk-dynamic]: iot-suite-dynamic-telemetry.md
[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[IoT Device SDK]: https://azure.microsoft.com/documentation/articles/iot-hub-sdks-summary/
[lnk-permissions]: iot-suite-permissions.md
[lnk-dashboard-controller]: https://github.com/Azure/azure-iot-remote-monitoring/blob/3fd43b8a9f7e0f2774d73f3569439063705cebe4/DeviceAdministration/Web/Controllers/DashboardController.cs#L27
[lnk-telemetry-api-controller-01]: https://github.com/Azure/azure-iot-remote-monitoring/blob/3fd43b8a9f7e0f2774d73f3569439063705cebe4/DeviceAdministration/Web/WebApiControllers/TelemetryApiController.cs#L27
[lnk-telemetry-api-controller-02]: https://github.com/Azure/azure-iot-remote-monitoring/blob/e7003339f73e21d3930f71ceba1e74fb5c0d9ea0/DeviceAdministration/Web/WebApiControllers/TelemetryApiController.cs#L25 
[lnk-sample-device-factory]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Common/Factory/SampleDeviceFactory.cs#L40
[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-direct-methods]: ../iot-hub/iot-hub-devguide-direct-methods.md
