---
uid: web-pages/overview/ui-layouts-and-themes/3-creating-a-consistent-look
title: Creazione di un Layout coerente in ASP.NET Web Pages siti (Razor) | Documenti Microsoft
author: tfitzmac
description: Per rendere più efficiente per creare pagine web per il sito, è possibile creare blocchi riutilizzabili del contenuto (ad esempio le intestazioni e piè di pagina) per il sito Web e si c...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/10/2014
ms.topic: article
ms.assetid: d7bd001b-6db2-4422-9b78-f3d08b743b00
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/3-creating-a-consistent-look
msc.type: authoredcontent
ms.openlocfilehash: 2c7631017f7c0fb31f43320c2ab78baddd87b516
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
ms.locfileid: "26530240"
---
<a name="creating-a-consistent-layout-in-aspnet-web-pages-razor-sites"></a>Creazione di un Layout coerenza in siti Web di ASP.NET di pagine (Razor)
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> In questo articolo viene illustrato come è possibile utilizzare pagine di layout in un sito Web ASP.NET Web Pages (Razor) per creare blocchi riutilizzabili del contenuto (ad esempio le intestazioni e piè di pagina) e per creare un aspetto coerente per tutte le pagine nel sito.
> 
> **Illustra quanto segue:** 
> 
> - Come creare blocchi riutilizzabili di contenuto, ad esempio le intestazioni e piè di pagina.
> - Come creare un aspetto coerente per tutte le pagine del sito utilizzando un layout.
> - Come passare i dati in fase di esecuzione a una pagina di layout.
> 
> Queste sono le funzionalità ASP.NET introdotte nell'articolo:
> 
> - Blocchi di contenuto, ovvero i file che includono contenuto in formato HTML da inserire in più pagine.
> - Pagine di layout, ovvero le pagine che contengono contenuto in formato HTML che può essere condivisa dalle pagine del sito Web.
> - Il `RenderPage`, `RenderBody`, e `RenderSection` metodi, che indicano dove inserire gli elementi di pagina ASP.NET.
> - Il `PageData` dizionario che consente di condividere dati tra i blocchi di contenuto e pagine di layout.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - Pagine Web ASP.NET (Razor) 3
>   
> 
> In questa esercitazione si integra inoltre con ASP.NET Web Pages 2.


## <a name="about-layout-pages"></a>Informazioni sulle pagine di Layout

Molti siti Web con contenuto che viene visualizzato in ogni pagina, come un'intestazione e piè di pagina o una casella che indica gli utenti che si è connessi. ASP.NET consente di creare un file separato con un blocco di contenuto che può contenere testo, markup e codice, come una pagina web regolari. È quindi possibile inserire il blocco di contenuto in altre pagine nel sito in cui si desiderano visualizzare le informazioni. In questo modo non è necessario copiare e incollare il contenuto stesso in ogni pagina. Creazione di contenuto comune simile al seguente rende anche più facile aggiornare il sito. Se è necessario modificare il contenuto, è possibile aggiornare solo un singolo file e le modifiche vengono quindi riportate ovunque è stato inserito il contenuto.

Il diagramma seguente mostra come contenuto Blocca lavoro. Quando un browser richiede una pagina dal server web, ASP.NET consente di inserire i blocchi di contenuto nel punto in cui il `RenderPage` metodo viene chiamato nella pagina principale. La pagina di completamento (merge) viene quindi inviata al browser.

![Diagramma concettuale che illustra come il metodo RenderPage inserisce una pagina di cui si fa riferimento alla pagina corrente.](3-creating-a-consistent-look/_static/image1.jpg)

