---
title: Настройка параметров сервера — портал Azure — "база данных Azure для PostgreSQL" — "один сервер"
description: В этой статье описывается, как настроить параметры postgres в базе данных Azure для PostgreSQL с помощью портал Azure.
author: lfittl-msft
ms.author: lufittl
ms.service: postgresql
ms.topic: how-to
ms.date: 02/28/2018
ms.openlocfilehash: e1b40e3116d56e87a2f397350ef2ba5510e04c0c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91707699"
---
# <a name="configure-server-parameters-in-azure-database-for-postgresql---single-server-via-the-azure-portal"></a>Настройка параметров сервера в базе данных Azure для PostgreSQL — Single Server с помощью портал Azure 
С помощью портала Azure можно вывести список параметров конфигурации для сервера базы данных Azure для PostgreSQL, а также отобразить и обновить их.

## <a name="prerequisites"></a>Предварительные требования
Чтобы приступить к выполнению этого руководства, вам потребуются следующие компоненты:
- [Сервер базы данных Azure для PostgreSQL](quickstart-create-server-database-portal.md)

## <a name="viewing-and-editing-parameters"></a>Просмотр и изменение параметров
1. Откройте [портал Azure](https://portal.azure.com).

2. Выберите сервер базы данных Azure для PostgreSQL.

3. В разделе **Параметры** выберите **Параметры сервера**. На странице отобразится список параметров, их значения и описания.
:::image type="content" source="./media/howto-configure-server-parameters-in-portal/3-overview-of-parameters.png" alt-text="Страница общих сведений для параметров":::

4. Нажмите кнопку **раскрывающегося списка**, чтобы просмотреть возможные значения параметров перечисляемого типа, например client_min_messages.
:::image type="content" source="./media/howto-configure-server-parameters-in-portal/4-enum-drop-down.png" alt-text="Страница общих сведений для параметров":::

5. Выберите кнопку с буквой **i** (информация) или наведите на нее указатель мыши, чтобы просмотреть диапазон возможных значений числовых параметров, например cpu_index_tuple_cost.
:::image type="content" source="./media/howto-configure-server-parameters-in-portal/4-information-button.png" alt-text="Страница общих сведений для параметров":::

6. При необходимости воспользуйтесь **полем поиска**, чтобы сузить область поиска до определенного параметра. Поиск выполняется по имени и описанию параметров.
:::image type="content" source="./media/howto-configure-server-parameters-in-portal/5-search.png" alt-text="Страница общих сведений для параметров":::

7. Измените значения требуемых параметров. Все изменения, вносимые в этом сеансе, выделены фиолетовым цветом. Изменив значения, можно нажать кнопку **Сохранить**. Также вы можете нажать кнопку **Отменить**, чтобы отменить изменения.
:::image type="content" source="./media/howto-configure-server-parameters-in-portal/6-save-and-discard-buttons.png" alt-text="Страница общих сведений для параметров":::

8. Если вы сохранили новые значения параметров, всегда можно восстановить значения по умолчанию, выбрав **Сбросить все к значениям по умолчанию**.
:::image type="content" source="./media/howto-configure-server-parameters-in-portal/7-reset-to-default-button.png" alt-text="Страница общих сведений для параметров":::

## <a name="next-steps"></a>Дальнейшие шаги
Кроме того, вы узнаете о таких возможностях, как:
- [Серверы базы данных Azure для PostgreSQL](concepts-servers.md)
- [Настройка параметров конфигурации сервера с помощью Azure CLI](howto-configure-server-parameters-using-cli.md)
