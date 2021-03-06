---
title: Обновление операционной системы для SAP HANA в Azure (крупные экземпляры) | Документация Майкрософт
description: Выполнение обновления операционной системы для SAP HANA в Azure (крупные экземпляры).
services: virtual-machines-linux
documentationcenter: ''
author: saghorpa
manager: juergent
editor: ''
ms.service: virtual-machines-linux
ms.subservice: workloads
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/04/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: cdc6dd49fe98085edf3c6fb16606b9f540b5a3a0
ms.sourcegitcommit: 4c89d9ea4b834d1963c4818a965eaaaa288194eb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/04/2020
ms.locfileid: "96608696"
---
# <a name="operating-system-upgrade"></a>Обновление операционной системы
В этом документе описывается обновление операционной системы в HANA (крупные экземпляры).

>[!NOTE]
>Обновление операционной системы — это ответственность клиента, служба поддержки Microsoft Operations может помочь вам в изходе обновления. Перед планированием обновления необходимо обратиться к поставщику операционной системы.

> [!NOTE]
> Эта статья содержит ссылки на термин « *черный* список», термин, который корпорация Майкрософт больше не использует. При удалении термина из программного обеспечения мы удалим его из этой статьи.

Во время подготовки модульной подсистемы Microsoft Operations ХЛИ устанавливает операционную систему.
Со временем вам потребуется обслуживать операционную систему (пример: установка исправлений, настройка, обновление и т. д.) на модуле HLI.

Перед внесением значительных изменений в операционную систему (например, обновление с пакетом обновления 1 (SP1) до пакета обновления 2 (SP2)) необходимо обратиться в службу Microsoft Operations, открыв запрос в службу поддержки.

Добавьте в свой билет:

* Идентификатор подписки HLI.
* Имя вашего сервера.
* Уровень исправления, который необходимо применить.
* Дата планирования этого изменения. 

Мы рекомендуем открыть этот билет по крайней мере на одну неделю до наступления требуемого обновления, что позволит команде применения узнать о нужной версии встроенного по.

