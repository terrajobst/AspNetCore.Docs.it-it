---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-an-ftp-client-vb
title: La distribuzione del sito utilizzando un Client FTP (VB) | Documenti Microsoft
author: rick-anderson
description: "Il modo più semplice per distribuire un'applicazione ASP.NET è copiare manualmente i file necessari dall'ambiente di sviluppo all'ambiente di produzione. Parola..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: 09279194-bcf9-4b59-a09d-c68e5926a758
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-an-ftp-client-vb
msc.type: authoredcontent
ms.openlocfilehash: 56b73820f8b770a5332cb39029ae25df4d5753dc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="deploying-your-site-using-an-ftp-client-vb"></a>La distribuzione del sito utilizzando un Client FTP (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare codice](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_03_VB.zip) o [Scarica il PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial03_DeployingViaFTP_vb.pdf)

> Il modo più semplice per distribuire un'applicazione ASP.NET è copiare manualmente i file necessari dall'ambiente di sviluppo all'ambiente di produzione. In questa esercitazione viene illustrato come utilizzare un client FTP per ottenere i file dal desktop per il provider di hosting web.


## <a name="introduction"></a>Introduzione

L'esercitazione precedente introdotte una semplice applicazione web ASP.NET revisione libro, è costituita da un numero limitato di pagine ASP.NET, una pagina master, una base personalizzata `Page` classe, un numero di immagini e fogli di stile tre CSS. Ora si è pronti distribuire l'applicazione a un provider di hosting web, a quel punto l'applicazione sarà accessibile a tutti gli utenti con una connessione a Internet.


