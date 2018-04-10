---
uid: whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application
title: 'Procedura: Aggiungere pagine per dispositivi mobili per il Web Form ASP.NET / applicazione MVC | Documenti Microsoft'
author: rick-anderson
description: In questa procedura vengono descritti vari modi per fornire pagine ottimizzate per i dispositivi mobili di Web Form ASP.NET / applicazione MVC e vengono suggerite alcune dell'architettura e...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2011
ms.topic: article
ms.assetid: 3124f28e-cc32-418a-afe3-519fa56f4c36
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application
msc.type: content
ms.openlocfilehash: a8358b91ca424f4f3e576057ab43d850081dda60
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/10/2018
---
<a name="how-to-add-mobile-pages-to-your-aspnet-web-forms--mvc-application"></a>Procedura: Aggiungere pagine per dispositivi mobili per il Web Form ASP.NET / applicazione MVC
====================
> **Si applica a**
> 
> - Versione di Web Form ASP.NET 4.0
> - ASP.NET MVC versione 3.0
> 
> **Riepilogo**
> 
> In questa procedura vengono descritti vari modi per fornire pagine ottimizzate per i dispositivi mobili di Web Form ASP.NET / applicazione MVC e suggerisce dell'architettura e problemi da prendere in considerazione quando la destinazione di un'ampia gamma di dispositivi di progettazione. Questo documento illustra anche i controlli di ASP.NET Mobile da ASP.NET 2.0 a 3.5 perché sono ora obsoleti e vengono illustrate alcune alternative moderne.


## <a name="contents"></a>Sommario

- Panoramica
- Opzioni dell'architettura
- Rilevamento di browser e dispositivi
- Come le applicazioni Web Form ASP.NET possono presentare pagine specifiche di dispositivi mobili
- Come le applicazioni ASP.NET MVC possono presentare pagine specifiche di dispositivi mobili
- Risorse aggiuntive

