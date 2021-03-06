---
title: Цены и связанные затраты
description: Узнайте о затратах, связанных с защитником Интернета вещей, и о том, как управлять ими.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/04/2020
ms.author: mlottner
ms.openlocfilehash: 24ae6c4014948639aa737a0d2d88ec15f98a7cb4
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "90939528"
---
# <a name="pricing-and-associated-costs"></a>Цены и связанные затраты

В этой статье объясняется модель ценообразования для центра Интернета вещей, приводятся все связанные затраты и объясняется, как управлять ими.

## <a name="pricing"></a>Цены

Модель ценообразования для центра Интернета вещей состоит из двух частей и оплачивается после [включения](quickstart-onboard-iot-hub.md) центра Интернета вещей в защитнике для Интернета вещей.

- Затраты на встроенные в устройства возможности безопасности, основанные на анализе журналов центра Интернета вещей.

- Затраты на повышение безопасности сообщений, основанные на сообщениях безопасности от IoT Edge или конечных устройств.

Дополнительные сведения см. на странице [цен на центр безопасности](https://azure.microsoft.com/pricing/details/security-center/).

## <a name="associated-costs"></a>Связанные затраты

Защитник для Интернета вещей связан с затратами, не входящими в прямые цены:

- Log Analytics стоимость хранилища

Вы можете уменьшить связанные затраты, отказаться от определенных функций решения. Отказаться от изменения параметров.

Чтобы изменить параметры, выполните следующие действия.

1. Откройте центр Интернета вещей.

1. В разделе **Безопасность**щелкните **Параметры**.

1. Щелкните **сбор данных**.

В следующей таблице приводится сводка связанных затрат и последствий каждого из них.

| Параметр | Использование | Комментарий |
| --- | --- | --- |
| **Хранилище Log Analytics** |  |
| Рекомендации и оповещения для устройств| Рекомендации по безопасности и предупреждения, созданные службой | Не является обязательным |
| Необработанные данные безопасности| Необработанные данные безопасности с устройств IoT, собранные агентами безопасности | Отключить _события безопасности хранения необработанных устройств_ |
|

>[!Important]
> Отказ от участия в защитнике имеет серьезные последствия для обеспечения доступности функций безопасности IoT.

| Отказаться | Последствия |
| --- | --- |
| _Коллекция метаданных двойника_ | Отключение [настраиваемых оповещений](quickstart-create-custom-alerts.md) |
| | Отключение рекомендаций по манифесту IoT Edge |
| | Отключение рекомендаций и оповещений на основе удостоверений устройств |
| _Хранение событий безопасности необработанных устройств_ | Сведения о базовых рекомендациях для устройств в ОС недоступны |
| | Подробные сведения об исследовании [предупреждений](concept-security-alerts.md) и [рекомендаций](concept-recommendations.md) недоступны |
|

## <a name="see-also"></a>См. также раздел

- Доступ к [необработанным данным безопасности](how-to-security-data-access.md)
- [Исследование устройства](how-to-investigate-device.md)
- Ознакомьтесь с [рекомендациями по безопасности](concept-recommendations.md)
- Изучение и изучение [оповещений системы безопасности](concept-security-alerts.md)
