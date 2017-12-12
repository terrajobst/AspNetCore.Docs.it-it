---
uid: visual-studio/overview/2013/creating-web-projects-in-visual-studio
title: Creazione di progetti Web ASP.NET in Visual Studio 2013 | Documenti Microsoft
author: tdykstra
description: "In questo argomento illustra le opzioni per la creazione di progetti web ASP.NET in Visual Studio 2013 con Update 3 seguito sono riportate alcune delle nuove funzionalità per c di sviluppo web..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/01/2014
ms.topic: article
ms.assetid: 61941e64-0c0d-4996-9270-cb8ccfd0cabc
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/creating-web-projects-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: 96960ef56b1206374458dbbba4befffaa83c1624
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="creating-aspnet-web-projects-in-visual-studio-2013"></a>La creazione di progetti Web ASP.NET in Visual Studio 2013
====================
Da [Tom Dykstra](https://github.com/tdykstra)

> In questo argomento vengono descritte le opzioni per la creazione di progetti web ASP.NET in Visual Studio 2013 con Update 3
> 
> Di seguito sono riportate alcune delle nuove funzionalità per lo sviluppo web rispetto alle versioni precedenti di Visual Studio:
> 
> - Una semplice interfaccia utente per la creazione di progetti dell'offerta [il supporto per più framework ASP.NET](#add) (Web Form, MVC e Web API).
> - [ASP.NET Identity](#indauth), un nuovo sistema di appartenenze ASP.NET che funziona allo stesso in tutti i framework ASP.NET e funziona con software diversi da IIS di hosting web.
> - L'utilizzo di [Bootstrap](#bootstrap) per fornire funzionalità di progettazione e i temi reattiva.
> - Nuove funzionalità per i moduli Web che consentono di essere forniti solo per MVC, ad esempio [la creazione del progetto di test automatico](#testproj) e [il modello di sito Intranet](#winauth).
> 
> Per informazioni su come creare progetti web per servizi Cloud di Azure o i servizi mobili di Azure, vedere [Introduzione a servizi Cloud di Azure e ASP.NET](https://azure.microsoft.com/en-us/documentation/articles/cloud-services-dotnet-get-started/) e [creando un'App Leaderboard con .NET di servizi mobili di Azure Back-end](https://azure.microsoft.com/en-us/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/).


<a id="prerequisites"></a>
## <a name="prerequisites"></a>Prerequisiti

In questo articolo si applica a [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) con [Update 3](https://go.microsoft.com/fwlink/?linkid=397827&amp;clcid=0x409) installato.

<a id="wap"></a>
## <a name="web-application-projects-versus-web-site-projects"></a>Progetti applicazione Web e progetti di siti web

ASP.NET consente di scegliere tra due tipi di progetti web: *progetti applicazione web* e *progetti di sito web*. Si consiglia di progetti di applicazione web per un nuovo sviluppo, e questo articolo si applica solo ai progetti di applicazione web. Per ulteriori informazioni, vedere [progetti applicazione Web e progetti di sito Web in Visual Studio](https://msdn.microsoft.com/en-us/library/dd547590(v=vs.120).aspx) nel sito MSDN.

<a id="overview"></a>
## <a name="overview-of-web-application-project-creation"></a>Panoramica della creazione del progetto applicazione web

La procedura seguente viene illustrato come creare un progetto web:

1. Fare clic su **nuovo progetto** nel **avviare** pagina o il **File** menu.
2. Nel **nuovo progetto** finestra di dialogo, fare clic su **Web** nel riquadro a sinistra e **applicazione Web ASP.NET** nel riquadro centrale.

    ![Finestra di dialogo Nuovo progetto](creating-web-projects-in-visual-studio/_static/image1.png)

    È possibile scegliere **Cloud** nel riquadro a sinistra per creare un [servizio Cloud di Azure](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy), [servizio Mobile di Azure](https://msdn.microsoft.com/en-us/library/windows/apps/dn629482.aspx), o [processo Web di Azure](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-deploy-webjobs). In questo argomento non copre tali modelli.
3. Nel riquadro di destra, fare clic su di **Aggiungi Application Insights al progetto** casella di controllo se si desidera integrità e il monitoraggio dell'utilizzo per l'applicazione. Per ulteriori informazioni, vedere [monitorare le prestazioni in applicazioni web](https://azure.microsoft.com/en-us/documentation/articles/app-insights-web-monitor-performance/).
4. Specificare progetto **nome**, **percorso**e altre opzioni e quindi fare clic su **OK**.

    Il **nuovo progetto ASP.NET** viene visualizzata la finestra.

    ![Finestra di dialogo Nuovo progetto](creating-web-projects-in-visual-studio/_static/image2.png)
5. Fare clic su un modello.

    ![Selezionare un modello](creating-web-projects-in-visual-studio/_static/image3.png)
6. Se si desidera aggiungere il supporto per Framework aggiuntive non incluse nel modello, fare clic sulla casella di controllo appropriata. (Nell'esempio illustrato, è possibile aggiungere MVC e/o API Web a un progetto di Web Form.)

    ![Aggiungere Framework](creating-web-projects-in-visual-studio/_static/image4.png)
7. <a id="testproj"></a>Se si desidera aggiungere un progetto di unit test, fare clic su **aggiungere unit test**.

    ![Aggiungere gli unit test](creating-web-projects-in-visual-studio/_static/image5.png)
8. Se si desidera un metodo di autenticazione diverso da quello fornite dal modello per impostazione predefinita, fare clic su **Modifica autenticazione**.

    ![Configurare l'autenticazione pulsante](creating-web-projects-in-visual-studio/_static/image6.png)

    ![Configurare una finestra di dialogo autenticazione](creating-web-projects-in-visual-studio/_static/image7.png)

<a id="azurenewproj"></a>
### <a name="create-a-web-app-or-virtual-machine-in-azure"></a>Creare un'app web o una macchina virtuale in Azure

Visual Studio include funzionalità che rendono più facile lavorare con i servizi di Azure per ospitare applicazioni web. Ad esempio, è possibile eseguire le seguenti direttamente dall'IDE di Visual Studio:

- Creare e gestire le app web o macchine virtuali che rendono disponibili l'applicazione tramite Internet.
- Visualizzare i log creati dall'applicazione durante l'esecuzione nel cloud.
- Esecuzione in modalità debug, in remoto mentre l'applicazione è in esecuzione nel cloud.
- Viiew e gestire altri servizi, ad esempio i database SQL di Azure.

È possibile [creare un account Azure](https://www.windowsazure.com/en-us/pricing/free-trial/) che include servizi di base, ad esempio le app web gratuito e se si è abbonati a MSDN possono [attivare i vantaggi](https://azure.microsoft.com/pricing/member-offers/visual-studio-subscriptions/) che consentono di crediti mensili per Azure aggiuntive servizi. 

Per impostazione predefinita il **nuovo progetto ASP.NET** la finestra di dialogo consente di creare un'app web o una macchina virtuale per un nuovo progetto web. Se non si desidera creare una nuova app web o una macchina virtuale, deselezionare il **Host nel cloud** casella di controllo.

![Crea risorse remote](creating-web-projects-in-visual-studio/_static/image8.png)

La didascalia della casella di controllo potrebbe essere **Host nel cloud** o **crea risorse remote**, e in entrambi i casi l'effetto è lo stesso. Se si lascia selezionata la casella di controllo, Visual Studio crea un'app web in Azure App Service per impostazione predefinita. È possibile utilizzare la casella di riepilogo a discesa per modificarlo in un **macchina virtuale** se si preferisce. Se sta non è già connesso a Azure, richiesto per le credenziali di Azure. Dopo l'accesso, una finestra di dialogo consente di configurare le risorse di Visual Studio verranno creati per il progetto. La figura seguente mostra la finestra di dialogo per un'app web; Se si sceglie di creare una macchina virtuale, verranno visualizzate opzioni diverse.

![Configurare le impostazioni dell'App di Azure](creating-web-projects-in-visual-studio/_static/image9.png)

Per ulteriori informazioni sull'utilizzo di questo processo per la creazione di risorse di Azure, vedere [Introduzione a Azure e ASP.NET](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet) e [la creazione di una macchina virtuale per un sito web con Visual Studio](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-dotnet-create-visual-studio-powershell/).

Il resto di questo articolo vengono fornite ulteriori informazioni sui modelli disponibili e le rispettive opzioni. L'articolo introduce anche il framework Bootstrap, il layout e l'applicazione di temi utilizzato nei modelli.

<a id="vs2013"></a>
## <a name="visual-studio-2013-web-project-templates"></a>Modelli di progetto Web di Visual Studio 2013

Visual Studio utilizza modelli per creare progetti web. Un modello di progetto possa creare file e cartelle nel nuovo progetto, installare i pacchetti NuGet e fornire codice di esempio per un'applicazione funzionante rudimentali. Modelli di implementano gli standard web più recenti e servono a illustrare le procedure consigliate sull'utilizzo delle tecnologie ASP.NET, nonché consentono un salto avviare sulla creazione di un'applicazione personalizzata.

Visual Studio 2013 offre le seguenti opzioni per i modelli di progetto web per i progetti destinati a .NET 4.5 o versioni successive di .NET framework:

- [Modello vuoto](#empty)
- [Modello di Web Form](#wf)
- [Modello MVC](#mvc)
- [Modello API Web](#webapi)
- [Modello di applicazione a pagina singola](#spa)
- [Modello di servizio Mobile di Azure](https://azure.microsoft.com/en-us/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/)
- [Modelli di Visual Studio 2012](#vs2012)

È inoltre possibile installare un'estensione di Visual Studio che fornisce un [modello Facebook](#facebook).

Per informazioni su come creare progetti destinati a .NET 4, vedere [modelli di Visual Studio 2012](#vs2012) più avanti in questo argomento.

Per informazioni su come creare applicazioni ASP.NET per client mobili, vedere [supporto Mobile in ASP.NET](../../../mobile/index.md).

<a id="empty"></a>
### <a name="empty-template"></a>Modello vuoto

Il modello vuoto fornisce le cartelle di minime bare e un file per un'app web ASP.NET, ad esempio un file di progetto (*csproj* o. *vbproj*) e un *Web. config* file. È possibile aggiungere il supporto per Web Form, MVC e/o API Web utilizzando le caselle di controllo sotto la **aggiungere cartelle e i riferimenti per core:** etichetta.

Per il modello vuoto opzioni di autenticazione non sono disponibili. La funzionalità di autenticazione viene implementata in applicazioni di esempio e il modello vuoto non ne crea un'applicazione di esempio.

<a id="wf"></a>
### <a name="web-forms-template"></a>Modello di Web Form

Il framework fornisce le funzionalità seguenti che consentono di effettuare rapidamente creare siti web che ampio set di dati e dell'interfaccia utente Web Form di accedere alle funzionalità:

- Finestra di progettazione WYSIWYG in Visual Studio.
- Controlli server che eseguono il rendering di HTML e che è possibile personalizzare impostando le proprietà e gli stili.
- Un'ampia gamma di controlli per l'accesso ai dati e visualizzazione dei dati.
- Un modello di eventi che espone gli eventi che è possibile programmare come verrebbe programmare un'applicazione client, ad esempio WPF.
- Conservazione automatico dello stato (dati) tra le richieste HTTP.

In generale, la creazione di un'applicazione Web Form richiede meno lavoro di programmazione rispetto alla creazione della stessa applicazione tramite il framework di MVC ASP.NET. Web Form, tuttavia, non è sufficiente per lo sviluppo rapido di applicazioni. Esistono molte applicazioni commerciali complesse e Framework basato su Web Form.

Poiché una pagina Web Form e i controlli nella pagina di generano automaticamente la maggior parte del codice che viene inviato al browser, non è il tipo di un controllo accurato il codice HTML che offre ASP.NET MVC. Il modello dichiarativo per la configurazione delle pagine e controlli di riduce al minimo la quantità di codice, è necessario scrivere ma nasconde alcuni dei comportamenti di HTML e HTTP. Ad esempio, non è sempre possibile specificare esattamente i tag potrebbero essere generati da un controllo.

Il framework di Web Form non portano facilità ASP.NET MVC consigliate per lo sviluppo basato su modelli, ad esempio [sviluppo basato su test](http://en.wikipedia.org/wiki/Test-driven_development), [separazione delle problematiche](http://en.wikipedia.org/wiki/Separation_of_concerns), [inversione di controllo](http://en.wikipedia.org/wiki/Inversion_of_control), e [inserimento di dipendenze](http://en.wikipedia.org/wiki/Dependency_injection). Se si desidera scrivere codice prenderle in considerazione in questo modo, è possibile; non è semplicemente come automatica, come avviene nel framework di MVC ASP.NET. Il [ASP.NET Web Forms MVP](http://webformsmvp.com/) progetto Mostra un approccio che facilita la separazione delle problematiche e testabilità mantenendo lo sviluppo rapido di Web Form compilato per il recapito. Microsoft SharePoint è basata su Web Form MVP.

Il modello Web Form consente di creare un'applicazione Web Form di esempio che utilizza [Bootstrap](#bootstrap) per fornire funzionalità di progettazione e i temi reattiva. Nella figura seguente mostra la home page.

![Home page di Web Form modello app](creating-web-projects-in-visual-studio/_static/image10.png)

Per ulteriori informazioni su Web Form, vedere [Web Form ASP.NET](https://asp.net/web-forms). Per informazioni sulle operazioni eseguite automaticamente il modello Web Form, vedere [la creazione di un'applicazione Web Form di base con Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/12/19/building-a-basic-web-forms-application-using-visual-studio-2013.aspx).

<a id="mvc"></a>
### <a name="mvc-template"></a>Modello MVC

ASP.NET MVC è stato progettato per facilitare consigliate per lo sviluppo basato su modelli, ad esempio [sviluppo basato su test](http://en.wikipedia.org/wiki/Test-driven_development), [separazione delle problematiche](http://en.wikipedia.org/wiki/Separation_of_concerns), [inversione di controllo](http://en.wikipedia.org/wiki/Inversion_of_control), e [inserimento di dipendenze](http://en.wikipedia.org/wiki/Dependency_injection). Grazie a questo framework che separa il livello di logica di business di un'applicazione web dal relativo livello di presentazione. Suddividendo l'applicazione in modelli (M), le viste (V) e controller (C), ASP.NET MVC può rendere più facile da gestire la complessità in applicazioni di grandi dimensioni.

Con ASP.NET MVC, si lavora direttamente con HTML e HTTP di Web Form. Ad esempio, Web Form automaticamente possibile mantenere lo stato tra le richieste HTTP, ma è necessario codice in modo esplicito che in MVC. Il vantaggio del modello MVC è che consente di eseguire il controllo completo esattamente cosa fa l'applicazione e al comportamento nell'ambiente di web. Lo svantaggio è che è necessario scrivere codice più.

MVC è stato progettato per essere estensibile, consentendo agli sviluppatori di power per personalizzare il framework per le esigenze dell'applicazione. Inoltre, il codice sorgente ASP.NET MVC è disponibile una licenza OSI.

Il modello MVC consente di creare un'applicazione MVC 5 di esempio che utilizza [Bootstrap](#bootstrap) per fornire funzionalità di progettazione e i temi reattiva. Nella figura seguente mostra la home page.

![Applicazione di esempio MVC](creating-web-projects-in-visual-studio/_static/image11.png)

Per ulteriori informazioni su MVC, vedere [ASP.NET MVC](https://asp.net/mvc). Per informazioni su come selezionare il modello MVC 4, vedere [modelli di Visual Studio 2012](#vs2012) più avanti in questo articolo.

<a id="webapi"></a>
### <a name="web-api-template"></a>Modello API Web

Il modello API Web crea un servizio web di esempio in base alle API Web, incluse le pagine della Guida API basate su MVC.

API Web ASP.NET è un framework che rende più semplice compilare servizi HTTP in grado di raggiungono una vasta gamma di client, inclusi browser e dispositivi mobili. API Web ASP.NET è una piattaforma ideale per la compilazione di servizi RESTful in .NET Framework.

Il modello API Web crea un servizio web di esempio. Nelle illustrazioni seguenti pagine della Guida di esempio.

![Pagina della Guida Web API](creating-web-projects-in-visual-studio/_static/image12.png)

![Pagina della Guida di API Web per ottenere l'API](creating-web-projects-in-visual-studio/_static/image13.png)

Per ulteriori informazioni sulle API Web, vedere [ASP.NET Web API](https://asp.net/web-api).

<a id="spa"></a>
### <a name="single-page-application-template"></a>Single Page Application Template

Il modello di applicazione a pagina singola (SPA) crea un'applicazione di esempio che utilizza JavaScript, HTML 5, e [Knockout.js](http://knockoutjs.com/) sul client e ASP.NET Web API nel server.

È l'unica opzione di autenticazione per il modello SPA [singoli account utente di](#indauth).

Nella figura seguente mostra lo stato iniziale dell'applicazione di esempio che consente di creare il modello SPA.

![Applicazione di esempio SPA](creating-web-projects-in-visual-studio/_static/image14.png)

Per informazioni su come creare un'applicazione utilizzando il modello SPA, vedere [API Web, servizi di autenticazione esterni](../../../web-api/overview/security/external-authentication-services.md).

Per ulteriori informazioni sulle applicazioni a pagina singola ASP.NET e sui modelli SPA aggiuntivi che utilizzano i framework JavaScript di diverso da Knockout.js, vedere le risorse seguenti:

- [ASP.NET singola pagina applicazione](../../../single-page-application/index.md).
- [Informazioni sulle funzionalità di sicurezza nel modello di SPA per VS2013 RC](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx)
- [Applicazioni a pagina singola: Compilazione di applicazioni Web moderne e reattive con ASP.NET](https://msdn.microsoft.com/en-us/magazine/dn463786.aspx)

<a id="facebook"></a>
### <a name="facebook-template"></a>Modello Facebook

È possibile installare un [estensione di Visual Studio che fornisce un modello Facebook](https://go.microsoft.com/fwlink/?LinkID=509965&amp;clcid=0x409). Questo modello consente di creare un'applicazione di esempio è progettata per l'esecuzione all'interno del sito web di Facebook. È basato su ASP.NET MVC e utilizza l'API Web per le funzionalità di aggiornamento in tempo reale.

Le opzioni di autenticazione non sono disponibili per il modello Facebook poiché Facebook applicazioni eseguiti all'interno del sito di Facebook e si basano sull'autenticazione di Facebook.

Per ulteriori informazioni sulle applicazioni Facebook ASP.NET, vedere [l'aggiornamento dell'API di Facebook MVC](https://blogs.msdn.com/b/webdev/archive/2014/06/10/updating-the-mvc-facebook-api.aspx).

<a id="vs2012"></a>
### <a name="visual-studio-2012-templates"></a>Modelli di Visual Studio 2012

Finestra di dialogo Creazione progetto di Visual Studio 2013 web non fornisce l'accesso per alcuni modelli che non erano disponibili in Visual Studio 2012. Se si desidera utilizzare uno di questi modelli, è possibile fare clic sul nodo di Visual Studio 2012 nel riquadro sinistro della finestra di dialogo Nuovo progetto di Visual Studio.

![Modelli di Visual Studio 2012](creating-web-projects-in-visual-studio/_static/image15.png)

Il **Visual Studio 2012** consente di nodo, scegliere i modelli web seguenti non si dispone di accesso per l'elenco predefinito di modelli per Visual Studio 2013:

- Applicazione Web ASP.NET MVC 4
- Applicazione Web entità ASP.NET Dynamic Data
- Controllo Server AJAX ASP.NET
- Estensione di controllo Server AJAX ASP.NET
- Controllo Server ASP.NET

<a id="bootstrap"></a>
## <a name="bootstrap-in-the-visual-studio-2013-web-project-templates"></a>Bootstrap nei modelli di progetto web di Visual Studio 2013

Utilizzano i modelli di progetto di Visual Studio 2013 [Bootstrap](http://getbootstrap.com/), un framework di layout e i temi creato da Twitter. Bootstrap utilizza CSS3 per fornire una progettazione reattiva, ovvero layout può adattarsi dinamicamente alle dimensioni della finestra del browser diverse. Ad esempio, in una finestra del browser wide nella home page creata dal modello Web Form è simile alla figura seguente:

![Home page di Web Form modello app](creating-web-projects-in-visual-studio/_static/image16.png)

Rendere più stretta la finestra e spostano le colonne disposti orizzontalmente in una disposizione verticale:

![Disposizione di bootstrap colonna verticale](creating-web-projects-in-visual-studio/_static/image17.png)

Limitare un po' più la finestra e menu in alto orizzontale si trasforma in un'icona che è possibile fare clic per espandere in un menu orientato verticalmente:

![Icona del menu di avvio](creating-web-projects-in-visual-studio/_static/image18.png)

![Menu verticale bootstrap](creating-web-projects-in-visual-studio/_static/image19.png)

Inoltre, è possibile utilizzare funzionalità dei temi del Bootstrap facilmente attuare una modifica nell'aspetto dell'applicazione. Ad esempio, è possibile eseguire i passaggi seguenti per modificare il tema.

1. Nel browser, passare a [http://Bootswatch.com](http://Bootswatch.com), scegliere un tema e quindi fare clic su **scaricare**. (In questo modo verranno *bootstrap.min.css* per impostazione predefinita; se si desidera esaminare il codice CSS, ottenere *bootstrap.css* anziché la versione minimizzata.)
2. Copiare il contenuto del file CSS scaricato.
3. In Visual Studio, creare un nuovo **foglio di stile** file denominato *bootstrap theme.css* nel *contenuto* cartella e Incolla il codice scaricato CSS codice al suo interno.
4. Aprire *App\_Start/Bundle.config* e modificare *bootstrap.css* a *bootstrap theme.css*.

Eseguire nuovamente il progetto e l'applicazione ha un aspetto nuovo. Nella figura seguente mostra l'effetto del tema Amelia:

![Tema Amelia bootstrap](creating-web-projects-in-visual-studio/_static/image20.png)

Sono disponibili numerosi temi Bootstrap versioni gratuite e premium. Programma di avvio offre anche un'ampia gamma di componenti dell'interfaccia utente, ad esempio [elenchi a discesa](http://twitter.github.io/bootstrap/components.html#dropdowns), [pulsante gruppi](http://twitter.github.io/bootstrap/components.html#buttonGroups), e [icone](http://twitter.github.io/bootstrap/base-css.html#images). Per ulteriori informazioni sull'avvio, vedere [il Bootstrap sito](http://twitter.github.io/bootstrap/).

Se si utilizza la finestra di progettazione Web Form in Visual Studio, si noti che la finestra di progettazione non supporta CSS3, pertanto non in modo accurato Visualizza tutti gli effetti di Bootstrap temi o modifiche di layout reattivo. Tuttavia, le pagine Web Form verranno visualizzato correttamente con un browser.

<a id="add"></a>
## <a name="adding-support-for-additional-frameworks"></a>Aggiunta del supporto per altri Framework

Quando si seleziona un modello, viene selezionata automaticamente la casella di controllo di framework(s) utilizzato dal modello. Ad esempio, se si seleziona il **Web Form** modello, il **Web Form** casella di controllo è selezionata e che è possibile deselezionarla.

![Selezionare un modello](creating-web-projects-in-visual-studio/_static/image21.png)

![Aggiungere Framework](creating-web-projects-in-visual-studio/_static/image22.png)

È possibile selezionare la casella di controllo per un framework che non è incluso nel modello per aggiungere il supporto per tale framework quando viene creato il progetto. Ad esempio, per consentire l'utilizzo di Web Form *aspx* pagine dopo aver selezionato il modello MVC, selezionare il **Web Form** casella di controllo. Oppure per abilitare MVC quando si utilizza il modello Web Form, fare clic su di **MVC** casella di controllo. Aggiunta di un framework Abilita il supporto in fase di progettazione, nonché in fase di esecuzione. Ad esempio, se si aggiunge il supporto MVC per un progetto di Web Form, sarà possibile eseguire lo scaffolding di controller e visualizzazioni.

Se si combinano i Web Form e MVC in un progetto e abilitare [URL brevi](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx) in Web Form, potrebbe esserci problemi di routing in cui un URL ha più possibili destinazioni imprevisto. Le route definite innanzitutto avrà la precedenza. Ad esempio, se dispone di un `Home` controller e un *aspx* pagina il `http://contoso.com/home` URL verrà inviata a *aspx* se si chiama il `EnableFriendlyUrls` metodo prima di chiamare il `MapRoute`metodo *RouteConfig.cs*, o lo stesso URL passerà alla visualizzazione predefinita per il `Home` controller se si chiama `MapRoute` prima `EnableFriendlyUrls`.

Aggiunta di un framework non aggiunge alcuna funzionalità di applicazione di esempio. Ad esempio, se si aggiungono Web Form supporta dopo aver selezionato il modello MVC, non *Default.aspx* file della home page viene creato. Vengono aggiunti solo le cartelle, file e i riferimenti necessari per supportare il framework. Aggiunta di Framework per questo motivo, non modifica le opzioni di autenticazione, che vengono implementate dal codice in applicazioni di esempio creati mediante i modelli. Ad esempio, se si seleziona il modello vuoto e aggiungere Web Form o MVC supporta, il **configurare l'autenticazione** ancora pulsante sarà disabilitato.

Nelle sezioni seguenti vengono descritti brevemente l'effetto di ogni casella di controllo.

### <a name="add-web-forms-support"></a>Aggiungere il supporto di Web Form

Crea vuoto *App\_dati* e *modelli* cartelle e un *Global. asax* file. Vengono già creati da tutti i modelli diverso dal modello vuoto, quindi selezionare la casella di controllo Web Form non rilevante per i modelli.

Il modello Web Form Abilita gli URL brevi per impostazione predefinita, ma quando si aggiungono che i moduli Web supportano ad altri modelli selezionando la casella di controllo Web Form che non vengono abilitati automaticamente gli URL brevi.

### <a name="add-mvc-support"></a>Aggiungere il supporto MVC

Installa i pacchetti WebPages NuGet MVC e Razor, Crea vuoto *App\_dati*, *controller*, *modelli*, e *viste*cartelle, crea *App\_avviare* cartella con *RouteConfig.cs* file e crea *Global. asax* file.

### <a name="add-web-api-support"></a>Aggiungere il supporto di API Web

Installa i pacchetti WebApi e NuGet newtonsoft. JSON, Crea vuoto *App\_dati*, *controller*, e *modelli* cartelle, crea  *App\_avviare* cartella con *WebApiConfig.cs* file e crea *Global. asax* file.

<a id="auth"></a>
## <a name="authentication-methods"></a>Metodi di autenticazione

Visual Studio 2013 offre diverse opzioni di autenticazione per i modelli Web Form, MVC e Web API:

- [Nessuna autenticazione](#noauth)
- [Account utente](#indauth) (ASP.NET Identity, precedentemente noto come sistema di appartenenze ASP.NET)
- [Gli account aziendali](#orgauth) (Windows Server Active Directory o Azure Active Directory)
- [L'autenticazione di Windows](#winauth) (Intranet)

![Configurare una finestra di dialogo autenticazione](creating-web-projects-in-visual-studio/_static/image23.png)

<a id="noauth"></a>

### <a name="no-authentication"></a>Nessuna autenticazione

Se si seleziona **Nessuna autenticazione**, l'applicazione di esempio non conterrà nessuna pagina web per l'accesso, non dell'interfaccia utente che indica che è connesso, non le classi di entità per un database di appartenenza e nessuna stringa di connessione per un database di appartenenza.

<a id="indauth"></a>
### <a name="individual-user-accounts"></a>Singoli account utente

Se si seleziona **singoli account utente di**, l'applicazione di esempio verrà configurato per l'utilizzo di ASP.NET Identity (precedentemente noto come sistema di appartenenze ASP.NET) per l'autenticazione utente. ASP.NET Identity consente a un utente registrare un account, creando un nome utente e password nel sito o eseguendo l'accesso con provider social come Facebook, Google, Account Microsoft o Twitter. L'archivio di dati predefinito per i profili utente in ASP.NET Identity è un database LocalDB di SQL Server, che è possibile distribuire a SQL Server o Database SQL di Azure per il sito di produzione.

In Visual Studio 2013 queste funzionalità sono le stesse Visual Studio 2012, ma il codice sottostante per il sistema di appartenenze ASP.NET è stato riscritto. Vantaggi della nuova base di codice sono i seguenti:

- Il nuovo sistema di appartenenza si basa sul [OWIN](http://owin.org/) anziché il modulo di autenticazione basata su form ASP.NET. Ciò significa che è possibile utilizzare lo stesso meccanismo di autenticazione se si usa Web Form o MVC in IIS o si sta hosting automatico di API Web o SignalR.
- Il nuovo database delle appartenenze è gestito da Entity Framework Code First e tutte le tabelle sono rappresentate dalle classi di entità che è possibile modificare. Ciò significa che è possibile personalizzare facilmente lo schema di database e i relativi al profilo web dell'interfaccia utente in base alle proprie esigenze è possibile distribuire facilmente gli aggiornamenti utilizzando migrazioni Code First.

Il nuovo sistema di appartenenze è implementato automaticamente in nuovi modelli e può essere implementata manualmente in qualsiasi progetto destinato a .NET 4.5 o versioni successive.

Identità di ASP.NET è una buona scelta se si sta creando un sito web di Internet che è progettato principalmente per i clienti esterni. Se l'organizzazione utilizza Active Directory o Office 365 e si desidera creare un progetto che consente a single sign-on per i dipendenti e partner commerciali, il **account aziendali** opzione potrebbe essere una scelta migliore.

Per ulteriori informazioni sull'opzione di account utente, vedere le risorse seguenti:

- [www.ASP.NET/Identity](../../../identity/index.md). Documentazione su ASP.NET Identity nel sito web ASP.NET.
- [Creare un'applicazione ASP.NET MVC 5 con Facebook, Google OAuth2 e OpenID Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md). Viene illustrato come personalizzare i dati del profilo utente.
- [Web API, servizi di autenticazione esterni](../../../web-api/overview/security/external-authentication-services.md)
- [Aggiunta di account di accesso esterni all'applicazione ASP.NET in Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/06/27/adding-external-logins-to-your-asp-net-application-in-visual-studio-2013.aspx)

<a id="orgauth"></a>
### <a name="organizational-accounts"></a>Account aziendali

Se si seleziona **account aziendali**, l'applicazione di esempio verrà configurato per utilizzare Windows Identity Foundation (WIF) per l'autenticazione basata su account utente in Azure Active Directory (Azure AD, che include Office 365) o Windows Server Active Directory. Per ulteriori informazioni, vedere [le opzioni di autenticazione account aziendale](#orgauthoptions) più avanti in questo argomento.

<a id="winauth"></a>
### <a name="windows-authentication"></a>Autenticazione di Windows

Se si seleziona **l'autenticazione di Windows**, l'applicazione di esempio verrà configurato per usare il modulo IIS di autenticazione di Windows per l'autenticazione. L'applicazione verrà visualizzato l'ID utente e dominio di Active directory o account del computer locale che viene registrato in Windows, ma non includono la registrazione utente o registro nell'interfaccia utente. Questa opzione è destinata a siti web Intranet.

In alternativa, è possibile creare un sito Intranet che utilizza l'autenticazione di Active Directory scegliendo il [opzione in un account aziendale locale](#orgauthonprem). L'opzione On-Premises Usa Windows Identity Foundation (WIF) anziché il modulo di autenticazione di Windows. Sono necessari alcuni passaggi aggiuntivi per configurare l'opzione locale, ma WIF Abilita le funzionalità che non sono disponibili con il modulo di autenticazione di Windows. Con WIF, ad esempio, è possibile configurare accesso alle applicazioni nei dati di directory Active Directory e delle query.

<a id="orgauthoptions"></a>
## <a name="organizational-account-authentication-options"></a>Opzioni di autenticazione account aziendale

Il **configurare l'autenticazione** finestra di dialogo offre diverse opzioni per l'autenticazione account di Windows Server Active Directory (AD) o Azure Active Directory (Azure AD, che include Office 365):

- [Cloud - singola organizzazione](#orgauthsingle) (Azure Active Directory o Active Directory mediante l'integrazione di directory con Azure AD)
- [Cloud - più organizzazione](#orgauthmulti) (Azure Active Directory o Active Directory mediante l'integrazione di directory con Azure AD)
- [On-premise](#orgauthonprem) (AD)

Se si desidera provare una delle opzioni di Azure AD, ma ancora non dispone di un account, [fare clic qui per iscriversi a un account di Azure AD](https://go.microsoft.com/fwlink/?LinkId=309942).

> [!NOTE]
> Se si sceglie una delle opzioni di Azure AD, il progetto richiede un database ed è necessario accedere a un account amministratore globale per il tenant di Azure AD. Immettere il nome e la password per un account aziendale (ad esempio, admin@contoso.onmicrosoft.com) che dispone di autorizzazioni amministrative per il tenant di Azure AD.
> 
> **Non immettere le credenziali per un account Microsoft (ad esempio, contoso@hotmail.com) nella finestra di dialogo accesso.**


<a id="orgauthsingle"></a>
### <a name="cloud---single-organization-authentication"></a>Cloud - autenticazione unica organizzazione

![Autenticazione unica organizzazione](creating-web-projects-in-visual-studio/_static/image24.png)

Scegliere questa opzione se si desidera abilitare l'autenticazione per gli account utente definiti in Azure un AD [tenant](https://technet.microsoft.com/en-us/library/jj573650.aspx). Ad esempio, il sito è contoso.com e verrà resa disponibile per i dipendenti della società Contoso nel tenant di contoso.onmicrosoft.com. Non sarà in grado di configurare Azure AD per consentire agli utenti di altri tenant di accedere all'applicazione.

#### <a name="domain"></a>Dominio

Immettere il dominio di Azure AD che si desidera configurare l'applicazione, ad esempio: `contoso.onmicrosoft.com`. Se dispone di un [dominio personalizzato](http://www.cloudidentity.com/blog/2013/04/14/adding-a-custom-domain-to-your-windows-azure-ad/), ad esempio `contoso.com` invece di `contoso.onmicrosoft.com`, è possibile immettere di seguito.

#### <a name="access-level"></a>Livello di accesso

Se l'applicazione deve eseguire una query o aggiornare le informazioni di directory tramite l'API Graph, scegliere **Single Sign-On, lettura dati Directory** o **Single Sign-On, lettura e scrittura dati Directory**. In caso contrario, scegliere **Single Sign-On**. Per ulteriori informazioni, vedere [livelli di accesso dell'applicazione](https://msdn.microsoft.com/en-us/library/windowsazure/b08d91fa-6a64-4deb-92f4-f5857add9ed8#BKMK_AccessLevels) e [mediante l'API Graph per eseguire Query in Azure AD](https://msdn.microsoft.com/en-US/library/windowsazure/dn151791.aspx).

#### <a name="application-id-uri"></a>URI ID applicazione

Per impostazione predefinita, il modello crea una URI ID applicazione per l'utente aggiungendo il nome del progetto per il dominio di Azure AD. Ad esempio, se il nome del progetto è `Example` e il dominio è `contoso.onmicrosoft.com`, l'applicazione diventa URI ID `https://contoso.onmicrosoft.com/Example`. Se si desidera specificare manualmente l'URI ID dell'applicazione, espandere il **altre opzioni** sezione e immettere l'URI ID applicazione nella casella di testo. L'applicazione ID URI deve iniziare con `https://`.

Per impostazione predefinita, se un'applicazione che è già disponibile in Azure AD include l'URI ID applicazione stesso di quello che Visual Studio viene utilizzato per il progetto, il progetto verrà connesso all'applicazione esistente anziché uno nuovo provisioning. Se si desidera una nuova applicazione di cui effettuare il provisioning in questo caso, deselezionare il **sovrascrivere la voce dell'applicazione se esiste uno con lo stesso ID già** casella di controllo.

Se il **sovrascrittura** casella di controllo è deselezionata e Visual Studio consente di trovare un'applicazione esistente con l'URI ID applicazione stessa, viene creato un nuovo URI aggiungendo un numero all'URI era in corso da utilizzare. Ad esempio, si supponga che il nome del progetto sia `Example`, si lascia vuota la casella di testo, se si deseleziona la **sovrascrittura** casella di controllo e tenant di Azure AD dispone già di un'applicazione con l'URI `https://contoso.onmicrosoft.com/Example`. In tal caso, sia sarà eseguito il provisioning di una nuova applicazione con una URI ID applicazione `https://contoso.onmicrosoft.com/Example_20130619330903`.

#### <a name="provisioning-the-application-in-azure-ad"></a>Provisioning dell'applicazione in Azure AD

Per eseguire il provisioning di applicazione in Azure AD o connettere il progetto per un'applicazione esistente, Visual Studio deve usare le credenziali di un amministratore globale per il dominio. Quando fa clic su **OK** nel **configurare l'autenticazione** nella finestra di dialogo viene richiesto per il nome utente e la password di un amministratore globale per il dominio specificato. Successivamente, quando si fa clic su **Crea progetto** nel **nuovo progetto ASP.NET** finestra di dialogo, Visual Studio esegue il provisioning di applicazione in Azure AD. Si noti come parte di questo processo di Visual Studio incorpora i valori dei segreti client nel file Web. config che scadere un anno dopo la creazione.

Per informazioni su come creare applicazioni che utilizzano **Cloud - singola organizzazione** l'autenticazione, vedere le risorse seguenti:

- [Autenticazione di Azure](../2012/windows-azure-authentication.md)
- [Aggiungere Sign-On all'applicazione Web mediante Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151790.aspx)
- [Sviluppo di applicazioni ASP.NET con Azure Active Directory](../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md)
- [Proteggere l'API Web ASP.NET con Azure AD e componenti Microsoft OWIN](https://msdn.microsoft.com/en-us/magazine/dn463788.aspx)

Le esercitazioni non sono ancora state aggiornate per Visual Studio 2013; alcune delle quali le esercitazioni le istruzioni per eseguire manualmente automatizzato in Visual Studio 2013.

<a id="orgauthmulti"></a>
### <a name="cloud---multi-organization-authentication"></a>Cloud - l'autenticazione a più organizzazione

![Autenticazione aziendale più](creating-web-projects-in-visual-studio/_static/image25.png)

Scegliere questa opzione se si desidera abilitare l'autenticazione per gli account utente definiti in Azure AD più [tenant](https://technet.microsoft.com/en-us/library/jj573650.aspx). Ad esempio, il sito è contoso.com e verrà resa disponibile per i dipendenti della società Contoso nel tenant contoso.onmicrosoft.com e dipendenti della società Fabrikam nel tenant fabrikam.onmicrosoft.com.

Le impostazioni che immettono e il passaggio di provisioning di applicazioni sono simili a [autenticazione singola organizzazione](#orgauthsingle).

Per informazioni su come creare applicazioni che utilizzano **Cloud - più organizzazione** l'autenticazione, vedere le risorse seguenti:

- [Integrazione di App Web con Azure Active Directory, ASP.NET &amp; Visual Studio](https://blogs.msdn.com/b/active_directory_team_blog/archive/2013/06/26/improved-windows-azure-active-directory-integration-with-asp-net-amp-visual-studio.aspx) nel blog del Team di Active Directory.
- [Sviluppo di applicazioni Web di multi-Tenant con Azure AD](https://msdn.microsoft.com/en-us/library/windowsazure/dn151789.aspx) esercitazione. L'esercitazione non sono ancora stata aggiornata per Visual Studio 2013; alcuni dei quali l'esercitazione vengono fornite informazioni per eseguire manualmente automatizzato in Visual Studio 2013.
- [È necessario iscriversi con più organizzazioni ASP.NET applicazione prima di poter firmare](http://www.cloudidentity.com/blog/2013/10/26/you-have-to-sign-up-with-your-own-multiple-organizations-asp-net-app-before-you-can-sign-in/). Post di blog di Vittorio Bertocci che illustra come risolvere una persone problema comune verificarsi quando si crea un progetto che utilizza l'autenticazione a più organizzazioni.

<a id="orgauthonprem"></a>
### <a name="on-premises-organizational-authentication"></a>Autenticazione aziendale locale

![Autenticazione azienda locale](creating-web-projects-in-visual-studio/_static/image26.png)

Scegliere questa opzione se si desidera abilitare l'autenticazione per gli account utente definiti in Windows Server Active Directory (AD) e non si desidera usare Azure AD. È possibile utilizzare questa opzione per creare un sito Intranet o a un sito Internet. Per un sito Internet, utilizzare Active Directory Federation Services (ADFS) per fornire l'accesso ad Active Directory. Per ulteriori informazioni, vedere [utilizzare l'opzione di autenticazione aziendale locale (ADFS) con ASP.NET in Visual Studio 2013](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/).

Per un sito Intranet, in alternativa è possibile scegliere [l'autenticazione di Windows](#winauth) anziché questa opzione. Per l'opzione di autenticazione di Windows non è necessario specificare un URL del documento dei metadati. Tuttavia, l'autenticazione di Windows non fornisce la possibilità per controllare l'accesso dell'applicazione in Active Directory o eseguire query sui dati di directory.

#### <a name="on-premises-authority"></a>Autorità locale

Immettere un URL che punta al documento di metadati. Il documento di metadati contiene le coordinate dell'autorità. L'applicazione userà queste coordinate per il flusso di accesso web.

#### <a name="application-id-uri"></a>URI ID applicazione

Specificare un URI univoco che Active Directory consente di identificare questa applicazione, o lasciare vuoto per consentire a Visual Studio di crearne uno.

<a id="nextsteps"></a>
## <a name="next-steps"></a>Passaggi successivi

Questo documento fornisce alcune informazioni di base per la creazione di un nuovo progetto web ASP.NET in Visual Studio 2013. Per ulteriori informazioni sull'utilizzo di Visual Studio per lo sviluppo web, vedere [https://www.asp.net/visual-studio/](../../index.md).
