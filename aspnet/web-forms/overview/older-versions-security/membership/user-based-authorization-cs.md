---
uid: web-forms/overview/older-versions-security/membership/user-based-authorization-cs
title: Autorizzazione basata sull'utente (c#) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione si esaminerà la limitazione dell'accesso alle pagine e imponendo limitazioni sulle funzionalità a livello di pagina tramite una serie di tecniche.
ms.author: riande
ms.date: 01/18/2008
ms.assetid: 3c815a9e-2296-4b9b-b945-776d54989daa
msc.legacyurl: /web-forms/overview/older-versions-security/membership/user-based-authorization-cs
msc.type: authoredcontent
ms.openlocfilehash: 18aad3bde961747af64de2b76ff83feb767597d9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837144"
---
<a name="user-based-authorization-c"></a>Autorizzazione basata sull'utente (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare il codice](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_07_CS.zip) o [Scarica il PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial07_UserAuth_cs.pdf)

> In questa esercitazione si esaminerà la limitazione dell'accesso alle pagine e imponendo limitazioni sulle funzionalità a livello di pagina tramite una serie di tecniche.


## <a name="introduction"></a>Introduzione

La maggior parte delle applicazioni web che offrono l'account utente di eseguire questa operazione nell'ambito per limitare alcuni visitatori di accedere a determinate pagine all'interno del sito. Nei siti bacheca più online, ad esempio, tutti gli utenti - anonimi e autenticati - siano in grado di visualizzare il post della bacheca, ma solo gli utenti autenticati possono visitare la pagina web per creare un nuovo post. E possono essere presenti pagine amministrative che sono accessibili a un determinato utente (o un set specifico di utenti) solo. Inoltre, la funzionalità a livello di pagina può variare in base a utente. Quando si visualizza un elenco dei post, gli utenti autenticati vengono visualizzati un'interfaccia per la valutazione di ogni post, mentre questa interfaccia non è disponibile per i visitatori anonimi.

ASP.NET semplifica definire le regole di autorizzazione basata sull'utente. Con un minimo di markup in `Web.config`, pagine web specifiche o intere directory possono essere bloccate in modo che solo sono accessibili a un subset di utenti specificato. Funzionalità a livello di pagina può essere attivata o disattivata in base sull'utente attualmente connesso in modo dichiarativo e a livello di codice.

In questa esercitazione si esaminerà la limitazione dell'accesso alle pagine e imponendo limitazioni sulle funzionalità a livello di pagina tramite una serie di tecniche. Iniziamo!

## <a name="a-look-at-the-url-authorization-workflow"></a>Esaminare il flusso di lavoro di autorizzazione URL

