---
title: Обработка порядка событий и просрочки в Azure Stream Analytics
description: В этой статье описывается, как Stream Analytics обрабатывает неупорядоченные или поздние события в потоках данных.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/20/2017
ms.openlocfilehash: f0ee486d9ff4c05269da23866edad281aa627889
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/29/2018
ms.locfileid: "37113900"
---
# <a name="azure-stream-analytics-event-order-considerations"></a>Рассмотрение порядка событий Azure Stream Analytics

## <a name="arrival-time-and-application-time"></a>Время получения и время приложения

В потоке временных данных событий каждому событию присваивается метка времени. Azure Stream Analytics присваивает каждому событию метку времени, используя время получения или время приложения. В столбце **System.Timestamp** указана метка времени, присвоенная событию. 

Время получения присваивается в источнике входных данных, когда событие достигает источника. Узнать время получения можно с помощью свойства **EventEnqueuedUtcTime** для входных данных концентраторов событий, свойства **IoTHub.EnqueuedTime** для Центра Интернета вещей и свойства [BlobProperties.LastModified](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.blob.blobproperties.lastmodified?view=azurestorage-8.1.3) для входных данных большого двоичного объекта. 

Время приложения присваивается при создании события и является частью полезных данных. Для обработки событий по времени приложения используйте предложение **Timestamp by** в запросе SELECT. Если предложение **Timestamp by** отсутствует, то события обрабатываются по времени получения. 

Azure Stream Analytics создает выходные данные в порядке меток времени и предоставляет параметры для работы с неупорядоченными данными.


## <a name="handling-of-multiple-streams"></a>Обработка нескольких потоков

Задание Azure Stream Analytics объединяет события из нескольких временных шкал в нескольких случаях, в том числе:

* Создание выходных данных из нескольких разделов. Запросы без явного предложения **разделения по PartitionId** должны были бы объединять события из всех разделов.
* Объединение двух или более различных источников входных данных.
* Присоединение источников входных данных.

В сценариях, где объединены несколько временных шкал, Azure Stream Analytics будет выдавать выходные данные для метки времени *t1* только после того, как все объединенные источники достигнут времени *t1*. Например, предположим, что запрос выполняет чтение из секции концентратора событий с двумя разделами. В одном из разделов, *P1*, есть события до времени *t1*. В одном из разделов, *P2*, есть события до времени *t1 + x*. В таком случае выходные данные выводятся до времени *t1*. Однако, если явно указано предложение **разделения по PartitionId**, оба раздела работают независимо друг от друга.

Параметр для установки допустимого интервала поступления с задержкой используется для устранения проблем при отсутствии данных в некоторых разделах.

## <a name="configuring-late-arrival-tolerance-and-out-of-order-tolerance"></a>Настройка допустимого интервала поступления с задержкой и интервала для событий, полученных в неправильном порядке
С неупорядоченными входными потоками происходит одно из следующих событий:
* сортировка (и, следовательно, удаление);
* корректировка системой с учетом пользовательской политики.

При обработке по времени приложения Stream Analytics допускает неупорядоченные события и события, поступившие с задержкой. На портале Azure в разделе **Упорядочение событий** доступны следующие параметры: 

![Обработка событий Stream Analytics](media/stream-analytics-event-handling/stream-analytics-event-handling.png)

### <a name="late-arrival-tolerance"></a>Допустимый интервал поступления с задержкой
Параметр "Допустимый интервал поступления с задержкой" применяется только при обработке по времени приложения. В противном случае параметр игнорируется.

"Допустимый интервал поступления с задержкой" — это максимальная разница между временем получения и временем приложения. Если событие поступает позже допустимого интервала поступления с задержкой (например, время приложения *app_t* < время прибытия *arr_t* - политика допустимого интервала поступления с задержкой *late_t*), событие корректируется на основе максимально допустимого интервала поступления с задержкой (*arr_t* - *late_t*). Интервал поступления с задержкой — это максимальная задержка между созданием события и его получением в источнике входных данных. 

