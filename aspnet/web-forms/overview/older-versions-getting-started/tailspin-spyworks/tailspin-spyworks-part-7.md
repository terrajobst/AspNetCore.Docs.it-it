---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
title: 'Parte 7: Aggiunta di funzionalità | Microsoft Docs'
author: JoeStagner
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks. Parte 7 aggiunge funzionalità aggiuntive, ad esempio account revie...
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 50223ee9-11b9-4cf3-bca2-e2f10bf471f3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
msc.type: authoredcontent
ms.openlocfilehash: cada8d9aee649e4f2a5afc1ca2b46863ea458207
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826068"
---
<a name="part-7-adding-features"></a>Parte 7: Aggiunta di funzionalità
====================
da [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks viene illustrato come saliente è davvero semplice per creare applicazioni potenti e scalabili per la piattaforma .NET. Illustra come usare le nuove funzionalità in ASP.NET 4 per creare un negozio online, tra cui acquisti, estrazione e l'amministrazione.
> 
> Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks. Parte 7 aggiunge funzionalità aggiuntive, ad esempio la revisione di account, recensioni dei prodotti e "diffusi elementi" e controlli utente "acquistato anche".


## <a id="_Toc260221673"></a>  Aggiunta di funzionalità

Anche se gli utenti possono cercare il nostro catalogo, inserire gli elementi nel carrello e completare il processo di estrazione, esistono che numerosi che supportano le funzionalità che verrà incluso per migliorare il sito.

1. Verifica account (elenco di ordini inseriti e visualizzano i dettagli.)
2. Aggiungere alcuni contenuti specifici di contesto nella prima pagina.
3. Aggiungere una funzionalità per consentire agli utenti esaminare i prodotti nel catalogo.
4. Creare un controllo utente per visualizzare gli elementi comuni e sul posto che controllano sulla prima pagina.
5. Creare un controllo utente "Acquistato anche" e aggiungerlo alla pagina dei dettagli del prodotto.
6. Aggiungere un contatto pagina.
7. Aggiungere sulla pagina.
8. Errori globale

## <a id="_Toc260221674"></a>  Verifica account

Creare due pagine aspx OrderList.aspx denominata uno e l'altra denominata OrderDetails.aspx nella cartella "Account"

OrderList.aspx sfrutteranno i controlli GridView ed EntityDataSoure così come sono disponibili in precedenza.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample1.aspx)]

Il EntityDataSoure Seleziona record dalla tabella Orders filtro attivo su nome utente (vedere il WhereParameter) che viene impostata in una variabile di sessione quando l'accesso degli utenti.

Si noti anche questi parametri in HyperlinkField di GridView:

[!code-xml[Main](tailspin-spyworks-part-7/samples/sample2.xml)]

Questi elementi specificano il collegamento alla visualizzazione dettagli dell'ordine per ogni prodotto che specifica il campo OrderID come parametro QueryString per la pagina OrderDetails.aspx.

## <a id="_Toc260221675"></a>  OrderDetails.aspx

Si userà un controllo EntityDataSource per accedere a ordini e un controllo FormView per visualizzare i dati dell'ordine e un altro EntityDataSource con un controllo GridView per visualizzare gli elementi di riga dell'ordine.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample3.aspx)]

Nel file Code-Behind (OrderDetails.aspx.cs) sono disponibili due bit little dell'attività di manutenzione.

Innanzitutto è necessario assicurarsi che OrderDetails ottiene sempre un OrderId.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample4.cs)]

È anche necessario calcolare e visualizzare il totale di voci dell'ordine.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample5.cs)]

## <a id="_Toc260221676"></a>  Home Page

Aggiungiamo del contenuto statico alla pagina default. aspx.

Prima di tutto creerà una cartella "Content" e al suo interno una cartella immagini (e includerò un'immagine da utilizzare nella home page).

Nel segnaposto nella parte inferiore della pagina default. aspx, aggiungere il markup seguente.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample6.aspx)]

## <a id="_Toc260221677"></a>  Recensioni dei prodotti

