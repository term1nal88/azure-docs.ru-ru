---
title: Отладка модулей Node.js для Azure IoT Edge | Документация Майкрософт
description: Сведения о разработке и отладке модулей Node.js для Azure IoT Edge в Visual Studio Code
services: iot-edge
keywords: ''
author: shizn
manager: timlt
ms.author: xshi
ms.date: 09/21/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: a1459e3cbd433e2997ffd822b961ac781a72ca90
ms.sourcegitcommit: 42405ab963df3101ee2a9b26e54240ffa689f140
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/28/2018
ms.locfileid: "47423533"
---
# <a name="use-visual-studio-code-to-develop-and-debug-nodejs-modules-for-azure-iot-edge"></a>Сведения о разработке и отладке модулей Node.js для Azure IoT Edge в Visual Studio Code

Вы можете перенести исполнение бизнес-логики в граничную систему, преобразовав ее в модули для Azure IoT Edge. В этой статье приведены подробные инструкции по использованию Visual Studio Code (VS Code) в качестве основного средства разработки для отладки модулей Node.js.

## <a name="prerequisites"></a>Предварительные требования
В этой статье предполагается, что для разработки вы используете компьютер или виртуальную машину под управлением Windows, macOS или Linux. Устройство IoT Edge может быть другим физическим устройством.

