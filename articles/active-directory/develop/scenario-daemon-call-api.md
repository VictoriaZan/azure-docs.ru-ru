---
title: Вызов веб-API из управляющего приложения — платформа Microsoft Identity | Службы
description: Узнайте, как создать управляющее приложение, вызывающее веб-API.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 10/30/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 0dfd729a48b7e81028078fd035b3b900f4d7b9bc
ms.sourcegitcommit: 6109f1d9f0acd8e5d1c1775bc9aa7c61ca076c45
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2020
ms.locfileid: "94444126"
---
# <a name="daemon-app-that-calls-web-apis---call-a-web-api-from-the-app"></a>Управляющее приложение, вызывающее веб-API — вызов веб-API из приложения

Приложения управляющей программы .NET могут вызывать веб-API. Приложения управляющей программы .NET также могут вызывать несколько предварительно утвержденных веб-API.

## <a name="calling-a-web-api-from-a-daemon-application"></a>Вызов веб-API из управляющего приложения

Вот как можно использовать маркер для вызова API:

# <a name="net"></a>[.NET](#tab/dotnet)

[!INCLUDE [Call web API in .NET](../../../includes/active-directory-develop-scenarios-call-apis-dotnet.md)]

# <a name="python"></a>[Python](#tab/python)

```Python
endpoint = "url to the API"
http_headers = {'Authorization': 'Bearer ' + result['access_token'],
                'Accept': 'application/json',
                'Content-Type': 'application/json'}
data = requests.get(endpoint, headers=http_headers, stream=False).json()
```

# <a name="java"></a>[Java](#tab/java)

```Java
HttpURLConnection conn = (HttpURLConnection) url.openConnection();

// Set the appropriate header fields in the request header.
conn.setRequestProperty("Authorization", "Bearer " + accessToken);
conn.setRequestProperty("Accept", "application/json");

String response = HttpClientHelper.getResponseStringFromConn(conn);

int responseCode = conn.getResponseCode();
if(responseCode != HttpURLConnection.HTTP_OK) {
    throw new IOException(response);
}

JSONObject responseObject = HttpClientHelper.processResponse(responseCode, response);
```

---

## <a name="calling-several-apis"></a>Вызов нескольких API

Для приложений управляющей программы необходимо предварительно утвердить веб-API, которые вызываются. Нет добавочного согласия с приложениями управляющей программы. (Нет взаимодействия с пользователем.) Администратор клиента должен заранее предоставить согласие на доступ к приложению и всем разрешениям API. Если требуется вызвать несколько API, необходимо получить маркер для каждого ресурса, каждый раз вызывая `AcquireTokenForClient` . MSAL будет использовать кэш маркеров приложений, чтобы избежать ненужных вызовов служб.

## <a name="next-steps"></a>Дальнейшие действия

# <a name="net"></a>[.NET](#tab/dotnet)

Перейдите к следующей статье в этом сценарии, [перейдите в рабочую среду](./scenario-daemon-production.md?tabs=dotnet).

# <a name="python"></a>[Python](#tab/python)

Перейдите к следующей статье в этом сценарии, [перейдите в рабочую среду](./scenario-daemon-production.md?tabs=python).

# <a name="java"></a>[Java](#tab/java)

Перейдите к следующей статье в этом сценарии, [перейдите в рабочую среду](./scenario-daemon-production.md?tabs=java).

---
