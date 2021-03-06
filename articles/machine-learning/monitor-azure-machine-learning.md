---
title: Мониторинг Машинное обучение Azure | Документация Майкрософт
description: Узнайте, как использовать Azure Monitor для просмотра, анализа и создания оповещений о метриках из Машинное обучение Azure.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: larryfr
ms.author: aashishb
author: aashishb
ms.date: 10/01/2020
ms.openlocfilehash: a77f9c8f7e37d2c5a040a48b6bd96bef11d51f14
ms.sourcegitcommit: 6ab718e1be2767db2605eeebe974ee9e2c07022b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/12/2020
ms.locfileid: "94533486"
---
# <a name="monitor-azure-machine-learning"></a>Мониторинг Машинного обучения Azure

При наличии критически важных приложений и бизнес-процессов, использующих ресурсы Azure, необходимо отслеживать эти ресурсы на предмет их доступности, производительности и работы. В этой статье описываются данные мониторинга, созданные Машинное обучение Azure, а также анализ и оповещение этих данных с помощью Azure Monitor.

> [!TIP]
> Сведения в этом документе предназначены главным образом для __администраторов__ , так как он описывает мониторинг службы машинное обучение Azure и связанных служб Azure. Если вы являетесь __анализуом данных__ или __разработчиком__ и хотите отслеживать сведения, относящиеся к *выполнению обучения модели* , см. следующие документы:
>
> * [Запуск, отслеживание и отмена обучающих запусков](how-to-manage-runs.md)
> * [Запись метрик для учебных запусков](how-to-track-experiments.md)
> * [Отслеживание экспериментов с помощью потока ML](how-to-use-mlflow.md)
> * [Визуализация выполняется с помощью TensorBoard](how-to-monitor-tensorboard.md)
>
> Если требуется отслеживать сведения, создаваемые моделями, развернутыми в виде веб-служб или модулей IoT Edge, см. раздел [сбора данных модели](how-to-enable-data-collection.md) и [мониторинг с помощью Application Insights](how-to-enable-app-insights.md).

## <a name="what-is-azure-monitor"></a>Общие сведения об Azure Monitor

Машинное обучение Azure создает данные мониторинга с помощью [Azure Monitor](../azure-monitor/overview.md), которая является полной службой мониторинга стека в Azure. Azure Monitor предоставляет полный набор функций для мониторинга ресурсов Azure. Он также может отслеживать ресурсы в других облаках и в локальной среде.

Начните с статьи [мониторинг ресурсов Azure с помощью Azure Monitor](../azure-monitor/insights/monitor-azure-resource.md), в котором описаны следующие понятия.

- Общие сведения об Azure Monitor
- затраты, связанные с мониторингом;
- данные мониторинга, собираемые в Azure;
- настройка сбора данных;
- стандартные средства Azure для анализа данных мониторинга и оповещения.

Следующие разделы посвящены этой статье путем описания конкретных данных, собранных для Машинное обучение Azure. В этих разделах также приведены примеры настройки сбора данных и анализа этих данных с помощью средств Azure.

> [!TIP]
> Сведения о затратах, связанных с Azure Monitor, см. в разделе [использование и оценка затрат](../azure-monitor/platform/usage-estimated-costs.md). Сведения о времени, которое требуется для отображения данных в Azure Monitor, см. в разделе [время приема данных журнала](../azure-monitor/platform/data-ingestion-time.md).

## <a name="monitoring-data-from-azure-machine-learning"></a>Мониторинг данных из Машинное обучение Azure

