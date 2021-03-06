---
title: "Практическое руководство. Использование объекта ThicknessConverter"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology: dotnet-wpf
ms.tgt_pltfrm: 
ms.topic: article
dev_langs:
- csharp
- vb
helpviewer_keywords:
- border thickness [WPF]
- ThicknessConverter objects [WPF]
ms.assetid: 52682194-d7fd-499c-8005-73fcc84e7b2c
caps.latest.revision: "9"
author: dotnet-bot
ms.author: dotnetcontent
manager: wpickett
ms.workload: dotnet
ms.openlocfilehash: c2445eba7556822c8265337ec97c2f0a9f1d5042
ms.sourcegitcommit: c0dd436f6f8f44dc80dc43b07f6841a00b74b23f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="how-to-use-a-thicknessconverter-object"></a>Практическое руководство. Использование объекта ThicknessConverter
## <a name="example"></a>Пример  
 В этом примере показано, как создать экземпляр <xref:System.Windows.ThicknessConverter> и использовать его для изменения толщины границы.  
  
 В примере определяется пользовательский метод с именем `changeThickness`; сначала этот метод преобразует содержимое <xref:System.Windows.Controls.ListBoxItem>, как определено в отдельном [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] файл, чтобы экземпляр <xref:System.Windows.Thickness>, а затем преобразует содержимое в <xref:System.String>. Этот метод передает <xref:System.Windows.Controls.ListBoxItem> для <xref:System.Windows.ThicknessConverter> объекта, который преобразует <xref:System.Windows.Controls.ContentControl.Content%2A> из <xref:System.Windows.Controls.ListBoxItem> к экземпляру компонента <xref:System.Windows.Thickness>. Это значение затем передается обратно в качестве значения <xref:System.Windows.Controls.Border.BorderThickness%2A> свойство <xref:System.Windows.Controls.Border>.  
  
 Этот пример не запускается.  
  
 [!code-csharp[ThicknessConverter#1](../../../../samples/snippets/csharp/VS_Snippets_Wpf/ThicknessConverter/CSharp/Window1.xaml.cs#1)]
 [!code-vb[ThicknessConverter#1](../../../../samples/snippets/visualbasic/VS_Snippets_Wpf/ThicknessConverter/VisualBasic/Window1.xaml.vb#1)]  
  
## <a name="see-also"></a>См. также  
 <xref:System.Windows.Thickness>  
 <xref:System.Windows.ThicknessConverter>  
 <xref:System.Windows.Controls.Border>  
 [Способ: измените Margin-свойство](http://msdn.microsoft.com/library/8a313efd-5f99-4097-b4c1-8fa49d8379a2)  
 [Как: преобразование ListBoxItem в новый тип данных](http://msdn.microsoft.com/library/7a080b88-184e-4b27-bb61-d42bafba9727)  
 [Общие сведения о панелях](../../../../docs/framework/wpf/controls/panels-overview.md)
