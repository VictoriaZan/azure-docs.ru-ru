---
title: SQL Server SQL Управляемый экземпляр — анализ производительности
description: Узнайте, как создавать и сравнивать базовые показатели производительности при переносе баз данных SQL Server в Azure SQL Управляемый экземпляр.
ms.service: sql-database
ms.subservice: migration
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: mokabiru
ms.date: 11/06/2020
ms.openlocfilehash: c47e4c1278f222feac35a2c6ab0b067c916c0217
ms.sourcegitcommit: 4295037553d1e407edeb719a3699f0567ebf4293
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/30/2020
ms.locfileid: "96326851"
---
# <a name="migration-performance-sql-server-to-sql-managed-instance-performance-analysis"></a>Производительность миграции: SQL Server в анализ производительности SQL Управляемый экземпляр
[!INCLUDE[appliesto-sqldb-sqlmi](../../includes/appliesto-sqlmi.md)]

Создайте базовый уровень производительности, чтобы сравнить производительность рабочей нагрузки на Управляемый экземпляр SQL с исходной рабочей нагрузкой, работающей на SQL Server. 

## <a name="create-a-baseline"></a>Создание базового плана

В идеале производительность аналогична или лучше после миграции, поэтому важно измерять и записывать базовые значения производительности в источнике, а затем сравнивать их с целевой средой. Базовый уровень производительности — это набор параметров, определяющих среднюю рабочую нагрузку на источник. 

Выберите набор запросов, которые важны для и бизнес-задачи. Оцените и задокументируйте минимальные и средние значения и максимальную продолжительность использования ЦП для этих запросов, а также метрики производительности на исходном сервере, такие как средняя и максимальная загрузка ЦП, средняя задержка и максимальное значение задержки ввода-вывода на диск, пропускная способность, число операций ввода-вывода, максимальное ожидаемое время жизни страницы и средний максимальный размер базы данных tempdb. 

