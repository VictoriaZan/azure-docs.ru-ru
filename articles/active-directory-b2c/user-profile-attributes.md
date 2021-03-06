---
title: Атрибуты профиля пользователя в Azure Active Directory B2C
description: Сведения об атрибутах ресурса типа "пользователь", которые поддерживаются профилем пользователя Azure AD B2C Directory. Узнайте о встроенных атрибутах, расширениях, а также о том, как атрибуты сопоставляются с Microsoft Graph.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/07/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 85030285810433dc77d1f466d160c50d1f89770e
ms.sourcegitcommit: ea551dad8d870ddcc0fee4423026f51bf4532e19
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/07/2020
ms.locfileid: "96750413"
---
# <a name="user-profile-attributes"></a>Атрибуты профиля пользователя

Профиль пользователя Azure Active Directory (Azure AD) B2C поставляется со встроенным набором атрибутов, таких как имя, фамилия, город, почтовый индекс и номер телефона. Вы можете расширить профиль пользователя с помощью собственных данных приложения без использования внешнего хранилища данных. Большинство атрибутов, которые могут использоваться с профилями пользователей Azure AD B2C, также поддерживаются Microsoft Graph. В этой статье описаны поддерживаемые атрибуты профиля пользователя Azure AD B2C. Здесь также указаны атрибуты, которые не поддерживаются Microsoft Graph, и атрибуты Microsoft Graph, которые не следует использовать с Azure AD B2C.

> [!IMPORTANT]
> Не следует использовать встроенные или атрибуты расширения для хранения конфиденциальных персональных данных, таких как учетные данные, идентификационные номера государственных организаций, данные владельцев платежных карт, данные финансовых счетов, сведения медицинского характера или конфиденциальные биографические сведения.

