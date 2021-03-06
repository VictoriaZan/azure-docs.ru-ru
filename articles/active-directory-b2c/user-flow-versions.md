---
title: Версии потоков пользователей в Azure Active Directory B2C | Документация Майкрософт
description: Дополнительные сведения о версиях потоков пользователей, доступных в Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 07/30/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 67949c31c710d88a05e1e110860fe703caf66d04
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "87481331"
---
# <a name="user-flow-versions-in-azure-active-directory-b2c"></a>Версии потоков пользователей в Azure Active Directory B2C

Потоки пользователей в Azure Active Directory B2C (Azure AD B2C) помогают настроить общие [политики](user-flow-overview.md) , которые полностью описывают возможности идентификации клиентов. Эти возможности взаимодействия включают в себя вход в систему, регистрацию, сброс пароля и изменение профиля. В приведенных ниже таблицах описаны потоки пользователей, доступные в Azure AD B2C.

> [!IMPORTANT]
> Мы изменили способ обращения к версиям потока пользователей. Ранее мы предлагали версию 1 (готовая к выпуску) и версии 1.1 и 2 (предварительная версия). Теперь мы объединили потоки пользователей в две версии:
>
>- **Рекомендуемые** пользовательские потоки — это новые предварительные версии потоков пользователей. Они тщательно тестируются и объединяют все функции устаревших версий **v2** и **v 1.1** . После пересылки новые рекомендованные пользовательские потоки будут поддерживаться и обновляться. После перехода к этим новым рекомендуемым потокам пользователей у вас будет доступ к новым функциям по мере их выпуска.
>- **Стандартные** потоки пользователей, ранее известные как **v1**, являются общедоступными и готовыми к работе рабочими потоками пользователей. Если потоки пользователей являются критически важными и зависят от высокостабильных версий, вы можете продолжать использовать стандартные потоки пользователей, зная, что эти версии не будут поддерживаться и обновляться.
>
>Все пользовательские потоки предварительной версии (версии 1.1 и v2) имеют путь к нерекомендуемому состоянию на **1 августа 2021**. Везде, где это возможно, настоятельно рекомендуется [переключиться на новые **Рекомендуемые** версии](#how-to-switch-to-a-new-recommended-user-flow) как можно скорее, чтобы всегда использовать новейшие функции и обновления. *Эти изменения относятся только к общедоступному облаку Azure. В других средах будет по прежнему использоваться [Управление версиями потока прежних](user-flow-versions-legacy.md)версий.*

## <a name="recommended-user-flows"></a>Рекомендуемые потоки пользователей

Рекомендуемые пользовательские потоки — это предварительные версии, сочетающие новые функции с устаревшими возможностями v2 и V 1.1. В дальнейшем рекомендованные потоки пользователей будут поддерживаться и обновляться.

| Поток пользователя | Описание |
| --------- | ----------- |
| Сброс пароля (Предварительная версия) | Позволяет пользователю выбрать новый пароль после подтверждения адреса электронной почты. Используя этот поток пользователя, можно настроить следующее: <ul><li>[Многофакторная проверка подлинности](custom-policy-multi-factor-authentication.md)</li><li>Параметры маркеров совместимости</li><li>[Фильтрация по возрасту](basic-age-gating.md)</li><li>[требования к сложности пароля](user-flow-password-complexity.md)</li></ul> |
| Изменение профиля (Предварительная версия) | Позволяет пользователю настроить свои атрибуты. Используя этот поток пользователя, можно настроить следующее: <ul><li>[Время существования токена](tokens-overview.md)</li><li>Параметры маркеров совместимости</li><li>Поведение сеанса</li></ul> |
| Вход (Предварительная версия) | Позволяет пользователю входить в свою учетную запись. Используя этот поток пользователя, можно настроить следующее: <ul><li>[Многофакторная проверка подлинности](custom-policy-multi-factor-authentication.md)</li><li>[Время существования токена](tokens-overview.md)</li><li>Параметры маркеров совместимости</li><li>Поведение сеанса</li><li>[Фильтрация по возрасту](basic-age-gating.md)</li><li>Страница входа в систему</li></ul> |
| Регистрация (Предварительная версия) | Позволяет пользователю создать учетную запись. Используя этот поток пользователя, можно настроить следующее: <ul><li>[Многофакторная проверка подлинности](custom-policy-multi-factor-authentication.md)</li><li>[Время существования токена](tokens-overview.md)</li><li>Параметры маркеров совместимости</li><li>Поведение сеанса</li><li>[Фильтрация по возрасту](basic-age-gating.md)</li><li>[Требования к сложности пароля](user-flow-password-complexity.md)</li></ul> |
| Регистрация и вход в систему (Предварительная версия) | Позволяет пользователю создать учетную запись или войти в свою учетную запись. Используя этот поток пользователя, можно настроить следующее: <ul><li>[Многофакторная проверка подлинности](custom-policy-multi-factor-authentication.md)</li><li>[Фильтрация по возрасту](basic-age-gating.md)</li><li>[Требования к сложности пароля](user-flow-password-complexity.md)</li></ul> |

## <a name="standard-user-flows"></a>Потоки обычных пользователей

Стандартные потоки пользователей (ранее назывались v1) являются общедоступными, готовыми к работе в рабочей среде пользователя. Потоки обычных пользователей не будут обновлены вперед.

| Поток пользователя | Описание |
| --------- | ----------- | ----------- |
| Сброс паролей | Позволяет пользователю выбрать новый пароль после подтверждения адреса электронной почты. Используя этот поток пользователя, можно настроить следующее: <ul><li>[Многофакторная проверка подлинности](custom-policy-multi-factor-authentication.md)</li><li>Параметры маркеров совместимости</li><li>[Требования к сложности пароля](user-flow-password-complexity.md)</li></ul> |
| Изменение профиля | Позволяет пользователю настроить свои атрибуты. Используя этот поток пользователя, можно настроить следующее: <ul><li>[Время существования токена](tokens-overview.md)</li><li>Параметры маркеров совместимости</li><li>Поведение сеанса</li></ul> |
| Войти | Позволяет пользователю входить в свою учетную запись. Используя этот поток пользователя, можно настроить следующее: <ul><li>[Многофакторная проверка подлинности](custom-policy-multi-factor-authentication.md)</li><li>[Время существования токена](tokens-overview.md)</li><li>Параметры маркеров совместимости</li><li>Поведение сеанса</li><li>Блокирование входа</li><li>Принудительный сброс пароля</li><li>Возможность оставаться в системе</ul><br>С помощью этого потока пользователя нельзя настроить пользовательский интерфейс. |
| Регистрация | Позволяет пользователю создать учетную запись. Используя этот поток пользователя, можно настроить следующее: <ul><li>[Многофакторная проверка подлинности](custom-policy-multi-factor-authentication.md)</li><li>[Время существования токена](tokens-overview.md)</li><li>Параметры маркеров совместимости</li><li>Поведение сеанса</li><li>[Требования к сложности пароля](user-flow-password-complexity.md)</li></ul> |
| регистрация и вход | Позволяет пользователю создать учетную запись или войти в свою учетную запись. Используя этот поток пользователя, можно настроить следующее: <ul><li>[Многофакторная проверка подлинности](custom-policy-multi-factor-authentication.md)</li><li>[Время существования токена](tokens-overview.md)</li><li>Параметры маркеров совместимости</li><li>Поведение сеанса</li><li>[Требования к сложности пароля](user-flow-password-complexity.md)</li></ul>|


## <a name="how-to-switch-to-a-new-recommended-user-flow"></a>Как переключиться на новый рекомендуемый поток пользователя

Чтобы переключиться с устаревшей версии пользователя на новую **рекомендуемую** предварительную версию, выполните следующие действия.

1. Создайте политику потока пользователей, выполнив действия, описанные в [учебнике Создание пользовательских потоков в Azure Active Directory](tutorial-create-user-flows.md). При создании потока пользователя выберите **рекомендуемую** версию.

3. Настройте новый поток пользователей с теми же параметрами, которые были настроены в политике прежних версий.

4. Обновите URL-адрес входа приложения для вновь созданной политики.

5. После проверки потока пользователя и подтверждения его работоспособности удалите поток предыдущих пользователей, выполнив следующие действия.
   1. В меню "Обзор клиента Azure AD B2C выберите пункт" **потоки пользователей**".
   2. Найдите поток пользователя, который нужно удалить.
   3. В последнем столбце выберите контекстное меню (**...**) и нажмите кнопку **Удалить**.

## <a name="frequently-asked-questions"></a>Часто задаваемые вопросы

### <a name="can-i-still-create-legacy-v2-and-v11-user-flows"></a>Можно ли по-прежнему создавать потоки пользователей прежних версий v2 и V 1.1?

Вы не сможете создавать новые потоки пользователей на основе устаревших версий v2 и V 1.1, но вы можете продолжить чтение, обновление и удаление всех устаревших пользовательских потоков v2 и V 1.1, которые вы используете в настоящее время.

### <a name="is-there-any-reason-to-continue-using-legacy-v2-and-v11-user-flows"></a>Есть ли какая-либо причина продолжить использовать потоки пользователей прежних версий v2 и V 1.1?

Не совсем. Новая **Рекомендуемая** версия содержит те же функциональные возможности, что и устаревшие версии v2 и v 1.1. Ничего не было удалено, и в действительности они включают дополнительные функции.

### <a name="if-i-dont-switch-from-legacy-v2-and-v11-policies-how-will-it-impact-my-application"></a>Если не переключаться с устаревших политик v2 и V 1.1, как это повлияет на мое приложение?

Если вы используете пользовательский поток прежних версий v2 или V 1.1, это изменение управления версиями не повлияет на приложение. Но чтобы получить доступ к новым функциям или изменениям политики, необходимо переключиться на новые **Рекомендуемые** версии.

### <a name="will-microsoft-still-support-my-legacy-v2-or-v11-user-flow-policy"></a>Будет ли корпорация Майкрософт поддерживать свою политику пользовательских потоков версии v2 или V 1.1?

Устаревшие версии версий 2 и V 1.1 для пользовательских потоков будут по прежнему поддерживаться полностью.
