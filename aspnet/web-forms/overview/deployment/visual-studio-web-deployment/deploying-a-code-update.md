---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
title: 'Distribuzione Web ASP.NET tramite Visual Studio: distribuzione di un aggiornamento del codice | Microsoft Docs'
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire, pubblicare, ASP.NET per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web da utilizza...
ms.author: aspnetcontent
ms.date: 02/15/2013
ms.assetid: c76dbc35-a914-4ee3-919c-4f4d1fa05104
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
msc.type: authoredcontent
ms.openlocfilehash: c058d4314ea0bed87e22c0e6b3fa09b89b2423d9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833355"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-code-update"></a>Distribuzione Web ASP.NET tramite Visual Studio: distribuzione di un aggiornamento del codice
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Download progetto iniziale](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Questa serie di esercitazioni illustra come distribuire, pubblicare, ASP.NET per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web usando Visual Studio 2012 o Visual Studio 2010. Per informazioni sulla serie, vedere [la prima esercitazione della serie](introduction.md).


## <a name="overview"></a>Panoramica

Dopo la distribuzione iniziale, continua il lavoro della manutenzione e il sito web di sviluppo e prima di prolungata è opportuno distribuire un aggiornamento. Questa esercitazione illustra il processo di distribuzione di aggiornamenti al codice dell'applicazione. L'aggiornamento che implementi e Distribuisci in questa esercitazione non comporta una modifica del database; scoprirai che cos'è diversi sulla distribuzione di una modifica al database nella prossima esercitazione.

Promemoria: Se viene visualizzato un messaggio di errore o qualcosa non funziona durante l'esecuzione dell'esercitazione, assicurarsi di controllare la [risoluzione dei problemi pagina](troubleshooting.md).

## <a name="make-a-code-change"></a>Modificare il codice

Un esempio semplice di un aggiornamento all'applicazione, si aggiungeranno al **instructors (insegnanti)** pagina un elenco dei corsi tenuti dall'insegnante selezionato.

Se si esegue la **instructors (insegnanti)** pagina, si noterà che esistono **selezionare** collegamenti nella griglia, ma non esegue alcuna operazione oltre marca il grigio turni di riga in background.

![Pagina instructors (insegnanti) con la selezione](deploying-a-code-update/_static/image1.png)

A questo punto si aggiungerà codice che viene eseguito quando il **seleziona** collegamento fa e visualizza un elenco dei corsi tenuti dall'insegnante selezionato.

1. Nelle *Instructors.aspx*, aggiungere il markup seguente immediatamente dopo il **ErrorMessageLabel** `Label` controllo:

    [!code-aspx[Main](deploying-a-code-update/samples/sample1.aspx)]
2. Eseguire la pagina e selezionare un insegnante. Viene visualizzato un elenco dei corsi tenuti da tale insegnante.

    ![Pagina instructors (insegnanti) con i corsi tenuti](deploying-a-code-update/_static/image2.png)
3. Chiudere il browser.

## <a name="deploy-the-code-update-to-the-test-environment"></a>Distribuire l'aggiornamento del codice nell'ambiente di test

Prima di utilizzare i profili di pubblicazione per la distribuzione di test, staging e produzione, è necessario modificare le opzioni di pubblicazione del database. È non è più necessario eseguire gli script di distribuzione grant e i dati per il database di appartenenza.

1. Aprire il **pubblica sul Web** procedura guidata facendo clic sul progetto ContosoUniversity e facendo clic su **Publish**.
2. Fare clic sui **Test** profilare nel **profilo** elenco a discesa.
3. Scegliere il **impostazioni** scheda.
4. Sotto **DefaultConnection** nel **database** sezione, deselezionare il **Aggiorna database** casella di controllo.
5. Fare clic sul **profilo** scheda e quindi fare clic sul **Staging** profilare nel **profilo** elenco a discesa.
6. Quando viene chiesto se si desidera salvare le modifiche apportate per il **Test** del profilo, fare clic su **Sì**.
7. Apportare la stessa modifica nel profilo di gestione temporanea.
8. Ripetere il processo per apportare la stessa modifica nel profilo di produzione.
9. Chiudi il **pubblica sul Web** procedura guidata.

La distribuzione nell'ambiente di test è ora una semplice questione di esecuzione con un clic pubblicare di nuovo. Per rendere più veloce il processo, è possibile usare la **Web-pubblicazione con un clic** sulla barra degli strumenti.

1. Nel **View** menu, scegliere **barre degli strumenti** e quindi selezionare **Web-pubblicazione con un clic**.

    ![Selecting_One_Click_Publish_toolbar](deploying-a-code-update/_static/image3.png)
2. Nelle **Esplora soluzioni**, selezionare il progetto ContosoUniversity.
3. il **Web-pubblicazione con un clic** sulla barra degli strumenti, scegliere il **Test** profilo di pubblicazione e quindi fare clic su **pubblica sul Web** (l'icona con frecce che puntano a sinistra e destra).

    ![Web_One_Click_Publish_toolbar](deploying-a-code-update/_static/image4.png)
4. Visual Studio distribuisce l'applicazione aggiornata e il browser viene aperta automaticamente alla home page.
5. Eseguire la pagina instructors (insegnanti) e selezionare un insegnante per verificare che l'aggiornamento è stato distribuito correttamente.

In genere viene usato anche eseguire test di regressione (vale a dire, il resto del sito per assicurarsi che la nuova modifica non interrompe con le funzionalità esistenti di test). Ma per questa esercitazione è possibile ignorare il passaggio e andare al distribuire l'aggiornamento a gestione temporanea e produzione.

Quando si ridistribuisce, distribuzione Web determina automaticamente quali file sono stati modificati e copia solo i file modificati nel server. Per impostazione predefinita, distribuzione Web viene utilizzato Data ultima modifica sui file per determinare quelle che sono stati modificati. Alcuni sistemi di controllo del codice sorgente cambiano file date anche quando non modifichi il contenuto del file. In tal caso, è possibile configurare distribuzione Web per usare checksum dei file per determinare quali file sono stati modificati. Per altre informazioni, vedere [motivo per cui tutti i file di ridistribuzione anche se non hai modificarli?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) nelle domande frequenti sulla distribuzione di ASP.NET.

## <a name="take-the-application-offline-during-deployment"></a>Portare offline l'applicazione durante la distribuzione

La modifica che si esegue la distribuzione è ora una semplice modifica a una singola pagina. Ma in alcuni casi si distribuiscono modifiche più estese, o si distribuiscono le modifiche di codice e database e il sito potrebbe comportarsi in modo non corretto se un utente richiede una pagina prima del completamento della distribuzione. Per impedire agli utenti di accedere al sito durante la distribuzione è in corso, è possibile usare un *app\_offline.htm* file. Quando si inserisce un file denominato *app\_offline.htm* nella cartella radice dell'applicazione, IIS consente di visualizzare automaticamente tale file invece di eseguire l'applicazione. Per impedire l'accesso durante la distribuzione, per l'inserimento *app\_offline.htm* nella cartella radice, eseguire il processo di distribuzione e quindi rimuovere *app\_offline.htm* dopo l'esito positivo distribuzione.

È possibile configurare distribuzione Web per inserire automaticamente un valore predefinito *app\_offline.htm* all'avvio la distribuzione di file nel server e rimuoverlo al termine. A scopo che tutto è necessario effettuare è aggiungere l'elemento XML seguente al file (con estensione pubxml) del profilo di pubblicazione:

[!code-xml[Main](deploying-a-code-update/samples/sample2.xml)]

Per questa esercitazione verrà illustrato come creare e usare una classe personalizzata *app\_offline.htm* file.

Usando *app\_offline.htm* nel sito di staging non è necessaria, perché non si dispone di utenti accede al sito di gestione temporanea. Ma è buona norma utilizzare gestione temporanea per testare tutti gli elementi il modo in cui che si intende distribuire nell'ambiente di produzione.

### <a name="create-appofflinehtm"></a>Creare app\_offline.htm

1. Nelle **Esplora soluzioni**, fare doppio clic la soluzione e fare clic su **Add**, quindi fare clic su **nuovo elemento**.
2. Creare un **pagina HTML** denominato *app\_offline.htm* (eliminare finale "l" nel *HTML* estensione creato da Visual Studio per impostazione predefinita).
3. Sostituire il markup del modello con il markup seguente:

    [!code-html[Main](deploying-a-code-update/samples/sample3.html)]
4. Salvare e chiudere il file.

### <a name="copy-appofflinehtm-to-the-root-folder-of-the-web-site"></a>Copia app\_offline.htm nella cartella radice del sito web

È possibile usare qualsiasi strumento FTP per copiare i file al sito web. [FileZilla](http://filezilla-project.org/) è uno strumento diffuso FTP e viene illustrato nelle schermate.

Per usare uno strumento FTP, sono necessarie tre cose: l'URL di FTP, il nome utente e la password.

L'URL è visualizzato nella pagina dashboard del sito web nel portale di gestione di Azure e il nome utente e password per FTP sono reperibili nel *publishsettings* file scaricato in precedenza. La procedura seguente illustra come ottenere questi valori.

1. Nel portale di gestione di Azure fare clic su **siti Web** scheda e quindi fare clic sul sito web di staging.
2. Nel **Dashboard** pagina, scorrere fino a trovare assegnare un nome host FTP nella **Quick Glance** sezione.

    ![Nome host FTP](deploying-a-code-update/_static/image5.png)
3. Aprire la gestione temporanea *publishsettings* file in blocco note o un altro editor di testo.
4. Trovare il `publishProfile` (elemento) per il profilo FTP.
5. Copia il `userName` e `userPWD` valori.

    ![Credenziali FTP](deploying-a-code-update/_static/image6.png)
6. Aprire lo strumento FTP e l'URL di FTP vi accede.
7. Copia *app\_offline.htm* dalla cartella della soluzione per il */site/wwwroot* cartella nel sito di staging.

    ![Copiare app_offline](deploying-a-code-update/_static/image7.png)
8. Passare all'URL del sito di gestione temporanea. Si può osservare che il *app\_offline.htm* viene visualizzata la pagina anziché la home page.

    ![App_offline. htm nella finestra del browser](deploying-a-code-update/_static/image8.png)

A questo punto si è pronti distribuire in gestione temporanea.

## <a name="deploy-the-code-update-to-staging-and-production"></a>Distribuire l'aggiornamento del codice in gestione temporanea e produzione

1. Nel **Web-pubblicazione con un clic** sulla barra degli strumenti, scegliere il **Staging** profilo di pubblicazione e quindi fare clic su **pubblica sul Web**.

    Visual Studio distribuisce l'applicazione aggiornata e verrà aperto il browser alla pagina iniziale del sito. Il *app\_offline.htm* file viene visualizzato. Prima di testare per verificare la corretta distribuzione, è necessario rimuovere il *app\_offline.htm* file.
2. Tornare allo strumento FTP ed eliminare **app\_offline.htm** dal sito di staging.
3. Nel browser, aprire la pagina instructors (insegnanti) nel sito di staging e selezionare un insegnante per verificare che l'aggiornamento è stato distribuito correttamente.
4. Come è stato fatto per la gestione temporanea, seguire la stessa procedura per la produzione.

<a id="specificfiles"></a>

## <a name="reviewing-changes-and-deploying-specific-files"></a>Verifica delle modifiche e la distribuzione di file specifici

Visual Studio 2012 offre inoltre la possibilità di distribuire i singoli file. Per un file selezionato è possibile visualizzare le differenze tra la versione locale e la versione distribuita, distribuire il file nell'ambiente di destinazione o copiare il file dall'ambiente di destinazione per il progetto locale. In questa sezione dell'esercitazione informazioni su come usare queste funzionalità.

### <a name="make-a-change-to-deploy"></a>Apportare una modifica per la distribuzione

1. Aprire *Content/Site.css*e trovare il blocco per il `body` tag.
2. Modificare il valore per `background-color` dal `#fff` a `darkblue`.

    [!code-css[Main](deploying-a-code-update/samples/sample4.css?highlight=2)]

### <a name="view-the-change-in-the-publish-preview-window"></a>Visualizzare la modifica nella finestra di anteprima pubblica

Quando si usa la **pubblica sul Web** procedura guidata per pubblicare il progetto, è possibile visualizzare alle modifiche che si desidera vengano pubblicati dal doppio clic sul file le **anteprima** finestra.

1. Fare clic sul progetto ContosoUniversity e fare clic su **pubblica**.
2. Dal **profilo** elenco a discesa, seleziona la **Test** profilo di pubblicazione.
3. Fare clic su **Preview**, quindi fare clic su **Avvia anteprima**.
4. Nel **Preview** riquadro, fare doppio clic su **CSS**.

    ![Fare doppio clic su CSS](deploying-a-code-update/_static/image9.png)

    Il **Anteprima modifiche** finestra di dialogo viene visualizzata un'anteprima delle modifiche che verranno distribuiti.

    ![Anteprima modifiche a CSS](deploying-a-code-update/_static/image10.png)

    Se si fa doppio clic il *Web. config* file, il **Anteprima modifiche** finestra di dialogo Mostra l'effetto della build le trasformazioni di configurazione e le trasformazioni di profilo di pubblicazione. A questo punto non è ancora alcuna operazione che comporterebbe la *Web. config* file sul server per modificare, in modo che si desidera non vedere alcuna modifica. Tuttavia, il **Anteprima modifiche** finestra Mostra in modo non corretto due modifiche. Sembra che due elementi XML verranno rimosso. Questi elementi vengono aggiunti dal processo di pubblicazione quando si seleziona **Esegui migrazioni Code First all'avvio dell'applicazione** per una classe del contesto Code First. Il confronto viene eseguito prima che il processo di pubblicazione aggiunge tali elementi, in modo che sembra proprio quello che vengono rimossi anche se essi non verranno rimossi. Questo errore verrà corretto in una versione futura.
5. Fare clic su **Chiudi**.
6. Fare clic su **Pubblica**.
7. Quando si apre il browser alla home page del sito di Test, premere CTRL+F5 per provocare un aggiornamento hardware per visualizzare l'effetto della modifica CSS.

    ![Effetto della modifica CSS](deploying-a-code-update/_static/image11.png)
8. Chiudere il browser.

### <a name="publish-specific-files-from-solution-explorer"></a>Pubblicare i file specifici da Esplora soluzioni

Si supponga che non desidera che lo sfondo blu e ripristinare il colore originale. In questa sezione è possibile ripristinare le impostazioni originali mediante la pubblicazione di un file specifico direttamente dal **Esplora soluzioni**.

1. Aprire *Content/Site.css* e il ripristino il `background-color` impostando su `#fff`.
2. Nelle **Esplora soluzioni**, fare doppio clic il *Content/Site.css* file.

    Menu di scelta rapida mostra che tre opzioni di pubblicazione.

    ![Pubblica opzioni da Esplora soluzioni](deploying-a-code-update/_static/image12.png)
3. Fare clic su **l'anteprima delle modifiche a CSS**.

    Verrà visualizzata una finestra per visualizzare le differenze tra il file locale e la versione nell'ambiente di destinazione.

    ![Diff-contenuto/Site. CSS](deploying-a-code-update/_static/image13.png)
4. Nelle **Esplora soluzioni**, fare doppio clic su **Site. CSS** nuovamente e fare clic su **pubblicare CSS**.

    Il **attività pubblicazione sul Web** finestra mostra che il file sia stato pubblicato.

    ![Finestra attività pubblicazione sul Web](deploying-a-code-update/_static/image14.png)
5. Aprire un browser per il `http://localhost/contosouniversity` URL e quindi premere CTRL+F5 per provocare un disco rigido aggiornare per visualizzare l'effetto del foglio di stile CSS di modifica.

    ![Home page con CSS normale](deploying-a-code-update/_static/image15.png)
6. Chiudere il browser.

## <a name="summary"></a>Riepilogo

Abbiamo visto diversi modi per distribuire un aggiornamento dell'applicazione che non comporta una modifica al database e si è appreso come visualizzare in anteprima le modifiche per verificare che cosa verrà aggiornata è quello previsto. La pagina instructors (insegnanti) ora ha un **corsi tenuti** sezione.

![Pagina instructors (insegnanti) con i corsi tenuti](deploying-a-code-update/_static/image16.png)

L'esercitazione successiva illustra come distribuire una modifica al database: si aggiungerà un campo Data di nascita per il database e per la pagina instructors (insegnanti).

> [!div class="step-by-step"]
> [Precedente](deploying-to-production.md)
> [Successivo](deploying-a-database-update.md)
