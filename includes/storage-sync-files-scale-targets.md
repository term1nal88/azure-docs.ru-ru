---
title: включение файла
description: включение файла
services: storage
author: wmgries
ms.service: storage
ms.topic: include
ms.date: 07/18/2018
ms.author: wgries
ms.custom: include file
ms.openlocfilehash: a29f1c4a625552dd958884c6a172bee470e61ca6
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/12/2018
ms.locfileid: "49312554"
---
| Ресурс | Цель | Строгое ограничение |
|----------|--------------|------------|
| Число служб синхронизации хранилища на подписку | 15 служб синхронизации хранилища | Нет  |
| Число групп синхронизации на службу синхронизации хранилища | 100 групп синхронизации | Yes |
| Число зарегистрированных серверов на службу синхронизации хранилища | 99 серверов | Yes |
| Число облачных конечных точек на группу синхронизации | 1 облачная конечная точка | Yes |
| Число конечных точек сервера на группу синхронизации | 50 конечных точек сервера | Нет  |
| Число конечных точек сервера на сервер | 33–99 конечных точек сервера | Да, но отличается в зависимости от конфигурации (ЦП, памяти, тома, обработка файла, количества файлов, и т.д.) |
| Размер конечной точки | 4 ТиБ | Нет  |
| Число объектов файловой системы (папок и файлов) на группу синхронизации | 25 млн объектов | Нет  |
| Максимальное число объектов файловой системы (папок и файлов) в каталоге | 200 000 объектов | Yes |
| Максимальная длина имени объекта максимальное (папок и файлов) | 255 символов | Yes |
| Максимальная длина дескриптора защиты объекта (папок и файлов) | 4 Киб | Yes |
| Размер файла | 100 ГиБ | Нет  |
| Минимальный размер для файла, который будет связан | 64 КиБ | Yes |
| Параллельные сеансы синхронизации | 2 активных сеанса синхронизации для каждого процессора или максимум 8 активных сеансов синхронизации для каждого сервера | Yes |
