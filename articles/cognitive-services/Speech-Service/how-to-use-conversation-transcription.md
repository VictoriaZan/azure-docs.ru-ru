---
title: Краткое руководство по голосовым беседам в режиме реального времени — служба речи
titleSuffix: Azure Cognitive Services
description: Узнайте, как использовать транскрипцию разговора в режиме реального времени с помощью речевого пакета SDK. Функция записи бесед позволяет транскрипция встречи и другие беседы с возможностью добавления, удаления и обнаружения нескольких участников путем потоковой передачи в службу распознавания речи.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/20/2020
ms.author: trbye
zone_pivot_groups: acs-js-csharp
ms.openlocfilehash: ae3d7b329c55ce0be101cee73e1fc7674a2d75aa
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/21/2020
ms.locfileid: "95026545"
---
# <a name="get-started-with-real-time-conversation-transcription"></a>Приступая к работе с обобщением в реальном времени

API **конверсатионтранскрибер** для речевых средств позволяет транскрипция встречи и другие беседы с возможностью добавления, удаления и обнаружения нескольких участников путем потоковой передачи в службу распознавания речи с помощью `PullStream` или `PushStream` . Сначала создайте подписи для каждого участника, используя REST API, а затем используйте подписи с помощью голоса с пакетом SDK, чтобы транскрипция беседы. Дополнительные сведения см. в [обзоре](conversation-transcription.md) транскрипции беседы.

## <a name="limitations"></a>Ограничения

* Доступно только в следующих регионах подписки: `centralus` , `eastasia` , `eastus` , `westeurope`
* Требуется 7-MIC циклический массив из нескольких микрофонов с потоком ссылки для воспроизведения. Массив микрофонов должен соответствовать [нашей спецификации](./speech-devices-sdk-microphone.md).
* [Пакет SDK для речевых устройств](speech-devices-sdk.md) предоставляет подходящие устройства и пример приложения, демонстрирующий запись разговора.

## <a name="prerequisites"></a>Предварительные требования

В этой статье предполагается, что у вас есть учетная запись Azure и подписка на службу "Речь". Если у вас нет учетной записи и подписки, [попробуйте службу "Речь" бесплатно](overview.md#try-the-speech-service-for-free).

::: zone pivot="programming-language-javascript"
[!INCLUDE [JavaScript Basics include](includes/how-to/conversation-transcription/real-time-javascript.md)]
::: zone-end

::: zone pivot="programming-language-csharp"
[!INCLUDE [C# Basics include](includes/how-to/conversation-transcription/real-time-csharp.md)]
::: zone-end

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Асинхронная транскрипция диалога](how-to-async-conversation-transcription.md) 
>  [Пример кода](https://github.com/Azure-Samples/Cognitive-Services-Speech-Devices-SDK/blob/master/Samples/Java/Android/Speech%20Devices%20SDK%20Starter%20App/example/app/src/main/java/com/microsoft/cognitiveservices/speech/samples/sdsdkstarterapp/ConversationTranscription.java) 
>  устройства Рубо [Пример кода для пакета Azure Kinect Dev Kit](https://github.com/Azure-Samples/Cognitive-Services-Speech-Devices-SDK/blob/master/Samples/Java/Windows_Linux/SampleDemo/src/com/microsoft/cognitiveservices/speech/samples/Cts.java)