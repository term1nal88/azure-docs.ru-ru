---
title: Прогнозирование исправности автомобиля и манеры вождения с помощью Azure | Документация Майкрософт
description: Используйте возможности Cortana Intelligence, чтобы получить прогнозы и актуальную информацию об исправности и манере вождения автомобиля в режиме реального времени.
services: machine-learning
documentationcenter: ''
author: deguhath
ms.author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: 09fad60b-2f48-488b-8a7e-47d1f969ec6f
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2018
ms.openlocfilehash: 02a12e917ed36367ffac1ac2e7a1fef1c6098ea7
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/24/2018
ms.locfileid: "46985373"
---
# <a name="vehicle-telemetry-analytics-solution-playbook"></a>Обзор решения для анализа данных телеметрии, полученных с автомобиля
Из этого меню можно переходить к другим разделам обзора. 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../../includes/cap-vehicle-telemetry-playbook-selector.md)]

## <a name="overview"></a>Обзор
Суперкомпьютеры покинули стены лабораторий и теперь паркуются в гаражах. Ими оснащаются современные автомобили с множеством датчиков. Такие датчики позволяют отслеживать миллионы событий каждую секунду. К 2020 г. большинство таких автомобилей будет подключено к Интернету. Использование такого огромного количества данных повышает безопасность и надежность, открывая совершенно новые возможности в области управления автомобилями. Благодаря решению Cortana Intelligence от Майкрософт эта мечта становится явью.

Cortana Intelligence — это полностью управляемое решение для расширенной аналитики и работы с большими данными, которое позволяет преобразовывать данные в обоснованные действия. Шаблон решения Cortana Intelligence для анализа данных телеметрии, полученных с автомобиля, демонстрирует, как автодилеры, автопроизводители и страховые компании могут получать прогнозы и актуальные сведения об исправности автомобиля и манере вождения в режиме реального времени.

Решение реализуется как [шаблон лямбда-архитектуры](https://en.wikipedia.org/wiki/Lambda_architecture), тем самым демонстрируя весь потенциал платформы Cortana Intelligence для пакетной обработки в реальном времени.

## <a name="architecture"></a>Архитектура
На схеме ниже показана архитектура решения для анализа данных телеметрии, полученных с автомобиля.

![Схема архитектуры решения](./media/cortana-analytics-playbook-vehicle-telemetry/fig1-vehicle-telemetry-annalytics-solution-architecture.png)


Решение включает в себя перечисленные ниже компоненты Cortana Intelligence и демонстрирует их интеграцию:

* **Центры событий Azure**. Принимают миллионы событий телеметрии автомобиля в Azure.
* **Azure Stream Analytics**. Предоставляет подробные данные о работоспособности автомобиля в реальном времени и сохраняет их в долговременное хранилище для более детальной пакетной аналитики.
* **Машинное обучение Azure**. Обнаруживает аномалии в реальном времени и использует пакетную обработку для предоставления прогнозной аналитики.
* **Azure HDInsight**. Преобразует данные в требуемом масштабе.
* **Фабрика данных Azure**. Предназначена для координации, планирования и мониторинга конвейера пакетной обработки, а также управления ресурсами.
* **Power BI** предоставляет для этого решения расширенную панель мониторинга для визуализации данных в режиме реального времени и прогнозной аналитики.

Это решение использует два разных источника данных. 

* **Имитированные сигналы автомобиля и диагностики**: имитатор телематики автомобиля выдает диагностическую информацию и сигналы, которые соответствуют состоянию транспортного средства и стилю движения в определенный момент времени. 
* **Каталог автомобилей.** Эталонный набор данных, в котором VIN-номера сопоставлены с моделями автомобилей.

