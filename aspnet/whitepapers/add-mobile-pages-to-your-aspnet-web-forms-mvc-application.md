---
uid: whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application
title: 'Procedura: Aggiungere pagine Mobile Web Form ASP.NET / MVC Application | Microsoft Docs'
author: rick-anderson
description: In questa procedura vengono descritti vari modi per fornire pagine ottimizzate per dispositivi mobili dai Web Form ASP.NET / applicazione MVC e suggerisce all'architettura e...
ms.author: aspnetcontent
ms.date: 01/20/2011
ms.assetid: 3124f28e-cc32-418a-afe3-519fa56f4c36
msc.legacyurl: /whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application
msc.type: content
ms.openlocfilehash: 59b81184852a7fe0ad2dcad9718b572a8c756918
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823355"
---
<a name="how-to-add-mobile-pages-to-your-aspnet-web-forms--mvc-application"></a>Procedura: Aggiungere pagine Mobile Web Form ASP.NET / MVC dell'applicazione
====================
> **Si applica a**
> 
> - Versione di Web Form ASP.NET 4.0
> - ASP.NET MVC versione 3.0
> 
> **Riepilogo**
> 
> In questa procedura vengono descritti vari modi per fornire pagine ottimizzate per dispositivi mobili dai Web Form ASP.NET / applicazione MVC e suggerisce all'architettura e i problemi da prendere in considerazione quando la destinazione un'ampia gamma di dispositivi di progettazione. Questo documento illustra anche il motivo per cui i controlli Mobile ASP.NET da ASP.NET 2.0 alla 3.5 sono ora obsoleti e illustra alcune alternative moderne.


## <a name="contents"></a>Sommario

- Panoramica
- Opzioni dell'architettura
- Rilevamento del browser e dispositivi
- Come le applicazioni Web Form ASP.NET possono presentare pagine specifiche per dispositivi mobili
- Come le applicazioni ASP.NET MVC possono presentare pagine specifiche per dispositivi mobili
- Risorse aggiuntive

