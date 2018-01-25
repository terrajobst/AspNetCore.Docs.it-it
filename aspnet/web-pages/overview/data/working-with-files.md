---
uid: web-pages/overview/data/working-with-files
title: Utilizzo dei file in un sito ASP.NET Web Pages (Razor) | Documenti Microsoft
author: tfitzmac
description: In questo capitolo viene illustrato come leggere, scrivere, aggiungere, eliminare e caricare i file.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: eee916e4-ba4c-439a-a24e-68df7d45a569
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/data/working-with-files
msc.type: authoredcontent
ms.openlocfilehash: 0f119f8fb4873e55292203f21a2efd8f26793ae4
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="working-with-files-in-an-aspnet-web-pages-razor-site"></a>Utilizzo dei file in un sito Web di ASP.NET di pagine (Razor)
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> In questo articolo viene illustrato come leggere, scrivere, aggiungere, eliminare e caricare i file in un sito di pagine Web ASP.NET (Razor).
> 
> > [!NOTE]
> > Se si desidera caricare immagini e modificarli (ad esempio, capovolge o ridimensionarle), vedere [utilizzo delle immagini in un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202897).
> 
> 
> **Illustra quanto segue:** 
> 
> - Come creare un file di testo e scrivere dati.
> - Come aggiungere dati a un file esistente.
> - Come leggere un file e visualizzare da esso.
> - Come eliminare file da un sito Web.
> - Come consentire agli utenti di caricare uno o più file.
> 
> Si tratta di programmazione delle funzionalità introdotte nell'articolo ASP.NET:
> 
> - Il `File` oggetto che fornisce un modo per gestire i file.
> - Il `FileUpload` helper.
> - Il `Path` oggetto che fornisce metodi che consentono di modificare i nomi file e percorso.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - Pagine Web ASP.NET (Razor) 2
> - WebMatrix 2
>   
> 
> In questa esercitazione si integra inoltre con 3 di WebMatrix.


<a id="Creating_a_Text_File"></a>
## <a name="creating-a-text-file-and-writing-data-to-it"></a>Creazione di un File di testo e scrivere dati

Oltre a utilizzare un database del sito Web, potrebbe funzionare con i file. Ad esempio, è possibile utilizzare i file di testo come un modo semplice per archiviare i dati per il sito. (Un file di testo che viene utilizzato per archiviare i dati in alcuni casi viene chiamato un *file flat*.) File di testo possono essere in formati diversi, ad esempio *. txt*, *XML*, o *CSV* (valori delimitati da virgole).

Se si desidera archiviare i dati in un file di testo, è possibile utilizzare il `File.WriteAllText` metodo per specificare il file per creare e i dati per la scrittura. In questa procedura, si creerà una pagina che contiene un modulo semplice con tre `input` elementi (nome, cognome e indirizzo di posta elettronica) e un **Invia** pulsante. Quando l'utente invia il form, che conterrà l'input dell'utente in un file di testo.

