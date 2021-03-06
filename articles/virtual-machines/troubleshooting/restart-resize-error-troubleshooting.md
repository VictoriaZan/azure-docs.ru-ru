---
title: Неполадки при перезапуске или изменении размера виртуальной машины в Azure | Документация Майкрософт
description: Устранение неполадок в развертывании Resource Manager при перезагрузке или изменении размера имеющейся виртуальной машины в Azure
services: virtual-machines
documentationcenter: ''
author: Deland-Han
manager: dcscontentpm
editor: ''
tags: top-support-issue
ms.assetid: 0756b52d-4f5a-4503-ae45-c00a6a2edcdf
ms.service: virtual-machines
ms.topic: troubleshooting
ms.date: 06/15/2018
ms.author: delhan
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 85acd8e26ca10730638332047a37d281358d205f
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/25/2020
ms.locfileid: "96022893"
---
# <a name="troubleshoot-deployment-issues-with-restarting-or-resizing-an-existing-windows-vm-in-azure"></a>Устранение неполадок в развертывании при перезагрузке или изменении размера существующей виртуальной машины Windows в Azure
Когда вы запускаете остановленную виртуальную машину Azure или изменяете размер существующей виртуальной машины Azure, часто возникает ошибка выделения ресурсов. Это происходит, когда кластер или регион не имеют доступных ресурсов или не поддерживают запрашиваемый размер виртуальной машины.

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]

## <a name="collect-activity-logs"></a>Сбор журналов действий
Для устранения неполадок прежде всего соберите журналы действий, чтобы определить ошибку, связанную с этой проблемой. Ниже представлены ссылки на подробные сведения о процессе.

[Просмотр операций развертывания](../../azure-resource-manager/templates/deployment-history.md)

[Просмотр журналов действий для управления ресурсами Azure](../../azure-resource-manager/management/view-activity-logs.md)

## <a name="issue-error-when-starting-a-stopped-vm"></a>Проблема: ошибка во время запуска остановленной виртуальной машины
При попытке запустить остановленную виртуальную машину отображается сообщение об ошибке выделения.

### <a name="cause"></a>Причина:
Запрос на запуск остановленной виртуальной машины нужно выполнять в исходном кластере, в котором размещена облачная служба. Но этот кластер не имеет свободного места для выполнения запроса.

### <a name="resolution"></a>Разрешение
* Остановите все виртуальные машины в группе доступности, затем перезапустите каждую из них.
  
  1. Щелкните **ресурс группы** ресурсов.  >  *your resource group*  >  **ресурсы** группы  >  *доступности*  >  **виртуальные машины**  >  , которые останавливает *Виртуальная машина*  >  **Stop**.
  2. После остановки всех виртуальных машин выберите каждую из них и нажмите кнопку "Пуск".
* Повторите запрос на перезапуск позже.

## <a name="issue-error-when-resizing-an-existing-vm"></a>Проблема: ошибка при изменении размера существующей виртуальной машины
При попытке изменить размер существующей виртуальной машины отображается сообщение об ошибке выделения.

### <a name="cause"></a>Причина:
Запрос на изменение размера виртуальной машины нужно выполнять в исходном кластере, в котором размещена облачная служба. Но этот кластер не поддерживает запрашиваемый размер виртуальной машины.

### <a name="resolution"></a>Разрешение
* Повторите запрос с указанием меньшего размера виртуальной машины.
* Если нельзя изменить размер запрошенной виртуальной машины,
  
  1. остановите все виртуальные машины в группе доступности.
     
     * Щелкните **ресурс группы** ресурсов.  >  *your resource group*  >  **ресурсы** группы  >  *доступности*  >  **виртуальные машины**  >  , которые останавливает *Виртуальная машина*  >  **Stop**.
  2. После остановки всех виртуальных машин увеличьте размер требуемой виртуальной машины.
  3. Выберите виртуальную машину с измененным размером, нажмите кнопку **Запустить** и запустите каждую из остановленных виртуальных машин.

## <a name="next-steps"></a>Следующие шаги
При возникновении проблем во время создания виртуальной машины Windows в Azure ознакомьтесь со статьей, посвященной [устранению неполадок в развертывании](./troubleshoot-deployment-new-vm-windows.md).
