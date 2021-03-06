---
title: Развертывание экземпляров контейнеров в виртуальной сети Azure
description: Сведения о том, как развернуть группы контейнеров в новой или существующей виртуальной сети Azure.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: article
ms.date: 09/24/2018
ms.author: danlep
ms.openlocfilehash: feb9547b004141a3c1d02ef4b356b9d00b74fc95
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/10/2018
ms.locfileid: "48902375"
---
# <a name="deploy-container-instances-into-an-azure-virtual-network"></a>Развертывание экземпляров контейнеров в виртуальной сети Azure

[Виртуальная сеть Azure](../virtual-network/virtual-networks-overview.md) обеспечивает безопасное и конфиденциальное сетевое подключение, включая фильтрацию, маршрутизацию и пиринг для Azure и локальных ресурсов. Развертывание групп контейнеров в виртуальной сети Azure позволяет контейнерам безопасно обмениваться данными с другими ресурсами в виртуальной сети.

Группы контейнеров, развернутых в виртуальной сети Azure, позволяют реализовать такие сценарии:

* прямой обмен данными между группами контейнеров в одной и той же подсети;
* отправка выходных данных рабочей нагрузки [на основе задач](container-instances-restart-policy.md) из экземпляров контейнеров в базу данных в виртуальной сети;
* получение содержимого для экземпляров контейнеров из [конечной точки службы](../virtual-network/virtual-network-service-endpoints-overview.md) в виртуальной сети;
* обмен данными контейнеров с виртуальными машинами в виртуальной сети;
* обмен данными контейнеров с локальными ресурсами через [VPN-шлюз](../vpn-gateway/vpn-gateway-about-vpngateways.md) или [ExpressRoute](../expressroute/expressroute-introduction.md).

