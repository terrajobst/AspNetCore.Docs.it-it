---
uid: web-pages/overview/ui-layouts-and-themes/9-working-with-images
title: Utilizzo di immagini in un sito ASP.NET Web Pages (Razor) | Documenti Microsoft
author: tfitzmac
description: In questo capitolo viene illustrato come aggiungere, visualizzare e modificare immagini (ridimensionare, capovolgere e aggiungere le filigrane) nel sito Web.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 778c4e58-4372-4d25-bab9-aec4a8d8e38d
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/9-working-with-images
msc.type: authoredcontent
ms.openlocfilehash: 74838dd364a43f3f4c966c1417d0f0b2cc242f28
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="working-with-images-in-an-aspnet-web-pages-razor-site"></a>Utilizzo di immagini in un sito Web di ASP.NET di pagine (Razor)
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> In questo articolo viene illustrato come aggiungere, visualizzare e modificare immagini (ridimensionare, capovolgere e aggiungere le filigrane) in un sito Web ASP.NET Web Pages (Razor).
> 
> Illustra quanto segue:
> 
> - Come aggiungere un'immagine a una pagina in modo dinamico.
> - Come consentire agli utenti di caricare un'immagine.
> - Come ridimensionare un'immagine.
> - Come capovolge o Ruota un'immagine.
> - Come aggiungere un'immagine di filigrana.
> - Descrive come usare un'immagine come filigrana.
> 
> Si tratta di programmazione delle funzionalità introdotte nell'articolo ASP.NET:
> 
> - Il `WebImage` helper.
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


<a id="Adding_an_Image"></a>
## <a name="adding-an-image-to-a-web-page-dynamically"></a>Aggiunta di un'immagine a una pagina Web in modo dinamico

È possibile aggiungere immagini al sito Web e alle singole pagine mentre si sviluppa il sito Web. È inoltre possibile consentire agli utenti il caricamento di immagini, che potrebbero essere utili per attività, ad esempio consentendo loro di aggiungere una foto del profilo.

Se un'immagine è già disponibile sul sito e si desidera visualizzarla in una pagina, utilizzare un elemento HTML `<img>` elemento simile al seguente:

[!code-html[Main](9-working-with-images/samples/sample1.html)]

In alcuni casi, tuttavia, è necessario essere in grado di visualizzare le immagini in modo dinamico & #8212; ovvero, non si conosce quale immagine da visualizzare fino a quando la pagina è in esecuzione.

La procedura descritta in questa sezione viene illustrato come visualizzare un'immagine al momento in cui gli utenti specificano il nome del file di immagine da un elenco di nomi di immagini. Essi selezionare il nome dell'immagine da un elenco a discesa e quando l'invio della pagina, viene visualizzata l'immagine che è selezionate.

![[immagine] ] (9-working-with-images/_static/image1.jpg "ch9images 1.jpg")

