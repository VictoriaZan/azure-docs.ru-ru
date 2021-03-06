---
title: Руководство по интеграции Azure Active Directory с RolePoint | Документация Майкрософт
description: Из этого руководства вы узнаете, как настроить единый вход Azure Active Directory в RolePoint.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 03/15/2019
ms.author: jeedes
ms.openlocfilehash: 3225aa9eaff5c3cd0acca99261935feb9774810f
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/25/2020
ms.locfileid: "96010272"
---
# <a name="tutorial-azure-active-directory-integration-with-rolepoint"></a>Руководство по интеграции Azure Active Directory с RolePoint

В этом руководстве описано, как интегрировать RolePoint с Azure Active Directory (Azure AD).
Такая интеграция обеспечивает следующие преимущества.

* С помощью Azure AD вы можете контролировать доступ к RolePoint.
* Вы можете включить автоматический вход пользователей в RolePoint (единый вход) с помощью учетных записей Azure Active Directory.
* Вы можете управлять учетными записями централизованно — через портал Azure.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см. в статье [Единый вход в приложениях в Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>предварительные требования

Чтобы настроить интеграцию Azure AD с RolePoint, вам потребуется:

* подписка Azure AD; (если у вас нет среды Azure AD, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/));
* подписка RolePoint с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure Active Directory в тестовой среде.

* RolePoint поддерживает единый вход, инициируемый поставщиком услуг.

## <a name="add-rolepoint-from-the-gallery"></a>Добавление RolePoint из коллекции

Чтобы настроить интеграцию RolePoint с Azure AD, необходимо добавить RolePoint из коллекции в список управляемых приложений SaaS.

