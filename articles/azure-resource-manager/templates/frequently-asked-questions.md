---
title: Часто задаваемые вопросы о шаблоне ARM
description: Часто задаваемые вопросы о шаблонах Azure Resource Manager.
ms.topic: conceptual
ms.date: 09/17/2020
ms.author: tomfitz
author: tfitzmac
ms.openlocfilehash: af6a46e16cd888e3ff6a382be2b1a4264fcc2941
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/26/2020
ms.locfileid: "96184014"
---
# <a name="frequently-asked-questions-about-arm-templates"></a>Часто задаваемые вопросы о шаблонах ARM

В этой статье содержатся ответы на часто задаваемые вопросы о шаблонах Azure Resource Manager (ARM).

## <a name="getting-started"></a>Начало работы

* **Что такое шаблоны ARM и зачем их использовать?**

  Шаблоны ARM — это файлы JSON, в которых вы определяете, что нужно развернуть в Azure. Шаблоны помогают реализовать решение с инфраструктурой "на основе кода" для Azure. Организация может многократно и надежно развертывать необходимую инфраструктуру в различных средах.
  
  Дополнительные сведения о том, как шаблоны ARM помогают управлять инфраструктурой Azure, см [. в статье что такое шаблоны ARM?](overview.md)

* **Разделы справки начать работу с шаблонами?**

  Для упрощения создания шаблонов ARM необходимы верные средства. Рекомендуется установить [Visual Studio Code](https://code.visualstudio.com/) и [расширение средств Azure Resource Manager](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools). Краткие сведения об этих средствах см. в разделе [Краткое руководство. Создание шаблонов Azure Resource Manager с помощью Visual Studio Code](quickstart-create-templates-use-visual-studio-code.md).

  Когда вы будете готовы ознакомиться с созданием шаблонов ARM, запустите [серию учебных руководств для начинающих по шаблонам ARM](template-tutorial-create-first-template.md). В этих руководствах вы узнаете, как пошагово выполнить процесс создания шаблона ARM. Вы узнаете о различных разделах шаблона и о том, как они работают вместе. Это содержимое также доступно в виде [модуля Microsoft Learn](/learn/modules/authoring-arm-templates/).

* **Следует ли использовать шаблоны ARM или terraform для развертывания в Azure?**

  Используйте параметр, который вам нравится лучше. Обе службы помогают автоматизировать развертывание в Azure.
  
  Мы считаем преимущества использования шаблонов ARM для других служб, основанных на инфраструктуре. Дополнительные сведения об этих преимуществах см [. в статье Зачем выбирать шаблоны ARM?](overview.md#why-choose-arm-templates)

## <a name="build-2020"></a>Сборка 2020

* **Презентация была пропущена в Microsoft Build 2020. Доступна ли презентация для просмотра?**

  Да, [Просмотрите его в любое время](https://mybuild.microsoft.com/sessions/82984db4-37a4-4ed3-bf8b-13298841ed18?source=sessions).

* **Где можно получить дополнительные сведения о новых функциях, о которых вы объявили при сборке?**

  Для получения общих сведений о функциях, которые мы работаем, присоединитесь к [группе развертываний Azure Advisor в Yammer](https://aka.ms/ARMMeet).

  Чтобы узнать о новом языке шаблона, [Зарегистрируйтесь для получения уведомлений](https://aka.ms/armLangUpdates).

  Дополнительные сведения о спецификациях шаблонов см. в разделе [спецификации шаблонов Azure Resource Manager (Предварительная версия)](template-specs.md).

## <a name="creating-and-testing-templates"></a>Создание и тестирование шаблонов

* **Где можно узнать о рекомендациях по использованию шаблонов ARM?**

  Рекомендации по реализации шаблонов см. в статье рекомендации по использованию шаблонов [ARM](template-best-practices.md). После создания шаблона запустите [набор средств тестирования ARM](https://github.com/azure/arm-ttk). Он проверяет, соответствует ли шаблон рекомендуемым рекомендациям.

* **Моя среда настроена на портале. Есть ли способ получить шаблон из существующей группы ресурсов?**

  Да, шаблон можно [экспортировать](export-template-portal.md) из группы ресурсов. Экспортированный шаблон является хорошей отправной точкой для изучения шаблонов, но вы, вероятно, захотите изменить его, прежде чем использовать его в рабочей среде.
  
  При экспорте шаблона можно выбрать ресурсы, которые необходимо включить в шаблон.

* **Можно ли создать группу ресурсов в шаблоне ARM и развернуть в ней ресурсы?**

  Да, вы можете создать группу ресурсов в шаблоне при развертывании шаблона на уровне подписки Azure. Пример создания группы ресурсов и развертывания ресурсов см. в разделе [Группа ресурсов и ресурсы](deploy-to-subscription.md#resource-groups).

* **Можно ли создать подписку в шаблоне ARM?**

  Да, дополнительные сведения см. [в разделе программное создание подписок Azure с помощью последних API-интерфейсов](../../cost-management-billing/manage/programmatically-create-subscription-enterprise-agreement.md).

* **Как можно протестировать шаблон перед развертыванием?**

  Перед развертыванием рекомендуется запустить [набор средств тестирования ARM](https://github.com/azure/arm-ttk) и [операцию "что если](template-deploy-what-if.md) " для шаблонов. Набор средств тестирования проверяет, использует ли шаблон рекомендации. Он выдает предупреждения при обнаружении изменений, которые могут улучшить процесс реализации шаблона.

  Операция "что если" показывает изменения, которые ваш шаблон будет вносить в вашу среду. Вы можете увидеть непредвиденные изменения перед их развертыванием. Что, если также возвращает ошибки, которые он может обнаружить во время предварительной проверки. Например, если шаблон содержит синтаксическую ошибку, он возвращает эту ошибку. Он также возвращает все ошибки, которые можно определить о последнем состоянии развернутых ресурсов. Например, если шаблон развертывает учетную запись хранения с уже используемым именем, то что если возвращает эту ошибку.

* **Где можно найти сведения о свойствах, доступных для каждого типа ресурсов?**

  VS Code предоставляет IntelliSense для работы со свойствами ресурсов. Можно также просмотреть ссылку на [шаблон](/azure/templates/) для свойств и описаний.

* **Мне нужно создать несколько экземпляров типа ресурса. Разделы справки создать итератор в шаблоне?**

  Используйте элемент Copy, чтобы указать более одного экземпляра. Копию можно использовать для [ресурсов](copy-resources.md), [свойств](copy-properties.md), [переменных](copy-variables.md)и [выходных](copy-outputs.md)данных.

## <a name="template-language"></a>Язык шаблона

* **Я слышал, что вы работаете с новым языком шаблона. Где можно найти дополнительные сведения?**

  Чтобы просмотреть новый язык, см. раздел [Project бицеп Repository](https://github.com/Azure/bicep). Чтобы получать сведения о новом языке, [Зарегистрируйтесь для получения уведомлений](https://aka.ms/armLangUpdates).

* **Существует ли план поддержки создания шаблонов в YAML?**

  В настоящее время нет плана для поддержки YAML. Мы считаем, что новый язык шаблона будет предлагать решение, которое проще в использовании, чем YAML или JSON.

* **Можно ли по-прежнему писать шаблоны в JSON после выпуска нового языка шаблона?**

  Да, можно продолжать использовать шаблоны JSON.

* **Будет ли предлагаться средство для преобразования шаблонов JSON в новый язык шаблона?**

  Да.

## <a name="template-specs"></a>Спецификации шаблонов

* **Как начать работу с предварительной версией спецификации шаблонов?**

  Установите последнюю версию PowerShell или Azure CLI. Для Azure PowerShell используйте версию [5.0.0 или более позднюю](/powershell/azure/install-az-ps). Для Azure CLI используйте версию [2.14.2 или более позднюю](/cli/azure/install-azure-cli).

* **Как связаны спецификации шаблонов и проекты Azure?**

  Схемы Azure будут использовать спецификации шаблонов в своей реализации, заменив `blueprint definition` ресурс на `template spec` ресурс. Мы предоставим путь перехода для преобразования определения схемы в спецификацию шаблона, но API определения схемы по-прежнему будут поддерживаться. Нет изменений в `blueprint assignment` ресурсе. Схемы останутся в пользовательском интерфейсе для составления управляемых сред в Azure.

* **Заменяют ли спецификации шаблонов связанные шаблоны?**

  Нет, но спецификации шаблонов предназначены для работы с связанными шаблонами. Не нужно перемещать связанный шаблон на общедоступную конечную точку перед развертыванием родительского шаблона. Вместо этого при создании спецификации шаблона родительский шаблон и его артефакты упаковываются вместе.

* **Можно ли совместно использовать спецификации шаблонов в подписках?**

  Да, они могут использоваться в подписках при условии, что пользователь имеет доступ на чтение к спецификации шаблона. Спецификации шаблонов нельзя использовать в клиентах.

## <a name="scripts-in-templates"></a>Скрипты в шаблонах

* **Можно ли включить сценарий в шаблон для выполнения задач, которые могут быть недоступны в шаблоне?**

  Да, использовать [скрипты развертывания](deployment-script-template.md). В шаблоны можно включить Azure PowerShell или Azure CLI скрипты. Эта функция доступна в предварительной версии.

* **Можно ли по-прежнему использовать расширения пользовательских скриптов и настройку требуемого состояния (DSC)?**

  Эти параметры по-прежнему доступны и не изменялись. Сценарии развертывания предназначены для выполнения действий, не связанных с гостевыми виртуальными машинами. Если необходимо выполнить сценарий в операционной системе узла на виртуальной машине, лучше выбрать расширение пользовательских сценариев и (или) DSC. Однако скрипты развертывания имеют свои преимущества, например задание времени ожидания.

* **Поддерживаются ли скрипты развертывания в Azure для государственных организаций?**

  Да, скрипты развертывания можно использовать в US Gov (Аризона) и US Gov (Вирджиния).

## <a name="preview-changes-before-deployment"></a>Предварительный просмотр изменений перед развертыванием

* **Можно ли предварительно просмотреть изменения, которые будут выполнены перед развертыванием шаблона?**

  Да, использовать [функцию "что если](template-deploy-what-if.md)". Он оценивает текущее состояние среды и сравнивает его с состоянием, которое будет существовать после развертывания. Вы можете проверить обобщенные изменения, чтобы убедиться, что у шаблона нет непредвиденных результатов.

* **Можно ли использовать то же, что и для инкрементного и полного режимов?**

  Да, оба [режима развертывания](deployment-modes.md) поддерживаются. Пример использования инкрементного режима см. в разделе [Run a-if Operation](template-deploy-what-if.md#run-what-if-operation). Пример использования полного режима см. в разделе [Подтверждение удаления](template-deploy-what-if.md#confirm-deletion).

* **Работает ли действие со связанными шаблонами?**

  Да, что, если оценивает состояние родительского шаблона и связанных с ним шаблонов.

* **Можно ли использовать, если в конвейере Azure?**

  Да, можно использовать параметр что если, чтобы убедиться, что конвейер должен продолжать работу.

* **Когда я использую "что если", я вижу изменения в свойствах, которых нет в моем шаблоне. Ожидается ли этот "шум"?**

  Что если находится в предварительной версии. Мы работаем над уменьшением шума. Вы Помогите нам улучшить, отправив проблемы в репозиторий GitHub здесь: https://aka.ms/WhatIfIssues

## <a name="template-visualizer"></a>Визуализатор шаблонов

* **Есть ли способ визуализировать шаблон ARM и его ресурсы?**

  У нас есть [расширение VS Code, созданное сообществом](https://aka.ms/ARMVisualizer) , которое отлично подходит для визуализации шаблона ARM. В нем отображаются развертываемые ресурсы и связи между ними.

* **Можно ли использовать визуализатор шаблонов вне VS Code?**

  На портале выполняется предварительный просмотр визуализатора шаблонов. Дополнительные сведения см. в этом [коротком сеансе из сборки](https://mybuild.microsoft.com/sessions/0525094b-1fd2-4f69-bfd6-6d2fff6ffe5f?source=sessions).

## <a name="deployment-limits"></a>Ограничения развертывания

* **Сколько групп ресурсов можно развернуть в одной операции развертывания?**

  В прошлом это ограничение было пятью группами ресурсов. Недавно было увеличено до 800 групп ресурсов. Дополнительные сведения см. в статье [Создание групп ресурсов и ресурсов на уровне подписки](deploy-to-subscription.md).

* **В журнале развертывания возникла ошибка с ограничением в 800 развертываний. Что мне делать?**

  Мы изменим, как ведется журнал развертывания для группы ресурсов. В прошлом, чтобы избежать этой ошибки, пришлось вручную удалять развертывания из этого журнала. Начиная с июня 2020, мы автоматически удаляем развертывание из журнала, так как приближается ограничение. См. статью [Автоматическое удаление из журнала развертывания](deployment-history-deletions.md).

  Удаление развертывания из журнала не влияет на развернутые ресурсы.

## <a name="templates-and-devops"></a>Шаблоны и инструменты DevOps

* **Можно ли интегрировать шаблоны ARM в Azure Pipelines?**

  Да. Описание использования шаблонов и конвейеров см. в разделе [учебник. Непрерывная интеграция шаблонов Azure Resource Manager с Azure pipelines](deployment-tutorial-pipeline.md) и [Интеграция шаблонов ARM с Azure pipelines](add-template-to-azure-pipelines.md).

* **Можно ли использовать действия GitHub для развертывания шаблона?**

  Да, см. статью [Развертывание шаблонов Azure Resource Manager с помощью действий GitHub](deploy-github-actions.md).

## <a name="next-steps"></a>Дальнейшие действия

Общие сведения о шаблонах ARM см. в статье [что такое шаблоны ARM?](overview.md).