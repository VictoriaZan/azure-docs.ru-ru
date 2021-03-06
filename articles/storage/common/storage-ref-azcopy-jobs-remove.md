---
title: Удаление заданий azcopy | Документация Майкрософт
description: В этой статье содержатся справочные сведения по команде azcopy Jobs Remove.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 07/24/2020
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: b1f1b58e9cce061aaa313457ec43256a766e3a2e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "87281979"
---
# <a name="azcopy-jobs-remove"></a>azcopy jobs remove

Удалить все файлы, связанные с заданным ИДЕНТИФИКАТОРом задания.

> [!NOTE] 
> Можно настроить расположение для сохранения файлов журнала и плана. Дополнительные сведения см. в описании команды [azcopy env](storage-ref-azcopy-env.md) .

```
azcopy jobs remove [jobID] [flags]
```

## <a name="related-conceptual-articles"></a>Связанные концептуальные статьи

- [Начало работы с AzCopy](storage-use-azcopy-v10.md)
- [Перенос данных с помощью AzCopy и хранилища BLOB-объектов](storage-use-azcopy-blobs.md)
- [Перенос данных с помощью AzCopy и хранилища файлов](storage-use-azcopy-files.md)
- [Configure, optimize, and troubleshoot AzCopy](storage-use-azcopy-configure.md) (Настройка, оптимизация и устранение неполадок с AzCopy)

## <a name="examples"></a>Примеры

```
  azcopy jobs rm e52247de-0323-b14d-4cc8-76e0be2e2d44
```

## <a name="options"></a>Параметры

**--Справка**                Справка по удалению.

## <a name="options-inherited-from-parent-commands"></a>Параметры, унаследованные от родительских команд

**--Cap-Мбит/с, с плавающей запятой**      Скорость передачи с прописными буквами в мегабит в секунду. Посекундная пропускная способность может немного отличаться от ограничения. Если этот параметр имеет значение 0 или пропущен, пропускная способность не ограничена.

**--выходной** формат строки выходных данных команды. Среди вариантов: Text, JSON. Значение по умолчанию — `text`. (по умолчанию `text` )

**--Trusted-Microsoft-суффиксы** указывает дополнительные суффиксы домена, в которых могут отправляться Azure Active Directory токены входа.  Значение по умолчанию — "*. Core.Windows.NET;*". core.chinacloudapi.cn; *. Core.cloudapi.de;*. core.usgovcloudapi.net ". Все перечисленные здесь значения добавляются к значениям по умолчанию. В целях безопасности следует размещать только Microsoft Azureные домены. Несколько записей разделяются точкой с запятой.

## <a name="see-also"></a>См. также

- [azcopy jobs](storage-ref-azcopy-jobs.md)
