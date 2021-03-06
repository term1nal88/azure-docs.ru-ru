---
title: Обнаружение угроз для хранилища данных SQL Azure | Документация Майкрософт
description: Настройка обнаружения угроз и изучение подозрительных событий в хранилище данных SQL Azure.
services: sql-data-warehouse
author: kavithaj
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: kavithaj
ms.reviewer: igorstan
ms.openlocfilehash: 7ff23235e9681301984e13e346b23f277662bb5c
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/30/2018
ms.locfileid: "43300940"
---
# <a name="threat-detection-in-azure-sql-data-warehouse"></a>Обнаружение угроз в хранилище данных SQL Azure
Настройка обнаружения угроз и изучение подозрительных событий в хранилище данных SQL Azure.

## <a name="what-is-threat-detection"></a>Что собой представляет обнаружение угроз
Система обнаружения угроз обнаруживает подозрительную активность в базе данных, указывающую на наличие потенциальных угроз безопасности. 

Благодаря оповещениям безопасности о подозрительной активности система обнаружения угроз формирует дополнительный уровень безопасности, позволяющий клиентам обнаруживать потенциальные угрозы по мере возникновения и реагировать на них. Пользователи могут анализировать подозрительные события с помощью [аудита хранилища данных SQL Azure](sql-data-warehouse-auditing-overview.md) , который позволяет определить причину таких событий (попытка получить доступ к данным в хранилище данных, нарушить их безопасность или использовать).
Система обнаружения угроз упрощает устранение потенциальных угроз безопасности хранилища данных, не требуя наличия у пользователя экспертных навыков в сфере безопасности либо умения работать в сложных системах отслеживания угроз.

Например, система обнаружения угроз обнаруживает определенную подозрительную активность в базе данных, указывающую на потенциальные попытки внедрения кода SQL. Внедрение кода SQL — один из распространенных видов угроз безопасности веб-приложений в Интернете, используемый для получения несанкционированного доступа к приложениям, управляемым данными. Взломщики используют уязвимые места приложения, чтобы ввести вредоносные инструкции SQL в поля ввода приложения, чтобы нарушить или изменить данные в базе данных.

## <a name="set-up-threat-detection-for-your-database"></a>Настройка системы обнаружения угроз для базы данных
1. Запустите портал Azure: [https://portal.azure.com](https://portal.azure.com).
2. Перейдите к колонке настройки хранилища данных SQL, которое требуется отслеживать. В колонке "Параметры" выберите **Аудит и обнаружение угроз**.
   
    ![Область навигации](media/sql-data-warehouse-security-threat-detection/1_td_click_on_settings.png)
3. В колонке настроек **Аудит и обнаружение угроз** установить переключатель аудита в положение **Вкл.**, после чего отобразятся настройки системы обнаружения угроз.
   
    ![Область навигации](media/sql-data-warehouse-security-threat-detection/2_td_turn_on_auditing.png)
4. Переведите переключатель системы обнаружения угроз в положение **ВКЛ.** .
5. Настройте список электронных адресов, на которые будут приходить оповещения безопасности в случае обнаружения подозрительной активности в хранилище данных.
6. В колонке настроек **Аудит и обнаружение угроз** щелкните **Сохранить**, чтобы сохранить новую или обновленную версию политики аудита и обнаружения угроз.
   
    ![Область навигации](media/sql-data-warehouse-security-threat-detection/3_td_turn_on_threat_detection.png)

## <a name="explore-anomalous-data-warehouse-activities-upon-detection-of-a-suspicious-event"></a>Анализ подозрительной активности в хранилище данных при обнаружении подозрительного события
1. При обнаружении подозрительной активности в базе данных вы получите уведомление по электронной почте. <br/>
   В электронном сообщении будет содержаться информация о подозрительном событии безопасности, включая характер подозрительной активности, имя базы данных и сервера, а также время, когда произошло событие. Кроме того, в сообщении приводится информация о возможных причинах возникновения события, а также рекомендуемые действия по поиску и устранению потенциальной угрозы безопасности базы данных.<br/>
   
    ![Область навигации](media/sql-data-warehouse-security-threat-detection/4_td_email.png)
2. В электронном сообщении щелкните ссылку **Журнал аудита SQL Azure** , после чего запустится портал Azure и будут показаны соответствующие записи аудита, зафиксированные примерно во время возникновения подозрительного события.
   
    ![Область навигации](media/sql-data-warehouse-security-threat-detection/5_td_audit_records.png)
3. Щелкните записи аудита, чтобы просмотреть более подробную информацию о подозрительной активности в базе данных, включая инструкции SQL, причины ошибки и IP-адрес клиента.
   
    ![Область навигации](media/sql-data-warehouse-security-threat-detection/6_td_audit_record_details.png)
4. В колонке записей аудита щелкните **Открыть в Excel**, чтобы открыть предварительно настроенный шаблон Excel и импортировать для более детального анализа записи из журнала аудита, зафиксированные примерно во время возникновения подозрительного события.<br/>
   **Примечание.** В Excel 2010 или более поздней версии требуется Power Query и параметр **Быстрое объединение**.
   
    ![Область навигации](media/sql-data-warehouse-security-threat-detection/7_td_audit_records_open_excel.png)
5. Чтобы настроить параметр **Быстрое объединение**, на вкладке ленты **POWER QUERY** выберите **Параметры**, чтобы открыть диалоговое окно "Параметры". Щелкните раздел «Конфиденциальность» и выберите в нем второй вариант — «Игнорировать уровни конфиденциальности и по возможности повышать производительность»:
   
    ![Область навигации](media/sql-data-warehouse-security-threat-detection/8_td_excel_fast_combine.png)
6. Чтобы загрузить записи журнала аудита SQL, убедитесь, что параметры на вкладке настроек установлены правильно, а затем откройте ленту «Данные» и нажмите кнопку «Обновить все».
   
    ![Область навигации](media/sql-data-warehouse-security-threat-detection/9_td_excel_parameters.png)
7. Результаты появятся на листе **Записи журнала аудита SQL** , что позволит выполнить более детальный анализ обнаруженной подозрительной активности, а также снизить степень влияния событий безопасности в приложении.

## <a name="next-steps"></a>Дополнительная информация
См. дополнительные сведения о [защите хранилища данных](sql-data-warehouse-overview-manage-security.md).
