---
uid: mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-vb
title: Creazione di layout di pagina con pagine Master di visualizzazione (VB) | Microsoft Docs
author: microsoft
description: In questa esercitazione descrive come creare un layout di pagina comuni per più pagine nell'applicazione, sfruttando i vantaggi della visualizzazione pagine master. È possibile usare un...
ms.author: aspnetcontent
ms.date: 10/16/2008
ms.assetid: d34f90a1-6de3-482a-a326-f87fdcbaaaff
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 75db98647b9595deedc601905f6a0c0ddc596302
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824690"
---
<a name="creating-page-layouts-with-view-master-pages-vb"></a>Creazione di layout di pagina con pagine Master di visualizzazione (VB)
====================
da [Microsoft](https://github.com/microsoft)

[Scaricare PDF](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_12_VB.pdf)

> In questa esercitazione descrive come creare un layout di pagina comuni per più pagine nell'applicazione, sfruttando i vantaggi della visualizzazione pagine master. È possibile utilizzare una pagina master di visualizzazione, ad esempio, per definire un layout di pagina a due colonne e usare il layout a due colonne per tutte le pagine nell'applicazione web.


## <a name="creating-page-layouts-with-view-master-pages"></a>Creazione di layout di pagina con pagine Master di visualizzazione

In questa esercitazione descrive come creare un layout di pagina comuni per più pagine nell'applicazione, sfruttando i vantaggi della visualizzazione pagine master. È possibile utilizzare una pagina master di visualizzazione, ad esempio, per definire un layout di pagina a due colonne e usare il layout a due colonne per tutte le pagine nell'applicazione web.

È anche possibile sfruttare i vantaggi della visualizzazione pagine master per condividere contenuto comune tra più pagine dell'applicazione. Ad esempio, è possibile inserire il logo del sito Web, i collegamenti di navigazione e annunci di banner in una pagina master di visualizzazione. In questo modo, ogni pagina nell'applicazione verrà visualizzati automaticamente questo contenuto.

In questa esercitazione descrive come creare una nuova pagina master di visualizzazione e creare una nuova pagina contenuto visualizzazione della pagina master.

### <a name="creating-a-view-master-page"></a>Creazione di una pagina Master di visualizzazione

Iniziamo creando una pagina master di visualizzazione che definisce un layout a due colonne. È aggiungere una nuova pagina master di visualizzazione per un progetto MVC facendo cartella Views\Shared selezionando l'opzione di menu **Aggiungi, elemento nuovo**e selezionando il modello di pagina Master visualizzazione MVC (vedere la figura 1).


[![Aggiunta di una pagina master di visualizzazione](creating-page-layouts-with-view-master-pages-vb/_static/image2.png)](creating-page-layouts-with-view-master-pages-vb/_static/image1.png)

**Figura 01**: aggiunta di una pagina master visualizzazione ([fare clic per visualizzare l'immagine con dimensioni normali](creating-page-layouts-with-view-master-pages-vb/_static/image3.png))


È possibile creare più di una pagina master di visualizzazione in un'applicazione. Ogni pagina master di visualizzazione è possibile definire un layout di pagina diversa. Ad esempio, è possibile determinate pagine di per hanno un layout a due colonne e altre pagine di per hanno un layout a tre colonne.

Una pagina master di visualizzazione è molto simile una visualizzazione ASP.NET MVC standard. Tuttavia, a differenza di una visualizzazione normale, una pagina master visualizzazione contiene uno o più `<asp:ContentPlaceHolder>` tag. Il `<contentplaceholder>` i tag vengono usati per contrassegnare le aree della pagina master che può essere sottoposto a override in una singola pagina contenuto.

La pagina master di visualizzazione nel listato 1, ad esempio, definisce un layout a due colonne. Contiene due `<contentplaceholder>` tag. Uno `<ContentPlaceHolder>` per ogni colonna.

**Listato 1: `Views\Shared\Site.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample1.aspx)]

Il corpo della visualizzazione della pagina master nel listato 1 contiene due `<div>` tag che corrispondono alle due colonne. La classe di colonna del foglio di stile viene applicata a entrambe `<div>` tag. Questa classe è definita nel foglio di stile dichiarato nella parte superiore della pagina master. È possibile visualizzare in anteprima come pagina master visualizzazione verrà visualizzata passando alla visualizzazione progettazione. Fare clic sulla scheda Progettazione in basso a sinistra dell'editor del codice sorgente (vedere la figura 2).


[![Visualizzazione in anteprima di una pagina master nella finestra di progettazione](creating-page-layouts-with-view-master-pages-vb/_static/image5.png)](creating-page-layouts-with-view-master-pages-vb/_static/image4.png)

**Figura 02**: la visualizzazione in anteprima una pagina master nella finestra di progettazione ([fare clic per visualizzare l'immagine con dimensioni normali](creating-page-layouts-with-view-master-pages-vb/_static/image6.png))


### <a name="creating-a-view-content-page"></a>Creazione di una pagina contenuto visualizzazione

Dopo aver creato una pagina master di visualizzazione, è possibile creare Visualizza uno o più pagine di contenuto basate sulla pagina master di visualizzazione. Ad esempio, è possibile creare una pagina contenuta visualizzazione di indice per il controller Home facendo clic su cartella Views\Home selezionando **Aggiungi, elemento nuove**, se si seleziona il **pagina contenuto visualizzazione MVC** modello, immettere il nome di index. aspx e facendo clic su Aggiungi pulsante (vedere la figura 3).


[![Aggiunta di una pagina contenuto visualizzazione](creating-page-layouts-with-view-master-pages-vb/_static/image8.png)](creating-page-layouts-with-view-master-pages-vb/_static/image7.png)

**Figura 03**: aggiunta di una pagina contenuto visualizzazione ([fare clic per visualizzare l'immagine con dimensioni normali](creating-page-layouts-with-view-master-pages-vb/_static/image9.png))


Dopo che si fa clic sul pulsante Aggiungi, viene visualizzata una nuova finestra di dialogo che consente di selezionare una pagina master di visualizzazione da associare alla pagina contenuto visualizzazione (vedere la figura 4). È possibile passare alla pagina master di visualizzazione Site. master che abbiamo creato nella sezione precedente.


[![Selezione di una pagina master](creating-page-layouts-with-view-master-pages-vb/_static/image11.png)](creating-page-layouts-with-view-master-pages-vb/_static/image10.png)

**Figura 04**: la selezione di una pagina master ([fare clic per visualizzare l'immagine con dimensioni normali](creating-page-layouts-with-view-master-pages-vb/_static/image12.png))


Dopo aver creato una nuova pagina contenuto visualizzazione della pagina master Site. master, ottenere il file nel listato 2.

**Listato 2: `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample2.aspx)]

Si noti che questa vista contiene un `<asp:Content>` tag che corrisponde a ogni il `<asp:ContentPlaceHolder>` tag nella pagina master di visualizzazione. Ciascuna `<asp:Content>` tag include un attributo ContentPlaceHolderID che punta a particolari `<asp:ContentPlaceHolder>` sottoposto a override.

Si noti inoltre che la pagina di visualizzazione di contenuto nel listato 2 non contiene uno qualsiasi dei normali tag di apertura e chiusura HTML. Ad esempio, non contiene l'apertura e chiusura `<html>` o `<head>` tag. Tutte le normali tag di apertura e chiusura sono contenute nella pagina master di visualizzazione.

Qualsiasi contenuto che si desidera visualizzare in una pagina contenuto visualizzazione deve essere inserito in un `<asp:Content>` tag. Se si inserisce codice HTML o altri contenuti di fuori di questi tag, quindi si otterrà un errore quando si prova a visualizzare la pagina.

Non è necessario eseguire l'override di ogni `<asp:ContentPlaceHolder>` tag da una pagina master in una pagina di visualizzazione del contenuto. È sufficiente eseguire l'override di un `<asp:ContentPlaceHolder>` tag quando si desidera sostituire il tag con contenuto specifico.

Ad esempio, la visualizzazione dell'indice modificata nel listato 3 contiene solo due `<asp:Content>` tag. Ognuna del `<asp:Content>` tag include i testo.

**Listato 3: `Views\Home\Index.aspx (modified)`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample3.aspx)]

