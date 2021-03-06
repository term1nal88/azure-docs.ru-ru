---
title: Обновление кластера службы Azure Kubernetes (AKS)
description: Обновление кластера службы Azure Kubernetes (AKS)
services: container-service
author: gabrtv
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 07/18/2018
ms.author: gamonroy
ms.custom: mvc
ms.openlocfilehash: 4ff2b56afc4496b6344735b4e3c813b06cee17e3
ms.sourcegitcommit: f057c10ae4f26a768e97f2cb3f3faca9ed23ff1b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/17/2018
ms.locfileid: "42144566"
---
# <a name="upgrade-an-azure-kubernetes-service-aks-cluster"></a>Обновление кластера службы Azure Kubernetes (AKS)

Служба Azure Kubernetes (AKS) упрощает выполнение общих задач управления, включая обновление кластеров Kubernetes.

## <a name="upgrade-an-aks-cluster"></a>Обновление кластера AKS

Перед обновлением кластера выполните команду `az aks get-upgrades`, чтобы проверить выпуски Kubernetes, доступные для обновления.

```azurecli-interactive
az aks get-upgrades --name myAKSCluster --resource-group myResourceGroup --output table
```

Выходные данные:

```console
Name     ResourceGroup    MasterVersion    NodePoolVersion    Upgrades
-------  ---------------  ---------------  -----------------  -------------------
default  mytestaks007     1.8.10           1.8.10             1.9.1, 1.9.2, 1.9.6
```

У нас есть три версии для обновления: 1.9.1, 1.9.2 и 1.9.6. Мы можем выполнить команду `az aks upgrade` для обновления до последней доступной версии.  Чтобы сократить время простоя работающих приложений, в ходе обновления AKS добавит новый узел в кластер, а затем узлы будут по очереди [блокироваться и останавливаться][kubernetes-drain].

> [!NOTE]
> При обновлении кластера AKS нельзя пропускать дополнительные номера версий Kubernetes. Например, обновлять систему с 1.8.x до 1.9.x или с 1.9.x до 1.10.x можно, а с 1.8 до 1.10 — нельзя. Для обновления с 1.8 до 1.10 нужно сначала обновить систему с 1.8 до 1.9, а затем выполнить еще одно обновление с 1.9 до 1.10.

```azurecli-interactive
az aks upgrade --name myAKSCluster --resource-group myResourceGroup --kubernetes-version 1.9.6
```

Выходные данные:

```json
{
  "id": "/subscriptions/<Subscription ID>/resourcegroups/myResourceGroup/providers/Microsoft.ContainerService/managedClusters/myAKSCluster",
  "location": "eastus",
  "name": "myAKSCluster",
  "properties": {
    "accessProfiles": {
      "clusterAdmin": {
        "kubeConfig": "..."
      },
      "clusterUser": {
        "kubeConfig": "..."
      }
    },
    "agentPoolProfiles": [
      {
        "count": 1,
        "dnsPrefix": null,
        "fqdn": null,
        "name": "myAKSCluster",
        "osDiskSizeGb": null,
        "osType": "Linux",
        "ports": null,
        "storageProfile": "ManagedDisks",
        "vmSize": "Standard_D2_v2",
        "vnetSubnetId": null
      }
    ],
    "dnsPrefix": "myK8sClust-myResourceGroup-4f48ee",
    "fqdn": "myk8sclust-myresourcegroup-4f48ee-406cc140.hcp.eastus.azmk8s.io",
    "kubernetesVersion": "1.9.6",
    "linuxProfile": {
      "adminUsername": "azureuser",
      "ssh": {
        "publicKeys": [
          {
            "keyData": "..."
          }
        ]
      }
    },
    "provisioningState": "Succeeded",
    "servicePrincipalProfile": {
      "clientId": "e70c1c1c-0ca4-4e0a-be5e-aea5225af017",
      "keyVaultSecretRef": null,
      "secret": null
    }
  },
  "resourceGroup": "myResourceGroup",
  "tags": null,
  "type": "Microsoft.ContainerService/ManagedClusters"
}
```

Проверьте, успешно ли прошло обновление, выполнив команду `az aks show`.

```azurecli-interactive
az aks show --name myAKSCluster --resource-group myResourceGroup --output table
```

Выходные данные:

```json
Name          Location    ResourceGroup    KubernetesVersion    ProvisioningState    Fqdn
------------  ----------  ---------------  -------------------  -------------------  ----------------------------------------------------------------
myAKSCluster  eastus     myResourceGroup  1.9.6                 Succeeded            myk8sclust-myresourcegroup-3762d8-2f6ca801.hcp.eastus.azmk8s.io
```

## <a name="next-steps"></a>Дополнительная информация

Узнайте больше о развертывании AKS и управлении ею из руководств по AKS.

> [!div class="nextstepaction"]
> [Руководство по AKS][aks-tutorial-prepare-app]

<!-- LINKS - external -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
