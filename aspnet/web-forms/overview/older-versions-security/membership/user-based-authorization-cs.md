---
uid: web-forms/overview/older-versions-security/membership/user-based-authorization-cs
title: Autorizzazione basata sull'utente (c#) | Documenti Microsoft
author: rick-anderson
description: "In questa esercitazione verranno esaminati limitando l'accesso alle pagine e funzionalità a livello di pagina tramite varie tecniche di limitazione."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: 3c815a9e-2296-4b9b-b945-776d54989daa
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/membership/user-based-authorization-cs
msc.type: authoredcontent
ms.openlocfilehash: 5bee98878b5191a096b851c65aaea19ad989f608
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="user-based-authorization-c"></a>Autorizzazione basata sull'utente (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare codice](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_07_CS.zip) o [Scarica il PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial07_UserAuth_cs.pdf)

> In questa esercitazione verranno esaminati limitando l'accesso alle pagine e funzionalità a livello di pagina tramite varie tecniche di limitazione.


## <a name="introduction"></a>Introduzione

La maggior parte delle applicazioni web che offrono gli account utente di eseguire questa operazione in parte per limitare determinati visitatori di accedere a determinate pagine all'interno del sito. In siti bacheca più online, ad esempio, tutti gli utenti - anonimi e autenticati - sono in grado di visualizzare i post della bacheca, ma solo gli utenti autenticati possono visitare la pagina web per creare un nuovo post. E potrebbero essere presenti le pagine amministrative che sono solo accessibili a un utente specifico (o un determinato gruppo di utenti). Inoltre, la funzionalità a livello di pagina può differire su base utente dall'utente. Quando si visualizza un elenco di inserimenti, gli utenti autenticati vengono visualizzati un'interfaccia per la valutazione di ogni post, mentre questa interfaccia non è disponibile per utenti anonimi.

ASP.NET consente di definire le regole di autorizzazione basata sull'utente. Con un minimo di markup in `Web.config`, pagine web specifiche o intere directory possono essere bloccate in modo che siano solo accessibili a un sottoinsieme di utenti specificato. Funzionalità a livello di pagina può essere attivata o disattivata in base a utente attualmente connesso in modo dichiarativo e a livello di codice.

In questa esercitazione verranno esaminati limitando l'accesso alle pagine e funzionalità a livello di pagina tramite varie tecniche di limitazione. Iniziamo!

## <a name="a-look-at-the-url-authorization-workflow"></a>Esaminare il flusso di lavoro di autorizzazione URL