Per esempi di codice scaricabile che illustrano le tecniche di questo white paper per Web Form ASP.NET e MVC, vedere [App per dispositivi mobili e siti con ASP.NET](https://docs.microsoft.com/aspnet/mobile/overview).

## <a name="overview"></a>Panoramica

I dispositivi mobili, Smartphone, telefoni e Tablet – continuano a crescere in popolarità come mezzo per accedere al Web. Per molti sviluppatori web e alle aziende orientata ai servizi web, ciò significa che è sempre più importante fornire un'eccezionale esperienza di esplorazione per visitatori che utilizzano tali dispositivi.

### <a name="how-earlier-versions-of-aspnet-supported-mobile-browsers"></a>Versioni precedenti la modalità del browser per dispositivi mobili ASP.NET supportata

Le versioni ASP.NET 2.0 alla 3.5 incluso *controlli Mobile ASP.NET*: un set di controlli server per i dispositivi mobili nel *DLL* assembly e  *MobileControls* dello spazio dei nomi. L'assembly è ancora incluso in ASP.NET 4, ma è deprecato. Gli sviluppatori sono invitati a eseguire la migrazione a approcci più moderni, ad esempio quelle descritte in questo documento.

Il motivo per cui i controlli Mobile ASP.NET sono stati contrassegnati come obsolete è che la struttura è orientata i telefoni cellulari che sono stati comuni intorno a 2005 e versioni precedenti. I controlli sono progettati principalmente per il rendering del markup cHTML o WML (anziché regolare HTML) per i browser WAP di quell'epoca. Ma WAP, cHTML e WML, non sono più rilevanti per la maggior parte dei progetti, poiché HTML è ora diventato il linguaggio di markup universale per i browser per dispositivi mobili e desktop nello stesso modo.

### <a name="the-challenges-of-supporting-mobile-devices-today"></a>Le sfide di supportare i dispositivi mobili oggi stesso

Anche se i browser per dispositivi mobili supportano ora quasi universalmente HTML, occorre comunque affrontare molte sfide concepito per creare eccezionali esperienze di esplorazione per dispositivi mobili:

- ***Dimensioni dello schermo*** : i dispositivi mobili variano notevolmente in forma e le schermate sono in genere molto inferiori a monitor. Pertanto, potrebbe essere necessario progettare il layout di pagina completamente diversa per loro.
- ***I metodi di input*** -alcuni dispositivi hanno tastierini numerici, alcuni dispongono degli stili del, mentre altri usano touch. Potrebbe essere necessario prendere in considerazione la navigazione più metodi di input meccanismi e i dati.
- ***Conformità agli standard*** – molti browser per dispositivi mobili non supportano gli standard più recenti di HTML, CSS o JavaScript.
- ***Larghezza di banda*** : variano notevolmente le prestazioni della rete dati cellulare e alcuni utenti sono sulle tariffe doganali che applicano un addebito per il megabyte.

Non vi è alcuna soluzione universale; l'applicazione sarà necessario eseguire ricerche e ottenere un comportamento diverso in base al dispositivo accede a tale colonna. A seconda di quale livello di supporto per dispositivi mobili desiderato, può essere una sfida per gli sviluppatori web più grande rispetto a quanto fosse mai il desktop "guerre browser".

Gli sviluppatori per raggiungere il supporto browser per dispositivi mobili per la prima volta spesso inizialmente pensare che solo è importante supportare gli Smartphone più sofisticati e più recenti (ad esempio, Windows Phone 7, iPhone o Android), forse perché gli sviluppatori spesso personalmente il proprietario, ad dispositivi. Tuttavia, i telefoni più economici sono comunque molto popolari e i proprietari di usarli per esplorare il web, in particolare nei paesi in cui i telefoni cellulari sono più semplici ottenere rispetto a una connessione a banda larga. L'azienda dovrà decidere quali gamma di dispositivi per supportare prendendo in considerazione i clienti probabilmente. Se si sta creando una brochure online per l'autenticazione spa integrità un lusso, si potrebbe prendere una decisione business solo come destinazione gli Smartphone avanzati, mentre se si sta creando un sistema di prenotazioni di ticket per un cinema, è probabilmente necessario tenere conto per i visitatori con meno potenti funzionalità telefoni.

## <a name="architectural-options"></a>Opzioni dell'architettura

Prima di passare ai dettagli tecnici specifici di MVC o Web Form ASP.NET, si noti che gli sviluppatori web devono in genere tre possibili opzioni principali per il supporto browser per dispositivi mobili:

1. ***Non fare nulla:*** è semplicemente possibile creare un'applicazione web basata su desktop standard e affidarsi a browser per dispositivi mobili per eseguire il rendering siano accettabili. 

    - **Vantaggio**: si tratta dell'opzione più economica da implementare e gestire: non è un ulteriore lavoro
    - **Svantaggio**: offre l'esperienza utente finale peggiore: 

        - I più recenti Smartphone possono eseguire il rendering HTML analoghe a quelle di browser desktop, ma gli utenti saranno costretti ancora fatto zoom avanti e scorrere orizzontalmente e verticalmente di utilizzare i contenuti su schermi di piccole dimensioni. Questo è tutt'altro che ottimale.
        - Telefoni e dispositivi meno recenti potrebbero non riuscire a eseguire il rendering di markup in modo soddisfacente.
        - Anche in dispositivi tablet più recenti (il cui schermate possono essere grandi come schermi di computer portatili), si applicano le regole di interazione differenti. Input basati su touch funziona meglio con i pulsanti più grandi e i collegamenti spread ulteriormente distanza e non è possibile passare il mouse un cursore del mouse su un menu a comparsa.
2. ***Risolvere il problema sul client* –** con l'uso attento di CSS e [miglioramento progressivo](http://en.wikipedia.org/wiki/Progressive_enhancement) è possibile creare markup, stili e script adattabili a li esegue indipendentemente dal browser. Ad esempio, con [query supporti CSS 3](http://www.w3.org/TR/2010/CR-css3-mediaqueries-20100727/), è possibile creare un layout a più colonne che trasforma in un layout di colonna singola su dispositivi cui schermate sono più strette rispetto a una soglia specifica. 

    - **Vantaggio**: Ottimizza per il rendering per il dispositivo specifico in uso, anche per i dispositivi futuri sconosciuti in base a qualsiasi schermata e le caratteristiche di input hanno
    - **Vantaggio**: facilmente ti permette di condividere la logica lato server in tutti i tipi di dispositivo: duplicazione minima di codice o impegno
    - **Svantaggio**: i dispositivi mobili sono sostanzialmente diversi dai dispositivi desktop che sarebbe davvero opportuno pagine per dispositivi mobili può essere completamente diverso dalle pagine del desktop, che mostra informazioni diverse. Tali varianti possono essere inefficiente o impossibili da soddisfare in modo affidabile attraverso CSS da solo, soprattutto considerando la modalità in modo incoerente dispositivi meno recenti di interpretare le regole CSS. Questo vale in particolare degli attributi di CSS 3.
    - **Svantaggio**: non fornisce supporto per flussi di lavoro per dispositivi diversi e diverse per la logica lato server. È possibile, ad esempio, implementare un acquisti carrello checkout flusso di lavoro semplificato per gli utenti mobili mezzo CSS da solo.
    - **Svantaggio**: uso non efficiente della larghezza di banda. Il server potrebbe essere necessario trasmettere il markup e stili che si applicano a tutti i dispositivi possibili, anche se il dispositivo di destinazione userà solo un subset di tali informazioni.
3. ***Risolvere il problema nel server* –** se il server sa quale dispositivo accede a tale colonna, o almeno le caratteristiche del dispositivo, ad esempio le dimensioni dello schermo e il metodo di input, e si tratti di un dispositivo mobile, può essere eseguita una logica diversa e markup HTML diverso di output. 

    - **Vantaggio:** flessibilità massima. Non sono previsti limiti per quanto è possibile variare la logica lato server per dispositivi mobili o ottimizzare i tag per il layout desiderato, specifici del dispositivo.
    - **Vantaggio:** uso efficiente della larghezza di banda. È sufficiente trasmettere il markup e informazioni sugli stili che userà il dispositivo di destinazione.
    - **Svantaggio:** talvolta forza la ripetizione di lavoro o di codice (ad esempio, rendendo si creare copie simile ma leggermente diverse del pagine Web Form o le visualizzazioni MVC). Dove possibile, che è necessario tenere out comuni per la logica in un livello sottostante o servizio, ma comunque alcune parti del codice dell'interfaccia utente o markup potrebbe essere necessario essere duplicati e quindi mantenuti in parallelo.
    - **Svantaggio:** rilevamento dispositivo non è semplice. Richiede un elenco o un database di tipi di dispositivi noti e le relative caratteristiche (che potrebbero non essere sempre aggiornati perfettamente) e non sempre corrispondono in modo accurato ogni richiesta in ingresso. Questo documento descrive alcune opzioni e i relativi problemi comuni in un secondo momento.

Per ottenere risultati ottimali, la maggior parte degli sviluppatori trovano che dovranno combinare opzioni (2) e (3). Lievi differenze stilistiche vengono gestite meglio nel client mediante CSS o JavaScript uniforme, mentre le principali differenze nel markup, del flusso di lavoro o i dati vengono implementate in modo più efficace nel codice lato server.

### <a name="this-paper-focuses-on-server-side-techniques"></a>Questo documento sono descritte le tecniche di lato server

Poiché Web Form ASP.NET e MVC sono entrambe le tecnologie principalmente sul lato server, in questo white paper verrà illustrato le tecniche di lato server che consentono di produrre codice diverso e per la logica per i browser per dispositivi mobili. Naturalmente, è anche possibile combinare questa con una qualsiasi tecnica lato client (ad esempio, CSS 3 query sui supporti, miglioramento progressivo JavaScript), ma che è più una questione di progettazione di siti web di sviluppo ASP.NET.

## <a name="browser-and-device-detection"></a>Rilevamento del browser e dispositivi

Il chiave prerequisito per tutte le tecniche di lato server per supportare i dispositivi mobili è sapere quali dispositivi Usa il visitatore. In effetti, ancora migliore rispetto a conoscere il numero di modello e produttore del dispositivo è conoscere la *caratteristiche* del dispositivo. Caratteristiche possono includere:

- Si tratta di un dispositivo mobile?
- Metodo di input (mouse e tastiera, tocco, tastierino, joystick,...)
- Dimensioni dello schermo (fisicamente e, in pixel)
- Formati di supporti e i dati supportati
- E così via.

Si consiglia di prendere decisioni basate sulle caratteristiche del numero di modello, perché quindi utenti saranno dunque dotati di gestire dispositivi futuri.

### <a name="using-aspnets-built-in-browser-detection-support"></a>Utilizzo di ASP. Supporto per il rilevamento di NET browser predefiniti

Gli sviluppatori Web Form ASP.NET e MVC possono immediatamente individuare importanti caratteristiche di un browser visitando controllando le proprietà del *Request* oggetto. Ad esempio, vedere

- Request.Browser.IsMobileDevice
- Request.Browser.MobileDeviceManufacturer, Request.Browser.MobileDeviceModel
- Request.Browser.ScreenPixelsWidth
- Request.Browser.SupportsXmlHttp
- ...e molti altri

Dietro le quinte, la piattaforma ASP.NET corrisponde in ingresso *User-Agent* intestazione HTTP (UA) contro espressioni regolari in un set di file XML di definizione del Browser. Per impostazione predefinita, la piattaforma include le definizioni per diversi dispositivi mobili comuni ed è possibile aggiungere file di definizione Browser personalizzati per gli altri utenti che si desidera riconoscere. Per altre informazioni, vedere la pagina MSDN [controlli Server Web ASP.NET e le funzionalità del Browser](https://msdn.microsoft.com/library/x3k2ssx2.aspx).

### <a name="using-the-wurfl-device-database-via-51degreesmobi-foundation"></a>Utilizzando il database di dispositivo WURFL tramite 51Degrees.mobi Foundation

Mentre ASP. Supporto per il rilevamento di NET browser predefinito sarà sufficiente per molte applicazioni, sono disponibili due casi principali potrebbe non essere sufficiente:

- ***Si desidera identificare i dispositivi più recenti***(senza creare manualmente i file di definizione del Browser relativa). Si noti che i file di definizione del Browser di .NET 4 non sono abbastanza recenti per riconoscere Apple iPad, telefoni Android, il browser Opera Mobile o Windows Phone 7.
- ***Sono necessarie informazioni più dettagliate sulle funzionalità del dispositivo***. Potrebbe essere necessario conoscere il metodo di input di un dispositivo (ad esempio, tocco vs tastierino) o quali audio formati il browser supporta. Queste informazioni non sono disponibile nei file di definizione del Browser standard.

Il [ *Wireless Universal Resource File* project (WURFL)](http://wurfl.sourceforge.net/) gestisce molte più aggiornate e dettagliate informazioni sui dispositivi mobili in uso oggi stesso.

La buona notizia per gli sviluppatori .NET è tale piano ASP. Funzionalità di rilevamento del browser di NET è estensibile, pertanto è possibile migliorare in modo da risolvere tali problemi. Ad esempio, è possibile aggiungere open source [ *51Degrees.mobi Foundation* ](https://github.com/51Degrees/dotNET-Device-Detection) libreria al progetto. È un'interfaccia IHttpModule ASP.NET o Browser funzionalità Provider (utilizzabile in applicazioni di Web Form e MVC), che legge i dati WURFL e lo collega in ASP direttamente. Meccanismo di rilevamento di NET browser incorporato. Dopo aver installato il modulo *Request* improvvisamente conterrà informazioni molto più precise e dettagliate: lo riconoscerà molti dei dispositivi descritto in precedenza ed elencare le relative funzionalità (inclusi correttamente funzionalità aggiuntive come metodo di input). Vedere la documentazione del progetto per altri dettagli.

## <a name="how-web-forms-applications-can-present-mobile-specific-pages"></a>Come le applicazioni Web Form possono presentare pagine specifiche per dispositivi mobili

Per impostazione predefinita, ecco come una nuova applicazione Web Form viene visualizzato nei dispositivi mobili più comuni:

![](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/_static/image1.png)

Chiaramente, né layout sembra molto per dispositivi mobili: questa pagina è stata progettata per un monitor di grandi dimensioni e orientamento orizzontale, non per schermi di piccole dimensioni e orientati alle verticale. Quindi, cosa puoi fare su di esso?

Come illustrato in precedenza in questo documento, esistono molti modi per personalizzare le pagine per i dispositivi mobili. Alcune tecniche sono basati su server, eseguito da altri utenti nel client.

### <a name="creating-a-mobile-specific-master-page"></a>Creazione di una pagina master specifici del dispositivo mobile

A seconda dei requisiti, è possibile usare la stessa di Web Form per tutti i visitatori, ma disporre di due pagine master distinte: una per i visitatori desktop, un altro per i visitatori per dispositivi mobili. Questo offre la flessibilità necessaria per modificare il foglio di stile CSS o nel markup HTML di primo livello per soddisfare i dispositivi mobili, senza dover duplicare qualsiasi logica della pagina.

Si tratta di un'operazione semplice. Ad esempio, è possibile aggiungere un gestore PreInit simile al seguente in un Web Form:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample1.cs)]

A questo punto, creare una pagina master denominata Mobile.Master nella cartella principale dell'applicazione e verrà usato quando viene rilevato un dispositivo mobile. Pagina master per dispositivi mobili può fare riferimento a un foglio di stile CSS specifici del dispositivo mobile se necessario. I visitatori desktop continueranno a vedere la pagina master predefinita, non quello per dispositivi mobili.

### <a name="creating-independent-mobile-specific-web-forms"></a>Creazione indipendente specifici del dispositivo mobile Web Form

Per la massima flessibilità, è possibile andare molto oltre all'appena visto le pagine master separate per diversi tipi di dispositivo. È possibile implementare due *separare completamente insiemi di pagine Web Form* : uno impostato per i browser desktop, un altro set per dispositivi mobili. Questa situazione è ideale se si vuole presentare informazioni molto diverse o flussi di lavoro ai visitatori per dispositivi mobili. La parte restante di questa sezione viene descritto questo approccio nel dettaglio.

Se che si dispone già di un'applicazione Web Forms progettata per i browser desktop, il modo più semplice per procedere è per creare una sottocartella denominata "Mobile" all'interno del progetto e compilare pagine per dispositivi mobili non esiste. È possibile costruire un intero sito secondario, con il proprio le pagine master, fogli di stile e pagine, usando le stesse tecniche che si utilizzerebbe per qualsiasi altra applicazione Web Form. Non è necessario un equivalente per dispositivi mobili per produrre *ogni* pagina nel sito desktop; è possibile scegliere quale sottoinsieme di funzionalità come meglio credono visitatori per dispositivi mobili.

Pagine per dispositivi mobili possono condividere le risorse statiche comuni (ad esempio immagini, JavaScript o CSS file) con le normali pagine se si desidera. Poiché la cartella "Mobile" verrà *non* essere contrassegnato come applicazione separata quando ospitati in IIS (è sufficiente una semplice sottocartella delle pagine Web Form), verrà inoltre condividono gli stessi configurazione, i dati della sessione e altra infrastruttura come i pagine per desktop.

> [!NOTE]
> Poiché questo approccio implica in genere alcuni la duplicazione del codice (pagine per dispositivi mobili sono probabilmente da condividere alcune somiglianze con pagine desktop), è importante fattore di qualsiasi business per la logica o dati accesso codice comune in un livello sottostante condiviso o un servizio. In caso contrario, si imposterà un double lo sforzo di creazione e gestione dell'applicazione.


#### <a name="redirecting-mobile-visitors-to-your-mobile-pages"></a>Reindirizzamento di dispositivi mobili ai visitatori di pagine per dispositivi mobili

Spesso risulta utile reindirizzare i visitatori per dispositivi mobili per le pagine per dispositivi mobili solo la *primo* richiesta nella sessione di esplorazione (e non a ogni richiesta nella propria sessione), perché:

- È quindi possibile consentire facilmente i visitatori per dispositivi mobili accedere alle pagine desktop se lo desiderano – sufficiente inserire un collegamento nella pagina master che passa a "Versione Desktop". Il visitatore non reindirizzato a una pagina per dispositivi mobili, perché non è più la prima richiesta nella propria sessione.
- Evita il rischio che interferiscano con le richieste per tutte le risorse dinamiche condivise tra le parti del desktop e per dispositivi mobili del sito (ad esempio, se si dispone di un modulo Web più comuni che consentono di visualizzare parti mobili e desktop del sito in un IFRAME, o alcuni gestori di Ajax)

A tale scopo, è possibile inserire la logica di reindirizzamento in un **sessione\_avviare** (metodo). Ad esempio, aggiungere il metodo seguente al file Global.asax.cs:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample2.cs)]

#### <a name="configuring-forms-authentication-to-respect-your-mobile-pages"></a>Configurazione di autenticazione basata su form in modo da rispettare le pagine per dispositivi mobili

Si noti che l'autenticazione basata su form include alcune ipotesi su cui poter reindirizzare i visitatori durante e dopo il processo di autenticazione:

- Quando un utente deve essere autenticata, autenticazione basata su form reindirizzerà alla pagina dell'account di accesso del desktop, indipendentemente dal fatto che siano connessi un utente di computer desktop o portatile (perché dispone solo su un concetto *uno* URL di accesso). Supponendo che si desidera applicare uno stile la pagina di accesso per dispositivi mobili in modo diverso, è necessario ottimizzare la pagina di accesso desktop in modo che gli utenti mobili viene reindirizzato a una pagina di accesso separato per dispositivi mobili. Ad esempio, aggiungere il codice seguente per il **desktop** login pagina code-behind: 

    [!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample3.cs)]
- Dopo che un utente accede correttamente, autenticazione basata su form per impostazione predefinita reindirizzerà alla home page del desktop (perché dispone solo su un concetto *uno* pagina predefinita). È necessario ottimizzare la pagina di accesso per dispositivi mobili in modo che esegue il reindirizzamento alla home page di mobile dopo un log ha esito positivo. Ad esempio, aggiungere il codice seguente per il **mobile** login pagina code-behind: 

    [!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample4.cs)]
  
  Questo codice presuppone che la pagina contiene un controllo server di accesso denominato LoginUser, come nel modello di progetto predefinito.

