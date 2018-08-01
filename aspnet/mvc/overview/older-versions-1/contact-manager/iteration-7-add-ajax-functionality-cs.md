---
uid: mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-cs
title: 'Iterazione #7-aggiungere funzionalità Ajax (c#) | Microsoft Docs'
author: microsoft
description: Nell'iterazione del settimo, aggiungendo il supporto per Ajax è migliorare la velocità di risposta e le prestazioni dell'applicazione.
ms.author: aspnetcontent
ms.date: 02/20/2009
ms.assetid: f1b0809e-8909-444e-b6bb-a5cd1dea3f72
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-cs
msc.type: authoredcontent
ms.openlocfilehash: 9b7a3acefbfeef9da5e45b40f03809b00364de05
ms.sourcegitcommit: a25b572eaed21791230c85416f449f66a405ec19
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/01/2018
ms.locfileid: "39396156"
---
<a name="iteration-7--add-ajax-functionality-c"></a>Iterazione #7-aggiungere funzionalità Ajax (c#)
====================
da [Microsoft](https://github.com/microsoft)

[Scaricare il codice](iteration-7-add-ajax-functionality-cs/_static/contactmanager_7_cs1.zip)

> Nell'iterazione del settimo, aggiungendo il supporto per Ajax è migliorare la velocità di risposta e le prestazioni dell'applicazione.


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Creazione di un'applicazione MVC ASP.NET di gestione dei contatti (c#)

In questa serie di esercitazioni, creiamo un'intera applicazione di gestione dei contatti dall'inizio alla fine. L'applicazione Contact Manager consente di archiviare le informazioni di contatto - nomi, i numeri di telefono e indirizzi di posta elettronica - per un elenco di persone.

Si compilerà l'applicazione a varie iterazioni. A ogni iterazione, abbiamo migliorare gradualmente l'applicazione. L'obiettivo di questo approccio iterazione più è che consentono di comprendere il motivo per ogni modifica.

- Iterazione #1 - creare l'applicazione. Nella prima iterazione, verranno create Contact Manager nel modo più semplice possibile. Viene aggiunto il supporto per le operazioni di base dei database: crea, lettura, aggiornamento ed eliminazione (CRUD).

- Iterazione #2 - migliorare l'applicazione aspetto interessante. In questa iterazione è migliorare l'aspetto dell'applicazione modificando il valore predefinito di pagina master visualizzazione MVC ASP.NET e foglio di stile CSS.

- Iterazione #3 - aggiungere la convalida dei form. Nella terza iterazione, aggiungiamo la convalida dei form di base. Persone è impedire l'invio di un modulo senza completare i campi del modulo richiesto. È anche convalidare gli indirizzi di posta elettronica e numeri di telefono.

- Iterazione #4 - rendere l'applicazione a regime. In questo quarta iterazione, possiamo usufruire dei diversi modelli di progettazione di software per renderne più semplice da gestire e modificare l'applicazione Contact Manager. Ad esempio, è eseguire il refactoring l'applicazione per usare il modello di Repository e il modello di inserimento delle dipendenze.

- Iterazione #5 - creare unit test. Nell'iterazione del quinto, si semplifica l'applicazione di manutenzione e la modifica mediante l'aggiunta di unit test. È simulare le classi di modello di dati e generare unit test per i controller e logica di convalida.

- Iterazione #6 - usare lo sviluppo basato su test. In questa iterazione sesta, viene aggiunto nuove funzionalità all'applicazione scrivendo unit test prima e scrivere codice sull'unit test. In questa iterazione, viene aggiunto gruppi di contatti.

- Iterazione #7 - aggiungere funzionalità Ajax. Nell'iterazione del settimo, aggiungendo il supporto per Ajax è migliorare la velocità di risposta e le prestazioni dell'applicazione.

## <a name="this-iteration"></a>Questa iterazione

In questa iterazione dell'applicazione Contact Manager, è eseguire il refactoring l'applicazione per l'utilizzo di Ajax. Grazie all'uso di Ajax, rendiamo la nostra applicazione più reattiva. È possibile evitare il rendering di un'intera pagina quando è necessario aggiornare solo una determinata area in una pagina.

Si effettuerà il refactoring la visualizzazione dell'indice, in modo che non abbiamo t necessità per visualizzare nuovamente l'intera pagina ogni volta che un utente seleziona un nuovo gruppo di contatto. Al contrario, quando un utente fa clic su un gruppo di contatto, verrà semplicemente aggiornare l'elenco dei contatti e lasciare il resto della pagina da solo.

Il modo in cui l'eliminazione collegamento works verranno anche modificati. Invece di visualizzare una pagina di conferma separata, verrà visualizzato una finestra di dialogo di conferma di JavaScript. Se si conferma che si desidera eliminare un contatto, un'operazione HTTP DELETE viene eseguita sul server per eliminare il record di contatto dal database.

Abbiamo, inoltre, sarà possibile avvalersi di jQuery per aggiungere effetti di animazione per la visualizzazione dell'indice. Verrà visualizzato un'animazione quando il nuovo elenco di contatti è in corso recuperato dal server.

Infine, Daremo sfruttare il supporto del framework ASP.NET AJAX per la gestione della cronologia del browser. Punti chiave di navigazione verrà creato ogni volta che si esegue una chiamata Ajax per aggiornare l'elenco dei contatto. In questo modo, il browser avanti e indietro pulsanti will di lavoro.

## <a name="why-use-ajax"></a>Perché usare Ajax?

Utilizzo di Ajax presenta numerosi vantaggi. In primo luogo, l'aggiunta di funzionalità Ajax a un'applicazione comporta una migliore esperienza utente. In un'applicazione web normale, l'intera pagina deve essere inviato al server ogni volta che un utente esegue un'azione. Ogni volta che si esegue un'azione, i blocchi di browser e l'utente deve attendere finché non viene recuperata e visualizzata di nuovo l'intera pagina.

Questa operazione è un'esperienza accettabile nel caso di un'applicazione desktop. Tuttavia, in genere, è una durata con questa esperienza utente negativa in caso di un'applicazione web perché è stata non sappiamo che possiamo creare risultati migliori. Abbiamo pensato che fosse una limitazione di applicazioni web quando, in realtà, era semplicemente una limitazione del nostro imaginations.

In un'applicazione Ajax, non è necessario t per portare l'esperienza utente per arrestare semplicemente per aggiornare una pagina. In alternativa, è possibile eseguire una richiesta asincrona in background per aggiornare la pagina. Non si forza t all'utente è in corso la parte della pagina viene aggiornata.

Grazie all'uso di Ajax, è anche possibile migliorare le prestazioni dell'applicazione. Prendere in considerazione come l'applicazione Contact Manager funziona ora senza funzionalità Ajax. Quando si fa clic su un gruppo di contatti, la visualizzazione dell'indice intera deve essere visualizzata nuovamente. L'elenco dei contatti e l'elenco di gruppi di contatto devono essere recuperati dal server di database. Tutti i dati devono essere passati attraverso la rete dal server web in web browser.

Dopo che la funzionalità Ajax all'applicazione, tuttavia, possibile evitare di visualizzare di nuovo l'intera pagina quando un utente fa clic su un gruppo di contatti. È necessario non è più in grado di catturare i gruppi di contatto dal database. Temiamo inoltre non è necessario eseguire il push di visualizzazione dell'indice intero attraverso la rete. Grazie all'uso di Ajax, si riduce la quantità di lavoro che deve eseguire il server di database e si riduce la quantità di traffico di rete richiesti dall'applicazione.

## <a name="don-t-be-afraid-of-ajax"></a>Tenere sempre essere paura di Ajax

Alcuni sviluppatori evitano l'utilizzo di Ajax poiché essi preoccuparsi di browser di livello inferiore. Si desidera assicurarsi che le applicazioni web funzioneranno comunque quando vi si accede da un browser che non supporta JavaScript. Poiché Ajax dipende da JavaScript, alcuni sviluppatori evitano l'utilizzo di Ajax.

Tuttavia, se si è necessario prestare particolare attenzione alla modalità di implementazione di Ajax è possibile compilare le applicazioni che funzionano con i browser di livello inferiore e superiore. L'applicazione Contact Manager funzionerà con i browser che supportano JavaScript e i browser che non li supportano.

Se si usa l'applicazione Contact Manager con un browser che supporti JavaScript si avrà una migliore esperienza utente. Ad esempio, quando si fa clic su un gruppo di contatto, verrà aggiornato solo nell'area della pagina che visualizza i contatti.

Se, d'altra parte, si usa l'applicazione Contact Manager con un browser che non supporta JavaScript (o che ha JavaScript disabilitato) si avrà un'esperienza utente leggermente meno consigliata. Ad esempio, quando si fa clic su un gruppo di contatti, la visualizzazione dell'indice intera deve essere registrata al browser per visualizzare l'elenco di contatti corrispondenti.

## <a name="adding-the-required-javascript-files"></a>Aggiunta di file JavaScript necessari

È necessario utilizzare tre file JavaScript per aggiungere funzionalità Ajax all'applicazione. Tutte e tre questi file sono inclusi nella cartella degli script di una nuova applicazione MVC ASP.NET.

Se si prevede di usare Ajax in più pagine nell'applicazione è consigliabile includere i file JavaScript necessari nella pagina master visualizzazione s dell'applicazione. In questo modo, i file JavaScript verranno automaticamente inclusa nel tutte le pagine nell'applicazione.

Aggiungere il codice JavaScript seguente include all'interno di &lt;head&gt; tag della pagina master di visualizzazione:

[!code-html[Main](iteration-7-add-ajax-functionality-cs/samples/sample1.html)]

## <a name="refactoring-the-index-view-to-use-ajax"></a>Refactoring la visualizzazione dell'indice per l'utilizzo di Ajax

Let s iniziare modificando la visualizzazione dell'indice in modo che facendo clic su un gruppo di contatto vengono aggiornati solo l'area della vista che consente di visualizzare i contatti. Il riquadro rosso nella figura 1 contiene l'area in cui si desidera aggiornare.


[![L'aggiornamento solo i contatti](iteration-7-add-ajax-functionality-cs/_static/image1.jpg)](iteration-7-add-ajax-functionality-cs/_static/image1.png)

**Figura 01**: l'aggiornamento solo i contatti ([fare clic per visualizzare l'immagine con dimensioni normali](iteration-7-add-ajax-functionality-cs/_static/image2.png))


Il primo passaggio consiste nel separare la parte della vista che si vuole aggiornare in modo asincrono in un elemento parziale separato (controllo utente visualizzazione). La sezione della visualizzazione Index che visualizza la tabella dei contatti è stata spostata in parziale nel listato 1.

**Listato 1 - Views\Contact\ContactList.ascx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample2.aspx)]

