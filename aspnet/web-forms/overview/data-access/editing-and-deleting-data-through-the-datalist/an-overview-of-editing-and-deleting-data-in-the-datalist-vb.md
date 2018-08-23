---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-vb
title: Una panoramica della modifica ed eliminazione dei dati in DataList (VB) | Microsoft Docs
author: rick-anderson
description: Mentre il controllo DataList manca incorporati di modifica ed eliminazione di funzionalità, in questa esercitazione verrà illustrato come creare un controllo DataList che supporta la modifica e l'eliminazione o...
ms.author: riande
ms.date: 10/30/2006
ms.assetid: 9410a23c-9697-4f07-bd71-e62b0ceac655
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-vb
msc.type: authoredcontent
ms.openlocfilehash: b1c7834e67f7682f82ecd0b2b5140260d104aecc
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832817"
---
<a name="an-overview-of-editing-and-deleting-data-in-the-datalist-vb"></a>Una panoramica della modifica ed eliminazione dei dati in DataList (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_36_VB.exe) o [Scarica il PDF](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/datatutorial36vb1.pdf)

> Mentre il controllo DataList manca incorporati di modifica ed eliminazione di funzionalità, in questa esercitazione vedremo come creare un controllo DataList che supporta la modifica ed eliminazione dei relativi dati sottostanti.


## <a name="introduction"></a>Introduzione

Nel [una panoramica di inserimento, aggiornamento ed eliminazione di dati](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) esercitazione è stato descritto come inserire, aggiornare ed eliminare dati tramite l'architettura dell'applicazione, ObjectDataSource e GridView, DetailsView e FormView controlli. Con ObjectDataSource e i controlli Web dei dati di tre, implementare le interfacce di modifica di dati semplice è stato un gioco da ragazzi e coinvolti scattare semplicemente una casella di controllo da uno smart tag. Nessun codice dovevano essere scritta.

Sfortunatamente, DataList non ha integrato modifica ed eliminazione funzionalità intrinseche nel controllo GridView. Questa funzionalità manca è dovuta in parte al fatto che il controllo DataList è un relic dalla versione precedente di ASP.NET, quando i controlli origine dati dichiarativi e pagine di modifica di dati senza codice sono state disponibili. Mentre DataList in ASP.NET 2.0 non offre lo stesso viene fornita la funzionalità di modifica dei dati come GridView, è possibile usare ASP.NET 1.x tecniche per includere tali funzionalità. Questo approccio richiede un po' di codice, ma, come vedremo in questa esercitazione, DataList include alcune proprietà e gli eventi per semplificare questo processo.

In questa esercitazione vedremo come creare un controllo DataList che supporta la modifica ed eliminazione dei relativi dati sottostanti. Nelle esercitazioni successive verranno esaminate più avanzate di modifica e l'eliminazione di scenari, tra cui la convalida del campo di input, gestisce correttamente le eccezioni generate dall'accesso ai dati o livelli di logica di Business e così via.

> [!NOTE]
> Come DataList, del controllo Repeater non ha l'esaurimento delle funzionalità casella per l'inserimento, aggiornamento o eliminazione. Sebbene sia possibile aggiungere tale funzionalità, DataList include proprietà ed eventi non trovati nel Repeater che semplificano l'aggiunta di tali funzionalità. Pertanto, questa esercitazione e quelli futuri che esaminano la modifica ed eliminazione verranno concentrerò esclusivamente sullo DataList.


## <a name="step-1-creating-the-editing-and-deleting-tutorials-web-pages"></a>Passaggio 1: Creazione di pagine Web di esercitazioni modifica ed eliminazione

Prima di iniziare a esplorare come aggiornare ed eliminare i dati da un controllo DataList, consentire s prima di tutto si consiglia di creare le pagine ASP.NET nel progetto di sito Web che è necessario per questa esercitazione e quelli più avanti. Iniziare aggiungendo una nuova cartella denominata `EditDeleteDataList`. Successivamente, aggiungere le seguenti pagine ASP.NET per quella cartella, assicurandosi di associare ogni pagina con il `Site.master` pagina master:

- `Default.aspx`
- `Basics.aspx`
- `BatchUpdate.aspx`
- `ErrorHandling.aspx`
- `UIValidation.aspx`
- `CustomizedUI.aspx`
- `OptimisticConcurrency.aspx`
- `ConfirmationOnDelete.aspx`
- `UserLevelAccess.aspx`


![Aggiungere le pagine ASP.NET per le esercitazioni](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image1.png)

**Figura 1**: aggiungere le pagine ASP.NET per le esercitazioni


