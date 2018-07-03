---
uid: web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-vb
title: Come si usa il controllo dell'Editor HTML? (VB) | Microsoft Docs
author: microsoft
description: HTMLEditor è un controllo AJAX di ASP.NET che consente di creare e modificare contenuto HTML tramite i pulsanti in una barra degli strumenti con facilità.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 32ec9321-7c8c-4b0f-8234-99acb56df6b5
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 4b3882f631799eb99f3c63da89097daba1fca3ff
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37401967"
---
<a name="how-do-i-use-the-html-editor-control-vb"></a>Come si usa il controllo dell'Editor HTML? (VB)
====================
da [Microsoft](https://github.com/microsoft)

> HTMLEditor è un controllo AJAX di ASP.NET che consente di creare e modificare contenuto HTML tramite i pulsanti in una barra degli strumenti con facilità.


L'obiettivo di questa esercitazione è fornire una panoramica del controllo HTMLEditor incluso in AJAX Control Toolkit. L'Editor HTML include opzioni per la modifica delle dimensioni del carattere, la selezione di un tipo di carattere, modifica il colore di sfondo, la modifica il colore di primo piano, l'aggiunta di collegamenti, aggiunta di immagini, modificare l'allineamento del testo e si eseguono le operazioni Taglia, copia e Incolla le operazioni (vedi figura 1).


[![L'Editor HTML](how-do-i-use-the-html-editor-control-vb/_static/image1.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image1.png)

**Figura 01**: l'Editor HTML ([fare clic per visualizzare l'immagine con dimensioni normali](how-do-i-use-the-html-editor-control-vb/_static/image2.png))


L'editor HTML consente di immettere il contenuto con una modalità di progettazione oppure è possibile immettere direttamente HTML. Viene anche fornito con l'opzione per visualizzare in anteprima il contenuto HTML (vedere la figura 2).


[![Progettazione, HTML e anteprima di pulsanti](how-do-i-use-the-html-editor-control-vb/_static/image2.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image3.png)

**Figura 02**: i pulsanti di progettazione, HTML e nell'anteprima ([fare clic per visualizzare l'immagine con dimensioni normali](how-do-i-use-the-html-editor-control-vb/_static/image4.png))


In questa esercitazione descrive come visualizzare l'Editor HTML, come personalizzare i pulsanti della barra degli strumenti che vengono visualizzati nell'Editor HTML e come evitare gli attacchi di Cross-Site Scripting.

## <a name="displaying-the-html-editor"></a>Visualizzazione dell'Editor HTML

Prima di utilizzare l'Editor HTML in una pagina ASP.NET, è innanzitutto necessario aggiungere un controllo ScriptManager alla pagina. Il controllo ScriptManager si trova sotto la scheda Estensioni AJAX nella casella degli strumenti di Visual Studio e Visual Web Developer Express.

È consigliabile inserire il controllo ScriptManager nella parte superiore della pagina prima di eventuali altri controlli nella pagina. Ad esempio, è possibile inserirlo immediatamente di sotto del lato server, apertura &lt;form&gt; tag.

Il controllo dell'Editor HTML si trova nella casella degli strumenti con il resto dei controlli AJAX Control Toolkit. Il file viene denominato il controllo dell'Editor (vedere la figura 3).


[![Il controllo dell'Editor HTML](how-do-i-use-the-html-editor-control-vb/_static/image3.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image5.png)

**Figura 03**: l'Editor HTML controllo ([fare clic per visualizzare l'immagine con dimensioni normali](how-do-i-use-the-html-editor-control-vb/_static/image6.png))


Dopo avere trascinato l'Editor HTML in una pagina, è possibile impostare le relative proprietà nella finestra delle proprietà. Ad esempio, in genere si desidera impostare le proprietà Width e Height. L'elenco 1 contiene l'origine per una pagina ASP.NET che contiene un editor HTML.

**Listato 1 - SimpleEditor.aspx**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-vb/samples/sample1.aspx)]

La pagina nel listato 1 contiene un controllo Editor HTML, un controllo Button e un controllo Literal. Quando si fa clic sul pulsante, il contenuto dell'Editor HTML viene visualizzato nel controllo Literal (vedere la figura 4).