### <a name="working-with-output-caching"></a>Uso di cache di Output

Se si usa la memorizzazione nella cache di output, tenere presente che per impostazione predefinita che è possibile che un utente del desktop accedere a un determinato URL (causando l'output da memorizzare nella cache), seguita da un utente per dispositivi mobili che quindi riceve l'output di desktop memorizzato nella cache. Questo avviso si applica solo variabili di pagina master per tipo di dispositivo, o implementazione completamente separato Web Form per ogni tipo di dispositivo.

Per evitare il problema, è possibile indicare ASP.NET per variare la voce della cache in base al fatto che il visitatore Usa un dispositivo mobile. Aggiungere un parametro VaryByCustom alla dichiarazione OutputCache della pagina nel modo seguente:

[!code-aspx[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample5.aspx)]

Successivamente, definire *isMobileDevice* come una cache personalizzata parametro aggiungendo il seguente metodo di override per il file Global.asax.cs:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample6.cs)]

Ciò garantisce che per dispositivi mobili visitatori alla pagina di non ricevano output precedentemente inserire nella cache per un visitatore di un desktop.

### <a name="a-working-example"></a>Un esempio pratico

Per visualizzare queste tecniche in azione, scaricare [esempi di codice in questo white paper](https://docs.microsoft.com/aspnet/mobile/overview). L'applicazione di esempio Web Form reindirizza automaticamente un set di pagine specifici del dispositivo mobile in una sottocartella denominata per dispositivi mobili degli utenti mobili. Il markup e lo stile di tali pagine meglio è ottimizzato per i browser per dispositivi mobili, come si può notare dagli screenshot seguenti:

![](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/_static/image2.png)

Per altri suggerimenti sull'ottimizzazione di markup e CSS per i browser per dispositivi mobili, vedere la sezione "Applicazione di stili pagine per dispositivi mobili per i browser per dispositivi mobili" più avanti in questo documento.

## <a name="how-aspnet-mvc-applications-can-present-mobile-specific-pages"></a>Come le applicazioni ASP.NET MVC possono presentare pagine specifiche per dispositivi mobili

Poiché il modello Model-View-Controller decuplica la logica dell'applicazione (in controller) dalla logica di presentazione (nelle visualizzazioni), è possibile scegliere uno degli approcci seguenti per il supporto per dispositivi mobili nel codice lato server per la gestione:

1. ***Usare lo stesso controller e visualizzazioni per i browser per dispositivi mobili e desktop, ma il rendering di visualizzazioni con layout Razor diversi a seconda del tipo di dispositivo *.** Questa opzione funziona meglio se si eseguono la visualizzazione dei dati identici in tutti i dispositivi, ma desidera semplicemente specificare diversi fogli di stile CSS o modificare alcuni elementi HTML primo livello per dispositivi mobili.
2. ***Usare gli stessi controller per i browser per dispositivi mobili e desktop, ma il rendering di visualizzazioni diverse a seconda del tipo di dispositivo***. Questa opzione funziona meglio se si ha la visualizzazione all'incirca gli stessi dati e fornire la stesse dei flussi di lavoro per gli utenti finali, ma vuole eseguire il rendering del markup HTML molto diversi per soddisfare il dispositivo in uso.
3. ***Creare aree distinte per i browser desktop e per dispositivi mobili, implementazione indipendente dal controller e visualizzazioni per ogni *.** Questa opzione funziona meglio se si ha la visualizzazione di schermi molto diversi, contenenti informazioni diverse e iniziali utente tramite diversi flussi di lavoro ottimizzato per il relativo tipo di dispositivo. È possibile che alcune ripetizioni di codice, ma è possibile ridurre al minimo eseguendo il factoring comuni per la logica in un servizio o un livello sottostante.

Se vuoi sfruttare il **primo** opzione e modificare solo il layout di Razor per ogni tipo di dispositivo, è molto semplice. È sufficiente modificare il \_Viewstart file come segue:

[!code-cshtml[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample7.cshtml)]

Ora è possibile creare un layout specifici del dispositivo mobile denominato \_LayoutMobile.cshtml con una struttura della pagina e CSS regole ottimizzate per dispositivi mobili.

Se vuoi sfruttare il **secondo** seleziona l'opzione di rendering di visualizzazioni completamente diverso in base al tipo di dispositivo del visitatore, vedere [post di blog di Scott Hanselman](http://www.hanselman.com/blog/ABetterASPNETMVCMobileDeviceCapabilitiesViewEngine.aspx).

Il resto di questo white paper è incentrato sulla **terzo** opzione-creazione di controller separato *e* visualizzazioni per dispositivi mobili, non è possibile controllare esattamente il sottoinsieme di funzionalità è disponibile per i visitatori per dispositivi mobili.

### <a name="setting-up-a-mobile-area-within-your-aspnet-mvc-application"></a>Configurazione di un'area all'interno dell'applicazione ASP.NET MVC per dispositivi mobili

È possibile aggiungere un'area denominata "Mobile" a un'applicazione ASP.NET MVC esistente in modo normale: pulsante destro del mouse sul nome del progetto in Esplora soluzioni e quindi scegliere Aggiungi à Area. È quindi possibile aggiungere controller e visualizzazioni come si farebbe per qualsiasi altra area all'interno di un'applicazione ASP.NET MVC. Ad esempio, aggiungere al portale Mobile un nuovo controller denominato HomeController di agire come home page per i visitatori per dispositivi mobili.

### <a name="ensuring-the-url-mobile-reaches-the-mobile-homepage"></a>Assicurando il /Mobile URL raggiunge la home page per dispositivi mobili

Se si desidera /Mobile l'URL per raggiungere l'operazione sull'indice in HomeController all'interno dell'area di dispositivi mobili, è necessario apportare due piccole modifiche alla configurazione del routing. In primo luogo, aggiornare la classe MobileAreaRegistration in modo che HomeController è il controller predefinito nella tua zona per dispositivi mobili, come illustrato nel codice seguente:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample8.cs)]

Ciò significa la home page per dispositivi mobili a questo punto si troverà /Mobile, piuttosto che per dispositivi mobili/Home, in quanto "Home" è ora l'in modo implicito i nome controller predefinito per le pagine mobile.

Successivamente, si noti che aggiungendo un secondo HomeController all'applicazione (ad esempio, per dispositivi mobili quello, oltre a desktop uno esistente), si verrà hanno interferito con regolari home page del desktop. Avrà esito negativo con l'errore "*sono stati trovati più tipi corrispondenti ai controller di denominato 'Home'*". Per risolvere questo problema, aggiornare la configurazione di routing di livello superiore (in Global.asax.cs) per specificare che la classe HomeController desktop devono avere la priorità quando vi è ambiguità:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample9.cs)]

