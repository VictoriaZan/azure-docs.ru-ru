---
title: Масштабирование ресурсов отдельной базы данных
description: В этой статье описано масштабирование вычислительных ресурсов и ресурсов хранилища, предоставляемых отдельной базе данных в Базе данных SQL Azure.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: sqldbrb=1, references_regions
ms.devlang: ''
ms.topic: conceptual
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.reviewer: sstein
ms.date: 09/16/2020
ms.openlocfilehash: da3c70baccc3c86f2ac57d61539456464e3042b6
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/02/2020
ms.locfileid: "96493412"
---
# <a name="scale-single-database-resources-in-azure-sql-database"></a>Масштабирование ресурсов отдельной базы данных в Базе данных SQL Azure

В этой статье описывается, как масштабировать ресурсы вычислений и хранения, доступные для базы данных SQL Azure, на подготовленном уровне вычислений. Кроме того, [уровень вычислений "бессерверный](serverless-tier-overview.md) " обеспечивает автоматическое масштабирование вычислений и счета за секунду для используемых вычислений.

После первоначального выбора количества виртуальных ядер или DTU можно динамически масштабировать отдельную базу данных в зависимости от фактического взаимодействия с помощью [портал Azure](single-database-manage.md#the-azure-portal), [Transact-SQL](/sql/t-sql/statements/alter-database-transact-sql#examples-1), [PowerShell](/powershell/module/az.sql/set-azsqldatabase), [Azure CLI](/cli/azure/sql/db#az-sql-db-update)или [REST API](/rest/api/sql/databases/update).

В следующем видео показано динамическое изменение уровня служб и объема вычислительных ресурсов для увеличения количества доступных единиц DTU отдельной базы данных.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-dynamically-scale-up-or-scale-down/player]

> [!IMPORTANT]
> Иногда требуется сжать базу данных, чтобы освободить неиспользуемое пространство. Дополнительные сведения см. [в статье Управление дисковым пространством в базе данных SQL Azure](file-space-manage.md).

## <a name="impact"></a>Влияние

Изменение уровня служб или размера вычислений в основном включает службу, выполняющую следующие действия:

1. Создайте новый вычислительный экземпляр для базы данных. 

    Будет создан новый вычислительный экземпляр с запрошенным уровнем служб и размером вычислений. Для некоторых сочетаний изменений уровня служб и размера вычислений реплика базы данных должна быть создана в новом вычислительном экземпляре, который включает копирование данных и может сильно повлиять на общую задержку. Независимо от этого, база данных остается в режиме «в сети» на этом шаге, а подключения по-прежнему направляются в базу данных в исходном вычислительном экземпляре.

2. Переключить маршрутизацию соединений на новый вычислительный экземпляр.

    Существующие соединения с базой данных в исходном вычислительном экземпляре удаляются. Все новые соединения устанавливаются для базы данных в новом вычислительном экземпляре. Для некоторых сочетаний изменений уровня служб и размера вычислений файлы базы данных отсоединяются и повторно присоединяются во время переключения.  Независимо от этого коммутатор может привести к кратковременному прерыванию работы службы, если база данных недоступна в обычном объеме менее 30 секунд и часто занимает всего несколько секунд. Если при отсоединении подключений выполняются длительные транзакции, длительность этого шага может занять больше времени для восстановления прерванных транзакций. [Ускоренное восстановление базы данных](../accelerated-database-recovery.md) может снизить последствия прерывания длительных транзакций.

> [!IMPORTANT]
> Во время какого-либо этапа рабочего процесса данные не теряются. Убедитесь, что вы реализовали некоторую [логику повторных попыток](troubleshoot-common-connectivity-issues.md) в приложениях и компонентах, которые используют базу данных SQL Azure при изменении уровня служб.

## <a name="latency"></a>Задержка

Расчетная задержка для изменения уровня служб, масштабирования размера вычислений отдельной базы данных или эластичного пула, перемещения базы данных в пул эластичных БД и из нее, а также для перемещения базы данных между пулами эластичных БД.

|Уровень служб|Базовая простая база данных,</br>Standard (S0-S1)|Базовый эластичный пул,</br>Standard (S2-S12), </br>общего назначения одной базы данных или эластичного пула|Premium или критически важный для бизнеса одна база данных или эластичный пул|Уровень "Гипермасштабирование"
|:---|:---|:---|:---|:---|
|**Базовая простая база данных, уровень " </br> Стандартный" (S0-S1)**|&bull;&nbsp;Постоянная задержка во времени независимо от используемого пространства</br>&bull;&nbsp;Обычно менее 5 минут|&bull;&nbsp;Задержка, пропорциональная объему базы данных, используемой из-за копирования данных</br>&bull;&nbsp;Обычно используется менее 1 минуты на ГБ пространства|&bull;&nbsp;Задержка, пропорциональная объему базы данных, используемой из-за копирования данных</br>&bull;&nbsp;Обычно используется менее 1 минуты на ГБ пространства|&bull;&nbsp;Задержка, пропорциональная объему базы данных, используемой из-за копирования данных</br>&bull;&nbsp;Обычно используется менее 1 минуты на ГБ пространства|
|**Базовый эластичный пул, </br> Standard (S2-S12), </br> общего назначения одна база данных или эластичный пул**|&bull;&nbsp;Задержка, пропорциональная объему базы данных, используемой из-за копирования данных</br>&bull;&nbsp;Обычно используется менее 1 минуты на ГБ пространства|&bull;&nbsp;Для отдельных баз данных, постоянная задержка во времени независимо от используемого пространства</br>&bull;&nbsp;Как правило, для отдельных баз данных менее 5 минут</br>&bull;&nbsp;Для эластичных пулов, пропорционально количеству баз данных|&bull;&nbsp;Задержка, пропорциональная объему базы данных, используемой из-за копирования данных</br>&bull;&nbsp;Обычно используется менее 1 минуты на ГБ пространства|&bull;&nbsp;Задержка, пропорциональная объему базы данных, используемой из-за копирования данных</br>&bull;&nbsp;Обычно используется менее 1 минуты на ГБ пространства|
|**Premium или критически важный для бизнеса одна база данных или эластичный пул**|&bull;&nbsp;Задержка, пропорциональная объему базы данных, используемой из-за копирования данных</br>&bull;&nbsp;Обычно используется менее 1 минуты на ГБ пространства|&bull;&nbsp;Задержка, пропорциональная объему базы данных, используемой из-за копирования данных</br>&bull;&nbsp;Обычно используется менее 1 минуты на ГБ пространства|&bull;&nbsp;Задержка, пропорциональная объему базы данных, используемой из-за копирования данных</br>&bull;&nbsp;Обычно используется менее 1 минуты на ГБ пространства|&bull;&nbsp;Задержка, пропорциональная объему базы данных, используемой из-за копирования данных</br>&bull;&nbsp;Обычно используется менее 1 минуты на ГБ пространства|
|**Уровень "Гипермасштабирование"**|Н/Д|Н/Д|Н/Д|&bull;&nbsp;Постоянная задержка во времени независимо от используемого пространства</br>&bull;&nbsp;Обычно менее 2 минут|

> [!NOTE]
> Кроме того, для баз данных Standard (S2-S12) и общего назначения, задержка перемещения базы данных в эластичный пул или между пулами эластичных пулов будет пропорционально размеру базы данных, если база данных использует хранилище привилегированного файлового ресурса ([PFS](../../storage/files/storage-files-introduction.md)).
>
> Чтобы определить, использует ли база данных PFS-хранилище, выполните следующий запрос в контексте базы данных. Если значение в столбце AccountType равно `PremiumFileStorage` или `PremiumFileStorage-ZRS` , то база данных использует хранилище PFS.
 
```sql
SELECT s.file_id,
       s.type_desc,
       s.name,
       FILEPROPERTYEX(s.name, 'AccountType') AS AccountType
FROM sys.database_files AS s
WHERE s.type_desc IN ('ROWS', 'LOG');
```

> [!TIP]
> Сведения о мониторинге выполняемых операций см. в статьях [Управление операциями с помощью REST API SQL](/rest/api/sql/operations/list), [Управление операциями с помощью интерфейса командной строки](/cli/azure/sql/db/op), [мониторинг операций с помощью T-SQL](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) и следующие две команды PowerShell: [Get-азсклдатабасеактивити](/powershell/module/az.sql/get-azsqldatabaseactivity) и [останавливают-азсклдатабасеактивити](/powershell/module/az.sql/stop-azsqldatabaseactivity).

## <a name="cancelling-changes"></a>Отмена изменений

Операция изменения или масштабирования уровня службы может быть отменена.

### <a name="the-azure-portal"></a>Портал Azure

В колонке обзор базы данных перейдите к разделу **уведомления** и щелкните плитку, указывающую на текущую операцию:

![Текущая операция](./media/single-database-scale/ongoing-operations.png)

Затем нажмите кнопку **отменить эту операцию**.

![Отменить текущую операцию](./media/single-database-scale/cancel-ongoing-operation.png)

### <a name="powershell"></a>PowerShell

В командной строке PowerShell задайте `$resourceGroupName` , `$serverName` и `$databaseName` , а затем выполните следующую команду:

```azurecli
$operationName = (az sql db op list --resource-group $resourceGroupName --server $serverName --database $databaseName --query "[?state=='InProgress'].name" --out tsv)
if (-not [string]::IsNullOrEmpty($operationName)) {
    (az sql db op cancel --resource-group $resourceGroupName --server $serverName --database $databaseName --name $operationName)
        "Operation " + $operationName + " has been canceled"
}
else {
    "No service tier change or compute rescaling operation found"
}
```

## <a name="additional-considerations"></a>Дополнительные сведения

- При обновлении до более высокого уровня служб или размера вычислений максимальный размер базы данных не увеличивается, если явно не задан больший размер (MAXSIZE).
- При понижении уровня базы данных используемое ею пространство не должно превышать максимальный допустимый размер целевых уровня служб и объема вычислительных ресурсов.
- При понижении уровня служб **Премиум** до уровня **Стандартный** дополнительная плата взимается в следующих случаях: 1) максимальный размер базы данных поддерживается в целевом объеме вычислительных ресурсов; 2) максимальный размер превышает включенный объем хранилища целевого объема вычислительных ресурсов. Например, если база данных P1 с максимальным размером 500 ГБ — понижается S3, то будет применяться дополнительное хранилище, так как S3 поддерживает максимальный размер в 1 ТБ, а Включенный объем хранилища — только 250 ГБ. Поэтому дополнительный объем хранилища равен 500 – 250 = 250 ГБ. Дополнительные сведения о ценах на дополнительную память см. на странице [цен на базу данных SQL Azure](https://azure.microsoft.com/pricing/details/sql-database/). Если фактический объем пространства, которое используется, меньше, чем включенный объем хранилища, то этих дополнительных затрат можно избежать, уменьшив максимальный размер базы данных до включенного объема.
- Если вы повышаете уровень базы данных со включенной [георепликацией](active-geo-replication-configure-portal.md), перед повышением уровня базы данных-источника сначала обновите базы данных-получатели до требуемого уровня служб и объема вычислительных ресурсов (общая рекомендация для оптимальной производительности). При обновлении до другого выпуска требуется, чтобы база данных-получатель обновлялась первыми.
- Если вы понижаете уровень базы данных со включенной [георепликацией](active-geo-replication-configure-portal.md), перед понижением уровня базы данных-получателя сначала понизьте категорию ее базы данных-источника до требуемого уровня служб и объема вычислительных ресурсов (общая рекомендация для оптимальной производительности). При переходе на другой выпуск необходимо сначала понизить уровень базы данных-источника.
- Предложения службы восстановления отличаются для разных уровней служб. При переходе на уровень " **базовый** " вы можете уменьшить срок хранения резервных копий. Ознакомьтесь со статьей [Подробнее об автоматически создаваемых резервных копиях в Базе данных SQL](automated-backups-overview.md).
- Новые свойства базы данных не применяются до тех пор, пока изменения не будут выполнены.

## <a name="billing"></a>Выставление счетов

Плата взимается за каждый час существования базы данных с максимальным размером служб и вычислений, которые были применены в течение этого часа, независимо от использования или активности базы данных менее часа. Например, если вы создадите отдельную базу данных и через 5 минут удалите ее, вам будет выставлен счет за 1 час использования базы данных.

## <a name="change-storage-size"></a>Изменение размера хранилища

### <a name="vcore-based-purchasing-model"></a>Модель приобретения на основе виртуальных ядер

- Хранилище можно подготовить к максимальному размеру хранилища данных с шагом в 1 ГБ. Минимальное настраиваемое хранилище данных — 1 ГБ. См. раздел Документация по ограничению ресурсов для [отдельных баз данных](resource-limits-vcore-single-databases.md) и [эластичных пулов](resource-limits-vcore-elastic-pools.md) для ограничения максимального размера хранилища данных в каждой цели обслуживания.
- Хранилище данных для отдельной базы данных может быть подготовлено путем увеличения или уменьшения максимального размера с помощью [портал Azure](https://portal.azure.com), [Transact-SQL](/sql/t-sql/statements/alter-database-transact-sql#examples-1), [PowerShell](/powershell/module/az.sql/set-azsqldatabase), [Azure CLI](/cli/azure/sql/db#az-sql-db-update)или [REST API](/rest/api/sql/databases/update). Если значение максимального размера указано в байтах, оно должно быть кратно 1 ГБ (1073741824 байт).
- Объем данных, которые могут храниться в файлах данных базы данных, ограничен максимальным размером хранилища данных. Помимо этого хранилища, база данных SQL Azure автоматически выделяет 30% больше места для хранения журнала транзакций.
- База данных SQL Azure автоматически выделяет 32 ГБ на виртуальное ядро для `tempdb` базы данных. `tempdb` находится в локальном хранилище SSD на всех уровнях служб.
- Стоимость хранилища для отдельной базы данных или эластичного пула — это сумма хранилища данных и объемов хранилища журнала транзакций, умноженная на цену единицы хранения уровня службы. Стоимость `tempdb` включается в цену. Дополнительные сведения о ценах на хранение см. на странице [цен на базу данных SQL Azure](https://azure.microsoft.com/pricing/details/sql-database/).

> [!IMPORTANT]
> Иногда требуется сжать базу данных, чтобы освободить неиспользуемое пространство. Дополнительные сведения см. [в статье Управление дисковым пространством в базе данных SQL Azure](file-space-manage.md).

### <a name="dtu-based-purchasing-model"></a>Модель приобретения на основе единиц DTU

- Цена DTU для отдельной базы данных включает в себя определенный объем хранилища, не требующий дополнительной платы. Дополнительный объем хранилища, сверх включенного, можно подготовить за дополнительную плату в пределах максимального допустимого размера с шагом в 250 ГБ при объеме хранилища до 1 ТБ и с шагом в 256 ГБ — при объеме более 1 ТБ. Сведения о включенном объеме хранилища и ограничениях максимального размера см. в разделе [Отдельная база данных: размеры хранилища и объемы вычислительных ресурсов](resource-limits-dtu-single-databases.md#single-database-storage-sizes-and-compute-sizes).
- Дополнительные хранилища для одной базы данных можно подготовить, увеличив ее максимальный размер с помощью портал Azure, [Transact-SQL](/sql/t-sql/statements/alter-database-transact-sql#examples-1), [PowerShell](/powershell/module/az.sql/set-azsqldatabase), [Azure CLI](/cli/azure/sql/db#az-sql-db-update)или [REST API](/rest/api/sql/databases/update).
- Стоимость дополнительного хранилища для отдельной базы данных равна объему хранилища, умноженному на цену единицы хранения этого хранилища для уровня служб. Дополнительные сведения о цене дополнительного хранилища см. на странице [цен на базу данных SQL Azure](https://azure.microsoft.com/pricing/details/sql-database/).

> [!IMPORTANT]
> Иногда требуется сжать базу данных, чтобы освободить неиспользуемое пространство. Дополнительные сведения см. [в статье Управление дисковым пространством в базе данных SQL Azure](file-space-manage.md).

### <a name="geo-replicated-database"></a>Геореплицированная база данных

Чтобы изменить размер базы данных реплицированной базы данных-получателя, измените размер базы данных-источника. Затем это изменение будет реплицировано и реализовано также в базе данных-получателе.

## <a name="p11-and-p15-constraints-when-max-size-greater-than-1-tb"></a>Ограничения P11 и P15, если максимальный размер превышает 1 ТБ

Более 1 ТБ хранилища на уровне "Премиум" в настоящее время доступны во всех регионах, кроме: Восточный Китай, Северный Китай, Центрально-Центральный регион и Северо-Восточная Германия. В этих регионах максимальный объем хранилища категории "Премиум" ограничен 1 ТБ. Ниже приведены рекомендации и ограничения для баз данных P11 и P15 с максимальным размером, превышающим 1 ТБ.

- Если в качестве максимального размера для базы данных P11 или p15 было задано значение больше 1 ТБ, то его можно восстановить или скопировать в базу данных P11 или P15.  Впоследствии база данных может масштабироваться до другого размера вычислений, при условии, что объем пространства, выделяемого во время операции перемасштабирования, не превышает максимальный размер нового вычислений.
- Для сценариев активной георепликации:
  - Настройка отношения георепликации. Если база данных-источник — P11 или P15, то вторичная (ий) также должна иметь значение P11 или P15. Меньший размер вычислений отклоняется как вторичные, так как они не поддерживают более 1 ТБ.
  - Обновление базы данных-источника в отношениях георепликации. При изменении максимального размера и указании значения больше 1 ТБ в базе данных-источнике такие же изменения произойдут в базе данных-получателе. Чтобы изменения в базе данных-источнике вступили в силу, оба обновления должны быть успешными. При этом применяются ограничения по регионам для максимального размера больше 1 ТБ. Если база данных-получатель находится в регионе, который не поддерживает более 1 ТБ, первичный сервер не обновляется.
- Использование службы импорта и экспорта для загрузки баз данных P11/p15 с более чем 1 ТБ не поддерживается. Для [импорта](database-import.md) и [экспорта](database-export.md) данных используйте файл SqlPackage.exe.

## <a name="next-steps"></a>Дальнейшие действия

Общие ограничения ресурсов см. в статье [ограничения ресурсов на основе виртуальное ядро базы данных SQL Azure — отдельные базы данных](resource-limits-vcore-single-databases.md) и [ограничения ресурсов на основе DTU базы данных SQL Azure — отдельные базы данных](resource-limits-dtu-single-databases.md).
