---
title: Что происходит с Azure Machine Learning Workbench? | Документация Майкрософт
description: Узнайте о том, что происходит с приложением Workbench, что изменилось в Машинном обучении Azure и каков срок предоставляемой поддержки.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: overview
ms.reviewer: jmartens
author: j-martens
ms.author: jmartens
ms.date: 09/24/2018
ms.openlocfilehash: 88e7dad15a7080c4132a6983d949f9451ad5ce69
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/03/2018
ms.locfileid: "48239271"
---
# <a name="what-is-happening-to-workbench-in-azure-machine-learning-preview"></a>Что происходит с Workbench (предварительная версия) в Машинном обучении Azure?

Приложение Workbench и некоторые другие ранние функции были заменены в сентябре 2018 г. другим выпуском с улучшениями [архитектуры](concept-azure-machine-learning-architecture.md). Этот выпуск содержит множество существенных обновлений по результатам отзывов клиентов для улучшения удобства работы. Базовые функциональные возможности, от экспериментальных запусков до развертывания модели, остались без изменений, но теперь можно использовать надежный пакет <a href="http://aka.ms/aml-sdk" target="_blank">SDK</a> и [интерфейс командной строки](reference-azure-machine-learning-cli.md) (CLI) для выполнения задач машинного обучения и конвейеров.  

В этой статье рассказывается, что было изменено и как эти изменения повлияют на уже существующие применения службы "Машинное обучение Azure".

## <a name="what-changed"></a>Что изменилось?

Последний выпуск Машинного обучения Azure включает следующие возможности:
+ [Упрощенная модель ресурсов Azure](concept-azure-machine-learning-architecture.md)
+ [Новый пользовательский интерфейс портала](how-to-track-experiments.md) для управления проводимыми экспериментами и целевыми объектами вычислений
+ Новый, более полный пакет <a href="http://aka.ms/aml-sdk" target="_blank">SDK</a> для Python
+ Новое, предоставляющее дополнительные возможности [расширение Azure интерфейса командной строки Azure](reference-azure-machine-learning-cli.md) для машинного обучения

