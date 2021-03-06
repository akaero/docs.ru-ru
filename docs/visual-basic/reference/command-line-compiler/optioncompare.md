---
title: "Дополнительные | Документы Microsoft"
ms.date: 2015-07-20
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology:
- devlang-visual-basic
ms.topic: article
f1_keywords:
- /optioncompare
dev_langs:
- VB
helpviewer_keywords:
- optioncompare compiler option [Visual Basic]
- -optioncompare compiler option [Visual Basic]
- /optioncompare compiler option [Visual Basic]
ms.assetid: 7237b766-b44d-4cc5-9a3c-885348a7d9e4
caps.latest.revision: 17
author: dotnet-bot
ms.author: dotnetcontent
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
translationtype: Machine Translation
ms.sourcegitcommit: a06bd2a17f1d6c7308fa6337c866c1ca2e7281c0
ms.openlocfilehash: 53dee5bb50e20204725f031e3e04f5c37c5b3f96
ms.lasthandoff: 03/13/2017

---
# <a name="optioncompare"></a>/optioncompare
Задает способ сравнения строк.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
/optioncompare:{binary | text}  
```  
  
## <a name="remarks"></a>Примечания  
 Можно указать `/optioncompare` в одну из двух форм: `/optioncompare:binary` использовать двоичное сравнение строк, и `/optioncompare:text` использовать текстовое сравнение строк. По умолчанию компилятор использует `/optioncompare:binary`.  
  
 В Microsoft Windows используется кодовая страница определяет двоичный порядок сортировки. Двоичный порядок сортировки выглядит следующим образом:  
  
 `A < B < E < Z < a < b < e < z < À < Ê < Ø < à < ê < ø`  
  
 Сравнение текстовых строк основано на порядке сортировки текста без учета регистра определяются языком вашей системы. Порядок сортировки текста обычно выглядит следующим образом:  
  
 `(A = a) < (À = à) < (B=b) < (E=e) < (Ê = ê) < (Z=z) < (Ø = ø)`  
  
### <a name="to-set-optioncompare-in-the-visual-studio-ide"></a>Чтобы задать дополнительные в Интегрированной среде разработки Visual Studio  
  
1.  Выберите проект в **Обозревателе решений**. На **проекта** меню, щелкните **свойства**. Дополнительные сведения см. в разделе [Знакомство с конструктором проектов](http://msdn.microsoft.com/en-us/898dd854-c98d-430c-ba1b-a913ce3c73d7).  
  
2.  Откройте вкладку **Компиляция**.  
  
3.  Измените значение в **Option Compare** поле.  
  
### <a name="to-set-optioncompare-programmatically"></a>Чтобы установить дополнительные программными средствами  
  
-   В разделе [Option Compare-оператор](../../../visual-basic/language-reference/statements/option-compare-statement.md).  
  
## <a name="example"></a>Пример  
 Следующий код компилирует P`rojFile.vb` и использует сравнения двоичных строк.  
  
```  
vbc /optioncompare:binary projFile.vb  
```  
  
## <a name="see-also"></a>См. также  
 [Компилятор командной строки Visual Basic](../../../visual-basic/reference/command-line-compiler/index.md)   
 [/ optionexplicit](../../../visual-basic/reference/command-line-compiler/optionexplicit.md)   
 [Дополнительные сведения](../../../visual-basic/reference/command-line-compiler/optionstrict.md)   
 [/ optioninfer](../../../visual-basic/reference/command-line-compiler/optioninfer.md)   
 [Примеры командных строк компиляции](../../../visual-basic/reference/command-line-compiler/sample-compilation-command-lines.md)   
 [Оператор Option Compare](../../../visual-basic/language-reference/statements/option-compare-statement.md)   
 [Страница "Параметры Visual Basic по умолчанию", папка "Проекты", диалоговое окно "Параметры"](https://docs.microsoft.com/visualstudio/ide/reference/visual-basic-defaults-projects-options-dialog-box)
