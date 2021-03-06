---
title: События Service Fabric Azure
description: Сведения о событиях Service Fabric, предоставляемых для отслеживания кластера Azure Service Fabric.
author: srrengar
ms.topic: conceptual
ms.date: 11/21/2018
ms.author: srrengar
ms.openlocfilehash: 638b650e485ad3e83bd6021639a7e55b540d9cdc
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "75451734"
---
# <a name="service-fabric-events"></a>События Service Fabric 

Платформа Service Fabric записывает несколько структурированных событий для основных операционных действий, происходящих внутри вашего кластера. Они варьируются от обновления кластеров до решений о размещении реплик. Каждое событие, предоставляемое службой Service Fabric, соответствует одному из следующих объектов в кластере:
* Кластер
* Приложение
* Служба
* Секция
* Реплика 
* Контейнер

Полный список событий, предоставленных платформой, см. в статье [Операционный канал](service-fabric-diagnostics-event-generation-operational.md).

Ниже приведены несколько примеров сценариев, которые вы увидите в своем кластере. 
* События жизненного цикла узла. События будут отображаться по мере того, как узлы извлекаются, свертываются, развертываются, активируются, деактивируются или перезапускаются. В них представлена информация о процессе, которая и помогает определить причину проблемы: были ли это неполадки с компьютером, или же служба вызвала API для изменения состояния узла.
* Обновление кластера. По мере обновления вашего кластера (версия Service Fabric или изменение конфигурации) будет отображаться инициирование обновления, прокрутка каждого домена обновления и завершение (или откат). 
* Обновления приложений. Точно так же, как и в обновлениях кластера, будет отображаться полный набор событий по мере обновления. Эти события указывают, когда было запланировано обновление, текущее состояние обновления и общую последовательность событий. С помощью этих событий можно вернуться назад и просмотреть, какие обновления были успешно отменены, и был ли инициирован откат.
* Развертывание или удаление приложений и служб. Эти события создаются во время каждого создания или удаления приложения, службы и контейнера, и их можно использовать при свертывании или развертывании, увеличивая, например, количество реплик.
* Перемещение раздела (перенастройка). Каждый раз при перенастройке раздела состояния (изменение набора реплик) создается событие. Это событие содержит сведения о частоте изменения или отработки отказа набора реплик вашей группы. Кроме того, с его помощью можно отследить, какой узел запускал основную реплику в любой момент времени.
* События Chaos. При использовании службы [Chaos](service-fabric-controlled-chaos.md) Service Fabric эти события будут отображаться каждый раз, когда служба запускается или останавливается, или вводит ошибку в системе.
* События работоспособности. Service Fabric предоставляет события работоспособности каждый раз, когда создается отчет о работоспособности с сообщением об ошибке или с предупреждением, объект возвращается в нормальное состояние работоспособности или истекает срок действия отчета о работоспособности. Эти события можно использовать для отслеживания исторической статистики работоспособности объекта. 

## <a name="how-to-access-events"></a>Получение доступа к событиям

Существует несколько способов, с помощью которых можно получить доступ к событиям Service Fabric.
* События записываются через стандартные каналы, такие как ETW или журналы событий Windows, и могут быть представлены любым средством мониторинга, которое поддерживает такие данные, как журналы Azure Monitor. По умолчанию в кластерах, созданных на портале, включена диагностика, и агент диагностики Microsoft Azure отправляет события в хранилище таблиц Azure, но вам все равно нужно интегрировать его с ресурсом log Analytics. Узнайте больше о настройке [агента система диагностики Azure](service-fabric-diagnostics-event-aggregation-wad.md) для изменения конфигурации диагностики кластера, чтобы получить дополнительные журналы, счетчики производительности и [интеграцию Azure Monitor журналов](service-fabric-diagnostics-event-analysis-oms.md) .
* Через Rest API службы EventStore, которые позволяют запрашивать кластер напрямую, или через клиентскую библиотеку Service Fabric. Дополнительные сведения см. в статье [Query EventStore APIs for cluster events](service-fabric-diagnostics-eventstore-query.md) (Запрос к API EventStore для события кластера).

## <a name="next-steps"></a>Дальнейшие действия
* Дополнительные сведения о мониторинге кластера см. в статье [Мониторинг кластера и платформы](service-fabric-diagnostics-event-generation-infra.md).
* Дополнительные сведения о службе EventStore см. в [этой статье](service-fabric-diagnostics-eventstore.md).
