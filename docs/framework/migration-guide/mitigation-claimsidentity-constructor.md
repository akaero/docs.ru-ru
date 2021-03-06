---
title: "Устранение рисков. Конструктор ClaimsIdentity"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology:
- dotnet-bcl
- dotnet-clr
ms.tgt_pltfrm: 
ms.topic: article
ms.assetid: 946903f1-0fbf-4f67-805b-1eb48352024d
caps.latest.revision: 5
author: rpetrusha
ms.author: ronpet
manager: wpickett
ms.translationtype: HT
ms.sourcegitcommit: 306c608dc7f97594ef6f72ae0f5aaba596c936e1
ms.openlocfilehash: a50cbd69aa1f2c72adc9fc4d10a070f5faa0cf54
ms.contentlocale: ru-ru
ms.lasthandoff: 07/28/2017

---
# <a name="mitigation-claimsidentity-constructor"></a>Устранение рисков. Конструктор ClaimsIdentity
Начиная с [!INCLUDE[net_v462](../../../includes/net-v462-md.md)] изменен способ задания конструктором <xref:System.Security.Claims.ClaimsIdentity.%23ctor%28System.Security.Principal.IIdentity%29?displayProperty=fullName> свойства <xref:System.Security.Claims.ClaimsIdentity.Actor%2A?displayProperty=fullName>. Если аргумент <xref:System.Security.Principal.IIdentity> является объектом <xref:System.Security.Claims.ClaimsIdentity>, а свойство <xref:System.Security.Claims.ClaimsIdentity.Actor%2A> этого объекта <xref:System.Security.Claims.ClaimsIdentity> не равно `null`, свойство <xref:System.Security.Claims.ClaimsIdentity.Actor%2A> присоединяется с помощью метода <xref:System.Security.Claims.ClaimsIdentity.Clone%2A?displayProperty=fullName>. В [!INCLUDE[net_v461](../../../includes/net-v461-md.md)] свойство <xref:System.Security.Claims.ClaimsIdentity.Actor%2A> присоединяется в качестве существующей ссылки.  
  
## <a name="impact"></a>Последствия  
 Начиная с [!INCLUDE[net_v462](../../../includes/net-v462-md.md)] свойство <xref:System.Security.Claims.ClaimsIdentity.Actor%2A?displayProperty=fullName> нового объекта <xref:System.Security.Claims.ClaimsIdentity> не равно свойству <xref:System.Security.Claims.ClaimsIdentity.Actor%2A?displayProperty=fullName> аргумента <xref:System.Security.Principal.IIdentity> конструктора. В [!INCLUDE[net_v461](../../../includes/net-v461-md.md)] и более ранних версиях они были равны.  
  
## <a name="mitigation"></a>Уменьшение  
 Если такое поведение нежелательно, можно восстановить прежнее поведение, задав переключателю `Switch.System.Security.ClaimsIdentity.SetActorAsReferenceWhenCopyingClaimsIdentity` в файле конфигурации приложения значение `true`. Для этого требуется добавить следующую информацию в раздел [\<runtime>](../../../docs/framework/configure-apps/file-schema/runtime/runtime-element.md) файла web.config:  
  
```xml  
<configuration>  
   <runtime>  
      <AppContextSwitchOverrides value="Switch.System.Security.ClaimsIdentity.SetActorAsReferenceWhenCopyingClaimsIdentity=true" />  
   </runtime>  
</configuration>  
```  
  
## <a name="see-also"></a>См. также  
 [Изменение целевой платформы](../../../docs/framework/migration-guide/retargeting-changes-in-the-net-framework-4-6.md)

