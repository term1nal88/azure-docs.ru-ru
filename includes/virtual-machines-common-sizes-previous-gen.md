---
title: включение файла
description: включение файла
services: virtual-machines-windows, virtual-machines-linux
author: cynthn
ms.service: multiple
ms.topic: include
ms.date: 07/06/2018
ms.author: cynthn;azcspmt;jonbeck
ms.custom: include file
ms.openlocfilehash: 36902edd7b2df472960d19b8ef9a4ebd4cdfe695
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/07/2018
ms.locfileid: "37906686"
---
В этой статье содержатся сведения о размерах виртуальных машин предыдущих поколений. Эти размеры еще можно использовать, но вам также доступны новые поколения.


## <a name="ds-series"></a>Серия DS

ACU: 160

Хранилище класса Premium: поддерживается

Кэширование в хранилище класса Premium: поддерживается

| Размер | vCPU | Память, ГиБ | Временное хранилище (SSD): ГиБ | Максимальное число дисков данных | Максимальная пропускная способность хранилища с кэшированием и временного хранилища: операций ввода-вывода в секунду / Мбит/с (размер кэша указан в ГиБ) | Максимальная пропускная способность дисков без кэширования, операций ввода-вывода в секунду / Мбит/с | Максимальное количество сетевых адаптеров / ожидаемая пропускная способность сети (Мбит/с) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS1 |1 |3,5 |7 |4. |4000 / 32 (43) |3200 / 32 |2 / 500 |
| Standard_DS2 |2 |7 |14 |8 |8000 / 64 (86) |6400 / 64 |2 / 1000 |
| Standard_DS3 |4. |14 |28 |16 |16 000 / 128 (172) |12 800 / 128 |4 / 2000 |
| Standard_DS4 |8 |28 |56 |32 |32 000 / 256 (344) |25 600 / 256 |8 / 4000 |

<br>

## <a name="ds-series---memory-optimized"></a>Серия DS: оптимизированные для операций в памяти

ACU: 160<sup>1</sup>

Хранилище класса Premium: поддерживается

Кэширование в хранилище класса Premium: поддерживается

| Размер | vCPU | Память, ГиБ | Временное хранилище (SSD): ГиБ | Максимальное число дисков данных | Максимальная пропускная способность хранилища с кэшированием и временного хранилища: операций ввода-вывода в секунду / Мбит/с (размер кэша указан в ГиБ) | Максимальная пропускная способность дисков без кэширования, операций ввода-вывода в секунду / Мбит/с | Максимальное количество сетевых адаптеров / ожидаемая пропускная способность сети (Мбит/с) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS11 |2 |14 |28 |8 |8000 / 64 (72) |6400 / 64 |2 / 1000 |
| Standard_DS12 |4. |28 |56 |16 |16 000 / 128 (144) |12 800 / 128 |4 / 2000 |
| Standard_DS13 |8 |56 |112 |32 |32 000 / 256 (288) |25 600 / 256 |8 / 4000 |
| Standard_DS14 |16 |112 |224 |64 |64 000 / 512 (576) |51 200 / 512 |8/8000 |

<sup>1</sup> Максимальная возможная пропускная способность дисков (в операциях ввода-вывода в секунду или Мбит/с) виртуальных машин серии DS может быть ограничена из-за количества, размера и чередования подключенных дисков.  Дополнительные сведения см. в разделе [Хранилище класса "Премиум": высокопроизводительная служба хранилища для рабочих нагрузок виртуальных машин Azure](../articles/virtual-machines/windows/premium-storage.md).



## <a name="d-series"></a>Серия D 

ACU: 160

Хранилище класса Premium: не поддерживается

Кэширование в хранилище класса Premium: не поддерживается

| Размер         | vCPU | Память, ГиБ | Временное хранилище (SSD): ГиБ | Максимальная пропускная способность временного хранилища: операций ввода-вывода в секунду / чтение Мбит/с / запись Мбит/с | Макс. число дисков данных / пропускная способность: операций ввода-вывода в секунду | Максимальное количество сетевых адаптеров / ожидаемая пропускная способность сети (Мбит/с) |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D1  | 1         | 3,5         | 50             | 3000 / 46 / 23                                           | 4 / 4x500                         | 2 / 500                 |
| Standard_D2  | 2         | 7           | 100            | 6000 / 93 / 46                                           | 8 / 8x500                         | 2 / 1000                     |
| Standard_D3  | 4.         | 14          | 200            | 12000 / 187 / 93                                         | 16 / 16x500                         | 4 / 2000                     |
| Standard_D4  | 8         | 28          | 400            | 24000 / 375 / 187                                        | 32 / 32x500                       | 8 / 4000                     |

<br>

## <a name="d-series---memory-optimized"></a>Серия D: оптимизированные для операций в памяти

ACU: 160

Хранилище класса Premium: не поддерживается

Кэширование в хранилище класса Premium: не поддерживается

| Размер         | vCPU | Память, ГиБ | Временное хранилище (SSD): ГиБ | Максимальная пропускная способность временного хранилища: операций ввода-вывода в секунду / чтение Мбит/с / запись Мбит/с | Макс. число дисков данных / пропускная способность: операций ввода-вывода в секунду | Максимальное количество сетевых адаптеров / ожидаемая пропускная способность сети (Мбит/с) |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D11 | 2         | 14          | 100            | 6000 / 93 / 46                                           | 8 / 8x500                         | 2 / 1000                     |
| Standard_D12 | 4.         | 28          | 200            | 12000 / 187 / 93                                         | 16 / 16x500                         | 4 / 2000                     |
| Standard_D13 | 8         | 56          | 400            | 24000 / 375 / 187                                        | 32 / 32x500                       | 8 / 4000                     |
| Standard_D14 | 16        | 112         | 800            | 48000 / 750 / 375                                        | 64 / 64x500                       | 8/8000                |

