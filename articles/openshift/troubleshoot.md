---
title: Устранение неполадок Azure Red Hat OpenShift
description: Устранение неполадок и решение распространенных проблем с помощью Azure Red Hat OpenShift
author: jimzim
ms.author: jzim
ms.service: container-service
ms.topic: troubleshooting
ms.date: 05/08/2019
ms.openlocfilehash: 55360ef295ff80b700b059d053203458f9f384db
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "89469088"
---
# <a name="troubleshooting-for-azure-red-hat-openshift"></a>Устранение неполадок в Azure Red Hat OpenShift

В этой статье описаны некоторые распространенные проблемы, возникающие при создании или управлении Microsoft Azure кластерах Red Hat OpenShift.

## <a name="retrying-the-creation-of-a-failed-cluster"></a>Повторная попытка создания кластера с ошибками

Если создать кластер Azure Red Hat OpenShift с помощью `az` команды CLI не удается, повторная попытка создания завершится ошибкой.
Используйте, `az openshift delete` чтобы удалить неисправный кластер, а затем создайте совершенно новый кластер.

## <a name="hidden-azure-red-hat-openshift-cluster-resource-group"></a>Скрытая группа ресурсов кластера Azure Red Hat OpenShift

В настоящее время `Microsoft.ContainerService/openShiftManagedClusters` ресурс, автоматически созданный Azure CLI ( `az openshift create` командой), скрыт в портал Azure. В представлении " **Группа ресурсов** " установите флажок **Показать скрытые типы** , чтобы просмотреть группу ресурсов.

![Снимок экрана: флажок скрытого типа на портале](./media/aro-portal-hidden-type.png)

## <a name="creating-a-cluster-results-in-error-that-no-registered-resource-provider-found"></a>Создание кластера приводит к ошибке, что зарегистрированный поставщик ресурсов не найден.

Если при создании кластера возникает ошибка `No registered resource provider found for location '<location>' and API version '2019-04-30' for type 'openShiftManagedClusters'. The supported api-versions are '2018-09-30-preview` , то вы были частью предварительной версии и теперь должны [приобрести зарезервированные экземпляры виртуальных машин Azure](https://aka.ms/openshift/buy) для использования общедоступного продукта. Резервирование сокращает расходы за счет предварительной оплаты для полностью управляемых служб Azure. Дополнительные сведения о резервировании и способах экономии денег см. в разделе [*что такое резервирование Azure*](../cost-management-billing/reservations/save-compute-costs-reservations.md) .

## <a name="next-steps"></a>Дальнейшие шаги

- Воспользуйтесь [центром справки Red Hat OpenShift](https://help.openshift.com/) для получения дополнительных сведений об устранении неполадок OpenShift.

- Найдите ответы на [часто задаваемые вопросы о Azure Red Hat OpenShift](openshift-faq.md).