1. На [портале Azure](https://portal.azure.com) в области слева щелкните **Azure Active Directory**.

    ![Выберите Azure Active Directory.](common/select-azuread.png)

2. Выберите **Корпоративные приложения** > **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, выберите **Новое приложение** в верхней части окна.

    ![Выбор элемента "Новое приложение"](common/add-new-app.png)

4. В поле поиска введите **RolePoint**. В результатах поиска выберите **RolePoint**, а затем щелкните **Добавить**.

     ![Результаты поиска](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в RolePoint с использованием тестового пользователя Britta Simon.
Для единого входа необходимо установить связь между пользователем в Azure AD и соответствующим пользователем в RolePoint.

Чтобы настроить и проверить единый вход Azure AD в RolePoint, потребуется выполнить следующие действия.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Настройка единого входа в RolePoint](#configure-rolepoint-single-sign-on)** на стороне приложения.
3. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить ему использовать единый вход Azure AD.
5. **[Создание тестового пользователя RolePoint](#create-a-rolepoint-test-user)** , связанного с одноименным пользователем в Azure AD.
6. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы убедиться, что конфигурация работает правильно.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано включение единого входа Azure AD на портале Azure.

Чтобы настроить единый вход Azure Active Directory в RolePoint, выполните следующие действия.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением RolePoint выберите **Единый вход**.

    ![Выбор параметра "Единый вход"](common/select-sso.png)

2. В диалоговом окне **Выбрать метод единого входа** выберите режим **SAML/WS-Fed**, чтобы включить единый вход.

    ![Выбор метода единого входа](common/select-saml-option.png)

3. На странице **Настройка единого входа с помощью SAML** щелкните **Изменить**, чтобы открыть диалоговое окно **Базовая конфигурация SAML**.

    ![Значок "Изменить"](common/edit-urls.png)

4. В диалоговом окне **Базовая конфигурация SAML** выполните следующие действия.

    ![Диалоговое окно "Базовая конфигурация SAML"](common/sp-identifier.png)

    1. В поле **URL-адрес для входа** введите URL-адрес в следующем формате:

       `https://<subdomain>.rolepoint.com/login`

    1. В поле **Идентификатор (сущности)** введите URL-адрес в следующем формате:

       `https://app.rolepoint.com/<instancename>`

    > [!NOTE]
    > Эти значения представляют собой заполнители. Необходимо указать фактические URL-адрес для входа и идентификатор. Мы рекомендуем использовать уникальное строковое значение для идентификатора. Чтобы получить эти значения, обратитесь к [группе поддержки RolePoint](mailto:info@rolepoint.com). Можно также ознакомиться с шаблонами в диалоговом окне **Базовая конфигурация SAML** на портале Azure.

5. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** щелкните ссылку **Скачать** рядом с требуемым элементом **XML метаданных федерации** и сохраните файл на компьютере.

    ![Ссылка для скачивания сертификата](common/metadataxml.png)

6. Требуемые URL-адреса можно скопировать из раздела **Настройка RolePoint**.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

    1. **URL-адрес входа**.

    1. **Идентификатор Azure AD**

    1. **URL-адрес выхода**.


### <a name="configure-rolepoint-single-sign-on"></a>Настройка единого входа в RolePoint

Чтобы настроить единый вход на стороне RolePoint, нужно обратиться к [группе поддержки RolePoint](mailto:info@rolepoint.com). Отправьте ей XML-файл метаданных федерации и URL-адреса, полученные на портале Azure. Группа поддержки обеспечит правильную настройку подключения для единого входа SAML с обеих сторон.

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

В этом разделе описано, как создать на портале Azure тестового пользователя с именем Britta Simon.

1. На портале Azure в области слева выберите **Azure Active Directory**, **Пользователи**, а затем — **Все пользователи**.

    ![Выбор "Все пользователи"](common/users.png)

2. В верхней части окна выберите **Новый пользователь**.

    ![Выбор "Новый пользователь"](common/new-user.png)

3. В диалоговом окне **Пользователь** сделайте следующее:

    ![Диалоговое окно "Пользователь"](common/user-properties.png)

    1. В поле **Имя** введите **BrittaSimon**.
  
    1. В поле **Имя пользователя** введите **BrittaSimon@\<yourcompanydomain>.\<extension>** . (Например, BrittaSimon@contoso.com).

    1. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле **Пароль**.

    1. Нажмите кнопку **Создать**.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к RolePoint.

1. На портале Azure выберите **Корпоративные приложения**, **Все приложения**, а затем — **RolePoint**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. Из списка приложений выберите **RolePoint**.

    ![Список приложений](common/all-applications.png)

3. В области слева выберите **Пользователи и группы**.

    ![Выбор параметра "Пользователи и группы"](common/users-groups-blade.png)

4. Выберите **Добавить пользователя**, а в диалоговом окне **Добавление назначения** — **Пользователи и группы**.

    ![Выбор элемента "Добавить пользователя"](common/add-assign-user.png)

5. В диалоговом окне **Пользователи и группы** выберите **Britta Simon** из списка пользователей, а затем в нижней части окна нажмите кнопку **Выбрать**.

6. Если ожидается, что в утверждении SAML будет получено значение роли, то в диалоговом окне **Выбор ролей** нужно выбрать соответствующую роль для пользователя из списка. В нижней части окна нажмите кнопку **Выбрать**.

7. В диалоговом окне **Добавление назначения** выберите **Назначить**.

### <a name="create-a-rolepoint-test-user"></a>Создание тестового пользователя RolePoint

Далее необходимо создать пользователя с именем Britta Simon в RolePoint. Чтобы добавить пользователей в RolePoint, обратитесь в [службу поддержки RolePoint](mailto:info@rolepoint.com). Перед использованием единого входа необходимо создать и активировать пользователей.

### <a name="test-single-sign-on"></a>Проверка единого входа

Теперь необходимо проверить конфигурацию единого входа Azure AD с помощью Панели доступа.

Щелкнув элемент "RolePoint" на Панели доступа, вы автоматически войдете в экземпляр RolePoint, для которого настроили единый вход. Дополнительные сведения о Панели доступа см. в статье [Access and use apps on the My Apps portal](../user-help/my-apps-portal-end-user-access.md) (Доступ к приложениям и их использование на портале "Мои приложения").

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Руководства по интеграции приложений SaaS с Azure Active Directory](./tutorial-list.md)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

- [Что представляет собой условный доступ в Azure Active Directory?](../conditional-access/overview.md)