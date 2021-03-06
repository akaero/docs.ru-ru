---
title: "Взаимодействие с приложениями POX | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 449276b8-4633-46f0-85c9-81f01d127636
caps.latest.revision: 15
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 15
---
# Взаимодействие с приложениями POX
Приложения типа «Plain Old XML» \(POX\) обмениваются простыми HTTP\-сообщениями, содержащими только XML\-данные приложения, не заключенные в конверт SOAP.[!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] может предоставлять как службы, так и клиенты, использующие сообщения POX.В случае службы [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] может использоваться для реализации служб, предоставляющих конечные точки таким клиентам, как веб\-браузеры и языки сценариев, которые передают и принимают сообщения POX.В случае клиента модель программирования [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] может использоваться для реализации клиентов, взаимодействующих со службами, основанными на POX.  
  
> [!NOTE]
>  Данный документ первоначально был написан для [!INCLUDE[dnprdnshort](../../../../includes/dnprdnshort-md.md)] 3.0.В [!INCLUDE[dnprdnshort](../../../../includes/dnprdnshort-md.md)] версии 3.5 имеется встроенная поддержка приложений POX.[!INCLUDE[crabout](../../../../includes/crabout-md.md)] см. в разделе [Модель веб\-программирования HTTP WCF](../../../../docs/framework/wcf/feature-details/wcf-web-http-programming-model.md)  
  
## Программирование POX с использованием WCF  
 В службах [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)], взаимодействующих по HTTP с помощью сообщений POX, используется элемент [\<customBinding\>](../../../../docs/framework/configure-apps/file-schema/wcf/custombinding.md).  
  
```  
<customBinding>  
   <binding name="poxServerBinding">  
       <textMessageEncoding messageVersion="None" />  
       <httpTransport />  
   </binding>  
</customBinding>  
  
```  
  
 Эта пользовательская привязка содержит два элемента:  
  
-   [\<httpTransport\>](../../../../docs/framework/configure-apps/file-schema/wcf/httptransport.md) и  
  
-   [\<textMessageEncoding\>](../../../../docs/framework/configure-apps/file-schema/wcf/textmessageencoding.md).  
  
 Стандартный кодировщик текстовых сообщений [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] специально настроен для использования значения <xref:System.ServiceModel.Channels.MessageVersion.None%2A>, которое позволяет обрабатывать полезную нагрузку сообщений XML, поступающую не внутри конвертов SOAP.  
  
 Клиенты [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)], взаимодействующие по протоколу HTTP с помощью сообщений POX, используют аналогичную привязку \(показанную в приведенном ниже императивном коде\).  
  
```  
private static Binding CreatePoxBinding()  
{  
    TextMessageEncodingBindingElement encoder =   
    new TextMessageEncodingBindingElement( MessageVersion.None, Encoding.UTF8 );  
    HttpTransportBindingElement transport = new HttpTransportBindingElement();  
    transport.ManualAddressing = true;  
    return new CustomBinding( new BindingElement[] { encoder, transport } );  
}   
```  
  
 Так как клиенты POX должны явно задавать универсальные коды ресурса \(URI\), которым они отправляют сообщения, им обычно требуется настроить для элемента <xref:System.ServiceModel.Channels.HttpTransportBindingElement> режим ручной адресации, задав в этом элементе для свойства <xref:System.ServiceModel.Channels.TransportBindingElement.ManualAddressing%2A> значение `true`.Это обеспечивает явную адресацию сообщений в коде приложения, исключая необходимость создавать новую фабрику <xref:System.ServiceModel.ChannelFactory> каждый раз, когда приложение передает сообщение другому универсальному коду ресурса \(URI\) протокола HTTP.  
  
 Так как сообщения POX не используют заголовки SOAP для передачи важной информации о протоколе, клиенты и службы POX часто должны обрабатывать части базового запроса HTTP, использованного для отправки или приема сообщения.Специфичная для HTTP информация о протоколе, такая как заголовки HTTP и коды состояния, отображается в модели программирования [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] с помощью двух классов.  
  
-   <xref:System.ServiceModel.Channels.HttpRequestMessageProperty>, содержащий информацию о запросе HTTP, такую как метод HTTP и заголовки запроса.  
  
-   <xref:System.ServiceModel.Channels.HttpResponseMessageProperty>, содержащий информацию об ответе HTTP, такую как код состояния и описание состояния HTTP, а также все заголовки ответа HTTP.  
  
 В следующем примере кода показано, как создать сообщения запроса HTTP GET, направляемое по адресу http:\/\/localhost:8100\/customers.  
  
```  
Message request = Message.CreateMessage( MessageVersion.None, String.Empty );  
request.Headers.To = “http://localhost:8100/customers”;  
  
HttpRequestMessageProperty property = new HttpRequestMessageProperty();  
property.Method = “GET”;  
property.SuppressEntityBody = true;  
request.Properties.Add( HttpRequestMessageProperty.Name, property );  
```  
  
 Сначала создается пустой запрос <xref:System.ServiceModel.Channels.Message> путем вызова метода <xref:System.ServiceModel.Channels.Message.CreateMessage%28System.ServiceModel.Channels.MessageVersion%2CSystem.String%29>.Параметр <xref:System.ServiceModel.Channels.MessageVersion.None%2A> используется для указания того, что конверт SOAP не требуется, а параметр <xref:System.String.Empty> передается как действие.Затем сообщение запроса адресуется путем задания в заголовке <xref:System.ServiceModel.Channels.MessageHeaders.To%2A> требуемого универсального кода ресурса \(URI\).Далее создается <xref:System.ServiceModel.Channels.HttpRequestMessageProperty>, для параметра <xref:System.ServiceModel.Channels.HttpRequestMessageProperty.Method%2A> задается HTTP\-команда GET, а для параметра <xref:System.ServiceModel.Channels.HttpRequestMessageProperty.SuppressEntityBody%2A> задается значение `true`, указывающее, что в теле исходящего сообщения запроса HTTP не должны передаваться никакие данные.Наконец, в коллекцию <xref:System.ServiceModel.Channels.Message.Properties%2A> сообщения запроса добавляется свойство запроса, чтобы можно было влиять на способ передачи запроса транспортом HTTP.После этого сообщение готово к отправке через соответствующий экземпляр класса <xref:System.ServiceModel.Channels.IRequestChannel>.  
  
 Аналогичные приемы могут использоваться в службе для извлечения свойства <xref:System.ServiceModel.Channels.HttpRequestMessageProperty> из входящего сообщения и построения ответа.