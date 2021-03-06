---
title: "Разрешение внешних ресурсов | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-standard"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "VB"
  - "CSharp"
  - "C++"
  - "jsharp"
ms.assetid: ad3fa320-4b8f-4e5c-b549-01157591007a
caps.latest.revision: 4
author: "mairaw"
ms.author: "mairaw"
manager: "wpickett"
caps.handback.revision: 4
---
# Разрешение внешних ресурсов
Свойство **XmlResolver** класса **XmlDocument** используется классом **XmlDocument** для нахождения ресурсов, не встроенных в XML\-данные, например внешних определений DTD, сущностей и схем.  Эти элементы могут находиться в сети или на локальном диске, и могут быть найдены с помощью URI.  Это позволяет классу **XmlDocument** разрешать имеющиеся в документе узлы **EntityReference** и проверять документ по внешнему определению DTD или схеме.  
  
## Полностью доверенный XmlDocument  
 Свойство **XmlResolver** влияет на функциональность метода **XmlDocument.Load**.  В следующей таблице показано, как работает свойство **XmlDocument.XmlResolver**, если объект **XmlDocument** является полностью доверенным.  В следующей таблице показаны методы **XmlDocument.Load**, когда в качестве входных данных метод Load принимает объект **TextReader**, **String**, **Stream** или **URI**.  Эта таблица не применяется к методу **Load**, если **XmlDocument** загружается из **XmlReader**.  
  
|XmlResolver, свойство|Функция|Примечания|  
|---------------------------|-------------|----------------|  
|Это свойство имеет значение для ранее созданного класса **XmlResolver**, который имеет свойства, уже заданные пользователем.|Класс **XmlDocument** использует класс **XmlResolver**, который предназначен для разрешения имен файлов, ссылок на внешние ресурсы, например определения DTD, сущности и схемы.<br /><br /> Класс **XmlResolver** также используется при разрешении внешних ресурсов, необходимых при добавлении или изменении узлов в объекте **XmlDocument**.|Класс **XmlResolver**, переданный классу **XmlDocument**, является арбитром, который используется, когда необходимо найти и разрешить внешние ресурсы.|  
|Это свойство имеет значение **null** \(**Nothing** в Microsoft Visual Basic .NET\).|Функции, которым требуется внешний ресурс, не поддерживаются, например нахождение внешней схемы или определения DTD.  Внешние сущности также не будут разрешаться, и такие функции изменения, как вставка узлов сущностей, которые требуют разрешения, не поддерживаются.|Класс **XmlDocument** загружает файлы как анонимные, и не пытается разрешать какие\-либо другие ресурсы.|  
|Свойство не устанавливается, а оставляется в состоянии по умолчанию.|При разрешении имен, поиске внешних определений DTD, сущностей и схем в объекте **XmlDocument** создается и используется экземпляр класса **XmlUrlResolver** с учетными данными NULL, а при изменении узлов в качестве учетных данных используется значение **null**.||  
  
 В следующей таблице показан метод **XmlDocument.Load**, когда метод **Load** в качестве входных данных принимает объект **XmlReader**, а **XmlDocument** является полностью доверенным.  
  
|XmlResolver, свойство|Функция|Примечания|  
|---------------------------|-------------|----------------|  
|Класс **XmlResolver**, используемый **XmlDocument** \- это тот же класс, который используется объектом **XmlReader**.|В **XmlDocument** используется **XmlResolver**, назначенный для **XmlReader**.<br /><br /> Свойство **XmlDocument.Resolver** установить нельзя вне зависимости от уровня доверия **XmlDocument**, поскольку оно получает значение **XmlResolver** из **XmlReader**.  Нельзя пытаться переопределить параметры **XmlResolver** класса **XmlReader** путем задания свойства **XmlResolver** класса **XmlDocument**.|Объект **XmlReader** может быть типа **XmlTextReader**, **XmlValidatingReader** или пользовательским модулем чтения данных.  Если используемое средство чтения поддерживает разрешение сущностей, внешние сущности разрешаются.  Если переданный модуль чтения не поддерживает ссылки на сущности, то ссылки на сущности не разрешаются.|  
  
