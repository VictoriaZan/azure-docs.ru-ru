---
title: Прогноз погоды с данными центра Интернета вещей Машинное обучение Azure Studio (классическая модель)
description: Используйте Машинное обучение Azure Studio (классическая модель) для прогнозирования вероятности дождя на основе данных о температуре и влажности, собираемых центром Интернета вещей из датчика.
author: robinsh
manager: philmea
keywords: Прогнозирование погоды с помощью машинного обучения
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 09/16/2020
ms.author: robinsh
ms.openlocfilehash: ab9e122ba0b2b50203a2d66ae14f03f3b6300f96
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/01/2020
ms.locfileid: "96452347"
---
# <a name="weather-forecast-using-the-sensor-data-from-your-iot-hub-in-azure-machine-learning-studio-classic"></a>Прогноз погоды с использованием данных датчика из центра Интернета вещей в Машинное обучение Azure Studio (классическая модель)

![Комплексная схема](media/iot-hub-get-started-e2e-diagram/6.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

Машинное обучение — это метод обработки и анализа данных, который позволяет компьютеру на основе исторических данных прогнозировать ожидаемые поведения, результаты и тенденции. Студия машинного обучения Azure (классическая) — это облачная служба прогнозной аналитики, которая позволяет быстро создавать и развертывать прогнозные модели в качестве решений аналитики.

## <a name="what-you-learn"></a>Что вы узнаете

Вы узнаете, как использовать Машинное обучение Azure Studio (классическая модель) для прогнозирования погоды (вероятность дождя) с помощью данных о температуре и влажности из центра Интернета вещей Azure. Вероятность дождя вычисляется по заранее подготовленной модели прогнозирования погоды. Эта модель создается на основе исторических данных и применяется для оценки вероятности дождя, исходя из данных о температуре и влажности.

## <a name="what-you-do"></a>Что нужно сделать

- Развернем модель прогнозирования погоды как веб-службу.
- добавим группу потребителей, чтобы обеспечить доступ к данным в Центре Интернета вещей;
- Создадим задание Stream Analytics и настроим его на выполнение следующих задач:
  - получение данных о температуре и влажности от Центра Интернета вещей;
  - вызов веб-службы для получения оценки вероятности дождя;
  - сохранение результатов в хранилище BLOB-объектов Azure;
- Откроем обозреватель хранилища Microsoft Azure для просмотра прогноза погоды.

## <a name="what-you-need"></a>Необходимые элементы

- Пройдите одно из руководств по [использованию симулятора Raspberry Pi в подключенном режиме](iot-hub-raspberry-pi-web-simulator-get-started.md), например [Подключение онлайн-симулятора Raspberry Pi к Центру Интернета вещей Azure (Node.js)](iot-hub-raspberry-pi-kit-node-get-started.md). В них используется следующее:
  - Активная подписка Azure.
  - Центр Интернета вещей Azure в подписке;
  - клиентское приложение, которое отправляет сообщения в Центр Интернета вещей Azure.
- Учетная запись [Студии машинного обучения Azure (классической)](https://studio.azureml.net/).
- Учетная запись [хранения Azure](../storage/common/storage-account-overview.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#types-of-storage-accounts)является **General-purpose v2** предпочтительной, но все учетные записи хранения Azure, поддерживающие хранилище BLOB-объектов Azure, также будут работать.

> [!Note]
> В этой статье используется Azure Stream Analytics и несколько других платных услуг. Дополнительная плата взимается Azure Stream Analytics, когда данные должны передаваться в регионах Azure. По этой причине было бы полезно убедиться, что группа ресурсов, центр Интернета вещей и учетная запись хранения Azure, а также Машинное обучение Studio (классическая модель) и Azure Stream Analytics задание, добавленные далее в этом руководстве, находятся в одном регионе Azure. Вы можете проверить региональную поддержку для Машинное обучение Azure Studio (классическая модель) и других служб Azure на [странице доступность продуктов Azure по регионам](https://azure.microsoft.com/global-infrastructure/services/?products=machine-learning-studio&regions=all).

## <a name="deploy-the-weather-prediction-model-as-a-web-service"></a>Развертывание модели прогнозирования погоды как веб-службы

В этом разделе показано, как получить модель прогнозирования погоды из библиотеки ИИ Azure. Затем вы добавите в модель модуль R-script для очистки данных о температуре и влажности. Наконец, вы развернете модель в качестве прогнозной веб-службы.

### <a name="get-the-weather-prediction-model"></a>Получение модели прогнозирования погоды

В этом разделе показано, как получить модель прогнозирования погоды из Коллекции решений ИИ Azure и открыть ее в Студии машинного обучения Azure (классической).

1. Откройте [страницу модели прогнозирования погоды](https://gallery.cortanaintelligence.com/Experiment/Weather-prediction-model-1).

   ![Открытая страница модели прогнозирования погоды в Коллекции решений ИИ Azure](media/iot-hub-weather-forecast-machine-learning/weather-prediction-model-in-azure-ai-gallery.png)

1. Выберите **Открыть в студии (классическая модель)** , чтобы открыть ее в студия машинного обучения Microsoft Azure (классической). Выберите регион рядом с центром Интернета вещей и нужную рабочую область в всплывающем окне **копирования эксперимента из коллекции** .

   ![Открытие модели прогнозирования погоды в Студии машинного обучения Azure (классической)](media/iot-hub-weather-forecast-machine-learning/open-ml-studio.png)

### <a name="add-an-r-script-module-to-clean-temperature-and-humidity-data"></a>Добавление модуля R-script для очистки данных о температуре и влажности

Для правильной работы модели данные температуры и влажности должны поддерживать преобразование в числовые данные. В этом разделе показано, как добавить в модель прогнозирования погоды модуль R-script, который удаляет все строки, значения температуры и влажности, которые нельзя преобразовать в числовые значения.

1. В левой части окна Машинное обучение Azure Studio (классическая модель) щелкните стрелку, чтобы развернуть панель Инструменты. Введите строку Execute в поле поиска. Выберите модуль **Execute R Script** (Выполнение скрипта R).

   ![Выбор модуля Execute R Script (Выполнение скрипта R)](media/iot-hub-weather-forecast-machine-learning/select-r-script-module.png)

1. Перетащите модуль **Execute R Script** (Выполнение скрипта R) и поместите рядом с модулем **Clean Missing Data** (Очистка отсутствующих данных) и уже существующим на схеме модулем **Execute R Script** (Выполнение скрипта R). Удалите связь между модулями **Clean Missing Data** (Очистка отсутствующих данных) и **Execute R Script** (Выполнение скрипта R), а затем соедините входы и выходы нового модуля, как показано ниже.

   ![Добавление модуля Execute R Script (Выполнение скрипта R)](media/iot-hub-weather-forecast-machine-learning/add-r-script-module.png)

1. Выберите новый модуль **Execute R Script** (Выполнение скрипта R), чтобы открыть окно его свойств. Скопируйте и вставьте следующий код в поле **R Script** (Скрипт R).

   ```r
   # Map 1-based optional input ports to variables
   data <- maml.mapInputPort(1) # class: data.frame

   data$temperature <- as.numeric(as.character(data$temperature))
   data$humidity <- as.numeric(as.character(data$humidity))

   completedata <- data[complete.cases(data), ]

   maml.mapOutputPort('completedata')

   ```

   Когда вы завершите работу, окно свойств будет выглядеть примерно так:

   ![Добавление кода в модуль Execute R Script (Выполнение скрипта R)](media/iot-hub-weather-forecast-machine-learning/add-code-to-module.png)

### <a name="deploy-predictive-web-service"></a>Развертывание прогнозной веб-службы

В этом разделе показано, как проверить модель, а также настроить и развернуть прогнозную веб-службу на основе этой модели.

1. Выберите **выполнить** , чтобы проверить шаги в модели. Этот шаг может занять несколько минут.

   ![Запуск эксперимента для проверки шагов](media/iot-hub-weather-forecast-machine-learning/run-experiment.png)

1. Выберите **настроить**  >  **прогнозную веб**-службу. Откроется схема прогнозного эксперимента

   ![Развертывание модели прогнозирования погоды в Студии машинного обучения Azure (классической)](media/iot-hub-weather-forecast-machine-learning/predictive-experiment.png)

1. На диаграмме прогнозного эксперимента удалите соединение между модулем **входные данные веб-службы** и **выберите столбцы в наборе данных в** верхней части. Затем перетащите модуль **Web service input** (Входные данные веб-службы) поближе к модулю **Score Model** (Оценка модели) и подключите его, как показано ниже.

   ![Соединение двух модулей в Студии машинного обучения Azure (классической)](media/iot-hub-weather-forecast-machine-learning/connect-modules-azure-machine-learning-studio.png)

1. Выберите **выполнить** , чтобы проверить шаги в модели.

1. Выберите пункт **развернуть веб-службу** , чтобы развернуть модель как веб-службу.

1. На панели мониторинга модели скачайте **Excel 2010 or earlier workbook** (Книга Excel 2010 или более ранних версий) для действия **REQUEST/RESPONSE** (Запрос — ответ).

   > [!Note]
   > Обязательно используйте именно **книгу Excel 2010 или более ранних версий**, даже если на вашем компьютере установлена более поздняя версия Excel.

   ![Скачивание книги Excel для конечной точки REQUEST RESPONSE ("запрос — ответ")](media/iot-hub-weather-forecast-machine-learning/download-workbook.png)

1. Откройте эту книгу Excel и запишите параметры **WEB SERVICE URL** (URL веб-службы) и **ACCESS KEY** (Ключ доступа).

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="create-configure-and-run-a-stream-analytics-job"></a>Создание, настройка и выполнение заданий Stream Analytics

### <a name="create-a-stream-analytics-job"></a>Создание задания Stream Analytics

1. В [портал Azure](https://portal.azure.com/)выберите **создать ресурс**. Введите "Задание Stream Analytics" в поле поиска и выберите **Stream Analytics задание** в раскрывающемся списке результатов. Когда откроется область **задания Stream Analytics** , выберите **создать**.
1. Введите представленные ниже сведения для задания.

   **Имя задания.** Имя задания. Оно должно быть глобально уникальным.

   **Подписка**. Выберите подписку, если она отличается от используемой по умолчанию.

   **Группа ресурсов.** Выберите ту же группу ресурсов, которую использует центр Интернета вещей.

   **Расположение.** Выберите то же расположение, которое использует группа ресурсов.

   Оставьте все остальные поля по умолчанию.

   ![Создание задания Stream Analytics в Azure](media/iot-hub-weather-forecast-machine-learning/create-stream-analytics-job.png)

1. Нажмите кнопку **создания**.

### <a name="add-an-input-to-the-stream-analytics-job"></a>Добавление входных данных в задание Stream Analytics

1. Откройте задание Stream Analytics.
1. В разделе **Топология задания** выберите **Входные данные**.
1. В области **входные данные** выберите **добавить потоковый вход**, а затем в раскрывающемся списке выберите **центр Интернета вещей** . На **новой панели ввода** выберите в **подписках пункт выбрать центр Интернета вещей** и введите следующие сведения:

   **Входной псевдоним.** Уникальный псевдоним для выходных данных.

   **Подписка**. Выберите подписку, если она отличается от используемой по умолчанию.

   **Центр Интернета вещей**. Выберите центр Интернета вещей из подписки.

   **Имя политики общего доступа**: выберите  **Служба**. (Можно также использовать **iothubowner**.)

   **Группа потребителей.** Выберите созданную группу потребителей.

   Оставьте все остальные поля по умолчанию.

   ![Добавление входных данных в задание Stream Analytics в Azure](media/iot-hub-weather-forecast-machine-learning/add-input-stream-analytics-job.png)

1. Щелкните **Сохранить**.

### <a name="add-an-output-to-the-stream-analytics-job"></a>Добавление выходных данных в задание Stream Analytics

1. В разделе **Топология задания** выберите **Выходные данные**.
1. В области **выходные данные** выберите **Добавить**, а затем выберите **хранилище BLOB-объектов и Data Lake Storage** из раскрывающегося списка. На **новой панели вывода** выберите **вариант хранилище из подписок** и введите следующие сведения.

   **Выходной псевдоним.** Уникальный псевдоним для выходных данных.

   **Подписка**. Выберите подписку, если она отличается от используемой по умолчанию.

   **Учетная запись хранения**. Это учетная запись хранения для вашего хранилища больших двоичных объектов. Можно использовать существующую учетную запись хранения или создать новую.

   **Контейнер.** Это контейнер, в котором хранится большой двоичный объект. Вы можете создать новый контейнер или использовать уже существующий.

   **Формат сериализации событий.** выберите **CSV**.

   ![Добавление выходных данных в задание Stream Analytics в Azure](media/iot-hub-weather-forecast-machine-learning/add-output-stream-analytics-job.png)

1. Щелкните **Сохранить**.

### <a name="add-a-function-to-the-stream-analytics-job-to-call-the-web-service-you-deployed"></a>Добавление в задание Stream Analytics функции для вызова развернутой веб-службы

1. В разделе **топология задания** выберите **функции**.
1. В области **функции** выберите **Добавить**, а затем выберите **Azure ML Studio** в раскрывающемся списке. (Убедитесь, что вы выбрали **Azure ML Studio**, а не **службу машинного обучения Azure**.) В области **Новая функция** выберите **параметр указать машинное обучение Azureную функцию вручную** и введите следующие сведения:

   **Псевдоним функции.** Введите `machinelearning`.

   **URL-адрес**. Введите URL веб-службы, который вы взяли из книги Excel.

   **Ключ**: Введите значение ACCESS KEY, полученное в книге Excel.

   ![Добавление функции в задание Stream Analytics в Azure](media/iot-hub-weather-forecast-machine-learning/add-function-stream-analytics-job.png)

1. Щелкните **Сохранить**.

### <a name="configure-the-query-of-the-stream-analytics-job"></a>Настройка запроса задания Stream Analytics

1. В разделе **Топология задания** выберите **Запрос**.
1. Замените текст запроса следующим кодом.

   ```sql
   WITH machinelearning AS (
      SELECT EventEnqueuedUtcTime, temperature, humidity, machinelearning(temperature, humidity) as result from [YourInputAlias]
   )
   Select System.Timestamp time, CAST (result.[temperature] AS FLOAT) AS temperature, CAST (result.[humidity] AS FLOAT) AS humidity, CAST (result.[scored probabilities] AS FLOAT ) AS 'probabalities of rain'
   Into [YourOutputAlias]
   From machinelearning
   ```

   Замените значение `[YourInputAlias]` значением псевдонима входных данных задания.

   Замените значение `[YourOutputAlias]` значением псевдонима выходных данных задания.

1. Выберите **Сохранить запрос**.

> [!Note]
> Если выбрать **тестовый запрос**, отобразится следующее сообщение: тестирование запросов с машинное обучениеными функциями не поддерживается. Измените запрос и повторите попытку. Можно спокойно проигнорировать это сообщение и нажать кнопку **ОК** , чтобы закрыть окно сообщения. Убедитесь, что запрос сохранен перед переходом к следующему разделу.

### <a name="run-the-stream-analytics-job"></a>Выполнение задания Stream Analytics

В Stream Analytics задание выберите **Обзор** в левой области. Затем выберите **запустить**  >  **сейчас**  >  **запустить**. После успешного запуска состояние задания **Остановлено** изменится на **Выполняется**.

![Выполнение задания Stream Analytics](media/iot-hub-weather-forecast-machine-learning/run-stream-analytics-job.png)

## <a name="use-microsoft-azure-storage-explorer-to-view-the-weather-forecast"></a>Использование обозревателя службы хранилища Microsoft Azure для просмотра прогноза погоды

Запустите клиентское приложение, чтобы начать сбор и отправку данных о температуре и влажности в Центр Интернета вещей. Для каждого сообщения, полученного Центром Интернета вещей, задание Stream Analytics вызывает веб-службу прогнозирования погоды, чтобы оценить вероятности дождя. Полученный результат сохраняется в хранилище BLOB-объектов Azure. Сохраненные результаты можно просмотреть с помощью обозревателя службы хранилища Azure.

1. [Скачайте и установите обозреватель хранилищ Microsoft Azure](https://storageexplorer.com/).
1. Откройте обозреватель службы хранилища Azure.
1. Войдите в учетную запись Azure.
1. Выберите свою подписку.
1. Выберите свою подписку > **учетные записи хранения** > учетной записи хранения > **контейнеры больших двоичных объектов** > контейнере.
1. Скачайте CSV-файл, чтобы увидеть результат. Последний столбец содержит прогнозируемую вероятность дождя.

   ![Получение результатов прогноза погоды с помощью Машинное обучение Azure Studio (классическая модель)](media/iot-hub-weather-forecast-machine-learning/weather-forecast-result.png)

## <a name="summary"></a>Итоги

Вы успешно использовали Машинное обучение Azure Studio (классическая модель) для создания шанса дождя на основе данных о температуре и влажности, получаемых Центром Интернета вещей.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]