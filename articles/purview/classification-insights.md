---
title: Отчетность по классификации данных с помощью зрения Insights (Предварительная версия)
description: В этом пошаговом руководство описано, как просматривать и использовать отчеты о классификации зрения Insights для ваших данных.
author: batamig
ms.author: bagol
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 11/24/2020
ms.openlocfilehash: 553c33b3d5ea2e3f1ee81503cb69fe15db387af6
ms.sourcegitcommit: ad83be10e9e910fd4853965661c5edc7bb7b1f7c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/06/2020
ms.locfileid: "96745032"
---
# <a name="classification-insights-about-your-data-from-azure-purview"></a>Сведения о классификации данных из Azure зрения

В этом пошаговом руководство описывается, как получать доступ к данным, просматривать и фильтровать отчеты зрения Classification Insights.

Поддерживаются следующие источники данных: хранилище BLOB-объектов Azure, Azure Data Lake Storage (ADLS) GEN 1, Azure Data Lake Storage (ADLS) GEN 2, Azure Cosmos DB (API SQL), Azure синапсе Analytics (ранее SQL DW), база данных SQL Azure, Управляемый экземпляр SQL Azure, SQL Server

В этом пошаговом руководство вы узнаете, как:

> [!div class="checklist"]
> - Запуск учетной записи зрения из Azure
> - Просмотр сведений об классификации данных
> - Детализация для получения дополнительных сведений о классификации данных

## <a name="prerequisites"></a>Предварительные требования

Прежде чем приступить к работе с зрения Insights, убедитесь, что выполнены следующие действия.

- Настройка ресурсов Azure и заполнение соответствующих учетных записей тестовыми данными

- Настройка и завершение проверки тестовых данных в каждом источнике данных 

Дополнительные сведения см. [в статье Управление источниками данных в Azure зрения (Предварительная версия)](manage-data-sources.md).

## <a name="use-purview-classification-insights"></a>Использовать аналитику классификации зрения

В Azure зрения классификации похожи на теги субъектов и используются для пометки и обнаружения данных определенного типа, находящихся в области данных во время сканирования.

Зрения использует те же конфиденциальные типы данных, что и Microsoft 365, что позволяет растянуть существующие политики безопасности и обеспечить защиту всей области данных.

> [!NOTE]
> После того как вы проверили исходные типы источников, предоставьте **классификацию** "аналитика" на несколько часов, чтобы отразить новые активы.

**Чтобы просмотреть аналитику классификации, выполните следующие действия.**

1. Перейдите на экран экземпляра **Azure зрения** [в портал Azure](https://aka.ms/purviewportal) и выберите свою учетную запись зрения.

1. На странице **Обзор** в разделе Начало **работы** выберите плитку **Запуск учетной записи зрения** .

1. В зрения выберите пункт меню **Insights** :::image type="icon" source="media/insights/ico-insights.png" border="false"::: слева, чтобы получить доступ к области **Insights** .

1. В области **аналитика** :::image type="icon" source="media/insights/ico-insights.png" border="false"::: выберите **классификация** , чтобы отобразить отчет зрения **Classification Insights** .

   :::image type="content" source="./media/insights/select-classification-labeling-small.png" alt-text="Отчет по классификации аналитики" lightbox="media/insights/select-classification-labeling.png":::

   На странице основная **аналитика классификации** отображаются следующие области.

   |Область  |Описание  |
   |---------|---------|
   |**Общие сведения об источниках с классификациями**     |Отображает плитки, которые предоставляют: <br>— Число подписок, найденных в данных; <br>— Число уникальных классификаций, обнаруженных в данных; <br>— Число найденных классифицированных источников <br>— Число найденных классифицированных файлов <br>— Число найденных классифицированных таблиц         |
   |**Лучшие источники с классифицированными данными (за последние 30 дней)**     |Показывает тенденцию за последние 30 дней от числа найденных источников с классифицированными данными.            |
   |**Основные категории классификации по источникам**     |Показывает количество источников, обнаруженных категорией классификации, например **финансовых** или **правительственных организаций**.      |
   |**Основные классификации для файлов**     |Показывает основные классификации, применяемые к файлам в данных, например номера кредитных карт или местные идентификационные номера.         |
   |**Основные классификации для таблиц**     | Показывает основные классификации, применяемые к таблицам в данных, например персональные идентификационные данные. |   
   |  **Действие классификации** <br>(файлы и таблицы) |  Отображает отдельные графы для файлов и таблиц, каждый из которых отображает количество файлов или таблиц, классифицированных за выбранный период. <br>**По умолчанию**: 30 дней<br>Выберите фильтр **времени** над диаграммами, чтобы выбрать другой интервал времени для показа.    |
   |    |    |

## <a name="classification-insights-drilldown"></a>Углубленная детализация классификации

На любой из следующих диаграмм **классификации аналитики** выберите ссылку **Просмотреть дополнительные** сведения для получения дополнительных сведений:

- **Основные категории классификации по источникам**
- **Основные классификации для файлов**
- **Основные классификации для таблиц**
- **Действие классификации > данные классификации**

Пример:

:::image type="content" source="media/insights/view-classifications-small.png" alt-text="Просмотреть все классификации" lightbox="media/insights/view-classifications.png":::

Чтобы получить дополнительные сведения, выполните одно из следующих действий.

|Параметр  |Описание  |
|---------|---------|
|**Фильтрация данных**     |  Используйте фильтры, расположенные над сеткой, чтобы отфильтровать отображаемые данные, включая имя классификации, имя подписки или тип источника. <br><br>Если точное имя классификации неизвестно, можно ввести часть или все имя в поле **Фильтровать по ключевому слову** .       |
|**Сортировка сетки** |Выберите заголовок столбца, чтобы отсортировать сетку по этому столбцу. | 
|**Изменить столбцы**     |  Чтобы в сетке отображалось больше или меньше столбцов, выберите **изменить столбцы** :::image type="icon" source="media/insights/ico-columns.png" border="false"::: , а затем выберите столбцы, которые необходимо просмотреть или изменить.   |
|**Детализация углублением**     | Чтобы выполнить детализацию по определенной классификации, выберите имя в столбце **классификация** , чтобы просмотреть отчет **классификация по источнику** . <br><br>В этом отчете отображаются данные для выбранной классификации, включая имя источника, тип источника, идентификатор подписки и количество классифицированных файлов и таблиц.      |
|**Обзор ресурсов**     |  Чтобы просмотреть активы, найденные с определенной классификацией или источником, выберите классификацию или источник в зависимости от просматриваемого отчета, а затем выберите **Обзор ресурсов** :::image type="icon" source="media/insights/ico-browse-assets.png" border="false"::: над фильтрами. <br><br>В результатах поиска отображаются все классифицированные активы, найденные для выбранного фильтра.  Дополнительные сведения см. [в статье Поиск в каталоге данных Azure зрения](how-to-search-catalog.md).       |
| | |

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения об отчетах Azure зрения Insights
> [!div class="nextstepaction"]
> [Глоссарий](glossary-insights.md)

> [!div class="nextstepaction"]
> [Просмотр аналитических сведений](scan-insights.md)

> [!div class="nextstepaction"]
> [Чувствительность меток к аналитике](./sensitivity-insights.md)

> [!div class="nextstepaction"]
> [Аналитика расширений файлов](file-extension-insights.md)