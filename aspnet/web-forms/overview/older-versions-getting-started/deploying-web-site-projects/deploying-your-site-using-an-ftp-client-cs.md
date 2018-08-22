---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-an-ftp-client-cs
title: Distribuzione del sito tramite un Client FTP (c#) | Microsoft Docs
author: rick-anderson
description: Il modo più semplice per distribuire un'applicazione ASP.NET consiste nel copiare manualmente i file necessari dall'ambiente di sviluppo all'ambiente di produzione. Corrente...
ms.author: riande
ms.date: 04/01/2009
ms.assetid: a3599cf7-8474-4006-954a-3bc693736b66
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-an-ftp-client-cs
msc.type: authoredcontent
ms.openlocfilehash: cdecc85c056fc5153763d938c665b473117df9ba
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823887"
---
<a name="deploying-your-site-using-an-ftp-client-c"></a>Distribuzione del sito tramite un Client FTP (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare il codice](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_03_CS.zip) o [Scarica il PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial03_DeployingViaFTP_cs.pdf)

> Il modo più semplice per distribuire un'applicazione ASP.NET consiste nel copiare manualmente i file necessari dall'ambiente di sviluppo all'ambiente di produzione. Questa esercitazione illustra come usare un client FTP per ottenere i file sul desktop per il provider di hosting web.


## <a name="introduction"></a>Introduzione

L'esercitazione precedente ha introdotto una semplice applicazione web ASP.NET di revisione manuale, è costituita da un numero limitato di pagine ASP.NET, una pagina master, una base personalizzata `Page` classe, un numero di immagini e fogli di stile tre CSS. A questo punto siamo pronti distribuire l'applicazione di un provider di hosting web, a quel punto l'applicazione sarà accessibile a tutti gli utenti con una connessione a Internet.


Da nostre discussioni nel [ *determinare quali file devono essere distribuiti* ](determining-what-files-need-to-be-deployed-cs.md) dell'esercitazione, si sa quali file devono essere copiati i provider di hosting web. (È opportuno ricordare che vengono copiati i file che varia a seconda se la compilazione dell'applicazione in modo esplicito o automaticamente.) Ma come possiamo ricavare i file dall'ambiente di sviluppo (desktop) fino a ambiente di produzione (web server gestiti dal provider host web)? Il [ **F** ile **T** trasferire **P** rotocol (FTP)](http://en.wikipedia.org/wiki/File_Transfer_Protocol) è due protocolli comunemente usati per copiare i file da un computer a un altro tramite una rete. Un'altra opzione è Server delle estensioni di FrontPage. Questa esercitazione è incentrata sull'utilizzo del software client FTP autonomo per distribuire i file necessari dall'ambiente di sviluppo all'ambiente di produzione.

> [!NOTE]
> Visual Studio include strumenti per la pubblicazione di siti Web tramite FTP. Questi strumenti, nonché un'occhiata a strumenti che usano FPSE, è illustrata nell'esercitazione successiva.


Per copiare i file tramite FTP è necessario un *client FTP* nell'ambiente di sviluppo. Un client FTP è un'applicazione progettata per copiare i file dal computer di cui è installato in un computer che esegue un' *server FTP*. (Se il provider di hosting web supporta i trasferimenti di file tramite FTP, come la maggior parte, quindi vi è un server FTP in esecuzione nei server web). Sono disponibili numerose applicazioni client FTP. Web browser può anche raddoppiare come un client FTP. Il client FTP preferito e quello si userà per questa esercitazione, viene [FileZilla](http://filezilla-project.org/), un client FTP gratuito e open source disponibile per Windows, Linux e Mac. Qualsiasi client FTP funzionerà, tuttavia, in tal caso è possibile usare qualsiasi client si ha maggiore familiarità con.

