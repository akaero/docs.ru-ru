---
title: "Практическое руководство. Экспорт метаданных из конечных точек службы | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: b6c4dfd0-f270-43ec-961a-e16eb6af2f2c
caps.latest.revision: 14
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 14
---
# Практическое руководство. Экспорт метаданных из конечных точек службы
В этом разделе объясняется, как экспортировать метаданные из конечных точек службы.  
  
### Экспорт метаданных из конечных точек службы  
  
1.  Создайте новый проект "Консольное приложение" в Visual Studio.  Добавьте в созданный внутри метода main\(\) файл Program.cs приведенный в описании следующих шагов код.  
  
2.  Создайте таблицу <xref:System.ServiceModel.Description.WsdlExporter>.  
  
     [!code-csharp[S_UEWsdlExporter#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_uewsdlexporter/cs/program.cs#1)]
     [!code-vb[S_UEWsdlExporter#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_uewsdlexporter/vb/program.vb#1)]  
  
3.  Присвойте свойству <xref:System.ServiceModel.Description.MetadataExporter.PolicyVersion%2A> одно из значений из перечисления <xref:System.ServiceModel.Description.PolicyVersion>.  В этом примере свойству задается значение <xref:System.ServiceModel.Description.PolicyVersion.Policy15%2A>, что соответствует WS\-Policy 1.5.  
  
     [!code-csharp[S_UEWsdlExporter#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_uewsdlexporter/cs/program.cs#2)]
     [!code-vb[S_UEWsdlExporter#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_uewsdlexporter/vb/program.vb#2)]  
  
4.  Создайте массив объектов <xref:System.ServiceModel.Description.ServiceEndpoint>.  
  
     [!code-csharp[S_UEWsdlExporter#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_uewsdlexporter/cs/program.cs#3)]
     [!code-vb[S_UEWsdlExporter#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_uewsdlexporter/vb/program.vb#3)]  
  
5.  Экспортируйте метаданные для каждой конечной точки службы.  
  
     [!code-csharp[S_UEWsdlExporter#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_uewsdlexporter/cs/program.cs#4)]
     [!code-vb[S_UEWsdlExporter#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_uewsdlexporter/vb/program.vb#4)]  
  
6.  Убедитесь, что в процессе экспорта не произошло ошибок, и извлеките метаданные.  
  
     [!code-csharp[S_UEWsdlExporter#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_uewsdlexporter/cs/program.cs#5)]
     [!code-vb[S_UEWsdlExporter#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_uewsdlexporter/vb/program.vb#5)]  
  
7.  После этого можно использовать метаданные, например записать их в файл, вызвав метод <xref:System.ServiceModel.Description.MetadataSet.WriteTo%28System.Xml.XmlWriter%29>.  
  
## Пример  
 Ниже приведен полный код этого примера.  
  
 [!code-csharp[S_UEWsdlExporter#0](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_uewsdlexporter/cs/program.cs#0)]
 [!code-vb[S_UEWsdlExporter#0](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_uewsdlexporter/vb/program.vb#0)]  
  
## Компиляция кода  
 При компиляции файла Program.cs необходимо сослаться на System.ServiceModel.dll.  
  
## См. также  
 [Общие сведения об архитектуре метаданных](../../../../docs/framework/wcf/feature-details/metadata-architecture-overview.md)   
 [Использование метаданных](../../../../docs/framework/wcf/feature-details/using-metadata.md)   
 [Конечные точки: адреса, привязки и контракты](../../../../docs/framework/wcf/feature-details/endpoints-addresses-bindings-and-contracts.md)