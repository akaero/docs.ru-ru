---
title: "Практическое руководство. Выполнение запроса к метаданным сборки при помощи отражения (LINQ) (C#)"
ms.custom: 
ms.date: 2015-07-20
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology:
- devlang-csharp
ms.topic: article
dev_langs:
- CSharp
ms.assetid: c4cdce49-b1c8-4420-b12a-9ff7e6671368
caps.latest.revision: 3
author: BillWagner
ms.author: wiwagn
ms.translationtype: HT
ms.sourcegitcommit: 306c608dc7f97594ef6f72ae0f5aaba596c936e1
ms.openlocfilehash: 181d72db228bbc43c00ce3d3266fde8e1d3324e9
ms.contentlocale: ru-ru
ms.lasthandoff: 07/28/2017

---
# <a name="how-to-query-an-assembly39s-metadata-with-reflection-linq-c"></a>Практическое руководство. Выполнение запроса к метаданным сборки при помощи отражения (LINQ) (C#)
В следующем примере показано использование LINQ с отражением для извлечения определенных метаданных о методах, соответствующих условиям поиска. В этом примере запрос будет искать имена всех методов в сборке, которые возвращают перечислимые типы, такие как массивы.  
  
## <a name="example"></a>Пример  
  
```csharp  
using System.Reflection;  
using System.IO;  
namespace LINQReflection  
{  
    class ReflectionHowTO  
    {  
        static void Main(string[] args)  
        {  
            Assembly assembly = Assembly.Load("System.Core, Version=3.5.0.0, Culture=neutral, PublicKeyToken= b77a5c561934e089");  
            var pubTypesQuery = from type in assembly.GetTypes()  
                        where type.IsPublic  
                            from method in type.GetMethods()  
                            where method.ReturnType.IsArray == true   
                                || ( method.ReturnType.GetInterface(  
                                    typeof(System.Collections.Generic.IEnumerable<>).FullName ) != null  
                                && method.ReturnType.FullName != "System.String" )  
                            group method.ToString() by type.ToString();  
  
            foreach (var groupOfMethods in pubTypesQuery)  
            {  
                Console.WriteLine("Type: {0}", groupOfMethods.Key);  
                foreach (var method in groupOfMethods)  
                {  
                    Console.WriteLine("  {0}", method);  
                }  
            }  
  
            Console.WriteLine("Press any key to exit");  
            Console.ReadKey();  
        }  
    }    
}  
```  
  
 В примере используется метод <xref:System.Reflection.Assembly.GetTypes%2A> для возвращения массива типов в указанной сборке. Чтобы вернуть только открытые типы, применяется фильтр [where](../../../../csharp/language-reference/keywords/where-clause.md). Для каждого открытого типа создается вложенный запрос с использованием массива <xref:System.Reflection.MethodInfo>, который возвращается из вызова <xref:System.Type.GetMethods%2A>. Эти результаты фильтруются для возвращения только тех методов, возвращаемый тип которых является массивом или типом, который реализует <xref:System.Collections.Generic.IEnumerable%601>. Наконец, эти результаты группируются с помощью имени типа в качестве ключа.  
  
## <a name="compiling-the-code"></a>Компиляция кода  
 Создайте проект, предназначенный для .NET Framework 3.5 или более поздней версии, со ссылкой на библиотеку System.Core.dll и директивами `using` для пространств имен System.Linq и System.IO.  
  
## <a name="see-also"></a>См. также  
 [LINQ to Objects (C#)](../../../../csharp/programming-guide/concepts/linq/linq-to-objects.md)

