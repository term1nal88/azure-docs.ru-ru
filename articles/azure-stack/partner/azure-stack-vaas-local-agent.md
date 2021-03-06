---
title: Развертывание локального агента | Документация Майкрософт
description: Разверните локальный агент в рамках проверки как услуги Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 10/19/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 6a7d1f886bd3a8457b6cad2e9fea11820805e7c9
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/23/2018
ms.locfileid: "49651947"
---
# <a name="deploy-the-local-agent"></a>Развертывание локального агента.

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Узнайте, как использовать локальный агент модели "проверка как услуга" (VaaS) для проверки оборудования. Локальный агент необходимо развернуть в решении Azure Stack, которое проверяется до выполнения проверочных тестов.

> [!Note]  
> Необходимо убедиться в том, что компьютер, на котором выполняется локальный агент, не потеряет исходящий доступ к Интернету. Этот компьютер должен быть доступен только для пользователей, которые авторизованы для использования VaaS от имени клиента.

Чтобы развернуть локальный агент, выполните приведенные ниже действия.

1. Установите локальный агент.
2. Выполните проверку работоспособности.
3. Запустите локальный агент.

## <a name="download-and-start-the-local-agent"></a>Скачивание и запуск локального агента

Скачайте агент на компьютер, соответствующий требованиям, в центре обработки данных, который не является частью системы Azure Stack, но имеет доступ ко всем конечным точкам Azure Stack.

### <a name="machine-prerequisites"></a>Предварительные требования к компьютерам

Убедитесь, что ваш компьютер соответствует следующим условиям:

- доступ ко всем конечным точкам Azure Stack;
- установка .NET 4.6 и PowerShell 5.0;
- не менее 8 ГБ ОЗУ;
- минимум 8-ядерные процессоры;
- не менее 200 ГБ дискового пространства;
- постоянное сетевое подключение к Интернету.

Azure Stack является тестируемой системы. Компьютер не должен быть частью Azure Stack или располагаться в облаке Azure Stack.

### <a name="download-and-install-the-agent"></a>Загрузка и установка агента

1. Откройте Windows PowerShell в командной строке с повышенными привилегиями на компьютере, который будет использоваться для выполнения тестов.
2. Выполните следующую команду, чтобы скачать локальный агент:

    ```PowerShell
    Invoke-WebRequest -Uri "https://storage.azurestackvalidation.com/packages/Microsoft.VaaSOnPrem.TaskEngineHost.latest.nupkg" -outfile "OnPremAgent.zip"
    Expand-Archive -Path ".\OnPremAgent.zip" -DestinationPath VaaSOnPremAgent -Force
    Set-Location VaaSOnPremAgent\lib\net46
    ```

