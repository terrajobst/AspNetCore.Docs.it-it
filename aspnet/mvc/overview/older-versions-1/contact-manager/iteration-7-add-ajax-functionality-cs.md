---
uid: mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-cs
title: 'Iterazione #7: la funzionalità Aggiungi Ajax (c#) | Documenti Microsoft'
author: microsoft
description: Nella settima iterazione, è migliorare la velocità di risposta e prestazioni dell'applicazione aggiunta del supporto per Ajax.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: f1b0809e-8909-444e-b6bb-a5cd1dea3f72
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-cs
msc.type: authoredcontent
ms.openlocfilehash: 35d62383a571725749b2fc629bbb17954657b2f6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875526"
---
<a name="iteration-7--add-ajax-functionality-c"></a>Iterazione #7: la funzionalità Aggiungi Ajax (c#)
====================
by [Microsoft](https://github.com/microsoft)

[Scaricare il codice](iteration-7-add-ajax-functionality-cs/_static/contactmanager_7_cs1.zip)

> Nella settima iterazione, è migliorare la velocità di risposta e prestazioni dell'applicazione aggiunta del supporto per Ajax.


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Creazione di un'applicazione ASP.NET MVC di gestione dei contatti (c#)

In questa serie di esercitazioni, si compila un'intera applicazione di gestione dei contatti dall'inizio completamento. L'applicazione Contact Manager consente di archiviare le informazioni di contatto - nomi, i numeri di telefono e indirizzi di posta elettronica: per un elenco di persone.

È compilare l'applicazione più iterazioni. A ogni iterazione, gradualmente è migliorare l'applicazione. L'obiettivo di questo approccio iterazione più è che consentono di comprendere il motivo per ogni modifica.

- Iterazione #1 - creare l'applicazione. Nella prima iterazione, verranno create Contact Manager in modo più semplice possibile. Viene aggiunto il supporto per le operazioni di database basic: creazione, lettura, aggiornamento ed eliminazione (CRUD).

- Iterazione #2 - verificare l'applicazione l'aspetto. In questa iterazione, è migliorare l'aspetto dell'applicazione modificando il valore predefinito di pagina master di visualizzazione ASP.NET MVC e foglio di stile CSS.

- Iterazione #3 - aggiungere la convalida dei form. Nella terza iterazione, è aggiungere la convalida di form di base. È impedire agli utenti di inviare un modulo senza completare i campi modulo necessari. È inoltre possibile convalidare gli indirizzi di posta elettronica e numeri di telefono.

- Iterazione #4: verificare l'applicazione ad accoppiamento debole. In questa terza iterazione, possiamo usufruire dei diversi modelli di progettazione di software per renderne più semplice gestire e modificare l'applicazione Gestione contatti. Ad esempio, si effettua il refactoring l'applicazione di utilizzare il modello di Repository e il modello di inserimento di dipendenze.

- Iterazione #5 - creare unit test. Nella quinta iterazione, si rende l'applicazione di più facile da gestire e modificare tramite l'aggiunta di unit test. È simulare il nostro classi del modello di dati e generare unit test per i controller e logica di convalida.

- Iterazione 6 # - utilizzare sviluppo basato su test. In questa iterazione sesto è aggiungere nuove funzionalità per l'applicazione scrivendo unit test prima e la scrittura di codice per gli unit test. In questa iterazione, è aggiungere gruppi di contatti.

- Iterazione #7 - aggiunta di funzionalità Ajax. Nella settima iterazione, è migliorare la velocità di risposta e prestazioni dell'applicazione aggiunta del supporto per Ajax.

## <a name="this-iteration"></a>Questa iterazione

In questa iterazione dell'applicazione Contact Manager, è effettuare il refactoring l'applicazione di usare Ajax. L'utilizzo di Ajax, si rende l'applicazione più reattiva. È possibile evitare il rendering di un'intera pagina quando è necessario aggiornare solo una determinata area geografica in una pagina.

Si sarà refactoring la visualizzazione dell'indice in modo che non abbiamo non è necessario visualizzare nuovamente l'intera pagina ogni volta che un utente seleziona un nuovo gruppo di contatto. Al contrario, quando un utente fa clic su un gruppo di contatti, è sarà sufficiente aggiornare l'elenco di contatti e lasciare solo il resto della pagina.

Verranno modificate anche la modalità di funzionamento di collegare l'eliminazione. Invece di visualizzare una pagina di conferma separato, verrà visualizzato una finestra di dialogo di conferma di JavaScript. Se si conferma che si desidera eliminare un contatto, un'operazione DELETE HTTP viene eseguita sul server per eliminare il record del contatto dal database.

Inoltre, ci si avvale di jQuery per aggiungere effetti di animazione per la visualizzazione dell'indice. Un'animazione viene visualizzato quando il nuovo elenco di contatti è recuperato dal server.

Infine, l'utente verrà reindirizzato sfruttare il supporto del framework di ASP.NET AJAX per la gestione della cronologia del browser. Punti chiave di navigazione verrà creato ogni volta che si esegue una chiamata Ajax per aggiornare l'elenco dei contatto. In questo modo, il browser con le versioni precedenti e successivi verranno di pulsanti di lavoro.

## <a name="why-use-ajax"></a>Perché utilizzare Ajax?

Utilizzo di Ajax offre numerosi vantaggi. In primo luogo, l'aggiunta di funzionalità Ajax in un'applicazione comporta una migliore esperienza utente. In un'applicazione web normale, l'intera pagina deve essere registrato al server ogni volta che un utente esegue un'azione. Ogni volta che si esegue un'azione, i blocchi di browser e l'utente deve attendere finché non viene recuperata e visualizzata di nuovo l'intera pagina.

Potrebbe trattarsi di un'esperienza accettabile in caso di un'applicazione desktop. Tuttavia, in genere, è lunga questa esperienza utente non valida nel caso di un'applicazione web perché non sappiamo che è possibile farlo qualsiasi una situazione migliore. È considerato che era stata una limitazione di applicazioni web, in realtà, è stato semplicemente una limitazione del nostro imaginations.

Non è un'applicazione Ajax, non è necessario portare l'esperienza utente arresto semplicemente per aggiornare una pagina. In alternativa, è possibile eseguire una richiesta asincrona in background per aggiornare la pagina. Non è forzato t all'utente di attesa durante la parte della pagina viene aggiornata.

L'utilizzo di Ajax, è inoltre possibile migliorare le prestazioni dell'applicazione. Prendere in considerazione l'applicazione Gestione contatti funzionamento ora senza la funzionalità Ajax. Quando si fa clic su un gruppo di contatti, la visualizzazione dell'intera indice deve essere visualizzato nuovamente. L'elenco di contatti e l'elenco di gruppi di contatti devono essere recuperati dal server di database. Tutti i dati deve essere passati attraverso la rete dal server web a web browser.

Dopo che la funzionalità Ajax per l'applicazione, è tuttavia possibile evitare rivisualizzazione l'intera pagina quando un utente fa clic su un gruppo di contatti. Non è necessario selezionare i gruppi di contatti dal database. Non abbiamo anche non è necessario eseguire il push della visualizzazione dell'intera indice attraverso la rete. L'utilizzo di Ajax, è possibile ridurre la quantità di lavoro che il server di database deve eseguire ed è possibile ridurre la quantità di traffico di rete richiesti per l'applicazione.

## <a name="don-t-be-afraid-of-ajax"></a>Tenere sempre essere termine della lezione di Ajax

Alcuni sviluppatori evitano l'utilizzo di Ajax perché sono preoccuparsi browser di livello inferiore. Si desidera assicurarsi che le applicazioni web funzionerà comunque quando vi si accede da un browser che non supporta JavaScript. Poiché Ajax dipende da JavaScript, alcuni sviluppatori evitano l'utilizzo di Ajax.

Tuttavia, se presta la dovuta attenzione sulle modalità di implementazione di Ajax è possibile compilare applicazioni che funzionano con i browser di livello inferiore e superiore. L'applicazione Contact Manager funziona con i browser che supportano JavaScript e i browser che non.

Se si utilizza l'applicazione Gestione contatti con un browser che supporta JavaScript si avrà una migliore esperienza utente. Ad esempio, quando si fa clic su un gruppo di contatti, verrà aggiornato solo l'area della pagina che visualizza i contatti.

Se, invece, si utilizza l'applicazione Gestione contatti con un browser che non supporta JavaScript (o con JavaScript disabilitato) si avrà un'esperienza utente leggermente meno convenienti. Ad esempio, quando si fa clic su un gruppo di contatti, la visualizzazione dell'intera indice da registrare al browser per visualizzare l'elenco di contatti corrispondente.

## <a name="adding-the-required-javascript-files"></a>Aggiungere i file JavaScript necessari

È necessario utilizzare tre file di JavaScript per aggiungere funzionalità Ajax per l'applicazione. Tutte e tre di questi file sono inclusi nella cartella script di una nuova applicazione MVC ASP.NET.

Se si prevede di utilizzare Ajax in più pagine dell'applicazione è consigliabile includere i file JavaScript necessari nell'applicazione s visualizzazione pagina master. In questo modo, i file JavaScript verranno automaticamente inclusa nel tutte le pagine dell'applicazione.

Aggiungere il codice JavaScript seguente include all'interno di &lt;head&gt; tag della pagina master visualizzazione:

[!code-html[Main](iteration-7-add-ajax-functionality-cs/samples/sample1.html)]

## <a name="refactoring-the-index-view-to-use-ajax"></a>Refactoring della visualizzazione dell'indice per l'utilizzo di Ajax

Consente di iniziare modificando la visualizzazione dell'indice in modo che solo facendo clic su un gruppo di contatti Aggiorna l'area di visualizzazione che elenca i contatti s. La casella di colore rossa nella figura 1 contiene l'area che si desidera aggiornare.


[![L'aggiornamento solo i contatti](iteration-7-add-ajax-functionality-cs/_static/image1.jpg)](iteration-7-add-ajax-functionality-cs/_static/image1.png)

**Figura 01**: l'aggiornamento solo i contatti ([fare clic per visualizzare l'immagine ingrandita](iteration-7-add-ajax-functionality-cs/_static/image2.png))


Il primo passaggio consiste nel separare la parte della vista che si desidera aggiornare in modo asincrono in un elemento separato parziale (controllo di visualizzazione utente). La sezione della visualizzazione di indice che viene visualizzata la tabella dei contatti è stata spostata in parziale nel listato 1.

**Elenco 1 - Views\Contact\ContactList.ascx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample2.aspx)]