Per esempi di codice scaricabile che illustra le tecniche di questo white paper per Web Form ASP.NET e MVC, vedere [App per dispositivi mobili e i siti con ASP.NET](https://docs.microsoft.com/aspnet/mobile/overview).

## <a name="overview"></a>Panoramica

I dispositivi mobili: Smartphone, funzionalità telefoni e Tablet: continuano a crescere in popolarità come mezzo per accedere al Web. Per molti sviluppatori web e alle aziende orientata ai servizi web, questo significa che è sempre più importante fornire un'esperienza di esplorazione ottimale per i visitatori che utilizzano tali dispositivi.

### <a name="how-earlier-versions-of-aspnet-supported-mobile-browsers"></a>La modalità più versioni di browser per dispositivi mobili ASP.NET supportata

Versioni di ASP.NET 2.0 a 3.5 incluso *controlli per dispositivi mobili ASP.NET*: un set di controlli server per i dispositivi mobili nel *System.Web.Mobile.dll* assembly e  *MobileControls* dello spazio dei nomi. L'assembly è ancora inclusa in ASP.NET 4, ma è deprecato. Gli sviluppatori sono invitati a eseguire la migrazione a più moderni approcci, ad esempio quelli descritti in questo documento.

Il motivo per cui controlli per dispositivi mobili ASP.NET sono stati contrassegnati come obsolete è che la struttura è orientata telefoni cellulari comuni intorno 2005 e versioni precedenti. I controlli sono progettati principalmente per il rendering del markup WML o cHTML (anziché regolare HTML) per i browser WAP che era. Ma WAP, gli elementi WML e cHTML non sono più rilevanti per la maggior parte dei progetti, in quanto HTML è diventato il linguaggio di markup universale per dispositivi mobili e desktop browser simili.

### <a name="the-challenges-of-supporting-mobile-devices-today"></a>I problemi di supportare i dispositivi mobili a oggi

Anche se i browser per dispositivi mobili supportano ora universalmente HTML, si dovranno affrontare comunque numerose problematiche, al fine di creare esperienze di esplorazione per dispositivi mobili:

- ***Dimensione dello schermo*** : i dispositivi mobili variano notevolmente in form e gli schermi sono in genere molto inferiori rispetto a quella dei desktop. In tal caso, potrebbe essere necessario progettare il layout di pagina completamente diversa per loro.
- ***Metodi di input*** – alcuni dispositivi dispongono di tastiere, alcuni degli stili del, mentre altri usano tocco. Potrebbe essere necessario prendere in considerazione la navigazione più dati e i meccanismi di metodi di input.
- ***Conformità agli standard*** – molti browser per dispositivi mobili non supportano gli standard più recenti di HTML, CSS o JavaScript.
- ***Larghezza di banda*** : variano notevolmente le prestazioni della rete dati del cellulare e alcuni utenti finali non sono in tariffe che applicano un addebito per il megabyte.

Non c'è alcuna soluzione giusto; l'applicazione sarà necessario esaminare e un comportamento diverso a seconda del dispositivo di accedervi. In base alla quale livello di supporto mobile desiderata, può trattarsi di una richiesta per gli sviluppatori web maggiore rispetto a quanto desktop "attraverso browser" mai.

Gli sviluppatori per raggiungere il supporto browser per dispositivi mobili per la prima volta spesso inizialmente considerare che solo è importante supportare le più recenti e più sofisticati Smartphone (ad esempio, Windows Phone 7, iPhone o Android), probabilmente perché gli sviluppatori proprietaria spesso personalmente, ad dispositivi. Tuttavia, telefoni più economici sono comunque molto comuni e proprietari usarli per navigare in rete, in particolare nei paesi in cui sono semplici ottenere una connessione a banda larga telefoni cellulari. L'azienda sarà necessario decidere quali gamma di dispositivi per supportare considerando i clienti di probabilità. Se si sta creando un brochure online per l'autenticazione spa integrità un lusso, si potrebbero prendere decisioni aziendali solo di destinazione avanzati Smartphone, mentre se si sta creando un sistema di prenotazione di ticket di cinema, è probabilmente necessario tenere conto per i visitatori con meno potenti funzionalità telefoni.

## <a name="architectural-options"></a>Opzioni dell'architettura

Prima di passare ai dettagli tecnici specifici di MVC o Web Form ASP.NET, si noti che gli sviluppatori web sono in genere tre principali opzioni possibili per il supporto browser per dispositivi mobili:

1. ***Non eseguire alcuna operazione –*** è semplicemente possibile creare un'applicazione web standard, orientato ai desktop e si basano su browser per dispositivi mobili per eseguire il rendering accettabile. 

    - **Vantaggio**: è l'opzione più economica da implementare e mantenere, non un ulteriore lavoro
    - **Svantaggio**: fornisce l'esperienza dell'utente finale peggiore: 

        - I più recenti Smartphone possono eseguire il rendering HTML così come un browser per desktop, ma gli utenti saranno costretti ancora per ingrandire e scorrere orizzontalmente e verticalmente per utilizzare il contenuto su schermi di piccole dimensioni. Questo è lontano dal ottimale.
        - Dispositivi meno recenti e i telefoni funzionalità potrebbero non riuscire a eseguire il rendering di markup in modo soddisfacente.
        - Anche in dispositivi tablet più recenti (il cui schermate possono essere grandi come schermi di computer portatili), si applicano le regole di interazione tra diversi. Input basato sul tocco funziona meglio con i pulsanti di dimensioni maggiori e collega spread ulteriormente separati e non vi è alcun modo per passare un puntatore del mouse su un menu a comparsa.
2. ***Risolvere il problema sul client* –** con un'attenta utilizzo di CSS e [progressivo miglioramento](http://en.wikipedia.org/wiki/Progressive_enhancement) è possibile creare markup, stili e script che si adattano a qualsiasi browser li esegue. Ad esempio, con [query di supporto di CSS 3](http://www.w3.org/TR/2010/CR-css3-mediaqueries-20100727/), è possibile creare un layout a più colonne che trasforma in un layout a colonna singola su dispositivi la cui schermate sono più ristretta rispetto a una soglia specifica. 

    - **Vantaggio**: consente di ottimizzare il rendering per il dispositivo specifico in uso, anche per i dispositivi futuri sconosciuti in base a qualsiasi schermata e le caratteristiche di input hanno
    - **Vantaggio**: facilmente consente di condividere logica sul lato server in tutti i tipi di dispositivo, la duplicazione minima di codice o il lavoro richiesto
    - **Svantaggio**: i dispositivi mobili sono talmente diversi dai dispositivi desktop che è effettivamente possibile pagine per dispositivi mobili può essere completamente diverso dalle pagine desktop, che mostra informazioni diverse. Le variazioni possono risultare inefficiente o impossibili da soddisfare in modo affidabile tramite CSS da solo, specie se si considera come incoerente dispositivi meno recenti interpretano le regole CSS. Questo vale in particolare degli attributi di CSS 3.
    - **Svantaggio**: non fornisce supporto per diversa logica sul lato server e i flussi di lavoro per dispositivi diversi. Mezzo CSS da solo non è possibile, ad esempio, implementare un workflow estrazione carrello acquisti semplificato per gli utenti mobili.
    - **Svantaggio**: uso non efficiente della larghezza di banda. Il server potrebbe essere necessario trasmettere il markup e stili che si applicano a tutti i dispositivi possibili, anche se il dispositivo di destinazione verrà utilizzato solo un subset di informazioni.
3. ***Risolvere il problema nel server* –** se il server sa quale dispositivo accede a tale colonna – o meno le caratteristiche del dispositivo, ad esempio le dimensioni dello schermo e il metodo di input, e se si tratta di un dispositivo mobile può essere eseguito una logica diversa e markup HTML diverso di output. 

    - **Vantaggio:** flessibilità massima. Non è previsto alcun limite per quanto è possibile variare la logica sul lato server per dispositivi mobili o ottimizzare il markup per il layout desiderato, specifico del dispositivo.
    - **Vantaggio:** utilizzo efficiente della larghezza di banda. È necessario solo trasmettere il markup e informazioni sugli stili che il dispositivo di destinazione che verrà utilizzata.
    - **Svantaggi:** volte forza la ripetizione del lavoro richiesto o del codice (ad esempio, rendendo si creare copie simili, ma leggermente diverse del pagine Web Form o le visualizzazioni MVC). Dove possibile, che verrà basta fattorizzare logica comune in un livello sottostante o il servizio, ma comunque alcune parti del markup o codice dell'interfaccia utente potrebbe essere necessario duplicato e quindi gestiti in parallelo.
    - **Svantaggi:** il rilevamento dei dispositivi non è semplice. Richiede un elenco o un database di tipi di dispositivi noti e le relative caratteristiche (che potrebbero non essere sempre aggiornati perfettamente) e non sempre corrispondono in modo accurato di ogni richiesta in ingresso. Questo documento descrive alcune opzioni e i relativi problemi in un secondo momento.

Per ottenere i migliori risultati, la maggior parte degli sviluppatori trovano che devono combinare opzioni (2) e (3). Commento differenze sono più indicate sul client utilizzando CSS o JavaScript anche, mentre le differenze principali nel markup, del flusso di lavoro o dati vengono implementate in modo più efficiente in codice lato server.

### <a name="this-paper-focuses-on-server-side-techniques"></a>Questo documento vengono descritte le tecniche sul lato server

Poiché MVC e Web Form ASP.NET sono entrambe le tecnologie principalmente sul lato server, in questo white paper sarà incentrata sui tecniche sul lato server che consentono di produrre codice diverso e la logica per browser per dispositivi mobili. Naturalmente, è anche possibile combinare questa con qualsiasi tecnica sul lato client (ad esempio, CSS 3 supporti le query, JavaScript progressivo miglioramento), ma è più una questione di progettazione web di sviluppo ASP.NET.

## <a name="browser-and-device-detection"></a>Rilevamento di browser e dispositivi

Il prerequisito di chiave per tutte le tecniche sul lato server per supportare i dispositivi mobili è sapere quale dispositivo utilizza il visitatore. Infatti, ancora migliore conoscere il numero di produttore e modello del dispositivo è conoscere il *caratteristiche* del dispositivo. Caratteristiche possono includere:

- Si tratta di un dispositivo mobile?
- Metodo di input (mouse o tastiera, tocco, tastierino, joystick,...)
- La dimensione dello schermo (fisicamente e, in pixel)
- Formati di dati e i supporti supportati
- E così via.

Si consiglia di prendere decisioni in base alle caratteristiche del numero di modello, in quanto, quindi è in grado di gestire dispositivi futuri.

### <a name="using-aspnets-built-in-browser-detection-support"></a>Utilizzo di ASP. Supporto per il rilevamento di rete browser predefinito

Gli sviluppatori di Web Form ASP.NET e MVC possono individuare immediatamente importanti caratteristiche di un controllo browser controllando le proprietà del *Request* oggetto. Ad esempio, vedere

- Request.Browser.IsMobileDevice
- Request.Browser.MobileDeviceManufacturer, Request.Browser.MobileDeviceModel
- Request.Browser.ScreenPixelsWidth
- Request.Browser.SupportsXmlHttp
- ... e molti altri

Dietro le quinte, la piattaforma di ASP.NET corrisponde in ingresso *User-Agent* intestazione HTTP (agente utente) con le espressioni regolari in un set di file XML di definizione del Browser. È possibile aggiungere file di definizione del Browser personalizzati ad altri utenti che si desidera riconoscere per impostazione predefinita la piattaforma include le definizioni per molti dispositivi mobili comuni. Per ulteriori informazioni, vedere la pagina MSDN [controlli Server Web ASP.NET e le funzionalità del Browser](https://msdn.microsoft.com/library/x3k2ssx2.aspx).

### <a name="using-the-wurfl-device-database-via-51degreesmobi-foundation"></a>Usando il database di dispositivo WURFL tramite 51Degrees.mobi Foundation

Mentre ASP. Supporto per il rilevamento di rete browser predefiniti saranno sufficiente per molte applicazioni, esistono due casi principali quando potrebbe non essere sufficiente:

- ***Per riconoscere i dispositivi più recenti***(senza creare manualmente i file di definizione del Browser per essi). Si noti che i file di definizione del Browser .NET 4 non sono abbastanza recenti per riconoscere Apple iPad, telefoni Android, il browser Opera Mobile o Windows Phone 7.
- ***Sono necessarie informazioni più dettagliate sulle funzionalità del dispositivo***. Potrebbe essere necessario conoscere il metodo di input di un dispositivo (ad esempio, tastierino vs tocco) o quali audio formati il browser supporta. Queste informazioni non sono disponibile nei file di definizione del Browser standard.

Il [ *File di risorsa universale Wireless* progetto (WURFL)](http://wurfl.sourceforge.net/) gestisce più aggiornate e dettagliate informazioni sui dispositivi mobili in uso oggi.

Le novità per gli sviluppatori di .NET è che ASP. Funzionalità di rilevamento del browser di rete è estensibile, pertanto è possibile migliorare in modo da risolvere tali problemi. Ad esempio, è possibile aggiungere open source [ *51Degrees.mobi Foundation* ](https://github.com/51Degrees/dotNET-Device-Detection) libreria al progetto. È un IHttpModule ASP.NET o un Browser funzionalità Provider (utilizzabile in applicazioni di Web Form e MVC), che legge i dati WURFL direttamente e lo collega in ASP. Meccanismo di rilevamento di rete browser predefinito. Dopo aver installato il modulo, *Request* improvvisamente conterrà informazioni molto più precise e dettagliate: verrà correttamente riconoscere molti dei dispositivi indicati in precedenza e le funzionalità (ad esempio elenco funzionalità aggiuntive come metodo di input). Vedere la documentazione del progetto per maggiori dettagli.

## <a name="how-web-forms-applications-can-present-mobile-specific-pages"></a>Come le applicazioni Web Form possono presentare pagine specifiche di dispositivi mobili

Per impostazione predefinita, ecco come una nuova applicazione Web Form viene visualizzata nei dispositivi mobili comuni:

![](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/_static/image1.png)

Chiaramente, né layout appaia molto mobile-friendly: questa pagina è stata progettata per un monitor di grandi dimensioni, con orientamento orizzontale, non per schermi di piccole dimensioni orientamento verticale. Pertanto, operazioni su di esso?

Come descritto in precedenza in questo documento, sono disponibili vari modi per personalizzare le pagine per i dispositivi mobili. Alcune tecniche sono basate su server, altri vengono eseguiti nel client.

### <a name="creating-a-mobile-specific-master-page"></a>Creazione di una pagina master specifici dispositivi mobili

A seconda dei requisiti, è possibile usare la stessa di Web Form per tutti i visitatori, ma disporre di due pagine master distinte: uno per i visitatori del desktop, un altro per i visitatori per dispositivi mobili. Questo offre la flessibilità della modifica del foglio di stile CSS o nel markup HTML di primo livello in base alle esigenze di dispositivi mobili, evitando di duplicare qualsiasi logica di pagina.

Si tratta di un'operazione semplice. Ad esempio, è possibile aggiungere un gestore PreInit come illustrato di seguito a un Web Form:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample1.cs)]

A questo punto, creare una pagina master denominata Mobile.Master nella cartella principale dell'applicazione e verrà utilizzato quando viene rilevato un dispositivo mobile. Pagina master per dispositivi mobili può fare riferimento a un foglio di stile CSS specifiche mobile se necessario. Visitatori desktop continueranno a vedere la pagina master predefinita, non a quello per dispositivi mobili.

### <a name="creating-independent-mobile-specific-web-forms"></a>Creazione di indipendenti specifiche mobile Web Form

Per la massima flessibilità, è possibile passare maggiore rispetto a solo pagine master separate per diversi tipi di dispositivo. È possibile implementare due *completamente separare insiemi di pagine Web Form* -impostata per i browser desktop, un altro set per dispositivi mobili. Questa situazione è ideale se si desidera presentare informazioni molto diverse o flussi di lavoro per dispositivi mobili visitatori. Il resto di questa sezione viene descritto questo approccio in modo dettagliato.

Se che si dispone già di un'applicazione Web Form progettata per i browser desktop, il modo più semplice per procedere consiste nel creare una sottocartella denominata "Mobile" all'interno del progetto e creare pagine per dispositivi mobili non esiste. È possibile costruire un intero sito secondario, con il proprio pagine master, fogli di stile e pagine, con le stesse tecniche che si utilizzerebbe per qualsiasi altra applicazione Web Form. Non è necessario produrre un equivalente per dispositivi mobili per *ogni* pagina nel sito desktop; è possibile scegliere il sottoinsieme di funzionalità è significativa per dispositivi mobili visitatori.

Pagine per dispositivi mobili possono condividere risorse statiche comuni (ad esempio immagini, JavaScript o CSS file) con le pagine regolari se si desidera. Poiché la cartella "Mobile" verrà *non* essere contrassegnato come applicazione separata ospitato in IIS (è sufficiente una semplice sottocartella delle pagine Web Form), anche condividono le stesse configurazione, i dati della sessione e un'altra infrastruttura come il pagine desktop.

> [!NOTE]
> Poiché in genere, questo approccio comporta alcuni la duplicazione del codice (pagine per dispositivi mobili sono probabile che condividono alcune analogie con pagine desktop), è importante separare comuni business logica dati accesso codice o in un livello sottostante condiviso o un servizio. In caso contrario, sarà necessario raddoppiare l'impegno di creazione e gestione dell'applicazione.


#### <a name="redirecting-mobile-visitors-to-your-mobile-pages"></a>Reindirizzamento visitatori pagine per dispositivi mobili per dispositivi mobili

Spesso risulta utile reindirizzamento visitatori pagine per dispositivi mobili per dispositivi mobili solo in caso di *prima* richiesta nella sessione di esplorazione e non a ogni richiesta nella sessione, perché:

- È possibile quindi consentire ai visitatori per dispositivi mobili accedere alle pagine desktop, se necessario, inserire un collegamento nella pagina master che passa a "Versione Desktop". Visitatore non verrà reindirizzato a una pagina mobile, perché non è più la prima richiesta nella sessione.
- Evita il rischio di interferire con le richieste per tutte le risorse dinamiche condivise tra le parti di desktop e mobile del sito (ad esempio, se si dispone di un Form Web comuni che consentono di visualizzare le parti mobili e desktop del sito in un IFRAME, o alcuni gestori Ajax)

A tale scopo, è possibile inserire la logica di reindirizzamento in un **sessione\_avviare** metodo. Ad esempio, aggiungere il metodo seguente al file Global.asax.cs:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample2.cs)]

#### <a name="configuring-forms-authentication-to-respect-your-mobile-pages"></a>Configurazione dell'autenticazione form di rispettare pagine per dispositivi mobili

Si noti che l'autenticazione form su alcuni presupposti in cui è possibile reindirizzare i visitatori durante e dopo il processo di autenticazione:

- Quando un utente deve essere autenticata, autenticazione basata su form reindirizzerà alla pagina di accesso del desktop, indipendentemente dal fatto che sono relative a un utente di computer desktop o portatile (perché dispone solo di un concetto di *uno* URL di accesso). Supponendo che si desidera applicare uno stile di pagina di accesso per dispositivi mobili in modo diverso, è necessario ottimizzare la pagina di accesso desktop in modo che gli utenti mobili viene reindirizzato a una pagina di accesso mobile separato. Ad esempio, aggiungere il codice seguente per il **desktop** codice pagina account di accesso: 

    [!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample3.cs)]
- Dopo che un utente accede correttamente, autenticazione basata su form per impostazione predefinita reindirizzerà alla home page del desktop (perché dispone solo di un concetto di *uno* pagina predefinita). È necessario ottimizzare la pagina di accesso per dispositivi mobili in modo che esegue il reindirizzamento alla home page di mobile dopo un log ha esito positivo. Ad esempio, aggiungere il codice seguente per il **mobili** codice pagina account di accesso: 

    [!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample4.cs)]
  
  Questo codice si presuppone che la pagina è un controllo di accesso server chiamato LoginUser, come il modello di progetto predefinito.

