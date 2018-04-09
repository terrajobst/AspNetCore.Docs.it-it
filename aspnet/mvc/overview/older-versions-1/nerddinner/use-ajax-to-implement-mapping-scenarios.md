---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
title: Utilizzare AJAX per implementare scenari di Mapping | Documenti Microsoft
author: microsoft
description: Passaggio 11 viene illustrato come integrare il supporto di AJAX mapping nell'applicazione NerdDinner, consentendo agli utenti che la creazione, modifica o visualizzazione dinners per visualizzare il l...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: f731990a-0a81-4d62-81df-87d676cdedd6
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
msc.type: authoredcontent
ms.openlocfilehash: 4b3f1e46886c4c1f054e43768b0a44695d71bf09
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="use-ajax-to-implement-mapping-scenarios"></a>Utilizzare AJAX per implementare scenari di Mapping
====================
by [Microsoft](https://github.com/microsoft)

[Scarica il PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Questo è il passaggio 11 di una liberazione [esercitazione applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che si interromperanno-through come compilare un piccolo, ma completa, un'applicazione web con ASP.NET MVC 1.
> 
> Passaggio 11 viene illustrato come integrare il supporto di AJAX mapping applicazione NerdDinner, consentendo agli utenti che la creazione, modifica o visualizzazione dinners per visualizzare il percorso della cena graficamente.
> 
> Se si utilizza ASP.NET MVC 3, è consigliabile seguire il [recupero avviato con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [negozio MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) esercitazioni.


## <a name="nerddinner-step-11-integrating-an-ajax-map"></a>NerdDinner passaggio 11: L'integrazione di una mappa di AJAX

Verrà ora effettuata l'applicazione di una piccola eccezionali più visivamente integrando il supporto di AJAX mapping. Ciò consentirà agli utenti che la creazione, modifica o visualizzazione dinners per visualizzare il percorso della cena graficamente.

### <a name="creating-a-map-partial-view"></a>Creazione di una visualizzazione parziale di mappa

Che verranno utilizzare funzionalità di mapping in diverse posizioni all'interno dell'applicazione. Per mantenere il codice sorgente è sarà incapsulano la funzionalità comune mappa all'interno di un singolo modello parziale che possiamo utilizzare nuovamente tra più azioni del controller e visualizzazioni. Si sarà nome questa visualizzazione parziale "map.ascx" e crearla all'interno della directory \Views\Dinners.

È possibile creare il map.ascx parziale facendo clic sulla directory \Views\Dinners e scegliendo Aggiungi -&gt;consente di visualizzare il comando di menu. Verrà nome della vista "Map.ascx", come una visualizzazione parziale e indicare che verrà passare una classe di modello fortemente tipizzata "Dinner":

![](use-ajax-to-implement-mapping-scenarios/_static/image1.png)

Quando si fa clic sul pulsante "Aggiungi" verrà creato il modello parziale. Verrà quindi aggiornata il file Map.ascx per avere il contenuto seguente:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample1.aspx)]

Il primo &lt;script&gt; riferimento alla libreria di mapping di Microsoft Virtual Earth 6.2. Il secondo &lt;script&gt; riferimento punta a un file map.js che verranno create a breve che verrà incapsulata la logica di mapping Javascript comune. Il &lt;div id = "theMap"&gt; elemento è il contenitore HTML utilizzato per ospitare la mappa Virtual Earth.

È quindi necessario incorporato &lt;script&gt; blocco che contiene due funzioni JavaScript specifiche per questa vista. La prima funzione utilizza jQuery wire-backup di una funzione che viene eseguito quando la pagina è pronta per l'esecuzione dello script lato client. Chiama una funzione di supporto LoadMap() che definiamo all'interno di questo file di script Map.js per caricare il controllo mappa virtual earth. La seconda funzione è un gestore eventi di callback che aggiunge un pin per la mappa che identifica una posizione.

Si noti come si sta usando sul lato server &lt;% = %&gt; all'interno del blocco di script sul lato client per incorporare la latitudine e longitudine della cena si desidera eseguire il mapping in JavaScript. Questa tecnica è molto utile per restituire valori dinamici che possono essere usati da script sul lato client (senza richiedere una AJAX chiamata separata al server per recuperare i valori, che rende più veloce). Il &lt;% = %&gt; blocchi verranno eseguita quando viene eseguito il rendering della visualizzazione sul server, e pertanto l'output del codice HTML appena finirà con valori di JavaScript incorporati (ad esempio: latitudine var = 47.64312;).

### <a name="creating-a-mapjs-utility-library"></a>Creazione di una raccolta di utilità Map.js

Creare ora il file Map.js che è possibile utilizzare per incapsulare le funzionalità di JavaScript per la mappa (e implementare i metodi LoadMap e LoadPin sopra). È possibile farlo facendo clic sulla directory \Scripts all'interno del progetto e quindi scegliere il "Add -&gt;nuovo elemento" il comando di menu, selezionare l'elemento di JScript e denominarla "Map.js".

Di seguito è il codice JavaScript verrà aggiunto al file Map.js che interagisce con Virtual Earth per visualizzare la mappa e aggiungervi il PIN di percorsi per il nostro dinners:

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample2.js)]