<br>

## <a name="a-series---compute-intensive-instances"></a>Серия А: экземпляры для ресурсоемких вычислений

ACU: 225

Хранилище класса Premium: не поддерживается

Кэширование в хранилище класса Premium: не поддерживается

Экземпляры A8–A11 и серия Н также называются *экземплярами с интенсивным использованием вычислительных ресурсов*. Оборудование, на котором выполняются эти типоразмеры, разработано и оптимизировано для ресурсоемких приложений, в том числе приложений высокопроизводительного вычислительного кластера (HPC) и моделирования. Экземпляры A8-A11 используют процессор Intel Xeon E5-2670 с частотой 2,6 ГГц, а экземпляры серии H используют Intel Xeon E5-2667 v3 с частотой 3,2 ГГц.  Эта статья содержит сведения о количестве виртуальных ЦП, дисков данных и сетевых адаптеров, а также о пропускной способности хранилища и сети для каждого размера ВМ этой группы. 

| Размер | vCPU | Память, ГиБ | Временное хранилище (HDD): Гиб | Максимальное число дисков данных | Максимальная пропускная способность дисков данных, операций ввода-вывода в секунду | Максимальное число сетевых адаптеров|
| --- | --- | --- | --- | --- | --- | --- |
| Standard_A8 <sup>1</sup> |8 |56 |382 |32 |32x500 |2 |
| Standard_A9 <sup>1</sup> |16 |112 |382 |64 |64x500 |4. |
| Standard_A10 |8 |56 |382 |32 |32x500 |2  |
| Standard_A11 |16 |112 |382 |64 |64x500 |4. |

<sup>1</sup> Для приложений MPI в сети FDR InfiniBand включена выделенная внутренняя сеть RDMA, которая обеспечивает сверхнизкие задержки и высокую пропускную способность.

<br>

## <a name="a-series"></a>Серия A

ACU: 50–100

Хранилище класса Premium: не поддерживается

Кэширование в хранилище класса Premium: не поддерживается

| Размер | vCPU | Память, ГиБ | Временное хранилище (HDD): Гиб | Максимальное число дисков данных | Максимальная пропускная способность дисков данных, операций ввода-вывода в секунду | Максимальное количество сетевых адаптеров / ожидаемая пропускная способность сети (Мбит/с)  |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_A0 <sup>1</sup> |1 |0,768 |20 |1 |1x500 |2 / 100 |
| Standard_A1 |1 |1,75 |70 |2 |2x500 |2 / 500  |
| Standard_A2 |2 |3,5 |135 |4. |4x500 |2 / 500 |
| Standard_A3 |4. |7 |285 |8 |8x500 |2 / 1000 |
| Standard_A4 |8 |14 |605 |16 |16x500 |4 / 2000 |
| Standard_A5 |2 |14 |135 |4. |4x500 |2 / 500 |
| Standard_A6 |4. |28 |285 |8 |8x500 |2 / 1000 |
| Standard_A7 |8 |56 |605 |16 |16x500 |4 / 2000 |
<br>

<sup>1</sup> Размер A0 характеризуется превышением лимита подписки на физическом оборудовании. Только при использовании этого размера другие клиентские развертывания могут повлиять на производительность выполняющейся рабочей нагрузки. Относительная производительность будет ниже ожидаемой (с приблизительной изменчивостью в 15 %).

### <a name="standard-a0---a4-using-cli-and-powershell"></a>Standard A0–A4 поддерживает интерфейс командной строки и PowerShell.

В классической модели развертывания названия некоторых размеров виртуальных машин немного отличаются в интерфейсе командной строки и PowerShell:

* Standard_A0 — "Очень мелкий". 
* Standard_A1 — "Мелкий".
* Standard_A2 — "Средний".
* Standard_A3 — "Крупный".
* Standard_A4 — "Очень крупный".

## <a name="basic-a"></a>Basic A

Хранилище класса Premium: не поддерживается

Кэширование в хранилище класса Premium: не поддерживается

Размеры уровня "Базовый" используются преимущественно при разработке рабочих нагрузок и других приложений, для которых не требуется балансировка нагрузки, автомасштабирование и виртуальные машины с высоким потреблением памяти.

|Размер: размер и имя | vCPU |Память|Сетевые адаптеры (макс.)|Максимальный размер временного диска |Макс. количество дисков данных (по 1023 ГБ)|Макс. количество операций ввода-вывода в секунду (300 на диск)|
|---|---|---|---|---|---|---|
|A0\Basic_A0|1|768 МБ|2| 20 ГБ|1|1x300|
|A1\Basic_A1|1|1,75 ГБ|2| 40 ГБ |2|2x300|
|A2\Basic_A2|2|3,5 ГБ|2| 60 ГБ|4.|4x300|
|A3\Basic_A3|4.|7 ГБ|2| 120 ГБ |8|8x300|
|A4\Basic_A4|8|14 ГБ|2| 240 ГБ |16|16x300|
 


