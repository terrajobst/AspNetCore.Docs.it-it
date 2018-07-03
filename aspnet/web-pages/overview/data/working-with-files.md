---
uid: web-pages/overview/data/working-with-files
title: Uso di file in un sito ASP.NET Web Pages (Razor) | Microsoft Docs
author: tfitzmac
description: In questo capitolo viene illustrato come leggere, scrivere, aggiungere, eliminare e caricare i file.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: eee916e4-ba4c-439a-a24e-68df7d45a569
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/data/working-with-files
msc.type: authoredcontent
ms.openlocfilehash: bd2b432eaa8bff31a8c4432d249308049a4c6aec
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37364672"
---
<a name="working-with-files-in-an-aspnet-web-pages-razor-site"></a>Uso di file in un sito di ASP.NET Web Pages (Razor)
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Questo articolo illustra come lettura, scrittura, aggiungere, eliminare e caricare file in un sito di ASP.NET Web Pages (Razor).
> 
> > [!NOTE]
> > Se si desidera caricare immagini e modificarli (ad esempio, capovolgere o li ridimensiono), vedere [utilizzo di immagini in un sito di ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202897).
> 
> 
> **Che cosa si apprenderà come:** 
> 
> - Come creare un file di testo e scrivere i dati a esso.
> - Come aggiungere dati a un file esistente.
> - Come leggere un file e visualizzare da quest'ultimo.
> - Come eliminare i file da un sito Web.
> - Come consentire agli utenti di caricare uno o più file.
> 
> Queste sono le funzionalità introdotte nell'articolo di programmazione ASP.NET:
> 
> - Il `File` oggetto, che fornisce un modo per gestire i file.
> - Il `FileUpload` helper.
> - Il `Path` oggetto, che fornisce i metodi che consentono di modificare i nomi di file e percorso.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 2
>   
> 
> Questa esercitazione si integra inoltre con WebMatrix 3.


<a id="Creating_a_Text_File"></a>
## <a name="creating-a-text-file-and-writing-data-to-it"></a>Creazione di un File di testo e la scrittura dei dati

Oltre a usare un database del sito Web, potrebbe funzionare con i file. Ad esempio, è possibile utilizzare i file di testo come un modo semplice per archiviare i dati per il sito. (Un file di testo che viene usato per archiviare i dati viene talvolta chiamato un *file flat*.) File di testo possono essere in formati diversi, ad esempio *. txt*, *XML*, o *CSV* (valori delimitati da virgole).

Se si desidera archiviare i dati in un file di testo, è possibile usare il `File.WriteAllText` metodo per specificare il file per creare e i dati per la scrittura. In questa procedura si creerà una pagina che contiene un modulo semplice con tre `input` elementi (nome, cognome e indirizzo di posta elettronica) e una **Submit** pulsante. Quando l'utente invia il form, verrà archiviato l'input dell'utente in un file di testo.

