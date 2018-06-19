---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
title: 'Distribuzione Web ASP.NET utilizzando Visual Studio: distribuzione di un aggiornamento del codice | Documenti Microsoft'
author: tdykstra
description: Questa serie di esercitazioni viene illustrato come distribuire un ASP.NET (pubblica) per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web da utilizza...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: c76dbc35-a914-4ee3-919c-4f4d1fa05104
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
msc.type: authoredcontent
ms.openlocfilehash: dd02b5c627fbfbb0034030f4c21207d24f6aabce
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30881334"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-code-update"></a>Distribuzione Web ASP.NET utilizzando Visual Studio: distribuzione di un aggiornamento del codice
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Scaricare il progetto Starter](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Questa serie di esercitazioni viene illustrato come distribuire un ASP.NET (pubblica) per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web utilizzando Visual Studio 2012 o Visual Studio 2010. Per informazioni sulla serie, vedere [la prima esercitazione di serie](introduction.md).


## <a name="overview"></a>Panoramica

Dopo la distribuzione iniziale, il lavoro di gestione e il sito web di sviluppo continua e poco tempo è possibile distribuire un aggiornamento. In questa esercitazione illustra il processo di distribuzione di un aggiornamento per il codice dell'applicazione. L'aggiornamento implementare e distribuire in questa esercitazione non comporta la modifica un database. che cos'è diverso sulla distribuzione di una modifica al database nella prossima esercitazione verrà visualizzato.

Promemoria: Se viene visualizzato un messaggio di errore o non funzioni man mano che Leggi l'esercitazione, assicurarsi di controllare il [risoluzione dei problemi di pagina](troubleshooting.md).

## <a name="make-a-code-change"></a>Modificare il codice

Un esempio semplice di un aggiornamento per l'applicazione, aggiungerai il **i docenti** pagina un elenco di corsi tenuti da istruttore selezionato.

Se si esegue il **i docenti** pagina, si noterà che non esistono **selezionare** collegamenti nella griglia, ma non se si diverso da rendere il grigio attiva in background di riga.

![Pagina istruttori con selezione](deploying-a-code-update/_static/image1.png)

Ora si aggiungerà il codice che viene eseguita quando il **selezionare** collegamento viene selezionato e visualizza un elenco di corsi tenuti da istruttore selezionato.

1. In *Instructors.aspx*, aggiungere il markup seguente immediatamente dopo il **ErrorMessageLabel** `Label` controllo:

    [!code-aspx[Main](deploying-a-code-update/samples/sample1.aspx)]
2. Eseguire la pagina e selezionare un docente. Viene visualizzato un elenco di corsi tenuti da tale istruttore.

    ![Pagina istruttori con corsi tenuti](deploying-a-code-update/_static/image2.png)
3. Chiudere il browser.

## <a name="deploy-the-code-update-to-the-test-environment"></a>Distribuire l'aggiornamento di codice per l'ambiente di test

È possibile utilizzare i profili di pubblicazione per la distribuzione di test, gestione temporanea e produzione, è necessario modificare le opzioni di pubblicazione di database. Non è necessario eseguire gli script di distribuzione grant e i dati per il database delle appartenenze.

1. Aprire il **pubblica sul Web** guidata facendo clic su progetto ContosoUniversity e facendo clic su **pubblica**.
2. Fare clic su di **Test** profilo nel **profilo** elenco a discesa.
3. Fare clic su di **impostazioni** scheda.
4. In **DefaultConnection** nel **database** sezione, deselezionare il **Aggiorna database** casella di controllo.
5. Fare clic sul **profilo** scheda e quindi scegliere il **gestione temporanea** profilo nel **profilo** elenco a discesa.
6. Quando viene chiesto se si desidera salvare le modifiche apportate al **Test** del profilo, fare clic su **Sì**.
7. Apportare la stessa modifica nel profilo di gestione temporanea.
8. Ripetere il processo per apportare la stessa modifica nel profilo di produzione.
9. Chiudi il **pubblica sul Web** procedura guidata.

La distribuzione nell'ambiente di testing è ora sufficiente in esecuzione un solo clic pubblicare di nuovo. Per rendere più veloce questo processo, è possibile utilizzare il **Web-pubblicazione con un clic** barra degli strumenti.

1. Nel **vista** menu, scegliere **barre degli strumenti** e quindi selezionare **Web-pubblicazione con un clic**.

    ![Selecting_One_Click_Publish_toolbar](deploying-a-code-update/_static/image3.png)
2. In **Esplora**, selezionare il progetto ContosoUniversity.
3. il **Web-pubblicazione con un clic** sulla barra degli strumenti, scegliere il **Test** profilo di pubblicazione e quindi fare clic su **pubblica sul Web** (l'icona con frecce rivolte a sinistra e destra).

    ![Web_One_Click_Publish_toolbar](deploying-a-code-update/_static/image4.png)
4. Visual Studio distribuisce l'applicazione aggiornata e il browser apre automaticamente alla home page.
5. Eseguire la pagina istruttori e selezionare un docente per verificare che l'aggiornamento è stato distribuito correttamente.

È normalmente anche eseguire test di regressione (vale a dire il resto del sito per assicurarsi che la nuova modifica non è stato interrotto le funzionalità esistenti di test). Ma per questa esercitazione verrà ignorare tale passaggio e continuare a distribuire l'aggiornamento di gestione temporanea e produzione.

Quando si ridistribuisce, solo copie modificare file nel server di distribuzione Web determina automaticamente quali file sono stati modificati. Per impostazione predefinita, distribuzione Web viene utilizzato Data ultima modifica su file per determinare quali sono stati modificati. Alcuni sistemi di controllo di origine modificare il file date anche quando non si modifica il contenuto del file. In tal caso, si potrebbe desidera configurare distribuzione Web per utilizzare i checksum di file per determinare quali file sono stati modificati. Per ulteriori informazioni, vedere [motivo per cui tutti i file ottenere ridistribuiti anche se non è stata modificarle?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) in domande frequenti sulla distribuzione di ASP.NET.

## <a name="take-the-application-offline-during-deployment"></a>Eseguire l'applicazione durante la distribuzione

La modifica che si esegue la distribuzione è ora una semplice modifica a una singola pagina. Ma a volte si distribuiscono le modifiche più estese, o di distribuire le modifiche di codice e database e il sito potrebbe comportarsi in modo non corretto se un utente richiede una pagina prima del completamento della distribuzione. Per impedire agli utenti di accedere al sito, mentre la distribuzione è in corso, è possibile utilizzare un *app\_offline.htm* file. Quando si inserisce un file denominato *app\_offline.htm* nella cartella radice dell'applicazione, IIS viene visualizzato automaticamente tale file anziché eseguire l'applicazione. Per impedire l'accesso durante la distribuzione, per l'inserimento *app\_offline.htm* nella cartella radice, eseguire il processo di distribuzione, quindi rimuovere *app\_offline.htm* dopo l'esito positivo distribuzione.

È possibile configurare la distribuzione Web per inserire automaticamente un valore predefinito *app\_offline.htm* file nel server quando viene avviata la distribuzione e rimuoverlo al termine. Per eseguire l'operazione che si dovranno eseguire è aggiungere l'elemento XML seguente nel file di profilo (con estensione pubxml) di pubblicazione:

[!code-xml[Main](deploying-a-code-update/samples/sample2.xml)]

Per questa esercitazione verrà visualizzato come creare e utilizzare un oggetto personalizzato *app\_offline.htm* file.

Utilizzando *app\_offline.htm* nel sito di staging non è necessario, perché non si dispone di utenti che accedono al sito di staging. Ma è consigliabile utilizzare gestione temporanea per testare tutto quello che si prevede di distribuire nell'ambiente di produzione.

### <a name="create-appofflinehtm"></a>Creare app\_offline.htm

1. In **Esplora**, fare doppio clic la soluzione e fare clic su **Aggiungi**, quindi fare clic su **nuovo elemento**.
2. Creare un **pagina HTML** denominato *app\_offline.htm* (eliminare finale "l" il *HTML* estensione di Visual Studio crea per impostazione predefinita).
3. Sostituire il markup del modello con il markup seguente:

    [!code-html[Main](deploying-a-code-update/samples/sample3.html)]
4. Salvare e chiudere il file.

### <a name="copy-appofflinehtm-to-the-root-folder-of-the-web-site"></a>Copia app\_offline.htm nella cartella radice del sito web

È possibile utilizzare qualsiasi strumento FTP per copiare i file del sito web. [FileZilla](http://filezilla-project.org/) è uno strumento largamente usato FTP e viene visualizzato nelle schermate.

Per utilizzare uno strumento FTP, sono necessari tre operazioni: l'URL di FTP, il nome utente e la password.

L'URL viene visualizzato nella pagina dashboard del sito web nel portale di gestione di Azure e il nome utente e la password per il FTP, vedere il *publishsettings* file scaricato in precedenza. La procedura seguente viene illustrato come ottenere questi valori.

1. Nel portale di gestione di Azure, fare clic su **siti Web** scheda e quindi scegliere il sito web di gestione temporanea.
2. Nel **Dashboard** scorrere fino a trovare il nome host FTP nella pagina di **Quick Glance** sezione.

    ![Nome host FTP](deploying-a-code-update/_static/image5.png)
3. Aprire la gestione temporanea *publishsettings* file in blocco note o un altro editor di testo.
4. Trovare il `publishProfile` elemento per il profilo FTP.
5. Copia il `userName` e `userPWD` valori.

    ![Credenziali FTP](deploying-a-code-update/_static/image6.png)
6. Aprire la strumento FTP e accedere a URL FTP.
7. Copia *app\_offline.htm* dalla cartella della soluzione per il */sito/wwwroot* cartella nel sito di staging.

    ![Copia app_offline](deploying-a-code-update/_static/image7.png)
8. Passare all'URL del sito di gestione temporanea. Vedrai che il *app\_offline.htm* pagina viene visualizzata invece la home page.

    ![App_offline.htm nella finestra del browser](deploying-a-code-update/_static/image8.png)

A questo punto si è pronti distribuire in gestione temporanea.

## <a name="deploy-the-code-update-to-staging-and-production"></a>Distribuire l'aggiornamento del codice in gestione temporanea e produzione

1. Nel **Web-pubblicazione con un clic** sulla barra degli strumenti, scegliere il **gestione temporanea** profilo di pubblicazione e quindi fare clic su **pubblica sul Web**.

    Visual Studio distribuisce l'applicazione aggiornata e viene aperto il browser alla home page del sito. Il *app\_offline.htm* file viene visualizzato. Poter eseguire il test per verificare la corretta distribuzione, è necessario rimuovere il *app\_offline.htm* file.
2. Tornare allo strumento FTP ed eliminare **app\_offline.htm** dal sito di staging.
3. Nel browser, aprire la pagina istruttori nel sito di staging e selezionare un docente per verificare che l'aggiornamento è stato distribuito correttamente.
4. Seguire la stessa procedura per la produzione utilizzata per la gestione temporanea.

<a id="specificfiles"></a>

## <a name="reviewing-changes-and-deploying-specific-files"></a>Revisione delle modifiche e la distribuzione di file specifici

Visual Studio 2012 offre inoltre la possibilità di distribuire singoli file. Per un file selezionato è possibile visualizzare le differenze tra la versione locale e la versione distribuita, distribuire il file nell'ambiente di destinazione o copiare il file dall'ambiente di destinazione del progetto locale. In questa sezione dell'esercitazione è illustrato come utilizzare queste funzionalità.

### <a name="make-a-change-to-deploy"></a>Apportare una modifica per la distribuzione

1. Aprire *Content/Site.css*e individuare il blocco per il `body` tag.
2. Modificare il valore di `background-color` da `#fff` a `darkblue`.

    [!code-css[Main](deploying-a-code-update/samples/sample4.css?highlight=2)]

### <a name="view-the-change-in-the-publish-preview-window"></a>Visualizzare la modifica nella finestra di anteprima pubblica

Quando si utilizza il **pubblica sul Web** procedura guidata per pubblicare il progetto, è possibile vedere alle modifiche che si desidera vengano pubblicati dal doppio clic sul file di **anteprima** finestra.

1. Fare clic sul progetto ContosoUniversity e fare clic su **pubblica**.
2. Dal **profilo** elenco a discesa, seleziona il **Test** profilo di pubblicazione.
3. Fare clic su **anteprima**, quindi fare clic su **avviare Anteprima**.
4. Nel **anteprima** riquadro, fare doppio clic su **Site.css**.

    ![Fare doppio clic su Site](deploying-a-code-update/_static/image9.png)

    Il **Anteprima modifiche** finestra di dialogo viene visualizzata un'anteprima delle modifiche che verranno distribuiti.

    ![Anteprima modifiche a Site](deploying-a-code-update/_static/image10.png)

    Se si fa doppio clic il *Web. config* file, il **Anteprima modifiche** finestra di dialogo Mostra l'effetto della compilazione trasformazioni di configurazione e le trasformazioni di profilo di pubblicazione. A questo punto non è stata eseguita alcuna operazione che fa sì che il *Web. config* file sul server per cambiare, pertanto è non prevista alcuna modifica. Tuttavia, il **Anteprima modifiche** finestra in modo non corretto mostra due modifiche. Sembra che due elementi XML verranno rimosso. Questi elementi vengono aggiunti dal processo di pubblicazione quando si seleziona **eseguire migrazioni Code First all'avvio dell'applicazione** per una classe del contesto Code First. Il confronto viene eseguito prima che il processo di pubblicazione aggiunge gli elementi, in modo che rispecchi come essi vengono rimossi anche se non verranno rimossi. Questo errore verrà corretto nelle versioni future.
5. Fare clic su **Chiudi**.
6. Fare clic su **Pubblica**.
7. Quando si apre il browser alla home page del sito di Test, premere CTRL + F5 per provocare un aggiornamento hardware per verificare l'effetto della modifica CSS.

    ![Effetto della modifica CSS](deploying-a-code-update/_static/image11.png)
8. Chiudere il browser.

### <a name="publish-specific-files-from-solution-explorer"></a>Pubblicare i file specifici da Esplora soluzioni

Si supponga non come sfondo blu e ripristinare il colore originale. In questa sezione potrai ripristinare le impostazioni originali tramite la pubblicazione di un file specifico direttamente da **Esplora**.

1. Aprire *Content/Site.css* e il ripristino di `background-color` impostando su `#fff`.
2. In **Esplora**, fare doppio clic su di *Content/Site.css* file.

    Menu di scelta rapida mostra che tre opzioni di pubblicazione.

    ![Pubblica opzioni da Esplora soluzioni](deploying-a-code-update/_static/image12.png)
3. Fare clic su **Anteprima modifiche a Site**.

    Verrà visualizzata una finestra per visualizzare le differenze tra il file locale e la versione nell'ambiente di destinazione.

    ![Diff-Content/Site.css](deploying-a-code-update/_static/image13.png)
4. In **Esplora**, fare doppio clic su **Site.css** nuovamente e fare clic su **pubblicare Site**.

    Il **attività di pubblicazione Web** finestra mostra che il file sia stato pubblicato.

    ![Finestra attività di pubblicazione Web](deploying-a-code-update/_static/image14.png)
5. Aprire una finestra del browser il `http://localhost/contosouniversity` URL e quindi premere CTRL + F5 per provocare un disco rigido aggiornare per visualizzare l'effetto del foglio di stile CSS di modifica.

    ![Home page con CSS normale](deploying-a-code-update/_static/image15.png)
6. Chiudere il browser.

## <a name="summary"></a>Riepilogo

Abbiamo visto diversi modi per distribuire un aggiornamento di un'applicazione che non comportano una modifica al database e si è visto come visualizzare in anteprima le modifiche per verificare che cosa verrà aggiornata è quello previsto. La pagina istruttori ora ha un **corsi invece** sezione.

![Pagina istruttori con corsi tenuti](deploying-a-code-update/_static/image16.png)

L'esercitazione successiva viene illustrato come distribuire una modifica al database: un campo Data di nascita verrà aggiunto al database e alla pagina i docenti.

> [!div class="step-by-step"]
> [Precedente](deploying-to-production.md)
> [Successivo](deploying-a-database-update.md)