1. Creare una nuova cartella denominata *App\_dati*, se non esiste già.
2. Alla radice del sito Web, creare un nuovo file denominato *UserData.cshtml*.
3. Sostituire il contenuto esistente con il seguente: 

    [!code-cshtml[Main](working-with-files/samples/sample1.cshtml)]

    Il markup HTML crea il form con tre caselle di testo. Nel codice, utilizzare il `IsPost` proprietà per determinare se è stata inviata la pagina prima di iniziare l'elaborazione.

    La prima attività è per ottenere l'input dell'utente e assegnarla a variabili. Il codice quindi concatena i valori delle variabili separate in un'unica stringa delimitata da virgole, che viene quindi archiviata in una variabile diversa. Si noti che il separatore virgola è una stringa racchiusi tra virgolette (","). intendi incorporare una virgola letterale nella stringa di grandi dimensioni che si sta creando. Alla fine dei dati che concatenano, aggiungere `Environment.NewLine`. Aggiunge un'interruzione di riga (carattere di nuova riga). Cosa si sta creando tutti questa concatenazione è una stringa simile al seguente:

    [!code-css[Main](working-with-files/samples/sample2.css)]

    (Con un'invisibile interruzione di riga alla fine.)

    Creare quindi una variabile (`dataFile`) che contiene il percorso e nome del file in cui archiviare i dati. Impostazione della posizione richiede alcune una gestione speciale. In siti Web, è una buona prassi per fare riferimento nel codice in percorsi assoluti come *C:\Folder\File.txt* per i file nel server web. Se viene spostato un sito Web, un percorso assoluto sarà corretto. Inoltre, per un sito di hosting (in contrapposizione nel proprio computer) in genere anche conosce il percorso corretto quando si scrive il codice.

    Ma in alcuni casi (ad esempio, ora, per la scrittura di un file) è necessario un percorso completo. La soluzione consiste nell'utilizzare il `MapPath` metodo il `Server` oggetto. Restituisce il percorso completo per il sito Web. Per ottenere il percorso per la radice del sito Web, si utilizza il `~` (operatore) (per represen del virtuale nel sito radice) per `MapPath`. (È anche possibile passare un nome della sottocartella, ad esempio *~/App\_dati /*, per ottenere il percorso per tale sottocartella.) È quindi possibile concatenare le informazioni aggiuntive su qualsiasi sia il metodo restituisce per creare un percorso completo. In questo esempio, si aggiunge un nome di file. (Ulteriori informazioni su come lavorare con i percorsi di file e cartelle in [Introduzione a ASP.NET Web Pages di programmazione utilizzando la sintassi Razor](https://go.microsoft.com/fwlink/?LinkId=195205#ID_WorkingWithFileAndFolderPaths).)

    Il file viene salvato nel *App\_dati* cartella. Questa cartella è una cartella speciale di ASP.NET che viene utilizzata per archiviare i file di dati, come descritto [Introduzione all'uso di un Database nei siti di ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=195209).

    Il `WriteAllText` metodo il `File` oggetto scrive i dati del file. Questo metodo accetta due parametri: il nome (con il percorso) del file da scrivere e i dati effettivi da scrivere. Si noti che il nome del primo parametro presenta un `@` carattere come prefisso. Questo indica ad ASP.NET che si forniscono un valore letterale stringa lettera e caratteri, ad esempio "/" che non deve essere interpretato in modo speciale. (Per ulteriori informazioni, vedere [Introduzione alla programmazione Web ASP.NET utilizzando la sintassi Razor](https://go.microsoft.com/fwlink/?LinkId=195205#ID_WorkingWithFileAndFolderPaths).)

    > [!NOTE]
    > Affinché il codice salvare i file nel *App\_dati* cartella, l'applicazione deve autorizzazioni di lettura-scrittura per la cartella. Nel computer di sviluppo non si tratta in genere un problema. Tuttavia, quando si pubblica il sito al server di un provider di hosting web, potrebbe essere necessario impostare in modo esplicito tali autorizzazioni. Se si esegue questo codice nel server del provider di hosting e si verificano errori, verificare con il provider di hosting per scoprire come impostare le autorizzazioni.

- Eseguire la pagina in un browser. 

    ![](working-with-files/_static/image1.jpg)
- Immettere i valori nei campi e quindi fare clic su **Invia**.
- Chiudere il browser.
- Tornare al progetto e aggiornare la visualizzazione.
- Aprire il *txt* file. I dati che è stato inviato il modulo sono nel file. 

    ![[immagine]](working-with-files/_static/image2.jpg)
- Chiudi il *txt* file.

<a id="Appending_Data"></a>
## <a name="appending-data-to-an-existing-file"></a>Aggiunta di dati a un File esistente

Nell'esempio precedente è stata usata `WriteAllText` per creare un file di testo che dispone di un solo pezzo di dati in essa contenuti. Se si chiama di nuovo il metodo e passarla lo stesso nome di file, il file esistente viene completamente sovrascritto. Tuttavia, dopo aver creato un file è spesso necessario aggiungere nuovi dati alla fine del file. È possibile eseguire tale utilizzando il `AppendAllText` metodo il `File` oggetto.

1. Nel sito Web, creare una copia del *UserData.cshtml* file e denominare la copia *UserDataMultiple.cshtml*.
2. Sostituire il blocco di codice prima dell'apertura `<!DOCTYPE html>` tag con il blocco di codice seguente: 

    [!code-cshtml[Main](working-with-files/samples/sample3.cshtml)]

    Questo codice contiene una modifica dell'esempio precedente. Anziché utilizzare `WriteAllText`, Usa `the AppendAllText` metodo. I metodi sono simili, ad eccezione del fatto che `AppendAllText` aggiunge i dati alla fine del file. Come con `WriteAllText`, `AppendAllText` crea il file se non esiste già.
3. Eseguire la pagina in un browser.
4. Immettere i valori per i campi e quindi fare clic su **Invia**.
5. Aggiungere più dati e inviare nuovamente il form.
6. Tornare al progetto, fare clic sulla cartella del progetto e quindi fare clic su **aggiornamento**.
7. Aprire il *txt* file. Include ora i nuovi dati appena immesso. 

    ![[immagine]](working-with-files/_static/image3.jpg)

<a id="Reading_and_Displaying_Data"></a>
## <a name="reading-and-displaying-data-from-a-file"></a>Lettura e la visualizzazione dei dati da un File

Anche se non è necessario scrivere dati in un file di testo, probabilmente a volte è necessario leggere i dati da uno. A tale scopo, è possibile utilizzare nuovamente il `File` oggetto. È possibile utilizzare il `File` oggetto per la lettura di ogni riga singolarmente (separate da interruzioni di riga) o la lettura di un elemento singolo indipendentemente dalla modalità di questi separato.

Questa procedura viene illustrato come leggere e visualizzare i dati che è stato creato nell'esempio precedente.

1. Alla radice del sito Web, creare un nuovo file denominato *DisplayData.cshtml*.
2. Sostituire il contenuto esistente con il seguente: 

    [!code-cshtml[Main](working-with-files/samples/sample4.cshtml)]

    Il codice avvia leggendo il file creato nell'esempio precedente in una variabile denominata `userData`, tramite questa chiamata al metodo:

    [!code-css[Main](working-with-files/samples/sample5.css)]

    Il codice per eseguire questa operazione è all'interno di un `if` istruzione. Quando si desidera leggere un file, è consigliabile utilizzare il `File.Exists` metodo per determinare innanzitutto se il file è disponibile. Il codice verifica inoltre se il file è vuoto.

    Il corpo della pagina contiene due `foreach` cicli, uno annidato all'interno di altro. Esterna `foreach` ciclo Ottiene una riga alla volta dal file di dati. In questo caso, le righe sono definite da interruzioni di riga nel file di &#8212; ogni elemento di dati è sulla riga corrispondente. Il ciclo esterno crea un nuovo elemento (`<li>` elemento) all'interno di un elenco ordinato (`<ol>` elemento).

    Il ciclo interno divide ogni riga di dati in elementi (campi) con una virgola come delimitatore. (In base all'esempio precedente, ciò significa che ogni riga contiene tre campi &#8212; il nome, cognome e indirizzo di posta elettronica, separati da virgola). Il ciclo interno crea anche un `<ul>` elementi di elenco e visualizza un elenco per ogni campo nella riga di dati.

    Il codice viene illustrato come utilizzare due tipi di dati, una matrice e `char` tipo di dati. La matrice è necessaria perché il `File.ReadAllLines` metodo restituisce i dati sotto forma di matrice. Il `char` tipo di dati è necessario perché il `Split` metodo restituisce un `array` in cui ogni elemento è del tipo `char`. (Per informazioni sulle matrici, vedere [Introduzione alla programmazione Web ASP.NET utilizzando la sintassi Razor](https://go.microsoft.com/fwlink/?LinkId=202890#ID_CollectionsAndObjects).)
3. Eseguire la pagina in un browser. I dati che immessi per gli esempi precedenti viene visualizzati. 

    ![[immagine]](working-with-files/_static/image4.jpg)

> [!TIP] 
> 
> **Visualizzazione dei dati da un File delimitato da virgole di Microsoft Excel**
> 
> È possibile utilizzare Microsoft Excel per salvare i dati contenuti in un foglio di calcolo come file delimitato da virgole (*CSV* file). Quando si esegue l'operazione, il file viene salvato in formato testo normale, non è in formato Excel. Ogni riga del foglio di calcolo è separato da un'interruzione di riga nel file di testo e ogni elemento di dati è separato da una virgola. Per leggere un file delimitato da virgole di Excel solo modificando il nome del file di dati nel codice, è possibile utilizzare il codice illustrato nell'esempio precedente.


<a id="Deleting_Files"></a>
## <a name="deleting-files"></a>L'eliminazione di file

Per eliminare file dal sito Web, è possibile utilizzare il `File.Delete` metodo. Questa procedura viene illustrato come consentire agli utenti di eliminare un'immagine (*jpg* file) da un *immagini* cartella se si conosce il nome del file.

> [!NOTE] 
> 
> **Importante** In un sito Web di produzione, è in genere limitare chi è consentito per apportare modifiche ai dati. Per informazioni su come configurare l'appartenenza e sui modi per consentire agli utenti di eseguire attività nel sito, vedere [sicurezza aggiunta e l'appartenenza a un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202904).


1. Nel sito Web, creare una sottocartella denominata *immagini*.
2. Copiare una o più *jpg* di file nel *immagini* cartella.
3. Nella radice del sito Web, creare un nuovo file denominato *FileDelete.cshtml*.
4. Sostituire il contenuto esistente con il seguente: 

    [!code-cshtml[Main](working-with-files/samples/sample6.cshtml)]

    Questa pagina contiene un modulo in cui gli utenti possono immettere il nome di un file di immagine. Non si immette il *jpg* estensione di file; limitando il nome del file simile al seguente, si guida impedisce agli utenti di eliminazione file arbitrari nel sito.

    Il codice legge il nome del file che l'utente ha immesso e genera quindi un percorso completo. Per creare il percorso, il codice Usa il percorso del sito Web corrente (come restituito dal `Server.MapPath` metodo), il *immagini* ". jpg" come valore letterale stringa, il nome fornito dall'utente e nome della cartella.

    Per eliminare il file, il codice chiama il `File.Delete` metodo, passando il percorso completo appena costruita. Alla fine del markup, codice visualizza un messaggio di conferma che il file è stato eliminato.
5. Eseguire la pagina in un browser. 

    ![[immagine]](working-with-files/_static/image5.jpg)
6. Immettere il nome del file da eliminare e quindi fare clic su **Invia**. Se il file è stato eliminato, il nome del file viene visualizzato nella parte inferiore della pagina.

<a id="Letting_Users_Upload_a_File"></a>
## <a name="letting-users-upload-a-file"></a>Caricamento di utenti consentendo agli sviluppatori di un File

Il `FileUpload` supporto consente agli utenti di caricare i file nel sito Web. La procedura seguente viene illustrato come consentire agli utenti di caricare un singolo file.

1. Aggiungere il sito Web ASP.NET Web Helpers Library come descritto in [helper per l'installazione in un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se non sono state aggiunte in precedenza.
2. Nel *App\_dati* cartella, creare un nuovo una cartella e denominarla *UploadedFiles*.
3. Nella radice, creare un nuovo file denominato *FileUpload.cshtml*.
4. Sostituire il contenuto esistente nella pagina con le operazioni seguenti: 

    [!code-cshtml[Main](working-with-files/samples/sample7.cshtml)]

    Usa la parte del corpo della pagina di `FileUpload` helper per creare la casella di caricamento e pulsanti che si sono probabilmente familiare con:

    ![[immagine]](working-with-files/_static/image6.jpg)

    Le proprietà impostate per il `FileUpload` helper specificare che una singola casella per il file da caricare e che il pulsante submit per leggere **caricare**. (Si aggiungeranno caselle più avanti in questo articolo.)

    Quando l'utente fa clic **caricare**, ottiene il file e lo salva il codice nella parte superiore della pagina. Il `Request` oggetto normalmente utilizzato per ottenere i valori dai campi modulo dispone anche di un `Files` matrice che contiene i file di () che sono stati caricati. È possibile ottenere i singoli file tra posizioni specifiche nella matrice &#8212; ad esempio, per ottenere il primo file caricato, ottenere `Request.Files[0]`, per ottenere il secondo file, si ottiene `Request.Files[1]`e così via. (Tenere presente che nella programmazione, il conteggio in genere inizia in corrispondenza di zero).

    Quando si recupera un file caricato, viene inserita in una variabile (in questo caso, `uploadedFile`) in modo da poterli modificare. Per determinare il nome del file caricato, viene visualizzato solo il relativo `FileName` proprietà. Tuttavia, quando l'utente carica un file, `FileName` contiene il nome dell'utente originale, che include l'intero percorso. Potrebbe essere simile al seguente:

    *C:\Users\Public\Sample.txt*

    Non si desidera tutte queste informazioni di percorso, tuttavia, poiché è il percorso nel computer dell'utente, non per il server. Si desidera semplicemente il nome del file effettivo (*Sample.txt*). È possibile rimuovere solo il file da un percorso utilizzando il `Path.GetFileName` metodo, simile al seguente:

    [!code-csharp[Main](working-with-files/samples/sample8.cs)]

    Il `Path` oggetto è un'utilità che ha un numero di metodi, ad esempio ciò che è possibile utilizzare per rimuovere i percorsi, combinare i tracciati e così via.

    Una volta il nome del file caricato, è possibile creare un nuovo percorso in cui si desidera archiviare il file caricato nel sito Web. In questo caso, si combinano `Server.MapPath`, i nomi delle cartelle (*App\_dati/UploadedFiles*) e il nome del file appena rimossi per creare un nuovo percorso. È quindi possibile chiamare il file caricato `SaveAs` metodo effettivamente salvare il file.
5. Eseguire la pagina in un browser. 

    ![[immagine]](working-with-files/_static/image7.jpg)
6. Fare clic su **Sfoglia** e quindi selezionare un file da caricare. 

    ![[immagine]](working-with-files/_static/image8.jpg)

    Casella di testo accanto al **Sfoglia** pulsante conterrà il percorso e il percorso file.

    ![[immagine]](working-with-files/_static/image9.jpg)
7. Fare clic su **Upload**.
8. Nel sito Web, fare clic sulla cartella del progetto e quindi fare clic su **aggiornamento**.
9. Aprire il *UploadedFiles* cartella. Il file caricato è nella cartella. 

    ![[immagine]](working-with-files/_static/image10.jpg)

<a id="Letting_Users_Upload_Multiple_Files"></a>
## <a name="letting-users-upload-multiple-files"></a>Consentendo agli utenti di caricare più file

Nell'esempio precedente, consentire agli utenti di caricare un file. Ma è possibile utilizzare il `FileUpload` helper per caricare più di un file alla volta. Ciò è utile per scenari come caricare foto, in cui è difficile caricare un file alla volta. (Per ulteriori informazioni sul caricamento delle foto in [utilizzo delle immagini in un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202897).) In questo esempio viene illustrato come consentire agli utenti di caricare due alla volta, sebbene sia possibile utilizzare la stessa tecnica per caricare qualcosa di più.

1. Aggiungere il sito Web ASP.NET Web Helpers Library come descritto in [helper per l'installazione in un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se hai già fatto.
2. Creare una nuova pagina denominata *FileUploadMultiple.cshtml*.
3. Sostituire il contenuto esistente nella pagina con le operazioni seguenti:  

    [!code-cshtml[Main](working-with-files/samples/sample9.cshtml)]

    In questo esempio, il `FileUpload` helper nel corpo della pagina è configurato per consentire agli utenti di caricare due file per impostazione predefinita. Poiché `allowMoreFilesToBeAdded` è impostato su `true`, l'helper esegue il rendering di un collegamento che consente di aggiungere altre caselle di caricamento utente:

    ![[immagine]](working-with-files/_static/image11.jpg)

    Per elaborare i file che l'utente carica, il codice utilizza la stessa tecnica di base utilizzati nell'esempio precedente &#8212; ottenere un file da `Request.Files` e quindi salvarlo. (Compresi i vari elementi necessario ottenere il nome file corretti e il percorso.) Questa volta l'innovazione è che l'utente sta per caricare più file e non si conosce molti. Per individuare, è possibile ottenere `Request.Files.Count`.

    Con questo numero di disponibilità, è possibile scorrere in ciclo `Request.Files`, recuperare, a sua volta di ogni file e salvarlo. Quando si desidera eseguire un ciclo noto il numero di volte tramite una raccolta, è possibile utilizzare un `for` ciclo, simile al seguente:

    [!code-csharp[Main](working-with-files/samples/sample10.cs)]

    La variabile `i` è semplicemente un contatore temporaneo che verrà inviata da zero a qualsiasi limite superiore è impostato. In questo caso, il limite massimo è il numero di file. Tuttavia, poiché il contatore inizia da zero, come avviene per il conteggio di scenari in ASP.NET, il limite superiore è uno, minore il numero di file. (Se vengono caricati i tre file, Microsoft il conteggio è zero per 2.)

    Il `uploadedCount` variabile totali di tutti i file che vengono caricati e salvati. Questo codice account la possibilità che potrebbe non essere in grado di caricare un file prevista.
4. Eseguire la pagina in un browser. Il browser visualizza la pagina e le finestre di caricamento di due.
5. Selezionare due file da caricare.
6. Fare clic su **aggiungere un altro file**. La pagina Visualizza una nuova finestra di caricamento. 

    ![[immagine]](working-with-files/_static/image12.jpg)
7. Fare clic su **Upload**.
8. Nel sito Web, fare clic sulla cartella del progetto e quindi fare clic su **aggiornamento**.
9. Aprire il *UploadedFiles* cartella per visualizzare i file caricati correttamente.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive


[Utilizzo di immagini in un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202897)

[Esportazione in un File CSV](https://msdn.microsoft.com/library/ms155919.aspx)
