---
title: Избыточные в пределах зоны шлюзы виртуальной сети в Зонах доступности Azure | Документы Майкрософт
description: Узнайте о VPN-шлюзах и шлюзах ExpressRoute в Зонах доступности.
services: vpn-gateway
author: cherylmc
Customer intent: As someone with a basic network background, I want to understand zone-redundant gateways.
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 09/21/2018
ms.author: cherylmc
ms.openlocfilehash: 27bac5265a5e884b808c4ccb58fda0b2fffeb774
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/24/2018
ms.locfileid: "46975665"
---
# <a name="about-zone-redundant-virtual-network-gateways-in-azure-availability-zones"></a>Избыточные в пределах зоны шлюзы виртуальной сети в Зонах доступности Azure

Вы можете развернуть VPN-шлюзы и шлюзы ExpressRoute в [зонах доступности Azure](../availability-zones/az-overview.md). В результате шлюзы виртуальной сети смогут работать на более высоких уровнях отказоустойчивости, масштабируемости и доступности. При развертывании в зонах доступности Azure происходит физическое и логическое разделение шлюзов в пределах региона с одновременной защитой локального сетевого подключения к Azure от сбоев на уровне зоны.

### <a name="zrgw"></a>Шлюзы, избыточные в пределах зоны

Чтобы автоматически развернуть шлюзы виртуальной сети в зонах доступности, можно использовать шлюзы виртуальной сети, избыточные в пределах зоны. Благодаря избыточным в пределах зоны шлюзам можно получать доступ к критически важным и масштабируемым службам в Azure.

<br>
<br>

![схема с шлюзами, избыточными в пределах зоны](./media/create-zone-redundant-vnet-gateway/zonered.png)

### <a name="zgw"></a>Зональные шлюзы

Зональные шлюзы применяются для развертывания шлюзов в определенной зоне. При развертывании зонального шлюза все его экземпляры развертываются в одной зоне доступности.

<br>
<br>

![схема с зональными шлюзами](./media/create-zone-redundant-vnet-gateway/zonal.png)

## <a name="gwskus"></a>SKU шлюзов

Шлюзы, избыточные в пределах зоны, и зональные шлюзы используют новые номера SKU шлюзов. Мы добавили новые номера SKU шлюза виртуальной сети в регионах Зон доступности Azure. Эти номера SKU аналогичны соответствующим существующим номерам SKU для шлюза ExpressRoute и VPN-шлюза, за исключением того, что они относятся к шлюзам, избыточным в пределах зоны, и зональным шлюзам.

Доступны следующие новые номера SKU шлюзов:

### <a name="vpn-gateway"></a>VPN-шлюз

* VpnGw1AZ
* VpnGw2AZ
* VpnGw3AZ

### <a name="expressroute"></a>ExpressRoute

* ErGw1AZ
* ErGw2AZ
* ErGw3AZ

## <a name="pipskus"></a>Номера SKU общедоступных IP-адресов

Для избыточных в пределах зоны и зональных шлюзов применяется номер SKU *Стандартный* для общедоступного IP-ресурса Azure. Конфигурация общедоступного IP-ресурса Azure определяет, является ли развертываемый шлюз избыточным в пределах зоны или зональным. При создании общедоступного IP-ресурса с помощью SKU *Базовый* у шлюза не будет избыточности в пределах зоны, в ресурсы шлюза будут региональными.

### <a name="pipzrg"></a>Шлюзы, избыточные в пределах зоны

При создании общедоступного IP-адреса с помощью SKU **Стандартный** для общедоступного IP-адреса без указания зоны поведение зависит от типа шлюза — VPN-шлюз или шлюз ExpressRoute. 

* Если используется VPN-шлюз, для обеспечения избыточности в пределах зоны два экземпляра шлюза будут развернуты в любых двух из этих трех зон. 
* Если используется шлюз ExpressRoute и допускается наличие нескольких экземпляров, шлюз может развертываться во всех трех зонах.

### <a name="pipzg"></a>Зональные шлюзы

При создании общедоступного IP-адреса с помощью номера SKU **Стандартный** для общедоступных IP-адресов и указании зоны (1, 2 или 3) все экземпляры шлюза будут развернуты в одной зоне.

### <a name="piprg"></a>Региональные шлюзы

При создании общедоступного IP-адреса с помощью номера SKU **Базовый** для общедоступных IP-адресов шлюз развертывается как региональный и не имеет встроенной избыточности в пределах зоны.

## <a name="faq"></a>Часто задаваемые вопросы

### <a name="what-will-change-when-i-deploy-these-new-skus"></a>Что изменится после развертывания новых номеров SKU?

Вы можете развертывать шлюзы с избыточностью в пределах зоны. Это означает, что все экземпляры шлюзов будут развернуты в зонах доступности Azure, а каждая зона доступности является сочетанием домена сбоя и домена обновления. В результате шлюзы становятся более надежными, доступными и устойчивыми к сбоям на уровне зоны.

### <a name="can-i-use-the-azure-portal"></a>Я могу использовать портал Azure?

Да, вы можете использовать портал Azure для развертывания новых номеров SKU. Но вы увидите эти новые номера SKU только в тех регионах Azure, в которых есть Зоны доступности Azure.

### <a name="what-regions-are-available-for-me-to-use-the-new-skus"></a>В каких регионах можно использовать новые номера SKU?

Новые номера SKU доступны в тех регионах Azure, в которых есть Зоны доступности Azure: центральная часть США, Центральная Франция и Западная Европа. В будущем шлюзы, избыточные в пределах зоны, будут доступны и в других общедоступных регионах Azure.

### <a name="can-i-changemigrateupgrade-my-existing-virtual-network-gateways-to-zone-redundant-or-zonal-gateways"></a>Можно ли обновить существующие шлюзы виртуальной сети до шлюзов, избыточных в пределах зоны, или зональных шлюзов?

Сейчас миграция существующих шлюзов виртуальной сети на избыточные в пределах зоны или зональные шлюзы не поддерживается. Однако вы можете удалить существующий шлюз и повторно создать избыточный в пределах зоны или зональный шлюз.

### <a name="can-i-deploy-both-vpn-and-express-route-gateways-in-same-virtual-network"></a>Можно ли развертывать VPN-шлюзы и шлюзы Express Route в одной виртуальной сети?

Сосуществование VPN-шлюзов и шлюзов Express Route в одной виртуальной сети поддерживается. Но вам нужно зарезервировать диапазон IP-адресов /27 для подсети шлюза.

## <a name="next-steps"></a>Дальнейшие действия

[Создание шлюза виртуальной сети, избыточного в пределах зоны](create-zone-redundant-vnet-gateway.md)