Quando viene richiesta la visualizzazione nel listato 3, rendering della pagina nella figura 5. Si noti che la visualizzazione esegue il rendering di una pagina con due colonne. Si noti inoltre che il contenuto della pagina di visualizzazione contenuto viene unito con il contenuto della pagina master di visualizzazione.


[![La pagina contenuta visualizzazione Index](creating-page-layouts-with-view-master-pages-vb/_static/image14.png)](creating-page-layouts-with-view-master-pages-vb/_static/image13.png)

**Figura 05**: pagina contenuto visualizzazione l'indice ([fare clic per visualizzare l'immagine con dimensioni normali](creating-page-layouts-with-view-master-pages-vb/_static/image15.png))


### <a name="modifying-view-master-page-content"></a>Contenuto della pagina Master di visualizzazione di modifica

Un problema che si verifica quasi immediatamente quando si lavora con le pagine master di visualizzazione è il problema della modifica del contenuto pagina master di visualizzazione quando vengono richieste le pagine di contenuto di una visualizzazione diversa. Ad esempio, si desidera che ogni pagina nell'applicazione web per avere un titolo univoco. Tuttavia, il titolo è dichiarato nella pagina master di visualizzazione e non nella pagina di contenuto della vista. Pertanto, come si personalizza il titolo della pagina per ogni pagina contenuto visualizzazione?