Ora l'errore non viene più di stoccaggio e l'URL http://<em>yoursite</em>/ raggiungeranno la home page di desktop e http://<em>yoursite</em>/mobile/ raggiungeranno la home page per dispositivi mobili.

### <a name="redirecting-mobile-visitors-to-your-mobile-area"></a>Reindirizzamento i visitatori per dispositivi mobili al portale per dispositivi mobili

Esistono molti diversi punti di estendibilità in ASP.NET MVC, quindi esistono molti modi possibili per inserire la logica di reindirizzamento. Un'opzione brillante consiste nel creare un attributo di filtro, [RedirectMobileDevicesToMobileArea], che esegue un reindirizzamento, se vengono soddisfatte le condizioni seguenti:

1. È la prima richiesta nella sessione dell'utente (ad esempio, Session.IsNewSession è uguale a true)
2. La richiesta proviene da un browser per dispositivi mobili (ad esempio, Request.Browser.IsMobileDevice è uguale a true)
3. L'utente non richiede già una risorsa nell'area di dispositivi mobili (ad esempio, il *percorso* parte dell'URL non inizia con /Mobile)

L'esempio scaricabile incluso in questo white paper include un'implementazione di questa logica. Viene implementato come un filtro di autorizzazione, derivato da AuthorizeAttribute, che funziona correttamente anche se si usa la memorizzazione nella cache di output (in caso contrario, se un visitatore desktop prima accessi un determinato URL, l'output desktop potrebbero essere memorizzato nella cache e quindi servito da le successive visitatori per dispositivi mobili).

Così com'è un filtro, è possibile scegliere di applicarlo a specifici controller e azioni, ad esempio,

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample10.cs)]

