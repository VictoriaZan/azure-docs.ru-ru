---
title: Справочник по схеме Стилесобжект для динамических Azure Maps
description: Справочное руководство по динамической Azure Maps схеме и синтаксису Стилесобжект.
author: anastasia-ms
ms.author: v-stharr
ms.date: 11/20/2020
ms.topic: reference
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: f6bc4c62febf24dee790ac6136b1661426d4d619
ms.sourcegitcommit: c95e2d89a5a3cf5e2983ffcc206f056a7992df7d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/24/2020
ms.locfileid: "95536954"
---
# <a name="stylesobject-schema-reference-guide-for-dynamic-maps"></a>Справочник по схеме Стилесобжект для динамических карт

 `StylesObject`Представляет собой массив, представляющий стили наборов `StyleObject` состояний. Используйте [службу состояния компонента](/rest/api/maps/featurestate) Azure Maps Creator, чтобы применить стили оформления к функциям данных карт. После создания стилей оформления и связывания их с функциями карт можно использовать их для создания динамических карт. Дополнительные сведения о создании динамических карт см. в статье [Реализация динамического стиля для карт создателя](indoor-map-dynamic-styling.md).

## <a name="styleobject"></a>стилеобжект

`StyleObject`Является одним из следующих правил стиля:

 * [`BooleanTypeStyleRule`](#booleantypestylerule)
 * [`NumericTypeStyleRule`](#numerictypestylerule)
 * [`StringTypeStyleRule`](#stringtypestylerule)

В JSON ниже показан пример использования каждого из трех типов стилей.  `BooleanTypeStyleRule`Используется для определения динамического стиля для компонентов, `occupied` свойство которых имеет значение true и false.  `NumericTypeStyleRule`Используется для определения стиля для компонентов, `temperature` свойство которых попадает в определенный диапазон. Наконец, `StringTypeStyleRule` используется для сопоставления конкретных стилей с `meetingType` .



```json
 "styles": [
     {
        "keyname": "occupied",
        "type": "boolean",
        "rules": [
            {
                "true": "#FF0000",
                "false": "#00FF00"
            }
        ]
    },
    {
        "keyname": "temperature",
        "type": "number",
        "rules": [
             {
                "range": {
                "minimum": 50,
                "exclusiveMaximum": 70
                },
                "color": "#343deb"
            },
            {
                "range": {
                "maximum": 70,
                "exclusiveMinimum": 30
              },
              "color": "#eba834"
            }
        ]
    },
    {
      "keyname": "meetingType",
      "type": "string",
      "rules": [
        {
          "private": "#FF0000",
          "confidential": "#FF00AA",
          "allHands": "#00FF00",
          "brownBag": "#964B00"
        }
      ]
    }
]
```

## <a name="numerictypestylerule"></a>нумериктипестилеруле

 A `NumericTypeStyleRule` —  [`StyleObject`](#styleobject) и состоит из следующих свойств:

| Свойство. | Тип | Описание | Обязательно |
|-----------|----------|-------------|-------------|
| `keyName` | строка | Имя *состояния* или динамического свойства. `keyName`Должен быть уникальным внутри `StyleObject` массива.| Да |
| `type` | строка | Значение "numeric". | Да |
| `rules` | [`NumberRuleObject`](#numberruleobject)[]| Массив числовых диапазонов стилей со связанными цветами. Каждый диапазон определяет цвет, который будет использоваться, когда значение *состояния* соответствует диапазону.| Да |

### <a name="numberruleobject"></a>нумберрулеобжект

Объект `NumberRuleObject` состоит из [`RangeObject`](#rangeobject) и `color` Свойства. Если значение *состояния* попадает в диапазон, его цвет для отображения будет иметь цвет, заданный в `color` свойстве.

Если вы определили несколько перекрывающихся диапазонов, выбранный цвет будет соответствовать цвету, определенному в первом диапазоне.

В следующем примере JSON оба диапазона будут иметь значение true, если *состояние* находится в диапазоне от 50-60. Тем не менее цвет, который будет использоваться, заключается в том, что `#343deb` это первый диапазон в списке, который был удовлетворен.

```json

    {
        "rules":[
            {
                "range": {
                "minimum": 50,
                "exclusiveMaximum": 70
                },
                "color": "#343deb"
            },
            {
                "range": {
                "minimum": 50,
                "maximum": 60
                },
                "color": "#eba834"
            }
        ]
    }
]
```

| Свойство. | Тип | Описание | Обязательно |
|-----------|----------|-------------|-------------|
| `range` | [ранжеобжект](#rangeobject) | [Ранжеобжект](#rangeobject) определяет набор условий логического диапазона, который, если `true` , изменяет цвет отображения *состояния* на цвет, указанный в `color` свойстве. Если `range` параметр не указан, всегда будет использоваться цвет, определенный в `color` свойстве.   | Нет |
| `color` | строка | Цвет, используемый, когда значение состояния попадает в диапазон. `color`Свойство является строкой JSON в одном из следующих форматов: <ul><li> Шестнадцатеричные значения в стиле HTML </li><li> RGB ("#ff0", "#ffff00", "RGB (255, 255, 0)")</li><li> RGBA ("RGBA (255, 255, 0, 1)")</li><li> HSL ("HSL (100, 50%, 50%)")</li><li> ХСЛА ("хсла (100, 50%, 50%, 1)")</li><li> Предопределенные имена HTML-цветов, например желтые и синие.</li></ul> | Да |

### <a name="rangeobject"></a>ранжеобжект

`RangeObject`Определяет числовое значение диапазона [`NumberRuleObject`](#numberruleobject) . Чтобы значение *состояния* попадало в диапазон, все определенные условия должны содержать значение true.

| Свойство. | Тип | Описание | Обязательно |
|-----------|----------|-------------|-------------|
| `minimum` | double | Все цифры x, которые ≥ x `minimum` .| Нет |
| `maximum` | double | Все цифры x, x ≤ `maximum` . | Нет |
| `exclusiveMinimum` | double | Все цифры x, указанные x > `exclusiveMinimum` .| Нет |
| `exclusiveMaximum` | double | Все цифры x, указанные x < `exclusiveMaximum` .| Нет |

### <a name="example-of-numerictypestylerule"></a>Пример Нумериктипестилеруле

Следующий код JSON иллюстрирует `NumericTypeStyleRule` *состояние* с именем `temperature` . В этом примере элемент [`NumberRuleObject`](#numberruleobject) содержит два определенных диапазона температуры и связанные с ними стили цвета. Если диапазон температуры равен 50-69, то на дисплее должен использоваться цвет `#343deb` .  Если диапазон температуры равен 31-70, то на дисплее должен использоваться цвет `#eba834` .

```json
{
    "keyname": "temperature",
    "type": "number",
    "rules":[
        {
            "range": {
            "minimum": 50,
            "exclusiveMaximum": 70
            },
            "color": "#343deb"
        },
        {
            "range": {
            "maximum": 70,
            "exclusiveMinimum": 30
            },
            "color": "#eba834"
        }
    ]
}
```

## <a name="stringtypestylerule"></a>стрингтипестилеруле

A `StringTypeStyleRule` — [`StyleObject`](#styleobject) и состоит из следующих свойств:

| Свойство. | Тип | Описание | Обязательно |
|-----------|----------|-------------|-------------|
| `keyName` | строка |  Имя *состояния* или динамического свойства.  `keyName`Должен быть уникальным внутри `StyleObject` массива.| Да |
| `type` | строка |Значение "String". | Да |
| `rules` | [`StringRuleObject`](#stringruleobject)[]| Массив из N числовых значений *состояния* .| Да |

### <a name="stringruleobject"></a>стрингрулеобжект

`StringRuleObject`Содержит до N количество значений состояния, которые являются возможными строковыми значениями свойства функции. Если значение свойства функции не совпадает ни с одним из определенных значений состояния, эта функция не будет иметь динамического стиля. Если заданы дублирующиеся значения состояния, первый из них имеет приоритет.

В соответствии со строковым значением учитывается регистр.

| Свойство. | Тип | Описание | Обязательно |
|-----------|----------|-------------|-------------|
| `stateValue1` | строка | Цвет, в котором строка значения — stateValue1. | Нет |
| `stateValue2` | строка | Цвет, в котором строка значения — Статевалуе. | Нет |
| `stateValueN` | строка | Цвет, в котором строка значения — Статевалуен. | Нет |

### <a name="example-of-stringtypestylerule"></a>Пример Стрингтипестилеруле

Следующий код JSON иллюстрирует `StringTypeStyleRule` , что определяет стили, связанные с конкретными типами собраний.

```json
    {
      "keyname": "meetingType",
      "type": "string",
      "rules": [
        {
          "private": "#FF0000",
          "confidential": "#FF00AA",
          "allHands": "#00FF00",
          "brownBag": "#964B00"
        }
      ]
    }

```

## <a name="booleantypestylerule"></a>булеантипестилеруле

A `BooleanTypeStyleRule` — [`StyleObject`](#styleobject) и состоит из следующих свойств:

| Свойство. | Тип | Описание | Обязательно |
|-----------|----------|-------------|-------------|
| `keyName` | строка |  Имя *состояния* или динамического свойства.  `keyName`Должен быть уникальным внутри `StyleObject` массива.| Да |
| `type` | строка |Значение "Boolean". | Да |
| `rules` | [`BooleanRuleObject`](#booleanruleobject)одного| Логическая пара с цветами для `true` `false` значений *состояния* и.| Да |

### <a name="booleanruleobject"></a>булеанрулеобжект

`BooleanRuleObject`Определяет цвета для `true` значений и `false` .

| Свойство. | Тип | Описание | Обязательно |
|-----------|----------|-------------|-------------|
| `true` | строка | Цвет, используемый, если значение *состояния* равно `true` . `color`Свойство является строкой JSON в одном из следующих форматов: <ul><li> Шестнадцатеричные значения в стиле HTML </li><li> RGB ("#ff0", "#ffff00", "RGB (255, 255, 0)")</li><li> RGBA ("RGBA (255, 255, 0, 1)")</li><li> HSL ("HSL (100, 50%, 50%)")</li><li> ХСЛА ("хсла (100, 50%, 50%, 1)")</li><li> Предопределенные имена HTML-цветов, например желтые и синие.</li></ul>| Да |
| `false` | строка | Цвет, используемый, если значение *состояния* равно `false` . | Да |

### <a name="example-of-booleantypestylerule"></a>Пример Булеантипестилеруле

Следующий код JSON иллюстрирует `BooleanTypeStyleRule` *состояние* с именем `occupied` . [`BooleanRuleObject`](#booleanruleobject)Определяет цвета для `true` значений и `false` .

```json
{
    "keyname": "occupied",
    "type": "boolean",
    "rules": [
    {
        "true": "#FF0000",
        "false": "#00FF00"
    }
    ]
}
```