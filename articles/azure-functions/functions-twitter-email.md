---
title: Создание функции, интегрируемой с Azure Logic Apps | Документация Майкрософт
description: Создайте функцию, которая интегрируется с Azure Logic Apps и Azure Cognitive Services для классификации мнений в твитах и отправки уведомлений, если мнение недопустимо.
services: functions, logic-apps, cognitive-services
keywords: рабочий процесс, облачные приложения, облачные службы, бизнес-процессы, системная интеграция, интеграция приложений, EAI
author: ggailey777
manager: jeconnoc
ms.assetid: 60495cc5-1638-4bf0-8174-52786d227734
ms.service: azure-functions
ms.topic: tutorial
ms.date: 09/24/2018
ms.author: glenga
ms.custom: mvc, cc996988-fb4f-47
ms.openlocfilehash: 0b2e0ff800ab80a2c638293ce23fc1911390f2dd
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/26/2018
ms.locfileid: "47221123"
---
# <a name="create-a-function-that-integrates-with-azure-logic-apps"></a>Создание функции, интегрируемой с Azure Logic Apps

Функции Azure интегрируются с Azure Logic Apps в конструкторе Logic Apps. Такая интеграция позволяет использовать вычислительную мощность Функций при оркестрации с другими службами Azure и сторонними службами. 

В этом руководстве показано, как анализировать мнения, выраженные в записях в Twitter, с помощью Функций, Logic Apps и Microsoft Cognitive Services в Azure. Функция, активируемая HTTP, классифицирует твиты на категории green, yellow или red на основе оценки мнения. При обнаружении недопустимого мнения отправляется электронное сообщение. 

![Изображение первых двух действий приложения в конструкторе приложений логики](media/functions-twitter-email/designer1.png)

Из этого руководства вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
> * Создание ресурса API Cognitive Services.
> * Создание функции, классифицирующей мнения, выраженные в твитах.
> * Создание приложения логики, подключающегося к Twitter.
> * Добавление обнаружения мнений в приложение логики. 
> * Подключение приложения логики к функции.
> * Отправка электронного сообщения в зависимости от ответа из функции.

## <a name="prerequisites"></a>Предварительные требования