Если одновременно используется несколько секций из одного входного потока или нескольких входных потоков, то допустимый интервал поступления с задержкой является максимальным временем, которое каждая секция ожидает поступления новых данных. 

Сначала выполняется корректировка на основе допустимого интервала поступления с задержкой, а затем корректируются события, полученные в неправильном порядке. В столбце **System.Timestamp** указана конечная метка времени, присвоенная событию.

### <a name="out-of-order-tolerance"></a>Интервал для событий, полученных в неправильном порядке
Последовательность событий, полученных в неправильном порядке, но в пределах заданного "интервала для событий, полученных в неправильном порядке", корректируется по метке времени. События, поступающие позже допустимого интервала:
* **Корректировка** предполагает изменение значения времени к крайнему допустимому. 
* **Удаление** предполагает удаление.

Когда Stream Analytics изменяет последовательность событий, полученных в пределах "интервала для событий, полученных в неправильном порядке", выходные данные запроса задерживаются на такой же интервал.

### <a name="early-events"></a>Ранние события
При обработке по времени приложения события, время приложения для которых превышает 5 минут с момента поступления, удаляются или корректируются в соответствии с выбранным параметром конфигурации.

### <a name="example"></a>Пример

* Допустимый интервал поступления с задержкой = 10 минут.<br/>
* Интервал для событий, полученных в неправильном порядке = 3 минуты<br/>
* Обработка по времени приложения<br/>
* События:
   1. **Время приложения** = 00:00:00, **время получения** = 00:10:01, **System.Timestamp** = 00:00:01, корректируется, так как значение (**время прибытия - время приложения**) больше, чем допустимый интервал поступления с задержкой.
   2. **Время приложения** = 00:10:01, **время получения** = 00:00:01, **System.Timestamp** = 00:00:01, не корректируется, так как время приложения входит в интервал поступления с задержкой.
   3. **Время приложения** = 00:10:00, **время получения** = 00:10:02, **System.Timestamp** = 00:10:00, не корректируется, так как время приложения входит в интервал поступления с задержкой.
   4. **Время приложения** = 00:09:00, **время получения** = 00:10:03, **System.Timestamp** = 00:09:00, принимается с исходной меткой времени, так как время приложения входит в допустимый интервал для событий, полученных в неправильном порядке.
   5. **Время приложения** = 00:06:00, **время получения** = 00:10:04, **System.Timestamp** = 00:07:00, корректируется, так как время приложения раньше интервала для событий, полученных в неправильном порядке.

### <a name="practical-considerations"></a>Практические рекомендации
Как упоминалось ранее, "Допустимый интервал поступления с задержкой" — это максимальная разница между временем приложения и временем получения. События, которые прибыли позже, чем настроенный допустимый интервал поступления с задержкой, при обработке по времени приложения корректируются до применения параметра интервала для событий, полученных в неправильном порядке. Таким образом эффективный интервал для событий, полученных в неправильном порядке, — это минимальная разница между допустимым интервалом поступления с задержкой и интервалом для событий, полученных в неправильном порядке.

Причины поступления неупорядоченных событий в потоке включают следующие:
* разница в показаниях часов между отправителями;
* переменная задержка между отправителем и источником входных событий.

Причины прибытия с задержкой могут быть следующими:
* на протяжении интервала отправляется пакет событий;
* задержка между отправкой события и получением события на источнике входных данных.

При настройке допустимого интервала поступления с задержкой и интервала для событий, полученных в неправильном порядке, для конкретного задания следует учитывать правильность, требования к задержке и вышеперечисленные факторы.

Ниже приводятся несколько примеров.

#### <a name="example-1"></a>Пример 1 
В запросе есть предложение **разделения по PartitionId**. В пределах одного раздела события отправляются с помощью синхронных методов отправки. Синхронные методы отправки блокируются до тех пор, пока события не будут отправлены.