Se si segue lungo è necessario creare un account con un provider di hosting web prima di può completerà questa esercitazione o quelle successive. Come indicato nell'esercitazione precedente, esistono un gruppo di società provider host web con un'ampia gamma di prezzi, le funzionalità e qualità del servizio. Per questa serie di esercitazioni si userà [Discount ASP.NET](http://discountasp.net) come l'host web provider, ma è possibile proseguire con qualsiasi provider di hosting web, purché supportino la versione di ASP.NET il sito è sviluppato in. (Queste esercitazioni sono state create con ASP.NET 3.5.) Inoltre, poiché si sarà necessario copiare i file per il provider di hosting web tramite FTP in questa esercitazione e in futuro quelle è fondamentale che il provider di hosting web supporta l'accesso FTP per i server web. Praticamente tutti i provider host web offrono questa funzionalità, ma è necessario verificare prima di registrarsi.

## <a name="deploying-the-book-review-web-application-project"></a>Distribuzione del progetto di applicazione Web revisione manuale

È importante ricordare che esistono due versioni dell'applicazione web recensione: uno implementato usando il modello di progetto di applicazione Web (BookReviewsWAP) e l'altro che usa il modello di progetto di sito Web (BookReviewsWSP). Il tipo di progetto influenza se il sito viene compilato automaticamente o in modo esplicito e tale modello di compilazione determina quali file devono essere distribuiti. Di conseguenza, verranno esaminate distribuisce i progetti BookReviewsWAP e BookReviewsWSP separatamente, a partire dal BookReviewsWAP. Si consiglia di scaricare questi due applicazioni ASP.NET, se non è già.

Avviare il progetto BookReviewsWAP passando al `BookReviewsWAP` cartella e fare doppio clic il `BookReviewsWAP.sln` file. Prima di distribuire il progetto è importante per la compilazione per assicurarsi che tutte le modifiche al codice sorgente sono inclusi nell'assembly compilato. Per compilare il progetto passare al menu della compilazione e scegliere l'opzione di menu BookReviewsWAP di compilazione. In un singolo assembly, viene compilato il codice sorgente nel progetto `BookReviewsWAP.dll`, che viene inserito nel `Bin` cartella.

A questo punto siamo pronti distribuire i file necessari. Avviare il client FTP e connettersi al server web del provider host web. (Quando effettua l'iscrizione con una società di hosting web invierà posta elettronica le informazioni su come connettersi al server FTP, inclusi l'indirizzo server FTP, nonché un nome utente e password).

Copiare i file seguenti dal desktop alla cartella sito Web radice del provider host web. Quando si FTP al server web in web l'hosting di provider è probabile che nella directory sito Web radice. Tuttavia, alcuni provider host web presente una sottocartella denominata `www` o `wwwroot` che funge da cartella radice per i file del sito Web. Infine, quando i file FTPing potrebbe essere necessario creare la struttura di cartelle corrispondenti nell'ambiente di produzione - i `Bin` cartella, il `Fiction` cartella, il `Images` cartella e così via.

- `~/Default.aspx`
- `~/About.aspx`
- `~/Site.master`
- `~/Web.config`
- `~/Web.sitemap`
- Il contenuto completo del `Styles` cartella
- Il contenuto completo della `Images` cartella (e nella relativa sottocartella `BookCovers`)
- `~/Fiction/Default.aspx`
- `~/Fiction/Blaze.aspx`
- `~/Tech/Default.aspx`
- `~/Tech/CYOW.aspx`
- `~/Tech/TYASP35.aspx`
- `~/Bin/BookReviewsWAP.dll`

Figura 1 mostra FileZilla dopo che sono stati copiati i file necessari. FileZilla vengono visualizzati i file nel computer locale a sinistra e i file nel computer remoto sulla destra. Come nella figura 1, i file di codice sorgente ASP.NET, ad esempio `About.aspx.cs`, sono nel computer locale (l'ambiente di sviluppo), ma non sono stati copiati i provider di hosting web (l'ambiente di produzione) perché non è necessario essere distribuito quando si usano i file di codice compilazione esplicita.

> [!NOTE]
> È possibile che i file del codice sorgente nel server di produzione, come vengono ignorati. Per impostazione predefinita, in ASP.NET non consente le richieste HTTP a file di codice sorgente in modo che anche se i file del codice sorgente sono presenti nel server di produzione siano inaccessibile ai visitatori del sito Web. (Ovvero, se un utente prova a visitare `http://www.yoursite.com/Default.aspx.cs` otterranno una pagina di errore che spiega che questi tipi di file - `.cs` file - non è consentita.)


[![Usare un Client FTP per copiare i file necessari dal Desktop al Server del Web il Provider di hosting Web](deploying-your-site-using-an-ftp-client-cs/_static/image2.png)](deploying-your-site-using-an-ftp-client-cs/_static/image1.png)

**Figura 1**: usare un FTP Client per copiare i file necessari da Your Desktop al Server del Web il Provider di hosting Web ([fare clic per visualizzare l'immagine con dimensioni normali](deploying-your-site-using-an-ftp-client-cs/_static/image3.png))


Dopo la distribuzione del sito si consiglia di testare il sito. Se è stato acquistato un nome di dominio e configurato le impostazioni DNS in modo corretto, è possibile visitare il sito, immettere il nome di dominio. In alternativa, il provider di hosting web deve avere ha fornito un URL del sito, che avrà un aspetto simile *NomeAccount*. *webhostprovider*. com oppure *webhostprovider*. com /*accountname*. Ad esempio, è l'URL per il mio account su ASP.NET Discount: `http://httpruntime.web703.discountasp.net`.

Figura 2 mostra il sito distribuito le recensioni dei libri. Si noti che lo sto visualizzando nel Discount ASP. I server della rete, alla `http://httpruntime.web703.discountasp.net`. In questo momento chiunque abbia una connessione a Internet è stato possibile visualizzare il mio sito Web. Come si può immaginare, il sito Cerca e si comporta esattamente come rispetto al test in ambiente di sviluppo.

> [!NOTE]
> Se si verifica un errore quando si visualizzano l'applicazione si consiglia di verificare che è stato distribuito il set corretto di file. Successivamente, controllare il messaggio di errore per vedere se rivela eventuali indizi in merito al problema. In seguito, è possibile impostare in modo da supporto tecnico dell'azienda di host web o inviare una domanda nel forum appropriati nel [forum ASP.NET](https://forums.asp.net/).


[![Il sito di revisioni del libro è ora accessibile a chiunque disponga di una connessione Internet](deploying-your-site-using-an-ftp-client-cs/_static/image5.png)](deploying-your-site-using-an-ftp-client-cs/_static/image4.png)

**Figura 2**: il sito di revisioni del libro è ora accessibile a chiunque disponga di una connessione Internet ([fare clic per visualizzare l'immagine con dimensioni normali](deploying-your-site-using-an-ftp-client-cs/_static/image6.png))


## <a name="deploying-the-book-review-web-site-project"></a>La distribuzione del progetto di sito Web di revisione della Rubrica

Quando si distribuisce un'applicazione ASP.NET che usa la compilazione automatica, ad esempio il progetto di sito Web BookReviewsWSP, non vi è alcun assembly compilato nel `Bin` cartella. Di conseguenza, i file di codice sorgente dell'applicazione web devono essere distribuiti nell'ambiente di produzione. Di seguito viene illustrato questo processo.

Come con il progetto di applicazione Web è consigliabile iniziare compilando l'applicazione prima di distribuirlo. Mentre si compila un progetto sito Web non crea un assembly, controllare gli eventuali errori in fase di compilazione nella pagina. Migliore per individuare questi errori ora anziché aventi un visitatore al sito di eseguirne l'individuazione per te!

Dopo avere compilato correttamente il progetto, usare il client FTP per copiare i file seguenti nella cartella del sito Web radice del provider host web. Potrebbe essere necessario creare la struttura di cartelle corrispondenti nell'ambiente di produzione.

> [!NOTE]
> Se è già stato distribuito il BookReviewsWAP progetto ancora ma si desidera provare a distribuire il progetto BookReviewsWSP, eliminare innanzitutto tutti i file nel server web che sono stati caricati durante la distribuzione BookReviewsWAP e quindi distribuire i file per BookReviewsWSP.


- `~/Default.aspx`
- `~/Default.aspx.cs`
- `~/About.aspx`
- `~/About.aspx.cs`
- `~/Site.master`
- `~/Site.master.cs`
- `~/Web.config`
- `~/Web.sitemap`
- Il contenuto completo del `Styles` cartella
- Il contenuto completo della `Images` cartella (e nella relativa sottocartella `BookCovers`)
- `~/App_Code/BasePage.cs`
- `~/Fiction/Default.aspx`
- `~/Fiction/Default.aspx.cs`
- `~/Fiction/Blaze.aspx`
- `~/Fiction/Blaze.aspx.cs`
- `~/Tech/Default.aspx`
- `~/Tech/Default.aspx.cs`
- `~/Tech/CYOW.aspx`
- `~/Tech/CYOW.aspx.cs`
- `~/Tech/TYASP35.aspx`
- `~/Tech/TYASP35.aspx.cs`

Figura 3 mostra FileZilla dopo aver copiato i file necessari. Come può notare, ASP.NET file del codice sorgente, ad esempio `About.aspx.cs`, sono presenti nel computer locale (l'ambiente di sviluppo) sia il provider di hosting web (l'ambiente di produzione) perché i file di codice devono essere distribuiti quando si usa automatico compilazione.


[![Usare un Client FTP per copiare i file necessari dal Desktop al Server del Web il Provider di hosting Web](deploying-your-site-using-an-ftp-client-cs/_static/image8.png)](deploying-your-site-using-an-ftp-client-cs/_static/image7.png)

**Figura 3**: usare un FTP Client per copiare i file necessari da Your Desktop al Server del Web il Provider di hosting Web ([fare clic per visualizzare l'immagine con dimensioni normali](deploying-your-site-using-an-ftp-client-cs/_static/image9.png))


L'esperienza dell'utente non è interessata dal modello di compilazione dell'applicazione. Le stesse pagine ASP.NET sono accessibili e l'aspetto e lo stesso comportamento se il sito Web è stato creato mediante il modello di progetto di applicazione Web o il modello di progetto di sito Web.

### <a name="updating-a-web-application-on-production"></a>L'aggiornamento di un'applicazione Web in produzione

Distribuzione e lo sviluppo di applicazioni web non sono un processo unico. Ad esempio, quando si crea il sito Web recensione compilato varie pagine e ha scritto il codice associato nella mio PC (l'ambiente di sviluppo). Dopo aver raggiunto un determinato stato stabile, distribuito l'applicazione in modo che altri utenti è stato possibile visitare il sito e leggere le mie recensioni. Tuttavia, la distribuzione non contrassegnare la fine del mio sviluppo questo sito. Potrei aggiungere ulteriori le recensioni dei libri o implementare nuove funzionalità, ad esempio consentire my visitatori alla documentazione di frequenza o lasciare i propri commenti. Tali miglioramenti sono potrebbe essere sviluppati nell'ambiente di sviluppo e, al termine, sarebbe necessario per la distribuzione. Sviluppo e la distribuzione, pertanto, sono ciclici. Si sviluppa un'applicazione e quindi distribuirlo. Mentre il sito è online e nell'ambiente di produzione, vengono aggiunte nuove funzionalità e correzione dei bug nel tempo, ciò richiede la ridistribuzione dell'applicazione. E così via e così via.

Come si può intuire, quando un'applicazione web che è sufficiente per copiare i file nuovi e modificati vengono nuovamente distribuite. Non è necessario ridistribuire le pagine non modificate o file di supporto dal lato client o server (anche se è possibile a tale scopo).

> [!NOTE]
> Un aspetto da tenere presenti quando si usa la compilazione esplicita è che ogni volta che si aggiunge una nuova pagina ASP.NET al progetto o apportare modifiche al codice, è necessario ricompilare il progetto, che aggiorna l'assembly nel `Bin` cartella. Di conseguenza, è necessario copiare questo assembly aggiornato nell'ambiente di produzione quando si aggiorna un'applicazione web in produzione (con gli altri contenuti nuovi e aggiornati).


Inoltre comprendere che tutte le modifiche per il `Web.config` o i file nei `Bin` directory verrà arrestato e riavviato il Pool di applicazioni del sito Web. Se lo stato della sessione verrà archiviato utilizzando il `InProc` modalità (impostazione predefinita), quindi i visitatori del sito perderanno stato sessione ogni volta che vengono modificati i file di chiave. Per evitare questo inconveniente, è consigliabile archiviare sessione usando il `StateServer` o `SQLServer` modalità. Per altre informazioni su questo argomento leggere [modalità stato sessione](https://msdn.microsoft.com/library/ms178586.aspx).

Infine, tenere presente che un'applicazione vengono nuovamente distribuite possono essere necessari da pochi secondi per alcuni minuti, a seconda del numero e dimensioni dei file che devono essere copiati nell'ambiente di produzione. Durante questo periodo gli utenti che visitano il sito potrebbero verificarsi errori o comportamenti anomali. È possibile "disattivare" tutta l'applicazione mediante l'aggiunta di una pagina denominata `App_Offline.htm` alla directory radice dell'applicazione che illustra agli utenti che il sito è inattivo per manutenzione (o qualsiasi) e sarà possibile eseguire il backup a breve. Quando il `App_Offline.htm` file è presente, il runtime ASP.NET reindirizza tutte le richieste in ingresso a tale pagina.

## <a name="summary"></a>Riepilogo

Distribuzione di un'applicazione web vengono copiati i file necessari dall'ambiente di sviluppo all'ambiente di produzione. Il mezzo più comune da cui i file vengono trasferiti in rete è il protocollo FTP (File Transfer) e la maggior parte dei provider di host web supportano l'accesso FTP per i server web. In questa esercitazione è stato illustrato come usare un client FTP per distribuire i file necessari per il server web. Una volta distribuito, il sito Web possono essere visitati dagli tutti gli utenti con una connessione a Internet.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [App\_Offline.htm e risolvere la funzionalità "Errori di inserimento/espulsione descrittivo"](https://weblogs.asp.net/scottgu/App_5F00_Offline.htm-and-working-around-the-_2200_IE-Friendly-Errors_2200_-feature)
- [Modalità stato sessione](https://msdn.microsoft.com/library/ms178586.aspx)

> [!div class="step-by-step"]
> [Precedente](determining-what-files-need-to-be-deployed-cs.md)
> [Successivo](deploying-your-site-using-visual-studio-cs.md)
