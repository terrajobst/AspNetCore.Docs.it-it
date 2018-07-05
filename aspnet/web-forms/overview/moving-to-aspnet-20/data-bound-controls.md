---
uid: web-forms/overview/moving-to-aspnet-20/data-bound-controls
title: Controlli con associazione a dati | Microsoft Docs
author: microsoft
description: La maggior parte delle applicazioni ASP.NET si basano su un certo livello di presentazione dei dati da un'origine dati back-end. Controlli con associazione a dati sono stati parte pivotal di interazione w...
ms.author: aspnetcontent
ms.date: 02/20/2005
ms.assetid: 0e23ff32-646d-43f3-8bec-6b2313d3abd6
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/data-bound-controls
msc.type: authoredcontent
ms.openlocfilehash: bfee5ed9c786cc8799da108b99781ff6eb508477
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833219"
---
<a name="data-bound-controls"></a>Controlli con associazione a dati
====================
da [Microsoft](https://github.com/microsoft)

> La maggior parte delle applicazioni ASP.NET si basano su un certo livello di presentazione dei dati da un'origine dati back-end. Controlli con associazione a dati sono stati una parte fondamentale dell'interazione con i dati in applicazioni Web dinamiche. ASP.NET 2.0 introduce alcuni miglioramenti sostanziali a controlli con associazione a dati, tra cui una nuova classe BaseDataBoundControl e sintassi dichiarativa.


La maggior parte delle applicazioni ASP.NET si basano su un certo livello di presentazione dei dati da un'origine dati back-end. Controlli con associazione a dati sono stati una parte fondamentale dell'interazione con i dati in applicazioni Web dinamiche. ASP.NET 2.0 introduce alcuni miglioramenti sostanziali a controlli con associazione a dati, tra cui una nuova classe BaseDataBoundControl e sintassi dichiarativa.

Il BaseDataBoundControl funge da classe base per la classe di DataBoundControl e la classe HierarchicalDataBoundControl. In questo modulo, verranno illustrate le classi seguenti che derivano da DataBoundControl:

- AdRotator
- Controlli List
- GridView
- FormView
- Controllo DetailsView

Verranno inoltre esaminate le classi seguenti che derivano dalla classe HierarchicalDataBoundControl:

- TreeView
- Menu
- SiteMapPath

## <a name="databoundcontrol-class"></a>Classe di DataBoundControl

La classe di DataBoundControl è una classe astratta (contrassegnata come MustInherit in Visual Basic) consente di interagire con tabulari o dati di tipo elenco. I seguenti controlli sono alcuni dei controlli che derivano da DataBoundControl.

## <a name="adrotator"></a>AdRotator

Il controllo AdRotator consente di visualizzare un banner di grafica in una pagina Web collegata a un URL specifico. Il grafico visualizzato viene ruotato mediante le proprietà per il controllo. È possibile configurare la frequenza di una visualizzazione ad specifico in una pagina tramite il **impression** proprietà e gli annunci possono essere filtrati utilizzando un filtro di parole chiave.

Controlli AdRotator utilizzano un file XML o una tabella in un database per i dati. Gli attributi seguenti vengono utilizzati nei file XML per configurare il controllo AdRotator.

### <a name="imageurl"></a>ImageUrl
L'URL di un'immagine da visualizzare per Active Directory.

### <a name="navigateurl"></a>NavigateUrl
URL a cui l'utente deve essere impiegato per quando viene selezionato Active Directory. Deve essere codificato in URL.

### <a name="alternatetext"></a>AlternateText
Testo alternativo che viene visualizzato nella descrizione comando e letti dai lettori dello schermo. Viene visualizzato anche quando l'immagine specificata dalla proprietà ImageUrl non è disponibile.

### <a name="keyword"></a>Parola chiave
Definisce una parola chiave che può essere utilizzata quando si usa il filtro (parola chiave). Se specificato, verranno visualizzati solo questi annunci con una parola chiave corrispondente al filtro (parola chiave).

### <a name="impressions"></a>Impression
Un numero di ponderazione che determina la frequenza ad un particolare è probabile che venga visualizzato. È relativo all'impressione di altri annunci nello stesso file. Il valore massimo di impression collettivo per tutti gli annunci in un file XML è superiore a 2.048.000.000 1.

### <a name="height"></a>Altezza
L'altezza di ad in pixel.

### <a name="width"></a>Larghezza
La larghezza di ad in pixel.


> [!NOTE]
> Eseguire l'override gli attributi di altezza e larghezza l'altezza e larghezza per il controllo AdRotator stesso.


Un tipico file XML potrebbe essere simile al seguente:

[!code-xml[Main](data-bound-controls/samples/sample1.xml)]

Nell'esempio precedente, è due volte come potrebbe apparire come Active Directory per il sito Web ASP.NET a causa del valore dell'attributo di impression di ad di Contoso.

Per visualizzare annunci pubblicitari nel file XML precedente, aggiungere un controllo AdRotator a una pagina e impostare il **AdvertisementFile** proprietà in modo da puntare al file XML, come illustrato di seguito:

[!code-aspx[Main](data-bound-controls/samples/sample2.aspx)]

Se si sceglie di usare una tabella di database come origine dati per il controllo AdRotator, è necessario innanzitutto configurare un database usando lo schema seguente:

| **Nome della colonna** | **Tipo di dati** | **Descrizione** |
| --- | --- | --- |
| Id | int | Chiave primaria. Questa colonna può avere qualsiasi nome. |
| ImageUrl | nvarchar (*lunghezza*) | L'URL assoluto o relativo dell'immagine da visualizzare per Active Directory. |
| NavigateUrl | nvarchar (*lunghezza*) | L'URL di destinazione per l'annuncio. Se non si specifica un valore, Active Directory non è un collegamento ipertestuale. |
| AlternateText | nvarchar (*lunghezza*) | Testo visualizzato se l'immagine non viene trovato. In alcuni browser, il testo viene visualizzato come descrizione comando. Testo alternativo viene inoltre utilizzato per l'accessibilità, in modo che gli utenti che non è possibile vedere il grafico possono sentirmi relativa descrizione leggere a voce alta. |
| Parola chiave | nvarchar (*lunghezza*) | Una categoria per l'annuncio in cui è possibile filtrare la pagina. |
| Impression | int(4) | Numero che indica la probabilità che la frequenza con cui l'annuncio viene visualizzato. Maggiore il numero, più spesso l'annuncio verrà visualizzato. Il totale di tutti i valori di impression nel file XML non può superare superiore a 2.048.000.000-1. |
| Larghezza | int(4) | La larghezza in pixel dell'immagine. |
| Altezza | int(4) | L'altezza in pixel dell'immagine. |

Nei casi in cui si dispone già di un database con uno schema diverso, è possibile usare la **AlternateTextField**, **ImageUrlField**, e **NavigateUrlField** le proprietà per eseguire il mapping di Attributi del controllo AdRotator al database esistente. Per visualizzare i dati dal database nel controllo AdRotator, aggiungere un controllo origine dati alla pagina, configurare la stringa di connessione per il controllo origine dati in modo che punti al database e impostare il controllo AdRotator **DataSourceID** proprietà per l'ID del controllo origine dati. Nei casi in cui si ha l'esigenza di configurare AdRotator ads a livello di codice, usare l'evento AdCreated. L'evento AdCreated accetta due parametri; un oggetto e l'altra è un'istanza di AdCreatedEventArgs. Il AdCreatedEventArgs è un riferimento ad Active Directory che si sta creando.

Il frammento di codice seguente imposta la proprietà ImageUrl, Instrumentedlink e AlternateText a livello di codice per un'istanza di ad:

[!code-csharp[Main](data-bound-controls/samples/sample3.cs)]

## <a name="list-controls"></a>Controlli List

Controlli elenco sono inclusi il controllo ListBox, DropDownList, CheckBoxList, RadioButtonList e BulletedList. Ognuno di questi controlli può essere dati associati a un'origine dati. Che usa un campo nell'origine dati come testo da visualizzare e, facoltativamente, può usare un secondo campo come valore di un elemento. È anche possibile aggiungere elementi in modo statico in fase di progettazione, ed è possibile combinare gli elementi statici e dinamici elementi aggiunti da un'origine dati.

Ai dati di associare un controllo elenco, aggiungere un controllo origine dati alla pagina. Specificare un comando SELECT per il controllo origine dati e quindi impostare la proprietà DataSourceID del controllo elenco per l'ID del controllo origine dati. Usare la **DataTextField** e **DataValueField** le proprietà per definire il testo visualizzato e il valore per il controllo. Inoltre, è possibile usare la **DataTextFormatString** proprietà per controllare l'aspetto del testo visualizzato come segue:

| **Espressione** | **Descrizione** |
| --- | --- |
| Prezzo: {0:C} | Per i dati numerici o decimali. Consente di visualizzare il valore letterale "Price:" seguita da numeri nel formato di valuta. Il formato della valuta dipende dalle impostazioni cultura specificate nell'attributo delle impostazioni cultura nel **pagina** direttiva o nel file Web. config. |
| {0:D4} | Per i dati integer. Non può essere utilizzato con numeri decimali. Gli Integer vengono visualizzati in un campo con degli zero di quattro caratteri wide. |
| {0:N2}% | Per i dati numerici. Visualizza il numero con 2 cifre decimali di precisione seguito dal valore letterale "%". |
| {0:000.0} | Per i dati numerici o decimali. I numeri vengono arrotondati a una cifra decimale. I numeri composti da meno di tre cifre sono riempiti con degli zero. |
| {0:D} | Per i dati di data/ora. Formato di data estesa ("giovedì, agosto 06, 1996") consente di visualizzare. Il formato della data dipende dalle impostazioni cultura della pagina o del file Web.config. |
| {0:d} | Per i dati di data/ora. Formato data breve consente di visualizzare ("12/31/99"). |
| {0:yy-MM-dd} | Per i dati di data/ora. Visualizza la data in formato numerico anno-mese-giorno (96-08-06) |

## <a name="gridview"></a>GridView

Il controllo GridView consente la visualizzazione di dati tabulari e la modifica usando un approccio dichiarativo ed è il successore al controllo DataGrid. Le funzionalità seguenti sono disponibili nel controllo GridView.

- Associazione a dati controlli di origine, ad esempio SqlDataSource.
- Funzionalità di ordinamento integrate.
- Predefinite, aggiornamento ed eliminazione di funzionalità.
- Funzionalità incorporate di paging.
- Funzionalità di selezione di riga predefinito.
- Accesso programmatico al modello a oggetti GridView per impostare dinamicamente le proprietà, la gestione degli eventi e così via.
- Più campi di chiave.
- Più campi di dati per le colonne di collegamento ipertestuale.
- Aspetto personalizzabile tramite i temi e stili.

**Campi delle colonne**

Ogni colonna nel controllo GridView è rappresentata da un oggetto DataControlField. Per impostazione predefinita, la proprietà AutoGenerateColumns è impostata su **true**, che consente di creare un oggetto AutoGeneratedField per ogni campo nell'origine dati. Viene quindi eseguito il rendering di ogni campo come colonna nel controllo GridView nell'ordine in cui ogni campo viene visualizzato nell'origine dati. È anche possibile definire manualmente quale colonna campi vengono visualizzati nel **GridView** controllo impostando la **AutoGenerateColumns** proprietà **false** e quindi definendo il proprio raccolta di campi colonna. Tipi di campi colonna diversa determinano il comportamento delle colonne nel controllo.

Nella tabella seguente elenca i tipi di campo colonna diversa che possono essere utilizzati.

| **Tipo di campo colonna** | **Descrizione** |
| --- | --- |
| BoundField | Visualizza il valore di un campo in un'origine dati. Questo è il tipo di colonna predefinito del controllo GridView. |
| ButtonField | Consente di visualizzare un pulsante di comando per ogni elemento nel controllo GridView. In questo modo è possibile creare una colonna di pulsanti personalizzati, ad esempio l'aggiunta o il pulsante Rimuovi. |
| CheckBoxField | Visualizza una casella di controllo per ogni elemento nel controllo GridView. Questo tipo di campo colonna viene comunemente usato per visualizzare campi con un valore booleano. |
| CommandField | Visualizza i pulsanti di comando per eseguire la selezione, modifica o eliminazione di operazioni predefiniti. |
| HyperLinkField | Visualizza il valore di un campo in un'origine dati come collegamento ipertestuale. Questo tipo di campo colonna consente di associare un secondo campo all'URL del collegamento ipertestuale. |
| ImageField | Visualizza un'immagine per ogni elemento nel controllo GridView. |
| TemplateField | Visualizza il contenuto definito dall'utente per ogni elemento nel controllo GridView in base a un modello specificato. Questo tipo di campo colonna consente di creare un campo colonna personalizzata. |

Per definire una raccolta di campi colonna in modo dichiarativo, aggiungere prima di apertura e chiusura **&lt;colonne&gt;** tag tra i tag di apertura e chiusura del controllo GridView. Successivamente, Elenca i campi di colonna che si desidera includere tra l'apertura e chiusura **&lt;colonne&gt;** tag. Le colonne specificate vengono aggiunti all'insieme di colonne nell'ordine elencato. Il **colonne** collection archivia tutti i campi nel controllo e consente di gestire a livello di programmazione i campi colonna nel controllo GridView, la colonna.

I campi dichiarati in modo esplicito di colonne possono essere visualizzati in combinazione con i campi colonna generato automaticamente. Quando vengono utilizzati entrambi, i campi dichiarati in modo esplicito di colonne vengono visualizzati per primi, seguito dai campi di colonna generato automaticamente.

## <a name="binding-to-data"></a>Associazione a dati

Il controllo GridView può essere associato a un controllo origine dati (ad esempio **SqlDataSource**, **ObjectDataSource**e così via), nonché per qualsiasi origine dati che implementa l'IEnumerable interfaccia (ad esempio System.Data.DataView, ArrayList e Hashtable). Per associare il controllo GridView per il tipo di origine di dati appropriato, usare uno dei metodi seguenti:

- Per associare un controllo origine dati, impostare la proprietà DataSourceID del controllo GridView per il valore ID del controllo origine dati. Il controllo GridView associato automaticamente al controllo origine dati specificata e può sfruttare la funzionalità del controllo per eseguire l'ordinamento, l'aggiornamento, eliminazione e la funzionalità di paging. Questo è il metodo preferito per associare ai dati.
- Per eseguire l'associazione a un'origine dati che implementa l'interfaccia IEnumerable, a livello di codice impostare la proprietà DataSource del controllo GridView all'origine dati e quindi chiamare il metodo DataBind. Quando si usa questo metodo, il controllo GridView non fornisce incorporato l'ordinamento, l'aggiornamento, eliminazione e la funzionalità di paging. È necessario specificare manualmente tale funzionalità.

## <a name="data-operations"></a>Operazioni sui dati

Il controllo GridView offre numerose funzionalità incorporate che consentono all'utente di ordinare, aggiornare, eliminare, selezionare e scorrere gli elementi del controllo. Quando il controllo GridView è associato a un controllo origine dati, il controllo GridView possa sfruttare le funzionalità del controllo di origine dati e fornire l'ordinamento, aggiornamenti automatici e l'eliminazione di funzionalità.

> [!NOTE]
> Il controllo GridView può fornire supporto per l'ordinamento, aggiornamento ed eliminazione con altri tipi di origini dati. Tuttavia, è necessario fornire un gestore eventi appropriato con l'implementazione di queste operazioni.


L'ordinamento consente all'utente di ordinare gli elementi nel controllo GridView rispetto a una colonna specifica facendo clic sull'intestazione della colonna. Per abilitare l'ordinamento, impostare la proprietà AllowSorting su **true**.

Le funzionalità automatiche di aggiornamento, eliminazione e selezione vengono abilitate quando un pulsante in un **ButtonField** oppure **TemplateField** campo colonna, con un nome di comando di "Modifica", "Delete" e "Select", è fatto clic su rispettivamente. Il controllo GridView può aggiungere automaticamente una **CommandField** campo colonna con una modifica, eliminazione o pulsante di selezione se la proprietà AutoGenerateEditButton, AutoGenerateDeleteButton o AutoGenerateSelectButton è impostata su **true**, rispettivamente.

> [!NOTE]
> Inserimento di record nell'origine dati non è direttamente supportata dal controllo GridView. Tuttavia, è possibile inserire i record usando il controllo GridView in combinazione con il controllo DetailsView o controllo FormView.


Invece di visualizzare tutti i record nell'origine dati allo stesso tempo, il controllo GridView possa suddividere automaticamente i record in pagine. Per abilitare il paging, impostare la proprietà AllowPaging su **true**.

## <a name="customizing-the-user-interface"></a>Personalizzazione dell'interfaccia utente

È possibile personalizzare l'aspetto del controllo GridView impostando le proprietà di stile per le diverse parti del controllo. Nella tabella seguente sono elencate le proprietà di stile diverso.

| **Proprietà di stile** | **Descrizione** |
| --- | --- |
| AlternatingRowStyle | Le impostazioni di stile per le righe di dati alternativi nel controllo GridView. Quando questa proprietà è impostata, vengono visualizzate le righe di dati alterne tra le impostazioni di RowStyle e il **AlternatingRowStyle** impostazioni. |
| EditRowStyle | Le impostazioni di stile per la riga in corso di modifica nel controllo GridView. |
| EmptyDataRowStyle | Le impostazioni di stile per la riga di dati vuota visualizzata nel controllo GridView quando l'origine dati non contiene alcun record. |
| FooterStyle | Le impostazioni di stile per la riga di piè di pagina del controllo GridView. |
| HeaderStyle | Le impostazioni di stile per la riga di intestazione del controllo GridView. |
| PagerStyle | Le impostazioni di stile per la riga di spostamento del controllo GridView. |
| RowStyle | Le impostazioni di stile per le righe di dati nel controllo GridView. Quando il **AlternatingRowStyle** proprietà è impostata, vengono visualizzate le righe di dati alternando tra la **RowStyle** impostazioni e le **AlternatingRowStyle** impostazioni. |
| SelectedRowStyle | Le impostazioni di stile per la riga selezionata nel controllo GridView. |

È anche possibile visualizzare o nascondere parti diverse del controllo. Nella tabella seguente sono elencate le proprietà che controllano quali parti vengono visualizzate o nascoste.

| **Property** | **Descrizione** |
| --- | --- |
| ShowFooter | Mostra o nasconde la sezione di piè di pagina del controllo GridView. |
| ShowHeader | Mostra o nasconde la sezione di intestazione del controllo GridView. |

### <a name="events"></a>Eventi

Il controllo GridView fornisce diversi eventi che è possibile programmare. In questo modo è possibile eseguire una routine personalizzata ogni volta che si verifica un evento. La tabella seguente elenca gli eventi supportati dal controllo GridView.

| **Event** | **Descrizione** |
| --- | --- |
| PageIndexChanged | Si verifica quando viene fatto clic su uno dei pulsanti di spostamento, ma dopo che il controllo GridView gestisce l'operazione di spostamento. Questo evento viene comunemente utilizzato quando è necessario eseguire un'attività dopo che l'utente passa a un'altra pagina nel controllo. |
| PageIndexChanging | Si verifica quando viene fatto clic su uno dei pulsanti di spostamento, ma prima che il controllo GridView controllo gestisca l'operazione di spostamento. Questo evento viene spesso utilizzato per annullare l'operazione di spostamento. |
| RowCancelingEdit | Si verifica quando si fa clic sul pulsante di annullamento di una riga, ma prima che il controllo GridView esce dalla modalità di modifica. Questo evento viene spesso utilizzato per arrestare l'operazione di annullamento. |
| RowCommand | Si verifica quando viene selezionato un pulsante nel controllo GridView. Questo evento viene spesso utilizzato per eseguire un'attività quando viene selezionato un pulsante nel controllo. |
| RowCreated | Si verifica quando viene creata una nuova riga nel controllo GridView. Questo evento viene spesso utilizzato per modificare il contenuto di una riga quando la riga viene creata. |
| RowDataBound | Si verifica quando una riga di dati è associata a dati nel controllo GridView. Questo evento viene spesso utilizzato per modificare il contenuto di una riga quando la riga è associata a dati. |
| RowDeleted | Si verifica quando si fa clic sul pulsante Elimina di una riga, ma dopo che il controllo GridView Elimina il record dall'origine dati. Questo evento viene spesso utilizzato per controllare i risultati dell'operazione di eliminazione. |
| RowDeleting | Si verifica quando si fa clic sul pulsante Elimina di una riga, ma prima di GridView controllo Elimina il record dall'origine dati. Questo evento viene spesso utilizzato per annullare l'operazione di eliminazione. |
| RowEditing | Si verifica quando si fa clic sul pulsante di modifica di una riga, ma prima che il controllo GridView controllo passi alla modalità di modifica. Questo evento viene spesso utilizzato per annullare l'operazione di modifica. |
| RowUpdated | Si verifica quando si fa clic sul pulsante di aggiornamento di una riga, ma dopo che il controllo GridView aggiorni la riga. Questo evento viene spesso utilizzato per controllare i risultati dell'operazione di aggiornamento. |
| RowUpdating | Si verifica quando si fa clic sul pulsante di aggiornamento di una riga, ma prima di GridView ha aggiornato la riga. Questo evento viene spesso utilizzato per annullare l'operazione di aggiornamento. |
| SelectedIndexChanged | Si verifica quando si fa clic sul pulsante di selezione di una riga, ma dopo che il controllo GridView gestisce l'operazione di selezione. Questo evento viene spesso utilizzato per eseguire un'attività dopo aver selezionata una riga nel controllo. |
| SelectedIndexChanging | Si verifica quando si fa clic sul pulsante di selezione di una riga, ma prima che il controllo GridView controllo gestisca l'operazione di selezione. Questo evento viene spesso utilizzato per annullare l'operazione di selezione. |
| Ordinamento | Si verifica quando si fa clic sul collegamento ipertestuale per ordinare una colonna, ma dopo che il controllo GridView gestisce l'operazione di ordinamento. Questo evento viene comunemente utilizzato per eseguire un'attività dopo che l'utente fa clic su un collegamento ipertestuale per ordinare una colonna. |
| Ordinamento | Si verifica quando si fa clic sul collegamento ipertestuale per ordinare una colonna, ma prima che il controllo GridView controllo gestisca l'operazione di ordinamento. Questo evento viene spesso utilizzato per annullare l'operazione di ordinamento o per eseguire una routine di ordinamento personalizzata. |

## <a name="formview"></a>FormView

Consente di visualizzare un singolo record da un'origine dati del controllo FormView. È simile al controllo DetailsView, ad eccezione del fatto che consente di visualizzare modelli definiti dall'utente anziché i campi riga. Creazione di modelli personalizzati offre maggiore flessibilità per la modalità di visualizzazione dei dati. Il controllo FormView supporta le funzionalità seguenti:

- Associazione ai dati di origine controlli, quali SqlDataSource e ObjectDataSource.
- Funzionalità di inserimento incorporate.
- Predefinite, aggiornamento ed eliminazione di funzionalità.
- Funzionalità incorporate di paging.
- Accesso programmatico al modello a oggetti FormView per impostare dinamicamente le proprietà, la gestione degli eventi e così via.
- Aspetto personalizzabile tramite modelli definiti dall'utente, temi e stili.

## <a name="templates"></a>Modelli

Per il controllo FormView visualizzare il contenuto, è necessario creare modelli per le diverse parti del controllo. La maggior parte dei modelli sono facoltativi. Tuttavia, è necessario creare un modello per la modalità in cui è configurato il controllo. Ad esempio, un controllo FormView che supporta l'inserimento di record deve avere un modello di elemento di inserimento definito. La tabella seguente elenca i diversi modelli che è possibile creare.

| **Tipo di modello** | **Descrizione** |
| --- | --- |
| EditItemTemplate | Definisce il contenuto della riga di dati quando il controllo FormView è in modalità di modifica. Questo modello contiene in genere i controlli di input e i pulsanti di comando con cui l'utente può modificare un record esistente. |
| EmptyDataTemplate | Definisce il contenuto della riga di dati vuota visualizzata quando il controllo FormView è associato a un'origine dati che non contiene alcun record. Questo modello contiene in genere il contenuto per avvisare l'utente che l'origine dati non contiene alcun record. |
| FooterTemplate | Definisce il contenuto per la riga di piè di pagina. Questo modello contiene in genere qualsiasi contenuto aggiuntivo da visualizzare nella riga del piè di pagina. In alternativa, è possibile specificare semplicemente il testo da visualizzare nella riga del piè di pagina impostando la proprietà FooterText. |
| Modello HeaderTemplate | Definisce il contenuto per la riga di intestazione. Questo modello contiene in genere qualsiasi contenuto aggiuntivo da visualizzare nella riga di intestazione. In alternativa, è possibile specificare semplicemente il testo da visualizzare nella riga di intestazione impostando la proprietà HeaderText. |
| ItemTemplate | Definisce il contenuto della riga di dati quando il controllo FormView è in modalità sola lettura. Questo modello contiene in genere il contenuto per visualizzare i valori di un record esistente. |
| InsertItemTemplate | Definisce il contenuto della riga di dati quando il controllo FormView è in modalità di inserimento. Questo modello contiene in genere i controlli di input e i pulsanti di comando con cui l'utente può aggiungere un nuovo record. |
| PagerTemplate | Definisce il contenuto della riga di spostamento visualizzata quando è abilitata la funzionalità di paging (quando la proprietà di AllowPaging è impostata su **true**). Questo modello contiene in genere i controlli con cui l'utente possa passare a un altro record. |

I controlli di input nel modello di elemento di modifica e il modello di elemento di inserimento possono essere associati ai campi di un'origine dati usando un'espressione di associazione bidirezionale. In questo modo il controllo FormView a estrarre automaticamente i valori del controllo di input per un aggiornamento o operazione di inserimento. Le espressioni di associazione bidirezionale consentono anche i controlli di input in un modello di elemento di modifica per visualizzare automaticamente i valori di campo originale.

### <a name="binding-to-data"></a>Associazione a dati

Il controllo FormView può essere associato a un controllo origine dati (ad esempio **SqlDataSource**, AccessDataSource, **ObjectDataSource** e così via), o a qualsiasi origine dati che implementa il Interfaccia IEnumerable (ad esempio System.Data.DataView ArrayList e Hashtable). Per associare il controllo FormView per il tipo di origine di dati appropriato, usare uno dei metodi seguenti:

- Per associare un controllo origine dati, impostare la proprietà DataSourceID del controllo FormView al valore dell'ID del controllo origine dati. Controllo FormView associato automaticamente al controllo origine dati specificata e possa sfruttare le funzionalità del controllo per l'esecuzione di inserimento, aggiornamento, eliminazione e la funzionalità di paging. Questo è il metodo preferito per associare ai dati.
- Per eseguire l'associazione a un'origine dati che implementa il **IEnumerable** interfaccia, a livello di codice impostare la proprietà DataSource del controllo FormView nell'origine dati e quindi chiamare il metodo DataBind. Quando si usa questo metodo, il controllo FormView non fornisce incorporate di inserimento, aggiornamento, eliminazione e la funzionalità di paging. È necessario fornire questa funzionalità usando l'evento appropriato.

## <a name="data-operations"></a>Operazioni sui dati

Il controllo FormView fornisce numerose funzionalità incorporate che consentono all'utente di aggiornare, eliminare, inserire e spostarsi tra gli elementi del controllo. Quando il controllo FormView è associato a un controllo origine dati, il controllo FormView possa sfruttare le funzionalità del controllo di origine dati e fornire aggiornamenti automatici, l'eliminazione, inserimento e funzionalità di paging. Il controllo FormView può fornire supporto per update, delete, insert e operazioni di paging con altri tipi di origini dati. Tuttavia, è necessario fornire un gestore eventi appropriato con l'implementazione di queste operazioni.

Poiché il controllo FormView utilizza modelli, non fornisce un modo per generare automaticamente i pulsanti di comando per eseguire l'aggiornamento, eliminazione o operazioni di inserimento. Nel modello appropriato, è necessario includere manualmente questi pulsanti di comando. Il controllo FormView riconosce alcuni pulsanti con loro **CommandName** impostate sui valori specifici. Nella tabella seguente sono elencati i pulsanti di comando riconosciuto dal controllo FormView.

| **Pulsante** | **Valore di proprietà CommandName** | **Descrizione** |
| --- | --- | --- |
| Annulla | "Annulla" | Utilizzato in l'aggiornamento o inserimento di operazioni per annullare l'operazione e ignorare i valori immessi dall'utente. Il controllo FormView restituisce quindi la modalità specificata dalla proprietà DefaultMode. |
| Eliminare | "Delete" | Utilizzato nelle operazioni di eliminazione per eliminare record visualizzato dall'origine dati. Genera gli eventi ItemDeleting e ItemDeleted. |
| Edit | "Modifica" | Utilizzato nelle operazioni di aggiornamento per inserire il controllo FormView nella modalità di modifica. Il contenuto specificato nel **EditItemTemplate** proprietà viene visualizzata per la riga di dati. |
| INS | "Insert" | Utilizzato nelle operazioni di inserimento per tentare di inserire un nuovo record nell'origine dati usando i valori forniti dall'utente. Genera gli eventi ItemInserting e ItemInserted. |
| Nuovo | "Nuovo" | Utilizzato nelle operazioni di inserimento per inserire il controllo FormView nella modalità di inserimento. Il contenuto specificato nel **InsertItemTemplate** proprietà viene visualizzata per la riga di dati. |
| Pagina | "Pagina" | Utilizzato nelle operazioni di paging per rappresentare un pulsante nella riga di spostamento che esegue il paging. Per specificare l'operazione di spostamento, impostare il **CommandArgument** proprietà del pulsante su "Avanti", "Prev", "First", "Ultimo" o l'indice della pagina al quale passare. Genera gli eventi PageIndexChanging e PageIndexChanged. |
| Aggiorna | "Aggiornamento" | Utilizzato nelle operazioni di aggiornamento per tentare di aggiornare il record visualizzato nell'origine dati con i valori forniti dall'utente. Genera gli eventi ItemUpdating e ItemUpdated. |

A differenza di eliminazione sul pulsante (che consente di eliminare il record visualizzato immediatamente), quando si fa clic sul pulsante Modifica o nuovo, FormView controllo entra in edit o insert rispettivamente in modalità. In modalità di modifica, il contenuto di **EditItemTemplate** proprietà viene visualizzata per l'elemento di dati corrente. In genere, il modello di elemento di modifica è definito in modo che il pulsante di modifica viene sostituito con un aggiornamento e un pulsante Annulla. I controlli di input che sono appropriati per il tipo di dati del campo (ad esempio una casella di testo o una casella di controllo) anch'essa solitamente vengono visualizzati con un valore di campo per l'utente da modificare. Fare clic sul pulsante Update aggiorna il record nell'origine dati, mentre facendo clic sul pulsante di annullamento Annulla tutte le modifiche.

Analogamente, il contenuto presente nel **InsertItemTemplate** proprietà viene visualizzata per l'elemento di dati quando il controllo è in modalità di inserimento. Il modello di elemento di inserimento è in genere definito in modo che il nuovo pulsante viene sostituito con un'operazione di inserimento e un pulsante Annulla e controlli di input vuoti vengono visualizzati all'utente di immettere i valori per il nuovo record. Fare clic sul pulsante Insert inserisce il record nell'origine dati, mentre facendo clic sul pulsante di annullamento Annulla tutte le modifiche.

Il controllo FormView fornisce una funzionalità di paging, che consente all'utente di spostarsi tra gli altri record nell'origine dati. Quando abilitata, viene visualizzata una riga del pager nel controllo FormView che contiene i controlli di spostamento. Per abilitare il paging, impostare il **AllowPaging** proprietà **true**. È possibile personalizzare la riga di spostamento, impostando le proprietà degli oggetti contenuti nel PagerStyle e la proprietà PagerSettings. Invece di usare la riga di spostamento predefiniti dell'interfaccia utente, è possibile creare la propria interfaccia utente usando il **PagerTemplate** proprietà.

## <a name="customizing-the-user-interface"></a>Personalizzazione dell'interfaccia utente

È possibile personalizzare l'aspetto del controllo FormView impostando le proprietà di stile per le diverse parti del controllo. Nella tabella seguente sono elencate le proprietà di stile diverso.

| **Proprietà di stile** | **Descrizione** |
| --- | --- |
| EditRowStyle | Le impostazioni di stile della riga di dati quando il controllo FormView è in modalità di modifica. |
| EmptyDataRowStyle | Le impostazioni di stile per la riga di dati vuota visualizzata nel controllo FormView quando l'origine dati non contiene alcun record. |
| FooterStyle | Le impostazioni di stile per la riga di piè di pagina del controllo FormView. |
| HeaderStyle | Le impostazioni di stile per la riga di intestazione del controllo FormView. |
| InsertRowStyle | Le impostazioni di stile della riga di dati quando il controllo FormView è in modalità di inserimento. |
| PagerStyle | Le impostazioni di stile per la riga di spostamento visualizzato nel controllo FormView quando è abilitata la funzionalità di spostamento. |
| RowStyle | Le impostazioni di stile della riga di dati quando il controllo FormView è in modalità sola lettura. |

## <a name="events"></a>Eventi

Il controllo FormView fornisce diversi eventi che è possibile programmare. In questo modo è possibile eseguire una routine personalizzata ogni volta che si verifica un evento. La tabella seguente elenca gli eventi supportati dal controllo FormView.

| **Event** | **Descrizione** |
| --- | --- |
| ItemCommand | Si verifica quando viene selezionato un pulsante all'interno di un controllo FormView. Questo evento viene spesso utilizzato per eseguire un'attività quando viene selezionato un pulsante nel controllo. |
| ItemCreated | Si verifica dopo che tutti gli oggetti FormViewRow vengono creati nel controllo FormView. Questo evento viene spesso utilizzato per modificare i valori di un record prima che venga visualizzato. |
| ItemDeleted | Si verifica quando un pulsante di eliminazione (un pulsante con relativi **CommandName** proprietà è impostata su "Delete") viene fatto clic, ma dopo che il controllo FormView Elimina il record dall'origine dati. Questo evento viene spesso utilizzato per controllare i risultati dell'operazione di eliminazione. |
| ItemDeleting | Si verifica quando viene premuto un pulsante di eliminazione, ma prima di FormView controllo Elimina il record dall'origine dati. Questo evento viene spesso utilizzato per annullare l'operazione di eliminazione. |
| ItemInserted | Si verifica quando un pulsante Inserisci (un pulsante con relativi **CommandName** proprietà è impostata su "Insert") viene fatto clic, ma dopo che il controllo FormView inserisce il record. Questo evento viene spesso utilizzato per controllare i risultati dell'operazione di inserimento. |
| ItemInserting | Si verifica quando viene selezionato un pulsante di inserimento, ma prima di FormView ha inserito il record. Questo evento viene spesso utilizzato per annullare l'operazione di inserimento. |
| ItemUpdated | Si verifica quando un pulsante di aggiornamento (un pulsante con relativi **CommandName** proprietà è impostata su "Update") viene fatto clic, ma dopo che il controllo FormView aggiorni la riga. Questo evento viene spesso utilizzato per controllare i risultati dell'operazione di aggiornamento. |
| ItemUpdating | Si verifica quando un pulsante di aggiornamento, ma prima di FormView ha aggiornato il record. Questo evento viene spesso utilizzato per annullare l'operazione di aggiornamento. |
| ModeChanged | Si verifica dopo che il controllo FormView cambia modalità (alla modifica, inserimento o la modalità di sola lettura). Questo evento viene spesso utilizzato per eseguire un'attività quando il controllo FormView cambia modalità. |
| ModeChanging | Si verifica prima che il controllo FormView cambia modalità (alla modifica, inserimento o la modalità di sola lettura). Questo evento viene spesso utilizzato per annullare una modifica della modalità. |
| PageIndexChanged | Si verifica quando viene fatto clic su uno dei pulsanti di spostamento, ma dopo che il controllo FormView gestisce l'operazione di spostamento. Questo evento viene comunemente utilizzato quando è necessario eseguire un'attività dopo che l'utente passa a un altro record nel controllo. |
| PageIndexChanging | Si verifica quando viene fatto clic su uno dei pulsanti di spostamento, ma prima che il controllo FormView controllo gestisca l'operazione di spostamento. Questo evento viene spesso utilizzato per annullare l'operazione di spostamento. |

## <a name="detailsview"></a>Controllo DetailsView

Il controllo DetailsView consente di visualizzare un singolo record da un'origine dati in una tabella, in cui viene visualizzato ogni campo del record in una riga della tabella. Può essere utilizzato in combinazione con un controllo GridView per gli scenari di master-dettagli. Il controllo DetailsView supporta le funzionalità seguenti:

- Associazione a dati controlli di origine, ad esempio SqlDataSource.
- Funzionalità di inserimento incorporate.
- Predefinite, aggiornamento ed eliminazione di funzionalità.
- Funzionalità incorporate di paging.
- Accesso programmatico al modello a oggetti DetailsView per impostare dinamicamente le proprietà, la gestione degli eventi e così via.
- Aspetto personalizzabile tramite i temi e stili.

## <a name="row-fields"></a>Campi riga

Ogni riga di dati nel controllo DetailsView viene creato con la dichiarazione di un controllo del campo. Tipi di campi riga diversi determinano il comportamento delle righe nel controllo. Controlli di campo derivano da DataControlField. Nella tabella seguente elenca i tipi di campo altra riga che possono essere utilizzati.

| **Tipo di campo colonna** | **Descrizione** |
| --- | --- |
| BoundField | Visualizza il valore di un campo in un'origine dati come testo. |
| ButtonField | Consente di visualizzare un pulsante di comando nel controllo DetailsView. In questo modo è possibile visualizzare una riga con un controllo pulsante personalizzato, ad esempio un pulsante Aggiungi o Rimuovi. |
| CheckBoxField | Visualizza una casella di controllo nel controllo DetailsView. Questo tipo di campo riga viene comunemente usato per visualizzare campi con un valore booleano. |
| CommandField | Comando integrato consente di visualizzare i pulsanti per eseguire una modifica, inserimento o eliminazione di operazioni nel controllo DetailsView. |
| HyperLinkField | Visualizza il valore di un campo in un'origine dati come collegamento ipertestuale. Questo tipo di campo riga consente di associare un secondo campo all'URL del collegamento ipertestuale. |
| ImageField | Visualizza un'immagine nel controllo DetailsView. |
| TemplateField | Visualizza il contenuto definito dall'utente per una riga nel controllo DetailsView in base a un modello specificato. Questo tipo di campo riga consente di creare un campo riga personalizzato. |

Per impostazione predefinita, la proprietà AutoGenerateRows è impostata su **true**, che genera automaticamente un oggetto campo riga associati per ogni campo di tipo associabile nell'origine dati. I tipi validi associabili sono String, DateTime, Decimal, Guid e il set di tipi primitivi. Ogni campo viene quindi visualizzato in una riga come testo, nell'ordine in cui ogni campo viene visualizzato nell'origine dati.

Generare automaticamente le righe fornisce un modo semplice e rapido per visualizzare tutti i campi nel record. Tuttavia, per usare il controllo DetailsView disponibili controllo funzionalità avanzate che è necessario dichiarare in modo esplicito i campi riga da includere nel controllo DetailsView. Per dichiarare i campi riga, impostare innanzitutto le **AutoGenerateRows** proprietà **false**. Aggiungere quindi l'apertura e chiusura **&lt;campi&gt;** tag tra i tag di apertura e chiusura del controllo DetailsView. Infine, Elenca i campi riga che si desidera includere tra l'apertura e chiusura **&lt;campi&gt;** tag. I campi riga specificati vengono aggiunti alla raccolta di campi nell'ordine elencato. Il **campi** raccolta consente di gestire a livello di programmazione i campi riga nel controllo DetailsView.

> [!NOTE]
> I campi non vengono aggiunti alla raccolta di campi riga generati automaticamente.


## <a name="binding-to-data"></a>Associazione a dati

Il controllo DetailsView può essere associato a un controllo origine dati, ad esempio **SqlDataSource** o AccessDataSource, o a qualsiasi origine dati che implementa l'interfaccia IEnumerable, ad esempio System.Data.DataView, ArrayList e Hashtable.

Per associare il controllo DetailsView per il tipo di origine di dati appropriato, usare uno dei metodi seguenti:

- Per associare un controllo origine dati, impostare la proprietà DataSourceID di controllo DetailsView al valore dell'ID del controllo origine dati. Il controllo DetailsView viene associato automaticamente al controllo origine dati specificata. Questo è il metodo preferito per associare ai dati.
- Per eseguire l'associazione a un'origine dati che implementa il **IEnumerable** interfaccia, a livello di codice impostare la proprietà DataSource del controllo DetailsView all'origine dati e quindi chiamare il metodo DataBind.

## <a name="security"></a>Sicurezza

Questo controllo è utilizzabile per visualizzare l'input utente, che può includere uno script client non autorizzato. Controllare tutte le informazioni che viene inviate da un client per lo script eseguibile, istruzioni SQL o altro codice prima di visualizzarli nell'applicazione. ASP.NET fornisce una funzionalità di convalida richiesta di input per lo script di blocco e il codice HTML nell'input dell'utente.

## <a name="data-operations"></a>Operazioni sui dati

Il controllo DetailsView include funzionalità incorporate che consentono all'utente di aggiornare, eliminare, inserire e spostarsi tra gli elementi del controllo. Quando il controllo DetailsView è associato a un controllo origine dati, il controllo DetailsView possa sfruttare le funzionalità del controllo di origine dati e fornire aggiornamenti automatici, l'eliminazione, inserimento e funzionalità di paging.

Il controllo DetailsView può fornire supporto per update, delete, insert e operazioni di paging con altri tipi di origini dati. Tuttavia, è necessario fornire l'implementazione per queste operazioni in un gestore eventi appropriato.

Il controllo DetailsView può aggiungere automaticamente una **CommandField** campo riga con un pulsante di modifica, eliminazione o New impostando le proprietà AutoGenerateEditButton, AutoGenerateDeleteButton o AutoGenerateInsertButton a **true**, rispettivamente. A differenza dell'eliminazione pulsante (che consente di eliminare il record selezionato immediatamente), quando si fa clic sul pulsante Modifica o nuovo, DetailsView controllo entra in edit o insert modalità, rispettivamente. In modalità di modifica, il pulsante di modifica viene sostituito con un aggiornamento e un pulsante Annulla. I controlli di input che sono appropriati per il tipo di dati del campo (ad esempio una casella di testo o una casella di controllo) vengono visualizzati con un valore di campo per l'utente da modificare. Fare clic sul pulsante Update aggiorna il record nell'origine dati, mentre facendo clic sul pulsante di annullamento Annulla tutte le modifiche. Analogamente, in modalità di inserimento, il nuovo pulsante viene sostituito con un'operazione di inserimento e un pulsante Annulla e controlli di input vuoti vengono visualizzati all'utente di immettere i valori per il nuovo record.

Il controllo DetailsView fornisce una funzionalità di paging, che consente all'utente di spostarsi tra gli altri record nell'origine dati. Quando abilitata, i controlli di navigazione pagina vengono visualizzati in una riga di spostamento. Per abilitare il paging, impostare la proprietà AllowPaging su **true**. La riga di spostamento può essere personalizzata usando le proprietà PagerStyle e PagerSettings.

## <a name="customizing-the-user-interface"></a>Personalizzazione dell'interfaccia utente

È possibile personalizzare l'aspetto del controllo DetailsView impostando le proprietà di stile per le diverse parti del controllo. Nella tabella seguente sono elencate le proprietà di stile diverso.

| **Proprietà di stile** | **Descrizione** |
| --- | --- |
| AlternatingRowStyle | Le impostazioni di stile per le righe di dati alternativi nel controllo DetailsView. Quando questa proprietà è impostata, vengono visualizzate le righe di dati alterne tra le impostazioni di RowStyle e il **AlternatingRowStyle** impostazioni. |
| CommandRowStyle | Le impostazioni di stile per la riga che contiene i pulsanti di comando integrato nel controllo DetailsView. |
| EditRowStyle | Le impostazioni di stile per le righe di dati quando il controllo DetailsView è in modalità di modifica. |
| EmptyDataRowStyle | Le impostazioni di stile per la riga di dati vuota visualizzata nel controllo DetailsView quando l'origine dati non contiene alcun record. |
| FooterStyle | Le impostazioni di stile per la riga di piè di pagina del controllo DetailsView. |
| HeaderStyle | Le impostazioni di stile per la riga di intestazione del controllo DetailsView. |
| InsertRowStyle | Le impostazioni di stile per le righe di dati quando il controllo DetailsView è in modalità di inserimento. |
| PagerStyle | Le impostazioni di stile per la riga di spostamento del controllo DetailsView. |
| RowStyle | Le impostazioni di stile per le righe di dati nel controllo DetailsView. Quando il **AlternatingRowStyle** proprietà è impostata, vengono visualizzate le righe di dati alternando tra la **RowStyle** impostazioni e le **AlternatingRowStyle** impostazioni. |
| FieldHeaderStyle | Le impostazioni di stile per la colonna dell'intestazione del controllo DetailsView. |

## <a name="events"></a>Eventi

Il controllo DetailsView fornisce diversi eventi che è possibile programmare. In questo modo è possibile eseguire una routine personalizzata ogni volta che si verifica un evento. La tabella seguente elenca gli eventi supportati dal controllo DetailsView. Il controllo DetailsView eredita anche questi eventi dalle relative classi base: Data Binding, con associazione a dati, Disposed, Init, Load, PreRender e rendering.

| **Event** | **Descrizione** |
| --- | --- |
| ItemCommand | Si verifica quando viene selezionato un pulsante nel controllo DetailsView. |
| ItemCreated | Si verifica dopo che tutti gli oggetti DetailsViewRow vengono creati nel controllo DetailsView. Questo evento viene spesso utilizzato per modificare i valori di un record prima che venga visualizzato. |
| ItemDeleted | Si verifica quando viene selezionato un pulsante di eliminazione, ma dopo che il controllo DetailsView Elimina il record dall'origine dati. Questo evento viene spesso utilizzato per controllare i risultati dell'operazione di eliminazione. |
| ItemDeleting | Si verifica quando viene premuto un pulsante di eliminazione, ma prima di DetailsView controllo Elimina il record dall'origine dati. Questo evento viene spesso utilizzato per annullare l'operazione di eliminazione. |
| ItemInserted | Si verifica quando viene selezionato un pulsante di inserimento, ma dopo che il controllo DetailsView inserisce il record. Questo evento viene spesso utilizzato per controllare i risultati dell'operazione di inserimento. |
| ItemInserting | Si verifica quando viene selezionato un pulsante di inserimento, ma prima di DetailsView ha inserito il record. Questo evento viene spesso utilizzato per annullare l'operazione di inserimento. |
| ItemUpdated | Si verifica quando viene selezionato un pulsante di aggiornamento, ma dopo che il controllo DetailsView aggiorni la riga. Questo evento viene spesso utilizzato per controllare i risultati dell'operazione di aggiornamento. |
| ItemUpdating | Si verifica quando un pulsante di aggiornamento, ma prima di DetailsView ha aggiornato il record. Questo evento viene spesso utilizzato per annullare l'operazione di aggiornamento. |
| ModeChanged | Si verifica dopo che il controllo DetailsView cambia modalità (modalità di modifica, inserimento o sola lettura). Questo evento viene spesso utilizzato per eseguire un'attività quando il controllo DetailsView cambia modalità. |
| ModeChanging | Si verifica prima che il controllo DetailsView cambia modalità (modalità di modifica, inserimento o sola lettura). Questo evento viene spesso utilizzato per annullare una modifica della modalità. |
| PageIndexChanged | Si verifica quando viene fatto clic su uno dei pulsanti di spostamento, ma dopo che il controllo DetailsView gestisce l'operazione di spostamento. Questo evento viene comunemente utilizzato quando è necessario eseguire un'attività dopo che l'utente passa a un altro record nel controllo. |
| PageIndexChanging | Si verifica quando viene fatto clic su uno dei pulsanti di spostamento, ma prima che il controllo DetailsView controllo gestisca l'operazione di spostamento. Questo evento viene spesso utilizzato per annullare l'operazione di spostamento. |

## <a name="the-menu-control"></a>Il controllo Menu

Il controllo Menu in ASP.NET 2.0 è progettato per essere un sistema di spostamento completo. Può essere con associazione a dati con facilità alle origini dati gerarchici, ad esempio il controllo SiteMapDataSource.

Una struttura di controlli Menu può essere definita in modo dichiarativo o in modo dinamico ed è costituito da un singolo nodo radice e qualsiasi numero di nodi secondari. Il codice seguente definisce in modo dichiarativo un menu di scelta per il controllo Menu.

[!code-aspx[Main](data-bound-controls/samples/sample4.aspx)]

Nell'esempio precedente, il nodo Home. aspx è il nodo radice. Tutti gli altri nodi sono annidati all'interno del nodo radice a vari livelli.

Esistono due tipi di menu che esegue il rendering del controllo Menu; menu statici e dinamici menu. Menu statici sono costituiti da voci di menu sono sempre visibili. Menu dinamici sono costituiti da voci di menu che sono visibili solo quando l'utente posiziona il mouse su di essi con il mouse. I clienti possono confondere spesso menu statici con menu definiti in modo dichiarativo e menu dinamici con i menu con associazione a dati in fase di esecuzione. In effetti, i menu statici e dinamici non sono correlati al metodo di popolamento. I termini *statici* e *dinamico* fare riferimento solo a o meno il menu di scelta è visualizzato per impostazione predefinita in modo statico o unico visualizzato quando l'utente esegue alcune operazioni.

Il **StaticDisplayLevels** proprietà viene utilizzata per configurare il numero di livelli del menu è statici e di conseguenza visualizzate per impostazione predefinita. Nell'esempio precedente, impostando il **StaticDisplayLevels** proprietà su un valore pari a 2 provocherebbe il menu da visualizzare in modo statico il nodo iniziale, il nodo di musica e il nodo di film. Tutti gli altri nodi verrebbe visualizzate in modo dinamico quando l'utente passa sopra il nodo padre.

Il **MaximumDynamicDisplayLevels** proprietà consente di configurare il numero massimo di livelli dinamici è in grado di visualizzare il menu di scelta. Menu dinamici a un livello supera rispetto al valore specificato per il **MaximumDynamicDisplayLevels** proprietà vengono ignorate.

> [!NOTE]
> È quasi certo che possono verificarsi situazioni in cui non vengono visualizzati i menu per eseguire il rendering a causa della proprietà MaximumDynamicDisplayLevels. In questi casi, assicurarsi che la proprietà è impostata sufficientemente per consentire la visualizzazione del menu di clienti.


## <a name="data-binding-the-menu-control"></a>Data Binding del controllo Menu

Il controllo Menu può essere associato a qualsiasi origine dati gerarchica, come XMLDataSource o SiteMapDataSource. Il controllo SiteMapDataSource è il metodo più comunemente utilizzato per il data binding a un controllo Menu perché che alimenta il file Web. sitemap e lo schema fornisce una API nota al controllo Menu. Nell'elenco di seguito è riportato un semplice file Web. sitemap.

[!code-xml[Main](data-bound-controls/samples/sample5.xml)]

Si noti che esiste solo un siteMapNode elemento radice, in questo caso, l'elemento iniziale. Alcuni attributi possono essere configurati per ogni siteMapNode. Gli attributi più comunemente usati sono:

- **URL** specifica l'URL da visualizzare quando un utente fa clic sulla voce di menu. Se questo attributo non è presente, il nodo semplicemente inserirà nuovamente quando si fa clic su.
- **titolo** specifica il testo che viene visualizzato sulla voce di menu.
- **Descrizione** usata come documentazione per il nodo. Viene visualizzato anche come descrizione comando quando si passa il mouse sul nodo.
- **siteMapFile** consente la funzionalità sitemaps annidati. Questo attributo deve puntare a un file di mappa del sito ASP.NET ben formato.
- **ruoli** consente l'aspetto di un nodo per essere controllato tramite la rimozione della protezione ASP.NET.

Si noti che anche se questi attributi sono facoltativi, il comportamento del menu di scelta potrebbe non essere quelli previsti se non sono specificati. Ad esempio, se il *url* attributo è specificato ma il *descrizione* attributo non è presente, il nodo non sarà visibile e non vi sarà alcuna possibilità di passare all'URL specificato.

## <a name="controlling-a-menus-operation"></a>Controllo di un'operazione di menu

Esistono diverse proprietà che influiscono sul funzionamento di un controllo Menu ASP.NET; il **orientamento** proprietà, il **DisappearAfter** proprietà, il **StaticItemFormatString** proprietà e il **StaticPopoutImageUrl**proprietà sono solo alcuni di questi.

- Il **orientamento** può essere impostato su *orizzontale* oppure *verticale* e controlla se le voci di menu statico sono disposti orizzontalmente in una riga o in verticale e impilate tra loro. Questa proprietà non influisce sulle menu dinamici.
- Il **DisappearAfter** proprietà consente di configurare quanto tempo un menu dinamico deve rimanere visibile dopo che il mouse è stato spostato dalla. Il valore è specificato in millisecondi e il valore predefinito è 500. Impostando questa proprietà su un valore pari a -1 causerà il menu a mai rimossi automaticamente. In tal caso, il menu scompare solo quando l'utente fa clic all'esterno di menu.
- Il **StaticItemFormatString** proprietà rende più semplice mantenere coerente e sono presenti nel sistema di menu. Quando si specifica questa proprietà, *{0}* companydomain sostituzione della descrizione che viene visualizzato nell'origine dati. Ad esempio, per avere la voce di menu da esercizio 1 say, visitare la nostra pagina prodotti e così via, si specificherà visita nostro {0} pagina per il StaticItemFormatString. In fase di runtime ASP.NET sostituirà tutte le occorrenze di {0} con la descrizione della voce di menu corretta.
- Il **StaticPopoutImageUrl** proprietà consente di specificare l'immagine che viene usato per indicare che un determinato menu nodo presenta nodi figlio che sono accessibili, passare il mouse su di esso. Menu dinamici continueranno a usare l'immagine predefinita.

## <a name="templated-menu-controls"></a>Controlli Menu basati su modelli

Il controllo Menu è un controllo basato su modelli e consente due modelli di elemento diversi; il StaticItemTemplate e il DynamicItemTemplate. Utilizzando questi modelli, è possibile facilmente aggiungere i controlli server o nei controlli utente per i menu.

Per modificare i modelli in Visual Studio .NET, fare clic sul pulsante di Smart Tag del menu e scegliere Modifica modelli. È quindi possibile scegliere tra la modifica di StaticItemTemplate o il DynamicItemTemplate.

I controlli aggiunti al StaticItemTemplate apparirà del menu statico quando il caricamento della pagina. I controlli aggiunti al DynamicItemTemplate verranno visualizzato in tutti i menu a comparsa.

## <a name="menu-events"></a>Eventi di menu

Il controllo Menu dispone di due eventi che sono univoci per esso. il **MenuItemClicked** e il **MenuItemDatabound** evento.

L'evento MenuItemClicked viene generato quando si fa clic su una voce di menu. L'evento MenuItemDatabound viene generato quando una voce di menu è associato a dati. Il **MenuEventArgs mostra** che viene passato all'evento gestore fornisce l'accesso alla voce di menu tramite la proprietà dell'elemento.

## <a name="controlling-a-menus-appearance"></a>Controllo un aspetto di menu

È inoltre possibile modificare l'aspetto di un controllo menu utilizzando uno o più dei molti stili disponibili per i menu Formato. Tra questi vi sono **StaticMenuStyle**, **DynamicMenuStyle**, **DynamicMenuItemStyle**, **DynamicSelectedStyle**e **DynamicHoverStyle**. Queste proprietà vengono configurate mediante una stringa in formato HTML standard. Ad esempio, di seguito influirà lo stile per i menu dinamici.

[!code-aspx[Main](data-bound-controls/samples/sample6.aspx)]

> [!NOTE]
> Se si usa uno qualsiasi degli stili del passaggio del mouse, è necessario aggiungere un &lt;head&gt; elemento alla pagina con il *runat* elemento impostato su *server*.


I controlli menu supportano anche l'utilizzo dei temi ASP.NET 2.0.

## <a name="the-treeview-control"></a>Il controllo TreeView

Il controllo TreeView Visualizza i dati in una struttura ad albero. Come con il controllo Menu, può essere facilmente i dati associati a qualsiasi origine dati gerarchica, come SiteMapDataSource.

La prima domanda che i clienti possono porre sul controllo TreeView di ASP.NET 2.0 è o meno è correlato ad WebControl IE TreeView che era disponibile per ASP.NET 1.x. Non è. Il controllo TreeView di ASP.NET 2.0 è stata scritta da zero e offre un miglioramento significativo rispetto WebControl IE TreeView precedentemente disponibile.

Non analizzerò in modo dettagliato su come associare un controllo TreeView a una mappa del sito quando viene eseguita in esattamente come il controllo Menu. Tuttavia, il controllo TreeView presenta alcune differenze distinti nel modo in cui opera.

Per impostazione predefinita, un controllo TreeView viene visualizzata completamente espanso. Per modificare il livello di espansione durante il caricamento iniziale, modificare il **ExpandDepth** proprietà del controllo. Ciò è particolarmente importante nei casi in cui la visualizzazione albero con associazione a dati al momento espandendo i nodi particolari.

## <a name="databinding-the-treeview-control"></a>Data Binding del controllo TreeView

A differenza del controllo Menu, TreeView si presta bene a gestire grandi quantità di dati. Pertanto, oltre ai data binding a un controllo SiteMapDataSource o XMLDataSource, visualizzazione albero è spesso i dati associati a un set di dati o altri dati relazionali. Nei casi in cui è associato il controllo TreeView per grandi quantità di dati, è consigliabile associare solo ai dati che è effettivamente visibili nel controllo. È possibile associare dati a dati aggiuntivi come nodi di TreeView vengono espanse.

In questi casi, il **PopulateOnDemand** proprietà di visualizzazione albero dovrebbe essere impostata su *true*. Sarà quindi necessario fornire un'implementazione per il **TreeNodePopulate** (metodo).

## <a name="data-binding-without-postback"></a>Data Binding senza PostBack

Si noti che quando si espande un nodo dell'esempio precedente per la prima volta, la pagina viene eseguito il postback e viene aggiornato. Memorizzate non è un problema in questo esempio, ma si possono immaginare che potrebbe essere in un ambiente di produzione con una grande quantità di dati. Uno scenario migliore sarebbe uno in cui il controllo TreeView sarebbe comunque in modo dinamico popolare i relativi nodi, ma senza una richiesta post il postback al server.

Impostando il **PopulateNodesFromClient** e il **PopulateOnDemand** proprietà su true, il controllo ASP.NET TreeView verrà popolato in modo dinamico i nodi senza un postback. Quando si espande il nodo padre, viene eseguita una richiesta XMLHttp dal client e viene generato l'evento OnTreeNodePopulate. Il server risponde con un'isola di dati XML che viene quindi usato per i dati associati ai nodi figlio.

ASP.NET crea dinamicamente il codice client che implementa questa funzionalità. Il &lt;script&gt; vengono generati i tag che contengono lo script che punta a un file AXD. Ad esempio, il codice seguente mostra i collegamenti di script per il codice di script che genera la richiesta XMLHttp.

[!code-html[Main](data-bound-controls/samples/sample7.html)]

Se si passa il file AXD sopra nel browser e aprirlo, noterete il codice che implementa la richiesta XMLHttp. Questo metodo evita ai clienti di modificare il file di script.

## <a name="controlling-the-operation-of-the-treeview-control"></a>Controllo del funzionamento di un controllo TreeView

Il controllo TreeView include diverse proprietà che influiscono sul funzionamento del controllo. Le proprietà più evidenti sono la **ShowCheckBoxes**, **ShowExpandCollapse**, e **ShowLines**.

Il **ShowCheckBoxes** proprietà influisce sulla se nei nodi viene visualizzata una casella di controllo quando sottoposto a rendering. I valori validi per questa proprietà sono **None**, **radice**, **padre**, **foglia**, e **tutti**. Queste influiscono sul controllo TreeView nel modo seguente:

| **Valore della proprietà** | **Effetto** |
| --- | --- |
| nessuno | Le caselle di controllo non vengono visualizzati in tutti i nodi. Questa è l'impostazione predefinita. |
| Radice | Una casella di controllo viene visualizzato solo nel nodo radice. |
| Padre | Una casella di controllo viene visualizzata solo su tali nodi contenenti nodi figlio. I nodi figlio possono essere nodi padre o nodi foglia. |
| Foglia | Una casella di controllo viene visualizzata solo per i nodi che non hanno nodi figlio. |
| Tutti | Viene visualizzata una casella di controllo su tutti i nodi. |

Quando vengono usate le caselle di controllo, il **CheckedNodes** proprietà restituisce una raccolta di nodi di TreeView che vengono controllati durante il postback.

Il **ShowExpandCollapse** proprietà controlla l'aspetto dell'immagine espandere/comprimere accanto ai nodi padre e radice. Se questa proprietà è impostata su **false**, nodi di TreeView sono Espandi/compressi facendo clic sul collegamento e vengono visualizzati come collegamenti ipertestuali.

Il **ShowLines** proprietà controlla se le righe vengono visualizzate la connessione di nodi padre ai nodi figlio. Quando **false** (impostazione predefinita), non vengono visualizzate linee. Quando **true**, il controllo TreeView utilizzerà le immagini di righe nella cartella specificata per il **LineImagesFolder** proprietà.

Per personalizzare l'aspetto delle linee di TreeView, Visual Studio .NET 2005 include uno strumento di progettazione di riga. È possibile accedere a questo strumento usando il pulsante di Smart Tag del controllo TreeView come di seguito.


![](data-bound-controls/_static/image1.jpg)

**Figura 1**


Quando seleziona il **Personalizza immagini linea** opzione di menu, verrà avviato lo strumento di progettazione di riga che consente di configurare l'aspetto delle linee di TreeView.

## <a name="treeview-events"></a>Eventi di TreeView

Il controllo TreeView presenta gli eventi univoci seguenti:

- SelectedNodeChanged si verifica quando viene selezionato un nodo in base al **SelectAction** proprietà.
- TreeNodeCheckChanged si verifica quando viene modificato uno stato checkboxs nodi.
- Eventi TreeNodeExpanded si verifica quando si espande un nodo in base al **SelectAction** proprietà.
- TreeNodeCollapsed si verifica quando si comprime un nodo.
- TreeNodeDataBound si verifica quando un nodo di dati associato.
- TreeNodePopulate si verifica quando un nodo viene popolato.

Il **SelectAction** proprietà consente di configurare quale evento viene generato quando si seleziona un nodo. La proprietà SelectAction fornisce le azioni seguenti:

- TreeNodeSelectAction. Expand genera eventi TreeNodeExpanded quando si seleziona il nodo.
- TreeNodeSelectAction non genera alcun evento quando si seleziona il nodo.
- TreeNodeSelectAction genera l'evento SelectedNodeChanged quando si seleziona il nodo.
- Su TreeNodeSelectAction. SelectExpand genera l'evento SelectedNodeChanged sia l'evento di eventi TreeNodeExpanded quando si seleziona il nodo.

## <a name="controlling-appearance-with-styles"></a>Controllo dell'aspetto degli stili

Il controllo TreeView fornisce molte proprietà per controllare l'aspetto del controllo con stili di visualizzazione. Sono disponibili le proprietà seguenti.

| **Nome della proprietà** | **Controlli** |
| --- | --- |
| HoverNodeStyle | Determina lo stile di nodi quando si passa il mouse su di essi. |
| LeafNodeStyle | Determina lo stile di nodi foglia. |
| NodeStyle | Determina lo stile per tutti i nodi. Stili dei nodi specifici (ad esempio LeafNodeStyle) eseguire l'override di questo stile. |
| ParentNodeStyle | Determina lo stile per tutti i nodi padre. |
| RootNodeStyle | Determina lo stile per il nodo radice. |
| SelectedNodeStyle | Determina lo stile per il nodo selezionato. |

Ognuna di queste proprietà è di sola lettura. Tuttavia, questi saranno attivati ogni restituzione del controllo un **TreeNodeStyle** oggetto e le proprietà di tale oggetto possono essere modificato usando la *sottoproprietà di proprietà* formato. Ad esempio, per impostare il **ForeColor** proprietà delle **SelectedNodeStyle**, è necessario utilizzare la sintassi seguente:

[!code-aspx[Main](data-bound-controls/samples/sample8.aspx)]

Si noti che il tag riportato sopra non è chiuso. Ciò avviene perché quando si usa la sintassi dichiarativa illustrata di seguito, è necessario includere i nodi di TreeView anche il codice HTML.

Le proprietà di stile possono anche essere specificate nel codice utilizzando il *Property. Subproperty* formato. Ad esempio, per impostare il **ForeColor** proprietà delle **RootNodeStyle** nel codice, si potrebbe usare la sintassi seguente:

[!code-csharp[Main](data-bound-controls/samples/sample9.cs)]

> [!NOTE]
> Per un elenco completo delle diverse proprietà di stile, vedere la documentazione MSDN sull'oggetto TreeNodeStyle.


## <a name="the-sitemappath-control"></a>Il controllo SiteMapPath

Il controllo SiteMapPath fornisce un controllo di navigazione Pane percorso per sviluppatori ASP.NET. Come gli altri controlli di navigazione, può essere facilmente i dati associati a dati gerarchici origini, ad esempio il controllo SiteMapDataSource o XmlDataSource.

Un controllo SiteMapPath è costituito da oggetti SiteMapNodeItem. Esistono tre tipi di nodi. il nodo radice, nodi padre e il nodo corrente. Il nodo radice è il nodo in cima alla struttura gerarchica. Il nodo corrente rappresenta la pagina corrente. Tutti gli altri nodi sono nodi padre.

## <a name="controlling-the-operation-of-the-sitemappath-control"></a>Controllo del funzionamento del controllo SiteMapPath

Le proprietà che controllano il funzionamento del controllo SiteMapPath sono come segue:

| **Property** | **Descrizione della proprietà** |
| --- | --- |
| ParentLevelsDisplayed | Controllare il numero di nodi padre è visualizzato. Il valore predefinito è -1 non pone limiti al numero di nodi padre visualizzati. |
| PathDirection | Controlla la direzione del controllo SiteMapPath. I valori validi sono RootToCurrent (impostazione predefinita) e CurrentToRoot. |
| PathSeparator | Stringa che controlla il carattere che separa i nodi in un controllo SiteMapPath. Il valore predefinito è:. |
| RenderCurrentNodeAsLink | Valore booleano che controlla se il nodo corrente viene eseguito il rendering come collegamento. Il valore predefinito è False. |
| SkipLinkText | Fornisce assistenza per accesso facilitato quando la pagina verrà visualizzata dai lettori dello schermo. Questa proprietà consente ai lettori dello schermo di ignorare il controllo SiteMapPath. Per disabilitare questa funzionalità, impostare la proprietà su String. Empty. |

## <a name="templated-sitemappath-controls"></a>Controlli SiteMapPath basato su modelli

Il SiteMapControl è un controllo basato su modelli e di conseguenza, è possibile definire modelli diversi per l'utilizzo nella visualizzazione del controllo. Per modificare i modelli in un controllo SiteMapPath, fare clic sul pulsante di Smart Tag del controllo e scegliere Modifica modelli dal menu. Verrà visualizzata nel menu SiteMapTasks come illustrato di seguito in cui è possibile scegliere tra i diversi modelli disponibili.


![](data-bound-controls/_static/image2.jpg)

**Figura 2**


Il **NodeTemplate** modello fa riferimento a tutti i nodi il controllo SiteMapPath. Se il nodo è un nodo radice o il nodo corrente e un **RootNodeTemplate** oppure **CurrentNodeTemplate** è configurato, il NodeTemplate viene sottoposto a override.

## <a name="sitemappath-events"></a>Eventi SiteMapPath

Il controllo SiteMapPath dispone di due eventi che non sono derivati dalla classe Control; il **ItemCreated** evento e il **ItemDataBound** evento. L'evento ItemCreated viene generato quando viene creato un elemento SiteMapPath. ItemDataBound viene generato quando viene chiamato il metodo DataBind durante l'associazione di dati di un nodo SiteMapPath. Oggetto **SiteMapNodeItemEventArgs** oggetto fornisce l'accesso al SiteMapNodeItem specifico tramite la proprietà dell'elemento.

## <a name="controlling-appearance-with-styles"></a>Controllo dell'aspetto degli stili

Gli stili seguenti sono disponibili per la formattazione di un controllo SiteMapPath.

| **Nome della proprietà** | **Controlli** |
| --- | --- |
| CurrentNodeStyle | Determina lo stile del testo per il nodo corrente. |
| RootNodeStyle | Determina lo stile del testo per il nodo radice. |
| NodeStyle | Determina lo stile del testo per tutti i nodi, presupponendo che non si applica un CurrentNodeStyle o RootNodeStyle. |

La proprietà NodeStyle viene sostituita la CurrentNodeStyle o il RootNodeStyle. Ognuna di queste proprietà è di sola lettura e restituisce un **stile** oggetto. Per modificare l'aspetto di un nodo usando una di queste proprietà, è necessario impostare le proprietà dell'oggetto stile restituito. Ad esempio, il codice seguente modifica la proprietà di colore di primo piano del nodo corrente.

[!code-aspx[Main](data-bound-controls/samples/sample10.aspx)]

La proprietà può essere applicata anche a livello di codice come indicato di seguito:

[!code-csharp[Main](data-bound-controls/samples/sample11.cs)]

> [!NOTE]
> Se viene applicato un modello, lo stile non essere applicato.


## <a name="lab-1-configuring-an-aspnet-menu-control"></a>Laboratorio 1: Configurazione di un controllo Menu ASP.NET

1. Creare un nuovo sito Web.
2. Aggiungere un file di mappa del sito selezionando File, nuovo, File e scegliendo mappa del sito dall'elenco dei modelli di file.
3. Aprire la mappa del sito (Web. sitemap per impostazione predefinita) e modificarlo in modo che risulti come di seguito. Le pagine a cui si crea un collegamento nel file di mappa del sito non esistessero nella realtà, ma che potrebbe non costituire un problema per questo esercizio.

    [!code-xml[Main](data-bound-controls/samples/sample12.xml)]
4. Aprire il Web form predefinito in visualizzazione progettazione.
5. Dalla sezione di spostamento della casella degli strumenti, aggiungere un nuovo controllo Menu alla pagina.
6. Dalla sezione dati della casella degli strumenti, aggiungere un controllo SiteMapDataSource nuovo. Il controllo SiteMapDataSource userà automaticamente il file Web. sitemap nel sito. (Il file Web. sitemap *necessario* trovarsi nella cartella radice del sito.)
7. Fare clic sul controllo Menu e quindi fare clic sul pulsante di Smart Tag per visualizzare la finestra di dialogo attività di Menu.
8. Nell'elenco a discesa Scegli origine dati, selezionare SiteMapDataSource1.
9. Fare clic sul collegamento di formattazione automatica e scegliere un formato per il menu di scelta.
10. Nel riquadro Proprietà impostare il **StaticDisplayLevels** proprietà su 2. Il controllo Menu dovrebbe ora visualizzare il nodo iniziale, i prodotti e servizi nella finestra di progettazione.
11. Esplorare la pagina nel browser per usare il menu di scelta. (Perché non esistono effettivamente le pagine di cui è stato aggiunto alla mappa del sito, si verrà visualizzato un errore quando si prova a passare a essi.)

Provare a modificare il StaticDisplayLevels e le proprietà MaximumDynamicDisplayLevels e vedere come cambia la modalità di rendering nel menu.

## <a name="lab-2-dynamically-binding-a-treeview-control"></a>Laboratorio 2: Associazione dinamica di un controllo TreeView

Questo esercizio si presuppone che si disponga di SQL Server in esecuzione in locale e che il database Northwind è presente nell'istanza di SQL Server. Se non vengono soddisfatte queste condizioni, modificare la stringa di connessione nell'esempio. Si noti che è necessario anche specificare l'autenticazione di SQL Server invece di una connessione trusted.

1. Creare un nuovo sito Web.
2. Passare alla visualizzazione codice per default. aspx e sostituire tutto il codice con il codice riportato di seguito. 

    [!code-aspx[Main](data-bound-controls/samples/sample13.aspx)]
3. Salvare la pagina come treeview.aspx.
4. Esplorare la pagina.
5. Quando la pagina viene visualizzata prima di tutto, visualizzare l'origine della pagina nel browser. Si noti che solo i nodi visibili sono stati inviati al client.
6. Fare clic sul segno più accanto ai nodi.
7. Visualizza origine in più la pagina. Si noti che i nodi appena visualizzati ora sono presenti.

## <a name="lab-3-details-view-and-editing-data-using-a-gridview-and-detailsview"></a>Laboratorio 3: Descrive in dettaglio visualizzazione e modifica dei dati tramite un controllo GridView e DetailsView

1. Creare un nuovo sito Web.
2. Aggiungere un nuovo file Web. config per il sito Web.
3. Aggiungere una stringa di connessione nel file Web. config come illustrato di seguito: 

    [!code-xml[Main](data-bound-controls/samples/sample14.xml)]

    > [!NOTE]
    > Potrebbe essere necessario modificare la stringa di connessione in base all'ambiente.
4. Salvare e chiudere il file Web.config.
5. Aprire Default. aspx e aggiungere un nuovo controllo SqlDataSource.
6. Modificare l'ID del controllo SqlDataSource **prodotti**.
7. Nel **Attività SqlDataSource** menu, fare clic su **Configura origine dati**.
8. Selezionare **Northwind** nell'elenco a discesa connessione e fare clic su Avanti.
9. Selezionare **prodotti** dal **nome** elenco a discesa e selezionare il **ProductID**, **ProductName**, **UnitPrice**, e **UnitsInStock** caselle di controllo come illustrato di seguito. 

![](data-bound-controls/_static/image3.jpg)

    **Figure 3**
10. Scegliere **Avanti**.
11. Scegliere **Fine**.
12. Passare alla visualizzazione origine ed esaminare il codice generato. Si noti che il **SelectCommand**, **DeleteCommand**, **InsertCommand**, e **UpdateCommand** che sono stati aggiunti e SqlDataSource controllo. Si notino anche i parametri che sono stati aggiunti.
13. Passare alla visualizzazione progettazione e aggiungere un nuovo controllo GridView alla pagina.
14. Selezionare **prodotti** dalle **Scegli origine dati** elenco a discesa.
15. Controllare **Abilita Paging** e **Abilita selezione** come illustrato di seguito. 

![](data-bound-controls/_static/image4.jpg)

    **Figure 4**
16. Fare clic sui **Modifica colonne** collegamento e assicurarsi che **genera campi automaticamente** sia selezionata.
17. Fare clic su **OK**.
18. Selezionata il controllo GridView, fare clic sul pulsante accanto al **DataKeyNames** proprietà nel riquadro proprietà.
19. Selezionare **ProductID** dalle **i campi dati disponibili** elenco e fare clic sui **&gt;** pulsante per aggiungerlo.
20. Fare clic su OK.
21. Aggiungere un nuovo controllo SqlDataSource per la pagina.
22. Modificare l'ID del controllo SqlDataSource **dettagli**.
23. Scegliere dal menu Attività SqlDataSource **Configura origine dati**.
24. Scegli **Northwind** nell'elenco a discesa e fare clic **successivo**.
25. Selezionare <strong>prodotti</strong> dal <strong>nome</strong> elenco a discesa e controllare il <strong> \</ strong > * nella casella di controllo la <strong>colonne</strong> listbox.
26. Scegliere il **in cui** pulsante.
27. Selezionare **ProductID** dalle **colonna** elenco a discesa.
28. Selezionare **=** nell'elenco a discesa operatore.
29. Selezionare **controllo** dalle **origine** elenco a discesa.
30. Selezionare **GridView1** dalle **ID controllo** elenco a discesa.
31. Scegliere il **Add** pulsante per aggiungere la clausola WHERE.
32. Fare clic su **OK**.
33. Fare clic sui **avanzate** pulsante e controllare le **genera istruzioni INSERT, UPDATE e DELETE** casella di controllo.
34. Fare clic su **OK**.
35. Fare clic su **successivo** e fare clic su **fine**.
36. Aggiungere un controllo DetailsView alla pagina.
37. Nel **Scegli origine dati** elenco a discesa, scegliere **dettagli**.
38. Verificare i **Abilita modifica** casella di controllo come illustrato di seguito. 

![](data-bound-controls/_static/image1.gif)

    **Figure 5**
39. Salvare la pagina e individuare default. aspx.
40. Scegliere il **seleziona** collegamento accanto a record diversi per visualizzare automaticamente l'aggiornamento di DetailsView.
41. Scegliere il **modifica** collegamento nel controllo DetailsView.
42. Apportare una modifica al record e fare clic su **Update**.
