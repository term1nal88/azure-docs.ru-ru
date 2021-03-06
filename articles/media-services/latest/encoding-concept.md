---
title: Кодирование в облаке с помощью Служб мультимедиа Azure | Документация Майкрософт
description: В этой статье описывается процесс кодирования при использовании Служб мультимедиа Azure
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 04/21/2018
ms.author: juliako
ms.openlocfilehash: 69c5516ee503d774b143bb2d83f09ea863a00b31
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/25/2018
ms.locfileid: "47091555"
---
# <a name="encoding-with-azure-media-services"></a>Кодирование с помощью Служб мультимедиа Azure

Службы мультимедиа Azure позволяют кодировать файлы мультимедиа высокого качества в разные форматы, пригодные для воспроизведения в разных браузерах и на различных устройствах. Например, можно организовать потоковую передачу содержимого в форматах HLS или MPEG DASH от Apple. Службы мультимедиа также позволяют анализировать видео- и аудиосодержимое. В этой статье содержатся рекомендации о том, как закодировать содержимое с помощью Служб мультимедиа версии 3.

Для кодирования с помощью Служб мультимедиа версии 3 необходимо создать преобразование и задание. Преобразование определяет инструкции для параметров кодирования и выходных данных, а задание является экземпляром инструкций. Дополнительные сведения см. в статье [Преобразования и задания](transform-concept.md).

При кодировании с помощью Служб мультимедиа Azure используются предустановки. На их основе кодировщик обрабатывает входные файлы мультимедиа. Например, можно указать разрешение видео и число аудиоканалов для закодированного содержимого. 

Можно быстро приступить к работе, используя одну из рекомендуемых встроенных предустановок, основанных на передовых отраслевых рекомендациях. Можно также создать пользовательскую предустановку в соответствии с требованиями сценария или устройства. 

Сведения о кодировщике можно найти в [спецификации OpenAPI](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/mediaservices/resource-manager/Microsoft.Media/preview/2018-03-30-preview). 

## <a name="built-in-presets"></a>Встроенные предустановки

В настоящее время Службы мультимедиа поддерживают следующие встроенные предустановки кодирования:  

|**Имя предустановки**|**Сценарий**|**Дополнительные сведения**|
|---|---|---|
|**AudioAnalyzerPreset**|Анализ аудио|Эта предустановка применяет предопределенный набор операций анализа на основе ИИ, включая транскрибирование речи. В настоящее время она поддерживает обработку содержимого с одной звуковой дорожкой.<br/>Можно указать язык для полезных данных аудио во входных данных. Для этого используется формат BCP-47 "тег языка — регион" (например, en-US). Поддерживаются такие языки: en-US, en-GB, es-ES, es-MX, fr-FR, it-IT, ja-JP, pt-BR, zh-CN.|
|**VideoAnalyzerPreset**|Анализ аудио и видео|Извлекает из аудио и видео аналитические данные (подробные метаданные) и выдает файл в формате JSON. Можно указать, если нужно извлекать аналитические данные только для аудио при обработке видеофайла. Дополнительные сведения см. в статье [Руководство. Анализ видео с помощью Служб мультимедиа Azure](analyze-videos-tutorial-with-api.md).|
|**BuiltInStandardEncoderPreset**|Потоковая передача|Задает встроенную предустановку для кодирования входящего видео с помощью кодировщика категории "Стандартный". <br/>В настоящее время поддерживаются приведенные ниже предустановки.<br/>**EncoderNamedPreset.AdaptiveStreaming** (рекомендуется). Дополнительные сведения см. в статье [Кодирование с помощью автоматически созданной схемы скоростей](autogen-bitrate-ladder.md).<br/>**EncoderNamedPreset.AACGoodQualityAudio**. Данная предустановка создает один MP4-файл, содержащий только закодированный стереозвук со скоростью 192 Кбит/с.<br/>**EncoderNamedPreset.H264MultipleBitrate1080p**. Данная предустановка создает набор из 8 MP4-файлов с одинаковыми группами GOP, скоростями 400–6000 Кбит/с и стереофоническим звуком в формате AAC. Разрешение — 360–1080 пикселей.<br/>**EncoderNamedPreset.H264MultipleBitrate720p**. Данная предустановка создает набор из 6 MP4-файлов с одинаковыми группами GOP, скоростями 400–3400 Кбит/с и стереофоническим звуком в формате AAC. Разрешение — 360–720 пикселей.<br/>**EncoderNamedPreset.H264MultipleBitrateSD**. Данная предустановка создает набор из 5 MP4-файлов с одинаковыми группами GOP, скоростями 400–1600 Кбит/с и стереофоническим звуком в формате AAC. Разрешение — 360–480 пикселей.<br/><br/>Дополнительные сведения см. в статье [Руководство. Отправка, кодирование и потоковая передача видео с помощью API](stream-files-tutorial-with-api.md).|
|**StandardEncoderPreset**|Потоковая передача|Описывает параметры, используемые при кодировании входящего видео с помощью кодировщика категории "Стандартный". <br/>Эту предустановку следует использовать при настройке предустановок преобразования. Дополнительные сведения см. в [этой статье](customize-encoder-presets-how-to.md).|

## <a name="custom-presets"></a>Пользовательские предустановки

Службы мультимедиа полностью поддерживают настройку всех значений в предустановках в соответствии с конкретными требованиями к кодированию. Предустановка **StandardEncoderPreset** используется при настройке предустановок преобразования. Подробные описания и примеры см. в статье о [настройке предустановок кодировщика](customize-encoder-presets-how-to.md).

## <a name="scaling-encoding-in-v3"></a>Масштабирование кодирования в версии 3

Сейчас клиенты должны использовать портал Azure или API AMS версии 2 для настройки ЕЗ (как описано в статье [Обзор масштабирования обработки мультимедиа](../previous/media-services-scale-media-processing-overview.md). 

## <a name="next-steps"></a>Дополнительная информация

### <a name="tutorials"></a>Учебники

В следующих руководствах показано, как кодировать содержимое с помощью Служб мультимедиа:

* [Отправка, кодирование и потоковая передача видео с помощью API](stream-files-tutorial-with-api.md)
* [Анализ видео с помощью Служб мультимедиа Azure](analyze-videos-tutorial-with-api.md)

### <a name="code-samples"></a>Примеры кода

В следующих примерах содержится код, который показывает, как выполнять кодирование с помощью Служб мультимедиа:

* [.NET Core](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/tree/master/NETCore)
* [интерфейс командной строки Azure](https://github.com/Azure/azure-docs-cli-python-samples/tree/master/media-services)

### <a name="sdks"></a>Пакеты SDK

Для кодирования содержимого можно использовать любой из следующих поддерживаемых пакетов SDK Служб мультимедиа версии 3.

* [интерфейс командной строки Azure](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
* [REST](https://docs.microsoft.com/rest/api/media/transforms)
* [.NET](https://docs.microsoft.com/dotnet/api/overview/azure/mediaservices/management?view=azure-dotnet)
* [Java](https://docs.microsoft.com/java/api/overview/azure/mediaservices)
* [Python](https://docs.microsoft.com/python/api/overview/azure/media-services?view=azure-python)

