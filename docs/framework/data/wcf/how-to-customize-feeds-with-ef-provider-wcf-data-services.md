---
title: "Как настроить каналы с помощью поставщика Entity Framework (службы WCF Data Services) | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-oob"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
helpviewer_keywords: 
  - "Службы WCF Data Services, настройка"
  - "Службы WCF Data Services, настройка каналов"
ms.assetid: fd16272e-36f2-415e-850e-8a81f2b17525
caps.latest.revision: 3
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 3
---
# Как настроить каналы с помощью поставщика Entity Framework (службы WCF Data Services)
Службы [!INCLUDE[ssAstoria](../../../../includes/ssastoria-md.md)] позволяют настроить сериализацию Atom в ответе службы данных так, чтобы свойства сущности сопоставлялись с неиспользуемыми элементами, определенными в протоколе AtomPub.  Этот раздел показывает, как настроить атрибуты сопоставления типов сущностей в модели данных, определенных в файле EDMX, с помощью поставщика Entity Framework.  Для получения дополнительной информации см. [Настройка канала](../../../../docs/framework/data/wcf/feed-customization-wcf-data-services.md).  
  
 В этом разделе мы вручную изменим сформированный программой файл EDMX, содержащий модель данных.  Поскольку расширения модели данных не поддерживаются конструктором сущностей, необходимо вручную модифицировать этот файл.  Дополнительные сведения о файле EDMX, порождаемом инструментальными средствами модели EDM, см. в разделе [.edmx File Overview](http://msdn.microsoft.com/ru-ru/f4c8e7ce-1db6-417e-9759-15f8b55155d4).  Пример в этом разделе использует образец службы данных Northwind и автоматически сформированные клиентские классы службы данных.  Эта служба и клиентские классы данных создаются после выполнения действий, описанных в разделе [Краткое руководство по службам WCF Data Services](../../../../docs/framework/data/wcf/quickstart-wcf-data-services.md).  
  
### Изменение файла Northwind.edmx вручную для добавления атрибутов настройки каналов  
  
1.  В окне **Обозреватель решений** щелкните правой кнопкой мыши файл `Northwind.edmx` и выберите команду **Открыть с помощью**.  
  
2.  В диалоговом окне **Открыть с помощью — Northwind.edmx** выберите **Редактор XML** и нажмите кнопку **ОК**.  
  
3.  Найдите элемент `ConceptualModels` и замените имеющийся тип сущности `Customers` на элемент, содержащий атрибуты сопоставления для настройки канала.  
  
     [!code-xml[Astoria Custom Feeds#EdmFeedCustomers](../../../../samples/snippets/xml/VS_Snippets_Misc/astoria custom feeds/xml/northwind.csdl#edmfeedcustomers)]  
  
4.  Сохраните изменения и закройте файл Northwind.edmx.  
  
5.  Щелкните файл Northwind.edmx правой кнопкой мыши и выберите команду **Пользовательское средство**. \(Необязательно.\)  
  
     При этом будет повторно сформирован файл уровня объектов, который может потребоваться.  
  
6.  Перекомпилируйте проект.  
  
## Пример  
 Предыдущий пример возвращает следующий результат для URI `http://myservice/` `Northwind.svc/Customers('ALFKI')`.  
  
 [!code-xml[Astoria Custom Feeds#EdmFeedResult](../../../../samples/snippets/xml/VS_Snippets_Misc/astoria custom feeds/xml/edmfeedresult.xml#edmfeedresult)]  
  
## См. также  
 [Поставщик Entity Framework](../../../../docs/framework/data/wcf/entity-framework-provider-wcf-data-services.md)