---
uid: web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-vb
title: Una panoramica dell'autenticazione basata su form (VB) | Documenti Microsoft
author: rick-anderson
description: In questa esercitazione verrà convertito da una semplice discussione di implementazione. in particolare, verranno esaminati l'implementazione di autenticazione basata su form. W l'applicazione web...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/14/2008
ms.topic: article
ms.assetid: 83267f7d-64d9-41ee-82cf-da91b1bf534d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-vb
msc.type: authoredcontent
ms.openlocfilehash: 6482b10a470b50a1fc6f163ee2d59682e83f5a2b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30891906"
---
<a name="an-overview-of-forms-authentication-vb"></a>Una panoramica dell'autenticazione basata su form (Visual Basic)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare codice](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/ASPNET_Security_Tutorial_02_VB.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial02_FormsAuth_vb.pdf)

> In questa esercitazione verrà convertito da una semplice discussione di implementazione. in particolare, verranno esaminati l'implementazione di autenticazione basata su form. L'applicazione web che viene avviata la creazione di questa esercitazione continuerà a essere basati su nelle esercitazioni successive, si sposta da autenticazione basata su form semplice per l'appartenenza e ruoli.
> 
> Vedere il video per altre informazioni su questo argomento: [utilizzando base autenticazione basata su form in ASP.NET](# "using-basic-forms-authentication-in-aspnet").


## <a name="introduction"></a>Introduzione

Nel [esercitazione precedente](security-basics-and-asp-net-support-vb.md) descritto le opzioni di un account utente, l'autorizzazione e autenticazione vari fornite da ASP.NET. In questa esercitazione verrà convertito da una semplice discussione di implementazione. in particolare, verranno esaminati l'implementazione di autenticazione basata su form. L'applicazione web che viene avviata la creazione di questa esercitazione continuerà a essere basati su nelle esercitazioni successive, si sposta da autenticazione basata su form semplice per l'appartenenza e ruoli.

In questa esercitazione inizia con un'analisi approfondita del flusso di lavoro di autenticazione form un argomento è modificate al momento nell'esercitazione precedente. Successivamente, verrà creato un sito Web ASP.NET tramite la quale demo i concetti di autenticazione basata su form. Passaggio successivo si configurerà il sito per utilizzare l'autenticazione basata su form, creare una pagina di accesso semplice e determinare, nel codice, se un utente è autenticato e in tal caso, il nome utente registrati con.

Comprendere i form, flusso di lavoro di autenticazione, abilitarlo in un'applicazione web e la creazione di pagine di accesso e disconnessione fondamentale tutti i passaggi nella creazione di un'applicazione ASP.NET che supporta gli account utente e autentica gli utenti tramite una pagina web. Di conseguenza - e perché queste esercitazioni si basano su uno altro, è consigliabile usare questa esercitazione in modo completo prima di passare al successivo anche se si hanno già avuto esperienza di configurazione di autenticazione basata su form in progetti precedenti.

## <a name="understanding-the-forms-authentication-workflow"></a>Il flusso di autenticazione form

Quando il runtime di ASP.NET elabora una richiesta per una risorsa ASP.NET, ad esempio una pagina ASP.NET o servizio Web ASP.NET, la richiesta genera un numero di eventi durante il ciclo di vita. Sono disponibili gli eventi generati alla fine molto iniziano e molto della richiesta, quelli generati quando la richiesta venga autenticata e autorizzato, un evento generato in caso di un'eccezione non gestita e così via. Per visualizzare un elenco completo degli eventi, vedere il [eventi dell'oggetto HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication_events.aspx).

*I moduli HTTP* sono classi gestite con un codice viene eseguito in risposta a un particolare evento del ciclo di vita di richiesta. ASP.NET viene fornito con un numero di moduli HTTP che eseguono le attività essenziali dietro le quinte. Due moduli HTTP incorporati che sono particolarmente rilevante per la discussione sono:

