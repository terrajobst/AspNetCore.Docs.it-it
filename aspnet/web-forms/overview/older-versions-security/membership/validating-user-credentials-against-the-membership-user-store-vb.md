---
uid: web-forms/overview/older-versions-security/membership/validating-user-credentials-against-the-membership-user-store-vb
title: Convalida delle credenziali dell'utente nell'archivio utente di appartenenza (VB) | Documenti Microsoft
author: rick-anderson
description: "In questa esercitazione verrà esaminato convalidare le credenziali dell'utente, un archivio utente di appartenenza utilizzando sia a livello di codice indica che il controllo di accesso..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: 17772912-b47b-4557-9ce9-80f22df642f7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/membership/validating-user-credentials-against-the-membership-user-store-vb
msc.type: authoredcontent
ms.openlocfilehash: 59becd46684d96f28bec43b7d5b4e0fd50c92117
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="validating-user-credentials-against-the-membership-user-store-vb"></a>Convalida le credenziali utente nell'archivio utente di appartenenza (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare codice](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_06_VB.zip) o [Scarica il PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial06_LoggingIn_vb.pdf)

> In questa esercitazione verrà esaminato convalidare le credenziali dell'utente, un archivio utente di appartenenza utilizzando sia a livello di codice indica che il controllo di accesso. Verranno inoltre esaminati come personalizzare l'aspetto e il comportamento del controllo di accesso.


## <a name="introduction"></a>Introduzione

Nel <a id="Tutorial05"> </a> [esercitazione precedente](creating-user-accounts-vb.md) abbiamo esaminato come creare un nuovo account utente nel framework di appartenenza. Abbiamo esaminato prima a livello di codice la creazione di account utente tramite il `Membership` della classe `CreateUser` (metodo) e quindi esaminato tramite il controllo CreateUserWizard Web. Tuttavia, la pagina di accesso convalida attualmente le credenziali specificate con un elenco hardcoded di coppie di nome utente e password. È necessario aggiornare la logica della pagina di accesso in modo che convalida le credenziali utente archivio di appartenenza framework.

Molto simile alla creazione di account utente, le credenziali possono essere convalidate a livello di programmazione o in modo dichiarativo. L'API di appartenenza include un metodo di convalida a livello di codice le credenziali dell'utente nell'archivio utente. E ASP.NET viene fornito con il controllo di accesso Web, che esegue il rendering di caselle di testo per il nome utente e password e un pulsante per l'accesso un'interfaccia utente.

In questa esercitazione verrà esaminato convalidare le credenziali dell'utente, un archivio utente di appartenenza utilizzando sia a livello di codice indica che il controllo di accesso. Verranno inoltre esaminati come personalizzare l'aspetto e il comportamento del controllo di accesso. Iniziamo!

## <a name="step-1-validating-credentials-against-the-membership-user-store"></a>Passaggio 1: Convalida le credenziali nell'archivio utente di appartenenza

Per i siti web che utilizzano l'autenticazione basata su form, un utente accede al sito Web, visitare una pagina di accesso e immettere le proprie credenziali. Queste credenziali vengono quindi confrontate con l'archivio dell'utente. Se sono validi, l'utente viene concesso un ticket di autenticazione form, ovvero un token di sicurezza che indica l'identità e l'autenticità del visitatore.

Per convalidare un utente con il framework di appartenenza, utilizzare il `Membership` della classe [ `ValidateUser` metodo](https://msdn.microsoft.com/en-us/library/system.web.security.membership.validateuser.aspx). Il `ValidateUser` metodo accetta due parametri di input - `username` e `password` - e restituisce un valore booleano che indica se le credenziali sono valide. Come con la `CreateUser` metodo sono state esaminate nell'esercitazione precedente, il `ValidateUser` metodo delega la convalida effettiva al provider di appartenenze configurato.

Il `SqlMembershipProvider` convalida le credenziali fornite per ottenere la password dell'utente specificato tramite il `aspnet_Membership_GetPasswordWithFormat` stored procedure. Tenere presente che il `SqlMembershipProvider` archivia le password degli utenti usando uno dei tre formati: clear, crittografato o con hash. Il `aspnet_Membership_GetPasswordWithFormat` stored procedure restituisce la password nel relativo formato non elaborato. Per le password crittografate o con hash, il `SqlMembershipProvider` trasforma il `password` valore passato il `ValidateUser` metodo nel relativo equivalente crittografati o eseguito l'hashing dello stato e quindi lo confronta con ciò che è stato restituito dal database. Se la password memorizzata nel database corrisponde alla password formattata immessa dall'utente, le credenziali sono valide.

Consente di aggiornare la pagina di accesso (~ /`Login.aspx`) in modo che la convalida delle credenziali fornite nell'archivio utente di appartenenza framework. È stata creata la pagina di accesso nel <a id="Tutorial02"> </a> [ *una panoramica dell'autenticazione basata su form* ](../introduction/an-overview-of-forms-authentication-vb.md) esercitazione, la creazione di un'interfaccia con due caselle di testo per il nome utente e password, un Memorizzazione dei dati, casella di controllo e un pulsante di accesso (vedere la figura 1). Il codice di convalida le credenziali immesse in un elenco hardcoded di coppie di nome utente e password (Scott/password, Jisun/password e Sam/password). Nel <a id="Tutorial03"> </a> [ *configurazione dell'autenticazione form e argomenti avanzati* ](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md) esercitazione sono aggiornati di codice della pagina di accesso per archiviare informazioni aggiuntive nei moduli ticket di autenticazione `UserData` proprietà.


