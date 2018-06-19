---
uid: web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-vb
title: Utilizzo del controllo Editor HTML (VB) | Microsoft Docs
author: microsoft
description: HTMLEditor è un controllo AJAX di ASP.NET che consente di creare e modificare contenuto HTML tramite i pulsanti in una barra degli strumenti con facilità.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 32ec9321-7c8c-4b0f-8234-99acb56df6b5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 4833949a54fa9ae12eaf7b596a5fe1ddfd1f7b7a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873300"
---
<a name="how-do-i-use-the-html-editor-control-vb"></a>Utilizzo del controllo Editor HTML (VB)
====================
by [Microsoft](https://github.com/microsoft)

> HTMLEditor è un controllo AJAX di ASP.NET che consente di creare e modificare contenuto HTML tramite i pulsanti in una barra degli strumenti con facilità.


L'obiettivo di questa esercitazione è per fornire una panoramica del controllo Editor HTML incluso con AJAX Control Toolkit. Editor HTML include opzioni per le dimensioni del carattere, la selezione di un tipo di carattere, colore di sfondo modifica, modifica il colore di primo piano, aggiunta di collegamenti, aggiunta di immagini, modificare l'allineamento del testo e si eseguono le operazioni Taglia, copia e Incolla di operazioni (vedere la figura 1).


[![Editor HTML](how-do-i-use-the-html-editor-control-vb/_static/image1.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image1.png)

**Figura 01**: l'Editor HTML ([fare clic per visualizzare l'immagine ingrandita](how-do-i-use-the-html-editor-control-vb/_static/image2.png))


Editor HTML consente di immettere contenuto utilizzando una modalità di progettazione o è possibile immettere direttamente HTML. Vengono inoltre fornite con l'opzione per visualizzare in anteprima il contenuto HTML (vedere la figura 2).


[![Progettazione, HTML e anteprima di pulsanti](how-do-i-use-the-html-editor-control-vb/_static/image2.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image3.png)

**Figura 02**: pulsanti di progettazione, HTML e nell'anteprima ([fare clic per visualizzare l'immagine ingrandita](how-do-i-use-the-html-editor-control-vb/_static/image4.png))


In questa esercitazione, è illustrato come visualizzare l'Editor HTML, come personalizzare i pulsanti della barra degli strumenti che vengono visualizzati nell'Editor HTML e come evitare attacchi di Cross-Site Scripting.

## <a name="displaying-the-html-editor"></a>Visualizzazione dell'Editor HTML

Prima di poter utilizzare l'Editor HTML in una pagina ASP.NET, è innanzitutto necessario aggiungere un controllo ScriptManager alla pagina. Il controllo ScriptManager si trova sotto la scheda Estensioni AJAX nella casella degli strumenti di Visual Studio e Visual Web Developer Express.

È consigliabile inserire il controllo ScriptManager nella parte superiore della pagina prima di eventuali altri controlli nella pagina. Ad esempio, è possibile inserirlo immediatamente di sotto del lato server, apertura &lt;modulo&gt; tag.

Il controllo dell'Editor HTML si trova nella casella degli strumenti con il resto dei controlli AJAX Control Toolkit. Il controllo dell'Editor è denominato (vedere la figura 3).


[![Il controllo dell'Editor HTML](how-do-i-use-the-html-editor-control-vb/_static/image3.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image5.png)

**Figura 03**: l'Editor HTML controllo ([fare clic per visualizzare l'immagine ingrandita](how-do-i-use-the-html-editor-control-vb/_static/image6.png))


Dopo avere trascinato l'Editor HTML in una pagina, è possibile impostare le proprietà nella finestra delle proprietà. Ad esempio, in genere si desidera impostare le proprietà di larghezza e altezza. Elenco 1 contiene l'origine di una pagina ASP.NET che contiene un editor HTML.

**Elenco 1 - SimpleEditor.aspx**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-vb/samples/sample1.aspx)]

La pagina nel listato 1 contiene un controllo Editor HTML, un controllo Button e un controllo Literal. Quando si fa clic sul pulsante, il contenuto dell'Editor HTML visualizzato nel controllo Literal (vedere la figura 4).