… oppure è possibile applicarlo a tutti i controller e azioni come un MVC 3 *filtro globale* nel file Global.asax.cs:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample11.cs)]

L'esempio scaricabile illustra inoltre come è possibile creare sottoclassi di questo attributo che reindirizzano a posizioni specifiche all'interno di area per dispositivi mobili. Ciò significa, ad esempio, che è possibile:

- Registrare un filtro globale come illustrato sopra che invia per dispositivi mobili visitatori alla home page per dispositivi mobili per impostazione predefinita.
- Si applicano anche un filtro [RedirectMobileDevicesToMobileProductPage] speciale a un'azione "Visualizza prodotto" che accetta visitatori per dispositivi mobili per la versione mobile di qualsiasi pagina del prodotto che aveva richiesti.
- Altre sottoclassi speciale del filtro si applicano anche ad azioni specifiche, reindirizzando i visitatori per dispositivi mobili alla pagina equivalente per dispositivi mobili

### <a name="configuring-forms-authentication-to-respect-your-mobile-pages"></a>Configurazione di autenticazione basata su form in modo da rispettare le pagine per dispositivi mobili

Se si usa l'autenticazione basata su form, si noti che quando un utente deve accedere, automaticamente l'utente viene reindirizzato a un singolo "accesso" URL specifico, per impostazione predefinita **/Account/LogOn**. Ciò significa che gli utenti mobili potrebbero essere reindirizzati all'azione desktop "Accedi".

