---
title: Настройка эксперимента автоматического машинного обучения в службе "Машинное обучение Azure"
description: Автоматическое машинное обучение выбирает алгоритм и создает модель, готовую для развертывания. Узнайте о параметрах, с помощью которых можно настроить эксперименты автоматического машинного обучения.
author: nacharya1
ms.author: nilesha
ms.reviewer: sgilley
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 1aeb1315cfafbcdf3507a6e49d71e1f1e69b537c
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/19/2018
ms.locfileid: "49430193"
---
# <a name="configure-your-automated-machine-learning-experiment"></a>Настройка эксперимента автоматического машинного обучения

Автоматическое машинное обучение выбирает алгоритм и гиперпараметры, а также создает модель, готовую для развертывания. Модель также можно загрузить, чтобы выполнить дальнейшую настройку. Есть несколько параметров, с помощью которых можно настроить эксперименты автоматического машинного обучения. В этом руководстве вы узнаете, как определить различные параметры конфигурации.

Примеры автоматического машинного обучения см. в статье [Tutorial: Train a classification model with automated machine learning in Azure Machine Learning service](tutorial-auto-train-models.md) (Руководство. Обучение модели классификации с помощью автоматического машинного обучения в службе "Машинное обучение Azure") или [Обучение моделей с помощью автоматического машинного обучения в облаке с использованием машинного обучения Azure](how-to-auto-train-remote.md).

Возможности настройки, доступные в автоматическом машинном обучении:

* выбор типа эксперимента (например, классификация или регрессия); 
* источник данных, форматы и получение данных;
* выбор целевого объекта вычислений (локальный или удаленный);
* параметры эксперимента автоматического машинного обучения;
* выполнение эксперимента автоматического машинного обучения;
* изучение метрик модели;
* регистрация и развертывание модели.

## <a name="select-your-experiment-type"></a>Выбор типа эксперимента
Прежде чем начать эксперимент, следует определить тип задачи машинного обучения, которую необходимо решить. Автоматическое машинное обучение поддерживает две категории контролируемого обучения: классификация и регрессия. Оно также поддерживает приведенные ниже алгоритмы во время автоматизации и настройки. Пользователю не нужно указывать алгоритм.
классификация; | Регрессия
--|--
sklearn.linear_model.LogisticRegression | sklearn.linear_model.ElasticNet
sklearn.linear_model.SGDClassifier  | sklearn.ensemble.GradientBoostingRegressor
sklearn.naive_bayes.BernoulliNB | sklearn.tree.DecisionTreeRegressor
sklearn.naive_bayes.MultinomialNB | sklearn.neighbors.KNeighborsRegressor
sklearn.svm.SVC | sklearn.linear_model.LassoLars
sklearn.svm.LinearSVC | sklearn.linear_model.SGDRegressor
sklearn.calibration.CalibratedClassifierCV |    sklearn.ensemble.RandomForestRegressor
sklearn.neighbors.KNeighborsClassifier |    sklearn.ensemble.ExtraTreesRegressor
sklearn.tree.DecisionTreeClassifier |   lightgbm.LGBMRegressor
sklearn.ensemble.RandomForestClassifier |
sklearn.ensemble.ExtraTreesClassifier   |
sklearn.ensemble.GradientBoostingClassifier |
lightgbm.LGBMClassifier |


## <a name="data-source-and-format"></a>Источник данных и формат
Автоматическое машинное обучение поддерживает данные, находящиеся на локальном компьютере или в облаке в хранилище BLOB-объектов Azure. Данные можно считать в поддерживаемых форматах данных scikit-learn. Данные можно считывать в массивы Numpy X (признаки) и y (целевая переменная, также известная как метка) или кадр данных Pandas. 

Примеры:

1.  Массивы Numpy

    ```python
    digits = datasets.load_digits()
    X_digits = digits.data 
    y_digits = digits.target
    ```

