---
title: 'Настройка BGP для VPN-шлюза: портал'
titleSuffix: Azure VPN Gateway
description: В этой статье описано, как настроить BGP с помощью Azure VPN Gateway с использованием PowerShell.
services: vpn-gateway
author: yushwang
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 09/18/2020
ms.author: yushwang
ms.openlocfilehash: f52d684d1e6ef63fdf4287c610608061f30395f8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "90996872"
---
# <a name="how-to-configure-bgp-on-azure-vpn-gateways"></a>Настройка BGP на VPN-шлюзах Azure

В этой статье рассматриваются действия по включению протокола BGP для VPN-подключения типа "сеть — сеть" (S2S) и подключения между виртуальными сетями с помощью портал Azure.

## <a name="about-bgp"></a><a name="about"></a>О протоколе BGP

Стандартный протокол маршрутизации BGP обычно используется в Интернете для обмена данными о маршрутизации и доступности между двумя или несколькими сетями. BGP позволяет VPN-шлюзам Azure и локальным VPN-устройствам, называемым одноранговыми узлами BGP или соседями, обмениваться маршрутами, которые будут сообщать о доступности и достижимости этих префиксов через шлюзы или маршрутизаторы. Также BGP позволяет передавать трафик транзитом через несколько сетей. Для этого шлюз BGP распространяет на все известные ему узлы BGP информацию о маршрутах, полученную от остальных узлов BGP.

Дополнительные сведения о преимуществах BGP и о технических требованиях и вопросах использования BGP см. в статье [Общие сведения о BGP с VPN-шлюзами Azure](vpn-gateway-bgp-overview.md).

## <a name="getting-started"></a>Начало работы

Каждая часть этой статьи поможет вам сформировать базовый Стандартный блок для включения BGP в сетевом подключении. Если вы завершите все три части, создайте топологию, как показано на схеме 1.

**Схема 1**

:::image type="content" source="./media/bgp-howto/bgp-crosspremises-v2v.png" alt-text="Схема, демонстрирующая сетевую архитектуру и параметры" border="false":::

Вы можете объединять блоки для создания более сложных, многоскачковых и транзитных сетей, в соответствии со своими задачами.

### <a name="prerequisites"></a>Предварительные требования