Come descritto nel [ *una panoramica dell'autenticazione basata su form* ](../introduction/an-overview-of-forms-authentication-cs.md) esercitazione, quando il runtime ASP.NET elabora una richiesta per una risorsa ASP.NET richiesta genera un numero di eventi durante il ciclo di vita. *Moduli HTTP* sono le classi gestite il cui codice viene eseguito in risposta a un particolare evento del ciclo di vita di richiesta. ASP.NET viene fornito con un numero di moduli HTTP che eseguono le attività essenziali dietro le quinte.

È uno di tali moduli HTTP [ `FormsAuthenticationModule` ](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx). Come illustrato nelle esercitazioni precedenti, la funzione principale del `FormsAuthenticationModule` consiste nel determinare l'identità della richiesta corrente. Questa operazione viene eseguita controllando il ticket di autenticazione form, che è che si trova in un cookie o incorporato all'interno dell'URL. Questa identificazione ha luogo durante la [ `AuthenticateRequest` evento](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx).

Un altro modulo HTTP importante è il [ `UrlAuthorizationModule` ](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx), che viene generato in risposta al [ `AuthorizeRequest` evento](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx) (situazione che si verifica dopo il `AuthenticateRequest` eventi). Il `UrlAuthorizationModule` esamina il markup di configurazione in `Web.config` per determinare se l'identità corrente dispone dell'autorità di visitare la pagina specificata. Questo processo è detto *autorizzazione URL*.

Esamina la sintassi per le regole di autorizzazione URL nel passaggio 1, ma prima è possibile esaminare cosa la `UrlAuthorizationModule` viene a seconda del fatto che la richiesta è autorizzata o non. Se il `UrlAuthorizationModule` determina che la richiesta è autorizzata, quindi non esegue alcuna operazione e la richiesta continua attraverso il ciclo di vita. Tuttavia, se la richiesta è *non* autorizzato, il `UrlAuthorizationModule` interrompe il ciclo di vita e indica il `Response` oggetto da restituire un [HTTP 401 non autorizzato](http://www.checkupdown.com/status/E401.html) lo stato. Quando si usa l'autenticazione basata su form questo stato HTTP 401 non viene mai restituito al client perché se le `FormsAuthenticationModule` rileva un HTTP 401 è stato modificato da un' [reindirizzamento 302 HTTP](http://www.checkupdown.com/status/E302.html) alla pagina di accesso.

La figura 1 illustra il flusso di lavoro della pipeline ASP.NET, il `FormsAuthenticationModule`e il `UrlAuthorizationModule` quando arriva una richiesta non autorizzata. In particolare, la figura 1 mostra una richiesta da un visitatore anonimo per `ProtectedPage.aspx`, ovvero una pagina che nega l'accesso agli utenti anonimi. Poiché è anonimo, il visitatore di `UrlAuthorizationModule` interrompe la richiesta e restituisce uno stato HTTP 401 non autorizzato. Il `FormsAuthenticationModule` quindi converte di stato 401 in 302 reindirizzamento alla pagina di accesso. Dopo che l'utente viene autenticato tramite la pagina di accesso, si viene reindirizzati al `ProtectedPage.aspx`. Questa volta il `FormsAuthenticationModule` identifica l'utente in base a esso il ticket di autenticazione. Ora che il visitatore dell'autenticazione, il `UrlAuthorizationModule` consente l'accesso alla pagina.


[![L'autenticazione basata su form e il flusso di lavoro di autorizzazione URL](user-based-authorization-cs/_static/image2.png)](user-based-authorization-cs/_static/image1.png)

**Figura 1**: flusso di lavoro autorizzazione URL e l'autenticazione basata su form ([fare clic per visualizzare l'immagine con dimensioni normali](user-based-authorization-cs/_static/image3.png))


Figura 1 viene illustrata l'interazione che si verifica quando un visitatore anonimo tenta di accedere a una risorsa che non è disponibile agli utenti anonimi. In tal caso, il visitatore anonimo viene reindirizzato alla pagina di accesso con la pagina che ha provato a visitare specificato nella stringa di query. Una volta che l'utente è connesso correttamente, Anna verrà reindirizzata automaticamente alla risorsa che utente è stato inizialmente prova a visualizzare.

Quando viene effettuata la richiesta non autorizzata da un utente anonimo, questo flusso di lavoro è semplice e facile per il visitatore comprendere cosa è accaduto e perché. Ma tenere presente che il `FormsAuthenticationModule` reindirizzerà *qualsiasi* unauthorized utente alla pagina di accesso, anche se la richiesta viene effettuata da un utente autenticato. Ciò può comportare un'esperienza utente poco chiaro se un utente autenticato tenta di visitare una pagina per cui Anna non dispone dell'autorità.

Si supponga che il sito Web Microsoft aveva le regole di autorizzazione URL configurate in modo che la pagina ASP.NET `OnlyTito.aspx` era accessibile solo a Tito. A questo punto, si supponga che Sam visita il sito esegue l'accesso e quindi tentano di visitare `OnlyTito.aspx`. Il `UrlAuthorizationModule` arresterà il ciclo di vita di richiesta e restituire uno stato HTTP 401 non autorizzato, quali il `FormsAuthenticationModule` rileverà e quindi reindirizzare Sam alla pagina di accesso. Poiché Sam ha già effettuato l'accesso, tuttavia, Anna lecito chiedersi perché Anna è stata inviata nuovamente alla pagina di accesso. Anna potrebbe motivo che le proprie credenziali di account di accesso sono state perse in qualche modo, o che ha immesso le credenziali non valide. Se Sam riaccedono le proprie credenziali dalla pagina di accesso Anna verrà connesso (nuovo) e reindirizzata al `OnlyTito.aspx`. Il `UrlAuthorizationModule` rileverà che Sam non è possibile visitare questa pagina e Anna torna alla pagina di accesso.

La figura 2 illustra questo flusso di lavoro poco chiaro.


[![Il flusso di lavoro predefinita può causare un ciclo di confuso](user-based-authorization-cs/_static/image5.png)](user-based-authorization-cs/_static/image4.png)

**Figura 2**: The Default del flusso di lavoro può causare un ciclo di un processo piuttosto complesso ([fare clic per visualizzare l'immagine con dimensioni normali](user-based-authorization-cs/_static/image6.png))


Il flusso di lavoro illustrato nella figura 2 può rapidamente befuddle anche la maggior parte dei computer esperti visitatore. Esamineremo i modi per evitare questa confusione ciclo nel passaggio 2.

> [!NOTE]
> ASP.NET utilizza due meccanismi per stabilire se l'utente corrente possa accedere a una particolare pagina web: autorizzazione URL e i file di autorizzazione. I file di autorizzazione viene implementato dal [ `FileAuthorizationModule` ](https://msdn.microsoft.com/library/system.web.security.fileauthorizationmodule.aspx), che determina l'autorità consultando i file richiesti gli ACL. I file di autorizzazione più comunemente utilizzato con autenticazione di Windows perché gli ACL sono le autorizzazioni valide per gli account di Windows. Quando si usa l'autenticazione basata su form, vengono eseguite tutte le richieste a livello di sistema del sistema operativo e file per lo stesso account di Windows, indipendentemente dall'utente che visitano il sito. Poiché questa serie di esercitazioni è incentrata sull'autenticazione basata su form, abbiamo non verranno esaminati i file di autorizzazione.


### <a name="the-scope-of-url-authorization"></a>L'ambito di autorizzazione URL

Il `UrlAuthorizationModule` è codice che fa parte di ASP.NET runtime gestito. Prima della versione 7 di Microsoft [Internet Information Services (IIS)](https://www.iis.net/) server web, si è verificato un ostacolo distinto tra pipeline HTTP dell'IIS e la pipeline di ASP.NET runtime. In breve, in IIS 6 e versioni precedenti, ASP. Di NET `UrlAuthorizationModule` venga eseguita solo quando una richiesta viene delegata da IIS per il runtime di ASP.NET. Per impostazione predefinita, IIS elabora il contenuto statico, ad esempio pagine HTML e CSS, JavaScript e i file di immagine - e passa solo le richieste per il runtime di ASP.NET quando una pagina con estensione `.aspx`, `.asmx`, o `.ashx` viene richiesto.

Le pipeline di IIS 7, tuttavia, consente di integrata di IIS e ASP.NET. Con alcune impostazioni di configurazione è possibile configurare IIS 7 per richiamare il `UrlAuthorizationModule` per *tutti* richieste, vale a dire che è possono definire regole di autorizzazione URL per i file di qualsiasi tipo. IIS 7 include inoltre il proprio motore di autorizzazione URL. Per altre informazioni sull'integrazione di ASP.NET e funzionalità di autorizzazione URL nativa di IIS 7, vedere [Understanding IIS7 URL autorizzazione](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization). Per un'analisi più approfondita integrazione ASP.NET e IIS 7, procurarsi una copia del libro di Shahram Khosravi *Professional IIS 7 e programmazione integrata ASP.NET* (ISBN: 978 0470152539).

In breve, nelle versioni precedenti di IIS 7, le regole di autorizzazione URL vengono applicate solo alle risorse gestite dal runtime di ASP.NET. Ma con IIS 7 è possibile usare funzionalità di autorizzazione URL nativo di IIS o l'integrazione ASP. Di NET `UrlAuthorizationModule` nella pipeline HTTP dell'IIS, estendendo così questa funzionalità a tutte le richieste.

> [!NOTE]
> Esistono alcune differenze meno evidenti ma importanti come ASP. Di NET `UrlAuthorizationModule` e funzionalità di autorizzazione URL di IIS 7 elaborare le regole di autorizzazione. Questa esercitazione non esamina le differenze nel modo in cui viene analizzata rispetto alle regole di autorizzazione o la funzionalità di autorizzazione URL IIS 7 il `UrlAuthorizationModule`. Per altre informazioni su questi argomenti, vedere la documentazione di IIS 7 in MSDN o al [www.iis.net](https://www.iis.net/).


## <a name="step-1-defining-url-authorization-rules-inwebconfig"></a>Passaggio 1: Definizione delle regole di autorizzazione URL in`Web.config`

Il `UrlAuthorizationModule` determina se concedere o negare l'accesso a una risorsa richiesta per una determinata identità basate sulle regole di autorizzazione URL definite nella configurazione dell'applicazione. Le regole di autorizzazione siano state digitate nel [ `<authorization>` elemento](https://msdn.microsoft.com/library/8d82143t.aspx) nel formato `<allow>` e `<deny>` elementi figlio. Ciascuna `<allow>` e `<deny>` elemento figlio è possibile specificare:

- Un utente specifico
- Un elenco delimitato da virgole di utenti
- Tutti gli utenti anonimi, rappresentati da un punto interrogativo (?)
- Tutti gli utenti, identificati da un asterisco (\*)

Il markup seguente illustra come usare le regole di autorizzazione URL per consentire agli utenti Tito e Scott e negare tutte le altre:

[!code-xml[Main](user-based-authorization-cs/samples/sample1.xml)]

Il `<allow>` elemento definisce gli utenti che sono consentiti - Tito e Scott - mentre le `<deny>` elemento indica che *tutte* agli utenti viene negato.

> [!NOTE]
> Il `<allow>` e `<deny>` elementi possono anche specificare le regole di autorizzazione per i ruoli. Verranno esaminate autorizzazione basata sui ruoli in un'esercitazione futura.


L'impostazione seguente concede l'accesso a chiunque, eccetto Sam (inclusi i visitatori anonimi):

[!code-xml[Main](user-based-authorization-cs/samples/sample2.xml)]

Per consentire solo agli utenti autenticati, usare la configurazione seguente, che nega l'accesso a tutti gli utenti anonimi:

[!code-xml[Main](user-based-authorization-cs/samples/sample3.xml)]

Le regole di autorizzazione definite all'interno di `<system.web>` elemento `Web.config` e si applicano a tutte le risorse ASP.NET nell'applicazione web. Spesso, un'applicazione ha regole di autorizzazione diverse per le diverse sezioni. Ad esempio, in un sito di e-commerce, tutti i visitatori possono consultare i prodotti, vedere recensioni dei prodotti, cercare il catalogo e così via. Tuttavia, solo gli utenti autenticati possono raggiungere estrazione o le pagine per la gestione di cronologia della spedizione. Inoltre, potrebbero esserci delle parti del sito che sono accessibili solo dal selezionare utenti, ad esempio gli amministratori del sito.

ASP.NET semplifica definire le regole di autorizzazione diverse per diversi file e cartelle nel sito. Le regole di autorizzazione specificate della cartella radice `Web.config` file si applicano a tutte le risorse ASP.NET nel sito. Tuttavia, queste impostazioni di autorizzazione predefiniti possono eseguire l'override per una particolare cartella aggiungendo un `Web.config` con un `<authorization>` sezione.

Aggiorniamo il nostro sito Web in modo che solo gli utenti autenticati possono visitare le pagine ASP.NET il `Membership` cartella. A tale scopo è necessario aggiungere un `Web.config` file per il `Membership` cartella e impostare le impostazioni di autorizzazione per negare agli utenti anonimi. Fare doppio clic il `Membership` cartella in Esplora soluzioni, scegliere il menu Aggiungi nuovo elemento dal menu di scelta rapida e aggiungere un nuovo File di configurazione Web denominato `Web.config`.


[![Aggiungere un File Web. config nella cartella di appartenenza](user-based-authorization-cs/_static/image8.png)](user-based-authorization-cs/_static/image7.png)

**Figura 3**: aggiungere un `Web.config` del File per il `Membership` cartella ([fare clic per visualizzare l'immagine con dimensioni normali](user-based-authorization-cs/_static/image9.png))


A questo punto il progetto deve contenere due `Web.config` file: uno nella directory radice e uno nel `Membership` cartella.


[![L'applicazione dovrebbe ora contenere due file Web. config](user-based-authorization-cs/_static/image11.png)](user-based-authorization-cs/_static/image10.png)

**Figura 4**: dell'applicazione dovrebbe ora contenere due `Web.config` i file ([fare clic per visualizzare l'immagine con dimensioni normali](user-based-authorization-cs/_static/image12.png))


Aggiornare il file di configurazione nel `Membership` cartella in modo che non consente l'accesso agli utenti anonimi.

[!code-xml[Main](user-based-authorization-cs/samples/sample4.xml)]

Questo è tutto qui!

Per testare questa modifica, visitare la home page in un browser e assicurarsi che è disconnesso. Poiché il comportamento predefinito di un'applicazione ASP.NET per consentire tutti i visitatori e poiché non è stata creata alcuna modifica di autorizzazione per la directory radice `Web.config` file, siamo in grado di vedere i file nella directory radice di un visitatore anonimo.

Fare clic sul collegamento di creazione degli account utente rilevato nella colonna a sinistra. Verrà visualizzata per il `~/Membership/CreatingUserAccounts.aspx`. Poiché il `Web.config` del file nei `Membership` cartella vengono definite le regole di autorizzazione da non consentire l'accesso anonimo, di `UrlAuthorizationModule` interrompe la richiesta e restituisce uno stato HTTP 401 non autorizzato. Il `FormsAuthenticationModule` questa modifica a uno stato di reindirizzamento 302, inviare alla pagina di accesso. Si noti che la pagina si stava tentando di accedere (`CreatingUserAccounts.aspx`) viene passato alla pagina di accesso tramite il `ReturnUrl` parametro querystring.


[![Poiché l'URL di autorizzazione regole proibire l'accesso anonimo, si verrà reindirizzati alla pagina di accesso](user-based-authorization-cs/_static/image14.png)](user-based-authorization-cs/_static/image13.png)

**Figura 5**: poiché l'URL di autorizzazione delle regole non consentire l'accesso anonimo, si verrà reindirizzati alla pagina di accesso ([fare clic per visualizzare l'immagine con dimensioni normali](user-based-authorization-cs/_static/image15.png))


Dopo l'accesso correttamente, si verrà reindirizzati al `CreatingUserAccounts.aspx` pagina. Questa volta il `UrlAuthorizationModule` consente l'accesso alla pagina perché non verranno più anonimi.

### <a name="applying-url-authorization-rules-to-a-specific-location"></a>Applicazione delle regole di autorizzazione URL in un percorso specifico

Le impostazioni di autorizzazione definite nel `<system.web>` sezione del `Web.config` si applicano a tutte le risorse ASP.NET in tale directory e nelle relative sottodirectory (fino a quando non in caso contrario, viene sottoposto a override da un'altra `Web.config` file). In alcuni casi, tuttavia, si può decidere di tutte le risorse ASP.NET in una determinata directory per avere una particolare configurazione dell'autorizzazione ad eccezione di uno o due pagine specifiche. Ciò può essere ottenuta mediante l'aggiunta di un `<location>` elemento `Web.config`puntandolo al file le cui regole di autorizzazione sono diversi e che definisce le regole di autorizzazione univoco al suo interno.

Per illustrare l'utilizzo di `<location>` elemento per eseguire l'override delle impostazioni di configurazione per una risorsa specifica, è possibile personalizzare le impostazioni di autorizzazione in modo che solo Tito può visitare `CreatingUserAccounts.aspx`. A tale scopo, aggiungere un `<location>` elemento per il `Membership` della cartella `Web.config` file e aggiornare il markup in modo che risulti simile al seguente:

[!code-xml[Main](user-based-authorization-cs/samples/sample5.xml)]

Il `<authorization>` elemento `<system.web>` definisce le regole di autorizzazione URL predefinito per le risorse in ASP.NET il `Membership` cartella e nelle relative sottocartelle. Il `<location>` elemento consente di eseguire l'override di queste regole per una determinata risorsa. Nel markup riportato sopra il `<location>` riferimenti a elementi di `CreatingUserAccounts.aspx` pagina e specifica l'autorizzazione delle regole ad esempio consentire Tito, ma nega a tutti gli altri.

Per testare la modifica di questa autorizzazione, per iniziare, visitare il sito Web come utente anonimo. Se si prova a visitare qualsiasi pagina il `Membership` cartella, ad esempio `UserBasedAuthorization.aspx`, il `UrlAuthorizationModule` la richiesta viene negata e si verrà reindirizzati alla pagina di accesso. Dopo l'accesso come, ad esempio, Scott, è possibile visitare qualsiasi pagina di `Membership` cartella *tranne* per `CreatingUserAccounts.aspx`. Provare a visitare `CreatingUserAccounts.aspx` connessi come tutti gli utenti ma Tito comporterà un tentativo di accesso non autorizzato, il reindirizzamento si torna alla pagina di accesso.

> [!NOTE]
> Il `<location>` deve essere presente l'elemento di fuori della configurazione `<system.web>` elemento. È necessario usare un oggetto separato `<location>` (elemento) per ogni risorsa di cui si desidera eseguire l'override le impostazioni di autorizzazione.


### <a name="a-look-at-how-theurlauthorizationmoduleuses-the-authorization-rules-to-grant-or-deny-access"></a>Esaminare il`UrlAuthorizationModule`Usa le regole di autorizzazione da concedere o negare l'accesso

Il `UrlAuthorizationModule` determina se per autorizzare una specifica identità per un determinato URL analizzando l'autorizzazione URL regole uno alla volta, a partire da quella del primo e in modo casuale. Non appena viene rilevata una corrispondenza, l'utente viene concesso o negato l'accesso, a seconda se è stata trovata la corrispondenza un `<allow>` o `<deny>` elemento. <strong>Se viene trovata alcuna corrispondenza, l'utente viene concesso l'accesso.</strong> Di conseguenza, se si desidera limitare l'accesso, è essenziale usare un `<deny>` l'ultimo elemento nella configurazione di autorizzazione URL. <strong>Se si omette un</strong><strong>`<deny>`</strong><strong>elemento, tutti gli utenti verranno concesso l'accesso.</strong>

Per comprendere meglio il processo usato dal `UrlAuthorizationModule` per determinare autorità, considerare l'esempio di regole di autorizzazione URL è stato illustrato in precedenza in questo passaggio. La prima regola è un `<allow>` elemento che consente l'accesso a Tito e Scott. Le regole di secondo è un `<deny>` elemento che nega l'accesso a tutti gli utenti. Se un utente anonimo visita, il `UrlAuthorizationModule` inizia ponendo, è anonima Scott o Tito? La risposta è ovviamente, No, in modo che consente di passare alla seconda regola. È anonimo nel set di tutti gli utenti? Poiché la risposta di seguito è Sì, il `<deny>` regola viene inserita in vigore e visitatore viene reindirizzato alla pagina di accesso. Analogamente, se viene visitato Jisun, il `UrlAuthorizationModule` inizia ponendo, Jisun è Scott o Tito? Poiché Anna non lo è, il `UrlAuthorizationModule` consente di passare alla seconda domanda, è Jisun nel set di tutti gli utenti? Anna è, in modo che Anna, troppo, viene negata l'accesso. Infine, se Tito visits, la prima domanda rappresentati dal `UrlAuthorizationModule` è una risposta affermativa, pertanto Tito viene concesso l'accesso.

Poiché il `UrlAuthorizationModule` processi delle regole di autorizzazione dall'alto verso il basso, arresto in corrispondenza di qualsiasi tipo, è importante stabilire le regole più specifiche precedere quelle meno specifiche. Vale a dire, per definire le regole di autorizzazione che impediscono Jisun e gli utenti anonimi, ma consentono tutti gli altri utenti autenticati, iniziare con la regola più specifica - Jisun che potrebbero influire su uno - e quindi procedere con le regole meno specifiche - quelli che consente tutti gli altri gli utenti autenticati, ma vengono negati tutti gli utenti anonimi. Le regole di autorizzazione URL seguente implementa questo criterio dal primo negare Jisun e successivamente negare qualsiasi utente anonimo. Qualsiasi utente autenticato a parte Jisun verrà concesso l'accesso perché nessuna di queste `<deny>` corrisponderà a istruzioni.

[!code-xml[Main](user-based-authorization-cs/samples/sample6.xml)]

## <a name="step-2-fixing-the-workflow-for-unauthorized-authenticated-users"></a>Passaggio 2: Correggere il flusso di lavoro per gli utenti non autorizzati, autenticati

Come accennato in precedenza in questa esercitazione in cui l'oggetto consultare la sezione del flusso di lavoro autorizzazione URL, in qualsiasi momento accade realmente una richiesta non autorizzata, il `UrlAuthorizationModule` interrompe la richiesta e restituisce uno stato HTTP 401 non autorizzato. Questo stato 401 viene modificato dal `FormsAuthenticationModule` in 302 reindirizzare stato che invia l'utente alla pagina di accesso. Questo flusso di lavoro si verifica su qualsiasi richiesta non autorizzata, anche se l'utente viene autenticato.

Restituzione di un utente autenticato alla pagina di accesso è probabile che deve essere confuso perché è già stato eseguito nel sistema. Con un po' di lavoro migliorare questo flusso di lavoro reindirizzando gli utenti autenticati che effettuano richieste non autorizzate a una pagina che spiega che tentano di accedere a una pagina con restrizioni.

Iniziare creando una nuova pagina ASP.NET nella cartella radice dell'applicazione web denominata `UnauthorizedAccess.aspx`; non dimenticare di associare questa pagina con il `Site.master` pagina master. Dopo aver creato questa pagina, rimuovere il controllo contenuto che fa riferimento il `LoginContent` ContentPlaceHolder in modo che la pagina master predefinita del contenuto verranno visualizzati. Successivamente, aggiungere un messaggio che descrive la situazione, vale a dire che l'utente ha tentato di accedere a una risorsa protetta. Dopo l'aggiunta di un messaggio di questo tipo, il `UnauthorizedAccess.aspx` markup dichiarativo della pagina dovrebbe essere simile al seguente:

[!code-aspx[Main](user-based-authorization-cs/samples/sample7.aspx)]

È ora necessario modificare il flusso di lavoro in modo che se una richiesta non autorizzata viene eseguita da un utente autenticato vengono inviati per il `UnauthorizedAccess.aspx` pagina anziché la pagina di accesso. La logica che reindirizza le richieste non autorizzate alla pagina di accesso è nascosto all'interno di un metodo privato del `FormsAuthenticationModule` classe, pertanto è possibile personalizzare questo comportamento. Cosa possiamo fare, tuttavia, viene aggiunta la propria logica alla pagina di accesso che reindirizza l'utente al `UnauthorizedAccess.aspx`, se necessario.

Quando la `FormsAuthenticationModule` reindirizza un visitatore non autorizzato alla pagina di accesso viene aggiunto l'URL richiesto, non autorizzato per la stringa di query con il nome `ReturnUrl`. Ad esempio, se si tenta di un utente non autorizzato a visitare `OnlyTito.aspx`, il `FormsAuthenticationModule` reindirizzarli a `Login.aspx?ReturnUrl=OnlyTito.aspx`. Pertanto, se si raggiunge la pagina di accesso da un utente autenticato con una stringa di query che include il `ReturnUrl` parametro e quindi si sa che questo utente non autenticato è appena provato a visitare una pagina Anna non è autorizzata a visualizzare. In tal caso, si desidera reindirizzare di `UnauthorizedAccess.aspx`.

A tale scopo, aggiungere il codice seguente alla pagina di accesso `Page_Load` gestore dell'evento:

[!code-csharp[Main](user-based-authorization-cs/samples/sample8.cs)]

Il codice sopra riportato reindirizza gli utenti autenticati, non autorizzati per il `UnauthorizedAccess.aspx` pagina. Per visualizzare questa logica in azione, visitare il sito come un visitatore anonimo e fare clic sul collegamento creazione degli account utente nella colonna sinistra. Verrà visualizzata per il `~/Membership/CreatingUserAccounts.aspx` pagina, che nel passaggio 1 è stato configurato solo per consentire l'accesso a Tito. Poiché gli utenti anonimi non sono consentiti, la `FormsAuthenticationModule` us reindirizza alla pagina di accesso.

A questo punto siamo anonimi, pertanto `Request.IsAuthenticated` restituisce `false` e non si verrà reindirizzati alla `UnauthorizedAccess.aspx`. Al contrario, verrà visualizzata la pagina di accesso. Accedere come utente diverso Tito, ad esempio Bruce. Dopo aver immesso le credenziali appropriate, l'account di accesso pagina us Reindirizza al `~/Membership/CreatingUserAccounts.aspx`. Comunque, poiché questa pagina è accessibile solo ai Tito, non sono autorizzati a visualizzarlo e vengono restituiti immediatamente alla pagina di accesso. Questa volta, tuttavia, `Request.IsAuthenticated` restituisce `true` (e il `ReturnUrl` parametro querystring esistente), quindi si verrà reindirizzati al `UnauthorizedAccess.aspx` pagina.


[![L'autenticazione, gli utenti non autorizzati vengono reindirizzati a UnauthorizedAccess.aspx](user-based-authorization-cs/_static/image17.png)](user-based-authorization-cs/_static/image16.png)

**Figura 6**: autenticazione, gli utenti non autorizzati vengono reindirizzati al `UnauthorizedAccess.aspx` ([fare clic per visualizzare l'immagine con dimensioni normali](user-based-authorization-cs/_static/image18.png))


Questo flusso di lavoro personalizzato presenta un'esperienza utente più ragionevole e semplice, corto circuito del ciclo illustrato nella figura 2.

## <a name="step-3-limiting-functionality-based-on-the-currently-logged-in-user"></a>Passaggio 3: Limitazione della funzionalità basate sull'utente attualmente connesso

Autorizzazione URL semplifica specificare le regole di autorizzazione grossolana. Come abbiamo visto nel passaggio 1, con l'autorizzazione URL si possiamo sinteticamente dello stato sono consentite le identità e quelle che invece non autorizzati alla visualizzazione di una particolare pagina o tutte le pagine in una cartella. In determinati scenari, tuttavia, si potrebbe essere necessario consentire tutti gli utenti visitano una pagina, ma limitare la funzionalità della pagina in base all'utente, visitare.

Si consideri il caso di un sito Web di e-commerce che consenta ai visitatori autenticati rivedere i propri prodotti. Quando un utente anonimo visita la pagina del prodotto, si vedrà solo le informazioni sul prodotto e potrebbe non essere ha la possibilità di inserire una recensione. Tuttavia, un utente autenticato visitando la pagina stessa verrebbe visualizzata l'interfaccia di revisione. Se l'utente autenticato non era ancora rivisti questo prodotto, l'interfaccia consenta loro di inviare una revisione; in caso contrario, questo verrà visualizzato li la verifica di inviati in precedenza. Per richiedere un passaggio di questo scenario, inoltre, la pagina del prodotto potrà visualizzare informazioni aggiuntive e offrono funzionalità estese per gli utenti che lavorano per l'azienda di e-commerce. Ad esempio, la pagina del prodotto potrebbe elencare le scorte in magazzino e includono opzioni per modificare descrizione quando visitati da un dipendente e il prezzo del prodotto.

Regole di autorizzazione granulare possono essere implementate in modo dichiarativo o a livello di codice (o tramite una combinazione dei due). Nella sezione successiva si vedrà come implementare l'autorizzazione granulare tramite il controllo LoginView. In seguito, verranno analizzati tecniche a livello di codice. Prima di poter esaminare applicando le regole di autorizzazione granulare, tuttavia, prima di tutto dobbiamo creare una pagina di cui funzionalità dipende dall'utente visita.

È possibile creare una pagina che elenca i file in una directory specifica all'interno di un controllo GridView. Insieme a elencare ogni nome di file, le dimensioni e altre informazioni, il controllo GridView includerà due colonne di LinkButton: uno denominato Vista e una Delete titolo. Se si seleziona il LinkButton visualizzazione, verrà visualizzato il contenuto del file selezionato; Se si seleziona il LinkButton Elimina, il file verrà eliminato. Inizialmente creare questa pagina in modo che sono disponibile per tutti gli utenti le funzionalità di visualizzazione e delete. Utilizzare le sezioni relative a controllo LoginView e a livello di programmazione delle funzionalità di limitazione si vedrà come abilitare o disabilitare queste funzionalità basate sull'utente visitando la pagina.

> [!NOTE]
> La pagina ASP.NET che si sta tentando di compilare Usa un controllo GridView per visualizzare un elenco di file. Poiché questa esercitazione, serie è incentrato sull'autenticazione basata su form, autorizzazione, gli account utente e ruoli, non desidera dedicare troppo tempo a descrivere i meccanismi interni del controllo GridView. Anche se in questa esercitazione offre istruzioni dettagliate specifiche per la configurazione di questa pagina, non approfondire i dettagli del motivo per cui sono state apportate alcune scelte, o ciò che hanno proprietà particolare effetto sull'output sottoposto a rendering. Per un esame approfondito del controllo GridView, consultare il *[usare i dati in ASP.NET 2.0](../../data-access/index.md)* serie di esercitazioni.


Iniziare aprendo il `UserBasedAuthorization.aspx` del file nei `Membership` cartella e l'aggiunta di un controllo GridView alla pagina denominata `FilesGrid`. Smart Tag del controllo GridView, fare clic sul collegamento Modifica colonne per visualizzare la finestra di dialogo campi. A questo punto, deselezionare l'opzione genera automaticamente campi casella di controllo nell'angolo inferiore sinistro. Successivamente, aggiungere un pulsante Seleziona un pulsante di eliminazione e due i BoundField dall'angolo superiore sinistro (i pulsanti di Select e Delete sono disponibili sotto il tipo CommandField). Impostare il pulsante di selezione `SelectText` proprietà alla visualizzazione e il primo BoundField `HeaderText` e `DataField` proprietà per nome. Impostare i BoundField secondo `HeaderText` proprietà alle dimensioni in byte, relative `DataField` proprietà lunghezza, relativo `DataFormatString` proprietà {0:N0} e il relativo `HtmlEncode` proprietà su False.

Dopo aver configurato sloupce prvku GridView, fare clic su OK per chiudere la finestra di dialogo campi. Dalla finestra delle proprietà, impostare il controllo GridView `DataKeyNames` proprietà `FullName`. Markup dichiarativo del controllo GridView a questo punto dovrebbe essere simile al seguente:

[!code-aspx[Main](user-based-authorization-cs/samples/sample9.aspx)]

Con il markup di GridView creato, siamo pronti a scrivere il codice che recupererà i file in una determinata directory e associarle a GridView. Aggiungere il codice seguente alla pagina `Page_Load` gestore dell'evento:

[!code-csharp[Main](user-based-authorization-cs/samples/sample10.cs)]

Il codice precedente Usa la [ `DirectoryInfo` classe](https://msdn.microsoft.com/library/system.io.directoryinfo.aspx) per ottenere un elenco dei file nella cartella radice dell'applicazione. Il [ `GetFiles()` metodo](https://msdn.microsoft.com/library/system.io.directoryinfo.getfiles.aspx) restituisce come matrice di tutti i file nella directory [ `FileInfo` oggetti](https://msdn.microsoft.com/library/system.io.fileinfo.aspx), che viene quindi associato a GridView. Il `FileInfo` oggetto include un'ampia gamma di proprietà, ad esempio `Name`, `Length`, e `IsReadOnly`, tra gli altri. Come può notare dal relativo markup dichiarativo, GridView Visualizza solo le `Name` e `Length` proprietà.

> [!NOTE]
> Il `DirectoryInfo` e `FileInfo` classi si trovano nel [ `System.IO` dello spazio dei nomi](https://msdn.microsoft.com/library/system.io.aspx). Pertanto, è necessario disporre per anteporre a questi nomi di classe con i rispettivi nomi dello spazio dei nomi o lo spazio dei nomi importato nel file di classe (tramite `using System.IO`).


Si consiglia di visitare questa pagina tramite un browser. Visualizzerà l'elenco di file che si trovano nella directory radice dell'applicazione. Facendo clic su uno della visualizzazione o eliminare i controlli LinkButton causerà un postback, ma si verificherà alcuna azione perché dobbiamo ancora creare gestori di eventi necessari.


[![Il controllo GridView sono elencati i file nella Directory radice dell'applicazione Web](user-based-authorization-cs/_static/image20.png)](user-based-authorization-cs/_static/image19.png)

**Figura 7**: il controllo GridView sono elencati i file nella Directory radice dell'applicazione Web ([fare clic per visualizzare l'immagine con dimensioni normali](user-based-authorization-cs/_static/image21.png))


È necessario un modo per visualizzare il contenuto del file selezionato. Tornare a Visual Studio e aggiungere una casella di testo denominato `FileContents` sopra il controllo GridView. Impostare relativi `TextMode` proprietà `MultiLine` e il relativo `Columns` e `Rows` proprietà al 95% e 10, rispettivamente.

[!code-aspx[Main](user-based-authorization-cs/samples/sample11.aspx)]

Successivamente, creare un gestore eventi per il controllo GridView [ `SelectedIndexChanged` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindexchanged.aspx) e aggiungere il codice seguente:

[!code-csharp[Main](user-based-authorization-cs/samples/sample12.cs)]

Questo codice Usa il controllo GridView `SelectedValue` proprietà per determinare il nome file completo del file selezionato. Internamente, il `DataKeys` raccolta viene fatto riferimento per ottenere il `SelectedValue`, quindi è necessario impostare il controllo GridView `DataKeyNames` proprietà per nome, come descritto in precedenza in questo passaggio. Il [ `File` classe](https://msdn.microsoft.com/library/system.io.file.aspx) viene utilizzato per leggere il contenuto del file selezionato in una stringa, che viene quindi assegnato al `FileContents` della casella di testo `Text` proprietà, in tal modo la visualizzazione del contenuto del file selezionato nella pagina.


[![Contenuto del File selezionato viene visualizzato nella casella di testo](user-based-authorization-cs/_static/image23.png)](user-based-authorization-cs/_static/image22.png)

**Figura 8**: contenuto del File seleziona la viene visualizzato nella casella di testo ([fare clic per visualizzare l'immagine con dimensioni normali](user-based-authorization-cs/_static/image24.png))


> [!NOTE]
> Se si visualizza il contenuto di un file che contiene il markup HTML e quindi prova a visualizzare o eliminare un file, si riceverà un `HttpRequestValidationException` errore. Ciò si verifica perché durante il postback contenuto della casella di testo viene inviato al server web. Per impostazione predefinita, ASP.NET genera un `HttpRequestValidationException` errore ogni volta che viene rilevato contenuto potenzialmente dannoso postback, ad esempio, il markup HTML. Per disabilitare questo errore venga visualizzato, disattivare la convalida delle richieste per la pagina aggiungendo `ValidateRequest="false"` per il `@Page` direttiva. Per altre informazioni sui vantaggi di convalida della richiesta come nonché quali precauzioni è necessario eseguire quando la disattivazione, leggere [richiesta di convalida - impedisce attacchi Script](https://asp.net/learn/whitepapers/request-validation/).


Infine, aggiungere un gestore eventi con il codice seguente per il controllo GridView [ `RowDeleting` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleting.aspx):

[!code-csharp[Main](user-based-authorization-cs/samples/sample13.cs)]

Il codice visualizza semplicemente il nome completo del file da eliminare nella `FileContents` casella di testo *senza* effettivamente l'eliminazione del file.


[![Fare clic sul pulsante di eliminazione non elimina effettivamente il File](user-based-authorization-cs/_static/image26.png)](user-based-authorization-cs/_static/image25.png)

**Figura 9**: facendo clic su di eliminare pulsante non elimina effettivamente i File ([fare clic per visualizzare l'immagine con dimensioni normali](user-based-authorization-cs/_static/image27.png))


Nel passaggio 1 è stato configurato le regole di autorizzazione URL per impedire a utenti anonimi le pagine nella visualizzazione di `Membership` cartella. Per poter presentare meglio autenticazione granulare, è possibile consentire agli utenti anonimi di visitare il `UserBasedAuthorization.aspx` pagina, ma con funzionalità limitata. Per aprire questa pagina per essere accessibili da tutti gli utenti, aggiungere quanto segue `<location>` elemento per il `Web.config` del file nei `Membership` cartella:

[!code-xml[Main](user-based-authorization-cs/samples/sample14.xml)]

Dopo aver aggiunto questo `<location>` elemento, verificare le nuove regole di autorizzazione URL tramite l'accesso all'esterno del sito. Come utente anonimo dovrebbero poter visitare il `UserBasedAuthorization.aspx` pagina.

Attualmente, è possibile visitare qualsiasi utente autenticato o anonimo la `UserBasedAuthorization.aspx` pagina e visualizzare o eliminare i file. È possibile fare in modo che solo gli utenti autenticati possono visualizzare contenuto di un file e solo Tito può eliminare un file. Regole di autorizzazione granulare possono essere applicate in modo dichiarativo, a livello di codice o tramite una combinazione di entrambi i metodi. È possibile usare l'approccio dichiarativo per limitare chi può visualizzare il contenuto del file. si userà l'approccio a livello di codice per limitare gli utenti possa eliminare un file.

### <a name="using-the-loginview-control"></a>Utilizzare il controllo LoginView

Come illustrato nelle esercitazioni precedenti, il controllo LoginView è utile per visualizzare le interfacce diverse per gli utenti autenticati e anonimi e offre un modo semplice per nascondere la funzionalità che non è accessibile agli utenti anonimi. Poiché gli utenti anonimi non possono visualizzare o eliminare i file, è necessario solo mostrare il `FileContents` nella casella di testo quando viene visitata la pagina da un utente autenticato. A tale scopo, aggiungere un controllo LoginView alla pagina, denominarlo `LoginViewForFileContentsTextBox`e spostare il `FileContents` markup dichiarativo della casella di testo del controllo LoginView `LoggedInTemplate`.

[!code-aspx[Main](user-based-authorization-cs/samples/sample15.aspx)]

I controlli Web nei modelli di LoginView non sono più accessibili direttamente dalla classe code-behind. Ad esempio, il `FilesGrid` GridView `SelectedIndexChanged` e `RowDeleting` gestori eventi fanno riferimento al `FileContents` controllo casella di testo con il codice, ad esempio:

[!code-csharp[Main](user-based-authorization-cs/samples/sample16.cs)]

Tuttavia, questo codice non è più valido. Spostando la `FileContents` casella di testo nel `LoggedInTemplate` la casella di testo non è possibile accedervi direttamente. In alternativa, è necessario usare il `FindControl("controlId")` metodo a cui fare riferimento a livello di codice il controllo. Aggiornamento di `FilesGrid` gestori eventi a cui fare riferimento la casella di testo come segue:

[!code-csharp[Main](user-based-authorization-cs/samples/sample17.cs)]

Dopo aver spostato la casella di testo di LoginView `LoggedInTemplate` e l'aggiornamento del codice della pagina di riferimento usando la casella di testo di `FindControl("controlId")` pattern, visitare la pagina come utente anonimo. Come illustrato nella figura 10, il `FileContents` non viene visualizzato nella casella di testo. Tuttavia, è ancora visualizzato il LinkButton di visualizzazione.


[![Il controllo LoginView rendering solo la casella di testo FileContents per gli utenti autenticati](user-based-authorization-cs/_static/image29.png)](user-based-authorization-cs/_static/image28.png)

**Figura 10**: The LoginView controllo solo esegue il rendering le `FileContents` nella casella di testo per gli utenti autenticati ([fare clic per visualizzare l'immagine con dimensioni normali](user-based-authorization-cs/_static/image30.png))


Un modo per nascondere il pulsante di visualizzazione per gli utenti anonimi è per convertire il campo di GridView in un TemplateField. Verrà generato un modello che contiene il markup dichiarativo per la visualizzazione LinkButton. È quindi possibile aggiungere un controllo LoginView per il TemplateField e inserire il LinkButton all'interno di LoginView `LoggedInTemplate`, pertanto se si nasconde il pulsante di visualizzazione dei visitatori anonimi. A tale scopo, fare clic sul collegamento Modifica colonne di GridView Smart tag per avviare la finestra di dialogo campi. Successivamente, selezionare il pulsante di selezione dall'elenco nell'angolo inferiore sinistro e quindi fare clic su Converti questo campo su un collegamento TemplateField. Questa operazione modificherà markup dichiarativo del campo da:

[!code-aspx[Main](user-based-authorization-cs/samples/sample18.aspx)]

 A: 

[!code-aspx[Main](user-based-authorization-cs/samples/sample19.aspx)]

A questo punto, possiamo aggiungere un LoginView per il TemplateField. Il markup seguente visualizza il LinkButton visualizzazione solo per gli utenti autenticati.

[!code-aspx[Main](user-based-authorization-cs/samples/sample20.aspx)]

Come illustrato nella figura 11, il risultato finale non è che piuttosto come visualizzazione colonna viene ancora visualizzata anche se il LinkButton visualizzazione all'interno della colonna sono nascoste. Si esaminerà come nascondere l'intera colonna GridView (e non solo il LinkButton) nella sezione successiva.


[![Il controllo LoginView consente di nascondere il LinkButton di visualizzazione per i visitatori anonimi](user-based-authorization-cs/_static/image32.png)](user-based-authorization-cs/_static/image31.png)

**Figura 11**: il controllo LoginView consente di nascondere il LinkButton di visualizzazione per i visitatori anonimi ([fare clic per visualizzare l'immagine con dimensioni normali](user-based-authorization-cs/_static/image33.png))


### <a name="programmatically-limiting-functionality"></a>Limitazione a livello di codice della funzionalità

In alcune circostanze, tecniche dichiarative non sono sufficienti per la limitazione della funzionalità a una pagina. Ad esempio, la disponibilità di alcune funzionalità della pagina potrebbe essere dipendente da criteri oltre che l'utente visitando la pagina sia anonimi o autenticati. In questi casi, i vari elementi dell'interfaccia utente possono essere visualizzati o nascosti a livello di.

Per limitare le funzionalità a livello di codice, è necessario eseguire due attività:

1. Determinare se l'utente visitando la pagina può accedere alla funzionalità, e
2. Modificare a livello di codice l'interfaccia utente basata sul fatto che l'utente può accedere alla funzionalità in questione.

Per illustrare l'applicazione di queste due attività, è possibile consentire solo Tito eliminare file dal controllo GridView. La prima attività, quindi, consiste nel determinare se sia Tito visitando la pagina. Una volta che è stato determinato, è necessario nascondere colonna eliminazione del controllo GridView (o mostrare). Sloupce prvku GridView sono accessibili tramite il `Columns` proprietà; una colonna viene eseguito rendering solo se relativi `Visible` è impostata su `true` (predefinito).

Aggiungere il codice seguente per il `Page_Load` gestore dell'evento prima dell'associazione dei dati a GridView:

[!code-csharp[Main](user-based-authorization-cs/samples/sample21.cs)]

Come accennato nel [ *una panoramica dell'autenticazione basata su form* ](../introduction/an-overview-of-forms-authentication-cs.md) esercitazione `User.Identity.Name` restituisce il nome dell'identità. Corrisponde al nome utente immesso nel controllo di accesso. Se è Tito visitando la pagina, seconda colonna del controllo GridView `Visible` è impostata su `true`; in caso contrario, impostarlo su `false`. Il risultato è che quando un utente diverso da Tito visita la pagina, un altro utente autenticato o un utente anonimo, la colonna di eliminazione non viene visualizzata (vedere Figura 12). Tuttavia, quando Tito visita la pagina, è presente la colonna di eliminazione (vedere la figura 13).


[![Elimina colonna non è viene eseguito il rendering quando visitati da un utente diverso da Tito (ad esempio, Bruce)](user-based-authorization-cs/_static/image35.png)](user-based-authorization-cs/_static/image34.png)

**Figura 12**: eliminare Sloupec non JE viene eseguito il rendering quando visitati da un utente diverso da Tito (ad esempio, Bruce) ([fare clic per visualizzare l'immagine con dimensioni normali](user-based-authorization-cs/_static/image36.png))


[![Elimina colonna di cui viene eseguito il rendering per Tito](user-based-authorization-cs/_static/image38.png)](user-based-authorization-cs/_static/image37.png)

**Figura 13**: Elimina la colonna viene eseguito il rendering per Tito ([fare clic per visualizzare l'immagine con dimensioni normali](user-based-authorization-cs/_static/image39.png))


## <a name="step-4-applying-authorization-rules-to-classes-and-methods"></a>Passaggio 4: Applicazione delle regole di autorizzazione alle classi e metodi

Nel passaggio 3 è non consentite agli utenti anonimi visualizzare il contenuto del file ed è vietato l'eliminazione di file di tutti gli utenti ma Tito. Questa operazione è stata eseguita nascondendo gli elementi dell'interfaccia utente associato ai visitatori non autorizzati tramite tecniche di strumenti dichiarativi e programmatici. Per questo semplice esempio, in modo corretto se si nasconde gli elementi dell'interfaccia utente è stato molto semplici, ma per quanto riguarda siti più complessi cui possono essere presenti molti modi diversi per eseguire la stessa funzionalità? Per limitare tale funzionalità per utenti non autorizzati, cosa accade se si dimentica di nascondere o disattivare tutti gli elementi dell'interfaccia utente applicabili?

È un modo semplice per garantire che una particolare parte della funzionalità non è accessibile da un utente non autorizzato per decorare quella classe o un metodo con il [ `PrincipalPermission` attributo](https://msdn.microsoft.com/library/system.security.permissions.principalpermissionattribute.aspx). Quando il runtime di .NET viene utilizzata una classe o esegue uno dei relativi metodi, verifica per assicurarsi che il contesto di sicurezza corrente sia autorizzato a usare la classe o il metodo execute. Il `PrincipalPermission` attributo fornisce un meccanismo attraverso il quale è possibile definire queste regole.

Di seguito viene illustrato l'utilizzo di `PrincipalPermission` attributo del controllo GridView `SelectedIndexChanged` e `RowDeleting` gestori eventi per non consentire l'esecuzione da utenti anonimi e utenti diversi da Tito, rispettivamente. Tutto dobbiamo fare è aggiungere l'attributo appropriato nella parte superiore di ogni definizione di funzione:

[!code-csharp[Main](user-based-authorization-cs/samples/sample22.cs)]

L'attributo per il `SelectedIndexChanged` stabilisce gestore eventi che solo gli utenti autenticavano possono eseguire il gestore eventi, mentre l'attributo nel `RowDeleting` gestore eventi consente di limitare l'esecuzione del Tito.

Se, in qualche modo, un utente diverso dal Tito tenta di eseguire la `RowDeleting` gestore dell'evento o un utente non autenticato prova a eseguire il `SelectedIndexChanged` gestore eventi, il runtime di .NET verrà generato un `SecurityException`.


[![Se il contesto di sicurezza non è autorizzato a eseguire il metodo, viene generata un'eccezione SecurityException](user-based-authorization-cs/_static/image41.png)](user-based-authorization-cs/_static/image40.png)

**Figura 14**: se il contesto di sicurezza non è autorizzato a eseguire il metodo, una `SecurityException` generata ([fare clic per visualizzare l'immagine con dimensioni normali](user-based-authorization-cs/_static/image42.png))


> [!NOTE]
> Per consentire a più contesti di sicurezza per accedere a una classe o metodo, decorare la classe o un metodo con un `PrincipalPermission` attributo per ogni contesto di sicurezza. Vale a dire, per consentire a Tito e Bruce eseguire la `RowDeleting` gestore dell'evento, aggiungere *due* `PrincipalPermission` attributi:


[!code-csharp[Main](user-based-authorization-cs/samples/sample23.cs)]

Oltre alle pagine ASP.NET, molte applicazioni hanno anche un'architettura che include vari livelli, ad esempio la logica di Business e livelli di accesso ai dati. Questi livelli vengono in genere implementati come librerie di classi e classi e metodi per l'esecuzione di funzionalità relative ai dati e per la logica di business dell'offerta. Il `PrincipalPermission` attributo è utile per l'applicazione delle regole di autorizzazione a questi livelli.

Per altre informazioni sull'uso di `PrincipalPermission` attributo per definire le regole di autorizzazione sulle classi e metodi, fare riferimento a [Scott Guthrie](https://weblogs.asp.net/scottgu/)del post di blog [aggiunta di regole di autorizzazione per Business e l'utilizzo di livelli di dati `PrincipalPermissionAttributes` ](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx).

## <a name="summary"></a>Riepilogo

In questa esercitazione è stato illustrato come applicare le regole di autorizzazione basata sull'utente. Abbiamo iniziato con uno sguardo ad ASP. Framework di autorizzazione URL della rete. Per del ogni richiesta, il motore ASP.NET `UrlAuthorizationModule` controlla le regole di autorizzazione URL definite nella configurazione dell'applicazione per determinare se l'identità è autorizzato ad accedere alla risorsa richiesta. In breve, l'autorizzazione URL semplifica specificare le regole di autorizzazione per una pagina specifica o per tutte le pagine in una directory specifica.

Il framework di autorizzazione URL si applica le regole di autorizzazione in base a una pagina per pagina. Autorizzazione URL, entrambi l'identità del richiedente è autorizzato per accedere a una determinata risorsa o non. Molti scenari, tuttavia, chiamare per le regole di autorizzazione più granulare. Invece di definire chi può accedere a una pagina, è possibile che sia necessario per consentire a tutti gli utenti di accedere a una pagina, ma mostrare dati diversi o funzionalità diverse a seconda dell'utente visitando la pagina dell'offerta. Autorizzazione a livello di pagina implica in genere come nascondere gli elementi dell'interfaccia utente specifico allo scopo di impedire che utenti non autorizzati l'accesso a funzionalità non consentite. Inoltre, è possibile usare gli attributi per limitare l'accesso alle classi e l'esecuzione dei relativi metodi per determinati utenti.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Aggiunta di regole di autorizzazione al Business e i livelli di dati utilizzando `PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)
- [Autorizzazione ASP.NET](https://msdn.microsoft.com/library/wce3kxhd.aspx)
- [Modifiche tra IIS6 e sicurezza di IIS7](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/Changes-between-IIS6-and-IIS7-Security)
- [Configurazione di sottodirectory e file specifici](https://msdn.microsoft.com/library/6hbkh9s7.aspx)
- [Limitazione della funzionalità di modifica dei dati in base all'utente](../../data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-cs.md)
- [Guide introduttive di controllo LoginView](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/loginview.aspx)
- [La comprensione di autorizzazione URL IIS 7](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization)
- [`UrlAuthorizationModule` Documentazione tecnica](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx)
- [Uso dei dati in ASP.NET 2.0](../../data-access/index.md)

### <a name="about-the-author"></a>Informazioni sull'autore

Scott Mitchell, autore di più libri e fondatore di 4GuysFromRolla.com, ha lavorato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola  *[Sams Teach Yourself ASP.NET 2.0 in 24 ore](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. È possibile contattarlo Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) o il suo blog all'indirizzo [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Precedente](validating-user-credentials-against-the-membership-user-store-cs.md)
> [Successivo](storing-additional-user-information-cs.md)
