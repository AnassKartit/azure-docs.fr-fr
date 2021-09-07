---
title: Résoudre les problèmes d’Azure IoT Edge | Microsoft Docs
description: Cet article vous permettra d’acquérir des compétences de diagnostic standard concernant Azure IoT Edge, telles que la récupération des journaux d’activité et de l’état des composants.
author: kgremban
ms.author: kgremban
ms.date: 05/04/2021
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 3bca470114b5fb22409d86c333e92f93155759f6
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2021
ms.locfileid: "122524566"
---
# <a name="troubleshoot-your-iot-edge-device"></a>Résoudre les problèmes de votre appareil IoT Edge

[!INCLUDE [iot-edge-version-201806-or-202011](../../includes/iot-edge-version-201806-or-202011.md)]

Si vous rencontrez des problèmes lors de l’exécution d’Azure IoT Edge dans votre environnement, consultez cet article qui vous guidera pour la résolution des problèmes et les diagnostics.

## <a name="run-the-check-command"></a>Exécuter la commande « check »

La première étape dans la résolution des problèmes relatifs à IoT Edge consiste à utiliser la commande `check` qui exécute une collection de tests de configuration et de connectivité à la recherche de problèmes courants. La commande `check` est disponible dans la [version 1.0.7](https://github.com/Azure/azure-iotedge/releases/tag/1.0.7) et supérieure.

>[!NOTE]
>L’outil de dépannage ne peut pas exécuter de vérifications de connectivité si l’appareil IoT Edge se trouve derrière un serveur proxy.

Vous pouvez exécuter la commande `check` comme suit, ou inclure l’indicateur `--help` pour afficher une liste complète des options :

<!-- 1.1 -->
:::moniker range="iotedge-2018-06"
Sur Linux :

```bash
sudo iotedge check
```

Sur Windows :

```powershell
iotedge check
```

:::moniker-end
<!-- end 1.1 -->

<!-- 1.1 -->
:::moniker range=">=iotedge-2020-11"

```bash
sudo iotedge check
```

:::moniker-end
<!-- end 1.2 -->

L’outil de résolution des problèmes exécute de nombreuses vérifications qui sont triées dans les trois catégories suivantes :

* La *Vérification de configuration* contrôle des détails susceptibles d’empêcher les appareils IoT Edge de se connecter au cloud, dont des problèmes avec le fichier config et le moteur de conteneur.
* La *Vérification de connexion* vérifie que le runtime IoT Edge peut accéder aux ports sur l’appareil hôte et que tous les composants IoT Edge peuvent se connecter à IoT Hub. Cet ensemble de vérifications renvoie des erreurs si l’appareil IoT Edge se trouve derrière un proxy.
* La *Vérification de disponibilité de la production* recherche les meilleures pratiques de production recommandées, comme l’état des certificats d’autorité de l’appareil et la configuration du fichier journal du module.

L’outil de vérification IoT Edge utilise un conteneur pour exécuter ses diagnostics. L’image conteneur, `mcr.microsoft.com/azureiotedge-diagnostics:latest`, est disponible via [Microsoft Container Registry](https://github.com/microsoft/containerregistry). S’il vous faut exécuter une vérification portant sur un appareil sans accès direct à Internet, vos appareils doivent pouvoir accéder à l’image conteneur.

<!-- <1.2> -->
:::moniker range=">=iotedge-2020-11"

Dans un scénario utilisant des appareils IoT Edge imbriqués, vous pouvez accéder à l’image de diagnostic sur les appareils enfants en acheminant le tirage (pull) de l’image par les appareils parents.

```bash
sudo iotedge check --diagnostics-image-name <parent_device_fqdn_or_ip>:<port_for_api_proxy_module>/azureiotedge-diagnostics:1.2
```

<!-- </1.2> -->
:::moniker-end

Pour plus d’informations sur chacune des vérifications de diagnostic exécutées par cet outil, notamment ce qu’il faut faire si vous obtenez une erreur ou un avertissement, consultez [Vérifications de dépannage IoT Edge](https://github.com/Azure/iotedge/blob/master/doc/troubleshoot-checks.md).

## <a name="gather-debug-information-with-support-bundle-command"></a>Collecter les informations de débogage avec la commande « support-bundle »

Pour collecter des journaux à partir d’un appareil IoT Edge, la méthode la plus pratique consiste à utiliser la commande `support-bundle`. Par défaut, cette commande collecte les journaux des modules, du gestionnaire de sécurité IoT Edge et du moteur de conteneur, la sortie JSON de la commande `iotedge check` et d’autres informations de débogage utiles. Elle les compresse dans un fichier unique pour faciliter le partage. La commande `support-bundle` est disponible dans la [version 1.0.9](https://github.com/Azure/azure-iotedge/releases/tag/1.0.9) et supérieure.

Exécutez la commande `support-bundle` avec l’indicateur `--since` pour spécifier la période pour laquelle vous souhaitez récupérer les journaux. Par exemple `6h` obtiendra les journaux des six dernières heures, `6d` des six derniers jours, `6m` des six dernières minutes, et ainsi de suite. Incluez l’indicateur `--help` pour voir la liste complète des options.

<!-- 1.1 -->
:::moniker range="iotedge-2018-06"

Sur Linux :

```bash
sudo iotedge support-bundle --since 6h
```

Sur Windows :

```powershell
iotedge support-bundle --since 6h
```

:::moniker-end
<!-- end 1.1 -->

<!-- 1.2 -->
:::moniker range=">=iotedge-2020-11"

```bash
sudo iotedge support-bundle --since 6h
```

:::moniker-end
<!-- end 1.2 -->

Par défaut, la commande `support-bundle` crée un fichier zip appelé **support_bundle.zip** dans le répertoire où la commande est appelée. Utilisez l’indicateur `--output` pour spécifier un chemin d’accès ou un nom de fichier différent pour la sortie.

Pour plus d’informations sur la commande, consultez ses informations d’aide.

```bash/cmd
iotedge support-bundle --help
```

Vous pouvez également utiliser l’appel de méthode directe intégré [UploadSupportBundle](how-to-retrieve-iot-edge-logs.md#upload-support-bundle-diagnostics) pour charger la sortie de la commande « support-bundle » dans Stockage Blob Azure.

> [!WARNING]
> La sortie de la commande `support-bundle` peut contenir des noms d’hôte, d’appareil et de module, des informations journalisées par vos modules, etc. Soyez conscient de cela si vous partagez la sortie dans un forum public.

## <a name="review-metrics-collected-from-the-runtime"></a>Examiner les métriques collectées à partir du runtime

Les modules de runtime IoT Edge produisent des métriques qui vous permettent de superviser et de comprendre l’intégrité de vos appareils IoT Edge. Ajoutez le module **collecteur de métriques** à vos déploiements pour gérer la collecte de ces métriques et leur envoi au cloud afin de faciliter la supervision.

Pour plus d’informations, consultez [Collecter et transporter les métriques](how-to-collect-and-transport-metrics.md).

## <a name="check-your-iot-edge-version"></a>Vérifier votre version d’IoT Edge

Si vous exécutez une version antérieure d’IoT Edge, une mise à niveau peut résoudre votre problème. L’outil `iotedge check` vérifie que le démon de sécurité IoT Edge est la version la plus récente, mais ne vérifie pas les versions des modules de hub et d’agent IoT Edge. Pour vérifier la version des modules de runtime sur votre appareil, utilisez les commandes `iotedge logs edgeAgent` et `iotedge logs edgeHub`. Le numéro de version est déclaré dans les journaux au démarrage du module.

Pour obtenir des instructions sur la mise à jour de votre appareil, consultez [Mettre à jour le runtime et le démon de sécurité IoT Edge](how-to-update-iot-edge.md).

## <a name="verify-the-installation-of-iot-edge-on-your-devices"></a>Vérifier l’installation d’IoT Edge sur vos appareils

Vous pouvez vérifier l’installation d’IoT Edge sur vos appareils en [surveillant le jumeau de module edgeAgent](./how-to-monitor-module-twins.md).

Pour obtenir le dernier jumeau de module, exécutez la commande suivante depuis [Azure Cloud Shell](https://shell.azure.com/) :

   ```azurecli-interactive
   az iot hub module-twin show --device-id <edge_device_id> --module-id '$edgeAgent' --hub-name <iot_hub_name>
   ```

Cette commande renvoie toutes les [propriétés edgeAgent signalées](./module-edgeagent-edgehub.md). Vous trouverez ci-dessous quelques propriétés utiles pour surveiller l’état de l’appareil :

* état du runtime
* heure de début du runtime
* heure de la dernière sortie du runtime
* nombre de redémarrages du runtime

## <a name="check-the-status-of-the-iot-edge-security-manager-and-its-logs"></a>Vérifier l’état du gestionnaire de sécurité IoT Edge et de ses journaux

Le [gestionnaire de sécurité IoT Edge](iot-edge-security-manager.md) est responsable d’opérations telles que l’initialisation du système IoT Edge au démarrage et l’approvisionnement des appareils. Si IoT Edge ne démarre pas, les journaux du gestionnaire de sécurité peuvent fournir des informations utiles.

<!-- 1.1 -->
:::moniker range="iotedge-2018-06"
Sur Linux :

* Consultez l’état du gestionnaire de sécurité IoT Edge :

   ```bash
   sudo systemctl status iotedge
   ```

* Consultez les journaux du gestionnaire de sécurité IoT Edge :

   ```bash
   sudo journalctl -u iotedge -f
   ```

* Consultez les journaux plus détaillés du gestionnaire de sécurité IoT Edge :

  1. Modifiez les paramètres du démon IoT Edge :

     ```bash
     sudo systemctl edit iotedge.service
     ```

  2. Mettez à jour les lignes suivantes :

     ```bash
     [Service]
     Environment=IOTEDGE_LOG=debug
     ```

  3. Redémarrez le démon de sécurité IoT Edge :

     ```bash
     sudo systemctl cat iotedge.service
     sudo systemctl daemon-reload
     sudo systemctl restart iotedge
     ```

Sur Windows :

* Consultez l’état du gestionnaire de sécurité IoT Edge :

   ```powershell
   Get-Service iotedge
   ```

* Consultez les journaux du gestionnaire de sécurité IoT Edge :

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; Get-IoTEdgeLog
   ```

* Affichez uniquement les 5 dernières minutes des journaux du gestionnaire de sécurité IoT Edge :

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; Get-IoTEdgeLog -StartTime ([datetime]::Now.AddMinutes(-5))
   ```

* Consultez les journaux plus détaillés du gestionnaire de sécurité IoT Edge :

  1. Ajouter une variable d’environnement au niveau du système

     ```powershell
     [Environment]::SetEnvironmentVariable("IOTEDGE_LOG", "debug", [EnvironmentVariableTarget]::Machine)
     ```

  2. Redémarrez le démon de sécurité IoT Edge :

     ```powershell
     Restart-Service iotedge
     ```

:::moniker-end
<!--end 1.1 -->

<!-- 1.2 -->
:::moniker range=">=iotedge-2020-11"

* Consultez l’état des services système IoT Edge :

   ```bash
   sudo iotedge system status
   ```

* Consultez les journaux des services système IoT Edge :

   ```bash
   sudo iotedge system logs -- -f
   ```

* Activez les journaux au niveau débogage pour afficher des journaux plus détaillés des services système IoT Edge :

  1. Activez les journaux au niveau débogage.

     ```bash
     sudo iotedge system set-log-level debug
     sudo iotedge system restart
     ```

  1. Revenez aux journaux de niveau informations par défaut après le débogage.

     ```bash
     sudo iotedge system set-log-level info
     sudo iotedge system restart
     ```

:::moniker-end
<!-- end 1.2 -->

## <a name="check-container-logs-for-issues"></a>Vérifier si les journaux d’activité des conteneurs indiquent des problèmes

Une fois que le démon de sécurité IoT Edge s’exécute, vérifiez si les journaux des conteneurs signalent des problèmes. Commencez par les conteneurs déployés, puis examinez les conteneurs qui composent le runtime IoT Edge : Edge Agent et Edge Hub. En général, les journaux d’activité de l’agent IoT Edge fournissent des informations sur le cycle de vie de chaque conteneur. Les journaux d’activité du hub IoT Edge fournissent des informations sur la messagerie et le routage.

Il est possible de récupérer les journaux de conteneur à plusieurs endroits :

* Sur l’appareil IoT Edge, exécutez la commande suivante pour afficher les journaux :

  ```cmd
  iotedge logs <container name>
  ```

* Sur le Portail Azure, utilisez l’outil de résolution des problèmes intégré. Pour plus d’informations, consultez [Surveillance et résolution des problèmes des appareils IoT Edge sur le Portail Azure](troubleshoot-in-portal.md).

* Utilisez la [méthode directe UploadModuleLogs](how-to-retrieve-iot-edge-logs.md#upload-module-logs) pour charger les journaux d’un module dans le Stockage Blob Azure.

## <a name="clean-up-container-logs"></a>Nettoyer les journaux de conteneur

Par défaut, le moteur de conteneur Moby ne définit pas de limites de taille pour le journal de conteneur. Au fil du temps, cela peut amener l’appareil à se remplir de journaux et à manquer d’espace disque. Si les journaux de conteneurs volumineux nuisent aux performances de votre appareil IoT Edge, utilisez la commande suivante pour forcer la suppression du conteneur et des journaux associés.

Si vous êtes toujours en train de résoudre des problèmes, attendez d’avoir inspecté les journaux du conteneur pour effectuer cette étape.

>[!WARNING]
>Si vous forcez la suppression du conteneur edgeHub alors qu’il contient un backlog de messages non remis et qu’aucun [stockage hôte](how-to-access-host-storage-from-module.md) n’est configuré, les messages non remis seront perdus.

```cmd
docker rm --force <container name>
```

Pour la maintenance continue des journaux et les scénarios de production, [limitez la taille du journal](production-checklist.md#place-limits-on-log-size).

## <a name="view-the-messages-going-through-the-iot-edge-hub"></a>Afficher les messages acheminés via hub IoT Edge

<!--1.1 -->
:::moniker range="iotedge-2018-06"

Vous pouvez afficher les messages acheminés via le hub IoT Edge et recueillir des insights à partir des journaux d’activité détaillés issus des conteneurs du runtime. Pour activer les journaux d’activité détaillés sur ces conteneurs, définissez `RuntimeLogLevel` dans votre fichier de configuration yaml. Pour ouvrir le fichier :

Sur Linux :

   ```bash
   sudo nano /etc/iotedge/config.yaml
   ```

Sur Windows :

   ```cmd
   notepad C:\ProgramData\iotedge\config.yaml
   ```

Par défaut, l’élément `agent` se présente comme dans l’exemple suivant :

   ```yaml
   agent:
     name: edgeAgent
     type: docker
     env: {}
     config:
       image: mcr.microsoft.com/azureiotedge-agent:1.1
       auth: {}
   ```

Remplacez `env: {}` par :

   ```yaml
   env:
     RuntimeLogLevel: debug
   ```

   > [!WARNING]
   > Les fichiers YAML ne peuvent pas contenir de tabulations en guise de mise en retrait. Utilisez 2 espaces à la place. Les éléments de niveau supérieur ne peuvent pas avoir d’espace blanc de début.

Enregistrez le fichier et redémarrez le gestionnaire de sécurité IoT Edge.

<!-- end 1.1 -->
:::moniker-end

<!-- 1.2 -->
:::moniker range=">=iotedge-2020-11"

Vous pouvez afficher les messages transitant via le hub IoT Edge et recueillir des insights à partir des journaux d’activité détaillés issus des conteneurs d’exécution. Pour activer les journaux détaillés sur ces conteneurs, définissez la variable d’environnement `RuntimeLogLevel` dans le manifeste de déploiement.

Pour afficher les messages transitant via le Hub IoT Edge, définissez la variable d’environnement `RuntimeLogLevel` sur `debug` pour le module edgeHub.

Les modules edgeHub et edgeAgent disposent de cette variable d’environnement de journal d’exécution, avec la valeur par défaut définie sur `info`. Cette variable d’environnement peut prendre les valeurs suivantes :

* fatal
* error
* warning
* info
* debug
* verbose

<!-- end 1.2 -->
:::moniker-end

Vous pouvez également vérifier les messages échangés entre IoT Hub et les appareils IoT. Affichez ces messages à l’aide de l’extension [Azure IoT Hub pour Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit). Pour plus d’informations, consultez [Handy tool when you develop with Azure IoT](https://blogs.msdn.microsoft.com/iotdev/2017/09/01/handy-tool-when-you-develop-with-azure-iot/).

## <a name="restart-containers"></a>Redémarrer les conteneurs

Une fois que vous avez examiné les journaux d’activité et les messages pour obtenir des informations, vous pouvez essayer de redémarrer les conteneurs.

Sur l’appareil IoT Edge, utilisez les commandes suivantes pour redémarrer les modules :

```cmd
iotedge restart <container name>
```

Redémarrez les conteneurs du runtime IoT Edge :

```cmd
iotedge restart edgeAgent && iotedge restart edgeHub
```

Vous pouvez également redémarrer les modules à distance sur le Portail Azure. Pour plus d’informations, consultez [Surveillance et résolution des problèmes des appareils IoT Edge sur le Portail Azure](troubleshoot-in-portal.md).

## <a name="check-your-firewall-and-port-configuration-rules"></a>Vérifier vos règles de configuration de pare-feu et de port

Azure IoT Edge permet la communication entre un serveur local et le cloud Azure au moyen des protocoles IoT Hub pris en charge (voir [Choisir un protocole de communication](../iot-hub/iot-hub-devguide-protocols.md)). Pour renforcer la sécurité, les canaux de communication entre Azure IoT Edge et Azure IoT Hub sont toujours configurés pour être sortants. Cette configuration est basée sur le [modèle de communication assistée par des services](/archive/blogs/clemensv/service-assisted-communication-for-connected-devices), ce qui réduit la surface d’attaque qu’une entité malveillante pourrait explorer. Les communications entrantes sont uniquement requises pour des scénarios spécifiques où Azure IoT Hub a besoin d’envoyer (push) des messages à l’appareil Azure IoT Edge. Les messages cloud-à-appareil sont protégés à l’aide de canaux TLS sécurisés, protection qui peut être renforcée à l’aide de certificats X.509 et de modules d’appareil TPM. Le Gestionnaire de sécurité Azure IoT Edge régit la façon dont cette communication peut être établie (voir [Gestionnaire de sécurité IoT Edge](../iot-edge/iot-edge-security-manager.md)).

Bien que IoT Edge assure une configuration améliorée pour la sécurisation du runtime Azure IoT Edge et des modules déployés, il dépend toujours de la configuration du réseau et de l’ordinateur sous-jacents. Il est donc indispensable de vérifier que les règles de pare-feu et de réseau sont bien configurées pour assurer la sécurisation de la communication Edge vers Cloud. Vous pouvez utiliser le tableau suivant pour configurer les règles de pare-feu pour les serveurs sous-jacents hébergeant le runtime Azure IoT Edge :

|Protocol|Port|Entrant|Sortant|Assistance|
|--|--|--|--|--|
|MQTT|8883|BLOQUÉ (par défaut)|BLOQUÉ (par défaut)|<ul> <li>Configurez Sortant sur Ouvert lorsque vous utilisez MQTT comme protocole de communication.<li>1883 pour MQTT n’est pas pris en charge par IoT Edge. <li>Les connexions entrantes doivent être bloquées.</ul>|
|AMQP|5671|BLOQUÉ (par défaut)|OUVERT (par défaut)|<ul> <li>Protocole de communication par défaut pour IoT Edge. <li> Doit être configuré sur Ouvert si Azure IoT Edge n’est pas configuré pour les autres protocoles pris en charge ou si AMQP est le protocole de communication souhaité.<li>5672 pour AMQP n’est pas pris en charge par IoT Edge.<li>Bloquez ce port quand Azure IoT Edge utilise un autre protocole IoT Hub pris en charge.<li>Les connexions entrantes doivent être bloquées.</ul></ul>|
|HTTPS|443|BLOQUÉ (par défaut)|OUVERT (par défaut)|<ul> <li>Configurez la connexion sortante pour être Ouverte sur le port 443 pour le provisionnement d’IoT Edge. Cette configuration est requise en cas d’utilisation de scripts manuels ou du service Azure IoT Device Provisioning. <li>La connexion entrante ne peut être Ouverte que dans certaines situations uniquement : <ul> <li>  Si vous disposez d’une passerelle transparente avec des appareils de nœud terminal pouvant envoyer des requêtes de méthode. Dans ce cas, le port 443 n’a pas besoin d’être ouvert aux réseaux externes pour se connecter à IoTHub ou fournir des services IoTHub via Azure IoT Edge. La règle entrante peut donc être limitée uniquement à l’ouverture du trafic entrant en provenance du réseau interne. <li> Pour les scénarios Client vers Appareil (C2D).</ul><li>80 pour HTTP n’est pas pris en charge par IoT Edge.<li>Si les protocoles non-HTTP (par exemple, AMQP ou MQTT) ne peuvent pas être configurés dans l’entreprise, les messages peuvent être envoyés sur WebSockets. Dans ce cas, le port 443 sera utilisé pour la communication WebSocket.</ul>|

## <a name="next-steps"></a>Étapes suivantes

Vous pensez que vous avez trouvé un bogue dans la plateforme IoT Edge ? [Soumettez un problème](https://github.com/Azure/iotedge/issues) afin que nous puissions poursuivre les améliorations.

Si vous avez d'autres questions, créez une [Support request](https://portal.azure.com/#create/Microsoft.Support) pour obtenir de l'aide.