Prima di tutto si aggiungerà un pulsante con un collegamento a un form che possiamo utilizzare per immettere una revisione di prodotto.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample7.aspx)]

![](tailspin-spyworks-part-7/_static/image1.jpg)

Si noti che il ProductID viene passato nella stringa di query

Avanti aggiungiamo pagina denominata ReviewAdd.aspx

Questa pagina userà ASP.NET AJAX Control Toolkit. Se non è già fatto in modo che è possibile scaricarlo dal [DevExpress](http://devexpress.com/act) ed è presente materiale sussidiario su come configurare il toolkit per l'uso con Visual Studio qui [ https://www.asp.net/learn/ajax-videos/video-76.aspx ](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).

Nella modalità progettazione trascinare i controlli e i validator dalla casella degli strumenti e creare un form simile a quello riportato di seguito.

![](tailspin-spyworks-part-7/_static/image2.jpg)

Il markup avrà un aspetto simile al seguente.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample8.aspx)]

Ora che è possibile immettere le revisioni, consente di visualizzare le recensioni nella pagina del prodotto.

Aggiungere questo markup alla pagina ProductDetails.aspx.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample9.aspx)]

Esegue l'applicazione presenta ora e passare a un prodotto Mostra le informazioni sul prodotto, tra cui le recensioni dei clienti.

![](tailspin-spyworks-part-7/_static/image3.jpg)

## <a id="_Toc260221678"></a>  Controllo di elementi più diffusi (creazione di controlli utente)

Per aumentare le vendite nel proprio sito web verrà aggiunto un paio di funzionalità ai prodotti più diffusi o correlati "Selling suggerite".

Il primo di tali funzionalità sarà un elenco del prodotto nel nostro catalogo dei prodotti più diffuso.

Si creerà un "controllo utente" per visualizzare gli elementi nella home page dell'applicazione più venduti. Poiché questo sarà un controllo, è possibile usare in qualsiasi pagina semplicemente trascinando e rilasciando il controllo nella finestra di progettazione di Visual Studio in qualsiasi pagina che si desidera.

In Esplora soluzioni di Visual Studio, fare doppio clic sul nome della soluzione e creare una nuova directory denominata "Controls". Anche se non è necessario eseguire questa operazione, ti aiuteremo a mantenere il progetto organizzato mediante la creazione di tutti i controlli utente nella directory "Controls".

Pulsante destro del mouse sulla cartella controlli e scegliere "Nuovo elemento":

![](tailspin-spyworks-part-7/_static/image4.jpg)

Specificare un nome per il controllo di "PopularItems". Si noti che l'estensione di file per i controlli utente ascx non. aspx.

Il controllo utente gli elementi più comunemente usati verrà definito come indicato di seguito.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample10.aspx)]

Qui utilizziamo un metodo che è stata non ancora usato in questa applicazione. Utilizziamo il controllo repeater e invece di usare un controllo origine dati viene eseguita l'associazione del controllo Repeater ai risultati di una query LINQ to Entities.

Code-behind del nostro controllo facciamo come indicato di seguito.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample11.cs)]

Si noti inoltre questa riga importante all'inizio del markup del controllo.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample12.aspx)]

Poiché gli elementi più popolari non verrà modificato in una base di un minuto a altro possiamo aggiungere una direttiva aching per migliorare le prestazioni dell'applicazione. Questa direttiva causerà il codice di controlli per essere eseguita solo quando l'output del controllo memorizzato nella cache scade. In caso contrario, verrà utilizzata la versione memorizzata nella cache dell'output del controllo.

A questo punto tutti noi dobbiamo fare è di includere il nuovo controllo nella nostra pagina Default.aspc.

Usare il trascinamento per inserire un'istanza del controllo nella colonna aprire il modulo predefinito.

![](tailspin-spyworks-part-7/_static/image5.jpg)

A questo punto quando eseguiamo l'applicazione della home page Visualizza gli elementi più popolari.

![](tailspin-spyworks-part-7/_static/image6.jpg)

## <a id="_Toc260221679"></a>  "Acquistato anche" controllare (controlli utente con parametri)

