---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-cs
title: Una panoramica di modifica ed eliminazione dei dati in DataList (c#) | Documenti Microsoft
author: rick-anderson
description: Quando il controllo DataList non dispone di modifica predefinite e l'eliminazione di funzionalità, in questa esercitazione si vedrà come creare un controllo DataList che supporta la modifica e l'eliminazione o...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: c3b0c86e-fe98-41ee-b26f-ca38cddaa75e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: be86707980b11453ef78fdbddead73ab9808b54d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="an-overview-of-editing-and-deleting-data-in-the-datalist-c"></a>Una panoramica di modifica ed eliminazione dei dati in DataList (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_36_CS.exe) o [Scarica il PDF](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/datatutorial36cs1.pdf)

> Quando il controllo DataList non dispone di modifica predefinite e l'eliminazione di funzionalità, in questa esercitazione vedremo come creare un controllo DataList che supporta la modifica ed eliminazione dei dati sottostanti.


## <a name="introduction"></a>Introduzione

Nel [una panoramica di inserimento, aggiornamento ed eliminazione di dati](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) esercitazione illustra in che modo inserire, aggiornare ed eliminare dati tramite l'architettura dell'applicazione, un ObjectDataSource e GridView, DetailsView e FormView controlli. Con ObjectDataSource e i controlli Web di tre dati, implementazione di interfacce di modifica di dati semplice è un gioco da ragazzi e coinvolti semplicemente scattare una casella di controllo da uno smart tag. Codice non deve essere scritto.

Purtroppo, DataList non predefinito, modificare ed eliminare le funzionalità intrinseche nel controllo GridView. Questa funzionalità manca è dovuta in parte per il fatto che DataList è un relic dalla versione precedente di ASP.NET, quando sono disponibili controlli origine dati dichiarativa e pagine di modifica di dati senza codice. Mentre DataList in ASP.NET 2.0 offre lo stesso non viene fornita la funzionalità di modifica dei dati di GridView, è possibile usare le tecniche di 1. x ASP.NET per includere tali funzionalità. Questo approccio richiede un po' di codice, ma come vedremo in questa esercitazione, DataList dispone alcuni eventi e proprietà per semplificare questo processo.

In questa esercitazione vedremo come creare un controllo DataList che supporta la modifica ed eliminazione dei dati sottostanti. Nelle esercitazioni successive verranno esaminate più avanzate di modifica e l'eliminazione di scenari, tra cui la convalida del campo di input, gestisce correttamente le eccezioni generate dall'accesso ai dati o un livello di logica di Business e così via.

> [!NOTE]
> Come DataList, controllo Repeater non dispone di fuori delle funzionalità casella per l'inserimento, aggiornamento o eliminazione. Sebbene sia possibile aggiungere tale funzionalità, DataList include proprietà ed eventi non trovati nel Repeater che semplificano l'aggiunta di tali funzionalità. Pertanto, in questa esercitazione e futuri che esaminare la modifica ed eliminazione esaminerà rigorosamente in DataList.


## <a name="step-1-creating-the-editing-and-deleting-tutorials-web-pages"></a>Passaggio 1: Creazione di pagine Web esercitazioni modifica ed eliminazione

Prima di iniziare l'esplorazione di come aggiornare ed eliminare dati da un controllo DataList, consentire s attentamente prima di creare le pagine ASP.NET nel progetto sito Web che è necessario per questa esercitazione, mentre quelle più avanti. Per iniziare, aggiungere una nuova cartella denominata `EditDeleteDataList`. Successivamente, aggiungere le seguenti pagine ASP.NET a quella cartella, assicurandosi di associare ogni pagina con il `Site.master` pagina master:

- `Default.aspx`
- `Basics.aspx`
- `BatchUpdate.aspx`
- `ErrorHandling.aspx`
- `UIValidation.aspx`
- `CustomizedUI.aspx`
- `OptimisticConcurrency.aspx`
- `ConfirmationOnDelete.aspx`
- `UserLevelAccess.aspx`


![Aggiungere le pagine ASP.NET per le esercitazioni](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image1.png)

**Figura 1**: aggiungere le pagine ASP.NET per le esercitazioni


Come in altre cartelle, `Default.aspx` nel `EditDeleteDataList` cartella vengono elencate le esercitazioni nella relativa sezione. Tenere presente che il `SectionLevelTutorialListing.ascx` controllo utente fornisce questa funzionalità. Pertanto, aggiungere il controllo utente `Default.aspx` trascinandolo da Esplora soluzioni nella pagina di visualizzazione della struttura s.


[![Aggiungere il controllo utente SectionLevelTutorialListing.ascx Default.aspx](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image3.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image2.png)

**Figura 2**: aggiungere il `SectionLevelTutorialListing.ascx` controllo utente al `Default.aspx` ([fare clic per visualizzare l'immagine ingrandita](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image4.png))


Infine, aggiungere le pagine come voci per il `Web.sitemap` file. In particolare, aggiungere il markup seguente dopo il report Master-Details DataList e Repeater `<siteMapNode>`:


[!code-xml[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample1.xml)]

Dopo aver aggiornato `Web.sitemap`, dedicare alcuni minuti per visualizzare il sito Web esercitazioni tramite un browser. Il menu a sinistra ora include le voci di modifica ed eliminazione esercitazioni DataList.


![Mappa del sito include ora le voci per DataList modifica ed eliminazione delle esercitazioni](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image5.png)

**Figura 3**: mappa del sito include ora le voci per DataList modifica ed eliminazione esercitazioni


## <a name="step-2-examining-techniques-for-updating-and-deleting-data"></a>Passaggio 2: Esaminare le tecniche per l'aggiornamento ed eliminazione dei dati

Modifica ed eliminazione di dati in GridView è estremamente semplice perché nel sistema il GridView e ObjectDataSource uso insieme. Come descritto nel [esaminando gli eventi associati con l'inserimento, aggiornamento ed eliminazione](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) esercitazione, quando viene premuto un pulsante di aggiornamento s riga GridView assegna automaticamente i campi utilizzati associazione dati bidirezionale per la `UpdateParameters` insieme di ObjectDataSource e quindi richiamata che s ObjectDataSource `Update()` metodo.

Purtroppo, DataList non è disponibile alcuna di queste funzionalità predefinite. È la responsabilità di garantire che i valori agli utenti vengono assegnati ai parametri s ObjectDataSource e che il relativo `Update()` metodo viene chiamato. Per facilitare tale operazione us, DataList fornisce le proprietà e gli eventi seguenti:

- **Il [ `DataKeyField` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basedatalist.datakeyfield.aspx)**  durante l'aggiornamento o eliminazione, è necessario essere in grado di identificare in modo univoco ogni elemento DataList. Impostare questa proprietà per il campo di chiave primaria dei dati visualizzati. In questo modo verrà popolato DataList s [ `DataKeys` raccolta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basedatalist.datakeys.aspx) con l'oggetto specificato `DataKeyField` valore per ogni elemento DataList.
- **Il [ `EditCommand` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.editcommand.aspx)**  viene generato quando un pulsante, LinkButton o ImageButton cui `CommandName` è impostata su viene fatto clic su Modifica.
- **Il [ `CancelCommand` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.cancelcommand.aspx)**  viene generato quando un pulsante, LinkButton o ImageButton cui `CommandName` è impostata su viene fatto clic su Annulla.
- **Il [ `UpdateCommand` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.updatecommand.aspx)**  viene generato quando un pulsante, LinkButton o ImageButton cui `CommandName` è impostata su viene fatto clic su Aggiorna.
- **Il [ `DeleteCommand` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.deletecommand.aspx)**  viene generato quando un pulsante, LinkButton o ImageButton cui `CommandName` è impostata su viene fatto clic su Elimina.

Utilizzando queste proprietà ed eventi, sono disponibili quattro approcci che è possibile utilizzare per aggiornare ed eliminare dati da DataList:

1. **Uso di tecniche 1.x ASP.NET** DataList esisteva prima di ASP.NET 2.0 e ObjectDataSources ed è stata in grado di aggiornare ed eliminare dati interamente a livello di. Questa tecnica. fossi completamente ObjectDataSource e richiede che l'associazione dati a DataList direttamente dal livello di logica di Business, sia per il recupero di dati da visualizzare e durante l'aggiornamento o eliminazione di un record.
2. **Utilizzando un singolo controllo ObjectDataSource nella pagina di selezione, aggiornamento ed eliminazione** quando DataList non dispone di GridView s inerente modificano e si eliminano le funzionalità, s non esiste alcun motivo è possibile t non aggiungerli nostra. Con questo approccio, è utilizzare ObjectDataSource come negli esempi di GridView, ma è necessario creare un gestore eventi per il controllo DataList s `UpdateCommand` evento in cui è impostato il parametri s ObjectDataSource e chiamare il relativo `Update()` metodo.
3. **Utilizzo di un controllo ObjectDataSource per la selezione, ma aggiornamento ed eliminazione direttamente con il livello Business LOGIC** quando si utilizza l'opzione 2, è necessario scrivere codice nel `UpdateCommand` evento, assegnazione di valori di parametro e così via. È invece possibile soffermeremo con ObjectDataSource per la selezione, ma eseguire le chiamate di aggiornamento ed eliminazione direttamente in base al livello Business LOGIC (ad esempio, con opzione 1). Ritengo, l'aggiornamento dei dati tramite l'interfaccia direttamente con il livello Business LOGIC comporta il codice più leggibile rispetto all'assegnazione s ObjectDataSource `UpdateParameters` e chiamando il relativo `Update()` metodo.
4. **Tramite mezzi dichiarativa tramite più ObjectDataSources** tre approcci precedenti tutti richiedono un bit di codice. Se d invece mantenere utilizzati come la sintassi dichiarativa molto possibile, un'opzione finale consiste nell'includere più ObjectDataSources nella pagina. Il primo ObjectDataSource recupera i dati da BLL e lo associa DataList. Per l'aggiornamento, un altro ObjectDataSource viene aggiunto, ma direttamente all'interno di DataList s `EditItemTemplate`. Per includere l'eliminazione di supporto, ancora un altro ObjectDataSource sarebbe necessari nel `ItemTemplate`. Con questo approccio, questi incorporati usano ObjectDataSource `ControlParameters` per associare in modo dichiarativo i parametri di s ObjectDataSource ai controlli di input utente (invece di dover specificare a livello di codice in DataList s `UpdateCommand` gestore dell'evento). Questo approccio richiede ancora un po' di codice, è necessario chiamare l'incorporato s ObjectDataSource `Update()` o `Delete()` comando ma richiede molto meno di con le altre tre approcci. In questo caso lo svantaggio è la più ObjectDataSources riempire la pagina, pregiudicare la leggibilità complessiva.

Se è necessario utilizzare solo uno di questi approcci, d è scegliere l'opzione 1 poiché offre la massima flessibilità e DataList è stato originariamente progettato per supportare questo pattern. Mentre DataList è stato esteso per lavorare con i controlli origine dati di ASP.NET 2.0, non dispone di tutti i punti di estendibilità o le funzionalità dei dati di ASP.NET 2.0 ufficiale controlli Web (GridView, DetailsView e FormView). Opzioni 2 a 4 non sono tuttavia senza merito.

Questo futuro la modifica e l'eliminazione di esercitazioni utilizzerà ObjectDataSource per recuperare i dati per visualizzare e le chiamate al livello Business Logic per aggiornare ed eliminare dati (opzione 3).

## <a name="step-3-adding-the-datalist-and-configuring-its-objectdatasource"></a>Passaggio 3: Aggiunta di DataList e configurazione ObjectDataSource

In questa esercitazione si creerà un DataList che elenca le informazioni di prodotto e, per ogni prodotto, consente all'utente la possibilità di modificare il nome e il prezzo e di eliminare completamente il prodotto. In particolare, si recupererà i record da visualizzare usando ObjectDataSource, ma eseguire l'aggiornamento e azioni di eliminazione interagendo direttamente con il livello Business LOGIC. Prima di preoccupazione che implementa le funzionalità di modificare ed eliminazione per il controllo DataList, consentire s prima di ottenere la pagina per visualizzare i prodotti in un'interfaccia di sola lettura. Poiché è ve esaminato questi passaggi nelle esercitazioni precedenti, sarà procedere al loro interno rapidamente.

Aprire il `Basics.aspx` nella pagina di `EditDeleteDataList` cartella e dalla visualizzazione progettazione, aggiungere un controllo DataList alla pagina. Successivamente, da DataList s smart tag, è possibile creare un nuovo oggetto ObjectDataSource. Poiché stiamo lavorando con dati di prodotto, configurare l'utilizzo di `ProductsBLL` classe. Per recuperare *tutti* prodotti, scegliere il `GetProducts()` metodo nella scheda Seleziona.


[![Configurare ObjectDataSource per utilizzare la classe ProductsBLL](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image7.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image6.png)

**Figura 4**: configurare ObjectDataSource per usare il `ProductsBLL` classe ([fare clic per visualizzare l'immagine ingrandita](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image8.png))


[![Restituire le informazioni sul prodotto utilizzando il metodo GetProducts()](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image10.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image9.png)

**Figura 5**: restituire le informazioni prodotto utilizzando il `GetProducts()` metodo ([fare clic per visualizzare l'immagine ingrandita](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image11.png))


DataList, ad esempio GridView, non è progettato per l'inserimento di nuovi dati. Pertanto, selezionare opzione (nessuno) dall'elenco a discesa nella scheda Inserisci. Inoltre scegliere (nessuno) per le schede UPDATE e DELETE perché gli aggiornamenti e le eliminazioni verranno eseguite a livello di codice tramite il livello Business LOGIC.


[![Verificare che l'elenco a discesa sono elencati in ObjectDataSource s INSERT, UPDATE ed eliminare le schede siano impostate su (nessuno)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image13.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image12.png)

**Figura 6**: confermare che l'elenco a discesa sono elencati nelle ObjectDataSource s INSERT, UPDATE, DELETE schede e siano impostati su (nessuno) ([fare clic per visualizzare l'immagine ingrandita](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image14.png))


Dopo aver configurato il ObjectDataSource, fare clic su Fine, tornare alla finestra di progettazione. Come è illustrata negli esempi precedenti, durante il completamento della configurazione di ObjectDataSource, Visual Studio automaticamente ve crea un `ItemTemplate` per DropDownList, visualizzando tutti i campi dati. Sostituire `ItemTemplate` con una che visualizza solo il nome del prodotto s e il prezzo. Inoltre, impostare il `RepeatColumns` proprietà su 2.

> [!NOTE]
> Come descritto nel *Panoramica di inserimento, aggiornamento ed eliminazione di dati* esercitazione, quando si modificano i dati utilizzando ObjectDataSource architettura richiede che si rimuove il `OldValuesParameterFormatString` proprietà da ObjectDataSource s markup dichiarativo (o reimpostata sul valore predefinito, `{0}`). In questa esercitazione, tuttavia, utilizziamo ObjectDataSource solo per il recupero dei dati. Pertanto, non è necessaria modificare gli oggetti ObjectDataSource `OldValuesParameterFormatString` valore della proprietà (anche se è t ridurre le a tale scopo).


Dopo aver sostituito il valore predefinito DataList `ItemTemplate` con uno personalizzato, il markup dichiarativo della pagina dovrebbe essere simile al seguente:


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample2.aspx)]

Richiedere qualche istante per visualizzare l'avanzamento attraverso un browser. Come illustrato nella figura 7, DataList consente di visualizzare il prezzo del prodotto, nome e l'unità per ogni prodotto in due colonne.


[![I nomi di prodotti e i prezzi vengono visualizzati in un controllo DataList due colonne](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image16.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image15.png)

**Figura 7**: il nomi di prodotti e i prezzi vengono visualizzati in un controllo DataList due colonne ([fare clic per visualizzare l'immagine ingrandita](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image17.png))


> [!NOTE]
> DataList ha un numero di proprietà necessarie per il processo di aggiornamento e l'eliminazione e tali valori vengono archiviati nello stato di visualizzazione. Pertanto, quando la compilazione di un controllo DataList che supporta la modifica o eliminazione di dati, è essenziale che lo stato di visualizzazione s DataList essere abilitato.  
>   
>  Il lettore attenti ricorderà che è stato possibile disabilitare lo stato di visualizzazione durante la creazione di GridView, DetailsViews e FormViews modificabile. Infatti, possono includere i controlli Web ASP.NET 2.0 *lo stato del controllo*, che è stato reso persistente durante i postback come lo stato di visualizzazione, ma sostituto essenziali.


La disabilitazione di visualizzazione dello stato in GridView semplicemente omette le informazioni sullo stato semplice, ma mantiene lo stato del controllo (che include lo stato necessario per la modifica ed eliminazione). DataList, creato nell'intervallo di tempo 1. x ASP.NET, non utilizzare lo stato del controllo e pertanto deve disporre dello stato di visualizzazione abilitato. Vedere [vs lo stato del controllo. Lo stato di visualizzazione](https://msdn.microsoft.com/library/1whwt1k7.aspx) per ulteriori informazioni sullo scopo dello stato di controllo e dallo stato di visualizzazione delle differenze.

## <a name="step-4-adding-an-editing-user-interface"></a>Passaggio 4: Aggiunta di un'interfaccia utente di modifica

Il controllo GridView è composta da una raccolta di campi (BoundField CheckBoxFields, TemplateFields e così via). Questi campi è possono modificare il markup sottoposto a rendering, a seconda della relativa modalità. Ad esempio, in modalità di sola lettura, un BoundField Visualizza il valore del campo dati come testo. Nella modalità di modifica, esegue il rendering di una casella di testo di Web controllo cui `Text` proprietà viene assegnato il valore del campo dati.

DataList, d'altra parte, esegue il rendering di elementi utilizzando i modelli. Rendering degli elementi di sola lettura utilizzando il `ItemTemplate` mentre gli elementi in modalità di modifica vengono visualizzati tramite il `EditItemTemplate`. A questo punto, il nostro DataList ha solo un `ItemTemplate`. Per supportare la funzionalità di modifica a livello di elemento è necessario aggiungere un `EditItemTemplate` contenente il markup da visualizzare per l'elemento modificabile. Per questa esercitazione, si userà i controlli casella di testo Web per la modifica di prodotto s nome e il prezzo unitario.

Il `EditItemTemplate` possono essere creati in modo dichiarativo o tramite la finestra di progettazione (selezionando l'opzione di modifica modelli dallo smart tag DataList s). Per utilizzare l'opzione di modifica modelli, innanzitutto fare clic sul collegamento di modifica modelli nello smart tag e quindi selezionare il `EditItemTemplate` elemento dall'elenco a discesa.


[![Optare per l'uso con EditItemTemplate s DataList](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image19.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image18.png)

**Figura 8**: optare per l'utilizzo di DataList s `EditItemTemplate` ([fare clic per visualizzare l'immagine ingrandita](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image20.png))


Successivamente, digitare il nome di prodotto: e prezzo: e quindi trascinare due controlli TextBox dalla casella degli strumenti nel `EditItemTemplate` interfaccia nella finestra di progettazione. Impostare le caselle di testo `ID` proprietà `ProductName` e `UnitPrice`.


[![Aggiungere una casella di testo per il prodotto s nome e il prezzo](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image22.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image21.png)

**Figura 9**: aggiungere una casella di testo per il nome di prodotto e prezzo ([fare clic per visualizzare l'immagine ingrandita](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image23.png))


È necessario associare i valori di campo di dati prodotto corrispondente per il `Text` le proprietà delle due caselle di testo. Da smart tag nelle caselle di testo, fare clic sul collegamento Modifica DataBindings e quindi associare il campo di dati appropriato con i `Text` proprietà, come illustrato nella figura 10.

> [!NOTE]
> Quando si associa il `UnitPrice` campo dei dati al prezzo di casella di testo s `Text` campo, è possibile formattarlo come valore di valuta (`{0:C}`), un numero generico (`{0:N}`), o lasciare il campo non formattato.


![Associare il ProductName e campi dati UnitPrice alle proprietà del testo delle caselle di testo](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image24.png)

**Figura 10**: associare il `ProductName` e `UnitPrice` campi dati per il `Text` le proprietà delle caselle di testo


Si noti il modo in cui utilizza la finestra di dialogo Modifica DataBindings nella figura 10 *non* includono la casella di controllo di associazione dati bidirezionale che è presente quando si modifica un TemplateField in GridView o DetailsView o un modello in FormView. Il valore immesso nel controllo di input Web venga assegnato automaticamente al s ObjectDataSource corrispondente è consentita la funzionalità di associazione dati bidirezionale `InsertParameters` o `UpdateParameters` durante l'inserimento o aggiornamento dei dati. DataList non supporta l'associazione dati bidirezionale come vedremo più avanti in questa esercitazione, dopo l'utente effettua proprio cambia ed è pronto per aggiornare i dati, è necessario accedere a livello di programmazione di queste caselle di testo `Text` proprietà e passare i valori per il appropriato `UpdateProduct` metodo la `ProductsBLL` classe.

Infine, è necessario aggiungere l'aggiornamento e annullamento di `EditItemTemplate`. Come illustrato nel [Master/dettaglio mediante un elenco puntato dei record Master con un controllo DataList dettagli](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) esercitazione, quando un pulsante, LinkButton o ImageButton cui `CommandName` impostata viene selezionato all'interno di un controllo Repeater o DataList, il S Repeater o DataList `ItemCommand` viene generato l'evento. Per il controllo DataList, se il `CommandName` è impostata su un determinato valore, un evento aggiuntivo può essere generato anche. Speciale `CommandName` includono i valori delle proprietà, tra gli altri:

- Annulla genera il `CancelCommand` evento
- Modifica genera il `EditCommand` evento
- Aggiornare genera il `UpdateCommand` evento

Tenere presente che questi eventi vengono generati *oltre a* il `ItemCommand` evento.

Aggiungere il `EditItemTemplate` due controlli pulsante Web, uno cui `CommandName` è impostato su Update e altri dispositivi impostato su Annulla. Dopo l'aggiunta di questi due controlli pulsante Web nella finestra di progettazione dovrebbe essere simile al seguente:


[![Aggiungere aggiornamento pulsanti EditItemTemplate e Annulla](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image26.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image25.png)

**Figura 11**: aggiungere aggiornare e annullare i pulsanti per il `EditItemTemplate` ([fare clic per visualizzare l'immagine ingrandita](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image27.png))


Con il `EditItemTemplate` completo markup dichiarativo DataList s dovrebbe essere simile al seguente:


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample3.aspx)]

## <a name="step-5-adding-the-plumbing-to-enter-edit-mode"></a>Passaggio 5: Aggiunta di Plumbing per passare alla modalità di modifica

A questo punto il nostro DataList è definita tramite un'interfaccia di modifica relativi `EditItemTemplate`; tuttavia, esiste s attualmente un modo per un utente visita la pagina per indicare che desidera modificare le informazioni di un prodotto s. È necessario aggiungere un pulsante Modifica per ogni prodotto che, quando si fa clic, esegue il rendering che DataList elemento in modalità di modifica. Per iniziare, aggiungere un pulsante Modifica per il `ItemTemplate`, tramite la finestra di progettazione o in modo dichiarativo. Assicurarsi di impostare il pulsante Modifica s `CommandName` proprietà da modificare.

Dopo aver aggiunto il pulsante Modifica, richiedere qualche istante per visualizzare la pagina tramite un browser. Con l'aggiunta, ogni voce del prodotto deve includere un pulsante Modifica.


[![Aggiungere aggiornamento pulsanti EditItemTemplate e Annulla](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image29.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image28.png)

**Figura 12**: aggiungere aggiornare e annullare i pulsanti per il `EditItemTemplate` ([fare clic per visualizzare l'immagine ingrandita](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image30.png))


Fare clic sul pulsante provoca un postback, ma *non* portare il prodotto di elenco in modalità di modifica. Per rendere modificabile il prodotto, è necessario:

1. Impostare DataList s [ `EditItemIndex` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) all'indice del `DataListItem` pulsante la cui modifica è stata selezionata.
2. Riassociare i dati di DataList. Quando viene eseguito il rendering, DataList il `DataListItem` il cui `ItemIndex` corrisponde a DataList s `EditItemIndex` verrà eseguito il rendering tramite il relativo `EditItemTemplate`.

Poiché il controllo DataList s `EditCommand` evento viene generato quando si fa clic sul pulsante di modifica, creare un `EditCommand` gestore dell'evento con il codice seguente:


[!code-csharp[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample4.cs)]

Il `EditCommand` gestore dell'evento viene passato un oggetto di tipo `DataListCommandEventArgs` come il secondo parametro di input, che include un riferimento al `DataListItem` è stato fatto clic sul pulsante la cui modifica (`e.Item`). Il gestore dell'evento imposta innanzitutto DataList s `EditItemIndex` per il `ItemIndex` di modificabile `DataListItem` e nuovamente i dati per il controllo DataList chiamando DataList s `DataBind()` metodo.

Dopo aver aggiunto il gestore eventi, rivedere la pagina in un browser. Fare clic sul pulsante Modifica ora esegue il prodotto selezionato modificabile (vedere Figura 13).


[![Scegliere la rende pulsante Modifica il prodotto modificabile](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image32.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image31.png)

**Figura 13**: fare clic sul pulsante Modifica rende il prodotto modificabile ([fare clic per visualizzare l'immagine ingrandita](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image33.png))


## <a name="step-6-saving-the-user-s-changes"></a>Passaggio 6: Il salvataggio delle modifiche s utente

La selezione prodotto modificato s, aggiornamento o pulsanti Annulla non esegue alcuna operazione. Per aggiungere questa funzionalità è necessario creare i gestori eventi per il controllo DataList s `UpdateCommand` e `CancelCommand` eventi. Iniziare creando la `CancelCommand` gestore eventi, che verrà eseguito quando si fa clic sul pulsante Annulla prodotto modificato s e occupano della restituzione DataList allo stato di pre-modifica.

Per eseguire il rendering di tutti gli elementi nella modalità di sola lettura DataList, è necessario:

1. Impostare DataList s [ `EditItemIndex` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) all'indice di un inesistente `DataListItem` indice. `-1` è una scelta sicura, poiché il `DataListItem` gli indici iniziano da `0`.
2. Riassociare i dati di DataList. Non essendo `DataListItem` `ItemIndex` es corrispondono a DataList s `EditItemIndex`, DataList intero verrà eseguito il rendering in una modalità di sola lettura.

Questa procedura può essere eseguita con il codice del gestore eventi seguente:


[!code-csharp[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample5.cs)]

Con l'aggiunta, fare clic su Annulla pulsante restituisce DataList allo stato di pre-modifica.

È l'ultimo gestore eventi è necessario completare il `UpdateCommand` gestore dell'evento. Questo gestore eventi deve:

1. Accedere a livello di codice il nome immesso dall'utente del prodotto e prezzo, nonché il prodotto modificato s `ProductID`.
2. Avviare il processo di aggiornamento chiamando il metodo appropriato `UpdateProduct` overload nella `ProductsBLL` classe.
3. Impostare DataList s [ `EditItemIndex` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) all'indice di un inesistente `DataListItem` indice. `-1` è una scelta sicura, poiché il `DataListItem` gli indici iniziano da `0`.
4. Riassociare i dati di DataList. Non essendo `DataListItem` `ItemIndex` es corrispondono a DataList s `EditItemIndex`, DataList intero verrà eseguito il rendering in una modalità di sola lettura.

I passaggi 1 e 2 sono responsabili per il salvataggio delle modifiche s; l'utente i passaggi 3 e 4 riportano DataList stato pre-modifica dopo le modifiche sono state salvate e sono identiche ai passaggi eseguiti nel `CancelCommand` gestore dell'evento.

Per ottenere il nome del prodotto aggiornato e il prezzo, è necessario utilizzare il `FindControl` metodo a livello di codice di riferimento controlli Web casella di testo all'interno di `EditItemTemplate`. È anche necessario ottenere il prodotto modificato s `ProductID` valore. Quando è associato inizialmente ObjectDataSource a DataList, Visual Studio assegnate DataList s `DataKeyField` proprietà al valore della chiave primaria dell'origine dati (`ProductID`). Questo valore può quindi essere recuperato da DataList s `DataKeys` insieme. È opportuno verificare che il `DataKeyField` è effettivamente impostata su `ProductID`.

Il codice seguente implementa i quattro passaggi:


[!code-csharp[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample6.cs)]

Il gestore dell'evento inizia con la lettura del prodotto modificato s `ProductID` dal `DataKeys` insieme. Successivamente, le due caselle di testo nel `EditItemTemplate` viene fatto riferimento e i relativi `Text` proprietà archiviate in variabili locali, `productNameValue` e `unitPriceValue`. Utilizziamo la `Decimal.Parse()` metodo da cui leggere il valore di `UnitPrice` casella di testo in modo che se il valore immesso è un simbolo di valuta, può ancora essere convertito correttamente in un `Decimal` valore.

> [!NOTE]
> I valori di `ProductName` e `UnitPrice` nelle caselle di testo viene assegnati solo per le variabili productNameValue e unitPriceValue se le proprietà del testo nelle caselle di testo sono specificato un valore. In caso contrario, un valore di `Nothing` viene utilizzato per le variabili, che ha l'effetto di aggiornamento dei dati con un database `NULL` valore. Vale a dire il nostro codice considera converte le stringhe di database vuote `NULL` valori, che è il comportamento predefinito dell'interfaccia di modifica nel controllo GridView, DetailsView e FormView.


Dopo la lettura dei valori, il `ProductsBLL` classe s `UpdateProduct` metodo viene chiamato, passando il nome del prodotto s, prezzo, e `ProductID`. Il gestore dell'evento di completamento restituendo DataList lo stato di pre-modifica utilizzando la stessa logica esattamente come nel `CancelCommand` gestore dell'evento.

Con il `EditCommand`, `CancelCommand`, e `UpdateCommand` completare i gestori eventi, un visitatore è possibile modificare il nome e il prezzo di un prodotto. 14-16 cifre mostrano il flusso di lavoro di modifica in azione.


[![Quando si trovano in modalità di sola lettura prima visita la pagina, tutti i prodotti](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image35.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image34.png)

**Figura 14**: durante la prima visita la pagina, tutti i prodotti sono in modalità di sola lettura ([fare clic per visualizzare l'immagine ingrandita](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image36.png))


[![Per aggiornare un prodotto s nome o il prezzo, fare clic sul pulsante Modifica](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image38.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image37.png)

**Figura 15**: per aggiornare un prodotto s nome o il prezzo, fare clic sul pulsante Modifica ([fare clic per visualizzare l'immagine ingrandita](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image39.png))


[![Dopo aver modificato il valore, fare clic su Aggiorna per tornare alla modalità di sola lettura](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image41.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image40.png)

**Figura 16**: dopo la modifica del valore, fare clic su Aggiorna per tornare alla modalità di sola lettura ([fare clic per visualizzare l'immagine ingrandita](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image42.png))


## <a name="step-7-adding-delete-capabilities"></a>Passaggio 7: Aggiunta di funzionalità di eliminazione

I passaggi per l'aggiunta di funzionalità di eliminazione a un controllo DataList sono simili a quelle per l'aggiunta di funzionalità di modifica. In breve, è necessario aggiungere un pulsante di eliminazione per il `ItemTemplate` che, quando si fa clic:

1. Legge in prodotto corrispondente s `ProductID` tramite il `DataKeys` insieme.
2. Esegue l'eliminazione chiamando il `ProductsBLL` classe s `DeleteProduct` metodo.
3. Nuovamente l'associazione dati DataList.

Consente di iniziare, aggiungere un pulsante Elimina per s il `ItemTemplate`.

Quando si fa clic, un pulsante la cui `CommandName` è modifica, aggiornamento o Annulla genera DataList s `ItemCommand` evento insieme a un evento aggiuntivo (ad esempio, quando l'uso di modifica di `EditCommand` evento viene generato anche). Analogamente, qualsiasi pulsante, LinkButton o ImageButton in DataList il cui `CommandName` proprietà è impostata per eliminare le cause di `DeleteCommand` l'evento (insieme a `ItemCommand`).

Aggiungere un pulsante Elimina accanto al pulsante di modifica nel `ItemTemplate`, impostando il relativo `CommandName` proprietà da eliminare. Dopo l'aggiunta di questo pulsante di controllo del controllo DataList s `ItemTemplate` la sintassi dichiarativa simile:


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample7.aspx)]

Successivamente, creare un gestore eventi per il controllo DataList s `DeleteCommand` evento, usando il codice seguente:


[!code-csharp[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample8.cs)]

Fare clic sul pulsante Delete provoca un postback e genera un avviso DataList s `DeleteCommand` evento. Nel gestore eventi, il prodotto si fa clic s `ProductID` valore sono accessibili dal `DataKeys` insieme. Successivamente, il prodotto viene eliminato chiamando il `ProductsBLL` classe s `DeleteProduct` metodo.

Dopo l'eliminazione del prodotto, è importante che abbiamo riassociare i dati DataList s (`DataList1.DataBind()`), in caso contrario DataList continuerà a visualizzare il prodotto che ha appena è stato eliminato.

## <a name="summary"></a>Riepilogo

Mentre DataList non è presente il punto e fare clic su Modifica ed eliminazione di supporto per GridView, con un bit breve del codice può essere estesa per includere queste funzionalità. In questa esercitazione è stato illustrato come creare un elenco di due colonne di prodotti che possono essere eliminati e il cui nome e il prezzo può essere modificati. Aggiunta, modifica ed eliminazione di supporto è una questione di inclusione di controlli Web appropriati di `ItemTemplate` e `EditItemTemplate`, la creazione di gestori di eventi corrispondenti, la lettura dei valori di chiave primari e immessi dall'utente e l'interazione con l'azienda Livello di logica.

Anche se sono state aggiunte modifiche di base e l'eliminazione di funzionalità per il controllo DataList, non dispone di funzionalità più avanzate. Ad esempio, non è presente alcuna convalida del campo di input - se un utente immette un prezzo di troppi costosi, verrà generata l'eccezione da `Decimal.Parse` durante il tentativo di convertire troppo costoso in una `Decimal`. Analogamente, se si verifica un problema durante l'aggiornamento i dati nella logica di Business o i livelli di accesso ai dati l'utente visualizzerà la schermata di errore standard. Senza una sorta di conferma sul pulsante Elimina, l'eliminazione accidentale di un prodotto è tutte troppo probabile.

In futuro esercitazioni vedremo come migliorare l'utente modifica esperienza.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Lead revisori per questa esercitazione sono stati Zack Jones e Ken Pespisa Randy Schmidt. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [avanti](performing-batch-updates-cs.md)
