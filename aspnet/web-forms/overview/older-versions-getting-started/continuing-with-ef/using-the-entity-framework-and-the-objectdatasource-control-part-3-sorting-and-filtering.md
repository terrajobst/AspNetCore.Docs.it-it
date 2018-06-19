---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
title: 'Utilizzo di Entity Framework 4.0 e il controllo ObjectDataSource, parte 3: ordinamento e filtro | Documenti Microsoft'
author: tdykstra
description: Questa serie di esercitazioni compila nell'applicazione web di Contoso University creato da introduttiva la serie di esercitazioni di Entity Framework 4.0. I...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 2990bd10-590d-43d5-9529-6b503ce5455d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
msc.type: authoredcontent
ms.openlocfilehash: e412d3ad98a37931e7190a4909cb09fa2abfb3d0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887655"
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-3-sorting-and-filtering"></a>Utilizzo di Entity Framework 4.0 e il controllo ObjectDataSource, parte 3: ordinamento e filtro
====================
da [Tom Dykstra](https://github.com/tdykstra)

> Questa serie di esercitazioni si basa sull'applicazione web di Contoso University creando il [introduzione di Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) serie di esercitazioni. Se non ha completato le esercitazioni precedenti, come punto di partenza per questa esercitazione è possibile [scaricare l'applicazione](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) che consente di creare. È anche possibile [scaricare l'applicazione](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) creati tramite la serie di esercitazioni completo. Nel caso di problemi con le esercitazioni, è possibile registrarli per il [forum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).


Nell'esercitazione precedente è stato implementato il modello di repository in un'applicazione web a più livelli che usa Entity Framework e `ObjectDataSource` controllo. In questa esercitazione viene illustrato come eseguire l'ordinamento e filtraggio e gestire scenari master-details. Si aggiungeranno i miglioramenti seguenti per il *Departments.aspx* pagina:

- Una casella di testo per consentire agli utenti di selezionare i reparti in base al nome.
- Un elenco di corsi per ogni reparto che viene visualizzato nella griglia.
- La possibilità di ordinare facendo clic sulle intestazioni di colonna.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)

## <a name="adding-the-ability-to-sort-gridview-columns"></a>Aggiunta della possibilità di GridView Ordina colonne

Aprire il *Departments.aspx* pagina e aggiungere un `SortParameterName="sortExpression"` attributo la `ObjectDataSource` controllo denominato `DepartmentsObjectDataSource`. (In seguito si creerà un `GetDepartments` metodo che accetta un parametro denominato `sortExpression`.) Il markup per il tag di apertura del controllo è ora simile all'esempio seguente.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample1.aspx)]

Aggiungere il `AllowSorting="true"` attributo al tag di apertura del `GridView` controllo. Il markup per il tag di apertura del controllo è ora simile all'esempio seguente.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample2.aspx)]

In *Departments.aspx.cs*, impostare l'ordinamento predefinito chiamando il `GridView` del controllo `Sort` metodo il `Page_Load` metodo:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample3.cs)]

È possibile aggiungere codice che consente di ordinare o i filtri nella classe della logica di business o la classe di repository. Se si fa nella classe della logica di business, all'ordinamento o filtro di lavoro non verrà eseguito dopo i dati vengono recuperati dal database, perché la classe di logica di business sta lavorando con un `IEnumerable` oggetto restituito dal repository. Se si aggiungono di ordinamento e filtro codice nella classe repository e farlo prima di un'espressione LINQ o query di oggetto è stato convertito in un `IEnumerable` dell'oggetto, i comandi verranno trasmesse al database per l'elaborazione, che è in genere più efficiente. In questa esercitazione viene implementato di ordinamento e filtro in modo che l'elaborazione da eseguire nel database, vale a dire nel repository.

Per aggiungere funzionalità di ordinamento, è necessario aggiungere un nuovo metodo per l'interfaccia del repository e classi repository anche la classe di logica di business. Nel *ISchoolRepository.cs* file, aggiungere un nuovo `GetDepartments` metodo che accetta un `sortExpression` parametro che verrà utilizzato per ordinare l'elenco dei reparti che viene restituito:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample4.cs)]

Il `sortExpression` parametro specificherà la colonna da ordinare e la direzione di ordinamento.

Aggiungere il codice per il nuovo metodo per il *SchoolRepository.cs* file:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample5.cs)]

Modificare l'oggetto esistente senza parametri `GetDepartments` metodo da chiamare il nuovo metodo:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample6.cs)]

Nel progetto di test, aggiungere il seguente nuovo metodo per *MockSchoolRepository.cs*:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample7.cs)]

Se si prevede di creare unit test che dipendono da questo metodo restituisce un elenco ordinato, è necessario ordinare l'elenco prima di restituirlo. È non essere la creazione di test simili a quelli presenti in questa esercitazione, pertanto il metodo può restituire solo l'elenco non ordinato di reparti.