Esistono due modi in cui è possibile modificare il titolo visualizzato da una pagina di visualizzazione del contenuto. In primo luogo, è possibile assegnare un titolo della pagina per l'attributo title del `<%@ page %>` direttiva dichiarato nella parte superiore di una pagina di visualizzazione del contenuto. Ad esempio, se si desidera assegnare il titolo della pagina "Sito Web eccezionali con privilegi avanzati" per la visualizzazione dell'indice, è possibile includere la direttiva seguente nella parte superiore della visualizzazione Index:

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample4.aspx)]

Quando la visualizzazione dell'indice viene eseguito il rendering nel browser, viene visualizzato il titolo desiderato nella barra del titolo del browser:


[![Barra del titolo del browser](creating-page-layouts-with-view-master-pages-vb/_static/image17.png)](creating-page-layouts-with-view-master-pages-vb/_static/image16.png)


È un requisito importante che una pagina di visualizzazione master deve soddisfare affinché l'attributo title lavorare. La pagina master visualizzazione deve contenere una `<head runat="server">` tag anziché una normale `<head>` tag per l'intestazione specificata. Se il `<head>` tag non include il runat = "server" attributo, quindi non sarà più visualizzato il titolo. La visualizzazione predefinita include la pagina master `<head runat="server">` tag.

È un approccio alternativo alla modifica del contenuto pagina master da una pagina di contenuto di visualizzazione per eseguire il wrapping l'area in cui si desidera modificare un `<asp:ContentPlaceHolder>` tag. Si supponga, ad esempio, che si desidera modificare non solo il titolo, ma anche il tag meta, viene eseguito il rendering di una pagina di visualizzazione master. La pagina della visualizzazione master nel listato 4 contiene un `<asp:ContentPlaceHolder>` all'interno del tag relativo `<head>` tag.

**Listato 4: `Views\Shared\Site2.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample5.aspx)]

Si noti che il `<asp:ContentPlaceHolder>` tag nel listato 4 include il contenuto predefinito: titolo predefinito e i tag di metadati predefiniti. Se si non esegue l'override di questo `<asp:ContentPlaceHolder>` tag in una pagina contenuto visualizzazione singoli, verrà visualizzato il contenuto predefinito.

La pagina di visualizzazione di contenuto nel listato 5 esegue l'override di `<asp:ContentPlaceHolder>` tag per visualizzare un titolo personalizzato e tag meta personalizzati.

**Listato 5: `Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample6.aspx)]

### <a name="summary"></a>Riepilogo

Questa esercitazione ha fornito un'introduzione di base per visualizzare le pagine master e le pagine di contenuto. Si è appreso come creare nuova vista pagine master e creare pagine di contenuto visualizzazione basate su di essi. Abbiamo anche esaminato come è possibile modificare il contenuto di una pagina master di visualizzazione da una pagina contenuto visualizzazione particolare.

> [!div class="step-by-step"]
> [Precedente](using-the-tagbuilder-class-to-build-html-helpers-vb.md)
> [Successivo](passing-data-to-view-master-pages-vb.md)