Il secondo controllo utente che verrà creata avrà suggerito al livello successivo di vendita tramite l'aggiunta di specificità di contesto.

La logica per calcolare i primi elementi "Acquistato anche" è non semplice.

Il controllo "Acquistato anche" verrà selezionare i record OrderDetails (acquistati in precedenza) per ProductID attualmente selezionato e agganciare OrderID per ogni ordine univoco che viene trovato.

Quindi si selezionerà tutti i prodotti da tutti i ordini e somma le quantità acquistate. Illustreremo per ordinare i prodotti che vengono sommate quantity e visualizzare i primi cinque elementi.

Data la complessità di questa logica, come stored procedure verrà implementato questo algoritmo.

L'istruzione T-SQL per la stored procedure è come indicato di seguito.

[!code-sql[Main](tailspin-spyworks-part-7/samples/sample13.sql)]

Si noti che questa stored procedure (SelectPurchasedWithProducts) presenti nel database quando è incluso nell'applicazione e quando viene generato l'Entity Data Model viene specificato che, oltre alle tabelle e viste che ci serviva, Entity Data Model deve includere questa stored procedure.

Per accedere alla stored procedure da Entity Data Model è necessario importare la funzione.

Fare doppio clic su Entity Data Model in Esplora soluzioni per aprirlo nella finestra di progettazione e aprire il Browser di modello, quindi fare doppio clic nella finestra di progettazione e selezionare "Aggiungi importazione di funzioni".

![](tailspin-spyworks-part-7/_static/image1.png)

In questo modo verrà aperta la finestra di dialogo.

![](tailspin-spyworks-part-7/_static/image2.png)

Compilare i campi come illustrato in precedenza, la selezione di "SelectPurchasedWithProducts" e usare il nome della routine per il nome della funzione importata.

Fare clic su "Ok".

Al termine che è semplicemente possibile programmare la stored procedure come si farebbe qualsiasi altro elemento del modello.

Quindi, nel nostro "Controls" cartella creare un nuovo controllo utente denominato AlsoPurchased.ascx.

Il markup per il controllo avrà un aspetto estremamente familiare per il controllo PopularItems.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample14.aspx)]

La differenza evidente è che non memorizza nella cache l'output in quanto il rendering dell'elemento risulteranno diversi dal prodotto.

Il ProductId sarà "property" per il controllo.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample15.cs)]

Nel gestore dell'evento PreRender del controllo è eed eseguire tre operazioni.

1. Verificare che sia impostato il ProductID.
2. Verificare se sono presenti tutti i prodotti che sono stati acquistati con quella corrente.
3. Alcuni elementi come stabilito in #2 di output.

Si noti come è facile per chiamare la stored procedure tramite il modello.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample16.cs)]

Dopo aver determinato che vi sono "acquistati anche" è sufficiente associare il controllo repeater ai risultati restituiti dalla query.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample17.cs)]

Se non erano disponibili in tutti gli elementi "acquistato anche" dal nostro catalogo verrà semplicemente visualizzato altri elementi comuni.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample18.cs)]

Per visualizzare gli elementi "Acquistato anche", aprire la pagina ProductDetails.aspx e trascinare il controllo AlsoPurchased da Esplora soluzioni in modo che venga visualizzato in questa posizione nel markup.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample19.aspx)]

In questo modo verrà creato un riferimento al controllo nella parte superiore della pagina Dettagli prodotto.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample20.aspx)]

Poiché il controllo utente AlsoPurchased richiede un numero di ID prodotto si imposterà la proprietà ProductID del nostro controllo tramite un'istruzione Eval sull'elemento del modello di dati corrente della pagina.

![](tailspin-spyworks-part-7/_static/image3.png)

Quando si compila ed Esegui ora e passare a un prodotto è visualizzare gli elementi "Acquistato anche".

![](tailspin-spyworks-part-7/_static/image7.jpg)

> [!div class="step-by-step"]
> [Precedente](tailspin-spyworks-part-6.md)
> [Successivo](tailspin-spyworks-part-8.md)