In questa procedura, si creerà una pagina che fa riferimento a due blocchi di contenuto (un'intestazione e piè di pagina) che si trovano in file distinti. È possibile utilizzare questi stessi blocchi di contenuto in qualsiasi pagina del sito. Al termine, si otterrà una pagina simile al seguente:

![Screenshot della pagina nel browser risultante dall'esecuzione di una pagina che include le chiamate al metodo RenderPage.](3-creating-a-consistent-look/_static/image2.jpg)

1. Nella cartella radice del sito Web, creare un file denominato *cshtml*.
2. Sostituire il codice esistente con il seguente:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample1.html)]
3. Nella cartella radice, creare una cartella denominata *Shared*.

    > [!NOTE]
    > È pratica comune per archiviare i file che vengono condivisi tra pagine web in una cartella denominata *Shared*.
4. Nel *Shared* cartella, creare un file denominato  *\_Header.cshtml*.
5. Sostituire qualsiasi contenuto esistente con il seguente:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample2.html)]

    Si noti che il nome del file è  *\_Header.cshtml*, con un carattere di sottolineatura (\_) come prefisso. ASP.NET non inviare una pagina nel browser se il nome inizia con un carattere di sottolineatura. Onde evitare che gli utenti richiedano (inavvertitamente o in caso contrario) queste pagine direttamente. È consigliabile utilizzare un carattere di sottolineatura alle pagine di nome con blocchi di contenuto, in quanto non si vuole che gli utenti siano in grado di richiedere queste pagine &#8212; Esistono esclusivamente per essere inserita nelle altre pagine.
6. Nel *Shared* cartella, creare un file denominato  *\_Footer.cshtml* e sostituire il contenuto con quanto segue:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample3.html)]
7. Nel *cshtml* pagina, aggiungere due chiamate al `RenderPage` (metodo), come illustrato di seguito:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample4.html)]

    Viene illustrato come inserire un blocco di contenuto in una pagina web. Chiamare il `RenderPage` metodo e passa il nome del file di cui si desidera inserire in quel punto il contenuto. In questo caso, si sta inserendo il contenuto del  *\_Header.cshtml* e  *\_Footer.cshtml* di file nel *cshtml* file.
8. Eseguire il *cshtml* pagina in un browser. (In WebMatrix, nel **file** area di lavoro, il pulsante destro e quindi selezionare **Avvia nel browser**.)
9. Nel browser, visualizzare l'origine della pagina. (Ad esempio, in Internet Explorer, fare clic sulla pagina e quindi fare clic su **Visualizza origine**.)

    Ciò consente di verificare il markup della pagina web che viene inviato al browser, che combina il markup della pagina di indice e i blocchi di contenuto. L'esempio seguente mostra l'origine della pagina che viene eseguito il rendering per *cshtml*. Le chiamate a `RenderPage` inserita in *cshtml* sono stati sostituiti con il contenuto effettivo dei file di intestazione e piè di pagina.

    [!code-html[Main](3-creating-a-consistent-look/samples/sample5.html)]

## <a name="creating-a-consistent-look-using-layout-pages"></a>Creazione di un aspetto coerente utilizzando le pagine di Layout

Finora si è visto che risulti semplice includere lo stesso contenuto in più pagine. Un approccio più strutturato per la creazione di un aspetto coerente per un sito consiste nell'utilizzare pagine di layout. Una pagina di layout definisce la struttura di una pagina web, ma non contiene contenuto effettivo. Dopo aver creato una pagina di layout, è possibile creare pagine web che includono il contenuto e collegarli alla pagina di layout. Quando vengono visualizzate queste pagine, essi verrà formattati in base alla pagina di layout. (In questo senso, una pagina di layout agisce come un tipo di modello per il contenuto è definito in altre pagine.)

Pagina di layout è esattamente come qualsiasi pagina HTML, ad eccezione del fatto che contiene una chiamata al `RenderBody` metodo. La posizione del `RenderBody` metodo nella pagina di layout determina in cui saranno incluse le informazioni della pagina di contenuto.