Si noti che la parziale nel listato 1 contiene un modello diverso rispetto alla visualizzazione dell'indice. Il *Inherits* attributo il &lt;% @ Page %&gt; direttiva specifica che la parziale eredita dal ViewUserControl&lt;gruppo&gt; classe.

La visualizzazione dell'indice aggiornata è contenuta nel listato 2.

**Listato 2 - Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample3.aspx)]

Esistono due aspetti che è possibile notare sulla vista aggiornata nel listato 2. In primo luogo, si noti che tutto il contenuto spostato in parziale viene sostituito con una chiamata a Html.RenderPartial(). Il metodo Html.RenderPartial() viene chiamato quando la visualizzazione dell'indice viene richiesto prima di tutto per visualizzare il set iniziale di contatti.

In secondo luogo, si noti che il Html.ActionLink() consente di visualizzare i gruppi di contatto sono stato sostituito con un Ajax.ActionLink(). Viene chiamato il Ajax.ActionLink() con i parametri seguenti:

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample4.aspx)]

Il primo parametro rappresenta il testo da visualizzare per il collegamento, il secondo parametro rappresenta i valori di route e il terzo parametro rappresenta le opzioni Ajax. In questo caso, si usa l'opzione UpdateTargetId Ajax in modo che punti al codice HTML &lt;div&gt; tag che si desidera aggiornare dopo il completamento della richiesta Ajax. Si desidera aggiornare il &lt;div&gt; tag con il nuovo elenco di contatti.

