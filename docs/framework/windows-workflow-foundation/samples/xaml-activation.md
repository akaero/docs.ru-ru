---
title: "Активация XAML | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 486760e2-bb10-4ed5-8c02-fe7472498d2d
caps.latest.revision: 11
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 11
---
# Активация XAML
В этом образце показано, как разместить декларативное бизнес\-правило в IIS.Образец представляет базовый рабочий процесс с именем `EchoService`, в котором имеется одна операция.  
  
> [!IMPORTANT]
>  Образцы уже могут быть установлены на компьютере.Перед продолжением проверьте следующий каталог \(по умолчанию\).  
>   
>  `<диск_установки>:\WF_WCF_Samples`  
>   
>  Если этот каталог не существует, перейдите на страницу загрузки, чтобы загрузить все образцы [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] и [!INCLUDE[wf1](../../../../includes/wf1-md.md)].Этот образец расположен в следующем каталоге.  
>   
>  `<диск_установки>:\WF_WCF_Samples\WF\Basic\Services\XAMLActivation`  
  
#### Настройка, построение и выполнение образца  
  
1.  Откройте решение XAMLActivation.sln в [!INCLUDE[vs2010](../../../../includes/vs2010-md.md)].  
  
2.  Постройте решение, нажав клавишу **F5**.  
  
3.  Запустите файл WcfTestClient.exe.Введите в командной строке следующее:  
  
    1.  cd %SystemDrive%\\Program Files\\Microsoft Visual Studio 10.0\\Common7\\IDE  
  
    2.  Запустите файл WcfTestClient.exe.  
  
4.  Установите адрес службы в WcfTestClient.exe, нажав CTRL\+SHIFT\+A и установив для адреса службы значение http:\/\/localhost:56133\/Service.xamlx.  
  
5.  Произведите операцию Echo для проверки службы.  
  
6.  Разверните службу в IIS, используя файл DeployToIIS.Bat из командной строки с правами администратора.  
  
7.  Обновите адрес службы в клиенте на http:\/\/localhost\/XAMLActivation\/Service.xamlx и снова проверьте службу, используя WcfTestClient.exe.