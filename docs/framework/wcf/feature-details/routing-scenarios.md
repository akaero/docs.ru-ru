---
title: "Сценарии маршрутизации | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
helpviewer_keywords: 
  - "маршрутизация [WCF], сценарии"
ms.assetid: ec22f308-665a-413e-9f94-7267cb665dab
caps.latest.revision: 7
author: "wadepickett"
ms.author: "wpickett"
manager: "wpickett"
caps.handback.revision: 7
---
# Сценарии маршрутизации
Хотя служба маршрутизации является настраиваемой, спроектировать эффективную логику маршрутизации при создании новой конфигурации с нуля может быть достаточно сложным.  Однако существует несколько типичных сценариев, которым следует большинство конфигураций службы маршрутизации.  Хотя эти сценарии могут не применяться напрямую к определенной конфигурации, освоение способа настройки службы маршрутизации для управления этими сценариями поможет в понимании работы службы маршрутизации.  
  
## Стандартные сценарии  
 Наиболее часто служба маршрутизации используется для объединения нескольких целевых конечных точек для снижения числа конечных точек, представленных клиентским приложениям, и последующего использования фильтров сообщений для перенаправления каждого сообщения в нужное назначение.  Сообщения можно перенаправить на основе логических или физических требований к обработке, например типа сообщения, которое должно обрабатываться определенной службой, или на основе произвольных бизнес\-требованиях, например обеспечении приоритета обработки сообщений от определенного источника.  В следующей таблице перечислены некоторые стандартные сценарии и условия, при которых они возникают.  
  
|Сценарий|Используется, если|  
|--------------|------------------------|  
|Управление версиями служб|Необходимо поддерживать несколько версий службы, или потребуется развертывание обновленной службы в будущем|  
|Секционирование данных службы|Необходимо секционировать службу в нескольких узлах|  
|Динамическое обновление|Необходимо динамически перенастроить логику маршрутизации во время выполнения для управления изменением развертывания служб|  
|Multicast|Необходимо отправить одно сообщение в несколько конечных точек|  
|Связывание протоколов|Получение сообщений выполняется через один транспортный протокол, а целевая конечная точка использует другой протокол|  
|Обработка ошибок|Необходимо обеспечить устойчивость к сетевым сбоям и ошибкам связи|  
  
> [!NOTE]
>  Хотя многие из представленных сценариев созданы для определенных бизнес\-требований или требований к обработке, планирование динамических обновлений и управление ошибками часто считается лучшей методикой, поскольку позволяет изменять логику маршрутизации во время выполнения и выполнять восстановление после временных неполадок в сети и ошибок связи.  
  
### Управление версиями службы  
 При выходе новой версии службы часто бывает необходимо поддерживать предыдущую версию до перевода всех клиентов на новую службу.  Это особенно важно, если служба является длительным процессом, требующим для выполнения дни, недели и даже месяцы.  Обычно это требует внедрения нового адреса конечной точки для новой службы с сохранением поддержки исходной конечной точки для предыдущей версии.  
  
 Служба маршрутизации позволяет через одну конечную точку принимать сообщения от клиентских приложений и перенаправлять каждое сообщение на основе его содержимого службе с требуемой версией.  Наиболее общая реализация включает добавление пользовательского заголовка в сообщение, определяющее версию службы, которая обрабатывает данное сообщение.  Служба маршрутизации может использовать объект XPathMessageFilter для проверки каждого сообщения на наличие пользовательского заголовка и перенаправлять сообщение в соответствующую целевую конечную точку.  
  
 Этапы создания конфигурации управления версиями службы см. в разделе [Как управлять версиями службы](../../../../docs/framework/wcf/feature-details/how-to-service-versioning.md).  Пример использования объекта XPathMessageFilter для перенаправления сообщений на основе пользовательского заголовка см. в образце [Дополнительные фильтры](../../../../docs/framework/wcf/samples/advanced-filters.md).  
  
### Секционирование данных служб  
 При проектировании распределенной среды часто требуется распределение нагрузки обработки среди нескольких компьютеров для обеспечения высокого уровня доступности, снижения нагрузки обработки на отдельных компьютерах или для предоставления выделенных ресурсов для определенных подмножеств сообщений.  Хотя служба маршрутизации не заменяет специализированное решение по балансу нагрузки, ее способность выполнять маршрутизацию на основе содержимого можно использовать для перенаправления похожих сообщений в определенные назначения.  Например, можно назначить обработку сообщений от определенного клиента отдельно от сообщений, полученных от других клиентов.  
  
 Этапы создания конфигурации секционирования данных службы см. в разделе [Как секционировать данные служб](../../../../docs/framework/wcf/feature-details/how-to-service-data-partitioning.md).  Пример использования фильтров для секционирования данных на основе URL\-адреса и пользовательских заголовков см. в образце [Дополнительные фильтры](../../../../docs/framework/wcf/samples/advanced-filters.md).  
  