Il metodo Index () aggiornato del controller di contatto è contenuto nel listato 3.

**Listato 3 - Controllers\ContactController.cs (metodo di indice)**

[!code-csharp[Main](iteration-7-add-ajax-functionality-cs/samples/sample5.cs)]

L'azione Index () aggiornato in modo condizionale restituisce uno dei due cose. Se viene richiamata l'azione Index () da una richiesta Ajax il controller restituisce un elemento parziale. In caso contrario, l'azione Index () restituisce una vista intera.

Si noti che l'azione Index () non è necessario restituire tutti i dati quando viene richiamati da una richiesta Ajax. Nel contesto di una normale richiesta, l'operazione sull'indice restituisce un elenco di tutti i gruppi di contatto e il gruppo di contatto selezionato. Nel contesto di una richiesta Ajax, l'azione Index () restituisce solo il gruppo selezionato. AJAX significa meno lavoro sul server di database.

La visualizzazione dell'indice modificato funziona nel caso di browser di livello inferiore e superiore. Se si fa clic su un gruppo di contatto e il browser supporta JavaScript, viene aggiornato solo nell'area della vista che contiene l'elenco dei contatti. Se, d'altra parte, il browser non supporta JavaScript, viene aggiornata la vista intera.


La visualizzazione dell'indice aggiornata presenta un problema. Quando si fa clic su un gruppo di contatto, il gruppo selezionato non viene evidenziato. Perché viene visualizzato l'elenco dei gruppi all'esterno dell'area che viene aggiornata durante una richiesta Ajax, il gruppo corretto non vengono evidenziato. Questo problema verrà risolto nella sezione successiva.