> [!IMPORTANT]
> Сейчас эта функция доступна в предварительной версии, и применяются [некоторые ограничения](#preview-limitations). Предварительные версии предоставляются только в том случае, если вы принимаете [дополнительные условия использования][terms-of-use]. Некоторые аспекты этой функции могут быть изменены до выхода общедоступной версии.

## <a name="virtual-network-deployment-limitations"></a>Ограничение развертывания в виртуальной сети

При развертывании групп контейнеров в виртуальной сети действуют определенные ограничения.

* Контейнеры Windows не поддерживаются.
* Для развертывания группы контейнеров в подсети эта подсеть не должна содержать другие типы ресурсов. Удалите все существующие ресурсы из существующей подсети, прежде чем развернуть в ней группы контейнеров, или создайте новую подсеть.
* Группы контейнеров, развернутых в виртуальной сети, сейчас не поддерживают общедоступные IP-адреса и метки DNS-имен.
* Из-за того, что задействуются дополнительные сетевые ресурсы, развертывание группы контейнеров в виртуальной сети обычно выполняется медленнее, чем развертывание стандартного экземпляра контейнера.

## <a name="preview-limitations"></a>Ограничения предварительной версии

Пока эта функция находится на этапе предварительной версии, при развертывании экземпляров контейнеров в виртуальной сети применяются следующие ограничения.

**Поддерживаемые** регионы:

* Западная Европа (westeurope).
* Западная часть США (westus).

**Неподдерживаемые** сетевые ресурсы:

* Группа безопасности сети
* Azure Load Balancer

Чтобы **удалить сетевые ресурсы**, вам потребуется выполнить [дополнительные действия](#delete-network-resources), если группы контейнеров развернуты в виртуальной сети.

## <a name="required-network-resources"></a>Необходимые сетевые ресурсы

Для развертывания групп контейнеров в виртуальной сети требуется три ресурса виртуальной сети Azure: сама [виртуальная сеть](#virtual-network), [делегированная подсеть](#subnet-delegated) в пределах этой виртуальной сети и [сетевой профиль](#network-profile).

### <a name="virtual-network"></a>Виртуальная сеть

Виртуальная сеть определяет адресное пространство, в котором можно создать одну или несколько подсетей. Затем следует развернуть ресурсы Azure (например, группы контейнеров) в подсетях виртуальной сети.

### <a name="subnet-delegated"></a>Подсеть (делегированная)

Подсети сегментируют виртуальную сеть на отдельные адресные пространства, которые применяются для размещения ресурсов Azure. В виртуальной сети следует создать одну или несколько подсетей.

Подсеть, используемая для групп контейнеров, может содержать только группы контейнеров. При первом развертывании группы контейнеров в подсети Azure делегирует эту подсеть службе "Экземпляры контейнеров Azure". После делегирования подсеть можно использовать только для групп контейнеров. Если попытаться развернуть в делегированной подсети ресурсы, отличные от групп контейнеров, операция завершится ошибкой.

### <a name="network-profile"></a>сетевой профиль.

Сетевой профиль — это шаблон конфигурации сети для ресурсов Azure. Он задает определенные свойства сети для ресурса, например подсеть, в который его следует развернуть. При первом развертывании группы контейнеров в подсети (а значит, и в виртуальной сети) Azure создает для вас сетевой профиль. Затем этот сетевой профиль можно использовать для будущих развертываний в подсети.

На следующей схеме несколько групп контейнеров развернуты в подсети, делегированной службе "Экземпляры контейнеров Azure". Развернув одну группу контейнеров в подсети, можно развернуть в ней дополнительные группы контейнеров, указав тот же сетевой профиль.

![Группы контейнеров в виртуальной сети][aci-vnet-01]

## <a name="deploy-to-virtual-network"></a>Развертывание в виртуальной сети

Группы контейнеров можно развернуть в новой виртуальной сети и позволить Azure создать необходимые сетевые ресурсы, или же выполнить развертывание в существующей виртуальной сети.

### <a name="new-virtual-network"></a>Новая виртуальная сеть

Чтобы обеспечить развертывание в новой виртуальной сети и автоматическое создание сетевых ресурсов в Azure, укажите следующее при выполнении команды [az container create][az-container-create]:

* Имя виртуальной сети
* Префикс адреса виртуальной сети в формате CIDR.
* Имя подсети
* Префикс адреса подсети в формате CIDR.

Префиксы адресов виртуальной сети и подсети определяют адресное пространство соответственно для виртуальной сети и подсети. Эти значения представлены в нотации бесклассовой междоменной маршрутизации (CIDR), например `10.0.0.0/16`. Дополнительные сведения о работе с подсетями см. в статье [Создание, изменение или удаление виртуальной сети](../virtual-network/virtual-network-manage-subnet.md).

Развернув первую группу контейнеров с помощью этого метода, можно выполнить развертывание в той же подсети, указав имена виртуальной сети и подсети, или же сетевой профиль, автоматически созданный в Azure. Так как Azure делегирует подсети службе "Экземпляры контейнеров Azure", в этой подсети можно развернуть *только* группы контейнеров.

### <a name="existing-virtual-network"></a>Существующая виртуальная сеть

Чтобы развернуть группу контейнеров в существующей виртуальной сети, сделайте следующее:

1. Создайте подсеть в существующей виртуальной сети или удалите в существующей подсети *все* другие ресурсы.
1. Разверните группу контейнеров с помощью команды [az container create][az-container-create] и укажите один из следующих вариантов настроек:
   * имя виртуальной сети и имя подсети;</br>
    или
   * идентификатор или имя сетевого профиля.

Когда вы развернете первую группу контейнеров в существующей подсети, Azure делегирует эту подсеть службе "Экземпляры контейнеров Azure". В этой подсети больше нельзя развернуть ресурсы, отличные от групп контейнеров.

В следующих разделах описывается, как развертывать группы контейнеров в виртуальной сети с помощью Azure CLI. Примеры команд форматированы для оболочки **Bash**. Если вы предпочитаете использовать другую оболочку, например PowerShell или командную строку, измените соответствующим образом символы продолжения строки.

## <a name="deploy-to-new-virtual-network"></a>Развертывание в новой виртуальной сети

Сначала разверните группу контейнеров и укажите параметры для новой виртуальной сети и подсети. Когда вы укажите эти параметры, Azure создаст виртуальную сеть и подсеть, делегирует подсеть службе "Экземпляры контейнеров Azure", а также создаст сетевой профиль. Когда эти ресурсы будут созданы, группа контейнеров будет развернута в подсети.

Выполните следующую команду [az container create][az-container-create], которая задает настройки для новой виртуальной сети и подсети. Эта команда развертывает контейнер [microsoft/aci-helloworld][aci-helloworld], который запускает небольшой веб-сервер на Node.js, обслуживающий статическую веб-страницу. В следующем разделе вы развернете вторую группу контейнеров в той же подсети и протестируете связь между двумя экземплярами контейнеров.

```azurecli
az container create \
    --name appcontainer \
    --resource-group myResourceGroup \
    --image microsoft/aci-helloworld \
    --vnet-name aci-vnet \
    --vnet-address-prefix 10.0.0.0/16 \
    --subnet aci-subnet \
    --subnet-address-prefix 10.0.0.0/24
```

Развертывание в новой виртуальной сети с помощью этого метода может занять несколько минут, пока создаются сетевые ресурсы. После развертывания первых групп контейнеров дополнительные группы будут развертываться быстрее.

## <a name="deploy-to-existing-virtual-network"></a>Развертывание в существующей виртуальной сети

Теперь, когда вы развернули группу контейнеров в новой виртуальной сети, разверните вторую группу контейнеров в той же подсети и проверьте связь между двумя экземплярами контейнеров.

Сначала получите IP-адрес первой развернутой группы контейнеров — *appcontainer*:

```azurecli
az container show --resource-group myResourceGroup --name appcontainer --query ipAddress.ip --output tsv
```

В выходных данных должен отображаться IP-адрес группы контейнеров в частной подсети:

```console
$ az container show --resource-group myResourceGroup --name appcontainer --query ipAddress.ip --output tsv
10.0.0.4
```

Теперь в качестве значения параметра `CONTAINER_GROUP_IP` укажите IP-адрес, полученный командой `az container show`, и выполните следующую команду `az container create`. Этот второй контейнер, *commchecker*, запускает образ на базе Alpine Linux и выполняет `wget` в отношении IP-адреса частной подсети первой группы контейнеров.

```azurecli
CONTAINER_GROUP_IP=<container-group-IP-here>

az container create \
    --resource-group myResourceGroup \
    --name commchecker \
    --image alpine:3.5 \
    --command-line "wget $CONTAINER_GROUP_IP" \
    --restart-policy never \
    --vnet-name aci-vnet \
    --subnet aci-subnet
```

Когда завершится развертывание второго контейнера, извлеките его журналы, чтобы просмотреть выходные данные выполненной им команды `wget`:

```azurecli
az container logs --resource-group myResourceGroup --name commchecker
```

Если второй контейнер успешно обменялся данными с первым, выходные данные должны выглядеть следующим образом:

```console
$ az container logs --resource-group myResourceGroup --name commchecker
Connecting to 10.0.0.4 (10.0.0.4:80)
index.html           100% |*******************************|  1663   0:00:00 ETA
```

Выходные данные журнала должны показывать, что программе `wget` удалось подключиться и скачать файл индекса из первого контейнера, используя его частный IP-адрес в локальной подсети. Сетевой трафик между двумя группами контейнеров оставался в пределах виртуальной сети.

## <a name="deploy-to-existing-virtual-network---yaml"></a>Развертывание в существующей виртуальной сети — YAML

Группу контейнеров можно также развернуть в существующей виртуальной сети с помощью YAML-файла. Для развертывания в подсети виртуальной сети следует указать несколько дополнительных свойств в YAML:

* `ipAddress` — параметры IP-адреса для группы контейнеров.
  * `ports` — открываемые порты, если таковые имеются.
  * `protocol` — протокол (TCP или UDP) для открытого порта.
* `networkProfile` — позволяет задать параметры сети, такие как виртуальная сеть и подсеть для ресурса Azure.
  * `id` — полный идентификатор ресурса Resource Manager для `networkProfile`.

Чтобы развернуть группу контейнеров в виртуальной сети с помощью файла YAML, сначала необходимо получить идентификатор сетевого профиля. Выполните команду [az network profile list][az-network-profile-list], указав имя группы ресурсов, которая содержит вашу виртуальную сеть и делегированную подсеть.

``` azurecli
az network profile list --resource-group myResourceGroup --query [0].id --output tsv
```

В выходных данных команды будет показан полный идентификатор ресурса для сетевого профиля.

```console
$ az network profile list --resource-group myResourceGroup --query [0].id --output tsv
/subscriptions/<Subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkProfiles/aci-network-profile-aci-vnet-aci-subnet
```

Получив идентификатор сетевого профиля, скопируйте следующий код YAML в новый файл с именем *vnet-deploy-aci.yaml*. В разделе `networkProfile` замените значение `id` только что полученным идентификатором, затем сохраните файл. Этот код YAML создает группу контейнеров с именем *appcontaineryaml* в виртуальной сети.

```YAML
apiVersion: '2018-09-01'
location: westus
name: appcontaineryaml
properties:
  containers:
  - name: appcontaineryaml
    properties:
      image: microsoft/aci-helloworld
      ports:
      - port: 80
        protocol: TCP
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
  ipAddress:
    type: Private
    ports:
    - protocol: tcp
      port: '80'
  networkProfile:
    id: /subscriptions/<Subscription ID>/resourceGroups/container/providers/Microsoft.Network/networkProfiles/aci-network-profile-aci-vnet-subnet
  osType: Linux
  restartPolicy: Always
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

Разверните группу контейнеров с помощью команды [az container create][az-container-create], передав имя YAML-файла в значении параметра `--file`:

```azurecli
az container create --resource-group myResourceGroup --file vnet-deploy-aci.yaml
```

Когда развертывание завершится, выполните команду [az container show][az-container-show], чтобы вывести данные о его состоянии:

```console
$ az container show --resource-group myResourceGroup --name appcontaineryaml --output table
Name              ResourceGroup    Status    Image                     IP:ports     Network    CPU/Memory       OsType    Location
----------------  ---------------  --------  ------------------------  -----------  ---------  ---------------  --------  ----------
appcontaineryaml  myResourceGroup  Running   microsoft/aci-helloworld  10.0.0.5:80  Private    1.0 core/1.5 gb  Linux     westus
```

## <a name="clean-up-resources"></a>Очистка ресурсов

### <a name="delete-container-instances"></a>Удаление экземпляров контейнеров

Когда завершите работу с созданными экземплярами контейнеров, удалите их с помощью следующих команд:

```azurecli
az container delete --resource-group myResourceGroup --name appcontainer -y
az container delete --resource-group myResourceGroup --name commchecker -y
az container delete --resource-group myResourceGroup --name appcontaineryaml -y
```

### <a name="delete-network-resources"></a>Удаление сетевых ресурсов

В первоначальной предварительной версии этой функции требуется несколько дополнительных команд для удаления сетевых ресурсов, созданных ранее. Если вы использовали примеры команд из предыдущих разделов этой статьи для создания виртуальной сети и подсети, можно удалить эти сетевые ресурсы с помощью следующего скрипта.

Перед выполнением скрипта задайте в качестве переменной `RES_GROUP` имя группы ресурсов, содержащей виртуальную сеть и подсеть, которые следует удалить. Скрипт отформатирован для оболочки Bash. Если вы предпочитаете использовать другую оболочку, например PowerShell или командную строку, измените методы доступа и присваивание значения переменной соответствующим образом.

> [!WARNING]
> Этот скрипт удаляет ресурсы! Он удаляет виртуальную сеть и все подсети, которые в ней содержатся. Убедитесь, что вам больше *не* требуются ресурсы в этой виртуальной сети, включая все содержащиеся в ней подсети, прежде чем запускать этот скрипт. После удаления **эти ресурсы восстановить невозможно**.

```azurecli
# Replace <my-resource-group> with the name of your resource group
RES_GROUP=<my-resource-group>

# Get network profile ID
NETWORK_PROFILE_ID=$(az network profile list --resource-group $RES_GROUP --query [0].id --output tsv)

# Delete the network profile
az network profile delete --id $NETWORK_PROFILE_ID -y

# Get the service association link (SAL) ID
SAL_ID=$(az network vnet subnet show --resource-group $RES_GROUP --vnet-name aci-vnet --name aci-subnet --query id --output tsv)/providers/Microsoft.ContainerInstance/serviceAssociationLinks/default

# Delete the default SAL ID for the subnet
az resource delete --ids $SAL_ID --api-version 2018-07-01

# Delete the subnet delegation to Azure Container Instances
az network vnet subnet update --resource-group $RES_GROUP --vnet-name aci-vnet --name aci-subnet --remove delegations 0

# Delete the subnet
az network vnet subnet delete --resource-group $RES_GROUP --vnet-name aci-vnet --name aci-subnet

# Delete virtual network
az network vnet delete --resource-group $RES_GROUP --name aci-vnet
```

## <a name="next-steps"></a>Дополнительная информация

В этой статье были кратко описаны несколько ресурсов и функций виртуальной сети. Более подробно эти темы рассматриваются в следующей документации по виртуальным сетям Azure:

* [Виртуальная сеть](../virtual-network/manage-virtual-network.md)
* [Подсеть](../virtual-network/virtual-network-manage-subnet.md)
* [Конечные точки службы](../virtual-network/virtual-network-service-endpoints-overview.md)
* [VPN-шлюз](../vpn-gateway/vpn-gateway-about-vpngateways.md)
* [ExpressRoute](../expressroute/expressroute-introduction.md)

<!-- IMAGES -->
[aci-vnet-01]: ./media/container-instances-vnet/aci-vnet-01.png

<!-- LINKS - External -->
[aci-helloworld]: https://hub.docker.com/r/microsoft/aci-helloworld/
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-show]: /cli/azure/container#az-container-show
[az-network-vnet-create]: /cli/azure/network/vnet#az-network-vnet-create
[az-network-profile-list]: /cli/azure/network/profile#az-network-profile-list