Убедитесь в том, что у вас уже есть подписка Azure. Если у вас еще нет подписки Azure, вы можете активировать преимущества для [подписчиков MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) или зарегистрироваться для использования [бесплатной учетной записи](https://azure.microsoft.com/pricing/free-trial/).

## <a name="part-1-configure-bgp-on-the-virtual-network-gateway"></a><a name ="config"></a>Часть 1. Настройка BGP в шлюзе виртуальной сети

В этом разделе вы создадите и настроите виртуальную сеть, создадите и настроите шлюз виртуальной сети с параметрами BGP и получите одноранговый IP-адрес Azure BGP. На схеме 2 показаны параметры конфигурации, используемые при работе с шагами, описанными в этом разделе.

**Схема 2**

:::image type="content" source="./media/bgp-howto/bgp-gateway.png" alt-text="Схема, демонстрирующая сетевую архитектуру и параметры" border="false":::

### <a name="1-create-and-configure-testvnet1"></a>1. Создание и настройка TestVNet1

На этом шаге вы создадите и настроите TestVNet1. Выполните действия, описанные в [руководстве по созданию шлюза](vpn-gateway-tutorial-create-gateway-powershell.md) , чтобы создать и настроить виртуальную сеть Azure и VPN-шлюз. Используйте параметры ссылок на снимках экрана ниже.

* Виртуальная сеть

   :::image type="content" source="./media/bgp-howto/testvnet-1.png" alt-text="Схема, демонстрирующая сетевую архитектуру и параметры":::

* Подсети:

   :::image type="content" source="./media/bgp-howto/testvnet-1-subnets.png" alt-text="Схема, демонстрирующая сетевую архитектуру и параметры":::

### <a name="2-create-the-vpn-gateway-for-testvnet1-with-bgp-parameters"></a>2. Создание VPN-шлюза для TestVNet1 с параметрами BGP

На этом шаге вы создадите VPN-шлюз с соответствующими параметрами BGP.

1. В портал Azure перейдите к ресурсу **шлюза виртуальной сети** из Marketplace и выберите **создать**.

1. Заполните параметры, как показано ниже:

   :::image type="content" source="./media/bgp-howto/create-gateway-1.png" alt-text="Схема, демонстрирующая сетевую архитектуру и параметры":::

1. В выделенном разделе **Настройка BGP** настройте следующие параметры.

   :::image type="content" source="./media/bgp-howto/create-gateway-1-bgp.png" alt-text="Схема, демонстрирующая сетевую архитектуру и параметры" в разделе BGP будет показан дополнительный **второй настраиваемый IP-адрес BGP для Azure APIPA**. Укажите другой адрес из разрешенного APIPA-диапазона (**169.254.21.0** to **169.254.22.255**).

   > [!IMPORTANT]
   >
   > * По умолчанию Azure автоматически назначает частный IP-адрес из диапазона префиксов GatewaySubnet в качестве IP-адреса Azure BGP на VPN-шлюзе Azure. Настраиваемый IP-адрес BGP для Azure требуется, если на локальных VPN-устройствах используется адрес APIPA (169.254.0.1 – 169.254.255.254) в качестве протокола BGP. VPN-шлюз Azure выберет пользовательский APIPA-адрес, если соответствующий ресурс шлюза локальной сети (локальная сеть) имеет адрес APIPA в качестве IP узла BGP. Если шлюз локальной сети использует обычный IP-адрес (не APIPA), VPN-шлюз Azure вернется к частному IP-адресу из диапазона GatewaySubnet.
   >
   > * Адреса BGP APIPA не должны пересекаться между локальными VPN-устройствами и всеми подключенными VPN-шлюзами Azure.
   >

1. Выберите **проверить и создать** , чтобы запустить проверку. После завершения проверки выберите **создать** , чтобы развернуть VPN-шлюз. Для полного создания и развертывания шлюза может потребоваться до 45 минут. Состояние развертывания можно просмотреть на странице Обзор шлюза.

### <a name="3-obtain-the-azure-bgp-peer-ip-addresses"></a>3. получение IP-адресов однорангового узла BGP Azure

После создания шлюза можно получить одноранговые IP-адреса BGP на VPN-шлюзе Azure. Эти адреса необходимы для настройки локальных VPN-устройств для установки сеансов BGP с VPN-шлюзом Azure.

1. Перейдите к ресурсу шлюза виртуальной сети и выберите страницу **Конфигурация** , чтобы просмотреть сведения о конфигурации BGP, как показано на следующем снимке экрана. На этой странице можно просмотреть все сведения о конфигурации BGP на VPN-шлюзе Azure: ASN, общедоступный IP-адрес и соответствующие IP-адреса однорангового узла BGP на стороне Azure (по умолчанию и APIPA).

   :::image type="content" source="./media/bgp-howto/vnet-1-gw-bgp.png" alt-text="Схема, демонстрирующая сетевую архитектуру и параметры":::

1. На странице **Конфигурация** можно внести следующие изменения в конфигурацию:

   * При необходимости можно обновить IP-адрес ASN или APIPA BGP.
   * Если у вас есть активный VPN-шлюз, на этой странице отображается общедоступный IP-адрес, по умолчанию и APIPA IP-адреса BGP второго экземпляра VPN-шлюза Azure.

1. Если вы внесли изменения, нажмите кнопку **сохранить** , чтобы зафиксировать изменения в VPN-шлюзе Azure.

## <a name="part-2-configure-bgp-on-cross-premises-s2s-connections"></a><a name ="crosspremises"></a>Часть 2. Настройка BGP для распределенных подключений S2S

Чтобы установить распределенное подключение, необходимо создать *локальный сетевой шлюз* , который будет представлять ваше локальное VPN-устройство, а также *Подключение* к VPN-шлюзу с помощью шлюза локальной сети, как описано в разделах [Создание подключения типа "сеть — сеть](vpn-gateway-howto-site-to-site-resource-manager-portal.md)". Эта статья содержит дополнительные свойства, необходимые для указания параметров конфигурации BGP.

**Схема 3**

:::image type="content" source="./media/bgp-howto/bgp-crosspremises.png" alt-text="Схема, демонстрирующая сетевую архитектуру и параметры" border="false":::

### <a name="1-configure-bgp-on-the-local-network-gateway"></a>1. Настройка BGP в шлюзе локальной сети

На этом шаге вы настроите BGP на шлюзе локальной сети. Используйте следующий снимок экрана в качестве примера. На снимке экрана показан шлюз локальной сети (site5) с параметрами, указанными в схеме 3.

:::image type="content" source="./media/bgp-howto/create-local-bgp.png" alt-text="Схема, демонстрирующая сетевую архитектуру и параметры":::

#### <a name="important-configuration-considerations"></a>Важные рекомендации по конфигурации

* Номер ASN и IP-адрес узла BGP должны соответствовать конфигурации локального VPN-маршрутизатора.
* **Адресное пространство** можно оставить пустым, только если для подключения к этой сети используется BGP. VPN-шлюз Azure будет внутренним образом добавить маршрут IP-адреса узла BGP в соответствующий туннель IPsec. Если вы **не** используете протокол BGP между VPN-шлюзом Azure и этой конкретной сетью, **необходимо** предоставить список допустимых префиксов адресов для **адресного пространства**.
* При необходимости можно использовать **IP-адрес APIPA** (169.254. x. x) в качестве локального однорангового узла BGP. Но вам также потребуется указать IP-адрес APIPA, как описано выше в этой статье для VPN-шлюза Azure. в противном случае сеанс BGP не может быть установлен для этого подключения.
* Сведения о конфигурации BGP можно ввести во время создания шлюза локальной сети. также можно добавить или изменить конфигурацию BGP на странице **Конфигурация** ресурса шлюза локальной сети.

**Пример**

В этом примере используется адрес APIPA (169.254.100.1) в качестве IP-адреса локального узла BGP:

:::image type="content" source="./media/bgp-howto/local-apipa.png" alt-text="Схема, демонстрирующая сетевую архитектуру и параметры":::

### <a name="2-configure-a-s2s-connection-with-bgp-enabled"></a>2. Настройка подключения S2S с включенным BGP

На этом шаге вы создадите новое подключение с включенным BGP. Если у вас уже есть подключение и вы хотите включить для него BGP, можно [Обновить существующее соединение](#update).

#### <a name="to-create-a-connection-with-bgp-enabled"></a>Создание подключения с включенным BGP

Чтобы создать новое подключение с включенным BGP, на странице " **Добавление подключения** " введите значения, а затем установите флажок **включить BGP** , чтобы включить BGP для этого подключения. Нажмите кнопку **ОК**, чтобы создать подключение.

:::image type="content" source="./media/bgp-howto/ipsec-connection-bgp.png" alt-text="Схема, демонстрирующая сетевую архитектуру и параметры":::

#### <a name="to-update-an-existing-connection"></a><a name ="update"></a>Обновление существующего подключения

Если вы хотите изменить параметр BGP для подключения, перейдите на страницу **конфигурации** ресурса подключения, а затем переключите параметр **BGP** , как показано в следующем примере. Щелкните **Сохранить**, чтобы сохранить все изменения.

:::image type="content" source="./media/bgp-howto/update-bgp.png" alt-text="Схема, демонстрирующая сетевую архитектуру и параметры":::

## <a name="part-3-configure-bgp-on-vnet-to-vnet-connections"></a><a name ="v2v"></a>Часть 3. Настройка подключений BGP при подключении между виртуальными сетями

Действия по включению и отключению протокола BGP для подключения между виртуальными сетями аналогичны шагам S2S в [части 2](#crosspremises). При создании подключения можно включить протокол BGP или обновить конфигурацию в существующем соединении между виртуальными сетями.

>[!NOTE] 
>Подключение между виртуальными сетями без BGP ограничит обмен данными только с двумя подключенными виртуальных сетей. Включите протокол BGP, чтобы разрешить транзитную маршрутизацию другим подключениям S2S или VNet-VNet этих двух виртуальных сетей.
>

Для контекста, ссылающегося на **схему 4**, если BGP должен быть отключен между TestVNet2 и TestVNet1, TestVNet2 не будет изучать маршруты для локальной сети site5 и, следовательно, не сможет связаться с сайтом 5. После включения BGP, как показано на схеме 4, все три сети смогут взаимодействовать через подключения IPsec и между виртуальными сетями.

**Схема 4**

:::image type="content" source="./media/bgp-howto/bgp-crosspremises-v2v.png" alt-text="Схема, демонстрирующая сетевую архитектуру и параметры" border="false":::

## <a name="next-steps"></a>Дальнейшие шаги

Установив подключение, можно добавить виртуальные машины в виртуальные сети. Инструкции см. в статье о [создании виртуальной машины](../virtual-machines/windows/quick-create-portal.md).