Nel *SchoolBL.cs* file, aggiungere il seguente nuovo metodo alla classe della logica di business:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample8.cs)]

Questo codice passa il parametro di ordinamento per il metodo di repository.

Eseguire il *Departments.aspx* pagina.

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)

È ora possibile fare clic su qualsiasi intestazione di colonna per ordinare in base alla colonna. Se la colonna già ordinata, facendo clic sull'intestazione di inverte la direzione di ordinamento.

## <a name="adding-a-search-box"></a>Aggiunta di una casella di ricerca

In questa sezione verrà aggiunta una casella di testo di ricerca, collegarlo al `ObjectDataSource` controllare l'utilizzo di un parametro di controllo e aggiungere un metodo alla classe della logica di business per supportare l'applicazione di filtri.

Aprire il *Departments.aspx* pagina e aggiungere il markup seguente tra i primo e l'intestazione `ObjectDataSource` controllo:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample9.aspx)]

Nel `ObjectDataSource` controllo denominato `DepartmentsObjectDataSource`, eseguire le operazioni seguenti:

- Aggiungere un `SelectParameters` elemento per un parametro denominato `nameSearchString` che ottiene il valore immesso nel `SearchTextBox` controllo.
- Modifica il `SelectMethod` valore di attributo `GetDepartmentsByName`. (Si creeranno questo metodo in un secondo momento.)

Il markup per il `ObjectDataSource` controllo ora simile all'esempio seguente:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample10.aspx)]

In *ISchoolRepository.cs*, aggiungere un `GetDepartmentsByName` metodo che accetta entrambi `sortExpression` e `nameSearchString` parametri:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample11.cs)]

In *SchoolRepository.cs*, aggiungere il seguente nuovo metodo:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample12.cs)]

Questo codice Usa un `Where` metodo per selezionare gli elementi che contengono la stringa di ricerca. Se la stringa di ricerca è vuota, verranno selezionati tutti i record. Si noti che quando si specifica metodo chiama insieme in un'istruzione simile alla seguente (`Include`, quindi `OrderBy`, quindi `Where`), il `Where` (metodo) deve essere sempre l'ultimo.

Modificare esistenti `GetDepartments` metodo che accetta un `sortExpression` parametro per chiamare il nuovo metodo:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample13.cs)]

In *MockSchoolRepository.cs* nel progetto di test, aggiungere il seguente nuovo metodo:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample14.cs)]

In *SchoolBL.cs*, aggiungere il seguente nuovo metodo:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample15.cs)]

Eseguire il *Departments.aspx* pagina e immettere una stringa di ricerca per assicurarsi che la logica di selezione funzioni. Lasciare vuota la casella di testo e provare a eseguire una ricerca per assicurarsi che tutti i record vengono restituiti.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)

## <a name="adding-a-details-column-for-each-grid-row"></a>Aggiunta di una colonna di dettagli per ogni riga della griglia

Successivamente, si desidera visualizzare tutti i corsi per ogni reparto visualizzato nella cella della griglia destra. A tale scopo, si userà un nidificata `GridView` databind e controllo per i dati dal `Courses` proprietà di navigazione del `Department` entità.

Aprire *Departments.aspx* e nel markup di `GridView` controllare, specificare un gestore per il `RowDataBound` evento. Il markup per il tag di apertura del controllo è ora simile all'esempio seguente.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample16.aspx)]

Aggiungere un nuovo `TemplateField` elemento dopo il `Administrator` campo modello:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample17.aspx)]

Questo codice crea un tipo annidato `GridView` controllo che mostra il numero di corsi e un titolo di un elenco dei corsi. Non specifica un'origine dati perché databind, sarà necessario all'interno del codice nel `RowDataBound` gestore.

Aprire *Departments.aspx.cs* e aggiungere il seguente gestore per il `RowDataBound` evento:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample18.cs)]

Il codice ottiene la `Department` entità dagli argomenti dell'evento, converte il `Courses` proprietà di navigazione per un `List` insieme e associa dati nidificata `GridView` alla raccolta.

Aprire il *SchoolRepository.cs* file e il caricamento non differito per specificare il `Courses` proprietà di navigazione chiamando il `Include` metodo nella query di oggetto creato nel `GetDepartmentsByName` metodo. Il `return` istruzione il `GetDepartmentsByName` metodo ora simile all'esempio seguente.

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample19.cs)]

Eseguire la pagina. Oltre a ordinamento e filtro funzionalità aggiunta in precedenza, il controllo GridView viene ora dettagli corso nidificata per ogni reparto.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)

Introduzione agli scenari di ordinamento, filtro e master-Details è stata completata. Nella prossima esercitazione, si noterà come gestire la concorrenza.

> [!div class="step-by-step"]
> [Precedente](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
> [Successivo](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)