+ Активная учетная запись [Twitter](https://twitter.com/). 
+ Учетная запись [Outlook.com](https://outlook.com/) (для отправки уведомлений).
+ В этой статье в качестве отправной точки используются ресурсы, созданные при работе со статьей [Создание первой функции на портале Azure](functions-create-first-azure-function.md).  
Выполните шаги в этой статье для создания приложения-функции, если вы еще не сделали этого.

## <a name="create-a-cognitive-services-resource"></a>Создание ресурса Cognitive Services

API-интерфейсы Cognitive Services доступны как отдельные ресурсы в Azure. Используйте API анализа текста для определения тональности отслеживаемых твитов.

1. Войдите на [портале Azure](https://portal.azure.com/).

2. Щелкните **Создать ресурс** в верхнем левом углу окна портала Azure.

3. Нажмите **AI + Analytics** (Искусственный интеллект и аналитика) > **API анализа текста**. Затем используйте параметры, указанные в таблице, примите условия и установите флажок **Закрепить на панели мониторинга**.

    ![Создание страницы ресурса Cognitive Services](media/functions-twitter-email/cog_svcs_resource.png)

    | Параметр      |  Рекомендуемое значение   | Описание                                        |
    | --- | --- | --- |
    | **Имя** | MyCognitiveServicesAccnt | Выберите уникальное имя учетной записи. |
    | **Местоположение.** | Запад США | Используйте ближайшее к вам местоположение. |
    | **Ценовая категория** | F0 | Всегда начинайте с самого нижнего уровня. Если вы превысили количество вызовов, перейдите на более высокий уровень.|
    | **Группа ресурсов** | myResourceGroup | Используйте одну группу ресурсов для всех служб в этом руководстве.|

4. Нажмите кнопку **Создать**, чтобы создать ресурс. После его создания выберите новый ресурс Cognitive Services, закрепленный на панели мониторинга. 

5. В столбце слева щелкните **Ключи** и скопируйте значение **ключа 1**, а затем сохраните его. Этот ключ используется для подключения приложения логики к API Cognitive Services. 
 
    ![ключей](media/functions-twitter-email/keys.png)

## <a name="create-the-function-app"></a>Создание приложения-функции

Функции предоставляют отличный способ разгрузки задач обработки в рабочем процессе приложений логики. В этом руководстве для обработки оценок мнений, выраженных в твитах, из Cognitive Services и возвращения значения категории используется функция, активируемая HTTP.  

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

## <a name="create-an-http-triggered-function"></a>Создание функции, активируемой HTTP  

1. Разверните приложение-функцию и нажмите кнопку **+** рядом с элементом **Функции**. Если это первая функция в приложении-функции, выберите **Пользовательская функция**. Откроется полный набор шаблонов функций.

    ![Страница быстрого начала работы с функциями на портале Azure](media/functions-twitter-email/add-first-function.png)

2. В поле поиска введите `http` и выберите **C#** для шаблона триггера HTTP. 

    ![Выбор триггера HTTP](./media/functions-twitter-email/select-http-trigger-portal.png)

    Все последующие функции, добавленные в приложение-функцию используют шаблоны языка C#.

3. Введите **имя** функции, выберите `Function` в качестве **[уровня проверки подлинности](functions-bindings-http-webhook.md#http-auth)**, а затем нажмите кнопку **Создать**. 

    ![Создание функции, активируемой HTTP](./media/functions-twitter-email/select-http-trigger-portal-2.png)

    В результате будет создана функция скрипта C# с использованием шаблона триггера HTTP. Код появится в виде файла `run.csx` в новом окне.

4. Замените содержимое файла `run.csx` следующим кодом и нажмите кнопку **Сохранить**:

    ```csharp
    #r "Newtonsoft.Json"
    
    using System;
    using System.Net;
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.Extensions.Primitives;
    using Newtonsoft.Json;
    
    public static async Task<IActionResult> Run(HttpRequest req, ILogger log)
    {
        string category = "GREEN";
    
        string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
        log.LogInformation(string.Format("The sentiment score received is '{0}'.", requestBody));
    
        double score = Convert.ToDouble(requestBody);
    
        if(score < .3)
        {
            category = "RED";
        }
        else if (score < .6) 
        {
            category = "YELLOW";
        }
    
        return requestBody != null
            ? (ActionResult)new OkObjectResult(category)
            : new BadRequestObjectResult("Please pass a value on the query string or in the request body");
    }
    ```
    Этот код функции возвращает цветовую категорию на основе оценки мнений, полученной в запросе. 

4. Чтобы проверить функцию, щелкните **Тест** в правой области сверху. После этого откроется вкладка "Тест". В разделе **Текст запроса** введите значение `0.2`, а затем нажмите кнопку **Запуск**. В тексте ответа возвратится значение **RED**. 

    ![Проверка функции на портале Azure](./media/functions-twitter-email/test.png)

Теперь у вас есть функция, классифицирующая оценку мнений. Создайте приложение логики, интегрирующее функцию с API Twitter и Cognitive Services. 

## <a name="create-a-logic-app"></a>Создайте приложение логики   

1. На портале Azure щелкните **Создать** в верхнем левом углу.

2. Щелкните **Интеграция Enterprise** > **Приложение логики**. Затем используйте параметры, указанные в таблице, установите флажок **Закрепить на панели мониторинга** и нажмите кнопку **Создать**.
 
4. Затем введите **имя**, например `TweetSentiment`, используйте параметры, указанные в таблице, примите условия и установите флажок **Закрепить на панели мониторинга**.

    ![Создание приложения логики на портале Azure](./media/functions-twitter-email/new_logic_app.png)

    | Параметр      |  Рекомендуемое значение   | Описание                                        |
    | ----------------- | ------------ | ------------- |
    | **Имя** | TweetSentiment | Выберите соответствующее имя для своего приложения. |
    | **Группа ресурсов** | myResourceGroup | Выберите ту же имеющуюся группу ресурсов, что и раньше. |
    | **Местоположение.** | Восточная часть США | Выберите расположение рядом с вами. |    

4. Выберите **Закрепить на панели мониторинга** и нажмите кнопку **Создать**, чтобы создать приложение логики. 

5. После создания приложения щелкните новое приложение логики, прикрепленное к панели мониторинга. Затем в конструкторе Logic Apps прокрутите вниз и выберите шаблон **Пустое приложение логики**. 

    ![Шаблон "Пустое приложение логики" Logic Apps](media/functions-twitter-email/blank.png)

Теперь вы можете добавлять в приложение службы и триггеры с помощью конструктора Logic Apps.

## <a name="connect-to-twitter"></a>Подключение к Twitter

Сначала подключитесь к своей учетной записи Twitter. Приложение логики выполняет опрос твитов, которые активируют запуск приложения.

1. В конструкторе щелкните службу **Twitter** и щелкните триггер **Время публикации нового твита**. Войдите в свою учетную запись Twitter и авторизуйте Logic Apps для ее использования.

2. Используйте параметры триггера Twitter, указанные в таблице. 

    ![Параметры соединителя Twitter](media/functions-twitter-email/azure_tweet.png)

    | Параметр      |  Рекомендуемое значение   | ОПИСАНИЕ                                        |
    | ----------------- | ------------ | ------------- |
    | **Текст поискового запроса** | #Azure | Используйте популярный хэш-тег, который приведет к созданию новых твитов за выбранный интервал. Если при использовании уровня "Бесплатный" и выбранного хэш-тега создается слишком много твитов, вы можете быстро использовать квоту транзакции в API Cognitive Services. |
    | **Frequency** | Минута | Единица, используемая для определения частоты опроса Twitter.  |
    | **Интервал** | 15 | Время между запросами Twitter в единицах, определяющих частоту. |

3.  Щелкните **Сохранить**, чтобы подключиться к своей учетной записи Twitter. 

Теперь приложение подключено к Twitter. Далее вы подключитесь к анализу текста для обнаружения мнений в собранных твитах.

## <a name="add-sentiment-detection"></a>Добавление определения мнений

1. Щелкните **New Step** (Создать шаг), а затем — **Добавить действие**.

    ![Выбор действий "Новый шаг" и "Добавить действие"](media/functions-twitter-email/new_step.png)

2. На странице **выбора действия** щелкните **Текстовая аналитика** и выберите действие **Detect sentiment** (Обнаружить мнение).

    ![Определение мнений](media/functions-twitter-email/detect_sent.png)

3. Введите имя подключения, например `MyCognitiveServicesConnection`, вставьте ключ для сохраненного API Cognitive Services и нажмите кнопку **Создать**.  

4. Щелкните **Text to analyze** (Текст для анализа) > **Текст твита**, а затем нажмите кнопку **Сохранить**.  

    ![Определение мнений](media/functions-twitter-email/ds_tta.png)

Теперь, когда настроено выявление мнений, можно добавить подключение к функции, которая использует результат оценки мнений.

## <a name="connect-sentiment-output-to-your-function"></a>Подключение выходных данных мнений к функции

1. В конструкторе Logic Apps щелкните **New step** (Создать шаг) > **Добавить действие**, а затем выберите **Функции Azure**. 

2. Щелкните **Выберите функцию Azure** и выберите функцию **CategorizeSentiment**, созданную ранее.  

    ![Окно с функцией Azure и параметром "Выберите функцию Azure"](media/functions-twitter-email/choose_fun.png)

3. В разделе **Текст запроса** щелкните кнопку **Score** (Оценка) и **Сохранить**.

    ![Оценка](media/functions-twitter-email/trigger_score.png)

Теперь функция активируется при отправке оценки мнений из приложения логики. Функция возвращает категорию с цветовой кодировкой в приложение логики. Далее следует добавить уведомление по электронной почте, отправляемое при возвращении мнения со значением **RED** из функции. 

## <a name="add-email-notifications"></a>Добавление уведомлений по почте

Последняя часть рабочего процесса заключается в активации электронного сообщения, если мнению присвоена оценка _RED_. В этом разделе использует соединитель Outlook.com. Вы можете выполнить аналогичные шаги для использования соединителя Gmail или Office 365 Outlook.   

1. В конструкторе Logic Apps щелкните **New step** (Создать шаг) > **Добавить условие**. 

2. Щелкните **Выберите значение**, а затем — **Текст**. Выберите **равняется**, щелкните **Выберите значение**, введите `RED` и нажмите кнопку **Сохранить**. 

    ![Добавьте условие к приложению логики.](media/functions-twitter-email/condition.png)

3. В разделе **Если истинно** нажмите кнопку **Add an action** (Добавить действие), найдите `outlook.com`, щелкните **Send an email** (Отправить электронное письмо) и выполните вход в учетную запись Outlook.com.
    
    ![Выбор действия для условия.](media/functions-twitter-email/outlook.png)

    > [!NOTE]
    > Если у вас нет учетной записи Outlook.com, можно выбрать другой соединитель, например Gmail или Office 365 Outlook

4. В разделе с действием **Отправить электронное письмо** используйте параметры электронной почты, указанные в таблице. 

    ![Настройка электронной почты для действия "Отправить электронное письмо".](media/functions-twitter-email/send_email.png)

    | Параметр      |  Рекомендуемое значение   | ОПИСАНИЕ  |
    | ----------------- | ------------ | ------------- |
    | **To** | Введите адрес электронной почты | Адрес электронной почты, на который приходит уведомление. |
    | **Тема** | Negative tweet sentiment detected (Обнаружено отрицательное мнение, выраженное в твите)  | Строка темы уведомления по почте.  |
    | **Текст** | "Текст твита", "Расположение" | Щелкните параметры **Текст твита** и **Расположение**. |

5.  Выберите команду **Сохранить**.

Теперь, когда рабочий процесс завершен, вы можете включить приложение логики и увидеть функцию в действии.

## <a name="test-the-workflow"></a>Проверка рабочего процесса

1. В конструкторе Logic Apps щелкните **Запуск**, чтобы запустить приложение.

2. В левом столбце щелкните **Обзор**, чтобы просмотреть сведения о состоянии приложения логики. 
 
    ![Состояние выполнения приложения логики](media/functions-twitter-email/over1.png)

3. (Необязательно.) Щелкните одну из запущенных функций, чтобы просмотреть сведения о выполнении.

4. Перейдите к функции, просмотрите журналы и убедитесь, что значения мнений получены и обработаны.
 
    ![Просмотр журналов функции](media/functions-twitter-email/sent.png)

5. При обнаружении потенциально негативного мнения вы получите электронное сообщение. Если вы еще не получили его, вы можете изменить код функции таким образом, чтобы каждый раз возвращалась категория RED:

        return req.CreateResponse(HttpStatusCode.OK, "RED");

    Проверив уведомления по почте, вернитесь к исходному коду:

        return req.CreateResponse(HttpStatusCode.OK, category);

    > [!IMPORTANT]
    > Завершив работу с этим руководством, вам следует отключить приложение логики. Так вы не будете платить за выполнение и не используете все транзакции в API Cognitive Services.

Вы узнали, насколько легко можно интегрировать Функции в рабочий процесс Logic Apps.

## <a name="disable-the-logic-app"></a>Отключение приложения логики

Чтобы отключить приложение логики, щелкните **Обзор** и **Отключить** в верхней части экрана. Приложение логики будет отключено, а вы не будете платить за его использование. При этом вам не нужно удалять приложение. 

![Журналы функций](media/functions-twitter-email/disable-logic-app.png)

## <a name="next-steps"></a>Дополнительная информация

Из этого руководства вы узнали, как выполнить следующие задачи:

> [!div class="checklist"]
> * Создание ресурса API Cognitive Services.
> * Создание функции, классифицирующей мнения, выраженные в твитах.
> * Создание приложения логики, подключающегося к Twitter.
> * Добавление обнаружения мнений в приложение логики. 
> * Подключение приложения логики к функции.
> * Отправка электронного сообщения в зависимости от ответа из функции.

Перейдите к следующему руководству, чтобы научиться создавать бессерверные API для функций.

> [!div class="nextstepaction"] 
> [Создание бессерверного API с помощью Функций Azure](functions-create-serverless-api.md)

Дополнительные сведения об Azure Logic Apps см. в [этой статье](../logic-apps/logic-apps-overview.md).

