---
title: Отчет о проверке для Azure Stack | Документация Майкрософт
description: Узнайте результаты проверки готовности Azure Stack с помощью отчета.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/23/2018
ms.author: sethm
ms.reviewer: ''
ms.openlocfilehash: 1b2b06c02dc54c4369dd8490b714d1444d4b3b01
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/24/2018
ms.locfileid: "49986191"
---
# <a name="azure-stack-validation-report"></a>Отчет о проверке Azure Stack
Используйте средство проверки готовности Azure Stack, чтобы определить, выполнены ли условия, необходимые для поддержки развертывания и обслуживания окружения Azure Stack. Результаты сохраняются в JSON-файле отчета. В отчете отображаются подробные и сводные данные о состоянии компонентов, необходимых для развертывания Azure Stack. Этот отчет также содержит сведения о смене секретов для существующих развертываний Azure Stack.  

 ## <a name="where-to-find-the-report"></a>Где найти отчет
При работе средства результаты сохраняются в файле **AzsReadinessCheckerReport.json**. Также создается файл журнала с именем **AzsReadinessChecker.log**. Расположение этих файлов указывается в PowerShell вместе с результатами проверки.

![Запуск проверки](./media/azure-stack-validation-report/validation.png)

В обоих файлах сохраняются результаты всех последовательных проверок на этом компьютере.  Например, вы можете запустить средство только для проверки сертификатов, затем снова запустить его для проверки удостоверений Azure, а в третий раз — для проверки регистрации. Результаты всех трех проверок будут доступны в JSON-файле отчета.  

По умолчанию оба файла сохраняются в расположении *C:\Users\<имя_пользователя>\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json*.  
- Чтобы задать другое расположение отчетов, при запуске проверки можно указать в конце командной строки параметр **-OutputPath** ***&lt;путь&gt;***.   
- Укажите параметр **-CleanReport** в конце команды, чтобы удалить из файла *AzsReadinessCheckerReport.json* сведения. о предыдущих запусках средства.

## <a name="view-the-report"></a>Просмотр отчета
Чтобы просмотреть отчет в PowerShell, укажите путь к отчету в параметре **-ReportPath**. С помощью этой команды выводится содержимое отчета и перечисляются проверки, для которых еще нет доступных результатов.

Например, следующая команда позволяет просмотреть из командной строки PowerShell отчет, расположенный в текущем каталоге: 
   > `Read-AzsReadinessReport -ReportPath .\AzsReadinessReport.json` 

Результат выглядит следующим образом:

````PowerShell
Reading All Validation(s) from Report C:\Contoso-AzsReadinessCheckerReport.json

############### Certificate Validation Summary ###############

Certificate Validation results not available.

############### Registration Validation Summary ###############

Azure Registration Validation results not available.

############### Azure Identity Results ###############

Test                          : ServiceAdministrator
Result                        : OK
AAD Service Admin             : admin@contoso.onmicrosoft.com
Azure Environment             : AzureCloud
Azure Active Directory Tenant : contoso.onmicrosoft.com
Error Details                 : 

############### Azure Identity Validation Summary ###############

    Azure Identity Validation found no errors or warnings.

############### Azure Stack Graph Validation Summary ###############

Azure Stack Graph Validation results not available.

############### Azure Stack ADFS Validation Summary ###############

Azure Stack ADFS Validation results not available.

############### AzsReadiness Job Summary ###############

Index             : 0
Operations        : 
StartTime         : 2018/10/22 14:24:16
EndTime           : 2018/10/22 14:24:19
Duration          : 3
PSBoundParameters : 
````

## <a name="view-the-report-summary"></a>Просмотр сводного отчета
Чтобы просмотреть сводную информацию из отчета, добавьте параметр **-Summary** в конец командной строки PowerShell. Например:  
 > `Read-AzsReadinessReport -ReportPath .\Contoso-AzsReadinessReport.json -summary`  

Сводные данные содержат список проверок, по которым пока нет данных, а также сведения об успешных или неудачных результатах проверок. Результат выглядит следующим образом:

````PowerShell
Reading All Validation(s) from Report C:\Contoso-AzsReadinessCheckerReport.json

############### Certificate Validation Summary ###############

    Certificate Validation found no errors or warnings.
    
############### Registration Validation Summary ###############

    Registration Validation found no errors or warnings.

############### Azure Identity Validation Summary ###############

    Azure Identity Validation found no errors or warnings.

############### Azure Stack Graph Validation Summary ###############

Azure Stack Graph Validation results not available.

############### Azure Stack ADFS Validation Summary ###############

Azure Stack ADFS Validation results not available.
````


## <a name="view-a-filtered-report"></a>Просмотр отчета с фильтром
Чтобы просмотреть отчет, отфильтрованный проверкам определенного типа, укажите параметр **-ReportSections** с одним из следующих значений:
- Сертификат
- AzureRegistration;
- AzureIdentity;
- График
- ADFS
- Задания   
- Все  

Например, для извлечения из отчета сводной информации только о сертификатах выполните следующую команду PowerShell: 
 > `Read-AzsReadinessReport -ReportPath .\Contoso-AzsReadinessReport.json -ReportSections Certificate – Summary`


## <a name="see-also"></a>См. также
