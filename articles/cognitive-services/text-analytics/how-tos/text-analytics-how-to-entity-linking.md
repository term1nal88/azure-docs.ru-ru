---
title: Использование распознавания сущностей с помощью API анализа текста
titleSuffix: Azure Cognitive Services
description: Узнайте, как извлечь сущности, используя REST API анализа текста.
services: cognitive-services
author: ashmaka
manager: cgronlun
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: article
ms.date: 10/01/2018
ms.author: ashmaka
ms.openlocfilehash: b2916e5c414562c55c35c9c5e7ab378963e004be
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/03/2018
ms.locfileid: "48248080"
---
# <a name="how-to-use-named-entity-recognition-in-text-analytics-preview"></a>Как использовать распознавание именованных сущностей в API анализа текста (предварительная версия)

[API распознавания сущностей](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1-Preview/operations/5ac4251d5b4ccd1554da7634) принимает неструктурированный текст и для каждого документа JSON возвращает список неоднозначных сущностей со ссылками на дополнительную информацию в Интернете (Википедия и Bing). 

## <a name="entity-linking-and-named-entity-recognition"></a>Связывание сущностей и распознавание именованных сущностей

Конечная точка `entities` API анализа текста поддерживает и распознавание именованных сущностей (NER), и связывание сущностей.

### <a name="entity-linking"></a>Связывание сущностей
Связывание сущностей позволяет идентифицировать сущности, обнаруженные в тексте и устранить неоднозначности (например, упоминание слова "Марс" может обозначать планету или римского бога войны). Этот процесс требует наличия базы знаний, с которыми будут связываться распознанные сущности. Например, конечная точка `entities` Текстовой аналитики использует в качестве такой базы знаний сайт Википедии.

В API анализа текста [версии 2.1 (предварительная версия)](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1-Preview/operations/5ac4251d5b4ccd1554da7634) доступно только связывание сущностей.

### <a name="named-entity-recognition-ner"></a>Распознавание именованных сущностей (NER)
Распознавание именованных сущностей (NER) позволяет идентифицировать различные сущности в тексте и классифицировать их в предварительно определенные классы. Ниже перечислены поддерживаемые классы сущностей.

В API анализа текста версии 2.1 (предварительная версия) (`https://[region].api.cognitive.microsoft.com/text/analytics/v2.1-preview/entities`) доступны и связывание сущностей, и распознавание именованных сущностей (NER).

### <a name="language-support"></a>Поддержка языков

Связывание сущностей на разных языках требует наличия базы знаний для каждого из этих языков. В Текстовой аналитике это означает, что конечная точка `entities` для распознанных сущностей на каждом поддерживаемом языке предоставляет ссылку на соответствующий раздел Википедии на этом языке. Поскольку наполнение Википедии существенно различается для разных языков, функциональность связывания сущностей также будет иметь существенные различия.

## <a name="supported-types-for-named-entity-recognition"></a>Поддерживаемые типы распознанных именованных сущностей

| type  | SubType | Пример |
|:-----------   |:------------- |:---------|
| Person        | Недоступно\*         | "Джеф", "Билл Гейтс"     |
| Расположение      | Недоступно\*         | "Редмонд, штат Вашингтон", "Париж"  |
| План  | Недоступно\*         | "Майкрософт"   |
| Количество      | Number        | "6", "шесть"     | 
| Количество      | Процент    | "50 %", "пятьдесят процентов"| 
| Количество      | Порядковый номер       | "2-й", "второй"     | 
| Количество      | Диапазон чисел   | "4–8"     | 
| Количество      | Age           | "90 дней", "30 лет"    | 
| Количество      | Валюта      | "10,99 долл. США"     | 
| Количество      | Измерение     | "10 миль", "40 см"     | 
| Количество      | температура;   | "32 градуса"    |
| Datetime      | Недоступно\*         | "18:30 4 февраля 2012 г."      | 
| Datetime      | Дата          | "2 мая 2017 г.", "02.05.2017"   | 
| Дата и время     | Время          | "8 утра", "8:00"  | 
| Datetime      | Диапазон дат     | "с 2 по 5 мая"    | 
| Datetime      | Диапазон времени     | "6:00–17:00"     | 
| Datetime      | Duration      | "1 минута и 45 секунд"   | 
| Datetime      | Set           | "каждый вторник"     | 
| Datetime      | TimeZone      |    | 
| URL-адрес           | Недоступно\*         | "http://www.bing.com"    |
| Email         | Недоступно\*         | "support@contoso.com" |
\* Некоторые сущности могут опускать `SubType` в зависимости от входных и извлеченных сущностей.



