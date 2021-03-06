---
title: Подключение хранилища BLOB-объектов Azure с помощью протокола NFS 3,0 (Предварительная версия) | Документация Майкрософт
description: Узнайте, как подключить контейнер в хранилище BLOB-объектов из виртуальной машины Azure или клиента, работающего локально, с помощью протокола NFS 3,0.
author: normesta
ms.subservice: blobs
ms.service: storage
ms.topic: conceptual
ms.date: 08/04/2020
ms.author: normesta
ms.reviewer: yzheng
ms.custom: references_regions
ms.openlocfilehash: 7419e8667f07eec03e860634c7b3fddcac0e186b
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/25/2020
ms.locfileid: "95901559"
---
# <a name="mount-blob-storage-by-using-the-network-file-system-nfs-30-protocol-preview"></a>Подключение хранилища BLOB-объектов с помощью протокола NFS 3,0 (Предварительная версия)

Контейнер в хранилище BLOB-объектов можно подключить из виртуальной машины Azure под управлением Windows или Linux или системы Windows или Linux, которая выполняется локально с помощью протокола NFS 3,0. В этой статье приводятся пошаговые инструкции. Дополнительные сведения о поддержке протокола NFS 3,0 в хранилище больших двоичных объектов см. в статье [Поддержка протоколов nfs 3,0 в хранилище BLOB-объектов Azure (Предварительная версия)](network-file-system-protocol-support.md).

> [!NOTE]
> Поддержка протокола NFS 3,0 в хранилище BLOB-объектов Azure доступна в общедоступной предварительной версии. она доступна в следующих регионах: Восточная часть США, Центральная часть США, Центральная часть США, Юго-Восточная Австралия, Северная Европа, западная часть Соединенного Королевства, Центральная Корея, Южная Корея и Центральная Канада.

## <a name="step-1-register-the-nfs-30-protocol-feature-with-your-subscription"></a>Шаг 1. регистрация функции протокола NFS 3,0 в подписке

1. Откройте командное окно PowerShell. 

2. Войдите в подписку Azure с помощью команды `Connect-AzAccount` и следуйте инструкциям на экране.

   ```powershell
   Connect-AzAccount
   ```

3. Если ваше удостоверение связано с более чем одной подпиской, установите активную подписку.

   ```powershell
   $context = Get-AzSubscription -SubscriptionId <subscription-id>
   Set-AzContext $context
   ```
   
   Замените значение заполнителя `<subscription-id>` идентификатором своей подписки.

4. Зарегистрируйте `AllowNFSV3` компонент с помощью следующей команды.

   ```powershell
   Register-AzProviderFeature -FeatureName AllowNFSV3 -ProviderNamespace Microsoft.Storage 
   ```

5. Зарегистрируйте `PremiumHns` функцию с помощью следующей команды.

   ```powershell
   Register-AzProviderFeature -FeatureName PremiumHns -ProviderNamespace Microsoft.Storage  
   ```

6. Зарегистрируйте поставщик ресурсов с помощью следующей команды.
    
   ```powershell
   Register-AzResourceProvider -ProviderNamespace Microsoft.Storage   
   ```

## <a name="step-2-verify-that-the-feature-is-registered"></a>Шаг 2. Проверка регистрации компонента 

Утверждение регистрации может занять до часа. Чтобы убедиться, что регистрация завершена, используйте следующие команды.

```powershell
Get-AzProviderFeature -ProviderNamespace Microsoft.Storage -FeatureName AllowNFSV3
Get-AzProviderFeature -ProviderNamespace Microsoft.Storage -FeatureName PremiumHns  
```

## <a name="step-3-create-an-azure-virtual-network-vnet"></a>Шаг 3. Создание виртуальной сети Azure (VNet)

Ваша учетная запись хранения должна содержаться в виртуальной сети. Виртуальная сеть позволяет клиентам безопасно подключаться к учетной записи хранения. Дополнительные сведения о виртуальной сети и о том, как ее создать, см. в [документации по виртуальным сетям](../../virtual-network/index.yml).