## Полудоверенный XmlDocument  
 В следующей таблице показано, как работает свойство **XmlDocument.XmlResolver**, если объект является полудоверенным.  Областью применения этой таблицы являются методы **XmlDocument.Load**, если входными данными для метода Load являются объекты **TextReader**, **String**, **Stream** или **URI**.  Эта таблица не применяется к методу **Load**, если **XmlDocument** загружается из **XmlReader**.  
  
|XmlResolver, свойство|Функция|Примечания|  
|---------------------------|-------------|----------------|  
|В случае неполного доверия свойству **XmlResolver** может быть задано только значение NULL.|При разрешении имен, поиске внешних DTD, сущностей и схем создается и используется в **XmlDocument** экземпляр **XmlUrlResolver** с учетными данными **null**, а при редактировании узлов используются учетные данные **null**.|Такое поведение идентично поведению в ситуации, когда свойство **XmlResolver** не задано, а оставлено в состоянии по умолчанию.<br /><br /> В **XmlDocument** для всех действий используются анонимные разрешения.|  
|Это свойство имеет значение **null** \(**Nothing** в Microsoft Visual Basic .NET\).|Функции, которым требуется внешний ресурс, не поддерживаются, например обнаружение внешней схемы или определения DTD.  Внешние сущности также не будут разрешаться, а такие действия по изменению, как вставка узлов сущностей, которые требуют разрешения, не поддерживаются.|Если это свойство имеет значение **null**, поведение остается неизменным независимо от уровня доверия **XmlDocument**.|  
|Свойство не устанавливается, а оставляется в состоянии по умолчанию.|При разрешении имен, поиске внешних DTD, сущностей и схем создается и используется в **XmlDocument** экземпляр **XmlUrlResolver** с учетными данными **null**, а при редактировании узлов используются учетные данные **null**.|В **XmlDocument** для всех действий используются анонимные разрешения.|  
  
 Областью применения этой таблицы является метод **XmlDocument.Load**, если входными данными для метода **Load** является объект **XmlReader**, а объект **XmlDocument** является полудоверенным.  
  
|XmlResolver, свойство|Функция|Примечания|  
|---------------------------|-------------|----------------|  
|Класс **XmlResolver**, используемый **XmlDocument** \- это тот же класс, который используется объектом **XmlReader**.|В **XmlDocument** используется **XmlResolver**, назначенный для **XmlReader**.<br /><br /> Свойство **XmlDocument.Resolver** установить нельзя вне зависимости от уровня доверия **XmlDocument**, поскольку оно получает значение **XmlResolver** из **XmlReader**.  Нельзя пытаться переопределить параметры **XmlResolver** класса **XmlReader** путем задания свойства **XmlResolver** класса **XmlDocument**.|Объект **XmlReader** может быть типа **XmlTextReader**, проверяющим объект <xref:System.Xml.XmlReader> или пользовательским модулем чтения данных.  Если используемое средство чтения поддерживает разрешение сущностей, внешние сущности разрешаются.  Если переданный модуль чтения не поддерживает ссылки на сущности, ссылки на сущности не разрешаются.|  
  
 Если настроить класс XmlResolver так, чтобы он содержал корректные учетные данные, можно получить доступ к внешним ресурсам.  
  
> [!NOTE]
>  Получить свойство **XmlResolver** невозможно.  Благодаря этому пользователь не может повторно использовать свойство **XmlResolver**, которому были заданы учетные данные.  Кроме того, если объект **XmlTextReader** или проверяющий объект <xref:System.Xml.XmlReader> используется для загрузки **XmlDocument**, а **XmlDocument** имеет заданный арбитр, арбитры из этих модулей чтения не кэшируются объектом **XmlDocument** после фазы **Load**, поскольку это также является рискованным для безопасности.  
  
 Дополнительные сведения см. в подразделе "Примечания" на справочной странице <xref:System.Xml.XmlResolver>.  
  
## См. также  
 [Модель DOM для XML](../../../../docs/standard/data/xml/xml-document-object-model-dom.md)