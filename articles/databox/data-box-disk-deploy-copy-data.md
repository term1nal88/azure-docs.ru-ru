---
title: Копирование данных на диск Microsoft Azure Data Box | Документация Майкрософт
description: Используйте это руководство, чтобы узнать, как скопировать данные на диск Azure Data Box
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: tutorial
ms.date: 10/09/2018
ms.author: alkohli
Customer intent: As an IT admin, I need to be able to order Data Box Disk to upload on-premises data from my server onto Azure.
ms.openlocfilehash: 7bc8b3ba415f8fe701098a9fa7e51d60ffb9df4e
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/11/2018
ms.locfileid: "49092462"
---
# <a name="tutorial-copy-data-to-azure-data-box-disk-and-verify"></a>Руководство. Копирование данных на диск Microsoft Azure Data Box и проверка

Это руководство содержит инструкции как копировать данные с главного компьютера, а затем создавать контрольные суммы для проверки целостности данных.

Из этого руководства вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
> * Копирование данных на диск Data Box
> * Проверка данных

## <a name="prerequisites"></a>Предварительные требования

Перед тем как начать, убедитесь в следующем.
- [Руководство по установке и настройке диска Azure Data Box](data-box-disk-deploy-set-up.md) изучено.
- Диски разблокированы и подключены к клиентскому компьютеру.
- На клиентском компьютере, с которого вы будете копировать данные на диски, установлена [поддерживаемая операционная система](data-box-disk-system-requirements.md).


## <a name="copy-data-to-disks"></a>Копирование данных на диски

Выполните следующие действия для подключения и копирования данных с компьютера на диск Data Box.

1. Просмотрите содержимое разблокированного диска. 

    ![Просмотр содержимого диска](media/data-box-disk-deploy-copy-data/data-box-disk-content.png)
 