Si noti che nel listato 1 parziale include un modello diverso rispetto alla visualizzazione di indice. Il *Inherits* attributo la &lt;% @ Page %&gt; direttiva specifica che parziale eredita il ViewUserControl&lt;gruppo&gt; classe.

La visualizzazione dell'indice aggiornata è contenuta nel listato 2.

**Il listato 2 - Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample3.aspx)]

Ci sono due aspetti che è opportuno notare sulla visualizzazione aggiornata nel listato 2. In primo luogo, si noti che tutto il contenuto è stato spostato in parziale viene sostituito con una chiamata a Html.RenderPartial(). Il metodo Html.RenderPartial() viene chiamato quando la visualizzazione dell'indice viene prima richiesto per visualizzare il set iniziale di contatti.

Si noti che il Html.ActionLink() utilizzato per visualizzare i gruppi di contatti in secondo luogo, sono stato sostituito con un Ajax.ActionLink(). Il Ajax.ActionLink() viene chiamato con i parametri seguenti:

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample4.aspx)]

Il primo parametro rappresenta il testo da visualizzare per il collegamento, il secondo parametro rappresenta i valori della route e il terzo parametro rappresenta le opzioni Ajax. In questo caso, è utilizzare l'opzione UpdateTargetId Ajax in modo che punti al formato HTML &lt;div&gt; tag che si desidera aggiornare una volta completata la richiesta Ajax. È necessario aggiornare il &lt;div&gt; tag con il nuovo elenco di contatti.

