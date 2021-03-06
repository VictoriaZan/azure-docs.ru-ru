---
title: включить файл
description: включить файл
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 06/15/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: c696bea902a25571c173d04e0eaa7304ed657151
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "85610455"
---
1. Убедитесь, что у вас установлена последняя версия [Azure PowerShell](/powershell/azure/install-az-ps) и вы вошли в учетную запись Azure с помощью командлета Connect-AzAccount.

1. Создайте экземпляр Azure Key Vault и ключ шифрования.

    При создании экземпляра Key Vault необходимо включить обратимое удаление и защиту от удаления. Обратимое удаление гарантирует, что Key Vault будет хранить удаленный ключ в течение заданного срока (по умолчанию 90 дней). Защита от удаления гарантирует, что удаленный ключ нельзя удалить навсегда до истечения срока хранения. Эти параметры защищают от потери данных из-за случайного удаления. Эти параметры являются обязательными при использовании Key Vault для шифрования управляемых дисков.
    
    ```powershell
    $ResourceGroupName="yourResourceGroupName"
    $LocationName="westcentralus"
    $keyVaultName="yourKeyVaultName"
    $keyName="yourKeyName"
    $keyDestination="Software"
    $diskEncryptionSetName="yourDiskEncryptionSetName"

    $keyVault = New-AzKeyVault -Name $keyVaultName -ResourceGroupName $ResourceGroupName -Location $LocationName -EnableSoftDelete -EnablePurgeProtection

    $key = Add-AzKeyVaultKey -VaultName $keyVaultName -Name $keyName -Destination $keyDestination  
    ```

1.    Создайте экземпляр DiskEncryptionSet. 
    
        ```powershell
        $desConfig=New-AzDiskEncryptionSetConfig -Location $LocationName -SourceVaultId $keyVault.ResourceId -KeyUrl $key.Key.Kid -IdentityType SystemAssigned
        
        $des=New-AzDiskEncryptionSet -Name $diskEncryptionSetName -ResourceGroupName $ResourceGroupName -InputObject $desConfig 
        ```

1.    Предоставьте ресурсу DiskEncryptionSet доступ к хранилищу ключей.

        > [!NOTE]
        > Создание удостоверения DiskEncryptionSet в Azure Active Directory может занять несколько минут. Если при выполнении следующей команды появляется сообщение об ошибке вида "Не удается найти объект Active Directory", подождите несколько минут и повторите попытку.
        
        ```powershell  
        Set-AzKeyVaultAccessPolicy -VaultName $keyVaultName -ObjectId $des.Identity.PrincipalId -PermissionsToKeys wrapkey,unwrapkey,get
        ```