1. Creare una nuova cartella denominata *App\_dati*, se non esiste già.
2. Nella radice del sito Web, creare un nuovo file denominato *UserData.cshtml*.
3. Sostituire il contenuto esistente con il codice seguente: 

    [!code-cshtml[Main](working-with-files/samples/sample1.cshtml)]

    Il markup HTML crea form con tre caselle di testo. Nel codice, si utilizza il `IsPost` proprietà per determinare se è stata inviata la pagina prima di iniziare l'elaborazione.

    La prima attività è per ottenere l'input dell'utente e assegnarlo alle variabili. Il codice quindi concatena i valori delle variabili separate in un'unica stringa delimitata da virgole, che viene quindi archiviata in una variabile diversa. Si noti che il virgola come separatore è una stringa racchiusa tra virgolette (","), perché si incorpora una virgola in modo letterale nella stringa di big data che si sta creando. Alla fine dei dati che concatenano, aggiungere `Environment.NewLine`. Verrà aggiunta un'interruzione di riga (un carattere di nuova riga). Che cosa si sta creando con tutto questo concatenazione è una stringa che si presenta come segue:

    [!code-css[Main](working-with-files/samples/sample2.css)]

    (Con un'invisibile interruzione di riga alla fine.)

    Creare quindi una variabile (`dataFile`) che contiene il percorso e nome del file in cui archiviare i dati. Il percorso dell'impostazione richiede una gestione speciale. In siti Web, è una prassi non corretta per fare riferimento nel codice percorsi assoluti, ad esempio *C:\Folder\File.txt* per i file nel server web. Se viene spostato un sito Web, un percorso assoluto, sarà errato. Inoltre, per un sito di hosting (in contrapposizione nel proprio computer) è in genere non anche conoscere qual è il percorso corretto quando si scrive il codice.

    Ma a volte (ad esempio, ora, per la scrittura di un file) è necessario un percorso completo. La soluzione consiste nell'usare la `MapPath` metodo di `Server` oggetto. Viene restituito il percorso completo al sito Web. Per ottenere il percorso per la radice del sito Web, si utilizza il `~` operatore (per represen del virtuale nel sito radice) per `MapPath`. (È anche possibile passare un nome della sottocartella, come *~/App\_dati /*, per ottenere il percorso per tale sottocartella.) È quindi possibile concatenare le informazioni aggiuntive su qualunque sia il metodo restituisce per creare un percorso completo. In questo esempio, si aggiunge un nome di file. (Altre informazioni su come usare i percorsi di file e cartelle nella [Introduzione a ASP.NET Web Pages di programmazione utilizzando la sintassi Razor](https://go.microsoft.com/fwlink/?LinkId=195205#ID_WorkingWithFileAndFolderPaths).)

    Il file viene salvato nel *App\_dati* cartella. Questa cartella è una cartella speciale in ASP.NET che consente di archiviare i file di dati, come descritto in [Introduzione all'uso di un Database nei siti di ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=195209).

    Il `WriteAllText` metodo di `File` oggetto scrive i dati nel file. Questo metodo accetta due parametri: il nome (con percorso) del file in cui scrivere e i dati effettivi da scrivere. Si noti che il nome del primo parametro presenta un `@` carattere come prefisso. Ciò indica ad ASP.NET che si forniscono un valore letterale stringa verbatim e caratteri, ad esempio "/" non deve essere interpretato in modo speciale. (Per altre informazioni, vedere [Introduzione alla programmazione Web ASP.NET usando la sintassi Razor](https://go.microsoft.com/fwlink/?LinkId=195205#ID_WorkingWithFileAndFolderPaths).)

    > [!NOTE]
    > Affinché il codice salvare i file nei *App\_dati* cartella, l'applicazione deve lettura / scrittura delle autorizzazioni per tale cartella. Nel computer di sviluppo non si tratta in genere un problema. Tuttavia, quando si pubblica il sito al server web del provider di hosting, potrebbe essere necessario impostare in modo esplicito tali autorizzazioni. Se si esegue questo codice nel server del provider di hosting e ottenere gli errori, controllare con il provider di hosting per scoprire come impostare queste autorizzazioni.

- Eseguire la pagina in un browser. 

    ![](working-with-files/_static/image1.jpg)
- Immettere i valori nei campi e quindi fare clic su **Submit**.
- Chiudere il browser.
- Tornare al progetto e aggiornare la visualizzazione.
- Aprire il *txt* file. I dati inviati nel formato sono nel file. 

    ![[immagine]](working-with-files/_static/image2.jpg)
- Chiudi il *txt* file.

<a id="Appending_Data"></a>
## <a name="appending-data-to-an-existing-file"></a>Accodamento dei dati in un File esistente

Nell'esempio precedente, è utilizzato `WriteAllText` per creare un file di testo che disponga di un solo pezzo di dati in esso. Se si chiama il metodo di nuovo e passarla lo stesso nome di file, il file esistente viene completamente sovrascritto. Tuttavia, dopo aver creato un file spesso desidera aggiungere nuovi dati alla fine del file. È possibile farlo tramite il `AppendAllText` metodo di `File` oggetto.

1. Nel sito Web, creare una copia del *UserData.cshtml* del file e denominare la copia *UserDataMultiple.cshtml*.
2. Sostituire il blocco di codice prima dell'apertura `<!DOCTYPE html>` tag con il blocco di codice seguente: 

    [!code-cshtml[Main](working-with-files/samples/sample3.cshtml)]

    Questo codice include una modifica dell'esempio precedente. Invece di usare `WriteAllText`, Usa `the AppendAllText` (metodo). I metodi sono simili, ad eccezione del fatto che `AppendAllText` aggiunge i dati alla fine del file. Come per gli `WriteAllText`, `AppendAllText` crea il file se non esiste già.
3. Eseguire la pagina in un browser.
4. Immettere i valori per i campi e quindi fare clic su **Submit**.
5. Aggiungere altri dati e inviare nuovamente il modulo.
6. Tornare al progetto, fare clic sulla cartella di progetto e quindi fare clic su **Aggiorna**.
7. Aprire il *txt* file. A questo punto contiene i nuovi dati appena immesso. 

    ![[immagine]](working-with-files/_static/image3.jpg)

<a id="Reading_and_Displaying_Data"></a>
## <a name="reading-and-displaying-data-from-a-file"></a>La lettura e visualizzazione dei dati da un File

Anche se non è necessario scrivere dati in un file di testo, probabilmente a volte è necessario leggere i dati da uno. A tale scopo, è possibile usare nuovamente il `File` oggetto. È possibile usare il `File` oggetto da leggere singolarmente ogni riga, separati da interruzioni di riga, o la lettura di singoli elementi, indipendentemente dal modo in cui sono suddivisi.

Questa procedura viene illustrato come leggere e visualizzare i dati che è stato creato nell'esempio precedente.

1. Nella radice del sito Web, creare un nuovo file denominato *DisplayData.cshtml*.
2. Sostituire il contenuto esistente con il codice seguente: 

    [!code-cshtml[Main](working-with-files/samples/sample4.cshtml)]

    Il codice inizia con la lettura del file creato nell'esempio precedente in una variabile denominata `userData`, tramite questa chiamata al metodo:

    [!code-css[Main](working-with-files/samples/sample5.css)]

    Il codice per eseguire questa operazione è all'interno di un `if` istruzione. Quando si vuole leggere un file, è consigliabile usare il `File.Exists` metodo per determinare innanzitutto se il file è disponibile. Il codice verifica anche se il file è vuoto.

    Il corpo della pagina contiene due `foreach` cicli, uno annidato all'interno di altro. L'outer `foreach` ciclo Ottiene una riga alla volta dal file di dati. In questo caso, le righe vengono definite da interruzioni di riga nel file &#8212; , ogni elemento di dati è nella riga a sé stante. Il ciclo esterno viene creato un nuovo elemento (`<li>` elemento) all'interno di un elenco ordinato (`<ol>` elemento).

    Il ciclo interno divide ogni riga di dati in elementi (campi) usando una virgola come delimitatore. (Basato sull'esempio precedente, questo significa che ogni riga contiene tre campi &#8212; il nome, cognome e indirizzo di posta elettronica, separati da una virgola.) Il ciclo interno crea anche un `<ul>` degli elementi elenco e consente di visualizzare un elenco per ogni campo nella riga di dati.

    Il codice viene illustrato come utilizzare due tipi di dati, una matrice e `char` tipo di dati. La matrice è necessaria perché il `File.ReadAllLines` metodo restituisce i dati sotto forma di matrice. Il `char` tipo di dati è necessario perché la `Split` metodo restituisce un' `array` in cui ogni elemento è del tipo `char`. (Per altre informazioni sulle matrici, vedere [Introduzione alla programmazione Web ASP.NET usando la sintassi Razor](https://go.microsoft.com/fwlink/?LinkId=202890#ID_CollectionsAndObjects).)
3. Eseguire la pagina in un browser. I dati inseriti per gli esempi precedenti viene visualizzati. 

    ![[immagine]](working-with-files/_static/image4.jpg)

> [!TIP] 
> 
> **Visualizzazione dei dati da un File delimitato da virgole di Microsoft Excel**
> 
> È possibile usare Microsoft Excel per salvare i dati contenuti in un foglio di calcolo come file delimitato da virgole (*CSV* file). Quando esegue questa operazione, il file viene salvato in formato testo normale, non in formato Excel. Ogni riga del foglio di calcolo è separato da un'interruzione di riga nel file di testo e ogni elemento di dati è separato da virgole. È possibile usare il codice illustrato nell'esempio precedente per leggere un file delimitato da virgole di Excel semplicemente modificando il nome del file di dati nel codice.


<a id="Deleting_Files"></a>
## <a name="deleting-files"></a>L'eliminazione di file

Per eliminare file dal sito Web, è possibile usare il `File.Delete` (metodo). Questa procedura illustra come consentire agli utenti di eliminare un'immagine (*jpg* file) da un *immagini* cartella se si conosce il nome del file.

> [!NOTE] 
> 
> **Importante** In siti Web di produzione, è in genere limitare chi è autorizzato per apportare modifiche ai dati. Per informazioni su come configurare l'appartenenza e sui modi per consentire agli utenti di eseguire attività nel sito, vedere [aggiunta di sicurezza e l'appartenenza a un sito con pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202904).


1. Nel sito Web, creare una sottocartella denominata *immagini*.
2. Copia una o più *jpg* di file nei *immagini* cartella.
3. Nella radice del sito Web, creare un nuovo file denominato *FileDelete.cshtml*.
4. Sostituire il contenuto esistente con il codice seguente: 

    [!code-cshtml[Main](working-with-files/samples/sample6.cshtml)]

    Questa pagina contiene un modulo in cui gli utenti possono immettere il nome di un file di immagine. Non immettono le *jpg* estensione del nome del file; limitando il nome del file simile al seguente, si guida impedisce agli utenti di eliminare file arbitrari nel sito.

    Il codice legge il nome del file che l'utente ha immesso e genera quindi un percorso completo. Per creare il percorso, il codice Usa il percorso del sito Web corrente (come restituito dal `Server.MapPath` metodo), il *immagini* nome della cartella e il nome fornito dall'utente ". jpg" come valore letterale stringa.

    Per eliminare il file, il codice chiama il `File.Delete` metodo, passando il percorso completo appena costruita. Alla fine del markup, codice visualizza un messaggio di conferma che il file è stato eliminato.
5. Eseguire la pagina in un browser. 

    ![[immagine]](working-with-files/_static/image5.jpg)
6. Immettere il nome del file da eliminare e quindi fare clic su **Submit**. Se il file è stato eliminato, il nome del file viene visualizzato nella parte inferiore della pagina.

<a id="Letting_Users_Upload_a_File"></a>
## <a name="letting-users-upload-a-file"></a>Consentire agli utenti di caricare un File

Il `FileUpload` helper consente agli utenti di caricare i file nel sito Web. La procedura seguente illustra come consentire agli utenti di caricare un singolo file.

1. Aggiungere ASP.NET Web Helpers Library nel sito Web come descritto in [installazione di helper in un sito con pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se non sono state aggiunte in precedenza.
2. Nel *App\_Data* cartella, creare una cartella e denominarlo *UploadedFiles*.
3. Nella radice, creare un nuovo file denominato *FileUpload.cshtml*.
4. Sostituire il contenuto esistente nella pagina con il codice seguente: 

    [!code-cshtml[Main](working-with-files/samples/sample7.cshtml)]

    La parte di corpo della pagina utilizza il `FileUpload` helper per creare la finestra di caricamento e pulsanti che avrà probabilmente dimestichezza con:

    ![[immagine]](working-with-files/_static/image6.jpg)

    Le proprietà impostate per il `FileUpload` helper specificare che si desidera che una singola casella per il file da caricare e di cui si desidera pulsante Invia per leggere **caricare**. (Si aggiungeranno altre caselle più avanti nell'articolo.)

    Quando l'utente sceglie **caricare**, il codice nella parte superiore della pagina Ottiene il file e salvarlo. Il `Request` oggetto che si usa normalmente per ottenere valori dai campi modulo ha anche un `Files` matrice che contiene i file (o file) che sono stati caricati. È possibile ottenere i singoli file da posizioni specifiche nella matrice &#8212; , ad esempio, per ottenere il primo file caricato, otterrai `Request.Files[0]`, per ottenere il secondo file, si ottiene `Request.Files[1]`e così via. (Tenere presente che nella programmazione, in genere il conteggio inizia da zero).

    Quando si recupera un file caricato, viene inserita in una variabile (in questo caso, `uploadedFile`) in modo da poterli modificare. Per determinare il nome del file caricato, è sufficiente ottenere relativo `FileName` proprietà. Tuttavia, quando l'utente ha caricato un file, `FileName` contiene il nome dell'utente originale, che include l'intero percorso. Potrebbe essere simile al seguente:

    *C:\Users\Public\Sample.txt*

    Non si desidera tutte queste informazioni di percorso, tuttavia, poiché questo è il percorso nel computer dell'utente, non per il server. È sufficiente il nome del file effettivo (*Sample. txt*). È possibile eliminare le solo il file da un percorso tramite il `Path.GetFileName` metodo, simile al seguente:

    [!code-csharp[Main](working-with-files/samples/sample8.cs)]

    Il `Path` oggetto è un'utilità che ha un numero di metodi simile al seguente che è possibile utilizzare per rimuovere i percorsi, combinare tracciati e così via.

    Una volta il nome del file caricato, è possibile creare un nuovo percorso in cui si desidera archiviare il file caricato nel sito Web. In questo caso, si combinano `Server.MapPath`, i nomi delle cartelle (*App\_dati/UploadedFiles*) e il nome del file appena rimosso per creare un nuovo percorso. È quindi possibile chiamare il file caricato `SaveAs` metodo effettivamente salvare il file.
5. Eseguire la pagina in un browser. 

    ![[immagine]](working-with-files/_static/image7.jpg)
6. Fare clic su **esplorare** e quindi selezionare un file da caricare. 

    ![[immagine]](working-with-files/_static/image8.jpg)

    Casella di testo accanto al **esplorare** pulsante conterrà il percorso e il percorso file.

    ![[immagine]](working-with-files/_static/image9.jpg)
7. Fare clic su **Upload**.
8. Nel sito Web, fare doppio clic la cartella del progetto e quindi fare clic su **Aggiorna**.
9. Aprire il *UploadedFiles* cartella. Il file caricato è nella cartella. 

    ![[immagine]](working-with-files/_static/image10.jpg)

<a id="Letting_Users_Upload_Multiple_Files"></a>
## <a name="letting-users-upload-multiple-files"></a>Consentire agli utenti di caricare più file

Nell'esempio precedente, consentire agli utenti di caricare un file. Ma è possibile usare il `FileUpload` helper per caricare più di un file alla volta. Ciò è utile per scenari come il caricamento di foto, in cui caricare i uno file alla volta è noioso. (Per ulteriori informazioni sul caricamento di foto nel [utilizzo di immagini in un sito di ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202897).) Questo esempio illustra come consentire agli utenti di caricare due alla volta, sebbene sia possibile usare la stessa tecnica per caricare qualcosa di più.

1. Aggiungere ASP.NET Web Helpers Library nel sito Web come descritto in [installazione di helper in un sito con pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se già stato fatto.
2. Creare una nuova pagina denominata *FileUploadMultiple.cshtml*.
3. Sostituire il contenuto esistente nella pagina con il codice seguente:  

    [!code-cshtml[Main](working-with-files/samples/sample9.cshtml)]

    In questo esempio, il `FileUpload` helper nel corpo della pagina è configurato per consentire agli utenti di caricare due file per impostazione predefinita. In quanto `allowMoreFilesToBeAdded` è impostata su `true`, l'helper esegue il rendering di un collegamento che consente di aggiungere altre caselle di caricamento utente:

    ![[immagine]](working-with-files/_static/image11.jpg)

    Per elaborare i file che l'utente ha caricato, il codice Usa la stessa tecnica di base utilizzato nell'esempio precedente &#8212; ottenere un file dal `Request.Files` e quindi salvare il file. (Tra cui le varie operazioni da eseguire per ottenere il nome del file a destra e il percorso.) Le innovazioni introdotte in questo momento sono che l'utente potrebbe essere caricamento di più file e non si conosce molte. Per altre, è possibile ottenere `Request.Files.Count`.

    Con questo numero a disposizione, è possibile scorrere in ciclo `Request.Files`, recuperare ogni file, a sua volta e salvarlo. Quando si desidera eseguire un ciclo noto il numero di volte in cui tramite una raccolta, è possibile usare un `for` ciclo, simile al seguente:

    [!code-csharp[Main](working-with-files/samples/sample10.cs)]

    La variabile `i` è semplicemente un contatore temporaneo che passa da zero per qualsiasi limite massimo è impostato. In questo caso, il limite superiore è il numero di file. Ma poiché il contatore inizia da zero, come avviene per il conteggio di scenari in ASP.NET, il limite superiore è effettivamente uno inferiore al numero di file. (Se vengono caricati i tre file, il conteggio è zero e 2).

    Il `uploadedCount` variabile totali di tutti i file che vengono caricati e salvati correttamente. Questo codice gli account per la possibilità che un file previsto potrebbe non essere in grado di essere caricato.
4. Eseguire la pagina in un browser. Il browser visualizza la pagina e le finestre di caricamento di due.
5. Selezionare due file da caricare.
6. Fare clic su **aggiungere un altro file**. La pagina Visualizza una nuova finestra di caricamento. 

    ![[immagine]](working-with-files/_static/image12.jpg)
7. Fare clic su **Upload**.
8. Nel sito Web, fare doppio clic la cartella del progetto e quindi fare clic su **Aggiorna**.
9. Aprire il *UploadedFiles* cartella per visualizzare i file sono stati caricati.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive


[Utilizzo di immagini in un sito con pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202897)

[Esportazione in un File CSV](https://msdn.microsoft.com/library/ms155919.aspx)