Сведения о совместимости другой версии SAP HANA с другими версиями Linux см. в [примечании SAP № 2235581](https://launchpad.support.sap.com/#/notes/2235581).

## <a name="known-issues"></a>Известные проблемы

Ниже приведены некоторые распространенные известные проблемы, возникающие во время обновления.
- Для категории номеров SKU класса II после установки новой версии операционной системы удаляется Software Foundation Server (SFS). После обновления ОС необходимо переустановить совместимый СФС.
- Драйверы карт Ethernet (ENIC и FNIC) откатываются до предыдущей версии. После обновления необходимо переустановить совместимую версию драйверов.

## <a name="sap-hana-large-instance-type-i-recommended-configuration"></a>Рекомендуемая конфигурация SAP HANA крупного экземпляра (тип I)

Конфигурация операционной системы может отменять Рекомендуемые параметры с течением времени из-за исправлений, обновлений системы и изменений, внесенных клиентами. Кроме того, корпорация Майкрософт выявляет обновления, необходимые для существующих систем, чтобы обеспечить оптимальную конфигурацию для оптимальной производительности и устойчивости. Следующие инструкции описывают рекомендации, которые устраняют производительность сети, стабильность системы и оптимальную производительность HANA.

### <a name="compatible-enicfnic-driver-versions"></a>Совместимые версии драйверов ЕНИК/Фник
  Чтобы обеспечить правильную производительность сети и стабильность системы, рекомендуется убедиться, что соответствующая версия драйверов ЕНИК и Фник для конкретной операционной системы установлена, как показано в следующей таблице совместимости. Серверы доставляются клиентам с совместимыми версиями. В некоторых случаях при установке исправлений для ОС и ядра драйверы можно откатить к версии драйвера по умолчанию. Убедитесь, что соответствующая версия драйвера запущена после операций исправления операционной системы или ядра.
       
      
  |  Поставщик ОС    |  Версия пакета ОС     |  Версия встроенного ПО  |  Драйвер ЕНИК |  Драйвер Фник | 
  |---------------|-------------------------|--------------------|--------------|--------------|
  |   SuSE        |  SLES 12 SP2            |   3.1.3 h           |  2.3.0.40    |   1.6.0.34   |
  |   SuSE        |  SLES 12 с пакетом обновления 3 (SP3)            |   3.1.3 h           |  2.3.0.44    |   1.6.0.36   |
  |   SuSE        |  SLES 12 с пакетом обновления 4 (SP4)            |   3.2.3 i           |  4.0.0.6     |   2.0.0.60   |
  |   SuSE        |  SLES 12 SP2            |   3.2.3 i           |  2.3.0.45    |   1.6.0.37   |
  |   SuSE        |  SLES 12 с пакетом обновления 3 (SP3)            |   3.2.3 i           |  2.3.0.43    |   1.6.0.36   |
  |   SuSE        |  SLES 12 с пакетом обновления 5 (SP5)            |   3.2.3 i           |  4.0.0.8     |   2.0.0.60   |
  |   Red Hat     |  RHEL 7.2;               |   3.1.3 h           |  2.3.0.39    |   1.6.0.34   |
 

### <a name="commands-for-driver-upgrade-and-to-clean-old-rpm-packages"></a>Команды для обновления драйверов и очистки старых пакетов RPM

#### <a name="command-to-check-existing-installed-drivers"></a>Команда для проверки существующих установленных драйверов
```
rpm -qa | grep enic/fnic 
```
#### <a name="delete-existing-enicfnic-rpm"></a>Удаление существующих ЕНИК/Фник RPM
```
rpm -e <old-rpm-package>
```
#### <a name="install-the-recommended-enicfnic-driver-packages"></a>Установка рекомендуемых пакетов драйверов ЕНИК/Фник
```
rpm -ivh <enic/fnic.rpm> 
```

#### <a name="commands-to-confirm-the-installation"></a>Команды для подтверждения установки
```
modinfo enic
modinfo fnic
```

#### <a name="steps-for-enicfnic-drivers-installation-during-os-upgrade"></a>Шаги для установки драйверов ЕНИК/Фник во время обновления ОС

* Обновление версии ОС
* Удалить старые пакеты RPM
* Установить совместимые драйверы ЕНИК/Фник в соответствии с установленной версией ОС
* Перезагрузить систему
* После перезагрузки проверьте версию ЕНИК/Фник.


### <a name="suse-hlis-grub-update-failure"></a>Сбой обновления для SuSE Хлис GRUB
При обновлении крупные экземпляры SAP в Azure HANA (тип I) могут находиться в незагрузочном состоянии. Описанная ниже процедура решает эту проблему.
#### <a name="execution-steps"></a>Шаги выполнения


*   Выполнить `multipath -ll` команду.
*   Получите идентификатор LUN, размер которого равен приблизительно 50G, или используйте команду: `fdisk -l | grep mapper`
*   Обновить `/etc/default/grub_installdevice` файл строкой `/dev/mapper/<LUN ID>` . Пример:/dev/Mapper/3600a09803830372f483f495242534a56
>[!NOTE]
>ИДЕНТИФИКАТОР LUN отличается от сервера к серверу.


### <a name="disable-edac"></a>Отключить ЕДАК 
   Модуль обнаружения и исправления ошибок (ЕДАК) помогает обнаружить и исправить ошибки памяти. Однако базовое оборудование для SAP HANA на крупных экземплярах Azure (тип I) уже выполняет ту же функцию. Наличие той же функции на уровнях оборудования и операционной системы (ОС) может привести к конфликтам и может привести к случайным и незапланированным завершениям работы сервера. Поэтому рекомендуется отключить модуль от операционной системы.

#### <a name="execution-steps"></a>Шаги выполнения

* Проверьте, включен ли модуль ЕДАК. Если в приведенной ниже команде возвращается результат, это означает, что модуль включен. 
```
lsmod | grep -i edac 
```
* Отключите модули, добавив следующие строки в файл `/etc/modprobe.d/blacklist.conf`
```
blacklist sb_edac
blacklist edac_core
```
Чтобы внести изменения на месте, требуется перезагрузка. Выполните `lsmod` команду и убедитесь, что модуль отсутствует в выходных данных.

### <a name="kernel-parameters"></a>параметры ядра;
   Убедитесь, что применен правильный параметр `transparent_hugepage` для `numa_balancing` , `processor.max_cstate` , `ignore_ce` и `intel_idle.max_cstate` .

* intel_idle intel_idle.max_cstate = 1
* processor.max_cstate = 1
* transparent_hugepage = Never
* numa_balancing = Disable
* MCE = ignore_ce

#### <a name="execution-steps"></a>Шаги выполнения

* Добавьте эти параметры в `GRB_CMDLINE_LINUX` строку в файле `/etc/default/grub`
```
intel_idle.max_cstate=1 processor.max_cstate=1 transparent_hugepage=never numa_balancing=disable mce=ignore_ce
```
* Создайте новый файл GRUB.
```
grub2-mkconfig -o /boot/grub2/grub.cfg
```
* Перезагрузка системы.


## <a name="next-steps"></a>Дальнейшие действия
- Ознакомьтесь с [резервным копированием и восстановлением](hana-overview-high-availability-disaster-recovery.md) операционной системы для номеров SKU класса I.
- См. статью [резервное копирование ОС для номеров SKU типа II версий 3](os-backup-type-ii-skus.md) для класса SKU типа II.