[![Invio di un modulo con un Editor HTML](how-do-i-use-the-html-editor-control-vb/_static/image4.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image7.png)

**Figura 04**: invio di un modulo con un Editor HTML ([fare clic per visualizzare l'immagine con dimensioni normali](how-do-i-use-the-html-editor-control-vb/_static/image8.png))


La proprietà Content dell'Editor HTML viene utilizzata per recuperare il contenuto HTML immesso nell'Editor di codice HTML. Tenere presente che il contenuto HTML può contenere JavaScript. La sezione successiva illustra come evitare attacchi Injection JavaScript.

## <a name="customizing-the-html-editor-toolbar"></a>Personalizzazione degli strumenti dell'Editor HTML

È possibile personalizzare esattamente quali pulsanti vengono visualizzati nell'editor. Ad esempio, è possibile rimuovere la scheda HTML per impedire agli utenti di attivare l'Editor HTML nella modalità HTML. In alternativa, è possibile rimuovere l'elenco a discesa delle dimensioni del tipo di carattere per impedire agli utenti di creare testo di grandi dimensioni eccessivamente in un forum dei messaggi post (vedere la figura 5).


[![Un Editor di codice HTML personalizzato](how-do-i-use-the-html-editor-control-vb/_static/image5.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image9.png)

**Figura 05**: oggetto personalizzato dell'Editor HTML ([fare clic per visualizzare l'immagine con dimensioni normali](how-do-i-use-the-html-editor-control-vb/_static/image10.png))


Per personalizzare i pulsanti della barra degli strumenti, la derivazione dalla classe di base dell'Editor di un nuovo Editor HTML. Ad esempio, l'editor personalizzato nel listato 2 contiene solo i pulsanti della barra degli strumenti per il grassetto e corsivo. Sono stati rimossi tutti gli altri pulsanti della barra degli strumenti. Inoltre, scheda HTML è stato rimosso dalla parte inferiore dell'editor (ma le schede di progettazione e anteprima sono ancora presenti).

**Listato 2 - App\_Code\CustomEditor.vb**

[!code-vb[Main](how-do-i-use-the-html-editor-control-vb/samples/sample2.vb)]

È necessario aggiungere la classe nel listato 2 all'app\_cartella del codice in modo che la classe verrà compilata automaticamente. Se l'App\_cartella del codice non esiste nel sito Web, è possibile aggiungere semplicemente la cartella.

Dopo aver creato un editor personalizzato, è possibile aggiungerlo a una pagina ASP.NET nello stesso modo quando si aggiungono l'Editor HTML normale (vedere il listato 3).

**Listato 3 - ShowCustomEditor.aspx**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-vb/samples/sample3.aspx)]

## <a name="avoiding-cross-site-scripting-xss-attacks"></a>Evitando gli attacchi di Cross-Site Scripting (XSS)

Ogni volta che si accettano input dell'utente e quindi visualizzare di nuovo quell'input nel tuo sito Web, è potenzialmente possibile aprire il sito Web da attacchi di Cross-Site Scripting (XSS). In teoria, un utente malintenzionato potrebbe inviare codice JavaScript che viene eseguito quando l'input viene nuovamente visualizzata. Il codice JavaScript grado di rubare le password degli utenti o altre informazioni riservate.

In genere, è possibile aggirare gli attacchi XSS per codifica qualunque input è recuperare da un utente prima di visualizzarla in una pagina web HTML. Tuttavia, l'output dell'Editor HTML codificate in formato HTML sarebbe non solo la codifica &lt;script&gt; tag, verrà usato anche per codificare tutti i tag HTML. In altre parole, si perderebbero tutta la formattazione, ad esempio il tipo di carattere, dimensioni del carattere e colore di sfondo.

Se si stanno raccogliendo le informazioni riservate da parte degli utenti, ad esempio password, numeri di carta di credito e numeri di previdenza sociale, quindi non possono essere visualizzate non codificato contenuto recuperati da un utente con l'Editor HTML. Utilizzare l'Editor HTML solo in situazioni in cui non si è visualizzare di nuovo il contenuto HTML o il contenuto HTML viene inviato al sito Web da un'entità attendibile.

Si supponga, ad esempio, che si sta creando un'applicazione blog. In questo caso, è opportuno usare l'Editor HTML durante la composizione di post di blog. Si è l'unico autorizzato invia un post di blog e, presumibilmente, si può considerare attendibili personalmente non per inviare codice JavaScript dannoso. Tuttavia non ha senso utilizzare l'Editor HTML quando si consente agli utenti anonimi di inviare commenti. È necessario prestare particolare attenzione in situazioni in cui gli utenti inviano le informazioni riservate, ad esempio le password. Potenzialmente, un utente malintenzionato è stato possibile pubblicare un commento che contiene il codice JavaScript a destra per rubare la password.

## <a name="summary"></a>Riepilogo

In questa esercitazione, sono stati specificati con una breve panoramica del controllo HTMLEditor incluso in AJAX Control Toolkit. Si è appreso come usare l'Editor HTML per contenuto avanzato tra un utente di accettare e inviare il contenuto al server. È stato anche illustrato come è possibile personalizzare i pulsanti della barra degli strumenti che vengono visualizzati dall'Editor HTML. Infine, si è appreso come evitare gli attacchi di Cross-Site Scripting quando si usa l'Editor HTML di accettare input potenzialmente dannosi.

> [!div class="step-by-step"]
> [Precedente](how-do-i-use-the-html-editor-control-cs.md)