### <a name="working-with-output-caching"></a>Utilizzo di memorizzazione nella cache di Output

Se si utilizza la memorizzazione nella cache di output, tenere presente che per impostazione predefinita, che è possibile che un utente desktop accedere a un determinato URL (causando l'output da memorizzare nella cache), seguita da un utente mobile, che quindi riceve l'output del desktop memorizzati nella cache. Questo avviso si applica sia appena variare la pagina master per tipo di dispositivo, implementare completamente separata Web Form per ogni tipo di dispositivo.

Per evitare il problema, è possibile indicare ASP.NET per variare la voce della cache in base che il visitatore utilizzi un dispositivo mobile. Aggiungere un parametro VaryByCustom alla dichiarazione OutputCache della pagina, come segue:

[!code-aspx[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample5.aspx)]

Successivamente, definire *isMobileDevice* come cache personalizzata parametro aggiungendo il seguente metodo di override per il file Global.asax.cs:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample6.cs)]

Ciò garantisce che i dispositivi mobili visitatori non ricevono output precedentemente inseriti nella cache per un visitatore di un desktop.

### <a name="a-working-example"></a>Un esempio funzionante

Per visualizzare queste tecniche in azione, scaricare [esempi di codice di questo white paper](https://docs.microsoft.com/aspnet/mobile/overview). L'applicazione di esempio di Web Form reindirizza automaticamente gli utenti mobili a un set di pagine mobile specifiche in una sottocartella denominata Mobile. Il markup e lo stile di tali pagine meglio è ottimizzata per il browser per dispositivi mobili, come si può vedere dalle schermate seguenti:

![](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/_static/image2.png)

Per altri suggerimenti su come ottimizzare il markup e CSS per i browser per dispositivi mobili, vedere la sezione "Applicazione di stili pagine per dispositivi mobili per i browser per dispositivi mobili" più avanti in questo documento.

## <a name="how-aspnet-mvc-applications-can-present-mobile-specific-pages"></a>Come le applicazioni ASP.NET MVC possono presentare pagine specifiche di dispositivi mobili

Poiché il modello Model-View-Controller separa la logica dell'applicazione (in controller) dalla logica di presentazione (nelle viste), è possibile scegliere da uno degli approcci seguenti per la gestione di supporto per dispositivi mobili nel codice sul lato server:

1. ***Utilizzare lo stesso controller e visualizzazioni per i browser desktop e mobili, ma il rendering di visualizzazioni con layout di Razor diversi a seconda del tipo di dispositivo *.** Questa opzione funziona meglio se si sta visualizzando dati identici in tutti i dispositivi, ma desidera semplicemente specificare diversi fogli di stile CSS o modificare alcuni elementi HTML di livello superiore per dispositivi mobili.
2. ***Utilizzare gli stessi controller per i browser desktop e mobili, ma il rendering di visualizzazioni diverse a seconda del tipo di dispositivo***. Questa opzione funziona meglio se è attiva la visualizzazione all'incirca gli stessi dati e fornire la stesse dei flussi di lavoro per gli utenti finali, ma desidera eseguire il rendering di markup HTML molto diversi per soddisfare il dispositivo in uso.
3. ***Creare aree separate per i browser desktop e mobile, implementazione indipendente controller e visualizzazioni per ogni *.** Questa opzione funziona meglio se si sta visualizzare schermate molto diverse, che contiene informazioni diverse e che l'utente attraverso diversi flussi di lavoro ottimizzato per il relativo tipo di dispositivo. Ciò può significare alcuni ripetizione di codice, ma è possibile ridurre al minimo eseguendo il factoring logica comune in un servizio o il livello sottostante.

Se si desidera eseguire il **prima** opzione e il layout di Razor variano per ogni tipo di dispositivo, è molto semplice. Modificare solo il \_ViewStart.cshtml file come segue:

[!code-cshtml[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample7.cshtml)]

Ora è possibile creare un layout specifico mobile denominato \_LayoutMobile.cshtml con una struttura della pagina e CSS regole ottimizzate per i dispositivi mobili.

Se si desidera eseguire il **secondo** opzione rendering viste completamente diversi in base al tipo di dispositivo del visitatore, vedere [post di blog di Scott Hanselman](http://www.hanselman.com/blog/ABetterASPNETMVCMobileDeviceCapabilitiesViewEngine.aspx).

Il resto di questo documento è incentrato sul **terzo** opzione: creazione di controller separato *e* viste per dispositivi mobili, è pertanto possibile controllare esattamente il sottoinsieme di funzionalità è disponibile per i visitatori per dispositivi mobili.

### <a name="setting-up-a-mobile-area-within-your-aspnet-mvc-application"></a>Configurazione di un'area all'interno dell'applicazione MVC ASP.NET per dispositivi mobili

È possibile aggiungere un'area denominata "Mobile" a un'applicazione MVC ASP.NET esistente in modo normale: fare clic sul nome del progetto in Esplora soluzioni, quindi scegliere Aggiungi à Area. È quindi possibile aggiungere controller e visualizzazioni, come si farebbe per qualsiasi altra area all'interno di un'applicazione MVC ASP.NET. Ad esempio, aggiungere al portale Mobile un nuovo controller chiamato HomeController di agire come una home page per i visitatori per dispositivi mobili.

### <a name="ensuring-the-url-mobile-reaches-the-mobile-homepage"></a>Garantire il /Mobile URL raggiunge la home page di dispositivi mobili

Se si desidera /Mobile l'URL per raggiungere l'operazione sull'indice in HomeController all'interno della superficie di dispositivi mobili, è necessario apportare due piccole modifiche alla configurazione di routing. In primo luogo, aggiornare la classe MobileAreaRegistration in modo da HomeController controller predefinito nella propria zona per dispositivi mobili, come illustrato nel codice seguente:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample8.cs)]