Il metodo aggiornato Index () del controller di contatto è contenuto nel listato 3.

**Elenco di 3 - Controllers\ContactController.cs (metodo di indice)**

[!code-csharp[Main](iteration-7-add-ajax-functionality-cs/samples/sample5.cs)]

L'azione Index () aggiornato in modo condizionale restituisce uno dei due elementi. Se l'azione Index () viene richiamato da una richiesta Ajax controller restituisce un elemento parziale. In caso contrario, l'azione Index () restituisce una vista intera.

Si noti che l'azione Index () non è necessario restituire tutti i dati quando richiamato da una richiesta Ajax. Nel contesto di una normale richiesta, l'operazione sull'indice restituisce un elenco di tutti i gruppi di contatto e il gruppo selezionato di contatto. Nel contesto di una richiesta Ajax, l'azione Index () restituisce solo il gruppo selezionato. AJAX significa meno lavoro sul server di database.

La visualizzazione dell'indice modificato funziona nel caso di browser di livello inferiore e superiore. Se si fa clic su un gruppo di contatto e il browser supporta JavaScript, viene aggiornato solo l'area della vista che contiene l'elenco di contatti. Se, invece, il browser non supporta JavaScript, viene aggiornato l'intera vista.


La visualizzazione dell'indice aggiornato è un problema. Quando si fa clic su un gruppo di contatti, il gruppo selezionato non viene evidenziato. Poiché l'elenco dei gruppi viene visualizzato all'esterno all'area in cui viene aggiornato durante una richiesta Ajax, il gruppo corretto non vengono evidenziato. Nella sezione successiva, è possibile risolvere questo problema.