Per evitare questo problema, aggiungere logica all'azione di "accesso" desktop in modo che gli utenti mobili viene reindirizzato nuovamente a un'azione di "accesso" per dispositivi mobili. Se si usa il modello di applicazione MVC di ASP.NET predefinito, aggiornare l'azione di accesso del AccountController come segue:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample12.cs)]

… e quindi implementare un'azione di "accesso" specifici del dispositivo mobile appropriato in un controller denominato AccountController nella propria area per dispositivi mobili.

### <a name="working-with-output-caching"></a>Uso di cache di Output

Se si usa il filtro [OutputCache], è necessario forzare la voce della cache per variano in base al tipo di dispositivo. Ad esempio, scrivere:

[!code-javascript[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample13.js)]

Aggiungere quindi il metodo seguente al file Global.asax.cs:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample14.cs)]

Ciò garantisce che per dispositivi mobili visitatori alla pagina di non ricevano output precedentemente inserire nella cache per un visitatore di un desktop.

### <a name="a-working-example"></a>Un esempio pratico

Per visualizzare queste tecniche in azione, scaricare [codice in questo white paper associati esempi](https://docs.microsoft.com/aspnet/mobile/overview). L'esempio include un'applicazione ASP.NET MVC 3 (versione finale candidata) migliorata per supportare i dispositivi mobili usando i metodi descritti in precedenza.

## <a name="further-guidance-and-suggestions"></a>Ulteriori indicazioni e suggerimenti

La discussione seguente si applica sia al Web Form e gli sviluppatori MVC che usano le tecniche illustrate in questo documento.

### <a name="enhancing-your-redirection-logic-using-51degreesmobi-foundation"></a>Migliorare la logica di reindirizzamento usando 51Degrees.mobi Foundation

La logica di reindirizzamento illustrata in questo documento può essere perfettamente sufficiente per l'applicazione, ma non funziona se è necessario disabilitare le sessioni o con un browser per dispositivi mobili che rifiuta i cookie (questi non possono avere sessioni), perché non conoscere se una determinata richiesta è il primo da tale visitatore.

Si è già appreso come 51Degrees.mobi l'open source Foundation possono migliorare l'accuratezza di ASP. Rilevamento del browser di NET. Include anche un integrato che consente di reindirizzare i visitatori per dispositivi mobili a indirizzi specifici configurati nel file Web. config. È in grado di funzionare senza a seconda delle sessioni ASP.NET (e pertanto i cookie) mediante l'archiviazione di un log temporaneo degli hash delle intestazioni HTTP e gli indirizzi IP dei visitatori, in modo che sappia se ogni richiesta è il primo da un determinato visitatore.

Il seguente elemento aggiunto alla sezione del file Web. config fiftyOne reindirizzerà la prima richiesta da un dispositivo mobile rilevato alla pagina ~ / Mobile/Default.aspx. Tutte le richieste di pagine nella cartella per dispositivi mobili verranno *non* reindirizzato, indipendentemente dal tipo di dispositivo. Se il dispositivo mobile è rimasto inattivo per un periodo di 20 minuti o più il dispositivo verranno rimosse e le richieste successive verranno considerate come quelli nuovi per gli scopi del reindirizzamento.

[!code-xml[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample15.xml)]

Per altre informazioni, vedere [51degrees.mobi documentazione Foundation](https://github.com/51Degrees/dotNET-Device-Detection).

> [!NOTE]
> Si *possibile* funzionalità di reindirizzamento Usa 51Degrees.mobi Foundation in applicazioni ASP.NET MVC, ma è necessario definire la configurazione di reindirizzamento in termini di normale URL, non in termini di parametri di routing o inserendo i filtri MVC per le azioni. Infatti (al momento della scrittura) 51Degrees.mobi Foundation non riconosce i filtri o routing.


### <a name="disabling-transcoders-and-proxy-servers"></a>La disabilitazione di server Proxy e Transcodificatori

Gli operatori di rete per dispositivi mobili hanno due obiettivi ampia nell'approccio a internet per dispositivi mobili:

1. Specificare come il contenuto pertinente molto presto
2. Ottimizzare il numero di clienti che possono condividere la larghezza di banda di rete limitata radio

Poiché la maggior parte delle pagine web sono stati progettati per schermi di grandi dimensioni desktop e le connessioni a banda larga fissa fast-line, molti operatori utilizzano *transcodificatori* oppure *i server proxy* che modificare dinamicamente il contenuto web. È possibile modificare il markup HTML o CSS in base alle proprie schermi più piccoli (soprattutto per "funzionalità telefoni" che non dispongono di potenza di elaborazione per gestire layout complessi) e potrebbero ricomprimerà le immagini (riducendo in modo significativo la qualità) per migliorare la velocità di recapito di pagina.

Ma se è stata acquisita l'impegno necessario per produrre una versione ottimizzata per dispositivi mobili del sito, non è opportuno all'operatore di rete di interferire con questo qualsiasi ulteriore. È possibile aggiungere la riga seguente alla pagina\_evento di caricamento in qualsiasi Web Form ASP.NET:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample16.cs)]