Ciò significa che la home page di dispositivi mobili ora sarà posizionata /Mobile, piuttosto che Mobile/Home, perché "Home" è ora l'in modo implicito nome controller predefinito per le pagine mobile.

Successivamente, si noti che aggiungendo un secondo HomeController all'applicazione (ad esempio, per dispositivi mobili quella, oltre a un desktop esistente), si verrà interrotto della home page desktop regolare. Avrà esito negativo con l'errore "*sono stati trovati più tipi corrispondenti del controller specificato"Home"*". Per risolvere questo problema, aggiornare la configurazione di routing di livello superiore (in Global.asax.cs) per specificare che il desktop HomeController devono avere la priorità di ambiguità:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample9.cs)]

Dopo l'errore entra di stoccaggio e l'URL http://<em>yoursite</em>/ raggiungerà la home page del desktop e http://<em>yoursite</em>/mobile/ raggiungerà la home page di dispositivi mobili.

### <a name="redirecting-mobile-visitors-to-your-mobile-area"></a>Reindirizzamento visitatori mobili al portale mobile

Vi sono molti punti di estendibilità diversi in ASP.NET MVC, pertanto vi sono molti modi per inserire la logica di reindirizzamento. I cosiddetti è possibile creare un attributo di filtro, [RedirectMobileDevicesToMobileArea], che esegue un reindirizzamento, se vengono soddisfatte le condizioni seguenti:

1. È la prima richiesta nella sessione dell'utente (ad esempio, Session.IsNewSession è uguale a true)
2. La richiesta proviene da un browser per dispositivi mobili (ad esempio, Request.Browser.IsMobileDevice è uguale a true)
3. L'utente non richiede già una risorsa nell'area di dispositivi mobili (ad esempio, il *percorso* parte dell'URL non inizia con /Mobile)

L'esempio scaricabile incluso in questo white paper include un'implementazione di questa logica. Viene implementato come un filtro di autorizzazione, derivato da AuthorizeAttribute, il che significa che è possibile utilizzare correttamente anche se si utilizza la memorizzazione nella cache di output (in caso contrario, se accede a un primo visitatore desktop un determinato URL, l'output desktop potrebbero essere memorizzate nella cache e quindi resi disponibili alle visitatori mobili successivi).

Perché è un filtro, è possibile scegliere di applicarlo ai controller specifico e le azioni, ad esempio,

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample10.cs)]

… è possibile applicare a tutti i controller e azioni come un MVC 3 *filtro globale* nel file di Global.asax.cs:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample11.cs)]

L'esempio scaricabile viene inoltre illustrato come è possibile creare sottoclassi di questo attributo che reindirizza a percorsi specifici all'interno della superficie di dispositivi mobili. Ciò significa, ad esempio, che è possibile:

- Registrare un filtro globale come illustrato sopra che invia mobili visitatori alla home page per dispositivi mobili per impostazione predefinita.
- Applica un filtro [RedirectMobileDevicesToMobileProductPage] speciale a un'azione "Visualizza prodotti" che accetta visitatori per dispositivi mobili per la versione mobile di qualsiasi pagina del prodotto che ha richiesto.
- Altri sottoclassi speciale del filtro si applicano anche alle azioni specifiche, reindirizzando mobili i visitatori equivalente per dispositivi mobili

### <a name="configuring-forms-authentication-to-respect-your-mobile-pages"></a>Configurazione dell'autenticazione form di rispettare pagine per dispositivi mobili

Se si utilizza l'autenticazione basata su form, si noti che quando un utente deve accedere, automaticamente l'utente viene reindirizzato a un singolo "accesso" URL specifico, che per impostazione predefinita è **/Account/accesso**. Ciò significa che gli utenti mobili reindirizzarlo all'azione desktop "accesso".

Per evitare questo problema, aggiungere logica all'azione di "accesso" desktop in modo che gli utenti mobili viene reindirizzato nuovamente a un'azione di "accesso" per dispositivi mobili. Se si utilizza il modello di applicazione MVC ASP.NET predefinito, aggiornare l'azione di accesso del AccountController come segue:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample12.cs)]