## <a name="adding-jquery-animation-effects"></a>Aggiunta di effetti di animazione jQuery

In genere, quando si fa clic su un collegamento in una pagina web, è possibile utilizzare l'indicatore di stato del browser per rilevare se il browser attivamente recupera il contenuto aggiornato. Quando si esegue una richiesta Ajax, d'altra parte, l'indicatore di stato del browser non mostra qualsiasi stato di avanzamento. Ciò può rendere nervoso gli utenti. Come si può sapere se il browser è bloccato?

Esistono diversi modi che è possibile indicare a un utente che lavoro viene eseguito durante l'esecuzione di una richiesta Ajax. Un approccio consiste nel visualizzare una semplice animazione. Ad esempio, è possibile dissolvenza un'area all'inizio e la dissolvenza in entrata l'area al completamento della richiesta di una richiesta Ajax.

Verrà utilizzata la libreria jQuery è inclusa con il framework di MVC ASP.NET di Microsoft, per creare gli effetti di animazione. La visualizzazione dell'indice aggiornata è contenuta in elenco 4.

**Listato 4 - Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample6.aspx)]

Si noti che la visualizzazione dell'indice aggiornata contiene tre nuove funzioni JavaScript. Le prime due funzioni utilizzano jQuery dissolvenza e nell'elenco dei contatti di dissolvenza quando si fa clic su un nuovo gruppo di contatto. Un errore (ad esempio, un timeout di rete), la terza funzione Visualizza un messaggio di errore quando un risultati della richiesta Ajax.

Inoltre, la prima funzione si occupa di evidenziare il gruppo selezionato. Una classe = attributo selezionato viene aggiunto all'elemento padre (l'elemento LI) dell'elemento selezionato. Nuovamente, jQuery consente di selezionare l'elemento corretto e aggiungere la classe CSS.