In alternativa, per un controller MVC ASP.NET, è possibile aggiungere l'override del metodo seguente in modo da essere applicata a tutte le azioni nell'ambito del controller:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample17.cs)]

Il messaggio HTTP risultante informa transcodificatori conformi W3C e i proxy non è necessario modificare il contenuto. Naturalmente, non c'è garanzia che gli operatori di rete mobile rispetterà questo messaggio.

### <a name="styling-mobile-pages-for-mobile-browsers"></a>Lo stile delle pagine per dispositivi mobili per i browser per dispositivi mobili

È oltre l'ambito di questo documento per descrivere in dettaglio quali tipologie di lavoro di markup HTML in modo corretto o quali tecniche di progettazione web ottimizzare l'usabilità in dispositivi specifici. Questo è iscrizione all'utente di individuare un layout sufficientemente semplice, ottimizzato per una schermata di dimensioni per dispositivi mobili, senza utilizzare non affidabile HTML o CSS trucchi di posizionamento. Una tecnica importante la pena sottolineare, tuttavia, è il *metatag viewport*.

Determinati browser per dispositivi mobili moderni, nelle pagine di web visualizzato impegno destinato dei desktop, il rendering della pagina in un'area di disegno virtuale, detto anche "viewport" (ad esempio, il riquadro di visualizzazione virtuale è 980 pixel di larghezza su iPhone e 850 pixel di larghezza in Opera Mobile per impostazione predefinita) e quindi ridurre il risultato per adattarla alle pixel fisici della schermata. L'utente può quindi eseguire lo zoom avanti e pan intorno a tale riquadro di visualizzazione. Ciò offre il vantaggio che consente al browser di visualizzare la pagina di layout desiderato, ma è anche comporta lo svantaggio che forza lo zoom e panoramiche, che non è pratico per l'utente. Se si sta progettando per dispositivi mobili, è preferibile alla progettazione per uno schermo stretto in modo che non lo zoom o lo scorrimento orizzontale è necessario.

