---
title: Конечные точки службы и правила виртуальной сети для Центров событий Azure | Документация Майкрософт
description: Добавление в виртуальную сеть конечных точек службы Microsoft.EventHub.
services: event-hubs
documentationcenter: ''
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.date: 08/16/2018
ms.author: shvija
ms.openlocfilehash: ff0ebbb140627caaaa71c5d09d0a4078eca86055
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2018
ms.locfileid: "48888140"
---
# <a name="use-virtual-network-service-endpoints-with-azure-event-hubs"></a>Использование конечных точек службы виртуальной сети с Центрами событий Azure

Интеграция Центров событий с [конечными точками службы виртуальной сети][vnet-sep] обеспечивает безопасный доступ к возможностям обмена сообщениями из рабочих нагрузок, таких как виртуальные машины, привязанные к виртуальным сетям, где трафик защищен с обеих сторон. 

> [!IMPORTANT]
> Виртуальные сети поддерживаются в **стандартном** и **выделенном** уровнях Центра событий. В "базовом" уровне не поддерживается. 

После настройки привязки по крайней мере к одной конечной точке службы подсети виртуальной сети пространство имен соответствующих концентраторов событий принимает трафик только из авторизованной виртуальной сети. С точки зрения виртуальной сети привязка пространства имен Центров событий к конечной точке службы настраивает изолированный сетевой туннель от подсети виртуальной сети к службе обмена сообщениями.

Результатом является частная и изолированная взаимосвязь между рабочими нагрузками, привязанными к подсети, и соответствующим пространством имен Центров событий, несмотря на то что наблюдаемый сетевой адрес конечной точки службы обмена сообщениями находится в общедоступном диапазоне IP-адресов.

## <a name="advanced-security-scenarios-enabled-by-vnet-integration"></a>Расширенные сценарии обеспечения безопасности, доступные при интеграции с виртуальной сетью 

Решения, требующие жесткой и раздельной защиты, где подсети виртуальной сети обеспечивают сегментирование между разделенными службами, обычно все же нуждаются в путях взаимодействия между службами, находящимися в этих секциях.

Любой прямой IP-маршрут между секциями, включая передачу трафика HTTPS через TCP/IP, несет риск использования уязвимостей на сетевом уровне. Службы обмена сообщениями обеспечивают полностью изолированные пути взаимодействия, где сообщения даже записываются на диск при переходе между сторонами. Рабочие нагрузки в двух разных виртуальных сетях, которые связаны с одним и тем же экземпляром Центров событий, могут эффективно и надежно связываться через сообщения, в то время как целостность границ изолированной сети сохраняется.
 
Это означает, что ваши конфиденциальные облачные решения, влияющие на безопасность, не только получают доступ к ведущим в отрасли надежным и масштабируемым асинхронным службам обмена сообщениями Azure, но теперь они могут использовать обмен сообщениями для создания путей передачи данных между защищенными секциями решений, которые по своей сути более безопасны, чем степень безопасности в любом режиме одноранговой связи, включая HTTPS и другие протоколы сокетов TLS.

## <a name="bind-event-hubs-to-virtual-networks"></a>Привязка Центров событий к виртуальным сетям

*Правила виртуальной сети* — это функция безопасности брандмауэра, которая контролирует, принимает ли сервер Центров событий Azure соединения из определенной подсети виртуальной сети.

Привязка пространства имен Центров событий к виртуальной сети состоит из двух этапов. Сначала необходимо создать **конечную точку службы виртуальной сети** в подсети виртуальной сети и подключить ее к Microsoft.EventHub, как описано в статье [Конечные точки службы виртуальной сети][vnet-sep]. После добавления конечной точки службы вы привязываете пространство имен Центров событий к ней с помощью *правила виртуальной сети*.

Правило виртуальной сети — это именованная связь пространства имен Центров событий с подсетью виртуальной сети. Пока правило существует, всем рабочим нагрузкам, привязанным к подсети, предоставляется доступ к пространству имен Центров событий. Центры событий никогда не устанавливают исходящие подключения, не нуждаются в доступе и поэтому не получают доступ к вашей подсети, если это правило включено.

### <a name="create-a-virtual-network-rule-with-azure-resource-manager-templates"></a>Создание правила виртуальной сети с использованием шаблонов Azure Resource Manager

Следующий шаблон Resource Manager позволяет добавить правило виртуальной сети в существующее пространство имен Центров событий.

Параметры шаблона:

* **namespaceName**. Пространство имен Центров событий.
* **vnetRuleName**. Имя создаваемого правила виртуальной сети.
* **virtualNetworkingSubnetId**: полный путь Resource Manager для подсети виртуальной сети, например `subscriptions/{id}/resourceGroups/{rg}/providers/Microsoft.Network/virtualNetworks/{vnet}/subnets/default` для подсети виртуальной сети по умолчанию.

```json
{  
   "$schema":"http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
   "contentVersion":"1.0.0.0",
   "parameters":{     
          "namespaceName":{  
             "type":"string",
             "metadata":{  
                "description":"Name of the namespace"
             }
          },
          "vnetRuleName":{  
             "type":"string",
             "metadata":{  
                "description":"Name of the Authorization rule"
             }
          },
          "virtualNetworkSubnetId":{  
             "type":"string",
             "metadata":{  
                "description":"subnet Azure Resource Manager ID"
             }
          }
      },
    "resources": [
        {
            "apiVersion": "2018-01-01-preview",
            "name": "[concat(parameters('namespaceName'), '/', parameters('vnetRuleName'))]",
            "type":"Microsoft.EventHub/namespaces/VirtualNetworkRules",         
            "properties": {             
                "virtualNetworkSubnetId": "[parameters('virtualNetworkSubnetId')]"  
            }
        } 
    ]
}
```

Инструкции по развертыванию шаблона см. в статье [Развертывание ресурсов с использованием шаблонов Resource Manager и Azure PowerShell][lnk-deploy].

## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения о виртуальных сетях см. по следующим ссылкам:

- [Конечные точки служб виртуальной сети][vnet-sep]
- [IP-фильтры Центров событий Azure][ip-filtering]

[vnet-sep]: ../virtual-network/virtual-network-service-endpoints-overview.md
[lnk-deploy]: ../azure-resource-manager/resource-group-template-deploy.md
[ip-filtering]: event-hubs-ip-filtering.md