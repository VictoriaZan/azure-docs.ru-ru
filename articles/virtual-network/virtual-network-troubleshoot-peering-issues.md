---
title: Устранение неполадок с пирингом виртуальной сети
description: Действия по устранению большинства проблем пиринга между виртуальными сетями.
services: virtual-network
documentationcenter: na
author: v-miegge
manager: dcscontentpm
editor: ''
tags: virtual-network
ms.assetid: 1a3d1e84-f793-41b4-aa04-774a7e8f7719
ms.service: virtual-network
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/28/2019
ms.author: kaushika
ms.openlocfilehash: 9685c1739a00788a974c200ddabb8cc975696b62
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "83587737"
---
# <a name="troubleshoot-virtual-network-peering-issues"></a>Устранение неполадок с пирингом виртуальной сети

В этом руководстве по устранению неполадок описаны действия, которые помогут устранить большинство проблем [пиринга между виртуальными сетями](virtual-network-peering-overview.md).

![Схема пиринга между виртуальными сетями](./media/virtual-network-troubleshoot-peering-issues/4489538_en_1.png)

## <a name="configure-virtual-network-peering-between-two-virtual-networks"></a>Настройка пиринга между двумя виртуальными сетями

Настройка зависит от того, принадлежат ли виртуальные сети к одной или разным подпискам.

### <a name="the-virtual-networks-are-in-the-same-subscription"></a>Виртуальные сети с одной подпиской

Чтобы настроить пиринг между виртуальными сетями, принадлежащие к одной подписке, используйте методы, описанные в следующих статьях.