Un modo per indicare al browser per dispositivi mobili ampiezza deve essere il viewport è tramite il conforme allo standard *viewport* tag meta. Ad esempio, se si aggiungere quanto segue alla sezione HEAD della pagina,

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample18.html)]

… quindi il supporto browser di smartphone layout di pagina in un'area di disegno virtuale a livello di 480 pixel. Ciò significa che, se gli elementi HTML definiscono le relative larghezze in termini di percentuale, le percentuali verranno interpretate in base a questa larghezza 480 pixel, non la larghezza del riquadro di visualizzazione predefinito. Di conseguenza, l'utente è meno probabile che è necessario eseguire lo zoom e panoramica orizzontalmente: notevolmente migliorando l'esperienza di navigazione per dispositivi mobili.

Se si desidera che la larghezza del viewport in modo che corrispondano pixel fisico del dispositivo, è possibile specificare quanto segue:

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample19.html)]

Per funzionare correttamente, non è necessario forzare in modo esplicito gli elementi a superino la larghezza specificata (ad esempio, tramite un *larghezza* attributo o una proprietà CSS), in caso contrario, il browser sarà costretti a usare indipendentemente dal fatto che un riquadro di visualizzazione più grande. Vedere anche: [altri dettagli sul tag viewport non standard](https://developer.apple.com/library/safari/#documentation/AppleApplications/Reference/SafariHTMLRef/Articles/MetaTags.html).

Supportano più moderni Smartphone *orientamento doppio*: può essere mantenuti in modalità verticale o orizzontale. Pertanto, è importante non presupporre la larghezza dello schermo in pixel. Non ancora dare per scontato che la larghezza dello schermo è fissa, perché l'utente può orientare nuovamente il dispositivo mentre si trovano nella pagina.

Dispositivi Windows Mobile e Blackberry meno recenti possono accettare anche i seguenti tag meta nell'intestazione di pagina per informarli del contenuto è stata ottimizzata per dispositivi mobili e pertanto non devono essere trasformate.

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample20.html)]

## <a name="additional-resources"></a>Risorse aggiuntive

Per un elenco di emulatori di dispositivi mobili e i simulatori è possibile usare per testare l'applicazione web ASP.NET per dispositivi mobili, vedere la pagina [simulare più diffusi dispositivi mobili per i test](../mobile/device-simulators.md).

## <a name="credits"></a>Crediti

- Autore principale: Steven Sanderson
- I revisori / altri autori del contenuto: James Rosewell, Mikael Söderström, Scott Hanselman e Scott Hunter
