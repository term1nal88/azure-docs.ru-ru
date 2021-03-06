---
title: Подготовка пропускной способности для Azure Cosmos DB | Документация Майкрософт
description: Узнайте, как задать подготовленную пропускную способность для контейнеров, коллекций, графов и таблиц Azure Cosmos DB.
services: cosmos-db
author: aliuy
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 10/02/2018
ms.author: andrl
ms.openlocfilehash: 2280a3f6b2a67d392a109a5294e1509bcc804bc3
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/08/2018
ms.locfileid: "48869930"
---
# <a name="set-and-get-throughput-for-azure-cosmos-db-containers-and-database"></a>Настройка и получение пропускной способности контейнеров и базы данных Azure Cosmos DB

Пропускную способность для контейнера или наборов контейнеров Azure Cosmos DB можно настроить на портале Azure или с помощью клиентских пакетов SDK. В этой статье описываются действия, необходимые для настройки пропускной способности на различных уровнях детализации для учетной записи Azure Cosmos DB.

## <a name="provision-throughput-by-using-azure-portal"></a>Подготовка пропускной способности с помощью портала Azure

### <a name="provision-throughput-for-a-container-collectiongraphtable"></a>Подготовка пропускной способности для контейнера (коллекции, графа или таблицы)

1. Войдите на [портале Azure](https://portal.azure.com).  
2. На панели навигации слева выберите **All resources** (Все ресурсы) и найдите учетную запись Azure Cosmos DB.  
3. Вы можете настроить пропускную способность во время создания контейнера (коллекции, графа или таблицы) или же обновить пропускную способность для имеющегося контейнера.  
4. Чтобы назначить пропускную способность при создании контейнера, откройте колонку **Обозреватель данных** и выберите **Новая коллекция** (New Graph (Создать граф), "Создать таблицу" для других API).  
5. Заполните форму в колонке **Добавить коллекцию**. Поля в этой колонке описаны в следующей таблице.  

   |**Параметр**  |**Описание**  |
   |---------|---------|
   |Идентификатор базы данных  |  Укажите уникальное имя для идентификации базы данных. База данных — логический контейнер для одной коллекции или нескольких. Имена баз данных должны быть длиной от 1 до 255 символов и не могут содержать символы /, \\, #, ? или пробел. |
   |Идентификатор коллекции  | Укажите уникальное имя для идентификации коллекции. Для идентификаторов коллекций предусмотрены те же требования к символам, что и для имен баз данных. |
   |Емкость хранилища   | Это значение представляет емкость хранилища базы данных. При подготовке пропускной способности для отдельной коллекции емкость хранилища может быть **фиксированной (10 ГБ)** или **неограниченной**. Неограниченная емкость хранилища требует установки ключа секции для ваших данных.  |
   |Пропускная способность   | Для каждой коллекции и базы данных можно настроить пропускную способность в единицах запросов в секунду.  И коллекция может обладать фиксированной или неограниченной емкостью хранилища. |

6. Указав значения в этих полях, нажмите кнопку **ОК**, чтобы сохранить параметры.  

   ![Настройка пропускной способности для коллекции](./media/set-throughput/set-throughput-for-container.png)

7. Чтобы обновить пропускную способность для имеющегося контейнера, разверните базу данных и контейнер, а затем щелкните **Параметры**. В открывшемся окне введите значение новой пропускной способности, а затем выберите **Сохранить**.  

   ![Обновление пропускной способности для коллекции](./media/set-throughput/update-throughput-for-container.png)

### <a name="provision-throughput-for-a-set-of-containers-or-at-the-database-level"></a>Подготовка пропускной способности для набора контейнеров и на уровне базы данных

1. Войдите на [портале Azure](https://portal.azure.com).  
2. На панели навигации слева выберите **All resources** (Все ресурсы) и найдите учетную запись Azure Cosmos DB.  
3. Вы можете настроить пропускную способность во время создания базы данных или же обновить ее для имеющейся базы данных.  
4. Чтобы назначить пропускную способность при создании базы данных, откройте **обозреватель данных** и выберите **Новая база данных**  
5. Укажите значение **идентификатора базы данных**, проверьте параметр **Provision throughput** (Подготовка пропускной способности) и настройте значение пропускной способности.  

   ![Настройка пропускной способности с помощью нового параметра базы данных](./media/set-throughput/set-throughput-with-new-database-option.png)

6. Чтобы обновить пропускную способность для имеющейся базы данных, разверните базу данных и контейнер, а затем щелкните **Масштаб**. В открывшемся окне введите значение новой пропускной способности, а затем выберите **Сохранить**.  

   ![Обновление пропускной способности для базы данных](./media/set-throughput/update-throughput-for-database.png)

### <a name="provision-throughput-for-a-set-of-containers-as-well-as-for-an-individual-container-in-a-database"></a>Подготовка пропускной способности для набора контейнеров, а также отдельного контейнера в базе данных

1. Войдите на [портале Azure](https://portal.azure.com).  
2. На панели навигации слева выберите **All resources** (Все ресурсы) и найдите учетную запись Azure Cosmos DB.  
3. Создайте базу данных и назначьте ее пропускную способность. Откройте колонку **Обозреватель данных** и выберите **Новая база данных**  
4. Укажите значение **идентификатора базы данных**, проверьте параметр **Provision throughput** (Подготовка пропускной способности) и настройте значение пропускной способности.  

   ![Настройка пропускной способности с помощью нового параметра базы данных](./media/set-throughput/set-throughput-with-new-database-option.png)

5. Далее создайте коллекцию в созданной ранее базе данных. Щелкните базу данных правой кнопкой мыши и выберите **Новая коллекция**.  

6. В колонке **Добавить коллекцию** введите имя коллекции и ключ секции. При необходимости можно подготовить пропускную способность для конкретного контейнера. Если вы решили не назначать значение пропускной способности, коллекция будет использовать пропускную способность, назначенную базе данных.  

   ![Настройка пропускной способности для контейнера](./media/set-throughput/optionally-set-throughput-for-the-container.png)

## <a name="considerations-when-provisioning-throughput"></a>Рекомендации при подготовке пропускной способности

Ниже приведены некоторые рекомендации, помогающие выбрать стратегию резервирования пропускной способности.

### <a name="considerations-when-provisioning-throughput-at-the-database-level"></a>Рекомендации по подготовке пропускной способности на уровне базы данных

Подготовку пропускной способности на уровне базы данных (то есть для набора контейнеров) можно выполнять в следующих случаях:

* Если у вас около десяти или больше контейнеров, среди которых все или несколько из них могут совместно использовать пропускную способность.  

* При переходе от однотенантной базы данных, предназначенной для использования на виртуальных машинах, размещенных в IaaS или локально (например, NoSQL или реляционные базы данных), к базе данных Azure Cosmos с большим количеством контейнеров.  

* Появление внеплановых пиков в рабочих нагрузках при использовании общей пропускной способности на уровне базы данных.  

* Вместо настройки пропускной способности для отдельного контейнера вам нужно настроить общую пропускную способность в наборе контейнера внутри базы данных.

### <a name="considerations-when-provisioning-throughput-at-the-container-level"></a>Рекомендации по подготовке пропускной способности на уровне контейнера

Возможность подготовки пропускной способности для отдельного контейнера следует рассмотреть в следующих случаях:

* Если у вас мало контейнеров Azure Cosmos DB.  

* Если вы хотите получить гарантированную пропускную способность для данного контейнера, поддерживаемого Соглашением об уровне обслуживания.

<a id="set-throughput-sdk"></a>

## <a name="set-throughput-by-using-sql-api-for-net"></a>Настройка пропускной способности с помощью API SQL для .NET

### <a name="set-throughput-at-the-container-level"></a>Настройка пропускной способности на уровне контейнера
Ниже приведен фрагмент кода для создания отдельного контейнера с 3000 ЕЗ/с с помощью пакета SDK для .NET для API SQL:

```csharp
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 3000 });
```

### <a name="set-throughput-for-a-set-of-containers-at-the-database-level"></a>Настройка пропускной способности для набора контейнеров на уровне базы данных

Ниже приведен фрагмент кода для подготовки 100 000 ЕЗ/с для набора контейнеров с помощью пакета SDK для .NET для API SQL:

```csharp
// Provision 100,000 RU/sec at the database level. 
// sharedCollection1 and sharedCollection2 will share the 100,000 RU/sec from the parent database
// dedicatedCollection will have its own dedicated 4,000 RU/sec, independant of the 100,000 RU/sec provisioned from the parent database
Database database = await client.CreateDatabaseAsync(new Database { Id = "myDb" }, new RequestOptions { OfferThroughput = 100000 });

DocumentCollection sharedCollection1 = new DocumentCollection();
sharedCollection1.Id = "sharedCollection1";
sharedCollection1.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(database.SelfLink, sharedCollection1, new RequestOptions())

DocumentCollection sharedCollection2 = new DocumentCollection();
sharedCollection2.Id = "sharedCollection2";
sharedCollection2.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(database.SelfLink, sharedCollection2, new RequestOptions())

DocumentCollection dedicatedCollection = new DocumentCollection();
dedicatedCollection.Id = "dedicatedCollection";
dedicatedCollection.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(database.SelfLink, dedicatedCollection, new RequestOptions { OfferThroughput = 4000 )
```

Azure Cosmos DB использует модель резервирования пропускной способности. То есть вы получаете счет за *зарезервированную* пропускную способность независимо от фактических показателей *использования*. Так как нагрузка, данные и шаблоны использования приложения меняются, вы можете легко увеличивать или уменьшать количество зарезервированных единиц запросов с помощью пакетов SDK или [портала Azure](https://portal.azure.com).

Каждый контейнер или набор контейнеров сопоставляется с ресурсом `Offer` в Azure Cosmos DB, который содержит метаданные о подготовленной пропускной способности. Вы можете изменить выделенную пропускную способность. Для этого найдите соответствующий ресурс предложения для контейнера и измените в нем значение пропускной способности. Ниже приведен фрагмент кода для изменения пропускной способности контейнера до 5000 единиц запроса в секунду с помощью пакета SDK для .NET. После изменения следует обновить все открытые окна на портале Azure, чтобы отображалось новое значение пропускной способности. 

```csharp
// Fetch the resource to be updated
// For a updating throughput for a set of containers, replace the collection's self link with the database's self link
Offer offer = client.CreateOfferQuery()
                .Where(r => r.ResourceLink == collection.SelfLink)    
                .AsEnumerable()
                .SingleOrDefault();

// Set the throughput to 5000 request units per second
offer = new OfferV2(offer, 5000);

// Now persist these changes to the database by replacing the original resource
await client.ReplaceOfferAsync(offer);
```

Изменение пропускной способности не влияет на доступность контейнера или набора контейнеров. Обычно новая зарезервированная пропускная способность становится доступной для приложения в течение нескольких секунд.

<a id="set-throughput-java"></a>

## <a name="to-set-the-throughput-by-using-the-sql-api-for-java"></a>Настройка пропускной способности с помощью API SQL для Java

Следующий фрагменте кода получает текущую пропускную способность и изменяет ее на 500 единиц запросов в секунду. Полный пример кода см. в файле [OfferCrudSamples.java](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/OfferCrudSamples.java) на сайте GitHub. 

```Java
// find offer associated with this collection
// To change the throughput for a set of containers, use the database's resource id instead of the collection's resource id
Iterator < Offer > it = client.queryOffers(
    String.format("SELECT * FROM r where r.offerResourceId = '%s'", collectionResourceId), null).getQueryIterator();
assertThat(it.hasNext(), equalTo(true));

Offer offer = it.next();
assertThat(offer.getString("offerResourceId"), equalTo(collectionResourceId));
assertThat(offer.getContent().getInt("offerThroughput"), equalTo(throughput));

// update the offer
int newThroughput = 500;
offer.getContent().put("offerThroughput", newThroughput);
client.replaceOffer(offer);
```

## <a name="get-the-request-charge-using-cassandra-api"></a>Получение платы за запросы с помощью API Cassandra 

API Cassandra поддерживает способ предоставления дополнительных сведений о плате за единицы запросов для конкретной операции. Например, плату за единицы запроса в секунду для операции вставки можно получить следующим образом.

```csharp
var insertResult = await tableInsertStatement.ExecuteAsync();
 foreach (string key in insertResult.Info.IncomingPayload)
        {
            byte[] valueInBytes = customPayload[key];
            string value = Encoding.UTF8.GetString(valueInBytes);
            Console.WriteLine($“CustomPayload:  {key}: {value}”);
        }
```


## <a name="get-throughput-by-using-mongodb-api-portal-metrics"></a>Получение пропускной способности с помощью метрик портала для API MongoDB

Чтобы без труда выполнить достоверную оценку затрат для единиц запроса для базы данных API MongoDB, используйте метрики [портала Azure](https://portal.azure.com). Используя диаграммы *Число запросов* и *Запросить оплату*, можно получить приблизительное количество единиц запроса, потребляемое каждой операцией, и количество единиц запроса, которое они потребляют относительно друг друга.

![Метрики портала API MongoDB][1]

### <a id="RequestRateTooLargeAPIforMongoDB"></a> Превышение лимита зарезервированной пропускной способности в API MongoDB
Для приложений, потребляющих свыше выделенного объема пропускной способности для контейнера или набора контейнеров, пропускная способность будет ограничена до тех пор, пока показатель ее потребления не упадет ниже предельного уровня. В случае ограничения пропускной способности серверная часть будет завершать запрос с кодом ошибки `16500`: `Too Many Requests`. По умолчанию API MongoDB автоматически повторяет запрос до 10 раз, прежде чем вернуть код ошибки `Too Many Requests`. Если возникает много ошибок с кодом `Too Many Requests`, рекомендуется либо добавить логику повтора в процедуры обработки ошибок в своем приложении, либо [увеличить подготовленную пропускную способность контейнера](set-throughput.md).

## <a id="GetLastRequestStatistics"></a>Получение сведений о затратах единиц запроса с помощью команды GetLastRequestStatistics в API MongoDB

API MongoDB поддерживает пользовательскую команду *getLastRequestStatistics*, которая используется, чтобы получить сведения о затратах единиц запроса для указанной операции.

Например, в оболочке Mongo выполните операцию, для которой необходимо проверить затраты единиц запроса.
```
> db.sample.find()
```

Затем выполните команду *getLastRequestStatistics*.
```
> db.runCommand({getLastRequestStatistics: 1})
{
    "_t": "GetRequestStatisticsResponse",
    "ok": 1,
    "CommandName": "OP_QUERY",
    "RequestCharge": 2.48,
    "RequestDurationInMilliSeconds" : 4.0048
}
```

Один из методов оценки необходимого для приложения объема зарезервированной пропускной способности — записать затраты единиц запросов на выполнение стандартных операций для определенного элемента, который используется приложением, а затем оценить предполагаемое количество операций в секунду.

> [!NOTE]
> Если размер и количество индексированных свойств для типов элемента существенно отличаются, запишите затраты единиц запросов для соответствующей операции, выполняемой с каждым *типом* стандартного элемента.
> 
> 

## <a name="throughput-faq"></a>Часто задаваемые вопросы о пропускной способности

**Можно ли задать значение пропускной способности ниже 400 ЕЗ/с?**

400 ЕЗ/с — это минимальное значение пропускной способности, доступное для односекционных контейнеров Cosmos DB (минимальное значение для секционированных контейнеров — 1000 ЕЗ/с). Единицы запроса можно задать с интервалом в 100 ЕЗ/с, но невозможно задать значение пропускной способности равное 100 ЕЗ/с или любое значение менее 400 ЕЗ/с. Чтобы определить экономически эффективный метод разработки и тестирования в Cosmos DB, можно воспользоваться бесплатным [эмулятором Azure Cosmos DB](local-emulator.md), который развертывается локально и без дополнительных затрат. 

**Как настроить пропускную способность с использованием API MongoDB?**

Для настройки пропускной способности нет расширения API MongoDB. Рекомендуем использовать API SQL в соответствии с инструкциями в разделе о [настройке пропускной способности с помощью API SQL для .NET](#set-throughput-sdk).

## <a name="next-steps"></a>Дополнительная информация

* Дополнительные сведения об оценке единиц запроса и пропускной способности см. в статье [Единицы запросов в базе данных Azure Cosmos DB](request-units.md).

* Дополнительные сведения о подготовке и глобальном масштабировании с помощью Cosmos DB см. в статье [Секционирование в базе данных Azure Cosmos DB с помощью API DocumentDB](partition-data.md).

[1]: ./media/set-throughput/api-for-mongodb-metrics.png