… e quindi implementare un'azione di "accesso" specifici dispositivi mobili adatta in un controller denominato AccountController nell'area di interesse per dispositivi mobili.

### <a name="working-with-output-caching"></a>Utilizzo di memorizzazione nella cache di Output

Se si utilizza il filtro [OutputCache], è necessario forzare la voce della cache per variano a seconda del tipo di dispositivo. Ad esempio, scrivere:

[!code-javascript[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample13.js)]

Quindi, aggiungere il metodo seguente al file Global.asax.cs:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample14.cs)]

Ciò garantisce che i dispositivi mobili visitatori non ricevono output precedentemente inseriti nella cache per un visitatore di un desktop.

### <a name="a-working-example"></a>Un esempio funzionante

Per visualizzare queste tecniche in azione, scaricare [codice di questo white paper associati esempi](https://docs.microsoft.com/aspnet/mobile/overview). L'esempio include un'applicazione ASP.NET MVC 3 (versione finale candidata) migliorata per supportare i dispositivi mobili utilizzando i metodi descritti sopra.

## <a name="further-guidance-and-suggestions"></a>Ulteriori linee guida e suggerimenti

La seguente discussione si applica sia al Web Form e gli sviluppatori MVC che utilizzano le tecniche illustrate in questo documento.

### <a name="enhancing-your-redirection-logic-using-51degreesmobi-foundation"></a>Miglioramento della logica di reindirizzamento utilizzando 51Degrees.mobi Foundation

La logica di reindirizzamento illustrata in questo documento può essere perfettamente sufficiente per l'applicazione, ma non funzionerà se è necessario disabilitare le sessioni o con un browser per dispositivi mobili che rifiuta i cookie (questi non possono avere sessioni), perché non conoscere se una determinata richiesta è il primo da tale visitatore.

Si è già appreso come il 51Degrees.mobi Apri origine Foundation può migliorare l'accuratezza di ASP. Rilevamento del browser di rete. Include anche un integrato che consente di reindirizzare mobili visitatori a indirizzi specifici configurati in Web. config. È in grado di funzionare a seconda delle sessioni ASP.NET (e pertanto i cookie) mediante l'archiviazione di un log temporaneo degli hash delle intestazioni HTTP e gli indirizzi IP dei visitatori, in modo che sappia se ogni richiesta è il primo da un determinato vistor.

Il seguente elemento aggiunto alla sezione del file Web. config fiftyOne reindirizzerà la prima richiesta da un dispositivo mobile rilevato alla pagina ~ / Mobile/Default.aspx. Tutte le richieste di pagine all'interno della cartella per dispositivi mobili verranno *non* reindirizzato, indipendentemente dal tipo di dispositivo. Se il dispositivo mobile è rimasto inattivo per un periodo di 20 minuti o più il dispositivo verrà dimenticato e le richieste successive verranno considerate come nuovi per gli scopi di reindirizzamento.

[!code-xml[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample15.xml)]

Per ulteriori informazioni, vedere [51degrees.mobi documentazione Foundation](https://github.com/51Degrees/dotNET-Device-Detection).

> [!NOTE]
> Si *possibile* funzionalità di reindirizzamento della Fondazione 51Degrees.mobi utilizzare in applicazioni ASP.NET MVC, ma è necessario definire la configurazione di reindirizzamento in termini di URL normale, non in termini di parametri di routing o inserendo i filtri MVC in azioni. In questo modo (al momento della scrittura) 51Degrees.mobi Foundation non riconosce il routing o filtri.


### <a name="disabling-transcoders-and-proxy-servers"></a>La disabilitazione di server Proxy e Transcodificatori

Gli operatori di telefonia mobile hanno due obiettivi generali nell'approccio a internet per dispositivi mobili.

1. Specificare come contenuto molto rilevante possibili
2. Aumentare il numero di clienti che possono condividere la larghezza di banda limitata radio

Poiché la maggior parte delle pagine web sono stati progettati per schermi di grandi dimensioni desktop e connessioni a banda larga fissa fast-line, molti operatori usano *transcodificatori* o *server proxy* che modificare dinamicamente il contenuto web. Si può modificare il markup HTML o CSS in base alle esigenze di schermi più piccoli (soprattutto per "funzionalità telefoni" che non dispongono di potenza di elaborazione per la gestione di layout complessi) e potrebbe ricomprimere immagini (riducendo notevolmente la qualità) per migliorare la velocità di recapito di pagina.

Ma se è stata acquisita l'impegno di produrre una versione ottimizzata del sito, probabilmente non si vuole interferire con eventuali ulteriormente l'operatore di rete. È possibile aggiungere la riga seguente alla pagina\_evento di caricamento in un Web Form ASP.NET:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample16.cs)]

