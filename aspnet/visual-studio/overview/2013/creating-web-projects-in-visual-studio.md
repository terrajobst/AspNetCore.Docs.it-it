---
uid: visual-studio/overview/2013/creating-web-projects-in-visual-studio
title: Creazione di progetti Web ASP.NET in Visual Studio 2013 | Microsoft Docs
author: tdykstra
description: Questo argomento illustra le opzioni per la creazione di progetti web ASP.NET in Visual Studio 2013 con Update 3 qui sono alcune delle nuove funzionalità per c di sviluppo web...
ms.author: aspnetcontent
ms.date: 12/01/2014
ms.assetid: 61941e64-0c0d-4996-9270-cb8ccfd0cabc
msc.legacyurl: /visual-studio/overview/2013/creating-web-projects-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: b57492a51f65e7ca861a7c354ded6ab170a92488
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814520"
---
<a name="creating-aspnet-web-projects-in-visual-studio-2013"></a>Creazione di progetti Web ASP.NET in Visual Studio 2013
====================
da [Tom Dykstra](https://github.com/tdykstra)

> In questo argomento illustra le opzioni per la creazione di progetti web ASP.NET in Visual Studio 2013 con Update 3
> 
> Ecco alcune delle nuove funzionalità per lo sviluppo web rispetto alle versioni precedenti di Visual Studio:
> 
> - Una semplice interfaccia utente per la creazione di progetti offerta [supporto per più framework ASP.NET](#add) (Web Form, MVC e API Web).
> - [ASP.NET Identity](#indauth), un nuovo sistema di appartenenze ASP.NET che funziona in tutti i framework di ASP.NET e funziona con software diversi da IIS di hosting web.
> - L'uso di [Bootstrap](#bootstrap) per fornire funzionalità reattive di progettazione e dei temi.
> - Nuove funzionalità per Web Form che consente di essere disponibili solo per MVC, ad esempio [la creazione del progetto di test automatici](#testproj) e un [modello di sito Intranet](#winauth).
> 
> Per informazioni su come creare progetti web per servizi Cloud di Azure o servizi mobili di Azure, vedere [Introduzione a servizi Cloud di Azure e ASP.NET](https://azure.microsoft.com/documentation/articles/cloud-services-dotnet-get-started/) e [la creazione di un'App Leaderboard con .NET di servizi mobili di Azure Back-end](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/).


<a id="prerequisites"></a>
## <a name="prerequisites"></a>Prerequisiti

Questo articolo si applica a [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) con [Update 3](https://go.microsoft.com/fwlink/?linkid=397827&amp;clcid=0x409) installato.

<a id="wap"></a>
## <a name="web-application-projects-versus-web-site-projects"></a>Progetti applicazione Web e progetti di siti web

ASP.NET consente di scegliere tra due tipi di progetti web: *progetti di applicazione web* e *progetti di siti web*. Si consiglia di progetti di applicazione web per i nuovi sviluppi e in questo articolo si applica solo ai progetti di applicazione web. Per altre informazioni, vedere [progetti di applicazioni Web e progetti di siti Web in Visual Studio](https://msdn.microsoft.com/library/dd547590(v=vs.120).aspx) nel sito MSDN.

<a id="overview"></a>
## <a name="overview-of-web-application-project-creation"></a>Panoramica della creazione del progetto dell'applicazione web

La procedura seguente illustra come creare un progetto web:

1. Fare clic su **nuovo progetto** nel **avviare** pagina o nella **File** menu.
2. Nel **nuovo progetto** finestra di dialogo, fare clic su **Web** nel riquadro sinistro e **applicazione Web ASP.NET** nel riquadro centrale.

    ![Finestra di dialogo Nuovo progetto](creating-web-projects-in-visual-studio/_static/image1.png)

    È possibile scegliere **Cloud** nel riquadro sinistro per creare un [servizio Cloud di Azure](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy), [servizi mobili di Azure](https://msdn.microsoft.com/library/windows/apps/dn629482.aspx), oppure [Azure WebJob](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-webjobs). Questo argomento non tratta tali modelli.
3. Nel riquadro di destra, scegliere il **aggiungere Application Insights al progetto** casella di controllo se si desidera che monitoraggio dell'integrità e sull'utilizzo per l'applicazione. Per altre informazioni, vedere [monitorare le prestazioni nelle applicazioni web](https://azure.microsoft.com/documentation/articles/app-insights-web-monitor-performance/).
4. Specificare il progetto **Name**, **posizione**e altre opzioni e quindi fare clic su **OK**.

    Il **nuovo progetto ASP.NET** viene visualizzata la finestra.

    ![Finestra di dialogo Nuovo progetto](creating-web-projects-in-visual-studio/_static/image2.png)
5. Fare clic su un modello.

    ![Selezionare un modello](creating-web-projects-in-visual-studio/_static/image3.png)
6. Se si desidera aggiungere il supporto per Framework aggiuntivi non inclusi nel modello, fare clic sulla casella di controllo appropriata. (Nell'esempio illustrato, è possibile aggiungere MVC e/o API Web a un progetto di Web Form.)

    ![Aggiungere i Framework](creating-web-projects-in-visual-studio/_static/image4.png)
7. <a id="testproj"></a>Se si desidera aggiungere un progetto unit test, fare clic su **aggiungere gli unit test**.

    ![Aggiungere gli unit test](creating-web-projects-in-visual-studio/_static/image5.png)
8. Se si desidera un metodo di autenticazione diverso rispetto a ciò che fornisce il modello per impostazione predefinita, fare clic su **Modifica autenticazione**.

    ![Configurare l'autenticazione pulsante](creating-web-projects-in-visual-studio/_static/image6.png)

    ![Finestra di dialogo di autenticazione Configura](creating-web-projects-in-visual-studio/_static/image7.png)

<a id="azurenewproj"></a>
### <a name="create-a-web-app-or-virtual-machine-in-azure"></a>Creare un'app web o una macchina virtuale in Azure

Visual Studio include funzionalità che rendono più semplice lavorare con i servizi Azure per l'hosting di applicazioni web. Ad esempio, è possibile eseguire le operazioni seguenti direttamente dall'IDE di Visual Studio:

- Creare e gestire le app web o macchine virtuali che rendono disponibili all'applicazione tramite Internet.
- Visualizzare i log creati dall'applicazione durante l'esecuzione nel cloud.
- Esecuzione in modalità debug, in remoto mentre l'applicazione è in esecuzione nel cloud.
- Viiew e gestire altri servizi di Azure, ad esempio i database SQL.

È possibile [creare un account di Azure](https://www.windowsazure.com/pricing/free-trial/) che include servizi di base, ad esempio App web gratuita e se sei un abbonato MSDN è possibile [attivare i benefici](https://azure.microsoft.com/pricing/member-offers/visual-studio-subscriptions/) che offrono crediti mensili per Azure aggiuntivi servizi. 

Per impostazione predefinita il **nuovo progetto ASP.NET** nella finestra di dialogo consente di creare un'app web o una macchina virtuale per un nuovo progetto web. Se non si desidera creare una nuova app web o una macchina virtuale, deselezionare il **ospita nel cloud** casella di controllo.

![Crea risorse remote](creating-web-projects-in-visual-studio/_static/image8.png)

La didascalia casella di controllo potrebbe essere **ospita nel cloud** oppure **crea risorse remote**, e in entrambi i casi l'effetto è lo stesso. Se si lascia selezionata la casella di controllo, Visual Studio crea un'app web nel servizio App di Azure per impostazione predefinita. È possibile usare la casella di riepilogo a discesa per modificarlo in un **macchina virtuale** se si preferisce. Se non è già stato effettuato l'accesso ad Azure, richiesto per le credenziali di Azure. Dopo l'accesso, una finestra di dialogo consente di configurare le risorse di Visual Studio verrà creato per il progetto. La figura seguente mostra la finestra di dialogo per un'app web. Se si sceglie di creare una macchina virtuale, verranno visualizzate opzioni diverse.

![Configurare le impostazioni dell'App di Azure](creating-web-projects-in-visual-studio/_static/image9.png)

Per altre informazioni su come usare questo processo per la creazione di risorse di Azure, vedere [Introduzione a Azure e ASP.NET](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet) e [creazione di una macchina virtuale per un sito web con Visual Studio](https://azure.microsoft.com/documentation/articles/virtual-machines-dotnet-create-visual-studio-powershell/).

Il resto di questo articolo offre altre informazioni sui modelli disponibili e le rispettive opzioni. L'articolo introduce anche framework Bootstrap, il layout e temi utilizzata nei modelli.

<a id="vs2013"></a>
## <a name="visual-studio-2013-web-project-templates"></a>Modelli di progetto Web di Visual Studio 2013

Visual Studio Usa i modelli per creare progetti web. Un modello di progetto possa creare file e cartelle nel nuovo progetto, installare i pacchetti NuGet e fornire codice di esempio per un'applicazione funzionante rudimentali. I modelli di implementano gli standard web più recenti e illustrano le procedure consigliate per informazioni su come usare tecnologie ASP.NET, nonché offrono un jump start sulla creazione di un'applicazione personalizzata.

Visual Studio 2013 fornisce le seguenti opzioni per i modelli di progetto web per i progetti destinati a .NET 4.5 o versioni successive di .NET framework:

- [Modello vuoto](#empty)
- [Modello di Web Form](#wf)
- [Modello MVC](#mvc)
- [Modello API Web](#webapi)
- [Modello di applicazione a pagina singola](#spa)
- [Modello di servizio per dispositivi mobili di Azure](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/)
- [Modelli di Visual Studio 2012](#vs2012)

È anche possibile installare un'estensione di Visual Studio che fornisce una [modello Facebook](#facebook).

Per informazioni su come creare progetti destinati a .NET 4, vedere [modelli di Visual Studio 2012](#vs2012) più avanti in questo argomento.

Per informazioni su come creare applicazioni ASP.NET per i client per dispositivi mobili, vedere [supporto per dispositivi mobili in ASP.NET](../../../mobile/index.md).

<a id="empty"></a>
### <a name="empty-template"></a>Modello vuoto

Il modello vuoto fornisce le bare minime cartelle e file per un'app web ASP.NET, ad esempio un file di progetto (*file con estensione csproj* o. *vbproj*) e una *Web. config* file. È possibile aggiungere il supporto per Web Form, MVC e/o API Web usando le caselle di controllo sotto le **aggiungere le cartelle e riferimenti per di base:** etichetta.

Per il modello vuoto nessuna opzione di autenticazione sono disponibile. La funzionalità di autenticazione viene implementata in applicazioni di esempio e il modello vuoto non crea un'applicazione di esempio.

<a id="wf"></a>
### <a name="web-forms-template"></a>Modello di Web Form

Web Forms framework fornisce le funzionalità seguenti che consentono la rapida creazione di siti web che fanno ampio set di dati e dell'interfaccia utente di accedere alle funzionalità:

- Finestra di progettazione WYSIWYG in Visual Studio.
- Controlli server che eseguono il rendering HTML e che è possibile personalizzare impostando le proprietà e gli stili.
- Un vasto assortimento di controlli per l'accesso ai dati e la visualizzazione dei dati.
- Un modello di eventi che espone gli eventi che è possibile programmare come si sarebbe programmare un'applicazione client, ad esempio WPF.
- Conservazione automatico dello stato (dati) tra le richieste HTTP.

In generale, la creazione di un'applicazione Web Form meno impegnativa programmazione rispetto alla creazione della stessa applicazione usando il framework ASP.NET MVC. Web Form, tuttavia, non è sufficiente per lo sviluppo rapido di applicazioni. Esistono più complesse applicazioni commerciali e Framework basato su Web Form.

Poiché una pagina Web Form e i controlli nella pagina generano automaticamente gran parte del markup che viene inviato al browser, non è il tipo di controllo con granularità fine sul codice HTML che offre di ASP.NET MVC. Il modello dichiarativo per la configurazione delle pagine e controlli di riduce al minimo la quantità di codice è necessario scrivere, ma consente di nascondere alcuni dei comportamenti di HTML e HTTP. Ad esempio, non sempre possibile specificare esattamente i tag potrebbero essere generati da un controllo.

Il framework di Web Form non si presta la stessa facilità come ASP.NET MVC per le procedure di sviluppo basato su modelli, ad esempio [test-driven development](http://en.wikipedia.org/wiki/Test-driven_development), [la separazione dei compiti](http://en.wikipedia.org/wiki/Separation_of_concerns), [inversione di controllo](http://en.wikipedia.org/wiki/Inversion_of_control), e [inserimento delle dipendenze](http://en.wikipedia.org/wiki/Dependency_injection). Se si desidera scrivere codice factored in questo modo, è possibile; non si tratta solo come automatico perché è il framework ASP.NET MVC. Il [ASP.NET Web Forms MVP](http://webformsmvp.com/) progetto illustra un approccio che facilita la separazione dei compiti e testabilità, mantenendo il rapido sviluppo di Web Form è stato progettato per offrire. Microsoft SharePoint è basata su Web Form MVP.

Il modello Web Form consente di creare un'applicazione Web Form di esempio che usa [Bootstrap](#bootstrap) per fornire funzionalità reattive di progettazione e dei temi. Nella figura seguente mostra la home page.

![Home page di Web Form del modello app](creating-web-projects-in-visual-studio/_static/image10.png)

Per altre informazioni sui Web Form, vedere [Web Form ASP.NET](https://asp.net/web-forms). Per informazioni sulle operazioni eseguite automaticamente il modello Web Form, vedere [compilazione di un'applicazione Web form base usando Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/12/19/building-a-basic-web-forms-application-using-visual-studio-2013.aspx).

<a id="mvc"></a>
### <a name="mvc-template"></a>Modello MVC

ASP.NET MVC è stato progettato per semplificare le procedure di sviluppo basato su modelli, ad esempio [test-driven development](http://en.wikipedia.org/wiki/Test-driven_development), [la separazione dei compiti](http://en.wikipedia.org/wiki/Separation_of_concerns), [inversione del controllo](http://en.wikipedia.org/wiki/Inversion_of_control), e [inserimento delle dipendenze](http://en.wikipedia.org/wiki/Dependency_injection). Il framework incoraggia che separa il livello di logica di business di un'applicazione web dal relativo livello di presentazione. Suddividendo l'applicazione in modelli (M), le viste (V) e controller (C), ASP.NET MVC può rendere più facile da gestire la complessità in applicazioni di grandi dimensioni.

Con ASP.NET MVC, si lavora direttamente con HTML e HTTP rispetto a in Web Form. Ad esempio, Web Form possono automaticamente mantiene lo stato tra le richieste HTTP, ma è necessario il codice in modo esplicito in MVC. Il vantaggio del modello MVC è che consente di assumere il controllo completo su esattamente ciò che l'applicazione sta facendo e sul relativo comportamento nell'ambiente di web. Lo svantaggio è che è necessario scrivere altro codice.

MVC è stato progettato per essere estendibile, fornendo agli sviluppatori di risparmio energia consente di personalizzare il framework per le esigenze delle applicazioni. Inoltre, il codice sorgente di ASP.NET MVC è disponibile con una licenza OSI.

Il modello MVC crea un'applicazione MVC 5 di esempio che usa [Bootstrap](#bootstrap) per fornire funzionalità reattive di progettazione e dei temi. Nella figura seguente mostra la home page.

![Applicazione di esempio MVC](creating-web-projects-in-visual-studio/_static/image11.png)

Per altre informazioni su MVC, vedere [ASP.NET MVC](https://asp.net/mvc). Per informazioni su come selezionare il modello MVC 4, vedere [modelli di Visual Studio 2012](#vs2012) più avanti in questo articolo.

<a id="webapi"></a>
### <a name="web-api-template"></a>Modello API Web

Il modello API Web crea un servizio web di esempio basato su API Web, incluse le pagine della Guida API basate su MVC.

API Web ASP.NET è un framework che rende più semplice compilare servizi HTTP che soddisfano una vasta gamma di client, inclusi browser e dispositivi mobili. API Web ASP.NET è una piattaforma ideale per creare servizi RESTful in .NET Framework.

Il modello API Web crea un servizio web di esempio. Le illustrazioni seguenti mostrano le pagine della Guida di esempio.

![Pagina della Guida di API Web](creating-web-projects-in-visual-studio/_static/image12.png)

![Pagina della Guida di API Web per l'API GET](creating-web-projects-in-visual-studio/_static/image13.png)

Per altre informazioni sull'API Web, vedere [API Web ASP.NET](https://asp.net/web-api).

<a id="spa"></a>
### <a name="single-page-application-template"></a>Modello di applicazione singola pagina

Il modello di applicazione a pagina singola (SPA) crea un'applicazione di esempio che usa JavaScript, HTML 5, e [Knockout. js](http://knockoutjs.com/) sul client e API Web ASP.NET nel server.

È l'unica opzione di autenticazione per il modello SPA [singoli account utente di](#indauth).

La figura seguente mostra lo stato iniziale dell'applicazione di esempio che consente di creare il modello di applicazione a singola pagina.

![Applicazione di esempio di applicazione a singola pagina](creating-web-projects-in-visual-studio/_static/image14.png)

Per informazioni su come creare un'applicazione usando il modello di applicazione a singola pagina, vedere [API Web - servizi di autenticazione esterni](../../../web-api/overview/security/external-authentication-services.md).

Per altre informazioni sulle applicazioni a pagina singola ASP.NET e su altri modelli di applicazione a singola pagina che usano i framework JavaScript diverse da Knockout. js, vedere le risorse seguenti:

- [Applicazione di pagina singola ASP.NET](../../../single-page-application/index.md).
- [Informazioni sulle funzionalità di sicurezza nel modello di applicazione a singola pagina per VS2013 RC](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx)
- [Applicazioni a singola pagina: Crea App Web moderne e reattive con ASP.NET](https://msdn.microsoft.com/magazine/dn463786.aspx)

<a id="facebook"></a>
### <a name="facebook-template"></a>Modello Facebook

È possibile installare una [estensione di Visual Studio che fornisce un modello Facebook](https://go.microsoft.com/fwlink/?LinkID=509965&amp;clcid=0x409). Questo modello crea un'applicazione di esempio che è progettata per l'esecuzione all'interno del sito web Facebook. Si basa su ASP.NET MVC e Usa l'API Web per la funzionalità di aggiornamento in tempo reale.

Nessuna opzione di autenticazione sono disponibile per il modello Facebook perché le applicazioni Facebook eseguite all'interno del sito di Facebook e si basano sull'autenticazione di Facebook.

Per altre informazioni sulle applicazioni Facebook di ASP.NET, vedere [l'aggiornamento dell'API di Facebook MVC](https://blogs.msdn.com/b/webdev/archive/2014/06/10/updating-the-mvc-facebook-api.aspx).

<a id="vs2012"></a>
### <a name="visual-studio-2012-templates"></a>Modelli di Visual Studio 2012

La finestra di dialogo Creazione di progetti web di Visual Studio 2013 fornisce l'accesso per alcuni modelli che non erano disponibili in Visual Studio 2012. Se si desidera usare uno di questi modelli, è possibile scegliere il nodo Visual Studio 2012 nel riquadro sinistro della finestra di dialogo Nuovo progetto di Visual Studio.

![Modelli di Visual Studio 2012](creating-web-projects-in-visual-studio/_static/image15.png)

Il **Visual Studio 2012** consente di nodo, scegliere i modelli web seguenti che non si devono di accesso per l'elenco predefinito di modelli per Visual Studio 2013:

- Applicazione Web ASP.NET MVC 4
- Applicazione Web entità ASP.NET Dynamic Data
- Controllo Server AJAX ASP.NET
- Estensione di controllo Server AJAX ASP.NET
- Controllo Server ASP.NET

<a id="bootstrap"></a>
## <a name="bootstrap-in-the-visual-studio-2013-web-project-templates"></a>Eseguire il bootstrap nei modelli di progetto web Visual Studio 2013

Usano i modelli di progetto di Visual Studio 2013 [Bootstrap](http://getbootstrap.com/), un framework di layout e temi creato da Twitter. Bootstrap Usa CSS3 per fornire progettazione reattiva, ovvero i layout possono adattarsi dinamicamente alle dimensioni di finestra diversa del browser. Ad esempio, in una finestra del browser di grandi la home page ha creato dal modello di Web Form è simile al seguente:

![Home page di Web Form del modello app](creating-web-projects-in-visual-studio/_static/image16.png)

Rendere più stretta la finestra e spostano le colonne disposte in orizzontale nella disposizione verticale:

![Disposizione di bootstrap colonna verticale](creating-web-projects-in-visual-studio/_static/image17.png)

Limitare un po' più la finestra e nel menu in alto orizzontale si trasforma in un'icona che è possibile fare clic per espandere in un menu orientato verticalmente:

![Icona del menu bootstrap](creating-web-projects-in-visual-studio/_static/image18.png)

![Menu verticale bootstrap](creating-web-projects-in-visual-studio/_static/image19.png)

È possibile usare anche funzionalità temi del Bootstrap per rendere facilmente effettiva una modifica nell'aspetto dell'applicazione. Ad esempio, è possibile eseguire la procedura seguente per modificare il tema.

1. Nel browser, passare a [ http://Bootswatch.com ](http://Bootswatch.com), scegliere un tema e quindi fare clic su **scaricare**. (Verrà scaricato *bootstrap.min.css* per impostazione predefinita; se si desidera esaminare il codice CSS, ottenere *bootstrap* invece della versione minimizzata.)
2. Copiare il contenuto del file CSS scaricato.
3. In Visual Studio, creare una nuova **foglio di stile** file denominato *bootstrap-theme. CSS* nel *contenuto* cartella e incollare il CSS scaricato codice al suo interno.
4. Aprire *App\_Start/Bundle.config* e modificare *bootstrap* alla *bootstrap-theme. CSS*.

Eseguire nuovamente il progetto e l'applicazione ha un nuovo aspetto. La figura seguente mostra l'effetto del tema Amelia:

![Tema bootstrap Amelia](creating-web-projects-in-visual-studio/_static/image20.png)

Sono disponibili numerosi temi Bootstrap versioni gratuite e premium. Bootstrap offre anche un'ampia gamma di componenti dell'interfaccia utente, ad esempio [elenchi a discesa](http://twitter.github.io/bootstrap/components.html#dropdowns), [pulsante gruppi](http://twitter.github.io/bootstrap/components.html#buttonGroups), e [icone](http://twitter.github.io/bootstrap/base-css.html#images). Per altre informazioni, vedere [sito di Bootstrap](http://twitter.github.io/bootstrap/).

Se si usa la finestra di progettazione di Web Form in Visual Studio, si noti che la finestra di progettazione non supporta CSS3, pertanto non visualizza tutti gli effetti di Bootstrap dei temi o modifiche di layout reattivo in modo accurato. Tuttavia, le pagine Web Form visualizzerà correttamente quando viene visualizzato con un browser.

<a id="add"></a>
## <a name="adding-support-for-additional-frameworks"></a>Aggiunta del supporto per Framework aggiuntivi

Quando si seleziona un modello, viene selezionata automaticamente la casella di controllo per il Framework usato dal modello. Ad esempio, se si seleziona il **Web Form** modello, il **Web Form** casella di controllo è selezionata e non è possibile deselezionare.

![Selezionare un modello](creating-web-projects-in-visual-studio/_static/image21.png)

![Aggiungere i Framework](creating-web-projects-in-visual-studio/_static/image22.png)

È possibile selezionare la casella di controllo per un framework che non è incluso nel modello per aggiungere il supporto per tale framework quando viene creato il progetto. Ad esempio, per consentire l'uso di Web Form *. aspx* pagine dopo aver selezionato il modello MVC, selezionare la **Web Form** casella di controllo. O per abilitare MVC quando si usa il modello Web Form, fare clic sui **MVC** casella di controllo. Aggiunta di un framework Abilita il supporto in fase di progettazione, nonché in fase di esecuzione. Ad esempio, se si aggiunge il supporto MVC per un progetto di Web Form, sarà possibile eseguire lo scaffolding di controller e visualizzazioni.

Se si combinano in un progetto Web Form e MVC e abilitare [URL brevi](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx) in Web Form, potrebbero esserci problemi di routing in cui un solo URL ha più destinazioni possibili imprevisto. Le route definite prima avrà la precedenza. Ad esempio, se si dispone di un `Home` controller e un *ridefinisce* pagina, il `http://contoso.com/home` URL verrà inviata a *ridefinisce* se si chiama il `EnableFriendlyUrls` metodo prima di chiamare il `MapRoute`nel metodo *RouteConfig.cs*, o lo stesso URL verrà inviata a una visualizzazione predefinita per il `Home` controller se si chiama `MapRoute` prima `EnableFriendlyUrls`.

Aggiunta di un framework non aggiunge alcuna funzionalità di applicazione di esempio. Ad esempio, se si aggiungono Web Form supporta dopo aver selezionato il modello MVC, non *default. aspx* file della home page viene creato. Vengono aggiunti solo le cartelle, file e i riferimenti richiesti per supportare il framework. Aggiunta di Framework per questo motivo, non cambia le opzioni di autenticazione, che vengono implementate dal codice nelle applicazioni di esempio creati mediante i modelli. Ad esempio, se si seleziona il modello vuoto e aggiungere Web Form o MVC supporta, il **configurare l'autenticazione** pulsante sarà comunque disabilitato.

Le sezioni seguenti descrivono brevemente l'effetto di ogni casella di controllo.

### <a name="add-web-forms-support"></a>Aggiungere il supporto di Web Form

Crea vuoto *App\_Data* e *modelli* cartelle e una *Global. asax* file. Questi sono già stati creati tramite modelli di tutto diverso da un modello vuoto, quindi selezionando la casella di controllo Web Form non è rilevante per gli altri modelli.

Il modello Web Form Abilita gli URL brevi per impostazione predefinita, ma quando si aggiungono che i moduli Web supportano ad altri modelli selezionando la casella di controllo Web Form che non viene abilitati automaticamente gli URL brevi.

### <a name="add-mvc-support"></a>Aggiungere il supporto MVC

Installa i pacchetti WebPages NuGet MVC e Razor, Crea vuoto *App\_Data*, *controller*, *modelli*, e *viste*cartelle e crea *App\_avviare* cartella con *RouteConfig.cs* file e crea *Global. asax* file.

### <a name="add-web-api-support"></a>Aggiungere il supporto di API Web

Installa i pacchetti NuGet newtonsoft. JSON e API Web, Crea vuoto *App\_Data*, *controller*, e *modelli* cartelle, crea  *App\_avviare* cartella con *WebApiConfig.cs* del file e crea *Global. asax* file.

<a id="auth"></a>
## <a name="authentication-methods"></a>Metodi di autenticazione

Visual Studio 2013 offre diverse opzioni di autenticazione per i modelli Web Form, MVC e API Web:

- [Nessuna autenticazione](#noauth)
- [Account utente individuali](#indauth) (ASP.NET Identity, precedentemente noto come sistema di appartenenze ASP.NET)
- [Gli account aziendali](#orgauth) (Windows Server Active Directory o Azure Active Directory)
- [L'autenticazione di Windows](#winauth) (Intranet)

![Finestra di dialogo di autenticazione Configura](creating-web-projects-in-visual-studio/_static/image23.png)

<a id="noauth"></a>

### <a name="no-authentication"></a>Nessuna autenticazione

Se si seleziona **Nessuna autenticazione**, l'applicazione di esempio non conterrà alcuna pagina web per l'accesso, non dell'interfaccia utente che indica chi è collegato, delle classi dell'entità per un database di appartenenza e nessuna stringa di connessione per un database di appartenenza.

<a id="indauth"></a>
### <a name="individual-user-accounts"></a>Account utente individuali

Se si seleziona **singoli account utente di**, l'applicazione di esempio verrà configurata per usare ASP.NET Identity (precedentemente noto come sistema di appartenenze ASP.NET) per l'autenticazione utente. ASP.NET Identity consente a un utente registrare un account, creando un nome utente e password nel sito o eseguendo l'accesso con provider social come Facebook, Google, Account Microsoft o Twitter. Archivio dati predefinito per i profili utente in ASP.NET Identity è un database LocalDB di SQL Server, che è possibile distribuire a SQL Server o Database SQL di Azure per il sito di produzione.

In Visual Studio 2013 queste funzionalità sono le stesse di Visual Studio 2012, ma è stato riscritto il codice sottostante per il sistema di appartenenze ASP.NET. Vantaggi del nuovo codice di base seguenti:

- Si basa il nuovo sistema di appartenenze [OWIN](http://owin.org/) anziché il modulo di autenticazione basata su form ASP.NET. Ciò significa che è possibile usare lo stesso meccanismo di autenticazione se si usa Web Form o MVC in IIS o se si sta self-hosting SignalR o API Web.
- Il nuovo database di appartenenza è gestito da Code First di Entity Framework, e tutte le tabelle sono rappresentate dalle classi di entità che è possibile modificare. Ciò significa che è possibile personalizzare facilmente lo schema di database e web relativi al profilo dell'interfaccia utente in base alle proprie esigenze e distribuire con facilità gli aggiornamenti con migrazioni Code First.

Il nuovo sistema di appartenenza viene implementato automaticamente in nuovi modelli e può essere implementata manualmente in qualsiasi progetto destinato a .NET 4.5 o versione successiva.

ASP.NET Identity è un'ottima scelta se si sta creando un sito web di Internet che è principalmente destinata a clienti esterni. Se l'organizzazione Usa Active Directory o Office 365 e si vuole creare un progetto che abilita single sign-on per i dipendenti e partner commerciali, il **gli account aziendali** opzione potrebbe essere una scelta migliore.

Per altre informazioni sull'opzione di singoli account utente, vedere le risorse seguenti:

- [www.asp.net/identity](../../../identity/index.md). Documentazione sull'identità di ASP.NET nel sito web ASP.NET.
- [Creare un'App ASP.NET MVC 5 con Facebook e Google OAuth2 Sign-on OpenID](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md). Viene anche illustrato come personalizzare i dati del profilo utente.
- [Web API - servizi di autenticazione esterni](../../../web-api/overview/security/external-authentication-services.md)
- [Aggiunta di account di accesso esterni all'applicazione ASP.NET in Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/06/27/adding-external-logins-to-your-asp-net-application-in-visual-studio-2013.aspx)

<a id="orgauth"></a>
### <a name="organizational-accounts"></a>Account aziendali

Se si seleziona **gli account aziendali**, l'applicazione di esempio verrà configurata per l'utilizzo di Windows Identity Foundation (WIF) per l'autenticazione basata su account utente in Azure Active Directory (Azure AD, che include Office 365) o Windows Server Active Directory. Per altre informazioni, vedere [opzioni di autenticazione di account dell'organizzazione](#orgauthoptions) più avanti in questo argomento.

<a id="winauth"></a>
### <a name="windows-authentication"></a>Autenticazione di Windows

Se si seleziona **Windows autenticazione**, l'applicazione di esempio verrà configurata per usare il modulo IIS di autenticazione di Windows per l'autenticazione. L'applicazione visualizzerà l'ID utente e dominio di Active directory o account del computer locale che viene registrato in Windows, ma non includono la registrazione utente o di log dell'interfaccia utente. Questa opzione è destinata a siti web Intranet.

In alternativa, è possibile creare un sito Intranet che utilizza l'autenticazione di Active Directory, scegliere il [gli account aziendali per l'opzione On-Premises](#orgauthonprem). L'opzione On-Premises Usa Windows Identity Foundation (WIF) anziché il modulo di autenticazione di Windows. Sono necessari alcuni passaggi aggiuntivi per configurare l'opzione On-Premises, ma Windows Identity Foundation Abilita le funzionalità che non sono disponibili con il modulo di autenticazione di Windows. Con WIF, ad esempio, è possibile configurare l'accesso alle applicazioni in Active Directory ed eseguire query sui dati di directory.

<a id="orgauthoptions"></a>
## <a name="organizational-account-authentication-options"></a>Opzioni di autenticazione di account dell'organizzazione

Il **configurare l'autenticazione** dialogo offre diverse opzioni per l'autenticazione di account di Windows Server Active Directory (AD) o Azure Active Directory (Azure AD, che include Office 365):

- [Cloud - organizzazione singola](#orgauthsingle) (Azure Active Directory o Active Directory tramite l'integrazione di directory con Azure AD)
- [Cloud - organizzazione con più](#orgauthmulti) (Azure Active Directory o Active Directory tramite l'integrazione di directory con Azure AD)
- [On-Premises](#orgauthonprem) (AD)

Se si vuole provare una delle opzioni di Azure AD ma non ha ancora un account [fare clic qui per effettuare l'iscrizione per un account Azure AD](https://go.microsoft.com/fwlink/?LinkId=309942).

> [!NOTE]
> Se si sceglie una delle opzioni di Azure AD, il progetto richiede un database e si devono eseguire l'accesso a un account amministratore globale per il tenant di Azure AD. Immettere il nome e la password per un account aziendale (ad esempio, admin@contoso.onmicrosoft.com) che dispone di autorizzazioni amministrative per il tenant di Azure AD.
> 
> **Non immettere le credenziali per un account Microsoft (ad esempio, contoso@hotmail.com) nella finestra di dialogo di accesso.**


<a id="orgauthsingle"></a>
### <a name="cloud---single-organization-authentication"></a>Cloud - organizzazione singola autenticazione

![Autenticazione di sola organizzazione](creating-web-projects-in-visual-studio/_static/image24.png)

Scegliere questa opzione se si desidera abilitare l'autenticazione per gli account utente definiti in un'istanza di Azure AD [tenant](https://technet.microsoft.com/library/jj573650.aspx). Ad esempio, il sito è contoso.com e verrà essere reso disponibile ai dipendenti della società Contoso che sono in tenant contoso.onmicrosoft.com. Sarà possibile configurare Azure AD per consentire agli utenti di altri tenant di accedere all'applicazione.

#### <a name="domain"></a>Dominio

Immettere il dominio di Azure AD che si desidera configurare l'applicazione, ad esempio: `contoso.onmicrosoft.com`. Se si dispone di un [dominio personalizzato](http://www.cloudidentity.com/blog/2013/04/14/adding-a-custom-domain-to-your-windows-azure-ad/), ad esempio `contoso.com` invece di `contoso.onmicrosoft.com`, è possibile immettere qui.

#### <a name="access-level"></a>Livello di accesso

Se l'applicazione deve eseguire una query o aggiornare le informazioni della directory usando l'API Graph, scegliere **Single Sign-On, lettura dati Directory** oppure **Single Sign-On, lettura e scrittura dati Directory**. In caso contrario, scegliere **Single Sign-On**. Per altre informazioni, vedere [livelli di accesso dell'applicazione](https://msdn.microsoft.com/library/windowsazure/b08d91fa-6a64-4deb-92f4-f5857add9ed8#BKMK_AccessLevels) e [usando l'API Graph per eseguire Query AD Azure](https://msdn.microsoft.com/library/windowsazure/dn151791.aspx).

#### <a name="application-id-uri"></a>URI ID applicazione

Per impostazione predefinita, il modello crea una URI ID applicazione per l'utente aggiungendo il nome del progetto al dominio di Azure AD. Ad esempio, se è il nome del progetto `Example` e il dominio è `contoso.onmicrosoft.com`, l'applicazione diventa URI dell'ID `https://contoso.onmicrosoft.com/Example`. Se si desidera manualmente specificare l'URI ID dell'applicazione, espandere la **altre opzioni** sezione e immettere l'URI ID dell'applicazione nella casella di testo. L'applicazione URI dell'ID deve iniziare con `https://`.

Per impostazione predefinita, se un'applicazione che è già effettuato il provisioning di Azure AD ha la stessa applicazione URI dell'ID di quello che Visual Studio viene utilizzato per il progetto, il progetto verrà connessa all'applicazione esistente anziché con il provisioning di una nuova. Se si desidera una nuova applicazione da sottoporre a provisioning in questo caso, deselezionare il **Sovrascrivi la voce dell'applicazione se ne esiste già uno con lo stesso ID** casella di controllo.

Se il **Sovrascrivi** casella di controllo è deselezionata e Visual Studio consente di trovare un'applicazione esistente con l'URI ID dell'applicazione stessa, viene creato un nuovo URI aggiungendo un numero all'URI avrebbe da utilizzare. Ad esempio, si supponga che sia il nome del progetto `Example`, si lascia vuota la casella di testo, si deseleziona il **Sovrascrivi** casella di controllo e il tenant di Azure AD dispone già di un'applicazione con l'URI `https://contoso.onmicrosoft.com/Example`. In tal caso, sia verrà eseguito il provisioning di una nuova applicazione con un'applicazione, ad esempio URI di ID `https://contoso.onmicrosoft.com/Example_20130619330903`.

#### <a name="provisioning-the-application-in-azure-ad"></a>Provisioning dell'applicazione in Azure AD

Per il provisioning dell'applicazione in Azure AD o connettere il progetto per un'applicazione esistente, Visual Studio richiede le credenziali di amministratore globale per il dominio. Quando fa clic su **OK** nel **configurare l'autenticazione** nella finestra di dialogo viene chiesto il nome utente e la password di amministratore globale per il dominio specificato. Successivamente, quando si fa clic su **Create Project** nel **nuovo progetto ASP.NET** finestra di dialogo, Visual Studio esegue il provisioning all'applicazione in Azure AD. Si noti che durante questo processo di Visual Studio incorpora i valori del segreto client nel file Web. config che scade un anno dopo la creazione.

Per informazioni su come creare applicazioni che usano **Cloud - organizzazione singola** l'autenticazione, vedere le risorse seguenti:

- [Autenticazione di Azure](../2012/windows-azure-authentication.md)
- [Aggiungere Sign-On all'applicazione Web usando Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151790.aspx)
- [Sviluppo di app ASP.NET con Azure Active Directory](../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md)
- [Proteggere l'API Web ASP.NET con Azure AD e i componenti Microsoft OWIN](https://msdn.microsoft.com/magazine/dn463788.aspx)

Le esercitazioni non sono ancora state aggiornate per Visual Studio 2013; alcuni dei quali le esercitazioni indirizzano gli utenti a eseguire l'operazione manualmente è automatizzato in Visual Studio 2013.

<a id="orgauthmulti"></a>
### <a name="cloud---multi-organization-authentication"></a>Cloud - l'autenticazione a più organizzazione

![Più autenticazioni dell'organizzazione](creating-web-projects-in-visual-studio/_static/image25.png)

Scegliere questa opzione se si desidera abilitare l'autenticazione per gli account utente che sono definiti in Azure AD più [tenant](https://technet.microsoft.com/library/jj573650.aspx). Ad esempio, il sito è contoso.com e saranno disponibili per i dipendenti della società Contoso che sono in tenant contoso.onmicrosoft.com e dipendenti della società Fabrikam che appartengono al tenant fabrikam.onmicrosoft.com.

Le impostazioni immesse e l'applicazione di passaggio di provisioning sono simili a [autenticazione singola organizzazione](#orgauthsingle).

Per informazioni su come creare applicazioni che usano **Cloud - organizzazione Multi** l'autenticazione, vedere le risorse seguenti:

- [Facile integrazione di App Web con Azure Active Directory e ASP.NET &amp; Visual Studio](https://blogs.msdn.com/b/active_directory_team_blog/archive/2013/06/26/improved-windows-azure-active-directory-integration-with-asp-net-amp-visual-studio.aspx) nel blog del Team di Active Directory.
- [Sviluppo di applicazioni Web multi-Tenant con Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151789.aspx) esercitazione. L'esercitazione non è ancora stata aggiornata per Visual Studio 2013; alcuni dei quali l'esercitazione seguendo le istruzioni in eseguire manualmente è automatizzato in Visual Studio 2013.
- [È necessario iscriversi con la propria App di ASP.NET più organizzazioni prima di poter accedere](http://www.cloudidentity.com/blog/2013/10/26/you-have-to-sign-up-with-your-own-multiple-organizations-asp-net-app-before-you-can-sign-in/). Blog di Vittorio Bertocci che spiega come risolvere una persone problema comune verificarsi quando si crea un progetto che utilizza l'autenticazione a più organizzazioni.

<a id="orgauthonprem"></a>
### <a name="on-premises-organizational-authentication"></a>Autenticazione aziendale locale

![Autenticazione azienda locale](creating-web-projects-in-visual-studio/_static/image26.png)

Scegliere questa opzione se si desidera abilitare l'autenticazione per gli account utente definiti in Windows Server Active Directory (AD) e non si vuole usare Azure AD. È possibile usare questa opzione per creare un sito Intranet o un sito Internet. Per un sito Internet, usare Active Directory Federation Services (ADFS) per fornire l'accesso ad AD. Per altre informazioni, vedere [usare l'opzione di autenticazione aziendale On-Premises (ADFS) con ASP.NET in Visual Studio 2013](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/).

Per un sito Intranet, in alternativa è possibile scegliere [Windows autenticazione](#winauth) anziché questa opzione. Per l'opzione autenticazione di Windows non è necessario fornire un URL del documento di metadati. Tuttavia, l'autenticazione di Windows non offrono la possibilità di per controllare l'accesso dell'applicazione in Active Directory o eseguire query sui dati di directory.

#### <a name="on-premises-authority"></a>Autorità locale

Immettere un URL che punta al documento di metadati. Il documento di metadati contiene le coordinate dell'autorità. L'applicazione userà queste coordinate per il flusso di sign-on web.

#### <a name="application-id-uri"></a>URI ID applicazione

Specificare un URI univoco che possa essere usato per identificare questa applicazione, o lasciare vuoto per consentire a Visual Studio di crearne uno AD.

<a id="nextsteps"></a>
## <a name="next-steps"></a>Passaggi successivi

Questo documento fornisce alcune informazioni di base per la creazione di un nuovo progetto web ASP.NET in Visual Studio 2013. Per altre informazioni sull'utilizzo per Visual Studio per lo sviluppo web, vedere [ https://www.asp.net/visual-studio/ ](../../index.md).
