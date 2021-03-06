---
title: "Изменения пространства имен System.Uri в версии 2.0"
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
ms.assetid: 35883fe9-2d09-4d8b-80ca-cf23a941e459
caps.latest.revision: 9
author: mcleblanc
ms.author: markl
manager: markl
ms.translationtype: HT
ms.sourcegitcommit: 306c608dc7f97594ef6f72ae0f5aaba596c936e1
ms.openlocfilehash: 7ce81e348b3e5de285a3517d70b8bc477198d3e4
ms.contentlocale: ru-ru
ms.lasthandoff: 08/21/2017

---
# <a name="changes-to-the-systemuri-namespace-in-version-20"></a>Изменения пространства имен System.Uri в версии 2.0
В класс <xref:System.Uri?displayProperty=fullName> были внесены некоторые изменения. Эти изменения были направлены на исправление некорректного поведения, повышение удобства использования и улучшение безопасности.  
  
## <a name="obsolete-and-deprecated-members"></a>Устаревшие и нерекомендуемые члены  
 Конструкторы:  
  
-   все конструкторы с параметром `dontEscape`.  
  
 Методы:  
  
-   <xref:System.Uri.CheckSecurity%2A>  
  
-   <xref:System.Uri.Escape%2A>  
  
-   <xref:System.Uri.Canonicalize%2A>  
  
-   <xref:System.Uri.Parse%2A>  
  
-   <xref:System.Uri.IsReservedCharacter%2A>  
  
-   <xref:System.Uri.IsBadFileSystemCharacter%2A>  
  
-   <xref:System.Uri.IsExcludedCharacter%2A>  
  
-   <xref:System.Uri.EscapeString%2A>  
  
## <a name="changes"></a>Изменения  
  
-   Для схем URI, которые заведомо не содержат части запроса (file, ftp и другие), символ "?" всегда экранируется и не обрабатывается как начало части <xref:System.Uri.Query%2A>.  
  
-   Для неявных файловых URI (имеют форму "c:\directory\file@name.txt") символ фрагмента ("#") всегда экранируется, за исключением случаев, когда запрашивается полная отмена экранирования или <xref:System.Uri.LocalPath%2A> имеет значение `true`.  
  
-   Прекращена поддержка имен узлов UNC. Принята спецификация IDN, определяющая представление международных имен узлов.  
  
-   <xref:System.Uri.LocalPath%2A> всегда возвращает строку с полной отменой экранирования.  
  
-   <xref:System.Uri.ToString%2A> не отменяет экранирование экранированного символа "%", "?" или "#".  
  
-   <xref:System.Uri.Equals%2A> теперь включает часть <xref:System.Uri.Query%2A> в проверку равенства.  
  
-   Операторы "==" и "!=" переопределены и связаны с методом <xref:System.Uri.Equals%2A>.  
  
-   <xref:System.Uri.IsLoopback%2A> теперь дает согласованные результаты.  
  
-   URI "`file:///path`" больше не преобразуется как "file://path".  
  
-   "#" теперь распознается как признак конца имени узла. Таким образом, "http://consoto.com#fragment" теперь преобразуется в "http://contoso.com/#fragment".  
  
-   Исправлена ошибка, возникающая при объединении базового URI с фрагментом.  
  
-   Исправлена ошибка в <xref:System.Uri.HostNameType%2A>.  
  
-   Исправлена ошибка при синтаксическом анализе NNTP.  
  
-   URI формата HTTP:contoso.com теперь приводят к возникновению исключения синтаксического анализа.  
  
-   Платформа правильно обрабатывает сведения о пользователях в URI.  
  
-   Сжатие пути URI фиксировано таким образом, чтобы неработающий URI не мог выполнять обход файловой системы на уровне выше корневого.  
  
## <a name="see-also"></a>См. также  
 <xref:System.Uri?displayProperty=fullName>