2.  Кадр данных Pandas

    ```python
    Import pandas as pd
    df = pd.read_csv("https://automldemods.blob.core.windows.net/datasets/PlayaEvents2016,_1.6MB,_3.4k-rows.cleaned.2.tsv", delimiter="\t", quotechar='"') 
    # get integer labels 
    le = LabelEncoder() 
    le.fit(df["Label"].values) 
    y = le.transform(df["Label"].values) 
    df = df.drop(["Label"], axis=1) 
    df_train, _, y_train, _ = train_test_split(df, y, test_size=0.1, random_state=42)
    ```

## <a name="fetch-data-for-running-experiment-on-remote-compute"></a>Получение данных для выполнения эксперимента на удаленных вычислительных ресурсах

Если эксперимент выполняется на удаленных вычислительных ресурсах, получение данных должно быть помещено в отдельный сценарий Python `get_data()`. Этот сценарий выполняется на удаленных вычислительных ресурсах, в которых выполняется эксперимент автоматического машинного обучения. `get_data` избавляет от необходимости получать данные по сети для каждой итерации. Без `get_data` эксперимент завершится ошибкой при выполнении на удаленных вычислительных ресурсах.

Вот пример `get_data`:

```python
%%writefile $project_folder/get_data.py 
import pandas as pd 
from sklearn.model_selection 
import train_test_split 
from sklearn.preprocessing import LabelEncoder 
def get_data(): # Burning man 2016 data 
    df = pd.read_csv("https://automldemods.blob.core.windows.net/datasets/PlayaEvents2016,_1.6MB,_3.4k-rows.cleaned.2.tsv", delimiter="\t", quotechar='"') 
    # get integer labels 
    le = LabelEncoder() 
    le.fit(df["Label"].values) 
    y = le.transform(df["Label"].values) 
    df = df.drop(["Label"], axis=1) 
    df_train, _, y_train, _ = train_test_split(df, y, test_size=0.1, random_state=42) 
    return { "X" : df, "y" : y }
```

В вашем объекте `AutoMLConfig` можно указать параметр `data_script` и ввести путь к файлу сценария `get_data`, как показано ниже:

```python
automl_config = AutoMLConfig(****, data_script=project_folder + "/get_data.py", **** )
```

Сценарий `get_data` может вернуть следующее:
Ключ | type |    Взаимно исключают друг друга с помощью | ОПИСАНИЕ
---|---|---|---
X | Кадр данных Pandas или массив Numpy | data_train, метка, столбцы |  Все признаки для обучения
y | Кадр данных Pandas или массив Numpy |   label   | Данные метки для обучения. Для классификации это должен быть массив целых чисел
X_valid | Кадр данных Pandas или массив Numpy   | data_train, метка | _Необязательно._ Все признаки для проверки. Если не указано, X разделяется на экземпляры для обучения и проверки
y_valid |   Кадр данных Pandas или массив Numpy | data_train, метка | _Необязательно._ Данные метки для проверки. Если не указано, y разделяется на экземпляры для обучения и проверки
sample_weight | Кадр данных Pandas или массив Numpy |   data_train, метка, столбцы| _Необязательно._ Значение веса для каждой выборки. Используйте, если необходимо назначить разный вес для точек данных 
sample_weight_valid | Кадр данных Pandas или массив Numpy | data_train, метка, столбцы |    _Необязательно._ Значение веса для каждой выборки для проверки. Если не указано, sample_weight разделяется на экземпляры для обучения и проверки
data_train |    Кадр данных Pandas |  X, y, X_valid, y_valid |    Все данные (признаки и метка) для обучения
label | строка  | X, y, X_valid, y_valid |  Какой столбец в data_train представляет метку
columns | Массив строк  ||  _Необязательно._ Список разрешений столбцов для использования с признаками
cv_splits_indices   | массив целых чисел ||  _Необязательно._ Список индексов для разделения данных для перекрестной проверки