1. In WebMatrix, creare un nuovo sito Web.
2. Aggiungere una nuova pagina denominata *DynamicImage.cshtml*.
3. Nella cartella radice del sito Web, aggiungere una nuova cartella e denominarla *immagini*.
4. Aggiungere quattro immagini di *immagini* cartella appena creata. (Tutte le immagini si che sarà utile, ma rientrino in una pagina). Rinominare le immagini *Photo1.jpg*, *Photo2.jpg*, *Photo3.jpg*, e *Photo4.jpg*. (Non utilizzare *Photo4.jpg* in questa procedura, ma si sarà utilizzarlo successivamente nell'articolo.)
5. Verificare che le quattro immagini non siano contrassegnate come di sola lettura.
6. Sostituire il contenuto esistente nella pagina con le operazioni seguenti:

    [!code-cshtml[Main](9-working-with-images/samples/sample2.cshtml)]

    Il corpo della pagina contiene un elenco a discesa (un `<select>` elemento) che è denominata `photoChoice`. Nell'elenco è disponibili tre opzioni e `value` attributo di ogni opzione di elenco con il nome di una delle immagini che si inserisce nel *immagini* cartella. In pratica, l'elenco consente all'utente di selezionare un nome descrittivo, ad esempio &quot;foto 1&quot;, e quindi passa il *jpg* nome file durante l'invio della pagina.

    Nel codice, è possibile ottenere la selezione dell'utente (in altre parole, il nome del file immagine) dall'elenco leggendo `Request["photoChoice"]`. Viene visualizzata se si esegue di una selezione. Se è presente, costruire un percorso per l'immagine che include il nome della cartella per le immagini e nome del file di immagine dell'utente. (Se si è tentato di creare un percorso, ma non c'era in `Request["photoChoice"]`, verrà visualizzato un errore.) Ciò produce un percorso relativo simile al seguente:

    *immagini/Photo1.jpg*

    Il percorso è archiviato nella variabile denominata `imagePath` che dovranno essere successivamente nella pagina.

    Nel corpo, è inoltre disponibile un `<img>` elemento utilizzato per visualizzare l'immagine che l'utente selezionato. Il `src` attributo non è impostato su un nome file o URL, come si farebbe per visualizzare un elemento statico. In alternativa, è impostata su `@imagePath`, vale a dire che ottiene il relativo valore dal percorso è impostata nel codice.

    La prima volta che la pagina viene eseguita, tuttavia, non è presente alcuna immagine da visualizzare, perché l'utente non è stato selezionato alcun elemento. Ciò in genere significa che il `src` attributo potrebbe essere vuoto e l'immagine viene visualizzato come rosse &quot;x&quot; (o qualsiasi il browser esegue il rendering quando non è possibile trovare un'immagine). Per evitare questo problema, inserire il `<img>` elemento in un `if` blocco che verifica se il `imagePath` variabile contiene un valore. Se si effettua una selezione, `imagePath` contiene il percorso. Se l'utente non è stato scegliere un'immagine o se questa è la prima volta che viene visualizzata la pagina, il `<img>` non è anche il rendering dell'elemento.
7. Salvare il file ed eseguire la pagina in un browser. (Assicurarsi che la pagina è selezionata nel **file** dell'area di lavoro prima di eseguirlo.)
8. Selezionare un'immagine dall'elenco a discesa e quindi fare clic su **immagine di esempio**. Assicurarsi che visualizzare immagini differenti per le diverse opzioni relative.

<a id="Uploading_an_Image"></a>
## <a name="uploading-an-image"></a>Caricare un'immagine

Nell'esempio precedente è stato illustrato come visualizzare un'immagine in modo dinamico, ma funzionava solo con le immagini che sono stati già nel sito Web. Questa procedura viene illustrato come consentire agli utenti di caricare un'immagine, che viene quindi visualizzata nella pagina. In ASP.NET, è possibile modificare immagini in tempo reale mediante la `WebImage` helper, che dispone di metodi che consentono di creare, modificare e salvare le immagini. Il `WebImage` helper supporta tutti i web immagine tipi di file comuni, inclusi *jpg*, *PNG*, e *bmp*. In questo articolo, si userà *jpg* immagini, ma è possibile usare uno dei tipi di immagine.

![[immagine] ] (9-working-with-images/_static/image2.jpg "ch9images 2.jpg")

1. Aggiungere una nuova pagina e denominarlo *UploadImage.cshtml*.
2. Sostituire il contenuto esistente nella pagina con le operazioni seguenti: 

    [!code-cshtml[Main](9-working-with-images/samples/sample3.cshtml)]

    Il corpo del testo ha un `<input type="file">` elemento, che consente agli utenti di selezionare un file da caricare. Quando si fa clic su **Invia**, il file che viene inviato insieme il modulo.

    Per ottenere l'immagine caricata, utilizzare il `WebImage` helper, dotato di tutti i tipi di metodi utili per l'utilizzo di immagini. In particolare, si utilizzano `WebImage.GetImageFromRequest` per ottenere l'immagine caricata (se presente) e archiviarlo in una variabile denominata `photo`.

    Molte operazioni in questo esempio prevede di ottenere e impostare i nomi di file e percorso. Il problema è che si desidera ottenere il nome (solo il nome) dell'immagine che viene caricato e quindi creare un nuovo percorso in cui si desidera archiviare l'immagine. Poiché gli utenti è stato possibile caricare potenzialmente più immagini che hanno lo stesso nome, utilizzare un po' di codice aggiuntivo per creare nomi univoci e assicurarsi che gli utenti non sovrascrivere immagini esistenti.

    Se un'immagine effettivamente caricata (test `if (photo != null)`), ottenere il nome dell'immagine dall'immagine `FileName` proprietà. Quando l'utente carica l'immagine, `FileName` contiene il nome dell'utente originale, che include il percorso dal computer dell'utente. Potrebbe essere simile al seguente:

    *C:\Users\Joe\Pictures\SamplePhoto1.jpg*

    Non si desidera tutte queste informazioni di percorso, anche se & #8212; si desidera semplicemente il nome del file effettivo (*SamplePhoto1.jpg*). È possibile rimuovere solo il file da un percorso utilizzando il `Path.GetFileName` metodo, simile al seguente:

    [!code-csharp[Main](9-working-with-images/samples/sample4.cs)]

    È quindi possibile creare un nuovo nome file univoco aggiungendo un GUID per il nome originale. (Per ulteriori informazioni sui GUID, vedere [sul GUID](#SB_AboutGUIDs) più avanti in questo articolo.) È quindi necessario creare un percorso completo che consente di salvare l'immagine. Salva percorso è costituito il nuovo nome file, la cartella (immagini) e il percorso del sito Web corrente.

    > [!NOTE]
    > Affinché il codice salvare i file nel *immagini* cartella, l'applicazione deve autorizzazioni di lettura-scrittura per la cartella. Nel computer di sviluppo non si tratta in genere un problema. Tuttavia, quando si pubblica il sito al server di un provider di hosting web, potrebbe essere necessario impostare in modo esplicito tali autorizzazioni. Se si esegue questo codice nel server del provider di hosting e si verificano errori, verificare con il provider di hosting per scoprire come impostare le autorizzazioni.

    Infine, si passa il salvataggio percorso il `Save` metodo il `WebImage` helper. Questo modo viene archiviata l'immagine caricata con il nuovo nome. Salva metodo è simile al seguente: `photo.Save(@"~\" + imagePath)`. Il percorso completo viene aggiunto al `@"~\"`, che rappresenta la posizione del sito Web corrente. (Per informazioni di `~` (operatore), vedere [Introduzione alla programmazione Web ASP.NET utilizzando la sintassi Razor](https://go.microsoft.com/fwlink/?LinkId=202890#ID_WorkingWithFileAndFolderPaths).)

    Come nell'esempio precedente, il corpo della pagina contiene un `<img>` elemento per visualizzare l'immagine. Se `imagePath` è stata impostata, il `<img>` elemento sottoposto a rendering e il relativo `src` attributo è impostato sul `imagePath` valore.
3. Eseguire la pagina in un browser.
4. Caricare un'immagine e verificare che viene visualizzato nella pagina.
5. Nel sito, aprire il *immagini* cartella. Viene visualizzato un nuovo file è stato aggiunto il cui nome è simile alla seguente: 

    *45ea4527-7ddd-4965-b9ca-c6444982b342\_MyPhoto.png*

    Questo è l'immagine che caricato con un GUID come preceduto al nome. (Il proprio file avrà un GUID diverso e probabilmente ha un nome diverso da quello *MyPhoto.png*.)

> [!TIP] 
> 
> <a id="SB_AboutGUIDs"></a>
> ### <a name="about-guids"></a>Sui GUID
> 
> Un GUID (identificatore univoco globale) è un identificatore che in genere viene eseguito il rendering in un formato simile al seguente: `936DA01F-9ABD-4d9d-80C7-02AF85C822A8`. I numeri e lettere (da A-F) diversi per ogni GUID, ma seguono il modello di utilizzo dei gruppi di 8-4-4-4-12 caratteri. (Tecnicamente, un GUID è un numero di 16 byte/128 bit) Quando è necessario un GUID, è possibile chiamare codice specifico che genera un GUID. L'idea GUID è tra le dimensioni enormi del numero (3,4 x 10<sup>38</sup>) e l'algoritmo per la generazione, il numero risultante è praticamente garantito può essere uno dei tipi. Pertanto, i GUID sono un ottimo modo per generare nomi per operazioni quando è necessario garantire che non verranno usate due volte lo stesso nome. Lo svantaggio, naturalmente, è che i GUID non sono particolarmente descrittivo, tendono a essere utilizzato quando il nome viene utilizzato solo nel codice.


<a id="Resizing_an_Image"></a>
## <a name="resizing-an-image"></a>Ridimensionamento di un'immagine

Se il sito Web accetta le immagini da un utente, si desidera ridimensionare le immagini prima di visualizzare o salvarli. È possibile utilizzare nuovamente il `WebImage` helper per l'oggetto.

Questa procedura viene illustrato come ridimensionare un'immagine caricata per creare un'immagine di anteprima, quindi salvare l'immagine di anteprima e l'immagine originale nel sito Web. Visualizzare l'anteprima della pagina e utilizzare un collegamento ipertestuale per reindirizzare gli utenti per l'immagine a schermo intero.

![[immagine] ] (9-working-with-images/_static/image3.jpg "ch9images 3.jpg")

1. Aggiungere una nuova pagina denominata *Thumbnail.cshtml*.
2. Nel *immagini* cartella, creare una sottocartella denominata *miniature*.
3. Sostituire il contenuto esistente nella pagina con le operazioni seguenti: 

    [!code-cshtml[Main](9-working-with-images/samples/sample5.cshtml)]

    Questo codice è simile al codice dell'esempio precedente. La differenza è che questo codice salva l'immagine due volte, una volta normalmente e una volta dopo aver creato una copia dell'immagine di anteprima. Innanzitutto si recupererà l'immagine caricata e salvarlo nel *immagini* cartella. È quindi possibile creare un nuovo percorso dell'immagine di anteprima. Per creare effettivamente l'anteprima, si chiama il `WebImage` dell'helper `Resize` metodo per creare un'immagine di 60 pixel per 60-pixel. Nell'esempio viene illustrato come si mantiene le proporzioni e come è possibile impedire l'immagine viene ingrandito (nel caso in cui le nuove dimensioni effettivamente renderebbe l'immagine più grande). Immagine ridimensionata verrà salvato nel *miniature* sottocartella.

    Alla fine del codice, usare la stessa `<img>` elemento con dinamica `src` attributo che è possibile vedere negli esempi precedenti per visualizzare in modo condizionale l'immagine. In questo caso, visualizzare l'anteprima. È inoltre possibile utilizzare un `<a>` elemento per creare un collegamento ipertestuale per la versione grande dell'immagine. Come con la `src` attributo del `<img>` elemento, impostare il `href` attributo del `<a>` elemento dinamico per il `imagePath`. Per assicurarsi che il percorso può essere usato come un URL, si passa `imagePath` per il `Html.AttributeEncode` metodo, che converte i caratteri riservati nel percorso in caratteri ok in un URL.
4. Eseguire la pagina in un browser.
5. Carica una foto e verificare che sia visualizzata l'anteprima.
6. Fare clic su Anteprima per visualizzare l'immagine ingrandita.
7. Nel *immagini* e *immagini/miniature*, si noti che sono stati aggiunti nuovi file.

<a id="Rotating_and_Flipping"></a>
## <a name="rotating-and-flipping-an-image"></a>La rotazione e capovolgimento di un'immagine

Il `WebImage` helper consente inoltre di capovolgimento e rotazione di immagini. Questa procedura viene illustrato come ottenere un'immagine dal server, capovolge l'immagine capovolta (verticalmente), salvarlo e quindi visualizzare l'immagine capovolta della pagina. In questo esempio, si utilizza solo un file già presenti nel server (*Photo2.jpg*). In un'applicazione reale, si sarebbe probabilmente capovolge un'immagine il cui nome viene visualizzato in modo dinamico, come negli esempi precedenti.

![[immagine] ] (9-working-with-images/_static/image4.jpg "ch9images 4.jpg")

1. Aggiungere una nuova pagina denominata *FlipImage.cshtml*.
2. Sostituire il contenuto esistente nella pagina con le operazioni seguenti: 

    [!code-cshtml[Main](9-working-with-images/samples/sample6.cshtml)]

    Il codice Usa il `WebImage` helper per ottenere un'immagine dal server. Si crea il percorso all'immagine utilizzando la stessa tecnica usata negli esempi precedenti per il salvataggio delle immagini e si passa tale percorso quando si crea un'immagine utilizzando `WebImage`:

    [!code-javascript[Main](9-working-with-images/samples/sample7.js)]

    Se viene trovata un'immagine, costruire un nuovo percorso e il nome, come negli esempi precedenti. Per invertire l'immagine, si chiama il `FlipVertical` (metodo), quindi salvare l'immagine di nuovo.

    L'immagine viene visualizzato di nuovo nella pagina utilizzando la `<img>` elemento con la `src` attributo impostato su `imagePath`.
3. Eseguire la pagina in un browser. L'immagine per *Photo2.jpg* viene visualizzato dall'alto in basso.
4. Aggiornare la pagina o richiedere la pagina per visualizzare che l'immagine è capovolta destra nuovamente.

Per ruotare un'immagine, utilizzare lo stesso codice, tranne il fatto che invece di chiamare il `FlipVertical` o `FlipHorizontal`, si chiama `RotateLeft` o `RotateRight`.

<a id="Adding_a_Watermark"></a>
## <a name="adding-a-watermark-to-an-image"></a>Aggiunta di una filigrana a un'immagine

Quando si aggiungono immagini per il sito Web, è necessario aggiungere una filigrana all'immagine di prima di salvarlo o visualizzarli in una pagina. Gli utenti usano spesso le filigrane per aggiungere informazioni sul copyright a un'immagine o per annunciare il proprio nome aziendale.

![[immagine] ] (9-working-with-images/_static/image5.jpg "ch9images 5.jpg")

1. Aggiungere una nuova pagina denominata *Watermark.cshtml*.
2. Sostituire il contenuto esistente nella pagina con le operazioni seguenti: 

    [!code-cshtml[Main](9-working-with-images/samples/sample8.cshtml)]

    Questo codice è simile al codice nel *FlipImage.cshtml* pagina precedenti (anche se in questo caso viene utilizzato il *Photo3.jpg* file). Per aggiungere il limite, si chiama il `WebImage` dell'helper `AddTextWatermark` metodo prima di salvare l'immagine. Nella chiamata a `AddTextWatermark`, si passa il testo &quot;filigrana My&quot;, impostare il colore del carattere in giallo e impostare la famiglia di caratteri Arial. (Anche se non è visualizzato in questo caso, il `WebImage` helper consente inoltre di specificare l'opacità, famiglia di caratteri e dimensioni del carattere e la posizione del testo della filigrana.) Quando si salva l'immagine deve essere in sola lettura.

    Come si è visto prima, utilizzando la pagina viene visualizzata l'immagine di `<img>` elemento con l'attributo src impostato su `@imagePath`.
3. Eseguire la pagina in un browser. Si noti il testo "My filigrana" nell'angolo in basso a destra dell'immagine.

<a id="Using_an_Image_as_a_Watermark"></a>
## <a name="using-an-image-as-a-watermark"></a>Utilizzo di un'immagine come filigrana

Invece di usare testo per una filigrana, è possibile utilizzare un'altra immagine. Persone talvolta utilizzano immagini ad esempio un logo aziendale come filigrana oppure un'immagine di filigrana anziché di testo per le informazioni sul copyright.

![[immagine] ] (9-working-with-images/_static/image6.jpg "ch9images 6.jpg")

1. Aggiungere una nuova pagina denominata *ImageWatermark.cshtml*.
2. Aggiungere un'immagine di *immagini* cartella in cui è possibile utilizzare come un logo e rinominare l'immagine *MyCompanyLogo.jpg*. L'immagine deve essere un'immagine che è possibile visualizzare chiaramente quando è impostata su 80 pixel di larghezza e 20 pixel.
3. Sostituire il contenuto esistente nella pagina con le operazioni seguenti: 

    [!code-cshtml[Main](9-working-with-images/samples/sample9.cshtml)]

    Si tratta di un'altra variazione del codice degli esempi precedenti. In questo caso, si chiama `AddImageWatermark` per aggiungere l'immagine di filigrana all'immagine di destinazione (*Photo3.jpg*) prima di salvare l'immagine. Quando si chiama `AddImageWatermark`, la larghezza viene impostata su 80 pixel e l'altezza su 20 pixel. Il *MyCompanyLogo.jpg* immagine è allineata al centro orizzontalmente e verticalmente nella parte inferiore dell'immagine di destinazione. L'opacità è impostato su 100% e il riempimento è impostato su 10 pixel. Se l'immagine della filigrana è maggiore dell'immagine di destinazione, non accade nulla. Se l'immagine di filigrana è maggiore dell'immagine di destinazione e si imposta la spaziatura interna per la filigrana immagine a zero, il limite viene ignorato.

    Come prima, si visualizza l'immagine usando il `<img>` elemento e dinamico `src` attributo.
4. Eseguire la pagina in un browser. Si noti che l'immagine di filigrana viene visualizzata nella parte inferiore dell'immagine principale.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive


[Utilizzo dei file in un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202896)

[Introduzione a ASP.NET Web Pages con sintassi Razor di programmazione](https://go.microsoft.com/fwlink/?LinkID=251587)