Come descritto nel [ *una panoramica dell'autenticazione basata su form* ](../introduction/an-overview-of-forms-authentication-cs.md) esercitazione, quando il runtime di ASP.NET elabora una richiesta per una richiesta di risorsa ASP.NET genera un numero di eventi durante il ciclo di vita. *I moduli HTTP* sono classi gestite con un codice viene eseguito in risposta a un particolare evento del ciclo di vita di richiesta. ASP.NET viene fornito con un numero di moduli HTTP che eseguono le attività essenziali dietro le quinte.

Uno dei tali moduli HTTP è [ `FormsAuthenticationModule` ](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx). Come illustrato nelle esercitazioni precedenti, la funzione principale del `FormsAuthenticationModule` consiste nel determinare l'identità della richiesta corrente. Questa operazione viene eseguita controllando il ticket di autenticazione form, che si trova in un cookie o incorporato all'interno dell'URL. Questa identificazione ha luogo durante il [ `AuthenticateRequest` evento](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx).

Un altro modulo HTTP importante è il [ `UrlAuthorizationModule` ](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx), che viene generato in risposta al [ `AuthorizeRequest` evento](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx) (situazione che si verifica dopo il `AuthenticateRequest` evento). Il `UrlAuthorizationModule` esamina il codice di configurazione in `Web.config` per determinare se l'identità corrente dispone di autorizzazioni sufficienti per visitare la pagina specificata. Questo processo è detto *autorizzazione URL*.

Esamina la sintassi per le regole di autorizzazione URL nel passaggio 1, ma prima di tutto si osservare cosa il `UrlAuthorizationModule` a seconda se la richiesta è autorizzata o meno. Se il `UrlAuthorizationModule` determina che la richiesta è autorizzata, quindi non esegue alcuna operazione e la richiesta continua attraverso il ciclo di vita. Tuttavia, se la richiesta è *non* autorizzato, il `UrlAuthorizationModule` interrompe il ciclo di vita e indica il `Response` per restituire un [HTTP 401 non autorizzato](http://www.checkupdown.com/status/E401.html) stato. Quando si utilizza l'autenticazione basata su form questo stato HTTP 401 non viene mai restituito al client perché se il `FormsAuthenticationModule` rileva un HTTP 401 è stato modificato un [HTTP 302 reindirizzare](http://www.checkupdown.com/status/E302.html) alla pagina di accesso.

La figura 1 illustra il flusso di lavoro della pipeline ASP.NET, il `FormsAuthenticationModule`e `UrlAuthorizationModule` quando una richiesta non autorizzata arriva. In particolare, la figura 1 mostra una richiesta da un visitatore anonimo per `ProtectedPage.aspx`, che è una pagina che nega l'accesso agli utenti anonimi. Poiché il visitatore è anonimo, il `UrlAuthorizationModule` interrompe la richiesta e restituisce uno stato HTTP 401 non autorizzato. Il `FormsAuthenticationModule` converte quindi di stato 401 nel reindirizzamento 302 alla pagina di accesso. Dopo che l'utente viene autenticato tramite la pagina di accesso, viene reindirizzato al `ProtectedPage.aspx`. Questa volta il `FormsAuthenticationModule` identifica l'utente in base a esso il ticket di autenticazione. Ora che il visitatore viene autenticato, il `UrlAuthorizationModule` consente l'accesso alla pagina.


[![L'autenticazione basata su form e un flusso di lavoro autorizzazione URL](user-based-authorization-cs/_static/image2.png)](user-based-authorization-cs/_static/image1.png)

**Figura 1**: l'autenticazione basata su form e flusso di lavoro autorizzazione URL ([fare clic per visualizzare l'immagine ingrandita](user-based-authorization-cs/_static/image3.png))


Figura 1 illustra l'interazione che si verifica quando un visitatore anonimo tenta di accedere a una risorsa che non è disponibile per gli utenti anonimi. In tal caso, il visitatore anonimo viene reindirizzato alla pagina di accesso con la pagina che ha tentato di visitare specificato nella stringa di query. Una volta che l'utente è connesso correttamente, lei verrà automaticamente reindirizzata nuovamente alla risorsa che inizialmente ha tentato di visualizzare.

Quando la richiesta non autorizzata viene effettuata da un utente anonimo, il flusso di lavoro è semplice e facile per il visitatore comprendere cosa è successo e sul motivo. Tenere presente che, ma il `FormsAuthenticationModule` reindirizzerà *qualsiasi* non autorizzato utente alla pagina di accesso, anche se la richiesta viene effettuata da un utente autenticato. Ciò può generare confusione nell'esperienza utente se un utente autenticato tenta di visitare una pagina per il quale ha non dispone di autorità.

Si supponga che il sito Web hanno regole di autorizzazione relativi URL configurate in modo che la pagina ASP.NET `OnlyTito.aspx` è accessibile solo a Tito. A questo punto, si supponga che Sam visita il sito esegue l'accesso e quindi tenta di visitare `OnlyTito.aspx`. Il `UrlAuthorizationModule` arresterà il ciclo di vita di richiesta e restituire lo stato HTTP 401 non autorizzato, quale il `FormsAuthenticationModule` rileverà e quindi reindirizzare Sam alla pagina di accesso. Poiché Sam ha già effettuato l'accesso, tuttavia, Lei ci potrebbe chiedere perché Mary è stata inviata nuovamente alla pagina di accesso. Lei potrebbe motivo che le proprie credenziali di account di accesso sono stati perse in qualche modo, o che ha immesso le credenziali non valide. Se Sam riaccedono le proprie credenziali dalla pagina di accesso utente verrà connesso (nuovo) e reindirizzata a `OnlyTito.aspx`. Il `UrlAuthorizationModule` rileverà che Sam non è possibile visitare questa pagina e verrà restituita alla pagina di accesso.

Figura 2 viene illustrata questa confusione del flusso di lavoro.


[![Il flusso di lavoro predefinita può causare un ciclo di confusione](user-based-authorization-cs/_static/image5.png)](user-based-authorization-cs/_static/image4.png)

**Figura 2**: il predefinito del flusso di lavoro può causare un ciclo confondere ([fare clic per visualizzare l'immagine ingrandita](user-based-authorization-cs/_static/image6.png))


Il flusso di lavoro illustrato nella figura 2 può rapidamente befuddle anche la maggior parte dei computer esperti visitatore. Verranno esaminati modi per evitare questo problema confondere ciclo nel passaggio 2.

> [!NOTE]
> ASP.NET utilizza due meccanismi per determinare se l'utente corrente possa accedere a una pagina web specifica: autorizzazione URL e l'autorizzazione del file. Autorizzazione del file viene implementata dal [ `FileAuthorizationModule` ](https://msdn.microsoft.com/library/system.web.security.fileauthorizationmodule.aspx), che determina l'autorità consultando i file richiesti ACL. Autorizzazione del file viene utilizzato più di frequente con l'autenticazione di Windows perché gli ACL sono le autorizzazioni valide per gli account di Windows. Quando si utilizza l'autenticazione basata su form, vengono eseguite tutte le richieste a livello di sistema di file e sistema operativo dello stesso account di Windows, indipendentemente dall'utente visita il sito. Poiché questa serie di esercitazioni si concentra sull'autenticazione basata su form, è non verranno esaminati l'autorizzazione del file.


### <a name="the-scope-of-url-authorization"></a>L'ambito di autorizzazione URL

Il `UrlAuthorizationModule` è il codice gestito che fa parte del runtime di ASP.NET. Prima versione 7 di Microsoft [Internet Information Services (IIS)](https://www.iis.net/) server web, si è verificato un ostacolo distinto tra pipeline HTTP di IIS e della pipeline del runtime ASP.NET. In breve, in IIS 6 e versioni precedenti, ASP. Del NET `UrlAuthorizationModule` viene eseguita solo quando una richiesta viene delegata da IIS al runtime di ASP.NET. Per impostazione predefinita, IIS elabora il contenuto statico stesso, ad esempio pagine HTML e CSS, JavaScript e i file di immagine - e passa solo le richieste al runtime di ASP.NET, quando una pagina con estensione `.aspx`, `.asmx`, o `.ashx` è richiesto.

Pipeline IIS 7, tuttavia, consente di integrata di IIS e ASP.NET. Con alcune impostazioni di configurazione è possibile configurare IIS 7 per richiamare il `UrlAuthorizationModule` per *tutti* richieste, vale a dire che è possono definire regole di autorizzazione URL per qualsiasi tipo di file. Inoltre, IIS 7 include il proprio motore di autorizzazione URL. Per ulteriori informazioni sull'integrazione di ASP.NET e funzionalità di autorizzazione URL nativa IIS 7, vedere [comprensione IIS7 URL autorizzazione](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization). Per una descrizione più approfondita di integrazione ASP.NET e IIS 7, selezionare una copia del libro di Shahram Khosravi *Professional IIS 7 e ASP.NET integrata programmazione* (ISBN: 978 0470152539).

In breve, nelle versioni precedenti di IIS 7, le regole di autorizzazione URL vengono applicate solo alle risorse gestite dal runtime ASP.NET. Ma con IIS 7 è possibile utilizzare la funzionalità di autorizzazione URL nativo di IIS o per l'integrazione ASP. Del NET `UrlAuthorizationModule` nella pipeline HTTP di IIS, in tal modo si estende tali funzionalità a tutte le richieste.

> [!NOTE]
> Esistono alcune differenze meno evidenti comunque importanti come ASP. Del NET `UrlAuthorizationModule` e funzionalità di autorizzazione URL di IIS 7 elaborare le regole di autorizzazione. In questa esercitazione non esamina la funzionalità di autorizzazione URL IIS 7 o le differenze nella modalità analizza rispetto alle regole di autorizzazione di `UrlAuthorizationModule`. Per ulteriori informazioni su questi argomenti, vedere la documentazione di IIS 7 in MSDN o a [www.iis.net](https://www.iis.net/).


## <a name="step-1-defining-url-authorization-rules-inwebconfig"></a>Passaggio 1: Definizione di regole di autorizzazione URL in`Web.config`

Il `UrlAuthorizationModule` determina se concedere o negare l'accesso a una risorsa richiesta per una determinata identità in base alle regole di autorizzazione URL definite nella configurazione dell'applicazione. Le regole di autorizzazione sono dichiarate nel [ `<authorization>` elemento](https://msdn.microsoft.com/library/8d82143t.aspx) sotto forma di `<allow>` e `<deny>` gli elementi figlio. Ogni `<allow>` e `<deny>` elemento figlio è possibile specificare:

- Un utente specifico
- Un elenco delimitato da virgole di utenti
- Tutti gli utenti anonimi, rappresentati da un punto interrogativo (?)
- Tutti gli utenti, identificati da un asterisco (\*)

Il markup seguente viene illustrato come utilizzare le regole di autorizzazione URL per consentire agli utenti Tito e Scott e negare a tutti gli altri:

[!code-xml[Main](user-based-authorization-cs/samples/sample1.xml)]

Il `<allow>` elemento definisce gli utenti che sono consentiti - Tito e Scott - mentre il `<deny>` elemento indica che *tutti* viene negato agli utenti.

> [!NOTE]
> Il `<allow>` e `<deny>` gli elementi possono anche specificare le regole di autorizzazione per i ruoli. Autorizzazione basata sui ruoli in un'esercitazione in futura verrà esaminato.


L'impostazione seguente concede l'accesso a chiunque, eccetto Sam (inclusi i visitatori anonimi):

[!code-xml[Main](user-based-authorization-cs/samples/sample2.xml)]

Per consentire solo agli utenti autenticati, utilizzare la configurazione seguente, che nega l'accesso a tutti gli utenti anonimi:

[!code-xml[Main](user-based-authorization-cs/samples/sample3.xml)]

Le regole di autorizzazione definite all'interno di `<system.web>` elemento `Web.config` e si applicano a tutte le risorse ASP.NET nell'applicazione web. Spesso, un'applicazione include regole di autorizzazione diversi per le diverse sezioni. Ad esempio, in un sito di e-commerce, tutti i visitatori possono studiando i prodotti, vedere recensioni dei prodotti, il catalogo di ricerca e così via. Tuttavia, solo gli utenti autenticati possono raggiungere l'estrazione o le pagine per la gestione di cronologia della spedizione. Inoltre, può essere parti del sito che sono solo accessibili dagli utenti di selezionare, ad esempio gli amministratori del sito.

ASP.NET consente di definire le regole di autorizzazione diversi per diversi file e cartelle nel sito. Le regole di autorizzazione specificate nella cartella radice `Web.config` file si applicano a tutte le risorse ASP.NET nel sito. Tuttavia, queste impostazioni di autorizzazione predefinito possono essere sottoposto a override per una particolare cartella aggiungendo un `Web.config` con un `<authorization>` sezione.

Consente di aggiornare il sito Web in modo che solo gli utenti autenticati possono visitare le pagine ASP.NET il `Membership` cartella. A tale scopo è necessario aggiungere un `Web.config` file per il `Membership` cartella e impostare le impostazioni di autorizzazione per negare agli utenti anonimi. Fare doppio clic su di `Membership` cartella in Esplora soluzioni, scegliere il menu Aggiungi nuovo elemento dal menu di scelta rapida e aggiungere un nuovo File di configurazione Web denominato `Web.config`.


[![Aggiungere un File Web. config nella cartella di appartenenza](user-based-authorization-cs/_static/image8.png)](user-based-authorization-cs/_static/image7.png)

**Figura 3**: aggiungere un `Web.config` File per il `Membership` cartella ([fare clic per visualizzare l'immagine ingrandita](user-based-authorization-cs/_static/image9.png))


A questo punto il progetto deve contenere due `Web.config` file: uno nella directory radice e uno nel `Membership` cartella.


[![L'applicazione dovrebbe ora contenere due file Web. config](user-based-authorization-cs/_static/image11.png)](user-based-authorization-cs/_static/image10.png)

**Figura 4**: l'applicazione dovrebbe ora contenere due `Web.config` file ([fare clic per visualizzare l'immagine ingrandita](user-based-authorization-cs/_static/image12.png))


Aggiornare il file di configurazione nel `Membership` cartella in modo che impedisce l'accesso agli utenti anonimi.

[!code-xml[Main](user-based-authorization-cs/samples/sample4.xml)]

Questo è tutto qui!

Per testare questa modifica, visitare la home page in un browser e verificare che si è connessi. Poiché il comportamento predefinito di un'applicazione ASP.NET per consentire tutti i visitatori e poiché è non apportare alcuna modifica di autorizzazione per la directory radice `Web.config` file, è possibile visitare i file nella directory radice di un visitatore anonimo.

Fare clic sul collegamento creazione degli account utente rilevato nella colonna a sinistra. Verrà visualizzata per il `~/Membership/CreatingUserAccounts.aspx`. Poiché il `Web.config` file nel `Membership` cartella vengono definite le regole di autorizzazione per impedire l'accesso anonimo, il `UrlAuthorizationModule` interrompe la richiesta e restituisce uno stato HTTP 401 non autorizzato. Il `FormsAuthenticationModule` questa modifica allo stato di reindirizzamento 302, invia alla pagina di accesso. Si noti che la pagina è durante il tentativo di accesso (`CreatingUserAccounts.aspx`) viene passato alla pagina di accesso tramite il `ReturnUrl` parametro querystring.


[![Perché l'URL autorizzazione regole non consentire l'accesso anonimo, si viene reindirizzati alla pagina di accesso](user-based-authorization-cs/_static/image14.png)](user-based-authorization-cs/_static/image13.png)

**Figura 5**: poiché l'autorizzazione URL regole non consentire l'accesso anonimo, si viene reindirizzati alla pagina di accesso ([fare clic per visualizzare l'immagine ingrandita](user-based-authorization-cs/_static/image15.png))


Seguito all'accesso, si viene reindirizzati al `CreatingUserAccounts.aspx` pagina. Questa volta il `UrlAuthorizationModule` consente l'accesso alla pagina perché non verrà più anonimi.

### <a name="applying-url-authorization-rules-to-a-specific-location"></a>L'applicazione delle regole di autorizzazione URL in un percorso specifico

Le impostazioni di autorizzazione definite nel `<system.web>` sezione `Web.config` si applicano a tutte le risorse ASP.NET in tale directory e nelle relative sottodirectory (fino a quando non in caso contrario, viene sottoposto a override da un altro `Web.config` file). In alcuni casi, tuttavia, si può decidere di tutte le risorse ASP.NET in una directory specificata per una particolare configurazione dell'autorizzazione ad eccezione di uno o due pagine specifiche. Questo può essere ottenuto mediante l'aggiunta di un `<location>` elemento `Web.config`modo che punti al file le cui regole di autorizzazione diversi e definire le regole di autorizzazione univoco al suo interno.

Per illustrare l'utilizzo di `<location>` elemento da sostituire le impostazioni di configurazione per una risorsa specifica, consente di personalizzare le impostazioni di autorizzazione in modo che solo Tito può visitare `CreatingUserAccounts.aspx`. A tale scopo, aggiungere un `<location>` elemento per il `Membership` della cartella `Web.config` file e aggiornare il codice in modo che risulti simile al seguente:

[!code-xml[Main](user-based-authorization-cs/samples/sample5.xml)]

Il `<authorization>` elemento `<system.web>` definisce le regole di autorizzazione URL predefinito per le risorse in ASP.NET il `Membership` cartella e nelle relative sottocartelle. Il `<location>` elemento consente di eseguire l'override di queste regole per una particolare risorsa. Nel codice precedente il `<location>` riferimenti a elementi di `CreatingUserAccounts.aspx` pagina e specifica l'autorizzazione regole tali da consentire Tito, ma negare tutti gli altri utenti.

Per testare la modifica di questa autorizzazione, avviare visitando il sito Web come utente anonimo. Se si tenta di visitare una pagina il `Membership` cartella, ad esempio `UserBasedAuthorization.aspx`, `UrlAuthorizationModule` rifiuterà la richiesta e si verrà reindirizzati alla pagina di accesso. Dopo l'accesso come, ad esempio, Scott, è possibile visitare una pagina di `Membership` cartella *tranne* per `CreatingUserAccounts.aspx`. Il tentativo di visitare `CreatingUserAccounts.aspx` connessi come tutti gli utenti ma Tito comporterà un tentativo di accesso non autorizzato, il reindirizzamento si torna alla pagina di accesso.

> [!NOTE]
> Il `<location>` elemento deve trovarsi all'esterno della configurazione `<system.web>` elemento. È necessario utilizzare un apposito `<location>` elemento per ogni risorsa di cui si desidera sostituire le impostazioni di autorizzazione.


### <a name="a-look-at-how-theurlauthorizationmoduleuses-the-authorization-rules-to-grant-or-deny-access"></a>Esaminare il modo in`UrlAuthorizationModule`utilizza le regole di autorizzazione per concedere o negare l'accesso

Il `UrlAuthorizationModule` determina se per autorizzare una specifica identità per un URL specifico analizzando l'autorizzazione URL regole uno alla volta, a partire dal primo e l'utilizzo dirigendo verso il basso. Non appena viene rilevata una corrispondenza, l'utente viene concesso o negato l'accesso, a seconda se è stata trovata la corrispondenza un `<allow>` o `<deny>` elemento. **Se viene trovata alcuna corrispondenza, l'utente viene concesso l'accesso.** Di conseguenza, se si desidera limitare l'accesso, è essenziale usare un `<deny>` elemento come ultimo elemento nella configurazione di autorizzazione URL. **Se si omette un * * *`<deny>`* * * elemento, tutti gli utenti verranno concesso l'accesso.**

Per comprendere meglio il processo utilizzato per il `UrlAuthorizationModule` per determinare l'autorità, considerare l'esempio regole di autorizzazione URL è stato esaminato in precedenza in questo passaggio. La prima regola è un `<allow>` elemento che consente l'accesso a Tito e Scott. Le regole secondo un `<deny>` elemento che nega l'accesso a tutti gli utenti. Se un utente anonimo visita, il `UrlAuthorizationModule` è anonimo inizia richiedendo, Scott o Tito? La risposta, è ovviamente, No, in modo che consente di passare alla seconda regola. È anonimo nel set di tutti gli utenti? Poiché la risposta qui è Sì, il `<deny>` regola viene inserita in effetti e visitatore viene reindirizzato alla pagina di accesso. Analogamente, se la visita di Jisun, il `UrlAuthorizationModule` chieda, Jisun è Scott o Tito? Dal momento che non lo è, il `UrlAuthorizationModule` consente di passare alla seconda domanda, è Jisun nel set di tutti gli utenti? Mary è, in modo che, troppo, viene negata l'accesso. Infine, se Tito visita, la prima domanda provocati dal `UrlAuthorizationModule` è una risposta affermativa, pertanto Tito viene concesso l'accesso.

Poiché il `UrlAuthorizationModule` processi le regole di autorizzazione dall'alto verso il basso, arresto in corrispondenza di qualsiasi tipo, è importante che le regole più specifiche precedere quelle meno specifiche. Ovvero, per definire le regole di autorizzazione che impediscono Jisun e gli utenti anonimi, ma consentono tutti gli altri utenti autenticati, iniziare con la regola più specifica - Jisun influisce negativamente su una - e quindi procedere con le regole meno specifiche - quelli consentendo a tutti gli altri gli utenti autenticati, ma vengono negati tutti gli utenti anonimi. Le regole di autorizzazione URL seguente implementa questo criterio prima negare Jisun e successivamente negare a utenti anonimi. Qualsiasi utente autenticato diverso Jisun verrà concesso l'accesso perché nessuna di queste `<deny>` istruzioni corrisponderanno.

[!code-xml[Main](user-based-authorization-cs/samples/sample6.xml)]

## <a name="step-2-fixing-the-workflow-for-unauthorized-authenticated-users"></a>Passaggio 2: Correzione del flusso di lavoro per gli utenti non autorizzati, autenticati

Come accennato in precedenza in questa esercitazione in A Look nella sezione del flusso di lavoro autorizzazione URL, in qualsiasi momento risulti una richiesta non autorizzata, il `UrlAuthorizationModule` interrompe la richiesta e restituisce uno stato HTTP 401 non autorizzato. Questo stato 401 viene modificata tramite il `FormsAuthenticationModule` in un 302 reindirizzare stato che invia l'utente alla pagina di accesso. Questo flusso di lavoro si verifica in qualsiasi richiesta non autorizzata, anche se l'utente viene autenticato.

Restituzione di un utente autenticato alla pagina di accesso è probabile che deve essere confuso perché è già stato eseguito nel sistema. Con un minimo di lavoro migliorare il flusso di lavoro mediante il reindirizzamento di autenticazione degli utenti che eseguono le richieste non autorizzate a una pagina che spiega che tentano di accedere a una pagina con restrizioni.

Iniziare creando una nuova pagina ASP.NET nella cartella radice dell'applicazione web denominata `UnauthorizedAccess.aspx`; non dimenticare di associare questa pagina con il `Site.master` pagina master. Dopo aver creato questa pagina, rimuovere il controllo contenuto che fa riferimento il `LoginContent` ContentPlaceHolder in modo che la pagina master predefinita del contenuto verranno visualizzati. Successivamente, aggiungere un messaggio che descrive la situazione, vale a dire che l'utente ha tentato di accedere a una risorsa protetta. Dopo l'aggiunta di un messaggio, il `UnauthorizedAccess.aspx` markup dichiarativo della pagina dovrebbe essere simile al seguente:

[!code-aspx[Main](user-based-authorization-cs/samples/sample7.aspx)]

È ora necessario modificare il flusso di lavoro in modo che se viene eseguita una richiesta non autorizzata da un utente autenticato sono inviati per la `UnauthorizedAccess.aspx` pagina anziché la pagina di accesso. La logica che reindirizza le richieste non autorizzate alla pagina di accesso viene utilizzata nell'ambito di un metodo privato del `FormsAuthenticationModule` classe, pertanto è possibile personalizzare questo comportamento. Cosa possiamo fare, tuttavia, viene aggiunta la logica alla pagina di accesso che reindirizza l'utente per `UnauthorizedAccess.aspx`, se necessario.

Quando il `FormsAuthenticationModule` un visitatore non autorizzato reindirizza alla pagina di accesso viene aggiunto l'URL richiesto non è autorizzato per la stringa di query con il nome `ReturnUrl`. Ad esempio, se un utente non autorizzato tenta di visitare `OnlyTito.aspx`, `FormsAuthenticationModule` reindirizzarle al `Login.aspx?ReturnUrl=OnlyTito.aspx`. Pertanto, se viene raggiunta la pagina di accesso da un utente autenticato con una stringa di query che include il `ReturnUrl` parametro, quindi si sa che questo utente non autenticato ha tentato solo di visitare una pagina che non è autorizzata a visualizzare. In tal caso, si desidera reindirizzare a `UnauthorizedAccess.aspx`.

A tale scopo, aggiungere il codice seguente alla pagina di accesso `Page_Load` gestore eventi:

[!code-csharp[Main](user-based-authorization-cs/samples/sample8.cs)]

Il codice sopra riportato reindirizza gli utenti autenticati non autorizzati per il `UnauthorizedAccess.aspx` pagina. Per visualizzare questa logica di azione, visitare il sito come un visitatore anonimo e fare clic sul collegamento nella colonna sinistra creazione degli account utente. Verrà visualizzata per il `~/Membership/CreatingUserAccounts.aspx` pagina, che nel passaggio 1 è stato configurato solo per consentire l'accesso a Tito. Poiché gli utenti anonimi non sono consentiti, la `FormsAuthenticationModule` ci reindirizza alla pagina di accesso.

A questo punto siamo anonimi, pertanto `Request.IsAuthenticated` restituisce `false` e non si viene reindirizzati alla `UnauthorizedAccess.aspx`. Al contrario, verrà visualizzata la pagina di accesso. Accedere come un utente diverso Tito, ad esempio Bruce. Dopo aver immesso le credenziali appropriate, l'account di accesso pagina ci Reindirizza al `~/Membership/CreatingUserAccounts.aspx`. Poiché questa pagina è accessibile soltanto ai Tito, ci sono non autorizzati a visualizzarlo e vengono restituiti immediatamente alla pagina di accesso. Questa volta, tuttavia, `Request.IsAuthenticated` restituisce `true` (e `ReturnUrl` parametro querystring esiste), pertanto si viene reindirizzati al `UnauthorizedAccess.aspx` pagina.


[![L'autenticazione, gli utenti non autorizzati vengono reindirizzati a UnauthorizedAccess.aspx](user-based-authorization-cs/_static/image17.png)](user-based-authorization-cs/_static/image16.png)

**Figura 6**: autenticazione, vengono reindirizzati gli utenti non autorizzati `UnauthorizedAccess.aspx` ([fare clic per visualizzare l'immagine ingrandita](user-based-authorization-cs/_static/image18.png))


Questo flusso di lavoro personalizzato presenta un'esperienza utente più appropriati e semplice da operazioni di corto circuito il ciclo illustrato nella figura 2.

## <a name="step-3-limiting-functionality-based-on-the-currently-logged-in-user"></a>Passaggio 3: Limitando funzionalità basate sull'utente attualmente connesso

Autorizzazione URL semplifica specificare le regole di autorizzazione grossolana. Come illustrato nel passaggio 1, con l'autorizzazione URL è possibile sinteticamente sullo stato sono consentite le identità e quelli non autorizzati alla visualizzazione di una pagina specifica o tutte le pagine in una cartella. In determinati scenari, tuttavia, potrebbe voler consentire tutti gli utenti di visitare una pagina, ma limitarne la funzionalità della pagina in base all'utente visita.

Si consideri il caso di un sito Web di e-commerce che consente di esaminare i loro prodotti autenticato. Quando un utente anonimo visita la pagina del prodotto, si vedrà solo le informazioni sul prodotto e potrebbe non avere la possibilità di lasciare una revisione. Tuttavia, un utente autenticato, visitare la pagina stessa vedrà l'interfaccia di revisione. Se l'utente autenticato non era ancora rivisti questo prodotto, l'interfaccia consenta loro di inviare una revisione; in caso contrario verrà inizialmente visualizzato li loro revisione inviati in precedenza. Per eseguire un passaggio di questo scenario, inoltre, pagina del prodotto potrebbe visualizzare informazioni aggiuntive e offrono funzionalità estese per gli utenti che lavorano per l'azienda e-commerce. Pagina del prodotto, ad esempio, potrebbe essere elenco dell'inventario in magazzino e includere opzioni per modificare il prezzo del prodotto e descrizione quando visitato da un dipendente.

Tali regole di autorizzazione singole possono essere implementate in modo dichiarativo o a livello di codice (o tramite una combinazione dei due). Nella sezione successiva viene verrà illustrato come implementare l'autorizzazione grana tramite il controllo LoginView. Successivamente, verranno analizzati tecniche a livello di codice. Prima di poter esaminare l'applicazione delle regole di autorizzazione grana, tuttavia, è necessario creare una pagina di cui funzionalità dipende dall'utente, visitare.

Creare una pagina che elenca i file in una directory specifica all'interno di un controllo GridView. Insieme a elenchi di ogni nome di file, dimensioni e altre informazioni, GridView includerà due colonne di LinkButton: uno denominato Vista e una Delete titolo. Se si fa clic sull'elemento LinkButton visualizzazione, verrà visualizzato il contenuto del file selezionato; Se si fa clic sull'elemento LinkButton eliminazione, il file verrà eliminato. Inizialmente creiamo questa pagina in modo che la funzionalità di visualizzazione e l'eliminazione è disponibile per tutti gli utenti. Utilizzare le sezioni a livello di programmazione delle funzionalità di limitazione e controllo LoginView verrà spiegato come abilitare o disabilitare queste funzionalità in base all'utente visita la pagina.

> [!NOTE]
> La pagina ASP.NET che si sta tentando di compilare utilizza un controllo GridView per visualizzare un elenco di file. Poiché in questa esercitazione serie è incentrata sulla autenticazione basata su form, autorizzazione, gli account utente e ruoli, non si desidera dedicare troppo tempo che illustrano il funzionamento interno del controllo GridView. Durante questa esercitazione vengono fornite istruzioni dettagliate specifiche per la configurazione di questa pagina, non approfondire i dettagli di perché in cui sono state apportate alcune scelte, o quali dispone di proprietà particolare effetto sull'output sottoposto a rendering. Per un esame esauriente del controllo GridView, consultare il  *[utilizzo dei dati in ASP.NET 2.0](../../data-access/index.md)*  serie di esercitazioni.


Aprire il `UserBasedAuthorization.aspx` file nel `Membership` cartella e aggiunta di un controllo GridView alla pagina denominata `FilesGrid`. Smart tag del controllo GridView, fare clic sul collegamento di modifica colonne per avviare la finestra di dialogo campi. Da qui, deselezionare l'opzione genera automaticamente i campi casella di controllo nell'angolo inferiore sinistro. Successivamente, aggiungere un pulsante Seleziona un pulsante Elimina e due BoundField dall'angolo superiore sinistro (i pulsanti di selezione e Delete sono reperibile in corrispondenza del tipo CommandField). Impostare il pulsante Seleziona `SelectText` proprietà alla visualizzazione e il primo BoundField `HeaderText` e `DataField` proprietà al nome. Impostare il secondo BoundField `HeaderText` proprietà alla dimensione in byte, relativo `DataField` proprietà lunghezza, il relativo `DataFormatString` proprietà {0: N0} e il relativo `HtmlEncode` la proprietà su False.

Dopo aver configurato le colonne del controllo GridView, fare clic su OK per chiudere la finestra di dialogo campi. La finestra Proprietà impostare il controllo GridView `DataKeyNames` proprietà `FullName`. Markup dichiarativo del controllo GridView a questo punto dovrebbe essere simile al seguente:

[!code-aspx[Main](user-based-authorization-cs/samples/sample9.aspx)]

Il markup di GridView creato, si è pronti a scrivere il codice che recupererà i file in una directory specifica e associarle a GridView. Aggiungere il codice seguente alla pagina `Page_Load` gestore eventi:

[!code-csharp[Main](user-based-authorization-cs/samples/sample10.cs)]

Il codice precedente viene usato il [ `DirectoryInfo` classe](https://msdn.microsoft.com/library/system.io.directoryinfo.aspx) per ottenere un elenco dei file nella cartella radice dell'applicazione. Il [ `GetFiles()` metodo](https://msdn.microsoft.com/library/system.io.directoryinfo.getfiles.aspx) restituisce come una matrice di tutti i file nella directory [ `FileInfo` oggetti](https://msdn.microsoft.com/library/system.io.fileinfo.aspx), che viene quindi associato a GridView. Il `FileInfo` oggetto dispone di un'ampia gamma di proprietà, ad esempio `Name`, `Length`, e `IsReadOnly`, tra gli altri. Come si può vedere dal relativo codice dichiarativo, GridView Visualizza solo il `Name` e `Length` proprietà.

> [!NOTE]
> Il `DirectoryInfo` e `FileInfo` in cui sono state trovate classi il [ `System.IO` dello spazio dei nomi](https://msdn.microsoft.com/library/system.io.aspx). Pertanto, è necessario anteporre questi nomi di classe con i relativi spazi dei nomi o nello spazio dei nomi importati nel file di classe (tramite `using System.IO`).


È opportuno visitare questa pagina tramite un browser. Visualizzerà l'elenco di file che si trovano nella directory radice dell'applicazione. Fare clic su qualsiasi della visualizzazione o eliminazione LinkButton causerà un postback, ma si verificherà alcuna azione perché è stata ancora per creare i gestori di eventi necessarie.


[![GridView sono elencati i file nella Directory radice dell'applicazione Web](user-based-authorization-cs/_static/image20.png)](user-based-authorization-cs/_static/image19.png)

**Figura 7**: GridView sono elencati i file nella Directory radice dell'applicazione Web ([fare clic per visualizzare l'immagine ingrandita](user-based-authorization-cs/_static/image21.png))


È necessario un modo per visualizzare il contenuto del file selezionato. Tornare a Visual Studio e aggiungere una casella di testo denominato `FileContents` sopra il controllo GridView. Impostare il relativo `TextMode` proprietà `MultiLine` e il relativo `Columns` e `Rows` proprietà al 95% e 10, rispettivamente.

[!code-aspx[Main](user-based-authorization-cs/samples/sample11.aspx)]

Successivamente, creare un gestore eventi per il controllo GridView [ `SelectedIndexChanged` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindexchanged.aspx) e aggiungere il codice seguente:

[!code-csharp[Main](user-based-authorization-cs/samples/sample12.cs)]

Questo codice Usa il controllo GridView `SelectedValue` proprietà per determinare il nome file completo del file selezionato. Internamente, il `DataKeys` insieme viene fatto riferimento per ottenere il `SelectedValue`, pertanto è necessario impostare il controllo GridView `DataKeyNames` proprietà per nome, come descritto in precedenza in questo passaggio. Il [ `File` classe](https://msdn.microsoft.com/library/system.io.file.aspx) viene utilizzato per leggere il contenuto del file selezionato in una stringa, che viene quindi assegnato al `FileContents` della casella di testo `Text` proprietà, in tal modo la visualizzazione del contenuto del file selezionato nella pagina.


[![Contenuto del File selezionato viene visualizzato nella casella di testo](user-based-authorization-cs/_static/image23.png)](user-based-authorization-cs/_static/image22.png)

**Figura 8**: contenuto del File selezionato viene visualizzato nella casella di testo ([fare clic per visualizzare l'immagine ingrandita](user-based-authorization-cs/_static/image24.png))


> [!NOTE]
> Se si visualizza il contenuto di un file che contiene markup HTML e quindi si tenta di visualizzare o eliminare un file, si riceverà un `HttpRequestValidationException` errore. Questo errore si verifica perché durante il postback contenuto della casella di testo viene inviate nuovamente al server web. Per impostazione predefinita, ASP.NET genera un `HttpRequestValidationException` errore ogni volta che viene rilevato contenuto postback potenzialmente pericoloso, ad esempio markup HTML. Per disabilitare l'errore che si verifichi, disattivare la convalida delle richieste per la pagina aggiungendo `ValidateRequest="false"` per il `@Page` direttiva. Per ulteriori informazioni sui vantaggi di convalida della richiesta come nonché le precauzioni necessarie quando disabilitarlo, leggere [la convalida della richiesta - impedisce attacchi Script](https://asp.net/learn/whitepapers/request-validation/).


Infine, aggiungere un gestore eventi con il codice seguente per il controllo GridView [ `RowDeleting` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleting.aspx):

[!code-csharp[Main](user-based-authorization-cs/samples/sample13.cs)]

Il codice visualizza semplicemente il nome completo del file da eliminare nella `FileContents` TextBox *senza* effettivamente l'eliminazione del file.


[![Fare clic sul pulsante Delete non elimina effettivamente il File](user-based-authorization-cs/_static/image26.png)](user-based-authorization-cs/_static/image25.png)

**Figura 9**: scegliendo di eliminare pulsante non elimina effettivamente il File ([fare clic per visualizzare l'immagine ingrandita](user-based-authorization-cs/_static/image27.png))


Passaggio 1 è stato configurato le regole di autorizzazione URL per impedire agli utenti anonimi di visualizzare le pagine di `Membership` cartella. Per presentare meglio l'autenticazione con granularità fine, si consente agli utenti anonimi di visitare il `UserBasedAuthorization.aspx` pagina, ma con funzionalità limitate. Per aprire questa pagina per tutti gli utenti di accedere, aggiungere il seguente `<location>` elemento per il `Web.config` file nel `Membership` cartella:

[!code-xml[Main](user-based-authorization-cs/samples/sample14.xml)]

Dopo aver aggiunto questo `<location>` elemento, verificare le nuove regole di autorizzazione URL tramite l'accesso all'esterno del sito. Come utente anonimo deve essere consentito per visitare il `UserBasedAuthorization.aspx` pagina.

Attualmente, qualsiasi utente autenticato o anonimo è possibile visitare il `UserBasedAuthorization.aspx` pagina e consente di visualizzare o eliminare i file. Di seguito fare in modo che solo gli utenti autenticati possono visualizzare il contenuto di un file ed Tito solo è possibile eliminare un file. Tali regole di autorizzazione singole possono essere applicate in modo dichiarativo, a livello di codice o tramite una combinazione di entrambi i metodi. È possibile utilizzare l'approccio dichiarativo per limitare chi può visualizzare il contenuto del file. si userà l'approccio a livello di codice per limitarne l'accesso è stato possibile eliminare un file.

### <a name="using-the-loginview-control"></a>Utilizzare il controllo LoginView

Come abbiamo visto nelle esercitazioni precedenti, il controllo LoginView è utile per la visualizzazione di interfacce diverse per gli utenti autenticati e anonimi e offre un modo semplice per nascondere la funzionalità che non è accessibile agli utenti anonimi. Poiché gli utenti anonimi non è possibile visualizzare o eliminare i file, è necessario solo mostrare il `FileContents` casella di testo quando si visita la pagina da un utente autenticato. A tale scopo, aggiungere un controllo LoginView alla pagina, denominarlo `LoginViewForFileContentsTextBox`e spostare il `FileContents` dichiarativo della casella di testo del controllo LoginView `LoggedInTemplate`.

[!code-aspx[Main](user-based-authorization-cs/samples/sample15.aspx)]

I controlli Web nei modelli di LoginView non sono più accessibili direttamente dalla classe code-behind. Ad esempio, il `FilesGrid` GridView `SelectedIndexChanged` e `RowDeleting` gestori eventi fanno riferimento al `FileContents` controllo casella di testo con il codice come:

[!code-csharp[Main](user-based-authorization-cs/samples/sample16.cs)]

Tuttavia, questo codice non è più valido. Spostando il `FileContents` nella casella di testo di `LoggedInTemplate` la casella di testo non è possibile accedere direttamente. In alternativa, è necessario utilizzare il `FindControl("controlId")` metodo a livello di codice fa riferimento il controllo. Aggiornamento di `FilesGrid` gestori eventi per fare riferimento la casella di testo come illustrato di seguito:

[!code-csharp[Main](user-based-authorization-cs/samples/sample17.cs)]

Dopo avere spostato la casella di testo di LoginView `LoggedInTemplate` e aggiornamento del codice della pagina di riferimento che la casella di testo utilizzando il `FindControl("controlId")` di schema, visitare la pagina come utente anonimo. Come mostrato nella figura 10, il `FileContents` casella di testo non viene visualizzata. Tuttavia, viene ancora visualizzato sull'elemento LinkButton vista.


[![Il controllo LoginView visualizza solo la casella di testo FileContents per gli utenti autenticati](user-based-authorization-cs/_static/image29.png)](user-based-authorization-cs/_static/image28.png)

**Figura 10**: il LoginView controllo solo esegue il rendering di `FileContents` casella di testo per gli utenti autenticati ([fare clic per visualizzare l'immagine ingrandita](user-based-authorization-cs/_static/image30.png))


Un modo per nascondere il pulsante di visualizzazione per gli utenti anonimi consiste nel convertire il campo di GridView in un TemplateField. Verrà generato un modello contenente il markup dichiarativo per la visualizzazione di LinkButton. È quindi possibile aggiungere un controllo LoginView il TemplateField e inserire il LinkButton all'interno di LoginView `LoggedInTemplate`, creando così come nascondere il pulsante di visualizzazione dai visitatori anonimi. A tale scopo, fare clic sul collegamento Modifica colonne del controllo GridView Smart tag per avviare la finestra di dialogo campi. Successivamente, selezionare il pulsante Seleziona dall'elenco in basso a sinistra e quindi fare clic su Converti questo campo per un collegamento TemplateField. In questo modo verrà modificata dichiarativo del campo da:

[!code-aspx[Main](user-based-authorization-cs/samples/sample18.aspx)]

 A: 

[!code-aspx[Main](user-based-authorization-cs/samples/sample19.aspx)]

A questo punto possiamo aggiungere un LoginView nel TemplateField. Il markup seguente consente di visualizzare la vista LinkButton solo per gli utenti autenticati.

[!code-aspx[Main](user-based-authorization-cs/samples/sample20.aspx)]

Come mostrato nella figura 11, il risultato finale non è che piuttosto come visualizzazione colonna viene ancora visualizzata anche se la vista LinkButton all'interno della colonna sono nascoste. Come nascondere l'intera colonna GridView (e non solo sull'elemento LinkButton) verranno esaminati nella sezione successiva.


[![Il controllo LoginView nasconde LinkButton la visualizzazione per i visitatori anonimi](user-based-authorization-cs/_static/image32.png)](user-based-authorization-cs/_static/image31.png)

**Figura 11**: il controllo LoginView nasconde LinkButton la visualizzazione per i visitatori anonimi ([fare clic per visualizzare l'immagine ingrandita](user-based-authorization-cs/_static/image33.png))


### <a name="programmatically-limiting-functionality"></a>Funzionalità di limitazione a livello di codice

In alcuni casi, non sono sufficienti per limitare la funzionalità a una pagina dichiarative tecniche. Ad esempio, la disponibilità di alcune funzionalità della pagina potrebbe essere dipendente da criteri oltre se l'utente visita la pagina è autenticato o anonimo. In questi casi, i vari elementi dell'interfaccia utente possono essere visualizzati o nascosti a livello di.

Per limitare le funzionalità a livello di codice, è necessario eseguire due attività:

1. Determinare se l'utente visita la pagina può accedere alla funzionalità, e
2. Modificare a livello di codice l'interfaccia utente in base che l'utente disponga dell'accesso alle funzionalità in questione.

Per dimostrare l'applicazione di queste due attività, si consentono solo Tito eliminare file dal controllo GridView. La prima attività, è quindi, per determinare se è Tito visita la pagina. Dopo aver determinato che, è necessario nascondere colonna eliminazione GridView (o visualizzare). Colonne del controllo GridView sono accessibili tramite il relativo `Columns` proprietà; una colonna viene visualizzata solo se il `Visible` è impostata su `true` (impostazione predefinita).

Aggiungere il codice seguente per il `Page_Load` gestore dell'evento prima di associare i dati a GridView:

[!code-csharp[Main](user-based-authorization-cs/samples/sample21.cs)]

Come accennato nel [ *una panoramica dell'autenticazione basata su form* ](../introduction/an-overview-of-forms-authentication-cs.md) esercitazione `User.Identity.Name` restituisce il nome dell'identità. Corrisponde al nome utente immesso nel controllo di accesso. Se è Tito visita, seconda colonna del controllo GridView della pagina `Visible` è impostata su `true`; in caso contrario, viene impostato su `false`. Il risultato finale è che quando un utente diverso da Tito visita la pagina, un altro utente autenticato o un utente anonimo, Elimina la colonna non viene eseguita (vedere Figura 12). Tuttavia, quando Tito visita la pagina, Elimina la colonna è presente (vedere Figura 13).


[![Elimina la colonna non è sottoposto a rendering quando visitato da un utente diverso da Tito (ad esempio Bruce)](user-based-authorization-cs/_static/image35.png)](user-based-authorization-cs/_static/image34.png)

**Figura 12**: eliminare Column non è sottoposto a rendering quando visitato da un utente diverso da Tito (ad esempio Bruce) ([fare clic per visualizzare l'immagine ingrandita](user-based-authorization-cs/_static/image36.png))


[![Elimina colonna viene eseguito il rendering per Tito](user-based-authorization-cs/_static/image38.png)](user-based-authorization-cs/_static/image37.png)

**Figura 13**: eliminare Column viene eseguito il rendering per Tito ([fare clic per visualizzare l'immagine ingrandita](user-based-authorization-cs/_static/image39.png))


## <a name="step-4-applying-authorization-rules-to-classes-and-methods"></a>Passaggio 4: Applicazione di regole di autorizzazione a classi e metodi

Nel passaggio 3 è consentito agli utenti anonimi di visualizzare il contenuto di un file e impediscono l'eliminazione di file a tutti gli utenti ma Tito. A tale scopo come nascondere gli elementi dell'interfaccia utente associato per i visitatori non autorizzati tramite tecniche dichiarativi e a livello di codice. Per questo esempio semplice, correttamente come nascondere gli elementi dell'interfaccia utente è semplice, ma cosa succede siti più complessi in cui potrebbero essere presenti diversi modi per eseguire la stessa funzionalità? Per limitare tale funzionalità per utenti non autorizzati, cosa accade se si dimentica di disattivare tutti gli elementi dell'interfaccia utente applicabili.

È un modo semplice per garantire che una particolare parte della funzionalità non è possibile accedere da un utente non autorizzato per decorare quella classe o metodo con il [ `PrincipalPermission` attributo](https://msdn.microsoft.com/library/system.security.permissions.principalpermissionattribute.aspx). Quando il runtime .NET viene utilizzata una classe o esegue uno dei relativi metodi, verifica per assicurarsi che il contesto di sicurezza corrente dispone dell'autorizzazione per utilizzare la classe o eseguire il metodo. Il `PrincipalPermission` attributo fornisce un meccanismo tramite il quale è possibile definire queste regole.

Di seguito viene illustrato l'utilizzo di `PrincipalPermission` attributo del controllo GridView `SelectedIndexChanged` e `RowDeleting` gestori eventi per impedire l'esecuzione da parte degli utenti anonimi e utenti diversi dal Tito, rispettivamente. Tutto è necessario fare è aggiungere l'attributo appropriato nella parte superiore di ogni definizione di funzione:

[!code-csharp[Main](user-based-authorization-cs/samples/sample22.cs)]

L'attributo per il `SelectedIndexChanged` stabilisce gestore eventi che solo gli utenti autenticavano possono eseguire il gestore dell'evento, mentre l'attributo di `RowDeleting` gestore eventi consente di limitare l'esecuzione del Tito.

Se, in qualche modo, un utente diverso Tito tenta di eseguire il `RowDeleting` gestore dell'evento o un utente autenticato non tenta di eseguire il `SelectedIndexChanged` gestore dell'evento, il runtime .NET genererà un `SecurityException`.


[![Se il contesto di sicurezza non è autorizzato a eseguire il metodo, viene generata un'eccezione SecurityException](user-based-authorization-cs/_static/image41.png)](user-based-authorization-cs/_static/image40.png)

**Nella figura 14**: se il contesto di sicurezza non è autorizzato a eseguire il metodo, un `SecurityException` viene generata un'eccezione ([fare clic per visualizzare l'immagine ingrandita](user-based-authorization-cs/_static/image42.png))


> [!NOTE]
> Per consentire più contesti di sicurezza accedere a una classe o metodo, decorare la classe o metodo con un `PrincipalPermission` attributo per ogni contesto di sicurezza. Vale a dire, per consentire Tito e Bruce eseguire il `RowDeleting` gestore dell'evento, aggiungere *due* `PrincipalPermission` attributi:


[!code-csharp[Main](user-based-authorization-cs/samples/sample23.cs)]

Oltre alle pagine ASP.NET, molte applicazioni hanno inoltre un'architettura che include vari livelli, ad esempio la logica di Business e i livelli di accesso ai dati. Questi livelli vengono in genere implementati come librerie di classi e offrono le classi e metodi per l'esecuzione delle funzionalità relative ai dati e logica di business. Il `PrincipalPermission` attributo è utile per l'applicazione delle regole di autorizzazione a questi livelli.

Per ulteriori informazioni sull'utilizzo di `PrincipalPermission` attributo per definire le regole di autorizzazione per le classi e metodi, fare riferimento a [Scott Guthrie](https://weblogs.asp.net/scottgu/)del post di blog [aggiungendo le regole di autorizzazione Business e i livelli di dati mediante `PrincipalPermissionAttributes` ](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx).

## <a name="summary"></a>Riepilogo

In questa esercitazione è stato illustrato come applicare le regole di autorizzazione basata sull'utente. Siamo partiti da esaminare ASP. Framework di autorizzazione URL della rete. Per del ogni richiesta, il motore ASP.NET `UrlAuthorizationModule` controlla le regole di autorizzazione URL definite nella configurazione dell'applicazione per determinare se l'identità è autorizzato ad accedere alla risorsa richiesta. In breve, autorizzazione URL consente di specificare le regole di autorizzazione per una pagina specifica o per tutte le pagine in una directory specifica.

Il framework di autorizzazione URL applica le regole di autorizzazione in base a una pagina per pagina. Con l'autorizzazione URL, entrambi l'identità del richiedente è autorizzato per accedere a una particolare risorsa o non. Molti scenari, tuttavia, chiamare per le regole di autorizzazione più grana. Invece di definire chi è autorizzato ad accedere a una pagina, è possibile che sia necessario per consentire a tutti gli utenti di accedere a una pagina, ma visualizzare dati diversi oppure offrire funzionalità diverse a seconda dell'utente visita la pagina. Autorizzazione a livello di pagina implica in genere come nascondere gli elementi dell'interfaccia utente specifico per impedire agli utenti non autorizzati di accedere alle funzionalità non consentita. Inoltre, è possibile utilizzare gli attributi per limitare l'accesso a classi e l'esecuzione dei relativi metodi per determinati utenti.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Aggiunta di regole di autorizzazione di Business e i livelli di dati tramite`PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)
- [Autorizzazione ASP.NET](https://msdn.microsoft.com/library/wce3kxhd.aspx)
- [Modifiche tra IIS 6 e IIS 7 di sicurezza](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/Changes-between-IIS6-and-IIS7-Security)
- [Configurazione di sottodirectory e file specifici](https://msdn.microsoft.com/library/6hbkh9s7.aspx)
- [Limitazione della funzionalità di modifica dei dati in base all'utente](../../data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-cs.md)
- [Guida introduttiva di controllo LoginView](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/loginview.aspx)
- [Informazioni di autorizzazione URL IIS 7](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization)
- [`UrlAuthorizationModule`Documentazione tecnica](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx)
- [Utilizzo dei dati in ASP.NET 2.0](../../data-access/index.md)

### <a name="about-the-author"></a>Informazioni sull'autore

Scott Mitchell, autore di più libri e fondatore di 4GuysFromRolla, ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è  *[SAM insegna manualmente ASP.NET 2.0 nelle 24 ore](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott può essere raggiunto al [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) o tramite il suo blog all'indirizzo [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

>[!div class="step-by-step"]
[Precedente](validating-user-credentials-against-the-membership-user-store-cs.md)
[Successivo](storing-additional-user-information-cs.md)