> [!NOTE]
> Клиенты в одной виртуальной сети могут подключать контейнеры в своей учетной записи. Вы также можете подключить контейнер из клиента, работающего в локальной сети, но сначала необходимо подключить локальную сеть к виртуальной сети. См. раздел [Поддерживаемые сетевые подключения](network-file-system-protocol-support.md#supported-network-connections).

## <a name="step-4-configure-network-security"></a>Шаг 4. Настройка сетевой безопасности

Единственный способ защитить данные в учетной записи — использовать виртуальную сеть и другие параметры безопасности сети. Любой другой инструмент, используемый для защиты данных, включая авторизацию ключа учетной записи, безопасность Azure Active Directory (AD) и списки управления доступом (ACL), пока не поддерживаются в учетных записях с включенной поддержкой протокола NFS 3,0. 

Чтобы защитить данные в учетной записи, см. следующие рекомендации: [рекомендации по сетевой безопасности для хранилища BLOB-объектов](security-recommendations.md#networking).

## <a name="step-5-create-and-configure-a-storage-account"></a>Шаг 5. Создание и настройка учетной записи хранения

Чтобы подключить контейнер с помощью NFS 3,0, необходимо создать учетную запись хранения **после** регистрации этой функции в подписке. Вы не можете включить учетные записи, существовавшие до регистрации функции. 

В предварительной версии этой функции протокол NFS 3,0 поддерживается только в учетных записях [блоккблобстораже](../blobs/storage-blob-create-account-block-blob.md) .

При настройке учетной записи выберите следующие значения:

|Параметр | Значение|
|----|---|
|Расположение|Одна из следующих областей: Восток США, Центральная часть США, Центральная часть США, Юго-Восточная Австралия, Северная Европа, западная часть Соединенного Королевства, Центральная Корея, Южная Корея и Центральная Канада |
|Производительность|Premium|
|Тип учетной записи|BlockBlobStorage|
|Репликация|Локально избыточное хранилище (LRS)|
|Метод подключения|Общедоступная конечная точка (выбранные сети) или частная конечная точка|
|Требуется безопасная передача данных|Выключено|
|Иерархическое пространство имен|Активировано|
|NFS V3|Активировано|

Можно принять значения по умолчанию для всех остальных параметров. 

## <a name="step-6-create-a-container"></a>Шаг 6. Создание контейнера

Создайте контейнер в учетной записи хранения с помощью любого из этих средств или пакетов SDK:

|Инструменты|Пакеты SDK|
|---|---|
|[Портал Azure](https://portal.azure.com)|[.NET](data-lake-storage-directory-file-acl-dotnet.md#create-a-container)|
|[AzCopy](../common/storage-use-azcopy-blobs.md#create-a-container)|[Java](data-lake-storage-directory-file-acl-java.md#create-a-container)|
|[PowerShell](data-lake-storage-directory-file-acl-powershell.md#create-a-container)|[Python](data-lake-storage-directory-file-acl-python.md#create-a-container)|
|[Azure CLI](data-lake-storage-directory-file-acl-cli.md#create-a-container);|[JavaScript](data-lake-storage-directory-file-acl-javascript.md)|
||[REST](/rest/api/storageservices/create-container)|

## <a name="step-7-mount-the-container"></a>Шаг 7. Подключение контейнера

Создайте каталог в системе Windows или Linux, а затем подключите контейнер в учетной записи хранения.

### <a name="linux"></a>[Linux](#tab/linux)

1. В системе Linux создайте каталог.

   ```
   mkdir -p /mnt/test
   ```

2. Подключите контейнер с помощью следующей команды.

   ```
   mount -o sec=sys,vers=3,nolock,proto=tcp <storage-account-name>.blob.core.windows.net:/<storage-account-name>/<container-name>  /mnt/test
   ```

   - Замените `<storage-account-name>` заполнитель, который отображается в этой команде, именем вашей учетной записи хранения.  

   - Замените `<container-name>` заполнитель именем своего контейнера.


### <a name="windows"></a>[Windows](#tab/windows)

1. Откройте диалоговое окно **компоненты Windows** , а затем включите функцию **клиент для NFS** . 

   ![Клиент для сетевой файловой системы](media/network-file-system-protocol-how-to/client-for-network-files-system-feature.png)

2. Подключите контейнер с помощью команды [Mount](/windows-server/administration/windows-commands/mount) .

   ```
   mount -o nolock <storage-account-name>.blob.core.windows.net:/<storage-account-name>/<container-name> *
   ```

   - Замените `<storage-account-name>` заполнитель, который отображается в этой команде, именем вашей учетной записи хранения.  

   - Замените `<container-name>` заполнитель именем своего контейнера.

3. Если требуются разрешения на запись, может потребоваться изменить UID и GID по умолчанию, используемые Windows для подключения к общей папке. Для этого выполните следующие команды PowerShell от имени администратора:

   ```
   New-ItemProperty -Path HKLM:\SOFTWARE\Microsoft\ClientForNFS\CurrentVersion\Default -Name AnonymousUid -PropertyType DWord -Value 0
   New-ItemProperty -Path HKLM:\SOFTWARE\Microsoft\ClientForNFS\CurrentVersion\Default -Name AnonymousGid -PropertyType DWord -Value 0
   ```
   
   - Перезапустите службу клиента NFS или перезагрузите сервер после внесения этого изменения.

---

## <a name="resolve-common-issues"></a>Устранение распространенных проблем

|Вопрос или ошибка | Разрешение|
|---|---|
|`Access denied by server while mounting`|Убедитесь, что клиент работает в поддерживаемой подсети. См. раздел [Поддерживаемые сетевые расположения](network-file-system-protocol-support.md#supported-network-connections).|
|`No such file or directory`| Убедитесь, что подключенный контейнер был создан после того, как подтвердилась регистрация этой возможности. См. [Шаг 2. Убедитесь, что компонент зарегистрирован](#step-2-verify-that-the-feature-is-registered). Кроме того, обязательно введите команду mount и ее параметры непосредственно в терминале. Если вы скопируете любую часть этой команды в терминал из другого приложения, скрытые символы в скопированных данных могут вызвать ошибку.|

## <a name="see-also"></a>См. также раздел

[Поддержка протокола NFS 3,0 в хранилище BLOB-объектов Azure (Предварительная версия)](network-file-system-protocol-support.md)