Машинное обучение Azure собирает данные мониторинга тех же типов, что и другие ресурсы Azure, описанные в разделе [мониторинг данных из ресурсов Azure](../azure-monitor/insights/monitor-azure-resource.md#monitoring-data). 

Подробный справочник по журналам и метрикам, созданным Машинное обучение Azure, см. в [справочнике по данным мониторинга машинное обучение Azure](monitor-resource-reference.md) .

<a id="configuration"></a>

## <a name="collection-and-routing"></a>Сбор и маршрутизация

Метрики платформы и журнал действий собираются и сохраняются автоматически, но их можно направить в другие расположения с помощью параметра диагностики.  

Журналы ресурсов не собираются и не сохраняются, пока вы не создадите параметр диагностики и не направите их в одно или несколько расположений.

Подробный процесс создания параметров диагностики с помощью портал Azure, CLI или PowerShell см. в статье [Создание параметров диагностики для сбора журналов и метрик платформы в Azure](../azure-monitor/platform/diagnostic-settings.md) . При создании параметра диагностики необходимо указать, какие категории журналов должны быть собраны. Категории для Машинное обучение Azure перечислены в [справочнике по данным мониторинга машинное обучение Azure](monitor-resource-reference.md#resource-logs).

> [!IMPORTANT]
> Для включения этих параметров требуются дополнительные службы Azure (учетная запись хранения, концентратор событий или Log Analytics), что может привести к увеличению затрат. Чтобы рассчитать оценочную стоимость, посетите [Калькулятор цен Azure](https://azure.microsoft.com/pricing/calculator).

Для Машинное обучение Azure можно настроить следующие журналы:

| Категория | Описание |
|:---|:---|
| амлкомпутеклустеревент | События из Машинное обучение Azure вычисление кластеров. |
| амлкомпутеклустернодивент | События из узлов в кластере Машинное обучение Azure COMPUTE. |
| амлкомпутежобевент | События из заданий, выполняемых на Машинное обучение Azure вычислений. |

> [!NOTE]
> При включении метрик в параметре диагностики сведения об измерении в настоящее время не включаются в данные, передаваемые в учетную запись хранения, в концентратор событий или в log Analytics.

Метрики и журналы, которые можно собираются, обсуждаются в следующих разделах.

## <a name="analyzing-metrics"></a>Анализ метрик

Вы можете анализировать метрики Машинное обучение Azure, а также метрики из других служб Azure, открыв **метрики** из меню **Azure Monitor** . Подробные сведения об использовании этого средства см. в статье [Начало работы с обозревателем метрик Azure](../azure-monitor/platform/metrics-getting-started.md).

Список собранных метрик платформы см. в разделе [мониторинг машинное обучение Azure метрики ссылок на данные](monitor-resource-reference.md#metrics).

Все метрики для Машинное обучение Azure находятся в **рабочей области машинное обучение службы** пространства имен.

![обозреватель метрик с выбранной рабочей областью службы Машинное обучение](./media/monitor-azure-machine-learning/metrics.png)

Для справки можно просмотреть список [всех метрик ресурсов, поддерживаемых в Azure Monitor](../azure-monitor/platform/metrics-supported.md).

### <a name="filtering-and-splitting"></a>Фильтрация и разбиение

Для метрик, поддерживающих измерения, фильтры можно применять с помощью значения измерения. Например, фильтрация **активных ядер** для **имени кластера** `cpu-cluster` . 

Можно также разделить метрику по измерению, чтобы визуализировать, как разные сегменты метрики сравниваются друг с другом. Например, можно разделить **тип шага конвейера** для просмотра числа типов шагов, используемых в конвейере.

Дополнительные сведения о фильтрации и разбиении см. в разделе [Advanced Features of Azure Monitor](../azure-monitor/platform/metrics-charts.md).

<a id="analyzing-log-data"></a>
## <a name="analyzing-logs"></a>анализ журналов;

Использование Azure Monitor Log Analytics требует создания диагностической конфигурации и включения __отправки сведений в log Analytics__. Дополнительные сведения см. в разделе [коллекции и маршрутизация](#collection-and-routing) .

Данные в журналах Azure Monitor хранятся в таблицах, где каждая таблица имеет собственный набор уникальных свойств. Машинное обучение Azure хранит данные в следующих таблицах:

| Таблица | Описание |
|:---|:---|
| амлкомпутеклустеревент | События из Машинное обучение Azure вычисление кластеров. |
| амлкомпутеклустернодивент | События из узлов в кластере Машинное обучение Azure COMPUTE. |
| амлкомпутежобевент | События из заданий, выполняемых на Машинное обучение Azure вычислений. |

> [!IMPORTANT]
> При выборе **журналов** в меню машинное обучение Azure log Analytics открывается с областью запроса, заданной в текущей рабочей области. Это означает, что запросы к журналам будут содержать данные только из этого ресурса. Если требуется выполнить запрос, включающий данные из других баз данных или служб Azure, выберите в меню **Azure Monitor** пункт **Журналы**. Подробные сведения см. в статье [Область запросов журнала и временной диапазон в Azure Monitor Log Analytics](../azure-monitor/log-query/scope.md).

Подробные справочные сведения о журналах и метриках см. в разделе [машинное обучение Azure мониторинге справочника по данным](monitor-resource-reference.md).

### <a name="sample-kusto-queries"></a>Примеры запросов Kusto

> [!IMPORTANT]
> При выборе **журналов** в меню [Service-name] log Analytics открывается с областью запроса, заданной в текущей рабочей области машинное обучение Azure. Это означает, что запросы к журналам будут содержать данные только из этого ресурса. Если требуется выполнить запрос, который включает данные из других рабочих областей или данных из других служб Azure, выберите **журналы** в меню **Azure Monitor** . Подробные сведения см. в статье [Область запросов журнала и временной диапазон в Azure Monitor Log Analytics](../azure-monitor/log-query/scope.md).

Ниже приведены запросы, которые можно использовать для мониторинга ресурсов Машинное обучение Azure. 

+ Получение невыполненных заданий за последние пять дней:

    ```Kusto
    AmlComputeJobEvent
    | where TimeGenerated > ago(5d) and EventType == "JobFailed"
    | project  TimeGenerated , ClusterId , EventType , ExecutionState , ToolType
    ```

+ Получение записей для определенного имени задания:

    ```Kusto
    AmlComputeJobEvent
    | where JobName == "automl_a9940991-dedb-4262-9763-2fd08b79d8fb_setup"
    | project  TimeGenerated , ClusterId , EventType , ExecutionState , ToolType
    ```

+ Получение событий кластера за последние пять дней для кластеров, в которых размер виртуальной машины Standard_D1_V2:

    ```Kusto
    AmlComputeClusterEvent
    | where TimeGenerated > ago(4d) and VmSize == "STANDARD_D1_V2"
    | project  ClusterName , InitialNodeCount , MaximumNodeCount , QuotaAllocated , QuotaUtilized
    ```

+ Получение узлов, выделенных за последние восемь дней:

    ```Kusto
    AmlComputeClusterNodeEvent
    | where TimeGenerated > ago(8d) and NodeAllocationTime  > ago(8d)
    | distinct NodeId
    ```

## <a name="alerts"></a>Предупреждения

Вы можете получить доступ к оповещениям для Машинное обучение Azure, открыв **оповещения** в меню **Azure Monitor** . Дополнительные сведения о создании оповещений см. в статье [Создание, просмотр и Управление оповещениями метрик с помощью Azure Monitor](../azure-monitor/platform/alerts-metric.md) .

В следующей таблице перечислены распространенные и Рекомендуемые правила оповещений метрик для Машинное обучение Azure.

| Тип оповещения | Условие | Описание |
|:---|:---|:---|
| Неудачные развертывания модели | Тип агрегирования: Total, оператор: больше, пороговое значение: 0 | При сбое развертывания одной или нескольких моделей |
| Процент использования квоты | Тип агрегирования: Average, оператор: больше, пороговое значение: 90| Когда процент использования квоты превышает 90% |
| Недоступные для использования узлы | Тип агрегирования: Total, оператор: больше, пороговое значение: 0 | При наличии одного или нескольких неиспользуемых узлов |

## <a name="next-steps"></a>Дальнейшие действия

- Справочные сведения о журналах и метриках см. в разделе [мониторинг машинное обучение Azure справочника по данным](monitor-resource-reference.md).
- Сведения о работе с квотами, связанными с Машинное обучение Azure, см. в статье [Управление квотами и их запрос для ресурсов Azure](how-to-manage-quotas.md).
- Дополнительные сведения о мониторинге ресурсов Azure см. в статье [мониторинг ресурсов Azure с помощью Azure Monitor](../azure-monitor/insights/monitor-azure-resource.md).
