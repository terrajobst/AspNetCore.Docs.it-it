---
uid: web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
title: Creazione di pagine della Guida per ASP.NET Web API | Documenti Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2013
ms.topic: article
ms.assetid: 0150e67b-c50d-4613-83ea-7b4ef8cacc5a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
msc.type: authoredcontent
ms.openlocfilehash: 37fd26ebaea192cb540c443eff8a07343ab8c15b
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/08/2018
ms.locfileid: "28037903"
---
<a name="creating-help-pages-for-aspnet-web-api"></a>Creazione di pagine della Guida per ASP.NET Web API
====================
da [Mike Wasson](https://github.com/MikeWasson)

Quando si crea un'API web, è spesso utile creare una pagina della Guida, in modo che altri sviluppatori saranno in grado di chiamare l'API. È possibile creare tutta la documentazione manualmente, ma è preferibile per la generazione automatica il più possibile.

Per semplificare questa attività, ASP.NET Web API fornisce una libreria per la generazione automatica di pagine della Guida in fase di esecuzione.

![](creating-api-help-pages/_static/image1.png)

## <a name="creating-api-help-pages"></a>Creazione di pagine della Guida di API

Installare [Web ASP.NET e strumenti di aggiornamento 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650). Questo aggiornamento si integra il modello di progetto API Web pagine della Guida.

Successivamente, creare un nuovo progetto ASP.NET MVC 4 e selezionare il modello di progetto API Web. Il modello di progetto crea un controller di API di esempio denominato `ValuesController`. Il modello crea anche le pagine della Guida di API. Tutti i file di codice per la pagina della Guida vengono inseriti nella cartella di aree del progetto.

![](creating-api-help-pages/_static/image2.png)

Quando si esegue l'applicazione, la home page contiene un collegamento alla pagina della Guida di API. Dalla home page, il percorso relativo è /Help.

![](creating-api-help-pages/_static/image3.png)

Questo collegamento consente di visualizzare una pagina di riepilogo di API.

![](creating-api-help-pages/_static/image4.png)

La visualizzazione MVC questa pagina è definita in Areas/HelpPage/Views/Help/Index.cshtml. È possibile modificare questa pagina per modificare il layout, introduzione, titolo, stili e così via.

La parte principale della pagina è una tabella delle API, raggruppate dal controller. Le voci della tabella vengono generate in modo dinamico, utilizzando il **IApiExplorer** interfaccia. (Più avanti parlerò su questa interfaccia in un secondo momento.) Se si aggiunge un nuovo controller API, la tabella viene aggiornata automaticamente in fase di esecuzione.

La colonna "API" Elenca l'URI relativo e il metodo HTTP. La colonna "Descrizione" contiene la documentazione per ogni API. La documentazione è inizialmente, solo il testo segnaposto. Nella sezione successiva verrà illustrato aggiungere documentazione da commenti XML.

Ogni API dispone di un collegamento a una pagina con informazioni più dettagliate, tra cui i testi di richiesta e risposta di esempio.

![](creating-api-help-pages/_static/image5.png)

## <a name="adding-help-pages-to-an-existing-project"></a>Aggiunta di pagine della Guida per un progetto esistente

È possibile aggiungere le pagine della Guida per un progetto di API Web esistente tramite Gestione pacchetti NuGet. Questa opzione è utile che iniziare da un modello di progetto diverso rispetto al modello "API Web".

Dal **strumenti** dal menu **Gestione pacchetti libreria**, quindi selezionare **Package Manager Console**. Nel [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) finestra, digitare uno dei seguenti comandi:

Per un **c#** applicazione: `Install-Package Microsoft.AspNet.WebApi.HelpPage`

Per un **Visual Basic** applicazione: `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`

Esistono due pacchetti, uno per c# e uno per Visual Basic. Assicurarsi di usare quello che corrisponde al progetto.

Questo comando Installa gli assembly necessari e aggiunge le visualizzazioni MVC per le pagine della Guida (che si trova nella cartella aree/HelpPage). È necessario aggiungere manualmente un collegamento alla pagina della Guida. L'URI è /Help. Per creare un collegamento in una visualizzazione razor, aggiungere quanto segue:

[!code-cshtml[Main](creating-api-help-pages/samples/sample1.cshtml)]

Assicurarsi inoltre che registrare le aree. Nel file Global. asax, aggiungere il codice seguente per il **applicazione\_avviare** metodo, se non esiste già:

[!code-csharp[Main](creating-api-help-pages/samples/sample2.cs?highlight=4)]

## <a name="adding-api-documentation"></a>Aggiunta di documentazione dell'API

Per impostazione predefinita, la Guida pagine sono stringhe di segnaposto per la documentazione. È possibile utilizzare [commenti della documentazione XML](https://msdn.microsoft.com/library/b2s063f7.aspx) per creare la documentazione. Per abilitare questa funzionalità, aprire il file HelpPage/aree/App\_Start/HelpPageConfig.cs e rimuovere il commento la riga seguente:

[!code-csharp[Main](creating-api-help-pages/samples/sample3.cs)]

Ora la documentazione XML. In Esplora soluzioni, fare clic sul progetto e selezionare **proprietà**. Selezionare il **compilare** pagina.

![](creating-api-help-pages/_static/image6.png)

In **Output**, controllare **file di documentazione XML**. Nella casella di modifica, digitare "App\_Data/XmlDocument.xml".

![](creating-api-help-pages/_static/image7.png)

Aprire quindi il codice per il `ValuesController` controller API che è definito in /Controllers/ValuesControler.cs. Aggiungere alcuni commenti di documentazione per i metodi del controller. Ad esempio:

[!code-csharp[Main](creating-api-help-pages/samples/sample4.cs)]

> [!NOTE]
> Suggerimento: Se si posiziona il punto di inserimento nella riga sopra il metodo e digitare tre barre, Visual Studio automaticamente inserisce gli elementi XML. È quindi possibile inserire gli spazi vuoti.


Ora di compilazione ed eseguire nuovamente l'applicazione e individuare le pagine della Guida. Le stringhe di documentazione dovrebbero essere visualizzato nella tabella API.

![](creating-api-help-pages/_static/image8.png)

La pagina della Guida legge le stringhe dal file XML in fase di esecuzione. (Quando si distribuisce l'applicazione, assicurarsi di distribuire il file XML.)

## <a name="under-the-hood"></a>Dietro le quinte

Le pagine della Guida si basano sul **ApiExplorer** (classe), che fa parte del framework Web API. Il **ApiExplorer** classe fornisce le materie prime per la creazione di una pagina della Guida. Per ogni API, **ApiExplorer** contiene un **ApiDescription** che descrive l'API. A tale scopo, un'API"" è definita come la combinazione di URI relativo e il metodo HTTP. Ad esempio, ecco alcune API distinti:

- OTTENERE /api/Products
- OTTENERE /api/prodotti / {id}
- Registra/api/prodotti

Se un'azione del controller supporta più metodi HTTP, il **ApiExplorer** considera ogni metodo di un'API distinta.

Per nascondere un'API del **ApiExplorer**, aggiungere il **ApiExplorerSettings** attributo per l'azione e set *IgnoreApi* su true.

[!code-csharp[Main](creating-api-help-pages/samples/sample5.cs)]

È anche possibile aggiungere questo attributo nel controller, escludere il controller di intero.

La classe ApiExplorer Ottiene le stringhe di documentazione dal **IDocumentationProvider** interfaccia. Come illustrato in precedenza, la libreria di pagine della Guida fornisce un **IDocumentationProvider** che ottiene documentazione da stringhe di documentazione XML. Il codice si trova in /Areas/HelpPage/XmlDocumentationProvider.cs. È possibile ottenere la documentazione di un'altra origine scrivendo proprio **IDocumentationProvider**. Per associare lo, chiamare il **SetDocumentationProvider** metodo di estensione, definito **HelpPageConfigurationExtensions**

**ApiExplorer** automaticamente richiamano il **IDocumentationProvider** interfaccia da ottenere stringhe della documentazione per ogni API. Li archivia nel **documentazione** proprietà del **ApiDescription** e **ApiParameterDescription** oggetti.

## <a name="next-steps"></a>Passaggi successivi

Non è per le pagine della guida indicate di seguito. In effetti, **ApiExplorer** non è limitata alla creazione di pagine della Guida. Yao Huang Lin ha scritto che alcuni grande post di blog per iniziare a pensare all'esterno della casella:

- [Aggiunta di un semplice Client di Test alla pagina della Guida di ASP.NET Web API](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/02/adding-a-simple-test-client-to-asp-net-web-api-help-page.aspx)
- [Rendendo pagina della Guida ASP.NET Web API lavorando servizi self-hosted](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/20/making-asp-net-web-api-help-page-work-on-self-hosted-services.aspx)
- [Fase di generazione della Guida pagina (o client) per ASP.NET Web API](https://blogs.msdn.com/b/yaohuang1/archive/2013/01/20/design-time-generation-of-help-page-or-proxy-for-asp-net-web-api.aspx)
- [Personalizzazioni avanzate della pagina della Guida](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/10/asp-net-web-api-help-page-part-3-advanced-help-page-customizations.aspx)