Questi script sono collegati i collegamenti di gruppo con l'aiuto del parametro AjaxOptions Ajax.ActionLink(). La chiamata al metodo Ajax.ActionLink() aggiornata è simile al seguente:

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample7.aspx)]

## <a name="adding-browser-history-support"></a>Aggiunta del supporto della cronologia del Browser

In genere, quando si fa clic su un collegamento per aggiornare una pagina, viene aggiornata la cronologia del browser. In questo modo, è possibile fare clic sul pulsante Indietro del browser per tornare nel tempo allo stato precedente della pagina. Ad esempio, se si fare clic sul gruppo contatto con amici e quindi fare clic sul gruppo di contatto di Business, è possibile fare clic sul pulsante Indietro del browser per passare allo stato della pagina quando è stato selezionato il gruppo di amici contatto.

Purtroppo, l'esecuzione di una richiesta Ajax non aggiorna la cronologia del browser automaticamente. Se si fa clic su un gruppo di contatti e viene recuperato l'elenco di contatti di corrispondenza con una richiesta Ajax, non viene aggiornata la cronologia del browser. È possibile utilizzare il pulsante Indietro del browser per passare a un gruppo di contatto dopo aver selezionato un nuovo gruppo di contatto.

Se si desidera che gli utenti siano in grado di utilizzare il browser torna pulsante dopo aver eseguito le richieste Ajax è necessario eseguire ulteriori operazioni. È necessario sfruttare la funzionalità di gestione della cronologia del browser compilata in ASP.NET AJAX Framework.

Cronologia del browser ASP.NET AJAX, è necessario eseguire tre operazioni:

1. Abilitare la cronologia del Browser impostando la proprietà enableBrowserHistory su true.
2. Cronologia punti di salvataggio quando cambia lo stato di una vista chiamando il metodo addHistoryPoint().
3. Quando viene generato l'evento di esplorazione, ricostruire lo stato della visualizzazione.

La visualizzazione dell'indice aggiornata è contenuta nel listato 5.

**Nel listato 5 - Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample8.aspx)]

Nel listato 5, la cronologia del Browser è abilitata nella funzione pageInit(). La funzione pageInit() viene utilizzata anche per impostare il gestore eventi per l'evento di esplorazione. Ogni volta che il browser in avanti o un pulsante Indietro causa lo stato della pagina per modificare, viene generato l'evento di esplorazione.

Il metodo beginContactList() viene chiamato quando si fa clic su un gruppo di contatti. Questo metodo crea un nuovo punto di cronologia chiamando il metodo addHistoryPoint(). L'id del gruppo di contatto selezionato viene aggiunto alla cronologia.

L'id di gruppo viene recuperato da un attributo expando sul collegamento di gruppo. Il collegamento viene eseguito il rendering con la chiamata seguente a Ajax.ActionLink().

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample9.aspx)]

L'ultimo parametro passato per il Ajax.ActionLink() aggiunge un attributo expando denominato groupid al collegamento (lettere minuscole per la compatibilità XHTML).

Quando un utente raggiunge il browser torna pulsante Avanti o indietro, viene generato l'evento di esplorazione e viene chiamato il metodo navigate(). Questo metodo aggiorna i contatti visualizzati nella pagina corrispondano allo stato della pagina che corrisponde al punto di cronologia del browser passato al metodo navigate.

## <a name="performing-ajax-deletes"></a>Esecuzione di Ajax Elimina

Attualmente, per eliminare un contatto, è necessario fare clic sul collegamento Elimina e quindi fare clic sul pulsante di eliminazione visualizzato nella pagina di conferma eliminazione (vedere la figura 2). Ciò dovrebbe essere un numero elevato di richieste di pagina per eseguire un'operazione semplice come l'eliminazione di un record di database.


[![La pagina di conferma eliminazione](iteration-7-add-ajax-functionality-cs/_static/image2.jpg)](iteration-7-add-ajax-functionality-cs/_static/image3.png)

