---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-visual-studio-vb
title: La distribuzione del sito utilizzando Visual Studio (VB) | Documenti Microsoft
author: rick-anderson
description: Visual Studio include strumenti per la distribuzione di un sito Web. Altre informazioni su questi strumenti in questa esercitazione.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: 977105f3-7987-4e50-8be7-afb53b4ca28a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-visual-studio-vb
msc.type: authoredcontent
ms.openlocfilehash: af4257a91c08efc498c86aceac6fa7f64e527a74
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="deploying-your-site-using-visual-studio-vb"></a>La distribuzione del sito utilizzando Visual Studio (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare codice](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_04_VB.zip) o [Scarica il PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial04_DeployingViaVS_vb.pdf)

> Visual Studio include strumenti per la distribuzione di un sito Web. Altre informazioni su questi strumenti in questa esercitazione.


## <a name="introduction"></a>Introduzione

L'esercitazione precedente esaminato come distribuire una semplice applicazione web ASP.NET per un provider di hosting web. In particolare, l'esercitazione è stato illustrato come utilizzare un client FTP come FileZilla per trasferire i file necessari dall'ambiente di sviluppo all'ambiente di produzione. Visual Studio offre strumenti per semplificare la distribuzione di un provider di hosting web. In questa esercitazione esamina due di questi strumenti: lo strumento Copia sito Web, in cui è possibile spostare i file da e verso un server web remoto tramite FTP o estensioni del Server di FrontPage; e lo strumento di pubblicazione, che copia l'intero sito Web in una posizione specificata.


> [!NOTE]
> Altri strumenti correlati alla distribuzione di offerte da Visual Studio includono [progetti di installazione Web](https://msdn.microsoft.com/en-us/library/wx3b589t.aspx) e [progetti di distribuzione Web](https://www.microsoft.com/downloads/details.aspx?FamilyId=0AA30AE8-C73B-4BDD-BB1B-FE697256C459&amp;displaylang=en) aggiuntivo. Progetti di installazione Web pacchetto di contenuto di un sito Web e le informazioni di configurazione in un unico file MSI. Questa opzione è particolarmente utile per i siti Web distribuiti all'interno di una rete intranet o per le aziende che vendono un'applicazione web sotto forma di pacchetto che installano nei server web. Il Web distribuzione progetti di componente aggiuntivo è che un Visual Studio componente aggiuntivo che facilita le differenze di configurazione specificando tra compilazioni per ambienti di sviluppo e di ambienti di produzione. Progetti di installazione Web non vengono trattati in questa serie di esercitazioni; Progetti di distribuzione Web sono riepilogati nella [ *comuni configurazione differenze tra lo sviluppo e produzione* ](common-configuration-differences-between-development-and-production-vb.md) esercitazione.


## <a name="deploying-your-site-using-the-copy-web-site-tool"></a>La distribuzione del sito utilizzando lo strumento Copia sito Web

Strumento Copia sito Web di Visual Studio è simile a un client FTP autonomo. In breve, lo strumento Copia sito Web consente di connettersi a un sito web remoto tramite FTP o le estensioni del Server di FrontPage. Simile all'interfaccia utente del FileZilla, l'interfaccia utente Copia sito Web costituita da due riquadri: il riquadro sinistro elenca i file locali, mentre nel riquadro destro sono elencati i file nel server di destinazione.

> [!NOTE]
> Lo strumento Copia sito Web è disponibile solo per progetti di siti Web. Visual Studio offre questo strumento quando si lavora con un progetto di applicazione Web.


Esaminiamo un utilizzo dello strumento Copia sito Web per pubblicare l'applicazione recensioni nell'ambiente di produzione. Poiché lo strumento Copia sito Web funziona solo con i progetti che utilizzano il modello di progetto di sito Web è possibile esaminare solo con questo strumento con il progetto BookReviewsWSP. Aprire il progetto.

Avviare il progetto strumento Copia sito Web facendo clic sull'icona Copia sito Web in Esplora soluzioni (questa icona viene visualizzato un cerchio nella figura 1). In alternativa, è possibile selezionare l'opzione Copia sito Web dal menu sito Web. Entrambi gli approcci consente di avviare l'interfaccia utente Copia sito Web mostrato nella figura 1. Poiché è ancora necessario connettersi a un server remoto, viene popolato solo riquadro a sinistra nella figura 1.


[![Interfaccia utente dello strumento Copia sito Web è diviso in due riquadri](deploying-your-site-using-visual-studio-vb/_static/image2.png)](deploying-your-site-using-visual-studio-vb/_static/image1.png)

**Figura 1**: del sito Web dell'interfaccia utente dello strumento Copia la è suddiviso in due riquadri ([fare clic per visualizzare l'immagine ingrandita](deploying-your-site-using-visual-studio-vb/_static/image3.png))