## <a name="adding-jquery-animation-effects"></a>Aggiunta di effetti di animazione di jQuery

In genere, quando si fa clic su un collegamento in una pagina web, è possibile utilizzare l'indicatore di stato del browser per rilevare se il browser attivamente recupera il contenuto aggiornato. Quando si esegue una richiesta Ajax, d'altra parte, l'indicatore di stato del browser non mostra alcun progresso. Ciò può rendere infastidisce gli utenti. Come si può sapere se il browser ha bloccato?

Esistono vari modi in cui è possibile indicare a un utente che le operazioni in esecuzione durante l'esecuzione di una richiesta Ajax. Un approccio consiste nel visualizzare una semplice animazione. Ad esempio, è possibile dissolvenza un'area quando una richiesta Ajax inizia e nell'area di dissolvenza al completamento della richiesta.

Si userà la libreria jQuery incluso con il framework Microsoft ASP.NET MVC, per creare gli effetti di animazione. La visualizzazione dell'indice aggiornata è contenuta nel listato 4.

**Listato 4 - Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample6.aspx)]

Si noti che la visualizzazione dell'indice aggiornata contiene tre nuove funzioni di JavaScript. Le prime due funzioni usano jQuery dissolvenza e dissolvenza nell'elenco di contatti quando si fa clic su un nuovo gruppo di contatto. La terza funzione viene visualizzato un messaggio di errore quando un risultati della richiesta Ajax in un errore (ad esempio, un timeout di rete).

La prima funzione si occupa anche di evidenziare il gruppo selezionato. Una classe = attributo selezionato viene aggiunto all'elemento padre (l'elemento LI) dell'elemento selezionato. Anche in questo caso, jQuery semplifica selezionare l'elemento a destra e aggiungere la classe CSS.

