---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
title: 'Utilizzo di Entity Framework 4.0 e controllo ObjectDataSource, parte 3: ordinamento e filtro | Microsoft Docs'
author: tdykstra
description: Questa serie di esercitazioni si basa sull'applicazione web di Contoso University specificano che viene creato da Getting Started with serie di esercitazioni in Entity Framework 4.0. POSSO...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: 2990bd10-590d-43d5-9529-6b503ce5455d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
msc.type: authoredcontent
ms.openlocfilehash: 71e26dc9a63131842daf4aa1549d761690b723ea
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831476"
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-3-sorting-and-filtering"></a>Utilizzo di Entity Framework 4.0 e controllo ObjectDataSource, parte 3: ordinamento e filtro
====================
da [Tom Dykstra](https://github.com/tdykstra)

> Questa serie di esercitazioni si basa sull'applicazione web Contoso University specificano che viene creato per il [Introduzione a Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) serie di esercitazioni. Se si non è stato completato le esercitazioni precedenti, come punto di partenza per questa esercitazione è possibile [scaricare l'applicazione](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) verrebbe creato. È anche possibile [scaricare l'applicazione](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) creato dalla serie di esercitazioni complete. Se si hanno domande sulle esercitazioni, è possibile pubblicarli per i [forum di ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).


Nell'esercitazione precedente è stato implementato il modello di repository in un'applicazione web a più livelli che usa Entity Framework e `ObjectDataSource` controllo. Questa esercitazione illustra come eseguire operazioni di ordinamento e filtraggio e gestire gli scenari di master-dettagli. Si aggiungeranno i miglioramenti seguenti per il *Departments.aspx* pagina:

- Una casella di testo per consentire agli utenti di selezionare i reparti in base al nome.
- Un elenco dei corsi per ogni reparto che viene visualizzato nella griglia.
- La possibilità di ordinare facendo clic sulle intestazioni di colonna.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)

## <a name="adding-the-ability-to-sort-gridview-columns"></a>Aggiunta della capacità Ordina colonne GridView

Aprire il *Departments.aspx* pagina e aggiungere un `SortParameterName="sortExpression"` attributo il `ObjectDataSource` controllo denominato `DepartmentsObjectDataSource`. (In seguito si creerà una `GetDepartments` metodo che accetta un parametro denominato `sortExpression`.) Il markup per il tag di apertura del controllo è ora simile al seguente.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample1.aspx)]

Aggiungere il `AllowSorting="true"` dell'attributo al tag di apertura del `GridView` controllo. Il markup per il tag di apertura del controllo è ora simile al seguente.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample2.aspx)]

In *Departments.aspx.cs*, impostare l'ordinamento predefinito chiamando il `GridView` del controllo `Sort` metodo dal `Page_Load` metodo:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample3.cs)]

È possibile aggiungere il codice che consente di ordinare o filtra la classe per la logica di business o la classe di repository. Se si fa nella classe della logica aziendale, l'ordinamento o filtro lavoro verrà eseguito dopo che i dati vengono recuperati dal database, perché la classe per la logica di business sta collaborando con un `IEnumerable` oggetto restituito dal repository. Se si aggiunge l'ordinamento e filtro codice nella classe di repository e effettuata prima di un'espressione LINQ o query di oggetto è stato convertito in un `IEnumerable` dell'oggetto, i comandi verranno passati al database per l'elaborazione, che corrisponde in genere più efficiente. In questa esercitazione è implementato l'ordinamento e filtri in modo che causa l'elaborazione da eseguire nel database, vale a dire nel repository.

Per aggiungere funzionalità di ordinamento, è necessario aggiungere un nuovo metodo per l'interfaccia del repository e classi repository anche la classe per la logica di business. Nel *ISchoolRepository.cs* , aggiungere un nuovo `GetDepartments` metodo che accetta un `sortExpression` parametro che verrà usato per ordinare l'elenco dei reparti che viene restituito:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample4.cs)]

Il `sortExpression` parametro specificherà la colonna da ordinare e la direzione di ordinamento.

Aggiungere il codice per il nuovo metodo per la *SchoolRepository.cs* file:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample5.cs)]

Modificare l'oggetto esistente senza parametri `GetDepartments` metodo da chiamare il nuovo metodo:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample6.cs)]

Nel progetto di test, aggiungere il nuovo metodo seguente a *MockSchoolRepository.cs*:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample7.cs)]

Se si prevede di creare tutti gli unit test che dipendono da questo metodo restituisce un elenco ordinato, è necessario ordinare l'elenco prima di restituirlo. È non verranno creati test simili in questa esercitazione, in modo che il metodo può restituire solo l'elenco non ordinato dei reparti.

Nel *SchoolBL.cs* , aggiungere il nuovo metodo seguente alla classe per la logica di business:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample8.cs)]