[![L'account di accesso dell'interfaccia pagina include due caselle di testo, un controllo CheckBoxList e un pulsante](validating-user-credentials-against-the-membership-user-store-vb/_static/image2.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image1.png)

**Figura 1**: un pulsante interfaccia include due caselle di testo della pagina di accesso di e un controllo CheckBoxList ([fare clic per visualizzare l'immagine ingrandita](validating-user-credentials-against-the-membership-user-store-vb/_static/image3.png))


Interfaccia utente della pagina di accesso può rimangono invariato, ma è necessario sostituire il pulsante di accesso `Click` gestore dell'evento con il codice che convalida l'utente nell'archivio utente di appartenenza framework. Aggiornare il gestore dell'evento in modo che il codice viene visualizzato come segue:

[!code-vb[Main](validating-user-credentials-against-the-membership-user-store-vb/samples/sample1.vb)]

Questo codice è molto semplice. Viene avviata chiamando il `Membership.ValidateUser` , passando il nome utente fornito e la password. Se tale metodo restituisce True, l'utente è connesso al sito tramite il `FormsAuthentication` metodo RedirectFromLoginPage della classe. (Come descritto nel <a id="Tutorial02"> </a> [ *una panoramica dell'autenticazione basata su form* ](../introduction/an-overview-of-forms-authentication-vb.md) esercitazione il `FormsAuthentication.RedirectFromLoginPage` crea le forme di ticket di autenticazione e reindirizza l'utente alla pagina appropriata.) Se le credenziali sono valide, tuttavia, il `InvalidCredentialsMessage` etichetta viene visualizzata, per informare l'utente che il nome utente o la password non è corretta.

Questo è tutto qui!

Per verificare che la pagina di accesso funziona come previsto, tentare di eseguire l'accesso con uno degli account utente creato nell'esercitazione precedente. In alternativa, se non è ancora stato creato un account, vado avanti e crearne uno dal `~/Membership/CreatingUserAccounts.aspx` pagina.

> [!NOTE]
> Quando l'utente immette le proprie credenziali e invia il form di pagina di accesso, le credenziali, inclusa la password, vengono trasmessi tramite Internet al server web in *testo normale*. Ciò significa che un pirata informatico lo sniffing del traffico di rete è possibile visualizzare il nome utente e password. Per evitare questo problema, è essenziale per crittografare il traffico di rete tramite [livelli SSL (Secure Socket)](http://en.wikipedia.org/wiki/Secure_Sockets_Layer). Ciò garantisce che le credenziali (così come markup HTML della pagina intera) vengono crittografati dal momento in cui che lasciano il browser finché vengono ricevuti dal server web.


### <a name="how-the-membership-framework-handles-invalid-login-attempts"></a>Come il Framework di appartenenza gestisce i tentativi di accesso non valido

Quando un visitatore raggiunge la pagina di accesso e invia le proprie credenziali, il browser invia una richiesta HTTP alla pagina di accesso. Se le credenziali siano valide, la risposta HTTP include il ticket di autenticazione in un cookie. Pertanto, un pirata informatico tentando di introdursi nel sito è stato possibile creare un programma che si reputano invia richieste HTTP alla pagina di accesso con un nome utente valido e una probabilità la password. Se l'ipotesi di password sono corretta, la pagina di accesso verrà restituito il cookie di ticket di autenticazione, a quel punto il programma riconosce che è stumbled su una coppia di nome utente/password valida. Tramite attacchi di forza bruta, tale programma potrebbe essere in grado di suo password di un utente, soprattutto se la password è debole.

Per evitare tali attacchi di forza bruta, il framework di appartenenza per bloccare un utente se sono presenti un certo numero di tentativi di accesso non riuscito entro un determinato periodo di tempo. I parametri esatti possono essere configurati tramite le impostazioni della configurazione dei provider di appartenenza due seguenti:

- `maxInvalidPasswordAttempts`-Specifica il numero di password non valida tentativi consentiti per l'utente entro il periodo di tempo prima che l'account è bloccato. Il valore predefinito è 5.
- `passwordAttemptWindow`-indica il periodo di tempo in minuti durante i quali il numero specificato di tentativi di accesso non validi causerà l'account venga bloccato. Il valore predefinito è 10.

Se un utente è stato bloccato, Lei non può accedere fino a quando non viene sbloccato dall'amministratore il proprio account. Quando un utente viene bloccato, il `ValidateUser` metodo verrà *sempre* restituire `False`, anche se vengono fornite credenziali valide. Durante questo comportamento riduce la probabilità che un pirata informatico verrà interrotte nel sito tramite i metodi di attacchi di forza bruta, possono finire blocca un utente valido semplicemente ha dimenticato la password o accidentalmente con il tasto BLOC MAIUSC attivo o ha un giorno digitazione errato.

Purtroppo, non è Nessuno strumento predefinito per lo sblocco di un account utente. Per sbloccare un account, è possibile modificare il database direttamente - modificare il `IsLockedOut` campo il `aspnet_Membership` tabella per l'account utente appropriati - o crearne una basata sul web interfaccia che elenca gli account selezionata con le opzioni per sbloccarli. Creazione di interfacce amministrativa per il completamento delle attività comuni di correlate ai ruoli e account utente in un'esercitazione in futura verrà esaminato.

> [!NOTE]
> Uno svantaggio di `ValidateUser` metodo è che quando le credenziali fornite non sono validi, non fornisce alcun spiegazione perché. Le credenziali possono essere non valide perché non esiste alcuna corrispondenza coppia di nome utente/password nell'archivio dell'utente, o perché l'utente non è ancora stato approvato o perché l'utente è stato bloccato. Nel passaggio 4 vedremo come visualizzare un messaggio più dettagliato per l'utente quando il tentativo di accesso ha esito negativo.


## <a name="step-2-collecting-credentials-through-the-login-web-control"></a>Passaggio 2: Raccogliere le credenziali tramite il controllo di accesso Web

Il [controllo di accesso Web](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.login.aspx) esegue il rendering di molto simile a quello creato in un'interfaccia utente predefinita di <a id="Tutorial02"> </a> [ *una panoramica dell'autenticazione basata su form* ](../introduction/an-overview-of-forms-authentication-vb.md) esercitazione. Utilizzo del controllo di accesso ci consente di risparmiare il lavoro di dover creare l'interfaccia per raccogliere le credenziali del visitatore. Inoltre, il controllo di accesso automaticamente accede l'utente (presumendo che le credenziali presentate siano valide), in tal modo salvataggio ci di dover scrivere alcun codice.

Consente di aggiornare `Login.aspx`, sostituendo l'interfaccia creato manualmente e con un controllo di accesso di codice. Avviare rimuovendo il markup esistente e di codice `Login.aspx`. È possibile eliminarlo definitive oppure semplicemente impostarlo come commento. Per impostare come commento markup dichiarativo, racchiuderlo tra il `<%--` e `--%>` delimitatori. È possibile immettere manualmente questi delimitatori o, come illustrato nella figura 2, è possibile selezionare il testo da impostare come commento e quindi fare clic sul commento l'icona di righe selezionate nella barra degli strumenti. Analogamente, è possibile utilizzare l'icona di righe selezionate come commento per impostare come commento il codice selezionato nella classe code-behind.


[![Impostare come commento il Markup dichiarativo esistente e il codice sorgente in login](validating-user-credentials-against-the-membership-user-store-vb/_static/image5.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image4.png)

**Figura 2**: commento Out il esistente Markup dichiarativo e codice sorgente Login ([fare clic per visualizzare l'immagine ingrandita](validating-user-credentials-against-the-membership-user-store-vb/_static/image6.png))


> [!NOTE]
> Il commento l'icona di righe selezionate non è disponibile quando si visualizza il markup dichiarativo in Visual Studio 2005. Se non si utilizza Visual Studio 2008, è necessario aggiungere manualmente il `<%--` e `--%>` delimitatori.


Quindi, trascinare un controllo di accesso dalla casella degli strumenti sulla pagina e impostare il relativo `ID` proprietà `myLogin`. A questo punto la schermata dovrebbe essere simile alla figura 3. Si noti che l'interfaccia predefinita del controllo di accesso include i controlli casella di testo per il nome utente e password, un memorizza dati successivo casella di controllo e un pulsante nel Log. Sono inoltre disponibili `RequiredFieldValidator` controlli per le due caselle di testo.


[![Aggiungere un controllo di accesso alla pagina](validating-user-credentials-against-the-membership-user-store-vb/_static/image8.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image7.png)

**Figura 3**: aggiungere un controllo di accesso alla pagina ([fare clic per visualizzare l'immagine ingrandita](validating-user-credentials-against-the-membership-user-store-vb/_static/image9.png))


E abbiamo finito! Quando si fa clic sul pulsante Accedi del controllo di accesso, si verificherà un postback e il controllo di accesso chiamerà il `Membership.ValidateUser` , passando il nome utente immesso e la password. Se le credenziali sono valide, il controllo di accesso viene visualizzato un apposito messaggio. Se, tuttavia, le credenziali sono valide, il controllo di accesso crea le forme di ticket di autenticazione e l'utente viene reindirizzato alla pagina appropriata.

Il controllo di accesso utilizza quattro fattori per determinare la pagina appropriata per reindirizzare l'utente a su un account di accesso ha esito positivo:

- Se il controllo di accesso è nella pagina di accesso definito da `loginUrl` impostazione nella configurazione dell'autenticazione form; questa impostazione predefinita è`Login.aspx`
- La presenza di un `ReturnUrl` parametro querystring
- Il valore di controllo di accesso [ `DestinationUrl` proprietà](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.login.destinationpageurl.aspx)
- Il `defaultUrl` valore specificato nei form di impostazioni di configurazione di autenticazione; valore predefinito di questa impostazione è Default.aspx

Figura 4 illustra come il controllo di accesso utilizza questi quattro parametri per giungere a decisione pagina appropriata.


[![Aggiungere un controllo di accesso alla pagina](validating-user-credentials-against-the-membership-user-store-vb/_static/image11.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image10.png)

**Figura 4**: aggiungere un controllo di accesso alla pagina ([fare clic per visualizzare l'immagine ingrandita](validating-user-credentials-against-the-membership-user-store-vb/_static/image12.png))


È opportuno testare il controllo di accesso per visitare il sito tramite un browser e accedere come un utente esistente nel framework di appartenenza.

Interfaccia viene eseguito il rendering del controllo di accesso è configurabile. Esistono una serie di proprietà che determinano l'aspetto; Inoltre, il controllo di accesso può essere convertito in un modello per un controllo preciso il layout di elementi dell'interfaccia utente. Nella parte restante di questo passaggio viene illustrato come personalizzare l'aspetto e il layout.

### <a name="customizing-the-login-controls-appearance"></a>Personalizzazione dell'aspetto del controllo di accesso

Impostazioni predefinite delle proprietà del controllo di accesso il rendering di un'interfaccia utente con un titolo (Log), i controlli casella di testo e l'etichetta per gli input dell'utente e password, un memorizza dati la volta successiva in casella di controllo e un pulsante Accedi. Gli aspetti di questi elementi sono tutti configurabili tramite numerose proprietà del controllo di accesso. Inoltre, è possono inserire elementi dell'interfaccia utente aggiuntive, ad esempio un collegamento a una pagina per creare un nuovo account utente - impostando una proprietà o due.

Richiederà qualche minuto per abbellire l'aspetto del controllo di accesso. Poiché il `Login.aspx` pagina già presente del testo nella parte superiore della pagina di accesso, il titolo del controllo di accesso è superfluo. Pertanto, cancellare il [ `TitleText` proprietà](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.login.titletext.aspx) valore allo scopo di rimuovere il titolo del controllo di accesso.

Il nome utente: e la Password: etichette a sinistra dei due controlli casella di testo possono essere personalizzate tramite la [ `UserNameLabelText` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.login.usernamelabeltext.aspx) e [ `PasswordLabelText` proprietà](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.login.passwordlabeltext.aspx), rispettivamente. Modificare il nome utente: etichetta di leggere il nome utente:. Gli stili di etichetta e casella di testo possono essere configurati tramite la [ `LabelStyle` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.login.labelstyle.aspx) e [ `TextBoxStyle` proprietà](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.login.textboxstyle.aspx), rispettivamente.

Il memorizza dati per la proprietà Text del successiva ora casella di controllo può essere impostato tramite il controllo di accesso [ `RememberMeText` proprietà](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.login.remembermetext.aspx), e all'impostazione predefinita è archiviato lo stato è configurabile tramite il [ `RememberMeSet` proprietà](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.login.remembermeset.aspx)(il valore predefinito è False). Per impostare il `RememberMeSet` proprietà su True in modo che memorizza dati la volta successiva in casella di controllo verrà controllato dall'impostazione predefinita.

Il controllo di accesso sono disponibili due proprietà per regolare il layout dei controlli dell'interfaccia utente. Il [ `TextLayout` proprietà](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.login.textlayout.aspx) indica se il nome utente: e la Password: vengono visualizzate le etichette a sinistra della loro corrispondente nelle caselle di testo (impostazione predefinita) o di sopra di essi. Il [ `Orientation` proprietà](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.login.orientation.aspx) indica se gli input di nome utente e password sono situati in verticale (uno sopra l'altro) o in orizzontale. Ora di lasciare queste due proprietà impostate sui valori predefiniti, ma si consiglia di provare a impostare queste due proprietà in base ai valori non predefiniti per visualizzare l'effetto risulta.

> [!NOTE]
> Nella sezione successiva, la configurazione di Layout del controllo di accesso, verranno esaminati l'utilizzo di modelli per definire il layout preciso di elementi dell'interfaccia utente del controllo di Layout.


Riepilogo impostazioni delle proprietà del controllo di accesso mediante l'impostazione di [ `CreateUserText` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.login.createusertext.aspx) e [ `CreateUserUrl` proprietà](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.login.createuserurl.aspx) a non ancora registrato? Creare un account. e `~/Membership/CreatingUserAccounts.aspx`, rispettivamente. Consente di aggiungere un collegamento ipertestuale all'interfaccia del controllo di accesso che punta alla pagina creata nel <a id="Tutorial05"> </a> [esercitazione precedente](creating-user-accounts-vb.md). Il controllo di accesso [ `HelpPageText` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.login.helppagetext.aspx) e [ `HelpPageUrl` proprietà](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.login.helppageurl.aspx) e [ `PasswordRecoveryText` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.login.passwordrecoverytext.aspx) e [ `PasswordRecoveryUrl` proprietà](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.login.passwordrecoveryurl.aspx) funzionano nello stesso modo, il rendering di collegamenti a una pagina della Guida in linea e una pagina di recupero della password.

Dopo aver apportato queste modifiche delle proprietà, markup dichiarativo e l'aspetto del controllo di accesso dovrebbe essere simile a quanto illustrato nella figura 5.


[![Valori delle proprietà del controllo di accesso determinano l'aspetto](validating-user-credentials-against-the-membership-user-store-vb/_static/image14.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image13.png)

**Figura 5**: i valori determinano aspetto delle proprietà del controllo di accesso relativo ([fare clic per visualizzare l'immagine ingrandita](validating-user-credentials-against-the-membership-user-store-vb/_static/image15.png))


### <a name="configuring-the-login-controls-layout"></a>Configurazione di Layout del controllo di accesso

L'account di accesso Web predefinito interfaccia utente di controllo è disposto dall'interfaccia in un elemento HTML `<table>`. Ma cosa accade se è necessario un maggiore controllo sull'output del rendering? Forse si desidera sostituire il `<table>` con una serie di `<div>` tag. O se l'applicazione richiede ulteriori credenziali per l'autenticazione? Molti siti Web finanziari, ad esempio, richiedere agli utenti di specificare non solo un nome utente e password, ma anche un numero di identificazione personale (PIN) o altre informazioni di identificazione. L'elemento potrebbero essere i motivi, è possibile convertire il controllo di accesso in un modello, da cui è possibile definire in modo esplicito il markup dichiarativo dell'interfaccia.

È necessario eseguire due operazioni per aggiornare il controllo di accesso per raccogliere ulteriori credenziali:

1. Aggiornare l'interfaccia del controllo di accesso per includere i controlli Web per raccogliere le credenziali aggiuntive.
2. Eseguire l'override della logica di autenticazione interna del controllo di accesso in modo che un utente è autenticato solo se il nome utente e la password è valido e le proprie credenziali aggiuntive sono valide, troppo.

Per eseguire la prima attività, è necessario convertire il controllo di accesso in un modello e aggiungere i controlli Web necessari. Come per la seconda attività, la logica di autenticazione del controllo di accesso può essere sostituita da creare un gestore eventi per il controllo [ `Authenticate` evento](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.login.authenticate.aspx).

Si aggiorna il controllo di accesso in modo che richiede agli utenti il nome utente, password e l'indirizzo di posta elettronica solo autentica l'utente se l'indirizzo di posta elettronica fornito corrisponde l'indirizzo di posta elettronica nel file. È innanzitutto necessario convertire l'interfaccia del controllo di accesso a un modello. Smart Tag del controllo di accesso, scegliere la funzione Convert per l'opzione del modello.


[![Converte il controllo di accesso a un modello](validating-user-credentials-against-the-membership-user-store-vb/_static/image17.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image16.png)

**Figura 6**: convertire il controllo di accesso a un modello ([fare clic per visualizzare l'immagine ingrandita](validating-user-credentials-against-the-membership-user-store-vb/_static/image18.png))


> [!NOTE]
> Per ripristinare il controllo di accesso con la versione pre-templata, fare clic sul collegamento di reimpostazione da Smart Tag del controllo.


Conversione del controllo di accesso a un modello aggiunge un `LayoutTemplate` al markup dichiarativo del controllo con gli elementi HTML e controlli Web di definizione dell'interfaccia utente. Come illustrato nella figura 7, conversione il controllo in un modello consente di rimuovere un numero di proprietà dalla finestra delle proprietà, ad esempio `TitleText`, `CreateUserUrl`e così via, in quanto questi valori vengono ignorati quando si utilizza un modello.


[![Proprietà di un numero inferiore sono che disponibili quando il controllo di accesso viene convertito in un modello](validating-user-credentials-against-the-membership-user-store-vb/_static/image20.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image19.png)

**Figura 7**: meno proprietà sono disponibili quando il controllo di accesso viene convertito in un modello ([fare clic per visualizzare l'immagine ingrandita](validating-user-credentials-against-the-membership-user-store-vb/_static/image21.png))


Il markup HTML nel `LayoutTemplate` può essere modificato in base alle esigenze. Analogamente, è possibile aggiungere nuovi controlli Web per il modello. Tuttavia, è importante tale account di accesso core Web controlli rimangono nel modello e mantenere assegnato loro `ID` valori. In particolare, non rimuovere o rinominare il `UserName` o `Password` nelle caselle di testo, il `RememberMe` casella di controllo, il `LoginButton` pulsante, il `FailureText` etichetta, o `RequiredFieldValidator` controlli.

Per raccogliere l'indirizzo di posta elettronica del visitatore, è necessario aggiungere una casella di testo per il modello. Aggiungere il seguente markup dichiarativo tra la riga della tabella (`<tr>`) che contiene il `Password` casella di testo e la riga della tabella che contiene memorizza dati accanto tempo casella di controllo:

[!code-aspx[Main](validating-user-credentials-against-the-membership-user-store-vb/samples/sample2.aspx)]

Dopo aver aggiunto il `Email` casella di testo, visitare la pagina tramite un browser. Come illustrato nella figura 8, interfaccia utente del controllo di accesso include ora una terza casella di testo.


[![Il controllo di accesso include ora una casella di testo per l'indirizzo di posta elettronica dell'utente](validating-user-credentials-against-the-membership-user-store-vb/_static/image23.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image22.png)

**Figura 8**: ora controllo di accesso include una casella di testo per l'indirizzo di posta elettronica dell'utente ([fare clic per visualizzare l'immagine ingrandita](validating-user-credentials-against-the-membership-user-store-vb/_static/image24.png))


A questo punto, il controllo di accesso utilizza ancora la `Membership.ValidateUser` metodo per convalidare le credenziali fornite. Di conseguenza, il valore immesso nella `Email` casella di testo non incide sul se l'utente può accedere. Nel passaggio 3 verranno esaminati come eseguire l'override di logica di autenticazione del controllo di accesso in modo che le credenziali vengono considerate valide solo se il nome utente e la password siano validi e l'indirizzo di posta elettronica specificato corrisponde all'indirizzo di posta elettronica nel file.

## <a name="step-3-modifying-the-login-controls-authentication-logic"></a>Passaggio 3: Modificare la logica di autenticazione del controllo di accesso

Quando un visitatore del proprio credenziali e fa clic che sul pulsante Accedi, un postback viene utilizzata e l'account di accesso controllo attraversa il flusso di lavoro di autenticazione. Avvia il flusso di lavoro mediante la generazione di [ `LoggingIn` evento](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.login.loggingin.aspx). I gestori di eventi associati a questo evento possono annullare il log in operazione impostando il `e.Cancel` proprietà `True`.

Se il log attivo non viene annullato, il flusso di lavoro che passa attraverso la visualizzazione di [ `Authenticate` evento](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.login.authenticate.aspx). Se è presente un gestore eventi per il `Authenticate` evento, è responsabile di determinare se le credenziali fornite sono valide o non. Se non viene specificato alcun gestore di evento, il controllo di accesso utilizza il `Membership.ValidateUser` metodo per determinare la validità delle credenziali.

Se le credenziali fornite sono valide, quindi viene creato il ticket di autenticazione form, il [ `LoggedIn` evento](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.login.loggedin.aspx) viene generato e l'utente viene reindirizzato alla pagina appropriata. Se, tuttavia, le credenziali sono considerate non validi, quindi il [ `LoginError` evento](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.login.loginerror.aspx) viene generato e viene visualizzato un messaggio che informa l'utente che le credenziali non sono valide. Per impostazione predefinita, in caso di errore dell'account di accesso controllo semplicemente imposta relativo `FailureText` etichetta proprietà Text del controllo a un messaggio di errore (tentativo di accesso non è riuscito. Riprova in seguito). Tuttavia, se il controllo di accesso [ `FailureAction` proprietà](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.login.failureaction.aspx) è impostato su `RedirectToLoginPage`, quindi i problemi di controllo di accesso un `Response.Redirect` alla pagina di accesso aggiungendo il parametro querystring `loginfailure=1` (che fa sì che l'account di accesso viene visualizzato il messaggio di errore).

Figura 9 offre un diagramma di flusso del flusso di lavoro autenticazione.


[![Flusso di lavoro di controllo di accesso autenticazione](validating-user-credentials-against-the-membership-user-store-vb/_static/image26.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image25.png)

**Figura 9**: flusso di lavoro del controllo di accesso di autenticazione ([fare clic per visualizzare l'immagine ingrandita](validating-user-credentials-against-the-membership-user-store-vb/_static/image27.png))


> [!NOTE]
> Sapere quando si utilizza il `FailureAction`del `RedirectToLogin` pagina opzione, si consideri lo scenario seguente. Ora il nostro `Site.master` pagina master è attualmente il testo, Hello, stranger visualizzato nella colonna sinistra quando visitato da un utente anonimo, ma si supponga di voler sostituire il testo con un controllo di accesso. In questo modo un utente anonimo accedere da qualsiasi pagina del sito, anziché dover direttamente, visitare la pagina di accesso. Tuttavia, se un utente non è in grado di accedere mediante il controllo di accesso di rendering della pagina master, può rivelarsi utile per reindirizza alla pagina di accesso (`Login.aspx`) poiché tale pagina è probabile che include istruzioni aggiuntive, i collegamenti e altre informazioni della Guida, ad esempio i collegamenti per creare un nuovo account o recuperare una password persa - che non sono stati aggiunti alla pagina master.


### <a name="creating-theauthenticateevent-handler"></a>Creazione di`Authenticate`gestore dell'evento

Per inserire la logica di autenticazione personalizzata, è necessario creare un gestore eventi per il controllo di accesso `Authenticate` evento. Creazione di un gestore eventi per il `Authenticate` evento genererà la definizione del gestore di evento seguente:

[!code-vb[Main](validating-user-credentials-against-the-membership-user-store-vb/samples/sample3.vb)]

Come si può notare, la `Authenticate` gestore eventi viene passato un oggetto di tipo [ `AuthenticateEventArgs` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.authenticateeventargs.aspx) come il secondo parametro di input. Il `AuthenticateEventArgs` classe contiene una proprietà booleana denominata `Authenticated` che viene utilizzata per specificare se le credenziali fornite sono valide. L'attività, è quindi scrivere qui il codice che determina se le credenziali fornite sono valide o non e per impostare il `e.Authenticate` proprietà conseguenza.

### <a name="determining-and-validating-the-supplied-credentials"></a>Determinare e convalida le credenziali fornite

Utilizzare il controllo di accesso [ `UserName` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.login.username.aspx) e [ `Password` proprietà](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.login.password.aspx) per determinare le credenziali di nome utente e password immesse dall'utente. Per determinare i valori immessi in tutti i controlli Web aggiuntivi (ad esempio il `Email` TextBox abbiamo aggiunto nel passaggio precedente), utilizzare `LoginControlID.FindControl`("*`controlID`*") per ottenere un riferimento a livello di codice per il Web controllo del modello cui `ID` proprietà è uguale a  *`controlID`* . Ad esempio, per ottenere un riferimento di `Email` casella di testo, utilizzare il codice seguente:

`Dim EmailTextBox As TextBox = CType(myLogin.FindControl("Email"), TextBox)`

Per convalidare le credenziali dell'utente è necessario eseguire due operazioni:

1. Verificare che il nome utente specificato e la password siano validi
2. Verificare che l'indirizzo di posta elettronica immesso corrisponda l'indirizzo di posta elettronica sul file per l'utente tenta l'accesso

Per eseguire il primo controllo, è possibile utilizzare il `Membership.ValidateUser` metodo come illustrato nel passaggio 1. Per il secondo controllo, è necessario determinare l'indirizzo di posta elettronica dell'utente in modo che è possibile confrontarlo con l'indirizzo di posta elettronica inseriti nel controllo TextBox. Per ottenere informazioni su un particolare utente, utilizzare il `Membership` della classe [ `GetUser` metodo](https://msdn.microsoft.com/en-us/library/system.web.security.membership.getuser.aspx).

Il `GetUser` metodo ha un numero di overload. Se utilizzato senza passare i parametri, restituisce informazioni sull'utente attualmente connesso. Per ottenere informazioni su un particolare utente, chiamare `GetUser` passando il nome utente. In entrambi i casi, `GetUser` restituisce un [ `MembershipUser` oggetto](https://msdn.microsoft.com/en-us/library/system.web.security.membershipuser.aspx), che dispone di proprietà come `UserName`, `Email`, `IsApproved`, `IsOnline`e così via.

Il codice seguente implementa questi due controlli. Se entrambi passa quindi `e.Authenticate` è impostato su `True`, in caso contrario viene assegnato `False`.

[!code-vb[Main](validating-user-credentials-against-the-membership-user-store-vb/samples/sample4.vb)]

Con questo codice, tentare di accedere come utente valido, immettere la correttezza del nome utente, password e l'indirizzo di posta elettronica. Tentare nuovamente, ma questa volta usare intenzionalmente un indirizzo di posta elettronica non corretto (vedere la figura 10). Infine, provare una terza volta utilizzando un nome utente inesistente. Nel primo caso è deve essere connesso al sito, ma negli ultimi due casi verrà visualizzato il messaggio di credenziali non valide del controllo di accesso.


[![Tito non potrà accedere quando si specifica un indirizzo di posta elettronica non corretto](validating-user-credentials-against-the-membership-user-store-vb/_static/image29.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image28.png)

**Figura 10**: Tito non Log In quando fornisce un indirizzo di posta elettronica non corretto ([fare clic per visualizzare l'immagine ingrandita](validating-user-credentials-against-the-membership-user-store-vb/_static/image30.png))


> [!NOTE]
> Come descritto nella sezione come l'appartenenza al Framework gestisce valido tentativi di accesso nel passaggio 1, quando il `Membership.ValidateUser` metodo viene chiamato e passato delle credenziali non valide, tiene traccia del tentativo di accesso non valido e per bloccare l'utente qualora superino una determinata soglia di tentativi non validi all'interno di un intervallo di tempo specificato. Poiché la logica di autenticazione personalizzato chiama il `ValidateUser` incrementa il contatore dei tentativi di accesso non validi (metodo), una password non corretta per un nome utente valido, ma questo contatore non viene incrementato nel caso in cui il nome utente e password validi, ma la indirizzo di posta elettronica non è corretto. Probabilità sono, questo comportamento è appropriato, perché è improbabile che un pirata informatico verrà conoscere il nome utente e password, ma è necessario utilizzare le tecniche di attacchi di forza bruta per determinare l'indirizzo di posta elettronica dell'utente.


## <a name="step-4-improving-the-login-controls-invalid-credentials-message"></a>Passaggio 4: Migliorare messaggio di credenziali non valide del controllo di accesso

Quando un utente tenta di accedere con credenziali non valide, il controllo di accesso viene visualizzato un messaggio che indica che il tentativo di accesso non è riuscito. In particolare, il controllo Visualizza il messaggio specificato dal relativo [ `FailureText` proprietà](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.login.failuretext.aspx), che presenta un valore predefinito di tentativo di accesso non è riuscita. Riprovare.

Tenere presente che vi sono molti i motivi per cui le credenziali dell'utente potrebbero essere non valide:

- Il nome utente non esiste
- Il nome utente esiste, ma la password non è valida
- Il nome utente e password sono validi, ma l'utente non è ancora approvato.
- Il nome utente e password sono validi, ma l'utente è bloccato out (probabilmente perché è stato superato il numero di tentativi di accesso non valido nell'intervallo di tempo specificato)

E potrebbero essere presenti altri motivi quando si utilizza la logica di autenticazione personalizzato. Ad esempio, con il codice è stata scritta nel passaggio 3, il nome utente e password siano valide, ma l'indirizzo di posta elettronica potrebbe non essere corretto.

Indipendentemente dal motivo per cui le credenziali sono valide, il controllo di accesso consente di visualizzare lo stesso messaggio di errore. La mancanza di commenti e suggerimenti può essere ambigui per un utente il cui account non è ancora stata approvata o che è stato bloccato. Con un minimo di lavoro, tuttavia, possibile abbiamo il controllo di accesso viene visualizzato un messaggio più appropriato.

Ogni volta che un utente tenta di accedere con credenziali non valide genera il controllo di accesso relativo `LoginError` evento. Vado avanti e creare un gestore eventi per questo evento e aggiungere il codice seguente:

[!code-vb[Main](validating-user-credentials-against-the-membership-user-store-vb/samples/sample5.vb)]

Il codice sopra riportato viene avviato, impostare il controllo di accesso `FailureText` proprietà sul valore predefinito (tentativo di accesso non è riuscito. Riprova in seguito). Viene quindi verificato se il nome utente specificato viene eseguito il mapping a un account utente esistente. Se in tal caso, consulta risultante `MembershipUser` dell'oggetto `IsLockedOut` e `IsApproved` le proprietà per determinare se l'account è stato bloccato o non è ancora stato approvato. In entrambi i casi il `FailureText` proprietà viene aggiornata in un valore corrispondente.

Per testare questo codice, intenzionalmente tentare di accedere come un utente esistente, ma utilizzare una password errata. Eseguire l'operazione di cinque volte in una riga all'interno di un intervallo di tempo di 10 minuti e l'account verrà bloccato. Come illustrato nella figura 11, accesso successivi tentativi verranno viene sempre esito negativo (anche con la password corretta), ma verranno visualizzati più descrittivo, l'account è stato bloccato a causa di troppi tentativi di accesso non valido. Contattare l'amministratore per il messaggio di sbloccare account.


[![Tito eseguiti troppi tentativi di accesso non validi ed è stato bloccato](validating-user-credentials-against-the-membership-user-store-vb/_static/image32.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image31.png)

**Figura 11**: Tito eseguita troppo numerosi tentativi non validi account di accesso ed è stato bloccato ([fare clic per visualizzare l'immagine ingrandita](validating-user-credentials-against-the-membership-user-store-vb/_static/image33.png))


## <a name="summary"></a>Riepilogo

Precedenti in questa esercitazione, la pagina di accesso convalidato le credenziali specificate con un elenco hardcoded di coppie di nome utente/password. In questa esercitazione è stata aggiornata la pagina per convalidare le credenziali per il framework di appartenenza. Nel passaggio 1 è stato esaminato utilizzando le `Membership.ValidateUser` metodo a livello di codice. Nel passaggio 2 pertanto abbiamo sostituito l'interfaccia utente creato manualmente e il codice con il controllo di accesso.

Il controllo di accesso viene eseguito il rendering di un'interfaccia utente di accesso standard e consente di convalidare automaticamente le credenziali dell'utente per il framework di appartenenza. Inoltre, in caso di credenziali valide, il controllo di accesso accede l'utente tramite l'autenticazione basata su form. In breve, è disponibile un'esperienza utente di accesso completamente funzionale semplicemente trascinando il controllo di accesso su una pagina, non molto dichiarativi markup o codice necessario. In particolare, il controllo di accesso è altamente personalizzabile, consentendo un ottimo livello di controllo sia visualizzabile logica dell'utente dell'interfaccia e l'autenticazione.

A questo punto visitatori del sito possono creare un nuovo account utente e log nel sito, ma non è stato ancora esaminare la limitazione dell'accesso alle pagine in base all'utente autenticato. Attualmente, qualsiasi utente autenticato o anonimo, è possibile visualizzare qualsiasi pagina sul sito. Oltre a controllare l'accesso alle pagine del sito su base utente dall'utente, potremmo avere alcune pagine la cui funzionalità dipende dall'utente. La prossima esercitazione verrà illustrato come limitare l'accesso e delle funzionalità nella pagina in base all'utente connesso.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Visualizzazione di messaggi personalizzati da bloccato e gli utenti di Non approvato](http://aspnet.4guysfromrolla.com/articles/050306-1.aspx)
- [Analisi di ASP.NET 2.0 appartenenza, ruoli e profilo](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Procedura: Creare una pagina di accesso ASP.NET](https://msdn.microsoft.com/en-us/library/ms178331.aspx)
- [Documentazione tecnica di controllo di accesso](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.login.aspx)
- [Utilizzo dei controlli di accesso](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/security/login.aspx)

### <a name="about-the-author"></a>Informazioni sull'autore

Scott Mitchell, autore di più libri e fondatore di 4GuysFromRolla, ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è  *[SAM insegna manualmente ASP.NET 2.0 nelle 24 ore](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott può essere raggiunto al [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) o tramite il suo blog all'indirizzo [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Lead revisori per questa esercitazione sono stati Teresa Murphy e Michael Olivero. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

>[!div class="step-by-step"]
[Precedente](creating-user-accounts-vb.md)
[Successivo](user-based-authorization-vb.md)
