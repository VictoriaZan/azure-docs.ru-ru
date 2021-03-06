---
title: Шифрование данных с помощью управляемого клиентом ключа — База данных Azure для MySQL
description: Шифрование данных в Базе данных Azure для MySQL с помощью управляемых пользователем ключей позволяет создавать собственные ключи (BYOK) для защиты неактивных данных. Он также позволяет организациям реализовать разделение обязанностей в управлении ключами и данными.
author: mksuni
ms.author: sumuth
ms.service: mysql
ms.topic: conceptual
ms.date: 01/13/2020
ms.openlocfilehash: f9b9681b08f5864dc34bbf1c35dc6919129c24cb
ms.sourcegitcommit: 84e3db454ad2bccf529dabba518558bd28e2a4e6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/02/2020
ms.locfileid: "96518810"
---
# <a name="azure-database-for-mysql-data-encryption-with-a-customer-managed-key"></a>Шифрование данных в Базе данных Azure для MySQL с помощью управляемого клиентом ключа

Шифрование данных с помощью управляемых пользователем ключей для Базы данных Azure для MySQL позволяет создавать собственный ключ (BYOK) для защиты неактивных данных. Он также позволяет организациям реализовать разделение обязанностей в управлении ключами и данными. При шифровании, управляемом пользователем, вы несете ответственность за полное управление жизненным циклом ключа, разрешения на использование ключа и за аудит операций с ключами.

Шифрование данных с помощью управляемых клиентом ключей для Базы данных Azure для MySQL задано на уровне сервера. На определенном сервере шифрование ключа шифрования данных (DEK), используемого службой, выполняется с помощью управляемого клиентом ключа, называемого ключом шифрования ключей (KEK). KEK — это асимметричный ключ, хранящийся в принадлежащем клиенту и управляемом им экземпляре [Azure Key Vault](../key-vault/general/secure-your-key-vault.md). Подробное описание ключа шифрования ключей (KEK) и ключа шифрования данных (DEK) приводится далее в этой статье.

Key Vault — это облачная внешняя система управления ключами. Она обладает высокой доступностью и является масштабируемым надежным хранилищем для криптографических ключей RSA, которое при необходимости поддерживается проверенными аппаратными модулями безопасности (HSM) FIPS 140-2 уровня 2. Эта система запрещает прямой доступ к хранимому ключу, но предоставляет авторизованным сущностям службы шифрования и расшифровки. Key Vault может создать ключ, импортировать его или [передать с локального устройства HSM](../key-vault/keys/hsm-protected-keys.md).

