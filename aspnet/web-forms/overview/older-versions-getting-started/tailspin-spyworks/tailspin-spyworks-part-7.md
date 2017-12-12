---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
title: "Parte 7: Aggiunta di funzionalità | Documenti Microsoft"
author: JoeStagner
description: "Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks. Parte 7 aggiunge funzionalità aggiuntive, ad esempio account revie..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 50223ee9-11b9-4cf3-bca2-e2f10bf471f3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
msc.type: authoredcontent
ms.openlocfilehash: 5280de44b3e75f9d1ae85e0248bc3ef6d5444f6d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="part-7-adding-features"></a>Parte 7: Aggiunta di funzionalità
====================
da [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks viene illustrato come particolarmente semplice è creare potenti applicazioni scalabili per la piattaforma .NET. Illustra come usare le nuove caratteristiche in ASP.NET 4 per creare un archivio online, inclusi gli acquisti, estrazione e l'amministrazione.
> 
> Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks. Parte 7 aggiunge funzionalità aggiuntive, come la revisione di account, recensioni dei prodotti e "elementi comuni" e controlli utente "anche acquistato".


## <a id="_Toc260221673"></a>Aggiunta di funzionalità

Se gli utenti possono cercare il catalogo, inserire gli elementi nel carrello acquisti e completare il processo di estrazione, sono disponibili che funzionalità di un numero di supporto che verrà incluso per migliorare il sito.

1. Revisione di account (elenco di ordini inseriti e visualizzano i dettagli.)
2. Aggiungere alcuni contenuti specifici di contesto nella prima pagina.
3. Aggiungere una funzionalità per consentire agli utenti di esaminare i prodotti nel catalogo.
4. Creare un controllo utente per visualizzare gli elementi comuni e sul posto che controllano sulla prima pagina.
5. Creare un controllo utente "Anche acquistato" e aggiungerlo alla pagina dei dettagli del prodotto.
6. Aggiungere un contatto pagina.
7. Aggiungere una pagina.
8. Errore globale

## <a id="_Toc260221674"></a>Revisione di account

Nella cartella "Account", creare due pagine aspx OrderList.aspx denominata uno e l'altra denominata OrderDetails.aspx

OrderList.aspx verranno utilizzati i controlli GridView ed EntityDataSoure così come è stato precedentemente definito.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample1.aspx)]

Il EntityDataSoure seleziona i record della tabella Orders filtrati al nome utente (vedere il WhereParameter) che è impostato in una variabile di sessione quando all'utente di accedere.

Si noti inoltre questi parametri, HyperlinkField di GridView:

[!code-xml[Main](tailspin-spyworks-part-7/samples/sample2.xml)]

Il collegamento alla visualizzazione Dettagli ordine per ogni prodotto specificando il campo OrderID come parametro QueryString per la pagina OrderDetails.aspx specificano.

## <a id="_Toc260221675"></a>OrderDetails.aspx

Un controllo EntityDataSource utilizzeremo per accedere gli ordini e un controllo FormView per visualizzare i dati dell'ordine e un altro EntityDataSource con un controllo GridView per visualizzare le voci dell'ordine.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample3.aspx)]

Nel file Code-Behind (OrderDetails.aspx.cs) sono disponibili due bit minimo di manutenzione.

È innanzitutto necessario assicurarsi che OrderDetails ottiene sempre un OrderId.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample4.cs)]

È anche necessario calcolare e visualizzare il totale di voci dell'ordine.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample5.cs)]

## <a id="_Toc260221676"></a>Home Page

Aggiungere contenuto statico alla pagina Default.aspx.

Prima di tutto creare una cartella "Contenuta" e al suo interno una cartella immagini (e possibile includere un'immagine da utilizzare nella home page).

Nel segnaposto nella parte inferiore della pagina aspx, aggiungere il markup seguente.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample6.aspx)]

## <a id="_Toc260221677"></a>Revisioni dei prodotti