[![Invio di un modulo con un Editor HTML](how-do-i-use-the-html-editor-control-vb/_static/image4.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image7.png)

**Figura 04**: invio di un modulo con un Editor HTML ([fare clic per visualizzare l'immagine ingrandita](how-do-i-use-the-html-editor-control-vb/_static/image8.png))


La proprietà Content di Editor HTML viene utilizzata per recuperare il contenuto HTML immesso nell'Editor HTML. Tenere presente che il contenuto HTML può contenere JavaScript. Nella sezione successiva, è illustrare come è possibile impedire attacchi Injection JavaScript.

## <a name="customizing-the-html-editor-toolbar"></a>Personalizzazione degli strumenti dell'Editor HTML

È possibile personalizzare esattamente quali pulsanti vengono visualizzati nell'editor. Potrebbe ad esempio, si desidera rimuovere la scheda HTML per impedire agli utenti di attivare l'Editor HTML nella modalità HTML. In alternativa, è possibile rimuovere l'elenco a discesa delle dimensioni del carattere per impedire agli utenti di creazione del testo di dimensioni molto grande in un forum messaggio post (vedere Figura 5).


[![Un Editor HTML personalizzato](how-do-i-use-the-html-editor-control-vb/_static/image5.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image9.png)

**Figura 05**: A personalizzato Editor HTML ([fare clic per visualizzare l'immagine ingrandita](how-do-i-use-the-html-editor-control-vb/_static/image10.png))


Per personalizzare i pulsanti della barra degli strumenti, un nuovo Editor HTML di derivazione dalla classe di base dell'Editor. Ad esempio, l'editor personalizzato nel listato 2 contiene solo i pulsanti della barra degli strumenti in grassetto e corsivo. Sono stati rimossi tutti gli altri pulsanti della barra degli strumenti. Inoltre, la scheda HTML è stata rimossa dalla parte inferiore dell'editor (ma le schede di progettazione e anteprima sono ancora presenti).

**Il listato 2 - App\_Code\CustomEditor.vb**

[!code-vb[Main](how-do-i-use-the-html-editor-control-vb/samples/sample2.vb)]

È necessario aggiungere la classe nel listato 2 all'app\_cartella del codice in modo che la classe verrà compilata automaticamente. Se l'App\_codice cartella non esiste nel sito Web, quindi è possibile aggiungere semplicemente la cartella.

Dopo aver creato un editor personalizzato, è possibile aggiungere, a una pagina ASP.NET nello stesso modo quando si aggiungono Editor HTML normale (vedere Listato 3).

**Elenco di 3 - ShowCustomEditor.aspx**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-vb/samples/sample3.aspx)]

## <a name="avoiding-cross-site-scripting-xss-attacks"></a>Come evitare attacchi di Cross-Site Scripting (XSS)

Ogni volta che si accetta l'input dell'utente e visualizzare nuovamente tale input nel sito Web, è potenzialmente aprire il sito Web ad attacchi di Cross-Site Scripting (XSS). In teoria, un utente malintenzionato potrebbe inviare codice JavaScript che viene eseguito quando viene visualizzata di nuovo l'input. Il codice JavaScript può essere usato per rubare le password utente o altre informazioni riservate.

In genere, si possono annullare gli effetti di attacchi XSS codifica qualsiasi input recuperate da un utente prima di visualizzarlo in una pagina web HTML. Tuttavia, l'output dell'Editor HTML codifica HTML sarebbe non solo la codifica &lt;script&gt; tag, sarebbe inoltre codificare tutti i tag HTML. In altre parole, si perderanno tutta la formattazione, ad esempio il tipo di carattere, dimensioni del carattere e colore di sfondo.

Se si stanno raccogliendo le informazioni riservate agli utenti, ad esempio password, numeri di carta di credito e i numeri della previdenza sociale, quindi non possono essere visualizzate contenuto decodificato recuperati da un utente con l'Editor HTML. Utilizzare l'Editor HTML solo in situazioni in cui non si è rivisualizzazione il contenuto HTML o il contenuto HTML viene inviato al sito Web da un'entità attendibile.

Si supponga, ad esempio, che si sta creando un'applicazione di blog. In questo caso, è consigliabile utilizzare l'Editor HTML durante la composizione dei post di blog. Si è l'unico utente invia un post di blog e, probabilmente, è possibile considerare attendibile per non inviare JavaScript dannoso. Tuttavia non ha senso per utilizzare l'Editor HTML quando si consente agli utenti anonimi di inviare commenti. È necessario prestare particolare attenzione in situazioni in cui gli utenti inviano informazioni riservate, ad esempio le password. Potenzialmente, un utente malintenzionato potrebbe inviare un commento che contiene il codice JavaScript corretto per l'acquisizione di una password.

## <a name="summary"></a>Riepilogo

In questa esercitazione sono stati forniti con una breve panoramica del controllo Editor HTML incluso in AJAX Control Toolkit. È stato descritto come utilizzare l'Editor HTML per accettare il contenuto dettagliato di un utente e inviare il contenuto al server. È stato descritto come personalizzare i pulsanti della barra degli strumenti che vengono visualizzati dall'Editor HTML. Infine, è stato descritto come evitare attacchi di Cross-Site Scripting quando si utilizza l'Editor HTML per accettare l'input potenzialmente dannosa.

> [!div class="step-by-step"]
> [Precedente](how-do-i-use-the-html-editor-control-cs.md)
