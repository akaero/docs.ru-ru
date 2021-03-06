---
title: "@ (указание файла ответа) (Visual Basic) | Microsoft Docs"
ms.date: "2015-07-20"
ms.prod: ".net"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "devlang-visual-basic"
ms.topic: "article"
dev_langs: 
  - "VB"
helpviewer_keywords: 
  - "@ (указание файла ответа) - параметр компилятора [Visual Basic]"
ms.assetid: a6847eaa-e5f9-4303-9421-45b55484b9ca
caps.latest.revision: 15
author: "stevehoag"
ms.author: "shoag"
caps.handback.revision: 15
---
# @ (указание файла ответа) (Visual Basic)
[!INCLUDE[vs2017banner](../../../visual-basic/includes/vs2017banner.md)]

Указывает файл, содержащий параметры компилятора и файлы исходного кода для компиляции.  
  
## Синтаксис  
  
```  
@response_file  
```  
  
## Аргументы  
 `response_file`  
 Обязательный.  Файл, содержащий параметры компилятора и файлы исходного кода для компиляции.  Если имя файла содержит пробел, следует заключить его в кавычки \(" "\).  
  
## Заметки  
 Компилятор обрабатывает указанные в файле ответа параметры и файлы исходного кода, как если бы они были указаны в командной строке.  
  
 Чтобы использовать несколько файлов ответа во время компиляции, следует задать параметры для нескольких файлов ответа.  
  
```  
@file1.rsp @file2.rsp  
```  
  
 В файле ответа на одной строке можно указать несколько параметров компилятора и файлов исходного кода.  Каждый параметр компилятора должен записываться только в одной строке \(нельзя переносить на другую строку\).  В файл ответа можно включать примечания, начинающиеся с символа `#`.  
  
 Можно сочетать использование параметров командной строки и параметров из одного или нескольких файлов ответа.  Компилятор обрабатывает параметры команды по мере их обнаружения.  Таким образом, параметры командной строки могут переопределять параметры, указанные в файле ответа.  Таким же образом параметры в файле ответа могут переопределять ранее указанные параметры командной строки или параметры из других файлов ответа.  
  
 Visual Basic предоставляет файл Vbc.rsp, расположенный в том же каталоге, что и файл Vbc.exe.  Файл Vbc.rsp включается по умолчанию, если используется параметр `/noconfig`.  Дополнительные сведения см. в разделе [\/noconfig](../../../visual-basic/reference/command-line-compiler/noconfig.md).  
  
> [!NOTE]
>  Параметр `@` недоступен из среды разработки Visual Studio. Он доступен только при выполнении компиляции из командной строки.  
  
## Пример  
 Вот несколько строк из возможного файла ответа:  
  
```  
# build the first output file  
/target:exe   
/out:MyExe.exe  
source1.vb   
source2.vb  
```  
  
## Пример  
 В следующем примере демонстрируется использование параметра `@` с файлом ответа `File1.rsp`.  
  
```  
vbc @file1.rsp  
```  
  
## См. также  
 [Компилятор Visual Basic с интерфейсом командной строки](../../../visual-basic/reference/command-line-compiler/index.md)   
 [\/noconfig](../../../visual-basic/reference/command-line-compiler/noconfig.md)   
 [Примеры командных строк компиляции](../../../visual-basic/reference/command-line-compiler/sample-compilation-command-lines.md)