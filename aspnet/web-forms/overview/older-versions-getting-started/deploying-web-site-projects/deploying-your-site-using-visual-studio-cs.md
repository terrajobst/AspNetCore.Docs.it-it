---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-visual-studio-cs
title: Distribuzione del sito tramite Visual Studio (c#) | Microsoft Docs
author: rick-anderson
description: Visual Studio include strumenti per la distribuzione di un sito Web. Altre informazioni su questi strumenti in questa esercitazione.
ms.author: aspnetcontent
ms.date: 04/01/2009
ms.assetid: cde4ee53-a5d0-4937-a54b-67877e8266c3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-visual-studio-cs
msc.type: authoredcontent
ms.openlocfilehash: 38fb133a30f8b4516078b14cd67b2a0d53c7d482
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37810131"
---
<a name="deploying-your-site-using-visual-studio-c"></a>Distribuzione del sito tramite Visual Studio (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare il codice](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_04_CS.zip) o [Scarica il PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial04_DeployingViaVS_cs.pdf)

> Visual Studio include strumenti per la distribuzione di un sito Web. Altre informazioni su questi strumenti in questa esercitazione.


## <a name="introduction"></a>Introduzione

L'esercitazione precedente ha illustrato come distribuire una semplice applicazione web ASP.NET a un provider di hosting web. In particolare, l'esercitazione ha illustrato come usare un client FTP, ad esempio FileZilla per trasferire i file necessari dall'ambiente di sviluppo all'ambiente di produzione. Visual Studio offre anche strumenti incorporati per facilitare la distribuzione di un provider di hosting web. Questa esercitazione esamina due di questi strumenti: lo strumento Copia sito Web, in cui è possibile spostare file da e verso un server web remoto tramite FTP o le estensioni di Server di FrontPage. e lo strumento di pubblicazione, che consente di copiare l'intero sito Web in una posizione specificata.


> [!NOTE]
> Altri strumenti correlati alla distribuzione offerte da Visual Studio includono [i progetti di installazione Web](https://msdn.microsoft.com/library/wx3b589t.aspx) e [progetti di distribuzione Web](https://www.microsoft.com/downloads/details.aspx?FamilyId=0AA30AE8-C73B-4BDD-BB1B-FE697256C459&amp;displaylang=en) aggiuntivo. I progetti di installazione Web pacchetto di contenuto di un sito Web e le informazioni di configurazione in un unico file MSI. Questa opzione è particolarmente utile per i siti Web distribuiti all'interno di una rete intranet o per le aziende che vendono un'applicazione web preconfezionato che i clienti installano nei propri server web. Il componente aggiuntivo In progetti di distribuzione Web è che un Visual Studio componente aggiuntivo che facilita specificando le differenze di configurazione tra le compilazioni di ambienti di sviluppo e ambienti di produzione. I progetti di installazione Web non vengono trattati in questa serie di esercitazioni; Progetti di distribuzione Web sono riepilogati nella [ *differenze di configurazione comuni tra sviluppo e produzione* ](common-configuration-differences-between-development-and-production-cs.md) esercitazione.


## <a name="deploying-your-site-using-the-copy-web-site-tool"></a>Distribuzione del sito tramite lo strumento Copia sito Web

Strumento Copia sito Web di Visual Studio è simile a un client FTP autonomo. In breve, lo strumento Copia sito Web consente di connettersi a un sito web remoto tramite FTP o estensioni del Server di FrontPage. Simile all'interfaccia utente di FileZilla, l'interfaccia utente Copia sito Web è costituito da due riquadri: nel riquadro a sinistra sono elencati i file locali mentre nel riquadro a destra sono elencati i file nel server di destinazione.

> [!NOTE]
> Lo strumento Copia sito Web è disponibile solo per progetti di siti Web. Visual Studio offre questo strumento quando si lavora con un progetto di applicazione Web.

Diamo un'occhiata a utilizzando lo strumento Copia sito Web per pubblicare l'applicazione di recensione nell'ambiente di produzione. Poiché lo strumento Copia sito Web funziona solo con i progetti che usano il modello di progetto di sito Web è possibile esaminare solo usando questo strumento con il progetto BookReviewsWSP. Aprire tale progetto.

Avviare il progetto dello strumento Copia sito Web facendo clic sull'icona Copia sito Web in Esplora soluzioni (questa icona viene visualizzato un cerchio nella figura 1). In alternativa, è possibile selezionare l'opzione di Copia sito Web dal menu sito Web. Entrambi gli approcci avvia l'interfaccia utente di Copia sito Web mostrato nella figura 1; solo il riquadro a sinistra nella figura 1 viene popolato perché abbiamo ancora a connettersi a un server remoto.


[![L'interfaccia utente dello strumento Copia sito Web è suddivisa in due riquadri](deploying-your-site-using-visual-studio-cs/_static/image2.png)](deploying-your-site-using-visual-studio-cs/_static/image1.png)

**Figura 1**: sito Web dell'interfaccia utente dello strumento Copia la è suddiviso in due riquadri ([fare clic per visualizzare l'immagine con dimensioni normali](deploying-your-site-using-visual-studio-cs/_static/image3.png))


Per distribuire il sito è necessario innanzitutto connettersi al provider di hosting web. Fare clic sul pulsante Connetti nella parte superiore dell'interfaccia utente Copia sito Web. Verrà visualizzata la finestra di dialogo Apri sito Web, illustrato nella figura 2.

È possibile connettersi al sito Web destinazione selezionando una delle quattro opzioni a sinistra:

- **File System** -selezionare questa opzione per distribuire il sito in una cartella o condivisione di rete accessibile dal computer.
- **IIS locale** -usare questa opzione per distribuire il sito al server web IIS installato nel computer.
- **Sito FTP** -connettersi a un sito web remoto tramite FTP.
- **Sito remoto** -connettersi a un sito Web remoto tramite estensioni del Server di FrontPage.

La maggior parte dei provider di host web supporto FTP, ma meno offre il supporto di estensione del Server di FrontPage. Per questo motivo, ho selezionato l'opzione sito FTP e quindi immettere le informazioni di connessione come illustrato nella figura 2.


[![Specificare il sito Web destinazione](deploying-your-site-using-visual-studio-cs/_static/image5.png)](deploying-your-site-using-visual-studio-cs/_static/image4.png)

**Figura 2**: specificare il sito Web di destinazione ([fare clic per visualizzare l'immagine con dimensioni normali](deploying-your-site-using-visual-studio-cs/_static/image6.png))


Dopo la connessione, lo strumento Copia sito Web carica i file nel sito remoto nel riquadro di destra e indica lo stato di ogni file: nuovi, eliminati, modificate o Unchanged. È possibile copiare un file dal sito locale per il sito remoto, e viceversa a.

È possibile aggiungere una nuova pagina al progetto BookReviewsWSP e distribuirlo in modo che possiamo vedere lo strumento Copia sito Web in azione. Creare una nuova pagina ASP.NET in Visual Studio nella directory radice denominata `Privacy.aspx`. Hanno la pagina utilizzare la pagina master `Site.master` e aggiungere informativa sulla privacy del sito in questa pagina. Figura 3 mostra Visual Studio dopo aver creata questa pagina.


[![Aggiungere una nuova pagina denominata &lt;codice&gt;Privacy.aspx&lt;/code&gt; alla cartella radice del sito Web](deploying-your-site-using-visual-studio-cs/_static/image8.png)](deploying-your-site-using-visual-studio-cs/_static/image7.png)

**Figura 3**: aggiungere una nuova pagina denominata `Privacy.aspx` alla cartella radice del sito Web ([fare clic per visualizzare l'immagine con dimensioni normali](deploying-your-site-using-visual-studio-cs/_static/image9.png))


Successivamente, tornare all'interfaccia utente Copia sito Web. Come illustrato nella figura 4, il riquadro sinistro include ora i nuovi file - `Policy.aspx` e `Policy.aspx.cs`. Inoltre, questi file sono contrassegnati con un'icona di freccia e un stato di nuovo, che indica che essi esistano nel sito locale, ma non sul sito remoto.


[![Lo strumento Copia sito Web include il nuovo &lt;codice&gt;Privacy.aspx&lt;/code&gt; pagina nel relativo riquadro a sinistra](deploying-your-site-using-visual-studio-cs/_static/image11.png)](deploying-your-site-using-visual-studio-cs/_static/image10.png)

**Figura 4**: lo strumento Copia sito Web include il nuovo `Privacy.aspx` pagina nel relativo riquadro di sinistra ([fare clic per visualizzare l'immagine con dimensioni normali](deploying-your-site-using-visual-studio-cs/_static/image12.png))


Per distribuire il nuovo file, selezionarli e quindi fare clic sull'icona di freccia per li trasferiscono al sito remoto. Dopo il trasferimento viene completato il `Policy.aspx` e `Policy.aspx.cs` file presenti in entrambi i siti locali e remoti con lo stato Unchanged.

Insieme a elencano i nuovi file, lo strumento Copia sito Web evidenzia tutti i file che differiscono tra i siti locali e remoti. Per visualizzare questa azione, tornare al `Privacy.aspx` pagina e aggiungere alcune altre parole l'informativa sulla privacy. Salvare la pagina e quindi tornare allo strumento Copia sito Web. Come illustrato nella figura 5, il `Privacy.aspx` pagina nel riquadro di sinistra lo stato di modifica che indica che è sincronizzato con il sito remoto.


[![Lo strumento Copia sito Web indica che il &lt;codice&gt;Privacy.aspx&lt;/code&gt; pagina è stata modificata](deploying-your-site-using-visual-studio-cs/_static/image14.png)](deploying-your-site-using-visual-studio-cs/_static/image13.png)

**Figura 5**: lo strumento Copia sito Web indica che il `Privacy.aspx` pagina è stata modificata ([fare clic per visualizzare l'immagine con dimensioni normali](deploying-your-site-using-visual-studio-cs/_static/image15.png))


Lo strumento Copia sito Web indica anche se un file è stato eliminato dall'ultima operazione di copia. Eliminare il `Privacy.aspx` dal progetto locale e aggiorna lo strumento Copia sito Web. Il `Privacy.aspx` e `Privacy.aspx.cs` file rimangono elencati nel riquadro sinistro, ma con stato Deleted che indica che essi sono stati rimossi dall'ultima operazione di copia.

## <a name="publishing-a-web-application"></a>Pubblicare un'applicazione Web

Un altro modo per distribuire l'applicazione web dall'interno di Visual Studio consiste nell'utilizzare l'opzione pubblica, che è accessibile tramite il menu Compila. L'opzione pubblica l'applicazione viene compilata in modo esplicito e copia quindi tutti i file necessari fino a sito remoto specificato. Come vedremo tra breve, l'opzione pubblica è più d'impatto, rispetto allo strumento Copia sito Web. Mentre lo strumento Copia sito Web consente di esaminare i file nei siti locali e remoti e consente di caricare o scaricare i singoli file in base alle necessità, l'opzione pubblica consente di distribuire l'applicazione web intera.


Oltre a copiare tutti i file necessari per il sito remoto specificato, l'opzione pubblica compila anche in modo esplicito l'applicazione. Dato che i progetti applicazione Web devono essere compilati in modo esplicito deve disporre come non è una sorpresa che l'opzione pubblica è disponibile per i progetti applicazione Web. Ciò che può essere un po' sorprendente è che l'opzione pubblica è anche disponibile per i progetti di sito Web. Come indicato nella [ *determinare quali file devono essere distribuiti* ](determining-what-files-need-to-be-deployed-cs.md) esercitazione, i progetti di siti Web può essere compilate in modo esplicito tramite un processo detto *precompilazione*. Questa esercitazione è incentrata sull'utilizzo dell'opzione di pubblicazione con i progetti applicazione Web. un'esercitazione futura illustrerà la precompilazione, momento in cui verrà restituita per spiega come usare l'opzione pubblica con progetti di siti Web.

> [!NOTE]
> Anche se l'opzione pubblica è disponibile in Visual Studio per progetti di siti Web e progetti di applicazione Web, Visual Web Developer offre solo l'opzione di pubblicazione per i progetti applicazione Web.


Diamo un'occhiata distribuzione dell'applicazione le recensioni dei libri usando l'opzione pubblica. Iniziare aprendo BookReviewsWAP (il progetto di applicazione Web) in Visual Studio. Dal menu di pubblicazione scegliere il progetto di compilazione BookReviewsWAP. Verrà visualizzata una finestra di dialogo che richiede il percorso di destinazione, tra le altre opzioni di configurazione (vedere la figura 6). Molto simile con lo strumento Copia sito Web è possibile immettere un percorso che punta a una cartella locale, un sito Web IIS locale, un sito Web remoto che supporta le estensioni del Server di FrontPage, o un indirizzo del server FTP. È possibile scegliere se si desidera sostituire i file nel server web remoto con i file distribuiti o eliminare tutto il contenuto dal sito remoto prima della pubblicazione. È anche possibile specificare se copiare:

- Solo i file di progetto necessari per eseguire l'applicazione, che consente di omettere il codice sorgente non necessari e i file correlati al progetto.
- Tutti i file di progetto, che include i file del codice sorgente e file di progetto di Visual Studio, ad esempio il file della soluzione.
- Tutti i file nella cartella di progetto di origine, che consente di copiare tutti i file nella cartella di progetto di origine indipendentemente dal fatto che sono stati inclusi nel progetto.

È inoltre disponibile un'opzione per caricare il contenuto del `App_Data` cartella.


[![Specificare il sito Web destinazione](deploying-your-site-using-visual-studio-cs/_static/image17.png)](deploying-your-site-using-visual-studio-cs/_static/image16.png)

**Figura 6**: specificare il sito Web di destinazione ([fare clic per visualizzare l'immagine con dimensioni normali](deploying-your-site-using-visual-studio-cs/_static/image18.png))


Per l'applicazione di recensione sito remoto contiene i file vengano distribuiti durante la copia del progetto BookReviewsWSP tramite lo strumento Copia sito Web. Pertanto, è possibile avere l'opzione pubblica provare innanzitutto a eliminare tutto il contenuto esistente. Inoltre, è possibile semplicemente copiare i file necessari, anziché ingombrare l'ambiente di produzione con file di progetto e codice sorgente non necessari. Dopo aver specificato queste opzioni, fare clic sul pulsante pubblica. Per i secondi diversi successivi Visual Studio distribuirà i file necessari al sito di destinazione, la visualizzazione dello stato di avanzamento nella finestra di Output.

Figura 7 mostra i file sul sito FTP dopo l'operazione di pubblicazione è stata completata. Si noti che solo le pagine di codice e i file di supporto necessari sever-client-side e sono stati caricati.


[![Solo i file necessari sono stati pubblicati nell'ambiente di produzione](deploying-your-site-using-visual-studio-cs/_static/image20.png)](deploying-your-site-using-visual-studio-cs/_static/image19.png)

**Figura 7**: solo i necessari file sono stati pubblicati nell'ambiente di produzione ([fare clic per visualizzare l'immagine con dimensioni normali](deploying-your-site-using-visual-studio-cs/_static/image21.png))


L'opzione pubblica è uno strumento meno maggior numero di sfumature rispetto allo strumento Copia sito Web. Mentre lo strumento Copia sito Web consente di esaminare i file nei siti locali e remoti e notare le differenze, l'opzione pubblica non fornisce alcuna interfaccia di questo tipo. Inoltre, lo strumento Copia sito Web consente di apportare modifiche occasionali, caricamento o l'eliminazione di singoli file. L'opzione pubblica non supporta tale controllo con granularità fine. al contrario, pubblica i *intera* dell'applicazione. Questo comportamento presenta vantaggi e svantaggi. D'altra parte, sa quando si usa l'opzione pubblica che è non possibile se si dimentica di caricare un file importante. Si consideri che cosa accade che se sono state apportate una piccola modifica per un sito Web di dimensioni molto grandi, con l'opzione pubblica non è possibile aggiornare la pagina o due che è stata modificata, ma è invece necessario attendere mentre Visual Studio distribuisce l'intero sito.

Non è insolito per essere determinati file il cui contenuto è diverso tra gli ambienti di sviluppo e produzione. Un esempio fondamentale è il file di configurazione dell'applicazione, `Web.config`. Poiché l'opzione pubblica alla cieca copia i file dell'applicazione web sovrascrive i file di configurazione personalizzato dell'ambiente di produzione con la versione nell'ambiente di sviluppo. L'esercitazione successiva illustra ulteriormente in questo argomento e offre suggerimenti per la distribuzione di un'applicazione web quando sono presenti differenze di questo tipo.

## <a name="summary"></a>Riepilogo

La distribuzione di un sito Web richiede la copia dei file necessari dall'ambiente di sviluppo all'ambiente di produzione. L'esercitazione precedente è stato illustrato come trasferire file usando un client FTP, ad esempio FileZilla. Questa esercitazione esaminati due strumenti di distribuzione in Visual Studio: lo strumento Copia sito Web e l'opzione pubblica. Lo strumento Copia sito Web è simile a un client FTP poiché dispone di un'interfaccia due riquadri elenco dei file nel computer locale e un computer remoto specificato che rende più semplice caricare o scaricare i file tra i due computer. L'opzione pubblica è uno strumento d'impatto, più che il progetto viene compilato in modo esplicito e quindi distribuisce l'intera applicazione nella destinazione specificata.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Copia sito Web con lo strumento Copia sito Web](https://msdn.microsoft.com/library/1cc82atw.aspx)
- [Come ricerca per categorie: distribuire un sito Web utilizzando lo strumento Copia sito Web](../../../videos/how-do-i/how-do-i-deploy-a-web-site-using-the-copy-web-site-tool.md) (Video)
- [Procedura: Pubblicare progetti di applicazione Web](https://msdn.microsoft.com/library/aa983453.aspx)
- [Procedura: Pubblicazione di siti Web](https://msdn.microsoft.com/library/20yh9f1b.aspx)
- [Il programma di installazione e distribuzione di progetti in Visual Studio](https://msdn.microsoft.com/library/wx3b589t.aspx)

> [!div class="step-by-step"]
> [Precedente](deploying-your-site-using-an-ftp-client-cs.md)
> [Successivo](common-configuration-differences-between-development-and-production-cs.md)
