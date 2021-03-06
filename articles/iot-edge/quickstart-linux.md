---
title: Краткое руководство по созданию устройства Azure IoT Edge на платформе Linux | Документация Майкрософт
description: Из этого краткого руководства вы узнаете, как создать устройство IoT Edge под управлением Linux и удаленно развернуть готовый код на портале Azure.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/30/2020
ms.topic: quickstart
ms.service: iot-edge
services: iot-edge
ms.custom: mvc, devx-track-azurecli
ms.openlocfilehash: 720a4d14a73350d98b3f9054f748b93d296be11b
ms.sourcegitcommit: 1d6ec4b6f60b7d9759269ce55b00c5ac5fb57d32
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/13/2020
ms.locfileid: "94579323"
---
# <a name="quickstart-deploy-your-first-iot-edge-module-to-a-virtual-linux-device"></a>Краткое руководство. Развертывание модуля IoT Edge на виртуальном устройстве Linux

Проверьте работу Azure IoT Edge, как описано в этом кратком руководстве, развернув контейнерный код на виртуальном устройстве IoT Edge (Linux). IoT Edge позволяет удаленно управлять кодом на устройствах, чтобы вы могли передавать больше рабочих нагрузок на пограничные устройства. Для работы с этим кратким руководством мы рекомендуем использовать виртуальную машину Azure для устройства IoT Edge. Так вы сможете быстро создать виртуальную машину для тестирования с установленной службой IoT Edge, а затем удалить ее по завершении работы.

Из этого краткого руководства вы узнаете, как выполнять следующие задачи:

* Создайте Центр Интернета вещей.
* Регистрация устройства IoT Edge в Центре Интернета вещей.
* Установка и запуск среды выполнения IoT Edge на виртуальном устройстве.
* Удаленное развертывание модуля на устройстве IoT Edge.

![Схема рассматриваемой в этом кратком руководстве архитектуры для устройства и облака](./media/quickstart-linux/install-edge-full.png)

В этом кратком руководстве описано, как создать виртуальную машину Linux, используемую как устройство IoT Edge. Затем вы развернете модуль на портале Azure для устройства. Модуль, который вы развернете с помощью этого краткого руководства, — это имитированный датчик, генерирующий данные температуры, влажности и давления. В других руководствах по Azure IoT Edge используются наработки из этой статьи: дополнительные развернутые модули, которые анализируют генерируемые данные для бизнес-аналитики.

