---
uid: web-forms/overview/older-versions-security/membership/validating-user-credentials-against-the-membership-user-store-vb
title: Convalida le credenziali dell'utente di appartenenza utente Store (VB) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione verrà esaminato come convalidare le credenziali dell'utente rispetto all'archivio utente di appartenenza utilizzando sia a livello di codice indica che il controllo di accesso...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: 17772912-b47b-4557-9ce9-80f22df642f7
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-security/membership/validating-user-credentials-against-the-membership-user-store-vb
msc.type: authoredcontent
ms.openlocfilehash: ad9f76f6c7c64c29971aed10ba165b5f4807eb64
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37364831"
---
<a name="validating-user-credentials-against-the-membership-user-store-vb"></a>Convalida le credenziali dell'utente di appartenenza utente Store (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare il codice](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_06_VB.zip) o [Scarica il PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial06_LoggingIn_vb.pdf)

> In questa esercitazione verrà esaminato come convalidare le credenziali dell'utente rispetto all'archivio utente di appartenenza utilizzando sia a livello di codice indica che il controllo di accesso. Si esaminerà anche come personalizzare l'aspetto e il comportamento del controllo di accesso.


## <a name="introduction"></a>Introduzione

Nel <a id="Tutorial05"> </a> [esercitazione precedente](creating-user-accounts-vb.md) è stato illustrato come creare un nuovo account utente nel framework di appartenenza. Abbiamo valutato prima di tutto a livello di codice la creazione di account utente tramite il `Membership` della classe `CreateUser` (metodo) e quindi esaminato tramite il controllo CreateUserWizard Web. Tuttavia, la pagina di accesso attualmente convalida le credenziali specificate con un elenco hardcoded di coppie di nome utente e password. È necessario aggiornare per la logica della pagina di accesso in modo che la convalida delle credenziali nell'archivio utente del framework di appartenenza.

Molto, come con la creazione di account utente, è possibile convalidare le credenziali a livello di programmazione o in modo dichiarativo. L'API di appartenenza include un metodo di convalida a livello di codice le credenziali dell'utente nell'archivio utente. E ASP.NET viene fornito con il controllo di accesso Web, che esegue il rendering di un'interfaccia utente con caselle di testo per il nome utente e password e un pulsante per l'accesso.

In questa esercitazione verrà esaminato come convalidare le credenziali dell'utente rispetto all'archivio utente di appartenenza utilizzando sia a livello di codice indica che il controllo di accesso. Si esaminerà anche come personalizzare l'aspetto e il comportamento del controllo di accesso. Iniziamo!

## <a name="step-1-validating-credentials-against-the-membership-user-store"></a>Passaggio 1: Convalida delle credenziali con il Store utente di appartenenza

Per i siti web che usano l'autenticazione basata su form, un utente accede al sito Web che visitano una pagina di accesso e immettendo le proprie credenziali. Queste credenziali vengono quindi confrontate con l'archivio dell'utente. Se sono validi, quindi l'utente viene concesso un ticket di autenticazione form, ovvero un token di sicurezza che indica l'identità e dell'autenticità del visitatore.

Per convalidare un utente con il framework di appartenenza, usare il `Membership` della classe [ `ValidateUser` metodo](https://msdn.microsoft.com/library/system.web.security.membership.validateuser.aspx). Il `ValidateUser` metodo accetta due parametri di input - `username` e `password` - e restituisce un valore booleano che indica se le credenziali sono valide. Come con le `CreateUser` metodo si sono esaminate nell'esercitazione precedente, il `ValidateUser` metodo delega la convalida effettiva per il provider di appartenenze configurato.

Il `SqlMembershipProvider` convalida le credenziali fornite per ottenere la password dell'utente specificato tramite la `aspnet_Membership_GetPasswordWithFormat` stored procedure. Si tenga presente che il `SqlMembershipProvider` archivia le password degli utenti usando uno dei tre formati: clear, crittografati o stato eseguito l'hashing. Il `aspnet_Membership_GetPasswordWithFormat` stored procedure restituisce la password nel relativo formato non elaborato. Per le password con hash o non crittografato, il `SqlMembershipProvider` trasforma le `password` valore passato nel `ValidateUser` (metodo) nel relativo equivalente crittografati o stato eseguito l'hashing e quindi la confronta con ciò che è stato restituito dal database. Se la password archiviata nel database corrisponda alla password formattata immessa dall'utente, le credenziali sono valide.

È possibile aggiornare la pagina di accesso (~ /`Login.aspx`) in modo che convalida le credenziali specificate nell'archivio utente framework di appartenenza. Abbiamo creato questa pagina di accesso nel <a id="Tutorial02"> </a> [ *una panoramica dell'autenticazione basata su form* ](../introduction/an-overview-of-forms-authentication-vb.md) esercitazione, creare un'interfaccia con due caselle di testo per il nome utente e password, un Memorizza account casella di controllo e un pulsante di accesso (vedere la figura 1). Il codice di convalida delle credenziali immesse in un elenco hardcoded di coppie di nome utente e password (Scott/password Jisun/password e Sam/password). Nel <a id="Tutorial03"> </a> [ *configurazione dell'autenticazione form e argomenti avanzati* ](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md) esercitazione abbiamo aggiornato il codice della pagina di accesso per memorizzare informazioni aggiuntive nei moduli ticket di autenticazione `UserData` proprietà.


