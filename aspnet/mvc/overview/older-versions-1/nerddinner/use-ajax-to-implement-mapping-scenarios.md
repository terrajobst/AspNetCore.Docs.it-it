---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
title: Usare AJAX per implementare scenari di Mapping | Microsoft Docs
author: microsoft
description: Passaggio 11 viene illustrato come integrare l'applicazione di NerdDinner, consentendo agli utenti che sono la creazione, modifica o la visualizzazione dinners per visualizzare i l supporto AJAX mapping...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: f731990a-0a81-4d62-81df-87d676cdedd6
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
msc.type: authoredcontent
ms.openlocfilehash: f7de23ca46e6dc00fe8075e28068a8b3f95d02cd
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826328"
---
<a name="use-ajax-to-implement-mapping-scenarios"></a>Usare AJAX per implementare scenari di Mapping
====================
da [Microsoft](https://github.com/microsoft)

[Scaricare PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Questo è il passaggio 11 di una liberazione [esercitazione sull'applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che si interromperanno-dettaglio come compilare una piccola, ma completa, applicazione web con ASP.NET MVC 1.
> 
> Passaggio 11 viene illustrato come integrare supporto del mapping di AJAX nella nostra applicazione NerdDinner, consentendo agli utenti che sono la creazione, modifica o la visualizzazione dinners per visualizzare graficamente la posizione della cena.
> 
> Se si usa ASP.NET MVC 3, è consigliabile seguire le [Guida introduttiva con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) oppure [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) esercitazioni.


## <a name="nerddinner-step-11-integrating-an-ajax-map"></a>NerdDinner passaggio 11: L'integrazione di una mappa di AJAX

Si renderà a questo punto l'applicazione di un piccolo interessanti visivamente più grazie all'integrazione di supporto del mapping di AJAX. Ciò consentirà agli utenti che sono la creazione, modifica o la visualizzazione dinners per visualizzare graficamente la posizione della cena.

### <a name="creating-a-map-partial-view"></a>Creazione di una visualizzazione parziale della mappa

Si intende utilizzare funzionalità di mapping in diverse posizioni all'interno dell'applicazione. Per mantenere il codice di prova è sarà incapsulano la funzionalità mappa comuni all'interno di un singolo modello parziale che è possibile usare nuovamente tra più azioni del controller e visualizzazioni. Illustreremo denominare questa visualizzazione parziale "map.ascx" e crearla all'interno della directory \Views\Dinners.

È possibile creare il map.ascx parziale facendo clic sulla directory \Views\Dinners e scegliendo Aggiungi -&gt;visualizzare comando di menu. Verrà assegnare un nome di visualizzazione "Map.ascx", archiviarlo come visualizzazione parziale e indicare che si intende passare una classe di modelli fortemente tipizzati "Dinner":

![](use-ajax-to-implement-mapping-scenarios/_static/image1.png)

Quando si fa clic sul pulsante "Aggiungi" verrà creato il modello parziale. Verrà quindi aggiornato il file Map.ascx per disporre il contenuto seguente:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample1.aspx)]

Il primo &lt;script&gt; fanno riferimento a punti nella raccolta di mapping di Microsoft Virtual Earth 6.2. La seconda &lt;script&gt; fa riferimento a un file map.js che verrà creato a breve verrà incapsulata la logica di mapping più comune di Javascript di riferimento. Il &lt;div id = "theMap"&gt; elemento è il contenitore HTML utilizzato per ospitare la mappa Virtual Earth.

Abbiamo quindi incorporato &lt;script&gt; blocco che contiene due funzioni di JavaScript specifiche di questa visualizzazione. La prima funzione Usa jQuery per wireup una funzione che viene eseguita quando la pagina è pronta per l'esecuzione dello script lato client. Chiama una funzione helper LoadMap() che verranno definiti all'interno il file di script Map.js per caricare il controllo mappa di virtual earth. La seconda funzione è un gestore di evento di callback che viene aggiunta una puntina alla mappa che identifica una posizione.

Si noti come viene usato un server-side &lt;% = %&gt; all'interno del blocco di script sul lato client per incorporare la latitudine e longitudine della cena si vuole eseguire il mapping in JavaScript. Questa è una tecnica utile per restituire i valori dinamici che possono essere usati da script sul lato client (senza richiedere una separata chiamata AJAX al server per recuperare i valori, che rende più veloce). Il &lt;% = %&gt; blocchi verranno eseguita quando viene eseguito il rendering della visualizzazione nel server, e pertanto l'output del codice HTML semplicemente finirà con valori di JavaScript incorporati (ad esempio: latitudine var = 47.64312;).

### <a name="creating-a-mapjs-utility-library"></a>Creazione di una libreria di utilità Map.js

Creare ora il file Map.js che è possibile usare per incapsulare la funzionalità di JavaScript per la mappa (e implementare i metodi LoadMap e LoadPin precedenti). È possibile eseguire questa operazione facendo clic sulla directory \Scripts all'interno del progetto e quindi scegliere il "Add -&gt;nuovo elemento" comandi di menu, selezionare l'elemento JScript e denominarlo "Map.js".

Di seguito è riportato il codice JavaScript verrà aggiunto al file Map.js che interagirà con Virtual Earth per visualizzare la mappa e aggiungervi i pin di percorsi per i nostri dinners:

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample2.js)]