* Если виртуальные сети находятся в *одном регионе*, см. раздел [Создание пиринга](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering#create-a-peering).
* Если виртуальные сети находятся в *различных регионах*, см. статью [Пиринг между виртуальными сетями](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview). 

> [!Note]
> Подключение с использованием глобального пиринга между виртуальными сетями не работает для следующих ресурсов: 
>
> * виртуальные машины (ВМ) за внутренней подсистемой балансировки нагрузки со SKU "Базовый";
> * Redis Cache (использует внутренняя подсистема балансировки нагрузки внутренний со SKU "Базовый");
> * шлюз приложений (использует внутренняя подсистема балансировки нагрузки внутренний со SKU "Базовый");
> * масштабируемый набор виртуальных машин (использует внутренняя подсистема балансировки нагрузки внутренний со SKU "Базовый");
> * кластеры Azure Service Fabric (использует внутренняя подсистема балансировки нагрузки внутренний со SKU "Базовый");
> * группы доступности AlwaysOn для SQL Server (использует внутренняя подсистема балансировки нагрузки внутренний со SKU "Базовый");
> * Среда службы приложений Azure для PowerApps (использует внутренняя подсистема балансировки нагрузки внутренний со SKU "Базовый");
> * Управление API Azure (использует внутренняя подсистема балансировки нагрузки внутренний со SKU "Базовый");
> * Доменные службы Azure Active Directory (AAD DS) (использует внутренняя подсистема балансировки нагрузки внутренний со SKU "Базовый");

Дополнительные сведения см. в разделе о [требованиях и ограничениях](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview#requirements-and-constraints) для глобального пиринга.

### <a name="the-virtual-networks-are-in-different-subscriptions-or-active-directory-tenants"></a>Виртуальные сети с разными подписками или клиентами Active Directory

Сведения о настройке пиринга между виртуальными сетями с разными подписками или клиентами Active Directory см. в разделе [Создание пиринга с помощью Azure CLI](https://docs.microsoft.com/azure/virtual-network/create-peering-different-subscriptions#cli).

> [!Note]
> Чтобы настроить пиринг между сетями, необходимы разрешения **Участник сетей** в обеих подписках. Дополнительные сведения см. в разделе о [разрешениях для пиринга](virtual-network-manage-peering.md#permissions).

## <a name="configure-virtual-network-peering-with-hub-spoke-topology-that-uses-on-premises-resources"></a>Настройка пиринга между виртуальными сетями со звездообразной топологией, использующими локальные ресурсы

![Схема пиринга между виртуальными сетями с локальным периферийными ресурсами](./media/virtual-network-troubleshoot-peering-issues/4488712_en_1a.png)

### <a name="for-a-site-to-site-connection-or-an-expressroute-connection"></a>Для подключений "сеть — сеть" или ExpressRoute

Выполните инструкции в статье [Настройка транзита VPN-шлюзов для включения пиринга между виртуальными сетями](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-peering-gateway-transit?toc=/azure/virtual-network/toc.json).

### <a name="for-point-to-site-connections"></a>Для подключений "точка — сеть"

1. Выполните инструкции в статье [Настройка транзита VPN-шлюзов для включения пиринга между виртуальными сетями](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-peering-gateway-transit?toc=/azure/virtual-network/toc.json).
2. После установки или изменения пиринга между виртуальными сетями скачайте и переустановите пакет "точка — сеть", чтобы клиенты "точка — сеть" могли получать обновленные маршруты к периферийной виртуальной сети.

## <a name="configure-virtual-network-peering-with-hub-spoke-topology-virtual-network"></a>Настройка пиринга между виртуальными сетями со звездообразной топологией

![Схема пиринга между виртуальными сетями на периферии](./media/virtual-network-troubleshoot-peering-issues/4488712_en_1b.png)

### <a name="the-virtual-networks-are-in-the-same-region"></a>Виртуальные сети в одном регионе


1. В центральной виртуальной сети настройте сетевой виртуальный модуль (NVA).
1. В периферийных виртуальных сетях примените определяемые пользователем маршруты с типом следующего прыжка "сетевой виртуальный модуль".

Дополнительные сведения см. в разделе [Цепочка служб](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview#service-chaining).

> [!Note]
> Если вам нужна помощь по настройке сетевого виртуального модуля, [обратитесь к его поставщику](https://support.microsoft.com/help/2984655/support-for-azure-market-place-for-virtual-machines).

Дополнительные сведения об устранении неполадок при настройке и маршрутизации устройств NVA см. в статье [Неполадки сетевого виртуального модуля в Azure](https://docs.microsoft.com/azure/virtual-network/virtual-network-troubleshoot-nva).

### <a name="the-virtual-networks-are-in-different-regions"></a>Виртуальные сети в разных регионах

Теперь поддерживается транзитное подключение с использованием глобального пиринга между виртуальными сетями. Подключение с использованием глобального пиринга между виртуальными сетями не работает для следующих ресурсов:

* Виртуальные машины за внутренним балансировщиком нагрузки SKU "Базовый"
* Redis Cache (использует внутренняя подсистема балансировки нагрузки внутренний со SKU "Базовый");
* шлюз приложений (использует внутренняя подсистема балансировки нагрузки внутренний со SKU "Базовый");
* Масштабируемые наборы (использует внутренняя подсистема балансировки нагрузки внутренний со SKU "Базовый");
* Кластеры Service Fabric (используют внутренний балансировщик нагрузки SKU "Базовый")
* группы доступности AlwaysOn для SQL Server (использует внутренняя подсистема балансировки нагрузки внутренний со SKU "Базовый");
* Среды службы приложений (ASE) (использует внутренняя подсистема балансировки нагрузки внутренний со SKU "Базовый");
* Управление API (использует внутренний балансировщик нагрузки SKU "Базовый")
* Azure AD DS (использует внутренняя подсистема балансировки нагрузки внутренний со SKU "Базовый").

Дополнительные сведения о требованиях и ограничениях для глобального пиринга между виртуальными сетями см. в [этом разделе](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview#requirements-and-constraints).

## <a name="troubleshoot-a-connectivity-issue-between-two-peered-virtual-networks"></a>Устранение проблем с подключением между двумя одноранговыми виртуальными сетями

Войдите на [портал Azure](https://portal.azure.com/) с помощью учетной записи, которой назначены необходимые [роли и разрешения](virtual-network-manage-peering.md#permissions). Выберите виртуальную сеть, выберите **Пиринг**, а затем проверьте поле **Состояние**. Определите состояние.

### <a name="the-peering-status-is-connected"></a>Состояние пиринга — "Подключено"

Чтобы устранить неполадку, выполните следующие действия.

1. Проверьте потоки сетевого трафика.

   На исходной виртуальной машине используйте функции [Устранение неполадок с подключением](https://docs.microsoft.com/azure/network-watcher/network-watcher-connectivity-overview) и [Проверка IP-потока](https://docs.microsoft.com/azure/network-watcher/network-watcher-ip-flow-verify-overview) для целевой виртуальной машины, чтобы определить, нет ли NSG или UDR, вызывающих помехи в потоках трафика.

   Если вы используете брандмауэр или NVA, выполните следующие действия. 
   1. Задокументируйте параметры UDR, чтобы их можно было восстановить после выполнения этого шага.
   2. Удалите UDR из подсети исходной виртуальной машины или сетевую карту, которая указывает на NVA в качестве места назначения для следующего прыжка. Проверьте подключение исходной виртуальной машины непосредственно к месту назначению, которое обходит NVA. Если это не сработало, см. [руководство по устранению неполадок NVA](https://docs.microsoft.com/azure/virtual-network/virtual-network-troubleshoot-nva).

2. Выполните трассировку сети. 
   1. Запустите трассировку сети на целевой виртуальной машине. Для Windows можно использовать **Netsh**. Для Linux используйте **TCPDump**.
   2. На исходной виртуальной машине запустите **TcpPing** или **PsPing** для IP-адреса места назначения.

      Вот приведен пример команды **TcpPing**: `tcping64.exe -t <destination VM address> 3389`.

   3. После завершения **TcpPing** остановите трассировку сети в месте назначения.
   4. Если пакеты поступают от исходной виртуальной машины, сетевая ошибка отсутствует. Изучите работу брандмауэра виртуальной машины и приложения, слушающего этот порт, для выявления проблемы конфигурации.

   > [!Note]
   > Вы не можете использовать глобальный пиринг между виртуальными сетями (виртуальным сетям в разных регионах) для подключения к ресурсам следующих типов:
   >
   > * Виртуальные машины за внутренним балансировщиком нагрузки SKU "Базовый"
   > * Redis Cache (использует внутренняя подсистема балансировки нагрузки внутренний со SKU "Базовый");
   > * шлюз приложений (использует внутренняя подсистема балансировки нагрузки внутренний со SKU "Базовый");
   > * Масштабируемые наборы (использует внутренняя подсистема балансировки нагрузки внутренний со SKU "Базовый");
   > * Кластеры Service Fabric (используют внутренний балансировщик нагрузки SKU "Базовый")
   > * группы доступности AlwaysOn для SQL Server (использует внутренняя подсистема балансировки нагрузки внутренний со SKU "Базовый");
   > * Среды службы приложений (ASE) (использует внутренняя подсистема балансировки нагрузки внутренний со SKU "Базовый");
   > * Управление API (использует внутренний балансировщик нагрузки SKU "Базовый")
   > * Azure AD DS (использует внутренняя подсистема балансировки нагрузки внутренний со SKU "Базовый").

Дополнительные сведения см. в разделе о [требованиях и ограничениях](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview#requirements-and-constraints) для глобального пиринга.

### <a name="the-peering-status-is-disconnected"></a>Состояние пиринга — "Отключено"

Чтобы устранить эту проблему, удалите пиринг из обеих виртуальных сетей, а затем создайте его повторно.

## <a name="troubleshoot-a-connectivity-issue-between-a-hub-spoke-virtual-network-and-an-on-premises-resource"></a>Устранение проблем с подключением между виртуальной сетью со звездообразной топологией и локальным ресурсом

Определите, используется ли в сети NVA или VPN-шлюз стороннего производителя.

### <a name="my-network-uses-a-third-party-nva-or-vpn-gateway"></a>В сети используется NVA или VPN-шлюз стороннего производителя

Сведения об устранении проблем с подключением, влияющих на NVA или VPN-шлюз стороннего производителя, см. по следующим ссылкам:

* [Руководство по устранению неполадок с NVA](https://docs.microsoft.com/azure/virtual-network/virtual-network-troubleshoot-nva)
* [Цепочка служб](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview#service-chaining)

### <a name="my-network-does-not-use-a-third-party-nva-or-vpn-gateway"></a>В сети не используется NVA или VPN-шлюз стороннего производителя

Определите, есть ли у центральной и периферийной виртуальных сетей VPN-шлюз.

#### <a name="both-the-hub-virtual-network-and-the-spoke-virtual-network-have-a-vpn-gateway"></a>У центральной и периферийной виртуальных сетей есть VPN-шлюз

Использование удаленного шлюза не поддерживается.

Если у периферийной виртуальной сети уже есть VPN-шлюз, параметр **Использовать удаленный шлюз** не поддерживается в периферийной виртуальной сети. Это связано с ограничением пиринга между виртуальными сетями.

#### <a name="both-the-hub-virtual-network-and-the-spoke-virtual-network-do-not-have-a-vpn-gateway"></a>У центральной и периферийной виртуальных сетей нет VPN-шлюза

Для подключений типа "сеть — сеть" или Azure ExpressRoute проверьте наличие следующих основных причин проблем с подключением к удаленной виртуальной сети из локальной среды.

* Убедитесь, что в виртуальной сети с шлюзом установлен флажок **Разрешить перенаправленный трафик**.
* Убедитесь, что в виртуальной сети без шлюза установлен флажок **Использовать удаленный шлюз**.
* Попросите администратора сети проверить локальные устройства и убедиться в том, что для них для всех добавлено адресное пространство удаленной виртуальной сети.

Для подключений "точка — сеть" выполните следующие действия.

* Убедитесь, что в виртуальной сети с шлюзом установлен флажок **Разрешить перенаправленный трафик**.
* Убедитесь, что в виртуальной сети без шлюза установлен флажок **Использовать удаленный шлюз**.
* Скачайте и переустановите пакет клиента "точка — сеть". Новые маршруты виртуальных сетей для пиринга не добавляются автоматически на клиенты "точка — сеть".

## <a name="troubleshoot-a-hub-spoke-network-connectivity-issue-between-spoke-virtual-networks-in-the-same-region"></a>Устранение неполадок с сетевым подключением между периферийными виртуальными сетями в одном регионе в звездообразной топологии

Центральная сеть должна включать NVA. Настройте маршруты UDR в периферийных сетях, для которых в качестве места назначения следующего прыжка задан сетевой виртуальный модуль, и активируйте параметр **Разрешить перенаправленный трафик** в центральной виртуальной сети.

Дополнительные сведения см. в разделе [Цепочка служб](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview#service-chaining). Указанные требования следует обсудить с выбранным [поставщиком NVA](https://support.microsoft.com/help/2984655/support-for-azure-market-place-for-virtual-machines).

## <a name="troubleshoot-a-hub-spoke-network-connectivity-issue-between-spoke-virtual-networks-in-different-regions"></a>Устранение неполадок с сетевым подключением между периферийными виртуальными сетями в разных регионах в звездообразной топологии

Теперь поддерживается транзитное подключение с использованием глобального пиринга между виртуальными сетями. Подключение с использованием глобального пиринга между виртуальными сетями не работает для следующих ресурсов:

* Виртуальные машины за внутренним балансировщиком нагрузки SKU "Базовый"
* Redis Cache (использует внутренняя подсистема балансировки нагрузки внутренний со SKU "Базовый");
* шлюз приложений (использует внутренняя подсистема балансировки нагрузки внутренний со SKU "Базовый");
* Масштабируемые наборы (использует внутренняя подсистема балансировки нагрузки внутренний со SKU "Базовый");
* Кластеры Service Fabric (используют внутренний балансировщик нагрузки SKU "Базовый")
* группы доступности AlwaysOn для SQL Server (использует внутренняя подсистема балансировки нагрузки внутренний со SKU "Базовый");
* Среды службы приложений (ASE) (использует внутренняя подсистема балансировки нагрузки внутренний со SKU "Базовый");
* Управление API (использует внутренний балансировщик нагрузки SKU "Базовый")
* Azure AD DS (использует внутренняя подсистема балансировки нагрузки внутренний со SKU "Базовый").

Дополнительные сведения см. в разделе о [требованиях и ограничениях](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview#requirements-and-constraints) для глобального пиринга и статье о [других топологиях VPN](https://blogs.msdn.microsoft.com/igorpag/2016/02/11/hubspoke-daisy-chain-and-full-mesh-vnet-topologies-in-azure-arm-v2/).

## <a name="troubleshoot-a-hub-spoke-network-connectivity-issue-between-a-web-app-and-the-spoke-virtual-network"></a>Устранение неполадок с сетевым подключением между веб-приложением и периферийной виртуальной сетью в звездообразной топологии

Чтобы устранить неполадку, выполните следующие действия.

1. Войдите на портал Azure. 
1. В веб-приложении выберите **сетевые подключения**, а затем выберите **Интеграция виртуальной сети**.
1. Проверьте, видите ли вы удаленную виртуальную сеть. Вручную введите адресное пространство удаленной виртуальной сети (**Синхронизировать сеть** и **Добавить маршруты**).

Дополнительные сведения см. в следующих статьях:

* [Интеграция приложения с виртуальной сетью Azure](https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet)
* [Сведения о маршрутизации VPN-подключений "точка — сеть"](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-point-to-site-routing)

## <a name="troubleshoot-a-virtual-network-peering-configuration-error-message"></a>Устранение неполадки, являющейся причиной сообщения об ошибке с конфигурацией пиринга между виртуальными сетями 

### <a name="current-tenant-tenant-id-isnt-authorized-to-access-linked-subscription"></a>Текущий `<TENANT ID>` клиента не имеет прав доступа к связанной подписке

Сведения о том, как устранить эту проблему, см. в раздел [Создание пиринга с помощью Azure CLI](https://docs.microsoft.com/azure/virtual-network/create-peering-different-subscriptions#cli).

### <a name="not-connected"></a>Не подключено

Чтобы устранить эту проблему, удалите пиринг из обеих виртуальных сетей, а затем создайте его повторно.

### <a name="failed-to-peer-a-databricks-virtual-network"></a>Сбой пиринга виртуальной сети Databricks

Чтобы устранить эту проблему, настройте пиринг виртуальных сетей в **Azure Databricks**, а затем укажите целевую виртуальную сеть в поле **Идентификатор ресурса**. Дополнительные сведения см. в разделе [Пиринг виртуальной сети Databricks в удаленную виртуальную сеть](https://docs.azuredatabricks.net/administration-guide/cloud-configurations/azure/vnet-peering.html#id2).

### <a name="the-remote-virtual-network-lacks-a-gateway"></a>В удаленной виртуальной сети отсутствует шлюз

Эта проблема возникает при пиринге виртуальных сетей из разных клиентов и последующей настройке `Use Remote Gateways`. У портала Azure есть ограничение, которое заключается в том, что он не может проверить наличие шлюза виртуальной сети в виртуальной сети другого клиента.

Эту проблему можно решить двумя способами.

 * Удалите пиринг и активируйте параметр `Use Remote Gateways` при создании нового пиринга.
 * Используйте PowerShell или CLI вместо портал Azure, чтобы активировать `Use Remote Gateways`.

## <a name="next-steps"></a>Дальнейшие действия

* [Устранение проблем с подключением между виртуальными машинами Azure](https://docs.microsoft.com/azure/virtual-network/virtual-network-troubleshoot-connectivity-problem-between-vms)
