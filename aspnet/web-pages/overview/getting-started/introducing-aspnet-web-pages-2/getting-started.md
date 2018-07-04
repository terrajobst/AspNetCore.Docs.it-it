---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
title: Introduzione a pagine Web ASP.NET - guida Introduzione | Microsoft Docs
author: tfitzmac
description: WebMatrix non è più consigliata come ambiente di sviluppo integrato per ASP.NET Web Pages. Usare Visual Studio o Visual Studio Code. Questo materiale sussidiario un...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: a36d3bdf-ef1b-47a4-b932-3a0cf4cad716
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 3717fe230f818f251ac5d0378701c28f6b427df8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382072"
---
<a name="introducing-aspnet-web-pages---getting-started"></a>Introduzione a pagine Web ASP.NET - introduzione
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> > [!NOTE] 
> > 
> > WebMatrix non è più consigliata come ambiente di sviluppo integrato per ASP.NET Web Pages. Uso [Visual Studio](../program-asp-net-web-pages-in-visual-studio.md) oppure [Visual Studio Code](https://code.visualstudio.com/).
> 
> 
> Questo materiale sussidiario e applicazione offre una panoramica di ASP.NET Web Pages (2 o versioni successive) e la sintassi Razor, un framework leggero per la creazione di siti Web dinamici. Offre anche WebMatrix, uno strumento per la creazione di siti e pagine.
> 
> **Livello**: novità di ASP.NET Web Pages.  
> **Si presuppone che le competenze**: HTML, i fogli di stile a cascata (CSS) base.
> 
> Contenuto dell'esercitazione nella prima esercitazione del set di:
> 
> - Quale tecnologia ASP.NET Web Pages è e cosa serve.
> - Che cos'è WebMatrix.
> - Come installare tutti gli elementi.
> - Come creare un sito Web con WebMatrix.
>   
> 
> Le funzionalità/tecnologie illustrate:
> 
> - Installazione guidata piattaforma Web Microsoft.
> - WebMatrix.
> - *con estensione cshtml* pagine
>   
> 
> Mike Pope originariamente scrisse in questa esercitazione. Tom FitzMacken aggiornato per Microsoft WebMatrix 3.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 3


## <a name="what-should-you-know"></a>Cosa è necessario sapere?

Si presuppone che si abbia familiarità con:

- **HTML**. Non è necessaria alcuna competenza approfondita. Senza entrare in spiegazioni HTML, ma non è possibile inoltre utilizzare operazioni complesse. Forniremo collegamenti alle esercitazioni HTML in cui si pensa che risultano utili.
- **CSS (Cascading style sheet)**. Uguale a con il linguaggio HTML.
- **Concetti di base dei database**. Se è stato usato un foglio di calcolo per i dati e ordinata e filtrata di dati, ad esempio il livello di esperienza in genere si presuppone per questa serie di esercitazioni.

Si presuppone che si è interessati alla programmazione di base di apprendimento. ASP.NET Web Pages utilizza un linguaggio di programmazione c# denominato. Non è necessario avere le nozioni di programmazione, solo un interesse a riguardo. Se hai mai scritto codice JavaScript in una pagina web prima di, hai una vasta gamma di sfondo.

Si noti che se si ha familiarità con la programmazione, si noterà che questa esercitazione è stata impostata inizialmente aumenta lentamente mentre vengono erogate accelerare i programmatori non esperti. Man mano che oltre le esercitazioni di alcuni prima, però, sarà presente a meno di base di programmazione per spiegare e cose si sposta lungo in una clip più veloce.

## <a name="what-do-you-need"></a>Che cosa serve?

Ecco cosa è necessario:

- Un computer che esegue Windows 8, Windows 7, Windows Server 2008 o Windows Server 2012.
- Una connessione internet attiva.
- Privilegi di amministratore (richiesto per il processo di installazione).

## <a name="what-is-aspnet-web-pages"></a>Che cos'è ASP.NET Web Pages?

ASP.NET Web Pages è un framework che è possibile usare per creare pagine web dinamiche. Una semplice pagina web HTML è statico. il contenuto è determinato dal markup HTML predefinito è nella pagina. Le pagine dinamiche, ad esempio quelli creati con ASP.NET Web Pages consentono di creare il contenuto della pagina in tempo reale, tramite codice.

Le pagine dinamiche consentono di eseguire moltissime operazioni. È possibile chiedere a un utente per l'input tramite un modulo e quindi modificare la pagina Visualizza o l'aspetto. È possibile richiedere informazioni da un utente, salvarlo in un database e quindi pubblicarla in un secondo momento. È possibile inviare tramite posta elettronica dal proprio sito. È possibile interagire con altri servizi nel web (ad esempio, un servizio di mapping) e generare le pagine che si integrano le informazioni da tali origini.

## <a name="what-is-webmatrix"></a>Che cos'è WebMatrix?

WebMatrix è uno strumento che integra un editor di pagine web, un'utilità di database, un server web per il test, pagine e funzionalità per la pubblicazione del sito Web a Internet. WebMatrix è gratuita ed è facile da installare e facile da usare. (Funziona anche per semplici pagine HTML, nonché per altre tecnologie come PHP.)

Non è in realtà *hanno* usare WebMatrix per lavorare con ASP.NET Web Pages. È possibile creare pagine con un testo dell'editor, ad esempio e testare le pagine usando un server web che è possibile utilizzare. Tuttavia, WebMatrix è tutto molto semplice, in modo che queste esercitazioni userà WebMatrix in tutto.

## <a name="about-these-tutorials"></a>Su queste esercitazioni

Questa serie di esercitazioni è un'introduzione a come utilizzare ASP.NET Web Pages. Sono disponibili 9 esercitazioni totale in questa serie di esercitazioni introduttiva. Relativo parte di una serie di set di esercitazioni che va dalla meno esperti di ASP.NET Web Pages alla creazione di siti Web reali, dall'aspetto professionale.

Questa prima esercitazione di concentrati è nastavit che illustra le nozioni di base dell'uso con ASP.NET Web Pages. Al termine, è possibile lavorare con set di esercitazioni aggiuntive prelevare dove termina questo e di esplorare le pagine Web in modo più approfondito.

È deliberatamente passare facilmente le spiegazioni dettagliate. E ogni volta che viene illustrato un elemento, per questa serie di esercitazioni è sempre scegliere il modo in cui si pensa è più facile da comprendere. Set di esercitazioni successive approfondiscono e descrive gli approcci più flessibile o più efficienti (anche più divertente). Ma tali esercitazioni richiedono la comprensione prima di tutto le nozioni di base.

La serie di esercitazioni che appena avviata descrive queste funzionalità di ASP.NET Web Pages:

- Introduzione e tutto ciò è installato. (Che è in questa esercitazione che si sta leggendo).
- Le nozioni di base della programmazione di ASP.NET Web Pages.
- Creazione di un database.
- La creazione e l'elaborazione di un modulo di input utente.
- Aggiunta, aggiornamento ed eliminazione dei dati nel database.

## <a name="what-will-you-create"></a>Che cosa sarà necessario creare?

Imposta questa esercitazione e le successive ruotano intorno a un sito Web in cui è possibile elencare i film che si desidera. Sarà in grado di immettere film, modificarli e visualizzarne l'elenco. Ecco un paio di pagine di cui che si creerà in questa serie di esercitazioni. Il primo screenshot Mostra il film listato pagina che verrà creata:

![Impostandolo Movie app che mostra un elenco di film](getting-started/_static/image1.png)

Ed ecco la pagina che consente di aggiungere nuove informazioni relative al filmato al sito:

![App completata film che mostra la pagina Aggiungi film](getting-started/_static/image2.png)

Compilazione di set di esercitazioni successive in questo set e aggiungere ulteriori funzionalità, quali il caricamento di immagini, consentire agli utenti l'accesso, l'invio di posta elettronica e l'integrazione con i social media.

## <a name="see-this-app-running-on-azure"></a>Vedere questa App in esecuzione in Azure

Si desidera vedere il sito completo in esecuzione come un'app web in tempo reale? È possibile distribuire una versione completa dell'app al tuo account Azure, facendo semplicemente clic sul pulsante seguente.

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebPagesMovies)