Il diagramma seguente mostra le pagine come contenuto e pagine di layout vengono combinate in fase di esecuzione per produrre la pagina web completata. Il browser richiede una pagina di contenuto. La pagina di contenuto contiene codice che specifica la pagina di layout da usare per la struttura della pagina. Nella pagina di layout, il contenuto viene inserito nel punto in cui il `RenderBody` metodo viene chiamato. Contenuto blocchi possono inoltre essere inseriti in una pagina di layout chiamando il `RenderPage` (metodo), il modo in cui è stato eseguito nella sezione precedente. Quando la pagina web è stata completata, viene inviato al browser.

![Screenshot della pagina nel browser risultante dall'esecuzione di una pagina che include le chiamate al metodo RenderBody.](3-creating-a-consistent-look/_static/image3.jpg)

La procedura seguente viene illustrato come creare un layout di pagine di contenuto di pagina e un collegamento ad esso.

1. Nel *Shared* cartella del sito Web, creare un file denominato  *\_Layout1.cshtml*.
2. Sostituire qualsiasi contenuto esistente con il seguente:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample6.html)]

    Utilizzare il `RenderPage` metodo in una pagina di layout per inserire i blocchi di contenuto. Una pagina di layout può contenere solo una chiamata al `RenderBody` metodo.
3. Nel *Shared* cartella, creare un file denominato  *\_Header2.cshtml* e sostituire qualsiasi contenuto esistente con il seguente:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample7.html)]
4. Nella cartella radice, creare una nuova cartella e denominarla *stili*.
5. Nel *stili* cartella, creare un file denominato *Site* e aggiungere le seguenti definizioni di stile:

    [!code-css[Main](3-creating-a-consistent-look/samples/sample8.css)]

    Queste definizioni di stile sono solo per mostrare come fogli di stile possono essere utilizzati con pagine di layout. Se si desidera, è possibile definire gli stili personalizzati per questi elementi.
6. Nella cartella radice, creare un file denominato *Content1.cshtml* e sostituire qualsiasi contenuto esistente con il seguente:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample9.cshtml)]

    Si tratta di una pagina che verrà utilizzata una pagina di layout. Il blocco di codice nella parte superiore della pagina indica la pagina di layout da usare per formattare il contenuto.
7. Eseguire *Content1.cshtml* in un browser. La pagina viene utilizzato il formato e foglio di stile definito in  *\_Layout1.cshtml* e il testo (contenuto) definito *Content1.cshtml*.

    ![[immagine]](3-creating-a-consistent-look/_static/image4.jpg)

    È possibile ripetere il passaggio 6 per creare altre pagine di contenuto che è quindi possono condividere la stessa pagina di layout.

    > [!NOTE]
    > È possibile impostare il sito in modo che si può usare automaticamente nella stessa pagina di layout per tutte le pagine di contenuto in una cartella. Per informazioni dettagliate, vedere [personalizzazione di un comportamento a livello di sito per ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906).

## <a name="designing-layout-pages-that-have-multiple-content-sections"></a>La progettazione di pagine di Layout che hanno più sezioni di contenuto

Una pagina di contenuto può avere più sezioni, questa opzione è utile se si desidera utilizzare i layout che hanno più aree con contenuto sostituibile. Nella pagina di contenuto, assegnare a ogni sezione di un nome univoco. (La sezione predefinita viene lasciata senza nome.) Nella pagina di layout, aggiungere un `RenderBody` per specificare dove cui dovrebbe essere visualizzata la sezione senza nome (impostazione predefinita). Aggiungere quindi separato `RenderSection` metodi per eseguire il rendering delle sezioni denominate singolarmente.

