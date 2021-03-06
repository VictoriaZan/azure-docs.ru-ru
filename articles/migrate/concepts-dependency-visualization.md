---
title: Анализ зависимостей в Azure — Оценка сервера
description: Описывает, как использовать анализ зависимостей для оценки с помощью функции "Миграция серверов в Azure".
ms.topic: conceptual
author: vineetvikram
ms.author: vivikram
ms.manager: abhemraj
ms.date: 09/15/2020
ms.openlocfilehash: 1f198d47191e7893e74b072ae8fd10546e3a6ee7
ms.sourcegitcommit: ea551dad8d870ddcc0fee4423026f51bf4532e19
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/07/2020
ms.locfileid: "96752215"
---
# <a name="dependency-analysis"></a>Анализ зависимостей

В этой статье описывается анализ зависимостей в службе "миграция Azure": Оценка сервера.


Анализ зависимостей определяет зависимости между обнаруженными локальными компьютерами. Он предоставляет следующие преимущества: 

- Вы можете собирать компьютеры в группы для оценки, более точно и с большей уверенностью.
- Вы можете определять компьютеры, которые необходимо перенести вместе. Это особенно полезно, если вы не уверены, какие компьютеры входят в развертывание приложения, которое требуется перенести в Azure.
- Можно определить, используются ли компьютеры, а какие компьютеры можно списать вместо переноса.
- Анализ зависимостей позволяет убедиться, что ничего не осталось, и таким образом избежать неожиданного простоя после миграции.
- [Ознакомьтесь](common-questions-discovery-assessment.md#what-is-dependency-visualization) с общими вопросами об анализе зависимостей.


## <a name="analysis-types"></a>Типы анализа

Существует два варианта развертывания анализа зависимостей.

**Параметр** | **Сведения** | **Общедоступное облако** | **Azure для государственных организаций**
----  |---- | ---- 
**Безагентное** | Опрашивает данные с виртуальных машин VMware с помощью API-интерфейсов vSphere.<br/><br/> Устанавливать агенты на виртуальных машинах не требуется.<br/><br/> Сейчас этот параметр доступен в предварительной версии только для виртуальных машин VMware. | Поддерживается. | Поддерживается.
**Анализ на основе агентов** | Использует [решение сопоставление служб](../azure-monitor/insights/service-map.md) в Azure Monitor, чтобы включить визуализацию и анализ зависимостей.<br/><br/> Агенты необходимо установить на каждом локальном компьютере, который необходимо проанализировать. | Поддерживается | Не поддерживается.


## <a name="agentless-analysis"></a>Анализ без агента

Анализ зависимостей без агента работает путем записи данных о подключении TCP с компьютеров, для которых он включен. На виртуальных машинах не установлены агенты. Соединения с одним и тем же исходным сервером и процессом, а также целевым сервером, процессом и портом группируются логически в зависимость. Захваченные данные зависимостей можно визуализировать в представлении карт или экспортировать в виде CSV-файла. На компьютерах, которые требуется проанализировать, не установлены агенты.

### <a name="dependency-data"></a>Данные зависимостей

После начала обнаружения данных зависимостей начинается опрос:

- Устройство "миграция Azure" опрашивает данные подключения TCP с компьютеров каждые пять минут для сбора данных.
- Данные собираются с гостевых виртуальных машин через vCenter Server с помощью API-интерфейсов vSphere.
- Опрос собирает эти данные:

    - Имя процессов, имеющих активные соединения.
    - Имя приложения, выполняющего процессы, имеющие активные соединения.
    - Порт назначения для активных подключений.

- Собранные данные обрабатываются на устройстве Azure Migrate, чтобы вывести сведения об удостоверениях и отправлять их в службу "миграция Azure каждые шесть часов".


## <a name="agent-based-analysis"></a>Анализ на основе агентов

При анализе на основе агента Серверная Оценка использует решение [сопоставление служб](../azure-monitor/insights/service-map.md) в Azure Monitor. На каждом компьютере, который необходимо проанализировать, устанавливается [агент Microsoft Monitoring Agent/log Analytics](../azure-monitor/platform/agents-overview.md#log-analytics-agent) и [Агент зависимостей](../azure-monitor/platform/agents-overview.md#dependency-agent).

### <a name="dependency-data"></a>Данные зависимостей

Анализ на основе агентов предоставляет следующие данные:

- Имя сервера исходного компьютера, процесс, имя приложения.
- Имя сервера конечного компьютера, процесс, имя приложения и порт.
- Количество соединений, задержка и сведения о передачи данных собираются и доступны для запросов Log Analytics. 



## <a name="compare-agentless-and-agent-based"></a>Сравнение без агента и на основе агентов

Различия между визуализацией без агента и визуализацией на основе агентов приведены в таблице.

**Требование** | **Безагентное** | **На основе агентов**
--- | --- | ---
**Поддержка** | Предварительный просмотр только для виртуальных машин VMware. [Ознакомьтесь](migrate-support-matrix-vmware.md#dependency-analysis-requirements-agentless) с поддерживаемыми операционными системами. | В общем доступе (GA).
**Агент** | На компьютерах, которые вы хотите проанализировать, не требуются агенты. | На каждом локальном компьютере, который необходимо проанализировать, должны быть агенты.
**Служба Log Analytics** | Не требуется. | Служба "миграция Azure" использует решение [сопоставление служб](../azure-monitor/insights/service-map.md) в [журналах Azure Monitor](../azure-monitor/log-query/log-query-overview.md) для анализа зависимостей.<br/><br/> Вы связываете рабочую область Log Analytics с проектом службы "миграция Azure". Рабочая область должна находиться в следующих регионах: Восточная часть США, Юго-Восточная Азия или Западная Европа. Рабочая область должна находиться в регионе, в котором [поддерживается Сопоставление служб](../azure-monitor/insights/vminsights-configure-workspace.md#supported-regions).
**Process** | Захватывает данные подключения TCP. После обнаружения он собирает данные через пять минут. | Сопоставление служб агенты, установленные на компьютере, собирают данные о TCP-процессах, а входящие и исходящие подключения для каждого процесса.
**Данные** | Имя сервера исходного компьютера, процесс, имя приложения.<br/><br/> Имя сервера конечного компьютера, процесс, имя приложения и порт. | Имя сервера исходного компьютера, процесс, имя приложения.<br/><br/> Имя сервера конечного компьютера, процесс, имя приложения и порт.<br/><br/> Количество соединений, задержка и сведения о передачи данных собираются и доступны для запросов Log Analytics. 
**Визуализация** | Карту зависимостей отдельного сервера можно просмотреть в течение одного часа до 30 дней. | Схема зависимостей одного сервера.<br/><br/> Схема зависимостей группы серверов.<br/><br/>  Карту можно просматривать только в течение часа.<br/><br/> Добавление и удаление серверов в группе из представления карт.
Экспорт данных | Данные за последние 30 дней можно скачать в формате CSV. | Данные можно запрашивать с помощью Log Analytics.



## <a name="next-steps"></a>Следующие шаги

- [Настройка](how-to-create-group-machine-dependencies.md) визуализации зависимостей на основе агента.
- [Испытайте](how-to-create-group-machine-dependencies-agentless.md) визуализацию зависимостей без агента для виртуальных машин VMware.
- Ознакомьтесь с [общими вопросами](common-questions-discovery-assessment.md#what-is-dependency-visualization) о визуализации зависимостей.
