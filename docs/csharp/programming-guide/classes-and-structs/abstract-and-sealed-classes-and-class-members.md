---
title: "Абстрактные и запечатанные классы и члены классов (Руководство по программированию на C#)"
ms.date: 2015-07-20
ms.prod: .net
ms.technology:
- devlang-csharp
ms.topic: article
dev_langs:
- CSharp
helpviewer_keywords:
- abstract classes [C#]
- sealed classes [C#]
- C# language, abstract classes
- C# language, sealed
ms.assetid: 99aa52f7-b435-43f9-936e-2470af734c4e
caps.latest.revision: 14
author: BillWagner
ms.author: wiwagn
translation.priority.ht:
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- ru-ru
- zh-cn
- zh-tw
translation.priority.mt:
- cs-cz
- pl-pl
- pt-br
- tr-tr
ms.translationtype: HT
ms.sourcegitcommit: 306c608dc7f97594ef6f72ae0f5aaba596c936e1
ms.openlocfilehash: 0788ea0778b3f8b846231fc2408938b2236314c9
ms.contentlocale: ru-ru
ms.lasthandoff: 07/28/2017

---
# <a name="abstract-and-sealed-classes-and-class-members-c-programming-guide"></a>Абстрактные и запечатанные классы и члены классов (Руководство по программированию на C#)
Ключевое слово [abstract](../../../csharp/language-reference/keywords/abstract.md) позволяет создавать классы и члены [классов](../../../csharp/language-reference/keywords/class.md), которые являются неполными и должны быть реализованы в производном классе.  
  
 Ключевое слово [sealed](../../../csharp/language-reference/keywords/sealed.md) позволяет предотвратить наследование класса или определенных членов класса, помеченных ранее как [virtual](../../../csharp/language-reference/keywords/virtual.md).  
  
## <a name="abstract-classes-and-class-members"></a>Абстрактные классы и члены классов  
 Классы могут быть объявлены абстрактными путем помещения ключевого слова `abstract` перед определением класса. Пример:  
  
 [!code-cs[csProgGuideInheritance#13](../../../csharp/programming-guide/classes-and-structs/codesnippet/CSharp/abstract-and-sealed-classes-and-class-members_1.cs)]  
  
 Создавать экземпляры абстрактного класса нельзя. Назначение абстрактного класса заключается в предоставлении общего определения для базового класса, которое могут совместно использовать несколько производных классов. Например, в библиотеке классов может быть определен абстрактный класс, используемый в качестве параметра для многих из ее функций, поэтому программисты, использующие эту библиотеку, должны задать свою реализацию этого класса, создав производный класс.  
  
 Абстрактные классы могут определять абстрактные методы. Для этого перед типом возвращаемого значения метода необходимо поместить ключевое слово `abstract`. Пример:  
  
 [!code-cs[csProgGuideInheritance#14](../../../csharp/programming-guide/classes-and-structs/codesnippet/CSharp/abstract-and-sealed-classes-and-class-members_2.cs)]  
  
 Абстрактные методы не имеют реализации, поэтому определение такого метода заканчивается точкой с запятой вместо обычного блока метода. Классы, производные от абстрактного класса, должны реализовывать все абстрактные методы. Если абстрактный класс наследует виртуальный метод из базового класса, абстрактный класс может переопределить виртуальный метод с помощью абстрактного метода. Пример:  
  
 [!code-cs[csProgGuideInheritance#15](../../../csharp/programming-guide/classes-and-structs/codesnippet/CSharp/abstract-and-sealed-classes-and-class-members_3.cs)]  
  
 Если метод `virtual` объявляется как `abstract`, он все равно считается виртуальным по отношению к любому классу, наследующему от абстрактного класса. Класс, наследующий абстрактный метод, не может получать доступ к исходной реализации метода (так, в предыдущем примере `DoWork` для класса F не может вызывать `DoWork` для класса D). Таким образом, абстрактный класс может принудительно задавать необходимость предоставлять новые реализации виртуальных методов в производных классах.  
  
## <a name="sealed-classes-and-class-members"></a>Запечатанные классы и члены классов  
 Классы могут быть объявлены как [запечатанные](../../../csharp/language-reference/keywords/sealed.md) путем помещения ключевого слова `sealed` перед определением класса. Пример:  
  
 [!code-cs[csProgGuideInheritance#16](../../../csharp/programming-guide/classes-and-structs/codesnippet/CSharp/abstract-and-sealed-classes-and-class-members_4.cs)]  
  
 Запечатанный класс не может использоваться в качестве базового класса. Поэтому он также не может быть абстрактным классом. Запечатанные классы предотвращают наследование. Поскольку их нельзя использовать в качестве базовых классов, определенная оптимизация во время выполнения позволяет несколько ускорить вызов членов запечатанных классов.  
  
 Метод, индексатор, свойство или событие для производного класса, переопределяющего виртуальный член базового класса, может объявлять этот член как запечатанный. Это делает бесполезным виртуальный аспект члена для каждого последующего производного класса. Для этого в объявлении члена класса необходимо перед ключевым словом [override](../../../csharp/language-reference/keywords/override.md) поместить ключевое слово `sealed`. Пример:  
  
 [!code-cs[csProgGuideInheritance#17](../../../csharp/programming-guide/classes-and-structs/codesnippet/CSharp/abstract-and-sealed-classes-and-class-members_5.cs)]  
  
## <a name="see-also"></a>См. также  
 [Руководство по программированию на C#](../../../csharp/programming-guide/index.md)   
 [Классы и структуры](../../../csharp/programming-guide/classes-and-structs/index.md)   
 [Наследование](../../../csharp/programming-guide/classes-and-structs/inheritance.md)   
 [Methods (C# Programming Guide)](../../../csharp/programming-guide/classes-and-structs/methods.md)  (Методы (руководство по программированию на C#)  
 [Поля](../../../csharp/programming-guide/classes-and-structs/fields.md)   
 [Практическое руководство. Определение абстрактных свойств](../../../csharp/programming-guide/classes-and-structs/how-to-define-abstract-properties.md)