Innanzitutto verrà aggiunto un pulsante con un collegamento a un form che è possibile utilizzare per immettere una revisione del prodotto.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample7.aspx)]

![](tailspin-spyworks-part-7/_static/image1.jpg)

Si noti che viene passato il ProductID nella stringa di query

Avanti aggiungere pagina denominata ReviewAdd.aspx

Questa pagina verrà utilizza ASP.NET AJAX Control Toolkit. Se non si è già connessi in modo è possibile scaricarlo da [DevExpress](http://devexpress.com/act) e indicazioni su come configurare il toolkit per l'uso con Visual Studio qui [https://www.asp.net/learn/ajax-videos/video-76.aspx](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).

Nella modalità progettazione, trascinare i controlli e validator dalla casella degli strumenti e creare un form simile a quello riportato di seguito.

![](tailspin-spyworks-part-7/_static/image2.jpg)

Il codice sarà simile al seguente.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample8.aspx)]

Ora che è possibile immettere le revisioni, consente di visualizzare le revisioni nella pagina del prodotto.

Aggiungere il markup alla pagina ProductDetails.aspx.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample9.aspx)]

In esecuzione l'applicazione ora e passare a un prodotto Mostra le informazioni sul prodotto, incluse le recensioni dei clienti.

![](tailspin-spyworks-part-7/_static/image3.jpg)

## <a id="_Toc260221678"></a>Controllo di elementi comuni (creazione di controlli utente)

Per aumentare le vendite nel sito web verrà aggiunto un paio di funzionalità per i prodotti più diffusi o correlati "vendere suggerite".

Il primo di queste funzionalità saranno un elenco del prodotto più popolare il catalogo dei prodotti.

Si creerà un "controllo utente" per visualizzare gli elementi nella home page dell'applicazione più venduti. Poiché questo sarà un controllo, è possibile utilizzare in qualsiasi pagina semplicemente trascinando e rilasciando il controllo nella finestra di progettazione di Visual Studio in qualsiasi pagina che si desidera.

In Esplora soluzioni di Visual Studio, fare clic sul nome della soluzione e creare una nuova directory denominata "Controlli". Mentre non è necessario eseguire questa operazione, si consente di mantenere il progetto organizzato mediante la creazione di tutti i controlli utente nella directory "Controlli".

Pulsante destro del mouse sulla cartella controlli e scegliere "New Item":

![](tailspin-spyworks-part-7/_static/image4.jpg)

Specificare un nome per il controllo di "PopularItems". Si noti che l'estensione di file per i controlli utente ascx non aspx.

Il controllo utente comune di elementi verrà definito come indicato di seguito.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample10.aspx)]

In questo caso si usa un metodo che è stato non ancora usato in questa applicazione. Utilizziamo controllo repeater e invece di usare un controllo origine dati si sta associando il controllo Repeater ai risultati di una query LINQ to Entities.

Nel code-behind di questo controllo è eseguire questa operazione come indicato di seguito.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample11.cs)]

Si noti inoltre questa riga importante nella parte superiore del markup del controllo.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample12.aspx)]

Poiché gli elementi più diffusi verrà modificato in una base di un minuto a altro è possibile aggiungere una direttiva aching per migliorare le prestazioni dell'applicazione. Questa direttiva causerà il codice di controlli per essere eseguita solo quando l'output del controllo memorizzati nella cache scade. In caso contrario, verrà utilizzata la versione memorizzata nella cache di output del controllo.

A questo punto è sufficiente includere il nuovo controllo nella nostra pagina Default.aspc.

Utilizzare trascinamento della selezione per inserire un'istanza del controllo nella colonna aperta il modulo predefinito.

![](tailspin-spyworks-part-7/_static/image5.jpg)

A questo punto quando si esegue l'applicazione della home page Visualizza gli elementi più comuni.

![](tailspin-spyworks-part-7/_static/image6.jpg)

## <a id="_Toc260221679"></a>"Anche acquistato" controllare (controlli utente con parametri)

