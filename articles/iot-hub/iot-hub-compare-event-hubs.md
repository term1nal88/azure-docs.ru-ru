---
title: Сравнение Центра Интернета вещей Azure и Центров событий Azure | Документация Майкрософт
description: Сравнение служб Центра Интернета вещей и Центров событий Azure, в котором выделены функциональные различия и случаи использования. Сравниваются поддерживаемые протоколы, функции мониторинга, управления устройствами и передачи файлов.
author: kgremban
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: kgremban
ms.openlocfilehash: 830052341c4f0e3488c8e63da59cbef1f72e158a
ms.sourcegitcommit: a2ae233e20e670e2f9e6b75e83253bd301f5067c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/13/2018
ms.locfileid: "42143853"
---
# <a name="connecting-iot-devices-to-azure-iot-hub-and-event-hubs"></a>Подключение устройств Интернета вещей в Azure. Центр Интернета вещей и Центры событий

Azure предоставляет службы, специально разработанные для различных типов подключения и обмена данными для подключения данных к облачным возможностям. Центр Интернета вещей и Центры событий Azure являются облачными службами, которые принимают большие объемы данных и обрабатывают или сохраняют эти данные для бизнес-аналитики. Две службы похожи тем, что они поддерживают прием данных с небольшой задержкой и высокой надежностью, но они предназначены для разных целей. Центр Интернета вещей разработан специально для удовлетворения уникальных требований к подключению масштабируемых устройств Интернета вещей к облаку Azure, а Центры событий — для потоковой передачи больших объемов данных. Именно поэтому корпорация Майкрософт рекомендует использовать Центр Интернета вещей Azure для подключения устройств Интернета вещей к Azure.

Центр Интернета вещей Azure представляет собой облачный шлюз, соединяющий устройства Интернета вещей и собирающий данные для автоматизации и активации бизнес-аналитики. Кроме того, Центр Интернета вещей включает в себя возможности, приумножающие связь между устройствами и серверными системами. Возможности двунаправленного обмена данными означают, что при получении данных с устройств вы также можете отправлять команды и политики обратно на устройства, например для обновления свойств или вызова действий управления устройством.  Такая возможность подключения из облака на устройство также обеспечивает важную возможность доставки облачных интеллектуальных средств пограничным устройствам с помощью Azure IoT Edge. Уникальное удостоверение на уровне устройств, предоставляемое в Центре Интернета вещей, позволяет обеспечить оптимальную защиту решения Интернета вещей от возможных атак. 

[Центры событий Azure](../event-hubs/event-hubs-what-is-event-hubs.md) — служба Azure, позволяющая выполнять потоковую передачу больших данных. Она предназначена для сценариев потоковой передачи данных с высокой пропускной способностью, где клиенты могут отправлять миллиарды запросов в день. Центры событий используют модель секционирования потребителей, чтобы масштабировать потоковую передачу. Они интегрированы со службами больших данных и аналитики Azure, включая Databricks, Stream Analytics, ADLS и HDInsight. Благодаря таким функциям, как "Сбор" в Центрах событий" и "Автоматическое расширение", эта служба предназначена для поддержки приложений и решений для работы с большими данными. Кроме того, Центр Интернета вещей использует Центры событий для собственного пути потоковой передачи телеметрии, что позволяет решению Интернета вещей использовать огромные возможности Центров событий.

Таким образом, в то время как оба решения предназначены для приема больших объемов данных, только Центр Интернета вещей предоставляет большие возможности для Интернета вещей, позволяющие извлечь максимальную пользу для бизнеса из подключения устройств Интернета вещей к облаку Azure.  Начиная работу с Интернетом вещей с помощью Центра Интернета вещей для поддержки сценариев приема данных, вы непременно получите мгновенный доступ ко всем функциональным возможностям Интернета вещей, как только они понадобятся (для бизнеса и технических целей).

В следующей таблице приведены сведения о сопоставлении двух уровней Центра Интернета вещей с Центрами событий при оценке их возможностей для Центра Интернета вещей. Дополнительные сведения об уровнях "Базовый" и "Стандартный" Центра Интернета вещей см. в статье [Выбор правильного уровня Центра Интернета вещей для решения](iot-hub-scaling.md).

| Возможность Центра Интернета вещей | Уровень "Стандартный" Центра Интернета вещей | Уровень "Базовый" Центра Интернета вещей | Центры событий; |
| --- | --- | --- | --- |
| Передача сообщений с устройства в облако | ![Проверка][checkmark] | ![Проверка][checkmark] | ![Проверка][checkmark] |
| Протоколы: HTTPS, AMQP, AMQP через WebSocket | ![Проверка][checkmark] | ![Проверка][checkmark] | ![Проверка][checkmark] |
| Протоколы: MQTT, MQTT через WebSocket | ![Проверка][checkmark] | ![Проверка][checkmark] |  |
| Удостоверение для каждого устройства | ![Проверка][checkmark] | ![Проверка][checkmark] |  |
| Передача файлов с устройств | ![Проверка][checkmark] | ![Проверка][checkmark] |  |
| Служба подготовки устройств | ![Проверка][checkmark] | ![Проверка][checkmark] |  |
| Передача сообщений из облака на устройство | ![Проверка][checkmark] |  |  |
| Двойник устройства и управление устройством | ![Проверка][checkmark] |  |  |
| IoT Edge | ![Проверка][checkmark] |  |  |

Даже если единственным вариантом использования является передача данных с устройства в облако, мы настоятельно рекомендуем использовать Центр Интернета вещей, так как он предоставляет службу, разработанную для подключения устройств Интернета вещей. 

### <a name="next-steps"></a>Дополнительная информация

Чтобы продолжить изучение возможностей Центра Интернета вещей, ознакомьтесь со статьей [Руководство разработчика для Центра Интернета вещей Azure](iot-hub-devguide.md).

<!-- This one reference link is used over and over. --robinsh -->
[checkmark]: ./media/iot-hub-compare-event-hubs/ic195031.png
