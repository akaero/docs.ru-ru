---
title: "Импорт пользовательских метаданных для расширения WCF | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 78beb28f-408a-4c75-9c3c-caefe9595b1a
caps.latest.revision: 10
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 10
---
# Импорт пользовательских метаданных для расширения WCF
В процессе импорта метаданных в [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] на основе этих метаданных создается абстрактное представление службы или ее компонентов.Например, [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] может импортировать экземпляры <xref:System.ServiceModel.Description.ServiceEndpoint>, <xref:System.ServiceModel.Channels.Binding> или <xref:System.ServiceModel.Description.ContractDescription> из документа WSDL службы.Чтобы импортировать метаданные службы в [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)], следует использовать реализацию абстрактного класса <xref:System.ServiceModel.Description.MetadataImporter?displayProperty=fullName>.Типы, наследуемые от класса <xref:System.ServiceModel.Description.MetadataImporter>, реализуют поддержку для импорта форматов метаданных, которые используют преимущества логики импорта WS\-Policy в [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)].  
  
 Пользовательские метаданные состоят из элементов XML, которые не могут быть импортированы с помощью средств импорта метаданных, предоставляемых системой.Как правило, они включают пользовательские расширения WSDL и утверждения политики.  
  
 В этом разделе описано, как импортировать пользовательские расширения WSDL и утверждения политики.В нем не рассматривается сам процесс импорта.Дополнительные сведения об использовании типов, которые экспортируют и импортируют метаданные независимо от того, являются ли они пользовательскими или поддерживаемыми системой, см. в разделе [Экспорт и импорт метаданных](../../../../docs/framework/wcf/feature-details/exporting-and-importing-metadata.md).  
  
## Общие сведения  
 Тип <xref:System.ServiceModel.Description.WsdlImporter?displayProperty=fullName> является реализацией абстрактного класса <xref:System.ServiceModel.Description.MetadataImporter>, входящего в [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)].Тип <xref:System.ServiceModel.Description.WsdlImporter> импортирует метаданные языка WSDL с прикрепленными политиками, объединенными в объекте <xref:System.ServiceModel.Description.MetadataSet?displayProperty=fullName>.Утверждения политики и расширения WSDL, не распознаваемые стандартными средствами импорта метаданных, передаются для импорта зарегистрированным средствам импорта пользовательской политики и WSDL.Как правило, средства импорта реализуются с целью поддержки пользовательских элементов привязки или изменения импортированного контракта.  
  
 В данном разделе рассматриваются следующие вопросы.  
  
1.  Реализация и использование интерфейса <xref:System.ServiceModel.Description.IWsdlImportExtension?displayProperty=fullName>, который предоставляет данные WSDL пользовательским средствам импорта до создания описаний и создания кода.Этот интерфейс можно использовать для проверки или изменения типов описаний и компиляции кода с использованием заданного набора метаданных.  
  
2.  Реализация и использование интерфейса <xref:System.ServiceModel.Description.IPolicyImportExtension?displayProperty=fullName>, который предоставляет утверждения политики средствам импорта до создания объектов описания.Этот интерфейс можно использовать для проверки или изменения привязки или контракта на основании загруженных политик.  
  
 Дополнительные сведения об экспорте пользовательских расширений WSDL и утверждений политики см. в разделе [Экспорт пользовательских метаданных для расширения WCF](../../../../docs/framework/wcf/extending/exporting-custom-metadata-for-a-wcf-extension.md).  
  
## Импорт пользовательских расширений WSDL  
 Чтобы добавить поддержку импорта расширений WSDL, следует реализовать интерфейс <xref:System.ServiceModel.Description.IWsdlImportExtension>, а затем добавить эту реализацию в свойство <xref:System.ServiceModel.Description.WsdlImporter.WsdlImportExtensions%2A>.Также <xref:System.ServiceModel.Description.WsdlImporter> может загружать реализации интерфейса <xref:System.ServiceModel.Description.IWsdlImportExtension>, зарегистрированного в файле конфигурации приложения.Обратите внимание, что регистрируется большое количество средств импорта WSDL, при этом имеет значение порядок зарегистрированных средств импорта WSDL.  
  
 Если объект <xref:System.ServiceModel.Description.WsdlImporter> загружает и использует пользовательское средство импорта WSDL, сначала вызывается метод <xref:System.ServiceModel.Description.IWsdlImportExtension.BeforeImport%2A>, чтобы разрешить изменение метаданных до начала процесса импорта.Затем выполняется импорт контрактов, после чего вызывается метод <xref:System.ServiceModel.Description.IWsdlImportExtension.ImportContract%2A>, чтобы разрешить изменение контрактов, импортированных из метаданных.И, наконец, вызывается метод <xref:System.ServiceModel.Description.IWsdlImportExtension.ImportEndpoint%2A>, чтобы разрешить изменение импортированных конечных точек.  
  
 Дополнительные сведения см. в разделе [Практическое руководство. Импорт пользовательской информации WSDL](../../../../docs/framework/wcf/extending/how-to-import-custom-wsdl.md).  
  