Если у вас нет активной подписки Azure, перед началом работы создайте [бесплатную учетную запись](https://azure.microsoft.com/free).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Для выполнения многих действий, описанных в этом кратком руководстве, используется Azure CLI, а также расширение Интернета вещей Azure для предоставления дополнительных функций.

Добавьте расширение Интернета вещей Azure в экземпляр Cloud Shell.

   ```azurecli-interactive
   az extension add --name azure-iot
   ```

[!INCLUDE [iot-hub-cli-version-info](../../includes/iot-hub-cli-version-info.md)]

## <a name="prerequisites"></a>Предварительные требования

Облачные ресурсы.

* Группа ресурсов для управления всеми ресурсами, которые вы используете в этом кратком руководстве. В этом кратком руководстве и последующих руководствах мы используем пример имени группы ресурсов **IoTEdgeResources**.

   ```azurecli-interactive
   az group create --name IoTEdgeResources --location westus2
   ```

## <a name="create-an-iot-hub"></a>Создание Центра Интернета вещей

Начните с создания Центра Интернета вещей с помощью Azure CLI.

![Схема создания Центра Интернета вещей в облаке](./media/quickstart-linux/create-iot-hub.png)

Для целей этого руководства можно использовать бесплатный уровень. Если у вас уже есть центр Интернета вещей, который вы использовали ранее, вы можете продолжить работу с ним.

При помощи следующего кода создается бесплатный центр **F1** в группе ресурсов **IoTEdgeResources**. Замените `{hub_name}` уникальным именем центра Интернета вещей. Создание Центра Интернета вещей может занять несколько минут.

   ```azurecli-interactive
   az iot hub create --resource-group IoTEdgeResources --name {hub_name} --sku F1 --partition-count 2
   ```

   Если отобразится сообщение об ошибке с уведомлением о том, что в вашей подписке уже имеется один бесплатный центр, измените номер SKU на **S1**. В подписке может быть только один бесплатный Центр Интернета вещей. Если отобразится сообщение об ошибке с уведомлением о том, что имя недоступно, значит кто-то уже создал Центр Интернета вещей с таким именем. Выберите другое имя.

## <a name="register-an-iot-edge-device"></a>Регистрация устройства IoT Edge

Зарегистрируйте устройство IoT Edge в только что созданном Центре Интернета вещей.

![Схема регистрации устройства с помощью удостоверения Центра Интернета вещей](./media/quickstart-linux/register-device.png)

Создайте удостоверение для своего имитированного устройства IoT Edge, чтобы оно могло обмениваться данными с Центром Интернета вещей. Удостоверение устройства находится в облаке. Чтобы связать физическое устройство с удостоверением, нужно использовать уникальную строку подключения к устройству.

Так как поведение и управление устройств IoT Edge и обычных устройств Интернета вещей отличаются, укажите в удостоверении, что это устройство IoT Edge, с помощью флага `--edge-enabled`.

1. Чтобы создать устройство с именем **myEdgeDevice** в своем центре, введите следующую команду в Azure Cloud Shell.

   ```azurecli-interactive
   az iot hub device-identity create --device-id myEdgeDevice --edge-enabled --hub-name {hub_name}
   ```

   Если отобразится сообщение об ошибке при использовании ключей политики iothubowner, убедитесь, что в Cloud Shell установлена последняя версия расширения azure-iot.

2. Просмотрите строку подключения для устройства, которая связывает физическое устройство с его удостоверением в Центре Интернета вещей. Она содержит имя центра Интернета вещей и имя устройства, а также общий ключ, который используется для аутентификации подключений между ними. Мы будем использовать эту строку подключения еще раз в следующем разделе при настройке устройства IoT Edge.

   ```azurecli-interactive
   az iot hub device-identity connection-string show --device-id myEdgeDevice --hub-name {hub_name}
   ```

   ![Просмотр строки подключения в выходных данных CLI](./media/quickstart/retrieve-connection-string.png)

## <a name="configure-your-iot-edge-device"></a>Настройка устройства IoT Edge

Создайте виртуальную машину со средой выполнения Azure IoT Edge.

![Схема запуска среды выполнения на устройстве](./media/quickstart-linux/start-runtime.png)

Среда выполнения IoT Edge развертывается на всех устройствах IoT Edge. Она состоит из трех компонентов. *Управляющая программа безопасности IoT Edge* запускается при каждой загрузке устройства Edge, перезагружая его путем запуска агента IoT Edge. *Агент IoT Edge* упрощает развертывание и мониторинг модулей на устройстве IoT Edge, включая центр IoT Edge. *Центр IoT Edge* управляет взаимодействием между модулями на устройстве IoT Edge, а также между устройством и Центром Интернета вещей.

Укажите строку подключения к устройству во время конфигурации среды выполнения. Это строка, полученная с помощью Azure CLI. Эта строка свяжет ваше физическое устройство с удостоверением устройства IoT Edge в Azure.

### <a name="deploy-the-iot-edge-device"></a>Развертывание устройства IoT Edge

В этом разделе используется шаблон Azure Resource Manager для создания виртуальной машины и установки на ней среды выполнения IoT Edge. Если вы хотите использовать собственное устройство Linux, см. статью [Установка или удаление среды выполнения Azure IoT Edge](how-to-install-iot-edge.md).

Используйте следующую команду CLI, чтобы создать устройство IoT Edge на основе предварительно созданного шаблона [iotedge-vm-deploy](https://github.com/Azure/iotedge-vm-deploy).

* Пользователям Bash и Cloud Shell нужно скопировать следующую команду в текстовый редактор, а затем заменить текст заполнителя своими данными и скопировать его в окно Bash или Cloud Shell:

   ```azurecli-interactive
   az deployment group create \
   --resource-group IoTEdgeResources \
   --template-uri "https://aka.ms/iotedge-vm-deploy" \
   --parameters dnsLabelPrefix='my-edge-vm' \
   --parameters adminUsername='azureUser' \
   --parameters deviceConnectionString=$(az iot hub device-identity connection-string show --device-id myEdgeDevice --hub-name
   <REPLACE_WITH_HUB_NAME> -o tsv) \
   --parameters authenticationType='password'
   --parameters adminPasswordOrKey="<REPLACE_WITH_PASSWORD>"
   ```

* Пользователям PowerShell нужно скопировать следующую команду в окно PowerShell, а затем заменить текст заполнителя своими данными:

   ```azurecli
   az deployment group create `
   --resource-group IoTEdgeResources `
   --template-uri "https://aka.ms/iotedge-vm-deploy" `
   --parameters dnsLabelPrefix='my-edge-vm1' `
   --parameters adminUsername='azureUser' `
   --parameters deviceConnectionString=$(az iot hub device-identity connection-string show --device-id myEdgeDevice --hub-name <REPLACE_WITH_HUB_NAME> -o tsv) `
   --parameters authenticationType='password' `
   --parameters adminPasswordOrKey="<REPLACE_WITH_PASSWORD>"
   ```

Этот шаблон принимает следующие параметры:

| Параметр | Описание |
| --------- | ----------- |
| **resource-group** | Группа ресурсов, в которой будут созданы ресурсы. Используйте имя по умолчанию **IoTEdgeResources**, которое применяется в этой статье, или укажите имя существующей группы ресурсов в подписке. |
| **template-uri** | Указатель на используемый шаблон Resource Manager. |
| **dnsLabelPrefix** | Строка, которая будет использоваться для создания имени узла виртуальной машины. Используйте пример **my-edge-vm** или укажите новую строку. |
| **adminUsername** | Имя пользователя для учетной записи администратора виртуальной машины. Используйте пример **azureUser** или укажите новое имя пользователя. |
| **deviceConnectionString** | Строка подключения из удостоверения устройства в Центре Интернета вещей, которая используется для настройки среды выполнения IoT Edge на виртуальной машине. Команда CLI в этом параметре извлекает строку подключения. Замените текст заполнителя именем центра Интернета вещей. |
| **authenticationType** | Метод аутентификации для учетной записи администратора. В этом кратком руководстве используется аутентификация на основе **пароля**, но для этого параметра также можно задать значение **sshPublicKey**. |
| **adminPasswordOrKey** | Пароль или значение ключа SSH для учетной записи администратора. Замените текст заполнителя надежным паролем. Пароль должен содержать не менее 12 символов и содержать три из четырех типов следующих символов: строчные буквы, прописные буквы, цифры и специальные символы. |

По завершении развертывания вы должны получить в CLI выходные данные в формате JSON, которые содержат сведения SSH для подключения к виртуальной машине. Скопируйте значение записи **public SSH** раздела **outputs**:

   ![Получение значения открытого ключа SSH из выходных данных](./media/quickstart-linux/outputs-public-ssh.png)

### <a name="view-the-iot-edge-runtime-status"></a>Просмотр состояния среды выполнения IoT Edge

Остальная часть команд из этого краткого руководства выполняется на вашем устройстве IoT Edge, таким образом, вы можете увидеть происходящее на устройстве. Если вы используете виртуальную машину, подключитесь к ней, используя настроенное имя пользователя администратора и DNS-имя, возвращенное командой развертывания. DNS-имя можно также узнать на странице общих сведений о виртуальной машине на портале Azure. Подключитесь к виртуальной машине, используя приведенную ниже команду. Замените `{admin username}` и `{DNS name}` собственными значениями.

   ```console
   ssh {admin username}@{DNS name}
   ```

Подключившись к виртуальной машине, убедитесь, что среда выполнения установлена и настроена на устройстве IoT Edge.

1. Убедитесь, что управляющая программа безопасности IoT Edge выполняется как системная служба.

   ```bash
   sudo systemctl status iotedge
   ```

   ![Проверка того, выполняется ли управляющая программа IoT Edge как системная служба](./media/quickstart-linux/iotedged-running.png)

   >[!TIP]
   >Для запуска команд `iotedge` требуется более высокий уровень привилегий. После установки среды выполнения IoT Edge, выхода из компьютера и обратного входа ваши разрешения обновляются автоматически. До этого момента используйте перед командой префикс `sudo`.

2. Если нужно устранить неполадки со службой, извлеките журналы службы.

   ```bash
   journalctl -u iotedge
   ```

3. Просмотрите данные обо всех модулях, запущенных на устройстве IoT Edge. Так как служба запущена первый раз, отобразится только запущенный модуль **edgeAgent**. Модуль edgeAgent запускается по умолчанию и позволяет установить и запустить любые дополнительные модули, развертываемые на устройстве.

   ```bash
   sudo iotedge list
   ```

   ![Просмотр данных об одном модуле на устройстве](./media/quickstart-linux/iotedge-list-1.png)

Теперь устройство IoT Edge настроено. Оно готово для запуска модулей, развернутых в облаке.

## <a name="deploy-a-module"></a>Развертывание модуля

Управляя устройством Azure IoT Edge из облака, разверните модуль, который будет передавать данные телеметрии в Центр Интернета вещей.

![Схема: развертывание модуля из облака на устройство](./media/quickstart-linux/deploy-module.png)

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]

## <a name="view-generated-data"></a>Просмотр сформированных данных

В этом руководстве мы создали новое устройство IoT Edge и установили на нем среду выполнения IoT Edge. Затем с помощью портала Azure мы развернули модуль IoT Edge на устройстве, обеспечив возможность запуска без необходимости менять настройки на устройстве.

В этом примере отправленный модуль создает примеры данных среды, которые можно использовать для последующего тестирования. Имитируемый датчик выполняет мониторинг оборудования и окружающей среды. Например, этот датчик может быть в серверной комнате, производственном цехе или ветроэлектрической установке. В сообщении отображаются данные о температуре и влажности окружающей среды, температуре и давлении оборудования, а также метка времени. При работе с руководствами по IoT Edge используйте данные, созданные этим модулем, как тестовые данные для аналитики.

Откройте повторно командную строку на устройстве IoT Edge или используйте соединение SSH из Azure CLI. Убедитесь, что развернутый из облака модуль работает на вашем устройстве IoT Edge.

   ```bash
   sudo iotedge list
   ```

   ![Просмотр трех модулей на устройстве](./media/quickstart-linux/iotedge-list-2.png)

Убедитесь, что сообщения отправляются из модуля датчика температуры.

   ```bash
   sudo iotedge logs SimulatedTemperatureSensor -f
   ```

   >[!TIP]
   >В командах IoT Edge при использовании имен модулей учитывается регистр.

   ![Просмотр получаемых с модуля данных](./media/quickstart-linux/iotedge-logs.png)

Вы также можете просматривать сообщения, которые поступают в Центр Интернета вещей, используя [расширение Центра Интернета вещей Azure для Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit).

## <a name="clean-up-resources"></a>Очистка ресурсов

Если вы хотите продолжить ознакомление с руководствами по IoT Edge, используйте устройство, зарегистрированное и настроенное в рамках этого краткого руководства. Если нет, вы можете удалить созданные ресурсы Azure, чтобы избежать расходов.

Если вы создали виртуальную машину и Центр Интернета вещей в новой группе ресурсов, можно удалить эту группу и все связанные с ней ресурсы. Внимательно проверьте содержимое группы ресурсов. В ней не должно быть важных ресурсов. Если вы не хотите удалять всю группу, можно удалить отдельные ресурсы.

> [!IMPORTANT]
> Удаление группы ресурсов — процесс необратимый.

Удалите группу **IoTEdgeResources**. Удаление группы ресурсов может занять несколько минут.

```azurecli-interactive
az group delete --name IoTEdgeResources
```

Чтобы проверить, удалена ли группа ресурсов, просмотрите список групп ресурсов.

```azurecli-interactive
az group list
```

## <a name="next-steps"></a>Дальнейшие действия

При работе с этим кратким руководством вы создали устройство IoT Edge и с помощью облачного интерфейса Azure IoT Edge развернули код на устройстве. В итоге вы получили устройство для тестирования, генерирующее необработанные данные о своей среде.

Далее вы можете настроить локальную среду разработки, чтобы приступить к созданию модулей IoT Edge, которые выполняют бизнес-логику.

> [!div class="nextstepaction"]
> [Разработка модулей IoT Edge для устройств Linux](tutorial-develop-for-linux.md)