Questi script sono associati i collegamenti di gruppo con l'aiuto del parametro Ajax.ActionLink() AjaxOptions. La chiamata al metodo Ajax.ActionLink() aggiornata è simile alla seguente:

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample7.aspx)]

## <a name="adding-browser-history-support"></a>Aggiunta del supporto della cronologia del Browser

In genere, quando si fa clic su un collegamento per aggiornare una pagina, viene aggiornata la cronologia del browser. In questo modo, è possibile fare clic sul pulsante Indietro del browser per spostare indietro nel tempo allo stato precedente della pagina. Ad esempio, se si sceglie il gruppo contatto con amici e quindi fare clic sul gruppo al contatto aziendale, è possibile fare clic sul pulsante Indietro del browser per passare allo stato della pagina quando è stato selezionato il gruppo di amici contatto.

Sfortunatamente, esecuzione di una richiesta Ajax non aggiorna la cronologia del browser automaticamente. Se si fa clic su un gruppo di contatti e viene recuperato l'elenco di contatti di corrispondenza con una richiesta Ajax, la cronologia del browser non viene aggiornata. È possibile usare il pulsante Indietro del browser per tornare al gruppo di contatti dopo aver selezionato un nuovo gruppo di contatto.

Se si desidera che gli utenti siano in grado di utilizzare il browser torna pulsante dopo l'esecuzione di richieste Ajax è necessario eseguire alcune ulteriori operazioni. È necessario sfruttare la funzionalità di gestione della cronologia del browser compilata nel Framework ASP.NET AJAX.

Cronologia del browser AJAX ASP.NET, è necessario eseguire tre operazioni:

1. Abilitare la cronologia del Browser impostando la proprietà enableBrowserHistory su true.
2. Cronologia punti di salvataggio quando cambia lo stato di una visualizzazione chiamando il metodo addHistoryPoint().
3. Quando viene generato l'evento di esplorazione di ricostruire lo stato della visualizzazione.

La visualizzazione dell'indice aggiornata è contenuta nel listato 5.

**Listato 5 - Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample8.aspx)]

Nel listato 5, la cronologia del Browser è abilitata nella funzione pageInit(). La funzione pageInit() viene utilizzata anche per configurare il gestore eventi per l'evento di esplorazione. L'evento di esplorazione viene generato ogni volta che il browser-avanti o indietro fa sì che lo stato della pagina da modificare.

Il metodo beginContactList() viene chiamato quando si fa clic su un gruppo di contatti. Questo metodo crea un nuovo punto di cronologia chiamando il metodo addHistoryPoint(). L'id del gruppo di contatto selezionato viene aggiunto alla cronologia.

L'id del gruppo viene recuperato da un attributo expando sul collegamento gruppo contatto. Il collegamento viene eseguito il rendering con la chiamata seguente a Ajax.ActionLink().

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample9.aspx)]

L'ultimo parametro passato al Ajax.ActionLink() aggiunge un attributo expando denominato groupid al collegamento (lettere minuscole per garantire la compatibilità XHTML).

Quando un utente raggiunge l'indietro del browser o il pulsante Avanti, viene generato l'evento di esplorazione e viene chiamato il metodo navigate(). Questo metodo aggiorna i contatti nella pagina corrispondano allo stato della pagina che corrisponde al punto della cronologia del browser passato al metodo navigate.

## <a name="performing-ajax-deletes"></a>Esecuzione di Ajax Elimina

Attualmente, per eliminare un contatto, è necessario fare clic sul collegamento Elimina e quindi fare clic sul pulsante di eliminazione visualizzato nella pagina di conferma delete (vedere la figura 2). Ciò dovrebbe essere un numero elevato di richieste di pagina per eseguire un'operazione semplice, come l'eliminazione di un record di database.


[![La pagina di conferma delete](iteration-7-add-ajax-functionality-cs/_static/image2.jpg)](iteration-7-add-ajax-functionality-cs/_static/image3.png)

