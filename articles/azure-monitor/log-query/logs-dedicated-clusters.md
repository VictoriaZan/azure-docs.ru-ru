---
title: Azure Monitor журналов выделенных кластеров
description: Клиенты, получившие более 1 ТБ данных мониторинга в день, могут использовать выделенные, а не общие кластеры.
ms.subservice: logs
ms.topic: conceptual
author: rboucher
ms.author: robb
ms.date: 09/16/2020
ms.openlocfilehash: d2446e866c0e12d50a0759373682f4f62bc4bba0
ms.sourcegitcommit: df66dff4e34a0b7780cba503bb141d6b72335a96
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/02/2020
ms.locfileid: "96512228"
---
# <a name="azure-monitor-logs-dedicated-clusters"></a>Azure Monitor журналов выделенных кластеров

Azure Monitor журналов выделенные кластеры — это вариант развертывания, который включает расширенные возможности для Azure Monitor журналов клиентов. Клиенты с выделенными кластерами могут выбрать рабочие области, которые будут размещены в этих кластерах.

Ниже перечислены возможности, требующие использования выделенных кластеров.

- **[Ключи, управляемые клиентом](../platform/customer-managed-keys.md)** — шифруют данные кластера с помощью ключей, предоставляемых и контролируемых клиентом.
- **[Защищенное хранилище](../platform/customer-managed-keys.md#customer-lockbox-preview)** — клиенты могут контролировать запросы на доступ к данным в службе поддержки Майкрософт.
- **[Двойное шифрование](../../storage/common/storage-service-encryption.md#doubly-encrypt-data-with-infrastructure-encryption)** защищает от ситуации, когда один из алгоритмов или ключей шифрования может быть скомпрометирован. В этом случае дополнительный уровень шифрования продолжит защищать ваши данные.
- **[Несколько рабочих областей](../log-query/cross-workspace-query.md)** . Если клиент использует более одной рабочей области для рабочей среды, может быть целесообразно использовать выделенный кластер. Запросы между рабочими областями будут выполняться быстрее, если все рабочие области находятся в одном кластере. Кроме того, использование выделенного кластера может быть более экономичным, так как назначенные уровни резервирования емкости принимают во внимание все приемы кластера и применяются ко всем его рабочим областям, даже если некоторые из них невелики и не имеют скидки на резервирование емкости.

Для выделенных кластеров клиенты должны использовать емкость не менее 1 ТБ приема данных в день. Миграция в выделенный кластер проста. Нет потерь данных или прерываний службы. 

> [!IMPORTANT]
> Выделенные кластеры утверждаются и полностью поддерживаются для рабочих развертываний. Однако из-за временных ограничений емкости для использования этой функции требуется предварительная регистрация. Используйте свои контактные данные в Майкрософт, чтобы предоставить идентификаторы подписок.

## <a name="management"></a>Управление 

Управление выделенными кластерами осуществляется через ресурс Azure, который представляет Azure Monitor кластеры журналов. Все операции выполняются с этим ресурсом с помощью PowerShell или REST API.

После создания кластера его можно настроить и связать с ним рабочие области. Если Рабочая область связана с кластером, новые данные, отправляемые в рабочую область, находятся в кластере. Только рабочие области, наявляющиеся в том же регионе, что и кластер, могут быть связаны с кластером. Рабочие области могут быть в отличие от кластера с некоторыми ограничениями. Дополнительные сведения об этих ограничениях приведены в этой статье. 

Данные, принимаемые в выделенные кластеры, шифруются дважды — один раз на уровне службы с помощью ключей, управляемых корпорацией Майкрософт или [управляемым клиентом ключом](../platform/customer-managed-keys.md), и один раз на уровне инфраструктуры с использованием двух разных алгоритмов шифрования и двух разных ключей. [Двойное шифрование](../../storage/common/storage-service-encryption.md#doubly-encrypt-data-with-infrastructure-encryption) защищает от ситуации, когда один из алгоритмов или ключей шифрования может быть скомпрометирован. В этом случае дополнительный уровень шифрования продолжит защищать ваши данные. Выделенный кластер также позволяет защищать данные с помощью элемента управления [защищенным хранилищем](../platform/customer-managed-keys.md#customer-lockbox-preview) .

Для всех операций на уровне кластера требуется `Microsoft.OperationalInsights/clusters/write` разрешение Action в кластере. Это разрешение можно предоставить с помощью владельца или участника, который содержит `*/write` действие, или с помощью роли участника log Analytics, которая содержит `Microsoft.OperationalInsights/*` действие. Дополнительные сведения о Log Analytics разрешений см. [в разделе Управление доступом к данным и рабочим областям журнала в Azure Monitor](../platform/manage-access.md). 


## <a name="cluster-pricing-model"></a>Ценовая модель кластера

В Log Analytics выделенных кластерах используется модель ценообразования резервирования емкости, которая не менее 1000 ГБ в день. При превышении уровня использования резервирования будет взиматься плата по мере использования.  Сведения о ценах на резервирование емкости доступны на [странице цен на Azure Monitor]( https://azure.microsoft.com/pricing/details/monitor/).  

Уровень резервирования емкости кластера настраивается программным путем с Azure Resource Manager с помощью `Capacity` параметра в разделе `Sku` . `Capacity` указывается в ГБ и может иметь значения 1000 ГБ/день или более с шагом приращения 100 ГБ/день.

Существует два режима выставления счетов за использование в кластере. Они могут быть заданы `billingType` параметром при настройке кластера. 

1. **Кластер**. в данном случае (по умолчанию) выставление счетов за принимаемые данные выполняется на уровне кластера. Полученные количества данных из каждой рабочей области, связанной с кластером, суммируются для вычисления ежедневного счета за кластер. 

2. **Рабочие области**. затраты на резервирование ресурсов для кластера настраиваются пропорционально рабочим областям в кластере (после учета для каждого узла из [центра безопасности Azure](../../security-center/index.yml) для каждой рабочей области).

Если ваша рабочая область использует ценовую категорию "устаревшие" на узел, если она связана с кластером, плата будет взиматься с учетом данных, полученных в отношении резервирования емкости кластера и более недоступных для каждого узла. Распределение данных по узлам из центра безопасности Azure будет по прежнему применено.

Дополнительные сведения о выставлении счетов за Log Analytics выделенные кластеры доступны [здесь]( https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#log-analytics-dedicated-clusters).


## <a name="creating-a-cluster"></a>Создание кластера

Сначала создайте ресурсы кластера, чтобы начать создание выделенного кластера.

Необходимо указать следующие свойства:

- **Имя_кластера**: используется для административных целей. Пользователи не предоставляются по этому имени.
- **ResourceGroupName**. как и для любого ресурса Azure, кластеры принадлежат к группе ресурсов. Мы рекомендуем использовать центральную группу ресурсов ИТ, так как кластеры обычно являются общими для многих команд в Организации. Дополнительные рекомендации по проектированию см. [в разделе Разработка развертывания журналов Azure Monitor](../platform/design-logs-deployment.md)
- **Расположение**: кластер находится в определенном регионе Azure. Только рабочие области, расположенные в этом регионе, могут быть связаны с этим кластером.
- **СкукапаЦити**: при создании ресурса *кластера* необходимо указать уровень *резервирования емкости* (SKU). Уровень *резервирования емкости* может быть в диапазоне от 1 000 гб до 3 000 ГБ в день. При необходимости его можно обновить, выполнив шаги, описанные в 100. Если требуется уровень резервирования емкости более 3 000 ГБ в день, свяжитесь с нами по адресу LAIngestionRate@microsoft.com . Дополнительные сведения о расходах на кластер см. в статье [Управление затратами для log Analytics кластеров](../platform/manage-cost-storage.md#log-analytics-dedicated-clusters) .

После создания ресурса *кластера* можно изменить дополнительные свойства, такие как *SKU*, * кэйваултпропертиес или *биллингтипе*. Дополнительные сведения см. ниже.

> [!WARNING]
> Создание кластера вызывает выделение ресурсов и подготовку. Выполнение этой операции может занять до часа. Рекомендуется выполнять его асинхронно.

Учетная запись пользователя, которая создает кластеры, должна иметь стандартное разрешение на создание ресурсов Azure `Microsoft.Resources/deployments/*` и разрешение на запись в кластер `(Microsoft.OperationalInsights/clusters/write)` .

### <a name="create"></a>Создание 

**PowerShell**

```powershell
New-AzOperationalInsightsCluster -ResourceGroupName {resource-group-name} -ClusterName {cluster-name} -Location {region-name} -SkuCapacity {daily-ingestion-gigabyte} -AsJob

# Check when the job is done
Get-Job -Command "New-AzOperationalInsightsCluster*" | Format-List -Property *
```

**REST**

*Вызов* 
```rst
PUT https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2020-03-01-preview
Authorization: Bearer <token>
Content-type: application/json

{
  "identity": {
    "type": "systemAssigned"
    },
  "sku": {
    "name": "capacityReservation",
    "Capacity": 1000
    },
  "properties": {
    "billingType": "cluster",
    },
  "location": "<region-name>",
}
```

*Ответ*

Должно быть 200 ОК и заголовок.

### <a name="check-provisioning-status"></a>Проверка состояния подготовки

Подготовка кластера Log Analytics занимает некоторое время. Проверить состояние подготовки можно несколькими способами.

- Выполните команду PowerShell Get-AzOperationalInsightsCluster с именем группы ресурсов и проверьте свойство ProvisioningState. Значение *провисионингаккаунт* во время подготовки и *успешного* завершения.
  ```powershell
  New-AzOperationalInsightsCluster -ResourceGroupName {resource-group-name} 
  ```

- Скопируйте значение URL-адреса Azure-AsyncOperation из ответа и выполните проверку состояния асинхронных операций.

- Отправьте запрос GET на ресурс *Кластер* и просмотрите значение *provisioningState*. Значение *провисионингаккаунт* во время подготовки и *успешного* завершения.

   ```rst
   GET https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2020-03-01-preview
   Authorization: Bearer <token>
   ```

   **Ответ**

   ```json
   {
     "identity": {
       "type": "SystemAssigned",
       "tenantId": "tenant-id",
       "principalId": "principal-id"
       },
     "sku": {
       "name": "capacityReservation",
       "capacity": 1000,
       "lastSkuUpdate": "Sun, 22 Mar 2020 15:39:29 GMT"
       },
     "properties": {
       "provisioningState": "ProvisioningAccount",
       "billingType": "cluster",
       "clusterId": "cluster-id"
       },
     "id": "/subscriptions/subscription-id/resourceGroups/resource-group-name/providers/Microsoft.OperationalInsights/clusters/cluster-name",
     "name": "cluster-name",
     "type": "Microsoft.OperationalInsights/clusters",
     "location": "region-name"
   }
   ```

Идентификатор GUID *principalId* создается управляемой службой удостоверений для ресурса *кластера* .

## <a name="change-cluster-properties"></a>Изменение свойств кластера

После создания ресурса *кластера* и его полной подготовки можно изменить дополнительные свойства на уровне кластера с помощью PowerShell или REST API. Кроме свойств, доступных во время создания кластера, дополнительные свойства можно задать только после подготовки кластера.

- **кэйваултпропертиес**: используется для настройки Azure Key Vault, используемого для предоставления [Azure Monitorного ключа, управляемого клиентом](../platform/customer-managed-keys.md#customer-managed-key-provisioning-procedure). Он содержит следующие параметры:  *KeyVaultUri*, *keyName*, *кэйверсион*. 
- **биллингтипе** — свойство *биллингтипе* определяет атрибуты выставления счетов для ресурса *кластера* и его данных:
  - **Кластер** (по умолчанию). затраты на резервирование емкости кластера приводятся к ресурсу *кластера* .
  - **Рабочие области** . затраты на резервирование ресурсов для кластера настраиваются пропорционально рабочим областям кластера, при этом ресурс *кластера* выставляет счет за использование, если общее количество принимаемых данных за день находится под резервированием емкости. Дополнительные сведения о модели ценообразования кластера см. в разделе [log Analytics выделенные кластеры](../platform/manage-cost-storage.md#log-analytics-dedicated-clusters) . 

> [!NOTE]
> Свойство *биллингтипе* не поддерживается в PowerShell.
> Обновления свойств кластера могут выполняться асинхронно, и для их завершения может потребоваться некоторое время.

**PowerShell**

```powershell
Update-AzOperationalInsightsCluster -ResourceGroupName {resource-group-name} -ClusterName {cluster-name} -KeyVaultUri {key-uri} -KeyName {key-name} -KeyVersion {key-version}
```

**REST**

> [!NOTE]
> Вы можете обновить *SKU* ресурса *кластера* , *КЭЙВАУЛТПРОПЕРТИЕС* или *биллингтипе* с помощью исправления.

Пример: 

*Вызов*

```rst
PATCH https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2020-03-01-preview
Authorization: Bearer <token>
Content-type: application/json

{
   "sku": {
     "name": "capacityReservation",
     "capacity": <capacity-reservation-amount-in-GB>
     },
   "properties": {
    "billingType": "cluster",
     "KeyVaultProperties": {
       "KeyVaultUri": "https://<key-vault-name>.vault.azure.net",
       "KeyName": "<key-name>",
       "KeyVersion": "<current-version>"
       }
   },
   "location":"<region-name>"
}
```

"KeyVaultProperties" содержит сведения об идентификаторе ключа Key Vault.

*Ответ*

200 ОК и заголовок

### <a name="check-cluster-update-status"></a>Проверка состояния обновления кластера

Распространение идентификатора ключа занимает несколько минут. Проверить состояние обновления можно двумя способами.

- Скопируйте значение URL-адреса Azure-AsyncOperation из ответа и выполните проверку состояния асинхронных операций. 

   OR

- Отправьте запрос GET на ресурс *Кластер* и просмотрите свойства *KeyVaultProperties*. В ответе должны возвращаться недавно обновленные сведения об идентификаторе ключа.

   Ответ на запрос GET в ресурсе *Кластер* после завершения обновления идентификатора ключа должен выглядеть следующим образом:

   ```json
   {
     "identity": {
       "type": "SystemAssigned",
       "tenantId": "tenant-id",
       "principalId": "principle-id"
       },
     "sku": {
       "name": "capacityReservation",
       "capacity": 1000,
       "lastSkuUpdate": "Sun, 22 Mar 2020 15:39:29 GMT"
       },
     "properties": {
       "keyVaultProperties": {
         "keyVaultUri": "https://key-vault-name.vault.azure.net",
         "kyName": "key-name",
         "keyVersion": "current-version"
         },
        "provisioningState": "Succeeded",
       "billingType": "cluster",
       "clusterId": "cluster-id"
     },
     "id": "/subscriptions/subscription-id/resourceGroups/resource-group-name/providers/Microsoft.OperationalInsights/clusters/cluster-name",
     "name": "cluster-name",
     "type": "Microsoft.OperationalInsights/clusters",
     "location": "region-name"
  }
  ```

## <a name="link-a-workspace-to-the-cluster"></a>Связывание рабочей области с кластером

Если Рабочая область связана с выделенным кластером, новые данные, полученные в рабочей области, направляются в новый кластер, а существующие данные остаются в существующем кластере. Если выделенный кластер шифруется с помощью управляемых клиентом ключей (CMK), с ключом шифруются только новые данные. Система является абстрактным различием от пользователей, и пользователи просто запрашивают рабочую область как обычно, а система выполняет запросы между кластерами в серверной части.

Кластер может быть связан с рабочими областями до 100. Связанные рабочие области находятся в том же регионе, что и кластер. Чтобы обеспечить защиту серверной части системы и избежать фрагментации данных, Рабочая область не может быть связана с кластером более чем дважды в месяц.

Для выполнения операции связывания необходимо иметь разрешения на запись в рабочую область и ресурс *кластера* :

- В рабочей области: *Microsoft. OperationalInsights/рабочие области/запись*
- В ресурсе *кластера* : *Microsoft. OperationalInsights/Clusters/Write*

В отличие от аспектов выставления счетов, связанная Рабочая область хранит собственные параметры, такие как продолжительность хранения данных.
Рабочая область и кластер могут находиться в разных подписках. Рабочая область и кластер могут находиться в разных клиентах, если Azure Лигхсаусе используется для их параллельного отображения с одним клиентом.

При любой операции кластера связывание рабочей области может выполняться только после завершения подготовки кластера Log Analytics.

> [!WARNING]
> Чтобы связать рабочую область с кластером, необходимо синхронизировать несколько серверных компонентов и гарантировать расконсервации кэша. Выполнение этой операции может занять до двух часов. Мы рекомендуем запускать его асинхронно.


**PowerShell**

Используйте следующую команду PowerShell для связи с кластером:

```powershell
# Find cluster resource ID
$clusterResourceId = (Get-AzOperationalInsightsCluster -ResourceGroupName {resource-group-name} -ClusterName {cluster-name}).id

# Link the workspace to the cluster
Set-AzOperationalInsightsLinkedService -ResourceGroupName {resource-group-name} -WorkspaceName {workspace-name} -LinkedServiceName cluster -WriteAccessResourceId $clusterResourceId -AsJob

# Check when the job is done
Get-Job -Command "Set-AzOperationalInsightsLinkedService" | Format-List -Property *
```


**REST**

Используйте следующий вызов функции RESTFUL, чтобы связать с кластером:

*Send*

```rst
PUT https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>/linkedservices/cluster?api-version=2020-03-01-preview 
Authorization: Bearer <token>
Content-type: application/json

{
  "properties": {
    "WriteAccessResourceId": "/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/clusters/<cluster-name>"
    }
}
```

*Ответ*

200 ОК и заголовок.

### <a name="using-customer-managed-keys-with-linking"></a>Использование управляемых клиентом ключей с компоновкой

Если вы используете ключи, управляемые клиентом, полученные данные будут храниться в зашифрованном виде с помощью управляемого ключа после операции связывания, что может занять до 90 минут. 

Проверить состояние сопоставления рабочей области можно двумя способами.

- Скопируйте значение URL-адреса Azure-AsyncOperation из ответа и выполните проверку состояния асинхронных операций.

- Отправка [рабочих областей — получение](/rest/api/loganalytics/workspaces/get) запроса и наблюдение за ответом. Связанная Рабочая область имеет Клустерресаурцеид в разделе "компоненты".

Запрос на отправку выглядит следующим образом:

*Send*

```rest
GET https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalInsights/workspaces/<workspace-name>?api-version=2020-03-01-preview
Authorization: Bearer <token>
```

*Ответ*

```json
{
  "properties": {
    "source": "Azure",
    "customerId": "workspace-name",
    "provisioningState": "Succeeded",
    "sku": {
      "name": "pricing-tier-name",
      "lastSkuUpdate": "Tue, 28 Jan 2020 12:26:30 GMT"
    },
    "retentionInDays": 31,
    "features": {
      "legacy": 0,
      "searchVersion": 1,
      "enableLogAccessUsingOnlyResourcePermissions": true,
      "clusterResourceId": "/subscriptions/subscription-id/resourceGroups/resource-group-name/providers/Microsoft.OperationalInsights/clusters/cluster-name"
    },
    "workspaceCapping": {
      "dailyQuotaGb": -1.0,
      "quotaNextResetTime": "Tue, 28 Jan 2020 14:00:00 GMT",
      "dataIngestionStatus": "RespectQuota"
    }
  },
  "id": "/subscriptions/subscription-id/resourcegroups/resource-group-name/providers/microsoft.operationalinsights/workspaces/workspace-name",
  "name": "workspace-name",
  "type": "Microsoft.OperationalInsights/workspaces",
  "location": "region-name"
}
```

## <a name="unlink-a-workspace-from-a-dedicated-cluster"></a>Отмена связи рабочей области с выделенным кластером

Вы можете удалить связь рабочей области с кластером. После разрыва связи с рабочей областью из кластера новые данные, связанные с этой рабочей областью, не отправляются в выделенный кластер. Кроме того, выставление счетов за рабочую область больше не выполняется через кластер. Старые данные несвязанной рабочей области могут остаться в кластере. Если эти данные шифруются с помощью ключей, управляемых клиентом (CMK), то сохраняются Key Vault секреты. Система абстрагирует это изменение от Log Analytics пользователей. Пользователи могут просто запросить рабочую область, как обычно. Система выполняет запросы между кластерами в серверной части по мере необходимости без указания пользователям.  

> [!WARNING] 
> В течение месяца существует ограничение на две операции связывания для рабочей области. Примите во внимание время, чтобы учесть и спланировать отсоединение действий соответствующим образом. 

## <a name="delete-a-dedicated-cluster"></a>Удаление кластера ценовой категории "Выделенный"

Выделенный ресурс кластера можно удалить. Необходимо разорвать связь всех рабочих областей с кластером, прежде чем удалять их. Чтобы выполнить эту операцию, требуются разрешения на "запись" для ресурса *Кластер*. 

После удаления ресурса кластера физический кластер переходит в процесс очистки и удаления. При удалении кластера удаляются все данные, хранящиеся в кластере. Данные могут быть из рабочих областей, которые были связаны с кластером в прошлом.

Ресурс *Кластер*, удаленный за последние 14 дней, находится в состоянии обратимого удаления, и его можно восстановить вместе с его данными. Так как все рабочие области не были связаны с *кластерным* ресурсом с удалением ресурса *кластера* , необходимо повторно связать рабочие области после восстановления. Пользователь не может выполнить операцию восстановления, обратившись к каналу Майкрософт или поддержку запросов на восстановление.

В течение 14 дней после удаления имя ресурса кластера зарезервировано и не может использоваться другими ресурсами.

**PowerShell**

Для удаления кластера используйте следующую команду PowerShell:

  ```powershell
  Remove-AzOperationalInsightsCluster -ResourceGroupName "resource-group-name" -ClusterName "cluster-name"
  ```

**REST**

Для удаления кластера используйте следующий вызов функции RESTFUL:

  ```rst
  DELETE https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2020-08-01
  Authorization: Bearer <token>
  ```

  **Ответ**

  200 ОК



## <a name="next-steps"></a>Следующие шаги

- Дополнительные сведения о [log Analytics выставлении счетов за выделенный кластер](../platform/manage-cost-storage.md#log-analytics-dedicated-clusters)
- Сведения о [правильном проектировании рабочих областей log Analytics](../platform/design-logs-deployment.md)
