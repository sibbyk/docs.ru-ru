---
title: "Включение режима MARS | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-ado"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 576079e4-debe-4ab5-9204-fcbe2ca7a5e2
caps.latest.revision: 6
author: "JennieHubbard"
ms.author: "jhubbard"
manager: "jhubbard"
caps.handback.revision: 6
---
# Включение режима MARS
Режим MARS \- это новая функция, которая в SQL Server используется для выполнения нескольких пакетов по одному соединению.  Если для работы с SQL Server включен режим MARS, каждый используемый объект команды добавляет сеанс к соединению.  
  
> [!NOTE]
>  Один сеанс режима MARS открывает одно логическое соединение для использования самим режимом MARS, а затем \- по одному логическому соединению для каждой активной команды.  
  
## Включение и отключение режима MARS в строке соединения  
  
> [!NOTE]
>  В следующей строке подключения используется образец базы данных **AdventureWorks**, входящий в состав SQL Server.  Строки соединения, представленные в образце кода, предполагают, что база данных установлена на сервере MSSQL1.  Измените строку соединения при необходимости в соответствии с вашей средой.  
  
 По умолчанию режим MARS отключен.  Включить его можно, добавив в строку соединения ключевое слово «MultipleActiveResultSets\=True». "  «True» \- единственное допустимое значение для включения режима MARS.  В следующем примере демонстрируется, как подключиться к экземпляру SQL Server, а также как указать, что режим MARS должен быть включен.  
  
```vb  
Dim connectionString As String = "Data Source=MSSQL1;" & _  
    "Initial Catalog=AdventureWorks;Integrated Security=SSPI;" & _  
    "MultipleActiveResultSets=True"  
```  
  
```csharp  
string connectionString = "Data Source=MSSQL1;" +   
    "Initial Catalog=AdventureWorks;Integrated Security=SSPI;" +  
    "MultipleActiveResultSets=True";  
```  
  
 Отключить режим MARS можно, добавив в строку соединения ключевое слово «MultipleActiveResultSets\=False». "  «False» \- единственное допустимое значение для отключения режима MARS.  Следующая строка соединения показывает, как отключать режим MARS.  
  
```vb  
Dim connectionString As String = "Data Source=MSSQL1;" & _  
    "Initial Catalog=AdventureWorks;Integrated Security=SSPI;" & _  
    "MultipleActiveResultSets=False"  
```  
  
```csharp  
string connectionString = "Data Source=MSSQL1;" +   
    "Initial Catalog=AdventureWorks;Integrated Security=SSPI;" +  
    "MultipleActiveResultSets=False";  
```  
  
## Особые рассуждения об использовании режима MARS  
 В общем случае необходимости изменять существующие приложения для использования соединений с режимом MARS быть не должно.  Однако если требуется использовать функцию режима MARS в приложениях, следует ознакомиться со следующими рассуждениями и понять их.  
  
### Чередование инструкций  
 На сервере операции режима MARS выполняются синхронно.  Разрешается чередование инструкций SELECT и BULK INSERT.  Однако инструкции на языке DML и языке DDL выполняются атомарным образом.  Попытка выполнения любых инструкций во время выполнения атомарного пакета блокируется.  Параллельное выполнение на сервере не является функцией режима MARS.  
  
 Если в рамках соединения режима MARS подаются два пакета \(один с инструкцией SELECT, другой с инструкцией DML\), выполнение инструкции DML может начаться во время выполнения инструкции SELECT.  Однако выполнение инструкции SELECT может быть продолжено только после полного выполнения инструкции DML.  Если обе инструкции выполняются в рамках одной транзакции, любые изменения, внесенные инструкцией DML после начала выполнения инструкции SELECT, являются невидимыми для операции чтения.  
  
 Инструкция WAITFOR внутри инструкции SELECT не выдает транзакцию во время ожидания, то есть пока не будет сформирована первая строка.  А это означает, что пока инструкция WAITFOR ждет, другие пакеты не могут выполняться в рамках одного соединения.  
  
