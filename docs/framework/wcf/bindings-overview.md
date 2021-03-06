---
title: "Общие сведения о привязках Windows Communication Foundation | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
helpviewer_keywords: 
  - "привязки [WCF], общие сведения"
ms.assetid: cfb5842f-e0f9-4c56-a015-f2b33f258232
caps.latest.revision: 16
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 16
---
# Общие сведения о привязках Windows Communication Foundation
Привязки \- это объекты, используемые для задания сведений о связи, необходимых для подключения к конечной точке службы [!INCLUDE[indigo1](../../../includes/indigo1-md.md)].  Каждая конечная точка службы [!INCLUDE[indigo2](../../../includes/indigo2-md.md)] требует точного задания привязки.  В этом разделе описываются типы сведений о связи, определяемых привязками, элементы привязки и привязки, входящие в состав [!INCLUDE[indigo2](../../../includes/indigo2-md.md)], а также демонстрируется задание привязки для конечной точки.  
  
## Что определяет привязка  
 Информация в привязке может быть очень простой или очень сложной.  Самые простые привязки указывают только транспортный протокол \(такой как HTTP\), который должен использоваться для подключения к конечной точке.  Как правило, привязка содержит сведения о подключении к конечной точке, которые попадают в одну из следующих категорий.  
  
 Протоколы  
 Определяют используемый механизм безопасности: возможность надежного обмена сообщениями или параметры потока контекста транзакций.  
  
 кодировка  
 Определяет используемую в сообщениях кодировку \(например, текст или двоичное кодирование\).  
  
 Transport  
 Определяет основной используемый транспортный протокол \(например, TCP или HTTP\).  
  
## Элементы привязки  
 По сути, привязка состоит из упорядоченного стека элементов привязки, каждый из которых задает часть информации о связи, необходимой для подключения к конечной точке службы.  Два нижних уровня стека являются обязательными.  В основании стека находится элемент привязки транспорта, над ним \- элемент, содержащий спецификации кодирования сообщения.  Необязательные элементы привязки, задающие другие протоколы взаимодействия, располагаются над этими двумя обязательными элементами.  [!INCLUDE[crabout](../../../includes/crabout-md.md)] этих элементах привязки и их правильном порядке использования см. в разделе [Пользовательские привязки](../../../docs/framework/wcf/extending/custom-bindings.md).  
  
## Привязки, предоставляемые системой  
 Информация в привязке может быть сложной, а некоторые параметры могут быть несовместимыми с другими.  Поэтому [!INCLUDE[indigo2](../../../includes/indigo2-md.md)] содержит набор привязок, предоставляемых системой.  Эти привязки удовлетворяют большинство требований приложения.  В следующих классах представлены некоторые примеры привязок, предоставляемых системой.  
  
-   <xref:System.ServiceModel.BasicHttpBinding>: привязка протокола HTTP, которая подходит для подключения к веб\-службам, соответствующим спецификации WS\-I Basic Profile \(например, службы на основе веб\-службы ASP.NET\).  
  
-   <xref:System.ServiceModel.WSHttpBinding>: привязка с возможностью взаимодействия, которая подходит для подключения к конечным точкам, соответствующим протоколам WS\-\*.  
  
-   <xref:System.ServiceModel.NetNamedPipeBinding>: использует [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] для подключения к другим конечным точкам [!INCLUDE[indigo2](../../../includes/indigo2-md.md)] на том же компьютере.  
  
-   <xref:System.ServiceModel.NetMsmqBinding>: использует [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] для создания подключений очередей сообщений к другим конечным точкам [!INCLUDE[indigo2](../../../includes/indigo2-md.md)].  
  
 Полный список предоставляемых [!INCLUDE[indigo2](../../../includes/indigo2-md.md)] привязок с описаниями см. в разделе [Привязки, предоставляемые системой](../../../docs/framework/wcf/system-provided-bindings.md).  
  
## Использование собственных привязок  
 Если ни одна из предоставляемых системой привязок не имеет требуемого приложением службы сочетания функций, можно создать собственную привязку.  Это можно сделать двумя способами.  Можно создать новую привязку из уже имеющихся элементов привязки с помощью объекта <xref:System.ServiceModel.Channels.CustomBinding> или создать полностью пользовательскую привязку, наследуемую от привязки <xref:System.ServiceModel.Channels.Binding>.  [!INCLUDE[crabout](../../../includes/crabout-md.md)] создании собственной привязки с помощью этих двух методов см. в разделах [Пользовательские привязки](../../../docs/framework/wcf/extending/custom-bindings.md) и [Создание пользовательских привязок](../../../docs/framework/wcf/extending/creating-user-defined-bindings.md).  
  
## Использование привязок  
 Использование привязок включает два основных этапа.  
  
1.  Выбор или определение привязки.  Самый простой способ \- выбрать одну из предоставляемых системой привязок, включенных в [!INCLUDE[indigo2](../../../includes/indigo2-md.md)], и использовать ее с параметрами по умолчанию.  Также можно выбрать предоставляемую системой привязку и сбросить значения ее свойств таким образом, чтобы они соответствовали нужным требованиям.  Кроме того, можно создать пользовательскую или определяемую пользователем привязку с расширенными возможностями управления и настройки.  
  
2.  Создайте конечную точку, которая использует выбранную или определенную привязку.  
  
## Код и конфигурация  
 Определить привязки можно двумя способами: через код или конфигурацию.  Применять эти два способа можно независимо от того, какая привязка используется \- предоставляемая системой или пользовательская.  Как правило, использование кода позволяет полностью управлять определением привязки во время разработки.  С другой стороны, использование конфигурации позволяет администратору или пользователю службы или клиента [!INCLUDE[indigo2](../../../includes/indigo2-md.md)] изменять параметры привязки без повторной компиляции приложения службы.  Такая гибкость часто желательна, поскольку нет способа предугадать требования конкретного компьютера, на котором будет развернуто приложение [!INCLUDE[indigo2](../../../includes/indigo2-md.md)].  Если не указывать привязку \(и адрес\) в коде, их можно изменять без повторной компиляции или повторного развертывания приложения.  Обратите внимание, что привязки, определенные в коде, создаются после задания привязок в конфигурации, что позволяет определенным в коде привязкам переопределять любые привязки, определенные в конфигурации.  
  
## См. также  
 [Использование привязок для настройки служб и клиентов](../../../docs/framework/wcf/using-bindings-to-configure-services-and-clients.md)