3. Выполните следующую команду, чтобы установить зависимости локального агента:

    ```PowerShell
    $ServiceAdminCreds = New-Object System.Management.Automation.PSCredential "<aadServiceAdminUser>", (ConvertTo-SecureString "<aadServiceAdminPassword>" -AsPlainText -Force)
    Import-Module .\VaaSPreReqs.psm1 -Force
    Install-VaaSPrerequisites -AadTenantId $AadTenantId `
                              -ServiceAdminCreds $ServiceAdminCreds `
                              -ArmEndpoint https://adminmanagement.$ExternalFqdn `
                              -Region $Region
    ```

    **Параметры**

    | Параметр | ОПИСАНИЕ |
    | --- | --- |
    | aadServiceAdminUser | Глобальный администратор для клиента Azure AD. Например, это может быть vaasadmin@contoso.onmicrosoft.com. |
    | aadServiceAdminPassword | Пароль для пользователя с правами администратора. |
    | AADTenantId | Идентификатор клиента Azure AD для учетной записи Azure, зарегистрированный с помощью проверки как услуги. |
    | ExternalFqdn | Полное доменное имя можно получить из файла конфигурации. Дополнительные инструкции см. в статье [Распространенные параметры рабочего процесса для проверки как услуги Azure Stack](azure-stack-vaas-parameters.md). |
    | Регион | Регион вашего клиента Azure AD. |

Команда скачивает образ общедоступного репозитория образов (VHD операционной системы) и выполняет копирование из хранилища BLOB-объектов Azure в хранилище Azure Stack.

![Скачивание необходимых компонентов](media/installingprereqs.png)

> [!Note]
> Если при скачивании этих образов скорость низкая, скачайте их по отдельности в локальный общий ресурс и укажите параметры **-LocalPackagePath** *FileShareOrLocalPath*. Дополнительные рекомендации по скачиванию общедоступного репозитория образов см. в разделе [Обработка медленного сетевого подключения](azure-stack-vaas-troubleshoot.md#handle-slow-network-connectivity) статьи [Проверка как услуга: устранение неполадок](azure-stack-vaas-troubleshoot.md).

## <a name="checks-before-starting-the-tests"></a>Проверки перед запуском тестов

В рамках тестов выполняются удаленные действия. Компьютеру, на котором выполняются тесты, необходим доступ к конечным точкам Azure Stack. В противном случае тесты не будут работать. При использовании VaaS на локальном агенте нужен компьютер, на котором будет выполняться агент. Чтобы убедиться, что компьютер имеет доступ к конечным точкам Azure Stack, выполните следующие проверки.

1. Проверьте, что базовый URI доступен. Откройте командную строку или оболочку Bash и запустите следующую команду, заменив `<EXTERNALFQDN>` внешним полным доменным именем среды:

    ```bash
    nslookup adminmanagement.<EXTERNALFQDN>
    ```

2. Откройте веб-браузер и перейдите к `https://adminportal.<EXTERNALFQDN>`, чтобы убедиться, что портал MAS доступен.

3. Войдите, используя значения имени и пароля администратора служб Azure AD, предоставленные при создании тестового прохода.

4. Проверьте состояние работоспособности системы, выполнив командлет PowerShell **Test-AzureStack**, как описано в статье [Запуск проверочного теста в Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-diagnostic-test). Устраните предупреждения и исправьте ошибки перед запуском каких-либо тестов.

## <a name="run-the-agent"></a>Запуск агента

1. Откройте Windows PowerShell из командной строки с повышенными привилегиями.

2. Выполните следующую команду:

    ````PowerShell
    .\Microsoft.VaaSOnPrem.TaskEngineHost.exe -u <VaaSUserId> -t <VaaSTenantId>
    ````

      **Параметры**  
    | Параметр | ОПИСАНИЕ |
    | --- | --- |
    | VaaSUserId | Идентификатор пользователя, используемый для входа на портал VaaS (например, UserName@Contoso.com). |
    | VaaSTenantId | Идентификатор клиента Azure AD для учетной записи Azure, зарегистрированный с помощью проверки как услуги. |

    > [!Note]  
    > При запуске агента текущий рабочий каталог должен быть расположением исполняемого файла узла обработчика задач **Microsoft.VaaSOnPrem.TaskEngineHost.exe.**

Если вы не видите сообщения об ошибках, локальный агент был успешно выполнен. Следующий пример текста отображается в окне консоли.

`Heartbeat Callback at 11/8/2016 4:45:38 PM`

![Запущенный агент](media/startedagent.png)

Агент однозначно идентифицируется по имени. По умолчанию используется полное доменное имя (FQDN) компьютера, на котором он был запущен. Уменьшите окно, чтобы случайно не щелкнуть его, так как при изменении фокуса остальные действия приостанавливаются.

## <a name="next-steps"></a>Дополнительная информация

- [Проверка как услуга: устранение неполадок](azure-stack-vaas-troubleshoot.md)
- [Validation as a Service key concepts](azure-stack-vaas-key-concepts.md) (Проверка как услуга: основные понятия)
- [Schedule your first test](azure-stack-vaas-schedule-test-pass.md) (Планирование первого теста)