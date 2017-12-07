---
title: "Состояния и данных в приложениях Docker"
description: "Архитектура Микрослужбами .NET для приложений .NET в контейнерах | Состояния и данных в приложениях Docker"
keywords: "Docker, Микрослужбами, ASP.NET, контейнер, SQL, CosmosDB, Docker"
author: CESARDELATORRE
ms.author: wiwagn
ms.date: 10/18/2017
ms.prod: .net-core
ms.technology: dotnet-docker
ms.topic: article
ms.openlocfilehash: 36d0fb9f27ef56b36c380e2fc972c79cff77003e
ms.sourcegitcommit: c2e216692ef7576a213ae16af2377cd98d1a67fa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/22/2017
---
# <a name="state-and-data-in-docker-applications"></a>Состояния и данных в приложениях Docker

В большинстве случаев можно представить контейнера в качестве экземпляра процесса. Процесс не поддерживают постоянное состояние. Пока контейнер можно записать на его локальное хранилище, при условии, что экземпляр будет происходить вокруг бесконечно бы как при условии, что место в памяти будет устойчивой. Образы контейнеров, таких как процессы, следует предположить, что существует несколько экземпляров, или они в конечном итоге будет завершен; Если они выполняется под управлением с orchestrator контейнера, следует полагать, что они могут получить перемещать из одного узла или виртуальной Машины в другую.

Docker обеспечивает функции с именем *наложения файловой системы*. Это реализуется задачу копирования при записи сохраняет обновленные сведения о файловой системе корневого контейнера. Эти сведения будут в дополнение к исходному изображению, лежащие в основе контейнера. Если контейнер удаляется из системы, эти изменения будут потеряны. Таким образом хотя и существует возможность сохранения состояния контейнер в локальном хранилище разработки системы этой противоречит предположения проектирования контейнера, который по умолчанию без сохранения состояния.

Следующие решения, используемые для управления постоянных данных в приложениях Docker.

-   [Тома данных](https://docs.docker.com/engine/tutorials/dockervolumes/) , подключения к узлу.

-   [Контейнеры томов данных](https://docs.docker.com/engine/tutorials/dockervolumes/#creating-and-mounting-a-data-volume-container) , обеспечивающие общее хранилище для контейнеров с помощью внешнего контейнера.

-   [Подключаемые модули тома](https://docs.docker.com/engine/tutorials/dockervolumes/) , подключите тома для удаленных служб, предоставляя долгосрочной сохраняемости.

-   [Хранилище Azure](https://docs.microsoft.com/azure/storage/), предоставляющее распространяемый geo хранилищам хорошо долговременное сохраняемости для контейнеров.

-   Удаленный реляционных баз данных, например [базы данных SQL Azure](https://azure.microsoft.com/services/sql-database/) или базах данных NoSQL, такие как [Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/introduction), или кэшировать служб, таких как [Redis](https://redis.io/).

В этом разделе представлены дополнительные сведения об этих параметрах.

**Тома данных** , каталогов, сопоставленных с ОС узла каталоги в контейнерах. Если код в контейнере есть доступ к каталогу, каталог на ОС узла имеет доступ. Этот каталог не привязан к времени существования контейнера и каталог может осуществляться из кода непосредственно в операционной системе сервера или по другой контейнер, сопоставляется один и тот же каталог узла к самому себе. Таким образом тома данных предназначены для хранения данных, независимо от существования контейнера. Если удалить контейнер или образа из узла Docker, данные сохраняются в том данных не удаляется. Данные в томе может осуществляться из ОС также.

**Контейнеры томов данных** являются развитием обычных данных тома. Контейнер томов данных является простой контейнер, имеющий один или несколько томов данных внутри него. Контейнер томов данных предоставляет доступ к контейнерам из точки центра подключения. Этот метод доступа к данным удобно, так как она создает уровень абстракции расположение исходных данных. За исключением этого его поведение аналогично томом обычных данных, данные сохраняются в этом контейнере выделенный независимо от жизненного цикла приложения контейнеров.

Как показано на рисунке 4-5, регулярного тома Docker могут храниться за пределами самим контейнерами, но в пределах физического сервера узла или виртуальной Машины. Тем не менее контейнеры Docker не может обращаться к тому с одного узла сервера или виртуальной Машины на другой. Другими словами с этих томов, он не поддерживается для управления данными, общим для контейнеров, выполняющихся на разных узлах Docker

![](./media/image5.png)

**Рис. 4-5**. Объемы данных и внешние источники данных для приложений на основе контейнера

Кроме того когда контейнеры Docker управляются orchestrator, контейнеры могут «переместить» между узлами в зависимости от оптимизацию, выполняемую в кластере. Таким образом не рекомендуется использовать тома данных для бизнес-данных. Однако, они хороший механизм для работы с файлами трассировки, временных файлов или аналогичные, не повлияет на согласованность данных business.

**Подключаемые модули тома** как [Flocker](https://clusterhq.com/flocker/) предоставляют доступ к данным на всех узлах в кластере. Не все подключаемые модули тома создаются одинаково, подключаемые модули тома обычно предоставляют во внешних хранилищах постоянного надежного хранилища из неизменяемого контейнеров.

**Удаленные источники данных и кэш** средств, таких как базы данных SQL Azure, Azure Cosmos DB или удаленного кэша Redis может использоваться в like контейнерных приложений так же, как они используются при разработке без контейнеров. Это проверенные способ хранения данных бизнес-приложения.

**Хранилище Azure.** Бизнес-данных обычно будет должны быть помещены в внешние ресурсы или базах данных, таких как хранилище Azure. Служба хранилища Azure, в конкретный, предоставляет следующие службы в облаке.

-   Хранилище BLOB-объектов хранит объекта неструктурированных данных. Большой двоичный объект может быть любым типом текстовых или двоичных данных, например документов или носителя файлы (изображения, аудио и видео). Хранилище больших двоичных объектов также называют хранения объектов.

-   Файл хранилища предоставляет общее хранилище для прежних версий приложений с помощью стандартного протокола SMB. Виртуальные машины Azure и облачных служб могут обмениваться данными файлов между компонентами приложения с помощью монтируемых хранилищ. Данные файла в общей папке посредством REST API файловой службы доступны локальных приложений.

-   Хранилище таблиц содержит структурированные наборы данных. Табличное хранилище является хранилищем данных ключевым атрибутом NoSQL, что позволяет ускорить разработку и быстрый доступ к большим объемам данных.

**Реляционные базы данных и базах данных NoSQL.** Существует множество вариантов для внешних баз данных из реляционных баз данных, например баз данных SQL Server, PostgreSQL, Oracle или NoSQL Azure Cosmos DB, MongoDB, и т. д. Эти базы данных не будет описано в рамках данного руководства, поскольку они находятся в совершенно разных субъекта.


>[!div class="step-by-step"]
[Предыдущие] (упаковываете монолитно applications.md) [Далее] (service ориентированного architecture.md)