---
uid: web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
title: Creazione di pagine della Guida per l'API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 04/01/2013
ms.assetid: 0150e67b-c50d-4613-83ea-7b4ef8cacc5a
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
msc.type: authoredcontent
ms.openlocfilehash: c081064a32151a71fc4f3ea407e0c48a1539432a
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/10/2018
ms.locfileid: "48913125"
---
<a name="creating-help-pages-for-aspnet-web-api"></a>Creazione di pagine della Guida per l'API Web ASP.NET
====================
da [Mike Wasson](https://github.com/MikeWasson)

Quando si crea un'API web, è spesso utile creare una pagina della Guida, in modo che altri sviluppatori saranno in grado di chiamare l'API. È possibile creare tutta la documentazione manualmente, ma è preferibile per la generazione automatica quanto più possibile.

Per semplificare questa attività, API Web ASP.NET fornisce una libreria per la generazione automatica di pagine della Guida in fase di esecuzione.

![](creating-api-help-pages/_static/image1.png)

## <a name="creating-api-help-pages"></a>Creazione di pagine della Guida di API

Installare [ASP.NET e Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650). Questo aggiornamento pagine della Guida si integra il modello di progetto API Web.

Successivamente, creare un nuovo progetto ASP.NET MVC 4 e selezionare il modello di progetto API Web. Il modello di progetto crea un controller API di esempio denominato `ValuesController`. Il modello crea anche le pagine della Guida dell'API. Tutti i file di codice per la pagina della Guida vengono inseriti nella cartella aree del progetto.

![](creating-api-help-pages/_static/image2.png)

Quando si esegue l'applicazione, la home page contiene un collegamento alla pagina della Guida dell'API. Dalla home page, il percorso relativo è /Help.

![](creating-api-help-pages/_static/image3.png)

Questo collegamento consente di accedere a una pagina di riepilogo di API.

![](creating-api-help-pages/_static/image4.png)

La visualizzazione MVC per questa pagina è definita nel Areas/HelpPage/Views/Help/Index.cshtml. È possibile modificare questa pagina per modificare il layout, introduzione, title, stili e così via.

La parte principale della pagina è una tabella delle API, raggruppate dal controller. Le voci della tabella vengono generate in modo dinamico, usando il **IApiExplorer** interfaccia. (Parlerò ulteriori informazioni su questa interfaccia in un secondo momento.) Se si aggiunge un nuovo controller di API, la tabella viene aggiornata automaticamente in fase di esecuzione.

La colonna "API" Elenca l'URI relativo e il metodo HTTP. La colonna "Description" contiene la documentazione per ogni API. Inizialmente, la documentazione è semplicemente testo segnaposto. Nella sezione successiva, vi mostrerò come aggiungere la documentazione da commenti in formato XML.

Ogni API contiene un collegamento a una pagina con informazioni più dettagliate, tra cui i corpi di richiesta e risposta di esempio.

![](creating-api-help-pages/_static/image5.png)

## <a name="adding-help-pages-to-an-existing-project"></a>Aggiunta di pagine della Guida per un progetto esistente

È possibile aggiungere le pagine della Guida per un progetto API Web esistente usando Gestione pacchetti NuGet. Questa opzione è utile che iniziare da un modello di progetto diverso rispetto al modello "API Web".

Dal **strumenti** dal menu **Gestione pacchetti NuGet**, quindi selezionare **Console di Gestione pacchetti**. Nel [Console di gestione pacchetti](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) finestra, digitare uno dei seguenti comandi:

Per un **c#** applicazione: `Install-Package Microsoft.AspNet.WebApi.HelpPage`

Per un **Visual Basic** applicazione: `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`

Esistono due pacchetti, uno per il linguaggio c# e uno per Visual Basic. Assicurarsi di usare quello che corrisponde al progetto.

Questo comando Installa gli assembly necessari e aggiunge le visualizzazioni MVC per le pagine della Guida (che si trova nella cartella aree/HelpPage). È necessario aggiungere manualmente un collegamento alla pagina della Guida. L'URI è /Help. Per creare un collegamento in una visualizzazione razor, aggiungere quanto segue:

[!code-cshtml[Main](creating-api-help-pages/samples/sample1.cshtml)]

Inoltre, assicurarsi di registrare le aree. Nel file Global. asax, aggiungere il codice seguente per il **Application\_avviare** metodo, se non esiste già:

[!code-csharp[Main](creating-api-help-pages/samples/sample2.cs?highlight=4)]

## <a name="adding-api-documentation"></a>Aggiunta di documentazione dell'API

Per impostazione predefinita, la Guida in linea nelle pagine siano stringhe di segnaposto per la documentazione. È possibile usare [commenti in formato documentazione XML](https://msdn.microsoft.com/library/b2s063f7.aspx) per creare la documentazione. Per abilitare questa funzionalità, aprire il file aree/HelpPage/App\_Start/HelpPageConfig.cs e rimuovere il commento la riga seguente:

[!code-csharp[Main](creating-api-help-pages/samples/sample3.cs)]

Abilitare ora la documentazione XML. In Esplora soluzioni fare clic sul progetto e selezionare **proprietà**. Selezionare il **compilazione** pagina.

![](creating-api-help-pages/_static/image6.png)

Sotto **Output**, controllare **file di documentazione XML**. Nella casella di modifica, digitare "App\_Data/XmlDocument.xml".

![](creating-api-help-pages/_static/image7.png)

Aprire quindi il codice per il `ValuesController` controller API, che viene definito in /Controllers/ValuesControler.cs. Aggiungere alcuni commenti della documentazione per i metodi del controller. Ad esempio:

[!code-csharp[Main](creating-api-help-pages/samples/sample4.cs)]

> [!NOTE]
> Suggerimento: Se si posiziona il cursore sulla riga sopra il metodo e digitare i tre barre, Visual Studio automaticamente inserisce gli elementi XML. È quindi possibile compilare i campi vuoti.


A questo punto compilare ed eseguire nuovamente l'applicazione e passare alle pagine della Guida. Stringhe di documentazione verrà visualizzato nella tabella dell'API.

![](creating-api-help-pages/_static/image8.png)

La pagina della Guida legge le stringhe dal file XML in fase di esecuzione. (Quando si distribuisce l'applicazione, assicurarsi di distribuire il file XML.)

## <a name="under-the-hood"></a>Dietro le quinte

Le pagine della Guida vengono compilate in cima il **ApiExplorer** (classe), che fa parte del framework API Web. Il **ApiExplorer** classe fornisce il materiale non elaborato per la creazione di una pagina della Guida. Per ogni API **ApiExplorer** contiene un' **ApiDescription** che descrive l'API. A tale scopo, un' "API" è definita come la combinazione di URI relativo e il metodo HTTP. Ad esempio, ecco alcune API distinte:

- OTTENERE /api/Products
- GET/API/prodotti / {id}
- POST/api/prodotti

Se un'azione del controller supporta più metodi HTTP, il **ApiExplorer** considera ogni metodo come un'API distinta.

Per nascondere un'API dei **ApiExplorer**, aggiungere il **ApiExplorerSettings** attributo per l'azione e il set *IgnoreApi* su true.

[!code-csharp[Main](creating-api-help-pages/samples/sample5.cs)]

È anche possibile aggiungere questo attributo nel controller, escludere l'intero controller.

La classe di ApiExplorer Ottiene le stringhe di documentazione dal **IDocumentationProvider** interfaccia. Come illustrato in precedenza, la libreria di pagine della Guida fornisce un' **IDocumentationProvider** che ottiene la documentazione da stringhe di documentazione XML. Il codice è disponibile in /Areas/HelpPage/XmlDocumentationProvider.cs. È possibile ottenere la documentazione di un'altra origine scrivendo il proprio **IDocumentationProvider**. Per collegare, iscrizione, chiama il **SetDocumentationProvider** metodo di estensione, definito **HelpPageConfigurationExtensions**

**ApiExplorer** chiama automaticamente il **IDocumentationProvider** interfaccia per recuperare le stringhe di documentazione per ogni API. Li archivia nel **documentazione** proprietà delle **ApiDescription** e **ApiParameterDescription** oggetti.

## <a name="next-steps"></a>Passaggi successivi

Non è limitato alle pagine della guida illustrate di seguito. In realtà **ApiExplorer** non è limitata alla creazione di pagine della Guida. Yao Huang Lin ha scritto che alcuni eccezionali post di blog per ottenere che possono risultare utili predefiniti:

- [Aggiunta di un semplice Client di prova alla pagina della Guida ASP.NET Web API](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/02/adding-a-simple-test-client-to-asp-net-web-api-help-page.aspx)
- [Funzionamento pagina della Guida ASP.NET Web API nei servizi self-hosted](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/20/making-asp-net-web-api-help-page-work-on-self-hosted-services.aspx)
- [Generazione in fase di progettazione della Guida pagina (o client) per l'API Web ASP.NET](https://blogs.msdn.com/b/yaohuang1/archive/2013/01/20/design-time-generation-of-help-page-or-proxy-for-asp-net-web-api.aspx)
- [Personalizzazioni avanzate di pagina della Guida](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/10/asp-net-web-api-help-page-part-3-advanced-help-page-customizations.aspx)