## <a name="train-and-validation-data"></a>Обучение и проверка данных

Указать отдельный набор для обучения и проверки можно с помощью get_data() либо непосредственно в методе `AutoMLConfig`.

## <a name="cross-validation-split-options"></a>Варианты разделения для перекрестной проверки

### <a name="k-folds-cross-validation"></a>Перекрестная проверка по K-сверткам

Используйте параметр `n_cross_validations`, чтобы указать количество перекрестных проверок. Набор данных для обучения будет разделен случайным образом на свертки `n_cross_validations` одинакового размера. Во время каждого цикла перекрестной проверки один из свертков будет использоваться для проверки модели, обученной на оставшихся свертках. Этот процесс повторяется для циклов `n_cross_validations`, пока каждый сверток не будет использован один раз в качестве набора для проверки. Наконец, будет отправлен отчет о средних показателях по всем циклам `n_cross_validations`, и соответствующая модель будет переобучена на целом наборе данных для обучения.

### <a name="monte-carlo-cross-validation-aka-repeated-random-sub-sampling"></a>Перекрестная проверка Монте-Карло (так называемая повторная случайная вложенная выборка)

С помощью `validation_size` укажите процент набора данных для обучения, который должен использоваться для проверки, а с помощью `n_cross_validations` укажите число перекрестных проверок. Во время каждого цикла перекрестной проверки подмножество размера `validation_size` будет выбрано случайным образом для проверки модели, обученной с использованием оставшихся данных. Наконец, будет отправлен отчет о средних показателях по всем циклам `n_cross_validations`, и соответствующая модель будет переобучена на целом наборе данных для обучения.

### <a name="custom-validation-dataset"></a>Настраиваемый проверочный набор данных

Используйте настраиваемый проверочный набор данных, если случайное разделение недопустимо (как правило, данные временных рядов или несбалансированные данные). Таким образом можно указать собственный проверочный набор данных. Модель будет анализироваться с использованием проверочного набора данных, указанного вместо случайного набора данных.

## <a name="compute-to-run-experiment"></a>Объект вычислений для выполнения эксперимента

Затем следует определить, где модель будет обучаться. Обучающий эксперимент автоматического машинного обучения выполняется в целевом объекте вычислений, которым вы владеете и управляете. 

Поддерживаются следующие варианты объектов вычислений:
1.  Локальный компьютер или ноутбук. Как правило, при наличии небольшого набора данных и если вы по-прежнему находитесь на этапе изучения.
2.  Удаленный компьютер в облаке — [Виртуальная машина для обработки и анализа данных Azure](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/) под управлением Linux. Если у вас большой набор данных и требуется выполнить масштабирование до большого компьютера, который доступен в облаке Azure. 
3.  Кластер Azure Batch AI — управляемый кластер, с помощью которого можно развернуть и параллельно выполнять итерации автоматического машинного обучения. 

<a name='configure-experiment'/>
## <a name="configure-your-experiment-settings"></a>Настройка параметров эксперимента

Есть несколько средств, с помощью которых можно настроить эксперименты автоматического машинного обучения. Эти параметры задаются путем создания экземпляра объекта `AutoMLConfig`.

Некоторые примеры:

1.  Эксперимент классификации со значением AUC_weighted в качестве основной метрики с наибольшим максимальным временем в 12 000 секунд на итерацию и окончанием эксперимента после 50 итераций и 2 свертков перекрестной проверки.

    ```python
    automl_classifier = AutoMLConfig(
        task='classification',
        primary_metric='AUC_weighted',
        max_time_sec=12000,
        iterations=50,
        X=X, 
        y=y,
        n_cross_validations=2)
    ```
2.  Ниже приведен пример эксперимента регрессии, который заканчивается после 100 итераций, каждая из которых длится до 600 секунд и в течение которых выполняется 5 свертков перекрестной проверки.

    ````python
    automl_regressor = AutoMLConfig(
        task='regression',
        max_time_sec=600,
        iterations=100,
        primary_metric='r2_score',
        X=X, 
        y=y,
        n_cross_validations=5)
    ````

