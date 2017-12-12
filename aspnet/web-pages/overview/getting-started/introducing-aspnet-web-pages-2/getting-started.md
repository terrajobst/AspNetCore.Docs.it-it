---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
title: Introduzione a ASP.NET Web Pages - introduzione | Documenti Microsoft
author: tfitzmac
description: "WebMatrix non è più consigliato come un ambiente di sviluppo integrato per ASP.NET Web Pages. Utilizzare Visual Studio o Visual Studio Code. Questa guida un..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: a36d3bdf-ef1b-47a4-b932-3a0cf4cad716
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 615ddc31d0d857e5bf9a7f372b7efcf67d185905
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="introducing-aspnet-web-pages---getting-started"></a>Introduzione a ASP.NET Web Pages - introduzione
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> > [!NOTE] 
> > 
> > WebMatrix non è più consigliato come un ambiente di sviluppo integrato per ASP.NET Web Pages. Utilizzare [Visual Studio](../program-asp-net-web-pages-in-visual-studio.md) o [codice di Visual Studio](https://code.visualstudio.com/).
> 
> 
> Questa Guida e l'applicazione offre una panoramica di ASP.NET Web Pages (2 o versioni successive) e una sintassi Razor, un framework semplice per la creazione di siti Web dinamici. Introduce anche WebMatrix, uno strumento per la creazione di siti e pagine.
> 
> **Livello**: nuovo alle pagine Web ASP.NET.  
> **Competenze presuppone**: HTML, i fogli di stile CSS (CSS) base.
> 
> Argomenti trattati nella prima esercitazione del set di:
> 
> - Quale tecnologia ASP.NET Web Pages è e novità per.
> - Che cos'è WebMatrix.
> - Come installare tutti gli elementi.
> - Come creare un sito Web con WebMatrix.
>   
> 
> Funzionalità/tecnologie descritte:
> 
> - Installazione guidata piattaforma Web Microsoft.
> - WebMatrix.
> - *cshtml* pagine
>   
> 
> Mike Pope scritto originariamente in questa esercitazione. Tom FitzMacken aggiornato per Microsoft WebMatrix 3.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - Pagine Web ASP.NET (Razor) 2
> - WebMatrix 3


## <a name="what-should-you-know"></a>Cosa è necessario conoscere?

Si suppone che si ha familiarità con:

- **HTML**. Nessun esperienza approfondita è obbligatorio. Non verrà spiegato HTML, ma non è possibile inoltre utilizzare operazioni complesse. Forniremo collegamenti alle esercitazioni HTML in cui si ritiene che risultano utili.
- **CSS (Cascading style sheet)**. Come con HTML.
- **Concetti di database Basic**. Se hai usato un foglio di calcolo per i dati e ordinati e filtrati i dati, il livello di esperienza in genere si presuppone per questo set di esercitazione.

Si presuppone che si è interessati apprendimento della programmazione di base. Le pagine Web ASP.NET utilizzano un linguaggio di programmazione c#. Non è necessario avere le nozioni di programmazione, solo un interesse in essa contenuti. Se hai mai scritto codice JavaScript in una pagina web prima, si ha una notevole quantità di sfondo.

Si noti che se si ha familiarità con la programmazione, è possibile che inizialmente impostato in questa esercitazione si sposta lentamente mentre verrà distribuito ai programmatori di nuovo rapidamente. Come è possibile ignorare alcune esercitazioni prima, tuttavia, vi sono meno base di programmazione per spiegare e operazioni si sposta lungo in clip più veloce.

## <a name="what-do-you-need"></a>Cosa occorre?

Ecco cosa è necessario:

- Un computer che esegue Windows 8, Windows 7, Windows Server 2008 o Windows Server 2012.
- Una connessione internet attiva.
- Privilegi di amministratore (richiesto per il processo di installazione).

## <a name="what-is-aspnet-web-pages"></a>Che cos'è ASP.NET Web Pages?

Le pagine Web ASP.NET è un framework che consente di creare pagine web dinamiche. Una semplice pagina web HTML è statico. il contenuto è il markup HTML predefinito nella pagina. Le pagine dinamiche come quelli creati con ASP.NET Web Pages consentono di creare il contenuto della pagina in tempo reale, tramite il codice.

Le pagine dinamiche consentono di eseguire moltissime operazioni. È possibile chiedere a un utente per l'input tramite un modulo e quindi modificare la pagina Visualizza o l'aspetto. È possibile richiedere informazioni da un utente, salvarlo in un database ed elenco in un secondo momento. È possibile inviare tramite posta elettronica dal proprio sito. È possibile interagire con altri servizi web (ad esempio, un servizio di mapping) e creare pagine che si integrano le informazioni da tali origini.

## <a name="what-is-webmatrix"></a>Che cos'è WebMatrix?

WebMatrix è uno strumento che integra un editor di pagine web, un'utilità di database, un server web per il test, pagine e funzionalità per la pubblicazione del sito Web a Internet. WebMatrix è gratuita ed è facile da installare e facile da utilizzare. (Funziona anche per le pagine HTML semplice, nonché per altre tecnologie, ad esempio PHP.)

Non effettivamente *hanno* utilizzare WebMatrix per operare con ASP.NET Web Pages. È possibile creare pagine utilizzando un testo dell'editor, ad esempio e testare le pagine utilizzando un server web che è possibile accedere. Tuttavia, WebMatrix semplifica tutti, pertanto queste esercitazioni verranno utilizzare WebMatrix in tutto.

## <a name="about-these-tutorials"></a>Su queste esercitazioni

Set di questa esercitazione viene fornita un'introduzione a come utilizzare le pagine Web ASP.NET. Sono disponibili 9 esercitazioni totale in questo set di esercitazione introduttivo. 'S parte di una serie di insiemi di esercitazione che va dalla richiedente pagine Web ASP.NET per la creazione di siti Web professionali e reali.

In questa esercitazione prima imposta concentrati per illustrare i concetti di base sulle modalità di utilizzo con ASP.NET Web Pages. Al termine, per lavorare con set aggiuntivi di esercitazione che prelevati in cui termina questa e di esplorare pagine Web in modo più approfondito.

È deliberatamente andare facile spiegazioni dettagliate. E ogni volta che si visualizza un elemento, per questa esercitazione set è sempre scegliere il modo in cui si ritiene che è più facile da comprendere. Set di esercitazione successivo andare in maniera più approfondita e mostrano gli approcci più efficienti o più flessibili (anche divertente). Ma queste esercitazioni è necessario comprendere in primo luogo le nozioni di base.

Il set di esercitazione che è stato appena avviato descrive queste funzionalità di ASP.NET Web Pages:

- Introduzione e recupero di tutti gli elementi installati. (Che è nell'esercitazione che si sta leggendo).
- Basi della programmazione di ASP.NET Web Pages.
- Creazione di un database.
- La creazione e l'elaborazione di un modulo di input utente.
- Aggiunta, aggiornamento ed eliminazione dei dati nel database.

## <a name="what-will-you-create"></a>Cosa sarà necessario creare?

Imposta questa esercitazione e quelli successivi riguardano un sito Web in cui è possibile elencare filmati che si desidera. Sarà in grado di immettere film, modificarli e visualizzarne l'elenco. Ecco un paio di pagine di cui che si creerà in questa esercitazione set. Il primo viene illustrato il film visualizzazione pagina in cui verrà creato:

![È stato completato film app che mostra un elenco di film](getting-started/_static/image1.png)

Di seguito è la pagina che consente di aggiungere nuove informazioni di film al sito:

![App filmato che illustra la pagina Aggiungi film](getting-started/_static/image2.png)

Compilazione di serie di esercitazioni successive su questo set e aggiungere ulteriori funzionalità, ad esempio il caricamento di immagini, consentendo agli utenti di accedere, l'invio di posta elettronica e l'integrazione con i social network.

## <a name="see-this-app-running-on-azure"></a>Vedere l'App in esecuzione in Azure

Si desidera vedere il sito completo in esecuzione come un'app web in tempo reale? È possibile distribuire una versione completa dell'app al proprio account Azure, facendo semplicemente clic sul pulsante seguente.

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebPagesMovies)

È necessario un account Azure per distribuire questa soluzione in Azure. Se si dispone già di un account, sono disponibili le opzioni seguenti:

- [Aprire un account Azure, gratuitamente](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A443DD604) -si ottiene crediti è possibile utilizzare per provare i servizi di Azure a pagamento e anche dopo l'uso massimo è possibile mantenere l'account e utilizzare senza servizi di Azure.
- [Attivare i benefici per sottoscrittori MSDN](https://azure.microsoft.com/en-us/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -Your sottoscrizione MSDN offre crediti ogni mese in cui è possibile utilizzare per i servizi di Azure a pagamento.

## <a name="installing-everything"></a>L'installazione di tutti gli elementi

È possibile installare tutti gli elementi utilizzando l'installazione guidata piattaforma Web Microsoft. In effetti, installare il programma di installazione e quindi utilizzarlo per installare tutti gli altri elementi.

Per utilizzare le pagine Web, è necessario disporre almeno Windows XP con SP3 installato, o Windows Server 2008 o versione successiva.

Nel [pagina Web Pages](../../../index.md) del sito Web ASP.NET, fare clic su **installare**.

![Visualizzazione del sito Web ASP.NET &quot;installa WebMatrix&quot; pulsante](getting-started/_static/image3.png)

È necessario accettare le condizioni di licenza e l'informativa sulla privacy prima dell'installazione di WebMatrix.

![accettare i termini per iniziare l'installazione](getting-started/_static/image4.png)

Fare clic su **eseguire** per avviare l'installazione. (Se si desidera salvare il programma di installazione, fare clic su **salvare** e quindi eseguire il programma di installazione dalla cartella in cui è stata salvata.)

![](getting-started/_static/image5.png)

Viene visualizzata l'installazione guidata piattaforma Web, pronto per l'installazione di WebMatrix. Fare clic su **Installa**.

![](getting-started/_static/image6.png)

Il processo di installazione determina quali è installato nel computer e avvia il processo. A seconda esattamente ciò che deve essere installato, il processo può richiedere ovunque da qualche minuto per alcuni minuti. Selezionare **accetto** per accettare le condizioni di licenza.

## <a name="hello-webmatrix"></a>Hello, WebMatrix

Al termine, il processo di installazione può avviare automaticamente WebMatrix. In caso contrario, in Windows, dal **avviare** menu, avviare **Microsoft WebMatrix**.

Quando si avvia WebMatrix per la prima volta, viene offerta la possibilità di accedere a Microsoft Azure con l'account Microsoft. Effettuando l'accesso, si riceverà 10 App web gratuito tramite Azure. Queste App web gratuito forniscono un modo pratico per testare le app. Se si dispone già di un account Azure, ma si dispone di una sottoscrizione MSDN, è possibile [attivare i vantaggi dell'abbonamento MSDN](https://www.windowsazure.com/en-us/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604). In caso contrario, è possibile creare un account di valutazione gratuito in pochi minuti. Per informazioni dettagliate, vedere [versione di valutazione gratuita di Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

Non è necessario accedere adesso per proseguire questa esercitazione. Se non riesci a questo punto, è possibile accedere in seguito sarà comunque necessario. L'ultimo [argomento](publishing.md) in questa esercitazione serie viene descritto come distribuire il sito Web in Azure; pertanto, è necessario accedere per completare questo argomento.

A questo punto, firmare con l'account Microsoft o selezionare **non ora** nell'angolo inferiore destro.

![Accedi](getting-started/_static/image7.png)

Per iniziare, si verrà creato un sito Web vuoto e aggiungere una pagina. In un'esercitazione successiva nel set verranno riprodotti con uno dei modelli di sito Web predefinito.

Nella finestra di avvio, fare clic su **New**.

![Schermata di avvio di WebMatrix](getting-started/_static/image8.png)

I modelli sono file predefiniti e pagine per tipi diversi di siti Web. Per visualizzare tutti i modelli disponibili per impostazione predefinita, selezionare l'opzione di raccolta di modelli.

![Raccolta di modelli selezionare](getting-started/_static/image9.png)

Nel **avvio rapido** selezionare **sito vuoto** dal **ASP.NET** gruppo e denominare il nuovo sito "WebPagesMovies".

![Finestra di avvio rapido di WebMatrix con modello di sito vuoto selezionato](getting-started/_static/image10.png)

Scegliere **Avanti**.

Se hanno effettuato l'accesso al tuo account Microsoft, si avrà la possibilità di creare il sito in Azure. In base al nome del sito, il nome predefinito di **WebPagesMovies.azurewebsites.net** viene suggerito; tuttavia, il punto esclamativo indica che questo nome non è disponibile in Windows Azure. Per semplicità, selezionare **Skip** per ignorare la creazione del sito web in Azure ora. Più avanti in questa serie, il sito verrà pubblicato in Azure.

![creare il sito di azure](getting-started/_static/image11.png)

WebMatrix crea e apre il sito:

![Nuovo sito WebPagesMovies aperti in WebMatrix](getting-started/_static/image12.png)

Nella parte superiore, è presente una barra di accesso e una barra multifunzione. In basso a sinistra, viene visualizzato il selettore dell'area di lavoro in cui si passa tra le attività (**sito**, **file**, **database**, **report**). Nella parte destra è il riquadro del contenuto per l'editor e i report. E nella parte inferiore verrà visualizzato in alcuni casi una barra di notifica per i messaggi.

Si apprenderà più su WebMatrix e delle relative funzionalità mentre si eseguono queste esercitazioni.

## <a name="creating-a-web-page"></a>Creazione di una pagina Web

Per acquisire familiarità con WebMatrix e ASP.NET Web Pages, si creeranno una semplice pagina.

Il selettore dell'area di lavoro, selezionare il **file** dell'area di lavoro. L'area di lavoro consente di collaborare con i file e cartelle. Il riquadro a sinistra mostra la struttura dei file del sito. La barra multifunzione cambia per mostrare le attività relative ai file.

![File area di lavoro di WebMatrix](getting-started/_static/image13.png)

Nella barra multifunzione, fare clic sulla freccia sotto **New** e quindi fare clic su **nuovo File**.

![Utilizzo di &quot;New&quot; comando sulla barra multifunzione per creare un nuovo file](getting-started/_static/image14.png)

WebMatrix consente di visualizzare un elenco di tipi di file. Selezionare **CSHTML**e il **nome** , digitare "HelloWorld". Una pagina CSHTML è una pagina ASP.NET Web Pages.

![Creazione di una nuova pagina CSHTML denominato HelloWorld.cshtml](getting-started/_static/image15.png)

Fare clic su **OK**.

WebMatrix crea la pagina e verrà aperto nell'editor.

![La nuova pagina HelloWorld nell'editor di WebMatrix](getting-started/_static/image16.png)

Come si può notare, la pagina contiene principalmente normale codice HTML, ad eccezione di un blocco nella parte superiore che è simile al seguente:

[!code-cshtml[Main](getting-started/samples/sample1.cshtml)]

Che per l'aggiunta di codice, come si vedrà a breve.

Si noti che le diverse parti della pagina &mdash; i nomi degli elementi, attributi e testo, oltre il blocco nella parte superiore, sono tutti in colori diversi. Si tratta di *evidenziazione della sintassi*, e rende più semplice mantenere tutti gli elementi non crittografato. È una delle funzionalità che rende più semplice utilizzare le pagine web in WebMatrix.

Aggiungere il contenuto per il `<head>` e `<body>` elementi come nell'esempio seguente. (Se si desidera, è possibile semplicemente copiare il blocco seguente e sostituire l'intera pagina esistente con questo codice.)

[!code-cshtml[Main](getting-started/samples/sample2.cshtml)]

Nella barra di accesso rapido o nella **File** menu, fare clic su **salvare**.

![Pulsante Salva nella barra di accesso rapido di WebMatrix](getting-started/_static/image17.png)

## <a name="testing-the-page"></a>Test della pagina

Nel **file** area di lavoro, fare doppio clic su di *HelloWorld.cshtml* pagina e quindi fare clic su **Avvia nel browser**.

![Esecuzione di una pagina con il pulsante Esegui sulla barra multifunzione di WebMatrix](getting-started/_static/image18.png)

WebMatrix avvia un server web incorporato, IIS Express, che è possibile utilizzare per testare le pagine nel computer in uso. (Senza IIS Express in WebMatrix, è necessario pubblicare la pagina a un server web in un punto prima è stato possibile eseguirne il test.) Verrà visualizzata la pagina nel browser predefinito.

![&quot;Hello World&quot; pagina in esecuzione nel browser](getting-started/_static/image19.png)

Si noti che quando si testa una pagina in WebMatrix, l'URL nel browser è simile al seguente `http://localhost:33651/HelloWorld.cshtml.` nome *localhost* fa riferimento a un server locale, vale a dire che la pagina è utilizzata da un server web nel proprio computer. Come già indicato, WebMatrix include un programma del server web denominato IIS Express che viene eseguito all'avvio di una pagina.

Il numero dopo *localhost* (ad esempio, *localhost:33651*) fa riferimento a un *il numero di porta* nel computer in uso. Questo è il numero di "canale" utilizzata per questo determinato sito Web IIS Express. Quando si crea un sito in modo diverso per ogni sito creato, il numero di porta è selezionato in modo casuale dall'intervallo tra 1024 e 65536. (Quando si testa il proprio sito, il numero di porta verrà quasi certamente essere un numero diverso da quello 33561.) Utilizzando una porta diversa per ogni sito Web, IIS Express consente di mantenere semplici quali siti con cui sta comunicando.

In seguito quando si pubblica il sito a un server web pubblico, non si visualizzano più *localhost* nell'URL. A questo punto, verrà visualizzato un URL più comune di comunicazione come `http://myhostingsite/mywebsite/HelloWorld.cshtml` o qualsiasi della pagina. Verranno fornite ulteriori informazioni sulla pubblicazione di un sito più avanti in questa serie di esercitazioni.

## <a name="adding-some-server-side-code"></a>Aggiunta di codice lato Server

Chiudere il browser e tornare alla pagina in WebMatrix.

Aggiungere una riga per il blocco di codice in modo che risulti simile al codice seguente:

[!code-cshtml[Main](getting-started/samples/sample3.cshtml)]

Questo è un po' di codice Razor. È probabile che chiaro che ottiene la data e ora correnti e inserire tale valore in un *variabile* denominato `currentDateTime`. È possibile leggere altre informazioni sulla sintassi Razor nella prossima esercitazione.

Nel corpo della pagina, dopo il `<p>Hello World!</p>` elemento, aggiungere quanto segue:

[!code-html[Main](getting-started/samples/sample4.html)]

Questo codice ottiene il valore inserito nella `currentDateTime` variabile nella parte superiore e lo inserisce nel markup della pagina. Il `@` carattere contrassegna il codice ASP.NET Web Pages nella pagina.

Eseguire nuovamente la pagina (WebMatrix consente di salvare le modifiche per la prima dell'esecuzione della pagina). Questa volta che viene visualizzata la data e ora nella pagina.

![&quot;Hello World&quot; pagina in esecuzione nel browser con un display ora generata in modo dinamico](getting-started/_static/image20.png)

Attendere qualche minuto e quindi aggiornare la pagina nel browser. La visualizzazione di data e ora viene aggiornata.

Esaminare l'origine della pagina nel browser. Sembra che il markup seguente:

[!code-html[Main](getting-started/samples/sample5.html)]

Si noti che il `@{ }` blocco all'inizio non è presente. Si noti inoltre che data e ora viene visualizzata una stringa di caratteri effettivi (`1/18/2012 2:49:50 PM` o qualunque), non `@currentDateTime` come era *. cshtml* pagina. Dov'è qui che al momento dell'esecuzione della pagina, ASP.NET elaborato tutto il codice (limitata in questo caso) che è stato contrassegnato con `@`. Output del codice e che l'output è stato inserito nella pagina.

## <a name="this-is-what-aspnet-web-pages-are-about"></a>Si tratta di che cosa sono le pagine Web ASP.NET su

Quando si leggono che ASP.NET Web Pages produce contenuti web dinamici, ciò che si è visto è l'idea. La pagina che appena creato contiene lo stesso markup HTML che si è visto prima. Può inoltre contenere codice che è possibile eseguire tutti i tipi di attività. In questo esempio, ha il compito di ottenere la data e ora correnti. Come si è visto, è possibile alternare codice con il codice HTML per produrre l'output della pagina. Quando un utente richiede un *. cshtml* pagina nel browser, ASP.NET elabora la pagina mentre è ancora in mani del server web. ASP.NET inserisce l'output del codice (se presente) nella pagina HTML. Quando viene eseguita l'elaborazione di codice, ASP.NET Invia pagina risultante nel browser. Tutti i browser venga è HTML. Di seguito è riportato un diagramma:

![Flusso concettuale di modalità ASP.NET genera l'errore HTML in modo dinamico](getting-started/_static/image21.png)

L'idea è semplice, ma esistono molte attività interessanti che è possibile eseguire il codice e sono disponibili molti modi interessanti in cui è possibile aggiungere contenuto HTML in modo dinamico alla pagina. ASP.NET e *. cshtml* pagine, come in qualsiasi pagina HTML, possono anche includere codice che viene eseguito nel browser stesso (codice JavaScript e jQuery). Si esamineranno tutte queste operazioni in questa esercitazione set e in quelli successivi.

## <a name="coming-up-next"></a>Esercitazione seguente

Nella prossima esercitazione di questa serie, è esplorare pagine Web ASP.NET di programmazione un po' più.

## <a name="additional-resources"></a>Risorse aggiuntive

[Creare un sito Web ASP.NET da zero](https://www.microsoft.com/web/post/create-an-aspnet-website-from-scratch). Si tratta di un'esercitazione in cui è in particolare sull'utilizzo di WebMatrix (non ASP.NET Web Pages). Diventa un po' più dettagli su alcune delle funzionalità aggiuntive di WebMatrix che non verranno trattati in questa esercitazione set.

>[!div class="step-by-step"]
[Successivo](intro-to-web-pages-programming.md)
