---
title: Архитектура Hadoop в Azure HDInsight
description: Сведения о компонентах Hadoop для хранения и обработки, используемые в кластерах HDInsight.
services: hdinsight
author: ashishthaps
ms.author: ashishth
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/19/2018
ms.openlocfilehash: f22cb6a56e0ef81e3d7799b38e33113f8b175457
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/05/2018
ms.locfileid: "43699436"
---
# <a name="hadoop-architecture-in-hdinsight"></a>Архитектура Hadoop в HDInsight

Hadoop состоит из двух основных компонентов — распределенной файловой системы Hadoop, обеспечивающей хранение, и модуля управления ресурсами YARN, обеспечивающего обработку. Благодаря возможностям хранения и обработки на кластере можно запускать программы MapReduce, чтобы выполнять требуемую обработку данных.

> [!NOTE]
> Обычно HDFS не развернута в кластере HDInsight для предоставления хранилища. Вместо этого компоненты Hadoop используют HDFS-совместимый слой интерфейса. Фактическая емкость хранилища предоставляется службой хранилища Azure или Azure Data Lake Store. Для Hadoop задания MapReduce выполняются в кластере HDInsight как будто HDFS присутствует и поэтому не требуют изменений для удовлетворения потребностей в хранении. В Hadoop в HDInsight хранилище является внешним, но обработка YARN остается внутренним компонентом. Дополнительную информацию см. во [введении в Azure HDInsight](hadoop/apache-hadoop-introduction.md).

В этой статье описан модуль YARN и как он координирует выполнение приложений в HDInsight.

## <a name="yarn-basics"></a>Основы YARN 

Платформа YARN управляет обработкой данных в Hadoop. Она состоит из двух основных служб, выполняющихся в качестве процессов на узлах кластера: 

* ResourceManager; 
* Диспетчер узлов

ResourceManager (диспетчер ресурсов) предоставляет вычислительные ресурсы кластера приложениям (например, заданиям MapReduce). Эти ресурсы предоставляются в качестве контейнеров, каждый из которых состоит из выделенных ядер ЦП и объема оперативной памяти. Если вы объединили все ресурсы, доступные в кластере, а затем распределили ядра и объем памяти в блоки, то каждый блок ресурсов — это контейнер. Каждый узел в кластере может содержать определенное число контейнеров. Таким образом, кластер имеет фиксированное ограничение на количество доступных контейнеров. Распределение ресурсов в контейнере можно настроить. 

При запуске приложения MapReduce в кластере диспетчер ресурсов предоставляет приложению контейнеры для выполнения. ResourceManager отслеживает состояние выполняемых приложений, доступную емкость кластера и наблюдает за приложениями, пока они не завершат работу и не освободят ресурсы. 

Он также запускает процесс веб-сервера, предоставляющий пользовательский веб-интерфейс для мониторинга состояния приложения.

Когда пользователь отправляет приложение MapReduce на выполнение в кластере, оно попадает в ResourceManager, который, в свою очередь, выделяет контейнер на доступных узлах NodeManager (диспетчер узлов). Именно на них и выполняется приложение. В первом выделенном контейнере выполняется специальное приложение ApplicationMaster. Оно отвечает за получение ресурсов в виде последующих контейнеров, необходимых для выполнения отправленного приложения. ApplicationMaster проверяет этапы приложения (например, этап сопоставления и снижения нагрузки) и факторы, определяемые количеством данных для обработки. Затем ApplicationMaster запрашивает (*согласовывает*) ресурсы у ResourceManager от имени приложения. ResourceManager, в свою очередь, предоставляет ресурсы, необходимые для выполнения приложения, из NodeManager в кластер для ApplicationMaster. 

NodeManager запускает задачи, составляющие приложение, а затем сообщает о ходе выполнения и состоянии приложению ApplicationMaster. Оно, в свою очередь, отправляет отчет о состоянии приложения в ResourceManager, который возвращает результаты клиенту.

## <a name="yarn-on-hdinsight"></a>YARN в HDInsight

YARN развертывается во всех типах кластера HDInsight. Для обеспечения высокого уровня доступности ResourceManager развертывается в двух экземплярах (первичный и вторичный), выполняющимися на первом и втором головных узлах в кластере соответственно. Одновременно может быть активным только один экземпляр ResourceManager. Экземпляры NodeManager выполняются на рабочих узлах, доступных в кластере.

![YARN в HDInsight](./media/hdinsight-hadoop-architecture/yarn-on-hdinsight.png)

## <a name="next-steps"></a>Дополнительная информация

* [Использование MapReduce в Hadoop в HDInsight](hadoop/hdinsight-use-mapreduce.md)
* [Введение в Azure HDInsight](hadoop/apache-hadoop-introduction.md)
