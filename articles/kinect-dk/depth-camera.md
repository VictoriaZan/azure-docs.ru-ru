---
title: Камера Azure Kinect DK Depth
description: Изучите рабочие принципы и основные возможности камеры с глубиной в Azure Kinect DK.
author: tesych
ms.author: tesych
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: conceptual
keywords: Kinect, Azure, датчик, пакет SDK, Камера глубины, ТОФ, принципы, производительность, недействительность
ms.openlocfilehash: 22f04b983ed7c6a2ab19a5c1c709621655ee31c0
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "85276956"
---
# <a name="azure-kinect-dk-depth-camera"></a>Камера Azure Kinect DK Depth

На этой странице описывается, как использовать камеру глубины в Azure Kinect DK. Камера глубины — это вторая из двух камер. Как обсуждалось в предыдущих разделах, другая камера является камерой RGB.  

## <a name="operating-principles"></a>Рабочие принципы

Камера глубины Azure Kinect DK реализует принцип "непрерывной" волновой волны (АМКВ). Камера выполняет модуляцию освещения в спектре практически-IR (NIR) на сцене. Затем он записывает косвенное измерение времени, которое уходит от камеры к сцене и обратно.

Эти измерения обрабатываются для создания карт глубины. Схема глубины — это набор значений координат Z для каждого пикселя изображения, измеряемый в единицах миллиметра.

Наряду с картой глубины мы также получаем так называемый чистый IR Read. Значение пикселей в чтении чистого IR пропорционально количеству освещения, возвращенному из сцены. Изображение выглядит так же, как и обычное изображение в формате IR. На рисунке ниже показан пример схемы глубины (слева) и соответствующее изображение очистки IR (справа).

![Параллельное и IR](./media/concepts/depth-camera-depth-ir.png)

## <a name="key-features"></a>Основные возможности

Ниже приведены технические характеристики камеры с глубиной глубины.

- 1-мегапикселная микросхема с образами ТОФ с расширенными пиксельными технологиями, обеспечивающая более высокую частоту модуляции и точность глубины.
- Два NIR лазерных диода позволяют использовать почти и широкие режимы глубины представления (фов).
- Самый маленький Тофный пиксель в мире 3,5 μм 3,5 μм.
- Автоматическая выборка с увеличением на пиксель, что позволяет использовать большой динамический диапазон, позволяющий аккуратно собирать почти и несколько объектов.
- Глобальная выдержка, позволяющая повысить производительность солнечного света.
- Многофазный метод вычисления глубины, обеспечивающий устойчивую точность даже при наличии микросхемы, лазера и источника питания.
- Низкая систематическая и случайная ошибки.

![Модуль глубины](./media/concepts/depth-camera-depth-module.jpg)

Камера с глубиной передает необработанные ИК-изображения с демодуляцией на главный компьютер. На компьютере программное обеспечение ядра глубины GPU преобразует необработанный сигнал в карты глубины.Камера глубины поддерживает несколько режимов. **Узкие поля режимов представления (фов)** идеально подходят для сцен с меньшими экстентами в измерениях X и Y, но большими экстентами в Z-измерении. Если сцена имеет большие экстенты X и Y, но меньше Z-диапазонов, **широкие режимы фов** лучше подходят.

Камера глубины поддерживает **режимы 2x2 группирования** для расширения Z-диапазона в сравнении с соответствующими **режимами унбиннед**. Группирования выполняется за счет снижения разрешения изображения. Все режимы могут выполняться с частотой до 30 кадров (в секунду) с исключением режима 1 мегапиксел (MP), который работает с максимальной частотой кадров, равной 15 кадрам/с. Камера глубины также обеспечивает **пассивный ИК-режим**. В этом режиме индикаторы на камере не активны и наблюдаются только окружающие освещения.

## <a name="camera-performance"></a>Производительность камеры

Производительность камеры измеряется как систематические и случайные ошибки.

### <a name="systematic-error"></a>Систематическая ошибка

Систематическая ошибка определяется как разница между измеряемой глубиной после удаления шума и соответствующей глубиной (земля). Мы вычислим временное среднее по многим кадрам статической сцены, чтобы свести к вам максимально возможное количество помех. Точнее, систематическая ошибка определяется следующим образом:

![Ошибка систематической глубины](./media/concepts/depth-camera-systematic-error.png)

Где *d<sub>t</sub> * обозначает глубину меры в момент *t*, *N* — число кадров, используемых в процедуре усреднения, а *d<sub>gt</sub> * — глубину самого себя.