Per distribuire il sito è necessario innanzitutto connettersi al provider host web. Fare clic sul pulsante Connetti nella parte superiore dell'interfaccia utente Copia sito Web. Verrà visualizzata la finestra di dialogo Apri sito Web mostrata nella figura 2.

È possibile connettersi al sito Web destinazione selezionando una delle quattro opzioni da sinistra:

- **File System** -selezionare questa opzione per distribuire il sito in una cartella o condivisione di rete accessibile dal computer.
- **IIS locale** -utilizzare questa opzione per distribuire il sito al server web IIS installato nel computer.
- **Sito FTP** -connettersi a un sito web remoto tramite FTP.
- **Sito remoto** -connettersi a un sito Web remoto utilizzando le estensioni del Server di FrontPage.

Supporta la maggior parte dei provider di host web FTP, ma meno offre il supporto delle estensioni del Server di FrontPage. Per questo motivo, ho selezionato l'opzione del sito FTP e quindi immettere le informazioni di connessione come illustrato nella figura 2.


[![Specificare il sito Web di destinazione](deploying-your-site-using-visual-studio-vb/_static/image5.png)](deploying-your-site-using-visual-studio-vb/_static/image4.png)

**Figura 2**: specificare il sito Web di destinazione ([fare clic per visualizzare l'immagine ingrandita](deploying-your-site-using-visual-studio-vb/_static/image6.png))


Dopo la connessione, lo strumento Copia sito Web carica i file nel sito remoto nel riquadro di destra e indica lo stato di ogni file: nuovi, eliminati, modificate o Unchanged. È possibile copiare un file dal sito locale per il sito remoto, o viceversa, a-mediante.

Aggiungere una nuova pagina al progetto BookReviewsWSP e distribuirlo in modo che possiamo vedere lo strumento Copia sito Web. Creare una nuova pagina ASP.NET in Visual Studio nella directory radice denominata `Privacy.aspx`. Impostare la pagina della pagina master `Site.master` e aggiungere l'informativa sulla privacy del sito in questa pagina. Figura 3 mostra Visual Studio dopo aver creata questa pagina.


[![Aggiungere una nuova pagina denominata &lt;codice&gt;Privacy.aspx&lt;/codice&gt; alla cartella radice del sito Web](deploying-your-site-using-visual-studio-vb/_static/image8.png)](deploying-your-site-using-visual-studio-vb/_static/image7.png)

**Figura 3**: aggiungere una nuova pagina denominata `Privacy.aspx` alla cartella radice del sito Web ([fare clic per visualizzare l'immagine ingrandita](deploying-your-site-using-visual-studio-vb/_static/image9.png))


Successivamente, tornare all'interfaccia utente di Copia sito Web. Come illustrato nella figura 4, il riquadro sinistro include ora i nuovi file - `Policy.aspx` e `Policy.aspx.vb`. Inoltre, questi file sono contrassegnati con un'icona di freccia e una stato della nuova, che indica l'esistenza del sito locale, ma non sul sito remoto.


[![Lo strumento Copia sito Web include il nuovo &lt;codice&gt;Privacy.aspx&lt;/codice&gt; pagina nel relativo riquadro di sinistra](deploying-your-site-using-visual-studio-vb/_static/image11.png)](deploying-your-site-using-visual-studio-vb/_static/image10.png)

**Figura 4**: lo strumento Copia sito Web include il nuovo `Privacy.aspx` pagina nel relativo riquadro di sinistra ([fare clic per visualizzare l'immagine ingrandita](deploying-your-site-using-visual-studio-vb/_static/image12.png))


Per distribuire il nuovo file, selezionarli e quindi fare clic sull'icona di freccia per trasferirle al sito remoto. Al termine il trasferimento di `Policy.aspx` e `Policy.aspx.vb` file presenti in entrambi i siti locali e remoti con lo stato Unchanged.

Con l'elenco di nuovi file, lo strumento Copia sito Web evidenzia tutti i file che differiscono tra i siti locali e remoti. Per vedere un esempio pratico, ripristinare il `Privacy.aspx` pagina e aggiungere alcune altre parole l'informativa sulla privacy. Salvare la pagina e quindi tornare allo strumento Copia sito Web. Come illustrato nella figura 5, il `Privacy.aspx` pagina nel riquadro di sinistra lo stato di modificato che indica che è sincronizzato con il sito remoto.


[![Lo strumento Copia sito Web indica che il &lt;codice&gt;Privacy.aspx&lt;/codice&gt; pagina è stata modificata.](deploying-your-site-using-visual-studio-vb/_static/image14.png)](deploying-your-site-using-visual-studio-vb/_static/image13.png)

**Figura 5**: lo strumento Copia sito Web indica che il `Privacy.aspx` pagina è stata modificata ([fare clic per visualizzare l'immagine ingrandita](deploying-your-site-using-visual-studio-vb/_static/image15.png))


Lo strumento Copia sito Web indica anche se un file è stato eliminato dall'ultima operazione di copia. Eliminare il `Privacy.aspx` dall'aggiornamento, lo strumento Copia sito Web del progetto locale. Il `Privacy.aspx` e `Privacy.aspx.vb` file rimangono elencati nel riquadro a sinistra, ma con stato Deleted che indica che sono stati rimossi dall'ultima operazione di copia.

## <a name="publishing-a-web-application"></a>Pubblicare un'applicazione Web

Un altro modo per distribuire l'applicazione web da Visual Studio consiste nell'utilizzare l'opzione di pubblicazione, che è accessibile tramite il menu Compila. L'opzione pubblica l'applicazione viene compilata in modo esplicito e copia tutti i file necessari fino a sito remoto specificato. Come si vedrà a breve, l'opzione di pubblicazione è più senza punta rispetto allo strumento Copia sito Web. Mentre lo strumento Copia sito Web consente di esaminare i file nei siti locali e remoti e consente di caricare o scaricare i singoli file in base alle esigenze, l'opzione pubblica consente di distribuire l'intera applicazione web.


Oltre a copiare tutti i file necessari per il sito remoto specificato, l'opzione pubblica consente di compilare in modo esplicito l'applicazione. Dato che i progetti di applicazione Web devono essere compilati in modo esplicito deve sorpresa che l'opzione di pubblicazione è disponibile per i progetti di applicazione Web. Quali potrebbero essere leggermente sorprendente è che l'opzione pubblica disponibile anche per i progetti di sito Web. Come indicato nel [ *determinare quali file devono essere distribuiti* ](determining-what-files-need-to-be-deployed-vb.md) esercitazione, i progetti di siti Web può essere compilate in modo esplicito tramite un processo noto come *precompilazione*. In questa esercitazione viene illustrato l'utilizzo dell'opzione di pubblicazione con progetti di applicazione Web. un'esercitazione in futura verrà esplorati pre-compilazione, a quel punto verrà restituiamo per esaminare utilizzando l'opzione pubblica i progetti di siti Web.

> [!NOTE]
> Mentre l'opzione di pubblicazione è disponibile in Visual Studio per progetti di siti Web e progetti di applicazione Web, Visual Web Developer offre solo l'opzione di pubblicazione per i progetti di applicazione Web.


Diamo un'occhiata distribuzione dell'applicazione recensioni utilizzando l'opzione di pubblicazione. Avvia aprendo BookReviewsWAP (il progetto di applicazione Web) in Visual Studio. Scegliere il progetto BookReviewsWAP compilare il menu di pubblicazione. Verrà visualizzata una finestra di dialogo che richiede il percorso di destinazione, tra le altre opzioni di configurazione (vedere Figura 6). Molto simile con lo strumento Copia sito Web è possibile immettere un percorso che punta a una cartella locale, un sito Web IIS locale, un sito Web remoto che supporta le estensioni del Server di FrontPage o un indirizzo del server FTP. È possibile scegliere se si desidera sostituire i file nel server web remoto con i file distribuiti o eliminare tutto il contenuto sul sito remoto prima della pubblicazione. È inoltre possibile specificare se copiare:

- Solo i file di progetto necessari per eseguire l'applicazione, che include il codice sorgente non necessari e i file di progetto.
- Tutti i file di progetto, che include i file del codice sorgente e i file di progetto di Visual Studio, come il file della soluzione.
- Tutti i file nella cartella di progetto di origine, che copia tutti i file nella cartella di progetto di origine indipendentemente dal fatto che vengono inclusi nel progetto.

È inoltre disponibile un'opzione per caricare il contenuto del `App_Data` cartella.


[![Specificare il sito Web di destinazione](deploying-your-site-using-visual-studio-vb/_static/image17.png)](deploying-your-site-using-visual-studio-vb/_static/image16.png)

**Figura 6**: specificare il sito Web di destinazione ([fare clic per visualizzare l'immagine ingrandita](deploying-your-site-using-visual-studio-vb/_static/image18.png))


Per l'applicazione recensioni sito remoto contiene i file distribuiti quando si copia il progetto BookReviewsWSP tramite lo strumento Copia sito Web. Pertanto, si avviare l'opzione di pubblicazione mediante l'eliminazione di tutto il contenuto esistente. Inoltre, si è sufficiente copiare i file necessari anziché ingombrare l'ambiente di produzione con file di codice e del progetto di origine non necessari. Dopo aver specificato queste opzioni, fare clic sul pulsante pubblica. Su prossimi secondi Visual Studio distribuirà i file necessari per il sito di destinazione, la visualizzazione dello stato di avanzamento nella finestra di Output.

Figura 7 illustra i file nel sito FTP, una volta completata l'operazione di pubblicazione. Si noti che solo le pagine di codice e i file di supporto necessario server - e sul lato client sono stati caricati.


[![Solo i file necessari sono stati pubblicati nell'ambiente di produzione](deploying-your-site-using-visual-studio-vb/_static/image20.png)](deploying-your-site-using-visual-studio-vb/_static/image19.png)

**Figura 7**: solo i necessari file sono stati pubblicati nell'ambiente di produzione ([fare clic per visualizzare l'immagine ingrandita](deploying-your-site-using-visual-studio-vb/_static/image21.png))


L'opzione pubblica è uno strumento sul minore rispetto allo strumento Copia sito Web. Mentre lo strumento Copia sito Web consente di controllare i file nei siti locali e remoti e visualizzare le differenze, l'opzione pubblica non fornisce alcuna interfaccia. Inoltre, lo strumento Copia sito Web consente di apportare modifiche a One-Off, il caricamento o l'eliminazione di singoli file. L'opzione pubblica non consente tale controllo con granularità fine; al contrario, pubblica il *intero* dell'applicazione. Questo comportamento presenta vantaggi e svantaggi. D'altra parte, conoscere quando si utilizza l'opzione di pubblicazione che è non dimenticare di caricare un file importanti. Si consideri cosa succede che se sono state apportate una piccola modifica a un sito Web di dimensioni molto grandi, con l'opzione pubblica non è possibile aggiornare la pagina o due che è stata modificata, ma invece è necessario attendere mentre Visual Studio distribuisce l'intero sito.

Non è insolito che determinati file il cui contenuto è diversa tra la produzione e di ambienti di sviluppo sia disponibile. Un esempio di chiave è il file di configurazione dell'applicazione, `Web.config`. Poiché l'opzione Pubblica copia automaticamente i file dell'applicazione web sovrascrive i file di configurazione personalizzato dell'ambiente di produzione con la versione nell'ambiente di sviluppo. L'esercitazione successive vengono illustrati in questo argomento vengono e offre suggerimenti per la distribuzione di un'applicazione web quando tali differenze.

## <a name="summary"></a>Riepilogo

Distribuzione di un sito Web comporta la copia i file necessari dall'ambiente di sviluppo all'ambiente di produzione. L'esercitazione precedente è stato illustrato come trasferire file tramite un client FTP come FileZilla. In questa esercitazione esaminati due strumenti di distribuzione in Visual Studio: lo strumento Copia sito Web e l'opzione pubblica. Lo strumento Copia sito Web è simile a un client FTP, in quanto dispone di un'interfaccia riquadri con due Elenca i file nel computer locale e un computer remoto specificato che rende più semplice caricare o scaricare i file tra i due computer. L'opzione pubblica è uno strumento più senza punta che il progetto viene compilato in modo esplicito e quindi distribuisce l'intera applicazione nella destinazione specificata.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Copia sito Web con lo strumento Copia sito Web](https://msdn.microsoft.com/en-us/library/1cc82atw.aspx)
- [Come ricerca per categorie: distribuire un sito Web tramite lo strumento Copia sito Web](../../../videos/how-do-i/how-do-i-deploy-a-web-site-using-the-copy-web-site-tool.md) (Video)
- [Procedura: Pubblicare progetti di applicazione Web](https://msdn.microsoft.com/en-us/library/aa983453.aspx)
- [Procedura: Pubblicazione di siti Web](https://msdn.microsoft.com/en-us/library/20yh9f1b.aspx)
- [Il programma di installazione e distribuzione di progetti in Visual Studio](https://msdn.microsoft.com/en-us/library/wx3b589t.aspx)

>[!div class="step-by-step"]
[Precedente](deploying-your-site-using-an-ftp-client-vb.md)
[Successivo](common-configuration-differences-between-development-and-production-vb.md)
