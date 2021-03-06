---
title: Таблица планирования емкости для Azure Stack | Документация Майкрософт
description: Сведения о таблице планирования емкости для развертываний Azure Stack.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/18/2018
ms.author: jeffgilb
ms.reviewer: prchint
ms.openlocfilehash: 5ebddbf1fea49fbf868d15a544a18e5a8c6369fd
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/10/2018
ms.locfileid: "49078313"
---
# <a name="azure-stack-capacity-planner"></a>Планировщик ресурсов Azure Stack
Планировщик ресурсов Azure Stack представляет собой электронную таблицу для планирования емкости ресурсов Azure Stack. Планировщик ресурсов дает возможность проектировать разные распределения вычислительных ресурсов и анализировать, как они соответствуют существующим аппаратным решениям. Ниже приведены подробные инструкции по использованию калькулятора Azure Stack.

## <a name="worksheet-descriptions"></a>Описания рабочих листов
Ниже представлено краткое описание рабочих листов в электронной таблице планировщика ресурсов Azure Stack, который можно загрузить по адресу [http://aka.ms/azstackcapacityplanner](http://aka.ms/azstackcapacityplanner):

|Имя вкладки|ОПИСАНИЕ|
|-----|-----|
|Version-Disclaimer|Краткий обзор назначения калькулятора, номер версии и дата выпуска.|
|Указания|Предоставляет подробные инструкции по использованию планировщика ресурсов Azure Stack.|
|DefinedSolutionSKUs|Таблица с несколькими столбцами, которая содержит до пяти определений оборудования. На этом листе приводятся примеры конфигурации. Пользователю следует изменить эти сведения так, чтобы они соответствовали фактической конфигурации системы, которую он планирует использовать и (или) приобретать.|
|DefineByVMFootprint|Найдите подходящий номер SKU для оборудования, создав коллекцию виртуальных машин разных размеров и в разном количестве.|
|DefineByWorkloadFootprint|Найдите подходящий номер SKU для оборудования, создав коллекцию рабочих нагрузок Azure Stack.|
|  |  |

## <a name="definedsolutionskus-instructions"></a>Инструкции по DefinedSolutionSKUs
Этот лист содержит до пяти примеров определений оборудования. Измените эти сведения так, чтобы они соответствовали фактической конфигурации системы, которую вы планируете использовать и (или) приобретать.

### <a name="hardware-selections-provided-by-authorized-hardware-partners"></a>Параметры оборудования, предоставляемые авторизованными партнерами по оборудованию
Azure Stack предоставляется как интегрированная система с программным обеспечением, установленным нашими партнерами. Эти партнеры используют собственные, заслуживающие доверия версии средств для планирования ресурсов Azure Stack. Для окончательных решений о выборе емкости решения следует использовать именно эти инструменты.

### <a name="multiple-ways-to-model-computing-resources"></a>Несколько способов моделирования вычислительных ресурсов
Основные стандартные блоки, используемые для моделирования ресурсов в планировщике Azure Stack — это разные размеры виртуальных машин Azure Stack. Самый меньший из них обозначается Basic 0, а самый крупный на данный момент — Standard_Fsv2. В зависимости от конкретных потребностей для среды вычислений можно выделить разное количество разных виртуальных машин, вычисляя эти параметры с помощью этого средства двумя разными способами.

1. Пользователь выбирает в планировщике конкретное предложение по оборудованию и видит, какая комбинация разных ресурсов может разместиться на этом ресурсе оборудования. 

2. Пользователь создает в планировщике определенное сочетание виртуальных машин и позволяет калькулятору ресурсов Azure определить доступные номера SKU для оборудования, которые смогут поддерживать выбранную конфигурацию.

Это средство предоставляет два метода выделять ресурсы виртуальных машин — либо в составе единой коллекции ресурсов виртуальных машин, либо как коллекцию нескольких (до шести) конфигураций рабочих нагрузок. Каждая конфигурация рабочих нагрузок может содержать разные распределения доступных ресурсов виртуальных машины. Ниже представлены подробные сведения по созданию и использованию каждой из этих моделей выделения. На этом листе следует изменять только те значения, которые размещены в ячейках без фонового затенения или в раскрывающихся списках номеров. Изменение значений в затененных ячейках может нарушить вычисления ресурсов.


## <a name="definebyvmfootprint-instructions"></a>Инструкции по DefineByVMFootprint
Чтобы создать модель на основе коллекции виртуальных машины разных размеров и в разном количестве, перейдите на вкладку DefineByVMFootprint и выполните следующую последовательность действий.

1. В правом верхнем углу этого листа с помощью предоставленных элементов управления с раскрывающимися списками выберите начальное количество серверов (от 4 до 12) для размещения в каждой системе оборудования. Это число можно изменять на любом этапе процесса моделирования, чтобы оценить его влияние на общую доступность ресурсов для выбранной модели выделения ресурсов.
2. Если вы хотите смоделировать выделение разных ресурсов виртуальных машины для одной определенной конфигурации оборудования, найдите синий раскрывающийся список непосредственно под меткой "Current SKU" (Текущий номер SKU) в правом верхнем углу страницы. Раскройте этот список и выберите нужный номер SKU для оборудования.
3. Теперь вы можете добавлять в модель виртуальные машины разных размеров. Чтобы включить в список виртуальную машину определенного типа, введите значение количества в поле рядом с нужным типом, обведенное синим прямоугольником.

  > [!NOTE]
  > Для каждой виртуальной машины используется изначально выделенный размер хранилища. Размер хранилища отображается в поле со списком, и вы можете изменить его в соответствии с нужным уровнем ресурса хранилища для каждой виртуальной машины Azure Stack. Если в этом списке нет нужного размера хранилища, добавьте его самостоятельно. Для этого следует изменить любой из 10 исходных размеров в списке "Available Storage Configurations" (Доступные конфигурации хранилища) в правой части страницы.<br><br>Для каждой виртуальной машины используется изначально выделенный размер временного локального хранилища. Поскольку для временного хранилища используется тонкая подготовка, вы можете изменить значение local-temp на любой элемент из раскрывающегося меню, включая максимально допустимый размер временного хранилища.

4. При добавлении виртуальных машин вы увидите, как на диаграммах изменяются значения доступных ресурсов для номера SKU. Так вы можете в процессе моделирования контролировать, как на них добавление виртуальных машин разных размеров и в разном количестве. Есть еще один способ оценить влияние изменений — отслеживать значения Consumed (Израсходовано) и Still Available (Остается доступным) прямо под списком доступных виртуальных машин. Эти числа отражают приблизительные значения, основанные на характеристиках выбранного номера SKU оборудования.
5. Завершив создание набора виртуальных машин, вы можете просмотреть предлагаемые номера SKU оборудования, нажав кнопку Suggested SKU (Рекомендованный номер SKU) в правом верхнем углу страницы, непосредственно под меткой Current SKU (Текущий номер SKU). С помощью этой кнопки вы можете изменять конфигурации виртуальных машин и проверять, какие варианты оборудования поддерживают каждую конфигурацию.


## <a name="definebyworkloadfootprint-instructions"></a>Инструкции по DefineByWorkloadFootprint
Чтобы создать модель на основе коллекции рабочих нагрузок Azure Stack, перейдите на вкладку DefineByWorkloadFootprint и выполните следующую последовательность действий. Рабочие нагрузки в Azure Stack создаются с использованием доступных ресурсов виртуальной машины.   

> [!TIP]
> Чтобы изменить размер представленного хранилища для виртуальной машины Azure Stack, изучите примечание из шага 3 в предыдущем разделе.

1. В правом верхнем углу этой страницы с помощью предоставленных элементов управления с раскрывающимися списками выберите начальное количество серверов (от 4 до 12) для размещения в каждой системе оборудования.
2. Если вы хотите смоделировать выделение разных ресурсов виртуальных машины для одной определенной конфигурации оборудования, найдите синий раскрывающийся список непосредственно под меткой "Current SKU" (Текущий номер SKU) в правом верхнем углу страницы. Раскройте этот список и выберите нужный номер SKU для оборудования.
3. Выберите подходящий размер хранилища для каждой нужной виртуальной машины Azure Stack на странице DefineByVMFootprint, как описано выше в шаге 3 инструкций DefineByVMFootprint. Объем хранилища для одной виртуальной машины указывается в таблице DefineByVMFootprint.
4. Начиная с левого верхнего угла страницы DefineByWorkloadFootprint, создайте несколько конфигураций (до шести) разных типов рабочих нагрузок, вводя количество виртуальных машин каждого типа, включаемых в определенную рабочую нагрузку. Чтобы указать это количество, поместите числовое значение в столбец непосредственно под именем нужной рабочей нагрузки. Вы можете изменить имена рабочих нагрузок в соответствии с типом рабочих нагрузок, которые будут поддерживаться этой конкретной конфигурацией.
5. Вы можете включить определенное количество рабочих нагрузок каждого типа, введя значение в нижней части этого столбца непосредственно под меткой Quantity (Количество).
6. Завершив создание типов и количества рабочих нагрузок, нажмите кнопку Suggested SKU (Рекомендуемый номер SKU) в правом верхнем углу страницы, прямо под меткой Current SKU (Текущий номер SKU). Это действие отображает самый маленький номер SKU с достаточным объемом ресурсов для поддержки настроенной конфигурации рабочих нагрузок.
7. Для продолжения моделирования можно изменить количество серверов, выбранных для номера SKU оборудования, или типы и количество виртуальных машин, выбранных в конфигурациях рабочих нагрузок. Соответствующие графики немедленно продемонстрируют, как эти изменения влияют на общее потребление ресурсов.
8. Когда вы будете удовлетворены результатами изменений, повторно нажмите кнопку Suggested SKU (Рекомендуемый номер SKU), чтобы отобразить рекомендацию для новой конфигурации.


## <a name="next-steps"></a>Дополнительная информация
Изучите [рекомендации по интеграции центра обработки данных в Azure Stack](azure-stack-datacenter-integration.md).