## <a name="preparation"></a>Подготовка

Необходимо иметь документы JSON в следующем формате: "идентификатор","текст","язык"

Поддерживаемые языки перечислены в [этом списке](../text-analytics-supported-languages.md).

Документ должен содержать менее 5 000 символов, и иметь до 1 000 элементов (идентификаторов) в коллекции. Коллекция передается в тексте запроса. Ниже приведен пример иллюстрации содержимого, который можно отправлять для связывания сущностей.

```
{"documents": [{"id": "1",
                "language": "en",
                "text": "Jeff bought three dozen eggs because there was a 50% discount."
                },
               {"id": "2",
                "language": "en",
                "text": "The Great Depression began in 1929. By 1933, the GDP in America fell by 25%."
                }
               ]
}
```    
    
## <a name="step-1-structure-the-request"></a>Шаг 1: Структура запроса

Сведения об определении запроса можно найти в статье [How to call the Text Analytics REST API](text-analytics-how-to-call-api.md) (Способ вызова REST API анализа текста). Для удобства повторим следующие моменты.

+ Создайте запрос **POST**. Изучите документацию по API для этого запроса: [API связывания сущностей](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/5ac4251d5b4ccd1554da7634).

+ Создайте конечную точку HTTP для извлечения ключевых фраз. Она должна включать ресурс `/entities`: `https://westus.api.cognitive.microsoft.com/text/analytics/v2.1-preview/entities`

+ Задайте заголовок запроса, чтобы включить ключ доступа для операций Анализа текста. Дополнительные сведения см. в статье [о поиске конечных точек и ключей доступа](text-analytics-how-to-access-key.md).

+ В тексте запроса укажите набор документов JSON, которые подготовлены для этого анализа

> [!Tip]
> Используйте [Postman](text-analytics-how-to-call-api.md) или откройте **консоль тестирования API** в [документации](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1-Preview/operations/5ac4251d5b4ccd1554da7634), чтобы структурировать запрос и передать его (используя метод POST) в службу.

## <a name="step-2-post-the-request"></a>Шаг 2. Передача запроса

Анализ выполняется при получении запроса. Служба принимает до 100 запросов в минуту. Каждый запрос не должен превышать 1 МБ.

Не забывайте, что эта служба работает без отслеживания состояния. Данные не хранятся в учетной записи. Результаты возвращаются немедленно в ответе.

## <a name="step-3-view-results"></a>Шаг 3. Просмотр результатов

Все запросы POST вернут ответ в формате JSON с идентификаторами и обнаруженными свойствами.

Вывод возвращается немедленно. Можно выполнить потоковую передачу результатов в приложение, которое принимает JSON или сохранить выходные данные в файле локальной системы, а затем импортировать их в приложение, которое дает возможность сортировки, поиска и управления данными.

Далее показан пример выходных данных для связывания сущностей:

