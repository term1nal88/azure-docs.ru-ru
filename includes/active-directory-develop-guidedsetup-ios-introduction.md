---
title: включение файла
description: включение файла
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: ios
ms.workload: identity
ms.date: 09/19/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: 8c7f11d74d0a0b81f9f0c40871b2eaa3eb25f51f
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/24/2018
ms.locfileid: "49988287"
---
# <a name="call-the-microsoft-graph-api-from-an-ios-application"></a>Вызов API Microsoft Graph из приложения iOS

В этом руководстве описано, как собственное приложение iOS (Swift) может вызывать API-интерфейсы, для которых требуются маркеры доступа от конечной точки Microsoft Azure Active Directory (Azure AD) версии 2.0. В этом руководстве описано, как получить маркеры доступа и использовать их в вызовах к API Microsoft Graph и другим API.

Когда вы выполните все инструкции из этого руководства, ваше приложение сможет вызывать защищенный API из любой компании или организации, где используется Azure AD. Для защищенных вызовов API ваше приложение сможет использовать личные учетные записи, например outlook.com, live.com и другие, а также рабочие или учебные учетные записи.

## <a name="prerequisites"></a>Предварительные требования

- Для создания примера в этом руководстве используется XCode версии 10.x. Скачать XCode можно с [сайта iTunes](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12 "XCode Download URL").
- Для управления пакетами требуется диспетчер зависимостей [Carthage](https://github.com/Carthage/Carthage).

## <a name="how-this-guide-works"></a>Принцип работы с руководством

![Принцип работы с руководством](media/active-directory-develop-guidedsetup-ios-introduction/iosintro.png)

В этом руководстве пример приложения позволяет приложению iOS выполнять запрос к API Microsoft Graph или веб-API, который принимает маркеры от конечной точки Azure AD версии 2.0. В этом сценарии маркер добавляется в запросы HTTP с помощью заголовка **авторизации**. Получение маркера и его обновление выполняет библиотека проверки подлинности Майкрософт (MSAL).

### <a name="handle-token-acquisition-for-access-to-protected-web-apis"></a>Получение маркера для доступа к защищенным веб-интерфейсам API

После аутентификации пользователя пример приложения получает маркер. Этот маркер используется для выполнения запроса к API Microsoft Graph или веб-API, который защищен конечной точкой Azure AD версии 2.0.

API-интерфейсам, таким как Microsoft Graph, для обращения к определенным ресурсам требуется маркер доступа. Маркеры необходимы для чтения профиля пользователя, доступа к календарю пользователя, отправки электронной почты и т. д. Приложение может запросить маркер доступа, используя MSAL и определяя области API. Затем этот маркер доступа добавляется в заголовок **авторизации** HTTP для каждого вызова к защищенному ресурсу.

MSAL управляет кэшированием и обновлением маркеров доступа, поэтому вашему приложению не нужно этого делать.

## <a name="libraries"></a>Библиотеки

В этом руководстве используется следующая библиотека:

|Библиотека|ОПИСАНИЕ|
|---|---|
|[MSAL.framework](https://github.com/AzureAD/microsoft-authentication-library-for-objc)|Предварительная версия библиотеки проверки подлинности Microsoft для iOS|
