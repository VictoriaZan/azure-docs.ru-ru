---
title: Краткое руководство. Добавление чата в приложение
titleSuffix: An Azure Communication Services quickstart
description: В этом кратком руководстве показано, как добавить в приложение чат Служб коммуникации.
author: fanche
manager: phans
services: azure-communication-services
ms.author: mikben
ms.date: 09/30/2020
ms.topic: quickstart
ms.service: azure-communication-services
zone_pivot_groups: acs-js-csharp-java-python
ms.openlocfilehash: 2d0d80be367dda3689566dec2ade6fd7fc7c01fc
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/05/2020
ms.locfileid: "91665383"
---
# <a name="quickstart-add-chat-to-your-app"></a>Краткое руководство. Добавление чата в приложение

[!INCLUDE [Public Preview Notice](../../includes/public-preview-include.md)]

Начало работы со Службами коммуникации Azure с помощью клиентской библиотеки чатов Служб коммуникации для добавления в приложение чата с поддержкой реального времени. В этом кратком руководстве показано, как использовать клиентскую библиотеку чатов, чтобы создавать цепочки чатов, с помощью которых пользователи смогут общаться между собой. См. сведения о [чатах](../../concepts/chat/concepts.md) в документации.

::: zone pivot="programming-language-javascript"
[!INCLUDE [Chat with JavaScript client library](./includes/chat-js.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [Chat with Python client library](./includes/chat-python.md)]
::: zone-end

::: zone pivot="programming-language-java"
[!INCLUDE [Chat with Java client library](./includes/chat-java.md)]
::: zone-end

::: zone pivot="programming-language-csharp"
[!INCLUDE [Chat with C# client library](./includes/chat-csharp.md)]
::: zone-end

## <a name="clean-up-resources"></a>Очистка ресурсов

Если вы хотите очистить и удалить подписку на Службы коммуникации, вы можете удалить ресурс или группу ресурсов. При этом удаляются все ресурсы, связанные с ней. См. сведения об [очистке ресурсов](../create-communication-resource.md#clean-up-resources).

## <a name="next-steps"></a>Дальнейшие действия

Из этого руководства вы узнали, как сделать следующее:

> [!div class="checklist"]
> * создать клиент чата;
> * создать цепочку с двумя пользователями;
> * отправить сообщение в цепочку;
> * получить сообщения из цепочки;
> * исключить пользователей из цепочки.

> [!div class="nextstepaction"]
> [Ознакомьтесь с главным имиджевым приложением чата](../../samples/chat-hero-sample.md)

Возможно, вы также захотите:

 - узнать о [понятиях, связанных с чатами](../../concepts/chat/concepts.md);
 - ознакомиться с нашей [клиентской библиотекой чатов](../../concepts/chat/sdk-features.md).
