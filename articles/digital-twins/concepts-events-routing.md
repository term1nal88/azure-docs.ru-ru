---
title: Маршрутизация событий и сообщений с помощью Azure Digital Twins | Документация Майкрософт
description: Общие сведения о маршрутизации событий и сообщений в конечные точки службы с помощью Digital Twins
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/08/2018
ms.author: alinast
ms.openlocfilehash: 9b861f0991b11637c7b27b645d4506e658ea53b3
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/15/2018
ms.locfileid: "49324033"
---
# <a name="routing-events-and-messages"></a>Маршрутизация событий и сообщений

Решения для Интернета вещей часто объединяют несколько мощных служб, включая службу хранилища, аналитики и другие. В этой статье описывается, как подключить приложения Azure Digital Twins к службе аналитики Azure, ИИ и службам хранилища, чтобы расширить их за счет более глубоких сведений и функций.

## <a name="route-types"></a>Типы маршрутов

Digital Twins предлагает два метода интеграции событий Интернета вещей в другие службы Azure или бизнес-приложения.

* **Маршрутизация событий Digital Twins**: события Digital Twins можно активировать при изменении объекта в пространственном графе, получении данных телеметрии или при создании уведомления на основе предварительно определенных условий с помощью определяемой пользователем функции. Пользователи могут отправлять эти события в [Центры событий](https://azure.microsoft.com/services/event-hubs/), [разделы служебной шины](https://azure.microsoft.com/services/service-bus/) или [Сетку событий](https://azure.microsoft.com/services/event-grid/) для дальнейшей обработки.

* **Маршрутизация данных телеметрии устройства**: кроме маршрутизации событий, Digital Twins также может направлять необработанные сообщения телеметрии устройства в Центры событий для последующего анализа. Такие типы сообщений не обрабатываются Digital Twins — они только перенаправляются в **Центр событий**.

Для отправки событий или пересылки сообщений пользователи могут указать одну или несколько исходящих конечных точек. События и сообщения будут отправляться в конечные точки в соответствии с этими предопределенными настройками маршрутизации. Другими словами, пользователи могут указать одну определенную конечную точку для получения событий операций графа и другую для получения событий телеметрии устройства и т. д.

![Маршрутизация событий Digital Twins][1]

При маршрутизации в **Центры событий** будет поддерживаться порядок, в котором отправляются сообщения телеметрии. Таким образом, они поступят в конечную точку в той же последовательности, в какой были изначально получены. **Сетка событий** и **служебная шина** не гарантируют, что конечные точки получат события в порядке их возникновения. Однако в схеме события есть метка времени, с помощью которой можно узнать порядок поступления событий в конечную точку.

## <a name="route-implementation"></a>Реализация маршрутизации

Служба Digital Twins в настоящее время поддерживает следующие типы **EndpointTypes**:

* **EventHub** — конечная точка строки подключения концентратора событий.
* **ServiceBus** — конечная точка строки подключения служебной шины.
* **EventGrid** — конечная точка строки подключения Сетки событий.

Digital Twins в настоящее время поддерживает следующие типы **EventTypes**, которые будут отправлены в выбранную конечную точку:

* **DeviceMessages** — сообщения телеметрии, отправленные с устройств пользователей и перенаправленные системой.
* **TopologyOperation** — это операции изменения графа или его метаданных. Например, добавление или удаление такой сущности, как пространство.
* **SpaceChange** — изменения в вычисленном значении пространства как результат сообщения телеметрии устройства.
* **SensorChange** — изменения в вычисленном значении датчика как результат сообщения телеметрии устройства.
* **UdfCustom** — настраиваемые уведомления из определяемой пользователем функции.

> [!IMPORTANT]
> Не все типы **EndpointTypes** поддерживают все типы **EventTypes**.
> Ознакомьтесь с таблицей ниже, чтобы узнать, какие типы **EventTypes** поддерживаются для типов **EndpointType**.

|             | DeviceMessages | TopologyOperation | SpaceChange | SensorChange | UdfCustom |
| ----------- | -------------- | ----------------- | ----------- | ------------ | --------- |
| **EventHub**|     X          |         X         |     X       |      X       |   X       |
| **Служебная шина**|              |         X         |     X       |      X       |   X       |
| **Сетка событий**|               |         X         |     X       |      X       |   X       |

>[!NOTE]
>Дополнительные сведения о создании конечных точек и примеры использования схемы событий см. в статье, посвященной [конечным точкам и исходящему трафику](how-to-egress-endpoints.md).

## <a name="next-steps"></a>Дополнительная информация

Чтобы узнать об ограничениях общедоступной предварительной версии, ознакомьтесь с [этой](concepts-service-limits.md) статьей.

Ознакомиться с примером Digital Twins на практике можно в [кратком руководстве по обнаружению свободных комнат](quickstart-view-occupancy-dotnet.md).

<!-- Images -->
[1]: media/concepts/digital-twins-events-routing.png