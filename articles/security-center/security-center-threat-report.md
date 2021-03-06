---
title: Отчет об анализе угроз, предоставляемый центром безопасности Azure | Документация Майкрософт
description: Эта страница поможет вам использовать отчеты по аналитике угроз центра безопасности Azure во время расследования, чтобы найти дополнительные сведения об оповещениях системы безопасности.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 5662e312-e8c2-4736-974e-576eeb333484
ms.service: security-center
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/15/2020
ms.author: memildin
ms.openlocfilehash: f9b3fd0000a1b5dbba00995c37f96a89319de0e1
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91440492"
---
# <a name="azure-security-center-threat-intelligence-report"></a>Отчет об анализе угроз в центре безопасности Azure

На этой странице объясняется, как с помощью отчетов по анализу угроз в центре безопасности Azure можно получить дополнительные сведения о угрозе, вызвавшем оповещение системы безопасности.


## <a name="what-is-a-threat-intelligence-report"></a>Отчет об анализе угроз

Защита от угроз для центра безопасности осуществляется за счет мониторинга сведений о безопасности из ресурсов Azure, сети и подключенных партнерских решений. Она анализирует эту информацию, часто сравнивая сведения из разных источников и определяя угрозы. Дополнительные сведения см. [в статье как центр безопасности Azure обнаруживает угрозы и реагирует на них](security-center-alerts-overview.md#detect-threats).

Когда центр безопасности определяет угрозу, он запускает [оповещение системы безопасности](security-center-managing-and-responding-alerts.md), содержащее подробные сведения о событии, включая рекомендации по исправлению. Чтобы специалисты по реагированию могли исследовать и устранять угрозы, центр безопасности предоставляет отчеты по анализу угроз, содержащие сведения об обнаруженных угрозах. Отчет содержит следующие сведения:

* удостоверение злоумышленника или имеющиеся связи (если такая информация доступна);
* цели злоумышленника;
* текущие и исторические кампании атак (если такая информация доступна);
* тактики злоумышленника, применяемые инструменты и процедуры;
* связанные признаки компрометации, такие как URL-адреса и хэши файлов;
* сведения о ресурсах или пользователях, подвергшихся атаке, включая данные об отрасли и регионе, которые помогают определить, если ли риск для ваших ресурсов Azure;
* сведения по устранению и исправлению.

> [!NOTE]
> Объем сведений в любом конкретном отчете будет отличаться. Уровень детализации зависит от активности вредоносных программ и их распространенности.

Центр безопасности предусматривает три типа отчетов об угрозах, которые могут различаться в зависимости от атаки, а именно:

* **Отчет о группе действий**. обеспечивает глубокую подробноку злоумышленников, их цели и тактику.
* **Отчет о кампаниях**: фокусируется на деталях конкретных кампаний атак.
* **сводный отчет об угрозах**, который включает в себя все элементы двух предыдущих отчетов.

Этот тип информации полезен в процессе реагирования на инциденты, при котором выполняется анализ источника атаки, мотивации злоумышленника и действия по устранению этой проблемы в будущем.



## <a name="how-to-access-the-threat-intelligence-report"></a>Получение доступа к отчету об анализе угроз

1. На боковой панели центра безопасности откройте страницу **оповещения системы безопасности** .
1. Выберите оповещение. 
    Откроется страница сведений о предупреждениях с дополнительными сведениями о предупреждении. Ниже приведена страница со сведениями о предупреждении, **обнаруженных индикатором атаки** .

    [![Обнаруженные индикаторы на странице сведений об оповещении](media/security-center-threat-report/ransomware-indicators-detected-link-to-threat-intel-report.png)](media/security-center-threat-report/ransomware-indicators-detected-link-to-threat-intel-report.png#lightbox)

1. Выберите ссылку на отчет, и в браузере по умолчанию откроется PDF-файл.

    [![Страница сведений о потенциально небезопасном действии](media/security-center-threat-report/threat-intelligence-report.png)](media/security-center-threat-report/threat-intelligence-report.png#lightbox)

    При необходимости можно скачать отчет PDF. 

    >[!TIP]
    > Объем сведений, доступных для каждого оповещения системы безопасности, отличается в зависимости от типа оповещения.



## <a name="next-steps"></a>Дальнейшие шаги

На этой странице описано, как открыть отчеты по анализу угроз при изучении оповещений системы безопасности. Связанные сведения см. на следующих страницах:

* [Управление оповещениями безопасности в центре безопасности Azure и реагирование на них](security-center-managing-and-responding-alerts.md). Узнайте, как управлять оповещениями системы безопасности и реагировать на них.
* [Обработка инцидентов безопасности в центре безопасности Azure](security-center-incident.md)