---
title: "EventWaitHandle | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-standard"
ms.tgt_pltfrm: ""
ms.topic: "article"
helpviewer_keywords: 
  - "threading [.NET Framework], EventWaitHandle class"
  - "EventWaitHandle class"
  - "event wait handles [.NET Framework]"
  - "threading [.NET Framework], cross-process synchronization"
ms.assetid: 11ee0b38-d663-4617-b793-35eb6c64e9fc
caps.latest.revision: 9
author: "rpetrusha"
ms.author: "ronpet"
manager: "wpickett"
caps.handback.revision: 9
---
# EventWaitHandle
Класс <xref:System.Threading.EventWaitHandle> позволяет потокам взаимодействовать друг с другом посредством подачи сигналов и ожидания сигналов.  Дескрипторы ожидания событий \(также называемые просто "события"\) — это дескрипторы ожидания, которые могут получать сигналы в целях освобождения одного или нескольких потоков.  После получения сигнала дескриптор ожидания событий сбрасывается или вручную, или автоматически.  Класс <xref:System.Threading.EventWaitHandle> может представлять локальный дескриптор ожидания событий \(локальное событие\) или именованный системный дескриптор ожидания событий \(именованное событие или системное событие, видимое всеми процессами\).  
  
> [!NOTE]
>  Дескрипторы ожидания событий не являются событиями в обычном смысле употребления этого термина в .NET Framework.  Здесь не участвуют делегаты или обработчики события.  Слово "событие" используется для описания этих дескрипторов, потому что они традиционно называются событиями операционной системы, а также из\-за того, что передача сигнала дескриптору ожидания указывает ожидающим потокам о возникновении события.  
  
 Как локальные, так и именованные дескрипторы ожидания событий используют объекты синхронизации системы, которые защищены оболочками <xref:Microsoft.Win32.SafeHandles.SafeWaitHandle> для обеспечения освобождения ресурсов.  Можно использовать метод <xref:System.Threading.WaitHandle.System%23IDisposable%23Dispose%2A> для немедленного освобождения ресурсов после завершения использования объекта.  
  
## Дескрипторы ожидания событий, которые сбрасываются автоматически  
 Событие можно создать и автоматически сбросить путем указания <xref:System.Threading.EventResetMode?displayProperty=fullName> при создании объекта <xref:System.Threading.EventWaitHandle>.  Как указано в этом имени, это событие синхронизации сбрасывается автоматически при получении сигнала и после освобождения одного ожидающего потока.  Сигнал событию можно подать посредством вызова метода <xref:System.Threading.EventWaitHandle.Set%2A>.  
  
 Автоматически сбрасываемые события, как правило, используются для единовременного предоставления отдельному потоку монопольного доступа к ресурсу.  Поток запрашивает ресурс посредством вызова метода <xref:System.Threading.WaitHandle.WaitOne%2A>.  Если другой поток не удерживает дескриптор ожидания, метод возвращает значение `true` и вызывающий поток получает управление ресурсом.  
  
> [!IMPORTANT]
>  Как и во всех механизмах синхронизации, необходимо убедиться, что все пути кода ожидают, используя подходящий дескриптор ожидания, перед получением доступа к защищенному ресурсу.  Синхронизация потоков выполняется совместно.  
  
 Если автоматически сбрасываемые события получают сигналы, когда ни один поток не находится в ожидании, они продолжают получать сигналы до тех пор, пока какой\-нибудь поток не предпринимает попытки попасть в ожидание.  Событие освобождает поток и немедленно сбрасывается, блокируя последующие потоки.  
  
## Дескрипторы ожидания событий, которые сбрасываются вручную  
 Можно создать событие, сбрасывающееся вручную, путем указания <xref:System.Threading.EventResetMode?displayProperty=fullName> при создании объекта <xref:System.Threading.EventWaitHandle>.  Как указывает его имя, это событие синхронизации должно быть сброшено вручную после получения сигнала.  Когда оно сброшено посредством вызова метода <xref:System.Threading.EventWaitHandle.Reset%2A>, потоки, ожидающие дескриптор события, продолжают выполнение операций без блокирования.  
  
 Событие ручного сброса можно уподобить воротам загона.  Если событие не получает сигнала, потоки, ожидающие сигнала, блокируются, как коровы в загоне.  При получении событием сигнала происходит вызов метода <xref:System.Threading.EventWaitHandle.Set%2A>, после чего все ожидающие потоки становятся свободными.  Событие получает сигнал до вызова метода <xref:System.Threading.EventWaitHandle.Reset%2A>.  Это делает событие ручного сброса идеальным способом удерживания потоков, которые должны ждать до тех пор, пока один поток не завершит выполняемую задачу.  
  
 Как и в случае с коровами, покидающими загон, освобожденные потоки нуждаются в определенном количестве заданного операционной системой времени, чтобы возобновить выполнение операций.  Если метод <xref:System.Threading.EventWaitHandle.Reset%2A> вызывается до того, как все потоки возобновили выполнение операций, оставшиеся потоки снова блокируются.  Какие потоки возобновляют работу, а какие блокируются, зависит от ряда случайных факторов, например от загрузки системы, количества потоков, ожидающих планировщика, и т. п.  Нет ничего плохого в том, что поток, передавший сигнал событию, завершается после подачи сигнала, что является наиболее распространенной моделью использования.  Если необходимо, чтобы поток, подавший сигнал событию, начал выполнение другой задачи после возобновления всех ожидающих потоков, необходимо заблокировать его до возобновления работы всех ожидающих потоков.  В противном случае возникнет состояние гонки, и поведение кода станет непредсказуемым.  
  
