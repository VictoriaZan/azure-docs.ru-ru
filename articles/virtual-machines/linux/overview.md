---
title: Общие сведения о виртуальных машинах Linux в Azure
description: Обзор виртуальных машин Linux в Azure.
author: cynthn
ms.service: virtual-machines-linux
ms.topic: overview
ms.workload: infrastructure
ms.date: 11/14/2019
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 54982189a5da584c7daf66855ffb655e403a455a
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/02/2020
ms.locfileid: "96500518"
---
# <a name="linux-virtual-machines-in-azure"></a>Виртуальные машины Linux в Azure

Виртуальные машины Azure — один из нескольких типов [запрашиваемых масштабируемых вычислительных ресурсов](/azure/architecture/guide/technology-choices/compute-decision-tree), которые предоставляет Azure. Обычно виртуальную машину выбирают, когда требуется более строгий контроль за вычислительной средой, чем в других вариантах. В этой статье содержатся сведения о том, что следует учитывать перед созданием виртуальной машины, а также инструкции по созданию виртуальной машины и управлению ею.

Виртуальная машина Azure предоставляет гибкие возможности виртуализации без необходимости приобретать и обслуживать физическое оборудование, на котором она выполняется. Однако вам по-прежнему приходится обслуживать виртуальную машину, выполняя разные задачи — настройка, установка исправлений и программного обеспечения, работающего на виртуальной машине.

Виртуальные машины Azure можно использовать разными способами. Некоторые примеры.

* **Разработка и тестирование.** Виртуальные машины Azure обеспечивают быстрый и простой способ создания компьютера с определенными конфигурациями, необходимыми для написания кода и тестирования приложения.
* **Приложения в облаке.** Так как уровень спроса на приложение может меняться, с экономической точки зрения разумно запускать его на виртуальной машине в Azure. Вы платите за дополнительные виртуальные машины, если они вам нужны, и отключаете их, если они не нужны.
* **Расширенный центр обработки данных.** Виртуальные машины в виртуальной сети Azure можно легко подключить к корпоративной сети.

Вы можете увеличить масштаб виртуальных машин, используемых приложением, а также развернуть дополнительные виртуальные машины в соответствии с требованиями.

## <a name="what-do-i-need-to-think-about-before-creating-a-vm"></a>О чем следует подумать перед созданием виртуальной машины?
При создании инфраструктуры приложения в Azure всегда нужно учитывать множество [рекомендаций по проектированию](/azure/architecture/reference-architectures/n-tier/linux-vm). Перед началом работы следует подумать о следующих аспектах для виртуальной машины:

* имена ресурсов приложения;
* расположение, в котором хранятся ресурсы;
* размер виртуальной машины;
* максимальное число виртуальных машин, которые можно создать;
* операционная система, под управлением которой будет работать виртуальная машина;
* конфигурация виртуальной машины после ее запуска;
* связанные ресурсы, необходимые для виртуальной машины.