**Figura 02**: la pagina di conferma delete ([fare clic per visualizzare l'immagine con dimensioni normali](iteration-7-add-ajax-functionality-cs/_static/image4.png))


Si è tentati di ignorare la pagina di conferma delete ed eliminare un contatto direttamente dalla visualizzazione Index. È consigliabile evitare la tentazione perché questo approccio consente di aprire l'applicazione per problemi di sicurezza. In generale, non si t desidera eseguire un'operazione GET HTTP quando si richiama un'azione che modifica lo stato dell'applicazione web. Quando si esegue un'operazione di eliminazione, si desidera eseguire una richiesta HTTP POST, o meglio ancora, un'operazione HTTP DELETE.

Il collegamento di eliminazione è contenuto nel ContactList parziale. Una versione aggiornata del ContactList parziale è contenuta nel listato 6.

**Listato 6 - Views\Contact\ContactList.ascx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample10.aspx)]

Il collegamento di eliminazione viene eseguito il rendering con la chiamata seguente al metodo Ajax.ImageActionLink():

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample11.aspx)]

> [!NOTE] 
> 
> Il Ajax.ImageActionLink() non è una parte standard del framework ASP.NET MVC. Il Ajax.ImageActionLink() è un metodi helper personalizzati inclusi nel progetto Contact Manager.


Il parametro AjaxOptions ha due proprietà. In primo luogo, la proprietà di Confirm consente di visualizzare una finestra di conferma popup JavaScript. In secondo luogo, la proprietà HttpMethod viene utilizzata per eseguire un'operazione HTTP DELETE.

Listato 7 contiene una nuova azione AjaxDelete() che è stato aggiunto al controller di contatto.

**Listato 7 - Controllers\ContactController.cs (AjaxDelete)**

[!code-csharp[Main](iteration-7-add-ajax-functionality-cs/samples/sample12.cs)]

L'azione AjaxDelete() è decorata con un attributo AcceptVerbs. Questo attributo impedisce l'azione venga chiamato, ad eccezione da qualsiasi operazione HTTP diverso da un'operazione HTTP DELETE. In particolare, è possibile richiamare questa azione con una richiesta HTTP GET.

Dopo aver eliminato il record di database, è necessario visualizzare l'elenco aggiornato dei contatti che non contiene il record eliminato. Il metodo AjaxDelete() restituisce il ContactList parziale e l'elenco aggiornato dei contatti.

## <a name="summary"></a>Riepilogo

Questa iterazione, abbiamo aggiunto la funzionalità Ajax all'applicazione Contact Manager. Abbiamo utilizzato Ajax per migliorare la velocità di risposta e le prestazioni dell'applicazione.

In primo luogo, viene effettuato il refactoring la visualizzazione dell'indice in modo che facendo clic su un gruppo di contatto non aggiorna l'intera vista. Al contrario, fare clic su un gruppo di contatto Aggiorna solo l'elenco dei contatti.

Successivamente, abbiamo utilizzato gli effetti di animazione di jQuery per dissolvenza e dissolvenza nell'elenco dei contatti. Aggiunta di animazione a un'applicazione Ajax è utilizzabile per fornire agli utenti dell'applicazione con l'equivalente di un indicatore di stato del browser.

Abbiamo anche aggiunto il supporto della cronologia del browser all'applicazione Ajax. È consentito agli utenti di fare clic su Indietro del browser pulsanti Avanti e indietro per modificare lo stato della visualizzazione Index.

Infine, abbiamo creato un collegamento di eliminazione che supporta le operazioni HTTP DELETE. Mediante l'esecuzione di eliminazioni di Ajax, forniamo agli utenti di eliminare i record del database senza richiedere all'utente di richiedere una pagina di conferma delete aggiuntive.

> [!div class="step-by-step"]
> [Precedente](iteration-6-use-test-driven-development-cs.md)
> [Successivo](iteration-1-create-the-application-vb.md)
