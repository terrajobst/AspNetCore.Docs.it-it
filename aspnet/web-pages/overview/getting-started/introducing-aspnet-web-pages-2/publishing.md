---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
title: 'Introduzione a ASP.NET Web Pages: pubblicazione di un sito utilizzando WebMatrix | Documenti Microsoft'
author: tfitzmac
description: In questa esercitazione è l'ultima parte del set di esercitazione che introduce ASP.NET Web Pages e WebMatrix Microsoft. Illustra come pubblicare il sito t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 7e85c70e-1a88-4408-8b3d-29611c7713ed
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
msc.type: authoredcontent
ms.openlocfilehash: 7b9bffac5cc72e1bea3f1b211cc03be2ccb8e499
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/08/2018
ms.locfileid: "30899590"
---
<a name="introducing-aspnet-web-pages---publishing-a-site-by-using-webmatrix"></a>Introduzione a ASP.NET Web Pages - pubblicazione di un sito tramite WebMatrix
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> In questa esercitazione è l'ultima parte del set di esercitazione che introduce ASP.NET Web Pages e WebMatrix Microsoft. Illustra come pubblicare il sito in Internet, in modo che altri possano lavorare con esso. Si presuppone di aver completato la serie tramite [la creazione di un aspetto coerente per i siti di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251585).
> 
> Si apprenderà come pubblicare il sito utilizzando:
> 
> - Microsoft Azure
> - Società di Hosting Web


## <a name="about-publishing-your-site"></a>Informazioni sulla pubblicazione del sito

Apportate fino a questo punto, tutto il lavoro in un computer locale, tra cui il test delle pagine. Per eseguire il<em>. cshtml</em> pagine, è stato utilizzato il server web che viene compilato in WebMatrix, vale a dire IIS Express. Ma nessun altro naturalmente possibile vedere il sito di che è stato creato, ad eccezione. Per consentire ad altri utenti di lavorare con il sito, è necessario pubblicarla in Internet.

A meno che non si ha già accesso a un server web pubblici, pubblicazione significa che è necessario disporre di un account con un *piattaforma cloud* o *provider di hosting*. Una piattaforma cloud, ad esempio Microsoft Azure, offre l'infrastruttura su richiesta per le applicazioni. Un provider di hosting è una società che possiede i server web accessibile pubblicamente e che verrà noleggiare è lo spazio per il sito. Hosting di piani di esecuzione da alcuni dollari al mese (o anche gratuiti) per i siti da molte centinaia di dollari mese per i siti Web commerciali di volumi elevati di piccole dimensioni.

> [!NOTE]
> Si potrebbe avere accesso a un server web pubblici tramite il provider di servizi internet (ISP) che consente di ottenere il servizio internet a casa. Tuttavia, il provider di hosting deve supportare ASP.NET Web Pages. Molti provider di servizi Internet non, ma è sempre opportuno controllo.


In questa esercitazione, si verrà fornita una panoramica sulle modalità di pubblicazione. Non è pratico fornire i dettagli esatti per tutto, perché in cui il processo è leggermente diverso per ogni provider di hosting. Tuttavia, si otterrà una buona idea di come funziona il processo.

In questa esercitazione contiene quattro sezioni:

