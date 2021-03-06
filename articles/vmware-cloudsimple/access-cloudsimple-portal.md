---
title: Доступ к решению Azure VMware с помощью Клаудсимпле — портал
description: Сведения о том, как получить доступ к решению VMware с помощью портала CloudSimple на портале Azure
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/04/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 0ea178655646f7f130476acaffc35c60181968ea
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "87058704"
---
# <a name="access-the-vmware-solution-by-cloudsimple-portal-from-the-azure-portal"></a>Доступ к решению VMware с помощью портала Клаудсимпле из портал Azure

Единый вход поддерживается для доступа к порталу Клаудсимпле. После входа в портал Azure вы можете получить доступ к порталу Клаудсимпле, не войдя в систему повторно. При первом обращении к порталу Клаудсимпле вам будет предложено авторизовать приложение [авторизации службы клаудсимпле](#consent-to-cloudsimple-service-authorization-application) .  Авторизация является однократным действием.

## <a name="before-you-begin"></a>Перед началом

Пользователи со встроенными ролями **владельца** и **участника** могут получить доступ к порталу клаудсимпле.  Роли должны быть настроены в группе ресурсов, в которой развернута служба Клаудсимпле.  Роли также можно настроить в объекте службы Клаудсимпле.  Дополнительные сведения о проверке роли см. в статье [Просмотр назначений ролей](../role-based-access-control/check-access.md) . Только пользователи со встроенными ролями **владельца** и **участника** могут получить доступ к порталу клаудсимпле.  Роли должны быть настроены в подписке.  Дополнительные сведения о проверке роли см. в статье [Просмотр назначений ролей](../role-based-access-control/check-access.md) .

При использовании настраиваемых ролей роль должна иметь одну из следующих операций в ```Actions``` .  Дополнительные сведения о пользовательских ролях см. в статье [пользовательские роли Azure](../role-based-access-control/custom-roles.md).  Если какая-либо из операций является частью ```NotActions``` , пользователь не может получить доступ к порталу клаудсимпле.

```
Microsoft.VMwareCloudSimple/*
Microsoft.VMwareCloudSimple/*/write
Microsoft.VMwareCloudSimple/dedicatedCloudServices/*
Microsoft.VMwareCloudSimple/dedicatedCloudServices/*/write
```

## <a name="sign-in-to-azure"></a>Вход в Azure

Войдите на портал Azure по адресу [https://portal.azure.com](https://portal.azure.com).

## <a name="access-the-cloudsimple-portal"></a>Доступ к порталу CloudSimple

1. Выберите элемент **Все службы**.

2. Выполните поиск по запросу **Клаудсимпле Services**.

3. Выберите службу Клаудсимпле, для которой нужно создать частное облако.

4. На странице **Обзор** нажмите кнопку **Переход на портал клаудсимпле**.  Если вы получаете доступ к порталу Клаудсимпле с портал Azure в первый раз, вам будет предложено авторизовать приложение [авторизации службы клаудсимпле](#consent-to-cloudsimple-service-authorization-application) . 

    ![Запуск портала Клаудсимпле](media/launch-cloudsimple-portal.png)

> [!NOTE]
> Если выбрать операцию частного облака (например, создать или развернуть частное облако) непосредственно из портал Azure, портал Клаудсимпле откроется на указанной странице.

На портале Клаудсимпле выберите **Главная** в боковом меню, чтобы отобразить сводную информацию о частных облаках. Здесь показаны ресурсы и емкость частных облаков, а также предупреждения и задачи, требующие внимания. Для общих задач щелкните именованные значки в верхней части страницы.

![Домашняя страница](media/cloudsimple-portal-home.png)

## <a name="consent-to-cloudsimple-service-authorization-application"></a>Согласие на приложение авторизации службы Клаудсимпле

Запуск портала Клаудсимпле из портал Azure в первый раз требует вашего согласия для приложения авторизации службы Клаудсимпле.  Выберите **принять** , чтобы предоставить запрошенные разрешения и получить доступ к порталу клаудсимпле.

![Согласие на авторизацию службы Клаудсимпле. Администраторы](media/cloudsimple-azure-consent.png)

Если у вас есть права глобального администратора, вы можете предоставить согласие для вашей организации.  Выберите **Согласие от имени вашей организации**.

![Согласие на авторизацию службы Клаудсимпле — глобальный администратор](media/cloudsimple-azure-consent-global-admin.png)

Если ваши разрешения не допускают доступ к порталу Клаудсимпле, обратитесь к глобальному администратору своего клиента, чтобы предоставить необходимые разрешения.  Глобальный администратор может согласиться от имени вашей организации.

![Согласие на авторизацию службы Клаудсимпле — требуются администраторы](media/cloudsimple-azure-consent-requires-administrator.png)

## <a name="next-steps"></a>Дальнейшие действия

* Узнайте, как [создать частное облако](./create-private-cloud.md)
* Узнайте, как [настроить среду частного облака](quickstart-create-private-cloud.md)
