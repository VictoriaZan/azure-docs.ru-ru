---
title: Руководство по Интеграция Azure Active Directory с Ungerboeck Software | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в Ungerboeck Software.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 06/19/2019
ms.author: jeedes
ms.openlocfilehash: ddc83da2836d414ee3fadd23288475a6829018aa
ms.sourcegitcommit: 59f506857abb1ed3328fda34d37800b55159c91d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/24/2020
ms.locfileid: "92521793"
---
# <a name="tutorial-integrate-ungerboeck-software-with-azure-active-directory"></a>Руководство по Интеграция Ungerboeck Software с Azure Active Directory

В этом руководстве описано, как интегрировать Ungerboeck Software с Azure Active Directory (Azure AD). Интеграция Ungerboeck Software с Azure AD обеспечивает следующие возможности.

* С помощью Azure AD можно контролировать доступ к Ungerboeck Software.
* Автоматический вход пользователей в Ungerboeck Software с помощью учетных записей Azure AD.
* Централизованное управление учетными записями через портал Azure.

Чтобы узнать больше об интеграции приложений SaaS с Azure AD, прочитайте статью [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Предварительные требования

Чтобы приступить к работе, потребуется следующее.

* Подписка Azure AD. Если у вас нет подписки, [здесь](https://azure.microsoft.com/pricing/free-trial/) вы можете получить бесплатную пробную версию сроком на один месяц.
* подписка Ungerboeck Software с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде. Ungerboeck Software поддерживает единый вход, инициируемый **поставщиком услуг**.

## <a name="adding-ungerboeck-software-from-the-gallery"></a>Добавление Ungerboeck Software из коллекции

Чтобы настроить интеграцию Ungerboeck Software с Azure AD, вам потребуется добавить Ungerboeck Software из коллекции в список управляемых приложений SaaS.

1. Войдите на [портал Azure](https://portal.azure.com) с помощью личной учетной записи Майкрософт либо рабочей или учебной учетной записи.
1. В области навигации слева выберите службу **Azure Active Directory**.
1. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.
1. Чтобы добавить новое приложение, выберите **Новое приложение**.
1. В разделе **Добавление из коллекции** в поле поиска введите **Ungerboeck Software**.
1. Выберите **Ungerboeck Software** в области результатов и добавьте это приложение. Подождите несколько секунд, пока приложение не будет добавлено в ваш клиент.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

Настройте и проверьте единый вход Azure AD в Ungerboeck Software с помощью тестового пользователя **B.Simon**. Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Ungerboeck Software.

Чтобы настроить и проверить единый вход Azure AD в Ungerboeck Software, выполните действия в следующих стандартных блоках.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-sso)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Настройка единого входа в Ungerboeck Software](#configure-ungerboeck-software-sso)** необходима, чтобы настроить параметры единого входа на стороне приложения.
3. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя B.Simon.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить B.Simon использовать единый вход Azure AD.
5. **[Создание тестового пользователя Ungerboeck Software](#create-ungerboeck-software-test-user)** нужно для того, чтобы в Ungerboeck Software существовал пользователь B.Simon, связанный с одноименным пользователем в Azure AD.
6. **[Проверка единого входа](#test-sso)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configure-azure-ad-sso"></a>Настройка единого входа Azure AD

Выполните следующие действия, чтобы включить единый вход Azure AD на портале Azure.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **Ungerboeck Software** найдите раздел **Управление** и выберите **Единый вход**.
1. На странице **Выбрать метод единого входа** выберите **SAML**.
1. На странице **Настройка единого входа с помощью SAML** щелкните значок "Изменить" (значок пера), чтобы открыть диалоговое окно **Базовая конфигурация SAML** и изменить параметры.

   ![Изменение базовой конфигурации SAML](common/edit-urls.png)

1. На странице **Базовая конфигурация SAML** введите значения для следующих полей.

    1. В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://<SUBDOMAIN>.ungerboeck.com/prod`.

    1. В текстовом поле **Идентификатор (сущности)** введите URL-адрес в следующем формате:
    
    *  **Для рабочей среды** :

       - `https://<SUBDOMAIN>.ungerboeck.com/prod`
       - `https://<SUBDOMAIN>.ungerboeck.net/prod`
       - `https://<SUBDOMAIN>.ungerboeck.io/prod`

   * **Для тестовой среды** :

     - `https://<SUBDOMAIN>.ungerboeck.com/test`
     - `https://<SUBDOMAIN>.ungerboeck.net/test`
     - `https://<SUBDOMAIN>.ungerboeck.io/test`

   > [!NOTE]
   > Эти значения приведены для примера. Замените их фактическими значениями идентификатора и URL-адреса входа, которые описываются далее в разделе **Настройка единого входа в Ungerboeck Software** этого руководства.

1. В разделе **Сертификат подписи SAML** щелкните кнопку **Правка** , чтобы открыть диалоговое окно **Сертификат подписи SAML**.

    ![Изменение сертификата подписи SAML](common/edit-certificate.png)

1. В разделе **Сертификат подписи SAML** скопируйте значение **Отпечаток** и сохраните его на компьютере.

    ![Копирование значения "Отпечаток"](common/copy-thumbprint.png)

1. Требуемые URL-адреса можно скопировать из раздела **Настройка Ungerboeck Software**.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

    а. URL-адрес входа.

    b. Идентификатор Azure AD

    c. URL-адрес выхода.

### <a name="configure-ungerboeck-software-sso"></a>Настройка единого входа в Ungerboeck Software

Чтобы настроить единый вход на стороне **Ungerboeck Software** , нужно отправить **значение отпечатка** и соответствующие URL-адреса, скопированные на портале Azure, [группе поддержки Ungerboeck Software](mailto:Rhonda.Jannings@ungerboeck.com). Специалисты службы поддержки настроят подключение единого входа SAML на обеих сторонах.

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

В этом разделе описано, как на портале Azure создать тестового пользователя с именем B.Simon.

1. На портале Azure в области слева выберите **Azure Active Directory** , **Пользователи** , а затем — **Все пользователи**.
1. В верхней части экрана выберите **Новый пользователь**.
1. В разделе **Свойства пользователя** выполните следующие действия.
   1. В поле **Имя** введите `B.Simon`.  
   1. В поле **Имя пользователя** введите username@companydomain.extension. Например, `B.Simon@contoso.com`.
   1. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле **Пароль**.
   1. Нажмите кнопку **Создать**.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как включить единый вход Azure для пользователя B.Simon, предоставив этому пользователю доступ к Ungerboeck Software.

1. На портале Azure выберите **Корпоративные приложения** , а затем — **Все приложения**.
1. Из списка приложений выберите **Ungerboeck Software**.
1. На странице "Обзор" приложения найдите раздел **Управление** и выберите **Пользователи и группы**.

   ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

1. Выберите **Добавить пользователя** , а в диалоговом окне **Добавление назначения**  выберите **Пользователи и группы**.

    ![Ссылка "Добавить пользователя"](common/add-assign-user.png)

1. В диалоговом окне **Пользователи и группы** выберите **B.Simon** в списке пользователей, а затем в нижней части экрана нажмите кнопку **Выбрать**.
1. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор роли** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать** , расположенную в нижней части экрана.
1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="create-ungerboeck-software-test-user"></a>Создание тестового пользователя Ungerboeck Software

В этом разделе описано, как создать пользователя B.Simon в Ungerboeck Software. Чтобы добавить пользователей на платформу Ungerboeck Software обратитесь в [службу технической поддержки Ungerboeck Software](mailto:Rhonda.Jannings@ungerboeck.com). Перед использованием единого входа необходимо создать и активировать пользователей.

### <a name="test-sso"></a>Проверка единого входа

Щелкнув плитку "Ungerboeck Software" на Панели доступа, вы автоматически войдете в приложение Ungerboeck Software, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](../user-help/my-apps-portal-end-user-access.md)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](./tutorial-list.md)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

- [Что представляет собой условный доступ в Azure Active Directory?](../conditional-access/overview.md)