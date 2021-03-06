---
title: "Расширяемость хранилища | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 7c3f4a46-4bac-4138-ae6a-a7c7ee0d28f5
caps.latest.revision: 15
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 15
---
# Расширяемость хранилища
<xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore> позволяет пользователям повышать уровень относящихся к приложению пользовательских свойств, которые могут использоваться для создания запросов к экземплярам в базе данных сохраняемости.Благодаря действию по повышению уровня свойства значение становится доступным в специальном представлении в базе данных.Свойства с повышенным уровнем \(т.е. свойства, которые могут использоваться в пользовательских запросах\), могут относиться к простым типам, например к Int64, Guid, String, DateTime или к сериализованному двоичному типу \(byte\[\]\).  
  
 Класс <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore> содержит метод <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore.Promote%2A>, который может использоваться для повышения уровня свойства, чтобы его можно было использовать в запросах.Ниже приведен подробный пошаговый пример расширения хранилища.  
  
1.  В данном образце сценария приложение обработки документов содержит рабочие процессы, в каждом из которых для обработки документов применяются пользовательские действия.Эти рабочие процессы содержат набор переменных состояния, которые необходимо сделать доступными для конечных пользователей.Для этого приложение обработки документов предоставляет расширение экземпляра типа <xref:System.Activities.Persistence.PersistenceParticipant>, используемое действиями для предоставления переменных состояния.  
  
    ```  
  
    class DocumentStatusExtension : PersistenceParticipant  
    {  
        public string DocumentId;  
        public string ApprovalStatus;  
        public string UserName;  
        public DateTime LastUpdateTime;  
    }  
  
    ```  
  
2.  Затем к узлу добавляется новое расширение.  
  
    ```  
    static Activity workflow = CreateWorkflow();  
    WorkflowApplication application = new WorkflowApplication(workflow);  
    DocumentStatusExtension documentStatusExtension = new DocumentStatusExtension ();  
    application.Extensions.Add(documentStatusExtension);  
  
    ```  
  
     Дополнительные сведения о добавлении нестандартного участника сохраняемости см. в образце [Участники сохраняемости](../../../docs/framework/windows-workflow-foundation//persistence-participants.md).  
  
3.  Пользовательские действия в приложении обработки документов заполняют различные поля состояния в методе **Execute**.  
  
    ```  
  
    public override void Execute(CodeActivityContext context)  
    {  
        // ...  
        context.GetExtension<DocumentStatusExtension>().DocumentId = Guid.NewGuid();  
        context.GetExtension<DocumentStatusExtension>().UserName = "John Smith";  
        context.GetExtension<DocumentStatusExtension>().ApprovalStatus = “Approved”;  
        context.GetExtension<DocumentStatusExtension>().LastUpdateTime = DateTime.Now();  
        // ...  
    }  
  
    ```  
  
4.  Когда экземпляр рабочего процесса достигает точки сохраняемости, метод **CollectValues** участника сохраняемости **DocumentStatusExtension** сохраняет данные свойства в коллекцию данных сохраняемости.  
  
    ```  
  
    class DocumentStatusExtension : PersistenceParticipant  
    {  
        const XNamespace xNS = XNamespace.Get("http://contoso.com/DocumentStatus");  
  
        protected override void CollectValues(out IDictionary<XName, object> readWriteValues, out IDictionary<XName, object> writeOnlyValues)  
        {  
            readWriteValues = new Dictionary<XName, object>();  
            readWriteValues.Add(xNS.GetName("UserName"), this.UserName);  
            readWriteValues.Add(xNS.GetName("ApprovalStatus"), this.ApprovalStatus);  
            readWriteValues.Add(xNS.GetName("DocumentId"), this.DocumentId);  
            readWriteValues.Add(xNS.GetName("LastModifiedTime"), this.LastUpdateTime);  
  
            writeOnlyValues = null;  
        }  
        // ...  
    }  
  
    ```  
  
    > [!NOTE]
    >  Все эти свойства передаются в хранилище **SqlWorkflowInstanceStore** платформой сохраняемости через коллекцию **SaveWorkflowCommand.InstanceData**.  
  
5.  Приложение обработки документов инициализирует хранилище экземпляров рабочих процессов SQL и вызывает метод **Promote** для повышения уровня этих данных.  
  
    ```  
    SqlWorkflowInstanceStore store = new SqlWorkflowInstanceStore(connectionString);  
  
    List<XName> variantProperties = new List<XName>()   
    {   
        xNS.GetName("UserName"),   
        xNS.GetName("ApprovalStatus"),   
        xNS.GetName("DocumentId"),   
        xNS.GetName("LastModifiedTime")   
    };  
  
    store.Promote("DocumentStatus", variantProperties, null);  
    ```  
  
     Основываясь на сведениях о повышении уровня, **SqlWorkflowInstanceStore** помещает свойства данных в столбцах [Представление [System.Activities.DurableInstancing.InstancePromotedProperties]](../../../docs/framework/windows-workflow-foundation//store-extensibility.md#InstancePromotedProperties).  
  
6.  Чтобы создать запрос к подмножеству данных из таблицы повышения уровня, приложение обработки данных добавляет пользовательское представление к представлению повышения уровня.  
  
    ```  
  
    create view [dbo].[DocumentStatus] with schemabinding  
    as  
        select  P.[InstanceId] as [InstanceId],  
            P.Value1 as [UserName],  
            P.Value2 as [ApprovalStatus],  
            P.Value3 as [DocumentId],  
            P.Value4 as [LastUpdatedTime]  
    from [System.Activities.DurableInstancing].[InstancePromotedProperties] as P  
    where P.PromotionName = N'DocumentStatus'  
    go  
  
    ```  
  
##  <a name="InstancePromotedProperties"></a> Представление \[System.Activities.DurableInstancing.InstancePromotedProperties\]  
  
|Имя столбца|Тип столбца|Описание|  
|-----------------|-----------------|--------------|  
|InstanceId|GUID|Экземпляр рабочего процесса, которому принадлежит это повышение уровня.|  
|PromotionName|nvarchar\(400\)|Имя самого продвижения.|  
|Value1, Value2, Value3,..,Value32|sql\_variant|Значение самого свойства, уровень которого повышен.Большинство примитивных типов данных SQL, за исключением больших двоичных объектов BLOB и строк длиной более 8000 байт, помещается в sql\_variant.|  
|Value33, Value34, Value35, …, Value64|varbinary\(max\)|Значение свойств повышаемого уровня, явно объявленных, как varbinary\(max\).|