### <a name="integrating-the-map-with-create-and-edit-forms"></a>L'integrazione di mappa con creazione e modifica di moduli

Il supporto della mappa verrà ora integrata con la creazione e modifica gli scenari esistenti. La buona notizia è che si tratta di attività piuttosto semplice e non richiede di modificare il codice del Controller. Poiché la creazione e modifica di viste condividono una visualizzazione parziale "DinnerForm" comune per implementare l'interfaccia utente del form dinner, è possibile aggiungere la mappa in un'unica posizione e hanno entrambi gli scenari di creazione e modifica di usarlo.

Abbiamo bisogno di cose da fare è sufficiente aprire la visualizzazione parziale \Views\Dinners\DinnerForm.ascx e aggiornare in modo da includere la nuova mappa parziale. Di seguito è riportato il DinnerForm aggiornato aspetto dopo aver aggiunto la mappa (Nota: gli elementi di form HTML vengono omessi dal frammento di codice seguente per brevità):

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample3.aspx)]

Il DinnerForm parziale sopra accetta un oggetto di tipo "DinnerFormViewModel" come tipo di modello (perché è necessario sia un oggetto Dinner, nonché un SelectList per popolare il controllo dropdownlist dei paesi). La mappa parziale deve solo un oggetto di tipo "Dinner" come tipo di modello e quindi, quando si esegue il rendering della mappa parziale viene passato semplicemente la proprietà secondaria Dinner di DinnerFormViewModel ad esso:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample4.aspx)]

La funzione JavaScript è stato aggiunto per il componente aggiuntivo jQuery parziale viene utilizzata per collegare un evento "sfocatura" alla casella di testo "Address" HTML. È già sentito parlare di eventi di "stato attivo" che vengono generati quando un utente fa clic o schede in una casella di testo. Un evento "sfocatura" che viene attivata quando un utente esce da una casella di testo è il contrario. Il gestore dell'evento precedente Cancella i valori di casella di testo di latitudine e longitudine in questo caso, e quindi viene tracciata la nuova posizione di indirizzo nel nostro mappa. Un gestore di evento di callback definito all'interno del file map.js aggiornerà quindi le caselle di testo indicata con longitudine e latitudine nel nostro form utilizzando i valori restituiti da virtual earth in base all'indirizzo che è stato assegnato.

E ora quando si esegue di nuovo l'applicazione e fare clic sulla scheda "Host Dinner" vedremo tra breve predefinito è eseguire il mapping visualizzati insieme ai nostri standard elementi form Dinner:

![](use-ajax-to-implement-mapping-scenarios/_static/image2.png)

Quando abbiamo digitare un indirizzo e premere tab da subito, la mappa verrà aggiornato in modo dinamico per visualizzare il percorso e il gestore dell'evento verrà compilate latitudine/longitudine caselle di testo con i valori di posizione:

![](use-ajax-to-implement-mapping-scenarios/_static/image3.png)

Se si salva il nuovo dinner e quindi aprirlo nuovamente per la modifica, verrà individuato che la posizione della mappa viene visualizzata quando il caricamento della pagina:

![](use-ajax-to-implement-mapping-scenarios/_static/image4.png)

Ogni volta che viene modificato il campo dell'indirizzo, verranno aggiornata la mappa e le coordinate di latitudine/longitudine.

Ora che la mappa Visualizza la posizione di Dinner, è anche possibile modificare i campi modulo latitudine e longitudine siano visibili nelle caselle di testo invece di essere gli elementi nascosti (dal momento che la mappa viene automaticamente aggiornarli ogni volta che viene immesso un indirizzo). Cose da fare ciò Sostituiamo da usando l'helper HTML Html.TextBox() all'utilizzo del metodo di helper Html.Hidden():

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample5.aspx)]

