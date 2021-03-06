---
title: Преимущества и функции Azure Defender для SQL
description: Сведения о преимуществах и функциях Azure Defender для SQL.
author: memildin
ms.author: memildin
ms.date: 11/22/2020
ms.topic: overview
ms.service: security-center
ms.custom: references_regions
manager: rkarlin
ms.openlocfilehash: bb24c04681b142aaa1c80738090afe2a13949495
ms.sourcegitcommit: c95e2d89a5a3cf5e2983ffcc206f056a7992df7d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/24/2020
ms.locfileid: "96014552"
---
# <a name="introduction-to-azure-defender-for-sql"></a>Общие сведения об Azure Defender для SQL

Azure Defender для SQL содержит два плана Azure Defender, которые расширяют [пакет безопасности данных](../azure-sql/database/azure-defender-for-sql.md) Центра безопасности Azure и позволяют защитить сами базы данных и размешенные в них данные в любом расположении. 

## <a name="availability"></a>Доступность

|Аспект|Сведения|
|----|:----|
|Состояние выпуска:|**Azure Defender для серверов баз данных SQL Azure** — общедоступная версия<br>**Azure Defender для серверов SQL на компьютерах** — предварительная версия<br>[!INCLUDE [Legalese](../../includes/security-center-preview-legal-text.md)] |
|Цены|Для двух планов, которые входят в предложение **Azure Defender для SQL**, плата взимается по тарифам, приведенным на [странице с ценами](security-center-pricing.md).|
|Защищаемые версии SQL|SQL на виртуальных машинах Azure ([Windows](../azure-sql/virtual-machines/windows/sql-server-on-azure-vm-iaas-what-is-overview.md) и [Linux](../azure-sql/virtual-machines/linux/sql-server-on-linux-vm-what-is-iaas-overview.md))<br>[Серверы SQL с поддержкой Arc](https://docs.microsoft.com/sql/sql-server/azure-arc/overview) (включая локальные серверы SQL)<br>[Отдельные базы данных](../azure-sql/database/single-database-overview.md) и [эластичные пулы](../azure-sql/database/elastic-pool-overview.md) Azure SQL<br>[Управляемый экземпляр SQL Azure](../azure-sql/managed-instance/sql-managed-instance-paas-overview.md)<br>[Выделенный пул SQL в Azure Synapse Analytics (ранее — Хранилище данных SQL)](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md#dedicated-sql-pool-in-azure-synapse)|
|Облако.|![Да](./media/icons/yes-icon.png) Коммерческие облака<br>![Да](./media/icons/yes-icon.png) US Gov<br>![Нет](./media/icons/no-icon.png) China Gov и другие правительственные облака|
|||

## <a name="what-does-azure-defender-for-sql-protect"></a>На что распространяется защита Azure Defender для SQL?

**Azure Defender для SQL** состоит из двух отдельных планов Azure Defender.

- **Azure Defender для серверов баз данных SQL Azure** защищает следующие компоненты:
  - [База данных SQL Azure](../azure-sql/database/sql-database-paas-overview.md)
  - [Управляемый экземпляр SQL Azure](../azure-sql/managed-instance/sql-managed-instance-paas-overview.md)
  - [Выделенный пул SQL в Azure Synapse](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md#dedicated-sql-pool-in-azure-synapse)

- **Azure Defender для серверов SQL на компьютерах (предварительная версия)** расширяет возможности защиты для собственных серверов SQL Azure, предоставляя полную поддержку гибридных сред, то есть обеспечивает защиту серверов SQL (все поддерживаемые версии) не только в Azure, но и в других облачных средах и даже на локальных компьютерах.


## <a name="what-are-the-benefits-of-azure-defender-for-sql"></a>Каковы преимущества Azure Defender для SQL?

Два предлагаемых плана включают возможности обнаружения и устранения потенциальных уязвимостей базы данных и обнаружения аномальных действий, которые могут указывать на угрозу для вашей базы данных.

- [Оценка уязвимостей](../azure-sql/database/sql-vulnerability-assessment.md) — служба проверки, которая поможет обнаруживать, отслеживать и устранять потенциальные уязвимости базы данных. Оценочная проверка предоставит общие сведения о состоянии безопасности для компьютеров SQL и подробные сведения о результатах проверки безопасности.

- [Расширенная защита от угроз](../azure-sql/database/threat-detection-overview.md) — служба обнаружения, которая постоянно проверяет наличие на серверах SQL таких угроз, как внедрение кода SQL, атаки методом перебора и злоупотребление привилегиями. Эта служба передает в Центр безопасности Azure оповещения безопасности, которые содержат описания подозрительных действий, рекомендации по устранению этих угроз и варианты для продолжения исследований с помощью Azure Sentinel.


## <a name="what-kind-of-alerts-does-azure-defender-for-sql-provide"></a>Какие типы оповещений предоставляет Azure Defender для SQL?

Оповещения системы безопасности активируются, если возникают такие события:

- **Потенциальные атаки путем внедрения кода SQL**, включая обнаружения уязвимостей, связанные с созданием приложениями ошибочных инструкций SQL в базе данных.
- **Аномальные шаблоны доступа и запросов к базе данных**, например необычно высокая доля неудачных попыток входа с разными учетными данными (как при атаках методом перебора).
- **Подозрительная активность в базе данных**, например изменения в целевом хранилище экспорта для операции импорта и экспорта SQL.

Оповещения содержат сведения об инциденте, который привел к их активации, а также рекомендации по определению причины угроз и их устранению.



## <a name="next-steps"></a>Дальнейшие действия

Из этой статьи вы узнали об Azure Defender для SQL.

Связанные материалы см. в следующих статьях: 

- [Как включить Azure Defender для серверов SQL на компьютерах](defender-for-sql-usage.md)
- [Как включить Azure Defender для серверов баз данных SQL](../azure-sql/database/azure-defender-for-sql.md)
- [Список оповещений Azure Defender для SQL](alerts-reference.md#alerts-sql-db-and-warehouse)