```json
{
    "Documents": [
        {
            "Id": "1",
            "Entities": [
                {
                    "Name": "Jeff",
                    "Matches": [
                        {
                            "Text": "Jeff",
                            "Offset": 0,
                            "Length": 4
                        }
                    ],
                    "Type": "Person"
                },
                {
                    "Name": "three dozen",
                    "Matches": [
                        {
                            "Text": "three dozen",
                            "Offset": 12,
                            "Length": 11
                        }
                    ],
                    "Type": "Quantity",
                    "SubType": "Number"
                },
                {
                    "Name": "50",
                    "Matches": [
                        {
                            "Text": "50",
                            "Offset": 49,
                            "Length": 2
                        }
                    ],
                    "Type": "Quantity",
                    "SubType": "Number"
                },
                {
                    "Name": "50%",
                    "Matches": [
                        {
                            "Text": "50%",
                            "Offset": 49,
                            "Length": 3
                        }
                    ],
                    "Type": "Quantity",
                    "SubType": "Percentage"
                }
            ]
        },
        {
            "Id": "2",
            "Entities": [
                {
                    "Name": "Great Depression",
                    "Matches": [
                        {
                            "Text": "The Great Depression",
                            "Offset": 0,
                            "Length": 20
                        }
                    ],
                    "WikipediaLanguage": "en",
                    "WikipediaId": "Great Depression",
                    "WikipediaUrl": "https://en.wikipedia.org/wiki/Great_Depression",
                    "BingId": "d9364681-98ad-1a66-f869-a3f1c8ae8ef8"
                },
                {
                    "Name": "1929",
                    "Matches": [
                        {
                            "Text": "1929",
                            "Offset": 30,
                            "Length": 4
                        }
                    ],
                    "Type": "DateTime",
                    "SubType": "DateRange"
                },
                {
                    "Name": "By 1933",
                    "Matches": [
                        {
                            "Text": "By 1933",
                            "Offset": 36,
                            "Length": 7
                        }
                    ],
                    "Type": "DateTime",
                    "SubType": "DateRange"
                },
                {
                    "Name": "Gross domestic product",
                    "Matches": [
                        {
                            "Text": "GDP",
                            "Offset": 49,
                            "Length": 3
                        }
                    ],
                    "WikipediaLanguage": "en",
                    "WikipediaId": "Gross domestic product",
                    "WikipediaUrl": "https://en.wikipedia.org/wiki/Gross_domestic_product",
                    "BingId": "c859ed84-c0dd-e18f-394a-530cae5468a2"
                },
                {
                    "Name": "United States",
                    "Matches": [
                        {
                            "Text": "America",
                            "Offset": 56,
                            "Length": 7
                        }
                    ],
                    "WikipediaLanguage": "en",
                    "WikipediaId": "United States",
                    "WikipediaUrl": "https://en.wikipedia.org/wiki/United_States",
                    "BingId": "5232ed96-85b1-2edb-12c6-63e6c597a1de",
                    "Type": "Location"
                },
                {
                    "Name": "25",
                    "Matches": [
                        {
                            "Text": "25",
                            "Offset": 72,
                            "Length": 2
                        }
                    ],
                    "Type": "Quantity",
                    "SubType": "Number"
                },
                {
                    "Name": "25%",
                    "Matches": [
                        {
                            "Text": "25%",
                            "Offset": 72,
                            "Length": 3
                        }
                    ],
                    "Type": "Quantity",
                    "SubType": "Percentage"
                }
            ]
        }
    ],
    "Errors": []
}
```


## <a name="summary"></a>Сводка

В этой статье рассматриваются основные понятия и рабочий процесс связывания сущностей в службе Текстовой аналитики Cognitive Services. В разделе "Сводка" сделайте следующее.

+ [Сущности API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1-Preview/operations/5ac4251d5b4ccd1554da7634) доступны для выбранных языков.
+ В тексте запроса передаются документы JSON, которые содержат идентификатор, текст и код языка.
+ Отправьте запрос POST на конечную точку `/entities`, используя правильные значения [ключа доступа и конечной точки](text-analytics-how-to-access-key.md) из вашей подписки.
+ Ответ будет содержать выходные данные, содержащие связанные сущности (включая оценки уверенности, смещения и веб-ссылки для каждого идентификатора документа), которые можно использовать в любом приложении

## <a name="see-also"></a>См. также 

 [Text Analytics overview](../overview.md) (Общие сведения об анализе текста)  
 [Часто задаваемые вопросы](../text-analytics-resource-faq.md)</br>
 [Анализ текста](//go.microsoft.com/fwlink/?LinkID=759712) 

## <a name="next-steps"></a>Дополнительная информация

> [!div class="nextstepaction"]
> [API анализа текста](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1-Preview/operations/5ac4251d5b4ccd1554da7634)