Следующие ресурсы помогут определить базовые показатели производительности. 

   - [Мониторинг использования ЦП ](https://techcommunity.microsoft.com/t5/azure-sql-database/monitor-cpu-usage-on-sql-server-and-azure-sql/ba-p/680777#M131)
   - [Мониторинг использования памяти](/sql/relational-databases/performance-monitor/monitor-memory-usage)   и определите объем памяти, используемый различными компонентами, такими как буферный пул, кэш планов, пул хранилища столбцов, выполняющаяся [в памяти OLTP](/sql/relational-databases/in-memory-oltp/monitor-and-troubleshoot-memory-usage)и т. д. Кроме того, вы должны найти средние и пиковые значения счетчика производительности "ожидаемая память страницы". 
   - Отслеживайте использование дискового ввода-вывода в экземпляре SQL Server источника с помощью представления [sys.dm_io_virtual_file_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-io-virtual-file-stats-transact-sql)   или [счетчиков производительности](/sql/relational-databases/performance-monitor/monitor-disk-usage). 
   - Отслеживайте производительность рабочей нагрузки и запросов, изучая динамические административные представления (или хранилище запросов при миграции с SQL Server 2016 и более поздних версий). Выявление средней длительности и загрузки ЦП наиболее важных запросов в рабочей нагрузке. 

Прежде чем выполнять миграцию, необходимо устранить все проблемы с производительностью исходного SQL Server. Миграция известных проблем в любую новую систему может привести к непредвиденным результатам и сделает недействительным сравнение производительности. 


## <a name="compare-performance"></a>Сравнить производительность 

После определения базовых показателей Сравните аналогичную производительность рабочей нагрузки на целевом Управляемый экземпляр SQL. Для точности важно, чтобы среда SQL Управляемый экземпляр была сравнима с SQL Serverной средой насколько это возможно. 

Существуют различия в инфраструктуре Управляемый экземпляр SQL, что делает согласованную производительность точно маловероятной. Некоторые запросы могут выполняться быстрее, чем ожидалось, а другие могут быть медленнее. Цель этого сравнения заключается в том, чтобы убедиться, что производительность рабочей нагрузки в управляемом экземпляре соответствует производительности SQL Server (в среднем) и определить критически важные запросы с производительностью, не соответствующей исходной производительности. 

Сравнение производительности, скорее всего, приведет к следующим результатам: 

- Производительность рабочей нагрузки на управляемом экземпляре выстраивается или повышается по сравнению с производительностью рабочей нагрузки на исходном SQL Server. В этом случае вы успешно подтвердили успешное выполнение миграции. 

- Большинство параметров производительности и запросов в рабочей нагрузке выполняются должным образом с некоторыми исключениями, приводящими к снижению производительности. В этом случае выявление различий и их важности. Если имеются важные запросы с низкой производительностью, выясните, изменились ли базовые планы SQL, или попадает ли запрос на достижение ограничений ресурсов. Это можно устранить, применив некоторые указания к критическим запросам (например, изменить уровень совместимости, механизм оценки количества элементов прежних версий) либо напрямую, либо используя структуры планов. Убедитесь, что статистика и индексы актуальны и эквивалентны в обеих средах. 

- Большинство запросов выполняются медленнее на управляемом экземпляре по сравнению с исходным экземпляром SQL Server. В этом случае попробуйте найти основные причины разницы, например [достижение некоторого предела ресурсов](../../managed-instance/resource-limits.md#service-tier-characteristics) , например ограничение скорости операций ввода-вывода, памяти или экземпляра. Если ограничения ресурсов не вызывают разницы, попробуйте изменить уровень совместимости базы данных или измените параметры базы данных, например оценку устаревшей кратности и повторно запустите тест. Ознакомьтесь с рекомендациями, предоставляемыми представлениями управляемого экземпляра или хранилища запросов, чтобы найти запросы с повышенной производительностью. 

SQL Управляемый экземпляр имеет встроенную функцию автоматического исправления плана, включенную по умолчанию. Эта функция гарантирует, что запросы, которые работали в прошлом, не понижаться в будущем. Если эта функция не включена, запустите рабочую нагрузку со старыми параметрами, чтобы SQL Управляемый экземпляр мог изучить базовые показатели производительности. Затем включите эту функцию и снова запустите рабочую нагрузку с новыми параметрами. 

Внесите изменения в параметры теста или обновите их до более высоких уровней служб, чтобы достичь оптимальной конфигурации для производительности рабочей нагрузки, удовлетворяющей вашим потребностям. 

## <a name="monitor-performance"></a>Мониторинг производительности 

SQL Управляемый экземпляр предоставляет расширенные средства для мониторинга и устранения неполадок, и их следует использовать для наблюдения за производительностью экземпляра. Ниже приведены некоторые основные метрики для мониторинга. 

- Использование ЦП на экземпляре, чтобы определить, соответствует ли подготовленное количество виртуальных ядер для вашей рабочей нагрузки. 
- Ожидаемый срок жизни страницы на управляемом экземпляре, чтобы определить, требуется ли [Дополнительная память](https://techcommunity.microsoft.com/t5/azure-sql-database/do-you-need-more-memory-on-azure-sql-managed-instance/ba-p/563444).
-  Статистические данные, такие как INSTANCE_LOG_GOVERNOR или ПАЖЕИОЛАТЧ, которые указывают на проблемы ввода-вывода хранилища, особенно на уровне общего назначения, где может потребоваться предварительное выделение файлов для повышения производительности ввода-вывода. 


## <a name="considerations"></a>Рекомендации  

При сравнении производительности учитывайте следующее. 

- Соответствие параметров источнику и целевому объекту. Проверьте, что различные параметры экземпляра, базы данных и tempdb эквивалентны двум средам. Различия в конфигурации, уровнях совместимости, параметрах шифрования, флагах трассировки и т. д. могут привести к снижению производительности. 

- Хранилище настраивается в соответствии [с рекомендациями.](https://techcommunity.microsoft.com/t5/datacat/storage-performance-best-practices-and-considerations-for-azure/ba-p/305525) Например, для общего назначения может потребоваться предварительно выделить размер файлов, чтобы повысить производительность. 

- Существуют [различия в основной среде](https://azure.microsoft.com/blog/key-causes-of-performance-differences-between-sql-managed-instance-and-sql-server/) , которые могут вызвать различия в производительности между управляемым экземпляром и SQL Server. Выявление рисков, относящихся к вашей среде, которые могут повлиять на производительность. 

- Хранилище запросов и автоматическая настройка должны быть включены на Управляемый экземпляр SQL, так как они помогают измерять производительность рабочей нагрузки и автоматически устраняют потенциальные проблемы с производительностью. 



## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о оптимизации новой среды Azure SQL Управляемый экземпляр см. в следующих ресурсах: 

- [Как узнать, почему производительность рабочей нагрузки Управляемый экземпляр Azure SQL отличается от SQL Server?](https://medium.com/azure-sqldb-managed-instance/what-to-do-when-azure-sql-managed-instance-is-slower-than-sql-server-dd39942aaadd)
- [Основные причины различий в производительности между SQL Управляемый экземпляр и SQL Server](https://azure.microsoft.com/blog/key-causes-of-performance-differences-between-sql-managed-instance-and-sql-server/)
- [Рекомендации по повышению производительности хранилища для Управляемый экземпляр Azure SQL (общего назначения)](https://techcommunity.microsoft.com/t5/datacat/storage-performance-best-practices-and-considerations-for-azure/ba-p/305525)
- [Наблюдение за производительностью в режиме реального времени для Управляемый экземпляр Azure SQL (Это архивная цель?)](/archive/blogs/sqlcat/real-time-performance-monitoring-for-azure-sql-database-managed-instance)