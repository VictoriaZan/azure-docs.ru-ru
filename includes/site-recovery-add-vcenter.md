---
author: rayne-wiselman
ms.service: site-recovery
ms.topic: include
ms.date: 10/26/2018
ms.author: raynew
ms.openlocfilehash: 926fb3e9a2c09d30da549330842d8b7e185674ae
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2019
ms.locfileid: "67185816"
---
В окне **Add vCenter** (Добавление vCenter) укажите понятное имя для узла vSphere или сервера vCenter, а также IP-адрес или полное доменное имя сервера. Если серверы VMware настроены на прослушивание запросов через другой порт, оставьте порт 443. Выберите учетную запись, чтобы подключиться к серверу VMware vCenter или vSphere ESXi. Последовательно выберите **ОК**.

    ![VMware](./media/site-recovery-add-vcenter/vmware-server.png)

   > [!NOTE]
   > Если вы добавляете сервер VMware vCenter или узел VMware vSphere с учетной записью, не имеют права администратора на сервере vCenter или узла, убедитесь, что учетная запись имеет следующие привилегии: Центра обработки данных, хранилище данных, папки, узла, сети, ресурса, виртуальной машины и распределенного коммутатора vSphere. Кроме того, для сервера vCenter VMware следует активировать права представлений хранилища.
