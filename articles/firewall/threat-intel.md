---
title: Фильтрация на основе аналитики угроз в Брандмауэре Azure
description: Фильтрация на основе Microsoft Threat Intelligence может быть включена в брандмауэре с целю создания оповещений и запрета трафика, поступающего с известных вредоносных IP-адресов и доменов, а также передающегося на них.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 05/12/2020
ms.author: victorh
ms.openlocfilehash: e51cc8905a7b4a88bb7f7dabaf24bb30159ff86c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "83655093"
---
# <a name="azure-firewall-threat-intelligence-based-filtering"></a>Фильтрация на основе аналитики угроз в Брандмауэре Azure

Фильтрация на основе Microsoft Threat Intelligence может быть включена в брандмауэре с целю создания оповещений и запрета трафика, поступающего с известных вредоносных IP-адресов и доменов, а также передающегося на них. IP-адреса и домены также передаются из канала Microsoft Threat Intelligence. [Intelligent Security Graph](https://www.microsoft.com/security/operations/intelligence) поддерживает Microsoft Threat Intelligence и используется во множестве служб, включая Центр безопасности Azure.<br>
<br>

:::image type="content" source="media/threat-intel/firewall-threat.png" alt-text="Аналитика угроз в брандмауэре" border="false":::

Если включена фильтрация на основе аналитики угроз, то связанные правила обрабатываются прежде любых правил NAT, правил сети или правил приложения.

Можно выбрать только ведение журнала оповещений при активации правила или выбрать режим оповещения и запрета.

По умолчанию фильтрация на основе аналитики угроз работает в режиме оповещения. Вы не сможете отключить эту функцию или изменить режим, пока соответствующий раздел интерфейса портала не станет доступен в вашем регионе.

:::image type="content" source="media/threat-intel/threat-intel-ui.png" alt-text="Аналитика угроз в брандмауэре":::

## <a name="logs"></a>Журналы

В следующем фрагменте журнала показано активированное правило.

```
{
    "category": "AzureFirewallNetworkRule",
    "time": "2018-04-16T23:45:04.8295030Z",
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/AZUREFIREWALLS/{resourceName}",
    "operationName": "AzureFirewallThreatIntelLog",
    "properties": {
         "msg": "HTTP request from 10.0.0.5:54074 to somemaliciousdomain.com:80. Action: Alert. ThreatIntel: Bot Networks"
    }
}
```

## <a name="testing"></a>Тестирование

- **Тестирование исходящего трафика**. Оповещения об исходящем трафике должны появляться редко, так как они означают, что ваша среда была скомпрометирована. Чтобы проверить работоспособность оповещений об исходящем трафике, было создано тестовое полное доменное имя, которое активирует оповещение. Используйте **testmaliciousdomain.eastus.cloudapp.azure.com** для тестирования исходящего трафика.

- **Тестирование входящего трафика**. Вы можете видеть оповещения о входящем трафике, если в брандмауэре настроены правила DNAT. Это происходит, даже если в правиле DNAT разрешены только определенные источники, а трафик из других источников отклоняется. Брандмауэр Azure не оповещает о всех известных сканерах портов. Он оповещает только об известных сканерах, которые замечены во вредоносных действиях.

## <a name="next-steps"></a>Дальнейшие действия

- Ознакомьтесь с [примерами запросов Log Analytics для Брандмауэра Azure](log-analytics-samples.md).
- Узнайте, как [развернуть и настроить Брандмауэр Azure](tutorial-firewall-deploy-portal.md).
- Изучите [аналитический отчет Microsoft Security](https://www.microsoft.com/en-us/security/operations/security-intelligence-report).