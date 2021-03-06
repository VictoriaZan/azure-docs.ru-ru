---
title: Создание общедоступного IP-адреса портал Azure
description: Узнайте, как создать общедоступный IP-адрес в портал Azure
services: virtual-network
documentationcenter: na
author: blehr
ms.service: virtual-network
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/28/2020
ms.author: blehr
ms.openlocfilehash: add763b713b93604e089d7aec586876fecd2887c
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/25/2020
ms.locfileid: "95895644"
---
# <a name="quickstart-create-a-public-ip-address-using-the-azure-portal"></a>Краткое руководство. Создание общедоступного IP-адреса с помощью портал Azure

В этой статье показано, как создать ресурс общедоступного IP-адреса с помощью портал Azure. Дополнительные сведения о том, к каким ресурсам можно связываться, разница между SKU "базовый" и "Стандартный" и другими связанными сведениями см. в разделе [общедоступные IP-адреса](https://docs.microsoft.com/azure/virtual-network/public-ip-addresses).  В этом примере мы рассмотрим только IPv4-адреса. Дополнительные сведения об IPv6-адресах см. в статье [IPv6 для виртуальной сети Azure](https://docs.microsoft.com/azure/virtual-network/ipv6-overview).

# <a name="standard-sku---using-zones"></a>[**SKU "Стандартный" — Использование зон**](#tab/option-create-public-ip-standard-zones)

Выполните следующие действия, чтобы создать стандартный общедоступный IP-адрес, избыточный в виде зоны с именем **мистандардзрпублиЦип**.

1. Войдите на [портал Azure](https://portal.azure.com/).
2. Выберите **Создать ресурс**. 
3. В поле поиска введите *общедоступный IP-адрес*.
4. В результатах поиска выберите **Общедоступный IP-адрес**. Затем на странице **Общедоступный IP-адрес** щелкните **Создать**.
5. На странице **Создание общедоступного IP-адреса** введите или выберите следующие сведения. 

    | Параметр                 | Значение                       |
    | ---                     | ---                         |
    | Версия IP-адреса              | Выберите IPv4                 |    
    | номер SKU                     | Выберите **Стандартный**         |
    | Имя                    | Введите *мистандардзрпублиЦип*          |
    | Назначение IP-адресов   | Обратите внимание, что он будет заблокирован как "статический"                                        |
    | Время ожидания простоя (в минутах)  | Оставьте значение 4        |
    | Метка DNS-имени          | Оставьте значение пустым.    |
    | Подписка            | Выберите свою подписку.   |
    | Группа ресурсов          | Выберите **создать** , введите myResourceGroup, а затем нажмите кнопку **ОК** . |
    | Расположение                | Выберите **Восточная часть США 2**      |
    | Зона доступности       | Выберите зону, **избыточную в зоне** , или выберите конкретную (см. Примечание ниже). |

Обратите внимание, что эти варианты допустимы только в регионах с [зоны доступности](https://docs.microsoft.com/azure/availability-zones/az-overview?toc=/azure/virtual-network/toc.json#availability-zones).  (Можно также выбрать конкретную зону в этих регионах, хотя она не будет устойчивой к зональные сбоям.)

# <a name="basic-sku"></a>[**SKU "Базовый"**](#tab/option-create-public-ip-basic)

Чтобы создать базовый статический общедоступный IP-адрес с именем **мибасикпублиЦип**, выполните следующие действия.  Основные общедоступные IP-адреса не имеют концепции зон доступности.

1. Войдите на [портал Azure](https://portal.azure.com/).
2. Выберите **Создать ресурс**. 
3. В поле поиска введите *общедоступный IP-адрес*.
4. В результатах поиска выберите **Общедоступный IP-адрес**. Затем на странице **Общедоступный IP-адрес** щелкните **Создать**.
5. На странице **Создание общедоступного IP-адреса** введите или выберите следующие сведения. 

    | Параметр                 | Значение                       |
    | ---                     | ---                         |
    | Версия IP-адреса              | Выберите IPv4                 |    
    | номер SKU                     | Выберите **Стандартный**         |
    | Имя                    | Введите *мибасикпублиЦип*          |
    | Назначение IP-адресов   | Выберите **статический** (см. Примечание ниже)                                     |
    | Время ожидания простоя (в минутах)  | Оставьте значение 4        |
    | Метка DNS-имени          | Оставьте значение пустым.    |
    | Подписка            | Выберите свою подписку.   |
    | Группа ресурсов          | Выберите **создать** , введите myResourceGroup, а затем нажмите кнопку **ОК** . |
    | Расположение                | Выберите **Восточная часть США 2**      |

Если IP-адрес может меняться с течением времени, можно выбрать **динамическое** назначение IP-адресов.

---

## <a name="additional-information"></a>Дополнительные сведения 

Дополнительные сведения об отдельных указанных выше полях см. в разделе [Управление общедоступными IP-адресами](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address#create-a-public-ip-address).

## <a name="next-steps"></a>Дальнейшие действия
- Связывание [общедоступного IP-адреса с виртуальной машиной](https://docs.microsoft.com/azure/virtual-network/associate-public-ip-address-vm#azure-portal)
- См. дополнительные сведения об [общедоступных IP-адресах в Azure](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses).
- См. сведения обо всех [параметрах общедоступных IP-адресов](virtual-network-public-ip-address.md#create-a-public-ip-address).