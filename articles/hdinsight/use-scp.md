---
title: Использование SCP с Apache Hadoop в Azure HDInsight
description: В этом документе содержатся сведения о подключении к HDInsight с помощью команд SSH и SCP.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.custom: seoapr2020
ms.date: 04/22/2020
ms.openlocfilehash: 44c87a3415a236f614510aa3163ad2e9cbd1f64f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91856183"
---
# <a name="use-scp-with-apache-hadoop-in-azure-hdinsight"></a>Использование SCP с Apache Hadoop в Azure HDInsight

В этой статье содержатся сведения о безопасной передаче файлов в кластер HDInsight.

## <a name="copy-files"></a>Копирование файлов

Программу `scp` можно использовать для копирования файлов в отдельные узлы в кластере и обратно. Например, следующая команда копирует каталог `test.txt` из локальной системы в первичный головной узел:

```bash
scp test.txt sshuser@clustername-ssh.azurehdinsight.net:
```

Так как после символа `:` не указан путь, файл помещается в домашний каталог `sshuser`.

Например, следующая команда копирует файл `test.txt` из домашнего каталога `sshuser` в первичный головной узел в локальной системе:

```bash
scp sshuser@clustername-ssh.azurehdinsight.net:test.txt .
```

Команда `scp` может получать доступ только к файловой системе отдельных узлов в кластере. Его нельзя использовать для доступа к данным в HDFS-совместимом хранилище кластера.

Используйте команду `scp`, если вам нужно отправить ресурс, чтобы использовать его из сеанса SSH. Например, отправьте скрипт Python, а затем запустите его из сеанса SSH.

Сведения о непосредственной загрузке данных в HDFS-совместимое хранилище см. в следующих документах:

* [Использование службы хранилища Azure с кластерами Azure HDInsight](hdinsight-hadoop-use-blob-storage.md).
* [HDInsight с использованием Azure Data Lake Storage 1-го поколения](../hdinsight/hdinsight-hadoop-use-data-lake-storage-gen1.md).

## <a name="next-steps"></a>Дальнейшие шаги

* [Использование SSH с HDInsight](./hdinsight-hadoop-linux-use-ssh-unix.md)
* [Доступ к граничному узлу](hdinsight-apps-use-edge-node.md#access-an-edge-node)