Questo codice passa il parametro di ordinamento per il metodo di repository.

Eseguire la *Departments.aspx* pagina.

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)

È ora possibile fare clic su un'intestazione di colonna per ordinare in base alla colonna. Se la colonna già ordinata, facendo clic sull'intestazione di inverte la direzione di ordinamento.

## <a name="adding-a-search-box"></a>Aggiunta di una casella di ricerca

In questa sezione si sarà aggiungere una casella di testo di ricerca, crea un collegamento per il `ObjectDataSource` controllare l'utilizzo di un parametro di controllo e aggiungere un metodo alla classe per la logica di business per supportare il filtro.

Aprire il *Departments.aspx* pagina e aggiungere il markup seguente tra i primo e l'intestazione `ObjectDataSource` controllo:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample9.aspx)]

Nel `ObjectDataSource` controllo denominato `DepartmentsObjectDataSource`, eseguire le operazioni seguenti:

- Aggiungere un `SelectParameters` (elemento) per un parametro denominato `nameSearchString` che ottiene il valore immesso nel `SearchTextBox` controllo.
- Modifica il `SelectMethod` al valore dell'attributo `GetDepartmentsByName`. (Verrà creato questo metodo in un secondo momento.)

Il markup per il `ObjectDataSource` controllo sarà ora simile all'esempio seguente:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample10.aspx)]

Nelle *ISchoolRepository.cs*, aggiungere un `GetDepartmentsByName` metodo che accetta entrambi `sortExpression` e `nameSearchString` parametri:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample11.cs)]

Nelle *SchoolRepository.cs*, aggiungere il nuovo metodo seguente:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample12.cs)]

Questo codice Usa un `Where` (metodo) per selezionare gli elementi che contengono la stringa di ricerca. Se la stringa di ricerca è vuota, verranno selezionati tutti i record. Si noti che quando si specifica chiamate al metodo insieme in un'istruzione simile alla seguente (`Include`, quindi `OrderBy`, quindi `Where`), il `Where` metodo deve sempre essere l'ultimo.

Modificare l'oggetto esistente `GetDepartments` metodo che accetta un `sortExpression` parametro per chiamare il nuovo metodo:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample13.cs)]

Nelle *MockSchoolRepository.cs* nel progetto di test, aggiungere il nuovo metodo seguente:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample14.cs)]

Nelle *SchoolBL.cs*, aggiungere il nuovo metodo seguente:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample15.cs)]

Eseguire la *Departments.aspx* pagina e immettere una stringa di ricerca per assicurarsi che la logica di selezione funziona. Lasciare vuota la casella di testo e provare a eseguire una ricerca per assicurarsi che tutti i record vengono restituiti.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)

## <a name="adding-a-details-column-for-each-grid-row"></a>Aggiunta di una colonna dei dettagli per ogni riga della griglia

Successivamente, si desidera visualizzare tutti i corsi per ogni reparto visualizzato nella cella a destra della griglia. A tale scopo, si userà un annidato `GridView` controllo e associa i dati per i dati dal `Courses` proprietà di navigazione del `Department` entità.

Aprire *Departments.aspx* e nel markup per il `GridView` controllare, specificare un gestore per il `RowDataBound` evento. Il markup per il tag di apertura del controllo è ora simile al seguente.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample16.aspx)]

Aggiungere un nuovo `TemplateField` elemento dopo il `Administrator` campo modello:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample17.aspx)]

Questo codice crea un nidificata `GridView` controllo che mostra il numero di corso e il titolo di un elenco dei corsi. Non specifica un'origine dati perché si imposterà un metodo databind all'interno del codice nel `RowDataBound` gestore.

Aprire *Departments.aspx.cs* e aggiungere il gestore seguente per il `RowDataBound` evento:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample18.cs)]

Questo codice ottiene la `Department` entità dagli argomenti dell'evento, converte il `Courses` proprietà di navigazione a un `List` raccolta e associa annidata `GridView` alla raccolta.

Aprire il *SchoolRepository.cs* file e specificare il caricamento eager per le `Courses` proprietà di navigazione chiamando il `Include` metodo della query di oggetto creato nel `GetDepartmentsByName` (metodo). Il `return` istruzione il `GetDepartmentsByName` metodo è ora simile al seguente.

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample19.cs)]

Eseguire la pagina. Oltre a ordinamento e filtro funzionalità aggiunta in precedenza, il controllo GridView ora mostra i dettagli di corso annidato per ogni reparto.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)

In questo passaggio si completa l'introduzione agli scenari di ordinamento, filtro e master-dettagli. Nella prossima esercitazione, verrà illustrato come gestire la concorrenza.

> [!div class="step-by-step"]
> [Precedente](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
> [Successivo](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)