**Figura 02**: la pagina di conferma eliminazione ([fare clic per visualizzare l'immagine ingrandita](iteration-7-add-ajax-functionality-cs/_static/image4.png))


Può essere tentati di ignorare la pagina Conferma eliminazione ed eliminare un contatto direttamente dalla visualizzazione dell'indice. È consigliabile evitare questa tentazione perché questo approccio consente di aprire l'applicazione di protezione. In generale, non si t desidera eseguire un'operazione HTTP GET, quando si richiama un'azione che modifica lo stato dell'applicazione web. Quando si esegue un'operazione di eliminazione, si desidera eseguire una richiesta POST HTTP o, meglio ancora, un'operazione DELETE HTTP.

Il collegamento di eliminazione è contenuto nel ContactList parziale. Una versione aggiornata del ContactList parziale è contenuta nel listato 6.

**Elenco 6 - Views\Contact\ContactList.ascx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample10.aspx)]

Il collegamento di eliminazione viene eseguito il rendering con la seguente chiamata al metodo Ajax.ImageActionLink():

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample11.aspx)]

> [!NOTE] 
> 
> Il Ajax.ImageActionLink() non è una parte standard del framework di MVC ASP.NET. Il Ajax.ImageActionLink() è incluso nel progetto Contact Manager metodi helper personalizzati.


Il parametro AjaxOptions ha due proprietà. La proprietà di conferma prima di tutto, viene utilizzata per visualizzare una finestra di dialogo popup conferma JavaScript. In secondo luogo, la proprietà HttpMethod viene utilizzata per eseguire un'operazione DELETE HTTP.

Elenco 7 contiene una nuova azione AjaxDelete() che è stato aggiunto al controller di contatto.

**Elenco 7 - Controllers\ContactController.cs (AjaxDelete)**

[!code-csharp[Main](iteration-7-add-ajax-functionality-cs/samples/sample12.cs)]

L'azione AjaxDelete() è decorato con un attributo AcceptVerbs. Questo attributo impedisce l'azione richiamata, ad eccezione per qualsiasi operazione HTTP diverso da un'operazione DELETE HTTP. In particolare, non è possibile richiamare l'operazione con un verbo HTTP GET.

Dopo l'eliminazione di record di database, è necessario visualizzare l'elenco aggiornato dei contatti che non contiene il record eliminato. Il metodo AjaxDelete() restituisce il ContactList parziale e l'elenco aggiornato dei contatti.

## <a name="summary"></a>Riepilogo

In questa iterazione, la funzionalità Ajax è aggiunto all'applicazione Gestione contatti. Per migliorare la velocità di risposta e prestazioni dell'applicazione utilizzato Ajax.

In primo luogo, la visualizzazione dell'indice abbiamo suddiviso in modo che facendo clic su un gruppo di contatti non aggiorna l'intera vista. Al contrario, fare clic su un gruppo di contatti Aggiorna solo l'elenco di contatti.

Successivamente, abbiamo utilizzato gli effetti di animazione jQuery per dissolvenza e dissolvenza nell'elenco dei contatti. Aggiunta di animazione a un'applicazione Ajax consente di fornire agli utenti dell'applicazione con l'equivalente di una barra di stato del browser.

È inoltre aggiunto il supporto della cronologia del browser all'applicazione Ajax. È abilitato, agli utenti di fare clic su Indietro del browser pulsanti Avanti e indietro per modificare lo stato della visualizzazione dell'indice.

Infine, abbiamo creato un collegamento di eliminazione che supporta le operazioni DELETE HTTP. Eseguendo eliminazioni Ajax, è consentire agli utenti di eliminare i record di database senza richiedere all'utente di richiedere una pagina di conferma eliminazione aggiuntive.

> [!div class="step-by-step"]
> [Precedente](iteration-6-use-test-driven-development-cs.md)
> [Successivo](iteration-1-create-the-application-vb.md)