- **[FormsAuthenticationModule](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx)**  -autentica l'utente controllando il ticket di autenticazione form, in genere incluse nella raccolta di cookie dell'utente. Se nessun ticket di autenticazione form è presente, l'utente è anonimo.
- **[UrlAuthorizationModule](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx)**  -determina se l'utente corrente è autorizzato ad accedere all'URL richiesto. Questo modulo determina l'autorità consultando le regole di autorizzazione specificate nei file di configurazione dell'applicazione. ASP.NET include anche il [FileAuthorizationModule](https://msdn.microsoft.com/library/system.web.security.fileauthorizationmodule.aspx) che determina l'autorità consultando i file richiesti ACL.

FormsAuthenticationModule tenta di autenticare l'utente prima di UrlAuthorizationModule (e FileAuthorizationModule) in esecuzione. Se l'utente effettua la richiesta non è autorizzato ad accedere alla risorsa richiesta, il modulo di autorizzazione termina la richiesta e restituisce un [HTTP 401 non autorizzato](http://www.checkupdown.com/status/E401.html) stato. In scenari di autenticazione di Windows, lo stato HTTP 401 viene restituito al browser. Questo codice di stato fa sì che il browser richiedere all'utente le credenziali tramite una finestra di dialogo modale. Con autenticazione basata su form, tuttavia, lo stato HTTP 401 non autorizzato non viene mai inviato al browser perché FormsAuthenticationModule rileva lo stato e modifica in modo da reindirizzare invece l'utente alla pagina di accesso (tramite un [HTTP 302 reindirizzare](http://www.checkupdown.com/status/E302.html) stato).

Responsabilità della pagina di accesso consiste nel determinare se le credenziali dell'utente sono valide e, in caso affermativo, per creare un ticket di autenticazione form e reindirizzerà l'utente torna alla pagina si è tentato di visitare. Il ticket di autenticazione è incluso nelle richieste successive per le pagine nel sito Web, che FormsAuthenticationModule utilizza per identificare l'utente.


[![Il flusso di lavoro autenticazione form](an-overview-of-forms-authentication-vb/_static/image2.png)](an-overview-of-forms-authentication-vb/_static/image1.png)

**Figura 01**: il flusso di lavoro form autenticazione ([fare clic per visualizzare l'immagine ingrandita](an-overview-of-forms-authentication-vb/_static/image3.png))


### <a name="remembering-the-authentication-ticket-across-page-visits"></a>Ricordare il Ticket di autenticazione tra gli accessi di pagina

Dopo l'accesso, il ticket di autenticazione form deve essere inviato al server web per ogni richiesta in modo che l'utente rimane connesso quando esplorano il sito. Questa operazione viene in genere eseguita inserendo il ticket di autenticazione nella raccolta di cookie dell'utente. [I cookie](http://en.wikipedia.org/wiki/HTTP_cookie) sono piccoli file di testo che si trovano sul computer dell'utente e vengono trasmessi nelle intestazioni HTTP su ogni richiesta per il sito Web che ha creato il cookie. Di conseguenza, dopo il ticket di autenticazione form è stato creato e archiviato nei cookie del browser, ogni successive visite al sito invia il ticket di autenticazione insieme alla richiesta, in tal modo che identifica l'utente.

> [!NOTE]
> L'applicazione web demo utilizzato in ogni esercitazione è disponibile come download. Questa applicazione scaricabile è stata creata con Visual Web Developer 2008 destinato a .NET Framework versione 3.5. Poiché l'applicazione è destinata a .NET 3.5, il relativo file Web. config include gli elementi di configurazione aggiuntive, specifiche 3.5. In breve, se si dispone ancora di installare .NET 3.5 nel computer in uso, quindi l'applicazione web scaricabile non funzionerà senza prima rimuovere il markup specifico della 3.5 da Web. config.


Un aspetto dei cookie è la scadenza, ovvero la data e l'ora in cui il browser Elimina il cookie. Alla scadenza del cookie di autenticazione form, l'utente può non essere autenticato e pertanto anonimo. Quando un utente visita da un terminale pubblico, ridimensionarla, che i ticket di autenticazione scadrà alla chiusura del browser. Durante la visita da casa, tuttavia, dello stesso utente potrebbe essere necessario memorizzare tra i vari riavvi browser in modo che non hanno il ticket di autenticazione per accedere nuovamente ogni volta che visitano il sito. Viene spesso definito dall'utente sotto forma di un controllo checkbox memorizza dati nella pagina di accesso. Nel passaggio 3, esamineremo come implementare un controllo checkbox memorizza dati nella pagina di accesso. Nell'esercitazione seguente illustra le impostazioni di timeout ticket di autenticazione in modo dettagliato.

> [!NOTE]
> È possibile che l'agente utente utilizzato per accedere al sito Web potrebbe non supportare i cookie. In tal caso, ASP.NET può utilizzare i ticket di autenticazione form senza cookie. In questa modalità, il ticket di autenticazione è codificato nell'URL. Verranno esaminati quando vengono utilizzati i ticket di autenticazione senza cookie e come vengono creati e gestiti nella prossima esercitazione.


### <a name="the-scope-of-forms-authentication"></a>L'ambito di autenticazione basata su form

FormsAuthenticationModule viene gestita il codice parte del runtime di ASP.NET. Prima versione 7 di Microsoft [Internet Information Services (IIS)](https://www.iis.net/) server web, si è verificato un ostacolo distinto tra pipeline HTTP di IIS e della pipeline del runtime ASP.NET. In breve, in IIS 6 e versioni precedenti, FormsAuthenticationModule viene eseguita solo quando una richiesta viene delegata da IIS al runtime di ASP.NET. Per impostazione predefinita, IIS elabora contenuto statico stesso, ad esempio pagine HTML e CSS e i file di immagine - e trasferisce le richieste solo al runtime di ASP.NET quando viene richiesta una pagina con estensione aspx, asmx o ashx.

Pipeline IIS 7, tuttavia, consente di integrata di IIS e ASP.NET. Con alcune impostazioni di configurazione è possibile configurare IIS 7 per richiamare FormsAuthenticationModule per *tutti* richieste. Inoltre, con IIS 7 è possibile definire regole di autorizzazione URL per qualsiasi tipo di file. Per ulteriori informazioni, vedere [modifiche tra IIS 6 e la sicurezza di IIS7](https://www.iis.net/learn/get-started/whats-new-in-iis-7/changes-in-security-between-iis-60-and-iis-7-and-above), [di sicurezza della piattaforma Web](https://www.iis.net/learn/get-started/whats-new-in-iis-7/iis7-and-above-security-improvements), e [comprensione IIS7 URL autorizzazione](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization).

In breve, nelle versioni precedenti di IIS 7, è possibile utilizzare solo autenticazione basata su form per proteggere le risorse gestite dal runtime di ASP.NET. Analogamente, le regole di autorizzazione URL vengono applicate solo alle risorse gestite dal runtime ASP.NET. Ma con IIS 7 è possibile integrare le FormsAuthenticationModule UrlAuthorizationModule nella pipeline HTTP di IIS, in tal modo estendere questa funzionalità in tutte le richieste.

## <a name="step-1-creating-an-aspnet-website-for-this-tutorial-series"></a>Passaggio 1: Creazione di un sito Web ASP.NET per questa serie di esercitazioni

Per raggiungere un vasto pubblico, il sito Web ASP.NET da compilare in tutta la serie verrà creato con una versione gratuita di Microsoft di Visual Studio 2008, [Visual Web Developer 2008](https://www.microsoft.com/express/vwd/). L'archivio utente SqlMembershipProvider verrà implementata una [Microsoft SQL Server 2005 Express Edition](https://msdn.microsoft.com/sql/Aa336346.aspx) database. Se si utilizza Visual Studio 2005 o un'edizione diversa di Visual Studio 2008 o SQL Server, non occorre preoccuparsi: la procedura sarà quasi identica e descritte le differenze non semplice.

Prima di è possibile configurare l'autenticazione basata su form, è necessario innanzitutto un sito Web ASP.NET. Iniziare creando un nuovo file system basato su sito Web ASP.NET. A tale scopo, avviare Visual Web Developer e quindi passare al menu File e scegliere Nuovo sito Web, la finestra di dialogo Nuovo sito Web. Scegliere il modello di sito Web ASP.NET, impostare l'elenco di riepilogo a discesa percorso al File System, scegliere una cartella in cui inserire il sito web e impostare il linguaggio VB. Si creerà un nuovo sito web con una pagina aspx ASP.NET, un'App\_cartella dati e un file Web. config.

> [!NOTE]
> Visual Studio supporta due modalità di gestione dei progetti: progetti di siti Web e progetti di applicazione Web. Progetti di sito Web non dispongono di un file di progetto, mentre i progetti di applicazione Web simulare l'architettura di progetto in Visual Studio .NET 2002/2003 - includono un file di progetto e compilare codice sorgente del progetto in un singolo assembly, viene inserito nella cartella /bin. Visual Studio 2005 inizialmente solo supportati progetti di sito Web, sebbene il modello di progetto di applicazione Web è stato reintrodotto con Service Pack 1. Visual Studio 2008 offre entrambi i modelli di progetto. Visual Web Developer 2005 e 2008 edizioni, tuttavia, supportano solo progetti di siti Web. Utilizzerà il modello di progetto di sito Web. Se si utilizza un'edizione non Express e si desidera utilizzare il [modello di progetto di applicazione Web](https://msdn.microsoft.com/library/aa730880(vs.80).aspx) , invece, è possibile eseguire questa operazione, ma tenere presente che potrebbero esistere alcune discrepanze tra sullo schermo e i passaggi da eseguire e il le schermate visualizzate e alle istruzioni fornite in queste esercitazioni.


[![Creare un nuovo sistema basato su sito Web di File](an-overview-of-forms-authentication-vb/_static/image5.png)](an-overview-of-forms-authentication-vb/_static/image4.png)

**Figura 02**: creare un sito Web New File System-Based ([fare clic per visualizzare l'immagine ingrandita](an-overview-of-forms-authentication-vb/_static/image6.png))


### <a name="adding-a-master-page"></a>Aggiunta di una pagina Master

Successivamente, aggiungere una nuova pagina Master per il sito nella directory radice denominato Site. master. [Pagine master](https://msdn.microsoft.com/library/wtxbf3hh.aspx) consentono allo sviluppatore di pagina definire un modello a livello di sito che può essere applicato alle pagine ASP.NET. Il vantaggio principale delle pagine master è che l'aspetto generale del sito può essere definito in un'unica posizione, rendendo semplice aggiornare o modificare il layout del sito.


[![Aggiungere una pagina Master denominata Site. master per il sito Web](an-overview-of-forms-authentication-vb/_static/image8.png)](an-overview-of-forms-authentication-vb/_static/image7.png)

**Figura 03**: aggiungere un site. Master pagina denominata master per il sito Web ([fare clic per visualizzare l'immagine ingrandita](an-overview-of-forms-authentication-vb/_static/image9.png))


Definire il layout delle pagine a livello di sito nella pagina master. È possibile utilizzare l'area di progettazione e aggiungere qualsiasi controlli di Layout o Web, è necessario oppure è possibile aggiungere manualmente il markup manualmente nella visualizzazione origine. È strutturato il layout della pagina master per simulare il layout utilizzato my *[utilizzo dei dati in ASP.NET 2.0](../../data-access/index.md)* serie di esercitazioni (vedere la figura 4). La pagina master utilizza [fogli di stile CSS](http://www.w3schools.com/css/default.asp) per il posizionamento e stili con le impostazioni di CSS definite nel file Style.css (che è incluso nel download associato dell'esercitazione). Sebbene non sia evidente dal tag riportati di seguito, le regole CSS sono definite in modo che la navigazione &lt;div&gt;del contenuto è posizionato in modo che viene visualizzata a sinistra e ha una larghezza fissa di 200 pixel.

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample1.aspx)]

Una pagina master definisce il layout di pagina statici sia le aree che possono essere modificate tramite le pagine ASP.NET che utilizzano la pagina master. Queste aree modificabili del contenuto sono indicate dal controllo ContentPlaceHolder, che può essere visualizzato all'interno del contenuto &lt;div&gt;. La pagina master contiene un solo ContentPlaceHolder (MainContent), ma della pagina master può contenere diversi ContentPlaceHolder.

Con il markup specificato sopra, passando alla visualizzazione progettazione, viene illustrato il layout della pagina master. Tutte le pagine ASP.NET che utilizzano la pagina master avrà questo layout uniforme, con la possibilità di specificare il markup per l'area MainContent.


[![La pagina Master, quando viene visualizzato tramite la visualizzazione di progettazione](an-overview-of-forms-authentication-vb/_static/image11.png)](an-overview-of-forms-authentication-vb/_static/image10.png)

**Figura 04**: la pagina Master, quando visualizzati tramite la visualizzazione della struttura ([fare clic per visualizzare l'immagine ingrandita](an-overview-of-forms-authentication-vb/_static/image12.png))


### <a name="creating-content-pages"></a>Creazione di pagine di contenuto

A questo punto si dispone di una pagina Default.aspx nel sito, ma non utilizzare la pagina master che appena creato. Sebbene sia possibile modificare il markup dichiarativo di una pagina web per utilizzare una pagina master, se la pagina non contiene alcun contenuto ancora risulta più semplice è sufficiente eliminare la pagina e aggiungerlo al progetto, specificare la pagina master da utilizzare. Pertanto, iniziare eliminando Default.aspx dal progetto.

Successivamente, fare doppio clic sul nome del progetto in Esplora soluzioni e scegliere di aggiungere un nuovo Web Form denominato Default.aspx. Questo tempo, selezionare la casella di controllo Seleziona pagina master e scegliere la pagina master Site. master dall'elenco.


[![Aggiungere una nuova pagina default. aspx scelta selezionare una pagina Master](an-overview-of-forms-authentication-vb/_static/image14.png)](an-overview-of-forms-authentication-vb/_static/image13.png)

**Figura 05**: aggiungere un nuovo default. aspx pagina scelta per selezionare una pagina Master ([fare clic per visualizzare l'immagine ingrandita](an-overview-of-forms-authentication-vb/_static/image15.png))


[![Utilizzare la pagina Master Site. master](an-overview-of-forms-authentication-vb/_static/image17.png)](an-overview-of-forms-authentication-vb/_static/image16.png)

**Figura 06**: utilizzare la pagina Master Site. master ([fare clic per visualizzare l'immagine ingrandita](an-overview-of-forms-authentication-vb/_static/image18.png))


> [!NOTE]
> Se si utilizza il modello di progetto applicazione Web nella finestra di dialogo Aggiungi nuovo elemento non include una casella di controllo Seleziona pagina master. Al contrario, è necessario aggiungere un elemento del tipo di modulo di contenuto Web. Dopo aver scelto l'opzione di formato di contenuto Web e fare clic su Aggiungi, Visual Studio verrà visualizzato di uno schema stesso, selezionare la finestra di dialogo illustrata nella figura 6.


Markup dichiarativo della nuova pagina aspx include solo un @Page direttiva specificando il percorso al master per la pagina file e un controllo contenuto MainContent ContentPlaceHolder della pagina master.

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample2.aspx)]

Per il momento lasciarlo vuoto Default.aspx. Più avanti in questa esercitazione per aggiungere contenuto è restituirà ad esso.

> [!NOTE]
> La pagina master include una sezione di un menu o altre interfacce di navigazione. Tale interfaccia verrà creata in un'esercitazione future.


## <a name="step-2-enabling-forms-authentication"></a>Passaggio 2: Abilitare l'autenticazione basata su form

Con il sito Web ASP.NET creato, l'attività successiva consiste per abilitare l'autenticazione basata su form. Configurazione dell'autenticazione dell'applicazione viene specificata tramite il [ &lt;autenticazione&gt; elemento](https://msdn.microsoft.com/library/532aee0e.aspx) in Web. config. Il &lt;autenticazione&gt; elemento contiene un singolo attributo denominato modalità che specifica il modello di autenticazione usato dall'applicazione. Questo attributo può avere uno dei quattro valori seguenti:

- **Windows** : come illustrato nell'esercitazione precedente, quando un'applicazione utilizza l'autenticazione di Windows è responsabilità del server web per autenticare il visitatore e questa operazione viene in genere eseguita tramite Basic, Digest o integrata di Windows autenticazione.
- **Form**-gli utenti vengono autenticati tramite un modulo in una pagina web.
- **Passport**-gli utenti vengono autenticati utilizzando Microsoft Passport Network.
- **Nessuno**-non viene utilizzato alcun modello di autenticazione, tutti i visitatori sono anonimi.

Per impostazione predefinita, le applicazioni ASP.NET utilizzano l'autenticazione di Windows. Per modificare il tipo di autenticazione per l'autenticazione basata su form, è necessario modificare il &lt;autenticazione&gt; attributo mode dell'elemento a un form.

Se il progetto non contiene ancora un file Web. config, aggiungere un'ora da facendo clic sul nome del progetto in Esplora soluzioni, scegliere Aggiungi nuovo elemento e quindi aggiungere un file di configurazione Web.


[![Se il progetto non Include ancora Web. config, aggiungerlo ora](an-overview-of-forms-authentication-vb/_static/image20.png)](an-overview-of-forms-authentication-vb/_static/image19.png)

**Figura 07**: se il progetto Does non ancora includere file Web. config, aggiungere ora ([fare clic per visualizzare l'immagine ingrandita](an-overview-of-forms-authentication-vb/_static/image21.png))


Successivamente, individuare il &lt;autenticazione&gt; elemento e l'aggiornamento, l'uso di moduli di autenticazione. Dopo questa modifica, markup del file Web. config dovrebbe essere simile al seguente:

[!code-xml[Main](an-overview-of-forms-authentication-vb/samples/sample3.xml)]

> [!NOTE]
> Poiché il file Web. config è un file XML, sono importante maiuscole e minuscole. Assicurarsi di impostare l'attributo mode a un form, con una lettera maiuscola F. Se si utilizzano una maiuscole/minuscole, ad esempio form, si riceverà un errore di configurazione quando si visita il sito tramite un browser.


Il &lt;autenticazione&gt; elemento può includere facoltativamente un &lt;form&gt; elemento figlio che contiene impostazioni specifiche dell'autenticazione form. Per il momento, si utilizza solo il valore predefinito Form le impostazioni di autenticazione. Verrà illustrato il &lt;form&gt; elemento figlio in modo più dettagliato nella prossima esercitazione.

## <a name="step-3-building-the-login-page"></a>Passaggio 3: Creazione della pagina di accesso

Per supportare l'autenticazione basata su form il sito Web richiede una pagina di accesso. Come descritto in comprendere la sezione del flusso di lavoro autenticazione form, FormsAuthenticationModule automaticamente reindirizzerà l'utente alla pagina di accesso se si tenta di accedere a una pagina che non sono autorizzati a visualizzare. Esistono anche i controlli Web ASP.NET che verranno visualizzato un collegamento alla pagina di accesso agli utenti anonimi. Chiedersi, qual è l'URL della pagina di accesso?

Per impostazione predefinita, il sistema di autenticazione form prevede la pagina di accesso per essere denominata Login e inserito nella directory radice dell'applicazione web. Se si desidera utilizzare un URL di pagina account di accesso diverso, è possibile farlo specificandola nel file Web. config. Verrà spiegato come eseguire questa operazione nell'esercitazione successiva.

Pagina di accesso ha tre responsabilità:

1. Fornire un'interfaccia che consente il visitatore di immettere le credenziali.
2. Determinare se le credenziali presentate siano valide.
3. Effettuare l'accesso all'utente tramite la creazione di moduli di ticket di autenticazione.

### <a name="creating-the-login-pages-user-interface"></a>Creazione dell'interfaccia utente di pagina di accesso

È possibile iniziare subito con la prima attività. Aggiungere una nuova pagina ASP.NET alla directory radice del sito denominata Login e associarlo a una pagina master Site. master.


[![Aggiungere una nuova pagina ASP.NET Login.](an-overview-of-forms-authentication-vb/_static/image23.png)](an-overview-of-forms-authentication-vb/_static/image22.png)

**Figura 08**: aggiungere un nuovo ASP.NET pagina denominata Login. aspx ([fare clic per visualizzare l'immagine ingrandita](an-overview-of-forms-authentication-vb/_static/image24.png))


L'interfaccia pagina account di accesso tipico è costituito da due caselle di testo, una per il nome dell'utente, uno per la propria password - e un pulsante di invio del form. Siti Web includono spesso memorizza dati per la casella di controllo che, se selezionata, mantiene il ticket di autenticazione risultante tra i vari riavvi browser.

Aggiungere due caselle di testo Login e impostare le relative proprietà ID nome utente e Password, rispettivamente. Impostare inoltre la proprietà TextMode su Password Password. Successivamente, aggiungere un controllo casella di controllo, impostare la proprietà ID su rememberMe disponibile e la proprietà Text su Ricorda utente. Successivamente, aggiungere un pulsante denominato LoginButton la cui proprietà di testo è impostata su account di accesso. Infine, aggiungere un controllo etichetta Web e impostarne la proprietà ID per InvalidCredentialsMessage, la proprietà Text su nome utente o password non è valida. Ripetere l'operazione., la proprietà ForeColor in rosso e la relativa proprietà Visible su False.

A questo punto la schermata dovrebbe essere simile alla schermata illustrata nella figura 9, e la sintassi dichiarativa della pagina sarà simile al seguente:

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample4.aspx)]


[![Pagina di accesso contiene due caselle di testo, una casella di controllo, un pulsante e un'etichetta](an-overview-of-forms-authentication-vb/_static/image26.png)](an-overview-of-forms-authentication-vb/_static/image25.png)

**Figura 09**: l'account di accesso pagina contiene due caselle di testo, una casella di controllo, un pulsante e un'etichetta ([fare clic per visualizzare l'immagine ingrandita](an-overview-of-forms-authentication-vb/_static/image27.png))


Infine, creare un gestore eventi per il clic del LoginButton evento. Dalla finestra di progettazione, semplicemente fare doppio clic sul pulsante per creare questo gestore eventi.

### <a name="determining-if-the-supplied-credentials-are-valid"></a>Determinare se le credenziali fornite non sono validi

È ora necessario implementare attività 2 in fare clic sul pulsante gestore eventi: determinare se le credenziali fornite sono valide. Per eseguire questa operazione deve essere un archivio utente che contiene tutte le credenziali degli utenti in modo che è possibile determinare se le credenziali fornite corrispondano con le credenziali note.

Prima di ASP.NET 2.0, gli sviluppatori sono responsabili dell'implementazione di entrambi gli archivi le proprie utente e la scrittura del codice per convalidare le credenziali specificate nell'archivio. La maggior parte degli sviluppatori implementi l'archivio utente in un database, creazione di una tabella denominata gli utenti con le colonne come nome utente, Password, messaggio di posta elettronica, LastLoginDate e così via. Questa tabella, quindi, sarebbe necessario un record per ogni account utente. Verifica delle credenziali dell'utente comporterebbe una query sul database per un nome utente corrispondente e quindi verificare che la password del database corrisponde alla password fornita.

Con ASP.NET 2.0, gli sviluppatori devono utilizzare uno dei provider di appartenenze per gestire l'archivio dell'utente. In questa serie di esercitazioni che verrà usato il SqlMembershipProvider, che utilizza un database di SQL Server per l'archivio dell'utente. Quando si utilizza il provider SqlMembershipProvider è necessario implementare uno schema di database specifico che include tabelle, viste e stored procedure prevede dal provider. Esamineremo come implementare questo schema di *[creazione dello Schema di appartenenza in SQL Server](../membership/creating-the-membership-schema-in-sql-server-vb.md)* esercitazione. Con il provider di appartenenze sul posto, la convalida delle credenziali dell'utente è semplice come chiamare il [la classe di appartenenza](https://msdn.microsoft.com/library/system.web.security.membership.aspx)del [ValidateUser (*username*, *password*) metodo](https://msdn.microsoft.com/library/system.web.security.membership.validateuser.aspx), che restituisce un valore booleano che indica se la validità del *username* e *password* combinazione. Visualizzare come archivio dell'utente del SqlMembershipProvider non è stata ancora implementata, non è possibile usare il appartenenza al metodo della classe ValidateUser in questo momento.

Anziché il tempo di compilazione personalizzata personalizzato utenti tabella di database (che sarebbe obsoleta quando è implementato SqlMembershipProvider), verrà invece hardcoded credenziali valide all'interno dell'account di accesso della pagina stessa. Nel LoginButton gestore dell'evento Click, aggiungere il codice seguente:

[!code-vb[Main](an-overview-of-forms-authentication-vb/samples/sample5.vb)]

Come si può notare, sono disponibili tre account utente validi - Scott Jisun e Sam - e tutte e tre hanno la stessa password (password). Il codice consente di scorrere le matrici di utenti e password per una corrispondenza di nome utente e password valida. Se il nome utente e la password sono validi, è necessario eseguire l'accesso dell'utente e reindirizza alla pagina appropriata. Se le credenziali non sono validi, viene visualizzata l'etichetta di InvalidCredentialsMessage.

Quando un utente immette le credenziali valide, ho detto che vengono quindi reindirizzati alla pagina appropriata. Che cos'è la pagina appropriata, tuttavia? Tenere presente che quando un utente visita una pagina che non sono autorizzati a visualizzare, FormsAuthenticationModule automaticamente reindirizzato alla pagina di accesso. In questo modo, include nella stringa di query tramite il parametro ReturnUrl URL richiesto. Ovvero, se un utente ha tentato di visitare ProtectedPage.aspx e non sono stati autorizzati a tale scopo, FormsAuthenticationModule sarebbe reindirizza a:

Login.aspx?ReturnUrl=ProtectedPage.aspx

Seguito all'accesso, l'utente deve essere reindirizzato al ProtectedPage.aspx. In alternativa, gli utenti possono visitare la pagina di accesso nel proprio volition. In tal caso, dopo l'accesso dell'utente sono inviare alla pagina Default.aspx della cartella radice.

### <a name="logging-in-the-user"></a>Registrazione utente

Presupponendo che le credenziali specificate siano valide, è necessario creare un ticket di autenticazione form, in tal modo la registrazione dell'utente al sito. Il [classe FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthentication.aspx) nel [dello spazio dei nomi System.Web.Security](https://msdn.microsoft.com/library/system.web.security.aspx) fornisce diversi metodi per la registrazione in e registrazione gli utenti tramite i moduli di sistema di autenticazione. Esistono diversi metodi nella classe FormsAuthentication, sono tre che siamo interessati a questo punto:

- [GetAuthCookie (*username*, *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.getauthcookie.aspx) -crea un ticket di autenticazione form per il nome fornito *username*. Successivamente, questo metodo crea e restituisce un oggetto HttpCookie che contiene il contenuto del ticket di autenticazione. Se *persistCookie* è True, viene creato un cookie permanente.
- [SetAuthCookie (*username*, *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) -chiama il GetAuthCookie (*username*, *persistCookie*) metodo per generare il cookie di autenticazione form. Questo metodo aggiunge quindi il cookie restituito da GetAuthCookie alla raccolta di cookie (presupponendo che l'autenticazione basata su form basato su cookie viene utilizzata; in caso contrario, questo metodo chiama una classe interna che gestisce la logica di ticket cookieless).
- [RedirectFromLoginPage (*username*, *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.redirectfromloginpage.aspx) -questo metodo chiama SetAuthCookie (*username*, *persistCookie*) e quindi l'utente viene reindirizzato alla pagina appropriata.

GetAuthCookie è utile quando è necessario modificare il ticket di autenticazione prima di scrivere il cookie out per la raccolta di cookie. SetAuthCookie è utile se si desidera creare i form di ticket di autenticazione e aggiungerlo alla raccolta dei cookie, ma non si desidera reindirizzare l'utente alla pagina appropriata. Ad esempio si desidera mantenerli nella pagina di accesso o li inviano alla pagina alcune alternativa.

Poiché si desidera accedere l'utente e reindirizza alla pagina appropriata, utilizziamo RedirectFromLoginPage. Fare clic su di LoginButton aggiornare gestore eventi, sostituendo le due righe di commenti TODO con la riga di codice seguente:

FormsAuthentication.RedirectFromLoginPage(UserName.Text, RememberMe.Checked)

Durante la creazione di ticket di autenticazione moduli utilizziamo proprietà Text di UserName TextBox per il ticket di autenticazione form *username* parametro e lo stato di selezione di RememberMe CheckBox per il  *persistCookie* parametro.

Per testare la pagina di accesso, visitare il in un browser. Avviare tramite l'immissione di credenziali non valide, ad esempio un nome utente di e la password errata. Facendo clic sul pulsante di accesso verrà eseguito un postback e verrà visualizzato l'etichetta InvalidCredentialsMessage.


[![L'etichetta InvalidCredentialsMessage è visualizzato quando immissione credenziali non valide](an-overview-of-forms-authentication-vb/_static/image29.png)](an-overview-of-forms-authentication-vb/_static/image28.png)

**Figura 10**: l'etichetta InvalidCredentialsMessage viene visualizzato quando immissione credenziali non valide ([fare clic per visualizzare l'immagine ingrandita](an-overview-of-forms-authentication-vb/_static/image30.png))


Immettere le credenziali valide, quindi fare clic sul pulsante di accesso. In questo momento il postback un ticket di autenticazione form viene creato e si verrà reindirizzati automaticamente nuovo a Default.aspx. A questo punto hanno effettuato l'accesso al sito Web, anche se non esistono segnali visivi per indicare che si è attualmente connessi. Nel passaggio 4 che verrà spiegato come determinare a livello di programmazione se un utente viene registrato in o non nonché come identificare l'utente visita la pagina.

Passaggio 5 vengono esaminate le tecniche per la registrazione di un utente di disconnessione dal sito Web.

### <a name="securing-the-login-page"></a>Protezione di pagina di accesso

Quando l'utente immette le proprie credenziali e invia il form di pagina di accesso, le credenziali, incluso la password, vengono trasmessi tramite Internet al server web in *testo normale*. Ciò significa che un pirata informatico lo sniffing del traffico di rete è possibile visualizzare il nome utente e password. Per evitare questo problema, è essenziale per crittografare il traffico di rete tramite [livelli SSL (Secure Socket)](http://en.wikipedia.org/wiki/Secure_Sockets_Layer). Ciò garantisce che le credenziali (così come markup HTML della pagina intera) vengono crittografati dal momento in cui che lasciano il browser finché vengono ricevuti dal server web.

A meno che il sito Web contiene informazioni riservate, è necessario solo per l'utilizzo di SSL nella pagina di accesso e in altre pagine in cui la password dell'utente in caso contrario potrebbe essere inviata in rete in testo normale. Non occorre preoccuparsi di moduli di protezione dei ticket di autenticazione poiché, per impostazione predefinita, è sia crittografato e firmato digitalmente (per impedire eventuali manomissioni). Nell'esercitazione seguente, viene visualizzata una descrizione più dettagliata sulla sicurezza di ticket di autenticazione form.

> [!NOTE]
> Molti siti Web finanziarie e mediche sono configurati per l'utilizzo di SSL in *tutti* pagine accessibile per gli utenti autenticati. Se si sta compilando un sito Web in modo che il ticket di autenticazione form verrà trasmessi solo tramite una connessione sicura, è possibile configurare il sistema di autenticazione form. Verranno esaminati varie opzioni di configurazione per l'autenticazione form nella prossima esercitazione,  *[configurazione dell'autenticazione form e argomenti avanzati](../membership/creating-the-membership-schema-in-sql-server-vb.md)*.


## <a name="step-4-detecting-authenticated-visitors-and-determining-their-identity"></a>Passaggio 4: Rilevamento visitatori autenticati e determinare l'identità

A questo punto abbiamo abilitata l'autenticazione basata su form e creazione di una pagina di accesso rudimentali, ma non è stato ancora esaminare come è possibile determinare se un utente è autenticato o anonimo. In alcuni scenari potrebbe voler visualizzare dati diversi o informazioni a seconda se un utente autenticato o anonimo visita la pagina. Inoltre, è spesso necessario conoscere l'identità dell'utente autenticato.

Consente di aumentare la pagina aspx esistente per illustrare le tecniche. In Default.aspx aggiungere due controlli pannello, AuthenticatedMessagePanel denominata uno e un altro AnonymousMessagePanel denominata. Aggiungere un controllo etichetta denominato WelcomeBackMessage nel primo pannello. Nel secondo pannello aggiungere un controllo HyperLink, impostare la proprietà Text su Accedi e la proprietà NavigateUrl su ~ / Login. Il markup dichiarativo per Default.aspx a questo punto dovrebbe essere simile al seguente:

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample6.aspx)]

Come probabilmente si immaginare a questo punto, l'idea è per visualizzare solo il AuthenticatedMessagePanel visitatori autenticati e solo il AnonymousMessagePanel ai visitatori anonimi. A tale scopo è necessario impostare le proprietà visibili i pannelli a seconda se l'utente è connesso o non.

Il [Request.IsAuthenticated proprietà](https://msdn.microsoft.com/library/system.web.httprequest.isauthenticated.aspx) restituisce un valore booleano che indica se la richiesta è stata autenticata. Immettere il codice seguente nella pagina\_caricare codice del gestore eventi:

[!code-vb[Main](an-overview-of-forms-authentication-vb/samples/sample7.vb)]

Con questo codice, visitare Default.aspx tramite un browser. Supponendo che ancora si dispone di accesso, verrà visualizzato un collegamento alla pagina di accesso (vedere Figura 11). Fare clic su questo collegamento e accedere al sito. Come illustrato nel passaggio 3, dopo aver immesso le credenziali verrà di nuovo a Default.aspx, ma questa volta la pagina Mostra la Bentornato! messaggi (vedere Figura 12).


[![Quando si visita in modo anonimo, un collegamento nel Log viene visualizzato](an-overview-of-forms-authentication-vb/_static/image32.png)](an-overview-of-forms-authentication-vb/_static/image31.png)

**Figura 11**: viene visualizzato quando si visita in modo anonimo, un collegamento nel Log ([fare clic per visualizzare l'immagine ingrandita](an-overview-of-forms-authentication-vb/_static/image33.png))


[![Gli utenti autenticati vengono visualizzati nella parte posteriore di benvenuto! Messaggio](an-overview-of-forms-authentication-vb/_static/image35.png)](an-overview-of-forms-authentication-vb/_static/image34.png)

**Figura 12**: Authenticated Users vengono visualizzati nella parte posteriore di benvenuto! Messaggio ([fare clic per visualizzare l'immagine ingrandita](an-overview-of-forms-authentication-vb/_static/image36.png))


È possibile determinare l'identità dell'utente attualmente connesso tramite il [oggetto HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.aspx)del [proprietà utente](https://msdn.microsoft.com/library/system.web.httpcontext.user.aspx). L'oggetto HttpContext e rappresenta le informazioni sulla richiesta corrente, la home page per tali oggetti comuni ASP.NET come risposta e richiesta di sessione, tra gli altri. La proprietà utente rappresenta il contesto di sicurezza di richiesta HTTP corrente e si implementa il [interfaccia IPrincipal](https://msdn.microsoft.com/library/system.security.principal.iprincipal.aspx).

L'utente viene impostata da FormsAuthenticationModule. In particolare, quando FormsAuthenticationModule rileva che un ticket di autenticazione form di richiesta in ingresso, crea un nuovo oggetto GenericPrincipal e assegnarlo alla proprietà utente.

Oggetti Principal (ad esempio GenericPrincipal) forniscono informazioni sull'identità dell'utente e i ruoli a cui appartengono. L'interfaccia IPrincipal definisce due membri:

- [IsInRole (*roleName*)](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole.aspx) -un metodo che restituisce un valore booleano che indica se l'entità a cui appartiene al ruolo specificato.
- [Identità](https://msdn.microsoft.com/library/system.security.principal.iprincipal.identity.aspx) -una proprietà che restituisce un oggetto che implementa le [interfaccia IIdentity](https://msdn.microsoft.com/library/system.security.principal.iidentity.aspx). L'interfaccia IIdentity definisce tre proprietà: [AuthenticationType](https://msdn.microsoft.com/library/system.security.principal.iidentity.authenticationtype.aspx), [IsAuthenticated](https://msdn.microsoft.com/library/system.security.principal.iidentity.isauthenticated.aspx), e [nome](https://msdn.microsoft.com/library/system.security.principal.iidentity.name.aspx).

È possibile determinare il nome del visitatore corrente utilizzando il codice seguente:

Dim currentUsersName As String = User.Identity.Name

Quando l'utilizzo di form, autenticazione un [oggetto FormsIdentity](https://msdn.microsoft.com/library/system.web.security.formsidentity.aspx) viene creato per la proprietà Identity del GenericPrincipal. La classe FormsIdentity restituisce sempre i formati di stringa per la relativa proprietà AuthenticationType e True per la relativa proprietà IsAuthenticated. La proprietà Name restituisce il nome utente specificato durante la creazione di moduli di ticket di autenticazione. Oltre a queste tre proprietà FormsIdentity include l'accesso al ticket di autenticazione sottostante tramite il relativo [Ticket proprietà](https://msdn.microsoft.com/library/system.web.security.formsidentity.ticket.aspx). La proprietà Ticket restituisce un oggetto di tipo [FormsAuthenticationTicket](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.aspx), che dispone di proprietà, ad esempio scadenza, IsPersistent, IssueDate, nome e così via.

Il punto importante da sottolineare qui è che il *username* parametro specificato nel FormsAuthentication.GetAuthCookie (*username*, *persistCookie*), SetAuthCookie (*username*, *persistCookie*) e FormsAuthentication (*username*, *persistCookie*) metodi corrisponde al valore restituito da User.Identity.Name. Inoltre, il ticket di autenticazione creato da questi metodi è disponibile eseguendo il cast di User. Identity a un oggetto FormsIdentity e quindi accedere alla proprietà Ticket:

Dim ident come FormsIdentity = CType (User. Identity, FormsIdentity)

Dim il ticket di autenticazione come FormsAuthenticationTicket = ident. Ticket

Di seguito, fornire un messaggio più personalizzato in Default.aspx. Aggiornare la pagina\_caricare in modo che la proprietà di testo dell'etichetta WelcomeBackMessage è assegnata la stringa, il gestore di evento *username*!

WelcomeBackMessage.Text = "Welcome back, " &amp; User.Identity.Name &amp; "!"

Figura 13 illustra l'effetto di questa modifica (durante l'accesso come utente Scott).


[![Il messaggio di benvenuto include attualmente connesso nel nome dell'utente](an-overview-of-forms-authentication-vb/_static/image38.png)](an-overview-of-forms-authentication-vb/_static/image37.png)

**Figura 13**: il messaggio di benvenuto include nome dell'utente nel registrati attualmente ([fare clic per visualizzare l'immagine ingrandita](an-overview-of-forms-authentication-vb/_static/image39.png))


### <a name="using-the-loginview-and-loginname-controls"></a>Utilizzando le LoginName controlli LoginView

La visualizzazione di contenuto diversi agli utenti autenticati e anonimi è un requisito comune; Pertanto, viene visualizzato il nome dell'utente attualmente connesso. Per questo motivo, in ASP.NET sono disponibili due controlli Web che forniscono la stessa funzionalità illustrata nella figura 13, ma senza la necessità di scrivere una singola riga di codice.

Il [controllo LoginView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) è un controllo Web basato su modello che rende più semplice visualizzare dati diversi agli utenti anonimi e autenticati. Il LoginView include due modelli predefiniti:

- AnonymousTemplate - qualsiasi tag aggiunto a questo modello viene visualizzato solo per i visitatori anonimi.
- LoggedInTemplate - markup del modello viene visualizzata solo per gli utenti autenticati.

Aggiungere il controllo LoginView alla pagina principale del sito, Site. master. Invece di aggiungere solo per il controllo LoginView, tuttavia, aggiungere un controllo ContentPlaceHolder nuovo e quindi inserire il controllo LoginView all'interno di tale ContentPlaceHolder nuovo. La logica di questa decisione verrà diventano evidente a breve.

> [!NOTE]
> Oltre alle AnonymousTemplate e LoggedInTemplate, il controllo LoginView può includere modelli specifici del ruolo. Modelli di ruolo specifiche mostrano commenti solo agli utenti che appartengono a un ruolo specificato. Le funzionalità in base al ruolo del controllo LoginView verrà esaminato in un'esercitazione future.


Per iniziare, aggiungere un controllo ContentPlaceHolder denominato LoginContent alla pagina master all'interno di navigazione &lt;div&gt; elemento. È semplicemente possibile trascinare un controllo ContentPlaceHolder dalla casella degli strumenti nella visualizzazione origine, inserire il codice risulta immediatamente di sopra del TODO: Menu entra in questo caso il testo.

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample8.aspx)]

Successivamente, aggiungere un controllo LoginView all'interno di LoginContent ContentPlaceHolder. Il contenuto inserito in controlli ContentPlaceHolder della pagina master vengono considerati *predefinito contenuto* per ContentPlaceHolder. Pagine ASP.NET che utilizzano la pagina master, ovvero possono specificare il contenuto per ogni ContentPlaceHolder o utilizzare contenuto predefinito della pagina master.

Nella scheda account di accesso della casella degli strumenti si trovano i LoginView e altri controlli relative all'accesso.


[![Il controllo LoginView nella casella degli strumenti](an-overview-of-forms-authentication-vb/_static/image41.png)](an-overview-of-forms-authentication-vb/_static/image40.png)

**Figura 14**: il controllo LoginView nella casella degli strumenti ([fare clic per visualizzare l'immagine ingrandita](an-overview-of-forms-authentication-vb/_static/image42.png))


Successivamente, aggiungere due &lt;Brasile /&gt; elementi immediatamente dopo il controllo LoginView, ma ancora all'interno di ContentPlaceHolder. A questo punto, la navigazione &lt;div&gt; markup dell'elemento dovrebbe essere simile al seguente:

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample9.aspx)]

È possibile definire modelli di LoginView dalla finestra di progettazione o markup dichiarativo. Progettazione di Visual Studio, espandere smart tag del LoginView, vengono elencati i modelli configurati in un elenco a discesa. Digitare il testo Hello, estraneo nel AnonymousTemplate; Successivamente, aggiungere un controllo collegamento ipertestuale e impostarne le proprietà di testo e NavigateUrl di Log e ~ / Login, rispettivamente.

Dopo aver configurato il modello AnonymousTemplate, passare alla LoggedInTemplate e immettere il testo, "Bentornato,". Trascinare un controllo LoginName dalla casella degli strumenti in LoggedInTemplate posizionarla subito dopo "Iniziale," testo. Il [controllo LoginName](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginname.aspx), come il nome implica, Visualizza il nome dell'utente attualmente connesso. Internamente, il controllo LoginName restituisce semplicemente la proprietà User.Identity.Name

Dopo aver apportato queste aggiunte ai modelli di LoginView, il markup dovrebbe essere simile al seguente:

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample10.aspx)]

Con l'aggiunta della pagina master Site. master, ogni pagina in questo sito Web verrà visualizzato un messaggio diverso a seconda che l'utente è autenticato. Figura 15 mostra la pagina aspx quando visitato tramite un browser dall'utente Jisun. Bentornato, Jisun messaggio viene ripetuto a due volte: una volta nella sezione di navigazione della pagina master a sinistra (tramite il controllo LoginView è stato appena aggiunto) e una volta nel Default.aspx area (tramite controlli Panel e logica di programmazione) del contenuto.


[![Il LoginView controllo Visualizza Bentornato, Jisun.](an-overview-of-forms-authentication-vb/_static/image44.png)](an-overview-of-forms-authentication-vb/_static/image43.png)

**Figura 15**: back LoginView controllo Visualizza schermata iniziale, Jisun. ([Fare clic per visualizzare l'immagine ingrandita](an-overview-of-forms-authentication-vb/_static/image45.png))


Poiché è stata aggiunta la LoginView della pagina master, ma può essere visualizzato in ogni pagina sono disponibili sul sito. Tuttavia, potrebbero esserci pagine web in cui non si desidera visualizzare più questo messaggio. Una tale pagina è la pagina di accesso, poiché un collegamento alla pagina di accesso sembra esiste sul posto. Poiché il controllo LoginView è stato inserito in un controllo ContentPlaceHolder nella pagina master, è possibile eseguire l'override di questo tag predefinito nella nostra pagina contenuta. Aprire Login e passare alla finestra di progettazione. Poiché non sono state definite in modo esplicito un controllo contenuto in login per LoginContent ContentPlaceHolder nella pagina master, la pagina di accesso mostrerà markup predefinito della pagina master per questo ContentPlaceHolder. È possibile vedere questo tramite la finestra di progettazione - LoginContent ContentPlaceHolder viene illustrato il markup predefinito (il controllo LoginView).


[![Pagina di accesso Mostra contenuto il valore predefinito per LoginContent ContentPlaceHolder della pagina Master](an-overview-of-forms-authentication-vb/_static/image47.png)](an-overview-of-forms-authentication-vb/_static/image46.png)

**Figura 16**: pagina di accesso viene illustrato il contenuto predefinito per LoginContent ContentPlaceHolder's Page the Master ([fare clic per visualizzare l'immagine ingrandita](an-overview-of-forms-authentication-vb/_static/image48.png))


Per ignorare il markup predefinito per il LoginContent ContentPlaceHolder, fare doppio clic sull'area nella finestra di progettazione e scegliere l'opzione Crea contenuto personalizzato dal menu di scelta rapida. (Quando si utilizza Visual Studio 2008 ContentPlaceHolder include uno smart tag che, se selezionata, offre la stessa opzione.) Aggiunge un nuovo controllo contenuto per il markup della pagina e in tal modo ci permette di definire il contenuto personalizzato per questa pagina. È possibile aggiungere un messaggio personalizzato in questo caso, ad esempio effettuare l'accesso, ma si lasciare vuoto questo campo.

> [!NOTE]
> In Visual Studio 2005, la creazione di contenuto personalizzato crea un oggetto vuoto in una pagina ASP.NET controllo contenuto. In Visual Studio 2008, tuttavia, la creazione di contenuto personalizzato copia il contenuto predefinito della pagina master nel controllo contenuto appena creato. Se si utilizza Visual Studio 2008, quindi, dopo aver creato il nuovo controllo contenuto assicurarsi di cancellare il contenuto copiato dalla pagina master.


Figura 17 Mostra pagina Login quando visitato da un browser dopo aver apportato questa modifica. Si noti che non vi sia alcun Hello, estraneo o Bentornato, *username* messaggio nel riquadro di spostamento sinistro &lt;div&gt; sia disponibile durante la visita Default.aspx.


[![Pagina di accesso nasconde Markup di LoginContent ContentPlaceHolder predefinito](an-overview-of-forms-authentication-vb/_static/image50.png)](an-overview-of-forms-authentication-vb/_static/image49.png)

**Figura 17**: pagina di accesso nasconde Markup del LoginContent ContentPlaceHolder predefinito ([fare clic per visualizzare l'immagine ingrandita](an-overview-of-forms-authentication-vb/_static/image51.png))


## <a name="step-5-logging-out"></a>Passaggio 5: La disconnessione

Nel passaggio 3 illustra la creazione di una pagina di accesso per la registrazione di un utente al sito, ma non è stato ancora per informazioni su come disconnettere un utente. Oltre ai metodi per la registrazione di un utente, la classe FormsAuthentication fornisce anche un [metodo SignOut](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx). Il metodo SignOut elimina semplicemente il ticket di autenticazione form, in tal modo l'utente dal sito di registrazione.

Una collegamento di disconnessione è una funzionalità comune di questo tipo di offerta da ASP.NET include un controllo progettato specificamente per disconnettere un utente. Il [controllo LoginStatus](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginstatus.aspx) Visualizza LinkButton un account di accesso o Logout LinkButton, a seconda dello stato di autenticazione dell'utente. Viene eseguito il rendering LinkButton un account di accesso per gli utenti anonimi, mentre un Logout LinkButton viene visualizzato agli utenti autenticati. Il testo per l'account di accesso e Logout LinkButtons può essere configurato tramite la LoginStatus LoginText e LogoutText proprietà.

Facendo clic sull'elemento LinkButton accesso provoca un postback, da cui viene eseguito un reindirizzamento alla pagina di accesso. Fare clic su di Logout LinkButton, il controllo LoginStatus richiamare il metodo FormsAuthentication.SignOff e reindirizza l'utente a una pagina. La pagina di registrazione è disattivata, viene reindirizzato a dipende dalla proprietà LogoutAction, che può essere assegnata a uno dei tre valori seguenti:

- Aggiornamento: il valore predefinito; Reindirizza l'utente alla pagina che sono stati semplice visita. Se la pagina che sono stati semplice visita non consentire agli utenti anonimi, quindi FormsAuthenticationModule automaticamente reindirizzerà l'utente alla pagina di accesso.

È possibile sapere come motivo per cui viene eseguito in questo caso un reindirizzamento. Se l'utente desidera rimangono nella stessa pagina, perché la necessità per il reindirizzamento esplicito? Il motivo è che quando viene selezionato il Logoff LinkButton, l'utente ha ancora il ticket di autenticazione form nella propria raccolta di cookie. Di conseguenza, la richiesta di postback è una richiesta autenticata. Il controllo LoginStatus chiama il metodo SignOut, ma che si verifica dopo FormsAuthenticationModule ha autenticato l'utente. Di conseguenza, un reindirizzamento esplicito causa il browser richiederà nuovamente la pagina. Una volta che il browser richiede nuovamente la pagina, il ticket di autenticazione form è stato rimosso e pertanto la richiesta in ingresso è anonima.

- Reindirizzamento - l'utente viene reindirizzato all'URL specificato dalla proprietà LogoutPageUrl del LoginStatus.
- RedirectToLoginPage - l'utente viene reindirizzato alla pagina di accesso.

È ora possibile aggiungere un controllo LoginStatus della pagina master e configurarlo per utilizzare l'opzione di reindirizzamento per inviare all'utente a una pagina che visualizza un messaggio per confermare che si è stato disconnesso. Iniziare creando una pagina nella directory radice denominata Logout. Non dimenticare di associare questa pagina della pagina master Site. master. Quindi, immettere un messaggio nel markup della pagina che spiega all'utente che è stato disconnesso.

Successivamente, tornare alla pagina master Site. master e aggiungere un controllo LoginStatus di sotto di LoginView LoginContent ContentPlaceHolder. Impostare proprietà del controllo LoginStatus LogoutAction reindirizzamento e la relativa proprietà LogoutPageUrl ~/Logout.aspx.

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample11.aspx)]

Poiché il LoginStatus all'esterno del controllo LoginView, verrà visualizzato per gli utenti anonimi e autenticati, ma che è OK, in quanto il LoginStatus correttamente verrà visualizzato un account di accesso o Logout LinkButton. Con l'aggiunta del controllo LoginStatus, il Log In collegamento ipertestuale il AnonymousTemplate è superfluo, quindi rimuoverlo.

Figura 18 Mostra Default.aspx quando Jisun visita. Si noti che la colonna a sinistra viene visualizzato il messaggio, Bentornato, Jisun insieme a un collegamento per la disconnessione. Scegliere la disconnessione LinkButton provoca un postback Jisun si disconnette dal sistema e utente Reindirizza quindi Logout. Come mostrato nella figura 19, una volta che jisun raggiunge Logout Mary è stato disconnesso ed è pertanto anonima. Di conseguenza, la colonna a sinistra mostra il testo di benvenuto, estraneo e un collegamento alla pagina di accesso.


[![Default. aspx Mostra Bentornato, Jisun insieme Logout LinkButton](an-overview-of-forms-authentication-vb/_static/image53.png)](an-overview-of-forms-authentication-vb/_static/image52.png)

**Figura 18**: default. aspx Mostra completamento dell'installazione precedente, Jisun lungo con Logout LinkButton ([fare clic per visualizzare l'immagine ingrandita](an-overview-of-forms-authentication-vb/_static/image54.png))


[![Logout Mostra iniziale, estraneo insieme a un account di accesso LinkButton](an-overview-of-forms-authentication-vb/_static/image56.png)](an-overview-of-forms-authentication-vb/_static/image55.png)

**Figura 19**: Logout Mostra iniziale, estraneo insieme LinkButton account di accesso ([fare clic per visualizzare l'immagine ingrandita](an-overview-of-forms-authentication-vb/_static/image57.png))


> [!NOTE]
> Ti suggeriamo di personalizzare la pagina Logout per nascondere LoginContent ContentPlaceHolder della pagina master (ad esempio, abbiamo fatto per login nel passaggio 4). Il motivo è che l'account di accesso di LinkButton sottoposto a rendering dal controllo LoginStatus (quello sotto Hello, estraneo) invia l'utente alla pagina di accesso, passando il parametro querystring ReturnUrl all'URL corrente. In breve, se un utente che si è disconnesso fa clic su account di accesso LinkButton di questo LoginStatus e quindi Registra cui verrà reindirizzati alla Logout, che potrebbe facilmente confondere l'utente.


## <a name="summary"></a>Riepilogo

In questa esercitazione è avviato con un esame del flusso di lavoro autenticazione form e quindi attivato per implementare l'autenticazione basata su form in un'applicazione ASP.NET. Autenticazione basata su form è una tecnologia FormsAuthenticationModule, che ha la responsabilità di due: identificare gli utenti in base alle loro ticket di autenticazione form e il reindirizzamento di utenti non autorizzati alla pagina di accesso.

Classe FormsAuthentication di .NET Framework include metodi per la creazione, analisi e la rimozione di ticket di autenticazione form. La proprietà di Request.IsAuthenticated e un oggetto utente di fornire ulteriore supporto a livello di codice per determinare se una richiesta è autenticata e informazioni sull'identità dell'utente. Sono inoltre disponibili i controlli LoginName Web LoginView e LoginStatus, che offrono agli sviluppatori un modo rapido e senza codice per l'esecuzione di molte attività comuni relative all'accesso. Verrà esaminato questi e altri controlli Web relative all'accesso in modo più dettagliato in esercitazioni future.

In questa esercitazione è fornita una panoramica dell'autenticazione basata su form superficiale. Non esaminare le opzioni di configurazione diversi, esaminare la modalità cookie form autenticazione ticket lavoro o esplorare come ASP.NET consente di proteggere il contenuto del ticket di autenticazione form. Verranno illustrate in questi argomenti e informazioni, vedere il [esercitazione successiva](forms-authentication-configuration-and-advanced-topics-vb.md).

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Modifiche tra IIS 6 e IIS 7 di sicurezza](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/Changes-between-IIS6-and-IIS7-Security)
- [Controlli ASP.NET di account di accesso](https://msdn.microsoft.com/library/d51ttbhx.aspx)
- [Professional ASP.NET 2.0 sicurezza, l'appartenenza e gestione dei ruoli](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (codice ISBN: 978-0-7645-9698-8)
- [Il &lt;autenticazione&gt; elemento](https://msdn.microsoft.com/library/532aee0e.aspx)
- [Il &lt;form&gt; elemento per &lt;autenticazione&gt;](https://msdn.microsoft.com/library/1d3t3c61.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Video di formazione su argomenti contenuti in questa esercitazione

- [Uso dell'autenticazione basata su form di base in ASP.NET](../../../videos/authentication/using-basic-forms-authentication-in-aspnet.md)

### <a name="about-the-author"></a>Informazioni sull'autore

Scott Mitchell, autore di più libri e fondatore di 4GuysFromRolla, ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è  *[SAM insegna manualmente ASP.NET 2.0 nelle 24 ore](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott può essere raggiunto al [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) o tramite il suo blog all'indirizzo [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Lead revisori per questa esercitazione includono Alicja Maziarz, John Suru e Teresa Murphy. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com).

> [!div class="step-by-step"]
> [Precedente](security-basics-and-asp-net-support-vb.md)
> [Successivo](forms-authentication-configuration-and-advanced-topics-vb.md)
