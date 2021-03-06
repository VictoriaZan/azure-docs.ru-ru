---
title: Планирование развертывания Решения Azure VMware
description: В этой статье описывается рабочий процесс развертывания Решения Azure VMware.  Его конечным результатом будет среда, готовая к созданию виртуальной машины и миграции.
ms.topic: tutorial
ms.date: 10/16/2020
ms.openlocfilehash: 1ef83a568e41fe99f1e8e385a599de9c5ab7c0ca
ms.sourcegitcommit: cd9754373576d6767c06baccfd500ae88ea733e4
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/20/2020
ms.locfileid: "95529739"
---
# <a name="planning-the-azure-vmware-solution-deployment"></a>Планирование развертывания Решения Azure VMware

В этой статье изложен процесс планирования для обнаружения и сбора данных, используемых во время развертывания. При планировании развертывания обязательно задокументируйте собранную информацию, которую затем будет удобно использовать для справки во время развертывания.

После выполнения процессов, описанных в этом кратком руководстве, будет создана готовая рабочая среда, в которой можно будет создать виртуальные машины и выполнить миграцию. 

>[!IMPORTANT]
>Перед созданием ресурса Решения Azure VMware отправьте в службу поддержки запрос на выделение узлов, как описано в статье [Включение ресурса Решения Azure VMware](enable-azure-vmware-solution.md). Когда группа поддержки получит ваш запрос, ей потребуется до пяти рабочих дней на его подтверждение и выделение узлов. Если у вас уже есть частное облако Решения Azure VMware и вы хотите выделить для него больше узлов, выполните те же действия. 


## <a name="subscription"></a>Подписка

Укажите подписку, которую планируется использовать для развертывания Решения Azure VMware.  Вы можете создать новую подписку или использовать существующую.

>[!NOTE]
>Подписка должна быть связана с Соглашением Microsoft Enterprise.

## <a name="resource-group"></a>Группа ресурсов

Укажите группу ресурсов, которую вы хотите использовать для Решения Azure VMware.  Как правило, группа ресурсов создается специально для Решения Azure VMware, но вы можете использовать уже имеющуюся.

## <a name="region"></a>Регион