### Кэш сеанса режима MARS  
 При открытии соединения с включенным режимом MARS создается логический сеанс, что требует дополнительных затрат.  Чтобы свести к минимуму затраты и повысить производительность, **SqlClient** кэширует сеанс режима MARS в рамках соединения.  Кэш может содержать максимум 10 сеансов режима MARS.  Это значение не может быть изменено пользователем.  При достижении лимита сеансов создается новый сеанс \- ошибка не формируется.  Сам кэш и содержащиеся в нем сеансы принадлежат одному соединению, они не могут использоваться в нескольких соединениях.  Когда сеанс освобождается, он возвращается в пул, если только не был достигнут верхний предел пула.  Если пул кэша заполнен, то сеанс закрывается.  Сеансы режима MARS не имеют срока действия.  Они очищаются только при удалении объекта соединения.  Кэш сеансов режима MARS предварительно не загружается.  Он загружается, когда приложению требуется больше сеансов.  
  
### Потокобезопасность  
 Операции режима MARS не обеспечивают безопасность потока.  
  
### Объединение подключений в пул  
 Как и любые другие соединения, соединения, для которых включен режим MARS, организуются в пулы.  Если приложение открывает два соединения \(одно с включенным режимом MARS и другое, для которого режим MARS отключен\), два эти соединения помещаются в отдельные пулы.  Для получения дополнительной информации см. [Организация пулов соединений SQL Server \(ADO.NET\)](../../../../../docs/framework/data/adonet/sql-server-connection-pooling.md).  
  
### Среда выполнения пакетов SQL Server  
 При открытии соединения определяется среда по умолчанию.  Затем эта среда копируется в логический сеанс режима MARS.  
  
 Среда пакетного выполнения состоит из следующих компонентов:  
  
-   Заданных параметров \(например, ANSI\_NULLS, DATE\_FORMAT, LANGUAGE, TEXTSIZE\)  
  
-   Контекста безопасности \(роль пользователя\-приложения\)  
  
-   Контекста базы данных \(текущая база данных\)  
  
-   Переменных состояния выполнения \(например, @@ERROR, @@ROWCOUNT, @@FETCH\_STATUS @@IDENTITY\)  
  
-   Временных таблиц верхнего уровня  
  
 При работе в режиме MARS среда выполнения по умолчанию ассоциируется с соединением.  Каждый новый пакет, начинающий выполнение в рамках данного соединения, получает копию среды по умолчанию.  Всякий раз при выполнении кода все изменения в среде выполнения применяются к данному конкретному пакету.  Как только выполнение завершается, настройки выполнения копируются в среду по умолчанию.  Если один пакет выдает несколько команд для последовательного выполнения в рамках одной транзакции, семантика такая же, как и при прошлых соединениях клиентов или серверов.  
  
### Параллельное выполнение  
 Режим MARS не предназначен для использования во всех ситуациях, когда приложению требуется несколько соединений.  Если приложению требуется настоящее параллельное выполнение команд для сервера, следует использовать несколько соединений.  
  
 Например, рассмотрим следующую ситуацию.  Создаются два объекта команд, один для обработки результирующего набора, а другой для обновления данных. Они используют одно соединение с режимом MARS.  В этой ситуации метод `Transaction`.`Commit` не сможет выполнить обновление, пока не будут прочитаны все результаты для первого объекта команды, при этом выдается следующее исключение:  
  
 Сообщение: контекст транзакции используется другим сеансом.  
  
 Источник: поставщик данных .NET SqlClient  
  
 Ожидается: \(значение NULL\).  
  
 Получено: System.Data.SqlClient.SqlException  
  
 Есть три способа обработки этой ситуации.  
  
1.  Запустить транзакцию после создания модуля чтения с тем, чтобы он не был частью транзакции.  После этого любое обновление будет становиться собственной транзакцией.  
  
2.  Зафиксировать всю работу после закрытия модуля чтения.  Это создает потенциал для последующего пакета обновлений.  
  
3.  Не использовать режим MARS, вместо этого использовать отдельное соединение для каждого объекта команды, что было стандартным решением до появления режима MARS.  
  
### Обнаружение поддержки режима MARS  
 Приложение может проверить, поддерживается ли режим MARS, считав значение `SqlConnection.ServerVersion`.  Основным номером должно быть 9 для SQL Server 2005 и 10 для SQL Server 2008.  
  
## См. также  
 [Режим MARS](../../../../../docs/framework/data/adonet/sql/multiple-active-result-sets-mars.md)   
 [Центр разработчиков, поставщики ADO.NET Managed Provider и набор данных](http://go.microsoft.com/fwlink/?LinkId=217917)