## Функциональные возможности, присущие автоматическим и ручным событиям  
 Как правило, один или несколько потоков блокируются в <xref:System.Threading.EventWaitHandle> до того момента, как незаблокированный поток вызовет метод <xref:System.Threading.EventWaitHandle.Set%2A>, что приведет к освобождению одного из ожидающих потоков \(в случае событий автоматического сброса\) или всех потоков \(в случае событий ручного сброса\).  Поток может сообщить <xref:System.Threading.EventWaitHandle>, а затем заблокироваться в нем как атомарная операция посредством вызова статического метода <xref:System.Threading.WaitHandle.SignalAndWait%2A?displayProperty=fullName>.  
  
 Объекты <xref:System.Threading.EventWaitHandle> могут использоваться вместе со статическими методами <xref:System.Threading.WaitHandle.WaitAll%2A?displayProperty=fullName> и <xref:System.Threading.WaitHandle.WaitAny%2A?displayProperty=fullName>.  Так как классы <xref:System.Threading.EventWaitHandle> и <xref:System.Threading.Mutex> являются производными из <xref:System.Threading.WaitHandle>, можно использовать оба этих класса совместно с указанными методами.  
  
### Именованные события  
 В операционной системе Windows дескрипторы ожидания событий могут иметь имена.  Именованное событие применимо ко всей системе.  То есть после создания именованного события, оно становится видимым для всех потоков во всех процессах.  Поэтому именованные события могут использоваться для синхронизации активности процессов и потоков.  
  
 Можно создать объект <xref:System.Threading.EventWaitHandle>, который представляет именованное системное событие посредством использования одного из конструкторов, указывающих имя события.  
  
> [!NOTE]
>  Так как именованные события применимы ко всей системе, можно использовать несколько объектов <xref:System.Threading.EventWaitHandle>, представляющих одно именованное событие.  Каждый раз при вызове конструктора или метода <xref:System.Threading.EventWaitHandle.OpenExisting%2A> создается новый объект <xref:System.Threading.EventWaitHandle>.  Повторное указание того же имени приводит к созданию нескольких объектов, представляющих то же самое именованное событие.  
  
 Использовать именованные события следует с осторожностью.  Так как они применимы ко всей системе, другой процесс, использующий то же имя, может неожиданно заблокировать потоки.  Вредоносный код, выполняемый на том же компьютере, может использовать это как основу для атак по типу "отказ в обслуживании".  
  
 Используйте безопасность управления доступом для защиты объекта <xref:System.Threading.EventWaitHandle>, представляющего именованное событие, предпочтительнее с помощью конструктора, который указывает объект <xref:System.Security.AccessControl.EventWaitHandleSecurity>.  Также можно применить безопасность управления доступом с помощью метода <xref:System.Threading.EventWaitHandle.SetAccessControl%2A>, однако это делает систему уязвимой в интервале между созданием дескриптора ожидания и его защитой.  Защита событий с помощью безопасности управления доступом способствует предотвращению атак злоумышленников, но не решает проблемы непреднамеренного конфликта имен.  
  
> [!NOTE]
>  В отличие от класса <xref:System.Threading.EventWaitHandle> производные классы <xref:System.Threading.AutoResetEvent> и <xref:System.Threading.ManualResetEvent> могут представлять только локальные дескрипторы ожидания.  Они не могут представлять именованные системные события.  
  
## См. также  
 <xref:System.Threading.EventWaitHandle>   
 <xref:System.Threading.WaitHandle>   
 <xref:System.Threading.AutoResetEvent>   
 <xref:System.Threading.ManualResetEvent>   
 [EventWaitHandle, AutoResetEvent, CountdownEvent, ManualResetEvent](../../../docs/standard/threading/eventwaithandle-autoresetevent-countdownevent-manualresetevent.md)