Спецификация систематической ошибки камеры с глубиной не включает помехи с несколькими путями (MPI). MPI происходит, когда один пиксель датчика интегрирует освещение, отраженное более чем одним объектом. MPI частично снижена в нашей камере с более высокой частотой модуляции, а также с недействительностью глубины, которую мы будем использовать позже.

### <a name="random-error"></a>Случайная ошибка

Предположим, что мы принимаем 100 изображений одного объекта без перемещения камеры. В каждом из 100 изображений глубина объекта будет немного отличаться. Это различие вызвано шумом на снимке. Шум при снимке — это число фотоны, на которое датчик имеет влияние случайного коэффициента в зависимости от времени. Мы определим эту случайную ошибку на статической сцене как стандартное отклонение глубины по времени, вычисленное как:

![Случайная ошибка глубины](./media/concepts/depth-camera-random-error.png)

Где *N* обозначает число измерений глубины, *d<sub>t</sub> * представляет измерение глубины в момент *t* , а *d* — среднее значение, вычисленное по всем измерениям глубины *d<sub>t</sub>*.

## <a name="invalidation"></a>Недействительность

В некоторых случаях камера глубины может не предоставлять правильные значения для некоторых пикселей. В таких случаях Пиксели глубины становятся недействительными. Недопустимые Пиксели обозначаются значением глубины, равным 0. Ниже перечислены причины, по которым механизму глубины не удается получить правильные значения.

- За пределами активной освещения маски IR
- Перегруженный ИК-сигнал
- Низкий ИК-сигнал
- Фильтрация выбросов
- Помехи с несколькими путями

### <a name="illumination-mask"></a>Маска освещения

Пиксели становятся недействительными, если они находятся за пределами активной освещения маски IR. Не рекомендуется использовать в качестве глубины вычислений сигнал таких пикселей. На рисунке ниже показан пример недействительности освещения Mask. Недействительные Пиксели — это черные пиксели за пределами окружности в широких режимах фов (слева), а шестиугольник в узких режимах фов (справа).

![Недействительность за пределами освещения Mask](./media/concepts/depth-camera-invalidation-illumination-mask.png)

### <a name="signal-strength"></a>Сила сигнала

Пиксели становятся недействительными, если они содержат насыщенный IR сигнал. Когда Пиксели насыщены, сведения о фазе теряются. На рисунке ниже показан пример недействительности по перегруженному ИК-сигналу. См. стрелки, указывающие на примеры пикселей в изображениях глубины и IR.

![Насыщенность недействительности](./media/concepts/depth-camera-invalidation-saturation.png)

Недействительность может также возникать, когда IR-сигнал достаточно надежен для формирования глубины. На приведенном ниже рисунке показан пример недействительности с низким ИК-сигналом. См. стрелки, указывающие на пример пикселей в изображениях глубины и IR.

![Низкий сигнал недействительности](./media/concepts/depth-camera-invalidation-low-signal.png)

### <a name="ambiguous-depth"></a>Неоднозначная глубина

Пиксели также могут быть недействительными, если они получили сигналы от нескольких объектов в сцене. Типичный случай, когда можно видеть такое недействительность, находится в углах.  Из-за геометрии сцены ИК-сигнал от камеры отражается на одной стене и на другой. Это отраженное освещение приводит к неоднозначности измеряемой глубины пикселя. Фильтры в алгоритме глубины обнаруживают эти неоднозначные сигналы и делают недействительными Пиксели.

На рисунках ниже показаны примеры недействительности при обнаружении нескольких путей. Вы также можете увидеть, как та же контактная зона, которая стала недействительной из одного представления камеры (верхняя строка), может снова появиться в другом представлении камеры (Нижняя строка). На этом рисунке показано, что поверхности, недействительные с одной перспективы, могут быть видимы из другой.

![Недействительность в многопутевом углу](./media/concepts/depth-camera-invalidation-multipath.png)

Другим распространенным случаем использования Multipath являются пиксели, которые содержат смешанный сигнал от переднего плана и фона (например, вокруг краев объектов). Во время быстрого перемещения вы можете видеть больше недопустимых пикселей вокруг краев. Дополнительные недействительные Пиксели из-за интервала выдержки необработанной записи глубины

![Недействительность многопутевых контуров](./media/concepts/depth-camera-invalidation-edge.png)

## <a name="next-steps"></a>Дальнейшие действия

[Системы координат](coordinate-systems.md)
