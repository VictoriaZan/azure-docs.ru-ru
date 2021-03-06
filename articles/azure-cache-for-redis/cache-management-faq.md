---
title: Вопросы и ответы по управлению кэшем Azure для Redis
description: Ознакомьтесь с ответами на распространенные вопросы, которые помогут управлять кэшем Azure для Redis.
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.topic: conceptual
ms.custom: devx-track-csharp
ms.date: 08/06/2020
ms.openlocfilehash: 15c7ed4ca9d04e4bb314eea8b92bef749d2369b1
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/26/2020
ms.locfileid: "92537666"
---
# <a name="azure-cache-for-redis-management-faqs"></a>Вопросы и ответы по управлению кэшем Azure для Redis
В этой статье содержатся ответы на часто задаваемые вопросы об управлении кэшем Azure для Redis.

## <a name="common-questions-and-answers"></a>Общие вопросы и ответы
В этом разделе рассматриваются следующие вопросы и ответы:

* [Когда следует включать порт, не являющийся портом TLS/SSL, для подключения к Redis?](#when-should-i-enable-the-non-tlsssl-port-for-connecting-to-redis)
* [Какие рекомендации следует учитывать для рабочей среды?](#what-are-some-production-best-practices)
* [Какие факторы следует учитывать при использовании общих команд Redis?](#what-are-some-of-the-considerations-when-using-common-redis-commands)
* [Как измерить и протестировать производительность моего кэша?](#how-can-i-benchmark-and-test-the-performance-of-my-cache)
* [Важные сведения о росте пула потоков ThreadPool](#important-details-about-threadpool-growth)
* [Включение сборки мусора сервера для увеличения пропускной способности на стороне клиента при использовании StackExchange.Redis](#enable-server-gc-to-get-more-throughput-on-the-client-when-using-stackexchangeredis)
* [Рекомендации по производительности для подключений](#performance-considerations-around-connections)

### <a name="when-should-i-enable-the-non-tlsssl-port-for-connecting-to-redis"></a>Когда следует включать порт, не являющийся портом TLS/SSL, для подключения к Redis?
Сервер Redis не имеет встроенной поддержки TLS, но кэш Azure для Redis имеет. Если вы подключаетесь к кэшу Azure для Redis и ваш клиент поддерживает протокол TLS, например StackExchange.Redis, то следует использовать TLS.

>[!NOTE]
>По умолчанию порт без TLS отключен для новых экземпляров кэша Azure для Redis. Если ваш клиент не поддерживает TLS, необходимо включить порт, не являющийся портом TLS, следуя указаниям, приведенным в разделе [Порты доступа](cache-configure.md#access-ports) статьи [Настройка кэша Azure для Redis](cache-configure.md).
>
>

Инструменты Redis, такие как `redis-cli`, не работают с портом TLS, но вы можете использовать служебную программу, например `stunnel`, для безопасного подключения инструментов к порту TLS в соответствии с указаниями в записи блога [Анонс поставщика состояний сеансов ASP.NET для предварительной версии Redis](https://devblogs.microsoft.com/aspnet/announcing-asp-net-session-state-provider-for-redis-preview-release/).

Инструкции по скачиванию инструментов Redis см. в разделе [Как выполнять команды Redis?](cache-development-faq.md#how-can-i-run-redis-commands).

### <a name="what-are-some-production-best-practices"></a>Какие рекомендации следует учитывать для рабочей среды?
* [Рекомендации для StackExchange.Redis](#stackexchangeredis-best-practices)
* [Конфигурация и основные понятия](#configuration-and-concepts)
* [Тестирование производительности](#performance-testing)

#### <a name="stackexchangeredis-best-practices"></a>Рекомендации для StackExchange.Redis
* Задайте для `AbortConnect` значение false, подождите, пока ConnectionMultiplexer автоматически восстановит подключение. [Щелкните здесь, чтобы узнать больше](https://gist.github.com/JonCole/36ba6f60c274e89014dd#file-se-redis-setabortconnecttofalse-md).
* Повторно используйте ConnectionMultiplexer — не создавайте новый экземпляр для каждого запроса. Рекомендуется использовать шаблон `Lazy<ConnectionMultiplexer>`, [показанный здесь](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache).
* Лучше всего Redis работает с меньшими значениями, поэтому рассмотрите возможность разделения больших данных на несколько ключей. В [этом обсуждении Redis](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ) размер в 100 КБ признан "большим". Прочитайте [эту статью](https://gist.github.com/JonCole/db0e90bedeb3fc4823c2#large-requestresponse-size) , чтобы ознакомиться с примером проблемы, вызванной большими значениями.
* Настройте [параметры ThreadPool](#important-details-about-threadpool-growth) , чтобы избежать тайм-аутов.
* Используйте по крайней мере значение connectTimeout по умолчанию, равное 5 секундам. Этот интервал дает StackExchange.Redis достаточно времени, чтобы повторно установить подключение в случае кратковременного сбоя сети.
* Учтите расходы на производительность, связанные с другими операциями, которые вы выполняете. Например, команда `KEYS` является операцией O(n), т. е. операцией, длительность которой линейно зависит от размера входных данных. Таких операций следует избегать. [Сайт redis.io](https://redis.io/commands/) содержит сведения о временной сложности каждой поддерживаемой операции. Щелкните каждую команду, чтобы просмотреть сведения о сложности операции.

#### <a name="configuration-and-concepts"></a>Конфигурация и основные понятия
* Для систем в рабочей среде используйте уровень "Стандартный" или "Премиум". Уровень "Базовый" — это система с одним узлом без репликации данных, которая не регулируется соглашением об уровне обслуживания. Кроме того, используйте по крайней мере кэш C1. Как правило, кэши C0 используются в простых сценариях разработки и тестирования.
* Помните, что Redis — это хранилище данных **в памяти** . Прочитайте [эту статью](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md) , чтобы ознакомиться со случаями, в которых может произойти потеря данных.
* Создайте свою систему таким образом, чтобы она могла справляться с кратковременными сбоями подключения, [связанными с установкой исправлений и отработкой отказа](https://gist.github.com/JonCole/317fe03805d5802e31cfa37e646e419d#file-azureredis-patchingexplained-md).

#### <a name="performance-testing"></a>Тестирование производительности
* Начните с запуска `redis-benchmark.exe` , чтобы определить возможную пропускную способность перед созданием собственных тестов производительности. Так как `redis-benchmark` не поддерживает протокол TLS, то перед запуском теста необходимо [включить порт без TLS посредством портала Azure](cache-configure.md#access-ports). Примеры приведены в разделе [Как измерить и протестировать производительность моего кэша?](#how-can-i-benchmark-and-test-the-performance-of-my-cache)
* Клиентская виртуальная машина, используемая для тестирования, должна находиться в том же регионе, что и экземпляр кэша Azure для Redis.
* В качестве клиента рекомендуется использовать виртуальные машины серии Dv2, так как в них используется более производительное оборудование и они выдают лучшие результаты.
* Убедитесь, что у выбранной клиентской виртуальной машины как минимум такая же емкость вычислительных ресурсов и пропускной способности, что и у тестируемого кэша.
* Включите VRSS на клиентском компьютере, если используется среда Windows. [Щелкните здесь, чтобы узнать больше](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn383582(v=ws.11)).
* Экземпляры Redis уровня "Премиум" обеспечивают меньшие сетевые задержки и большую пропускную способность, так как используют более производительные ЦП и сетевое оборудование.

### <a name="what-are-some-of-the-considerations-when-using-common-redis-commands"></a>Какие факторы следует учитывать при использовании общих команд Redis?

* Избегайте определенных команд Redis, которые выполняются слишком долго, если не до конца понимаете влияние этих команд. Например, не запускайте команду [KEYS](https://redis.io/commands/keys) в рабочей среде. В зависимости от числа ключей выполнение может занять много времени. Redis является однопотоковым сервером и обрабатывает команды по одной. Если вы выдадите после KEYS другие команды, они не будут обрабатываться, пока Redis обрабатывает команду KEYS. [Сайт redis.io](https://redis.io/commands/) содержит сведения о временной сложности каждой поддерживаемой операции. Щелкните каждую команду, чтобы просмотреть сведения о сложности операции.
* Размеры ключей — следует использовать пары "ключ-значение" небольшого или большого размера? Это зависит от сценария развертывания. Если в сценарии требуются большие ключи, то можно настроить значения повторов и ConnectionTimeout, а также настроить логику повторов. С точки зрения сервера Redis чем меньше значения, тем выше производительность.
* Это не означает, что в Redis нельзя хранить большие значения; вы должны учитывать описанные ниже факторы. Задержки будут выше. Если имеется один большой и один небольшой набор данных, можно использовать несколько экземпляров ConnectionMultiplexer, настроенных с разным набором значений времени ожидания и повторов, как описано в предыдущем разделе [Что делают параметры конфигурации StackExchange.Redis](cache-development-faq.md#what-do-the-stackexchangeredis-configuration-options-do).

### <a name="how-can-i-benchmark-and-test-the-performance-of-my-cache"></a>Как измерить и протестировать производительность моего кэша?
* [Включите диагностику кэша](cache-how-to-monitor.md#enable-cache-diagnostics), чтобы можно было [наблюдать](cache-how-to-monitor.md) за работоспособностью кэша. Вы можете просмотреть метрики на портале Azure. Кроме того, их можно [скачать и просмотреть](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) с помощью привычных вам инструментов.
* Для нагрузочного теста сервера Redis можно использовать benchmark.exe.
* Убедитесь, что клиент нагрузочного тестирования и кэш Azure для Redis находятся в одном регионе.
* Используйте redis-cli.exe и наблюдайте за кэшем с помощью команды INFO.
* Если ваша нагрузка приводит к высокой степени фрагментации памяти, то необходимо масштабирование до большего размера кэша.
* Инструкции по скачиванию инструментов Redis см. в разделе [Как выполнять команды Redis?](cache-development-faq.md#how-can-i-run-redis-commands).

Следующие команды приводят пример использования redis-benchmark.exe. Чтобы получить точные результаты, выполните эти команды на виртуальной машине, размещенной в том же регионе, что и кэш.

* Тестирование конвейерных запросов SET с полезными данными размером в 1 КБ.

  `redis-benchmark.exe -h **yourcache**.redis.cache.windows.net -a **yourAccesskey** -t SET -n 1000000 -d 1024 -P 50`
* Тестирование конвейерных запросов GET с полезными данными размером в 1 КБ.

>[!NOTE]
> Сначала выполните приведенный выше тест SET, чтобы заполнить кэш
>

  `redis-benchmark.exe -h **yourcache**.redis.cache.windows.net -a **yourAccesskey** -t GET -n 1000000 -d 1024 -P 50`

### <a name="important-details-about-threadpool-growth"></a>Важные сведения о росте пула потоков ThreadPool
В CLR ThreadPool существует два типа потоков — "Рабочий поток" и "Порт завершения ввода-вывода" (IOCP).

* Рабочие потоки используются, например, для обработки методов `Task.Run(…)` или `ThreadPool.QueueUserWorkItem(…)`. Эти потоки также используются различными компонентами в среде CLR, когда работа должна быть выполнена в фоновом потоке.
* Потоки IOCP используются в случае асинхронного ввода-вывода, например при чтении из сети.

Пул потоков предоставляет новые рабочие потоки или потоки завершения ввода-вывода по запросу (без регулирования), пока не будет достигнут параметр "Минимум" для каждого типа потока. По умолчанию минимальное количество потоков равно количеству процессоров в системе.

Когда количество существующих (занятых) потоков достигает "минимального" количества потоков, ThreadPool изменяет частоту внедрения новых потоков до величины в один поток за 500 миллисекунд. Как правило, если нагрузка системы достигает величины, для которой необходим поток IOCP, то эта работа будет сделана быстро. Однако если величина нагрузки больше настроенного параметра "Минимум", при обработке некоторых операций возникнет некоторая задержка, так как ThreadPool будет ожидать возникновения одного из двух событий.

* Освобождение существующего потока для выполнения работы.
* Создание нового потока из-за того, что ни один из существующих потоков не стал доступным в течение 500 мс.

По сути, это означает, что если количество занятых потоков превышает минимальное количество потоков, вы, скорее всего, столкнетесь с задержкой в 500 мс перед обработкой сетевого трафика приложением. Кроме того, важно обратить внимание, что если существующий поток простаивает более 15 секунд (на основе моих наблюдений), он будет удален, и этот цикл наращивания и уменьшения количества потоков может повториться.

Если взглянуть на пример сообщения об ошибке от StackExchange.Redis (сборка 1.0.450 или более поздней версии), вы увидите, что теперь оно содержит статистику ThreadPool (подробности о рабочих потоках и потоках IOCP см. ниже).

```
System.TimeoutException: Timeout performing GET MyKey, inst: 2, mgr: Inactive,
queue: 6, qu: 0, qs: 6, qc: 0, wr: 0, wq: 0, in: 0, ar: 0,
IOCP: (Busy=6,Free=994,Min=4,Max=1000),
WORKER: (Busy=3,Free=997,Min=4,Max=1000)
```

В предыдущем примере можно увидеть, что для потока IOCP существует шесть занятых потоков, а минимальное количество потоков — четыре. В этом случае клиент, скорее всего, столкнется с двумя задержками по 500 мс, так как 6 больше 4.

Обратите внимание, что StackExchange.Redis может достигнуть времени ожидания, если происходит регулирование количества рабочих потоков или потоков IOCP.

#### <a name="recommendation"></a>Рекомендация

С учетом этой информации мы настоятельно рекомендуем клиентам устанавливать значение параметра "Минимум" для рабочих потоков и потоков IOCP так, чтобы оно превышало значение по умолчанию. Мы не можем дать универсальных рекомендаций для этого значения, так как подходящее значение для одного приложения будет слишком высоким или низким для другого. Этот параметр также может повлиять на производительность других компонентов сложных приложений, поэтому каждый клиент должен точно настроить этот параметр в соответствии со своими потребностями. Хорошей отправной точкой является 200 или 300, после чего нужно проверить и изменить этот параметр в соответствии с результатом.

Как настроить этот параметр:

* Рекомендуется изменить этот параметр программно с помощью метода [ThreadPool.SetMinThreads (...)](/dotnet/api/system.threading.threadpool.setminthreads#System_Threading_ThreadPool_SetMinThreads_System_Int32_System_Int32_) в `global.asax.cs`. Пример:

    ```csharp
    private readonly int minThreads = 200;
    void Application_Start(object sender, EventArgs e)
    {
        // Code that runs on application startup
        AreaRegistration.RegisterAllAreas();
        RouteConfig.RegisterRoutes(RouteTable.Routes);
        BundleConfig.RegisterBundles(BundleTable.Bundles);
        ThreadPool.SetMinThreads(minThreads, minThreads);
    }
    ```

    > [!NOTE]
    > Значение, указанное в этом методе, — это глобальный параметр, который влияет на весь домен приложения. Например, если у вас 4-ядерный компьютер и вам нужно установить для параметров *minWorkerThreads* и *minIOThreads* значение 50 на ЦП во время выполнения, используется **ThreadPool.SetMinThreads (200, 200)** .

* Кроме того, можно указать минимальное число потоков с помощью [параметра конфигурации *minIoThreads* или *minWorkerThreads*](/previous-versions/dotnet/netframework-4.0/7w2sway1(v=vs.100)) в элементе конфигурации `<processModel>` в `Machine.config`, обычно расположенном в `%SystemRoot%\Microsoft.NET\Framework\[versionNumber]\CONFIG\`. **Установка минимального числа потоков таким способом обычно не рекомендуется, так как это параметр на уровне системы.**

  > [!NOTE]
  > В этом элементе конфигурации задается параметр *per-core* . Например, если у вас 4-ядерный компьютер и вы хотите установить для параметра *minIOThreads* значение 200 во время выполнения, то можно использовать `<processModel minIoThreads="50"/>`.
  >

### <a name="enable-server-gc-to-get-more-throughput-on-the-client-when-using-stackexchangeredis"></a>Включение сборки мусора сервера для получения более высокой пропускной способности на стороне клиента при использовании StackExchange.Redis
Включение сборки мусора сервера может оптимизировать работу клиента и улучшить производительность и пропускную способность при использовании StackExchange.Redis. Дополнительные сведения о сборке мусора сервера и о том, как ее включить, можно найти в следующих статьях:

* [Включение сборки мусора сервера](/dotnet/framework/configure-apps/file-schema/runtime/gcserver-element)
* [Основные сведения о сборке мусора](/dotnet/standard/garbage-collection/fundamentals)
* [Сборка мусора и производительность](/dotnet/standard/garbage-collection/performance)

### <a name="performance-considerations-around-connections"></a>Рекомендации по производительности для подключений

Каждая ценовая категория имеет различные ограничения для количества клиентских подключений, памяти и пропускной способности. Хотя каждый размер кэша *допускает* определенное число подключений, с каждым подключением к Redis связаны накладные расходы. Примером таких накладных расходов могут служить загрузка ЦП и использование памяти в результате шифрования TLS/SSL. Максимальное число подключений для данного размера кэша предполагает низкую загрузку кэша. Если *суммарная* нагрузка, связанная с подключениями и клиентскими операциями, превышает емкость системы, могут возникнуть проблемы с работой кэша, даже если максимальное число подключений для его текущего размера не превышено.

Дополнительные сведения о разных пределах подключений для каждого уровня см. в статье [Цены на Кэш Azure для Redis](https://azure.microsoft.com/pricing/details/cache/). Дополнительные сведения о подключениях и других конфигурациях по умолчанию см. в статье [Конфигурация сервера Redis по умолчанию](cache-configure.md#default-redis-server-configuration).

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о другом [кэше Azure для Redis часто задаваемых вопросов](cache-faq.md).