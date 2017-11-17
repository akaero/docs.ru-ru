---
title: "Практическое руководство. Управление версиями с помощью ключевых слов \"Override\" и \"New\" (Руководство по программированию в C#)"
ms.date: 2015-07-20
ms.prod: .net
ms.technology:
- devlang-csharp
ms.topic: article
dev_langs:
- CSharp
helpviewer_keywords:
- C# language, versioning
- C# language, override and new
ms.assetid: 88247d07-bd0d-49e9-a619-45ccbbfdf0c5
caps.latest.revision: 25
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
ms.openlocfilehash: 167262f7b2423fffec61b1e903f849d2ab387ed2
ms.contentlocale: ru-ru
ms.lasthandoff: 07/28/2017

---
# <a name="versioning-with-the-override-and-new-keywords-c-programming-guide"></a>Практическое руководство. Управление версиями с помощью ключевых слов "Override" и "New" (Руководство по программированию в C#)
Язык C# построен таким образом, что управление версиями [базовых](../../../csharp/language-reference/keywords/base.md) и производных классов в различных библиотеках может включать сохранение и расширение обратной совместимости. Это означает, например, то, что C# полностью поддерживает введение в базовый [класс](../../../csharp/language-reference/keywords/class.md) нового члена с таким же именем, как у члена производного класса, и никакое непредвиденное поведение при этом не возникает. Кроме того, это значит, что класс должен прямо указывать, в каких случаях метод будет перезаписывать унаследованный метод, а в каких он будет становиться новым методом, скрывающим одноименный унаследованный метод.  
  
 В C# производные классы могут содержать методы с такими же именами, как у методов базового класса.  
  
-   Метод базового класса должен быть определен как [виртуальный](../../../csharp/language-reference/keywords/virtual.md).  
  
-   Если методу в производном классе не предшествуют ключевые слова [new](../../../csharp/language-reference/keywords/new.md) или [override](../../../csharp/language-reference/keywords/override.md), компилятор выдаст предупреждение, а метод будет вести себя так, как если бы имелось ключевое слово `new`.  
  
-   Если методу в производном классе предшествует ключевое слово `new`, он определяется как независимый от метода в базовом классе.  
  
-   Если методу в производном классе предшествует ключевое слово `override`, объекты производного класса вызывают этот метод вместо метода базового класса.  
  
-   Метод базового класса можно вызывать из производного класса с помощью ключевого слова `base`.  
  