1. [Impostazione della pagina predefinita](#defaultpage)
2. Pubblicazione (scegliere uno dei seguenti)  
 a. [Pubblicazione del sito di Microsoft Azure](#azure)  
 b. [Pubblicazione del sito per una società di Hosting Web](#host)
3. [L'aggiornamento del sito in tempo reale: ripubblicazione](#update)

<a id="defaultpage"></a>
## <a name="setting-up-the-default-page"></a>Impostazione della pagina predefinita

Quando un utente passa all'indirizzo di base per il sito web, la pagina predefinita per il sito viene visualizzata all'utente. Ad esempio, quando Default.htm è impostata come pagina predefinita per il sito a www.contoso.com, quindi passare a <strong>www.contoso.com</strong> equivale passando a <strong>www.contoso.com/Default.htm</strong>.

Attualmente, il sito utilizza **cshtml** come pagina predefinita. Questa pagina è appropriata per la pagina predefinita, ma in questa esercitazione non è stato aggiunto alcun contenuto alla pagina in modo verrebbe visualizzata una pagina vuota. Aprire Default. cshtml e sostituire il contenuto con il codice seguente.

[!code-cshtml[Main](publishing/samples/sample1.cshtml)]

Ora il sito è pronto per la pubblicazione. È in primo luogo, verrà visualizzato come distribuire il sito in Azure e come eseguire la distribuzione di una società di hosting web. Entrambi funzionano opzione per il sito web ed è sufficiente seguire una delle opzioni di distribuzione.

<a id="azure"></a>
## <a name="publishing-your-site-to-microsoft-azure"></a>Pubblicazione del sito di Microsoft Azure

In questa esercitazione verrà innanzitutto viene illustrato come distribuire il sito di Microsoft Azure. Con l'accesso con un account Microsoft, è possibile creare fino a 10 siti disponibili in Azure. Questi siti liberi offrono un modo pratico per testare i siti. È sempre possibile eliminare il sito di esempio in un secondo momento per evitare di utilizzare tutti i siti disponibile. È possibile creare un account di valutazione gratuito in pochi minuti. Per informazioni dettagliate, vedere [versione di valutazione gratuita di Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

Nella barra multifunzione di WebMatrix, fare clic su di **pubblica** pulsante.

![Pubblicare pulsante nella barra multifunzione di WebMatrix](publishing/_static/image1.png)

Il **pubblica del sito** viene visualizzata la finestra di dialogo. Se hanno non è connesso al tuo account Microsoft, la finestra di dialogo conterrà un **iniziare a usare Azure** collegamento. Fare clic su questo collegamento.

![Pubblicare il sito](publishing/_static/image2.png)

Se hanno non è connesso a un account Microsoft, è più la possibilità di eseguire l'accesso. È necessario accedere a un account Microsoft per pubblicare il sito in Azure.

![Accedi](publishing/_static/image3.png)

Dopo aver effettuato l'accesso al tuo account Microsoft, la finestra di dialogo contiene i collegamenti per creare un nuovo sito in Azure o connettersi a uno dei siti esistenti in Azure.

![Crea nuovo sito](publishing/_static/image4.png)

Selezionare **creare un nuovo sito**.

Se il progetto denominata **WebPagesMovies**, il nome predefinito per il sito sarà **webpagesmovies.azurewebsites.net**. Il nome predefinito è probabilmente non è disponibile, come indicato dal punto esclamativo rosso.

![nome del sito Web predefinito](publishing/_static/image5.png)

Selezionare un percorso che è vicino al percorso di modificare il nome del sito su un valore che è disponibile.

![Nome sito modificato](publishing/_static/image6.png)

Fare clic su **OK**.

WebMatrix performss un test per determinare se il server è compatibile con il sito.

![test di compatibilità](publishing/_static/image7.png)

Selezionare **continuare**.

Vengono visualizzati i risultati del test di compatibilità.

![risultato di compatibilità](publishing/_static/image8.png)

Selezionare **continuare**.

WebMatrix consente di visualizzare i file e i database che verranno pubblicati nel sito. Poiché questa è la prima volta che si esegue la pubblicazione del sito, sono elencati tutti i file. È possibile deselezionare un file che non è pronto per la pubblicazione. Nelle pubblicazioni successive, verranno visualizzati solo i file che sono stati modificati. Vedere [l'aggiornamento del sito in tempo reale: ripubblicazione](#update).

![Anteprima di pubblicazione](publishing/_static/image9.png)

Selezionare **continuare**.

Dopo che il sito è stato distribuito in Azure, viene visualizzato un messaggio che indica che la distribuzione è stata completata.

![pubblicazione completata](publishing/_static/image10.png)

Il sito e il database sono stati pubblicati in Azure e sono ora disponibili al pubblico. Fare clic sul collegamento nel messaggio che indica la pubblicazione è stata completata e verrà visualizzato il sito distribuito. Chiunque disponga di accesso a Internet è possibile aggiungere o modificare i record nel database.

![](publishing/_static/image11.png)

<a id="host"></a>
## <a name="publishing-your-site-to-a-web-hosting-company"></a>Pubblicazione del sito per una società di Hosting Web

Se si decide di non pubblicare in Azure, è invece possibile pubblicare il sito per una società di hosting web.

Fare clic su di **trovare hosting web** collegamento.

![Pulsante "Trovare hosting web" nella finestra di dialogo Impostazioni di pubblicazione](publishing/_static/image12.png)

Passare a una pagina sul sito Microsoft che elenca i provider di hosting che supportano ASP.NET.

![Pagina del sito Microsoft che elenca i provider di hosting](publishing/_static/image13.png)

Ovviamente, può essere difficile sapere esattamente quali funzionalità di hosting potrebbero essere necessari a lungo termine. Ecco un paio di aspetti da considerare:

- Ai fini del sito WebPagesMovies, non è necessario disporre di un componente aggiuntivo separato per SQL Server, che spesso costo aggiuntivo. Nel sito, si utilizza SQL Server Compact Edition, che è indipendente. Tuttavia, si potrebbe essere necessario accesso a SQL Server per operazioni future sito Web che è. Se si ritiene che è possibile, assicurarsi che sia possibile aggiungere funzionalità di SQL Server in un secondo momento.
- Verificare se il provider di hosting supporta il protocollo di pubblicazione di distribuzione Web. È possibile pubblicare tramite il protocollo FTP, ma è più comodo usare distribuzione Web.

Alcuni siti offrono un periodo di valutazione gratuita. Una versione di valutazione gratuita è un buon metodo per tentare di pubblicazione e hosting mentre si sta ancora sperimentare WebMatrix e ASP.NET Web Pages.

Selezionare quello desiderato. Per questa esercitazione, è selezionato DiscountASP.NET, perché durante l'esercitazione è durante la creazione, la società è una promozione che consente di ospitare un sito disponibile per alcuni mesi.

> [!NOTE]
> La scelta di un provider di hosting per questa esercitazione non deve essere interpretata come implica l'approvazione di tale società su qualsiasi altro. Ma è stato necessario scegliere uno scopo illustrativo e DiscountASP.NET è una delle molte aziende che supporta le pagine Web ASP.NET e il protocollo distribuzione Web per la pubblicazione.


In genere, dopo aver effettuato l'iscrizione con il provider di hosting, l'azienda invia un messaggio di posta elettronica che contiene un nome utente e una password, l'URL del server web e così via. La società di hosting supporta il protocollo di distribuzione Web, potrebbe inviare posta elettronica che è un file che contiene le impostazioni di pubblicazione o consentono di scaricare uno. Un file di impostazioni di pubblicazione semplifica il processo per l'utente.

Quando si effettuata l'iscrizione e si è pronti a pubblicare, fare clic su di **pubblica** pulsante nella barra multifunzione di WebMatrix. Il **impostazioni di pubblicazione** viene visualizzata la finestra di dialogo.

Se il provider di hosting ti ha inviato un file di impostazioni di pubblicazione, fare clic su di **importare impostazioni di pubblicazione** collegare e importare il file. Se non si dispone di un file di impostazioni di pubblicazione, compilare i campi utilizzando i valori che la società di hosting inviato tramite posta elettronica. Ecco il **impostazioni di pubblicazione** la finestra di dialogo potrebbe essere simile al termine:

![Impostazioni di pubblicazione nella finestra di dialogo Impostazioni pubblicazione compilato](publishing/_static/image14.png)

Fare clic su **convalidare la connessione**. Se non vi sono problemi, la finestra di dialogo segnala **stabilita**, vale a dire che può comunicare con il server del provider di hosting.

![Messaggio di esito positivo se pubblicare le impostazioni siano corrette](publishing/_static/image15.png)

Se si verifica un problema, WebMatrix fa del suo meglio per indicare il problema:

![Messaggio di errore se si è verificato un problema con le impostazioni di pubblicazione.](publishing/_static/image16.png)

Fare clic su **salvare** per salvare le impostazioni. WebMatrix offre eseguire un test per assicurarsi che possa comunicare correttamente con il sito di hosting:

![Messaggio di offerta per eseguire un test del processo di pubblicazione](publishing/_static/image17.png)

Scegliere **Sì**. WebMatrix consente di caricare alcuni file di esempio per il provider di hosting. Quando viene eseguita la verifica di compatibilità, WebMatrix segnala i risultati:

![Risultati del test di pubblicazione](publishing/_static/image18.png)

Se si è pronti passare, possiamo fare **continua** per avviare il processo di pubblicazione reali. WebMatrix figure quali file sono presenti nel sito e sono già disponibili nel server host (ora, nessuno) e offre un'anteprima del processo di pubblicazione:

![Anteprima del quale i file che verrà caricato il processo di pubblicazione](publishing/_static/image19.png)

L'elenco di file da pubblicare include le pagine web che è stato creato come *Movies.cshtml*. L'elenco include anche i file per gli helper che è stato installato, i file per eseguire SQL Server Compact Edition per il database e così via. Di conseguenza, il primo processo di pubblicazione può essere significativo.

Scegliere **Continua**. WebMatrix consente di copiare i file al server del provider di hosting. Al termine, nella barra di stato vengono segnalati i risultati:

![Messaggio visualizzato sulla barra di stato al termine del processo di pubblicazione](publishing/_static/image20.png)

Per visualizzare il sito in tempo reale, fare clic sul collegamento nella barra di stato. Aggiungere *filmati* all'URL, si noterà la *Movies.cshtml* file creato:

![Il sito in tempo reale che illustra la pagina di filmati](publishing/_static/image21.png)

<a id="update"></a>
## <a name="updating-the-live-site-republishing"></a>L'aggiornamento del sito in tempo reale: ripubblicazione

Dopo aver pubblicato il sito (per Azure o una società di hosting web), sono disponibili due copie di esso &mdash; la versione installata nel computer e la versione sul provider di servizi. È opportuno continuare a sviluppare il sito (se nient'altro, come parte del set di esercitazione successivo). Quando si esegue l'operazione, è necessario ripubblicare il sito per copiare le modifiche dal computer per il provider del servizio. Il processo di pubblicazione in WebMatrix è possibile determinare quali file sono stati modificati sul sito e pubblicare solo i file.

Per verificare il funzionamento del ripubblicazione, aprire il *Movies.cshtml* del sito, apportare alcune modifiche di piccola e quindi salvare il file. Ad esempio, modificare il titolo in `Movies - Updated`.

Fare clic su di **pubblica** pulsante nella barra multifunzione. WebMatrix determina cosa viene modificata e ne mostra un'anteprima dei file che verrà pubblicati.

![La finestra di dialogo 'Pubblica' visualizzazione modificati file pronti per la ripubblicazione](publishing/_static/image22.png)

> [!IMPORTANT] 
> 
> Per impostazione predefinita, WebMatrix pubblica il database (*sdf* file) solo la prima volta che si pubblica il sito. Una volta il sito è pubblicato e gli utenti interagiscono con il sito Web, il database nel sito in tempo reale ha in genere dati reali del sito. È necessario prestare attenzione a non sovrascrivere il database in tempo reale con il *sdf* file che si trova sul computer, che in genere contiene solo dati di test. Per tale motivo viene visualizzato l'avviso **pubblicazione sovrascriverà qualsiasi database remoti**, e il motivo per cui la casella di controllo *WebPagesMovies.sdf* è deselezionata per impostazione predefinita.


Scegliere **Continua**. WebMatrix pubblica i file modificati e Mostra un messaggio di conferma, analoga a quella la prima volta che è stato pubblicato.

Passare al sito in tempo reale (è possibile fare clic sul collegamento nel messaggio di esito positivo se è ancora visualizzato) e verificare che la modifica è stata pubblicata.

> [!TIP] 
> 
> **Modifica dei file in modalità remota**
> 
> In alternativa alla modifica del sito e quindi di ripubblicazione, è possibile modificare i file remoti direttamente in WebMatrix. In questo scenario, si apre un file che utilizza il provider del servizio e WebMatrix Scarica una copia da modificare. Ogni volta che si salva il file, WebMatrix invia le modifiche al sito.
> 
> La modifica remota è un modo semplice per apportare modifiche al sito in tempo reale. Le modifiche apportate in questo modo, tuttavia, non sono sincronizzate con i file nel sito locale. Per sincronizzare i file locali con il sito remoto, è possibile scaricare i file remoti. Questo processo funziona come la pubblicazione, tranne in ordine inverso.
> 
> È non verranno descritte più sulla modifica remoto e remote download funzioni di WebMatrix qui. Si tratta piuttosto utile se più persone devono lavorare sullo stesso sito su computer diversi. Per ulteriori informazioni, vedere [pubblicare e modificare un sito remoto con WebMatrix 2 Beta](https://go.microsoft.com/fwlink/?LinkId=251591).


## <a name="additional-resources"></a>Risorse aggiuntive

- [Forum di WebMatrix ASP.NET Web Pages ASP.NET](https://forums.asp.net/1224.aspx/1?WebMatrix+and+ASP+NET+Web+Pages), un ottimo punto di registrazione domande e risposte.

> [!div class="step-by-step"]
> [Precedente](layouts.md)
