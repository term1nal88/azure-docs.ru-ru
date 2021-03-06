---
title: Создание определения для управляемого приложения Azure | Документация Майкрософт
description: Показано, как создать управляемое приложение Azure, предназначенное для членов вашей организации.
services: managed-applications
author: tfitzmac
ms.service: managed-applications
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.date: 10/04/2018
ms.author: tomfitz
ms.openlocfilehash: 1f80d7e63d994f0e3eb3733b99afaa1b056f4686
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/05/2018
ms.locfileid: "48804918"
---
# <a name="publish-an-azure-managed-application-definition"></a>Публикация определения для управляемого приложения Azure

Это краткое руководство содержит общие сведения о работе с управляемыми приложениями. Вы добавили определение управляемого приложения во внутренний каталог для пользователей в своей организации. Чтобы упростить вступление, мы уже создали файлы для управляемого приложения. Эти файлы доступны на GitHub. Сведения о создании этих файлов см. в руководстве [Публикация управляемого приложения для внутреннего использования](publish-service-catalog-app.md).

По завершении у вас будет группа ресурсов с именем **appDefinitionGroup** и определением управляемого приложения.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-a-resource-group-for-definition"></a>Создание группы ресурсов для определения

Ваше определение управляемого приложения существует в группе ресурсов. Группа ресурсов Azure — это логическая коллекция, в которой выполняется развертывание и администрирование ресурсов Azure.

Чтобы создать группу ресурсов, выполните следующую команду:

```azurecli-interactive
az group create --name appDefinitionGroup --location westcentralus
```

## <a name="create-the-managed-application-definition"></a>Создание определения управляемого приложения

При определении управляемого приложения необходимо выбрать пользователя, группу или приложение, которые управляют ресурсами для потребителя. Этому удостоверению предоставляются разрешения в управляемой группе ресурсов в соответствии с назначенной ролью. Обычно вы создаете группу Azure Active Directory для управления ресурсами. Однако для этой статьи используйте собственное удостоверение.

Чтобы получить идентификатор объекта своего удостоверения, укажите свое имя участника-пользователя в следующей команде:

```azurecli-interactive
userid=$(az ad user show --upn-or-object-id example@contoso.org --query objectId --output tsv)
```

Далее необходимо получить идентификатор определения встроенной роли RBAC, к которой вы хотите предоставить доступ пользователю. Следующая команда позволяет получить идентификатор определения роли "Владелец".

```azurecli-interactive
roleid=$(az role definition list --name Owner --query [].name --output tsv)
```

Теперь создайте ресурс определения управляемого приложения. Управляемое приложение содержит только учетную запись хранения.

```azurecli-interactive
az managedapp definition create \
  --name "ManagedStorage" \
  --location "westcentralus" \
  --resource-group appDefinitionGroup \
  --lock-level ReadOnly \
  --display-name "Managed Storage Account" \
  --description "Managed Azure Storage Account" \
  --authorizations "$userid:$roleid" \
  --package-file-uri "https://raw.githubusercontent.com/Azure/azure-managedapp-samples/master/samples/201-managed-storage-account/managedstorage.zip"
```

После выполнения команды в вашей группе ресурсов будет определение управляемого приложения. 

Далее приведены некоторые параметры, которые использовались в предыдущем примере:

* **resource-group**. Имя группы ресурсов, в которой создается определение управляемого приложения.
* **lock-level**. Тип блокировки управляемой группы ресурсов. Этот параметр предотвращает выполнение нежелательных операций в группе ресурсов. Сейчас поддерживается только тип блокировки ReadOnly. Если указать этот тип, пользователь будет иметь только доступ на чтение к ресурсам в управляемой группе ресурсов. Удостоверения издателей, которым предоставлен доступ к группе управляемых ресурсов, исключаются из блокировки.
* **authorizations**. Содержит идентификатор субъекта и идентификатор определения роли, используемые для предоставления разрешения для управляемой группы ресурсов. Это свойство указывается в формате `<principalId>:<roleDefinitionId>`. Если требуется более одного значения, укажите их в формате `<principalId1>:<roleDefinitionId1> <principalId2>:<roleDefinitionId2>`. Значения разделяются пробелами.
* **package-file-uri.** Расположение пакета ZIP, содержащего необходимые файлы. Пакет должен содержать файлы **mainTemplate.json** и **createUiDefinition.json**. **mainTemplate.json.** Определяет ресурсы Azure, созданные как часть управляемого приложения. Этот шаблон ничем не отличается от обычного шаблона Resource Manager. **createUiDefinition.json.** Создает пользовательский интерфейс для пользователей, создающих управляемое приложение на портале Azure.

## <a name="next-steps"></a>Дополнительная информация

Вы опубликовали определение управляемого приложения. Теперь можно приступать к развертыванию экземпляра этого определения.

> [!div class="nextstepaction"]
> [Краткое руководство. Развертывание приложения из каталога служб](deploy-service-catalog-quickstart.md)