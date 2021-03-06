---
title: "Практическое руководство. Уменьшение эффекта дрожания изображения посредством двойной буферизации для форм и элементов управления"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology: dotnet-winforms
ms.tgt_pltfrm: 
ms.topic: article
dev_langs:
- csharp
- vb
helpviewer_keywords:
- flicker [Windows Forms], reducing in Windows Forms
- graphics [Windows Forms], reducing double-buffered flicker
ms.assetid: 91083d3a-653f-4f15-a467-0f37b2aa39d6
caps.latest.revision: "11"
author: dotnet-bot
ms.author: dotnetcontent
manager: wpickett
ms.workload: dotnet
ms.openlocfilehash: fc782ba262527a319cbb05cc6d36ca568afc55c0
ms.sourcegitcommit: 16186c34a957fdd52e5db7294f291f7530ac9d24
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/22/2017
---
# <a name="how-to-reduce-graphics-flicker-with-double-buffering-for-forms-and-controls"></a>Практическое руководство. Уменьшение эффекта дрожания изображения посредством двойной буферизации для форм и элементов управления
Двойная буферизация использует буфер памяти для решения проблем мерцания, связанных с несколькими операциями рисования. Если двойная буферизация включена, все операции рисования сначала обрабатываются в буфере памяти вместо области рисования на экране. После завершения всех операций рисования буфер памяти копируется непосредственно в связанную с ним область рисования. Поскольку на экране выполняется лишь одна графическая операция, мерцание в сложных операциях рисования устраняется. Для большинства приложений по умолчанию двойной буферизации [!INCLUDE[dnprdnshort](../../../../includes/dnprdnshort-md.md)] обеспечивает лучшие результаты. Стандартные элементы управления Windows Forms двойной буферизации по умолчанию. Вы можете включить двойную буферизацию в формах и элементах управления двумя способами. Можно либо установить <xref:System.Windows.Forms.Control.DoubleBuffered%2A> свойства `true`, может вызвать <xref:System.Windows.Forms.Control.SetStyle%2A> метод, чтобы задать <xref:System.Windows.Forms.ControlStyles.OptimizedDoubleBuffer> флаг `true`. Оба метода включает двойной буферизации по умолчанию для формы или элемента управления и предоставляют отрисовки графики мерцания. Вызов <xref:System.Windows.Forms.Control.SetStyle%2A> метода рекомендуется только для пользовательских элементов управления, для которых вы написали код отрисовки.  
  
 Для более сложных случаях буферизации, например анимации или сложном управлении памятью можно реализовать собственную логику двойной буферизации. Дополнительные сведения см. в разделе [как: управление буферизацией графики](../../../../docs/framework/winforms/advanced/how-to-manually-manage-buffered-graphics.md).  
  
### <a name="to-reduce-flicker"></a>Чтобы понизить мерцание  
  
-   Задайте для свойства <xref:System.Windows.Forms.Control.DoubleBuffered%2A> значение `true`.  
  
     [!code-csharp[System.Windows.Forms.LegacyBufferedGraphics#31](../../../../samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.LegacyBufferedGraphics/CS/Class1.cs#31)]
     [!code-vb[System.Windows.Forms.LegacyBufferedGraphics#31](../../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.LegacyBufferedGraphics/VB/Class1.vb#31)]  
  
 \- или -  
  
-   Вызовите <xref:System.Windows.Forms.Control.SetStyle%2A> метод, чтобы задать <xref:System.Windows.Forms.ControlStyles.OptimizedDoubleBuffer> флаг `true`.  
  
     [!code-csharp[System.Windows.Forms.LegacyBufferedGraphics#32](../../../../samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.LegacyBufferedGraphics/CS/Class1.cs#32)]
     [!code-vb[System.Windows.Forms.LegacyBufferedGraphics#32](../../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.LegacyBufferedGraphics/VB/Class1.vb#32)]  
  
## <a name="see-also"></a>См. также  
 <xref:System.Windows.Forms.Control.DoubleBuffered%2A>  
 <xref:System.Windows.Forms.Control.SetStyle%2A>  
 [Двойная буферизация графики](../../../../docs/framework/winforms/advanced/double-buffered-graphics.md)  
 [Объекты Graphics и Drawing в Windows Forms](../../../../docs/framework/winforms/advanced/graphics-and-drawing-in-windows-forms.md)