Ora e i form sono un po' più semplice evitare visualizzando il raw di latitudine/longitudine (e archiviarle ancora con ciascuna etichetta nel database):

![](use-ajax-to-implement-mapping-scenarios/_static/image5.png)

### <a name="integrating-the-map-with-the-details-view"></a>L'integrazione di mappa con la visualizzazione dei dettagli

Ora che abbiamo la mappa integrata con gli scenari di creazione e modifica, è possibile integrarlo anche con questo scenario i dettagli. È sufficiente attività da eseguire consiste nel chiamare &lt;Html.RenderPartial("map") %; %&gt; all'interno della vista Dettagli.

Ecco come appare il codice sorgente per la visualizzazione di informazioni dettagliate completa (con l'integrazione di mappa):

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample6.aspx)]

Oltre a questo punto quando l'utente passa a un /Dinners/dettagli / [id] URL a visualizzare informazioni dettagliate sulla cena, la posizione della cena sulla mappa (completata con una puntina da disegno che, quando vi passa sopra consente di visualizzare il titolo della finestra di dinner e l'indirizzo di esso), e dispone di un collegamento di AJAX per fo RSVP r è:

![](use-ajax-to-implement-mapping-scenarios/_static/image6.png)

### <a name="implementing-location-search-in-our-database-and-repository"></a>Implementazione della ricerca di posizione nel Database e del Repository

Per concludere la nostra implementazione di AJAX, è possibile aggiungere una mappa alla home page dell'applicazione che consente agli utenti di cercare graficamente dinners vicini.

![](use-ajax-to-implement-mapping-scenarios/_static/image7.png)

Inizieremo mediante l'implementazione di supporto entro il livello del repository del database e i dati in modo efficiente eseguire una ricerca basata sulla posizione radius per Dinners. È possibile usare il nuovo [funzionalità geospaziale di SQL 2008](https://www.microsoft.com/sqlserver/2008/en/us/spatial-data.aspx) implementarla, o in alternativa è possibile usare un approccio di funzione SQL Gary Dryden illustrati in questo articolo: [ http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx ](http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx) e Rob Conery discusso sull'uso di LINQ to SQL qui: [http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/](http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/)

Per implementare questa tecnica, si verrà aprire "Esplora Server" all'interno di Visual Studio, selezionare il database di NerdDinner e quindi fare clic sul nodo secondario "funzioni" sotto di esso e scegliere di creare una nuovo "funzione a valori scalari":

![](use-ajax-to-implement-mapping-scenarios/_static/image8.png)

Abbiamo quindi incollare nella funzione DistanceBetween seguente:

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample7.sql)]

Si creerà quindi una nuova funzione con valori di tabella in SQL Server che chiameremo "NearestDinners":

![](use-ajax-to-implement-mapping-scenarios/_static/image9.png)

Questa funzione di tabella "NearestDinners" viene utilizzata la funzione helper DistanceBetween per restituire tutti Dinners entro 100 miglia la latitudine e longitudine è inserire:

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample8.sql)]

Per chiamare questa funzione, si verrà innanzitutto aprire LINQ to SQL designer facendo doppio clic sul file NerdDinner.dbml entro la directory \Models:

![](use-ajax-to-implement-mapping-scenarios/_static/image10.png)

Si verranno quindi trascinare le funzioni NearestDinners e DistanceBetween LINQ a SQL designer, che provoca l'aggiunta come metodi nel nostro LINQ a SQL NerdDinnerDataContext classe:

![](use-ajax-to-implement-mapping-scenarios/_static/image11.png)

È quindi possibile esporre un metodo di query "FindByLocation" nella classe DinnerRepository che utilizza la funzione NearestDinner restituire imminenti Dinners che si trovano all'interno di 100 miglia dal percorso specificato:

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample9.cs)]

### <a name="implementing-a-json-based-ajax-search-action-method"></a>Implementazione di un metodo di azione di ricerca basati su JSON AJAX

È ora verrà implementato un metodo di azione del controller che sfrutta il nuovo metodo di repository FindByLocation() per restituire un elenco di dati Dinner che possono essere usati per popolare una mappa. Avremo questo metodo di azione restituiscono nuovamente i dati di Dinner in formato JSON (JavaScript Object Notation) in modo che si possono essere facilmente modificato utilizzando JavaScript nel client.

