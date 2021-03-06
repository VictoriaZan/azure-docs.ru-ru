---
author: mikeparker104
ms.author: miparker
ms.date: 06/02/2020
ms.service: notification-hubs
ms.topic: include
ms.openlocfilehash: 72065ce602c6d29255a3500e26d8c6f36a19ebed
ms.sourcegitcommit: 971a3a63cf7da95f19808964ea9a2ccb60990f64
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/19/2020
ms.locfileid: "85095510"
---
### <a name="register-your-ios-app-for-push-notifications"></a>Регистрация приложения iOS для работы с push-уведомлениями

Чтобы отправлять push-уведомления в приложение iOS, зарегистрируйте приложение в системе Apple, а также зарегистрируйте его для получения push-уведомлений.  

1. Если вы еще не зарегистрировали свое приложение, перейдите на [портал подготовки iOS](https://go.microsoft.com/fwlink/p/?LinkId=272456) в центре разработчиков Apple. Войдите на портал с идентификатором Apple ID, перейдите к разделу **Certificates, Identifiers & Profiles** (Сертификаты, идентификаторы и профили), а затем выберите **Identifiers** (Идентификаторы). Щелкните **+** , чтобы зарегистрировать новое приложение.

    ![Страница с идентификаторами приложения на портале подготовки iOS](./media/notification-hubs-common-enable-apple-push-notifications/notification-hubs-ios-appids.png)

1. На экране **Register a New Identifier** (Зарегистрировать новый идентификатор) выберите переключатель **App IDs** (Идентификаторы приложений). Затем выберите **Continue** (Продолжить).

    ![Страница регистрации нового идентификатора на портале подготовки iOS](./media/notification-hubs-common-enable-apple-push-notifications/notification-hubs-ios-appids-new.png)

1. Обновите следующие три значения для нового приложения и нажмите кнопку **Continue** (Продолжить).

   * **Описание**: Введите описательное имя для приложения.

   * **Bundle ID** (Идентификатор пакета). Введите идентификатор пакета в формате **com.<идентификатор_организации>.<имя_продукта>** , как описано в [руководстве по распространению приложения](https://help.apple.com/xcode/mac/current/#/dev91fe7130a). На следующем снимке экрана в качестве идентификатора организации используется значение `mobcat`, а в качестве названия продукта — значение **PushDemo**.

      ![Страница регистрации идентификатора приложения на портале подготовки iOS](./media/notification-hubs-common-enable-apple-push-notifications/notification-hubs-new-appid-bundle.png)

   * **Push-уведомления**. Проверьте опцию **Push Notifications** (Push-уведомления) в разделе **Capabilities** (Возможности).

      ![Форма для регистрации нового идентификатора приложения](./media/notification-hubs-common-enable-apple-push-notifications/notification-hubs-new-appid-push.png)

      При этом будет создан идентификатор вашего приложения, а также запрошено подтверждение информации. Выберите **Continue** (Продолжить), а затем нажмите **Register** (Зарегистрировать), чтобы подтвердить новый идентификатор приложения.

      ![Подтверждение нового идентификатора приложения](./media/notification-hubs-common-enable-apple-push-notifications/notification-hubs-new-appid-register.png)

      После выбора **Register** (Зарегистрировать) вы увидите новый идентификатор приложения в виде элемента строки на странице **Certificates, Identifiers & Profiles** (Сертификаты, идентификаторы и профили).

1. На странице **Certificates, Identifiers & Profiles** (Сертификаты, идентификаторы и профили) в разделе **Identifiers** (Идентификаторы) найдите строку с созданным элементом идентификатора приложения. Затем выберите эту строку, чтобы отобразить окно **Edit your App ID Configuration** (Изменение конфигурации идентификатора приложения).

#### <a name="creating-a-certificate-for-notification-hubs"></a>Создание сертификата для Центров уведомлений

Сертификат необходим для того, чтобы Центр уведомлений мог работать со **Службами push-уведомлений Apple (APNS)** и может предоставляться одним из двух способов:

1. [создание сертификата p12 службы push-уведомлений, который можно отправить прямо в концентратор уведомлений](#option-1-creating-a-p12-push-certificate-that-can-be-uploaded-directly-to-notification-hub) (*оригинальный подход*);

1. [создание сертификата p8, который можно использовать для проверки подлинности на основе токена](#option-2-creating-a-p8-certificate-that-can-be-used-for-token-based-authentication) (*более новый и на данный момент рекомендуемый подход*).

Новый подход имеет ряд преимуществ, которые описаны в документе о [проверке подлинности на основе токена (HTTP/2) для APNs](https://docs.microsoft.com/azure/notification-hubs/notification-hubs-push-notification-http2-token-authentification). Хотя при этом требуется меньше шагов, в некоторых сценариях этот подход является обязательным. Но здесь мы опишем процедуры для обоих подходов, так как для работы с этим руководством можно применить любой из них.

##### <a name="option-1-creating-a-p12-push-certificate-that-can-be-uploaded-directly-to-notification-hub"></a>Вариант 1. Создание сертификата p12 службы push-уведомлений, который можно отправить прямо в концентратор уведомлений

1. На компьютере Mac запустите средство Keychain Access. Его можно запустить из папки **Служебные программы** или **Другое** на панели запуска.

1. Нажмите **Связка ключей**, разверните **Помощник по сертификатам**, а затем выберите **Запросить сертификат в центре сертификации**.

    ![Использование Keychain Access для запрашивания нового сертификата](./media/notification-hubs-common-enable-apple-push-notifications/notification-hubs-request-cert-from-ca.png)

   > [!NOTE]
   > По умолчанию при осуществлении доступа к цепочке ключей выбирается первый элемент списка. Это может вызвать проблемы, если у вас открыта категория **сертификатов** и **центр сертификации Apple Worldwide Developer Relations** не указан в списке первым. Прежде чем создавать CSR (запрос на подписывание сертификата), убедитесь, что выбран элемент, не являющийся ключом, или ключ **центра сертификации Apple Worldwide Developer Relations**.

1. Заполните поля **Адрес электронной почты пользователя** и **Личное имя**, выберите **Сохранено на диск**, а затем — **Продолжить**. Оставьте поле **Адрес электронной почты ЦС** пустым, так как оно является необязательным.

    ![Ожидаемые сведения о сертификате](./media/notification-hubs-common-enable-apple-push-notifications/notification-hubs-csr-info.png)

1. Введите имя для **файла запроса подписи сертификата (CSR)** в поле **Save As** (Сохранить как), выберите расположение в поле **Where** (Папка) и щелкните **Save** (Сохранить).

    ![Выбор имени файла для сертификата](./media/notification-hubs-common-enable-apple-push-notifications/notification-hubs-save-csr.png)

    Это действие сохраняет **CSR-файл** в выбранном месте. Расположением по умолчанию является **рабочий стол**. Запомните расположение, выбранное для файла.

1. Вернитесь на страницу **Certificates, Identifiers & Profiles** (Сертификаты, идентификаторы и профили) на [портале подготовки iOS](https://go.microsoft.com/fwlink/p/?LinkId=272456), прокрутите вниз до флажка **Push Notifications** (Push-уведомления), который должен быть установлен, а затем выберите **Configure** (Настроить), чтобы создать сертификат.

    ![Страница изменения идентификатора приложения](./media/notification-hubs-common-enable-apple-push-notifications/notification-hubs-edit-appid.png)

1. Откроется окно **Apple Push Notification service TLS/SSL Certificates** (TLS- и SSL-сертификаты службы push-уведомлений Apple). Нажмите кнопку **Create Certificate** (Создать сертификат) в разделе **Development TLS/SSL Certificate** (TLS- или SSL-сертификат разработки).

    ![Кнопка создания сертификата для идентификатора приложения](./media/notification-hubs-common-enable-apple-push-notifications/notification-hubs-appid-create-cert.png)

    Появится экран **Create a new Certificate** (Создать новый сертификат).

    > [!NOTE]
    > В этом учебнике используется сертификат разработки. Тот же процесс используется при регистрации сертификата производства. Только убедитесь, что при отправке уведомлений используется тот же тип сертификата.

1. Щелкните **Choose File** (Выбрать файл), перейдите к папке, в которую вы сохранили **CSR-файл**, и дважды щелкните имя сертификата, чтобы загрузить его. Затем выберите **Continue** (Продолжить).

1. После того как сертификат будет создан на портале, нажмите кнопку **Download** (Скачать). Сохраните сертификат и запомните расположение, в котором он сохранен.

    ![Страница скачивания созданного сертификата](./media/notification-hubs-common-enable-apple-push-notifications/notification-hubs-appid-download-cert.png)

    При этом сертификат будет скачан и сохранен на вашем компьютере в папке **Загрузки**.

    ![Поиск файла сертификата в папке "Загрузки"](./media/notification-hubs-common-enable-apple-push-notifications/notification-hubs-cert-downloaded.png)

    > [!NOTE]
    > Скачанному сертификату разработки по умолчанию задано имя **aps_development.cer**.

1. Дважды щелкните скачанный сертификат push-уведомлений **aps_development.cer**. При этом новый сертификат устанавливается в Keychain, как на следующем изображении.

    ![Список сертификатов Keychain Access с новым сертификатом](./media/notification-hubs-common-enable-apple-push-notifications/notification-hubs-cert-in-keychain.png)

    > [!NOTE]
    > Имя вашего сертификата может отличаться, но оно обязательно будет начинаться с префикса **Apple Development iOS Push Services** и иметь связанный идентификатор пакета.

1. В программе Keychain Access с нажатой клавишей **CTRL** **щелкните** новый сертификат push-уведомлений, который вы создали в категории **Certificates** (Сертификаты). Щелкните **Export** (Экспорт), укажите имя файла, выберите формат **p12** и нажмите кнопку **Save** (Сохранить).

    ![Экспорт сертификата в формате р12](./media/notification-hubs-common-enable-apple-push-notifications/notification-hubs-export-cert-p12.png)

    Вы можете выбрать защиту сертификата с помощью пароля, но это необязательно. Нажмите кнопку **ОК**, если хотите обойти создание пароля. Запишите имя файла и расположение экспортируемого сертификата в формате p12. Они нужны для проверки подлинности в APNs.

    > [!NOTE]
    > Имя файла в формате p12 и его расположение могут отличаться от изображенных в этом руководстве.

##### <a name="option-2-creating-a-p8-certificate-that-can-be-used-for-token-based-authentication"></a>Вариант 2. Создание сертификата p8, который можно использовать для проверки подлинности на основе маркеров

1. Запишите следующие значения:

    * значение **App ID Prefix** (Префикс идентификатора приложения, который выполняет роль **Идентификатора команды**);
    * значение **Bundle ID** (Идентификатор пакета).

1. Вернитесь на страницу **Certificates, Identifiers & Profiles** (Сертификаты, идентификаторы и профили), щелкните **Keys** (Ключи).

   > [!NOTE]
   > Если вы уже настроили ключ для **APNs**, вы можете повторно использовать сертификат p8, который вы скачали после его создания. В таком случае можно пропустить шаги **3**–**5**.

1. Чтобы создать ключ, нажмите кнопку **+** (кнопка **Create a key** (Создать ключ)).
1. Укажите подходящее значение в поле **Key Name** (Имя ключа), установите флажок **Apple Push Notifications service (APNs)** (Служба push-уведомлений Apple (APNs)), щелкните **Continue** (Продолжить) и на следующем экране нажмите кнопку **Register** (Зарегистрировать).
1. Щелкните **Download** (Скачать), переместите файл **p8** (с префиксом *AuthKey_* ) в защищенный локальный каталог и нажмите кнопку **Done** (Готово).

   > [!NOTE]
   > Обязательно сохраните файл p8 в безопасном месте и создайте его резервную копию. Скачав ключ один раз, вы не сможете скачать его повторно, так как копия ключа удаляется с сервера.
  
1. В разделе **Keys** (Ключи) щелкните имя ключа, который вы создали, или имя существующего ключа, если вы решили использовать его.
1. Запишите значение **Key ID** (Идентификатор ключа).
1. Откройте файл сертификата с расширением p8 в подходящем приложении, например [**Visual Studio Code**](https://code.visualstudio.com). Сохраните значение ключа, которое находится между строками **-----BEGIN PRIVATE KEY-----** и **-----END PRIVATE KEY-----** .

    -----BEGIN PRIVATE KEY-----  
    <значение_ключа>  
    -----END PRIVATE KEY-----

    > [!NOTE]
    > Это **значение токена**, которое понадобится позже для настройки **концентратора уведомлений**.

По завершении у вас должны быть следующие сведения, которые вы будете использовать для [настройки концентратора уведомлений на основе сведений APNs](#configure-your-notification-hub-with-apns-information):

* **идентификатор команды** (см. шаг 1);
* **идентификатор пакета** (см. шаг 1);
* **идентификатор ключа** (см. шаг 7);
* **Значение маркера** (значение ключа p8, полученное на шаге 8)

### <a name="create-a-provisioning-profile-for-the-app"></a>Создание профиля подготовки для приложения

1. Вернитесь на [портал подготовки iOS](https://go.microsoft.com/fwlink/p/?LinkId=272456), выберите **Certificates, Identifiers & Profiles** (Сертификаты, идентификаторы и профили), нажмите **Profiles** (Профили) в меню слева, а затем выберите **+** , чтобы создать новый профиль. Откроется окно **Register a New Provisioning Profile** (Зарегистрировать новый профиль подготовки).

1. Выберите **iOS App Development** (Разработка приложений для iOS) в разделе **Development** (Разработка) в качестве типа профиля подготовки и нажмите кнопку **Continue** (Продолжить).

    ![Список с профилем подготовки](./media/notification-hubs-common-enable-apple-push-notifications/notification-hubs-new-provisioning-profile.png)

1. Затем в раскрывающемся списке **App ID** (Идентификатор приложения) выберите созданный идентификатор приложения и нажмите кнопку **Continue** (Продолжить).

    ![Выбор идентификатора приложения](./media/notification-hubs-common-enable-apple-push-notifications/notification-hubs-select-appid-for-provisioning.png)

1. В окне **Select certificates** (Выбор сертификатов) выберите сертификат для разработки, используемый для подписывания кода, и нажмите кнопку **Continue** (Продолжить).

    > [!NOTE]
    > Это не тот сертификат push-уведомлений, который вы создали на [предыдущем шаге](#creating-a-certificate-for-notification-hubs). Это ваш сертификат разработки. Если он не существует, его необходимо создать, так как это [необходимое условие](#prerequisites) для работы с этим руководством. Сертификаты разработчика можно создавать на [Портале разработчика Apple](https://developer.apple.com)с помощью [Xcode](https://developer.apple.com/library/archive/documentation/ToolsLanguages/Conceptual/YourFirstAppStoreSubmission/ProvisionYourDevicesforDevelopment/ProvisionYourDevicesforDevelopment.html) или в [Visual Studio](https://docs.microsoft.com/xamarin/ios/get-started/installation/device-provisioning/).

1. Вернитесь на страницу **Certificates, Identifiers & Profiles** (Сертификаты, идентификаторы и профили), нажмите **Profiles** (Профили) в меню слева, а затем **+** , чтобы создать новый профиль. Откроется окно **Register a New Provisioning Profile** (Зарегистрировать новый профиль подготовки).

1. В окне **Select certificates** (Выбор сертификатов) выберите сертификат разработки, который вы создали. Затем выберите **Continue** (Продолжить).

1. Затем выберите устройства для тестирования и нажмите кнопку **Continue** (Продолжить).

1. Наконец, выберите имя профиля в поле **Provisioning Profile Name** (Имя профиля подготовки) и нажмите кнопку **Generate** (Создать).

    ![Выбор имени профиля подготовки](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-name-profile.png)

1. После создания нового профиля подготовки выберите **Download** (Скачать). Запомните расположение, в котором он сохранен.

1. Перейдите в расположение профиля подготовки и дважды щелкните его, чтобы установить на компьютере разработки.