[![Interfaccia di pagina di accesso include due caselle di testo, un controllo CheckBoxList e un pulsante](validating-user-credentials-against-the-membership-user-store-vb/_static/image2.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image1.png)

**Figura 1**: interfaccia include due caselle di testo della pagina di accesso di, un controllo CheckBoxList e un pulsante ([fare clic per visualizzare l'immagine con dimensioni normali](validating-user-credentials-against-the-membership-user-store-vb/_static/image3.png))


Interfaccia utente della pagina di accesso può rimanere invariato, ma è necessario sostituire il pulsante di accesso `Click` gestore dell'evento con il codice che convalida l'utente rispetto all'archivio utente framework di appartenenza. Aggiornare il gestore dell'evento in modo che il relativo codice viene visualizzato come segue:

[!code-vb[Main](validating-user-credentials-against-the-membership-user-store-vb/samples/sample1.vb)]

Questo codice è straordinariamente semplice. Si inizia chiamando il `Membership.ValidateUser` , passando il nome utente fornito e la password. Se tale metodo restituisce True, allora l'utente è connesso al sito tramite il `FormsAuthentication` metodo RedirectFromLoginPage della classe. (Come illustrato nella <a id="Tutorial02"> </a> [ *una panoramica dell'autenticazione basata su form* ](../introduction/an-overview-of-forms-authentication-vb.md) esercitazione il `FormsAuthentication.RedirectFromLoginPage` crea moduli di ticket di autenticazione e quindi reindirizza l'utente alla pagina appropriata.) Se le credenziali sono valide, tuttavia, il `InvalidCredentialsMessage` etichetta viene visualizzata, per informare l'utente che il nome utente o la password non è corretta.

Questo è tutto qui!

Per verificare che la pagina di accesso funziona come previsto, provare ad accedere con uno degli account utente che è stato creato nell'esercitazione precedente. In alternativa, se non è stato ancora creato un account, andare avanti e crearne una dal `~/Membership/CreatingUserAccounts.aspx` pagina.

> [!NOTE]
> Quando l'utente immette le proprie credenziali e invia il modulo della pagina account di accesso, le credenziali, inclusa la password vengono trasmesse tramite Internet al server web nel *testo normale*. Ciò significa che qualsiasi utente malintenzionato sniffing del traffico di rete è possibile visualizzare il nome utente e password. Per evitare questo problema, è essenziale per crittografare il traffico di rete mediante [livelli SSL (Secure Socket)](http://en.wikipedia.org/wiki/Secure_Sockets_Layer). Ciò garantisce che le credenziali (così come markup HTML della pagina intera) vengono crittografati dal momento in cui che gli utenti lasciano il browser fino a quando non vengono ricevuti dal server web.


### <a name="how-the-membership-framework-handles-invalid-login-attempts"></a>Come il Framework di appartenenza gestisce i tentativi di accesso non valido

Quando un visitatore raggiunge la pagina di accesso e invia le proprie credenziali, il browser invia una richiesta HTTP alla pagina di accesso. Se le credenziali sono valide, la risposta HTTP include il ticket di autenticazione in un cookie. Pertanto, un hacker tenta di inserire un'interruzione nel sito è stato possibile creare un programma che modo esauriente invia richieste HTTP alla pagina di accesso con un nome utente valido e una stima alla password. Se l'ipotesi di password non è corretta, la pagina di accesso restituirà il cookie di ticket di autenticazione, a quel punto il programma riconosce che dispone di codifiche su una coppia nome utente/password valida. Tramite attacchi di forza bruta, soprattutto se la password è debole potrebbe essere in grado di progressivo password di un utente, tale programma.

Per evitare tali attacchi di forza bruta, il framework di appartenenza Blocca utente se sono presenti un certo numero di tentativi di accesso non riuscito entro un determinato periodo di tempo. I parametri esatti possono essere configurati tramite le impostazioni della configurazione dei provider di appartenenza due seguenti:

- `maxInvalidPasswordAttempts` -Specifica quante password non valida per i tentativi sono consentiti per l'utente entro il periodo di tempo prima che l'account è bloccato. Il valore predefinito è 5.
- `passwordAttemptWindow` -indica il periodo di tempo in minuti durante i quali il numero specificato di tentativi di accesso non validi causerà l'account vengano bloccati. Il valore predefinito è 10.

Se un utente è stato bloccato, Anna non è possibile eseguire l'accesso fino a quando non viene sbloccato il proprio account dall'amministratore. Quando un utente è bloccato, il `ValidateUser` metodo verrà *sempre* restituire `False`, anche se vengono fornite credenziali valide. Sebbene questo comportamento riduce la probabilità che un pirata informatico riesca a entrare nel sito tramite i metodi di attacco di forza bruta, possono finire bloccare un utente valido che semplicemente ha dimenticato la password o accidentalmente dispone il BLOC o con un maggior numero di eccezioni tipizzazione.

Sfortunatamente, non è Nessuno strumento incorporato per sbloccare un account utente. Per sbloccare un account, è possibile modificare il database direttamente - modificare la `IsLockedOut` campo il `aspnet_Membership` tabella per l'account - utente appropriato o creare un'interfaccia web che elenca bloccata dell'account con le opzioni per sbloccarli. Verranno esaminate le interfacce amministrative di creazione per portare a termine le attività correlate al ruolo e account utente comuni in un'esercitazione futura.

> [!NOTE]
> Uno svantaggio del `ValidateUser` metodo è che quando le credenziali specificate non sono validi, non fornisce alcun spiegazione per questo motivo. Le credenziali potrebbero non essere valide poiché non esiste alcuna coppia di nome utente/password corrispondenti nell'archivio dell'utente, o perché l'utente non è ancora stata approvata, oppure perché l'utente è stato bloccato. Nel passaggio 4 si vedrà come visualizzare un messaggio più dettagliato per l'utente quando il tentativo di accesso ha esito negativo.


## <a name="step-2-collecting-credentials-through-the-login-web-control"></a>Passaggio 2: Raccogliere le credenziali tramite il controllo di accesso Web

Il [controllo di accesso Web](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx) esegue il rendering di molto simile a quello creati in un'interfaccia utente predefinita di <a id="Tutorial02"> </a> [ *una panoramica dell'autenticazione basata su form* ](../introduction/an-overview-of-forms-authentication-vb.md) esercitazione. Utilizzo del controllo di accesso ci consente di risparmiare il lavoro di dover creare l'interfaccia per raccogliere le credenziali del visitatore. Inoltre, il controllo di accesso accede automaticamente all'utente (presupponendo che le credenziali inviate non sono valide), in tal modo il salvataggio di Stati Uniti di dover scrivere alcun codice.

Aggiorniamo `Login.aspx`, sostituendo l'interfaccia creato manualmente e scrivere il codice con un controllo di accesso. Iniziare rimuovendo il markup esistente e scrivere il codice `Login.aspx`. Si può eliminarlo direttamente o semplicemente impostarlo come commento. Per impostare come commento markup dichiarativo, racchiuderlo tra il `<%--` e `--%>` delimitatori. È possibile immettere manualmente questi delimitatori o, come illustrato nella figura 2, è possibile selezionare il testo da impostare come commento e quindi il commento l'icona di righe selezionate nella barra degli strumenti. Analogamente, è possibile utilizzare il commento l'icona di righe selezionate come commento il codice selezionato nella classe code-behind.


[![Commento di Markup dichiarativo esistente e il codice sorgente in Login. aspx](validating-user-credentials-against-the-membership-user-store-vb/_static/image5.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image4.png)

**Figura 2**: commento Out the esistente Markup dichiarativo e codice sorgente in Login. aspx ([fare clic per visualizzare l'immagine con dimensioni normali](validating-user-credentials-against-the-membership-user-store-vb/_static/image6.png))


> [!NOTE]
> Il commento l'icona di righe selezionate non è disponibile quando si Visualizza markup dichiarativo in Visual Studio 2005. Se non si usa Visual Studio 2008 è necessario aggiungere manualmente il `<%--` e `--%>` delimitatori.


Successivamente, trascinare un controllo di accesso dalla casella degli strumenti sulla pagina e impostare relativi `ID` proprietà `myLogin`. A questo punto la schermata dovrebbe essere simile alla figura 3. Si noti che l'interfaccia predefinita del controllo di accesso include i controlli casella di testo per il nome utente e password, un memorizza dati per la volta successiva che la casella di controllo e un pulsante nel registro. Sono inoltre disponibili `RequiredFieldValidator` controlli per le due caselle di testo.


[![Aggiungere un controllo di accesso alla pagina](validating-user-credentials-against-the-membership-user-store-vb/_static/image8.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image7.png)

**Figura 3**: aggiungere un controllo di accesso alla pagina ([fare clic per visualizzare l'immagine con dimensioni normali](validating-user-credentials-against-the-membership-user-store-vb/_static/image9.png))


E abbiamo finito! Quando si fa clic sul pulsante Accedi del controllo di accesso, si verificherà un postback e il controllo Login chiamerà il `Membership.ValidateUser` , passando il nome utente immesso e una password. Se le credenziali sono valide, il controllo di accesso viene visualizzato un apposito messaggio. Se, tuttavia, le credenziali sono valide, il controllo Login crea moduli di ticket di autenticazione e l'utente viene reindirizzato alla pagina appropriata.

Per determinare la pagina appropriata per reindirizzare l'utente al momento di un account di accesso ha esito positivo, il controllo di accesso Usa quattro fattori:

- Indica se il controllo di accesso è nella pagina di accesso definito da `loginUrl` impostazione nella configurazione dell'autenticazione form; valore predefinito di questa impostazione è `Login.aspx`
- La presenza di un `ReturnUrl` parametro querystring
- Il valore di controllo di accesso [ `DestinationUrl` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.destinationpageurl.aspx)
- Il `defaultUrl` valore specificato nei moduli delle impostazioni di configurazione di autenticazione, valore predefinito di questa impostazione è default. aspx

Figura 4 illustra come il controllo di accesso utilizza questi quattro parametri per poi arrivare alla propria decisione pagina appropriata.


[![Aggiungere un controllo di accesso alla pagina](validating-user-credentials-against-the-membership-user-store-vb/_static/image11.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image10.png)

**Figura 4**: aggiungere un controllo di accesso alla pagina ([fare clic per visualizzare l'immagine con dimensioni normali](validating-user-credentials-against-the-membership-user-store-vb/_static/image12.png))


Si consiglia di testare il controllo di accesso da visitare il sito tramite un browser e accedere come un utente esistente nel framework di appartenenza.

Interfaccia viene eseguito il rendering del controllo di accesso è ampiamente configurabile. Esistono una serie di proprietà che determinano l'aspetto; Inoltre, il controllo di accesso può essere convertito in un modello per un controllo preciso il layout di elementi dell'interfaccia utente. Nella parte restante di questo passaggio viene illustrato come personalizzare l'aspetto e layout.

### <a name="customizing-the-login-controls-appearance"></a>Personalizzazione dell'aspetto del controllo di accesso

Impostazioni predefinite delle proprietà del controllo di accesso il rendering di un'interfaccia utente con un titolo (Accedi), i controlli casella di testo e l'etichetta per gli input di nome utente e password, un memorizza dati la volta successiva in casella di controllo e un pulsante Accedi. Gli aspetti di questi elementi sono tutte configurabili mediante numerose proprietà del controllo di accesso. Inoltre, è possibile aggiungere elementi dell'interfaccia utente aggiuntive, ad esempio un collegamento a una pagina per creare un nuovo account utente - impostando una proprietà o due.

È possibile dedicare alcuni minuti per abbellire l'aspetto del nostro controllo di accesso. Poiché il `Login.aspx` pagina già presente del testo nella parte superiore della pagina con la dicitura account di accesso, il titolo del controllo di accesso è superfluo. Pertanto, cancelliamo i [ `TitleText` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.titletext.aspx) valore allo scopo di rimuovere il titolo del controllo di accesso.

Il nome utente: e la Password: etichette a sinistra dei due controlli casella di testo possono essere personalizzate tramite il [ `UserNameLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.usernamelabeltext.aspx) e [ `PasswordLabelText` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordlabeltext.aspx), rispettivamente. È possibile modificare il nome utente: etichetta per la lettura di nome utente:. Gli stili di etichetta e casella di testo possono essere configurati tramite il [ `LabelStyle` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.labelstyle.aspx) e [ `TextBoxStyle` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.textboxstyle.aspx), rispettivamente.

La memorizza dati per la proprietà Text del successiva ora casella di controllo può essere impostato tramite il controllo di accesso [ `RememberMeText` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.remembermetext.aspx), e sul valore predefinito è archiviato lo stato è configurabile tramite il [ `RememberMeSet` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.remembermeset.aspx)(il valore predefinito è False). Proseguire e impostare il `RememberMeSet` proprietà su True in modo che memorizza dati la volta successiva in casella di controllo verrà controllato dall'impostazione predefinita.

Il controllo di accesso offre due proprietà per regolare il layout dei relativi controlli dell'interfaccia utente. Il [ `TextLayout` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.textlayout.aspx) indica se il nome utente: e la Password: le etichette vengono visualizzate a sinistra della loro corrispondente nelle caselle di testo (impostazione predefinita) o sovrastanti. Il [ `Orientation` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.orientation.aspx) indica se gli input di nome utente e password sono situati in verticale, una sopra l'altro, orizzontalmente o verticalmente. Voglio lasciare queste due proprietà impostate sui valori predefiniti, ma invitiamo a provare a impostare queste due proprietà con i relativi valori non predefiniti per visualizzare l'effetto risulta.

> [!NOTE]
> Nella sezione successiva, la configurazione di Layout del controllo di accesso, si esaminerà usando i modelli per definire il layout preciso degli elementi dell'interfaccia utente del controllo di Layout.


Riepilogo impostazioni delle proprietà del controllo di accesso, impostando il [ `CreateUserText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.createusertext.aspx) e [ `CreateUserUrl` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.createuserurl.aspx) a non ancora registrato? Crea un account. e `~/Membership/CreatingUserAccounts.aspx`, rispettivamente. Verrà aggiunto un collegamento ipertestuale all'interfaccia del controllo di accesso che punta alla pagina creata nel <a id="Tutorial05"> </a> [esercitazione precedente](creating-user-accounts-vb.md). Il controllo di accesso [ `HelpPageText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.helppagetext.aspx) e [ `HelpPageUrl` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.helppageurl.aspx) e [ `PasswordRecoveryText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordrecoverytext.aspx) e [ `PasswordRecoveryUrl` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordrecoveryurl.aspx) funzionano allo stesso modo, il rendering di collegamenti a una pagina della Guida in linea e una pagina di recupero della password.

Dopo aver apportato queste modifiche delle proprietà, l'account di accesso markup dichiarativo e l'aspetto dovrebbe essere simile a quella mostrata nella figura 5.


[![I valori delle proprietà del controllo di accesso determinano l'aspetto del controllo](validating-user-credentials-against-the-membership-user-store-vb/_static/image14.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image13.png)

**Figura 5**: valori dettare un aspetto delle proprietà del controllo di accesso di relativo ([fare clic per visualizzare l'immagine con dimensioni normali](validating-user-credentials-against-the-membership-user-store-vb/_static/image15.png))


### <a name="configuring-the-login-controls-layout"></a>Configurazione di Layout del controllo di accesso

Interfaccia utente predefinita del controllo Web di accesso viene disposto l'interfaccia in un elemento HTML `<table>`. Ma cosa accade se è necessario un controllo più preciso sull'output sottoposto a rendering? Forse si desidera sostituire il `<table>` con una serie di `<div>` tag. O se l'applicazione richiede ulteriori credenziali per l'autenticazione? Molti siti Web finanziari, ad esempio, richiedere agli utenti di specificare non solo un nome utente e password, ma anche un numero di identificazione personale (PIN) o altre informazioni di identificazione. Qualunque sia i motivi possono essere, è possibile convertire il controllo di accesso a un modello, da cui è possibile definire in modo esplicito markup dichiarativo dell'interfaccia.

È necessario eseguire due operazioni per aggiornare il controllo di accesso per raccogliere ulteriori credenziali:

1. Aggiornare l'interfaccia del controllo di accesso per includere i controlli Web per raccogliere le credenziali aggiuntive.
2. Eseguire l'override della logica di autenticazione interna del controllo di accesso in modo che un utente viene autenticato solo se il nome utente e la password è valido e le proprie credenziali aggiuntive sono valide, troppo.

Per portare a termine la prima attività, è necessario convertire il controllo di accesso in un modello e aggiungere i controlli Web necessari. Come per la seconda attività, la logica di autenticazione del controllo di accesso può essere sostituita mediante la creazione di un gestore eventi per il controllo [ `Authenticate` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.authenticate.aspx).

Aggiorniamo il controllo di accesso in modo che richiede agli utenti il nome utente, password e l'indirizzo di posta elettronica e l'utente viene autenticato solo se l'indirizzo di posta elettronica fornito corrisponde al proprio indirizzo di posta elettronica sul file. È innanzitutto necessario convertire interfaccia del controllo account di accesso a un modello. Dallo Smart Tag del controllo di accesso, scegliere la funzione Convert per l'opzione del modello.


[![Convertire il controllo di accesso a un modello](validating-user-credentials-against-the-membership-user-store-vb/_static/image17.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image16.png)

**Figura 6**: converte il controllo di accesso a un modello ([fare clic per visualizzare l'immagine con dimensioni normali](validating-user-credentials-against-the-membership-user-store-vb/_static/image18.png))


> [!NOTE]
> Per ripristinare il controllo di accesso alla versione pre-templata, fare clic sul collegamento di reimpostazione dallo Smart Tag del controllo.


Conversione di controllo di accesso a un modello aggiunge un `LayoutTemplate` al markup dichiarativo del controllo con gli elementi HTML e controlli Web che definisce l'interfaccia utente. Come illustrato nella figura 7, convertendo il controllo a un modello rimuove un numero di proprietà dalla finestra delle proprietà, ad esempio `TitleText`, `CreateUserUrl`e così via, poiché i valori delle proprietà vengono ignorati quando si usa un modello.


[![Meno proprietà sono che disponibili quando il controllo di accesso viene convertito in un modello](validating-user-credentials-against-the-membership-user-store-vb/_static/image20.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image19.png)

**Figura 7**: meno proprietà sono disponibili quando il controllo di accesso viene convertito in un modello ([fare clic per visualizzare l'immagine con dimensioni normali](validating-user-credentials-against-the-membership-user-store-vb/_static/image21.png))


Il markup HTML nel `LayoutTemplate` può essere modificato in base alle esigenze. Analogamente, è possibile aggiungere nuovi controlli Web per il modello. Tuttavia, è importante tale account di accesso principali Web controlli rimangono nel modello e mantenere assegnato loro `ID` valori. In particolare, non rimuovere o rinominare il `UserName` o `Password` caselle di testo, il `RememberMe` casella di controllo, il `LoginButton` pulsante, il `FailureText` etichetta, o il `RequiredFieldValidator` controlli.

Per raccogliere l'indirizzo di posta elettronica del visitatore, è necessario aggiungere una casella di testo per il modello. Aggiungere il seguente markup dichiarativo tra la riga della tabella (`<tr>`) che contiene il `Password` nella casella di testo e la riga della tabella che contiene la memorizza dati successivamente tempo casella di controllo:

[!code-aspx[Main](validating-user-credentials-against-the-membership-user-store-vb/samples/sample2.aspx)]

Dopo aver aggiunto il `Email` nella casella di testo, visitare la pagina tramite un browser. Come illustrato nella figura 8, interfaccia utente del controllo di accesso include ora una terza casella di testo.


[![Il controllo di accesso ora include una casella di testo per l'indirizzo di posta elettronica dell'utente](validating-user-credentials-against-the-membership-user-store-vb/_static/image23.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image22.png)

**Figura 8**: ora controllo di accesso include una casella di testo per l'indirizzo di posta elettronica dell'utente ([fare clic per visualizzare l'immagine con dimensioni normali](validating-user-credentials-against-the-membership-user-store-vb/_static/image24.png))


A questo punto, il controllo di accesso continua a usare il `Membership.ValidateUser` metodo per convalidare le credenziali fornite. Di conseguenza, il valore immesso nel `Email` nella casella di testo non incide sul fatto che l'utente può accedere. Nel passaggio 3 si esaminerà come eseguire l'override della logica di autenticazione del controllo di accesso in modo che le credenziali vengono considerate valide solo se il nome utente e la password siano validi e l'indirizzo di posta elettronica specificato corrisponde all'indirizzo di posta elettronica sul file.

## <a name="step-3-modifying-the-login-controls-authentication-logic"></a>Passaggio 3: Modifica la logica di autenticazione del controllo di accesso

Quando un visitatore fornisce relativa credenziali e fa clic che sul pulsante Accedi, un postback previsioni e l'account di accesso controllo attraversa il flusso di lavoro di autenticazione. Il flusso di lavoro viene avviato tramite la generazione di [ `LoggingIn` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loggingin.aspx). I gestori eventi associati all'evento potrebbero essere annullati i log nell'operazione impostando il `e.Cancel` proprietà `True`.

Se il log nell'operazione non viene annullato, il flusso di lavoro avanza generando il [ `Authenticate` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.authenticate.aspx). Se è presente un gestore eventi per il `Authenticate` eventi, è responsabile di determinare se le credenziali specificate sono valide o meno. Se non viene specificato alcun gestore eventi, il controllo di accesso utilizza il `Membership.ValidateUser` metodo per determinare la validità delle credenziali.

Se le credenziali specificate sono valide, quindi viene creato il ticket di autenticazione form, il [ `LoggedIn` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loggedin.aspx) viene generato, e l'utente viene reindirizzato alla pagina appropriata. Se, tuttavia, le credenziali sono considerate non validi, il [ `LoginError` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loginerror.aspx) viene generato e viene visualizzato un messaggio che informa l'utente che le credenziali non sono validi. Per impostazione predefinita, in caso di errore l'account di accesso controllo imposta semplicemente la `FailureText` etichetta proprietà Text del controllo a un messaggio di errore (tentativo di accesso non riuscito. Riprovare). Tuttavia, se il controllo di accesso [ `FailureAction` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.failureaction.aspx) è impostata su `RedirectToLoginPage`, quindi i problemi di controllo di accesso un `Response.Redirect` alla pagina di accesso aggiungendo il parametro querystring `loginfailure=1` (in modo che l'account di accesso controllo per visualizzare il messaggio di errore).

Figura 9 offre un diagramma di flusso del flusso di lavoro autenticazione.


[![Flusso di lavoro di autenticazione del controllo di accesso](validating-user-credentials-against-the-membership-user-store-vb/_static/image26.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image25.png)

**Figura 9**: flusso di lavoro autenticazione sul controllo account di accesso ([fare clic per visualizzare l'immagine con dimensioni normali](validating-user-credentials-against-the-membership-user-store-vb/_static/image27.png))


> [!NOTE]
> Sapere quando si utilizza il `FailureAction`del `RedirectToLogin` pagina opzione, prendere in considerazione lo scenario seguente. Attualmente il `Site.master` pagina master ha attualmente il testo Hello, stranger visualizzato nella colonna sinistra quando visitati da un utente anonimo, ma si supponga di voler sostituire tale testo con un controllo di accesso. In questo modo un utente anonimo per l'accesso da qualsiasi pagina nel sito, anziché richiedere di visitare la pagina di accesso direttamente. Tuttavia, se un utente non è riuscito per l'accesso tramite il controllo di accesso viene eseguito il rendering dalla pagina master, si potrebbe avere senso per reindirizzarle alla pagina di accesso (`Login.aspx`) poiché tale pagina probabilmente include istruzioni aggiuntive, collegamenti e altri argomenti della Guida, ad esempio i collegamenti per creare un nuovo account o recuperare una password persa - che non sono stati aggiunti alla pagina master.


### <a name="creating-theauthenticateevent-handler"></a>Creazione di`Authenticate`gestore dell'evento

Per inserire la logica di autenticazione personalizzata, è necessario creare un gestore eventi per il controllo di accesso `Authenticate` evento. Creazione di un gestore eventi per il `Authenticate` eventi genera la definizione di gestore di evento seguente:

[!code-vb[Main](validating-user-credentials-against-the-membership-user-store-vb/samples/sample3.vb)]

Come può notare, il `Authenticate` gestore dell'evento viene passato un oggetto di tipo [ `AuthenticateEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.authenticateeventargs.aspx) come il secondo parametro di input. Il `AuthenticateEventArgs` classe contiene una proprietà booleana denominata `Authenticated` che consente di specificare se le credenziali specificate sono valide. L'attività, è quindi, per scrivere qui il codice che determina se le credenziali specificate sono valide o non e impostare il `e.Authenticate` proprietà conseguenza.

### <a name="determining-and-validating-the-supplied-credentials"></a>Determinazione e convalidare le credenziali fornite

Usare il controllo di accesso [ `UserName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.username.aspx) e [ `Password` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.password.aspx) per determinare le credenziali nome utente e password immesse dall'utente. Per determinare i valori immessi negli eventuali controlli Web (ad esempio la `Email` nella casella di testo è stato aggiunto nel passaggio precedente), usare `LoginControlID.FindControl`("*`controlID`*") per ottenere un riferimento a livello di codice per il Web controllo nel modello di cui `ID` proprietà è uguale al *`controlID`*. Ad esempio, per ottenere un riferimento di `Email` nella casella di testo, usare il codice seguente:

`Dim EmailTextBox As TextBox = CType(myLogin.FindControl("Email"), TextBox)`

Per convalidare le credenziali dell'utente è necessario eseguire due operazioni:

1. Assicurarsi che il nome utente e la password siano validi
2. Assicurarsi che l'indirizzo di posta elettronica immesso corrisponda l'indirizzo di posta elettronica sul file per l'utente tenta di accedere

Per eseguire il primo controllo è possibile utilizzare il `Membership.ValidateUser` (metodo), come illustrato nel passaggio 1. Per il secondo controllo, è necessario determinare l'indirizzo di posta elettronica dell'utente in modo che è possibile confrontarlo con l'indirizzo di posta elettronica che vengono immessi nel controllo casella di testo. Per ottenere informazioni su un particolare utente, usare il `Membership` della classe [ `GetUser` metodo](https://msdn.microsoft.com/library/system.web.security.membership.getuser.aspx).

Il `GetUser` metodo ha un numero di overload. Se utilizzata senza passare alcun parametro, restituisce informazioni sull'utente attualmente connesso. Per ottenere informazioni su un particolare utente, chiamare `GetUser` passando il nome utente. In entrambi i casi `GetUser` restituisce un [ `MembershipUser` oggetto](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx), che dispone di proprietà, ad esempio `UserName`, `Email`, `IsApproved`, `IsOnline`e così via.

Il codice seguente implementa questi due controlli. Se entrambi passa, quindi `e.Authenticate` è impostata su `True`, in caso contrario, viene assegnato `False`.

[!code-vb[Main](validating-user-credentials-against-the-membership-user-store-vb/samples/sample4.vb)]

Con questo codice, tentare di accedere come un utente valido, immettere la correttezza del nome utente, password e l'indirizzo di posta elettronica. Provare di nuovo, ma questa volta intenzionalmente usare un indirizzo di posta elettronica non corretto (vedere la figura 10). Infine, è possibile provarlo una terza volta con un nome utente inesistente. Nel primo caso è necessario essere è riuscito ad accedere al sito, ma negli ultimi due casi verrà visualizzato il messaggio di credenziali non valide del controllo di accesso.


[![Non è possibile accedere tito quando si specifica un indirizzo di posta elettronica non corretto](validating-user-credentials-against-the-membership-user-store-vb/_static/image29.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image28.png)

**Figura 10**: Tito Impossibile Log In quando fornisce un indirizzo di posta elettronica non corretto ([fare clic per visualizzare l'immagine con dimensioni normali](validating-user-credentials-against-the-membership-user-store-vb/_static/image30.png))


> [!NOTE]
> Come descritto nella sezione come l'appartenenza al Framework gestisce non valido i tentativi di accesso nel passaggio 1, quando il `Membership.ValidateUser` metodo viene chiamato e passato credenziali non valide, tiene traccia del tentativo di accesso non è valido e blocca utente se superano una determinata soglia di tentativi non validi all'interno di un intervallo di tempo specificato. Poiché le chiamate di autenticazione personalizzato per la logica di `ValidateUser` metodo, una password errata per un nome utente valido incrementa il contatore dei tentativi di accesso non validi, ma questo contatore viene incrementato non nel caso in cui il nome utente e password sono validi, ma il indirizzo di posta elettronica non è corretto. Probabile che questo comportamento è appropriato, poiché è improbabile che un pirata informatico verrà conosce il nome utente e password, ma è necessario utilizzare le tecniche di attacco di forza bruta per determinare l'indirizzo di posta elettronica dell'utente.


## <a name="step-4-improving-the-login-controls-invalid-credentials-message"></a>Passaggio 4: Migliorare il messaggio di credenziali non valide del controllo di accesso

Quando un utente tenta di accedere con credenziali non valide, il controllo di accesso viene visualizzato un messaggio che indica che il tentativo di accesso non è riuscito. In particolare, il controllo viene visualizzato il messaggio specificato dal relativo [ `FailureText` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.failuretext.aspx), che ha il valore predefinito di tentativo di accesso non è riuscita. Riprovare.

È importante ricordare che esistono molti motivi per cui le credenziali dell'utente potrebbero essere non valida:

- Il nome utente non esiste
- Il nome utente esiste, ma la password non è valida
- Il nome utente e password sono validi, ma l'utente non è ancora approvato.
- Il nome utente e password sono validi, ma l'utente è stato bloccato out (probabilmente perché superano il numero di tentativi di accesso valido entro l'intervallo di tempo specificato)

E potrebbero esserci altri motivi quando si usa la logica di autenticazione personalizzato. Ad esempio, con il codice è stata scritta nel passaggio 3, il nome utente e password siano valide, ma l'indirizzo di posta elettronica potrebbe non essere corretto.

Indipendentemente dal motivo per cui le credenziali sono valide, il controllo Login consente di visualizzare lo stesso messaggio di errore. Questa mancanza di commenti e suggerimenti può generare confusione per un utente il cui account non è ancora stata approvata o che è stato bloccato. Con un po' di lavoro, tuttavia, possiamo il controllo di accesso visualizzato un messaggio più appropriato.

Ogni volta che un utente tenta di eseguire l'accesso con credenziali non valide, genera il controllo di accesso relativo `LoginError` evento. Proseguire e creare un gestore eventi per questo evento e aggiungere il codice seguente:

[!code-vb[Main](validating-user-credentials-against-the-membership-user-store-vb/samples/sample5.vb)]

Il codice sopra riportato viene avviato impostando il controllo di accesso `FailureText` proprietà sul valore predefinito (tentativo di accesso non riuscito. Riprovare). Viene quindi verificato se il nome utente specificato viene eseguito il mapping a un account utente esistente. Se pertanto consulta risultante `MembershipUser` dell'oggetto `IsLockedOut` e `IsApproved` proprietà per determinare se l'account è stato bloccato o non è ancora stata approvata. In entrambi i casi il `FailureText` proprietà viene aggiornata in un valore corrispondente.

Per testare questo codice, intenzionalmente tenta di accedere come un utente esistente, ma usare una password errata. Eseguire l'operazione cinque volte in una riga all'interno di un intervallo di tempo di 10 minuti e l'account verrà bloccato. Come illustrato nella figura 11, accesso successivi tentativi verranno viene sempre esito negativo (anche con la password corretta), ma verranno ora visualizzati più descrittivo all'account è stato bloccato a causa di troppi tentativi di accesso non è valido. Contattare l'amministratore per richiedere il messaggio di sbloccare account.


[![Tito eseguiti troppi tentativi di accesso non è valido ed è stato bloccato](validating-user-credentials-against-the-membership-user-store-vb/_static/image32.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image31.png)

**Figura 11**: Tito eseguita troppo numerosi tentativi non validi account di accesso e ha stati bloccati ([fare clic per visualizzare l'immagine con dimensioni normali](validating-user-credentials-against-the-membership-user-store-vb/_static/image33.png))


## <a name="summary"></a>Riepilogo

Precedenti in questa esercitazione, la pagina di accesso convalidato le credenziali specificate con un elenco hardcoded di coppie di nome utente/password. In questa esercitazione abbiamo aggiornato la pagina per convalidare le credenziali per il framework di appartenenza. Nel passaggio 1 viene esaminato utilizzando le `Membership.ValidateUser` metodo a livello di codice. Nel passaggio 2 è stata sostituita l'interfaccia utente creato manualmente e il codice con il controllo di accesso.

Il controllo di accesso viene eseguito il rendering di un'interfaccia utente di account di accesso standard e convalida automaticamente le credenziali dell'utente per il framework di appartenenza. Inoltre, in caso di credenziali valide, il controllo Login accede l'utente tramite autenticazione basata su form. In breve, è disponibile un'esperienza utente di accesso completamente funzionale semplicemente trascinando il controllo di accesso in una pagina, non molto dichiarativo markup o codice necessario. Inoltre, il controllo di accesso è altamente personalizzabile, consentendo un buon livello di controllo su entrambi l'utente sottoposto a rendering dell'interfaccia e l'autenticazione per la logica.

A questo punto visitatori del nostro sito Web può creare un nuovo account utente e un log nel sito, ma è ancora esaminare la limitazione dell'accesso alle pagine in base all'utente autenticato. Attualmente, qualsiasi utente autenticato o anonimo, è possibile visualizzare qualsiasi pagina sul nostro sito. Insieme a controllo dell'accesso alle pagine del sito in base a utente, potremmo avere determinate pagine di cui funzionalità dipende dall'utente. La prossima esercitazione verrà illustrato come limitare l'accesso e nella pagina funzionalità basate sull'utente connesso.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Visualizzazione di messaggi personalizzati a bloccato e gli utenti di Non approvato](http://aspnet.4guysfromrolla.com/articles/050306-1.aspx)
- [Analisi di ASP.NET 2.0 appartenenza, ruoli e profilo](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Procedura: Creare una pagina di accesso ASP.NET](https://msdn.microsoft.com/library/ms178331.aspx)
- [Documentazione tecnica di controllo di accesso](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx)
- [Usando i controlli di accesso](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/security/login.aspx)

### <a name="about-the-author"></a>Informazioni sull'autore

Scott Mitchell, autore di più libri e fondatore di 4GuysFromRolla.com, ha lavorato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola  *[Sams Teach Yourself ASP.NET 2.0 in 24 ore](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. È possibile contattarlo Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) o il suo blog all'indirizzo [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. I revisori per questa esercitazione sono state Teresa Murphy e Michael Olivero. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Precedente](creating-user-accounts-vb.md)
> [Successivo](user-based-authorization-vb.md)
