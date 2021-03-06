---
title: Источники данных мониторинга в Azure
description: Узнайте о всех источниках данных мониторинга, доступных в Azure в настоящее время.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/12/2018
ms.author: johnkem
ms.component: ''
ms.openlocfilehash: 55dc43f238b487392be48047b3d608f84e1e3ee2
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/14/2018
ms.locfileid: "45579316"
---
# <a name="consume-monitoring-data-from-azure"></a>Использование данных мониторинга из Azure

Данные мониторинга всей платформы Azure собираются в одном месте с помощью конвейера Azure Monitor, но пока, как показывает практика, в этом конвейере доступны не все данные мониторинга. В этой статье собраны различные способы программного доступа к данным мониторинга из служб Azure.

## <a name="options-for-data-consumption"></a>Варианты использования данных

| Тип данных | Категория | Поддерживаемые службы | Варианты доступа |
| --- | --- | --- | --- |
| Метрики Azure Monitor уровня платформы | Метрики | [Список доступен здесь](monitoring-supported-metrics.md). | <ul><li>**REST API:** [API метрик Azure Monitor](https://docs.microsoft.com/rest/api/monitor/metrics).</li><li>**Большой двоичный объект службы хранилища или концентратор событий:** [параметры диагностики](monitoring-overview-of-diagnostic-logs.md#diagnostic-settings).</li></ul> |
| Метрики гостевой ОС (например, счетчики производительности) | Метрики | Виртуальные машины [Windows](../virtual-machines-dotnet-diagnostics.md) и Linux (версии 2), [облачные службы](../cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md), [Service Fabric](../service-fabric/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md). | <ul><li>**Таблица или большой двоичный объект службы хранилища:** [система диагностики Azure для Windows или Linux](../cloud-services/cloud-services-dotnet-diagnostics-storage.md).</li><li>**Концентратор событий:** [система диагностики Microsoft Azure](../event-hubs/event-hubs-streaming-azure-diags-data.md).</li></ul> |
| Пользовательские метрики или метрики приложения | Метрики | Любое приложение, инструментированное с помощью Application Insights. | <ul><li>**REST API:** [REST API Application Insights](https://dev.applicationinsights.io/reference).</li></ul> |
| Метрики хранения | Метрики | Хранилище Azure | <ul><li>**Таблица службы хранилища:** [аналитика службы хранилища](https://docs.microsoft.com/rest/api/storageservices/storage-analytics).</li></ul> |
| Данные об оплате | Метрики | Все службы Azure. | <ul><li>**REST API:** [использование ресурсов Azure и интерфейсы API RateCard](../billing/billing-usage-rate-card-overview.md)</li></ul> |
| Журнал действий | События | Все службы Azure. | <ul><li>**REST API:** [API событий Azure Monitor](https://docs.microsoft.com/rest/api/monitor/eventcategories).</li><li>**Большой двоичный объект службы хранилища или концентратор событий:** [профиль журнала](monitoring-overview-activity-logs.md#export-the-activity-log-with-a-log-profile).</li></ul> |
| Журналы диагностики Azure Monitor | События | [Список доступен здесь](monitoring-diagnostic-logs-schema.md). | <ul><li>**Большой двоичный объект службы хранилища или концентратор событий:** [параметры диагностики](monitoring-overview-of-diagnostic-logs.md#diagnostic-settings).</li></ul> |
| Журналы гостевой ОС (например, IIS, трассировка событий Windows, системные журналы) | События | Виртуальные машины [Windows](../virtual-machines-dotnet-diagnostics.md) и Linux (версии 2), [облачные службы](../cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md), [Service Fabric](../service-fabric/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md). | <ul><li>**Таблица или большой двоичный объект службы хранилища:** [система диагностики Azure для Windows или Linux](../cloud-services/cloud-services-dotnet-diagnostics-storage.md).</li><li>**Концентратор событий:** [система диагностики Microsoft Azure](../event-hubs/event-hubs-streaming-azure-diags-data.md).</li></ul> |
| Журналы службы приложений | События | Службы приложений | <ul><li>**Хранилище файлов, таблиц или BLOB-объектов:** [диагностика веб-приложений](../app-service/web-sites-enable-diagnostic-log.md).</li></ul> |
| Журналы хранилища | События | Хранилище Azure | <ul><li>**Таблица службы хранилища:** [аналитика службы хранилища](https://docs.microsoft.com/rest/api/storageservices/storage-analytics).</li></ul> |
| Оповещения центра безопасности | События | Центр безопасности Azure | <ul><li>**REST API:** [оповещения системы безопасности](https://msdn.microsoft.com/library/mt704050.aspx).</li></ul> |
| Отчеты Azure Active Directory | События | Azure Active Directory | <ul><li>**REST API:** [API Graph Azure Active Directory](../active-directory/active-directory-reporting-api-getting-started.md).</li></ul> |
| Состояние ресурсов центра безопасности | Status | [Все поддерживаемые ресурсы](https://msdn.microsoft.com/library/mt704041.aspx#Anchor_1). | <ul><li>**REST API:** [состояния системы безопасности](https://msdn.microsoft.com/library/mt704041.aspx).</li></ul> |
| Работоспособность ресурса | Status | Поддерживаемые службы | <ul><li>**REST API:** [REST API работоспособности ресурсов](https://azure.microsoft.com/blog/reduce-troubleshooting-time-with-azure-resource-health/).</li></ul> |
| Оповещения о метриках Azure Monitor | Уведомления | [Список доступен здесь](monitoring-supported-metrics.md). | <ul><li>**Webhook:** [оповещения о метриках Azure](insights-webhooks-alerts.md).</li></ul> |
| Оповещения журнала действий Azure Monitor | Уведомления | Все службы Azure. | <ul><li>**Webhook**: оповещения журнала действий Azure.</li></ul> |
| Уведомления об автомасштабировании | Уведомления | [Список доступен здесь](monitoring-overview-autoscale.md#supported-services-for-autoscale). | <ul><li>**Webhook:** [схема полезных данных webhook уведомлений об автомасштабировании](insights-autoscale-to-webhook-email.md#autoscale-notification-webhook-payload-schema).</li></ul> |
| Оповещения о запросах поиска по журналам | Уведомления | Log Analytics | <ul><li>**Webhook:** [действия веб-перехватчика для правил оповещений журнала](../monitoring-and-diagnostics/monitor-alerts-unified-log-webhook.md).</li></ul> |
| Оповещения о метриках Application Insights | Уведомления | Application Insights | <ul><li>**Webhook:** [оповещения Application Insights](../application-insights/app-insights-alerts.md).</li></ul> |
| Веб-тесты Application Insights | Уведомления | Application Insights | <ul><li>**Webhook:** [оповещения Application Insights](../application-insights/app-insights-alerts.md).</li></ul> |

## <a name="next-steps"></a>Дополнительная информация

- Узнайте больше о [метриках Azure Monitor](monitoring-overview-metrics.md).
- Узнайте больше о [журнале действий Azure](monitoring-overview-activity-logs.md).
- Узнайте больше о [журналах диагностики Azure](monitoring-overview-of-diagnostic-logs.md).
