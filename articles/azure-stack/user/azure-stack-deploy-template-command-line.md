---
title: Развертывание шаблонов в Azure Stack с помощью командной строки | Документация Майкрософт
description: Узнайте, как использовать кроссплатформенный интерфейс командной строки (CLI) для развертывания шаблонов в Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 9584177f-4af3-4834-864d-930b09ae0995
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/18/2018
ms.author: sethm
ms.reviewer: ''
ms.openlocfilehash: d40f9e8bf319f327879a71b23f9d9b82da924c2f
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/19/2018
ms.locfileid: "46365440"
---
# <a name="deploy-templates-in-azure-stack-using-the-command-line"></a>Развертывание шаблонов в Azure Stack с помощью командной строки

*Область применения: интегрированные системы Azure Stack и Пакет средств разработки Azure Stack*

С помощью командной строки можно развертывать шаблоны Azure Resource Manager в среде пакета средств разработки Azure Stack. Используя шаблоны Azure Resource Manager, можно развернуть и подготовить все ресурсы для приложения в рамках одной скоординированной операции.

## <a name="before-you-begin"></a>Перед началом работы

 - [Выполните установку и подключение](azure-stack-version-profiles-azurecli2.md) к Azure Stack с помощью Azure CLI.
 - Скачайте файлы *azuredeploy.json* и *azuredeploy.parameters.json* из [примера шаблона создания учетной записи хранения](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-create-storage-account).
 
## <a name="deploy-template"></a>Развертывание шаблона

Перейдите к папке, куда скачаны эти файлы, и выполните следующую команду для развертывания шаблона:

```azurecli
az group create "cliRG" "local" –f azuredeploy.json –d "testDeploy" –e azuredeploy.parameters.json
```

Эта команда развертывает шаблон в группу ресурсов **cliRG** в стандартном расположении подтверждения концепции Azure Stack.

## <a name="validate-template-deployment"></a>Проверка развертывания шаблона

Чтобы просмотреть эту группу ресурсов и учетную запись хранения, используйте следующие команды.

```azurecli
az group list

az storage account list
```

## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения о развертывании шаблонов см. в разделе:

[Развертывание шаблонов с помощью PowerShell](azure-stack-deploy-template-powershell.md)