> [!NOTE]
> Эта функция доступна во всех регионах Azure, где База данных Azure для MySQL поддерживает ценовые категории "Общего назначения" и "Оптимизировано для операций в памяти". Другие ограничения см. в разделе об [ограничениях](concepts-data-encryption-mysql.md#limitations) .

## <a name="benefits"></a>Преимущества

Шифрование данных с помощью ключей, управляемых клиентом, для базы данных Azure для MySQL предоставляет следующие преимущества.

* Полный контроль доступа к данным с возможностью удалить ключ и сделать базу данных недоступной 
* Полный контроль жизненного цикла ключей, включая смену ключей в соответствии с корпоративными политиками
* Централизованное управление ключами и их организация в Azure Key Vault
* Возможность разделения обязанностей между специалистами по обеспечению безопасности, администраторами баз данных и системными администраторами


## <a name="terminology-and-description"></a>Терминология и описание

**Ключ шифрования данных (DEK)** : симметричный ключ AES256 для шифрования раздела или блока данных. Шифрование каждого блока данных другим ключом создает дополнительные сложности для выполнения атак в отношении зашифрованных данных. Доступ к DEK требуется поставщику ресурсов или экземпляру приложения, выполняющему шифрование или расшифровку определенного блока. Когда DEK заменяется новым ключом, повторного шифрования этим ключом требуют только данные в его связанном блоке.

**Ключ шифрования ключей (KEK)** : ключ шифрования, используемый для шифрования ключей DEK. Ключ KEK, который всегда остается в Key Vault, позволяет шифровать и контролировать сами ключи DEK. Сущность, у которой есть доступ к ключу KEK, может отличаться от сущности, требующей ключа DEK. Так как KEK требуется для расшифровки ключей DEK, его можно фактически рассматривать как единую точку, с помощью которой можно удалить ключи DEK (непосредственно удалив KEK).

Ключи DEK, зашифрованные с помощью ключей KEK, хранятся отдельно. Расшифровать ключи DEK может только сущность с доступом к ключу KEK. Дополнительные сведения см. в статье [Шифрование неактивных данных](../security/fundamentals/encryption-atrest.md).

## <a name="how-data-encryption-with-a-customer-managed-key-work"></a>Шифрование данных с помощью управляемого клиентом ключа

:::image type="content" source="media/concepts-data-access-and-security-data-encryption/mysqloverview.png" alt-text="Схема, на которой показан обзор подхода &quot;Создание собственных ключей&quot;":::

Чтобы сервер MySQL мог использовать управляемые клиентом и хранящиеся в Key Vault ключи для шифрования ключа DEK, администратор Key Vault предоставляет следующие права доступа.

* **get**. Для получения общедоступной части и свойств ключа в хранилище ключей.
* **wrapKey**. Для шифрования ключа DEK. Зашифрованный DEK хранится в базе данных Azure для MySQL.
* **unwrapKey**. Для расшифровки ключа DEK. Службе "база данных Azure для MySQL" требуются расшифрованные DEK для шифрования и расшифровки данных

Администратор хранилища ключей может также [включить ведение журнала событий аудита Key Vault](../azure-monitor/insights/key-vault-insights-overview.md), чтобы их можно было проверить позже.

Если сервер настроен для использования управляемого клиентом ключа, хранящегося в хранилище ключей, он отправляет ключ DEK в хранилище ключей для шифрования. Key Vault возвращает зашифрованный ключ DEK, который сохраняется в базе данных пользователя. Аналогично при необходимости сервер отправляет защищенный ключ DEK в хранилище ключей для расшифровки. Если включено ведение журнала, аудиторы могут использовать Azure Monitor для просмотра журналов событий аудита Key Vault.

## <a name="requirements-for-configuring-data-encryption-for-azure-database-for-mysql"></a>Требования к настройке шифрования данных для Базы данных Azure для MySQL

Ниже приведены требования к настройке Key Vault.

* Key Vault и База данных Azure для MySQL должны принадлежать одному клиенту Azure Active Directory (Azure AD). Взаимодействие между Key Vault и сервером, размещенными в разных клиентах, не поддерживается. Чтобы переместить Key Vault ресурс, необходимо перенастроить шифрование данных.
* Включите функцию [обратимого удаления](../key-vault/general/soft-delete-overview.md) в хранилище ключей с периодом хранения, равным **90 дней**, для защиты от потери данных при возникновении случайного удаления ключа (или Key Vault). Ресурсы с обратимым удалением по умолчанию хранятся в течение 90 дней, если срок хранения явно не установлен в <= 90 дней. С действиями "Восстановить" и "Удалить" связаны отдельные разрешения в политике доступа хранилища ключей. Функция обратимого удаления отключена по умолчанию, но ее можно включить с помощью PowerShell или Azure CLI (обратите внимание, что ее нельзя включить на портале Azure).
* Включите функцию " [Очистка защиты](../key-vault/general/soft-delete-overview.md#purge-protection) " в хранилище ключей с периодом хранения, равным **90 дней**. Защиту от очистки можно включить только после включения обратимого удаления. Его можно включить с помощью Azure CLI или PowerShell. Если включена защита от удаления, хранилище или объект в удаленном состоянии нельзя очистить до тех пор, пока не будет пройден срок хранения. Обратимо удаленные хранилища и объекты по-прежнему можно восстановить, убедившись, что политика хранения будет соблюдена. 
* Предоставьте отдельной Базе данных Azure для MySQL доступ к хранилищу ключей с разрешениями get, wrapKey и unwrapKey, используя его уникальное управляемое удостоверение. В портал Azure уникальное удостоверение "Service" создается автоматически при включении шифрования данных в MySQL. Подробные пошаговые инструкции по использованию портала Azure см. в статье о [настройке шифрования данных для MySQL](howto-data-encryption-portal.md).

Ниже приведены требования к настройке ключа, управляемого клиентом.

* Управляемый клиентом ключ, используемый для шифрования ключа DEK, может быть только асимметричным 2048-битным ключом RSA.
* Дата активации ключа (если задана) должна быть датой и временем в прошлом. Дата окончания срока действия не задана.
* Ключ должен находиться в состоянии *Включено*.
* Ключ должен иметь [обратимое удаление](../key-vault/general/soft-delete-overview.md) с периодом хранения, равным **90 дням**. Это неявно задает необходимый ключевой атрибут Рековерилевел: "восстанавливаемый". Если для параметра retention задано значение < 90 дней, то Рековерилевел: "Кустомизедрековерабле", который не является обязательным, поэтому убедитесь, что для параметра срок хранения задано значение **90 дней**.
* Для ключа должна быть [включена защита от очистки](../key-vault/general/soft-delete-overview.md#purge-protection).
* Если вы [импортируете существующий ключ](/rest/api/keyvault/ImportKey/ImportKey) в хранилище ключей, убедитесь, что он предоставлен в поддерживаемых форматах файлов ( `.pfx` , `.byok` , `.backup` ).

## <a name="recommendations"></a>Рекомендации

Ниже приведены рекомендации по настройке Key Vault при использовании шифрования данных с помощью ключа, управляемого клиентом.

* Установите блокировку ресурсов в Key Vault, чтобы управлять правами на удаление этого критически важного ресурса и предотвратить случайное или несанкционированное удаление.
* Включите функции аудита и отчетности для всех ключей шифрования. Key Vault предоставляет журналы, которые можно легко передать в любые средства управления информационной безопасностью и событиями безопасности. Например, они уже интегрированы в службу Azure Monitor Log Analytics.
* Убедитесь, что Key Vault и База данных Azure для MySQL находятся в одном регионе, чтобы обеспечить быстрый доступ к операциям упаковки или распаковки ключа DEK.
* Сделайте Azure KeyVault доступным только для **частной конечной точки и ряда определенных сетей** и используйте для защиты ресурсов только *доверенные службы Майкрософт*.

    :::image type="content" source="media/concepts-data-access-and-security-data-encryption/keyvault-trusted-service.png" alt-text="trusted-service-with-AKV":::

Ниже приведены рекомендации по настройке ключа, управляемого клиентом.

* Храните копию управляемого клиентом ключа в надежном месте или передайте ее в службу депонирования.

* Если ключ создается в Key Vault, создайте резервную копию ключа перед его первым использованием. Резервную копию можно восстановить только в Key Vault. Дополнительные сведения о команде резервного копирования см. в статье [Backup-AzKeyVaultKey](/powershell/module/az.keyVault/backup-azkeyVaultkey).

## <a name="inaccessible-customer-managed-key-condition"></a>Условие отсутствия доступа к ключу, управляемому клиентом

Когда шифрование данных в Key Vault настраивается с помощью ключа, управляемого клиентом, то, чтобы сервер оставался в режиме "в сети", ему требуется постоянный доступ к этому ключу. Если сервер теряет доступ к управляемому клиентом ключу в Key Vault, он начинает отклонять все подключения в течение 10 минут. Сервер выдает соответствующее сообщение об ошибке, и его состояние изменяется на *Недоступен*. Ниже приведены некоторые причины перехода сервера в это состояние.

* Если для Базы данных Azure для MySQL с включенным шифрованием данных создается сервер восстановления на определенный момент времени, этот новый сервер будет находиться в состоянии *Недоступен*. Это можно исправить на [портале Azure](howto-data-encryption-portal.md#using-data-encryption-for-restore-or-replica-servers) или с помощью [интерфейса командной строки](howto-data-encryption-cli.md#using-data-encryption-for-restore-or-replica-servers).
* Если для Базы данных Azure для MySQL с включенным шифрованием данных создается реплика чтения, сервер реплики будет находиться в состоянии *Недоступен*. Это можно исправить на [портале Azure](howto-data-encryption-portal.md#using-data-encryption-for-restore-or-replica-servers) или с помощью [интерфейса командной строки](howto-data-encryption-cli.md#using-data-encryption-for-restore-or-replica-servers).
* При удалении KeyVault База данных Azure для MySQL не сможет получать доступ к ключу и перейдет в состояние *Недоступен*. Чтобы перевести сервер в состояние *Доступен*, следует восстановить [Key Vault](../key-vault/general/key-vault-recovery.md) и повторно проверить шифрование данных.
* При удалении ключа из KeyVault База данных Azure для MySQL не сможет получать доступ к ключу и перейдет в состояние *Недоступен*. Чтобы перевести сервер в состояние *Доступен*, следует восстановить [ключ](../key-vault/general/key-vault-recovery.md) и повторно проверить шифрование данных.
* Если срок действия ключа, хранящегося в Azure KeyVault, истечет, ключ станет недействительным, а База данных Azure для MySQL перейдет в состояние *Недоступен*. Чтобы перевести сервер в состояние *Доступен*, продлите срок действия ключа с помощью [интерфейса командной строки](/cli/azure/keyvault/key#az-keyvault-key-set-attributes), а затем повторно проверьте шифрование данных.

### <a name="accidental-key-access-revocation-from-key-vault"></a>Непреднамеренный отзыв доступа к ключу из Key Vault

Может случиться так, что пользователь с достаточными правами доступа к Key Vault случайно отключает доступ сервера к ключу в результате выполнения следующих действий:

* отзыв разрешений get, wrapKey и unwrapKey для хранилища ключей с сервера;
* удаление ключа;
* удаление хранилища ключей;
* изменение правил брандмауэра хранилища ключей;
* удаление управляемого удостоверения сервера в Azure AD.

## <a name="monitor-the-customer-managed-key-in-key-vault"></a>Мониторинг управляемого клиентом ключа в Key Vault

Чтобы отслеживать состояние базы данных и активировать оповещения в случае потери доступа к средству защиты прозрачного шифрования данных, настройте следующие функции и компоненты Azure.

* [Работоспособность ресурсов Azure](../service-health/resource-health-overview.md). Недоступная база данных, которая потеряла доступ к ключу клиента, отображается как недоступная после отклонения первого подключения к базе данных.
* [Журнал действий](../service-health/alerts-activity-log-service-notifications-portal.md). В случае сбоя доступа к ключу клиента в управляемом клиентом Key Vault в журнал действий добавляются записи. Если для таких событий создать правила генерации оповещений, можно восстанавливать доступ максимально быстро.

* [Группы действий](../azure-monitor/platform/action-groups.md). Определите эти группы для отправки уведомлений и оповещений в соответствии с вашими предпочтениями.

## <a name="restore-and-replicate-with-a-customers-managed-key-in-key-vault"></a>Восстановление и репликация с помощью управляемого клиентом ключа в Key Vault

После шифрования Базы данных Azure для MySQL с помощью управляемого ключа клиента, хранящегося в Key Vault, все создаваемые копии сервера также шифруются. Новую копию можно создать с помощью операции локального восстановления или геовосстановления либо с помощью реплик чтения. Копию можно изменить в соответствии с новым управляемым клиентом ключом, используемым для шифрования. При изменении ключа, управляемого клиентом, старые резервные копии сервера начинают использовать последний ключ.

Чтобы избежать проблем при настройке шифрования данных, управляемого клиентом, во время восстановления или создания реплики чтения, важно выполнить следующие действия на исходном и восстановленном серверах/репликах.

* Запустите процесс восстановления или чтения реплики из исходной базы данных Azure для MySQL.
* Оставьте созданный сервер (восстановленный сервер или сервер-реплику) в недоступном состоянии, так как его уникальному удостоверению еще не предоставлены разрешения для Key Vault.
* На восстановленном сервере или сервере-реплике повторно проверьте ключ, управляемый клиентом, в параметрах шифрования данных, чтобы созданному серверу были предоставлены разрешения на упаковку и распаковку ключа, хранящегося в Key Vault.

## <a name="limitations"></a>Ограничения

Для базы данных Azure для MySQL Поддержка шифрования неактивных данных с помощью управляемого ключа клиентов (CMK) имеет несколько ограничений.

* Поддержка этой функции ограничена **общего назначения** и **оптимизированными для памяти** ценовыми категориями.
* Эта функция поддерживается только для регионов и серверов с поддержкой хранилища объемом до 16 ТБ. Список регионов Azure, поддерживающих хранилище вплоть до 16TB, см. в разделе о хранилище документации [здесь](concepts-pricing-tiers.md#storage) .

    > [!NOTE]
    > - Все новые серверы MySQL, созданные в перечисленных выше регионах **, поддерживают шифрование** с помощью ключей клиента. Сервер, восстановленный на момент времени (PITR) или реплика чтения, не будет указывать на то, что в теории они являются "новыми".
    > - Чтобы проверить, поддерживает ли подготовленный сервер 16TB, перейдите в колонку ценовая категория на портале и просмотрите максимальный размер хранилища, поддерживаемый подготовленным сервером. Если вы можете переместить ползунок вверх до 4 ТБ, сервер может не поддерживать шифрование с помощью управляемых клиентом ключей. Тем не менее данные шифруются с помощью управляемых службой ключей в любое время. AskAzureDBforMySQL@service.microsoft.comЕсли у вас возникнут вопросы, свяжитесь с вами.

* Шифрование поддерживается только при использовании криптографического ключа RSA 2048.

## <a name="next-steps"></a>Дальнейшие действия

Узнайте, как настроить шифрование данных с помощью управляемого клиентом ключа для базы данных Azure для MySQL, используя [портал Azure](howto-data-encryption-portal.md) и [Azure CLI](howto-data-encryption-cli.md).
