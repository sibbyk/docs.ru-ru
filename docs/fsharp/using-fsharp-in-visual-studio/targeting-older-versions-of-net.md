---
title: "Выбор .NET Framework 2.0 в качестве целевой платформы в Windows 8"
description: "Дополнительные сведения о назначении старой версии платформы .NET, при использовании F #."
keywords: "visual f#, f#, функциональное программирование"
author: cartermp
ms.author: phcart
ms.date: 05/16/2016
ms.topic: language-reference
ms.prod: .net
ms.technology: devlang-fsharp
ms.devlang: fsharp
ms.assetid: 63989543-95c3-4ab7-81f3-3834a8b15010
ms.openlocfilehash: 2c0191267da5bee7b844c11cdee168ccfb3321c4
ms.sourcegitcommit: bd1ef61f4bb794b25383d3d72e71041a5ced172e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/18/2017
---
# <a name="targeting-older-versions-of-net"></a>Нацеливание на предыдущие версии .NET

> [!NOTE]
Данная статья не в курсе последних Visual Studio.  Он будет обновляться.

Может возникнуть следующая ошибка при попытке платформы .NET Framework 2.0, 3.0 или 3.5 в языке F # проекта во время установки Visual Studio на Windows 8.1: 

```
This project requires the 2.0 F# runtime, but that runtime is not installed.
```

Эта ошибка известно, что происходит при следующем сочетании условий:


- Вы установили Visual Studio на Windows 8.1.
<br />

- Не удалось включить .NET Framework 3.5 перед установкой Visual Studio.
<br />

- Проект предназначен для .NET Framework 2.0, 3.0 или 3.5.
<br />

При установке Visual Studio, он обнаруживает установленные версии платформы .NET Framework и устанавливает F # 2.0 Runtime только в том случае, если установлен и включен на .NET Framework 3.5.


## <a name="resolving-the-error"></a>Устранение ошибок
Чтобы устранить эту ошибку, можно либо целевой более новой версии платформы .NET Framework, либо можно включить .NET Framework 3.5 в Windows 8.1 и установить среду выполнения F # 2.0, запустив программу установки для Visual Studio с **восстановления** параметр.


#### <a name="to-enable-the-net-framework-35-on-windows-81"></a>Чтобы включить .NET Framework 3.5 в Windows 8.1

1. На **запустить** экрана, начать ввод **панели управления**.
<br />  При вводе этого имени **панели управления** отображается значок **приложений** заголовок.
<br />

2. Выберите **панели управления** значок, выберите **программы** значок и выберите **Включение или отключение компонентов** ссылку.
<br />

3. Убедитесь, что **.NET Framework 3.5 (включает .NET 2.0 и 3.0)** флажок установлен, а затем выберите **ОК** кнопки.
<br />  Установите флажки для всех дочерних узлов для дополнительных компонентов платформы .NET Framework не требуется.
<br />  .NET Framework 3.5 доступен в том случае, если он еще не инициализирован.
<br />


#### <a name="to-install-the-f-20-runtime"></a>Чтобы установить среду выполнения F # 2.0

1. В панели управления выберите **программы** связь, а затем выберите **программы и компоненты** ссылку.
<br />

2. В списке установленных программ выберите выпуск Visual Studio, установлен, а затем выберите **изменений** кнопки.
<br />

3. После запуска программы установки, выберите **восстановления** кнопки.
<br />  Дополнительные сведения см. в разделе [установка Visual Studio](https://msdn.microsoft.com/library/e2h7fzkw.aspx).
<br />
## <a name="see-also"></a>См. также
[Visual F#](../index.md)