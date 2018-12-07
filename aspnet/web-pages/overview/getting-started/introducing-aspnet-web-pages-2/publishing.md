---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
title: 'Introduzione a ASP.NET Web Pages: pubblicazione di un sito tramite WebMatrix | Microsoft Docs'
author: Rick-Anderson
description: Questa esercitazione è l'ultima puntata della saga la serie di esercitazioni che introduce Microsoft WebMatrix e pagine Web ASP.NET. Illustra come pubblicare il sito t...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: 7e85c70e-1a88-4408-8b3d-29611c7713ed
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
msc.type: authoredcontent
ms.openlocfilehash: bd6611a03ee4940f5d4176ce23464f313b9ec884
ms.sourcegitcommit: eef99d14d96dc8c3c1bb0e2c4cb14da152f8a952
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/07/2018
ms.locfileid: "53022505"
---
<a name="introducing-aspnet-web-pages---publishing-a-site-by-using-webmatrix"></a>Introduzione a pagine Web ASP.NET - pubblicazione di un sito tramite WebMatrix
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Questa esercitazione è l'ultima puntata della saga la serie di esercitazioni che introduce Microsoft WebMatrix e pagine Web ASP.NET. Descrive come pubblicare il sito in Internet, in modo che altri utenti possano utilizzarlo. Si presuppone di aver completato la serie attraverso [creazione di un aspetto coerente per i siti di ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251585).
> 
> Si apprenderà come pubblicare il sito usando:
> 
> - Microsoft Azure
> - Società di Hosting Web


## <a name="about-publishing-your-site"></a>Informazioni sulla pubblicazione del sito

Finora, sono state eseguite tutte le attività in un computer locale, tra cui il test delle pagine. Per eseguire la<em>cshtml</em> pagine, è stato usato il server web incorporato in WebMatrix, vale a dire IIS Express. Ma naturalmente non possono visitare il sito creato è. Per consentire ad altri utenti di lavorare con il sito, è necessario pubblicarla in Internet.

A meno che non si ha già accesso a un server web pubblico, la pubblicazione significa che è necessario avere un account con un *piattaforma cloud* o una *provider di hosting*. Una piattaforma cloud, ad esempio Microsoft Azure, offre un'infrastruttura su richiesta per le applicazioni. Un provider di hosting è una società che possiede i server web accessibile pubblicamente e che verranno noleggiare è lo spazio per il sito. I piani di esecuzione da alcuni dollari al mese (o persino gratuiti) di hosting di siti per molte centinaia di dollari al mese per i siti Web commerciali di volumi elevati di piccole dimensioni.

> [!NOTE]
> È necessario accedere a un server web pubbliche tramite il provider di servizi internet (ISP) che consente di ottenere il servizio internet a casa. Tuttavia, il provider di hosting deve supportare le pagine Web ASP.NET. Non molti provider di servizi Internet, ma è sempre opportuno verificare.


In questa esercitazione, si assegnerà è fornita una panoramica sulle modalità di pubblicazione. Non è pratico fornire informazioni dettagliate precise per tutti gli elementi, poiché il processo è leggermente diverso per ogni provider di hosting. Tuttavia, si otterrà una buona idea di come funziona il processo.

Questa esercitazione contiene quattro sezioni:

1. [Configurare la pagina predefinita](#defaultpage)
2. Pubblicazione (scegliere uno dei seguenti)  
 a. [Pubblicazione del sito in Microsoft Azure](#azure)  
 b. [Pubblicazione del sito in una società di Hosting Web](#host)
3. [L'aggiornamento del sito Live: ripubblicazione](#update)

<a id="defaultpage"></a>
## <a name="setting-up-the-default-page"></a>Configurare la pagina predefinita

Quando un utente passa all'indirizzo di base per il sito web, la pagina predefinita per il sito viene visualizzata all'utente. Ad esempio, quando *default. htm* viene impostato come pagina predefinita per il sito all'indirizzo `www.contoso.com`, quindi passando alla `www.contoso.com` equivale a passare a `www.contoso.com/Default.htm`.

Attualmente, i siti che utilizzano **cshtml** della pagina predefinita. Questa pagina è adeguata per la pagina predefinita, ma in questa esercitazione non è stato aggiunto alcun contenuto in tale pagina, in modo verrebbe visualizzata una pagina vuota. Aprire Default. cshtml e sostituire il contenuto con il codice seguente.

[!code-cshtml[Main](publishing/samples/sample1.cshtml)]

A questo punto il sito sarà pronto per la pubblicazione. In primo luogo, si noterà come distribuire il sito di Azure e quindi come eseguire la distribuzione di una società di hosting web. Entrambi funzionano opzione per il sito web ed è sufficiente seguire una delle opzioni di distribuzione.

<a id="azure"></a>
## <a name="publishing-your-site-to-microsoft-azure"></a>Pubblicazione del sito in Microsoft Azure

Questa esercitazione prima di tutto verrà illustrato come distribuire il sito di Microsoft Azure. Effettuando l'accesso con un account Microsoft, è possibile creare siti gratuiti fino a 10 in Azure. Questi siti gratuiti offrono un modo pratico per testare i tuoi siti. È sempre possibile eliminare il sito di esempio in un secondo momento per evitare di utilizzare tutti i siti gratuiti. È possibile creare un account di valutazione gratuito in pochi minuti. Per informazioni dettagliate, vedere [versione di valutazione gratuita di Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

Nella barra multifunzione di WebMatrix, fare clic sui **pubblica** pulsante.

!['Publish' pulsante nella barra multifunzione di WebMatrix](publishing/_static/image1.png)

Il **pubblicare nel sito del** verrà visualizzata la finestra di dialogo. Se hanno non è connesso al tuo account Microsoft, la finestra di dialogo contiene un **iniziare a usare Azure** collegamento. Fare clic su questo collegamento.

![Pubblicare il sito](publishing/_static/image2.png)

Se hanno non è connesso a un account Microsoft, è anche in questo caso la possibilità di accedere. È necessario accedere a un account Microsoft per pubblicare il sito in Azure.

![Accedi](publishing/_static/image3.png)

Dopo l'accesso al tuo account Microsoft, la finestra di dialogo contiene i collegamenti per creare un nuovo sito in Azure o connettersi a uno dei siti esistenti in Azure.

![Crea nuovo sito](publishing/_static/image4.png)

Selezionare **creare un nuovo sito**.

Se il progetto **WebPagesMovies**, il nome predefinito per il sito sarà **webpagesmovies.azurewebsites.net**. Questo nome predefinito è molto probabilmente non è disponibile, come indicato dal punto esclamativo rosso.

![nome del sito Web predefinito](publishing/_static/image5.png)

Modificare il nome del sito su un valore che è disponibile e scegliere una località vicina alla propria posizione.

![Nome sito modificato](publishing/_static/image6.png)

Fare clic su **OK**.

WebMatrix performss un test per determinare se il server è compatibile con il sito.

![test di compatibilità](publishing/_static/image7.png)

Selezionare **continuare**.

Vengono visualizzati i risultati del test di compatibilità.

![risultati di compatibilità](publishing/_static/image8.png)

Selezionare **continuare**.

WebMatrix consente di visualizzare i file e i database che verranno pubblicati nel sito. Poiché questa è la prima volta che si pubblica il sito, vengono elencati tutti i file. È possibile deselezionare un file che non è pronto per la pubblicazione. Nelle pubblicazioni successive, verranno visualizzati solo i file che sono stati modificati. Visualizzare [l'aggiornamento del sito Live: ripubblicazione](#update).

![Anteprima di pubblicazione](publishing/_static/image9.png)

Selezionare **continuare**.

Dopo aver distribuito il sito in Azure, viene visualizzato un messaggio che indica che la distribuzione è stata completata.

![pubblicazione completata](publishing/_static/image10.png)

Il sito e il database sono stati pubblicati in Azure e sono ora disponibili al pubblico. Fare clic sul collegamento nel messaggio che indica la pubblicazione è stata completata e verrà ora visualizzato il sito distribuito. Chiunque disponga di accesso a Internet possono aggiungere o modificare record nel database.

![](publishing/_static/image11.png)

<a id="host"></a>
## <a name="publishing-your-site-to-a-web-hosting-company"></a>Pubblicazione del sito in una società di Hosting Web

Se si decide di non pubblicare in Azure, è invece possibile pubblicare il sito per una società di hosting web.

Scegliere il **trovare hosting web** collegamento.

![Pulsante "Trovare hosting web" nella finestra di dialogo Impostazioni di pubblicazione](publishing/_static/image12.png)

Passa a una pagina nel sito Microsoft che elenca i provider di hosting che supportano ASP.NET.

![Pagina sul sito Microsoft che elenca i provider di hosting](publishing/_static/image13.png)

Ovviamente, può essere difficile sapere esattamente quali funzionalità di hosting potrebbero essere necessari a lungo termine. Ecco un paio di aspetti da considerare:

- Ai fini del sito WebPagesMovies, non è necessario disporre di un componente aggiuntivo separato per SQL Server, che spesso ha un costo aggiuntivo. Nel sito, si usa SQL Server Compact Edition, che è indipendente. Tuttavia, potrebbe essere necessario l'accesso di SQL Server per alcune operazioni di sito Web future effettuate. Se si ritiene che potrebbe, assicurarsi che è possibile aggiungere funzionalità di SQL Server in un secondo momento.
- Verificare se il provider di hosting supporta il protocollo di pubblicazione con distribuzione Web. È possibile pubblicare tramite il protocollo FTP, ma è più comodo usare distribuzione Web.

Alcuni siti offrono un periodo di valutazione gratuita. Una versione di valutazione gratuita è un buon metodo per provare la pubblicazione e hosting mentre si sta ancora sperimentato con ASP.NET Web Pages e WebMatrix.

Sceglierne uno che si desidera. Per questa esercitazione, è selezionato DiscountASP.NET, mentre è stavamo l'esercitazione per la creazione, la società risultavano una promozione che consentano alle persone di ospitare un sito gratuito per alcuni mesi.

> [!NOTE]
> La nostra scelta di un provider di hosting per questa esercitazione non deve essere interpretato come una verifica dell'autenticità della società o qualsiasi altra. Ma è stato necessario scegliere uno scopo illustrativo e DiscountASP.NET è una delle molte aziende che supporta il protocollo distribuzione Web e pagine Web ASP.NET per la pubblicazione.


In genere, dopo aver effettuato l'iscrizione con il provider di hosting, la società invia un messaggio di posta elettronica che contiene un nome utente e password, l'URL del server web e così via. Se la società di hosting supporta protocollo distribuzione Web, si potrebbe inviare è un file che contiene le impostazioni di pubblicazione o consentono di scaricare uno. Un file di impostazioni di pubblicazione semplifica il processo per l'utente.

Quando si è iscritti e sono pronti a pubblicare, scegliere il **pubblica** pulsante della barra multifunzione di WebMatrix. Il **impostazioni di pubblicazione** verrà visualizzata la finestra di dialogo.

Se il provider di hosting ti ha inviato un file di impostazioni di pubblicazione, fare clic sui **importare impostazioni di pubblicazione** collegare e importare il file. Se non si dispone di un file di impostazioni di pubblicazione, compilare i campi utilizzando i valori che la società di hosting ha inviato nel messaggio di posta elettronica. Questa figura viene illustrata la **impostazioni di pubblicazione** nella finestra di dialogo potrebbe essere simile al termine:

![Impostazioni di pubblicazione compilato nella finestra di dialogo 'Impostazioni di pubblicazione'](publishing/_static/image14.png)

Fare clic su **convalidare la connessione**. Se tutto ciò che è accettabile, la finestra di dialogo segnala **connesso correttamente**, vale a dire che possa comunicare con server del provider di hosting.

![Operazione riuscita dei messaggi se pubblicare le impostazioni siano corrette](publishing/_static/image15.png)

Se si è verificato un problema, WebMatrix nel modo migliore per stabilire qual è il problema:

![Messaggio di errore se si è verificato un problema con le impostazioni di pubblicazione](publishing/_static/image16.png)

Fare clic su **salvare** per salvare le impostazioni. WebMatrix offre per eseguire un test per assicurarsi che possa comunicare correttamente con il sito di hosting:

![Messaggio che propone di eseguire un test del processo di pubblicazione](publishing/_static/image17.png)

Scegliere **Sì**. WebMatrix consente di caricare alcuni file di esempio per il provider di hosting. Quando viene eseguita la verifica di compatibilità, WebMatrix segnala i risultati:

![Risultati del test di pubblicazione](publishing/_static/image18.png)

Se si è pronti per passare, possiamo fare **continuazione** per avviare il processo di pubblicazione reale. WebMatrix determina quali file sono presenti nel sito e sono già nel server host (al momento, nessuno) e ti offre un'anteprima del processo di pubblicazione:

![Anteprima del quale i file che verrà caricato il processo di pubblicazione](publishing/_static/image19.png)

L'elenco dei file da pubblicare include le pagine web create, ad esempio *Movies.cshtml*. L'elenco include anche i file per gli helper che è stato installato, i file per eseguire SQL Server Compact Edition per il database e così via. Di conseguenza, processo di pubblicazione iniziale può essere significativo.

Scegliere **Continua**. WebMatrix consente di copiare i file al server del provider di hosting. Al termine, i risultati vengono segnalati nella barra di stato:

![Messaggio visualizzato sulla barra di stato al termine del processo di pubblicazione](publishing/_static/image20.png)

Per visualizzare il sito in tempo reale, fare clic sul collegamento nella barra di stato. Aggiungere *film* all'URL, si noterà il *Movies.cshtml* file creato:

![Il sito in tempo reale che mostra la pagina Movies](publishing/_static/image21.png)

<a id="update"></a>
## <a name="updating-the-live-site-republishing"></a>L'aggiornamento del sito Live: ripubblicazione

Dopo aver pubblicato il sito (da Azure o una società di hosting web), sono disponibili due copie di esso &mdash; della versione installata nel computer e la versione sul provider di servizi. Probabilmente vorrai continuare a sviluppare il sito (se non altro, come parte del set di esercitazioni successivo). Quando esegue questa operazione, è necessario ripubblicare il sito per copiare le modifiche dal computer al provider del servizio. Il processo di pubblicazione in WebMatrix è possibile determinare quali file sono stati modificati nel sito e pubblicare solo dei file.

Per verificare il funzionamento di ripubblicazione, aprire il *Movies.cshtml* del sito, apportare alcune piccole modifiche e quindi salvare il file. Ad esempio, modificare il titolo in `Movies - Updated`.

Scegliere il **pubblica** pulsante della barra multifunzione. WebMatrix determina ciò che viene modificato e Mostra l'anteprima dei file che verrà pubblicati.

![La finestra di dialogo 'Pubblica' visualizzazione modificati file pronti per la ripubblicazione](publishing/_static/image22.png)

> [!IMPORTANT] 
> 
> Per impostazione predefinita, WebMatrix pubblica il database (*sdf* file) solo la prima volta che si pubblica il sito. Una volta il sito è pubblicato e le persone interagiscono con il sito Web, il database del sito live è in genere dati reali del sito. È necessario prestare attenzione a non sovrascrivere il database in tempo reale con il *sdf* file che si trova sul computer, che in genere contiene solo i dati di test. Ecco perché viene visualizzato l'avviso **pubblicazione sovrascriverà qualsiasi database remoti**, e il motivo per cui la casella di controllo *WebPagesMovies.sdf* è deselezionata per impostazione predefinita.


Scegliere **Continua**. WebMatrix pubblica i file modificati e Mostra un messaggio di conferma, come accade la prima volta è stato pubblicato.

Passare al sito live (è possibile fare clic sul collegamento nel messaggio di esito positivo se viene ancora visualizzato) e verificare che la modifica è stata pubblicata.

> [!TIP] 
> 
> **Modifica dei file in modalità remota**
> 
> Come alternativa alla modifica del sito e quindi di ripubblicazione, è possibile modificare i file remoti direttamente in WebMatrix. In questo scenario, si apre un file che si trova su provider di servizi e WebMatrix Scarica una copia da modificare. Ogni volta che si salva il file, WebMatrix invia le modifiche al sito.
> 
> La modifica remota è un modo semplice per apportare modifiche al sito live. Tuttavia, le modifiche apportate in questo modo non sono sincronizzate con i file nel sito locale. Per sincronizzare i file locali con il sito remoto, è possibile scaricare i file remoti. Questo processo funziona in modo analogo la pubblicazione, tranne in ordine inverso.
> 
> Non viene più sul remoto per la modifica e il download di remote strutture di WebMatrix qui. Risultano molto utili se più persone devono lavorare sullo stesso sito su computer diversi. Per altre informazioni, vedere [pubblicare e modificare un sito remoto con la versione Beta 2 di WebMatrix](https://go.microsoft.com/fwlink/?LinkId=251591).


## <a name="additional-resources"></a>Risorse aggiuntive

- [Forum su WebMatrix ASP.NET Web Pages ASP.NET](https://forums.asp.net/1224.aspx/1?WebMatrix+and+ASP+NET+Web+Pages), un posto ideale per inviare domande e risposte.

> [!div class="step-by-step"]
> [Precedente](layouts.md)