### <a name="integrating-the-map-with-create-and-edit-forms"></a>L'integrazione della mappa con Create e moduli di modifica

È ora sarà integrare il supporto della mappa con gli scenari di creazione e modifica esistenti. Buone notizie sono che si tratti di attività da eseguire un'operazione abbastanza semplice e non richiede di modificare il codice del Controller. Poiché la creazione e modifica le viste condividono una visualizzazione parziale "DinnerForm" comune per implementare l'interfaccia utente del form dinner, è possibile aggiungere la mappa in un'unica posizione e hanno entrambi gli scenari di creazione e modifica di utilizzarlo.

Ecco cosa da fare è possibile aprire la visualizzazione parziale \Views\Dinners\DinnerForm.ascx e aggiornare in modo da includere la nuova mappa parziale. Di seguito è l'aspetto di DinnerForm aggiornata dopo aver aggiunto la mappa (Nota: gli elementi di form HTML vengono omessi dal frammento di codice seguente per brevità):

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample3.aspx)]

Il DinnerForm parziale sopra accetta un oggetto di tipo "DinnerFormViewModel" come tipo di modello (perché richiede sia un oggetto Dinner, nonché un SelectList per popolare dropdownlist dei paesi). La mappa parziale è necessario impostare solo un oggetto di tipo "Dinner" come tipo di modello e pertanto quando si esegue il rendering della mappa parziale viene passato semplicemente la cena sottoproprietà del DinnerFormViewModel è:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample4.aspx)]

La funzione JavaScript è stato aggiunto per il jQuery parziale viene utilizzata per collegare un evento "sfocatura" per la casella di testo "Address" HTML. È già sentito parlare di eventi di "stato attivo" che vengono generati quando un utente fa clic o schede in una casella di testo. L'opposto è un evento di "sfocatura" che viene generato quando un utente esce da una casella di testo. Il gestore dell'evento precedente Cancella i valori di latitudine e longitudine casella di testo quando questo accade e quindi vengono tracciate nella nuova posizione indirizzo la mappa. Un gestore eventi di callback che è stato definito all'interno del file map.js verrà aggiornati nelle caselle di testo longitudine e latitudine sul form utilizzando i valori restituiti da virtual earth in base all'indirizzo che è stato assegnato.

E, quando si Esegui nuovamente l'applicazione e fare clic sulla scheda "Host Dinner" vedremo predefinito mapping visualizzato unitamente al nostro gli elementi form Dinner standard:

![](use-ajax-to-implement-mapping-scenarios/_static/image2.png)

Digitare un indirizzo, e quindi sulla scheda assente, la mappa verrà aggiornato in modo dinamico per visualizzare il percorso e il gestore dell'evento verrà compilate latitudine e longitudine caselle di testo con i valori del percorso:

![](use-ajax-to-implement-mapping-scenarios/_static/image3.png)

Se si salva il nuovo dinner e poi riaprirla per la modifica, verrà individuato che la posizione della mappa viene visualizzata quando il caricamento della pagina:

![](use-ajax-to-implement-mapping-scenarios/_static/image4.png)

Ogni volta che viene modificato il campo dell'indirizzo, verranno aggiornata la mappa e le coordinate di latitudine e longitudine.

Ora che la mappa viene visualizzato il percorso di Dinner, è anche possibile modificare i campi modulo latitudine e longitudine saranno visibili nelle caselle di testo invece di essere elementi nascosti (perché la mappa viene automaticamente aggiornarli ogni volta che viene immesso un indirizzo). Cose da fare ciò è possibile passare da utilizzando l'helper HTML Html.TextBox() a utilizzare il metodo helper Html.Hidden():

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample5.aspx)]

