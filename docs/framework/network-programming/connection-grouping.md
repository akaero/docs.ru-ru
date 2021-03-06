---
title: "Группирование подключений"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.tgt_pltfrm: 
ms.topic: article
dev_langs:
- VB
- CSharp
- C++
- jsharp
helpviewer_keywords:
- Internet, connections
- connections [.NET Framework], grouping
- WebRequest class, connection grouping
- network resources, connections
- connection pooling
ms.assetid: 2ec502e8-4ba0-4c22-9410-f28eaf4eee63
caps.latest.revision: 7
author: mcleblanc
ms.author: markl
manager: markl
ms.translationtype: HT
ms.sourcegitcommit: 306c608dc7f97594ef6f72ae0f5aaba596c936e1
ms.openlocfilehash: 8f16a539bb7c2bef494c7b5551e1f12bc71de60f
ms.contentlocale: ru-ru
ms.lasthandoff: 08/21/2017

---
# <a name="connection-grouping"></a>Группирование подключений
При группировании подключений отдельный запрос в одном приложении связывается с определенным пулом подключений. Это может быть необходимо для приложения среднего уровня, которое подключается к внутреннему серверу от имени пользователя и использует протокол проверки подлинности с поддержкой делегирования (например, Kerberos), а также для приложения среднего уровня, которое предоставляет свои собственные учетные данные, как показано в следующем примере. Допустим, пользователь переходит на внутренний веб-сайт с информацией о его заработной плате. После проверки подлинности пользователя сервер приложения среднего уровня использует его учетные данные для подключения к внутреннему серверу и извлечение сведений о его заработной плате. Далее другой пользователь переходит на этот сайт и запрашивает сведения о своей зарплате. Поскольку приложение среднего уровня уже установило подключение от имени первого пользователя, внутренний сервер в ответе возвращает именно его данные. Тем не менее, если приложение назначает каждый запрос, отправляемый на внутренний сервер, группе подключений, формируемой по имени пользователя, то каждый пользователь будет принадлежать отдельному пулу подключений и не сможет случайно использовать одни и те же сведения для проверки подлинности совместно с другим пользователем.  
  
 Чтобы назначить запрос конкретной группе подключений, необходимо присвоить имя свойству <xref:System.Net.WebRequest.ConnectionGroupName%2A> для <xref:System.Net.WebRequest>, прежде чем выполнять запрос.  
  
## <a name="see-also"></a>См. также  
 [Управление подключениями](../../../docs/framework/network-programming/managing-connections.md)   
 [Практическое руководство. Присвоение данных пользователя групповым подключениям](../../../docs/framework/network-programming/how-to-assign-user-information-to-group-connections.md)