In alternativa, per un controller di ASP.NET MVC, è possibile aggiungere il seguente override del metodo in modo che venga applicata a tutte le azioni nell'ambito del controller:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample17.cs)]

Il messaggio HTTP risultante informa transcodificatori conformi W3C e i proxy non è necessario modificare il contenuto. Ovviamente, non è garantito che gli operatori di telefonia mobile rispetterà questo messaggio.

### <a name="styling-mobile-pages-for-mobile-browsers"></a>Stile di pagine per i browser per dispositivi mobili per dispositivi mobili

Rientra nell'ambito di questo documento per descrivere in dettaglio i tipi di lavoro di markup HTML correttamente o le tecniche di progettazione web ottimizzare l'usabilità in dispositivi specifici. Dispone di backup per individuare un layout sufficientemente semplice, ottimizzato per una schermata di dimensioni di dispositivi mobili, senza utilizzare non affidabile HTML o CSS posizionamento consigli. Una tecnica importante importante ricordare, tuttavia, è il *tag meta viewport*.

Alcuni browser per dispositivi mobili moderni, nelle pagine di web visualizzato impegno destinata dei desktop, il rendering della pagina in un'area di disegno virtuale, detta anche "viewport" (ad esempio, il riquadro di visualizzazione virtuale è 980 pixel su iPhone e 850 pixel in Opera Mobile per impostazione predefinita), quindi ridurre il risultato per adattarla pixel fisici della schermata. L'utente può quindi eseguire lo zoom avanti e scorrere per il viewport. Il vantaggio che consente il browser di visualizzare la pagina in un layout desiderato, ma è anche ha lo svantaggio che impone lo zoom e traslazione, che non è pratico per l'utente. Se si sta progettando per dispositivi mobili, è consigliabile progettare una schermata "narrow" in modo che non sia necessario alcun valore di zoom o lo scorrimento orizzontale.

Un modo per indicare al browser in dispositivi mobili la larghezza deve essere il viewport è tramite il non conforme allo standard *viewport* tag meta. Ad esempio, se si aggiunge il seguente alla sezione di intestazione della pagina,

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample18.html)]

… quindi supporto browser smartphone layout della pagina in un'area di disegno virtuale a livello di 480 pixel. Ciò significa che se gli elementi HTML definiscono la larghezza in percentuale, le percentuali verranno interpretate rispetto a questa larghezza 480 pixel, non la larghezza del riquadro di visualizzazione predefinita. Di conseguenza, l'utente è meno probabile che è necessario eseguire lo zoom e scorrere orizzontalmente: notevolmente migliorare l'esperienza di esplorazione per dispositivi mobili.

Se si desidera la larghezza del viewport per la corrispondenza di pixel fisici del dispositivo, è possibile specificare quanto segue:

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample19.html)]

Per questa soluzione funzioni correttamente, non è necessario forzare in modo esplicito gli elementi supera la larghezza (ad esempio, utilizzando un *larghezza* attributo o una proprietà CSS), in caso contrario il browser dovrà utilizzare indipendentemente dal fatto che un riquadro di visualizzazione più grande. Vedere anche: [ulteriori dettagli sul tag viewport non conforme allo standard](https://developer.apple.com/library/safari/#documentation/AppleApplications/Reference/SafariHTMLRef/Articles/MetaTags.html).

Supportano più recenti Smartphone *Orientamento doppia*: può essere mantenuti in modalità verticale o orizzontale. In tal caso, è importante evitare di basarsi su presupposti relativi alla larghezza dello schermo in pixel. Non ancora presupporre che la larghezza dello schermo è fissa, perché l'utente può orientare nuovamente il dispositivo mentre si trovano nella pagina.

Dispositivi Windows Mobile e Blackberry meno recenti possono accettare anche i seguenti tag meta nell'intestazione di pagina per informarli del contenuto è stato ottimizzato per dispositivi mobili e pertanto non devono essere trasformate.

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample20.html)]

## <a name="additional-resources"></a>Risorse aggiuntive

Per un elenco di emulatori di dispositivi mobili e i simulatori è possibile utilizzare per testare l'applicazione web ASP.NET per dispositivi mobili, vedere la pagina [simulare diffusi dispositivi mobili per il testing](../mobile/device-simulators.md).

## <a name="credits"></a>Crediti

- Autore principale: Steven Sanderson
- Revisori / aggiuntive contenuto writer: James Rosewell, Mikael Söderström, Scott Hanselman, Scott Hunter