Определите регион, в котором должно быть развернуто Решение Azure VMware.  Дополнительные сведения см. в [руководстве по доступности продуктов по регионам](https://azure.microsoft.com/en-us/global-infrastructure/services/?products=azure-vmware).

## <a name="resource-name"></a>Имя ресурса

Определите имя ресурса, которое будет использоваться во время развертывания.  Имя ресурса представляет собой понятное описательное имя, обозначающее частное облако Решения Azure VMware.

>[!IMPORTANT]
>Длина имени не должна превышать 40 символов. Если имя превышает это ограничение, вы не сможете создавать общедоступные IP-адреса для использования с частным облаком. 

## <a name="size-hosts"></a>Размер узлов

Определите размер узлов, которые вы хотите использовать при развертывании Решения Azure VMware.  Полный список см. в разделе документации по [частным облакам и кластерам Решения Azure VMware](concepts-private-clouds-clusters.md#hosts).

## <a name="number-of-hosts"></a>Число узлов

Определите число узлов, которые требуется развернуть в частном облаке Решения Azure VMware.  Минимальное число узлов равно трем, а максимальное — 16 на кластер.  Дополнительные сведения см. в документации по [частному облаку и кластерам Решения Azure VMware](concepts-private-clouds-clusters.md#clusters) документации.

Вы всегда можете расширить кластер позже, если вам потребуется больше узлов, чем было развернуто изначально.

## <a name="vcenter-admin-password"></a>Пароль администратора vCenter
Задайте пароль администратора vCenter.  Во время развертывания вы создадите пароль администратора vCenter. Пароль относится к учетной записи администратора cloudadmin@vsphere.local, используемой в процессе сборки vCenter. Вы будете использовать его для входа в vCenter.

## <a name="nsx-t-admin-password"></a>Пароль администратора NSX-T
Задайте пароль администратора NSX-T.  Во время развертывания вы создадите пароль администратора NSX-T. Пароль назначается администратору в учетной записи NSX во время сборки NSX. Он будет использоваться для входа в NSX-T Manager.

## <a name="ip-address-segment"></a>Сегмент IP-адреса

Первым шагом в планировании развертывания является планирование сегментации IP-адресов.  Решение Azure VMware принимает предоставленную вами сеть /22. Затем оно делит эту сеть на сегменты меньшего размера и использует такие IP-сегменты для vCenter, VMware HCX, NSX-T и vMotion.

Решение Azure VMware подключается к виртуальной сети Microsoft Azure через внутренний канал ExpressRoute. В большинстве случаев оно подключается к центру обработки данных с помощью ExpressRoute Global Reach. 

Решение Azure VMware, имеющаяся среда Azure и локальная среда обмениваются данными о маршрутах (как правило). В этом случае блок сетевых адресов CIDR /22, определенный на этом шаге, не должен перекрываться с другими адресами, которые у вас уже есть в локальной среде или Azure.

**Пример**. 10.0.0.0/22

Дополнительные сведения см. в [контрольном списке планирования сети](tutorial-network-checklist.md#routing-and-subnet-considerations).

:::image type="content" source="media/pre-deployment/management-vmotion-vsan-network-ip-diagram.png" alt-text="Определение сегмента IP-адреса" border="false":::  

## <a name="ip-address-segment-for-virtual-machine-workloads"></a>Сегмент IP-адресов для рабочих нагрузок виртуальных машин

Определите сегмент IP-адресов, чтобы создать первую сеть (сегмент NSX) в частном облаке.  Иными словами, необходимо создать сегмент сети в Решении Azure VMware для развертывания в нем виртуальных машины.   

Даже если планируется только расширение сетей 2-го уровня, создайте такой сегмент сети для проверки среды.

Напомним, что все создаваемые IP-сегменты должны быть уникальными в масштабах Azure и локальной среды.  

**Пример**. 10.0.4.0/24

:::image type="content" source="media/pre-deployment/nsx-segment-diagram.png" alt-text="Определение сегмента IP-адресов для рабочих нагрузок виртуальных машин" border="false":::     

## <a name="optional-extend-networks"></a>Расширения сетей (необязательно)

Вы можете расширить сегменты сети из локальной среды, включив в них Решение Azure VMware. Если вы планируете это делать, определите эти сети сейчас.  

Помните о следующем:

- Если вы планируете расширять сети из локальной среды, эти сети должны быть подключены к [распределенному коммутатору vSphere (vDS)](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.networking.doc/GUID-B15C6A13-797E-4BCB-B9D9-5CBC5A60C3A6.html) в локальной среде VMware.  
- Если сети, которые вы хотите расширить, работают через [стандартный коммутатор vSphere](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.networking.doc/GUID-350344DE-483A-42ED-B0E2-C811EE927D59.html), их расширить нельзя.

## <a name="azure-virtual-network-to-attach-azure-vmware-solution"></a>Виртуальная сеть Azure для подключения Решения Azure VMware

Чтобы получить доступ к частному облаку Решения Azure VMware, необходимо подключить канал ExpressRoute, входящий в состав Решения Azure VMware, к виртуальной сети Azure.  Во время развертывания можно определить новую виртуальную сеть или выбрать существующую.

Канал ExpressRoute из Решения Azure VMware подключается к шлюзу ExpressRoute в виртуальной сети Azure, которую вы определили на этом шаге.  

>[!IMPORTANT]
>Для подключения к Решению Azure VMware можно использовать существующий шлюз ExpressRoute, если он не превышает ограничение в четыре канала ExpressRoute на виртуальную сеть.  Но для доступа к Решению Azure VMware из локальной среды через ExpressRoute требуется служба ExpressRoute Global Reach, так как шлюз ExpressRoute не обеспечивает транзитивную маршрутизацию между подключенными каналами.  

Если вы хотите подключить канал ExpressRoute из Решения Azure VMware к существующему шлюзу ExpressRoute, это можно сделать после развертывания.  

Определите, хотите ли вы подключить Решение Azure VMware к существующему шлюзу Express Route.  

* **Да** — определите виртуальную сеть, которая не используется во время развертывания.
* **Нет** — определите существующую виртуальную сеть или создайте новую во время развертывания.

В любом случае задокументируйте, что требуется сделать на этом шаге.

>[!NOTE]
>Эта виртуальная сеть находится в локальной среде и Решении Azure VMware, поэтому убедитесь, что сегменты IP-адресов, используемые в этой виртуальной сети и подсетях, не перекрываются.

:::image type="content" source="media/pre-deployment/azure-vmware-solution-expressroute-diagram.png" alt-text="Определение виртуальной сети Azure для подключения Решения Azure VMware" border="false":::

## <a name="vmware-hcx-network-segments"></a>Сегменты сети VMware HCX

VMware HCX — это технология, которая входит в состав Решения Azure VMware. VMware HCX используется преимущественно для миграции рабочих нагрузок и аварийного восстановления. Если вы собираетесь делать что-либо из этого, лучше спланировать это сейчас.   В противном случае можно пропустить этот шаг и перейти к следующему.

[!INCLUDE [hcx-network-segments](includes/hcx-network-segments.md)]

## <a name="next-steps"></a>Дальнейшие действия
После сбора и документирования необходимой информации перейдите к следующему разделу, чтобы создать частное облако Решения Azure VMware.

> [!div class="nextstepaction"]
> [Развертывание Решения Azure VMware](deploy-azure-vmware-solution.md)