Come nelle altre cartelle, `Default.aspx` nella `EditDeleteDataList` cartella vengono elencate le esercitazioni nella relativa sezione. Si tenga presente che il `SectionLevelTutorialListing.ascx` controllo utente fornisce questa funzionalità. Pertanto, aggiungere questo controllo utente da `Default.aspx` trascinandolo da Esplora soluzioni nella pagina di visualizzazione della struttura s.


[![Aggiungere il controllo utente sectionleveltutoriallisting. ascx a default. aspx](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image3.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image2.png)

**Figura 2**: aggiungere i `SectionLevelTutorialListing.ascx` controllo utente da `Default.aspx` ([fare clic per visualizzare l'immagine con dimensioni normali](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image4.png))


Infine, aggiungere le pagine come voci per il `Web.sitemap` file. In particolare, aggiungere il markup seguente dopo il report Master-Details con DataList e Repeater `<siteMapNode>`:


[!code-xml[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample1.xml)]

Dopo aver aggiornato `Web.sitemap`, si consiglia di visualizzare il sito Web di esercitazioni tramite un browser. Il menu a sinistra include ora le voci di DataList modifica ed eliminazione di esercitazioni.


![Mappa del sito include ora voci per il controllo DataList di modifica ed eliminazione esercitazioni](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image5.png)

**Figura 3**: la mappa del sito include ora voci per il controllo DataList di modifica ed eliminazione esercitazioni


## <a name="step-2-examining-techniques-for-updating-and-deleting-data"></a>Passaggio 2: Esaminare le tecniche per l'aggiornamento ed eliminazione dei dati

Modifica ed eliminazione dei dati con il controllo GridView è estremamente semplice perché, sotto le quinte, il controllo GridView e ObjectDataSource funzionano in concerto. Come descritto nel [esaminare gli eventi associati con inserimento, aggiornamento ed eliminazione](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) esercitazione, quando si fa clic sul pulsante Aggiorna una riga o le righe, il controllo GridView assegna automaticamente i relativi campi usati Data Binding bidirezionale per il `UpdateParameters` raccolta di ObjectDataSource e richiama quindi che s ObjectDataSource `Update()` (metodo).

Purtroppo, DataList non fornisce queste funzionalità predefinite. È il compito del programmatore garantire che i valori di assegnare agli utenti vengono assegnati ai parametri s ObjectDataSource e che il `Update()` viene chiamato il metodo. Per favorire noi in questa attività, DataList fornisce le proprietà e gli eventi seguenti:

- **Il [ `DataKeyField` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basedatalist.datakeyfield.aspx)**  durante l'aggiornamento o eliminazione, è necessario essere in grado di identificare in modo univoco ogni elemento DataList. Impostare questa proprietà per il campo di chiave primaria dei dati visualizzati. In questo modo verrà popolato s DataList [ `DataKeys` collection](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basedatalist.datakeys.aspx) con la proprietà specificata `DataKeyField` valore per ogni elemento DataList.
- **Il [ `EditCommand` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.editcommand.aspx)**  viene attivata quando un pulsante, LinkButton e ImageButton cui `CommandName` è impostata su viene fatto clic su Modifica.
- **Il [ `CancelCommand` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.cancelcommand.aspx)**  viene attivata quando un pulsante, LinkButton e ImageButton cui `CommandName` è impostata su viene fatto clic su Annulla.
- **Il [ `UpdateCommand` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.updatecommand.aspx)**  viene attivata quando un pulsante, LinkButton e ImageButton cui `CommandName` è impostata su viene fatto clic su Aggiorna.
- **Il [ `DeleteCommand` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.deletecommand.aspx)**  viene attivata quando un pulsante, LinkButton e ImageButton cui `CommandName` è impostata su viene fatto clic su Elimina.

Utilizzando queste proprietà ed eventi, sono disponibili quattro approcci che è possibile usare per aggiornare ed eliminare dati dalle DataList:

1. **Con ASP.NET 1.x tecniche** DataList esisteva prima di ASP.NET 2.0 e ObjectDataSource ed è stata in grado di aggiornare ed eliminare interamente a livello di dati. Questa tecnica fossi completamente ObjectDataSource e richiede è associare i dati per il controllo DataList direttamente dal livello della logica di Business, sia durante il recupero dei dati da visualizzare e durante l'aggiornamento o eliminazione di un record.
2. **Usando un singolo controllo ObjectDataSource nella pagina di selezione, aggiornamento ed eliminazione** mentre DataList non dispone di GridView s intrinseci di modifica ed eliminazione di funzionalità, 3!s ci alcun motivo possiamo t non aggiungerli in noi stessi. Con questo approccio, è utilizzare analoga a ObjectDataSource in GridView examples, ma è necessario creare un gestore eventi per il controllo DataList s `UpdateCommand` eventi in cui abbiamo impostato i parametri di ObjectDataSource s e chiamano relativo `Update()` (metodo).
3. **Utilizzo di un controllo ObjectDataSource per la selezione, ma l'aggiornamento e l'eliminazione direttamente con il livello BLL** quando si usa l'opzione 2, è necessario scrivere un frammento di codice nel `UpdateCommand` evento, l'assegnazione di valori di parametro e così via. È possibile invece rispettarla utilizzando ObjectDataSource per la selezione, ma effettuare le chiamate ad aggiornamento e l'eliminazione direttamente con il livello BLL (ad esempio, con opzione 1). A mio parere, l'aggiornamento dei dati tramite l'interazione diretta con il livello BLL conduce al codice più leggibile rispetto all'assegnazione s ObjectDataSource `UpdateParameters` e chiamando il `Update()` (metodo).
4. **Utilizzando metodi dichiarativi attraverso più ObjectDataSource** tre approcci precedenti tutti richiedono un po' di codice. Se è il d continuare invece a usare come sintassi dichiarativa molto presto, un'opzione finale consiste nell'includere più ObjectDataSource nella pagina. Il primo ObjectDataSource recupera i dati da BLL e associarlo a DataList. Per l'aggiornamento, un altro oggetto ObjectDataSource viene aggiunto, ma direttamente all'interno di DataList s `EditItemTemplate`. Per includere l'eliminazione di supporto, ancora un altro oggetto ObjectDataSource sarebbero necessari nel `ItemTemplate`. Con questo approccio, queste incorporato uso ObjectDataSource `ControlParameters` per associare in modo dichiarativo i parametri di ObjectDataSource s per i controlli di input utente (invece di dover specificare a livello di codice in DataList s `UpdateCommand` gestore dell'evento). Questo approccio richiede ancora un po' di codice è necessario chiamare l'incorporato s ObjectDataSource `Update()` o `Delete()` comando ma che richiede molto inferiori con le altre tre approcci. In questo caso lo svantaggio è che i più ObjectDataSource ingombrano pagina distoglie l'attenzione dalla leggibilità complessiva.

Se era necessario usare solo uno di questi approcci, è il d scegliere l'opzione 1 perché offre la massima flessibilità e DataList è stata originariamente progettato per supportare questo pattern. Mentre il controllo DataList è stato esteso per lavorare con i controlli origine dati di ASP.NET 2.0, non ha invece tutti i punti di estendibilità o le funzionalità dei dati di ASP.NET 2.0 ufficiale controlli Web (il controllo GridView, DetailsView e FormView). Opzioni 2 a 4 non sono tuttavia privo di fondamento.

Questo e il futuro modifica ed eliminazione esercitazioni utilizzerà ObjectDataSource per recuperare i dati per visualizzare e indirizzare le chiamate per il livello BLL aggiornare ed eliminare i dati (opzione 3).

## <a name="step-3-adding-the-datalist-and-configuring-its-objectdatasource"></a>Passaggio 3: Aggiunta di DataList e configurazione di ObjectDataSource

In questa esercitazione si creerà un controllo DataList che elenca le informazioni sul prodotto e, per ogni prodotto, fornisce all'utente la possibilità di modificare il nome e il prezzo e per eliminare completamente il prodotto. In particolare, si recupererà i record da visualizzare usando ObjectDataSource, ma eseguire l'aggiornamento e azioni di eliminazione da interfacciarsi direttamente con il livello BLL. Prima ci preoccupiamo implementa le funzionalità di modifica ed eliminazione per il controllo DataList, farti s prima di tutto la pagina per visualizzare i prodotti in un'interfaccia di sola lettura. Poiché si va esaminato questi passaggi nelle esercitazioni precedenti, mi accingo a ognuna di esse rapidamente.

Iniziare aprendo il `Basics.aspx` nella pagina di `EditDeleteDataList` cartella e dalla visualizzazione progettazione, aggiungere un controllo DataList alla pagina. Successivamente, DataList s nello smart tag, creare un nuovo oggetto ObjectDataSource. Poiché stiamo lavorando con dati del prodotto, configurarlo per usare il `ProductsBLL` classe. Per recuperare *tutte* prodotti, scegliere il `GetProducts()` metodo nella scheda Seleziona.


[![Configurare ObjectDataSource per usare la classe ProductsBLL](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image7.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image6.png)

**Figura 4**: configurare ObjectDataSource per usare il `ProductsBLL` classe ([fare clic per visualizzare l'immagine con dimensioni normali](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image8.png))


[![Restituisce le informazioni sul prodotto utilizzando il metodo GetProducts()](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image10.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image9.png)

**Figura 5**: restituisce le informazioni di prodotto usando il `GetProducts()` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image11.png))


DataList, come per GridView, non è progettato per l'inserimento di nuovi dati. Pertanto, selezionare opzione (nessuno) nell'elenco a discesa nella scheda Inserisci. Scegliere anche (nessuno) per le schede UPDATE e DELETE perché gli aggiornamenti e le eliminazioni verranno eseguite a livello di programmazione tramite il livello BLL.


[![Verificare che l'elenco a discesa Elenca in ObjectDataSource s inserimento, aggiornamento ed eliminare le schede siano impostate su (nessuno)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image13.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image12.png)

**Figura 6**: confermare che l'elenco a discesa sono elencati in s inserimento, aggiornamento ed eliminare schede di ObjectDataSource sono impostati su (nessuno) ([fare clic per visualizzare l'immagine con dimensioni normali](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image14.png))


Dopo la configurazione di ObjectDataSource, fare clic su Fine, tornare alla finestra di progettazione. Come abbiamo ve illustrata negli esempi precedenti, durante il completamento della configurazione di ObjectDataSource, Visual Studio automaticamente crea un `ItemTemplate` per DropDownList, visualizzando ogni campo dati. Sostituire questo `ItemTemplate` con uno che visualizza solo il nome del prodotto s e prezzo. Inoltre, impostare il `RepeatColumns` proprietà su 2.

> [!NOTE]
> Come descritto nel *Panoramica di inserimento, aggiornamento ed eliminazione di dati* esercitazione, quando si modificano i dati con ObjectDataSource richiede la nostra architettura che è possibile rimuovere il `OldValuesParameterFormatString` proprietà da s ObjectDataSource markup dichiarativo (o reimpostata sul valore predefinito, `{0}`). In questa esercitazione, tuttavia, utilizziamo l'oggetto ObjectDataSource solo per il recupero dei dati. Pertanto, non è necessario modificare gli oggetti ObjectDataSource `OldValuesParameterFormatString` valore della proprietà (anche se si t influire negativamente a tale scopo).


Dopo aver sostituito il valore predefinito DataList `ItemTemplate` con uno personalizzato, il markup dichiarativo nella pagina dovrebbe essere simile al seguente:


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample2.aspx)]

Si consiglia di visualizzare lo stato di avanzamento tramite un browser. Come illustrato nella figura 7, DataList consente di visualizzare il prezzo del prodotto, nome e l'unità per ogni prodotto di due colonne.


[![I nomi di prodotti e i prezzi vengono visualizzati in un controllo DataList di due colonne](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image16.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image15.png)

**Figura 7**: i nomi dei prodotti di e i prezzi vengono visualizzati in un controllo DataList di due colonne ([fare clic per visualizzare l'immagine con dimensioni normali](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image17.png))


> [!NOTE]
> DataList è dotato di numerose proprietà che sono necessari per il processo di aggiornamento e l'eliminazione e questi valori vengono archiviati nello stato di visualizzazione. Pertanto, quando si compila un controllo DataList che supporta la modifica o eliminazione dei dati, è essenziale che lo stato di visualizzazione s DataList sia attivato.  
>   
>  Il lettore astuto potrebbe tenere presente che è stato in grado di disabilitare lo stato di visualizzazione durante la creazione di GridView, maggior facilità ai DetailsView e FormViews modificabile. Infatti, possono includere i controlli Web ASP.NET 2.0 *lo stato del controllo*, che è stato persistente tramite i postback, ad esempio lo stato di visualizzazione, ma essenziali sostituto.


La disabilitazione di visualizzazione dello stato in GridView semplicemente omette le informazioni sullo stato banale, ma mantiene lo stato del controllo (che include lo stato necessario per la modifica ed eliminazione). DataList, nell'intervallo di tempo ASP.NET 1.x, risulta creato non utilizza lo stato del controllo e pertanto deve avere lo stato di visualizzazione abilitato. Vedere [Visual Studio lo stato del controllo. Lo stato di visualizzazione](https://msdn.microsoft.com/library/1whwt1k7.aspx) per altre informazioni sullo scopo dello stato di controllo e la modalità è diverso dallo stato di visualizzazione.

## <a name="step-4-adding-an-editing-user-interface"></a>Passaggio 4: Aggiunta di un'interfaccia utente di modifica

Il controllo GridView è costituito da una raccolta di campi (BoundField, CheckBoxFields, TemplateFields e così via). Questi campi è possono modificare il markup sottoposto a rendering in base alla loro modalità. Ad esempio, quando è in modalità di sola lettura, un BoundField Visualizza il valore del campo dati come testo. in modalità di modifica, esegue il rendering di un sito Web nella casella di testo controllo la cui `Text` proprietà viene assegnato il valore del campo dati.

DataList, d'altra parte, esegue il rendering dei relativi elementi usando i modelli. Rendering degli elementi di sola lettura usando il `ItemTemplate` mentre gli elementi in modalità di modifica vengono sottoposti a rendering tramite il `EditItemTemplate`. A questo punto, il DataList ha solo un `ItemTemplate`. Per supportare la funzionalità di modifica a livello di elemento è necessario aggiungere un `EditItemTemplate` contenente il markup da visualizzare per l'elemento modificabile. Per questa esercitazione, useremo i controlli Web nella casella di testo per la modifica di prodotto s nome e il prezzo unitario.

Il `EditItemTemplate` può essere creato in modo dichiarativo o tramite la finestra di progettazione (selezionando l'opzione di modifica modelli dello smart tag DataList s). Per usare l'opzione di modifica modelli, prima di tutto fare clic sul collegamento di modifica modelli nello smart tag e quindi selezionare il `EditItemTemplate` elemento nell'elenco a discesa.


[![Consenso esplicito per l'uso con EditItemTemplate s DataList](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image19.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image18.png)

**Figura 8**: consenso esplicito per l'uso con il DataList s `EditItemTemplate` ([fare clic per visualizzare l'immagine con dimensioni normali](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image20.png))


Successivamente, digitare il nome del prodotto: e prezzo: e quindi trascinare due caselle di testo dalla casella degli strumenti nel `EditItemTemplate` interfaccia nella finestra di progettazione. Impostare le caselle di testo `ID` delle proprietà per `ProductName` e `UnitPrice`.


[![Aggiungere una casella di testo per lo s nome prodotto e prezzo](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image22.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image21.png)

**Figura 9**: aggiungere una casella di testo per il nome di prodotto e prezzo ([fare clic per visualizzare l'immagine con dimensioni normali](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image23.png))


È necessario associare i valori dei campi dati del prodotto corrispondente per il `Text` le proprietà delle due caselle di testo. Gli smart tag nelle caselle di testo, fare clic sul collegamento Modifica DataBindings e quindi associare il campo di dati appropriato con i `Text` proprietà, come illustrato nella figura 10.

> [!NOTE]
> Quando si associa la `UnitPrice` campo dati il prezzo s nella casella di testo `Text` campo, è possibile formattarlo come valore di valuta (`{0:C}`), un numero generico (`{0:N}`), o lasciare il campo non formattato.


![Associare i campi di dati di UnitPrice e ProductName per le proprietà di testo delle caselle di testo](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image24.png)

**Figura 10**: associare il `ProductName` e `UnitPrice` campi dati i `Text` le proprietà delle caselle di testo


Si noti come la finestra di dialogo Modifica DataBindings nella figura 10 fa *non* includono la casella di controllo di associazione dati bidirezionale che è presente quando si modifica un TemplateField in GridView o DetailsView o un modello nel controllo FormView. Il valore immesso nel controllo Web di input venga assegnato automaticamente ai dispositivi ObjectDataSource corrispondenti è consentita la funzionalità di associazione dati bidirezionale `InsertParameters` o `UpdateParameters` durante l'inserimento o aggiornamento dei dati. DataList non supporta l'associazione dati bidirezionale come vedremo più avanti in questa esercitazione, dopo l'utente effettua altre relativa cambia ed è pronto per aggiornare i dati, è necessario accedere a livello di programmazione di queste caselle di testo `Text` proprietà e passare i valori per il appropriato `UpdateProduct` metodo di `ProductsBLL` classe.

Infine, è necessario aggiungere l'aggiornamento e i pulsanti per annullare il `EditItemTemplate`. Come abbiamo visto nel [Master/dettaglio mediante un elenco puntato di record Master e un controllo DataList di dettagli](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) esercitazione, quando un pulsante, LinkButton e ImageButton cui `CommandName` viene impostata fa all'interno di un controllo Repeater o DataList, il DataList o Repeater s `ItemCommand` viene generato l'evento. Per il controllo DataList, se il `CommandName` è impostata su un determinato valore, un evento aggiuntivo può essere generato anche. Speciale `CommandName` tra gli altri valori di proprietà includono:

- Annulla genera il `CancelCommand` evento
- Modifica genera il `EditCommand` evento
- Aggiornare genera il `UpdateCommand` evento

Tenere presente che questi eventi vengono generati *oltre alle* il `ItemCommand` evento.

Aggiungere il `EditItemTemplate` due controlli pulsante Web, uno cui `CommandName` è impostato su aggiornamento e di altri spazi dei nomi impostato su Annulla. Dopo l'aggiunta di questi due controlli Web di pulsante della finestra di progettazione dovrebbe essere simile al seguente:


[![Aggiungere l'aggiornamento e i pulsanti per EditItemTemplate Annulla](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image26.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image25.png)

**Figura 11**: aggiungere aggiornare e annullare i pulsanti per il `EditItemTemplate` ([fare clic per visualizzare l'immagine con dimensioni normali](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image27.png))


Con la `EditItemTemplate` completo markup dichiarativo DataList s dovrebbe essere simile al seguente:


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample3.aspx)]

## <a name="step-5-adding-the-plumbing-to-enter-edit-mode"></a>Passaggio 5: Aggiungere il codice interno per attivare la modalità di modifica

A questo punto il DataList ha definita tramite un'interfaccia di modifica relativo `EditItemTemplate`; tuttavia, qui s attualmente un modo per un utente potrebbe visitare la pagina per indicare che si desidera modificare le informazioni di un prodotto s. È necessario aggiungere un pulsante Modifica per ogni prodotto che, quando si fa clic, esegue il rendering che DataList elemento in modalità di modifica. Iniziare aggiungendo un pulsante Modifica per il `ItemTemplate`, tramite la finestra di progettazione o in modo dichiarativo. Assicurarsi di impostare il pulsante Modifica s `CommandName` proprietà da modificare.

Dopo aver aggiunto questo pulsante di modifica, si consiglia di visualizzare la pagina tramite un browser. Grazie a questa aggiunta, ogni presentazione del prodotto deve includere un pulsante di modifica.


[![Aggiungere l'aggiornamento e i pulsanti per EditItemTemplate Annulla](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image29.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image28.png)

**Figura 12**: aggiungere aggiornare e annullare i pulsanti per il `EditItemTemplate` ([fare clic per visualizzare l'immagine con dimensioni normali](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image30.png))


Fare clic sul pulsante causa un postback, ma *non* portare il prodotto listato alla modalità di modifica. Per rendere modificabile il prodotto, è necessario:

1. Impostare il controllo DataList s [ `EditItemIndex` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) all'indice del `DataListItem` cui modifica è stato appena fatto clic sul pulsante.
2. Riassociare i dati di DataList. Quando il controllo DataList viene nuovamente eseguito il rendering, il `DataListItem` la cui `ItemIndex` corrisponde con il DataList s `EditItemIndex` verrà eseguito il rendering utilizzando relativo `EditItemTemplate`.

Poiché il controllo DataList s `EditCommand` evento viene generato quando si fa clic sul pulsante di modifica, creare un `EditCommand` gestore dell'evento con il codice seguente:


[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample4.vb)]

Il `EditCommand` gestore dell'evento viene passato un oggetto di tipo `DataListCommandEventArgs` come il secondo parametro di input, che include un riferimento per il `DataListItem` è stato fatto clic sul pulsante la cui modifica (`e.Item`). Il gestore dell'evento prima imposta s DataList `EditItemIndex` per il `ItemIndex` dell'oggetto modificabile `DataListItem` e nuovamente i dati per il controllo DataList chiamando DataList s `DataBind()` (metodo).

Dopo aver aggiunto questo gestore eventi, visitare di nuovo la pagina in un browser. Facendo clic sul pulsante Modifica ora esegue il prodotto selezionato modificabile (vedere la figura 13).


[![Scegliendo la rende pulsante Modifica il prodotto modificabile](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image32.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image31.png)

**Figura 13**: facendo clic sul pulsante Modifica rende il prodotto modificabile ([fare clic per visualizzare l'immagine con dimensioni normali](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image33.png))


## <a name="step-6-saving-the-user-s-changes"></a>Passaggio 6: Il salvataggio delle modifiche utente s

La selezione prodotto modificato s Update o pulsanti Annulla non ha alcun effetto a questo punto; Per aggiungere questa funzionalità è necessario creare i gestori eventi per s DataList `UpdateCommand` e `CancelCommand` eventi. Iniziare creando la `CancelCommand` gestore eventi, che verrà eseguito quando si fa clic sul pulsante Annulla s di prodotto modificato e assegnato il compito di restituzione di DataList lo stato di pre-editing.

Per eseguire il rendering di tutti i relativi elementi nella modalità di sola lettura di DataList, è necessario:

1. Impostare il controllo DataList s [ `EditItemIndex` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) all'indice di un inesistenti `DataListItem` indice. `-1` è una scelta sicura, poiché il `DataListItem` gli indici iniziano da `0`.
2. Riassociare i dati di DataList. Non essendo `DataListItem` `ItemIndex` es corrispondono a DataList s `EditItemIndex`, DataList intero verrà eseguito il rendering in una modalità di sola lettura.

Questi passaggi possono essere eseguiti con il codice del gestore eventi seguente:


[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample5.vb)]

Grazie a questa aggiunta, fare clic su torna poi pulsante Annulla DataList al relativo stato di pre-modifica.

È l'ultimo gestore di evento è necessario completare il `UpdateCommand` gestore dell'evento. Questo gestore eventi deve:

1. Accedere a livello di codice il nome immesso dall'utente del prodotto e prezzo, nonché il prodotto modificato s `ProductID`.
2. Avviare il processo di aggiornamento chiamando appropriato `UpdateProduct` rapporto di overload nel `ProductsBLL` classe.
3. Impostare il controllo DataList s [ `EditItemIndex` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) all'indice di un inesistenti `DataListItem` indice. `-1` è una scelta sicura, poiché il `DataListItem` gli indici iniziano da `0`.
4. Riassociare i dati di DataList. Non essendo `DataListItem` `ItemIndex` es corrispondono a DataList s `EditItemIndex`, DataList intero verrà eseguito il rendering in una modalità di sola lettura.

I passaggi 1 e 2 sono responsabili per il salvataggio delle modifiche s; l'utente i passaggi 3 e 4 riportano DataList pre-modifica lo stato dopo le modifiche sono state salvate e sono identiche a quelli eseguiti nel `CancelCommand` gestore dell'evento.

Per ottenere il nome aggiornato del prodotto e prezzo, è necessario usare il `FindControl` metodo al riferimento a livello di programmazione Web nella casella di testo controlli all'interno di `EditItemTemplate`. È anche necessario ottenere il prodotto modificato s `ProductID` valore. Quando viene inizialmente associato ObjectDataSource a DataList, Visual Studio assegnata s DataList `DataKeyField` proprietà al valore della chiave primaria dall'origine dati (`ProductID`). Questo valore può quindi essere recuperato dalla DataList s `DataKeys` raccolta. Si consiglia di verificare che il `DataKeyField` viene effettivamente impostata su `ProductID`.

Il codice seguente implementa i quattro passaggi:


[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample6.vb)]

Il gestore dell'evento inizia con la lettura nel prodotto modificato s `ProductID` dal `DataKeys` raccolta. Le due caselle di testo in avanti il `EditItemTemplate` viene fatto riferimento e le relative `Text` proprietà archiviate nelle variabili locali, `productNameValue` e `unitPriceValue`. Usiamo il `Decimal.Parse()` metodo da cui leggere il valore di `UnitPrice` nella casella di testo in modo che, se il valore immesso è un simbolo di valuta, può comunque essere convertito correttamente in un `Decimal` valore.

> [!NOTE]
> I valori di `ProductName` e `UnitPrice` caselle di testo viene assegnati solo alle variabili productNameValue e unitPriceValue se le proprietà del testo nelle caselle di testo sono specificato un valore. In caso contrario, un valore pari `Nothing` viene usato per le variabili, che ha l'effetto di aggiornamento dei dati con un database `NULL` valore. Vale a dire, il nostro codice considera converte le stringhe database vuote `NULL` valori, ovvero il comportamento predefinito dell'interfaccia di modifica dei controlli GridView, DetailsView e FormView.


Dopo aver letto i valori, il `ProductsBLL` classe s `UpdateProduct` viene chiamato il metodo, passando il nome del prodotto s, prezzi, e `ProductID`. Il gestore eventi viene completata tramite la restituzione di DataList lo stato di pre-editing utilizzando la stessa logica esattamente come nel `CancelCommand` gestore dell'evento.

Con il `EditCommand`, `CancelCommand`, e `UpdateCommand` completare i gestori eventi, un visitatore può modificare il nome e il prezzo di un prodotto. Figure 14 a 16 Mostra questo flusso di lavoro di modifica in azione.


[![Quando si trovano in modalità di sola lettura prima visita la pagina, tutti i prodotti](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image35.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image34.png)

**Figura 14**: durante la prima visita la pagina, tutti i prodotti sono in modalità di sola lettura ([fare clic per visualizzare l'immagine con dimensioni normali](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image36.png))


[![Per aggiornare un prodotto s nome o il prezzo, fare clic sul pulsante Modifica](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image38.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image37.png)

**Figura 15**: per aggiornare un nome di prodotto o prezzo, fare clic sul pulsante Modifica ([fare clic per visualizzare l'immagine con dimensioni normali](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image39.png))


[![Dopo aver modificato il valore, fare clic su Aggiorna per tornare alla modalità di sola lettura](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image41.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image40.png)

**Figura 16**: dopo la modifica del valore, fare clic su Aggiorna per tornare alla modalità di sola lettura ([fare clic per visualizzare l'immagine con dimensioni normali](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image42.png))


## <a name="step-7-adding-delete-capabilities"></a>Passaggio 7: Aggiunta di funzionalità di eliminazione

I passaggi per l'aggiunta di funzionalità di eliminazione per un controllo DataList sono simili a quelle per l'aggiunta di funzionalità di modifica. In breve, è necessario aggiungere un pulsante Elimina per il `ItemTemplate` che, quando si fa clic:

1. Letture all'interno del prodotto corrispondente 1!s `ProductID` tramite il `DataKeys` raccolta.
2. Esegue l'eliminazione chiamando il `ProductsBLL` classe s `DeleteProduct` (metodo).
3. Riassocia i dati di DataList.

Let s mediante l'aggiunta di un pulsante Elimina per avviare il `ItemTemplate`.

Quando si fa clic, un pulsante di cui `CommandName` è modifica, aggiornamento o Annulla genera DataList s `ItemCommand` evento insieme a un evento aggiuntivo (ad esempio, quando si usa modifica il `EditCommand` l'evento viene generato anche). Analogamente, qualsiasi pulsante, LinkButton o ImageButton in DataList cui `CommandName` viene impostata per eliminare le cause di `DeleteCommand` dell'evento da generare (insieme a `ItemCommand`).

Aggiungere un pulsante di eliminazione accanto al pulsante di modifica nel `ItemTemplate`, l'impostazione relativa `CommandName` proprietà da eliminare. Dopo l'aggiunta di questo controllo pulsante la il DataList s `ItemTemplate` sintassi dichiarativa sarà simile al seguente:


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample7.aspx)]

Successivamente, creare un gestore eventi per il controllo DataList s `DeleteCommand` eventi, usando il codice seguente:


[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample8.vb)]

Facendo clic sul pulsante Elimina causa un postback e viene attivato il controllo DataList s `DeleteCommand` evento. Nel gestore eventi, il prodotto selezionato 1!s `ProductID` valore sono accessibili dal `DataKeys` raccolta. Successivamente, il prodotto viene eliminato chiamando il `ProductsBLL` classe s `DeleteProduct` (metodo).

Dopo l'eliminazione del prodotto, è s importante che abbiamo riassociare i dati di DataList (`DataList1.DataBind()`), in caso contrario, il controllo DataList Continuerai a visualizzare il prodotto appena eliminato.

## <a name="summary"></a>Riepilogo

Anche se DataList non ha il punto e fare clic su Modifica ed eliminazione supporto godono di GridView, con un breve frammento di codice può essere migliorata per includere queste funzionalità. In questa esercitazione è stato illustrato come creare un elenco di due colonne di prodotti che è stato possibile eliminare e il cui nome e il prezzo può essere modificati. Aggiunta, modifica ed eliminazione di supporto è una questione di inclusi controlli appropriati Web il `ItemTemplate` e `EditItemTemplate`, la creazione di gestori di eventi corrispondenti, leggendo i valori di chiave primari e immesso dall'utente e l'interazione con l'azienda Livello per la logica.

Anche se sono state aggiunte modifiche di base e l'eliminazione di funzionalità per il controllo DataList, manca funzionalità più avanzate. Ad esempio, non è presente alcuna convalida del campo di input - se un utente immette un prezzo di troppi costosa, verrà generata l'eccezione da `Decimal.Parse` quando si prova a convertire troppi costoso in una `Decimal`. Analogamente, se si è verificato un problema nell'aggiornamento i dati nella logica di Business o livelli di accesso ai dati l'utente visualizzerà la schermata di errore standard. Senza una sorta di conferma del pulsante di eliminazione, l'eliminazione accidentale di un prodotto è probabilmente tutti di troppo.

In futuro esercitazioni si vedrà come migliorare l'utente modifica esperienza.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. I revisori per questa esercitazione sono state Zack Jones, Ken Pespisa e Randy Schmidt. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](customizing-the-datalist-s-editing-interface-cs.md)
> [Successivo](performing-batch-updates-vb.md)
