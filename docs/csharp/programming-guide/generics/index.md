---
title: "Универсальные шаблоны (Руководство по программированию на C#)"
ms.date: 2015-07-20
ms.prod: .net
ms.technology:
- devlang-csharp
ms.topic: article
dev_langs:
- CSharp
helpviewer_keywords:
- C# language, generics
- generics [C#]
ms.assetid: 75ea8509-a4ea-4e7a-a2b3-cf72482e9282
caps.latest.revision: 23
author: BillWagner
ms.author: wiwagn
translation.priority.ht:
- cs-cz
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pl-pl
- pt-br
- ru-ru
- tr-tr
- zh-cn
- zh-tw
ms.translationtype: HT
ms.sourcegitcommit: 1e548df4de2c07934313311a7ffcfae82be76000
ms.openlocfilehash: de81058173b0985577474e8601aa84d4e83336a5
ms.contentlocale: ru-ru
ms.lasthandoff: 08/29/2017

---
# <a name="generics-c-programming-guide"></a>Универсальные шаблоны (Руководство по программированию на C#)
Универсальные шаблоны были добавлены в язык C# и общеязыковую среду выполнения (CLR) в версии 2.0. Универсальные шаблоны вводят на платформе .NET Framework концепцию параметров универсального типа. Благодаря им вы можете создавать классы и методы с типами, спецификация которых отложена до момента объявления и создания экземпляров в клиентском коде. Как пример, ниже показан класс с параметром T универсального типа. Этот класс может использоваться в другом клиентском коде, не требуя ресурсов и не создавая рисков, связанных с операциями приведения и упаковки-преобразования в среде выполнения.  
  
 [!code-cs[csProgGuideGenerics#1](../../../csharp/programming-guide/generics/codesnippet/CSharp/index_1.cs)]  
  
## <a name="generics-overview"></a>Общие сведения об универсальных шаблонах  
  
-   Используйте универсальные типы, чтобы получить максимально широкие возможности многократного использования кода, обеспечения безопасности типов и повышения производительности.  
  
-   Чаще всего универсальные шаблоны используются для создания классов коллекций.  
  
-   Библиотека классов .NET Framework содержит несколько новых универсальных классов коллекций в пространстве имен <xref:System.Collections.Generic>. Рекомендуется как можно чаще использовать их вместо таких классов, как <xref:System.Collections.ArrayList> в пространстве имен <xref:System.Collections>.  
  
-   Вы можете создавать собственные универсальные интерфейсы, классы, методы, события и делегаты.  
  
-   Универсальные классы можно ограничить, чтобы они разрешали доступ к методам только для определенных типов данных.  
  
-   Сведения о типах, используемых в универсальном типе данных, можно получить во время выполнения с помощью отражения.  
  
## <a name="related-sections"></a>Связанные разделы  
 Дополнительные сведения:  
  
-   [Введение в универсальные шаблоны](../../../csharp/programming-guide/generics/introduction-to-generics.md)  
  
-   [Преимущества универсальных шаблонов](../../../csharp/programming-guide/generics/benefits-of-generics.md)  
  
-   [Параметры универсального типа](../../../csharp/programming-guide/generics/generic-type-parameters.md)  
  
-   [Ограничения параметров типа](../../../csharp/programming-guide/generics/constraints-on-type-parameters.md)  
  
-   [Универсальные классы](../../../csharp/programming-guide/generics/generic-classes.md)  
  
-   [Универсальные интерфейсы](../../../csharp/programming-guide/generics/generic-interfaces.md)  
  
-   [Универсальные методы](../../../csharp/programming-guide/generics/generic-methods.md)  
  
-   [Универсальные делегаты](../../../csharp/programming-guide/generics/generic-delegates.md)  
  
-   [Различия между шаблонами языка C++ и универсальными шаблонами языка C#](../../../csharp/programming-guide/generics/differences-between-cpp-templates-and-csharp-generics.md)  
  
-   [Универсальные типы и отражение](../../../csharp/programming-guide/generics/generics-and-reflection.md)  
  
-   [Универсальные типы во время выполнения](../../../csharp/programming-guide/generics/generics-in-the-run-time.md)  
  
-   [Универсальные шаблоны в библиотеке классов платформы .NET Framework](../../../csharp/programming-guide/generics/generics-in-the-net-framework-class-library.md)  
  
## <a name="c-language-specification"></a>Спецификация языка C#  
 Дополнительные сведения см. в [спецификации языка C#](../../../csharp/language-reference/language-specification/index.md).  
  
## <a name="see-also"></a>См. также  
 <xref:System.Collections.Generic>   
 [Руководство по программированию на C#](../../../csharp/programming-guide/index.md)   
 [Типы](../../../csharp/programming-guide/types/index.md)   
 [\<typeparam>](../../../csharp/programming-guide/xmldoc/typeparam.md)   
 [\<typeparamref>](../../../csharp/programming-guide/xmldoc/typeparamref.md)