E ora il form sono un po' più semplice ed evitare la visualizzazione di latitudine e longitudine non elaborata (archiviando ancora li con ciascuna etichetta nel database):

![](use-ajax-to-implement-mapping-scenarios/_static/image5.png)

### <a name="integrating-the-map-with-the-details-view"></a>La mappa di integrazione con la visualizzazione dei dettagli

Dopo aver creato la mappa integrata con gli scenari di creazione e modifica, verrà anche integrarlo in questo scenario di dettagli. È sufficiente attività da eseguire consiste nel chiamare &lt;% % Html.RenderPartial("map");&gt; all'interno della visualizzazione dettagli.

Di seguito è il codice sorgente per la visualizzazione completa dei dettagli (con l'integrazione di mappa) l'aspetto seguente:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample6.aspx)]

E ora quando un utente accede a un /Dinners/dettagli / [id] URL verranno visualizzati i dettagli sulla cena, la posizione della cena sulla mappa (con una puntina da disegno che, se al passaggio del mouse consente di visualizzare il titolo della cena e l'indirizzo di esso), e di disporre di un collegamento AJAX RSVP fo r è:

![](use-ajax-to-implement-mapping-scenarios/_static/image6.png)

### <a name="implementing-location-search-in-our-database-and-repository"></a>Implementazione della ricerca percorso nei Database e del Repository

Per concludere l'implementazione di AJAX, aggiungere una mappa alla home page dell'applicazione che consente agli utenti di cercare graficamente dinners vicini.

![](use-ajax-to-implement-mapping-scenarios/_static/image7.png)

Inizieremo mediante l'implementazione di supporto all'interno di questo livello di repository database e i dati per eseguire una ricerca in base alla posizione radius per Dinners. È possibile usare il nuovo [geospatial caratteristiche di SQL 2008](https://www.microsoft.com/sqlserver/2008/en/us/spatial-data.aspx) per implementare questa, o in alternativa è possibile usare un approccio di funzione SQL Gary Dryden citata nell'articolo qui: [ http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx ](http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx) e Rob Conery problema sull'utilizzo con LINQ to SQL qui: [http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/](http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/)

Per implementare questa tecnica, si verrà aprire "Esplora Server" all'interno di Visual Studio, selezionare il database NerdDinner e quindi fare clic sul nodo secondario "funzioni" in essa contenute e scegliere di creare una nuovo "funzione a valori scalari":

![](use-ajax-to-implement-mapping-scenarios/_static/image8.png)

È quindi da incollare nella funzione DistanceBetween seguente:

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample7.sql)]

In SQL Server che verrà chiamato "NearestDinners", si creerà quindi una nuova funzione con valori di tabella:

![](use-ajax-to-implement-mapping-scenarios/_static/image9.png)

Questa funzione di tabella "NearestDinners" utilizza la funzione di supporto DistanceBetween per restituire tutti Dinners entro 100 miglia la latitudine e longitudine è fornire:

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample8.sql)]

Per chiamare questa funzione, verrà prima aperto di LINQ to SQL designer facendo doppio clic sul file NerdDinner.dbml all'interno di nostra directory \Models:

![](use-ajax-to-implement-mapping-scenarios/_static/image10.png)

Si verranno quindi trascinare le funzioni NearestDinners e DistanceBetween LINQ alla finestra di progettazione SQL, che impedirà a essere aggiunto come metodi sul nostro LINQ a SQL NerdDinnerDataContext classe:

![](use-ajax-to-implement-mapping-scenarios/_static/image11.png)

È quindi possibile esporre un metodo di query "FindByLocation" nella classe DinnerRepository che utilizza la funzione NearestDinner per restituire future Dinners che sono di 100 miglia della posizione specificata:

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample9.cs)]

### <a name="implementing-a-json-based-ajax-search-action-method"></a>Implementazione di un metodo di azione di ricerca basata su JSON AJAX

È ora viene implementato un metodo di azione del controller che consente di sfruttare il nuovo metodo repository FindByLocation() per restituire un elenco di dati Dinner che possono essere utilizzati per popolare una mappa. È necessario questo metodo di azione restituire i dati cena in un formato JSON (JavaScript Object Notation) in modo che si possono essere facilmente modificato tramite JavaScript nel client.

A questo scopo, si creerà una nuova classe "SearchController" facendo clic sulla directory \Controllers e scegliendo Aggiungi -&gt;comando di menu Controller. Verrà quindi implementato un metodo di azione "SearchByLocation" all'interno della nuova classe SearchController come di seguito:

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample10.cs)]