[Архитектура](concept-azure-machine-learning-architecture.md) была переработана с учетом простоты использования. Вместо нескольких ресурсов Azure и учетных записей требуется только [рабочая область Машинного обучения Azure](concept-azure-machine-learning-architecture.md#workspace).  Новые рабочие области можно быстро создать на [портале Azure](quickstart-get-started.md).  Рабочая область может использоваться несколькими пользователями для хранения целевых объектов вычислений для обучения и развертывания, экспериментов с моделями, образов Docker, развернутых моделей и т. д.

Хотя в текущем выпуске используются новые клиенты с улучшенным интерфейсом командной строки и пакетом SDK, само классическое приложение Workbench для рабочего стола является устаревшим. Теперь можно отслеживать свои эксперименты в [на панели мониторинга рабочей области веб-портала Azure](how-to-track-experiments.md#view-the-experiment-in-the-azure-portal). Используйте панель мониторинга, чтобы просматривать историю эксперимента, управлять целевыми объектами вычислений, подключенными к рабочей области, управлять моделями и образами Docker и даже развертывать веб-службы.

## <a name="how-do-i-migrate"></a>Как выполнить миграцию

Большинство артефактов, созданных в предыдущей версии службы "Машинное обучение Azure", хранятся в вашем локальном или облачном хранилище. Эти артефакты не исчезнут. Для перехода нужно повторно зарегистрировать артефакты в обновленной службе "Машинное обучение Azure". Узнайте, что можно перенести и как это сделать, в этой [статье о переносе](how-to-migrate.md).

<a name="timeline"></a>

## <a name="support-timeline"></a>Сроки поддержки

Использование учетных записей для экспериментирования и управления моделями, а также приложения Workbench можно продолжить в течение некоторого времени после сентября 2018 г. Поддержка приведенных ниже ресурсов будет постепенно прекращена в течение 3–4 месяцев после выхода этого выпуска. Документацию по старым функциям можно найти в разделе [Ресурсы](../desktop-workbench/tutorial-classifying-iris-part-1.md) в нижней части содержания.

|Этап|Сведения о поддержке более ранних функций|
|:---:|----------------|
|1|Возможность создавать для службы "Машинное обучение Azure" _учетную запись экспериментирования_ и _учетную запись управления моделями_ на портале Azure и с помощью CLI. Возможность создавать вычислительные среды машинного обучения с помощью CLI также исчезает. При наличии учетной записи интерфейс командной строки и классическое приложение Workbench продолжат работать на этом этапе.|
|2|Прекращается поддержка используемых API для создания старых рабочих областей и проектов в классическом приложении Workbench и с помощью CLI. На этом этапе еще можно открывать существующие проекты и добавлять в них скрипты, выполнять скрипты в существующих проектах, а также развертывать веб-службы в существующих вычислительных средах машинного обучения.|
|3|Поддержка остальных ресурсов, включая оставшиеся API и классическое приложение Workbench, на этом этапе прекращается.|

[Начните переход](how-to-migrate.md) прямо сегодня. Все новейшие возможности доступны при использовании нового пакета <a href="http://aka.ms/aml-sdk" target="_blank">SDK</a>, [CLI](reference-azure-machine-learning-cli.md) и [портала](quickstart-get-started.md).

## <a name="what-about-run-histories"></a>Что будет с журналами выполнения?

Журналы выполнения останутся доступны некоторое время. Когда все будет готово к переходу на обновленную версию службы "Машинное обучение Azure", можно экспортировать эти журналы выполнения, если нужно сохранить копию.

Теперь журналы выполнения в текущем выпуске называются _экспериментами_. Можно собрать эксперименты для своей модели и исследовать их, используя пакет SDK, интерфейс командной строки или веб-портал.

Панель мониторинга для рабочей области портала поддерживается только в браузерах Edge, Chrome и Firefox.

[ ![Интернет-портал](./media/overview-what-happened-to-workbench/image001.png) ] (./media/overview-what-happened-to-workbench/image001.png#lightbox)


## <a name="can-i-still-prep-data"></a>Можно ли по-прежнему готовить данные?

Уже существующие файлы подготовки данных нельзя перенести в последнюю версию, так как приложение Workbench больше не будет использоваться. Но можно по-прежнему готовить данные для моделирования.  

В случае небольших наборов данных можно использовать <a href="http://aka.ms/aml-sdk" target="_blank">пакет SDK для подготовки данных службы "Машинное обучение Azure"</a>, чтобы быстро подготавливать данные перед моделированием. 

Этот же <a href="http://aka.ms/aml-sdk" target="_blank">пакет SDK</a> можно использовать для более крупных наборов данных. Так же для подготовки больших наборов данных можно применить Azure Databricks. 

## <a name="will-projects-persist"></a>Сохранятся ли проекты?

Никакой код и никакие наработки не будут потеряны. В более ранней версии проекты являются объектами облака с локальным каталогом. В последней версии локальные каталоги привязываются к рабочей области Машинного обучения Azure с помощью локального файла конфигурации. [См. схему последней архитектуры](concept-azure-machine-learning-architecture.md).

Так как большая часть содержимого проекта уже находится на локальном компьютере, для подключения к рабочей области необходимо просто создать файл конфигурации в этом каталоге и сослаться на него в своем коде. [Узнайте, как перенести существующие проекты.](how-to-migrate.md#projects)

Узнайте, как приступить к работе [в среде Python с помощью основного пакета SDK](quickstart-get-started.md).

## <a name="what-about-my-registers-models-and-images"></a>Что будет с уже занесенными в реестр моделями и образами?
 
Если нужно продолжить использование моделей, уже зарегистрированных в старом реестре моделей, их необходимо перенести в новую рабочую область. Для этого можно [загрузить модели и повторно зарегистрировать их](how-to-migrate.md) в новой рабочей области. 

Чтобы продолжить использование образов, созданных в старом реестре образов, их необходимо создать повторно в новой рабочей области. Описание соответствующих действий см. в разделе [Создание образа Docker](how-to-deploy-to-aci.md#configure-an-image). 

## <a name="what-about-deployed-web-services"></a>Что случится с развернутыми веб-службами?

Модели, развернутые как веб-службы с помощью учетной записи управления моделями, продолжат работать, пока не прекратится поддержка службы контейнеров Azure (ACS). Эти веб-службы будут работать и после окончания поддержки для учетных записей управления моделями. Но с окончанием поддержки старого интерфейса командной строки управление этими веб-службами станет невозможным.

В более новой версии модели развертываются как веб-службы в [экземпляры контейнеров Azure](how-to-deploy-to-aci.md) (ACI) или в кластеры [службы Azure Kubernetes](how-to-deploy-to-aks.md) (AKS). Можно также выполнить [развертывание в FPGA и IoT Edge](how-to-deploy-and-where.md). С помощью нового пакета SDK или интерфейса командной строки модели можно повторно развернуть без необходимости изменять какие-либо файлы оценки, зависимости и схемы. 

## <a name="what-about-the-old-sdk--cli"></a>Что будет со старым пакетом SDK и интерфейсом командной строки?

Да, они продолжат работать некоторое время (см. выше [сроки поддержки](#timeline)). Рекомендуется начать создание экспериментов и моделей с помощью последнего пакета SDK и (или) интерфейса командной строки.

В последнем выпуске новый пакет SDK для Python позволяет взаимодействовать со службой "Машинное обучение Azure" в любой среде Python. Узнайте, как установить последний пакет <a href="http://aka.ms/aml-sdk" target="_blank">SDK</a>.  Для взаимодействия со службой в любой среде командной строки, включая среду Cloud Shell портала Azure, можно также использовать [обновленное расширение машинного обучения интерфейса командной строки Azure](reference-azure-machine-learning-cli.md) с богатым набором команд `az ml`.

## <a name="what-about-vs-code-tools-for-ai"></a>Что произойдет с инструментами VS Code для ИИ?

В этом последнем выпуске расширение Visual Studio (VS) Code Tools for AI (инструменты кода Visual Studio для ИИ) были доработаны и улучшены, чтобы обеспечить работу с вышеописанными новыми функциями.

[ ![Visual Studio Code Tools for AI](./media/overview-what-happened-to-workbench/vscode.png) ] (./media/overview-what-happened-to-workbench/vscode-big.png#lightbox)

## <a name="what-about-domain-packages"></a>Что будет с пакетами домена?

Пакеты домена для [компьютерного зрения, анализа текста и прогнозирования](../desktop-workbench/reference-python-package-overview.md) нельзя использовать с последней версией машинного обучения Azure. Но по-прежнему можно создать и обучить модели компьютерного зрения, текста и прогнозирования, используя последний пакет <a href="http://aka.ms/aml-sdk" target="_blank">SDK</a> Python для машинного обучения Azure. Чтобы узнать, как перенести уже существующие модели, созданные с использованием пакетов компьютерного зрения, анализа текста и прогнозирования, обратитесь по адресу [AML-Packages@microsoft.com](mailto:AML-Packages@microsoft.com).

## <a name="next-steps"></a>Дополнительная информация

Воспользуйтесь одним из следующих кратких руководств или учебников, чтобы получить дополнительные сведения о [последней архитектуре для службы "Машинное обучение Azure"](concept-azure-machine-learning-architecture.md):

* [Что такое служба машинного обучения Azure](overview-what-is-azure-ml.md)
* [Краткое руководство. Создание рабочей области с помощью Python](quickstart-get-started.md)
* [Руководство. Обучение модели](tutorial-train-models-with-aml.md)