Dal nostro discussioni nel [ *determinare quali file devono essere distribuiti* ](determining-what-files-need-to-be-deployed-vb.md) dell'esercitazione, si sa quali file devono essere copiati i provider di hosting web. (Tenere presente che i file vengono copiati varia a seconda se la compilazione dell'applicazione in modo esplicito o automaticamente). Ma, come si ottengono i file dall'ambiente di sviluppo (il desktop) fino all'ambiente di produzione (web server gestiti dal provider host web)? Il [ **F** ile **T** trasferire **P** rotocol (FTP)](http://en.wikipedia.org/wiki/File_Transfer_Protocol) è un protocolli comunemente usati per la copia dei file da un computer a un altro in una rete. Un'altra opzione consiste estensioni del Server (FrontPage). Questa esercitazione viene illustrato l'utilizzo di software di client FTP autonomo per distribuire i file necessari dall'ambiente di sviluppo all'ambiente di produzione.

> [!NOTE]
> Visual Studio include strumenti per la pubblicazione di siti Web tramite FTP. Questi strumenti, nonché un'occhiata a strumenti che usano FPSE, è descritte nell'esercitazione successiva.


Per copiare i file tramite FTP, è necessario un *client FTP* nell'ambiente di sviluppo. Un client FTP è un'applicazione progettata per copiare i file dal computer è installato in un computer che esegue un *server FTP*. (Se il provider di hosting web supporta i trasferimenti di file tramite FTP, come la maggior parte, è un server FTP in esecuzione nei server web.) Sono disponibili numerose applicazioni client FTP. Il browser può anche double come client FTP. Il client FTP preferito e la si utilizzerà per questa esercitazione, è [FileZilla](http://filezilla-project.org/), un client FTP gratuito e open source che è disponibile per Windows, Linux e computer Mac. Qualsiasi client FTP funzionerà, tuttavia, in tal caso è possibile utilizzare qualsiasi client si ha maggiore familiarità con.

Se si segue lungo è verrà necessario per creare un account con un provider di hosting web prima di completare questa esercitazione o quelli successivi. Come indicato nell'esercitazione precedente, sono disponibili un ad della società di provider host web con un'ampia gamma di prezzi e funzionalità di qualità del servizio. Per questa serie di esercitazioni si utilizzerà [Discount ASP.NET](http://discountasp.net) come l'host web provider, ma è possibile proseguire con qualsiasi provider di hosting web a condizione che supportano la versione ASP.NET del sito è stato sviluppato in. (Queste esercitazioni sono state create usando ASP.NET 3.5.) Inoltre, poiché si copia dei file per il provider di hosting web tramite FTP in questa esercitazione e, in futuro, quelle è fondamentale che il provider di hosting web supporta l'accesso ai server web FTP. Quasi tutti i provider host web offrono questa funzionalità, ma è necessario verificare prima di effettuare l'iscrizione.

## <a name="deploying-the-book-review-web-application-project"></a>La distribuzione del progetto di applicazione Web di libro revisione

Tenere presente che sono disponibili due versioni dell'applicazione web recensioni: implementata utilizzando il modello di progetto di applicazione Web (BookReviewsWAP) e l'altro utilizza il modello di progetto di sito Web (BookReviewsWSP). Il tipo di progetto determina se il sito viene compilato automaticamente o in modo esplicito e il modello di compilazione determina quali file devono essere distribuiti. Di conseguenza, esamineremo distribuisce i progetti BookReviewsWAP e BookReviewsWSP separatamente, a partire dal BookReviewsWAP. È opportuno scaricare queste due applicazioni ASP.NET, se non è già.

Avviare il progetto BookReviewsWAP passando al `BookReviewsWAP` cartella e fare doppio clic su di `BookReviewsWAP.sln` file. Prima di distribuire il progetto è importante compilare in modo da garantire che tutte le modifiche al codice sorgente sono inclusi nell'assembly compilato. Per compilare il progetto dal menu di compilazione e scegliere l'opzione del menu Compila BookReviewsWAP. Il codice sorgente nel progetto verrà compilato in un unico assembly `BookReviewsWAP.dll`, che viene inserito nella `Bin` cartella.

Ci sono pronti per distribuire i file necessari. Avviare il client FTP e connettersi al server web del provider host web. (Quando effettua l'iscrizione con una società di hosting web verrà inviato un messaggio informazioni su come connettersi al server FTP, ciò include l'indirizzo del server FTP, nonché un nome utente e password).

Copiare i file seguenti dal desktop alla cartella radice del sito Web del provider di hosting web. Quando si FTP nel server web in web ospita provider in genere che nella directory principale del sito Web. Tuttavia, alcuni provider host web dispone di una sottocartella denominata `www` o `wwwroot` che serve come la cartella radice per i file del sito. Infine, quando FTPing i file potrebbe essere necessario creare la struttura di cartelle corrispondenti nell'ambiente di produzione - il `Bin` cartella, il `Fiction` cartella, il `Images` cartella e così via.

- `~/Default.aspx`
- `~/About.aspx`
- `~/Site.master`
- `~/Web.config`
- `~/Web.sitemap`
- Il contenuto completo del `Styles` cartella
- Il contenuto completo del `Images` cartella (e nella relativa sottocartella, `BookCovers`)
- `~/Fiction/Default.aspx`
- `~/Fiction/Blaze.aspx`
- `~/Tech/Default.aspx`
- `~/Tech/CYOW.aspx`
- `~/Tech/TYASP35.aspx`
- `~/Bin/BookReviewsWAP.dll`

Figura 1 mostra FileZilla dopo avere copiati i file necessari. FileZilla Visualizza i file nel computer locale a sinistra e i file nel computer remoto sulla destra. Come nella figura 1 mostra, file di codice sorgente ASP.NET, ad esempio `About.aspx.vb`, sono nel computer locale (ambiente di sviluppo), ma non sono stati copiati i provider di hosting web (ambiente di produzione) perché non è necessario essere distribuiti quando si utilizzano file di codice compilazione esplicita.

> [!NOTE]
> Non è causa problemi con i file del codice sorgente nel server di produzione, vengono ignorati. Per impostazione predefinita, in ASP.NET non consente le richieste HTTP ai file del codice sorgente in modo che anche se i file del codice sorgente sono presenti nel server di produzione sono inaccessibili ai visitatori del sito Web. (Ovvero, se un utente tenta di visitare `http://www.yoursite.com/Default.aspx.vb` che verrà visualizzato come una pagina di errore che spiega che questi tipi di file - `.vb` - file non sono consentite.)


[![Utilizzare un Client FTP per copiare i file necessari dal Desktop al Server Web del provider Host Web.](deploying-your-site-using-an-ftp-client-vb/_static/image2.png)](deploying-your-site-using-an-ftp-client-vb/_static/image1.png)

**Figura 1**: utilizzare un FTP Client per copiare i file necessari nel Desktop nel server Web al Provider di hosting Web ([fare clic per visualizzare l'immagine ingrandita](deploying-your-site-using-an-ftp-client-vb/_static/image3.png))


Dopo la distribuzione del sito è opportuno testare il sito. Se è stato acquistato un nome di dominio e configurato le impostazioni DNS in modo corretto, è possibile visitare il sito, immettere il nome di dominio. In alternativa, il provider di hosting web deve avere ha fornito un URL al sito, che avrà un aspetto simile *accountname*. *webhostprovider*. com o *webhostprovider*. com /*accountname*. Ad esempio, l'URL per l'account su ASP.NET di sconto è: `http://httpruntime.web703.discountasp.net`.

Figura 2 mostra il sito recensioni distribuito. Si noti che lo sto visualizzando nel ASP di sconto. I server della rete, in `http://httpruntime.web703.discountasp.net`. In questo momento chiunque disponga di una connessione a Internet potrebbe visualizzare il sito Web. Come previsto, il sito esegue la ricerca e si comporta come avveniva quando eseguire il test nell'ambiente di sviluppo.

> [!NOTE]
> Se si verifica un errore durante la visualizzazione dell'applicazione è opportuno verificare che è stato distribuito il set corretto di file. Successivamente, controllare il messaggio di errore per vedere se rivela eventuali indizi il problema. Successivamente, è possibile attivare al supporto tecnico della società host web o inviare una domanda nel forum appropriato nel [forum ASP.NET](https://forums.asp.net/).


[![Il sito di revisioni del libro è ora accessibile agli utenti con una connessione Internet.](deploying-your-site-using-an-ftp-client-vb/_static/image5.png)](deploying-your-site-using-an-ftp-client-vb/_static/image4.png)

**Figura 2**: il sito di revisioni del libro è ora accessibile agli utenti con una connessione Internet ([fare clic per visualizzare l'immagine ingrandita](deploying-your-site-using-an-ftp-client-vb/_static/image6.png))


## <a name="deploying-the-book-review-web-site-project"></a>La distribuzione del progetto di sito Web di revisione della Rubrica

Quando si distribuisce un'applicazione ASP.NET che viene utilizzata la compilazione automatica, ad esempio il progetto di sito Web BookReviewsWSP, non sono presenti assembly compilato nel `Bin` cartella. Di conseguenza, il file di codice sorgente dell'applicazione web devono essere distribuito nell'ambiente di produzione. Si analizzerà questo processo.

Come con il progetto di applicazione Web è consigliabile prima Build l'applicazione prima di distribuirlo. Durante la creazione di un progetto di sito Web non viene creato un assembly, controllare gli eventuali errori in fase di compilazione nella pagina. Migliore per individuare questi errori ora anziché con un visitatore del sito individua automaticamente!

Dopo aver creato correttamente il progetto, è possibile utilizzare il client FTP per copiare i file seguenti nella cartella del sito Web radice del provider host web. Si potrebbe essere necessario creare la struttura di cartelle corrispondenti nell'ambiente di produzione.

> [!NOTE]
> Se già stato distribuito il progetto comunque ma si desidera provare a distribuire il progetto BookReviewsWSP di BookReviewsWAP, eliminare innanzitutto tutti i file nel server web che sono stati caricati quando si distribuisce BookReviewsWAP e quindi distribuire i file per BookReviewsWSP.


- `~/Default.aspx`
- `~/Default.aspx.vb`
- `~/About.aspx`
- `~/About.aspx.vb`
- `~/Site.master`
- `~/Site.master.vb`
- `~/Web.config`
- `~/Web.sitemap`
- Il contenuto completo del `Styles` cartella
- Il contenuto completo del `Images` cartella (e nella relativa sottocartella, `BookCovers`)
- `~/App_Code/BasePage.vb`
- `~/Fiction/Default.aspx`
- `~/Fiction/Default.aspx.vb`
- `~/Fiction/Blaze.aspx`
- `~/Fiction/Blaze.aspx.vb`
- `~/Tech/Default.aspx`
- `~/Tech/Default.aspx.vb`
- `~/Tech/CYOW.aspx`
- `~/Tech/CYOW.aspx.vb`
- `~/Tech/TYASP35.aspx`
- `~/Tech/TYASP35.aspx.vb`

Figura 3 mostra FileZilla dopo la copia dei file necessari. Come si può notare, ASP.NET file di codice sorgente, ad esempio `About.aspx.vb`, sono presenti nel computer locale (ambiente di sviluppo) sia il provider di hosting web (ambiente di produzione) perché i file di codice devono essere distribuiti quando si usa automatico compilazione.


[![Utilizzare un Client FTP per copiare i file necessari dal proprio computer al Server Web il Provider di hosting Web](deploying-your-site-using-an-ftp-client-vb/_static/image8.png)](deploying-your-site-using-an-ftp-client-vb/_static/image7.png)

**Figura 3**: utilizzare un FTP Client per copiare i file necessari nel Desktop nel server Web al Provider di hosting Web ([fare clic per visualizzare l'immagine ingrandita](deploying-your-site-using-an-ftp-client-vb/_static/image9.png))


L'esperienza utente non è interessata dal modello di compilazione dell'applicazione. Le stesse pagine ASP.NET sono accessibili e l'aspetto e lo stesso comportamento se il sito Web è stato creato utilizzando il modello di progetto di applicazione Web o il modello di progetto di sito Web.

## <a name="updating-a-web-application-on-production"></a>Aggiornamento di un'applicazione Web in produzione

Distribuzione e lo sviluppo di applicazioni web non è un processo unico. Ad esempio, quando si crea il sito Web recensioni è compilato diverse pagine e ha scritto il codice associato nel computer personale (ambiente di sviluppo). Dopo aver raggiunto un determinato stato stabile, è distribuito l'applicazione in modo che altri utenti potrebbe visitare il sito e leggere il revisioni. Tuttavia, la distribuzione non contrassegnare la fine del mio lo sviluppo in questo sito. Possono aggiungere ulteriori recensioni o implementare nuove funzionalità, ad esempio per consentire i visitatori alla documentazione di frequenza o lasciare i propri commenti. Tali miglioramenti sono sviluppare nell'ambiente di sviluppo e, al termine, dovrà essere distribuita. Sviluppo e distribuzione, di conseguenza, sono ciclici. Si sviluppa un'applicazione e quindi distribuirlo. Mentre il sito è attivo nell'ambiente di produzione, vengono aggiunte nuove funzionalità e i bug risolti nel tempo, che richiede la distribuzione dell'applicazione. E così via e così via.

Come prevedibile, quando un'applicazione web che è necessario copiare i file nuovi e modificati vengono nuovamente distribuite. Non è necessario distribuire nuovamente pagine non modificate o i file di supporto sul lato server o client (anche se non esiste in questo modo non causa problemi).

> [!NOTE]
> È necessario tenere in considerazione quando si utilizza la compilazione esplicita è che ogni volta che si aggiunge una nuova pagina ASP.NET al progetto o apportano modifiche relative al codice, è necessario ricompilare il progetto, che aggiorna l'assembly nel `Bin` cartella. Di conseguenza, è necessario copiare l'assembly aggiornato nell'ambiente di produzione quando si aggiorna un'applicazione web in produzione (insieme a altro contenuto nuovo e aggiornato).


Inoltre comprendere che le modifiche apportate al `Web.config` o i file di `Bin` directory verrà arrestato e riavviato il Pool di applicazioni del sito Web. Se lo stato della sessione verrà archiviato utilizzando il `InProc` modalità (impostazione predefinita), quindi i visitatori del sito perderà lo stato di sessione ogni volta che vengono modificati i file di chiave. Per evitare questo inconveniente, è consigliabile archiviare sessione utilizzando il `StateServer` o `SQLServer` modalità. Per ulteriori informazioni su questo argomento, leggere [modalità stato sessione](https://msdn.microsoft.com/en-us/library/ms178586.aspx).

Infine, tenere presente che la distribuzione di un'applicazione può richiedere alcuni secondi a diversi minuti, a seconda del numero e dimensioni dei file che devono essere copiati nell'ambiente di produzione. Durante questo periodo gli utenti che visitano il sito potrebbero verificarsi errori o comportamenti anomali. È possibile "disattivare" l'intera applicazione mediante l'aggiunta di una pagina denominata `App_Offline.htm` directory radice dell'applicazione che spiega agli utenti che il sito è inattivo per manutenzione (o qualsiasi altro) e verrà possibile eseguire il backup a breve. Quando il `App_Offline.htm` file è presente, il runtime di ASP.NET reindirizza tutte le richieste in ingresso a questa pagina.

## <a name="summary"></a>Riepilogo

Distribuzione di un'applicazione web implica la copia i file necessari dall'ambiente di sviluppo all'ambiente di produzione. Il mezzo più comune con cui i file vengono trasferiti in rete è il protocollo FTP (File Transfer) e la maggior parte dei provider di host web supporta l'accesso ai server web FTP. In questa esercitazione è stato illustrato come utilizzare un client FTP per distribuire i file necessari al server web. Una volta distribuito, il sito Web è possibile visitare da qualsiasi utente con una connessione a Internet.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [App\_Offline.htm e risolvere la funzionalità "IE descrittivo errori"](https://weblogs.asp.net/scottgu/App_5F00_Offline.htm-and-working-around-the-_2200_IE-Friendly-Errors_2200_-feature)
- [Modalità stato sessione](https://msdn.microsoft.com/en-us/library/ms178586.aspx)

>[!div class="step-by-step"]
[Precedente](determining-what-files-need-to-be-deployed-vb.md)
[Successivo](deploying-your-site-using-visual-studio-vb.md)
