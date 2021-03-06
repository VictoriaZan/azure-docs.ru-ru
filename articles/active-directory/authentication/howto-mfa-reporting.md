---
title: Сведения о событии входа для многофакторной идентификации Azure AD — Azure Active Directory
description: Узнайте, как просмотреть действия входа для событий многофакторной идентификации Azure AD и сообщений о состоянии.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 05/15/2020
ms.author: justinha
author: justinha
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: e2a02ae7bd89e99dc2eee013394a1f85139c1c00
ms.sourcegitcommit: ad83be10e9e910fd4853965661c5edc7bb7b1f7c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/06/2020
ms.locfileid: "96742782"
---
# <a name="use-the-sign-ins-report-to-review-azure-ad-multi-factor-authentication-events"></a>Использование отчета о событиях входа для просмотра событий многофакторной идентификации Azure AD

Для просмотра и понимания событий многофакторной идентификации Azure AD можно использовать отчет о входах в Azure Active Directory (Azure AD). В этом отчете представлены сведения о проверке подлинности для событий, когда пользователю предлагается выполнить многофакторную проверку подлинности, и о том, использовались ли какие-либо политики условного доступа. Подробные сведения по отчету о действиях входа см. в [обзоре отчетов о действиях входа в Azure AD](../reports-monitoring/concept-sign-ins.md).

В этой статье показано, как просмотреть отчет по действиям входа в Azure AD на портале Azure, а затем — модуль PowerShell MSOnline v1.

## <a name="view-the-azure-ad-sign-ins-report"></a>Просмотр отчета по действиям входа Azure AD

Этот отчет предоставляет информацию об использовании управляемых приложений и действиях входа пользователей, которые включают сведения об использовании многофакторной проверки подлинности (MFA). Эти данные позволяют понять, как работает многофакторная проверка подлинности в вашей организации. Он позволяет ответить на следующие вопросы.

- Выполнена ли многофакторная проверка подлинности при входе?
- Как пользователь выполнил ее?
- Почему пользователю не удалось это сделать?
- Сколько пользователей проходят многофакторную проверку подлинности?
- Скольким пользователям не удалось ее пройти?
- С какими общими проблемами сталкиваются пользователи, проходя многофакторную проверку подлинности?

