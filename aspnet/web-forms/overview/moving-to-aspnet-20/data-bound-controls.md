---
uid: web-forms/overview/moving-to-aspnet-20/data-bound-controls
title: Controlli con associazione a dati | Documenti Microsoft
author: microsoft
description: La maggior parte delle applicazioni ASP.NET si basano su un certo grado di presentazione dei dati da un'origine dati back-end. Controlli con associazione a dati sono stati una parte fondamentale di interazione w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 0e23ff32-646d-43f3-8bec-6b2313d3abd6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/data-bound-controls
msc.type: authoredcontent
ms.openlocfilehash: 3ebb0f9a7a2f071b7bf7aa3855920f1a5784a61f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="data-bound-controls"></a>Controlli con associazione a dati
====================
da [Microsoft](https://github.com/microsoft)

> La maggior parte delle applicazioni ASP.NET si basano su un certo grado di presentazione dei dati da un'origine dati back-end. Controlli con associazione a dati sono stati una parte fondamentale dell'interazione con i dati in applicazioni Web dinamiche. ASP.NET 2.0 vengono introdotti alcuni miglioramenti sostanziali in termini di controlli con associazione a dati, ad esempio, una nuova classe BaseDataBoundControl e sintassi dichiarativa.


La maggior parte delle applicazioni ASP.NET si basano su un certo grado di presentazione dei dati da un'origine dati back-end. Controlli con associazione a dati sono stati una parte fondamentale dell'interazione con i dati in applicazioni Web dinamiche. ASP.NET 2.0 vengono introdotti alcuni miglioramenti sostanziali in termini di controlli con associazione a dati, ad esempio, una nuova classe BaseDataBoundControl e sintassi dichiarativa.

Il BaseDataBoundControl funge da classe base per la classe DataBoundControl e la classe HierarchicalDataBoundControl. In questo modulo, verranno illustrate le seguenti classi che derivano da DataBoundControl:

- AdRotator
- Controlli elenco
- GridView
- Controllo FormView
- Controllo DetailsView.

Verranno inoltre illustrate le seguenti classi che derivano dalla classe HierarchicalDataBoundControl:

- TreeView
- Menu
- SiteMapPath

## <a name="databoundcontrol-class"></a>Classe di DataBoundControl

La classe DataBoundControl è una classe astratta (contrassegnata MustInherit in VB) utilizzata per interagire con tabulari o dati di tipo elenco. I controlli seguenti sono alcuni dei controlli che derivano da DataBoundControl.

## <a name="adrotator"></a>AdRotator

Il controllo AdRotator consente di visualizzare l'intestazione grafica in una pagina Web che è collegata a un URL specifico. Il grafico visualizzato viene ruotato utilizzando le proprietà per il controllo. La frequenza di visualizzazione di Active Directory specifico in una pagina può essere configurata utilizzando il **impressioni** proprietà e gli annunci possono essere filtrati tramite parole chiave.

Controlli AdRotator utilizzano un file XML o una tabella in un database per i dati. Per configurare il controllo AdRotator, gli attributi seguenti vengono utilizzati in file XML.

### <a name="imageurl"></a>ImageUrl
L'URL di un'immagine da visualizzare per Active Directory.

### <a name="navigateurl"></a>NavigateUrl
URL a cui l'utente è necessario adottare quando si fa clic su Active Directory. Deve essere codificato in URL.

### <a name="alternatetext"></a>AlternateText
Testo alternativo che viene visualizzato in una descrizione comando e letto. Consente di visualizzare anche quando l'immagine specificata dalla proprietà ImageUrl non è disponibile.

### <a name="keyword"></a>Parola chiave
Definisce una parola chiave che può essere usata quando si utilizza il filtro (parola chiave). Se specificato, verranno visualizzati solo tali annunci con una parola chiave corrispondenti al filtro (parola chiave).

### <a name="impressions"></a>Impressioni
Numero di ponderazione che determina la frequenza con cui ad un particolare è probabile che venga visualizzato. È relativo l'impressione di altri annunci nello stesso file. Il valore massimo di impressioni collettivi per tutti gli annunci in un file XML è superiore a 2.048.000.000 1.

### <a name="height"></a>Altezza
L'altezza dell'annuncio in pixel.

### <a name="width"></a>Larghezza
La larghezza dell'annuncio in pixel.


> [!NOTE]
> Eseguire l'override gli attributi di altezza e larghezza l'altezza e larghezza del controllo AdRotator stesso.


Un tipico file XML potrebbe essere simile al seguente:

[!code-xml[Main](data-bound-controls/samples/sample1.xml)]

Nell'esempio precedente, Active Directory per Contoso è due volte come potrebbe apparire come Active Directory per il sito Web ASP.NET a causa del valore dell'attributo impressioni.

Per visualizzare gli annunci dal file XML illustrato in precedenza, aggiungere un controllo AdRotator a una pagina e impostare il **AdvertisementFile** proprietà in modo da puntare al file XML, come illustrato di seguito:

[!code-aspx[Main](data-bound-controls/samples/sample2.aspx)]

Se si sceglie di utilizzare una tabella di database come origine dati per il controllo AdRotator, è necessario innanzitutto configurare un database utilizzando lo schema seguente:

| **Nome colonna** | **Tipo di dati** | **Descrizione** |
| --- | --- | --- |
| Id | int | Chiave primaria. Questa colonna può avere qualsiasi nome. |
| ImageUrl | nvarchar (*lunghezza*) | L'URL relativo o assoluto dell'immagine da visualizzare per l'annuncio. |
| NavigateUrl | nvarchar (*lunghezza*) | L'URL di destinazione per l'annuncio. Se non si specifica un valore, Active Directory non è un collegamento ipertestuale. |
| AlternateText | nvarchar (*lunghezza*) | Il testo visualizzato se l'immagine non viene trovato. In alcuni browser, il testo viene visualizzato come descrizione comando. Testo alternativo viene inoltre utilizzato per l'accessibilità in modo che gli utenti che non è possibile vedere il grafico è possono ascoltare la relativa descrizione leggere a voce alta. |
| Parola chiave | nvarchar (*lunghezza*) | Una categoria per Active Directory in cui è possibile filtrare la pagina. |
| Impressioni | int(4) | Numero che indica la probabilità che la frequenza con cui verrà visualizzato l'annuncio. Il più elevato il numero, più di frequente Active Directory verrà visualizzata. Il totale di tutti i valori di impressioni nel file XML non può superare superiore a 2.048.000.000-1. |
| Larghezza | int(4) | La larghezza in pixel dell'immagine. |
| Altezza | int(4) | L'altezza in pixel dell'immagine. |

Nei casi in cui si dispone già di un database con uno schema diverso, è possibile utilizzare il **AlternateTextField**, **ImageUrlField**, e **NavigateUrlField** proprietà per eseguire il mapping di AdRotator gli attributi del database esistente. Per visualizzare i dati dal database nel controllo AdRotator, aggiungere un controllo origine dati, configurare la stringa di connessione per il controllo origine dati in modo che punti al database e impostare il controllo di AdRotator **DataSourceID** proprietà per l'ID del controllo origine dati. Nei casi in cui è necessario configurare a livello di programmazione AdRotator annunci, utilizzare l'evento AdCreated. L'evento AdCreated accetta due parametri. un oggetto e l'altro è un'istanza di AdCreatedEventArgs. Il AdCreatedEventArgs è un riferimento ad Active Directory che viene creato.

Frammento di codice seguente imposta la proprietà ImageUrl, NavigateUrl e AlternateText a livello di codice per un annuncio:

[!code-csharp[Main](data-bound-controls/samples/sample3.cs)]

## <a name="list-controls"></a>Controlli elenco

Controlli elenco sono inclusi la casella di riepilogo, DropDownList, CheckBoxList, RadioButtonList e BulletedList. Ognuno di questi controlli può essere dati associati a un'origine dati. Essi usare un campo nell'origine dati come testo da visualizzare e, facoltativamente, utilizzare un secondo campo come valore di un elemento. È possibile anche aggiungere elementi in modo statico in fase di progettazione, ed è possibile combinare elementi statici e dinamici elementi aggiunti da un'origine dati.

Ai dati di associare un elenco di controllo, aggiungere un controllo origine dati. Specificare un comando SELECT per il controllo origine dati e quindi impostare la proprietà DataSourceID del controllo elenco per l'ID del controllo origine dati. Utilizzare il **DataTextField** e **DataValueField** proprietà per definire il testo visualizzato e il valore per il controllo. Inoltre, è possibile utilizzare il **DataTextFormatString** proprietà per controllare l'aspetto del testo visualizzato come segue:

| **Espressione** | **Descrizione** |
| --- | --- |
| Price: {0:C} | Per i dati numerici o decimali. Visualizza il valore letterale "Price:" seguita da numeri nel formato di valuta. Il formato della valuta dipende dalle impostazioni cultura specificate nell'attributo delle impostazioni cultura nel **pagina** direttiva o nel file Web. config. |
| {0:D4} | Per i dati integer. Non può essere utilizzato con numeri decimali. Numeri interi vengono visualizzati in un campo con degli zero di quattro caratteri. |
| {0:N2}% | Per i dati numerici. Visualizza il numero con 2 cifre decimali precisione seguito dal valore letterale "%". |
| {0:000.0} | Per i dati numerici o decimali. I numeri vengono arrotondati a una cifra decimale. I numeri composti da meno di tre cifre sono riempiti con degli zero. |
| {0:D} | Per i dati di data/ora. Formato di data estesa ("giovedì, agosto 06, 1996") consente di visualizzare. Il formato della data dipende dalle impostazioni cultura della pagina o del file Web.config. |
| {0:d} | Per i dati di data/ora. Consente di visualizzare la data breve ("31/12/99") di formato. |
| {0:yy-MM-dd} | Per i dati di data/ora. Visualizza la data in formato anno-mese-giorno numerico (96-08-06) |

## <a name="gridview"></a>GridView

Il controllo GridView consente la visualizzazione di dati in formato tabulare e la modifica usando un approccio dichiarativo ed è il successore al controllo DataGrid. Le funzionalità seguenti sono disponibili nel controllo GridView.

- Associazione a dati i controlli di origine, ad esempio SqlDataSource.
- Funzionalità di ordinamento incorporate.
- Aggiornamento ed eliminazione incorporate le funzionalità.
- Funzionalità di paging incorporate.
- Funzionalità di selezione di riga predefinito.
- L'accesso programmatico al modello a oggetti GridView per impostare dinamicamente le proprietà, la gestione degli eventi e così via.
- Più campi chiave.
- Più campi di dati per le colonne di un collegamento ipertestuale.
- Aspetto personalizzabile mediante temi e stili.

**Campi delle colonne**

Ogni colonna nel controllo GridView è rappresentata da un oggetto DataControlField. Per impostazione predefinita, la proprietà AutoGenerateColumns è impostata su **true**, che consente di creare un oggetto AutoGeneratedField per ogni campo nell'origine dati. Viene quindi eseguito il rendering di ogni campo come colonna nel controllo GridView nell'ordine in cui ogni campo viene visualizzato nell'origine dati. È possibile controllare manualmente i campi colonna da visualizzare nel **GridView** controllo impostando il **AutoGenerateColumns** proprietà **false** e quindi definire la propria raccolta di campi di colonna. Tipi di colonna diversi campi determinano il comportamento delle colonne nel controllo.

Nella tabella seguente elenca i tipi di campo colonna diversa che possono essere utilizzati.

| **Tipo di campo colonna** | **Descrizione** |
| --- | --- |
| BoundField | Visualizza il valore di un campo in un'origine dati. Questo è il tipo di colonna predefinito del controllo GridView. |
| ButtonField | Visualizza un pulsante di comando per ogni elemento nel controllo GridView. Ciò consente di creare una colonna di pulsanti personalizzati, ad esempio l'aggiunta o il pulsante Rimuovi. |
| CheckBoxField | Visualizza una casella di controllo per ogni elemento nel controllo GridView. Questo tipo di campo colonna viene comunemente utilizzato per visualizzare i campi con un valore booleano. |
| CommandField | Visualizza i pulsanti di comando per eseguire la selezione, modifica o eliminazione di operazioni predefiniti. |
| HyperLinkField | Visualizza il valore di un campo in un'origine dati come collegamento ipertestuale. Questo tipo di campo colonna consente di associare un secondo campo all'URL del collegamento ipertestuale. |
| ImageField | Visualizza un'immagine per ogni elemento nel controllo GridView. |
| TemplateField | Visualizza il contenuto definito dall'utente per ogni elemento nel controllo GridView in base a un modello specificato. Questo tipo di campo colonna consente di creare un campo colonna personalizzato. |

Per definire una raccolta di campi di colonna in modo dichiarativo, aggiungere prima di apertura e chiusura  **&lt;colonne&gt;**  tag tra i tag di apertura e chiusura del controllo GridView. Successivamente, elencare i campi di colonna che si desidera includere tra l'apertura e chiusura  **&lt;colonne&gt;**  tag. Le colonne specificate vengono aggiunte alla raccolta di colonne nell'ordine elencato. Il **colonne** collection archivia tutti i campi nel controllo e consente di gestire a livello di codice i campi colonna nel controllo GridView, la colonna.

In combinazione con i campi colonna generati automaticamente, è possono visualizzare i campi colonna dichiarati in modo esplicito. Quando vengono utilizzati i valori dei campi colonna dichiarati in modo esplicito vengono visualizzati per primi, aggiungendo i campi colonna generata automaticamente.

## <a name="binding-to-data"></a>Associazione a dati

Il controllo GridView può essere associato a un controllo origine dati (ad esempio **SqlDataSource**, **ObjectDataSource**e così via), nonché di qualsiasi origine dati che implementa l'IEnumerable interfaccia (ad esempio System.Data.DataView, ArrayList e Hashtable). Per associare il controllo GridView per il tipo di origine di dati appropriato, utilizzare uno dei metodi seguenti:

- Per associare a un controllo origine dati, impostare la proprietà DataSourceID del controllo GridView per il valore ID del controllo origine dati. Il controllo GridView associato automaticamente al controllo dell'origine dati specificata e può sfruttare le funzionalità del controllo per eseguire l'ordinamento, l'aggiornamento, eliminazione e la funzionalità di paging. Questo è il metodo preferito per associare i dati.
- Per associare a un'origine dati che implementa l'interfaccia IEnumerable, a livello di codice impostare la proprietà DataSource del controllo GridView all'origine dati, quindi chiamare il metodo DataBind. Quando si utilizza questo metodo, il controllo GridView non fornisce predefinite di ordinamento, l'aggiornamento, eliminazione e funzionalità di paging. È necessario fornire questa funzionalità manualmente.

## <a name="data-operations"></a>Operazioni sui dati

Il controllo GridView fornisce numerose funzionalità incorporate che consentono all'utente di ordinare, aggiornare, eliminare, selezionare e scorrere gli elementi nel controllo. Quando il controllo GridView è associato a un controllo origine dati, il controllo GridView possa sfruttare le funzionalità del controllo dell'origine dati e fornire automatico ordinamento, aggiornamento ed eliminazione di funzionalità.

> [!NOTE]
> Il controllo GridView può fornire supporto per l'ordinamento, aggiornamento ed eliminazione con altri tipi di origini dati. Tuttavia, è necessario fornire un gestore eventi appropriato con l'implementazione di queste operazioni.


L'ordinamento consente all'utente di ordinare gli elementi nel controllo GridView rispetto a una colonna specifica facendo clic sull'intestazione della colonna. Per attivare l'ordinamento, impostare la proprietà AllowSorting **true**.

Le funzionalità automatiche di aggiornamento, eliminazione e selezione sono abilitate quando un pulsante in un **ButtonField** o **TemplateField** campo colonna, con un nome di comando "Modifica", "Delete" e "Select", viene fatto clic su rispettivamente. Il controllo GridView può aggiungere automaticamente una **CommandField** campo colonna con una modifica, eliminazione o Seleziona pulsante se la proprietà AutoGenerateEditButton, AutoGenerateDeleteButton o AutoGenerateSelectButton è impostata su **true**, rispettivamente.

> [!NOTE]
> Inserimento di record nell'origine dati non è supportato direttamente dal controllo GridView. Tuttavia, è possibile inserire record utilizzando il controllo GridView in combinazione con il controllo DetailsView o controllo FormView.


Invece di visualizzare tutti i record nell'origine dati nello stesso momento, il controllo GridView può suddividere automaticamente i record in più pagine. Per attivare il paging, impostare la proprietà AllowPaging su **true**.

## <a name="customizing-the-user-interface"></a>Personalizzazione dell'interfaccia utente

È possibile personalizzare l'aspetto del controllo GridView impostando le proprietà di stile per le diverse parti del controllo. Nella tabella seguente sono elencate le proprietà di stile diverso.

| **Proprietà di stile** | **Descrizione** |
| --- | --- |
| AlternatingRowStyle | Le impostazioni di stile per le righe di dati alternativi nel controllo GridView. Quando questa proprietà è impostata, vengono visualizzate le righe di dati alternando tra le impostazioni di RowStyle e **AlternatingRowStyle** impostazioni. |
| EditRowStyle | Le impostazioni di stile per la riga da modificare nel controllo GridView. |
| EmptyDataRowStyle | Le impostazioni di stile della riga di dati vuota visualizzata nel controllo GridView quando l'origine dati non contiene alcun record. |
| FooterStyle | Le impostazioni di stile per la riga di piè di pagina del controllo GridView. |
| HeaderStyle | Le impostazioni di stile per la riga di intestazione del controllo GridView. |
| PagerStyle | Le impostazioni di stile per la riga del pager del controllo GridView. |
| RowStyle | Le impostazioni di stile per le righe di dati nel controllo GridView. Quando il **AlternatingRowStyle** proprietà è impostata, le righe di dati vengono visualizzate alternando tra il **RowStyle** impostazioni e **AlternatingRowStyle** impostazioni. |
| SelectedRowStyle | Le impostazioni di stile per la riga selezionata nel controllo GridView. |

È anche possibile mostrare o nascondere parti diverse del controllo. Nella tabella seguente sono elencate le proprietà che controllano quali parti vengono visualizzate o nascoste.

| **Property** | **Descrizione** |
| --- | --- |
| ShowFooter | Mostra o nasconde la sezione di piè di pagina del controllo GridView. |
| ShowHeader | Mostra o nasconde la sezione di intestazione del controllo GridView. |

### <a name="events"></a>Eventi

Il controllo GridView fornisce diversi eventi che è possibile programmare. In questo modo è possibile eseguire una routine personalizzata ogni volta che si verifica un evento. Nella tabella seguente elenca gli eventi supportati dal controllo GridView.

| **Event** | **Descrizione** |
| --- | --- |
| PageIndexChanged | Si verifica quando viene fatto clic su uno dei pulsanti di spostamento, ma dopo che il controllo GridView gestisce l'operazione di spostamento. Questo evento viene comunemente utilizzato quando è necessario eseguire un'attività dopo che l'utente passa a una pagina diversa nel controllo. |
| PageIndexChanging | Si verifica quando viene fatto clic su uno dei pulsanti di spostamento, ma prima di GridView controllo gestisce l'operazione di spostamento. Questo evento viene spesso utilizzato per annullare l'operazione di spostamento. |
| RowCancelingEdit | Si verifica quando si fa clic sul pulsante di annullamento di una riga, ma prima che il controllo GridView esce dalla modalità di modifica. Questo evento viene spesso utilizzato per arrestare l'operazione di annullamento. |
| RowCommand | Si verifica quando viene premuto un pulsante nel controllo GridView. Questo evento viene spesso utilizzato per eseguire un'attività quando viene premuto un pulsante nel controllo. |
| RowCreated | Si verifica quando viene creata una nuova riga nel controllo GridView. Questo evento viene spesso utilizzato per modificare il contenuto di una riga quando viene creata la riga. |
| RowDataBound | Si verifica quando una riga di dati è associata a dati nel controllo GridView. Questo evento viene spesso utilizzato per modificare il contenuto di una riga quando la riga è associata a dati. |
| RowDeleted | Si verifica quando si fa clic sul pulsante Elimina una riga, ma dopo che il controllo GridView Elimina il record dall'origine dati. Questo evento viene spesso utilizzato per controllare i risultati dell'operazione di eliminazione. |
| RowDeleting | Si verifica quando si fa clic sul pulsante Elimina una riga, ma prima di GridView controllo Elimina il record dall'origine dati. Questo evento viene spesso utilizzato per annullare l'operazione di eliminazione. |
| RowEditing | Si verifica quando si fa clic sul pulsante di modifica di una riga, ma prima di GridView viene attivata la modalità di controllo. Questo evento viene spesso utilizzato per annullare l'operazione di modifica. |
| RowUpdated | Si verifica quando si fa clic sul pulsante Aggiorna una riga, ma dopo che il controllo GridView aggiorni la riga. Questo evento viene spesso utilizzato per controllare i risultati dell'operazione di aggiornamento. |
| RowUpdating | Si verifica quando si fa clic sul pulsante Aggiorna una riga, ma prima di GridView ha aggiornato la riga. Questo evento viene spesso utilizzato per annullare l'operazione di aggiornamento. |
| SelectedIndexChanged | Si verifica quando si fa clic sul pulsante Seleziona una riga, ma dopo che il controllo GridView gestisce l'operazione select. Questo evento viene spesso utilizzato per eseguire un'attività dopo aver selezionata una riga nel controllo. |
| SelectedIndexChanging | Si verifica quando si fa clic sul pulsante Seleziona una riga, ma prima di GridView controllo gestisce l'operazione select. Questo evento viene spesso utilizzato per annullare l'operazione di selezione. |
| Ordinamento | Si verifica quando si fa clic sul collegamento ipertestuale per ordinare una colonna, ma dopo che il controllo GridView gestisce l'operazione di ordinamento. Questo evento viene comunemente utilizzato per eseguire un'attività dopo che l'utente fa clic su un collegamento ipertestuale per ordinare una colonna. |
| Ordinamento | Si verifica quando si fa clic sul collegamento ipertestuale per ordinare una colonna, ma prima di GridView controllo gestisce l'operazione di ordinamento. Questo evento viene spesso utilizzato per annullare l'operazione di ordinamento o per eseguire una routine di ordinamento personalizzata. |

## <a name="formview"></a>Controllo FormView

Il controllo FormView è utilizzato per visualizzare un singolo record da un'origine dati. È simile al controllo DetailsView, ad eccezione del fatto che visualizza modelli definiti dall'utente anziché i campi riga. La creazione di modelli personalizzati consente maggiore flessibilità per la modalità di visualizzazione dei dati. Il controllo FormView supporta le funzionalità seguenti:

- Associazione a dati i controlli di origine, ad esempio SqlDataSource e ObjectDataSource.
- Funzionalità incorporate di inserimento.
- Aggiornamento ed eliminazione incorporate le funzionalità.
- Funzionalità di paging incorporate.
- L'accesso programmatico al modello a oggetti FormView per impostare dinamicamente le proprietà, la gestione degli eventi e così via.
- Aspetto personalizzabile tramite modelli definiti dall'utente, temi e stili.

## <a name="templates"></a>Modelli

Per il controllo FormView visualizzare il contenuto, è necessario creare modelli per le diverse parti del controllo. La maggior parte dei modelli sono facoltativi. Tuttavia, è necessario creare un modello per la modalità in cui è stato configurato il controllo. Ad esempio, un controllo FormView che supporta l'inserimento di record deve disporre di un modello di elemento di inserimento definito. La tabella seguente elenca i diversi modelli che è possibile creare.

| **Tipo di modello** | **Descrizione** |
| --- | --- |
| EditItemTemplate | Definisce il contenuto della riga di dati quando il controllo FormView è in modalità di modifica. Questo modello contiene in genere i controlli di input e i pulsanti di comando con cui l'utente può modificare un record esistente. |
| EmptyDataTemplate | Definisce il contenuto della riga di dati vuota visualizzata quando il controllo FormView è associato a un'origine dati che non contiene record. Questo modello contiene in genere contenuto per avvisare l'utente che l'origine dati non contiene alcun record. |
| FooterTemplate | Definisce il contenuto per la riga di piè di pagina. Questo modello contiene in genere qualsiasi contenuto aggiuntivo che si desidera visualizzare nella riga di piè di pagina. In alternativa, è possibile specificare semplicemente il testo da visualizzare nella riga di piè di pagina impostando la proprietà FooterText. |
| HeaderTemplate | Definisce il contenuto della riga di intestazione. Questo modello contiene in genere qualsiasi contenuto aggiuntivo che si desidera visualizzare nella riga di intestazione. In alternativa, è possibile specificare semplicemente il testo da visualizzare nella riga di intestazione impostando la proprietà HeaderText. |
| ItemTemplate | Definisce il contenuto della riga di dati quando il controllo FormView è in modalità di sola lettura. Questo modello contiene in genere contenuto per visualizzare i valori di un record esistente. |
| InsertItemTemplate | Definisce il contenuto della riga di dati quando il controllo FormView è in modalità di inserimento. Questo modello contiene in genere i controlli di input e i pulsanti di comando con cui l'utente può aggiungere un nuovo record. |
| PagerTemplate | Definisce il contenuto per la riga del pager visualizzato quando viene abilitata la funzionalità di paging (quando la proprietà di AllowPaging è impostata su **true**). In genere, questo modello contiene controlli con cui l'utente possa passare a un altro record. |

I controlli di input il modello di elemento di modifica e il modello di elemento di inserimento possono essere associati ai campi di un'origine dati utilizzando un'espressione di associazione bidirezionale. In questo modo il controllo FormView automaticamente estrarre i valori del controllo di input per un aggiornamento o l'operazione di inserimento. Espressioni di associazione bidirezionale consentono inoltre a controlli di input in un modello di elemento di modifica per visualizzare automaticamente i valori dei campi originale.

### <a name="binding-to-data"></a>Associazione a dati

Il controllo FormView può essere associato a un controllo origine dati (ad esempio **SqlDataSource**, AccessDataSource, **ObjectDataSource** e così via), o a qualsiasi origine dati che implementa il Interfaccia IEnumerable (ad esempio System.Data.DataView ArrayList e Hashtable). Per associare il controllo FormView al tipo di origine di dati appropriato, utilizzare uno dei metodi seguenti:

- Per associare a un controllo origine dati, impostare la proprietà DataSourceID del controllo FormView per il valore ID del controllo origine dati. Il controllo FormView associato automaticamente al controllo dell'origine dati specificata e può sfruttare le funzionalità del controllo per eseguire l'inserimento, aggiornamento, eliminazione e la funzionalità di paging. Questo è il metodo preferito per associare i dati.
- Per associare a un'origine dati che implementa il **IEnumerable** di interfaccia, a livello di codice impostare la proprietà DataSource del controllo FormView all'origine dati e quindi chiamare il metodo DataBind. Quando si utilizza questo metodo, il controllo FormView non fornisce incorporate di inserimento, aggiornamento, eliminazione e la funzionalità di paging. È necessario fornire questa funzionalità utilizzando l'evento appropriato.

## <a name="data-operations"></a>Operazioni sui dati

Il controllo FormView fornisce numerose funzionalità incorporate che consentono all'utente di aggiornare, eliminare, inserire e spostarsi tra gli elementi nel controllo. Quando il controllo FormView è associato a un controllo origine dati, il controllo FormView possa sfruttare le funzionalità del controllo dell'origine dati e fornire automatico di aggiornamento, eliminazione, inserimento e la funzionalità di paging. Il controllo FormView può fornire supporto per update, delete, insert e le operazioni di paging con altri tipi di origini dati. Tuttavia, è necessario fornire un gestore eventi appropriato con l'implementazione di queste operazioni.

Poiché il controllo FormView utilizza i modelli, non fornisce un modo per generare automaticamente i pulsanti di comando per eseguire l'aggiornamento, eliminazione o operazioni di inserimento. Nel modello appropriato, è necessario includere manualmente questi pulsanti di comando. Il controllo FormView riconosce alcuni pulsanti con i relativi **CommandName** impostate sui valori specifici. Nella tabella seguente sono elencati i pulsanti di comando riconosciuto dal controllo FormView.

| **Pulsante** | **Valore CommandName** | **Descrizione** |
| --- | --- | --- |
| Annulla | "Annulla" | Utilizzato in operazioni di inserimento o per annullare l'operazione e ignorare i valori immessi dall'utente. Il controllo FormView quindi torna alla modalità specificata dalla proprietà DefaultMode. |
| Eliminare | "Delete" | Utilizzato nelle operazioni di eliminazione per eliminare il record visualizzato dall'origine dati. Genera gli eventi ItemDeleting e ItemDeleted. |
| Edit | "Edit" | Utilizzato nelle operazioni di aggiornamento per inserire il controllo FormView in modalità di modifica. Il contenuto specificato nella **EditItemTemplate** proprietà viene visualizzata per la riga di dati. |
| INS | "Insert" | Utilizzato nelle operazioni di inserimento per tentare di inserire un nuovo record nell'origine dati utilizzando i valori forniti dall'utente. Genera gli eventi ItemInserting e ItemInserted. |
| Nuovo | "Nuovo" | Utilizzato nelle operazioni di inserimento per inserire il controllo FormView in modalità di inserimento. Il contenuto specificato nella **InsertItemTemplate** proprietà viene visualizzata per la riga di dati. |
| Pagina | "Pagina" | Usato in operazioni di paging per rappresentare un pulsante nella riga del pager che esegue il paging. Per specificare l'operazione di spostamento, impostare il **CommandArgument** proprietà del pulsante su "Avanti", "Precedente", "First", "Ultimo" o l'indice della pagina per cui si desidera spostarsi. Genera gli eventi PageIndexChanging e PageIndexChanged. |
| Aggiorna | "Aggiornamento" | Utilizzato nelle operazioni di aggiornamento per tentare di aggiornare il record visualizzato nell'origine dati con i valori specificati dall'utente. Genera gli eventi ItemUpdating e ItemUpdated. |

A differenza dell'eliminazione pulsante (che consente di eliminare il record visualizzato immediatamente), quando si fa clic sul pulsante Modifica o nuovo, FormView controllo entra modifica o modalità di inserimento rispettivamente. In modalità di modifica, il contenuto di **EditItemTemplate** proprietà viene visualizzata per l'elemento di dati corrente. In genere, il modello di elemento di modifica è definito in modo che il pulsante Modifica viene sostituito con un aggiornamento e un pulsante Annulla. I controlli di input appropriati per il tipo di dati (ad esempio una casella di testo o un controllo casella di controllo) vengono visualizzati anche in genere con un valore di campo per l'utente da modificare. Fare clic sul pulsante aggiornamento aggiorna il record nell'origine dati, mentre facendo clic sul pulsante di annullamento Annulla tutte le modifiche.

Analogamente, il contenuto di **InsertItemTemplate** proprietà viene visualizzata per l'elemento di dati quando il controllo è in modalità di inserimento. Il modello di elemento di inserimento è in genere definito in modo che il pulsante nuovo viene sostituito con un'operazione di inserimento e un pulsante di annullamento e controlli di input vuoti vengono visualizzati all'utente di immettere i valori per il nuovo record. Fare clic sul pulsante Insert inserisce il record nell'origine dati, mentre facendo clic sul pulsante di annullamento Annulla tutte le modifiche.

Il controllo FormView fornisce una funzionalità di spostamento, che consente all'utente di spostarsi tra gli altri record nell'origine dati. Quando abilitata, viene visualizzata una riga di spostamento del controllo FormView che contiene i controlli di navigazione della pagina. Per abilitare il paging, impostare il **AllowPaging** proprietà **true**. È possibile personalizzare la riga del pager impostando le proprietà degli oggetti contenuti nel PagerStyle e la proprietà PagerSettings. Anziché utilizzare la riga del pager predefinite dell'interfaccia utente, è possibile creare la propria interfaccia utente utilizzando il **PagerTemplate** proprietà.

## <a name="customizing-the-user-interface"></a>Personalizzazione dell'interfaccia utente

È possibile personalizzare l'aspetto del controllo FormView impostando le proprietà di stile per le diverse parti del controllo. Nella tabella seguente sono elencate le proprietà di stile diverso.

| **Proprietà di stile** | **Descrizione** |
| --- | --- |
| EditRowStyle | Le impostazioni di stile della riga di dati quando il controllo FormView è in modalità di modifica. |
| EmptyDataRowStyle | Le impostazioni di stile della riga di dati vuota visualizzata nel controllo FormView quando l'origine dati non contiene alcun record. |
| FooterStyle | Le impostazioni di stile per la riga di piè di pagina del controllo FormView. |
| HeaderStyle | Le impostazioni di stile per la riga di intestazione del controllo FormView. |
| InsertRowStyle | Le impostazioni di stile della riga di dati quando il controllo FormView è in modalità di inserimento. |
| PagerStyle | Le impostazioni di stile per la riga del pager visualizzati nel controllo FormView quando è abilitata la funzionalità di paging. |
| RowStyle | Le impostazioni di stile della riga di dati quando il controllo FormView è in modalità di sola lettura. |

## <a name="events"></a>Eventi

Il controllo FormView fornisce diversi eventi che è possibile programmare. In questo modo è possibile eseguire una routine personalizzata ogni volta che si verifica un evento. Nella tabella seguente elenca gli eventi supportati dal controllo FormView.

| **Event** | **Descrizione** |
| --- | --- |
| ItemCommand | Si verifica quando viene premuto un pulsante all'interno di un controllo FormView. Questo evento viene spesso utilizzato per eseguire un'attività quando viene premuto un pulsante nel controllo. |
| ItemCreated | Si verifica dopo che tutti gli oggetti FormViewRow vengono creati nel controllo FormView. Questo evento viene spesso utilizzato per modificare i valori di un record prima che venga visualizzato. |
| ItemDeleted | Si verifica quando un pulsante Elimina (un pulsante con il relativo **CommandName** proprietà è impostata su "Elimina") si fa clic, ma dopo che il controllo FormView Elimina il record dall'origine dati. Questo evento viene spesso utilizzato per controllare i risultati dell'operazione di eliminazione. |
| ItemDeleting | Si verifica quando si fa clic sul pulsante Elimina, ma prima di FormView controllo Elimina il record dall'origine dati. Questo evento viene spesso utilizzato per annullare l'operazione di eliminazione. |
| ItemInserted | Si verifica quando un pulsante Inserisci (un pulsante con il relativo **CommandName** proprietà impostata su "Insert") viene selezionato, ma dopo che il controllo FormView inserisce il record. Questo evento viene spesso utilizzato per controllare i risultati dell'operazione di inserimento. |
| ItemInserting | Si verifica quando viene fatto clic su un pulsante Inserisci, ma prima di FormView ha inserito il record. Questo evento viene spesso utilizzato per annullare l'operazione di inserimento. |
| ItemUpdated | Si verifica quando un pulsante Aggiorna (un pulsante con il relativo **CommandName** proprietà è impostata su "Update") si fa clic, ma dopo che il controllo FormView aggiorni la riga. Questo evento viene spesso utilizzato per controllare i risultati dell'operazione di aggiornamento. |
| ItemUpdating | Si verifica quando un pulsante di aggiornamento, ma prima di FormView ha aggiornato il record. Questo evento viene spesso utilizzato per annullare l'operazione di aggiornamento. |
| ModeChanged | Si verifica dopo che il controllo FormView cambia modalità (in modalità di modifica, inserimento o sola lettura). Questo evento viene spesso utilizzato per eseguire un'attività quando il controllo FormView cambia modalità. |
| ModeChanging | Si verifica prima che il controllo FormView cambia modalità (in modalità di modifica, inserimento o sola lettura). Questo evento viene spesso utilizzato per annullare una modifica di modalità. |
| PageIndexChanged | Si verifica quando viene fatto clic su uno dei pulsanti di spostamento, ma dopo che il controllo FormView gestisce l'operazione di spostamento. Questo evento viene comunemente utilizzato quando è necessario eseguire un'attività dopo che l'utente passa a un altro record nel controllo. |
| PageIndexChanging | Si verifica quando viene fatto clic su uno dei pulsanti di spostamento, ma prima che il controllo FormView controllo gestisca l'operazione di spostamento. Questo evento viene spesso utilizzato per annullare l'operazione di spostamento. |

## <a name="detailsview"></a>Controllo DetailsView.

Il controllo DetailsView viene utilizzato per visualizzare un singolo record da un'origine dati in una tabella, in cui ogni campo del record viene visualizzato in una riga della tabella. E può essere utilizzato in combinazione con un controllo GridView per scenari master-details. Il controllo DetailsView supporta le funzionalità seguenti:

- Associazione a dati i controlli di origine, ad esempio SqlDataSource.
- Funzionalità incorporate di inserimento.
- Aggiornamento ed eliminazione incorporate le funzionalità.
- Funzionalità di paging incorporate.
- L'accesso programmatico al modello a oggetti DetailsView per impostare dinamicamente le proprietà, la gestione degli eventi e così via.
- Aspetto personalizzabile mediante temi e stili.

## <a name="row-fields"></a>Campi riga

Ogni riga di dati nel controllo DetailsView viene creato mediante la dichiarazione di un controllo del campo. Tipi di campi riga diversi determinano il comportamento delle righe nel controllo. Controlli di campo derivano da DataControlField. Nella tabella seguente sono elencati i tipi di campo di riga diversi che possono essere utilizzati.

| **Tipo di campo colonna** | **Descrizione** |
| --- | --- |
| BoundField | Visualizza il valore di un campo in un'origine dati come testo. |
| ButtonField | Visualizza un pulsante di comando nel controllo DetailsView. Ciò consente di visualizzare una riga con un controllo pulsante personalizzato, ad esempio un pulsante Aggiungi o Rimuovi. |
| CheckBoxField | Visualizza una casella di controllo nel controllo DetailsView. Questo tipo di campo riga viene comunemente utilizzato per visualizzare i campi con un valore booleano. |
| CommandField | Visualizza comandi incorporata pulsanti per l'esecuzione di modifica, inserimento o eliminazione di operazioni nel controllo DetailsView. |
| HyperLinkField | Visualizza il valore di un campo in un'origine dati come collegamento ipertestuale. Questo tipo di campo riga consente di associare un secondo campo all'URL del collegamento ipertestuale. |
| ImageField | Visualizza un'immagine nel controllo DetailsView. |
| TemplateField | Visualizza il contenuto definito dall'utente per una riga nel controllo DetailsView in base a un modello specificato. Questo tipo di campo riga consente di creare un campo di riga personalizzato. |

Per impostazione predefinita, la proprietà AutoGenerateRows è impostata su **true**, che genera automaticamente un oggetto campo riga associato per ogni campo di tipo associabile nell'origine dati. Tipi associabili validi sono String, DateTime, Decimal, Guid e il set di tipi primitivi. Ogni campo viene quindi visualizzato in una riga come testo, nell'ordine in cui ogni campo viene visualizzato nell'origine dati.

Generare automaticamente le righe fornisce un modo rapido e semplice per visualizzare tutti i campi nel record. Tuttavia, per l'utilizzo del controllo DetailsView controllo funzionalità avanzate che è necessario dichiarare in modo esplicito i campi riga da includere nel controllo DetailsView. Per dichiarare i campi riga, impostare innanzitutto la **AutoGenerateRows** proprietà **false**. Successivamente, aggiungere di apertura e chiusura  **&lt;campi&gt;**  tag tra i tag di apertura e chiusura del controllo DetailsView. Infine, elencare i campi riga che si desidera includere tra l'apertura e chiusura  **&lt;campi&gt;**  tag. I campi riga specificati vengono aggiunti alla raccolta di campi nell'ordine elencato. Il **campi** raccolta consente di gestire a livello di codice i campi riga nel controllo DetailsView.

> [!NOTE]
> I campi non vengono aggiunti alla raccolta di campi riga generati automaticamente.


## <a name="binding-to-data"></a>Associazione a dati

Il controllo DetailsView può essere associato a un controllo origine dati, ad esempio **SqlDataSource** o AccessDataSource, o a qualsiasi origine dati che implementa l'interfaccia IEnumerable, ad esempio System.Data.DataView, ArrayList e Hashtable.

Per associare il controllo DetailsView al tipo di origine di dati appropriato, utilizzare uno dei metodi seguenti:

- Per associare a un controllo origine dati, impostare la proprietà DataSourceID del controllo DetailsView per il valore ID del controllo origine dati. Il controllo DetailsView associa automaticamente al controllo origine dati specificata. Questo è il metodo preferito per associare i dati.
- Per associare a un'origine dati che implementa il **IEnumerable** di interfaccia, a livello di codice impostare la proprietà DataSource del controllo DetailsView all'origine dati e quindi chiamare il metodo DataBind.

## <a name="security"></a>Sicurezza

Per visualizzare l'input dell'utente che può includere uno script client dannoso, è possibile utilizzare questo controllo. Verificare tutte le informazioni inviate da un client per uno script eseguibile, istruzioni SQL o altro codice prima di visualizzarlo nell'applicazione. ASP.NET fornisce una funzionalità di convalida richiesta di input per bloccare script e HTML nell'input dell'utente.

## <a name="data-operations"></a>Operazioni sui dati

Il controllo DetailsView fornisce funzionalità incorporate che consentono all'utente di aggiornare, eliminare, inserire e spostarsi tra gli elementi nel controllo. Quando il controllo DetailsView è associato a un controllo origine dati, il controllo DetailsView possa sfruttare le funzionalità del controllo dell'origine dati e fornire automatico di aggiornamento, eliminazione, inserimento e la funzionalità di paging.

Il controllo DetailsView può fornire supporto per update, delete, insert e le operazioni di paging con altri tipi di origini dati. Tuttavia, è necessario fornire l'implementazione per queste operazioni in un gestore eventi appropriato.

Il controllo DetailsView può aggiungere automaticamente una **CommandField** campo riga con un pulsante di modifica, eliminazione o nuovo impostando le proprietà AutoGenerateEditButton, AutoGenerateDeleteButton o AutoGenerateInsertButton per **true**, rispettivamente. A differenza dell'eliminazione pulsante (che consente di eliminare il record selezionato immediatamente), quando si fa clic sul pulsante Modifica o nuovo, DetailsView controllo entra modifica modalità o di inserimento, rispettivamente. In modalità di modifica, il pulsante Modifica viene sostituito con un aggiornamento e un pulsante Annulla. I controlli di input appropriati per il tipo di dati (ad esempio una casella di testo o un controllo casella di controllo) vengono visualizzati con un valore di campo per l'utente da modificare. Fare clic sul pulsante aggiornamento aggiorna il record nell'origine dati, mentre facendo clic sul pulsante di annullamento Annulla tutte le modifiche. Analogamente, in modalità di inserimento, pulsante nuovo viene sostituito con un'operazione di inserimento e un pulsante di annullamento e controlli di input vuoti vengono visualizzati all'utente di immettere i valori per il nuovo record.

Il controllo DetailsView fornisce una funzionalità di spostamento, che consente all'utente di spostarsi tra gli altri record nell'origine dati. Quando abilitata, i controlli di navigazione pagina vengono visualizzati in una riga del pager. Per attivare il paging, impostare la proprietà AllowPaging su **true**. La riga del pager può essere personalizzata utilizzando le proprietà PagerStyle e PagerSettings.

## <a name="customizing-the-user-interface"></a>Personalizzazione dell'interfaccia utente

È possibile personalizzare l'aspetto del controllo DetailsView impostando le proprietà di stile per le diverse parti del controllo. Nella tabella seguente sono elencate le proprietà di stile diverso.

| **Proprietà di stile** | **Descrizione** |
| --- | --- |
| AlternatingRowStyle | Le impostazioni di stile per le righe di dati alternativi nel controllo DetailsView. Quando questa proprietà è impostata, vengono visualizzate le righe di dati alternando tra le impostazioni di RowStyle e **AlternatingRowStyle** impostazioni. |
| CommandRowStyle | Le impostazioni di stile per la riga che contiene i pulsanti di comando incorporata nel controllo DetailsView. |
| EditRowStyle | Le impostazioni di stile per le righe di dati quando il controllo DetailsView è in modalità di modifica. |
| EmptyDataRowStyle | Le impostazioni di stile della riga di dati vuota visualizzata nel controllo DetailsView quando l'origine dati non contiene alcun record. |
| FooterStyle | Le impostazioni di stile per la riga di piè di pagina del controllo DetailsView. |
| HeaderStyle | Le impostazioni di stile per la riga di intestazione del controllo DetailsView. |
| InsertRowStyle | Le impostazioni di stile per le righe di dati quando il controllo DetailsView è in modalità di inserimento. |
| PagerStyle | Le impostazioni di stile per la riga del pager del controllo DetailsView. |
| RowStyle | Le impostazioni di stile per le righe di dati nel controllo DetailsView. Quando il **AlternatingRowStyle** proprietà è impostata, le righe di dati vengono visualizzate alternando tra il **RowStyle** impostazioni e **AlternatingRowStyle** impostazioni. |
| FieldHeaderStyle | Le impostazioni di stile per la colonna di intestazione del controllo DetailsView. |

## <a name="events"></a>Eventi

Il controllo DetailsView fornisce diversi eventi che è possibile programmare. In questo modo è possibile eseguire una routine personalizzata ogni volta che si verifica un evento. Nella tabella seguente elenca gli eventi supportati dal controllo DetailsView. Il controllo DetailsView eredita anche questi eventi dalle relative classi base: l'associazione dati, con associazione a dati, Disposed, Init, Load, PreRender e rendering.

| **Event** | **Descrizione** |
| --- | --- |
| ItemCommand | Si verifica quando viene premuto un pulsante nel controllo DetailsView. |
| ItemCreated | Si verifica dopo che tutti gli oggetti DetailsViewRow vengono creati nel controllo DetailsView. Questo evento viene spesso utilizzato per modificare i valori di un record prima che venga visualizzato. |
| ItemDeleted | Si verifica quando si fa clic sul pulsante Elimina, ma dopo che il controllo DetailsView Elimina il record dall'origine dati. Questo evento viene spesso utilizzato per controllare i risultati dell'operazione di eliminazione. |
| ItemDeleting | Si verifica quando si fa clic sul pulsante Elimina, ma prima di DetailsView controllo Elimina il record dall'origine dati. Questo evento viene spesso utilizzato per annullare l'operazione di eliminazione. |
| ItemInserted | Si verifica quando viene fatto clic su un pulsante Inserisci, ma dopo che il controllo DetailsView inserisce il record. Questo evento viene spesso utilizzato per controllare i risultati dell'operazione di inserimento. |
| ItemInserting | Si verifica quando viene fatto clic su un pulsante Inserisci, ma prima di DetailsView ha inserito il record. Questo evento viene spesso utilizzato per annullare l'operazione di inserimento. |
| ItemUpdated | Si verifica quando un pulsante di aggiornamento, ma dopo che il controllo DetailsView aggiorni la riga. Questo evento viene spesso utilizzato per controllare i risultati dell'operazione di aggiornamento. |
| ItemUpdating | Si verifica quando un pulsante di aggiornamento, ma prima di DetailsView ha aggiornato il record. Questo evento viene spesso utilizzato per annullare l'operazione di aggiornamento. |
| ModeChanged | Si verifica dopo che il controllo DetailsView cambia modalità (modalità di modifica, inserimento o sola lettura). Questo evento viene spesso utilizzato per eseguire un'attività quando il controllo DetailsView cambia modalità. |
| ModeChanging | Si verifica prima che il controllo DetailsView cambia modalità (modalità di modifica, inserimento o sola lettura). Questo evento viene spesso utilizzato per annullare una modifica di modalità. |
| PageIndexChanged | Si verifica quando viene fatto clic su uno dei pulsanti di spostamento, ma dopo che il controllo DetailsView gestisce l'operazione di spostamento. Questo evento viene comunemente utilizzato quando è necessario eseguire un'attività dopo che l'utente passa a un altro record nel controllo. |
| PageIndexChanging | Si verifica quando viene fatto clic su uno dei pulsanti di spostamento, ma prima di DetailsView controllo gestisce l'operazione di spostamento. Questo evento viene spesso utilizzato per annullare l'operazione di spostamento. |

## <a name="the-menu-control"></a>Il controllo Menu

Il controllo Menu in ASP.NET 2.0 è progettato per essere un sistema di navigazione completa. Può essere associato a dati con facilità alle origini di dati gerarchici, ad esempio SiteMapDataSource.

Una struttura di controlli Menu può essere definita in modo dichiarativo o in modo dinamico ed è costituito da un unico nodo radice e qualsiasi numero di nodi secondari. Il codice seguente definisce in modo dichiarativo un menu per il controllo Menu.

[!code-aspx[Main](data-bound-controls/samples/sample4.aspx)]

Nell'esempio precedente, il nodo aspx è il nodo radice. Tutti gli altri nodi sono annidati all'interno del nodo radice a vari livelli.

Esistono due tipi di menu che è possibile eseguire il rendering del controllo Menu; i menu statici e un menu dinamico. Menu statici sono costituiti da voci di menu sono sempre visibili. Menu dinamici sono costituiti da voci di menu che sono visibili solo quando l'utente viene spostato su di essi con il mouse. I clienti possono confondere spesso statici menu con menu definiti in modo dichiarativo e menu dinamici con i menu con associazione a dati in fase di esecuzione. Infatti, non sono correlati al metodo di popolamento menu statici e dinamici. Le condizioni di *statico* e *dinamica* fare riferimento solo se è o meno il menu statico visualizzati per impostazione predefinita o solo visualizzato quando l'utente intraprende un'azione.

Il **StaticDisplayLevels** proprietà viene utilizzata per configurare il numero di livelli del menu è statici e pertanto visualizzato per impostazione predefinita. Nell'esempio precedente, l'impostazione di **StaticDisplayLevels** su un valore pari a 2 provocherebbe il menu da visualizzare in modo statico il nodo iniziale, il nodo di musica e il nodo di film. Tutti gli altri nodi verrebbero visualizzate in modo dinamico quando l'utente passa sopra il nodo padre.

Il **MaximumDynamicDisplayLevels** proprietà consente di configurare il numero massimo di livelli dinamici è in grado di visualizzare il menu. Menu dinamici a un livello superiore rispetto al valore specificato per il **MaximumDynamicDisplayLevels** proprietà vengono ignorate.

> [!NOTE]
> È quasi certo che potrebbero verificarsi situazioni in cui i menu non vengono visualizzati per il rendering a causa della proprietà MaximumDynamicDisplayLevels. In questi casi, verificare che la proprietà è impostata sufficiente per consentire la visualizzazione del menu di clienti.


## <a name="data-binding-the-menu-control"></a>Data Binding del controllo Menu

Il controllo Menu può essere associato a qualsiasi origine dati gerarchici, ad esempio SiteMapDataSource o XMLDataSource. SiteMapDataSource è il metodo più utilizzato per il data binding a un controllo Menu perché alimenta i file Web. sitemap e lo schema fornisce un'API nota per il controllo Menu. Di seguito è riportato un semplice file sitemap.

[!code-xml[Main](data-bound-controls/samples/sample5.xml)]

Si noti che un solo siteMapNode elemento radice, in questo caso, l'elemento iniziale. Diversi attributi possono essere configurati per ogni siteMapNode. Gli attributi utilizzati più di frequente sono:

- **URL** specifica l'URL da visualizzare quando un utente fa clic sulla voce di menu. Se questo attributo non è presente, il nodo registrerà semplicemente quando fa clic su.
- **titolo** specifica il testo che viene visualizzato nella voce di menu.
- **Descrizione** utilizzato come documentazione per il nodo. Visualizza anche come descrizione comando quando si passa il mouse sul nodo.
- **siteMapFile** consente sitemaps annidati. Questo attributo deve puntare a un file sitemap ASP.NET ben formato.
- **ruoli** consente l'aspetto di un nodo di essere controllato tramite la rimozione della protezione ASP.NET.

Si noti che anche se questi attributi sono tutti facoltativi, il comportamento del menu di scelta potrebbe non essere quelli previsti se non sono specificati. Ad esempio, se il *url* attributo viene specificato, ma il *descrizione* attributo non è, il nodo non saranno visibile e che sarà presente alcun modo per passare all'URL specificato.

## <a name="controlling-a-menus-operation"></a>Il controllo di un'operazione di menu

Esistono diverse proprietà che influiscono sul funzionamento di un controllo Menu ASP.NET. il **orientamento** proprietà, il **DisappearAfter** proprietà, il **StaticItemFormatString** , proprietà e **StaticPopoutImageUrl**proprietà sono solo alcune di queste.

- Il **orientamento** può essere impostata su *orizzontali* o *verticale* e controlla se le voci di menu statico sono disposti orizzontalmente in una riga o in verticale e in pila al momento tra loro. Questa proprietà non influisce sul menu dinamici.
- Il **DisappearAfter** proprietà consente di configurare quanto tempo un menu dinamico deve rimanere visibile dopo il mouse è stato spostato il. Il valore è specificato in millisecondi e valore predefinito è 500. Impostazione di questa proprietà su un valore di -1 causerà il menu di mai eliminate automaticamente. In tal caso, il menu scompare solo quando l'utente fa clic all'esterno del menu.
- Il **StaticItemFormatString** proprietà rende più semplice mantenere coerenza lettera nel sistema di menu. Quando si specifica questa proprietà, *{0}* deve essere immesso al posto di descrizione che verrà visualizzata nell'origine dati. Per avere la voce di menu da pronunciare esercizio 1 visitare la nostra pagina prodotti e così via, ad esempio, si specificherà visita la pagina per il StaticItemFormatString {0}. In fase di esecuzione, ASP.NET sostituirà tutte le occorrenze di {0} con la descrizione della voce di menu corretta.
- Il **StaticPopoutImageUrl** proprietà specifica l'immagine che viene utilizzato per indicare che un nodo particolare menu dispone di nodi figlio che è possibile accedere al passaggio del mouse su di esso. Menu dinamici continuerà a utilizzare l'immagine predefinita.

## <a name="templated-menu-controls"></a>Controlli Menu basati su modelli

Il controllo Menu è un controllo basato su modelli e consente due ItemTemplates diversi; il StaticItemTemplate e il DynamicItemTemplate. Utilizzo di questi modelli, è possibile facilmente aggiungere controlli server o utente per i menu.

Per modificare i modelli di Visual Studio .NET, fare clic sul pulsante Smart Tag del menu e scegliere Modifica modelli. È quindi possibile scegliere tra la modifica di StaticItemTemplate o il DynamicItemTemplate.

I controlli aggiunti al StaticItemTemplate verranno visualizzato nel menu statico quando il caricamento della pagina. I controlli aggiunti al DynamicItemTemplate verranno visualizzati tutti i menu a comparsa.

## <a name="menu-events"></a>Eventi di menu

Il controllo Menu ha due eventi che sono univoci. il **MenuItemClicked** e **MenuItemDatabound** evento.

L'evento MenuItemClicked viene generato quando si fa clic su una voce di menu. L'evento MenuItemDatabound viene generato quando una voce di menu è associato a dati. Il **MenuEventArgs mostra** che viene passato all'evento gestore consente di accedere alla voce di menu tramite la proprietà dell'elemento.

## <a name="controlling-a-menus-appearance"></a>Controllo dell'aspetto di menu

È inoltre possibile modificare l'aspetto di un controllo menu utilizzando una o più degli stili del numero disponibile per i menu Formato. Tra questi vi sono **StaticMenuStyle**, **DynamicMenuStyle**, **DynamicMenuItemStyle**, **DynamicSelectedStyle**e **DynamicHoverStyle**. Queste proprietà vengono configurate tramite una stringa di stile HTML standard. Ad esempio, di seguito influirà lo stile per i menu dinamici.

[!code-aspx[Main](data-bound-controls/samples/sample6.aspx)]

> [!NOTE]
> Se si utilizza uno degli stili al passaggio del mouse, è necessario aggiungere un &lt;head&gt; elemento alla pagina con il *runat* impostato su *server*.


I controlli menu supportano anche l'uso dei temi di ASP.NET 2.0.

## <a name="the-treeview-control"></a>Il controllo TreeView

Il controllo TreeView Visualizza dati in una struttura ad albero. Come per il controllo Menu, può essere facilmente i dati associati a qualsiasi origine dati gerarchici, ad esempio SiteMapDataSource.

La prima domanda che i clienti possono richiedere informazioni sul controllo TreeView di ASP.NET 2.0 è o meno è correlato al WebControl IE TreeView che era disponibile per ASP.NET 1. x. Non è. Il controllo TreeView di ASP.NET 2.0 è stata scritta interamente e offre miglioramenti significativi rispetto TreeView WebControl IE che in precedenza era disponibile.

Non verrà esaminato in dettaglio su come associare un controllo TreeView a una mappa del sito quando viene eseguita nello stesso modo come il controllo Menu. Tuttavia, il controllo TreeView presenta alcune differenze distinti in modo che opera.

Per impostazione predefinita, un controllo TreeView viene visualizzato completamente espanso. Per modificare il livello di espansione al caricamento iniziale, modificare il **ExpandDepth** proprietà del controllo. Ciò è particolarmente importante in casi in cui il controllo TreeView è associato a dati su nodi specifici di espansione.

## <a name="databinding-the-treeview-control"></a>Associazione dati, il controllo TreeView

A differenza del Menu, il controllo TreeView si presta per gestire grandi quantità di dati. Pertanto, oltre all'associazione dati a un SiteMapDataSource o XMLDataSource, il controllo TreeView è spesso i dati associati a un set di dati o altri dati relazionali. Nei casi in cui è associato il controllo TreeView per grandi quantità di dati, è consigliabile associare solo ai dati che sono visibili nel controllo. Sarà possibile associare dati ai dati aggiuntivi come nodi di TreeView vengono espanse.

In questi casi, il **PopulateOnDemand** proprietà del controllo TreeView deve essere impostata su *true*. Sarà quindi necessario fornire un'implementazione per il **TreeNodePopulate** metodo.

## <a name="data-binding-without-postback"></a>Data Binding senza PostBack

Si noti che quando si espande un nodo nell'esempio precedente per la prima volta, la pagina viene eseguito il postback e viene aggiornata. Memorizzate non è un problema in questo esempio, ma si possono immaginare che potrebbe essere in un ambiente di produzione con una grande quantità di dati. Uno scenario migliore sarebbe uno in cui il controllo TreeView sarebbe comunque dinamicamente popolare i relativi nodi, ma senza un post il postback al server.

Impostando il **PopulateNodesFromClient** e **PopulateOnDemand** proprietà su true, il controllo TreeView di ASP.NET verranno compilate in modo dinamico i nodi senza un postback. Quando si espande il nodo padre, viene effettuata una richiesta di XMLHttp dal client e viene generato l'evento OnTreeNodePopulate. Il server risponde con un'isola di dati XML che viene quindi usato per i dati di associare i nodi figlio.

ASP.NET crea in modo dinamico il codice client che implementa questa funzionalità. Il &lt;script&gt; vengono generati i tag che contengono lo script che punta a un file AXD. Ad esempio, l'elenco seguente vengono visualizzati i collegamenti di script per il codice di script che genera la richiesta XMLHttp.

[!code-html[Main](data-bound-controls/samples/sample7.html)]

Se si passa il file AXD sopra nel browser e aprirlo, verrà visualizzato il codice che implementa la richiesta XMLHttp. Questo metodo impedisce i clienti di modificare il file di script.

## <a name="controlling-the-operation-of-the-treeview-control"></a>Controllo del funzionamento del controllo TreeView

Il controllo TreeView ha diverse proprietà che influiscono sul funzionamento del controllo. Le proprietà più evidenti sono la **ShowCheckBoxes**, **ShowExpandCollapse**, e **ShowLines**.

Il **ShowCheckBoxes** proprietà determina se nei nodi viene visualizzata una casella di controllo visualizzata o meno. I valori validi per questa proprietà sono **Nessuno**, **radice**, **padre**, **foglia**, e **tutti**. Questi influiscono sul controllo TreeView come indicato di seguito:

| **Valore della proprietà** | **Effetto** |
| --- | --- |
| Nessuno | Caselle di controllo non vengono visualizzati in tutti i nodi. Questa è l'impostazione predefinita. |
| radice | Una casella di controllo viene visualizzata solo per il nodo radice. |
| Padre | Una casella di controllo viene visualizzata solo in tali nodi contenenti nodi figlio. I nodi figlio possono essere nodi padre o nodi foglia. |
| Foglia | Una casella di controllo viene visualizzata solo per i nodi che non hanno nodi figlio. |
| Tutti | Una casella di controllo viene visualizzato in tutti i nodi. |

Quando vengono utilizzate le caselle di controllo, il **CheckedNodes** proprietà restituirà una raccolta di nodi di TreeView selezionati durante il postback.

Il **ShowExpandCollapse** proprietà controlla l'aspetto dell'immagine di espansione/compressione accanto ai nodi radice e padre. Se questa proprietà è impostata su **false**, nodi di TreeView vengono visualizzati come collegamenti ipertestuali e sono espanso o compresso facendo clic sul collegamento.

Il **ShowLines** proprietà controlla se sono visualizzate linee di connessione di nodi padre ai nodi figlio. Quando **false** (impostazione predefinita), non vengono visualizzate linee. Quando **true**, il controllo TreeView utilizzerà le immagini di righe nella cartella specificata per il **LineImagesFolder** proprietà.

Per personalizzare l'aspetto delle linee di TreeView, Visual Studio .NET 2005 include uno strumento di progettazione di riga. È possibile accedere a questo strumento utilizzando il pulsante Smart Tag nel controllo TreeView come di seguito.


![](data-bound-controls/_static/image1.jpg)

**Figura 1**


Quando seleziona il **Personalizza immagini linea** opzione di menu, verrà avviato lo strumento di progettazione di riga che consente di configurare l'aspetto delle linee di TreeView.

## <a name="treeview-events"></a>Eventi di TreeView

Il controllo TreeView ha i seguenti eventi univoci:

- SelectedNodeChanged si verifica quando viene selezionato un nodo in base al **SelectAction** proprietà.
- TreeNodeCheckChanged si verifica quando viene modificato uno stato checkboxs nodi.
- Eventi TreeNodeExpanded si verifica quando un nodo viene espanso in base al **SelectAction** proprietà.
- TreeNodeCollapsed si verifica quando si comprime un nodo.
- TreeNodeDataBound si verifica quando un nodo di dati associato.
- TreeNodePopulate si verifica quando un nodo viene compilato.

Il **SelectAction** proprietà consente di configurare l'evento viene generato quando si seleziona un nodo. La proprietà SelectAction fornisce le seguenti azioni:

- TreeNodeSelectAction. Expand genera eventi TreeNodeExpanded quando è selezionato il nodo.
- TreeNodeSelectAction non genera alcun evento quando viene selezionato il nodo.
- TreeNodeSelectAction genera l'evento SelectedNodeChanged quando è selezionato il nodo.
- Su TreeNodeSelectAction. SelectExpand genera l'evento SelectedNodeChanged e l'evento di eventi TreeNodeExpanded quando è selezionato il nodo.

## <a name="controlling-appearance-with-styles"></a>Controllo dell'aspetto con stili

Il controllo TreeView fornisce molte proprietà per controllare l'aspetto del controllo con stili. Le proprietà seguenti sono disponibili.

| **Nome della proprietà** | **Controlli** |
| --- | --- |
| HoverNodeStyle | Controlla lo stile dei nodi quando si passa il mouse su di essi. |
| LeafNodeStyle | Controlla lo stile dei nodi foglia. |
| NodeStyle | Controlla lo stile di tutti i nodi. Gli stili di nodo specifico (ad esempio LeafNodeStyle) eseguire l'override di questo stile. |
| ParentNodeStyle | Controlla lo stile di tutti i nodi padre. |
| RootNodeStyle | Controlla lo stile per il nodo radice. |
| SelectedNodeStyle | Controlla lo stile per il nodo selezionato. |

Ognuna di queste proprietà è di sola lettura. Tuttavia, essi verrà ciascuno un **TreeNodeStyle** oggetto e le proprietà di tale oggetto possono essere modificata nel *sottoproprietà di proprietà* formato. Ad esempio, per impostare il **ForeColor** proprietà del **SelectedNodeStyle**, utilizzare la sintassi seguente:

[!code-aspx[Main](data-bound-controls/samples/sample8.aspx)]

Si noti che il tag precedente non è chiuso. Questo accade perché quando si utilizza la sintassi dichiarativa illustrata di seguito, è necessario includere i nodi di controlli TreeView nonché il codice HTML.

Le proprietà di stile possono anche essere specificate nel codice utilizzando il *Property. Subproperty* formato. Ad esempio, per impostare il **ForeColor** proprietà del **RootNodeStyle** nel codice, utilizzare la sintassi seguente:

[!code-csharp[Main](data-bound-controls/samples/sample9.cs)]

> [!NOTE]
> Per un elenco completo delle proprietà di stile diverse, vedere la documentazione MSDN sull'oggetto TreeNodeStyle.


## <a name="the-sitemappath-control"></a>Il controllo SiteMapPath

Il controllo SiteMapPath fornisce un controllo di navigazione di navigazione Pane per sviluppatori ASP.NET. Come gli altri controlli di navigazione, può essere facilmente dati associati a dati gerarchici origini, ad esempio le XmlDataSource SiteMapDataSource.

Un controllo SiteMapPath è costituito da oggetti SiteMapNodeItem. Esistono tre tipi di nodi. il nodo radice, nodi padre e il nodo corrente. Il nodo radice è il nodo nella parte superiore della struttura gerarchica. Il nodo corrente rappresenta la pagina corrente. Tutti gli altri nodi sono nodi padre.

## <a name="controlling-the-operation-of-the-sitemappath-control"></a>Controllo del funzionamento del controllo SiteMapPath

Come indicato di seguito sono riportate le proprietà che controllano il funzionamento del controllo SiteMapPath:

| **Property** | **Descrizione della proprietà** |
| --- | --- |
| ParentLevelsDisplayed | Determina il numero di nodi padre viene visualizzato. Il valore predefinito è -1 che non pone limiti al numero di nodi padre visualizzati. |
| PathDirection | Controlla la direzione del controllo SiteMapPath. I valori validi sono RootToCurrent (impostazione predefinita) e CurrentToRoot. |
| PathSeparator | Stringa che controlla il carattere che separa i nodi in un controllo SiteMapPath. Il valore predefinito è:. |
| RenderCurrentNodeAsLink | Valore booleano che controlla se il nodo corrente viene eseguito il rendering come collegamento. Il valore predefinito è False. |
| SkipLinkText | Fornisce assistenza con accessibilità quando viene visualizzata la pagina lettura dello schermo. Questa proprietà consente di ignorare il controllo SiteMapPath gli assistenti. Per disabilitare questa funzionalità, impostare la proprietà su String. Empty. |

## <a name="templated-sitemappath-controls"></a>Controlli basati su modelli SiteMapPath

Il SiteMapControl è un controllo basato su modelli e di conseguenza, è possibile definire modelli diversi per l'utilizzo nella visualizzazione del controllo. Per modificare i modelli in un controllo SiteMapPath, fare clic sul pulsante Smart Tag del controllo e scegliere Modifica modelli dal menu. Verrà visualizzato il menu SiteMapTasks come illustrato di seguito in cui è possibile scegliere tra i diversi modelli disponibili.


![](data-bound-controls/_static/image2.jpg)

**Figura 2**


Il **NodeTemplate** modello fa riferimento a tutti i nodi il controllo SiteMapPath. Se il nodo è un nodo radice o il nodo corrente e un **RootNodeTemplate** o **CurrentNodeTemplate** è configurato, il NodeTemplate viene sottoposto a override.

## <a name="sitemappath-events"></a>Eventi SiteMapPath

Il controllo SiteMapPath dispone di due eventi che non sono derivati dalla classe Control; il **ItemCreated** evento e **ItemDataBound** evento. L'evento ItemCreated viene generato quando viene creato un elemento SiteMapPath. ItemDataBound viene generato quando viene chiamato il metodo DataBind durante l'associazione di dati di un nodo SiteMapPath. Oggetto **SiteMapNodeItemEventArgs** oggetto fornisce l'accesso al SiteMapNodeItem specifico tramite la proprietà dell'elemento.

## <a name="controlling-appearance-with-styles"></a>Controllo dell'aspetto con stili

Gli stili seguenti sono disponibili per la formattazione di un controllo SiteMapPath.

| **Nome della proprietà** | **Controlli** |
| --- | --- |
| CurrentNodeStyle | Controlla lo stile del testo per il nodo corrente. |
| RootNodeStyle | Controlla lo stile del testo per il nodo radice. |
| NodeStyle | Controlla lo stile del testo per tutti i nodi, presupponendo che non si applica un CurrentNodeStyle o RootNodeStyle. |

La proprietà NodeStyle viene eseguito l'override di CurrentNodeStyle o il RootNodeStyle. Ognuna di queste proprietà è di sola lettura e restituisce un **stile** oggetto. Per definire l'aspetto di un nodo con una di queste proprietà, è necessario impostare le proprietà dell'oggetto stile restituito. Ad esempio, il codice seguente modifica la proprietà forecolor del nodo corrente.

[!code-aspx[Main](data-bound-controls/samples/sample10.aspx)]

La proprietà può essere applicata anche a livello di programmazione come indicato di seguito:

[!code-csharp[Main](data-bound-controls/samples/sample11.cs)]

> [!NOTE]
> Se viene applicato un modello, non verrà applicato lo stile.


## <a name="lab-1-configuring-an-aspnet-menu-control"></a>Esercitazione 1: Configurazione di un controllo Menu ASP.NET

1. Creare un nuovo sito Web.
2. Aggiungere un file di mappa del sito selezionando File, nuovo, File e scegliendo mappa del sito dall'elenco dei modelli di file.
3. Aprire la mappa del sito (sitemap per impostazione predefinita) e modificarlo in modo che risulti simile di seguito. Le pagine che sono collegati nel file di mappa del sito non esistessero nella realtà, ma che potrebbe non costituire un problema per questo esercizio.

    [!code-xml[Main](data-bound-controls/samples/sample12.xml)]
4. Aprire il modulo Web predefinito in visualizzazione progettazione.
5. Dalla sezione di spostamento della casella degli strumenti, aggiungere un nuovo controllo Menu.
6. Dalla sezione di dati della casella degli strumenti, aggiungere un nuovo SiteMapDataSource. SiteMapDataSource utilizzeranno automaticamente il file sitemap del sito. (Il file sitemap *deve* nella cartella radice del sito.)
7. Fare clic sul controllo Menu e quindi fare clic sul pulsante Smart Tag per visualizzare la finestra di dialogo attività di Menu.
8. Nell'elenco a discesa Scegli origine dati, selezionare SiteMapDataSource1.
9. Fare clic sul collegamento di formattazione automatica e scegliere un formato per il Menu.
10. Nel riquadro proprietà, impostare il **StaticDisplayLevels** proprietà su 2. Il controllo Menu dovrebbe ora visualizzare il nodo iniziale, i prodotti e servizi nella finestra di progettazione.
11. Selezionare la pagina nel browser per utilizzare il menu. (Poiché non esistono effettivamente le pagine in cui è già aggiunto alla mappa del sito, si verrà visualizzato un errore durante il tentativo di passare a essi.)

Provare a modificare il StaticDisplayLevels e le proprietà MaximumDynamicDisplayLevels e di controllare la modalità di rendering di menu.

## <a name="lab-2-dynamically-binding-a-treeview-control"></a>Laboratorio 2: Associazione dinamica di un controllo TreeView

Questo esercizio si presuppone che SQL Server in esecuzione in locale e che il database Northwind è presente nell'istanza di SQL Server. Se non vengono soddisfatte queste condizioni, modificare la stringa di connessione nell'esempio. Si noti che è necessario anche specificare l'autenticazione di SQL Server anziché una connessione trusted.

1. Creare un nuovo sito Web.
2. Passare alla visualizzazione codice per Default.aspx e sostituire tutto il codice con il codice riportato di seguito. 

    [!code-aspx[Main](data-bound-controls/samples/sample13.aspx)]
3. Salvare la pagina come treeview.aspx.
4. Selezionare la pagina.
5. Quando la pagina viene visualizzata prima di tutto, visualizzare l'origine della pagina nel browser. Si noti che solo i nodi visibili sono stati inviati al client.
6. Fare clic sul segno più accanto a ogni nodo.
7. Visualizza l'origine della pagina nuovamente. Si noti che i nodi appena visualizzati ora sono presenti.

## <a name="lab-3-details-view-and-editing-data-using-a-gridview-and-detailsview"></a>Esercitazione 3: Dettagli visualizzazione e modifica dei dati mediante un GridView e controllo DetailsView.

1. Creare un nuovo sito Web.
2. Aggiungere un nuovo file Web. config per il sito Web.
3. Aggiungere una stringa di connessione nel file Web. config come illustrato di seguito: 

    [!code-xml[Main](data-bound-controls/samples/sample14.xml)]

    > [!NOTE]
    > Potrebbe essere necessario modificare la stringa di connessione in base all'ambiente.
4. Salvare e chiudere il file Web.config.
5. Aprire Default.aspx e aggiungere un nuovo controllo SqlDataSource.
6. Modificare l'ID del controllo SqlDataSource per **prodotti**.
7. Nel **Attività SqlDataSource** menu, fare clic su **Configura origine dati**.
8. Selezionare **Northwind** nell'elenco a discesa connessione e fare clic su Avanti.
9. Selezionare **prodotti** dal **nome** elenco a discesa e selezionare il **ProductID**, **ProductName**, **UnitPrice**, e **UnitsInStock** caselle di controllo come illustrato di seguito. 

![](data-bound-controls/_static/image3.jpg)

    **Figure 3**
10. Scegliere **Avanti**.
11. Scegliere **Fine**.
12. Passare alla visualizzazione origine ed esaminare il codice generato. Si noti il **SelectCommand**, **DeleteCommand**, **InsertCommand**, e **UpdateCommand** che sono stati aggiunti per SqlDataSource controllo. Inoltre, i parametri che sono stati aggiunti.
13. Passare alla visualizzazione struttura e aggiungere un nuovo controllo GridView.
14. Selezionare **prodotti** dal **Scegli origine dati** elenco a discesa.
15. Controllare **Abilita Paging** e **Abilita selezione** come illustrato di seguito. 

![](data-bound-controls/_static/image4.jpg)

    **Figure 4**
16. Fare clic su di **Modifica colonne** collegare e verificare che **generare automaticamente campi** è selezionata.
17. Fare clic su **OK**.
18. Con il controllo GridView selezionato, fare clic sul pulsante accanto al **DataKeyNames** proprietà nel riquadro proprietà.
19. Selezionare **ProductID** dal **i campi dati disponibili** elenco e fare clic su di  **&gt;**  pulsante per aggiungerlo.
20. Fare clic su OK.
21. Aggiungere un nuovo controllo SqlDataSource per la pagina.
22. Modificare l'ID del controllo SqlDataSource per **dettagli**.
23. Scegliere dal menu Attività SqlDataSource **Configura origine dati**.
24. Scegliere **Northwind** dall'elenco a discesa scegliere **Avanti**.
25. Selezionare **prodotti** dal **nome** elenco a discesa e controllare il  **\***  nella casella di controllo di **colonne** listbox.
26. Fare clic su di **dove** pulsante.
27. Selezionare **ProductID** dal **colonna** elenco a discesa.
28. Selezionare  **=**  nell'elenco a discesa operatore.
29. Selezionare **controllo** dal **origine** elenco a discesa.
30. Selezionare **GridView1** dal **ID controllo** elenco a discesa.
31. Fare clic su di **Aggiungi** pulsante per aggiungere la clausola WHERE.
32. Fare clic su **OK**.
33. Fare clic su di **avanzate** pulsante e controllare il **genera istruzioni INSERT, UPDATE e DELETE** casella di controllo.
34. Fare clic su **OK**.
35. Fare clic su **Avanti** e fare clic su **fine**.
36. Aggiungere un controllo DetailsView.
37. Nel **Scegli origine dati** elenco a discesa scegliere **dettagli**.
38. Controllare il **Abilita modifica** casella di controllo come illustrato di seguito. 

![](data-bound-controls/_static/image1.gif)

    **Figure 5**
39. Salvare la pagina e selezionare Default.aspx.
40. Fare clic su di **selezionare** collegamento accanto a record diversi per visualizzare automaticamente l'aggiornamento di DetailsView.
41. Fare clic su di **modifica** collegamento nel controllo DetailsView.
42. Apportare una modifica al record e fare clic su **aggiornamento**.