Il diagramma seguente mostra come ASP.NET gestisce il contenuto che viene suddiviso in più sezioni. Ogni sezione denominata è contenuto in un blocco di sezione nella pagina di contenuto. (Sono denominati `Header` e `List` nell'esempio.) Il framework inserisce sezione di contenuto nella pagina di layout nel punto in cui il `RenderSection` metodo viene chiamato. La sezione senza nome (impostazione predefinita) viene inserita nel punto in cui il `RenderBody` metodo viene chiamato, come illustrato in precedenza.

![Diagramma concettuale che illustra come il metodo RenderSection inserisce riferimenti a sezioni della pagina corrente.](3-creating-a-consistent-look/_static/image5.jpg)

Questa procedura viene illustrato come creare una pagina di contenuto con più sezioni di contenuto e come eseguire il rendering utilizzando una pagina di layout che supporta più sezioni di contenuto.

1. Nel *Shared* cartella, creare un file denominato  *\_Layout2.cshtml*.
2. Sostituire qualsiasi contenuto esistente con il seguente:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample10.html)]

    Utilizzare il `RenderSection` metodo per eseguire il rendering delle sezioni di intestazione e l'elenco.
3. Nella cartella radice, creare un file denominato *Content2.cshtml* e sostituire qualsiasi contenuto esistente con il seguente:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample11.cshtml)]

    La pagina contenuta contiene un blocco di codice nella parte superiore della pagina. Ogni sezione denominata è contenuto in un blocco di sezione. Il resto della pagina contiene una sezione di contenuto predefinito (senza nome).
4. Eseguire *Content2.cshtml* in un browser.

    ![Screenshot della pagina nel browser risultante dall'esecuzione di una pagina che include le chiamate al metodo RenderSection.](3-creating-a-consistent-look/_static/image6.jpg)

## <a name="making-content-sections-optional"></a>Rendendo facoltativo sezioni di contenuto

In genere, le sezioni creati in una pagina di contenuto deve essere uguale nelle sezioni che sono definite nella pagina di layout. È possibile ottenere errori se si verifica una delle operazioni seguenti:

- La pagina di contenuto contiene una sezione che non presenta alcuna sezione corrispondente nella pagina di layout.
- Pagina di layout contiene una sezione in cui non è presente alcun contenuto.
- La pagina di layout include chiamate al metodo che tenta di eseguire il rendering nella stessa sezione più volte.

Tuttavia, è possibile eseguire l'override di questo comportamento per una sezione denominata dichiarando la sezione sia facoltativo nella pagina di layout. Ciò consente di definire più pagine di contenuto che possono condividere una pagina di layout ma che potrebbe o non dispone di contenuto per una sezione specifica.

1. Aprire *Content2.cshtml* e rimuovere la sezione seguente:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample12.cshtml)]
2. Salvare la pagina e quindi eseguirlo in un browser. Viene visualizzato un messaggio di errore, perché la pagina di contenuto non include il contenuto per una sezione definita nella pagina di layout, in particolare la sezione di intestazione.

    ![Schermata che illustra l'errore che si verifica se si esegue una pagina che chiama il metodo RenderSection ma la sezione corrispondente non è specificata.](3-creating-a-consistent-look/_static/image7.jpg)
3. Nel *Shared* cartella, aprire il  *\_Layout2.cshtml* pagina e sostituire questa riga:

    [!code-javascript[Main](3-creating-a-consistent-look/samples/sample13.js)]

    con il codice seguente:

    [!code-javascript[Main](3-creating-a-consistent-look/samples/sample14.js)]

    In alternativa, è Impossibile sostituire la riga di codice precedente con il blocco di codice seguente, che produce gli stessi risultati:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample15.cshtml)]
4. Eseguire il *Content2.cshtml* pagina in un browser nuovamente. (Se è ancora aperta questa pagina nel browser, è possibile semplicemente aggiornarlo.) Questa volta la pagina viene visualizzata senza errori, anche se non ha alcuna intestazione.

## <a name="passing-data-to-layout-pages"></a>Passaggio di dati a pagine di Layout

Si dispone di dati definiti nella pagina di contenuto che è necessario fare riferimento in una pagina di layout. In questo caso, è necessario passare i dati della pagina di contenuto alla pagina di layout. Ad esempio, si consiglia di visualizzare lo stato di accesso di un utente o è possibile visualizzare o nascondere le aree di contenuto in base all'input utente.