Чтобы просмотреть отчет о действиях входа в [портал Azure](https://portal.azure.com), выполните следующие действия. Можно также запрашивать данные с помощью [API отчетов](../reports-monitoring/concept-reporting-api.md).

1. Войдите на [портал Azure](https://portal.azure.com), используя учетную запись с правами *глобального администратора*.
1. Найдите и выберите **Azure Active Directory**, а затем — раздел **Пользователи** в меню слева.
1. В разделе *Действия* в меню слева выберите **События входа**.
1. Отобразится список событий входа, включая состояние. Здесь можно выбрать событие для просмотра дополнительных сведений о нем.

    Вкладка сведений о событии *Сведения о проверке подлинности* или *Условный доступ* отображает код состояния или политику, вызвавшую запрос MFA.

    [![Снимок экрана примера отчета о действиях входа Azure Active Directory на портале Azure](media/howto-mfa-reporting/sign-in-report-cropped.png)](media/howto-mfa-reporting/sign-in-report.png#lightbox)

Если доступно, отображается то, что было использовано для проверки подлинности, например текстовое сообщение, уведомление приложения Microsoft Authenticator или телефонный звонок.

Следующие сведения отображаются в окне *Сведения о проверке подлинности* для события входа, которое показывает, был ли запрос MFA удовлетворен или отклонен.

* Если многофакторная проверка подлинности выполнена, в этом столбце можно просмотреть дополнительные сведения о ее выполнении.
   * Завершено в облаке.
   * Истек срок действия из-за политик, настроенных для клиента.
   * Запрос регистрации.
   * Выполнено путем утверждения в токене.
   * Выполнено путем утверждения, предоставленного внешним поставщиком.
   * Выполнено за счет строгой проверки подлинности.
   * Пропущено, так как был выполнен поток входа в брокер Windows.
   * Пропущено из-за пароля приложения.
   * Пропущено из-за расположения.
   * Пропущено из-за зарегистрированного устройства.
   * Пропущено из-за устройства.
   * Успешно завершено.

* Если произошел сбой многофакторной проверки подлинности, в этом столбце будет указана причина сбоя.
   * Выполняется проверка подлинности.
   * Попытка повторного выполнения проверки подлинности.
   * Неверный код указан слишком много раз.
   * Недействительная проверка подлинности.
   * Недействительный код проверки в мобильном приложении.
   * Неправильные настройки.
   * Звонок был направлен на голосовую почту.
   * Недопустимый формат номера телефона.
   * Ошибка службы.
   * Не удается подключиться к телефону пользователя.
   * Не удается отправить уведомление от мобильного приложения на устройстве.
   * Не удается отправить уведомление от мобильного приложения.
   * Пользователь отклонил проверку подлинности.
   * Пользователь не отвечает на уведомление от мобильного приложения.
   * Пользователь не зарегистрировал методы проверки.
   * Пользователь ввел неправильный код.
   * Пользователь ввел неправильный ПИН-код.
   * Пользователь отклонил телефонный звонок без успешного выполнения проверки подлинности.
   * Пользователь заблокирован.
   * Пользователь не ввел код проверки.
   * Не удалось найти пользователя.
   * Код проверки уже использован один раз.

## <a name="powershell-reporting-on-users-registered-for-mfa"></a>Отчеты PowerShell для пользователей, зарегистрированных для MFA

Сначала убедитесь, что у вас установлен модуль PowerShell [MSOnline v1](/powershell/azure/active-directory/overview?view=azureadps-1.0).

Определите пользователей, зарегистрированных для многофакторной проверки подлинности с помощью Powershell: Этот набор команд исключает отключенных пользователей, так как эти учетные записи не могут пройти проверку подлинности в Azure AD:

```powershell
Get-MsolUser -All | Where-Object {$_.StrongAuthenticationMethods -ne $null -and $_.BlockCredential -eq $False} | Select-Object -Property UserPrincipalName
```

Определите пользователей, не зарегистрированных для многофакторной проверки подлинности с помощью Powershell: Этот набор команд исключает отключенных пользователей, так как эти учетные записи не могут пройти проверку подлинности в Azure AD:

```powershell
Get-MsolUser -All | Where-Object {$_.StrongAuthenticationMethods.Count -eq 0 -and $_.BlockCredential -eq $False} | Select-Object -Property UserPrincipalName
```

Выявление зарегистрированных пользователей и методов вывода:

```powershell
Get-MsolUser -All | Select-Object @{N='UserPrincipalName';E={$_.UserPrincipalName}},

@{N='MFA Status';E={if ($_.StrongAuthenticationRequirements.State){$_.StrongAuthenticationRequirements.State} else {"Disabled"}}},

@{N='MFA Methods';E={$_.StrongAuthenticationMethods.methodtype}} | Export-Csv -Path c:\MFA_Report.csv -NoTypeInformation
```

## <a name="downloaded-activity-reports-result-codes"></a>Коды результатов для загруженных отчетов о действиях

Следующая таблица поможет устранить неполадки событий с помощью скачанной версии отчета о действиях из предыдущих шагов портала или команд PowerShell. Эти коды результатов не отображаются непосредственно на портале Azure.

| Результат вызова | Описание | Общее описание |
| --- | --- | --- |
| SUCCESS_WITH_PIN | ПИН-код введен | Пользователь ввел ПИН-код.   Если проверка подлинности успешно пройдена, значит, он ввел правильный ПИН-код.   Если в проверке подлинности отказано, значит, введен неправильный ПИН-код. |
| SUCCESS_NO_PIN | Введен только "#" | Если пользователь находится в режиме ПИН-кода и в проверке подлинности отказано, это значит, что он не ввел ПИН-код, а ввел только знак "#".  Если пользователь находится в стандартном режиме и проверка подлинности успешно пройдена, это значит, что он ввел только "#", что является правильным действием в этом режиме. |
| SUCCESS_WITH_PIN_BUT_TIMEOUT | После ввода не введен знак "#" | Пользователь не отправил цифры DTMF, поскольку не был введен знак "#".   Другие введенные цифры не отправляются до тех пор, пока не будет введен знак "#", означающий завершение ввода. |
|SUCCESS_NO_PIN_BUT_TIMEOUT | Нет ввода на телефоне — превышено время ожидания | Звонок был принят, но ответ получен не был.   Обычно это значит, что сработала голосовая почта. |
| SUCCESS_PIN_EXPIRED | Срок действия ПИН-кода истек, но сам код не изменен | Срок действия ПИН-кода истек, и пользователь получил запрос на изменение ПИН-кода. Однако изменение ПИН-кода завершилось ошибкой. |
| SUCCESS_USED_CACHE | Использовался кэш | Проверка подлинности успешно выполнена без вызова многофакторной проверки подлинности, так как предыдущая успешная проверка подлинности для одного имени пользователя произошла в течение заданного интервала. |
| SUCCESS_BYPASSED_AUTH | Проверка подлинности обойдена | Проверка подлинности успешно пройдена с помощью одноразового обхода проверки, инициированного для пользователя.  Дополнительные сведения об обходе проверки см. в разделе Отчет по журналу обойденных пользователей. |
| SUCCESS_USED_IP_BASED_CACHE | Использован кэш для IP-адреса | Проверка подлинности успешно выполнена без вызова многофакторной проверки подлинности, так как предыдущая успешная проверка подлинности для того же имени пользователя, типа проверки подлинности, имени приложения и IP-адреса произошла в течение заданного интервала. |
| SUCCESS_USED_APP_BASED_CACHE | Использован кэш для приложения | Проверка подлинности успешно выполнена без вызова многофакторной проверки подлинности, так как предыдущая успешная проверка подлинности для того же имени пользователя, типа проверки подлинности и имени приложения произошла в течение заданного интервала. |
| SUCCESS_INVALID_INPUT | На телефоне введены недопустимые данные | С телефона отправлен недопустимый ответ.   Это может быть ответ факсимильного аппарата или модема, либо пользователь ввел знак "*" как часть своего ПИН-кода. |
| SUCCESS_USER_BLOCKED | Пользователь заблокирован | Номер телефона пользователя заблокирован.   Заблокированный номер может инициироваться пользователем во время вызова проверки подлинности или администратором с помощью портала Azure. <br> Примечание.  Заблокированный номер также является побочным результатом предупреждения о мошенничестве. |
| SUCCESS_SMS_AUTHENTICATED | SMS прошло проверку подлинности | Пользователь правильно ответил на двустороннее SMS, используя одноразовый пароль (OTP) или OTP и ПИН-код. |
| SUCCESS_SMS_SENT | SMS отправлено | SMS-сообщение, содержащее одноразовый пароль (OTP), успешно отправлено.   Пользователь введет в приложении этот пароль или пароль + ПИН-код для завершения проверки подлинности. |
| SUCCESS_PHONE_APP_AUTHENTICATED | Мобильное приложение прошло проверку подлинности | Пользователь успешно прошел проверку подлинности с помощью мобильного приложения. |
| SUCCESS_OATH_CODE_PENDING | OATH-код в ожидании | У пользователя был запрошен OATH-код, но ответа не последовало. |
| SUCCESS_OATH_CODE_VERIFIED | OATH-код проверен | В ответ на запрос пользователь ввел допустимый OATH-код. |
| SUCCESS_FALLBACK_OATH_CODE_VERIFIED | Резервный OATH-код проверен | Пользователю отказано в проверке подлинности с помощью его основного метода многофакторной проверки подлинности, а затем предоставлен допустимый OATH-код для резервного действия. |
| SUCCESS_FALLBACK_SECURITY_QUESTIONS_ANSWERED | Получены ответы на резервные секретные вопросы | Пользователю было отказано в проверке подлинности с помощью его основного метода многофакторной проверки подлинности, а затем были получены правильные ответы на контрольные вопросы резервного действия. |
| FAILED_PHONE_BUSY | Проверка подлинности уже выполняется | Многофакторная проверка подлинности уже обрабатывает проверку подлинности для этого пользователя.   Как правило, это связано с RADIUS-клиентами, которые во время одного входа отправляют несколько запросов на проверку подлинности. |
| CONFIG_ISSUE | Телефон недоступен | Предпринята попытка вызова, но либо не удалось выполнить вызов, либо не получен ответ.   Причиной может быть наличие сигналов занятой линии, сигналов отключенной линии, сигналов необслуживаемого номера, истечение времени звонка и т. д. |
| FAILED_INVALID_PHONENUMBER | Недопустимый формат номера телефона | Номер телефона имеет недопустимый формат.   Номера телефонов должны быть числовыми и состоять из 10 цифр для кода страны +1 (США и Канада). |
| FAILED_USER_HUNGUP_ON_US | Пользователь повесил трубку | Пользователь взял трубку, но затем положил ее, не нажав ни на одну кнопку. |
| FAILED_INVALID_EXTENSION | Недопустимый добавочный номер | Добавочный номер содержит недопустимые символы.   Можно вводить только цифры, запятые, знаки "*" и "#".   Допускается использование префикса @. |
| FAILED_FRAUD_CODE_ENTERED | Код защиты от мошенничества введен | Пользователь решил сообщить о мошенничестве во время вызова, что привело к отказу в проверке подлинности и блокировке телефонного номера.| 
| FAILED_SERVER_ERROR | Не удалось сделать вызов | Службе многофакторной проверки подлинности не удалось сделать вызов. |
| FAILED_SMS_NOT_SENT | Не удалось отправить SMS | Не удалось отправить SMS.   В проверке подлинности отказано. |
| FAILED_SMS_OTP_INCORRECT | SMS: неверный OTP | Пользователь указал одноразовый пароль (OTP), не соответствующий паролю из полученного текстового сообщения.   В проверке подлинности отказано. |
| FAILED_SMS_OTP_PIN_INCORRECT | SMS: неверный OTP + ПИН-код | Пользователь указал неверный одноразовый пароль (OTP) и (или) неверный ПИН-код.   В проверке подлинности отказано. |
| FAILED_SMS_MAX_OTP_RETRY_REACHED | Превышено допустимое число попыток ввода одноразового пароля | Пользователь превысил максимальное число попыток одноразового пароля (OTP). |
| FAILED_PHONE_APP_DENIED | Мобильное приложение отклонено | Пользователь отклонил проверку подлинности в мобильном приложении, нажав кнопку "Отклонить". |
| FAILED_PHONE_APP_INVALID_PIN | Мобильное приложение: недопустимый ПИН-код | При прохождении проверки подлинности в мобильном приложении пользователь ввел недопустимый ПИН-код. |
| FAILED_PHONE_APP_PIN_NOT_CHANGED | Мобильное приложение: ПИН-код не изменен | Пользователю не удалось выполнить необходимое изменение ПИН-кода в мобильном приложении. |
| FAILED_FRAUD_REPORTED | Получено сообщение о мошенничестве | Пользователь сообщил о мошенничестве в мобильном приложении. |
| FAILED_PHONE_APP_NO_RESPONSE | Мобильное приложение: нет ответа | Пользователь не ответил на запрос проверки подлинности в мобильном приложении. |
| FAILED_PHONE_APP_ALL_DEVICES_BLOCKED | Мобильное приложение: все устройства заблокированы | Устройства с мобильным приложением этого пользователя больше не отвечают на уведомления и были заблокированы. |
| FAILED_PHONE_APP_NOTIFICATION_FAILED | Мобильное приложение: не удалось отправить уведомление | Произошла ошибка при попытке отправить уведомление мобильному приложению на устройстве пользователя. |
| FAILED_PHONE_APP_INVALID_RESULT | Мобильное приложение: недопустимый результат | Мобильное приложение вернуло недопустимый результат. |
| FAILED_OATH_CODE_INCORRECT | Неправильный OATH-код | Пользователь ввел неправильный OATH-код.  В проверке подлинности отказано. |
| FAILED_OATH_CODE_PIN_INCORRECT | OATH-код + ПИН-код неверны | Пользователь указал неверный OATH-код и (или) неверный ПИН-код.  В проверке подлинности отказано. |
| FAILED_OATH_CODE_DUPLICATE | Дубликат OATH-кода | Пользователь ввел OATH-код, который использовался ранее.  В проверке подлинности отказано. |
| FAILED_OATH_CODE_OLD | Устаревший OATH-код | Пользователь ввел OATH-код, предшествующий ранее использованному OATH-коду.  В проверке подлинности отказано. |
| FAILED_OATH_TOKEN_TIMEOUT | Превышено время ожидания результата OATH-кода | Пользователь потратил слишком много времени на ввод OATH-кода, и время попытки многофакторной проверки подлинности истекло. |
| FAILED_SECURITY_QUESTIONS_TIMEOUT | Превышено время ожидания результатов ответов на контрольные вопросы | Пользователь потратил слишком много времени на ввод ответов на контрольные вопросы, и время попытки многофакторной проверки подлинности истекло. |
| FAILED_AUTH_RESULT_TIMEOUT | Время ожидания результатов проверки подлинности | Выполнение многофакторной проверки подлинности пользователем заняло слишком много времени. |
| FAILED_AUTHENTICATION_THROTTLED | Регулирование проверки подлинности | Попытка многофакторной проверки подлинности была отрегулирована службой. |

## <a name="additional-mfa-reports"></a>Дополнительные отчеты Многофакторной проверки подлинности (MFA)

Следующие дополнительные сведения и отчеты доступны для событий MFA, в том числе событий сервера MFA:

| Report | Расположение | Описание |
|:--- |:--- |:--- |
| Журнал заблокированных пользователей | Azure AD > Безопасность > MFA > Блокировка и разблокировка пользователей | История запросов на блокировку или разблокирование пользователей. |
| Использование локальных компонентов | Отчет о действиях > Azure AD > Безопасность > MFA | Сведения об общем использовании сервера MFA через расширение NPS, ADFS и сервер MFA. |
| Журнал обойденных пользователей | Azure AD > Безопасность > MFA > Одноразовый обход проверки | Предоставляет историю запросов сервера MFA, чтобы обойти MFA для пользователя. |
| Состояние сервера | Azure AD > Безопасность > MFA > Состояние сервера | Сведения о состоянии серверов Многофакторной проверки подлинности (MFA), с которыми связана ваша учетная запись. |

## <a name="next-steps"></a>Дальнейшие действия

В этой статье представлен обзор отчета о действиях входа. Дополнительные сведения о том, что этот отчет содержит и как понять его данные, см. в разделе [отчеты о действиях входа в Azure AD](../reports-monitoring/concept-sign-ins.md).
