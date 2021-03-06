---
title: "Ограничения на использование уровней доступности (справочник по C#)"
ms.date: 2015-07-20
ms.prod: .net
ms.technology:
- devlang-csharp
ms.topic: article
dev_langs:
- CSharp
helpviewer_keywords:
- access modifiers [C#], accessibility level restrictions
ms.assetid: 987e2f22-46bf-4fea-80ee-270b9cd01045
caps.latest.revision: 21
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
ms.sourcegitcommit: 306c608dc7f97594ef6f72ae0f5aaba596c936e1
ms.openlocfilehash: 8e49afd38fd776593b87f065a079da0d546df4a6
ms.contentlocale: ru-ru
ms.lasthandoff: 09/25/2017

---
# <a name="restrictions-on-using-accessibility-levels-c-reference"></a>Ограничения на использование уровней доступности (справочник по C#)
При задании типа в объявлении необходимо проверить, зависит ли уровень доступности типа от уровня доступности члена или другого типа. Например, прямой базовый класс должен иметь по крайней мере такой же уровень доступности, как и производный класс. Следующие объявления вызывают ошибку компиляции, так как базовый класс `BaseClass` менее доступен, чем `MyClass`:  
  
```  
class BaseClass {...}  
public class MyClass: BaseClass {...} // Error  
```  
  
 В таблице ниже приведены все ограничения на объявленные уровни доступности.  
  
|Контекст|Примечания|  
|-------------|-------------|  
|[Классы](../../../csharp/programming-guide/classes-and-structs/classes.md)|Прямой базовый класс для типа класса должен иметь по крайней мере такой же уровень доступности, как и сам тип класса.|  
|[Интерфейсы](../../../csharp/programming-guide/interfaces/index.md)|Явные базовые интерфейсы для типа интерфейса должны иметь по крайней мере такой же уровень доступности, как и сам тип интерфейса.|  
|[Делегаты](../../../csharp/programming-guide/delegates/index.md)|Тип возвращаемого значения и типы параметров для типа делегата должны иметь по крайней мере такой же уровень доступности, как и сам тип делегата.|  
|[Константы](../../../csharp/programming-guide/classes-and-structs/constants.md)|Тип константы должен иметь по крайней мере такой же уровень доступности, как и сама константа.|  
|[Поля](../../../csharp/programming-guide/classes-and-structs/fields.md)|Тип поля должен иметь по крайней мере такой же уровень доступности, как и само поле.|  
|[Методы](../../../csharp/programming-guide/classes-and-structs/methods.md)|Тип возвращаемого значения и типы параметров для метода должны иметь по крайней мере такой же уровень доступности, как и сам метод.|  
|[Свойства](../../../csharp/programming-guide/classes-and-structs/properties.md)|Тип свойства должен иметь по крайней мере такой же уровень доступности, как и само свойство.|  
|[События](../../../csharp/programming-guide/events/index.md)|Тип события должен иметь по крайней мере такой же уровень доступности, как и само событие.|  
|[Индексаторы](../../../csharp/programming-guide/indexers/index.md)|Тип и типы параметров для индексатора должны иметь по крайней мере такой же уровень доступности, как и сам индексатор.|  
|[Операторы](../../../csharp/programming-guide/statements-expressions-operators/operators.md)|Тип возвращаемого значения и типы параметров для оператора должны иметь по крайней мере такой же уровень доступности, как и сам оператор.|  
|[Конструкторы](../../../csharp/programming-guide/classes-and-structs/constructors.md)|Типы параметров для конструктора должны иметь по крайней мере такой же уровень доступности, как и сам конструктор.|  
  
## <a name="example"></a>Пример  
 В приведенном ниже примере содержатся ошибочные объявления различных типов. В комментарии после каждого объявления указывается предполагаемая ошибка компиляции.  
  
```  
// Restrictions on Using Accessibility Levels  
// CS0052 expected as well as CS0053, CS0056, and CS0057  
// To make the program work, change access level of both class B  
// and MyPrivateMethod() to public.  
  
using System;  
  
// A delegate:  
delegate int MyDelegate();  
  
class B  
{  
    // A private method:  
    static int MyPrivateMethod()  
    {  
        return 0;  
    }  
}  
  
public class A  
{  
    // Error: The type B is less accessible than the field A.myField.  
    public B myField = new B();  
  
    // Error: The type B is less accessible  
    // than the constant A.myConst.  
    public readonly B myConst = new B();  
  
    public B MyMethod()  
    {  
        // Error: The type B is less accessible   
        // than the method A.MyMethod.  
        return new B();  
    }  
  
    // Error: The type B is less accessible than the property A.MyProp  
    public B MyProp  
    {  
        set  
        {  
        }  
    }  
  
    MyDelegate d = new MyDelegate(B.MyPrivateMethod);  
    // Even when B is declared public, you still get the error:   
    // "The parameter B.MyPrivateMethod is not accessible due to   
    // protection level."  
  
    public static B operator +(A m1, B m2)  
    {  
        // Error: The type B is less accessible  
        // than the operator A.operator +(A,B)  
        return new B();  
    }  
  
    static void Main()  
    {  
        Console.Write("Compiled successfully");  
    }  
}  
```  
  
## <a name="c-language-specification"></a>Спецификация языка C#  
 [!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]  
  
## <a name="see-also"></a>См. также  
 [Справочник по C#](../../../csharp/language-reference/index.md)   
 [Руководство по программированию на C#](../../../csharp/programming-guide/index.md)   
 [Ключевые слова в C#](../../../csharp/language-reference/keywords/index.md)   
 [Модификаторы доступа](../../../csharp/language-reference/keywords/access-modifiers.md)   
 [Домен доступности](../../../csharp/language-reference/keywords/accessibility-domain.md)   
 [Уровни доступности](../../../csharp/language-reference/keywords/accessibility-levels.md)   
 [Модификаторы доступа](../../../csharp/programming-guide/classes-and-structs/access-modifiers.md)   
 [public](../../../csharp/language-reference/keywords/public.md)   
 [private](../../../csharp/language-reference/keywords/private.md)   
 [protected](../../../csharp/language-reference/keywords/protected.md)   
 [internal](../../../csharp/language-reference/keywords/internal.md)

