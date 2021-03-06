---
title: Сведения о ключах (Azure Key Vault)
description: В этой статье приведены общие сведения об интерфейсе Azure Key Vault REST и подробные сведения о ключах.
services: key-vault
author: amitbapat
manager: msmbaldwin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: keys
ms.topic: overview
ms.date: 09/15/2020
ms.author: ambapat
ms.openlocfilehash: 2ae7b28d5e9e7a520ee8cbd090b6681d5ad7015a
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/06/2020
ms.locfileid: "93422762"
---
# <a name="about-keys"></a>Сведения о ключах

Azure Key Vault предоставляет два типа ресурсов для хранения криптографических ключей и управления ими.

|Тип ресурса|Методы защиты ключа|Базовый URL-адрес конечной точки в плоскости данных|
|--|--|--|
| **Хранилища** | Защищенные программным обеспечением<br/><br/>и<br/><br/>Защищенные модулем HSM (с ценовой категорией Premium)</li></ul> | https://{имя_хранилища}.vault.azure.net |
| **Пулы управляемых устройств HSM** | Защищенные модулем HSM | https://{имя_hsm}.managedhsm.azure.net |
||||

- **Хранилища** — это недорогие, простые в развертывании, многоклиентские, устойчивые к отказу зоны (если доступно) и высокодоступные решения для управления ключами, которые отлично подходят для наиболее распространенных сценариев облачных приложений.
- **Управляемые устройства HSM** — это однотенантные устройства HSM с устойчивостью к отказу зоны (если доступно) и высоким уровнем доступности для хранения криптографических ключей и управления ими. Они лучше всего подходят для приложений и сценариев использования с особо ценными ключами. Также они помогают соблюдать наиболее строгие стандарты безопасности, условия соответствия и нормативные требования. 

> [!NOTE]
> Хранилища также позволяют хранить и администрировать не только криптографические ключи, но и объекты других типов, например секреты, сертификаты и ключи учетной записи хранения.

Ключи шифрования в Key Vault представлены объектами веб-ключей JSON (JWK). Спецификации JSON и JOSE:

-   [Веб-ключ JSON (JWK)](https://tools.ietf.org/html/draft-ietf-jose-json-web-key)  
-   [Веб-шифрование JSON (JWE)](http://tools.ietf.org/html/draft-ietf-jose-json-web-encryption)  
-   [Веб-алгоритмы JSON (JWA)](http://tools.ietf.org/html/draft-ietf-jose-json-web-algorithms)  
-   [Веб-подпись JSON (JWS)](https://tools.ietf.org/html/draft-ietf-jose-json-web-signature) 

Также расширены базовые спецификации JWK и JWA для поддержки типов ключей, специфичных для реализаций Azure Key Vault и "Управляемые устройства HSM". 

Ключи, защищенные модулем HSM (или просто HSM-ключи), обрабатываются внутри HSM (аппаратный модуль защиты) и никогда не покидают границу защиты этого модуля. 

- В хранилищах используются HSM, для которых подтверждено соответствие **FIPS 140-2 уровня 2**, для защиты HSM-ключей в общей внутренней инфраструктуре HSM. 
- Для защиты ключей управляемые устройства HSM используют модули HSM, для которых подтверждено соответствие **FIPS 140-2 уровня 3**. Каждый пул HSM является изолированным однотенантным экземпляром с собственным [доменом безопасности](../managed-hsm/security-domain.md), что обеспечивает полную изоляцию криптографии от всех других пулов HSM, использующих ту же аппаратную инфраструктуру.

Эти ключи защищены в однотенантных пулах HSM. Вы можете импортировать RSA, EC и симметричный ключ программными средствами или путем экспорта из поддерживаемого устройства HSM. Вы также можете создавать ключи в пулах HSM. При импорте HSM-ключей с использованием метода, который описан в [спецификации BYOK (создание собственных ключей)](../keys/byok-specification.md), вы можете безопасно перенести материал ключа в пулы управляемых устройств HSM. 

Дополнительные сведения о географических ограничениях см. в [центре управления безопасностью Microsoft Azure](https://azure.microsoft.com/support/trust-center/privacy/).

## <a name="key-types-and-protection-methods"></a>Типы ключей и методы защиты

Key Vault поддерживает RSA, EC и симметричные ключи. 

### <a name="hsm-protected-keys"></a>Ключи, защищенные модулем HSM

|Тип ключа|Хранилища (только ценовой категории "Премиум")|Пулы управляемых устройств HSM|
|--|--|--|--|
**EC-HSM**. Ключ на основе эллиптической кривой|HSM FIPS 140-2 уровня 2|HSM FIPS 140-2 уровня 3
**RSA-HSM**. Ключ RSA|HSM FIPS 140-2 уровня 2|HSM FIPS 140-2 уровня 3
**oct-HSM**: Симметричный|Не поддерживается|HSM FIPS 140-2 уровня 3
||||

### <a name="software-protected-keys"></a>Ключи, защищенные программным обеспечением

|Тип ключа|Хранилища|Пулы управляемых устройств HSM|
|--|--|--|--|
**RSA**. Ключ RSA, защищенный программным обеспечением.|FIPS 140-2 уровня 1|Не поддерживается
**EC**. Ключ на основе эллиптической кривой, защищенный программным обеспечением|FIPS 140-2 уровня 1|Не поддерживается
||||

Подробные сведения о типах ключей, алгоритмах, операциях, атрибутах и тегах см. в статье [Типы ключей, алгоритмы и операции](about-keys-details.md).

## <a name="next-steps"></a>Дальнейшие действия
- [Сведения о Key Vault](../general/overview.md)
- [Сведения о службе "Управляемое устройство HSM"](../managed-hsm/overview.md)
- [О секретах](../secrets/about-secrets.md)
- [Сведения о сертификатах](../certificates/about-certificates.md)
- [Обзор REST API Key Vault](../general/about-keys-secrets-certificates.md)
- [Аутентификация, запросы и ответы](../general/authentication-requests-and-responses.md)
- [Руководство разработчика Azure Key Vault](../general/developers-guide.md)