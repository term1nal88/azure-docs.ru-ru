---
title: Подключение к виртуальной машине Windows Server | Документация Майкрософт
description: Узнайте, как подключиться к виртуальной машине Windows и войти на нее с помощью портала Azure и модели развертывания Resource Manager.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ef62b02e-bf35-468d-b4c3-71b63fe7f409
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 09/13/2018
ms.author: cynthn
ms.openlocfilehash: b9cce5658b705e9d3255d2662b2a0157a2e548c3
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/27/2018
ms.locfileid: "47409037"
---
# <a name="how-to-connect-and-log-on-to-an-azure-virtual-machine-running-windows"></a>Как подключиться к виртуальной машине Azure под управлением Windows и войти на нее
Чтобы запустить сеанс удаленного рабочего стола, на портале Azure нажмите кнопку **Подключиться** на рабочем столе Windows. Сначала подключитесь к виртуальной машине, а затем войдите в систему.

Чтобы подключиться к виртуальной машине Windows с компьютера Mac, необходимо установить клиент RDP для Mac, например [Удаленный рабочий стол (Майкрософт)](https://itunes.apple.com/app/microsoft-remote-desktop/id715768417).

## <a name="connect-to-the-virtual-machine"></a>Подключение к виртуальной машине
1. Перейдите на [портал Azure](https://portal.azure.com/), если вы еще этого не сделали.
2. В меню слева выберите **Виртуальные машины**.
3. Затем выберите виртуальную машину из списка.
4. В верхней части страницы виртуальной машины щелкните **Подключиться**.
2. На странице **Connect to virtual machine** (Подключение к виртуальной машине) выберите соответствующие параметры и щелкните **Скачать RDP-файл**.
2. Откройте скачанный RDP-файл и при появлении запроса щелкните **Подключиться**. 
2. Появится предупреждение о том, что издатель RDP-файла неизвестен. Это ожидаемое поведение. Чтобы продолжить, в окне **Remote Desktop Connection** (Подключение к удаленному рабочему столу) выберите **Подключиться**.
   
    ![Снимок экрана с предупреждением о неизвестном издателе.](./media/connect-logon/rdp-warn.png)
3. В окне **Безопасность Windows** выберите **Варианты выбора** и нажмите **Использовать другую учетную запись**. На виртуальной машине введите учетные данные учетной записи и нажмите кнопку **ОК**.
   
     **Локальная учетная запись** — как правило, это имя пользователя и пароль, указанные при создании виртуальной машины. В данном случае домен представляет собой имя виртуальной машины и вводится в формате *имя_виртуальной_машины*&#92;*имя_пользователя*.  
   
    **Присоединенная к домену виртуальная машина** — если виртуальная машина входит в домен, введите имя пользователя в формате *домен*&#92;*имя_пользователя*. Учетная запись также должна входить в группу "Администраторы" или ей должны быть назначены права удаленного доступа к виртуальной машине.
   
    **Контроллер домена** — если виртуальная машина является контроллером домена, введите имя пользователя и пароль для учетной записи администратора домена для этого домена.
4. Щелкните **Да** для проверки удостоверения виртуальной машины и завершения входа в систему.
   
   ![Снимок экрана с сообщением о проверке удостоверения виртуальной машины](./media/connect-logon/cert-warning.png)


   > [!TIP]
   > Если кнопка **Подключиться** на портале неактивна и вы не используете канал [Express Route](../../expressroute/expressroute-introduction.md) или [VPN-подключение типа "сеть — сеть"](../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) для обмена данными с Azure, вам сначала нужно будет создать общедоступный IP-адрес и назначить его виртуальной машине. Только после этого вы сможете использовать протокол RDP. Дополнительные сведения см. в статье [Типы IP-адресов и методы распределения в Azure](../../virtual-network/virtual-network-ip-addresses-overview-arm.md).
   > 
   > 


## <a name="next-steps"></a>Дополнительная информация
Если у вас возникли проблемы с подключением, ознакомьтесь со статьей [Устранение неполадок с подключением к виртуальной машине Azure через удаленный рабочий стол](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