Также поддерживается и интеграция с внешними системами. Например, вы можете использовать Azure AD B2C для проверки подлинности, а доверенным источником данных о пользователях назначить внешнюю систему CRM или базу данных лояльности клиентов. Дополнительные сведения см. в решении, посвященном [удаленному профилю](https://github.com/azure-ad-b2c/samples/tree/master/policies/remote-profile).

В таблице ниже представлены атрибуты [ресурса типа "пользователь"](/graph/api/resources/user), которые поддерживаются профилем пользователя Azure AD B2C Directory. В ней приведены сведения о каждом атрибуте.

- Имя атрибута, используемое Azure AD B2C (за которым следует имя Microsoft Graph в круглых скобках, если оно отличается)
- Тип данных атрибута
- Описание атрибута
- Доступность атрибута на портале Azure
- Возможность использования атрибута в потоке пользователей
- Возможность использовать атрибут в настраиваемой политике [технического профиля Azure AD](active-directory-technical-profile.md) и в каком разделе (&lt;InputClaims&gt;, &lt;OutputClaims&gt; или &lt;PersistedClaims&gt;)

|Имя     |Тип     |Описание|Портал Azure|Маршруты пользователей|Настраиваемая политика|
|---------|---------|----------|------------|----------|-------------|
|AccountEnabled  |Логическое|Указывает на то, включена или отключена ли учетная запись пользователя: **true**, если учетная запись включена, в противном случае — **false**.|Да|нет|Persisted, Output|
|ageGroup        |Строка|Возрастная группа пользователя. Возможные значения: null, Undefined, Minor, Adult, NotAdult.|Да|нет|Persisted, Output|
|alternativeSecurityId ([удостоверения](manage-user-accounts-graph-api.md#identities-property))|Строка|Единственное удостоверение пользователя от внешнего поставщика удостоверений.|нет|нет|Input, Persisted, Output|
|alternativeSecurityIds ([удостоверения](manage-user-accounts-graph-api.md#identities-property))|Альтернативная коллекция securityId|Коллекция удостоверений пользователя от внешних поставщиков удостоверений.|нет|нет|Persisted, Output|
|city            |Строка|Город, в котором находится пользователь. Максимальная длина — 128 символов.|Да|Да|Persisted, Output|
|consentProvidedForMinor|Строка|Предоставлено ли согласие для несовершеннолетнего. Допустимые значения: null, granted, denied или notRequired.|Да|нет|Persisted, Output|
|country         |Строка|Страна или регион, в котором находится пользователь. Пример "США" или "Соединенное Королевство". Максимальная длина — 128 символов.|Да|Да|Persisted, Output|
|createdDateTime|Дата и время|Дата создания объекта "пользователь". Только для чтения.|нет|нет|Persisted, Output|
|creationType    |Строка|Если учетная запись пользователя была создана как локальная учетная запись для клиента Azure Active Directory B2C, то используется значение LocalAccount или nameCoexistence. Только для чтения.|нет|нет|Persisted, Output|
|dateOfBirth     |Дата|Дата рождения.|нет|нет|Persisted, Output|
|department      |Строка|Название отдела, в котором работает пользователь. Максимальная длина — 64 символа.|Да|нет|Persisted, Output|
|displayName     |Строка|Отображаемое имя пользователя. Максимальная длина — 256 символов.|Да|Да|Persisted, Output|
|facsimileTelephoneNumber<sup>1</sup>|Строка|Номер телефона факсимильного аппарата пользователя.|Да|нет|Persisted, Output|
|givenName       |Строка|Имя пользователя. Максимальная длина — 64 символа.|Да|Да|Persisted, Output|
|jobTitle        |Строка|Название должности пользователя. Максимальная длина — 128 символов.|Да|Да|Persisted, Output|
|immutableId     |Строка|Идентификатор, который обычно используется для пользователей, перенесенных из локальной службы Active Directory.|нет|нет|Persisted, Output|
|legalAgeGroupClassification|Строка|Юридическая классификация возрастных групп. Только для чтения и вычисляется на основе свойств ageGroup и consentProvidedForMinor. Допустимые значения: null, minorWithOutParentalConsent, minorWithParentalConsent, minorNoParentalConsentRequired, notAdult и adult.|Да|нет|Persisted, Output|
|legalCountry<sup>1</sup>  |Строка|Страна или регион для юридических целей.|нет|нет|Persisted, Output|
|mail            |Строка|SMTP-адрес для пользователя (например, "bob@contoso.com"). Только для чтения.|нет|нет|Persisted, Output|
|mailNickName    |Строка|Псевдоним почты для пользователя. Максимальная длина — 64 символа.|нет|нет|Persisted, Output|
|mobile (mobilePhone) |Строка|Основной номер сотового телефона для пользователя. Максимальная длина — 64 символа.|Да|нет|Persisted, Output|
|netId           |Строка|Идентификатор сети.|нет|нет|Persisted, Output|
|objectId        |Строка|Глобальный уникальный идентификатор (GUID), который является уникальным идентификатором пользователя. Пример 12345678-9abc-def0-1234-56789abcde. Только для чтения, неизменяемый.|Только для чтения|Да|Input, Persisted, Output|
|otherMails      |Коллекция строк|Список дополнительных адресов электронной почты пользователя. Например: ["bob@contoso.com", "Robert@fabrikam.com"].|Да (дополнительный адрес электронной почты)|нет|Persisted, Output|
|password        |Строка|Пароль для локальной учетной записи во время создания пользователя.|нет|нет|Persisted|
|passwordPolicies     |Строка|Политика пароля. Это строка, состоящая из имени другой политики через запятую. Например "DisablePasswordExpiration, DisableStrongPassword".|нет|нет|Persisted, Output|
|physicalDeliveryOfficeName (officeLocation)|Строка|Расположение офиса в компании пользователя. Максимальная длина — 128 символов.|Да|нет|Persisted, Output|
|postalCode      |Строка|Почтовый индекс для почтового адреса пользователя. Почтовый индекс зависит от страны или региона пользователя. В США этот атрибут содержит ZIP-код. Максимальная длина — 40 символов.|Да|нет|Persisted, Output|
|preferredLanguage    |Строка|Предпочитаемый язык для пользователя. Должен соответствовать кодировке ISO 639-1. Например, "en-US".|нет|нет|Persisted, Output|
|refreshTokensValidFromDateTime|Дата и время|Все маркеры обновления, выданные до этого времени, являются недействительными, и приложения будут получать ошибку при использовании недействительного маркера обновления для получения нового маркера доступа. В этом случае приложению потребуется получить новый маркер обновления, выполнив запрос авторизации. Только для чтения.|нет|нет|Выходные данные|
|signInNames ([удостоверения](manage-user-accounts-graph-api.md#identities-property)) |Строка|Уникальное имя для входа пользователя локальной учетной записи любого типа в каталоге. Используйте этот параметр, чтобы получить пользователя со значением входа без указания типа локальной учетной записи.|нет|нет|Входные данные|
|signInNames.userName ([удостоверения](manage-user-accounts-graph-api.md#identities-property)) |Строка|Уникальное имя пользователя локальной учетной записи в каталоге. Используйте этот параметр, чтобы создать или получить пользователя с указанным именем пользователя для входа. Указание этого параметра только лишь в PersistedClaims во время операции исправления приведет к удалению других типов signInNames. Чтобы добавить новый тип signInNames, необходимо также сохранить существующий signInNames.|нет|нет|Input, Persisted, Output|
|signInNames.phoneNumber ([удостоверения](manage-user-accounts-graph-api.md#identities-property)) |Строка|Уникальный номер телефона локальной учетной записи в каталоге. Используйте этот параметр, чтобы создать или получить пользователя с указанным номером телефона для входа. Указание этого параметра только лишь в PersistedClaims во время операции исправления приведет к удалению других типов signInNames. Чтобы добавить новый тип signInNames, необходимо также сохранить существующий signInNames.|нет|нет|Input, Persisted, Output|
|signInNames.emailAddress ([удостоверения](manage-user-accounts-graph-api.md#identities-property))|Строка|Уникальный адрес электронной почты локальной учетной записи в каталоге. Используйте этот параметр, чтобы создать или получить пользователя с указанным адресом электронной почты для входа. Указание этого параметра только лишь в PersistedClaims во время операции исправления приведет к удалению других типов signInNames. Чтобы добавить новый тип signInNames, необходимо также сохранить существующий signInNames.|нет|нет|Input, Persisted, Output|
|state           |Строка|Область или край в адресе пользователя. Максимальная длина — 128 символов.|Да|Да|Persisted, Output|
|streetAddress   |Строка|Почтовый адрес организации пользователя. Максимальная длина — 1024 символа.|Да|Да|Persisted, Output|
|strongAuthentication AlternativePhoneNumber<sup>1</sup>|Строка|Дополнительный номер телефона пользователя для многофакторной проверки подлинности.|Да|нет|Persisted, Output|
|strongAuthenticationEmailAddress<sup>1</sup>|Строка|SMTP-адрес для пользователя. Пример: "bob@contoso.com". Этот атрибут используется для входа с помощью политики имени пользователя для хранения адреса электронной почты пользователя. Адрес электронной почты, который затем используется в потоке сброса пароля.|Да|нет|Persisted, Output|
|Стронгаусентикатионфоненумбер<sup>2</sup>|Строка|Основной номер телефона пользователя для многофакторной проверки подлинности.|Да|нет|Persisted, Output|
|surname         |Строка|Фамилия пользователя. Максимальная длина — 64 символа.|Да|Да|Persisted, Output|
|telephoneNumber (первая запись businessPhones)|Строка|Основной номер телефона организации пользователя.|Да|нет|Persisted, Output|
|userPrincipalName    |Строка|Имя участника-пользователя (UPN) этого пользователя. UPN — это атрибут, который является именем пользователя для входа через Интернет на основе интернет-стандарта RFC 822. Домен должен присутствовать в коллекции подтвержденных доменов клиента. Это свойство является обязательным при создании учетной записи. Неизменяемый.|нет|нет|Input, Persisted, Output|
|usageLocation   |Строка|Требуется для пользователей, которым будут назначены лицензии в соответствии с юридическим требованием для проверки доступности служб в странах и регионах. Не допускает значения NULL. Двухбуквенный код региона или страны (в соответствии со стандартом ISO 3166). Примеры: "US", "JP" и "GB".|Да|нет|Persisted, Output|
|userType        |Строка|Значение строки, которое будет использоваться для классификации типов пользователей в вашем каталоге. Значение должно быть "Member". Только для чтения.|Только для чтения|нет|Persisted, Output|
|userState (Екстерналусерстате)<sup>3</sup>|Строка|Только для учетной записи Azure AD B2B. Указывает, является ли приглашение PendingAcceptance или Accepted.|нет|нет|Persisted, Output|
|userStateChangedOn (externalUserStateChangeDateTime)<sup>2</sup>|Дата и время|Показывает метку времени для последних изменений в свойстве UserState.|нет|нет|Persisted, Output|

<sup>1 </sup>Не поддерживается Microsoft Graph<br><sup>2</sup> Дополнительные сведения см. в статье [атрибут номера телефона MFA](#mfa-phone-number-attribute) .<br><sup>3 </sup> Не следует использовать с Azure AD B2C

## <a name="mfa-phone-number-attribute"></a>Атрибут номера телефона MFA

При использовании телефона для многофакторной проверки подлинности (MFA) для проверки удостоверения пользователя используется мобильный телефон. Чтобы [Добавить](https://docs.microsoft.com/graph/api/authentication-post-phonemethods) новый номер телефона программно, [Обновить](https://docs.microsoft.com/graph/api/b2cauthenticationmethodspolicy-update), [получить](https://docs.microsoft.com/graph/api/b2cauthenticationmethodspolicy-get)или [Удалить](https://docs.microsoft.com/graph/api/phoneauthenticationmethod-delete) номер телефона, используйте [метод проверки подлинности](https://docs.microsoft.com/graph/api/resources/phoneauthenticationmethod)MS API Graph Phone.

В Azure AD B2C [пользовательских политик](custom-policy-overview.md)номер телефона доступен через `strongAuthenticationPhoneNumber` тип утверждения.

## <a name="extension-attributes"></a>Атрибуты расширения

Зачастую приходится создавать собственные атрибуты, как в следующих случаях:

- Клиентскому приложению требуется сохранить атрибут **LoyaltyNumber**.
- Поставщик удостоверений имеет уникальный идентификатор пользователя, который необходимо сохранить, например **uniqueUserGUID**.
- В настраиваемом пути взаимодействия пользователя нужно сохранить состояние, например **migrationStatus**.

Azure AD B2C расширяет набор атрибутов, хранящихся в каждой учетной записи пользователя. Атрибуты расширения [расширяют схему](/graph/extensibility-overview#schema-extensions) объектов пользователей в каталоге. Атрибуты расширения можно зарегистрировать только в объекте приложения, даже если они могут содержать данные для пользователя. Атрибут расширения прикрепляется к приложению с именем b2c-extensions-app. Не изменяйте это приложение, поскольку оно используется Azure AD B2C для хранения данных пользователя. Это приложение находится в разделе регистрации приложений Azure Active Directory.

> [!NOTE]
> - В любую учетную запись пользователя можно записать до 100 атрибутов расширения.
> - При удалении приложения b2c-extensions-app эти атрибуты расширения удаляются для всех пользователей вместе с любыми данными, которые они содержат.
> - Если атрибут расширения удаляется приложением, оно удаляется из всех учетных записей пользователей вместе со значениями.
> - Базовое имя атрибута расширения создается в формате "extension_" + ИД приложения + "_" + "имя атрибута". Например, если создать атрибут расширения LoyaltyNumber, а идентификатор приложения b2c-extensions-app — 831374b3-bd50-41bf-aa54-263ec9e050fc, то базовым именем атрибута расширения будет extension_831374b3bd5041bfaa54263ec9e050fc_LoyaltyNumber. Базовое имя используется при выполнении запросов API Graph для создания или обновления учетных записей пользователей.

При определении свойства в расширении схемы поддерживаются следующие типы данных.

|Тип свойства |Remarks  |
|--------------|---------|
|Логическое    | Возможные значения: **true** или **false**. |
|Дата и время   | Необходимо указать в формате ISO 8601. Будет храниться в формате UTC.   |
|Целое число    | 32-разрядное значение.               |
|Строка     | Не более 256 знаков.     |

## <a name="next-steps"></a>Дальнейшие действия
Узнайте подробнее об атрибутах расширения:
- [Расширения схемы](/graph/extensibility-overview#schema-extensions)
- [Определение настраиваемых атрибутов с помощью потока пользователя](user-flow-custom-attributes.md)
- [Определение настраиваемых атрибутов с помощью настраиваемой политики](custom-policy-custom-attributes.md)