---
title: Настройка аварийного восстановления для виртуальных машин Azure после миграции в Azure с помощью Azure Site Recovery | Документация Майкрософт
description: В этой статье объясняется, как подготовить виртуальные машины для настройки аварийного восстановления между регионами Azure после миграции в Azure с помощью Site Recovery.
services: site-recovery
author: ponatara
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: ponatara
ms.openlocfilehash: 3e26e40c26a27fdab1ec565dd4112b40acdd17d2
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/23/2018
ms.locfileid: "39213373"
---
# <a name="set-up-disaster-recovery-for-azure-vms-after-migration-to-azure"></a>Настройка аварийного восстановления виртуальных машин Azure после миграции в Azure 


Используйте приведенные здесь инструкции после [переноса локальных виртуальных машин на виртуальные машины Azure](tutorial-migrate-on-premises-to-azure.md) с помощью службы [Site Recovery](site-recovery-overview.md). Эта статья поможет вам подготовить виртуальные машины Azure к настройке аварийного восстановления во вторичный регион Azure с помощью Site Recovery.



## <a name="before-you-start"></a>Перед началом работы

Прежде чем настраивать аварийное восстановление, убедитесь, что миграция завершена правильно. Для успешного завершения миграции после отработки отказа следует выбрать параметр **Завершение миграции** для каждой виртуальной машины, которую требуется перенести. 



## <a name="install-the-azure-vm-agent"></a>Установка агента виртуальной машины Azure

Чтобы служба Site Recovery могла реплицировать виртуальную машину, на ней должен быть установлен [агент виртуальной машины](../virtual-machines/extensions/agent-windows.md) Azure.


1. Чтобы установить этот агент на виртуальной машине под управлением Windows, скачайте и запустите [установщик агента](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Для установки необходимы права администратора виртуальной машины.
2. Чтобы установить агент виртуальной машины на виртуальной машине под управлением Linux, установите последнюю версию [агента Linux](../virtual-machines/extensions/agent-linux.md). Чтобы выполнить установку, необходимо иметь права администратора. Мы рекомендуем устанавливать агент из репозитория дистрибутива. Не советуем устанавливать агент виртуальной машины Linux непосредственно из Github. 


## <a name="validate-the-installation-on-windows-vms"></a>Подтверждение установки на виртуальных машинах Windows

1. На виртуальной машине Azure в папке C:\WindowsAzure\Packages должен находиться файл WaAppAgent.exe.
2. Щелкните этот файл правой кнопкой мыши, выберите пункт **Свойства** и перейдите на вкладку **Подробно**.
3. В поле **Версия продукта** должно отображаться значение 2.6.1198.718 или выше.



## <a name="migration-from-vmware-vms-or-physical-servers"></a>Миграция с виртуальных машин VMware или физических серверов

При переносе в Azure локальных виртуальных машин VMware (или физических серверов) учтите следующее:

- Агент виртуальной машины Azure необходимо устанавливать вручную, только если служба Mobility Service, установленная на перенесенном компьютере, имеет версию 9.6 или более раннюю.
- На виртуальных машинах Windows начиная с версии службы Mobility Service 9.7.0.0 установщик также устанавливает последнюю доступную версию агента виртуальных машин Azure. При миграции эти виртуальные машины уже соответствуют предварительным требованиям агента установки для использования любого расширения виртуальной машины, включая расширение Site Recovery.
- Службу Mobility Service необходимо вручную удалить с виртуальной машины Azure с помощью одного из указанных ниже методов. Перед настройкой репликации перезапустите виртуальную машину.
    - Windows. На панели управления выберите **Установка и удаление программ** и удалите компонент **Microsoft Azure Site Recovery Mobility Service/Главный целевой сервер**. В командной строке с повышенными привилегиями выполните следующую команду:
        ```
        MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
        ```
    - Linux: войдите как привилегированный пользователь. В окне терминала перейдите в расположение **/user/local/ASR** и выполните следующую команду:
        ```
        uninstall.sh -Y
        ```


## <a name="next-steps"></a>Дополнительная информация

Ознакомьтесь с [кратким руководством по репликации](azure-to-azure-quickstart.md) виртуальных машин Azure в дополнительный регион.
