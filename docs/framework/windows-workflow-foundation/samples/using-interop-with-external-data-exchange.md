---
title: "Использование Interop для обмена с внешними данными | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 96f6fe26-5305-494f-9119-7748e0c4b3fa
caps.latest.revision: 6
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 6
---
# Использование Interop для обмена с внешними данными
Действие <xref:System.Activities.Statements.Interop> можно использовать для выполнения действий из [!INCLUDE[wf](../../../../includes/wf-md.md)] в [!INCLUDE[vstecwinfx](../../../../includes/vstecwinfx-md.md)] и [!INCLUDE[netfx35_long](../../../../includes/netfx35-long-md.md)] \(WF3\), а также рабочих процессов в [!INCLUDE[wf2](../../../../includes/wf2-md.md)] в [!INCLUDE[netfx_current_long](../../../../includes/netfx-current-long-md.md)] \(WF4\).В этом образце описывается настройка и выполнение рабочего процесса WF3, использующего <xref:System.Workflow.Activities.ExternalDataExchangeService> \(и соответствующих настраиваемых действий для вызова методов и обработки событий\) с помощью действия <xref:System.Activities.Statements.Interop> в службе рабочего процесса WF4.  
  
> [!IMPORTANT]
>  Образцы уже могут быть установлены на компьютере.Перед продолжением проверьте следующий каталог \(по умолчанию\).  
>   
>  `<диск_установки>:\WF_WCF_Samples`  
>   
>  Если этот каталог не существует, перейдите на страницу [Образцы Windows Communication Foundation \(WCF\) и Windows Workflow Foundation \(WF\) для .NET Framework 4](http://go.microsoft.com/fwlink/?LinkId=150780), чтобы загрузить все образцы [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] и [!INCLUDE[wf1](../../../../includes/wf1-md.md)].Этот образец расположен в следующем каталоге.  
>   
>  `<диск_установки>:\WF_WCF_Samples\WF\Basic\Migration\ExternalDataExchange`  
  
#### Использование этого образца  
  
1.  Откройте файл ExternalDataExchange.sln в среде [!INCLUDE[vs2010](../../../../includes/vs2010-md.md)].  
  
2.  Для построения образца нажмите CTRL\+SHIFT\+B.  
  
3.  Нажмите клавишу F5, чтобы запустить образец.