Per passare dati da una pagina di contenuto a una pagina di layout, è possibile inserire valori nel `PageData` proprietà della pagina di contenuto. Il `PageData` proprietà è una raccolta di coppie nome/valore contenenti i dati che si desidera passare tra le pagine. Nella pagina di layout, sarà quindi possibile leggere i valori di timeout di `PageData` proprietà.

Di seguito è riportato un altro diagramma. Qui viene illustrato come è possibile utilizzare ASP.NET il `PageData` proprietà per passare i valori da una pagina di contenuto alla pagina di layout. Quando inizia la creazione della pagina web ASP.NET, viene creato il `PageData` insieme. Nella pagina di contenuto, scrivere il codice per inserire dati nella `PageData` insieme. I valori di `PageData` raccolta sono accessibili anche da altre sezioni della pagina contenuta o da altri blocchi di contenuto.

![Diagramma concettuale che illustra come una pagina di contenuto è possibile popolare un dizionario PageData e passare tali informazioni alla pagina di layout.](3-creating-a-consistent-look/_static/image8.jpg)

La procedura seguente viene illustrato come passare i dati da una pagina contenuto per una pagina di layout. Quando la pagina viene eseguita, viene visualizzato un pulsante che consente all'utente di nascondere o visualizzare un elenco definito nella pagina di layout. Quando gli utenti fanno clic sul pulsante, imposta un valore true o false (Boolean) `PageData` proprietà. Pagina di layout legge di tale valore e se è false, vengono nascosti nell'elenco. Il valore viene anche utilizzato nella pagina di contenuto per determinare se visualizzare il **Nascondi elenco** pulsante o **Mostra elenco** pulsante.

![[immagine]](3-creating-a-consistent-look/_static/image9.jpg)

1. Nella cartella radice, creare un file denominato *Content3.cshtml* e sostituire qualsiasi contenuto esistente con il seguente:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample16.cshtml)]

    Il codice memorizza due tipi di dati di `PageData` proprietà &#8212; il titolo della pagina web e true o false per specificare se visualizzare un elenco.

    Si noti che ASP.NET consente di inserire il markup HTML della pagina in modo condizionale utilizzando un blocco di codice. Ad esempio, il `if/else` blocco nel corpo della pagina determina il modulo da visualizzare a seconda che `PageData["ShowList"]` è impostata su true.
2. Nel *Shared* cartella, creare un file denominato  *\_Layout3.cshtml* e sostituire qualsiasi contenuto esistente con il seguente:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample17.cshtml)]

    Pagina di layout include un'espressione nel `<title>` elemento che ottiene il valore del titolo di `PageData` proprietà. Utilizza inoltre il `ShowList` valore il `PageData` proprietà per determinare se visualizzare il blocco di contenuto di elenco.
3. Nel *Shared* cartella, creare un file denominato  *\_List.cshtml* e sostituire qualsiasi contenuto esistente con il seguente:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample18.html)]
4. Eseguire il *Content3.cshtml* pagina in un browser. Verrà visualizzata la pagina con l'elenco visibile sul lato sinistro della pagina e un **Nascondi elenco** nella parte inferiore.

    ![Screenshot che illustra la pagina che include l'elenco e un pulsante contenente la stringa 'Nascondi elenco'.](3-creating-a-consistent-look/_static/image10.jpg)
5. Fare clic su **Nascondi elenco**. L'elenco viene rimosso e il pulsante diventa **Mostra elenco**.

    ![Screenshot che illustra la pagina che non include l'elenco e su un pulsante Mostra elenco.](3-creating-a-consistent-look/_static/image11.jpg)
6. Fare clic su di **Mostra elenco** pulsante e l'elenco viene visualizzato nuovamente.

## <a name="additional-resources"></a>Risorse aggiuntive


[Personalizzazione del comportamento a livello di sito per le pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202906)
