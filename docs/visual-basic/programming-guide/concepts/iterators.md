---
title: "Итераторы (Visual Basic) | Документы Microsoft"
ms.custom: 
ms.date: 2015-07-20
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology:
- devlang-visual-basic
ms.topic: article
dev_langs:
- VB
ms.assetid: f26b5c1e-fe9d-4004-b287-da7919d717ae
caps.latest.revision: 3
author: stevehoag
ms.author: shoag
translation.priority.mt:
- cs-cz
- pl-pl
- pt-br
- tr-tr
translationtype: Machine Translation
ms.sourcegitcommit: a06bd2a17f1d6c7308fa6337c866c1ca2e7281c0
ms.openlocfilehash: 4ea1e21bd8cc392889c477e78338384ed05d4cbb
ms.lasthandoff: 03/13/2017

---
# <a name="iterators-visual-basic"></a>Итераторы (Visual Basic)
*Итератор* можно использовать для прохода по коллекции, такие как списки и массивы.  
  
 Метод итератора или `get` доступа выполняющий настраиваемую итерацию по коллекции. Использует метод итератора [Yield](../../../visual-basic/language-reference/statements/yield-statement.md) инструкции для возврата каждого элемента одному за раз. При `Yield` к оператору запоминается текущее расположение в коде. Выполнение возобновляется с этого места при очередном вызове функции итератора.  
  
 Использовать итератор из клиентского кода с помощью [For Each... Далее](../../../visual-basic/language-reference/statements/for-each-next-statement.md) оператор, или с помощью запроса LINQ.  
  
 В следующем примере первой итерации `For Each` цикл приводит к выполнению перейти в `SomeNumbers` метода итератора до первого `Yield` к оператору. Этой итерации возвращает значение 3, и сохраняется текущее расположение в методе итератора. На следующей итерации цикла, выполнение в методе итератора продолжается с места останавливается, еще раз при достижении `Yield` инструкции. Этой итерации возвращает значение 5, и еще раз сохраняется текущее расположение в методе итератора. При достижении конца метода итератора завершения цикла.  
  
```vb  
Sub Main()  
    For Each number As Integer In SomeNumbers()  
        Console.Write(number & " ")  
    Next  
    ' Output: 3 5 8  
    Console.ReadKey()  
End Sub  
  
Private Iterator Function SomeNumbers() As System.Collections.IEnumerable  
    Yield 3  
    Yield 5  
    Yield 8  
End Function  
```  
  
 Возвращаемый тип метода итератора или `get` доступа может быть <xref:System.Collections.IEnumerable>, <xref:System.Collections.Generic.IEnumerable%601>, <xref:System.Collections.IEnumerator>, или <xref:System.Collections.Generic.IEnumerator%601>.</xref:System.Collections.Generic.IEnumerator%601> </xref:System.Collections.IEnumerator> </xref:System.Collections.Generic.IEnumerable%601> </xref:System.Collections.IEnumerable>  
  
 Можно использовать `Exit Function` или `Return` инструкции для завершения итерации.  
  
 Итератор функции Visual Basic или `get` объявление метода доступа включает [итератор](../../../visual-basic/language-reference/modifiers/iterator.md) модификатор.  
  
 Итераторы появились в Visual Basic в Visual Studio 2012.  
  
 **Содержание раздела**  
  