-   Ключевые слова `override`, `virtual` и`new` можно также применять к свойствам, индексаторам и событиям.  
  
 По умолчанию методы C# не являются виртуальными. Если метод объявлен как виртуальный, любой наследующий его класс может реализовать свою собственную версию. Чтобы сделать метод виртуальным, в объявление метода базового класса добавляется модификатор `virtual`. После этого производный класс может переопределить базовый виртуальный метод с помощью ключевого слова `override` или скрыть виртуальный метод в базовом классе с помощью ключевого слова `new`. Если ключевое слово `override` или `new` не указано, компилятор выдает предупреждение, а метод в производном классе скрывает метод в базовом классе.  
  
 Чтобы продемонстрировать это на практике, предположим, что компания А создала класс с именем `GraphicsClass`, который использует ваша программа. Ниже показан `GraphicsClass`:  
  
 [!code-cs[csProgGuideInheritance#27](../../../csharp/programming-guide/classes-and-structs/codesnippet/CSharp/versioning-with-the-override-and-new-keywords_1.cs)]  
  
 Ваша компания использует этот класс, на основе которого вы создаете собственный производный класс, добавляя новый метод:  
  
 [!code-cs[csProgGuideInheritance#28](../../../csharp/programming-guide/classes-and-structs/codesnippet/CSharp/versioning-with-the-override-and-new-keywords_2.cs)]  
  
 Ваше приложение работает, пока компания А не выпускает новую версию `GraphicsClass`, напоминающую следующий код:  
  
 [!code-cs[csProgGuideInheritance#29](../../../csharp/programming-guide/classes-and-structs/codesnippet/CSharp/versioning-with-the-override-and-new-keywords_3.cs)]  
  
 Новая версия `GraphicsClass` содержит метод с именем `DrawRectangle`. Сначала ничего не происходит. Новая версия по-прежнему совместима со старой версией на уровне двоичного кода. Любая развернутая вами программа будет работать как раньше, даже если в системе соответствующего компьютера установлен новый класс. Существующие вызовы метода `DrawRectangle` будут и дальше ссылаться на вашу версию в производном классе.  
  
 Однако, как только вы выполните повторную компиляцию приложения с новой версией `GraphicsClass`, компилятор выдаст предупреждение CS0108. В предупреждении будет сказано, что вам необходимо указать, каким образом метод `DrawRectangle` будет вести себя в приложении.  
  
 Если метод должен переопределять новый метод базового класса, используйте ключевое слово `override`:  
  
 [!code-cs[csProgGuideInheritance#30](../../../csharp/programming-guide/classes-and-structs/codesnippet/CSharp/versioning-with-the-override-and-new-keywords_4.cs)]  
  
 Ключевое слово `override` гарантирует, что все объекты, производные от `YourDerivedGraphicsClass`, будут использовать версию производного класса `DrawRectangle`. Объекты, производные от `YourDerivedGraphicsClass`, сохраняют доступ к версии базового класса `DrawRectangle` за счет ключевого слова base:  
  
 [!code-cs[csProgGuideInheritance#44](../../../csharp/programming-guide/classes-and-structs/codesnippet/CSharp/versioning-with-the-override-and-new-keywords_5.cs)]  
  
 Если вы не хотите, чтобы ваш метод переопределял новый метод базового класса, воспользуйтесь следующими рекомендациями. Чтобы избежать путаницы между двумя методами, вы можете переименовать свой метод. Эта работа требует времени и подвержена ошибкам, а в некоторых случаях непрактична. В то же время, если проект относительно небольшой, метод можно переименовать, используя параметры рефакторинга в Visual Studio. Дополнительные сведения см. в разделе [Рефакторинг классов и типов (конструктор классов)](/visualstudio/ide/refactoring-classes-and-types-class-designer).  
  
 Кроме того, предупреждения можно избежать с помощью ключевого слова `new` в определении производного класса:  
  
 [!code-cs[csProgGuideInheritance#31](../../../csharp/programming-guide/classes-and-structs/codesnippet/CSharp/versioning-with-the-override-and-new-keywords_6.cs)]  
  
 Ключевое слово `new` сообщает компилятору о том, что ваше определение скрывает определение, которое содержится в базовом классе. Это поведение установлено по умолчанию.  
  
## <a name="override-and-method-selection"></a>Переопределение и выбор метода  
 Если метод в классе именован, с вызовом совместимы сразу несколько методов (например, если существуют два метода с одинаковыми именами), а параметры совместимы с переданным параметром, компилятор C# выбирает метод, наиболее подходящий для вызова. Пример совместимых методов:  
  
 [!code-cs[csProgGuideInheritance#32](../../../csharp/programming-guide/classes-and-structs/codesnippet/CSharp/versioning-with-the-override-and-new-keywords_7.cs)]  
  
 При вызове `DoWork` к экземпляру `Derived` компилятор C#, в первую очередь, пытается сделать вызов совместимым с версиями `DoWork`, изначально объявленными в `Derived`. Методы переопределения не считаются объявленными в классе, они представляют собой новые реализации метода, объявленного в базовом классе. Если же компилятор C# не может сопоставить вызов метода с исходным методом в `Derived`, он пытается сопоставить его с переопределенным методом, имеющим такое же имя и совместимые параметры. Например:  
  
 [!code-cs[csProgGuideInheritance#33](../../../csharp/programming-guide/classes-and-structs/codesnippet/CSharp/versioning-with-the-override-and-new-keywords_8.cs)]  
  
 Поскольку переменная `val` может быть преобразована в значение double неявно, компилятор C# вызывает `DoWork(double)`, а не `DoWork(int)`. Избежать этого можно двумя способами. Во-первых, избегайте объявления новых методов, имена которых совпадают с виртуальными методами. Во-вторых, можно настроить компилятор C# на вызов виртуального метода, заставив его выполнить поиск метода базового класса путем приведения экземпляра `Derived` к `Base`. Поскольку метод виртуальный, будет вызвана реализация `DoWork(int)` в `Derived`. Пример:  
  
 [!code-cs[csProgGuideInheritance#34](../../../csharp/programming-guide/classes-and-structs/codesnippet/CSharp/versioning-with-the-override-and-new-keywords_9.cs)]  
  
 Дополнительные примеры `new` и `override` см. в разделе [Использование ключевых слов Override и New](../../../csharp/programming-guide/classes-and-structs/knowing-when-to-use-override-and-new-keywords.md).  
  
## <a name="see-also"></a>См. также  
 [Руководство по программированию на C#](../../../csharp/programming-guide/index.md)   
 [Классы и структуры](../../../csharp/programming-guide/classes-and-structs/index.md)   
 [Методы](../../../csharp/programming-guide/classes-and-structs/methods.md)   
 [Наследование](../../../csharp/programming-guide/classes-and-structs/inheritance.md)