Il secondo controllo utente che verrà creata richiederà suggerito aggiungendo specificità contesto delle vendite al livello successivo.

La logica per calcolare i primi elementi "Anche acquistato" è non semplice.

Del controllo "Anche acquistato" verrà selezionare i record OrderDetails (acquistati in precedenza) per il ProductID attualmente selezionato e agganciare i OrderIDs per ogni ordine univoco che viene trovato.

Quindi si selezionerà tutti i prodotti da tutti i ordini e somma le quantità acquistate. Viene ordinare i prodotti per la somma delle quantità e visualizzare i primi cinque elementi.

Data la complessità di questa logica, come una stored procedure verrà implementato questo algoritmo.

L'istruzione T-SQL per la stored procedure è come indicato di seguito.

[!code-sql[Main](tailspin-spyworks-part-7/samples/sample13.sql)]

Si noti che questa stored procedure (SelectPurchasedWithProducts) devono essere presenti nel database, quando sono inclusi nell'applicazione e quando viene generato è specificato che, oltre alle tabelle e viste che è necessario, Entity Data Model Entity Data Model deve includere questa stored procedure.

Per accedere alla stored procedure da Entity Data Model, è necessario importare la funzione.

Fare doppio clic su Entity Data Model in Esplora soluzioni per aprirlo nella finestra di progettazione e aprire il Browser modello, quindi fare doppio clic nella finestra di progettazione e selezionare "Aggiungi funzione importazione".

![](tailspin-spyworks-part-7/_static/image1.png)

In questo modo verrà aperta la finestra di dialogo.

![](tailspin-spyworks-part-7/_static/image2.png)

Compilare i campi come visualizzato in precedenza, la selezione di "SelectPurchasedWithProducts" e utilizzare il nome della routine per il nome del nostro funzione importata.

Fare clic su "Ok".

Al termine che è semplicemente possibile programmare la stored procedure come si farebbe qualsiasi altro elemento del modello.

In tal caso, la cartella "Controlli" creare un nuovo controllo utente denominato AlsoPurchased.ascx.

Il markup per il controllo sarà molto familiare per il controllo PopularItems.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample14.aspx)]

La differenza evidente è che non memorizza nella cache l'output dopo l'esecuzione del rendering dell'elemento sarà diversa dal prodotto.

ProductId sarà "property" per il controllo.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample15.cs)]

Nel gestore di evento PreRender del controllo è eed per eseguire tre operazioni.

1. Verificare che sia impostato il ProductID.
2. Verificare se sono presenti tutti i prodotti che sono stati acquistati con quella corrente.
3. L'output di alcuni elementi, come determinato # 2.

Si noti quanto sia facile per chiamare la stored procedure tramite il modello.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample16.cs)]

Dopo aver determinato che non esiste "inoltre acquistati" Ripetitore semplicemente possibile associare ai risultati restituiti dalla query.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample17.cs)]

Se non erano disponibili in tutti gli elementi "anche acquistato" è semplicemente verranno visualizzati altri elementi comuni dal nostro catalogo.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample18.cs)]

Per visualizzare gli elementi "Anche acquistato", aprire la pagina ProductDetails.aspx e trascinare il controllo AlsoPurchased da Esplora soluzioni, in modo che venga visualizzato in questa posizione nel markup.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample19.aspx)]

In questo modo verrà creato un riferimento al controllo nella parte superiore della pagina Dettagli prodotto.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample20.aspx)]

Poiché il controllo utente AlsoPurchased richiede un numero di ID prodotto si imposterà la proprietà ProductID del controllo utilizzando un'istruzione Eval sull'elemento del modello di dati corrente della pagina.

![](tailspin-spyworks-part-7/_static/image3.png)

Quando si compila ed Esegui ora e passare a un prodotto è visualizzare gli elementi "Anche acquistato".

![](tailspin-spyworks-part-7/_static/image7.jpg)

>[!div class="step-by-step"]
[Precedente](tailspin-spyworks-part-6.md)
[Successivo](tailspin-spyworks-part-8.md)