В этом случае неупорядоченность равна нулю, так как события отправляются в порядке и с явным подтверждением перед отправкой следующего события. Позднее прибытие — это максимальная задержка между генерированием события и отправкой события плюс максимальная задержка между отправителем и источником входных данных.

#### <a name="example-2"></a>Пример 2
В запросе есть предложение **разделения по PartitionId**. В пределах одного раздела события отправляются с помощью синхронных методов отправки. Методы асинхронной отправки могут инициировать несколько отправлений одновременно, что может привести к неупорядоченным событиям.

В этом случае интервал для событий, полученных в неправильном порядке, и опоздание — это максимальная задержка между генерированием события и отправкой события, а также максимальная задержка между отправителем и источником входных данных.

#### <a name="example-3"></a>Пример 3
В запросе нет предложения **разделения по PartitionId** и существует по крайней мере два раздела.

Конфигурация та же, что и в примере 2. Однако отсутствие данных в одном из разделов может задержать выходные данные с учетом значения дополнительного допустимого интервала поступления с задержкой.

## <a name="handling-event-producers-with-differing-timelines-with-substreams"></a>Обработка поставщиков событий с различными временными шкалами с использованием "подпотоков"
В одном потоке входных событий часто содержатся события, поступающие и от нескольких поставщиков событий, и с отдельных устройств. Эти события могут поступать в неправильном порядке из-за причин, рассмотренных ранее. Несмотря на то, что степень неупорядоченности между поставщиками событий может быть значительной, неупорядоченность между событиями одного поставщика незначительна (или вовсе отсутствует).

Azure Stream Analytics предоставляет общие механизмы обработки неупорядоченных событий. Такие механизмы могут вызвать задержки обработки (из-за ожидания, пока неупорядоченные события достигнут системы), удалить или корректировать события.

Однако во многих сценариях требуемый запрос обрабатывает события от разных поставщиков событий отдельно. Например, он может агрегировать события для каждого окна и для каждого устройства. В этих случаях не нужно задерживать выходные данные, которые соответствуют одному поставщику событий, ожидая появления других поставщиков событий. Другими словами нет необходимости заботиться о разнице во времени между поставщиками. Его можно пропустить.

Это означает, что сами события вывода данных не упорядочены по отношению к своим меткам времени. Подчиненный объект-получатель должен уметь работать с таким поведением. Но каждое событие в выходных данных указано правильно.

Azure Stream Analytics реализует эту функцию с помощью предложения [TIMESTAMP BY OVER](https://msdn.microsoft.com/library/azure/mt573293.aspx).

## <a name="summary"></a>Сводка
* Настройте допустимый интервал поступления с задержкой и интервал поступления неупорядоченных событий на основе требований к правильности и задержке. Также рассмотрите способы отправки событий.
* Мы рекомендуем настроить интервал для событий, полученных в неправильном порядке, меньше, чем допустимый интервал поступления с задержкой.
* При объединении нескольких временных шкал отсутствие данных в одном из источников или разделов может задержать выходные данные с учетом значения дополнительного допустимого интервала поступления с задержкой.

## <a name="get-help"></a>Получение справки
Дополнительную помощь вы можете получить на нашем [форуме Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Дополнительная информация
* [Что такое Stream Analytics?](stream-analytics-introduction.md)
* [Приступая к работе с Azure Stream Analytics: выявление мошенничества в режиме реального времени](stream-analytics-real-time-fraud-detection.md)
* [Масштабирование заданий Azure Stream Analytics для повышения пропускной способности базы данных](stream-analytics-scale-jobs.md)
* [Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Справочник по языку запросов Stream Analytics)
* [Stream Analytics management REST API reference](https://msdn.microsoft.com/library/azure/dn835031.aspx) (Справочник по API-интерфейсу REST для управления Stream Analytics)
