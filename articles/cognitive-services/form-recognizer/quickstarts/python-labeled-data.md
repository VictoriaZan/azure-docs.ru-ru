---
title: Краткое руководство. Распознаватель документов. Обучение с использованием меток, REST API и Python
titleSuffix: Azure Cognitive Services
description: Узнайте о том, как обучить настраиваемую модель с помощью маркированных данных в Распознавателе документов с использованием REST API и Python.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: quickstart
ms.date: 10/05/2020
ms.author: pafarley
ms.custom: devx-track-python
ms.openlocfilehash: f944a793d721e93d818723eae25a9ce80d9c15bc
ms.sourcegitcommit: b8eba4e733ace4eb6d33cc2c59456f550218b234
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/23/2020
ms.locfileid: "96005097"
---
# <a name="train-a-form-recognizer-model-with-labels-using-rest-api-and-python"></a>Обучение модели Распознавателя документов по примерам с метками с помощью REST API и Python

В этом кратком руководстве вы будете использовать REST API Распознавателя документов и Python для обучения модели с использованием данных с метками, присвоенными вручную. Дополнительные сведения об этой функции см. в разделе об [обучении с использованием меток](../overview.md#train-with-labels).

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись](https://azure.microsoft.com/free/cognitive-services/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Для работы с этим кратким руководством требуется следующее:
- Среда [Python](https://www.python.org/downloads/), если вы хотите выполнить этот пример кода локально.
- Минимум шесть документов одного типа. Вы будете использовать эти данные для обучения модели и тестирования формы. Для работы с этим кратким руководством вы можете использовать [пример набора данных](https://go.microsoft.com/fwlink/?linkid=2090451). Скачайте и разархивируйте файл *sample_data.zip*. Передайте файлы для обучения в корневой каталог контейнера Хранилища BLOB-объектов в учетной записи хранения Azure со стандартным уровнем производительности.

> [!NOTE]
> В этом кратком руководстве используются удаленные документы, доступ к которым осуществляется по URL-адресу. Чтобы вместо них использовать локальные файлы, см. справочную документацию для [версии 2.0](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/TrainCustomModelAsync) или [версии 2.1](https://westcentralus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1-preview-2/operations/TrainCustomModelAsync).

## <a name="create-a-form-recognizer-resource"></a>Создание ресурса Распознавателя документов

[!INCLUDE [create resource](../includes/create-resource.md)]

## <a name="set-up-training-data"></a>Подготовка данных для обучения

Затем вам нужно настроить входные данные. Функция маркированных данных предъявляет особые требования к входным данным наряду с теми, которые применяются при обучении настраиваемой модели без меток.

Убедитесь, что все обучающие документы имеют одинаковый формат. Если у вас есть формы в разных форматах, рассортируйте их по вложенным папкам соответствующим образом. При обучении вам нужно будет обращаться к вложенным папкам с помощью API.

Чтобы обучить модель с использованием маркированных данных, во вложенной папке должны размещаться следующие входные файлы. Ниже описано, как создать эти файлы.

* **Исходные формы**, из которых извлекаются данные. Поддерживаются типы JPEG, PNG, PDF и TIFF.
* **Файлы макетов OCR** — это файлы в формате JSON, которые описывают размеры и положения всех доступных для чтения фрагментов текста в каждой исходной форме. Для создания этих данных вы будете использовать API макета Распознавателя документов. 
* **Файлы меток** — это файлы в формате JSON, которые описывают метки данных, заданные пользователем вручную.

Все эти файлы должны размещаться в одной вложенной папке с именами в следующем формате:

* input_file1.pdf 
* input_file1.pdf.ocr.json
* input_file1.pdf.labels.json 
* input_file2.pdf 
* input_file2.pdf.ocr.json
* input_file2.pdf.labels.json
* ...

> [!TIP]
> Если метки присваиваются формам с помощью [средства маркировки данных](./label-tool.md) Распознавателя документов, оно автоматически создает такие файлы для меток и макета OCR.

### <a name="create-the-ocr-output-files"></a>Создание выходных файлов OCR

Файлы результатов OCR нужны для того, чтобы служба распознала соответствующие входные файлы для обучения с использованием меток. Чтобы получить результаты OCR для определенной исходной формы, выполните следующие действия.

1. Вызовите API **[анализа макета](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/AnalyzeLayoutAsync)** для входного контейнера макета, указав входной файл в тексте запроса. Сохраните обнаруженный идентификатор в заголовке ответа **Operation-Location**.
1. Вызовите API **[получения результатов анализа макета](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/GetAnalyzeLayoutResult)** , используя идентификатор операции из предыдущего шага.
1. Получите ответ и запишите его содержимое в файл. Для каждой исходной формы файл распознавания текста должен иметь то же имя, что и оригинал, с добавлением суффикса `.ocr.json`. Выходные данные OCR в формате JSON должны иметь следующий формат. См. полный пример [примера файла OCR](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/curl/form-recognizer/Invoice_1.pdf.ocr.json). 

    # <a name="v20"></a>[Версия 2.0](#tab/v2-0)
    ```json
    {
    "status": "succeeded",
    "createdDateTime": "2019-11-12T21:18:12Z",
    "lastUpdatedDateTime": "2019-11-12T21:18:17Z",
    "analyzeResult": {
        "version": "2.0.0",
        "readResults": [
            {
                "page": 1,
                "language": "en",
                "angle": 0,
                "width": 8.5,
                "height": 11,
                "unit": "inch",
                "lines": [
                    {
                        "language": "en",
                        "boundingBox": [
                            0.5384,
                            1.1583,
                            1.4466,
                            1.1583,
                            1.4466,
                            1.3534,
                            0.5384,
                            1.3534
                        ],
                        "text": "Contoso",
                        "words": [
                            {
                                "boundingBox": [
                                    0.5384,
                                    1.1583,
                                    1.4466,
                                    1.1583,
                                    1.4466,
                                    1.3534,
                                    0.5384,
                                    1.3534
                                ],
                                "text": "Contoso",
                                "confidence": 1
                            }
                        ]
                    },
                    ...
    ```    
    # <a name="v21-preview"></a>[Предварительная версия 2.1](#tab/v2-1)
    ```json
    {
    "status": "succeeded",
    "createdDateTime": "2019-11-12T21:18:12Z",
    "lastUpdatedDateTime": "2019-11-12T21:18:17Z",
    "analyzeResult": {
        "version": "2.1.0",
        "readResults": [
            {
                "page": 1,
                "language": "en",
                "angle": 0,
                "width": 8.5,
                "height": 11,
                "unit": "inch",
                "lines": [
                    {
                        "language": "en",
                        "boundingBox": [
                            0.5384,
                            1.1583,
                            1.4466,
                            1.1583,
                            1.4466,
                            1.3534,
                            0.5384,
                            1.3534
                        ],
                        "text": "Contoso",
                        "words": [
                            {
                                "boundingBox": [
                                    0.5384,
                                    1.1583,
                                    1.4466,
                                    1.1583,
                                    1.4466,
                                    1.3534,
                                    0.5384,
                                    1.3534
                                ],
                                "text": "Contoso",
                                "confidence": 1
                            }
                        ]
                    },
                    ...
    ```    


    ---



### <a name="create-the-label-files"></a>Создание файлов меток

Файлы меток содержат связи "ключ — значение", которые пользователь вводит вручную. Они необходимы для обучения с использованием маркированных данных, при этом не каждый исходный файл должен иметь соответствующий файл меток. Исходные файлы без меток будут рассматриваться как обычные обучающие документы. Для надежного обучения мы рекомендуем предоставить пять или более файлов с метками. Для создания этих файлов можно использовать инструмент пользовательского интерфейса, например [средство маркировки данных выборки](./label-tool.md).

При создании файла меток вы можете дополнительно указать регионы, то есть точные позиции этих значений в документе. Это дополнительно повысит точность обучения. Регионы предоставляются как наборы из восьми значений, соответствующих четырем координатам X и Y: верхний левый, верхний правый, нижний правый и нижний левый. Координаты могут иметь значения от нуля до единицы с возможностью масштабирования до размеров страницы.

Для каждой исходной формы файл меток должен иметь то же имя, что и оригинал, с добавлением суффикса `.labels.json`. Файл меток должен иметь следующий формат. См. полный пример [примера файла меток](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/curl/form-recognizer/Invoice_1.pdf.labels.json).

```json
{
    "document": "Invoice_1.pdf",
    "labels": [
        {
            "label": "Provider",
            "key": null,
            "value": [
                {
                    "page": 1,
                    "text": "Contoso",
                    "boundingBoxes": [
                        [
                            0.06334117647058823,
                            0.1053,
                            0.17018823529411767,
                            0.1053,
                            0.17018823529411767,
                            0.12303636363636362,
                            0.06334117647058823,
                            0.12303636363636362
                        ]
                    ]
                }
            ]
        },
        {
            "label": "For",
            "key": null,
            "value": [
                {
                    "page": 1,
                    "text": "Microsoft",
                    "boundingBoxes": [
                        [
                            0.6122941176470589,
                            0.1374,
                            0.6841764705882353,
                            0.1374,
                            0.6841764705882353,
                            0.14682727272727272,
                            0.6122941176470589,
                            0.14682727272727272
                        ]
                    ]
                },
                {
                    "page": 1,
                    "text": "1020",
                    "boundingBoxes": [
                        [
                            0.6121882352941176,
                            0.156,
                            0.6462941176470588,
                            0.156,
                            0.6462941176470588,
                            0.1653181818181818,
                            0.6121882352941176,
                            0.1653181818181818
                        ]
                    ]
                },
                ...
```

> [!IMPORTANT]
> Каждому текстовому элементу можно присвоить только одну метку, и каждая метка может применяться только один раз на каждой странице. Метку нельзя применить к нескольким страницам.


## <a name="train-a-model-using-labeled-data"></a>Обучение модели с использованием маркированных данных

Чтобы обучить модель с использованием маркированных данных, вызовите API **[обучения настраиваемой модели](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/TrainCustomModelAsync)** , выполнив следующий код Python. Перед выполнением команды внесите следующие изменения:

1. Замените `<Endpoint>` на URL-адрес конечной точки для ресурса Распознавателя документов.
1. Замените `<SAS URL>` подписанным URL-адресом контейнера хранилища BLOB-объектов Azure. Чтобы получить подписанный URL-адрес, откройте Обозреватель службы хранилища Microsoft Azure, щелкните контейнер правой кнопкой мыши и выберите **Get shared access signature** (Получить подписанный URL-адрес). Убедитесь, что разрешение на **чтение** и разрешение **списка** установлены и нажмите кнопку **Создать**. Затем скопируйте значение в разделе **URL-адрес**. Оно должно быть в таком формате: `https://<storage account>.blob.core.windows.net/<container name>?<SAS value>`.
1. Замените `<Blob folder name>` именем папки со входными данными, которая размещается в контейнере больших двоичных объектов. Если данные находятся в корневой папке, оставьте это поле пустым и удалите поле `"prefix"` из тела HTTP-запроса.

# <a name="v20"></a>[Версия 2.0](#tab/v2-0)
```python
########### Python Form Recognizer Labeled Async Train #############
import json
import time
from requests import get, post

# Endpoint URL
endpoint = r"<Endpoint>"
post_url = endpoint + r"/formrecognizer/v2.0/custom/models"
source = r"<SAS URL>"
prefix = "<Blob folder name>"
includeSubFolders = False
useLabelFile = True

headers = {
    # Request headers
    'Content-Type': 'application/json',
    'Ocp-Apim-Subscription-Key': '<subsription key>',
}

body =     {
    "source": source,
    "sourceFilter": {
        "prefix": prefix,
        "includeSubFolders": includeSubFolders
    },
    "useLabelFile": useLabelFile
}

try:
    resp = post(url = post_url, json = body, headers = headers)
    if resp.status_code != 201:
        print("POST model failed (%s):\n%s" % (resp.status_code, json.dumps(resp.json())))
        quit()
    print("POST model succeeded:\n%s" % resp.headers)
    get_url = resp.headers["location"]
except Exception as e:
    print("POST model failed:\n%s" % str(e))
    quit() 
```    
# <a name="v21-preview"></a>[Предварительная версия 2.1](#tab/v2-1)    
```python
########### Python Form Recognizer Labeled Async Train #############
import json
import time
from requests import get, post

# Endpoint URL
endpoint = r"<Endpoint>"
post_url = endpoint + r"/formrecognizer/v2.1-preview.2/custom/models"
source = r"<SAS URL>"
prefix = "<Blob folder name>"
includeSubFolders = False
useLabelFile = True

headers = {
    # Request headers
    'Content-Type': 'application/json',
    'Ocp-Apim-Subscription-Key': '<subsription key>',
}

body =     {
    "source": source,
    "sourceFilter": {
        "prefix": prefix,
        "includeSubFolders": includeSubFolders
    },
    "useLabelFile": useLabelFile
}

try:
    resp = post(url = post_url, json = body, headers = headers)
    if resp.status_code != 201:
        print("POST model failed (%s):\n%s" % (resp.status_code, json.dumps(resp.json())))
        quit()
    print("POST model succeeded:\n%s" % resp.headers)
    get_url = resp.headers["location"]
except Exception as e:
    print("POST model failed:\n%s" % str(e))
    quit() 
```   

---



## <a name="get-training-results"></a>Получение результатов обучения

Запустив операцию обучения, вы можете получить ее состояние, используя полученный идентификатор. Добавьте следующий код в нижнюю часть сценария Python. В новом вызове API используется значение идентификатора, полученное из вызова обучения. Операция обучения выполняется асинхронно, поэтому этот скрипт регулярно опрашивает API, пока состояние обучения не примет значение "Завершено". Мы рекомендуем установить интервал одну секунду или более.

```python 
n_tries = 15
n_try = 0
wait_sec = 5
max_wait_sec = 60
while n_try < n_tries:
    try:
        resp = get(url = get_url, headers = headers)
        resp_json = resp.json()
        if resp.status_code != 200:
            print("GET model failed (%s):\n%s" % (resp.status_code, json.dumps(resp_json)))
            quit()
        model_status = resp_json["modelInfo"]["status"]
        if model_status == "ready":
            print("Training succeeded:\n%s" % json.dumps(resp_json))
            quit()
        if model_status == "invalid":
            print("Training failed. Model is invalid:\n%s" % json.dumps(resp_json))
            quit()
        # Training still running. Wait and retry.
        time.sleep(wait_sec)
        n_try += 1
        wait_sec = min(2*wait_sec, max_wait_sec)     
    except Exception as e:
        msg = "GET model failed:\n%s" % str(e)
        print(msg)
        quit()
print("Train operation did not complete within the allocated time.")
```

По завершении процесса обучения вы получите ответ `201 (Success)` с содержимым JSON, которое выглядит примерно так: Ответ сокращен для простоты.

```json
{ 
  "modelInfo":{ 
    "status":"ready",
    "createdDateTime":"2019-10-08T10:20:31.957784",
    "lastUpdatedDateTime":"2019-10-08T14:20:41+00:00",
    "modelId":"1cfb372bab404ba3aa59481ab2c63da5"
  },
  "trainResult":{ 
    "trainingDocuments":[ 
      { 
        "documentName":"invoices\\Invoice_1.pdf",
        "pages":1,
        "errors":[ 

        ],
        "status":"succeeded"
      },
      { 
        "documentName":"invoices\\Invoice_2.pdf",
        "pages":1,
        "errors":[ 

        ],
        "status":"succeeded"
      },
      { 
        "documentName":"invoices\\Invoice_3.pdf",
        "pages":1,
        "errors":[ 

        ],
        "status":"succeeded"
      },
      { 
        "documentName":"invoices\\Invoice_4.pdf",
        "pages":1,
        "errors":[ 

        ],
        "status":"succeeded"
      },
      { 
        "documentName":"invoices\\Invoice_5.pdf",
        "pages":1,
        "errors":[ 

        ],
        "status":"succeeded"
      }
    ],
    "errors":[ 

    ]
  },
  "keys":{ 
    "0":[ 
      "Address:",
      "Invoice For:",
      "Microsoft",
      "Page"
    ]
  }
}
```

Скопируйте это значение `"modelId"`, чтобы использовать на следующих шагах.

[!INCLUDE [analyze forms](../includes/python-custom-analyze.md)]

По завершении процесса вы получите ответ `202 (Success)` с содержимым JSON в следующем формате. Ответ сокращен для простоты. Основные связи "ключ — значение" находятся в узле `"documentResults"`. Узел `"selectionMarks"` (версии 2.1) отображает каждую отметку выделения (флажок или переключатель) и состояние "выбрано" или "не выбрано". Результаты вызова API макета (содержимое и положения всех текстовых элементов в документе) находятся в узле `"readResults"`.

# <a name="v20"></a>[Версия 2.0](#tab/v2-0)
```json
{
  "status": "succeeded",
  "createdDateTime": "2020-08-21T02:16:28Z",
  "lastUpdatedDateTime": "2020-08-21T02:16:35Z",
  "analyzeResult": {
    "version": "2.0.0",
    "readResults": [
      {
        "page": 1,
        "language": "en",
        "angle": 0,
        "width": 8.5,
        "height": 11,
        "unit": "inch",
        "lines": [
          {
            "boundingBox": [
              0.5826,
              0.4411,
              2.3387,
              0.4411,
              2.3387,
              0.7969,
              0.5826,
              0.7969
            ],
            "text": "Contoso, Ltd.",
            "words": [
              {
                "boundingBox": [
                  0.5826,
                  0.4411,
                  1.744,
                  0.4411,
                  1.744,
                  0.7969,
                  0.5826,
                  0.7969
                ],
                "text": "Contoso,",
                "confidence": 1
              },
              {
                "boundingBox": [
                  1.8448,
                  0.4446,
                  2.3387,
                  0.4446,
                  2.3387,
                  0.7631,
                  1.8448,
                  0.7631
                ],
                "text": "Ltd.",
                "confidence": 1
              }
            ]
          },
          ...
            ]
          }
        ] 
      }
    ],
    "pageResults": [
      {
        "page": 1,
        "tables": [
          {
            "rows": 5,
            "columns": 5,
            "cells": [
              {
                "rowIndex": 0,
                "columnIndex": 0,
                "text": "Training Date",
                "boundingBox": [
                  0.5133,
                  4.2167,
                  1.7567,
                  4.2167,
                  1.7567,
                  4.4492,
                  0.5133,
                  4.4492
                ],
                "elements": [
                  "#/readResults/0/lines/14/words/0",
                  "#/readResults/0/lines/14/words/1"
                ]
              },
              ...
            ]
          },
        ]
      }
    ],
    "documentResults": [
      {
        "docType": "custom:form",
        "pageRange": [
          1,
          1
        ],
        "fields": {
          "Receipt No": {
            "type": "string",
            "valueString": "9876",
            "text": "9876",
            "page": 1,
            "boundingBox": [
              7.615,
              1.245,
              7.915,
              1.245,
              7.915,
              1.35,
              7.615,
              1.35
            ],
            "confidence": 1,
            "elements": [
              "#/readResults/0/lines/3/words/3"
            ]
          },
          ...
        }
      }
    ],
    "errors": []
  }
}
```
# <a name="v-2"></a>[Версия 2](#tab/v2-1) 
```json   
{
  "status": "succeeded",
  "createdDateTime": "2020-08-21T02:29:42Z",
  "lastUpdatedDateTime": "2020-08-21T02:29:50Z",
  "analyzeResult": {
    "version": "2.1.0",
    "readResults": [
      {
        "page": 1,
        "angle": 0,
        "width": 8.5,
        "height": 11,
        "unit": "inch",
        "lines": [
          {
            "boundingBox": [
              0.5826,
              0.4411,
              2.3387,
              0.4411,
              2.3387,
              0.7969,
              0.5826,
              0.7969
            ],
            "text": "Contoso, Ltd.",
            "words": [
              {
                "boundingBox": [
                  0.5826,
                  0.4411,
                  1.744,
                  0.4411,
                  1.744,
                  0.7969,
                  0.5826,
                  0.7969
                ],
                "text": "Contoso,",
                "confidence": 1
              },
              {
                "boundingBox": [
                  1.8448,
                  0.4446,
                  2.3387,
                  0.4446,
                  2.3387,
                  0.7631,
                  1.8448,
                  0.7631
                ],
                "text": "Ltd.",
                "confidence": 1
              }
            ]
          },
          ...
        ], 
        "selectionMarks": [
          {
            "boundingBox": [
              3.9737,
              3.7475,
              4.1693,
              3.7475,
              4.1693,
              3.9428,
              3.9737,
              3.9428
            ],
            ...
        ] 
      }
    ],
    "pageResults": [
      {
        "page": 1,
        "tables": [
          {
            "rows": 5,
            "columns": 5,
            "cells": [
              {
                "rowIndex": 0,
                "columnIndex": 0,
                "text": "Training Date",
                "boundingBox": [
                  0.5133,
                  4.2167,
                  1.7567,
                  4.2167,
                  1.7567,
                  4.4492,
                  0.5133,
                  4.4492
                ],
                "elements": [
                  "#/readResults/0/lines/12/words/0",
                  "#/readResults/0/lines/12/words/1"
                ]
              },
              ...
            ]
          }
        ] 
      }
    ], 
    "documentResults": [
      {
        "docType": "custom:e1073364-4f3d-4797-8cc4-4bdbcd0dab6b",
        "modelId": "e1073364-4f3d-4797-8cc4-4bdbcd0dab6b",
        "pageRange": [
          1,
          1
        ],
        "fields": {
          "ID #": {
            "type": "string",
            "valueString": "5554443",
            "text": "5554443",
            "page": 1,
            "boundingBox": [
              2.315,
              2.43,
              2.74,
              2.43,
              2.74,
              2.515,
              2.315,
              2.515
            ],
            "confidence": 1,
            "elements": [
              "#/readResults/0/lines/8/words/1"
            ]
          },
          ...
        },
        "docTypeConfidence": 1
      }
    ],
    "errors": []
  }
}
```

---


## <a name="improve-results"></a>Улучшение результатов

Изучите значения `"confidence"` для каждого результата "ключ — значение" в узле `"documentResults"`. Также следует обратить внимание на оценки достоверности в узле `"readResults"` с учетом операции макета. Достоверность результатов операции макета не влияет на достоверность результатов извлечения пар "ключ — значение", поэтому следует проверять оба значения.
* Если оценка достоверности для операции макета мала, попробуйте улучшить качество входных документов (см. [требования к входным данным](../overview.md#input-requirements)).
* Если оценки достоверности для операции извлечения ключей и значений низкие, убедитесь, что у анализируемых документов и документов в наборе для обучения одинаковый тип. Если внешний вид документов в обучающем наборе отличается, попробуйте рассортировать их по разным папкам и обучить разные модели соответствующим образом.

### <a name="avoid-cluttered-labels"></a>Избегайте перегруженности метками

Иногда служба может объединять в одно поле несколько меток, указанных в пределах одной строки текста. Например, в строке адреса вы можете пометить город, штат и почтовый индекс как разные поля, но на этапе прогнозирования эти поля не будут распознаны отдельно.

Мы понимаем, что такой сценарий работы важен для наших клиентов, и стараемся улучшить его обработку. Сейчас мы рекомендуем пользователям маркировать несколько перегруженных полей как одно поле, а затем разделять термины при дальнейшей обработке результатов извлечения.

## <a name="next-steps"></a>Дальнейшие действия

В этом кратком руководстве описано, как выполнять обучение модели с использованием данных с метками, присвоенными вручную, с помощью REST API Распознавателя документов и Python. Ознакомьтесь со справочной документацией по API, чтобы узнать больше об API Распознавателя документов.

> [!div class="nextstepaction"]
> [Справочная документация по REST API](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/AnalyzeWithCustomForm)
