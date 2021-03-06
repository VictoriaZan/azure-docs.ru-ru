---
title: Обзор функций Azure
description: Узнайте, как служба "Функции Azure" может помочь создать масштабируемые бессерверные приложения.
author: craigshoemaker
ms.assetid: 01d6ca9f-ca3f-44fa-b0b9-7ffee115acd4
ms.topic: overview
ms.date: 11/20/2020
ms.author: cshoe
ms.custom: contperfq2
ms.openlocfilehash: 514f2e9a82a50f95f9c054c6a54e7b5af3c0af15
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/26/2020
ms.locfileid: "96167783"
---
# <a name="introduction-to-azure-functions"></a>Введение в функции Azure

Мы часто создаем системы, чтобы реагировать на ряд критических событий. В случае возникновения этих событий каждое приложение должно иметь возможность запускать код независимо от того, что вы делаете: создаете веб-API, реагируете на изменения базы данных, обрабатываете потоки данных Интернета вещей или даже управляете очередями сообщений.

Для этого служба "Функции Azure" предоставляет возможность выполнять вычисления по запросу двумя основными способами.

Первый — служба "Функции Azure" позволяет реализовать логику системы в быстро доступных блоках кода. Эти блоки кода называются "функциями". Вы можете запускать различные функции в любое время для реагирования на критические события.

Второй — по мере увеличения количества запросов служба "Функции Azure" полностью удовлетворяет потребности в необходимом количестве ресурсов и экземпляров функций (но только при необходимости). По мере уменьшения количества запросов все дополнительные ресурсы и экземпляры приложений автоматически отключаются.

Откуда берутся все эти вычислительные ресурсы? Служба "Функции Azure" [предоставляет необходимое количество вычислительных ресурсов](./functions-scale.md) в соответствии с требованиями вашего приложения.

Суть [бессерверных вычислений](https://azure.microsoft.com/solutions/serverless/) службы "Функции Azure" — в предоставлении вычислительных ресурсов по запросу.

## <a name="scenarios"></a>Сценарии

Во многих случаях функция [интегрируется с массивом облачных служб](./functions-triggers-bindings.md), чтобы обеспечить широкие возможности для реализаций.

Ниже перечислены распространенные наборы сценариев для службы "Функции Azure". _Это неполный список сценариев_.

| Цель... | Действие… |
| --- | --- |
| **Создание веб-API** | Реализуйте конечную точку для веб-приложений с помощью [триггера HTTP](./functions-bindings-http-webhook.md) |
| **Обработка передаваемых файлов** | Выполните код при передаче или изменении файла в [хранилище BLOB-объектов](./functions-bindings-storage-blob.md) |
| **Создание бессерверного рабочего процесса** | Объедините набор функций с помощью [устойчивых функций](./durable/durable-functions-overview.md) |
| **Реагирование на изменения базы данных** | Запустите настраиваемую логику при создании или обновлении документа в [Cosmos DB](./functions-bindings-cosmosdb-v2.md) |
| **Выполнение запланированных задач** | Выполните код во [время установки](./functions-bindings-timer.md) |
| **Создание надежных систем очереди сообщений** | Обработайте очереди сообщений с помощью [Хранилища очередей](./functions-bindings-storage-queue.md), [служебной шины](./functions-bindings-service-bus.md) или [Центров событий](./functions-bindings-event-hubs.md) |
| **Анализ потоков данных Интернета вещей** | Получите и обработайте [данные с устройств Интернета вещей](./functions-bindings-event-iot.md) |
| **Обработка данных в реальном времени** | Используйте [службу "Функции Azure" и Signal R](./functions-bindings-signalr-service.md) для реагирования на данные в реальном времени |

При создании функций доступны следующие возможности и ресурсы:

- **Использование предпочитаемого языка.** Пишите функции на языке [C#, Java, JavaScript, PowerShell или Python](./supported-languages.md) или используйте [настраиваемый обработчик](./functions-custom-handlers.md), чтобы воспользоваться любым другим языком.

- **Автоматическое развертывание.** Существует [множество вариантов развертывания](./functions-deployment-technologies.md): от использования средств до применения внешних конвейеров.

- **Устранение неполадок функции.** Используйте [средства мониторинга](./functions-monitoring.md) и [стратегии тестирования](./functions-test-a-function.md), чтобы получить аналитические сведения о приложениях.

- **Гибкие тарифные планы.** Используя план [потребления](./pricing.md), вы платите только за время выполнения функций. Планы [службы приложений](./pricing.md) и [Премиум](./pricing.md) предлагают функции для особых потребностей.

## <a name="next-steps"></a>Next Steps

> [!div class="nextstepaction"]
> [Уроки, примеры и интерактивные учебники для начала работы](./functions-get-started.md)