> [!NOTE]
> В этой статье об отладке показано два распространенных способа отладки модуля Node.js в VS Code. Один из способов — подключить процесс в контейнере модуля, а второй — запустить код модуля в режиме отладки. С возможностями отладки Visual Studio Code можно ознакомиться в [этой статье](https://code.visualstudio.com/Docs/editor/debugging).

Так как в этой статье Visual Studio Code используется как основное средство разработки, установите VS Code, а затем добавьте необходимые расширения:
* [Visual Studio Code](https://code.visualstudio.com/) 
* [расширение Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge); 
* [Расширение Docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker).

Чтобы создать модуль, требуется Node.js, включая npm для создания папки проекта, Docker для создания образа модуля и реестр контейнеров для хранения образа модуля:
* [Node.js](https://nodejs.org)
* [Docker](https://docs.docker.com/engine/installation/)
* [Реестр контейнеров Azure](https://docs.microsoft.com/azure/container-registry/) или [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags).
   * Вместо облачного реестра можно использовать локальный реестр Docker для создания прототипов и тестирования. 

Чтобы настроить локальную среду разработки для отладки, запуска и тестирования решения IoT Edge, требуется [средство разработки Azure IoT EdgeHub](https://pypi.org/project/iotedgehubdev/). Установите [Python (2.7/3.6) и Pip](https://www.python.org/). PIP входит в состав установщика Python. Затем установите **iotedgehubdev**, выполнив следующую команду в окне терминала.

   ```cmd
   pip install --upgrade iotedgehubdev
   ```

Для тестирования модуля на устройстве требуется активный Центр Интернета вещей по крайней мере с одним созданным идентификатором устройства IoT Edge. Если вы запустили управляющую программу IoT Edge на компьютере разработки, может потребоваться остановить EdgeHub и EdgeAgent, прежде чем перейти к следующему шагу. 

## <a name="create-a-new-solution-template"></a>Создайте новый шаблон решения

На следующих этапах показано, как создать модуль IoT Edge на основе Node.js с использованием Visual Studio Code и расширения Azure IoT Edge. Сначала нужно создать решение, а затем сформировать первый модуль в этом решении. Каждое решение может содержать несколько модулей. 

1. В Visual Studio Code выберите **Вид** > **Интегрированный терминал**.
2. В интегрированном терминале введите следующую команду, чтобы установить (или обновить) последнюю версию шаблона модуля Azure IoT Edge для Node.js:

   ```cmd/sh
   npm install -g yo generator-azure-iot-edge-module
   ```

3. В Visual Studio Code выберите **Вид** > **Палитра команд**. 
4. В палитре команд введите и выполните команду**Azure IoT Edge: New IoT Edge Solution**.

   ![Запуск команды создания решения IoT Edge](./media/how-to-develop-csharp-module/new-solution.png)

5. Перейдите к папке, где требуется создать решение, и щелкните **Выбрать папку**. 
6. Укажите имя для решения. 
7. Выберите **Модуль Node.js** в качестве шаблона для первого модуля в решении.
8. Присвойте модулю имя. Оно должно быть уникальным в пределах реестра контейнеров. 
9. Укажите репозиторий образов для модуля. VS Code автоматически заполняет имя модуля, поэтому нужно только заменить **localhost:5000** данными вашего реестра. Если для тестирования вы используете локальный реестр Docker, вполне подойдет значение localhost. Если используется Реестр контейнеров Azure, укажите сервер входа, заданный в параметрах реестра. Значение для сервера входа выглядит так: **\<имя реестра\>.azurecr.io**. Замените только часть строки localhost, не удаляйте имя модуля.

   ![Выбор репозитория образа Docker](./media/how-to-develop-node-module/repository.png)

VS Code принимает предоставленные сведения, создает решение IoT Edge, а затем загружает его в новом окне.

В решении имеется три элемента: 
* Папка **.vscode** содержит конфигурации отладки.
* Папка **modules** содержит вложенные папки для каждого модуля. Сейчас у вас только один модуль, но вы можете добавить модули в палитре команд с помощью команды **Azure IoT Edge: Add IoT Edge Module**. 
* В файле **ENV** перечислены переменные среды. Если реестр контейнеров Azure является вашим реестром, вы получите в нем имя пользователя и пароль для реестра контейнеров Azure.

   >[!NOTE]
   >Файл среды создается только в том случае, если вы указали репозиторий образов для модуля. Если вы не изменяли значения по умолчанию localhost для тестирования и отладки локально, объявлять переменные среды не требуется. 

* В файле **deployment.template.json** указан новый модуль, а также пример модуля **tempSensor**, который моделирует данные для тестирования. Дополнительные сведения о работе манифестов развертывания см. в статье [Сведения об использовании, настройке и повторном использовании модулей Azure IoT Edge (предварительная версия)](module-composition.md).

## <a name="develop-your-module"></a>Разработка модуля

Код Node.js, который поставляется с решением, по умолчанию находится в **modules** > [имя вашего модуля] > **app.js**. Модуль и файл deployment.template.json настраиваются так, чтобы можно было собрать решение, передать его в реестр контейнеров и развернуть на устройстве, чтобы начать тестирование, не меняя код. Модуль просто принимает входные данные из источника (в данном случае модуль tempSensor имитирует данные) и передает их в Центр Интернета вещей. 

Когда вы будете готовы настроить шаблон Node.js с добавлением собственного кода, используйте [пакеты SDK для Центра Интернета вещей Azure](../iot-hub/iot-hub-devguide-sdks.md), чтобы создать модули, которые удовлетворяют ключевые потребности решений Интернета вещей, такие как безопасность, управление устройствами и надежность. 

Visual Studio Code поддерживает Node.js. Дополнительные сведения о [способах работы с Node.js в VS Code](https://code.visualstudio.com/docs/nodejs/nodejs-tutorial).

## <a name="launch-and-debug-module-code-without-container"></a>Запуск и отладка кода модуля без контейнера

Модуль IoT Edge с кодом Node.js зависит от пакета SDK для устройств Azure IoT для Node.js. В коде модуля по умолчанию вы инициализируете **ModuleClient** с параметрами среды и именем входного канала. Это означает, что модуль IoT Edge Node.js требует, чтобы параметры среды запускались и выполнялись. Также необходимо отправлять или маршрутизировать сообщения во входные каналы. Модуль Node.js по умолчанию содержит только один канал входных данных, который называется **input1**.

### <a name="setup-iot-edge-simulator-for-single-module-app"></a>Настройка симулятора IoT Edge для приложения одного модуля

1. Чтобы настроить и запустить симулятор, в палитре команд VS Code введите и выберите **Azure IoT Edge: Start IoT Edge Hub Simulator for Single Module** (Azure IoT Edge. Запуск симулятора центра IoT Edge для одного модуля). Необходимо также указать имена входных каналов для приложения одного модуля, введите **input1** и нажмите клавишу ВВОД. Команда активирует CLI **iotedgehubdev** и запустит симулятор IoT Edge и тестирование контейнера модуля служебной программы. Вы можете увидеть приведенные ниже выходные данные во встроенном терминале, если симулятор был успешно запущен в режиме одного модуля. Также вы увидите команду `curl`, помогающую при пересылке сообщений. Оно понадобится вам позже.

   ![Настройка симулятора IoT Edge для приложения одного модуля](media/how-to-develop-csharp-module/start-simulator-for-single-module.png)

   Вы можете перейти в Docker Explorer и просмотреть состояние работы модуля.

   ![Состояние модуля симулятора](media/how-to-develop-csharp-module/simulator-status.png)

   Контейнер **edgeHubDev** — это ядро локального симулятора IoT Edge. Можно запустить его на компьютере разработки без управляющей программы безопасности IoT Edge и предоставлять параметры среды для собственного приложения модуля или контейнеров модуля. Контейнер **input** предоставляет REST API, помогающие отправлять сообщения в целевой входной канал в вашем модуле.

2. В палитре команд VS Code введите и выберите команду **Azure IoT Edge: Set Module Credentials to User Settings** (Azure IoT Edge. Установка учетных данных модуля для параметров пользователей), чтобы установить параметры среды модуля в `azure-iot-edge.EdgeHubConnectionString` и `azure-iot-edge.EdgeModuleCACertificateFile` в пользовательских параметрах. На эти параметры среды содержатся ссылки в **.vscode** > **launch.json**, и они также упоминаются в [параметрах пользователя VS Code](https://code.visualstudio.com/docs/getstarted/settings).

### <a name="debug-nodejs-module-in-launch-mode"></a>Отладка модуля Node.js в режиме запуска

1. Во встроенном терминале измените каталог на папку **NodeModule** и выполните следующую команду, чтобы установить пакеты Node.

   ```cmd
   npm install
   ```

2. Перейдите на страницу `app.js`. Добавьте точку останова в этот файл.

3. Перейдите в представление отладки VS Code. Выберите конфигурацию отладки **ModuleName Local Debug (Node.js)** (Локальная отладка имя_модуля (Node.js)). 

4. Нажмите кнопку **Начать отладку** или клавишу **F5**. Вы начнете сеанс отладки.

5. Во встроенном терминале VS Code выполните следующую команду, чтобы отправить сообщение **Hello World** в модуль. Это команда, показанная на предыдущих шагах, когда симулятор IoT Edge настроен успешно. Если текущий терминал блокируется, может потребоваться создать интегрированный терминал или переключиться на другой.

    ```cmd
    curl --header "Content-Type: application/json" --request POST --data '{"inputName": "input1","data":"hello world"}' http://localhost:53000/api/v1/messages
    ```

   > [!NOTE]
   > Если вы используете Windows, убедитесь, что используется оболочка встроенного терминала VS Code **Git Bash** или **WSL Bash**. Команду `curl` нельзя запустить в PowerShell или командной строке. 
   
   > [!TIP]
   > Вместо `curl` для отправки сообщений можно использовать [PostMan](https://www.getpostman.com/) или другие средства API.

6. В представлении отладки VS Code можно просмотреть переменные на панели слева. 

7. Чтобы остановить сеанс отладки, нажмите кнопку "Остановить" или клавиши **SHIFT+F5**. В палитре команд VS Code введите и выберите остановку **Azure IoT Edge: Stop IoT Edge Simulator** (Azure IoT Edge. Остановка симулятора IoT Edge), а затем очистите симулятор.


## <a name="build-module-container-for-debugging-and-debug-in-attach-mode"></a>Создание контейнера модуля для отладки и отладка в режиме подключения

Решение по умолчанию содержит два модуля: имитированный модуль датчика температуры и модуль канала Node.js. Имитируемый модуль датчика температуры продолжает отправлять сообщения в модуль канала Node.js, а затем сообщения отправляются по каналу в Центр Интернета вещей. В созданной папке модуля находится несколько файлов Docker для разных типов контейнеров. Вы можете использовать любой **DEBUG**-файл, чтобы создать модуль для тестирования. В настоящее время модули Node.js поддерживают отладку только в контейнерах linux amd64, windows-amd64 и linux-arm32v7.

### <a name="setup-iot-edge-simulator-for-iot-edge-solution"></a>Настройка симулятора IoT Edge для решения IoT Edge

Вместо установки управляющей программы безопасности IoT Edge на компьютере разработки можно запустить симулятор IoT Edge для выполнения решения IoT Edge. 

1. В обозревателе устройств с левой стороны щелкните идентификатор устройства IoT Edge правой кнопкой мыши и выберите **Setup IoT Edge Simulator** (Настроить симулятор IoT Edge), чтобы запустить симулятор со строкой подключения устройства.

2. Вы можете увидеть, что симулятор IoT Edge успешно настроен во встроенном терминале.

### <a name="build-and-run-container-for-debugging-and-debug-in-attach-mode"></a>Создание и выполнение контейнера для отладки и отладка в режиме подключения

1. В VS Code перейдите к файлу `deployment.template.json`. Обновите URL-адрес образа модуля, добавив в конце **.debug**.

2. Замените модуль Node.js createOptions в файле **deployment.template.json** содержимым ниже и сохраните этот файл: 
    ```json
    "createOptions": "{\"ExposedPorts\":{\"9229/tcp\":{}},\"HostConfig\":{\"PortBindings\":{\"9229/tcp\":[{\"HostPort\":\"9229\"}]}}}"
    ```

3. Перейдите в представление отладки VS Code. Выберите файл конфигурации отладки для модуля. Имя параметра отладки должно быть аналогично названиям **ModuleName Remote Debug (Node.js)** (Удаленная отладка имя_модуля (Node.js)) или **ModuleName Remote Debug (Node.js in Windows Container)** (Удаленная отладка имя_модуля (Node.js в контейнере Windows)), которые зависят от типа контейнера на компьютере разработки.

4. Выберите **Начать отладку** или нажмите клавишу **F5**. Выберите процесс для присоединения.

5. В представлении отладки VS Code можно просмотреть переменные на панели слева.

6. Чтобы остановить сеанс отладки, нажмите кнопку "Остановить" или клавиши **SHIFT+F5**. В палитре команд VS Code введите и выберите **Azure IoT Edge: Stop IoT Edge Simulator** (Azure IoT Edge. Остановка симулятора IoT Edge).

> [!NOTE]
> В предыдущем примере показаны способы отладки модулей Node.js IoT Edge в контейнерах. В нем добавлены открытые порты в контейнере модуля createOptions. После завершения отладки модулей Node.js рекомендуется удалить эти открытые порты для модулей IoT Edge, готовых для рабочей среды.

## <a name="next-steps"></a>Дополнительная информация

После создания модуля узнайте, как [развернуть модули Azure IoT Edge в Visual Studio Code](how-to-deploy-modules-vscode.md).

Чтобы разрабатывать модули для устройств IoT Edge, см. раздел [Понимание и использование пакетов SDK для Центра Интернета вещей Azure](../iot-hub/iot-hub-devguide-sdks.md).
