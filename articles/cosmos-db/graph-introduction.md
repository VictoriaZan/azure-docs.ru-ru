---
title: 'Общие сведения об Azure Cosmos DB: API Gremlin'
description: Узнайте, как можно использовать базу данных Azure Cosmos DB, чтобы хранить, запрашивать и передавать массивные графы с минимальной задержкой с помощью языка запросов графа Gremlin в Apache TinkerPop.
author: christopheranderson
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: overview
ms.date: 07/10/2020
ms.author: chrande
ms.openlocfilehash: d0bd94037a75db8d69cfd44820a80ae8b403c9ea
ms.sourcegitcommit: 6a902230296a78da21fbc68c365698709c579093
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93357085"
---
# <a name="introduction-to-gremlin-api-in-azure-cosmos-db"></a>Общие сведения об API Gremlin в Azure Cosmos DB
[!INCLUDE[appliesto-gremlin-api](includes/appliesto-gremlin-api.md)]

[Azure Cosmos DB](introduction.md)  — это глобально распределенная, многомодельная служба базы данных Майкрософт, необходимая для работы с критически важными приложениями. Это многомодельная база данных, которая поддерживает модели документов, пар "ключ-значение", графов и "столбцов-семейств". Azure Cosmos DB предоставляет службу графовой базы данных с использованием API Gremlin в полностью управляемой службе базы данных для любого масштаба.  

:::image type="content" source="./media/graph-introduction/cosmosdb-graph-architecture.png" alt-text="Архитектура графа базы данных Azure Cosmos DB" border="false":::