A tale scopo, si creerà una nuova classe "SearchController" facendo clic sulla directory \Controllers e scegliendo Aggiungi -&gt;comando di menu Controller. Si verrà quindi implementare un metodo di azione "SearchByLocation" all'interno della nuova classe SearchController come di seguito:

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample10.cs)]

Metodo di azione del SearchController SearchByLocation chiama internamente il metodo FindByLocation su DinnerRespository per ottenere un elenco di dinners nelle vicinanze. Invece di restituire gli oggetti Dinner direttamente al client, tuttavia, invece restituisce JsonDinner oggetti. La classe JsonDinner espone un subset delle proprietà Dinner (ad esempio: per motivi di sicurezza non divulgherà i nomi delle persone che hanno dato per una cena). Include anche una proprietà RSVPCount che non esiste nel Dinner – e che in modo dinamico vengono calcolate contando il numero di oggetti RSVP associati a una particolare cena.

Utilizziamo quindi il metodo helper Json() nella classe base Controller per restituire la sequenza di dinners utilizzando un formato wire basati su JSON. JSON è un formato di testo standard per la rappresentazione di strutture di dati semplici. Di seguito è riportato un esempio dell'aspetto di un elenco in formato JSON di due oggetti JsonDinner analogo quando restituito dal metodo di azione:

[!code-json[Main](use-ajax-to-implement-mapping-scenarios/samples/sample11.json)]

### <a name="calling-the-json-based-ajax-method-using-jquery"></a>Chiamare il metodo basato su JSON AJAX tramite jQuery

A questo punto siamo pronti aggiornare la home page dell'applicazione NerdDinner Usa metodo di azione del SearchController SearchByLocation. Cose da fare questo, si sarà aprire il modello di vista /Views/Home/Index.aspx e aggiornarlo in modo da avere una casella di testo, pulsante di ricerca, la mappa e un &lt;div&gt; elemento denominato dinnerList:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample12.aspx)]

È quindi possibile aggiungere due funzioni di JavaScript alla pagina:

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample13.html)]

La prima funzione di JavaScript viene caricata la mappa al primo caricamento della pagina. La seconda cavi di funzione JavaScript di JavaScript di fare clic sul gestore eventi sul pulsante di ricerca. Quando viene premuto il pulsante chiama la funzione FindDinnersGivenLocation() JavaScript che verrà aggiunto al file Map.js:

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample14.js)]

Questa funzione FindDinnersGivenLocation() chiama mappa. Find () sul controllo Virtual Earth al centro nel percorso specificato. Quando restituisce il servizio di mappa virtual earth, la mappa. Metodo Find () richiama il metodo di callback callbackUpdateMapDinners che viene passato come argomento finale.

Il metodo callbackUpdateMapDinners() consiste in cui viene eseguita l'operazione effettiva. Usa metodo di supporto di jQuery $.post() per eseguire una chiamata AJAX al metodo di azione del nostro SearchController SearchByLocation() – passandogli la latitudine e longitudine della mappa appena centrata. Definisce una funzione inline che verrà chiamata al completamento del metodo di helper $.post(), e i risultati in formato JSON dinner restituiti dai SearchByLocation() metodo di azione verrà passato tramite una variabile denominata "dinners". Quindi esegue un ciclo foreach via ogni dinner restituito e Usa latitudine del dinner e la longitudine e altre proprietà per aggiungere un nuovo pin sulla mappa. Aggiunge anche una voce dinner all'elenco HTML di dinners a destra della mappa. Quindi cavi-up un evento del mouse per i simboli e l'elenco HTML in modo che siano visualizzati i dettagli relativi la cena quando l'utente passa su di essi:

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample15.html)]

E ora quando si esegue l'applicazione e si visita la home page che viene visualizzate con una mappa. Quando si immette il nome di una città della mappa verrà visualizzati l'imminente dinners vicino al telefono:

![](use-ajax-to-implement-mapping-scenarios/_static/image12.png)

Posizionare il mouse su un dinner visualizzerà i relativi dettagli.

Facendo clic su titolo Dinner il cerchio o sul lato destro nell'elenco HTML si sposterà noi la cena – che è possibile quindi facoltativamente partecipazione:

![](use-ajax-to-implement-mapping-scenarios/_static/image13.png)

### <a name="next-step"></a>Passo successivo

A questo punto abbiamo implementato tutte le funzionalità di applicazione della nostra applicazione NerdDinner. È possibile ora esaminare come è possibile abilitare automatizzata unit test di esso.

> [!div class="step-by-step"]
> [Precedente](use-ajax-to-deliver-dynamic-updates.md)
> [Successivo](enable-automated-unit-testing.md)