È necessario un account di Azure per distribuire questa soluzione in Azure. Se non hai già un account, sono disponibili le opzioni seguenti:

- [Aprire un account Azure gratuito](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -ricevono crediti è possibile usare per provare i servizi di Azure a pagamento e anche dopo che sono abituati fino è possibile mantenere l'account e usare i servizi Azure gratuiti.
- [Attivare i benefici della sottoscrizione MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -la sottoscrizione MSDN si accumulano crediti ogni mese in cui è possibile usare per i servizi di Azure a pagamento.

## <a name="installing-everything"></a>Installazione di tutti gli elementi

È possibile installare tutti gli elementi usando l'installazione guidata piattaforma Web Microsoft. In effetti, l'installazione guidata e quindi usarla per installare tutti gli altri elementi.

Per usare le pagine Web, è necessario essere avere almeno Windows XP con SP3 installati o Windows Server 2008 o versione successiva.

Nel [pagina Web Pages](../../../index.md) del sito Web ASP.NET, fare clic su **installare**.

![Visualizzazione del sito Web ASP.NET &quot;installa WebMatrix&quot; pulsante](getting-started/_static/image3.png)

Viene chiesto di accettare le condizioni di licenza e informativa sulla privacy prima di installare WebMatrix.

![accettare i termini per iniziare l'installazione](getting-started/_static/image4.png)

Fare clic su **eseguiti** per avviare l'installazione. (Se si desidera salvare il programma di installazione, fare clic su **salvare** e quindi eseguire il programma di installazione dalla cartella in cui è stato salvato.)

![](getting-started/_static/image5.png)

Viene visualizzata l'installazione guidata piattaforma Web, pronto per l'installazione di WebMatrix. Fare clic su **Installa**.

![](getting-started/_static/image6.png)

Il processo di installazione determina ciò che deve installare nel computer e avvia il processo. A seconda esattamente ciò che deve essere installato, il processo può richiedere da qualche istante per alcuni minuti. Selezionare **accetto** per accettare le condizioni di licenza.

## <a name="hello-webmatrix"></a>Hello, WebMatrix

Al termine, il processo di installazione può avviare automaticamente WebMatrix. In caso contrario, in Windows, dal **avviare** dal menu di avvio **Microsoft WebMatrix**.

Quando si avvia WebMatrix per la prima volta, si ha la possibilità di accedere a Microsoft Azure con l'account Microsoft. Effettuando l'accesso, si riceverà 10 App web gratuito tramite Azure. Queste App web gratuito offrono un modo pratico per testare le app. Se si ha già un account Azure, ma è necessario un abbonamento MSDN, puoi [attivare i benefici della sottoscrizione MSDN](https://www.windowsazure.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604). In caso contrario, è possibile creare un account di valutazione gratuito in pochi minuti. Per informazioni dettagliate, vedere [versione di valutazione gratuita di Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

Non è necessario accedere adesso per continuare questa esercitazione. Se non riesci a questo punto, si avrà ancora la possibilità di accedere in seguito. L'ultima [argomento](publishing.md) in questa esercitazione, serie illustra come distribuire il sito Web di Azure; pertanto, è necessario accedere per completare questo argomento.

A questo punto, accedere con l'account Microsoft o select **non ora** nell'angolo inferiore destro.

![Accedi](getting-started/_static/image7.png)

Per iniziare, è possibile creare un sito Web vuoto e aggiungere una pagina. In un'esercitazione più avanti in questo set si proveranno con uno dei modelli di sito Web predefinito.

Nella finestra di avvio, fare clic su **New**.

![Schermata di avvio di WebMatrix](getting-started/_static/image8.png)

I modelli sono file precompilati e le pagine per i diversi tipi di siti Web. Per visualizzare tutti i modelli disponibili per impostazione predefinita, selezionare l'opzione di raccolta di modelli.

![Raccolta di modelli di Select](getting-started/_static/image9.png)

Nel **avvio rapido** finestra, seleziona **sito vuoto** dal **ASP.NET** raggruppare e denominare il nuovo sito "WebPagesMovies".

![Finestra di avvio rapido di WebMatrix con modello di sito vuoto](getting-started/_static/image10.png)

Scegliere **Avanti**.

Se hanno effettuato l'accesso al tuo account Microsoft, sarà possibile l'opportunità di creare il sito in Azure. In base al nome del sito, il nome predefinito della **WebPagesMovies.azurewebsites.net** viene suggerito; tuttavia, il punto esclamativo indica che questo nome non è disponibile in Windows Azure. Per semplicità, selezionare **Skip** ignorare Crea subito il sito web in Azure. Più avanti in questa serie, il sito verranno pubblicate in Azure.

![creare il sito di azure](getting-started/_static/image11.png)

WebMatrix crea e apre il sito:

![Nuovo sito WebPagesMovies aperto in WebMatrix](getting-started/_static/image12.png)

Nella parte superiore, è presente una barra di accesso rapido e una barra multifunzione. In basso a sinistra, viene visualizzato il selettore dell'area di lavoro in cui si passa tra le attività (**Site**, **file**, **database**, **report**). Nella parte destra è il riquadro del contenuto per l'editor e i report. E nella parte inferiore si vedrà in alcuni casi una barra di notifica per i messaggi.

Si apprenderà più su WebMatrix e delle relative funzionalità man mano che procede attraverso queste esercitazioni.

## <a name="creating-a-web-page"></a>Creazione di una pagina Web

Per acquisire familiarità con ASP.NET Web Pages e WebMatrix, si creerà una semplice pagina.

Nel selettore dell'area di lavoro, selezionare la **file** dell'area di lavoro. Questa area di lavoro consente di lavorare con file e cartelle. Il riquadro a sinistra mostra la struttura di file del sito. Le modifiche della barra multifunzione per visualizzare le attività correlate ai file.

![Area di lavoro di file in WebMatrix](getting-started/_static/image13.png)

Nella barra multifunzione, fare clic sulla freccia sotto **New** e quindi fare clic su **nuovo File**.

![Usando il &quot;New&quot; comando sulla barra multifunzione per creare un nuovo file](getting-started/_static/image14.png)

WebMatrix consente di visualizzare un elenco di tipi di file. Selezionare **CSHTML**e il **nome** , digitare "HelloWorld". Una pagina con estensione CSHTML è una pagina ASP.NET Web Pages.

![Creazione di una nuova pagina CSHTML denominato HelloWorld.cshtml](getting-started/_static/image15.png)

Fare clic su **OK**.

WebMatrix consente di creare la pagina e viene aperto nell'editor.

![La nuova pagina HelloWorld nell'editor di WebMatrix](getting-started/_static/image16.png)

Come può notare, la pagina contiene principalmente ordinario il markup HTML, ad eccezione di un blocco nella parte superiore che si presenta come segue:

[!code-cshtml[Main](getting-started/samples/sample1.cshtml)]

Questa cartella per l'aggiunta di codice, come si vedrà a breve.

Si noti che le diverse parti della pagina &mdash; i nomi degli elementi, attributi e testo, oltre il blocco nella parte superiore, ovvero sono tutte in diversi colori. Questa operazione viene definita *evidenziazione della sintassi*, risulta più semplice mantenere tutto chiaro. È una delle funzionalità che rende più semplice lavorare con le pagine web in WebMatrix.

Aggiungere il contenuto per il `<head>` e `<body>` elementi, come illustrato nell'esempio seguente. (Se si desidera, è possibile semplicemente copiare il blocco seguente e sostituire l'intera pagina esistente con questo codice.)

[!code-cshtml[Main](getting-started/samples/sample2.cshtml)]

Nella barra di accesso rapido o nella **File** menu, fare clic su **salvare**.

![Pulsante Salva nella barra di accesso rapido WebMatrix](getting-started/_static/image17.png)

## <a name="testing-the-page"></a>Test della pagina

Nel **file** dell'area di lavoro, fare doppio clic sui *HelloWorld.cshtml* e quindi fare clic su **Avvia nel browser**.

![Esecuzione di una pagina con il pulsante Esegui sulla barra multifunzione di WebMatrix](getting-started/_static/image18.png)

WebMatrix avvia un server web incorporato, IIS Express, che è possibile usare per testare le pagine nel computer. (Senza IIS Express in WebMatrix, è necessario pubblicare la pagina a un server web in un punto prima che è possibile eseguirne il test.) Verrà visualizzata la pagina nel browser predefinito.

![&quot;Hello World&quot; pagina in esecuzione nel browser](getting-started/_static/image19.png)

Si noti che quando si testa una pagina in WebMatrix, l'URL nel browser è simile `http://localhost:33651/HelloWorld.cshtml.` il nome *localhost* fa riferimento a un server locale, vale a dire che la pagina è gestita da un server web nel proprio computer. Come accennato, WebMatrix include un programma di server web denominato IIS Express che viene eseguito quando si avvia una pagina.

Il numero dopo *localhost* (ad esempio, *localhost:33651*) fa riferimento a una *il numero di porta* nel computer. Questo è il numero di "canale" che IIS Express viene utilizzato per il sito Web specifico. Quando si crea un sito ed è diverso per ogni sito che si crea, il numero di porta viene selezionato in modo casuale dall'intervallo di 1024 e 65536. (Quando si testa il proprio sito, il numero di porta verrà quasi certamente essere un numero diverso da quello 33561.) Utilizzando una porta diversa per ogni sito Web, IIS Express possono mantenere semplici che dei siti di comunicare con.

In seguito quando si pubblica il sito in un server web pubblico, non si visualizzano più *localhost* nell'URL. A questo punto, si noterà un URL più tipico come `http://myhostingsite/mywebsite/HelloWorld.cshtml` o qualunque sia la pagina è. Verranno fornite ulteriori informazioni sulla pubblicazione di un sito più avanti in questa serie di esercitazioni.

## <a name="adding-some-server-side-code"></a>Aggiunta di codice lato Server

Chiudere il browser e tornare alla pagina in WebMatrix.

Aggiungere una riga per il blocco di codice in modo che risulti simile al codice seguente:

[!code-cshtml[Main](getting-started/samples/sample3.cshtml)]

Questo è un po' di codice Razor. È probabile che chiaro che ottiene la data e ora correnti e inserire tale valore in una *variabile* denominata `currentDateTime`. Si leggeranno informazioni sulla sintassi Razor nella prossima esercitazione.

Nel corpo della pagina, dopo il `<p>Hello World!</p>` elemento, aggiungere quanto segue:

[!code-html[Main](getting-started/samples/sample4.html)]

Questo codice ottiene il valore inserito nel `currentDateTime` variabili nella parte superiore e lo inserisce nel markup della pagina. Il `@` carattere contrassegna il codice ASP.NET Web Pages nella pagina.

Eseguire nuovamente la pagina (WebMatrix consente di salvare le modifiche per l'utente prima dell'esecuzione della pagina). Questa volta che viene visualizzata la data e ora nella pagina.

![&quot;Hello World&quot; pagina in esecuzione nel browser con una visualizzazione cronologica generata dinamicamente](getting-started/_static/image20.png)

Attendere alcuni istanti e quindi aggiornare la pagina nel browser. La visualizzazione di data e ora viene aggiornata.

Nel browser, esaminare il codice sorgente della pagina. Sembra che il markup seguente:

[!code-html[Main](getting-started/samples/sample5.html)]

Si noti che il `@{ }` blocco nella parte superiore non è presente. Si noti inoltre che la data e ora viene visualizzata una stringa effettiva di caratteri (`1/18/2012 2:49:50 PM` o qualunque), non `@currentDateTime` , ad esempio è presente nel *cshtml* pagina. Che cosa è successo qui è che, durante l'esecuzione della pagina, ASP.NET elaborato tutto il codice (molto poco in questo caso) che è stato contrassegnato con `@`. Output del codice e che l'output è stato inserito nella pagina.

## <a name="this-is-what-aspnet-web-pages-are-about"></a>Questo è ciò che riguardano le pagine Web ASP.NET

Quando si legge che ASP.NET Web Pages produce contenuto web dinamico, ho illustrato di seguito è l'idea. La pagina che appena creata contiene lo stesso markup HTML che si è visto prima. Può anche contenere codice che è possibile eseguire moltissime operazioni. In questo esempio, ha il compito di ottenere la data e ora correnti. Come si è visto, è possibile inserire nei codice HTML per produrre l'output della pagina. Quando un utente richiede un *cshtml* pagina nel browser, ASP.NET elabora la pagina mentre è ancora in tutti i vantaggi del server web. ASP.NET inserisce l'output del codice (se presente) nella pagina HTML. Quando viene eseguita l'elaborazione di codice, ASP.NET invia la pagina risultante nel browser. Tutti i browser che mai ottiene è HTML. Ecco un diagramma:

![Flusso concettuale del modo in cui ASP.NET genera l'errore HTML in modo dinamico](getting-started/_static/image21.png)

L'idea è semplice, ma esistono molte attività interessante che può eseguire il codice e in molti modi interessanti in cui è possibile aggiungere in modo dinamico contenuto HTML alla pagina. E ASP.NET *cshtml* pagine, come in qualsiasi pagina HTML, possono includere anche codice che viene eseguito nel browser stesso (codice di JavaScript e jQuery). Si esamineranno tutte queste operazioni in questa serie di esercitazioni e in quelle successive.

## <a name="coming-up-next"></a>In arrivo

Nella prossima esercitazione della serie, si Esplora un po' più di programmazione ASP.NET Web Pages.

## <a name="additional-resources"></a>Risorse aggiuntive

[Creare un sito Web ASP.NET da zero](https://www.microsoft.com/web/post/create-an-aspnet-website-from-scratch). Si tratta di un'esercitazione in cui è in particolare sull'utilizzo di WebMatrix (non ASP.NET Web Pages). Diventa un po' più in dettaglio alcune delle funzionalità aggiuntive di WebMatrix che non verranno illustrati in questa serie di esercitazioni.

> [!div class="step-by-step"]
> [avanti](intro-to-web-pages-programming.md)