2. Скопируйте данные, необходимые для импорта, как блочные BLOB-объекты в папку BlockBlob. Аналогичным образом скопируйте такие данные как VHD/VHDX в папку PageBlob. 

    Хранилище создается в учетной записи хранения Azure для каждой подпапки в папках BlockBlob и PageBlob. Все файлы в папках BlockBlob и PageBlob копируются по умолчанию в контейнер `$root` в учетную запись хранения Azure. Все файлы в контейнере `$root` всегда передаются как блочные BLOB-объекты.

    Если файлы и папки находятся в корневом каталоге, прежде чем начать копирование данных, необходимо перенести их в другую папку.

    Следуйте требованиям Azure к именованию контейнера и большого двоичного объекта.

    #### <a name="azure-naming-conventions-for-container-and-blob-names"></a>Соглашение об именовании Azure для имен контейнеров и больших двоичных объектов
    |Сущность   |Соглашения  |
    |---------|---------|
    |Имена контейнеров для блочного BLOB-объекта и страничного BLOB-объекта     |Поле должно начинаться с буквы или цифры и может содержать только строчные буквы, цифры и дефисы. Перед каждым дефисом и после него должна стоять буква или цифра. Последовательные дефисы в именах использовать запрещено. <br>Имя DNS должно быть допустимым, длиной от 3 до 63 символов.          |
    |Имена блочного BLOB-объекта и страничного BLOB-объекта    |Имена BLOB-объектов указываются с учетом регистра и могут содержать любое сочетание символов. <br>Длина имени BLOB-объекта должна составлять от 1 до 1024 символов.<br>Зарезервированные веб-адреса должны быть надлежащим образом экранированы.<br>Число сегментов пути, содержащих имя BLOB-объекта, не может превышать 254. Сегмент пути — это строка между последовательными символами-разделителями (например, косая черта"/"), которая соответствует имени виртуального каталога.         |

    > [!IMPORTANT] 
    > Все контейнеры и большие двоичные объекты должны соответствовать [соглашениям об именовании Azure](data-box-disk-limits.md#azure-block-blob-and-page-blob-naming-conventions). Если не следовать этим правилам, произойдет сбой передачи данных в Azure.

3. При копировании файлов убедитесь, что файлы не превышают ~4.7 TиБ для блочных BLOB-объектов и ~8 TиБ для страничных BLOB-объектов. 
4. Для копирования данных можно использовать функцию "Перетащить и отпустить" с помощью проводника. Также можно использовать любое средство SMB для копирования файлов, например Robocopy. Несколько заданий копирования могут быть инициированы с использованием следующей команды Robocopy.

    `Robocopy <source> <destination>  * /MT:64 /E /R:1 /W:1 /NFL /NDL /FFT /Log:c:\RobocopyLog.txt` 
    
    Параметры и функции для команды приведены в таблице следующим образом:
    
    |Параметры или функции  |ОПИСАНИЕ |
    |--------------------|------------|
    |Источник            | Указание пути к исходному каталогу.        |
    |Место назначения       | Указание пути к целевому каталогу.        |
    |/E                  | Копирование подкаталогов, включая пустые каталоги. |
    |/MT [: N]             | Создание многопоточных копий с N потоками, где N — целое число от 1 до 128. <br>Значение по умолчанию для N — 8.        |
    |/R: <N>             | Указание количества повторных попыток для неудавшихся копий. Значение по умолчанию N составляет 1 000 000 (один миллион попыток).        |
    |/W: <N>             | Указание времени ожидания между повторными попытками в секундах. Значение по умолчанию N — 30 (время ожидания 30 секунд).        |
    |/NFL                | Указание, что имена файлов не должны регистрироваться.        |
    |/NDL                | Указание, что имена каталогов не должны регистрироваться.        |
    |/FFT                | Предположение времени файла FAT (двухсекундная точность).        |
    |/Log:<Log File>     | Запись выходных данных о состоянии в файл журнала (перезапись существующего файла журнала).         |

    Можно использовать несколько дисков параллельно с несколькими заданиями, выполняемыми на каждом диске. 

6. Проверьте состояние копирования, когда выполняется задание. Ниже приведен пример выходных данных команды Robocopy для копирования файлов на диск Data Box.

    ```
    C:\Users>robocopy
        -------------------------------------------------------------------------------
       ROBOCOPY     ::     Robust File Copy for Windows
    -------------------------------------------------------------------------------
    
      Started : Thursday, March 8, 2018 2:34:53 PM
           Simple Usage :: ROBOCOPY source destination /MIR
    
                 source :: Source Directory (drive:\path or \\server\share\path).
            destination :: Destination Dir  (drive:\path or \\server\share\path).
                   /MIR :: Mirror a complete directory tree.
    
        For more usage information run ROBOCOPY /?    
    
    ****  /MIR can DELETE files as well as copy them !
    
    C:\Users>Robocopy C:\Git\azure-docs-pr\contributor-guide \\10.126.76.172\devicemanagertest1_AzFile\templates /MT:64 /E /R:1 /W:1 /FFT 
    -------------------------------------------------------------------------------
       ROBOCOPY     ::     Robust File Copy for Windows
    -------------------------------------------------------------------------------
    
      Started : Thursday, March 8, 2018 2:34:58 PM
       Source : C:\Git\azure-docs-pr\contributor-guide\
         Dest : \\10.126.76.172\devicemanagertest1_AzFile\templates\
    
        Files : *.*
    
      Options : *.* /DCOPY:DA /COPY:DAT /MT:8 /R:1000000 /W:30
    
    ------------------------------------------------------------------------------
    
    100%        New File                 206        C:\Git\azure-docs-pr\contributor-guide\article-metadata.md
    100%        New File                 209        C:\Git\azure-docs-pr\contributor-guide\content-channel-guidance.md
    100%        New File                 732        C:\Git\azure-docs-pr\contributor-guide\contributor-guide-index.md
    100%        New File                 199        C:\Git\azure-docs-pr\contributor-guide\contributor-guide-pr-criteria.md
                New File                 178        C:\Git\azure-docs-pr\contributor-guide\contributor-guide-pull-request-co100%  .md
                New File                 250        C:\Git\azure-docs-pr\contributor-guide\contributor-guide-pull-request-et100%  e.md
    100%        New File                 174        C:\Git\azure-docs-pr\contributor-guide\create-images-markdown.md
    100%        New File                 197        C:\Git\azure-docs-pr\contributor-guide\create-links-markdown.md
    100%        New File                 184        C:\Git\azure-docs-pr\contributor-guide\create-tables-markdown.md
    100%        New File                 208        C:\Git\azure-docs-pr\contributor-guide\custom-markdown-extensions.md
    100%        New File                 210        C:\Git\azure-docs-pr\contributor-guide\file-names-and-locations.md
    100%        New File                 234        C:\Git\azure-docs-pr\contributor-guide\git-commands-for-master.md
    100%        New File                 186        C:\Git\azure-docs-pr\contributor-guide\release-branches.md
    100%        New File                 240        C:\Git\azure-docs-pr\contributor-guide\retire-or-rename-an-article.md
    100%        New File                 215        C:\Git\azure-docs-pr\contributor-guide\style-and-voice.md
    100%        New File                 212        C:\Git\azure-docs-pr\contributor-guide\syntax-highlighting-markdown.md
    100%        New File                 207        C:\Git\azure-docs-pr\contributor-guide\tools-and-setup.md
    ------------------------------------------------------------------------------
    
                   Total    Copied   Skipped  Mismatch    FAILED    Extras
        Dirs :         1         1         1         0         0         0
       Files :        17        17         0         0         0         0
       Bytes :     3.9 k     3.9 k         0         0         0         0
       Times :   0:00:05   0:00:00                       0:00:00   0:00:00
        
       Speed :                5620 Bytes/sec.
       Speed :               0.321 MegaBytes/min.
       Ended : Thursday, March 8, 2018 2:34:59 PM
        
    C:\Users>
    ```
 
    
7. Откройте папку назначения для просмотра и проверки скопированных файлов. При наличии ошибок во время копирования скачайте файлы журналов для устранения неполадок. Файлы журналов располагаются так, как указано в команде Robobopy.
 


> [!IMPORTANT]
> - Вы несете ответственность за копирование данных в папки, соответствующие необходимому формату данных. Например, скопируйте данные блочного блока в соответствующую ему папку. Если формат данных не соответствует соответствующей папке (типу хранилища), то на более позднем этапе загрузка данных в Azure завершится с ошибкой.
> -  При копировании данных убедитесь, что размер данных соответствует ограничениям размера, указанным в разделе [Azure Data Box Disk limits (Preview)](data-box-disk-limits.md) (Ограничения для дисков Azure Data Box (предварительная версия)). 
> - Если данные, которые загружаются с помощью диска Data Box, одновременно загружаются другими приложениями за пределами диска Data Box, это может привести к сбоям задания загрузки и повреждению данных.

### <a name="split-and-copy-data-to-disks"></a>Разделение данных и их копирование на диски

Эту дополнительную процедуру можно применять при использовании нескольких дисков и при наличии большого набора данных, который нужно разделить и скопировать на все диски. Инструмент Data Box Split Copy позволяет разделить и скопировать данные на компьютере с Windows.

1. Скачайте инструмент Data Box Split Copy на компьютер с Windows и извлеките его в локальную папку. Этот инструмент был скачан при скачивании набора инструментов Диска Data Box для Windows.
2. Откройте проводник. Запишите диск источника данных и буквы диска, назначенного Диску Data Box. 

     ![Разбиение и копирование данных ](media/data-box-disk-deploy-copy-data/split-copy-1.png)
 
3. Определите исходные данные для копирования. Так, в нашем примере:
    - Были определены следующие данные блочного BLOB-объекта.

         ![Разбиение и копирование данных ](media/data-box-disk-deploy-copy-data/split-copy-2.png)    

    - Были определены следующие данные страничного BLOB-объекта.

         ![Разбиение и копирование данных ](media/data-box-disk-deploy-copy-data/split-copy-3.png)
 
4. Перейдите в папку, в которой извлекается программное обеспечение. В этой папке найдите файл SampleConfig.json. Это файл только для чтения, который можно изменить и сохранить.

   ![Разбиение и копирование данных ](media/data-box-disk-deploy-copy-data/split-copy-4.png)
 
5. Измените файл SampleConfig.json.
 
    - Укажите имя задания. На Диске Data Box будет создана папка, которая в конечном итоге станет контейнером в учетной записи хранения Azure, связанной с этими дисками. Имя задания должно соответствовать соглашениям об именовании контейнеров Azure. 
    - Укажите путь источника, записав формат пути в файле SampleConfigFile.json. 
    - Введите буквы дисков, соответствующие целевым дискам. Данные берутся из исходного пути и копируются на несколько дисков.
    - Укажите путь для файлов журналов. По умолчанию он отправляется в текущий каталог, в котором находится EXE-файл.

     ![Разбиение и копирование данных ](media/data-box-disk-deploy-copy-data/split-copy-5.png)

6. Чтобы проверить формат этого файла, перейдите в JSONlint. Сохраните файл как ConfigFile.json. 

     ![Разбиение и копирование данных ](media/data-box-disk-deploy-copy-data/split-copy-6.png)
 
7. Откройте окно командной строки и 

8. Запустите файл DataBoxDiskSplitCopy.exe. type

    `DataBoxDiskSplitCopy.exe PrepImport /config:<Your-config-file-name.json>`

     ![Разбиение и копирование данных ](media/data-box-disk-deploy-copy-data/split-copy-7.png)
 
9. Чтобы продолжить выполнение сценария, введите следующее:

    ![Разбиение и копирование данных ](media/data-box-disk-deploy-copy-data/split-copy-8.png)
  
10. Когда набор данных разделен и скопирован, отображается сводка инструмента Split Copy для сеанса копирования. Результат выполнения команды показан ниже.

    ![Разбиение и копирование данных ](media/data-box-disk-deploy-copy-data/split-copy-9.png)
 
11. Убедитесь, что данные на целевых дисках разделены. 
 
    ![Разбиение и копирование данных ](media/data-box-disk-deploy-copy-data/split-copy-10.png)
    ![Split copy data ](media/data-box-disk-deploy-copy-data/split-copy-11.png)
     
    Если вы изучите содержимое диска n, вы увидите, что созданы две подпапки, соответствующие данным формата блочного и страничного BLOB-объектов.
    
     ![Разбиение и копирование данных ](media/data-box-disk-deploy-copy-data/split-copy-12.png)

12. Если сеанс копирования завершается ошибкой, для восстановления используйте следующую команду:

    `DataBoxDiskSplitCopy.exe PrepImport /config:<configFile.json> /ResumeSession`


После копирования данные нужно проверить. 


## <a name="validate-data"></a>Проверка данных 

Чтобы проверить данные, выполните описанные ниже действия.

1. Запустите `DataBoxDiskValidation.cmd`, чтобы проверить контрольную сумму в папке *AzureImportExport* вашего диска. 
    
    ![Выходные данные инструмента проверки диска Data Box](media/data-box-disk-deploy-copy-data/data-box-disk-validation-tool-output.png)

2. Выберите подходящий вариант. **Мы рекомендуем всегда проверять файлы и создавать контрольные суммы с помощью варианта 2**. В зависимости от размера данных, это действие может занять некоторое время. После выполнения сценария выйдите из командного окна. При возникновении ошибок во время проверки и создания контрольной суммы вы получите уведомление и ссылку на журнал ошибок.

    ![Выходные Checksum](media/data-box-disk-deploy-copy-data/data-box-disk-checksum-output.png)

    > [!TIP]
    > - Сбросьте инструмент между запусками.
    > - Используйте вариант 1, чтобы проверить файлы, которые работают с большим набором данных, содержащим небольшие файлы (~КБ). В таких случаях создание контрольной суммы может занять много времени и производительность может быть снижена.

3. Если используется несколько дисков, выполните команду для каждого диска.

## <a name="next-steps"></a>Дополнительная информация

В этом руководстве раскрыты следующие сведения о диске Azure Data Box.

> [!div class="checklist"]
> * Копирование данных на диск Data Box
> * Проверка целостности данных

Перейдите к следующему руководству, чтобы узнать, как возвратить диск Data Box и проверить отправку данных в Azure.

> [!div class="nextstepaction"]
> [Return Azure Data Box Disk and verify data upload to Azure](./data-box-disk-deploy-picked-up.md) (Возврат диска Azure Data Box и проверка загрузки данных в Azure)