В этой статье содержатся общие сведения об интерфейсе API Gremlin для Azure Cosmos DB и объясняется, как его использовать для хранения больших графов с миллиардами вершин и ребер. Вы можете запрашивать графы с задержкой в миллисекунды и легко развивать структуру графов. API Gremlin в Azure Cosmos DB создан на основе [Apache TinkerPop](https://tinkerpop.apache.org), платформы графических вычислений. API Gremlin в Azure Cosmos DB использует язык запросов Gremlin.

Gremlin API Azure Cosmos DB объединяет мощь алгоритмов базы данных графов с хорошо масштабируемой управляемой инфраструктурой, обеспечивая уникальное гибкое решение для наиболее распространенных проблем с данными, связанных с отсутствием гибкости и реляционных подходов.

## <a name="features-of-azure-cosmos-dbs-gremlin-api"></a>Функции API Gremlin в Azure Cosmos DB
 
База данных Azure Cosmos DB является полностью управляемой базой данных графа, которая обеспечивает глобальное распределение, гибкое масштабирование хранилища и пропускной способности, автоматическое индексирование и возможность выполнять запросы, настраиваемые уровни согласованности, а также поддержку стандартной платформы вычисления графа TinkerPop.

Ниже перечислены отличительные особенности, которые предлагает Azure Cosmos DB Gremlin API.

* **Гибко масштабируемые пропускная способность и хранилище**

  На практике графы должны масштабироваться за пределы емкости одного сервера. Azure Cosmos DB поддерживает горизонтально масштабируемые базы данных графа, которые могут иметь практически неограниченный размер с точки зрения хранилища и предоставляемой пропускной способности. По мере увеличения масштаба базы данных графа, данные будут автоматически распределяться с использованием [секционирования данных графа](./graph-partitioning.md).

* **Репликация между несколькими регионами**

  Azure Cosmos DB может автоматически реплицировать данные графа в любой регион Azure по всему миру. Глобальная репликация упрощает разработку приложений, которым требуется глобальный доступ к данным. Помимо минимизации задержки чтения и записи в любой точке мира, Azure Cosmos DB предоставляет механизм автоматической отработки отказа в любом регионе, который может обеспечить непрерывность работы вашего приложения в редких случаях прерывания обслуживания в регионе.

* **Быстрые запросы и обходы с использованием наиболее распространенного стандарта запросов графа**

  Храните разные типы вершин и ребер, а также выполняйте их запросы, используя знакомый синтаксис Gremlin. Gremlin — это императивный, функциональный язык запросов, который предоставляет комфортный интерфейс для реализации общих алгоритмов графов.
  
  Azure Cosmos DB позволяет формировать многофункциональные запросы и обходы в режиме реального времени без необходимости указывать подсказки для схемы, вторичные индексы или представления. Дополнительные сведения см. в статье [Поддержка графа Gremlin в базе данных Azure Cosmos DB](gremlin-support.md).

* **Полностью управляемая база данных графа**

  Azure Cosmos DB устраняет необходимость управления базой данных и вычислительными ресурсами. Большинство существующих платформ баз данных графов связаны с ограничениями их инфраструктуры и часто требуют высокой степени обслуживания для обеспечения их работы. 
  
  Полностью управляемая служба Cosmos DB избавляет от необходимости управлять виртуальными машинами, обновлять программное обеспечение среды выполнения, управлять сегментированием или репликацией или заниматься сложными обновлениями на уровне данных. Каждый граф автоматически сохраняется и защищается от региональных сбоев. Это позволяет разработчикам сосредоточиться на обеспечении ценности приложения вместо обслуживания баз данных графов и управления ими. 

* **Автоматическая индексация**

  По умолчанию Azure Cosmos DB автоматически индексирует все свойства в узлах (также называемых вершинами) и ребрах в графе, не ожидая и не требуя наличия какой-либо схемы или создания дополнительных индексов. Дополнительные сведения об [индексировании в Azure Cosmos DB](/azure/cosmos-db/index-overview).

* **Совместимость с Apache TinkerPop**

  Azure Cosmos DB поддерживает [стандарт Apache TinkerPop с открытым исходным кодом](https://tinkerpop.apache.org/). Стандарт Tinkerpop имеет обширную экосистему приложений и библиотек, которые легко интегрируются с Gremlin API Azure Cosmos DB.

* **Настраиваемые уровни согласованности**

  Azure Cosmos DB предоставляет пять четко определенных уровней согласованности для достижения правильного компромисса между согласованностью и производительностью приложения. Для запросов и операций чтения служба Azure Cosmos DB предлагает пять отдельных уровней согласованности — сильный, с ограниченным устареванием, сеансовый, с согласованностью префиксов и согласованный в конечном счете. Эти разделенные, четко определенные уровни согласованности позволяют принимать обоснованные компромиссы между показателями согласованности, доступности и задержки. Дополнительные сведения см. в статье [Настраиваемые уровни согласованности данных в Azure Cosmos DB](consistency-levels.md).

## <a name="scenarios-that-use-gremlin-api"></a>Сценарии, в которых используется API Gremlin

Ниже приведены некоторые полезные примеры использования поддержки графа базы данных Azure Cosmos DB.

* **Социальные сети/Клиент 365**

  Объединяя данные о клиентах и их взаимодействиях с другими людьми, вы можете разработать персонализированные средства, предугадать поведение клиента или объединить людей на основе их интересов. Базу данных Azure Cosmos DB можно использовать для управления социальными сетями и отслеживания данных о клиенте и его предпочтениях.

* **Системы рекомендаций**

  Широко используется в сфере розничной торговли. Объединяя сведения о продукции, пользователях и операциях пользователей, например приобретении, поиске и оценке товара, вы можете создать пользовательские рекомендации. База данных Azure Cosmos DB с минимальной задержкой, гибким масштабированием и встроенной поддержкой графа представляет собой идеальный вариант моделирования этих взаимосвязей.

* **Геопространственные функции**

  Многим приложениям в сферах телекоммуникаций, логистики и туристического планирования требуется поиск интересующего расположения в регионе или же обнаружение самого короткого (оптимального) пути между двумя расположениями. База данных Azure Cosmos DB поможет легко справиться с этими задачами.

* **Интернет вещей**

  Исследуя сеть и подключения между устройствами Интернета вещей, смоделированными как граф, вы сможете разобраться с состоянием своих устройств и ресурсов. Кроме того, вы сможете узнать, как изменения одного компонента сети могут повлиять на другой.

## <a name="introduction-to-graph-databases"></a>Основные сведения о базах данных графов

В реальном мире все данные связаны естественным образом. Традиционное моделирование данных фокусируется на отдельном определении объектов и вычислении их отношений во время выполнения. Несмотря на то, что у этой модели есть свои преимущества, управление данными с высокой степенью связности может оказаться сложной задачей в условиях ограничений.  

Подход базы данных графа основан на сохранении связей на уровне хранилища, что приводит к высокоэффективным операциям извлечения графов. Azure Cosmos DB Gremlin API поддерживает [модель графа свойства](https://tinkerpop.apache.org/docs/current/reference/#intro).

### <a name="property-graph-objects"></a>Объекты графа свойства

[Граф](http://mathworld.wolfram.com/Graph.html) — это структура, состоящая из [вершин](http://mathworld.wolfram.com/GraphVertex.html) и [ребер](http://mathworld.wolfram.com/GraphEdge.html). Оба объекта могут иметь произвольное число пар ключ-значение в качестве свойств. 

* **Вершинами или узлами** обозначаются дискретные объекты, например пользователь, место или событие.

* **Ребра или связи** обозначают взаимосвязи между вершинами. Например, пользователь может знать другого пользователя, участвовать в событии или посетить мероприятие.

* **Свойства** выражают сведения о вершинах и ребрах. В вершинах или ребрах может быть любое количество свойств, и их можно использовать для описания и фильтрации объектов в запросе. Свойства примера включают в себя вершину, у которой есть имя и возраст, или ребро, которое может иметь отметку времени и (или) вес.

* **Метка**  — это имя или идентификатор вершины либо ребра. Можно группировать несколько вершин или ребер таким образом, чтобы все вершины или ребра в группе были снабжены определенной меткой. Например, на графе может быть несколько вершин с меткой типа person (человек).

Базы данных графов часто относят к категории баз данных NoSQL, или нереляционных баз данных, поскольку в них нет зависимости от схемы или ограничений модели данных. Это отсутствие схемы позволяет естественно и эффективно моделировать и хранить связанные структуры.

### <a name="graph-database-by-example"></a>Пример графовой базы данных

Давайте рассмотрим пример графа, чтобы лучше понять, как в Gremlin могут выражаться запросы. На рисунке ниже в форме графа показано бизнес-приложение, управляющее данными о пользователях, их интересах и устройствах.  

:::image type="content" source="./media/gremlin-support/sample-graph.png" alt-text="Пример базы данных, в котором показаны люди, их интересы и устройства" border="false"::: 

Этот граф содержит следующие типы *вершин* (в Gremlin также называются метками):

* **Люди**. На графе представлено три человека: Робин (Robin), Томас (Thomas) и Бен (Ben).
* **Интересы**. Их интересы. В этом случае — футбол.
* **Устройства**. Устройства, которые эти люди используют.
* **Операционные системы**. Операционные системы, под управлением которых работают устройства.
* **Место.** Места, из которых осуществляется доступ к устройствам.

Мы представим взаимосвязи между этими сущностями, используя следующие типы *ребер* :

* **Знакомства**. Например, "Томас знает Робин".
* **Интересы**. Используется, чтобы представить интересы людей на графе. Например, "Бен интересуется футболом".
* **Использующаяся операционная система**. Ноутбук работает под управлением Windows.
* **Используемые устройства**. Применяется, чтобы представить используемое устройство. Например, Робин использует телефон Motorola с серийным номером 77.
* **Расположение.** Представляет расположение, из которого осуществляется доступ к устройствам.

Консоль Gremlin — это интерактивный терминал от Apache TinkerPop, используемый для взаимодействия с данными графа. Дополнительные сведения см. в кратком руководстве по [использованию консоли Gremlin](create-graph-gremlin-console.md). Вы также можете выполнить эти операции с помощью драйверов Gremlin на платформе по вашему усмотрению — Java, Node.js, Python или .NET. В следующих примерах показано, как обращаться к данным графа с помощью консоли Gremlin.

Сначала рассмотрим операции CRUD (создание, чтение, обновление и удаление). Следующая инструкция Gremlin вставляет вершину "Thomas" в граф:

```java
:> g.addV('person').property('id', 'thomas.1').property('firstName', 'Thomas').property('lastName', 'Andersen').property('age', 44)
```

Затем инструкция Gremlin вставляет ребро "знакомства" между вершинами Thomas и Robin.

```java
:> g.V('thomas.1').addE('knows').to(g.V('robin.1'))
```

Следующий запрос возвращает вершины "людей" в порядке по убыванию их имен:

```java
:> g.V().hasLabel('person').order().by('firstName', decr)
```

Если часть графа подсвечивается, необходимо ответить на вопросы типа: "Какие операционные системы используют друзья Томаса?" Вы можете выполнить эту операцию обхода Gremlin, чтобы получить сведения из графа:

```java
:> g.V('thomas.1').out('knows').out('uses').out('runsos').group().by('name').by(count())
```

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о поддержке графа в базе данных Azure Cosmos DB см. в следующих ресурсах:

* [Azure Cosmos DB. Создание приложения .NET с помощью API Graph](create-graph-dotnet.md)
* Дополнительные сведения см. в статье [Поддержка графа Gremlin в базе данных Azure Cosmos DB](gremlin-support.md).