В этой таблице приведены параметры, доступные для вашего эксперимента, и их значения по умолчанию.

Свойство |  ОПИСАНИЕ | По умолчанию
--|--|--
`task`  |Укажите тип задачи машинного обучения. Допустимые значения: <li>классификация;</li><li>регрессия;</li>    | None |
`primary_metric` |Метрика, которую требуется оптимизировать при компиляции модели. Например, если задать в качестве свойства primary_metric значение accuracy (точность), автоматическое машинное обучение выполнит поиск модели с максимальной точностью. Указать свойство primary_metric для эксперимента можно только один раз. Допустимые значения: <br/>**Классификация**:<br/><li> accuracy;  </li><li> AUC_weighted;</li><li> precision_score_weighted; </li><li> balanced_accuracy; </li><li> average_precision_score_weighted. </li><br/>**Регрессия:** <br/><li> normalized_mean_absolute_error; </li><li> spearman_correlation; </li><li> normalized_root_mean_squared_error; </li><li> normalized_root_mean_squared_log_error;</li><li> R2_score.    </li> | Для классификации: accuracy <br/>Для регрессии: spearman_correlation <br/> |
`exit_score` |  Для свойства primary_metric можно задать целевое значение. После обнаружения модели, которая отвечает целевой метрике primary_metric, автоматическое машинное обучение прекращает итерацию и эксперимент завершается. Если это значение не задано (по умолчанию), эксперимент автоматического машинного обучения продолжит выполнять указанное количество итераций. Принимает значение типа double. Если целевой метрики невозможно достичь, автоматическое машинное обучение будет выполнять итерации, пока не будет достигнуто указанное количество итераций.|   None
`iterations` |Максимальное количество итераций. Каждая итерация равняется заданию обучения, которое предоставляет конвейер. Конвейер — это предварительная обработка данных и модель. Чтобы получить модель высокого качества, используйте 250 или более итераций. | 100
`Concurrent_iterations`|    Максимальное количество итераций, выполняемых параллельно. Этот параметр работает только для удаленных объектов вычислений.|    1
`max_cores_per_iteration`   | Указывает, сколько ядер в целевом объекте вычислений будет использоваться для обучения одного конвейера. Если для алгоритма может использоваться несколько ядер, это увеличивает производительность на компьютере с несколькими ядрами. Для него можно задать значение -1, чтобы использовать все доступные ядра на компьютере.|  1
`max_time_sec` |    Ограничивает количество времени (в секундах), в течение которого длится определенная итерация. Если итерация превышает заданное количество, эта итерация отменяется. Если количество не задано, итерация продолжает выполняться до завершения. |   None
`n_cross_validations`   |Количество разделений перекрестной проверки.| None
`validation_size`   |Размер проверочного набора, выраженный в проценте от всего обучающего примера.|  None
`preprocess` | True или False <br/>Если задано значение true, эксперимент может выполнять предварительную обработку входных данных. Ниже приведено подмножество предварительной обработки:<li>отсутствующие данные: замещает отсутствующие числовые данные средним значением, текстовые — наиболее часто встречающимися вхождениями; </li><li>категориальные значения: если тип данных числовой, а количество уникальных значений — менее 5 %, преобразует в прямое кодирование; </li><li>И другое. Полный список см. в [репозитории GitHub](https://aka.ms/aml-notebooks).</li><br/>Примечание. Если данные разрежены, для предварительной обработки нельзя задать значение true. |  Ложь | 
`blacklist_algos`   | В эксперименте автоматического машинного обучения есть множество различных алгоритмов, которые он пытается выполнить. Исключите определенные алгоритмы из эксперимента автоматического машинного обучения. Это полезно, если известно, что какой-либо алгоритм не подходит для вашего набора данных. Исключение алгоритмов может сэкономить вычислительные ресурсы и время обучения.<br/>Допустимые значения для классификации:<br/><li>логистическая регрессия;</li><li>классификатор SGD;</li><li>MultinomialNB;</li><li>BernoulliNB;</li><li>SVM</li><li>LinearSVM;</li><li>kNN;</li><li>DT</li><li>RF;</li><li>дополнительные деревья;</li><li>градиентное усиление;</li><li>lgbm_classifier.</li><br/>Допустимые значения для регрессии:<br/><li>эластичная сеть;</li><li>регрессор градиентного усиления;</li><li>регрессор DT;</li><li>регрессор kNN;</li><li>Lasso lars;</li><li>регрессор SGD;</li><li>регрессор RF;</li><li>регрессор дополнительных деревьев.</li>|   None
`verbosity` |Определяет уровень ведения журнала со значением INFO для наиболее подробного протоколирования и CRITICAL для наименее подробного протоколирования.<br/>Допустимые значения:<br/><li>logging.INFO</li><li>logging.WARNING;</li><li>logging.ERROR;</li><li>logging.CRITICAL;</li>  | logging.INFO</li> 
`X` | Все признаки для обучения |  None
`y` |   Данные метки для обучения. Для классификации это должен быть массив целых чисел|  None
`X_valid`|_Необязательно._ Все признаки для проверки. Если не указано, X разделяется на экземпляры для обучения и проверки |   None
`y_valid`   |_Необязательно._ Данные метки для проверки. Если не указано, y разделяется на экземпляры для обучения и проверки    | None
`sample_weight` |   _Необязательно._ Значение веса для каждой выборки. Используйте, если необходимо назначить разный вес для точек данных |   None
`sample_weight_valid`   |   _Необязательно._ Значение веса для каждой выборки для проверки. Если не указано, sample_weight разделяется на экземпляры для обучения и проверки   | None
`run_configuration` |   Объект RunConfiguration.  Используется для удаленных выполнений. |None
`data_script`  |    Путь к файлу, содержащему метод get_data.  Требуется для удаленных выполнений.   |None


## <a name="run-experiment"></a>Выполнение эксперимента

Теперь можно инициировать выполнение эксперимента и создание модели. Передайте `AutoMLConfig` в метод `submit`, чтобы создать модель.

```python
run = experiment.submit(automl_config, show_output=True)
```

>[!NOTE]
>Сначала зависимости устанавливаются на новую DSVM.  Это может занять до 10 минут, прежде чем отобразятся выходные данные.
>Если для параметра `show_output` задать значение True, выходные данные отобразятся в консоли.


## <a name="explore-model-metrics"></a>Изучение метрик модели
Просмотреть результаты можно в мини-приложении или во встроенном окне при работе с записной книжкой. Просмотрите подробные сведения, чтобы отследить и оценить модели. (Убедитесь, что содержимое машинного обучения Azure содержит необходимые сведения для автоматического машинного обучения.)

В каждой итерации сохраняются следующие метрики:
* AUC_macro;
* AUC_micro;
* AUC_weighted
* accuracy
* average_precision_score_macro;
* average_precision_score_micro;
* average_precision_score_weighted.
* balanced_accuracy;
* f1_score_macro;
* f1_score_micro;
* f1_score_weighted;
* log_loss;
* norm_macro_recall;
* precision_score_macro;
* precision_score_micro;
* precision_score_weighted;
* recall_score_macro;
* recall_score_micro;
* recall_score_weighted;
* weighted_accuracy

## <a name="next-steps"></a>Дополнительная информация

Узнайте больше о том, [как и где можно развернуть модель](how-to-deploy-and-where.md).

Узнайте больше о том, [как обучить модель классификации с помощью автоматического машинного обучения](tutorial-auto-train-models.md) или [как обучить с использованием автоматического машинного обучения на удаленном ресурсе](how-to-auto-train-remote.md). 