### <a name="locations"></a>Расположения
Все ресурсы, созданные в Azure, распределяются по нескольким [географическим регионам](https://azure.microsoft.com/regions/) во всем мире. Как правило, при создании виртуальной машины регион называется **расположением**. Для виртуальной машины расположение указывает на место хранения виртуальных жестких дисков.

В этой таблице приведены некоторые способы, с помощью которых можно получить список доступных расположений.

| Метод | Описание |
| --- | --- |
| Портал Azure |Выберите расположение из списка при создании виртуальной машины. |
| Azure PowerShell |Используйте команду [Get-AzLocation](/powershell/module/az.resources/get-azlocation). |
| REST API |Используйте операцию [вывода списка расположений](/rest/api/resources/subscriptions). |
| Azure CLI |Используйте операцию [az account list-locations](/cli/azure/account?view=azure-cli-latest). |

### <a name="singapore-data-residency"></a>Размещение данных в Сингапуре

В Azure функция хранения данных клиентов в одном регионе в настоящее время доступна только для региона "Юго-Восточная Азия (Сингапур)" в Азиатско-Тихоокеанском географическом регионе. Для всех других регионов данные клиента хранятся в геообъектах. Дополнительные сведения см. на [этой странице](https://azuredatacentermap.azurewebsites.net/).

## <a name="availability"></a>Доступность
В рамках отраслевого соглашения об уровне обслуживания Azure мы объявили о поддержке одного экземпляра виртуальной машины с гарантированной доступностью в течение 99,9 % времени при условии его развертывания с использованием хранилища класса Premium для всех дисков.  Чтобы обеспечить соответствие соглашению об уровне обслуживания с гарантированной доступностью виртуальных машин в течение 99,95 % времени, вам так или иначе нужно развернуть две или несколько виртуальных машин для выполнения рабочих нагрузок в рамках группы доступности. В группе доступности виртуальные машины распределяются по несколькими доменам сбоя в центрах обработки данных Azure, а также развертываются на узлах с разными периодами обслуживания. В полном [соглашении об уровне обслуживания Azure](https://azure.microsoft.com/support/legal/sla/virtual-machines/) поясняется гарантированная доступность Azure в целом.

## <a name="vm-size"></a>Размер виртуальной машины
Используемый [размер](../sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) виртуальной машины зависит от рабочей нагрузки, которую требуется выполнить. Позже выбранный размер определяет разные факторы, например вычислительную мощность, объем памяти и хранилища. Azure предлагает широкий спектр размеров для поддержки разных вариантов использования.

Azure взимает [почасовую оплату](https://azure.microsoft.com/pricing/details/virtual-machines/linux/), исходя из размера и операционной системы виртуальной машины. При частичном использовании Azure взимает плату только за использованные минуты. Плата за использование хранилища взимается отдельно.

## <a name="vm-limits"></a>Ограничения виртуальной машины
Для подписки Azure предусмотрена [квота](../../azure-resource-manager/management/azure-subscription-service-limits.md) по умолчанию, от которой зависит возможность развертывания большого количества виртуальных машин для проекта. Текущее ограничение для каждой подписки составляет 20 виртуальных машин на регион. Чтобы увеличить квоту, следует отправить [соответствующий запрос в службу поддержки](../../azure-portal/supportability/resource-manager-core-quotas-request.md).

## <a name="managed-disks"></a>Управляемые диски

Управляемые диски выполняют создание учетной записи хранения Azure и управление ею в фоновом режиме. При этом вам не нужно беспокоиться об ограничениях масштабируемости учетной записи хранения. Вам необходимо указать размер диска и уровень производительности ("Стандартный" или "Премиум"), а создание и управление Azure берет на себя. При добавлении дисков или масштабировании виртуальной машины не нужно беспокоиться об используемом хранилище. Чтобы создать виртуальные машины с управляемыми дисками ОС и данных, [используйте интерфейс командной строки Azure](quick-create-cli.md) или портал Azure. Если у вас есть виртуальные машины с неуправляемыми дисками, можно [преобразовать виртуальные машины для архивации с помощью Управляемых дисков](convert-unmanaged-to-managed-disks.md).

Вы также можете управлять пользовательскими образами в одной учетной записи хранения на каждый регион Azure и использовать их для создания сотен виртуальных машин в одной подписке. Дополнительные сведения об управляемых дисках см. в [этой статье](../managed-disks-overview.md).

## <a name="distributions"></a>Дистрибутивы 
Microsoft Azure поддерживает различные дистрибутивы Linux, которые предоставляются и обслуживаются партнерами.  Доступные дистрибутивы можно найти в Azure Marketplace. Майкрософт активно сотрудничает с разными сообществами Linux, чтобы расширить список [рекомендованных дистрибутивов Linux для Azure](endorsed-distros.md).

Если в коллекции отсутствует необходимый дистрибутив Linux, вы всегда можете использовать свою виртуальную машину Linux, [создав VHD-файл виртуальной машины Linux и передав его в Azure](create-upload-generic.md).

Корпорация Майкрософт тесно сотрудничает с партнерами, чтобы гарантировать обновление и оптимизацию доступных образов для среды выполнения Azure.  Дополнительные сведения о предложениях партнеров Azure см. по следующим ссылкам:

* Linux в Azure — [рекомендованные дистрибутивы](endorsed-distros.md)
* SUSE — [Azure Marketplace — SUSE Linux Enterprise Server](https://azuremarketplace.microsoft.com/marketplace/apps?page=1&search=suse)
* RedHat — [Azure Marketplace — Red Hat Enterprise Linux](https://azuremarketplace.microsoft.com/marketplace/apps?search=Red%20Hat%20Enterprise%20Linux)
* Canonical — [Azure Marketplace — Ubuntu Server](https://azuremarketplace.microsoft.com/marketplace/apps/Canonical.UbuntuServer)
* Debian — [Azure Marketplace — Debian](https://azuremarketplace.microsoft.com/marketplace/apps?search=Debian&page=1)
* FreeBSD — [Azure Marketplace —FreeBSD](https://azuremarketplace.microsoft.com/marketplace/apps?search=freebsd&page=1)
* Flatcar — [Azure Marketplace — Flatcar Container Linux](https://azuremarketplace.microsoft.com/marketplace/apps?search=Flatcar&page=1)
* RancherOS — [Azure Marketplace —RancherOS](https://azuremarketplace.microsoft.com/marketplace/apps/rancher.rancheros)
* Bitnami — [Bitnami Library для Azure](https://azure.bitnami.com/)
* Mesosphere — [Azure Marketplace — Mesosphere DC/OS в Azure](https://azure.microsoft.com/services/kubernetes-service/mesosphere/)
* Docker — [Azure Marketplace — образы Docker](https://azuremarketplace.microsoft.com/marketplace/apps?search=docker&page=1&filters=virtual-machine-images)
* Jenkins — [Azure Marketplace — платформа CloudBees Jenkins](https://azuremarketplace.microsoft.com/marketplace/apps/cloudbees.cloudbees-core-contact)


## <a name="cloud-init"></a>Cloud-init 

В соответствии с правильной культурой разработки и операций, всю инфраструктуру необходимо включить в код.  Когда вся инфраструктура находится в коде, ее можно легко воссоздать.  Azure работает со всеми основными средствами автоматизации, включая Ansible, SaltStack, Puppet и Chef.  Кроме того, в Azure имеются собственные средства автоматизации:

* [Шаблоны Azure](create-ssh-secured-vm-from-template.md);
* [Azure `VMaccess`](../extensions/vmaccess.md)

Azure поддерживает пакет [cloud-init](https://cloud-init.io/) для большинства дистрибутивов Linux, которые поддерживают его.  Мы активно сотрудничаем с нашими утвержденными партнерами, работающими над дистрибутивами Linux, чтобы образы с поддержкой cloud-init стали доступными в Azure Marketplace. Эти образы обеспечивают бесперебойную работу развертываний и конфигураций cloud-init с виртуальными машинами и масштабируемыми наборами виртуальных машин.

* [Использование cloud-init на виртуальных машинах Linux в Azure](using-cloud-init.md)

## <a name="storage"></a>Память
* [Введение в службу хранилища Microsoft Azure](../../storage/common/storage-introduction.md)
* [Добавление диска к виртуальной машине Linux](add-disk.md)
* [Подключение диска данных к виртуальной машине Linux на портале Azure](attach-disk-portal.md)

## <a name="networking"></a>Сеть
* [Обзор виртуальной сети](../../virtual-network/virtual-networks-overview.md)
* [IP-адреса в Azure](../../virtual-network/public-ip-addresses.md)
* [Открытие портов для виртуальной машины Linux в Azure](nsg-quickstart.md)
* [Создание полного доменного имени на портале Azure](../create-fqdn.md)


## <a name="data-residency"></a>Местонахождение данных

В Azure функция хранения данных клиентов в одном регионе сейчас доступна только для регионов "Юго-Восточная Азия (Сингапур)" в Азиатско-Тихоокеанском географическом регионе и "Южная Бразилия" (штат Сан-Паулу) в географическом регионе "Бразилия". Для всех других регионов данные клиента хранятся в геообъектах. Дополнительные сведения см. на [этой странице](https://azuredatacentermap.azurewebsites.net/).


## <a name="next-steps"></a>Дальнейшие действия

Создайте свою первую виртуальную машину!

- [Портал](quick-create-portal.md)
- [Azure CLI](quick-create-cli.md)
- [PowerShell](quick-create-powershell.md)