### Динамическая маршрутизация  
 Часто требуется изменить конфигурацию маршрутизации для удовлетворения изменений бизнес\-требований, например добавления маршрута к более новой версии службы, изменения критерия маршрутизации или изменения целевой конечной точки определенного сообщения, которое перенаправляет фильтр.  Служба маршрутизации позволяет выполнять вышеуказанные задачи с помощью класса <xref:System.ServiceModel.Routing.RoutingExtension>, который позволяет предоставлять новую конфигурацию маршрутизации во время выполнения.  Новая конфигурация вступает в силу немедленно, но влияет только на новые сеансы, обрабатываемые службой маршрутизации.  
  
 Этапы реализации динамической маршрутизации см. в разделе [Как проводить динамическое обновление](../../../../docs/framework/wcf/feature-details/how-to-dynamic-update.md).  Пример использования динамической маршрутизации см. в образце [Динамическая реконфигурация](../../../../docs/framework/wcf/samples/dynamic-reconfiguration.md).  
  
### Multicast  
 При маршрутизации сообщений, как правило, выполняется перенаправление каждого сообщения в одну определенную целевую конечную точку.  Однако может потребоваться перенаправить копию сообщения в несколько целевых конечных точек.  Для выполнения многоадресной маршрутизации необходимо соблюдение следующих условий.  
  
-   Форма канала не должна иметь тип «запрос\-ответ» \(хотя он может быть односторонним или дуплексным\), так как данный тип требует, чтобы на запрос от клиентского приложения выдавался только один ответ.  
  
-   При оценке сообщения несколько фильтров должны возвращать значение **true**.  
  
 При выполнении этих условий каждая целевая конечная точка, связанная с фильтром, возвращающим значение true, получит копию сообщения.  
  
### Связывание протоколов  
 При маршрутизации сообщений между разнородными протоколами SOAP служба маршрутизации использует API\-интерфейсы WCF для преобразования сообщения из одного протокола в другой.  Это происходит автоматически, если конечная точка службы, представленная службой маршрутизации, использует протокол, отличный от протокола, используемого клиентской точкой, в которую перенаправляются сообщения.  Данное поведение можно отключить, если используемые протоколы не являются стандартными. Однако затем необходимо предоставить свой собственный код преобразования.  
  
 .  Пример использования службы маршрутизации для преобразования сообщений между протоколами см. в образце [Использование моста и обработка ошибок](../../../../docs/framework/wcf/samples/bridging-and-error-handling.md).  
  
### Обработка ошибок  
 В распределенной среде временные неполадки в сети или ошибки связи обычно не возникают.  Без наличия службы\-посредника, такой как служба маршрутизации, нагрузка по устранению таких ошибок ложится на клиентское приложение.  Если клиентское приложение не содержит определенную логику повторения при ошибках сети или связи и набора знаний об альтернативных расположениях, то пользователь может столкнуться с ситуацией, когда сообщение необходимо отправить несколько раз, перед тем как оно будет успешно обработано целевой службой.  Это приводит к неудовлетворению заказчика качеством работы приложения, и оно может быть воспринято как ненадежное.  
  
 Служба маршрутизации помогает решить проблемы такого сценария, предоставляя сообщениям возможность надежной обработки ошибок или сбоев связи, возникающих в сети.  Путем создания списка возможных целевых точек и сопоставления данного списка с каждым фильтром сообщений решается проблема, вызванная наличием только одного возможного назначения.  В случае сбоя служба маршрутизации попытается доставить сообщение следующей конечной точке в списке до окончательной доставки сообщения, возникновения сбоя, не относящегося к связи, или пока не закончатся все конечные точки.  
  
 Этапы настройки обработки ошибок см. в разделе [Как обрабатывать ошибки](../../../../docs/framework/wcf/feature-details/how-to-error-handling.md).  Пример реализации обработки ошибок см. в образцах [Использование моста и обработка ошибок](../../../../docs/framework/wcf/samples/bridging-and-error-handling.md) и [Расширенная обработка ошибок](../../../../docs/framework/wcf/samples/advanced-error-handling.md).  
  
### Содержание  
 [Как управлять версиями службы](../../../../docs/framework/wcf/feature-details/how-to-service-versioning.md)  
  
 [Как секционировать данные служб](../../../../docs/framework/wcf/feature-details/how-to-service-data-partitioning.md)  
  
 [Как проводить динамическое обновление](../../../../docs/framework/wcf/feature-details/how-to-dynamic-update.md)  
  
 [Как обрабатывать ошибки](../../../../docs/framework/wcf/feature-details/how-to-error-handling.md)  
  
## См. также  
 [Введение в маршрутизацию](../../../../docs/framework/wcf/feature-details/routing-introduction.md)