-   [Простой итератор](#BKMK_SimpleIterator)  
  
-   [Создание класса коллекции](#BKMK_CollectionClass)  
  
-   [Блоки try](#BKMK_TryBlocks)  
  
-   [Анонимные методы](#BKMK_AnonymousMethods)  
  
-   [Использование итераторов с универсальный список](#BKMK_GenericList)  
  
-   [Сведения о синтаксисе](#BKMK_SyntaxInformation)  
  
-   [Техническая реализация](#BKMK_Technical)  
  
-   [Использование итераторов](#BKMK_UseOfIterators)  
  
> [!NOTE]
>  Все примеры в разделе, за исключением того, в примере простой итератор включают [импорта](../../../visual-basic/language-reference/statements/imports-statement-net-namespace-and-type.md) инструкции для `System.Collections` и `System.Collections.Generic` пространства имен.  
  
##  <a name="BKMK_SimpleIterator"></a>Простой итератор  
 В следующем примере имеется один `Yield` оператор, находящийся внутри [для... Далее](../../../visual-basic/language-reference/statements/for-next-statement.md) цикла. В `Main`, каждая итерация `For Each` тела оператора создает вызов функции итератора, который переходит к следующему `Yield` инструкции.  
  
```vb  
Sub Main()  
    For Each number As Integer In EvenSequence(5, 18)  
        Console.Write(number & " ")  
    Next  
    ' Output: 6 8 10 12 14 16 18  
    Console.ReadKey()  
End Sub  
  
Private Iterator Function EvenSequence(  
ByVal firstNumber As Integer, ByVal lastNumber As Integer) _  
As System.Collections.Generic.IEnumerable(Of Integer)  
  
    ' Yield even numbers in the range.  
    For number As Integer = firstNumber To lastNumber  
        If number Mod 2 = 0 Then  
            Yield number  
        End If  
    Next  
End Function  
```  
  
##  <a name="BKMK_CollectionClass"></a>Создание класса коллекции  
 В следующем примере `DaysOfTheWeek` реализует <xref:System.Collections.IEnumerable>интерфейс, который требует <xref:System.Collections.IEnumerable.GetEnumerator%2A>метод.</xref:System.Collections.IEnumerable.GetEnumerator%2A> </xref:System.Collections.IEnumerable> Компилятор неявно вызывает `GetEnumerator` метод, возвращающий <xref:System.Collections.IEnumerator>.</xref:System.Collections.IEnumerator>  
  
 `GetEnumerator` Метод возвращает каждую строку, одним одновременно с помощью `Yield` инструкции и `Iterator` является модификатором в объявлении функции.  
  
```vb  
Sub Main()  
    Dim days As New DaysOfTheWeek()  
    For Each day As String In days  
        Console.Write(day & " ")  
    Next  
    ' Output: Sun Mon Tue Wed Thu Fri Sat  
    Console.ReadKey()  
End Sub  
  
Private Class DaysOfTheWeek  
    Implements IEnumerable  
  
    Public days =  
        New String() {"Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"}  
  
    Public Iterator Function GetEnumerator() As IEnumerator _  
        Implements IEnumerable.GetEnumerator  
  
        ' Yield each day of the week.  
        For i As Integer = 0 To days.Length - 1  
            Yield days(i)  
        Next  
    End Function  
End Class  
```  
  
 В следующем примере создается `Zoo` класс, содержащий коллекцию животных.  
  
 `For Each` Оператору, который ссылается на экземпляр класса (`theZoo`) неявно вызывает `GetEnumerator` метод. `For Each` Инструкций, ссылающихся на `Birds` и `Mammals` используют свойства `AnimalsForType` с именем метода итератора.  
  
```vb  
Sub Main()  
    Dim theZoo As New Zoo()  
  
    theZoo.AddMammal("Whale")  
    theZoo.AddMammal("Rhinoceros")  
    theZoo.AddBird("Penguin")  
    theZoo.AddBird("Warbler")  
  
    For Each name As String In theZoo  
        Console.Write(name & " ")  
    Next  
    Console.WriteLine()  
    ' Output: Whale Rhinoceros Penguin Warbler  
  
    For Each name As String In theZoo.Birds  
        Console.Write(name & " ")  
    Next  
    Console.WriteLine()  
    ' Output: Penguin Warbler  
  
    For Each name As String In theZoo.Mammals  
        Console.Write(name & " ")  
    Next  
    Console.WriteLine()  
    ' Output: Whale Rhinoceros  
  
    Console.ReadKey()  
End Sub  
  
Public Class Zoo  
    Implements IEnumerable  
  
    ' Private members.  
    Private animals As New List(Of Animal)  
  
    ' Public methods.  
    Public Sub AddMammal(ByVal name As String)  
        animals.Add(New Animal With {.Name = name, .Type = Animal.TypeEnum.Mammal})  
    End Sub  
  
    Public Sub AddBird(ByVal name As String)  
        animals.Add(New Animal With {.Name = name, .Type = Animal.TypeEnum.Bird})  
    End Sub  
  
    Public Iterator Function GetEnumerator() As IEnumerator _  
        Implements IEnumerable.GetEnumerator  
  
        For Each theAnimal As Animal In animals  
            Yield theAnimal.Name  
        Next  
    End Function  
  
    ' Public members.  
    Public ReadOnly Property Mammals As IEnumerable  
        Get  
            Return AnimalsForType(Animal.TypeEnum.Mammal)  
        End Get  
    End Property  
  
    Public ReadOnly Property Birds As IEnumerable  
        Get  
            Return AnimalsForType(Animal.TypeEnum.Bird)  
        End Get  
    End Property  
  
    ' Private methods.  
    Private Iterator Function AnimalsForType( _  
    ByVal type As Animal.TypeEnum) As IEnumerable  
        For Each theAnimal As Animal In animals  
            If (theAnimal.Type = type) Then  
                Yield theAnimal.Name  
            End If  
        Next  
    End Function  
  
    ' Private class.  
    Private Class Animal  
        Public Enum TypeEnum  
            Bird  
            Mammal  
        End Enum  
  
        Public Property Name As String  
        Public Property Type As TypeEnum  
    End Class  
End Class  
```  
  
##  <a name="BKMK_TryBlocks"></a>Блоки try  
 Visual Basic позволяет `Yield` инструкции в `Try` блока [Try... CATCH... Оператор Finally](../../../visual-basic/language-reference/statements/try-catch-finally-statement.md). Объект `Try` блок, который имеет `Yield` инструкция может иметь `Catch` блокируется и может иметь `Finally` блок.  
  
 Следующий пример включает `Try`, `Catch`, и `Finally` блоков в функции итератора. `Finally` Блок в функции итератора, выполняется перед `For Each` завершения итерации.  
  
```vb  
Sub Main()  
    For Each number As Integer In Test()  
        Console.WriteLine(number)  
    Next  
    Console.WriteLine("For Each is done.")  
  
    ' Output:  
    '  3  
    '  4  
    '  Something happened. Yields are done.  
    '  Finally is called.  
    '  For Each is done.  
    Console.ReadKey()  
End Sub  
  
Private Iterator Function Test() As IEnumerable(Of Integer)  
    Try  
        Yield 3  
        Yield 4  
        Throw New Exception("Something happened. Yields are done.")  
        Yield 5  
        Yield 6  
    Catch ex As Exception  
        Console.WriteLine(ex.Message)  
    Finally  
        Console.WriteLine("Finally is called.")  
    End Try  
End Function  
```  
  
 Объект `Yield` оператор не может находиться внутри `Catch` блока или `Finally` блока.  
  
 Если `For Each` текст (вместо метода итератора) вызывает исключение, `Catch` блок в функции итератора не выполняется, но `Finally` выполняется блок в функции итератора. A `Catch` блок внутри функции итератора перехватывает только исключения, которые возникают внутри функции итератора.  
  
##  <a name="BKMK_AnonymousMethods"></a>Анонимные методы  
 В Visual Basic анонимная функция может быть функцией итератора. Это показано в следующем примере.  
  
```vb  
Dim iterateSequence = Iterator Function() _  
                      As IEnumerable(Of Integer)  
                          Yield 1  
                          Yield 2  
                      End Function  
  
For Each number As Integer In iterateSequence()  
    Console.Write(number & " ")  
Next  
' Output: 1 2  
Console.ReadKey()  
```  
  
 В следующем примере имеется метод не итератор, который проверяет аргументы. Метод возвращает результат анонимный итератора, который описывает элементы коллекции.  
  
```vb  
Sub Main()  
    For Each number As Integer In GetSequence(5, 10)  
        Console.Write(number & " ")  
    Next  
    ' Output: 5 6 7 8 9 10  
    Console.ReadKey()  
End Sub  
  
Public Function GetSequence(ByVal low As Integer, ByVal high As Integer) _  
As IEnumerable  
    ' Validate the arguments.  
    If low < 1 Then  
        Throw New ArgumentException("low is too low")  
    End If  
    If high > 140 Then  
        Throw New ArgumentException("high is too high")  
    End If  
  
    ' Return an anonymous iterator function.  
    Dim iterateSequence = Iterator Function() As IEnumerable  
                              For index = low To high  
                                  Yield index  
                              Next  
                          End Function  
    Return iterateSequence()  
End Function  
```  
  
 Если проверка находится внутри функции итератора, проверка не может выполняться до начала первой итерации `For Each` текста.  
  
##  <a name="BKMK_GenericList"></a>Использование итераторов с универсальный список  
 В следующем примере `Stack(Of T)` реализует универсальный класс <xref:System.Collections.Generic.IEnumerable%601>универсальный интерфейс.</xref:System.Collections.Generic.IEnumerable%601> `Push` Метод присваивает значения массивом типа `T`. <xref:System.Collections.Generic.IEnumerable%601.GetEnumerator%2A>Метод возвращает массив значений с помощью `Yield` инструкции.</xref:System.Collections.Generic.IEnumerable%601.GetEnumerator%2A>  
  
 Помимо универсальный <xref:System.Collections.Generic.IEnumerable%601.GetEnumerator%2A>метод, неуниверсальные <xref:System.Collections.IEnumerable.GetEnumerator%2A>также должен быть реализован метод.</xref:System.Collections.IEnumerable.GetEnumerator%2A> </xref:System.Collections.Generic.IEnumerable%601.GetEnumerator%2A> Это обусловлено <xref:System.Collections.Generic.IEnumerable%601>наследует <xref:System.Collections.IEnumerable>.</xref:System.Collections.IEnumerable> </xref:System.Collections.Generic.IEnumerable%601> Реализация неуниверсального откладывает универсальные реализации.  
  
 В примере именованные итераторы для поддержки различных возможностей перебора одной коллекции данных. Эти итераторы с именем `TopToBottom` и `BottomToTop` свойства и `TopN` метод.  
  
 `BottomToTop` Содержит объявление свойства `Iterator` ключевое слово.  
  
```vb  
Sub Main()  
    Dim theStack As New Stack(Of Integer)  
  
    ' Add items to the stack.  
    For number As Integer = 0 To 9  
        theStack.Push(number)  
    Next  
  
    ' Retrieve items from the stack.  
    ' For Each is allowed because theStack implements  
    ' IEnumerable(Of Integer).  
    For Each number As Integer In theStack  
        Console.Write("{0} ", number)  
    Next  
    Console.WriteLine()  
    ' Output: 9 8 7 6 5 4 3 2 1 0  
  
    ' For Each is allowed, because theStack.TopToBottom  
    ' returns IEnumerable(Of Integer).  
    For Each number As Integer In theStack.TopToBottom  
        Console.Write("{0} ", number)  
    Next  
    Console.WriteLine()  
    ' Output: 9 8 7 6 5 4 3 2 1 0  
  
    For Each number As Integer In theStack.BottomToTop  
        Console.Write("{0} ", number)  
    Next  
    Console.WriteLine()  
    ' Output: 0 1 2 3 4 5 6 7 8 9   
  
    For Each number As Integer In theStack.TopN(7)  
        Console.Write("{0} ", number)  
    Next  
    Console.WriteLine()  
    ' Output: 9 8 7 6 5 4 3  
  
    Console.ReadKey()  
End Sub  
  
Public Class Stack(Of T)  
    Implements IEnumerable(Of T)  
  
    Private values As T() = New T(99) {}  
    Private top As Integer = 0  
  
    Public Sub Push(ByVal t As T)  
        values(top) = t  
        top = top + 1  
    End Sub  
  
    Public Function Pop() As T  
        top = top - 1  
        Return values(top)  
    End Function  
  
    ' This function implements the GetEnumerator method. It allows  
    ' an instance of the class to be used in a For Each statement.  
    Public Iterator Function GetEnumerator() As IEnumerator(Of T) _  
        Implements IEnumerable(Of T).GetEnumerator  
  
        For index As Integer = top - 1 To 0 Step -1  
            Yield values(index)  
        Next  
    End Function  
  
    Public Iterator Function GetEnumerator1() As IEnumerator _  
        Implements IEnumerable.GetEnumerator  
  
        Yield GetEnumerator()  
    End Function  
  
    Public ReadOnly Property TopToBottom() As IEnumerable(Of T)  
        Get  
            Return Me  
        End Get  
    End Property  
  
    Public ReadOnly Iterator Property BottomToTop As IEnumerable(Of T)  
        Get  
            For index As Integer = 0 To top - 1  
                Yield values(index)  
            Next  
        End Get  
    End Property  
  
    Public Iterator Function TopN(ByVal itemsFromTop As Integer) _  
        As IEnumerable(Of T)  
  
        ' Return less than itemsFromTop if necessary.  
        Dim startIndex As Integer =  
            If(itemsFromTop >= top, 0, top - itemsFromTop)  
  
        For index As Integer = top - 1 To startIndex Step -1  
            Yield values(index)  
        Next  
    End Function  
End Class  
  
```  
  
##  <a name="BKMK_SyntaxInformation"></a>Сведения о синтаксисе  
 Итератор может возникать как метод или `get` доступа. Итератор не может содержаться событие, конструктор экземпляра, статический конструктор или статический деструктор.  
  
 Должно существовать неявное преобразование из типа выражения в `Yield` инструкции в возвращаемый тип итератора.  
  
 В Visual Basic метод итератора не может содержать `ByRef` параметров.  
  
 В Visual Basic «Доход» не является зарезервированным словом и имеет специальное значение только в том случае, если он используется в `Iterator` метода или `get` доступа.  
  
##  <a name="BKMK_Technical"></a>Техническая реализация  
 Несмотря на то, что итератор создается как метод, компилятор преобразует его в вложенного класса и в результате, конечный автомат. Этот класс данные о позиции итератора, как долго `For Each...Next` продолжает цикл в коде клиента.  
  
 Чтобы увидеть, компилятор выполняет, можно использовать средство Ildasm.exe для отображения кода промежуточного языка Microsoft, созданный для метода итератора.  
  
 При создании итератора для [класса](../../../csharp/language-reference/keywords/class.md) или [структуры](../../../csharp/language-reference/keywords/struct.md), необходимо реализовать весь <xref:System.Collections.IEnumerator>интерфейса.</xref:System.Collections.IEnumerator> Когда компилятор обнаруживает итератора, он автоматически создает `Current`, `MoveNext`, и `Dispose` методов <xref:System.Collections.IEnumerator>или <xref:System.Collections.Generic.IEnumerator%601>интерфейса.</xref:System.Collections.Generic.IEnumerator%601> </xref:System.Collections.IEnumerator>  
  
 В последующих итерациях из `For Each…Next` цикла (или прямом вызове `IEnumerator.MoveNext`), следующий текст кода итератора возобновляется после предыдущего `Yield` инструкции. Перейдет к следующему `Yield` инструкции, пока не будет достигнут конец тела итератора, пока не `Exit Function` или `Return` встречается оператор.  
  
 Итераторы не поддерживают <xref:System.Collections.IEnumerator.Reset%2A?displayProperty=fullName>метод.</xref:System.Collections.IEnumerator.Reset%2A?displayProperty=fullName> Для повторной итерации с самого начала, необходимо получить новый итератор.  
  
 Дополнительные сведения см. в разделе [спецификация языка Visual Basic](../../../visual-basic/reference/language-specification.md).  
  
##  <a name="BKMK_UseOfIterators"></a>Использование итераторов  
 Итераторы позволяют поддерживать простоту `For Each` цикл, когда необходимо использовать сложный код для заполнения списка последовательности. Это может быть полезно, когда необходимо сделать следующее:  
  
-   Изменение последовательности список после первого `For Each` итерации цикла.  
  
-   Избежать полной загрузки большого списка перед первой итерацией `For Each` цикла. Например, разбитых на страницы выборки для загрузки пакета строк таблицы. Другой пример — <xref:System.IO.DirectoryInfo.EnumerateFiles%2A>метод, который реализует итераторы в .NET Framework.</xref:System.IO.DirectoryInfo.EnumerateFiles%2A>  
  
-   Инкапсуляция, Создание списка в итераторе. В методе итератора можно составить список и затем выдает каждый результат в цикле.  
  
## <a name="see-also"></a>См. также  
 <xref:System.Collections.Generic></xref:System.Collections.Generic>   
 <xref:System.Collections.Generic.IEnumerable%601></xref:System.Collections.Generic.IEnumerable%601>   
 [Для каждого... Следующий оператор](../../../visual-basic/language-reference/statements/for-each-next-statement.md)   
 [Оператор yield](../../../visual-basic/language-reference/statements/yield-statement.md)   
 [Iterator](../../../visual-basic/language-reference/modifiers/iterator.md)