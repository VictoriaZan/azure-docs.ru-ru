---
title: Импорт среды Postman для вызовов REST в службах мультимедиа Azure
description: В этом разделе приведено определение среды Postman для вызовов REST в службах мультимедиа Azure.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2019
ms.author: juliako
ms.openlocfilehash: 2ccc765a3fb0063d92382edcec260808183f27a2
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "89259257"
---
# <a name="import-the-postman-environment"></a>Импорт среды Postman

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)] 

Эта статья содержит определение переменных среды **Postman**, которые используются в [коллекции Postman](postman-collection.md), содержащий сгруппированные HTTP-запросы для вызова REST API служб мультимедиа. Файлы среды и коллекции используются в учебнике[Configure Postman for Media Services REST API calls](media-rest-apis-with-postman.md) (Настройка Postman для вызовов REST API служб мультимедиа).

> [!NOTE]
> Значение `AzureADSTSEndpoint `  =  `https://login.microsoftonline.com/{{TenantId}}/oauth2/token` . Чтобы получить идентификатор клиента, можно навести указатель мыши на имя пользователя на портале (в правом верхнем углу) и поместить его в каталог: Microsoft ({{TENANTID}}).

```
{
  "id": "2dbce3ce-74c2-2ceb-0461-c4c2323f5b09",
  "name": "AzureMedia",
  "values": [
    {
      "enabled": true,
      "key": "TenantID",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "AzureADSTSEndpoint",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "RESTAPIEndpoint",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "ClientID",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "ClientSecret",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "AccessToken",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "LastAssetId",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "LastAccessPolicyId",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "UploadURL",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "MediaFileName",
      "value": "BigBuckBunny.mp4",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "LastChannelId",
      "value": "",
      "type": "text"
    }
  ],
  "_postman_variable_scope": "environment",
  "_postman_exported_using": "Postman/5.5.0"
}
```