### Импорт утверждений пользовательской политики  
 Тип <xref:System.ServiceModel.Description.WsdlImporter> и [Служебное средство ServiceModel Metadata Utility Tool \(Svcutil.exe\)](../../../../docs/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe.md) выполняют автоматическую обработку различных типов утверждений политики в выражениях политики, прикрепленных к документам WSDL.Эти средства собирают, нормализуют и объединяют выражения политики, прикрепленные к привязкам WSDL и портам WSDL.  
  
 Чтобы добавить поддержку утверждений пользовательской политики, следует реализовать интерфейс <xref:System.ServiceModel.Description.IPolicyImportExtension>, а затем добавить эту реализацию в свойство <xref:System.ServiceModel.Description.MetadataImporter.PolicyImportExtensions%2A>.Также <xref:System.ServiceModel.Description.MetadataImporter> может загружать реализации интерфейса <xref:System.ServiceModel.Description.IPolicyImportExtension>, зарегистрированного в файле конфигурации приложения.Обратите внимание, что регистрируется большое количество средств политик, при этом имеет значение порядок зарегистрированных средств импорта политик.  
  
 Система метаданных многократно вызывает метод <xref:System.ServiceModel.Description.IPolicyImportExtension.ImportPolicy%2A?displayProperty=fullName> для всех зарегистрированных расширений импорта политики для каждой комбинации альтернативной политики, прикрепленной к сообщению, операции и субъекта политики конечной точки.При импорте порта WSDL политики, прикрепленные к порту и соответствующей привязке WSDL, объединяются перед вызовом расширений импорта политики.Доступ к альтернативным политикам осуществляется с помощью <xref:System.ServiceModel.Description.PolicyConversionContext> в виде объектов <xref:System.ServiceModel.Description.PolicyAssertionCollection>.Каждый объект <xref:System.ServiceModel.Description.PolicyAssertionCollection> является коллекцией утверждений политики, представленных объектами <xref:System.Xml.XmlElement>.  
  
 Свойства <xref:System.ServiceModel.Description.PolicyConversionContext.Contract%2A> и <xref:System.ServiceModel.Description.PolicyConversionContext.BindingElements%2A> объекта <xref:System.ServiceModel.Description.PolicyConversionContext> предоставляют объекты <xref:System.ServiceModel.Description.ContractDescription> и <xref:System.ServiceModel.Channels.BindingElement>, импортированные из WSDL.Расширения импорта политики обрабатывают утверждения политики путем поиска экземпляров определенного типа утверждения политики, внесения соответствующих изменений в объекты <xref:System.ServiceModel.Description.ContractDescription> или <xref:System.ServiceModel.Channels.BindingElement> и последующего удаления утверждений политики из соответствующего экземпляра <xref:System.ServiceModel.Description.PolicyAssertionCollection>.  
  
 Атрибут `wsp:Optional` и вложенные выражения политики не нормализуются, поэтому обработка этих конструкций политики выполняется с помощью расширений импорта политики.Кроме того, возможен неоднократный вызов расширений импорта политики с одними и теми же объектами <xref:System.ServiceModel.Description.ContractDescription> и <xref:System.ServiceModel.Channels.BindingElement>, поэтому расширения импорта политики должны быть надежными в такой ситуации.  
  
> [!IMPORTANT]
>  В средство импорта могут быть переданы недопустимые или неправильные метаданные.Убедитесь в том, что пользовательские средства импорта являются надежными для всех форм XML.  
  
## См. также  
 [Практическое руководство. Импорт пользовательской информации WSDL](../../../../docs/framework/wcf/extending/how-to-import-custom-wsdl.md)   
 [Как импортировать утверждения пользовательской политики](../../../../docs/framework/wcf/extending/how-to-import-custom-policy-assertions.md)   
 [Как разработать расширения для ServiceContractGenerator](../../../../docs/framework/wcf/extending/how-to-write-an-extension-for-the-servicecontractgenerator.md)