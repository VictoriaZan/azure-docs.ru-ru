---
title: Обновление сертификата шлюза приложений Azure
description: Узнайте, как обновить сертификат, связанный с прослушивателем шлюза приложений.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: how-to
ms.date: 8/15/2018
ms.author: victorh
ms.openlocfilehash: 413ae2ee19f0b8e427de9167b52971e413cdf573
ms.sourcegitcommit: 0ce1ccdb34ad60321a647c691b0cff3b9d7a39c8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93397235"
---
# <a name="renew-application-gateway-certificates"></a>Обновление сертификатов шлюза приложений

В какой-то момент вам потребуется продлить сертификаты, если вы настроили шлюз приложений для шифрования TLS/SSL.

Сертификат, связанный с прослушивателем, можно обновить с помощью портала Azure, Azure PowerShell или Azure CLI:

## <a name="azure-portal"></a>Портал Azure

Чтобы обновить сертификат прослушивателя на портале, перейдите к прослушивателям шлюза приложений. Щелкните прослушиватель, сертификат которого необходимо обновить, и выберите **Продлить или изменить выбранный сертификат**.

![Продление сертификата](media/renew-certificate/ssl-cert.png)

Передайте новый сертификат в формате PFX, присвойте ему имя, введите пароль и нажмите кнопку **Сохранить**.

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Чтобы обновить сертификат с помощью Azure PowerShell, используйте следующий сценарий:

```azurepowershell-interactive
$appgw = Get-AzApplicationGateway `
  -ResourceGroupName <ResourceGroup> `
  -Name <AppGatewayName>

$password = ConvertTo-SecureString `
  -String "<password>" `
  -Force `
  -AsPlainText

set-AzApplicationGatewaySSLCertificate -Name <oldcertname> `
-ApplicationGateway $appgw -CertificateFile <newcertPath> -Password $password

Set-AzApplicationGateway -ApplicationGateway $appgw
```
## <a name="azure-cli"></a>Azure CLI

```azurecli-interactive
az network application-gateway ssl-cert update \
  -n "<CertName>" \
  --gateway-name "<AppGatewayName>" \
  -g "ResourceGroupName>" \
  --cert-file <PathToCerFile> \
  --cert-password "<password>"
```

## <a name="next-steps"></a>Дальнейшие действия

Сведения о настройке разгрузки TLS с помощью шлюза приложений Azure см. в статье [Настройка разгрузки TLS](./create-ssl-portal.md) .