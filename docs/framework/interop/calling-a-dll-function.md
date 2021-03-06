---
title: "Вызов функции DLL"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology:
- dotnet-clr
ms.tgt_pltfrm: 
ms.topic: article
dev_langs:
- VB
- CSharp
- C++
- jsharp
helpviewer_keywords:
- unmanaged functions, calling
- unmanaged functions
- COM interop, platform invoke
- platform invoke, calling unmanaged functions
- interoperation with unmanaged code, platform invoke
- DLL functions
ms.assetid: 113646de-7ea0-4f0e-8df0-c46dab3e8733
caps.latest.revision: 12
author: rpetrusha
ms.author: ronpet
manager: wpickett
ms.translationtype: HT
ms.sourcegitcommit: 306c608dc7f97594ef6f72ae0f5aaba596c936e1
ms.openlocfilehash: b676599513b923ae46d6ec27d7506435d9cbfcd2
ms.contentlocale: ru-ru
ms.lasthandoff: 08/21/2017

---
# <a name="calling-a-dll-function"></a>Вызов функции DLL
Хотя вызов неуправляемых функций DLL почти идентичен вызову другого управляемого кода, все же существуют отличия, которые поначалу могут вызвать некоторые сложности в освоении функций DLL. В этом разделе представлены статьи, касающиеся некоторых особенностей вызовов.  
  
 Структуры, возвращаемые из вызовов неуправляемого кода, должны быть типами данных, имеющими одинаковые представления в управляемом и неуправляемом коде. Такие типы называются *непреобразуемыми типами*, так как они не требуют преобразования (см. раздел [Непреобразуемые и преобразуемые типы](../../../docs/framework/interop/blittable-and-non-blittable-types.md)). Чтобы вызвать функцию с преобразуемой структурой в качестве ее возвращаемого типа, можно определить непреобразуемый вспомогательный тип того же размера, что и преобразуемый тип, и преобразовать данные после возвращения функции.  
  
## <a name="in-this-section"></a>Содержание  
 [Передача структур](../../../docs/framework/interop/passing-structures.md)  
 Описываются вопросы передачи структур данных с предопределенной компоновкой.  
  
 [Функции обратного вызова](../../../docs/framework/interop/callback-functions.md)  
 Основные сведения о функциях обратного вызова.  
  
 [Практическое руководство. Реализация функций обратного вызова](../../../docs/framework/interop/how-to-implement-callback-functions.md)  
 Описывается реализация функций обратного вызова в управляемом коде.  
  
## <a name="related-sections"></a>Связанные разделы  
 [Использование неуправляемых функций DLL](../../../docs/framework/interop/consuming-unmanaged-dll-functions.md)  
 Описывает способ вызова неуправляемых функций DLL с помощью вызова платформы.  
  
 [Маршалинг данных при вызове неуправляемого кода](../../../docs/framework/interop/marshaling-data-with-platform-invoke.md)  
 Описывается способ объявления параметров метода и передачи аргументов в функции, экспортируемые неуправляемыми библиотеками.