Metodo di azione del SearchController SearchByLocation chiama internamente il metodo FindByLocation su DinnerRespository per ottenere un elenco di dinners nelle vicinanze. Anziché restituire gli oggetti Dinner direttamente al client, tuttavia, invece restituisce JsonDinner oggetti. La classe JsonDinner espone un subset di proprietà Dinner (ad esempio: per motivi di sicurezza non divulgazione i nomi delle persone che hanno RSVP'd per una cena). Include inoltre una proprietà RSVPCount che non esiste nel Dinner – e che viene calcolato in modo dinamico contando il numero di oggetti RSVP associata a una particolare cena.

Viene quindi usato il metodo di supporto Json() sulla classe di base Controller per restituire la sequenza di dinners utilizzando un formato wire basata su JSON. JSON è un formato di testo standard per la rappresentazione di strutture di dati semplice. Di seguito è riportato un esempio di un elenco in formato JSON di due oggetti JsonDinner aspetto quando restituito dal metodo di azione:

[!code-json[Main](use-ajax-to-implement-mapping-scenarios/samples/sample11.json)]

### <a name="calling-the-json-based-ajax-method-using-jquery"></a>Chiamare il metodo basato su JSON AJAX tramite jQuery

Ci sono ora pronti per aggiornare la home page dell'applicazione NerdDinner come metodo di azione del SearchController SearchByLocation utilizzare. Attività da eseguire questa operazione, si sarà aprire il modello di visualizzazione /Views/Home/Index.aspx e aggiornare in modo da disporre di una casella di testo, il pulsante di ricerca, la mappa e un &lt;div&gt; elemento denominato dinnerList:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample12.aspx)]

È quindi possibile aggiungere due funzioni JavaScript alla pagina:

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample13.html)]

La prima funzione JavaScript carica la mappa al primo caricamento della pagina. I cavi di funzione JavaScript secondo backup JavaScript fare clic su gestore dell'evento sul pulsante di ricerca. Quando viene premuto il pulsante chiama la funzione JavaScript FindDinnersGivenLocation() che verrà aggiunto al file Map.js:

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample14.js)]

Questa funzione FindDinnersGivenLocation() chiama mappa. Find() sul controllo Virtual Earth al centro il percorso immesso. Quando il servizio di mappa virtual earth restituisce, la mappa. Metodo Find() richiama il metodo di callback callbackUpdateMapDinners che è passato come argomento finale.

Il metodo callbackUpdateMapDinners() è in cui viene eseguito il lavoro effettivo. Usa metodo di supporto di jQuery $.post() per eseguire una chiamata AJAX al metodo di azione del nostro SearchController SearchByLocation() – passandogli la latitudine e longitudine della mappa appena centrata. Definisce una funzione inline che verrà chiamata al completamento del metodo di supporto $.post() e i risultati in formato JSON dinner restituiti dal SearchByLocation() verrà passato il metodo di azione utilizzando una variabile denominata "dinners". Quindi esegue un ciclo foreach via ogni dinner restituito e Usa longitudine e latitudine del dinner e le altre proprietà per aggiungere un nuovo pin sulla mappa. Aggiunge inoltre una voce dinner all'elenco HTML di dinners a destra della mappa. Quindi cavi-up un evento di passaggio del mouse per i simboli e l'elenco HTML in modo che vengono visualizzati i dettagli relativi la cena quando un utente viene spostato su di essi:

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample15.html)]

E ora quando si esegue l'applicazione e visitare la home page che viene visualizzate con una mappa. Quando si immette il nome di una città la mappa verrà visualizzata la prossima dinners vicino:

![](use-ajax-to-implement-mapping-scenarios/_static/image12.png)

Passaggio del mouse su una cena sarà visualizzati i dettagli su di esso.

Facendo clic titolo Dinner bolla o sul lato destro nell'elenco HTML verrà visualizzata ci la cena – che è quindi possibile facoltativamente RSVP per:

![](use-ajax-to-implement-mapping-scenarios/_static/image13.png)

### <a name="next-step"></a>Passo successivo

Ora sono state implementate tutte le funzionalità di applicazione dell'applicazione NerdDinner. Verrà ora esaminata come è possibile abilitare automatizzata unit test di esso.

> [!div class="step-by-step"]
> [Precedente](use-ajax-to-deliver-dynamic-updates.md)
> [Successivo](enable-automated-unit-testing.md)
