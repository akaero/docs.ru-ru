---
title: "Как реализовать прокси-сервера обнаружения | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 78d70e0a-f6c3-4cfb-a7ca-f66ebddadde0
caps.latest.revision: 19
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 19
---
# Как реализовать прокси-сервера обнаружения
В этом разделе приведены сведения о реализации прокси\-сервера обнаружения.[!INCLUDE[crabout](../../../../includes/crabout-md.md)] функции обнаружения в [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] см. в разделе [Общие сведения об обнаружении WCF](../../../../docs/framework/wcf/feature-details/wcf-discovery-overview.md).Прокси\-сервер обнаружения реализуется созданием класса, расширяющего абстрактный класс <xref:System.ServiceModel.Discovery.DiscoveryProxy>.В этом образце определены и использованы несколько других вспомогательных классов.`OnResolveAsyncResult`, `OnFindAsyncResult` и `AsyncResult`.Эти классы реализуют интерфейс <xref:System.IAsyncResult>.[!INCLUDE[crabout](../../../../includes/crabout-md.md)]<xref:System.IAsyncResult> см. в разделе [Интерфейс System.IAsyncResult](http://go.microsoft.com/fwlink/?LinkId=128519)  
  
 В данном разделе реализация прокси\-сервера обнаружения разделена на три основные части.  
  
-   Определение класса, который содержит хранилище данных и расширяет абстрактный класс <xref:System.ServiceModel.Discovery.DiscoveryProxy>.  
  
-   Реализация вспомогательного класса `AsyncResult`.  
  
-   Размещение прокси\-сервера обнаружения.  
  
### Создание нового проекта консольного приложения  
  
1.  Запустите [!INCLUDE[vs_current_long](../../../../includes/vs-current-long-md.md)].  
  
2.  Создайте новый проект консольного приложения.Задайте имя `DiscoveryProxy` для проекта и имя `DiscoveryProxyExample` для решения.  
  
3.  Добавьте в проект следующие ссылки  
  
    1.  System.ServiceModel.dll  
  
    2.  System.Servicemodel.Discovery.dll  
  
    > [!CAUTION]
    >  Ссылки должны указывать на версию этих сборок 4.0 или выше.  
  
### Реализация класса ProxyDiscoveryService  
  
1.  Добавьте новый файл кода в проект и назовите его DiscoveryProxy.cs.  
  
2.  Добавьте следующие операторы `using` в файл DiscoveryProxy.cs.  
  
    ```  
    using System;  
    using System.Collections.Generic;  
    using System.ServiceModel;  
    using System.ServiceModel.Discovery;  
    using System.Xml;  
    ```  
  
3.  Создайте класс `DiscoveryProxyService`, производный от <xref:System.ServiceModel.Discovery.DiscoveryProxy>.Примените атрибут `ServiceBehavior` к классу, как показано в следующем примере.  
  
    ```  
    // Implement DiscoveryProxy by extending the DiscoveryProxy class and overriding the abstract methods  
    [ServiceBehavior(InstanceContextMode = InstanceContextMode.Single, ConcurrencyMode = ConcurrencyMode.Multiple)]  
    public class DiscoveryProxyService : DiscoveryProxy  
    {  
    }  
  
    ```  
  
4.  Определите в пределах класса `DiscoveryProxy` словарь для хранения зарегистрированных служб.  
  
    ```  
    // Repository to store EndpointDiscoveryMetadata.   
    Dictionary<EndpointAddress, EndpointDiscoveryMetadata> onlineServices;  
  
    ```  
  
5.  Определите конструктор, который инициализирует словарь.  
  
    ```  
    public DiscoveryProxyService()  
            {  
                this.onlineServices = new Dictionary<EndpointAddress, EndpointDiscoveryMetadata>();  
            }  
  
    ```  
  
### Определение методов для обновления кэша прокси\-сервера обнаружения  
  
1.  Реализуйте метод `AddOnlineservice` для добавления служб в кэш.Он вызывается каждый раз, когда прокси\-сервер получает сообщение объявления.  
  
    ```  
    void AddOnlineService(EndpointDiscoveryMetadata endpointDiscoveryMetadata)  
            {  
                lock (this.onlineServices)  
                {  
                    this.onlineServices[endpointDiscoveryMetadata.Address] = endpointDiscoveryMetadata;                  
                }  
  
                PrintDiscoveryMetadata(endpointDiscoveryMetadata, "Adding");  
            }  
  
    ```  
  
2.  Реализуйте метод `RemoveOnlineService`, который используется для удаления служб из кэша.  
  
    ```  
    void RemoveOnlineService(EndpointDiscoveryMetadata endpointDiscoveryMetadata)  
            {  
                if (endpointDiscoveryMetadata != null)  
                {  
                    lock (this.onlineServices)  
                    {  
                        this.onlineServices.Remove(endpointDiscoveryMetadata.Address);                      
                    }  
  
                    PrintDiscoveryMetadata(endpointDiscoveryMetadata, "Removing");  
                }      
            }  
  
    ```  
  
3.  Реализуйте методы `MatchFromOnlineService`, которые выполняют сопоставление службы со службой из словаря.  
  
    ```  
    void MatchFromOnlineService(FindRequestContext findRequestContext)  
            {  
                lock (this.onlineServices)  
                {  
                    foreach (EndpointDiscoveryMetadata endpointDiscoveryMetadata in this.onlineServices.Values)  
                    {  
                        if (findRequestContext.Criteria.IsMatch(endpointDiscoveryMetadata))  
                        {  
                            findRequestContext.AddMatchingEndpoint(endpointDiscoveryMetadata);  
                        }  
                    }  
                }  
            }  
  
    ```  
  
    ```  
    EndpointDiscoveryMetadata MatchFromOnlineService(ResolveCriteria criteria)  
            {  
                EndpointDiscoveryMetadata matchingEndpoint = null;  
                lock (this.onlineServices)  
                {  
                    foreach (EndpointDiscoveryMetadata endpointDiscoveryMetadata in this.onlineServices.Values)  
                    {  
                        if (criteria.Address == endpointDiscoveryMetadata.Address)  
                        {  
                            matchingEndpoint = endpointDiscoveryMetadata;  
                        }  
                    }  
                }  
                return matchingEndpoint;  
            }  
  
    ```  
  
4.  Реализуйте метод `PrintDiscoveryMetadata`, который выводит пользователю на консоль текстовые данные об операциях прокси\-сервера обнаружения.  
  
    ```  
    void PrintDiscoveryMetadata(EndpointDiscoveryMetadata endpointDiscoveryMetadata, string verb)  
            {  
                Console.WriteLine("\n**** " + verb + " service of the following type from cache. ");  
                foreach (XmlQualifiedName contractName in endpointDiscoveryMetadata.ContractTypeNames)  
                {  
                    Console.WriteLine("** " + contractName.ToString());  
                    break;  
                }  
                Console.WriteLine("**** Operation Completed");  
            }  
  
    ```  
  
5.  Добавьте следующие классы AsyncResult в DiscoveryProxyService.Эти классы позволяют различать результаты асинхронных операций.  
  
    ```  
    sealed class OnOnlineAnnouncementAsyncResult : AsyncResult  
            {  
                public OnOnlineAnnouncementAsyncResult(AsyncCallback callback, object state)  
                    : base(callback, state)  
                {  
                    this.Complete(true);  
                }  
  
                public static void End(IAsyncResult result)  
                {  
                    AsyncResult.End<OnOnlineAnnouncementAsyncResult>(result);  
                }  
            }  
  
            sealed class OnOfflineAnnouncementAsyncResult : AsyncResult  
            {  
                public OnOfflineAnnouncementAsyncResult(AsyncCallback callback, object state)  
                    : base(callback, state)  
                {  
                    this.Complete(true);  
                }  
  
                public static void End(IAsyncResult result)  
                {  
                    AsyncResult.End<OnOfflineAnnouncementAsyncResult>(result);  
                }  
            }  
  
            sealed class OnFindAsyncResult : AsyncResult  
            {  
                public OnFindAsyncResult(AsyncCallback callback, object state)  
                    : base(callback, state)  
                {  
                    this.Complete(true);  
                }  
  
                public static void End(IAsyncResult result)  
                {  
                    AsyncResult.End<OnFindAsyncResult>(result);  
                }  
            }  
  
            sealed class OnResolveAsyncResult : AsyncResult  
            {  
                EndpointDiscoveryMetadata matchingEndpoint;  
  
                public OnResolveAsyncResult(EndpointDiscoveryMetadata matchingEndpoint, AsyncCallback callback, object state)  
                    : base(callback, state)  
                {  
                    this.matchingEndpoint = matchingEndpoint;  
                    this.Complete(true);  
                }  
  
                public static EndpointDiscoveryMetadata End(IAsyncResult result)  
                {  
                    OnResolveAsyncResult thisPtr = AsyncResult.End<OnResolveAsyncResult>(result);  
                    return thisPtr.matchingEndpoint;  
                }  
            }  
  
    ```  
  
### Определение методов, реализующих функции прокси\-сервера обнаружения  
  
1.  Переопределите метод <xref:System.ServiceModel.Discovery.DiscoveryProxyBase.OnBeginOnlineAnnouncement%2A>.Этот метод вызывается, когда прокси\-сервер обнаружения получает оперативное сообщение объявления.  
  
    ```  
    // OnBeginOnlineAnnouncement method is called when a Hello message is received by the Proxy  
            protected override IAsyncResult OnBeginOnlineAnnouncement(DiscoveryMessageSequence messageSequence, EndpointDiscoveryMetadata endpointDiscoveryMetadata, AsyncCallback callback, object state)  
            {          
                this.AddOnlineService(endpointDiscoveryMetadata);  
                return new OnOnlineAnnouncementAsyncResult(callback, state);  
            }  
  
    ```  
  
2.  Переопределите метод <xref:System.ServiceModel.Discovery.DiscoveryProxyBase.OnEndOnlineAnnouncement%2A>.Этот метод вызывается, когда прокси\-сервер обнаружения завершает обработку сообщения объявления.  
  
    ```  
    protected override void OnEndOnlineAnnouncement(IAsyncResult result)  
            {  
                OnOnlineAnnouncementAsyncResult.End(result);  
            }  
  
    ```  
  
3.  Переопределите метод <xref:System.ServiceModel.Discovery.DiscoveryProxyBase.OnBeginOfflineAnnouncement%2A>.Этот метод вызывается вместе с получением прокси\-сервером обнаружения автономного сообщение объявления.  
  
    ```  
    // OnBeginOfflineAnnouncement method is called when a Bye message is received by the Proxy  
            protected override IAsyncResult OnBeginOfflineAnnouncement(DiscoveryMessageSequence messageSequence, EndpointDiscoveryMetadata endpointDiscoveryMetadata, AsyncCallback callback, object state)  
            {  
                this.RemoveOnlineService(endpointDiscoveryMetadata);  
                return new OnOfflineAnnouncementAsyncResult(callback, state);  
            }  
  
    ```  
  
4.  Переопределите метод <xref:System.ServiceModel.Discovery.DiscoveryProxyBase.OnEndOfflineAnnouncement%2A>.Этот метод вызывается, когда прокси\-сервер обнаружения завершает обработку автономного сообщения объявления.  
  
    ```  
    protected override void OnEndOfflineAnnouncement(IAsyncResult result)  
            {  
                OnOfflineAnnouncementAsyncResult.End(result);  
            }  
  
    ```  
  
5.  Переопределите метод <xref:System.ServiceModel.Discovery.DiscoveryProxyBase.OnBeginFind%2A>.Этот метод вызывается, когда прокси\-сервер обнаружения получает запрос поиска.  
  
    ```  
    // OnBeginFind method is called when a Probe request message is received by the Proxy  
            protected override IAsyncResult OnBeginFind(FindRequestContext findRequestContext, AsyncCallback callback, object state)  
            {  
                this.MatchFromOnlineService(findRequestContext);  
                return new OnFindAsyncResult(callback, state);  
            }  
    protected override IAsyncResult OnBeginFind(FindRequest findRequest, AsyncCallback callback, object state)  
    {  
        Collection<EndpointDiscoveryMetadata> matchingEndpoints = MatchFromCache(findRequest.Criteria);  
        return new OnFindAsyncResult(  
                    matchingEndpoints,  
                    callback,  
                    state);  
    }  
    ```  
  
6.  Переопределите метод <xref:System.ServiceModel.Discovery.DiscoveryProxyBase.OnEndFind%2A>.Этот метод вызывается, когда прокси\-сервер обнаружения завершает обработку запроса поиска.  
  
    ```  
    protected override void OnEndFind(IAsyncResult result)  
            {  
                OnFindAsyncResult.End(result);  
            }  
  
    ```  
  
7.  Переопределите метод <xref:System.ServiceModel.Discovery.DiscoveryProxyBase.OnBeginResolve%2A>.Этот метод вызывается, когда прокси\-сервер обнаружения получает сообщение разрешения.  
  
    ```  
    // OnBeginFind method is called when a Resolve request message is received by the Proxy  
            protected override IAsyncResult OnBeginResolve(ResolveCriteria resolveCriteria, AsyncCallback callback, object state)  
            {  
                return new OnResolveAsyncResult(this.MatchFromOnlineService(resolveCriteria), callback, state);  
            }  
    protected override IAsyncResult OnBeginResolve(ResolveRequest resolveRequest, AsyncCallback callback, object state)  
    {  
        return new OnResolveAsyncResult(  
            this.proxy.MatchFromOnlineService(resolveRequest.Criteria),  
            callback,  
            state);  
    }  
    ```  
  
8.  Переопределить метод <xref:System.ServiceModel.Discovery.DiscoveryProxyBase.OnEndResolve%2A>.Этот метод вызывается, когда прокси\-сервер обнаружения завершает обработку сообщения разрешения.  
  
    ```  
    protected override EndpointDiscoveryMetadata OnEndResolve(IAsyncResult result)  
    {  
        return OnResolveAsyncResult.End(result);  
    }  
  
    ```  
  
 Методы OnBegin..\/ OnEnd..методы предоставляют логику для последующих операций обнаружения.Например, методы <xref:System.ServiceModel.Discovery.DiscoveryProxyBase.OnBeginFind%2A> и <xref:System.ServiceModel.Discovery.DiscoveryProxyBase.OnEndFind%2A> реализуют для прокси\-сервера обнаружения логику поиска.Когда прокси\-сервер обнаружения получает сообщение зонда, эти методы вызываются для отправки ответа клиенту.При необходимости логику поиска можно изменить. Например, можно включить в состав операции поиска поиск по пользовательской области путем анализа XML\-метаданных, определяемых алгоритмами или приложениями.  
  
### Реализация класса AsyncResult  
  
1.  Определите абстрактный базовый класс AsyncResult. Различные классы асинхронных результатов будут производными от него.  
  
2.  Создайте новый файл кода с именем AsyncResult.cs.  
  
3.  Добавьте в файл AsyncResult.cs следующие операторы `using`.  
  
    ```  
    using System;  
    using System.Threading;  
  
    ```  
  
4.  Добавьте следующий класс AsyncResult.  
  
    ```  
    abstract class AsyncResult : IAsyncResult  
        {  
            AsyncCallback callback;  
            bool completedSynchronously;  
            bool endCalled;  
            Exception exception;  
            bool isCompleted;  
            ManualResetEvent manualResetEvent;  
            object state;  
            object thisLock;  
  
            protected AsyncResult(AsyncCallback callback, object state)  
            {  
                this.callback = callback;  
                this.state = state;  
                this.thisLock = new object();  
            }  
  
            public object AsyncState  
            {  
                get  
                {  
                    return state;  
                }  
            }  
  
            public WaitHandle AsyncWaitHandle  
            {  
                get  
                {  
                    if (manualResetEvent != null)  
                    {  
                        return manualResetEvent;  
                    }  
                    lock (ThisLock)  
                    {  
                        if (manualResetEvent == null)  
                        {  
                            manualResetEvent = new ManualResetEvent(isCompleted);  
                        }  
                    }  
                    return manualResetEvent;  
                }  
            }  
  
            public bool CompletedSynchronously  
            {  
                get  
                {  
                    return completedSynchronously;  
                }  
            }  
  
            public bool IsCompleted  
            {  
                get  
                {  
                    return isCompleted;  
                }  
            }  
  
            object ThisLock  
            {  
                get  
                {  
                    return this.thisLock;  
                }  
            }  
  
            protected static TAsyncResult End<TAsyncResult>(IAsyncResult result)  
                where TAsyncResult : AsyncResult  
            {  
                if (result == null)  
                {  
                    throw new ArgumentNullException("result");  
                }  
  
                TAsyncResult asyncResult = result as TAsyncResult;  
  
                if (asyncResult == null)  
                {  
                    throw new ArgumentException("Invalid async result.", "result");  
                }  
  
                if (asyncResult.endCalled)  
                {  
                    throw new InvalidOperationException("Async object already ended.");  
                }  
  
                asyncResult.endCalled = true;  
  
                if (!asyncResult.isCompleted)  
                {  
                    asyncResult.AsyncWaitHandle.WaitOne();  
                }  
  
                if (asyncResult.manualResetEvent != null)  
                {  
                    asyncResult.manualResetEvent.Close();  
                }  
  
                if (asyncResult.exception != null)  
                {  
                    throw asyncResult.exception;  
                }  
  
                return asyncResult;  
            }  
  
            protected void Complete(bool completedSynchronously)  
            {  
                if (isCompleted)  
                {  
                    throw new InvalidOperationException("This async result is already completed.");  
                }  
  
                this.completedSynchronously = completedSynchronously;  
  
                if (completedSynchronously)  
                {  
                    this.isCompleted = true;  
                }  
                else  
                {  
                    lock (ThisLock)  
                    {  
                        this.isCompleted = true;  
                        if (this.manualResetEvent != null)  
                        {  
                            this.manualResetEvent.Set();  
                        }  
                    }  
                }  
  
                if (callback != null)  
                {  
                    callback(this);  
                }  
            }  
  
            protected void Complete(bool completedSynchronously, Exception exception)  
            {  
                this.exception = exception;  
                Complete(completedSynchronously);  
            }  
        }  
  
    ```  
  
### Размещение прокси\-сервера обнаружения  
  
1.  Откройте файл Program.cs в проекте DiscoveryProxyExample.  
  
2.  Добавьте следующие инструкции `using`.  
  
    ```  
    using System;  
    using System.ServiceModel;  
    using System.ServiceModel.Discovery;  
    ```  
  
3.  В метод `Main()` добавьте следующий код.Он создает экземпляр класса `DiscoveryProxy`.  
  
    ```  
    Uri probeEndpointAddress = new Uri("net.tcp://localhost:8001/Probe");  
                Uri announcementEndpointAddress = new Uri("net.tcp://localhost:9021/Announcement");  
  
                // Host the DiscoveryProxy service  
                ServiceHost proxyServiceHost = new ServiceHost(new DiscoveryProxyService());  
  
    ```  
  
4.  Затем добавьте следующий код, который добавляет конечную точку обнаружения и конечную точку объявления.  
  
    ```  
    try  
              {                  
                  // Add DiscoveryEndpoint to receive Probe and Resolve messages  
                  DiscoveryEndpoint discoveryEndpoint = new DiscoveryEndpoint(new NetTcpBinding(), new EndpointAddress(probeEndpointAddress));  
                  discoveryEndpoint.IsSystemEndpoint = false;  
  
                  // Add AnnouncementEndpoint to receive Hello and Bye announcement messages  
                  AnnouncementEndpoint announcementEndpoint = new AnnouncementEndpoint(new NetTcpBinding(), new EndpointAddress(announcementEndpointAddress));                  
  
                  proxyServiceHost.AddServiceEndpoint(discoveryEndpoint);  
                  proxyServiceHost.AddServiceEndpoint(announcementEndpoint);  
  
                  proxyServiceHost.Open();  
  
                  Console.WriteLine("Proxy Service started.");  
                  Console.WriteLine();  
                  Console.WriteLine("Press <ENTER> to terminate the service.");  
                  Console.WriteLine();  
                  Console.ReadLine();  
  
                  proxyServiceHost.Close();  
              }  
              catch (CommunicationException e)  
              {  
                  Console.WriteLine(e.Message);  
              }  
              catch (TimeoutException e)  
              {  
                  Console.WriteLine(e.Message);  
              }     
  
              if (proxyServiceHost.State != CommunicationState.Closed)  
              {  
                  Console.WriteLine("Aborting the service...");  
                  proxyServiceHost.Abort();  
              }  
  
    ```  
  
 Реализация прокси\-сервера обнаружения завершена.Продолжение см. в разделе [Как реализовать обнаружимую службу, которая регистрируется в прокси\-сервере обнаружения](../../../../docs/framework/wcf/feature-details/discoverable-service-that-registers-with-the-discovery-proxy.md).  
  
## Пример  
 Далее приведен полный код, используемый в этом подразделе.  
  
```  
// DiscoveryProxy.cs  
//----------------------------------------------------------------  
// Copyright (c) Microsoft Corporation.  All rights reserved.  
//----------------------------------------------------------------  
  
using System;  
using System.Collections.Generic;  
using System.ServiceModel;  
using System.ServiceModel.Discovery;  
using System.Xml;  
  
namespace Microsoft.Samples.Discovery  
{  
    // Implement DiscoveryProxy by extending the DiscoveryProxy class and overriding the abstract methods  
    [ServiceBehavior(InstanceContextMode = InstanceContextMode.Single, ConcurrencyMode = ConcurrencyMode.Multiple)]  
    public class DiscoveryProxyService : DiscoveryProxy  
    {  
        // Repository to store EndpointDiscoveryMetadata. A database or a flat file could also be used instead.  
        Dictionary<EndpointAddress, EndpointDiscoveryMetadata> onlineServices;  
  
        public DiscoveryProxyService()  
        {  
            this.onlineServices = new Dictionary<EndpointAddress, EndpointDiscoveryMetadata>();  
        }  
  
        // OnBeginOnlineAnnouncement method is called when a Hello message is received by the Proxy  
        protected override IAsyncResult OnBeginOnlineAnnouncement(DiscoveryMessageSequence messageSequence, EndpointDiscoveryMetadata endpointDiscoveryMetadata, AsyncCallback callback, object state)  
        {          
            this.AddOnlineService(endpointDiscoveryMetadata);  
            return new OnOnlineAnnouncementAsyncResult(callback, state);  
        }  
  
        protected override void OnEndOnlineAnnouncement(IAsyncResult result)  
        {  
            OnOnlineAnnouncementAsyncResult.End(result);  
        }  
  
        // OnBeginOfflineAnnouncement method is called when a Bye message is received by the Proxy  
        protected override IAsyncResult OnBeginOfflineAnnouncement(DiscoveryMessageSequence messageSequence, EndpointDiscoveryMetadata endpointDiscoveryMetadata, AsyncCallback callback, object state)  
        {  
            this.RemoveOnlineService(endpointDiscoveryMetadata);  
            return new OnOfflineAnnouncementAsyncResult(callback, state);  
        }  
  
        protected override void OnEndOfflineAnnouncement(IAsyncResult result)  
        {  
            OnOfflineAnnouncementAsyncResult.End(result);  
        }  
  
        // OnBeginFind method is called when a Probe request message is received by the Proxy  
        protected override IAsyncResult OnBeginFind(FindRequestContext findRequestContext, AsyncCallback callback, object state)  
        {  
            this.MatchFromOnlineService(findRequestContext);  
            return new OnFindAsyncResult(callback, state);  
        }  
  
        protected override void OnEndFind(IAsyncResult result)  
        {  
            OnFindAsyncResult.End(result);  
        }  
  
        // OnBeginFind method is called when a Resolve request message is received by the Proxy  
        protected override IAsyncResult OnBeginResolve(ResolveCriteria resolveCriteria, AsyncCallback callback, object state)  
        {  
            return new OnResolveAsyncResult(this.MatchFromOnlineService(resolveCriteria), callback, state);  
        }  
  
        protected override EndpointDiscoveryMetadata OnEndResolve(IAsyncResult result)  
        {  
            return OnResolveAsyncResult.End(result);  
        }  
  
        // The following are helper methods required by the Proxy implementation  
        void AddOnlineService(EndpointDiscoveryMetadata endpointDiscoveryMetadata)  
        {  
            lock (this.onlineServices)  
            {  
                this.onlineServices[endpointDiscoveryMetadata.Address] = endpointDiscoveryMetadata;                  
            }  
  
            PrintDiscoveryMetadata(endpointDiscoveryMetadata, "Adding");  
        }  
  
        void RemoveOnlineService(EndpointDiscoveryMetadata endpointDiscoveryMetadata)  
        {  
            if (endpointDiscoveryMetadata != null)  
            {  
                lock (this.onlineServices)  
                {  
                    this.onlineServices.Remove(endpointDiscoveryMetadata.Address);                      
                }  
  
                PrintDiscoveryMetadata(endpointDiscoveryMetadata, "Removing");  
            }      
        }  
  
        void MatchFromOnlineService(FindRequestContext findRequestContext)  
        {  
            lock (this.onlineServices)  
            {  
                foreach (EndpointDiscoveryMetadata endpointDiscoveryMetadata in this.onlineServices.Values)  
                {  
                    if (findRequestContext.Criteria.IsMatch(endpointDiscoveryMetadata))  
                    {  
                        findRequestContext.AddMatchingEndpoint(endpointDiscoveryMetadata);  
                    }  
                }  
            }  
        }  
  
        EndpointDiscoveryMetadata MatchFromOnlineService(ResolveCriteria criteria)  
        {  
            EndpointDiscoveryMetadata matchingEndpoint = null;  
            lock (this.onlineServices)  
            {  
                foreach (EndpointDiscoveryMetadata endpointDiscoveryMetadata in this.onlineServices.Values)  
                {  
                    if (criteria.Address == endpointDiscoveryMetadata.Address)  
                    {  
                        matchingEndpoint = endpointDiscoveryMetadata;  
                    }  
                }  
            }  
            return matchingEndpoint;  
        }  
  
        void PrintDiscoveryMetadata(EndpointDiscoveryMetadata endpointDiscoveryMetadata, string verb)  
        {  
            Console.WriteLine("\n**** " + verb + " service of the following type from cache. ");  
            foreach (XmlQualifiedName contractName in endpointDiscoveryMetadata.ContractTypeNames)  
            {  
                Console.WriteLine("** " + contractName.ToString());  
                break;  
            }  
            Console.WriteLine("**** Operation Completed");  
        }  
  
        sealed class OnOnlineAnnouncementAsyncResult : AsyncResult  
        {  
            public OnOnlineAnnouncementAsyncResult(AsyncCallback callback, object state)  
                : base(callback, state)  
            {  
                this.Complete(true);  
            }  
  
            public static void End(IAsyncResult result)  
            {  
                AsyncResult.End<OnOnlineAnnouncementAsyncResult>(result);  
            }  
        }  
  
        sealed class OnOfflineAnnouncementAsyncResult : AsyncResult  
        {  
            public OnOfflineAnnouncementAsyncResult(AsyncCallback callback, object state)  
                : base(callback, state)  
            {  
                this.Complete(true);  
            }  
  
            public static void End(IAsyncResult result)  
            {  
                AsyncResult.End<OnOfflineAnnouncementAsyncResult>(result);  
            }  
        }  
  
        sealed class OnFindAsyncResult : AsyncResult  
        {  
            public OnFindAsyncResult(AsyncCallback callback, object state)  
                : base(callback, state)  
            {  
                this.Complete(true);  
            }  
  
            public static void End(IAsyncResult result)  
            {  
                AsyncResult.End<OnFindAsyncResult>(result);  
            }  
        }  
  
        sealed class OnResolveAsyncResult : AsyncResult  
        {  
            EndpointDiscoveryMetadata matchingEndpoint;  
  
            public OnResolveAsyncResult(EndpointDiscoveryMetadata matchingEndpoint, AsyncCallback callback, object state)  
                : base(callback, state)  
            {  
                this.matchingEndpoint = matchingEndpoint;  
                this.Complete(true);  
            }  
  
            public static EndpointDiscoveryMetadata End(IAsyncResult result)  
            {  
                OnResolveAsyncResult thisPtr = AsyncResult.End<OnResolveAsyncResult>(result);  
                return thisPtr.matchingEndpoint;  
            }  
        }  
    }  
}  
  
```  
  
```  
// AsyncResult.cs  
//----------------------------------------------------------------  
// Copyright (c) Microsoft Corporation.  All rights reserved.  
//----------------------------------------------------------------  
  
using System;  
using System.Threading;  
  
namespace Microsoft.Samples.Discovery  
{  
    abstract class AsyncResult : IAsyncResult  
    {  
        AsyncCallback callback;  
        bool completedSynchronously;  
        bool endCalled;  
        Exception exception;  
        bool isCompleted;  
        ManualResetEvent manualResetEvent;  
        object state;  
        object thisLock;  
  
        protected AsyncResult(AsyncCallback callback, object state)  
        {  
            this.callback = callback;  
            this.state = state;  
            this.thisLock = new object();  
        }  
  
        public object AsyncState  
        {  
            get  
            {  
                return state;  
            }  
        }  
  
        public WaitHandle AsyncWaitHandle  
        {  
            get  
            {  
                if (manualResetEvent != null)  
                {  
                    return manualResetEvent;  
                }  
                lock (ThisLock)  
                {  
                    if (manualResetEvent == null)  
                    {  
                        manualResetEvent = new ManualResetEvent(isCompleted);  
                    }  
                }  
                return manualResetEvent;  
            }  
        }  
  
        public bool CompletedSynchronously  
        {  
            get  
            {  
                return completedSynchronously;  
            }  
        }  
  
        public bool IsCompleted  
        {  
            get  
            {  
                return isCompleted;  
            }  
        }  
  
        object ThisLock  
        {  
            get  
            {  
                return this.thisLock;  
            }  
        }  
  
        protected static TAsyncResult End<TAsyncResult>(IAsyncResult result)  
            where TAsyncResult : AsyncResult  
        {  
            if (result == null)  
            {  
                throw new ArgumentNullException("result");  
            }  
  
            TAsyncResult asyncResult = result as TAsyncResult;  
  
            if (asyncResult == null)  
            {  
                throw new ArgumentException("Invalid async result.", "result");  
            }  
  
            if (asyncResult.endCalled)  
            {  
                throw new InvalidOperationException("Async object already ended.");  
            }  
  
            asyncResult.endCalled = true;  
  
            if (!asyncResult.isCompleted)  
            {  
                asyncResult.AsyncWaitHandle.WaitOne();  
            }  
  
            if (asyncResult.manualResetEvent != null)  
            {  
                asyncResult.manualResetEvent.Close();  
            }  
  
            if (asyncResult.exception != null)  
            {  
                throw asyncResult.exception;  
            }  
  
            return asyncResult;  
        }  
  
        protected void Complete(bool completedSynchronously)  
        {  
            if (isCompleted)  
            {  
                throw new InvalidOperationException("This async result is already completed.");  
            }  
  
            this.completedSynchronously = completedSynchronously;  
  
            if (completedSynchronously)  
            {  
                this.isCompleted = true;  
            }  
            else  
            {  
                lock (ThisLock)  
                {  
                    this.isCompleted = true;  
                    if (this.manualResetEvent != null)  
                    {  
                        this.manualResetEvent.Set();  
                    }  
                }  
            }  
  
            if (callback != null)  
            {  
                callback(this);  
            }  
        }  
  
        protected void Complete(bool completedSynchronously, Exception exception)  
        {  
            this.exception = exception;  
            Complete(completedSynchronously);  
        }  
    }  
}  
  
```  
  
```  
// program.cs  
//----------------------------------------------------------------  
// Copyright (c) Microsoft Corporation.  All rights reserved.  
//----------------------------------------------------------------  
  
using System;  
using System.ServiceModel;  
using System.ServiceModel.Discovery;  
  
namespace Microsoft.Samples.Discovery  
{  
    class Program  
    {  
        public static void Main()  
        {  
            Uri probeEndpointAddress = new Uri("net.tcp://localhost:8001/Probe");  
            Uri announcementEndpointAddress = new Uri("net.tcp://localhost:9021/Announcement");  
  
            // Host the DiscoveryProxy service  
            ServiceHost proxyServiceHost = new ServiceHost(new DiscoveryProxyService());  
  
            try  
            {                  
                // Add DiscoveryEndpoint to receive Probe and Resolve messages  
                DiscoveryEndpoint discoveryEndpoint = new DiscoveryEndpoint(new NetTcpBinding(), new EndpointAddress(probeEndpointAddress));  
                discoveryEndpoint.IsSystemEndpoint = false;  
  
                // Add AnnouncementEndpoint to receive Hello and Bye announcement messages  
                AnnouncementEndpoint announcementEndpoint = new AnnouncementEndpoint(new NetTcpBinding(), new EndpointAddress(announcementEndpointAddress));                  
  
                proxyServiceHost.AddServiceEndpoint(discoveryEndpoint);  
                proxyServiceHost.AddServiceEndpoint(announcementEndpoint);  
  
                proxyServiceHost.Open();  
  
                Console.WriteLine("Proxy Service started.");  
                Console.WriteLine();  
                Console.WriteLine("Press <ENTER> to terminate the service.");  
                Console.WriteLine();  
                Console.ReadLine();  
  
                proxyServiceHost.Close();  
            }  
            catch (CommunicationException e)  
            {  
                Console.WriteLine(e.Message);  
            }  
            catch (TimeoutException e)  
            {  
                Console.WriteLine(e.Message);  
            }     
  
            if (proxyServiceHost.State != CommunicationState.Closed)  
            {  
                Console.WriteLine("Aborting the service...");  
                proxyServiceHost.Abort();  
            }  
        }  
    }  
}  
  
```  
  
## См. также  
 [Общие сведения об обнаружении WCF](../../../../docs/framework/wcf/feature-details/wcf-discovery-overview.md)   
 [Как реализовать обнаружимую службу, которая регистрируется в прокси\-сервере обнаружения](../../../../docs/framework/wcf/feature-details/discoverable-service-that-registers-with-the-discovery-proxy.md)   
 [Как реализовать клиентское приложение, которое для поиска служб использует прокси\-сервер обнаружения](../../../../docs/framework/wcf/feature-details/client-app-discovery-proxy-to-find-a-service.md)   
 [Как проверить прокси\-сервер обнаружения](../../../../docs/framework/wcf/feature-details/how-to-test-the-discovery-proxy.md)