---
title: Создание рабочей области Log Analytics на портале Azure | Документация Майкрософт
description: Узнайте, как создать рабочую область Log Analytics для включения решений по управлению и сбора данных из облачной и локальной сред на портале Azure.
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptal
ms.date: 08/23/2018
ms.author: magoedte
ms.component: ''
ms.openlocfilehash: edebeec493b025a81a99c0458344aafe59e769e9
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/02/2018
ms.locfileid: "48040880"
---
# <a name="create-a-log-analytics-workspace-in-the-azure-portal"></a>Создание рабочей области Log Analytics на портале Azure
На портале Azure вы можете настроить рабочую область Log Analytics — уникальную среду с собственными репозиторием данных, источниками данных и решениями.  Действия, описанные в этой статье, помогут вам настроить сбор данные из следующих источников:

* ресурсы Azure в подписке;
* локальные компьютеры, которые отслеживает System Center Operations Manager;
* коллекции устройств из System Center Configuration Manager; 
* Данные диагностики и журнала из Службы хранилища Azure

Сведения о других источниках, таких как виртуальные машины Azure и виртуальные машины Windows или Linux в вашей среде, см. в статьях ниже.

*  [Сбор данных о виртуальных машинах Azure](log-analytics-quick-collect-azurevm.md) 
*  [Настройка агента Log Analytics для компьютеров Linux в гибридной среде](log-analytics-quick-collect-linux-computer.md)
*  [Настройка агента Log Analytics для компьютеров Windows в гибридной среде](log-analytics-quick-collect-windows-computer.md)

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.

## <a name="sign-in-to-azure-portal"></a>Вход на портал Azure
Войдите на портал Azure по адресу [https://portal.azure.com](https://portal.azure.com). 

## <a name="create-a-workspace"></a>Создание рабочей области
1. На портале Azure щелкните **Все службы**. В списке ресурсов введите **Log Analytics**. Как только вы начнете вводить символы, список отфильтруется соответствующим образом. Выберите **Log Analytics**.

    ![Портал Azure](media/log-analytics-quick-collect-azurevm/azure-portal-01.png)
  
2. Щелкните **Создать** и задайте следующие параметры:

  * Введите имя для новой **рабочей области Log Analytics**, например *DefaultLAWorkspace*. 
  * Выберите в раскрывающемся списке **подписку**, с которой нужно связать рабочую область, если выбранная по умолчанию не подходит.
  * Для **группы ресурсов** выберите существующую и уже настроенную группу ресурсов или создайте новую.  
  * Выберите доступное **расположение**.  Дополнительные сведения о доступности службы Log Analytics в регионах см. в [этой статье](https://azure.microsoft.com/regions/services/).
  * При создании рабочей области в новой подписке, созданной после 2 апреля 2018 г., будет автоматически использоваться тарифный план *За ГБ*, и выбор ценовой категории будет недоступен.  При создании рабочей области в существующей подписке, созданной до 2 апреля, или в подписке, которая была привязана к существующей регистрации Соглашения Enterprise (EA), выберите нужную ценовую категорию.  Дополнительные сведения о конкретной ценовой категории см. в статье [Цены на Log Analytics](https://azure.microsoft.com/pricing/details/log-analytics/).

        ![Create Log Analytics resource blade](media/log-analytics-quick-collect-azurevm/create-loganalytics-workspace-02.png)  

3. После ввода необходимых сведений в области **Рабочая область Log Analytics** щелкните **OK**.  

Пока проверяются данные, ход создания рабочей области можно проверить в разделе **Уведомления** в меню. 

## <a name="next-steps"></a>Дополнительная информация
Теперь, когда рабочая область доступна, вы можете настроить сбор данных телеметрии для мониторинга, выполнять поиск по журналам для анализа этих данных, а также добавить решение по управлению для предоставления дополнительных данных и аналитических сведений. 

* Сведения о том, как включить сбор данных из ресурсов Azure с помощью системы диагностики Azure или службы хранилища Azure, см. в статье [Сбор журналов и метрик для служб Azure для использования в Log Analytics](log-analytics-azure-storage.md).  
* Добавьте [System Center Operations Manager в качестве источника данных](log-analytics-om-agents.md), чтобы собирать данные с агентов, которые предоставляют отчеты группе управления Operations Manager, и хранить эти данные в рабочей области Log Analytics. 
* Подключите [Configuration Manager](log-analytics-sccm.md) для импорта данных с компьютеров, которые являются элементами коллекций в иерархии.  
* Просмотрите список доступных [решений по управлению](https://docs.microsoft.com/azure/monitoring/monitoring-solutions-inventory?toc=%2fazure%2flog-analytics%2ftoc.json) и узнайте, как добавить решение в рабочую область или удалить его из нее.
