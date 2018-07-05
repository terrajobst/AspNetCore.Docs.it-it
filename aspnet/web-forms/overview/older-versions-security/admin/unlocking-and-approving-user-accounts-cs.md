---
uid: web-forms/overview/older-versions-security/admin/unlocking-and-approving-user-accounts-cs
title: Lo sblocco e approvazione degli account utente (c#) | Microsoft Docs
author: rick-anderson
description: Questa esercitazione illustra come creare una pagina web per gli amministratori per la gestione degli utenti bloccati e approvato gli stati. Si vedrà anche come approvare di nuovo o gli utenti...
ms.author: aspnetcontent
ms.date: 04/01/2008
ms.assetid: 5346aab1-9974-489f-a065-ae3883b8a350
msc.legacyurl: /web-forms/overview/older-versions-security/admin/unlocking-and-approving-user-accounts-cs
msc.type: authoredcontent
ms.openlocfilehash: ab6fa1b460de671559dc16ca85f2a4df1e3d5922
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818304"
---
<a name="unlocking-and-approving-user-accounts-c"></a>Lo sblocco e approvazione degli account utente (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare il codice](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/CS.14.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial14_UnlockAndApprove_cs.pdf)

> Questa esercitazione illustra come creare una pagina web per gli amministratori per la gestione degli utenti bloccati e approvato gli stati. Si vedrà anche come approvare i nuovi utenti solo dopo che si è verificato l'indirizzo di posta elettronica.


## <a name="introduction"></a>Introduzione

Insieme a un nome utente, password e messaggio di posta elettronica, ognuno dei quali include due campi di stato che determinano se l'utente può accedere al sito: bloccati e approvata. Un utente venga bloccato automaticamente se forniscono credenziali non valide un numero specificato di volte all'interno di un numero specificato di minuti (le impostazioni predefinite bloccano un utente dopo 5 tentativi di accesso non valido all'interno di 10 minuti). Lo stato approvato è utile negli scenari in cui un'azione deve trascorrere prima che un nuovo utente è in grado di accedere al sito. Ad esempio, un utente potrebbe dover verificare innanzitutto l'indirizzo di posta elettronica o essere approvata da un amministratore prima di essere in grado di account di accesso.

Poiché un blocco obsoleto o non approvate utente non è possibile eseguire l'accesso, si tratta solo naturale chiedersi come questi stati possono essere reimpostati. ASP.NET non include una funzionalità incorporata o controlli Web per la gestione degli utenti bloccati e approvato gli stati, in parte perché queste decisioni devono essere gestiti in modo da sito a sito. Alcuni siti potrebbero approvare automaticamente tutti i nuovi account utente (il comportamento predefinito). Altri utenti hanno un amministratore di approvare i nuovi account o non si approva gli utenti fino a quando non accedono a un collegamento inviato all'indirizzo di posta elettronica fornito quando si hanno effettuato l'iscrizione. Analogamente, alcuni siti potrebbero bloccare gli utenti fino a quando un amministratore reimposta il relativo stato, mentre altri siti di inviano un messaggio di posta elettronica all'utente bloccato con un URL possono accedere per sbloccare l'account.

Questa esercitazione illustra come creare una pagina web per gli amministratori per la gestione degli utenti bloccati e approvato gli stati. Si vedrà anche come approvare i nuovi utenti solo dopo che si è verificato l'indirizzo di posta elettronica.

## <a name="step-1-managing-users-locked-out-and-approved-statuses"></a>Passaggio 1: La gestione degli utenti bloccati e approvato gli Stati

Nel <a id="Tutorial12"> </a> [ *creazione di un'interfaccia per selezionare un Account utente da molti* ](building-an-interface-to-select-one-user-account-from-many-cs.md) esercitazione sono state create una pagina elencata ogni account utente in un paging, filtrati GridView. La griglia elenca ogni nome utente e indirizzo di posta elettronica, i relativi stati approvati e bloccati, che siano connessi attualmente online e i commenti relativi all'utente. Per la gestione degli utenti approvati e bloccati gli stati, possiamo rendere questa griglia modificabile. Per modificare lo stato di approvazione dell'utente, l'amministratore potrebbe innanzitutto individuare l'account utente e quindi modificare la riga corrispondente di GridView, selezionando o deselezionando la casella di controllo approvato. In alternativa, è possibile gestire gli stati approvati e bloccati tramite una pagina ASP.NET separata.

Per questa esercitazione si userà due pagine ASP.NET: `ManageUsers.aspx` e `UserInformation.aspx`. L'idea è che `ManageUsers.aspx` Elenca gli account utente nel sistema, mentre `UserInformation.aspx` consente all'amministratore di gestire gli stati approvati e bloccati per un utente specifico. Il primo è per aumentare il controllo GridView in `ManageUsers.aspx` per includere un HyperLinkField, che esegue il rendering come una colonna di collegamenti. Vogliamo ogni collegamento in modo che punti `UserInformation.aspx?user=UserName`, dove *UserName* è il nome dell'utente da modificare.

> [!NOTE]
> Se è stato scaricato il codice per il <a id="Tutorial13"> </a> [ *recupero e modifica delle password* ](recovering-and-changing-passwords-cs.md) esercitazione si è notato che la `ManageUsers.aspx` pagina contiene già un set di " Gestire"collegamenti e `UserInformation.aspx` pagina fornisce un'interfaccia per modificare la password dell'utente selezionato. Ho deciso di non replicare tale funzionalità nel codice associato a questa esercitazione perché ha funzionato per eludere l'API di appartenenza e l'uso direttamente con il database di SQL Server per modificare la password dell'utente. Questa esercitazione inizia da zero con la `UserInformation.aspx` pagina.


### <a name="adding-manage-links-to-theuseraccountsgridview"></a>Aggiunta di "Manage" collegamenti al`UserAccounts`GridView

Aprire il `ManageUsers.aspx` pagina e aggiungere un HyperLinkField al `UserAccounts` GridView. Impostare il HyperLinkField `Text` proprietà su "Gestisci" e la relativa `DataNavigateUrlFields` e `DataNavigateUrlFormatString` delle proprietà per `UserName` e "UserInformation.aspx?user={0}" rispettivamente. Queste impostazioni configurano il HyperLinkField in modo che tutti i collegamenti ipertestuali visualizzare il testo "Manage", ma ogni collegamento passa nelle rispettive caselle *UserName* valore nella stringa di query.

Dopo aver aggiunto il HyperLinkField GridView, si consiglia di visualizzare il `ManageUsers.aspx` pagina tramite un browser. Come illustrato nella figura 1, ogni riga GridView ora include un collegamento "Gestisci". Il collegamento "Manage" per Bruce rimanda `UserInformation.aspx?user=Bruce`, mentre il collegamento "Manage" per Dave punta a `UserInformation.aspx?user=Dave`.


[![Aggiunge il HyperLinkField un](unlocking-and-approving-user-accounts-cs/_static/image2.png)](unlocking-and-approving-user-accounts-cs/_static/image1.png)

**Figura 1**: il HyperLinkField aggiunge un collegamento "Manage" per ogni Account utente ([fare clic per visualizzare l'immagine con dimensioni normali](unlocking-and-approving-user-accounts-cs/_static/image3.png))


Verrà creato l'interfaccia utente e del codice per il `UserInformation.aspx` pagina in un momento, ma prima si discorso su come modificare a livello di codice un utente bloccato e approvato gli stati. Il [ `MembershipUser` classe](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx) ha [ `IsLockedOut` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.islockedout.aspx) e [ `IsApproved` proprietà](https://msdn.microsoft.com/library/system.web.security.membershipuser.isapproved.aspx). Il `IsLockedOut` proprietà è di sola lettura. Non è disponibile alcun meccanismo venga bloccato a livello di codice utente. Per sbloccare un utente, usare il `MembershipUser` della classe [ `UnlockUser` metodo](https://msdn.microsoft.com/library/system.web.security.membershipuser.unlockuser.aspx). Il `IsApproved` proprietà è leggibile e modificabile. Per salvare eventuali modifiche a questa proprietà, è necessario chiamare il `Membership` della classe [ `UpdateUser` metodo](https://msdn.microsoft.com/library/system.web.security.membership.updateuser.aspx)passando modificato `MembershipUser` oggetto.

Poiché il `IsApproved` proprietà è leggibile e scrivibile, una casella di controllo è probabilmente il migliore elemento dell'interfaccia utente per la configurazione di questa proprietà. Tuttavia, una casella di controllo non funziona per le `IsLockedOut` proprietà perché un amministratore non è possibile bloccare un utente, Anna può sbloccare solo un utente. Un'interfaccia utente adatta per il `IsLockedOut` proprietà è un pulsante che, quando si fa clic, sblocca l'account utente. Questo pulsante deve essere abilitato solo se l'utente è bloccato.

### <a name="creating-theuserinformationaspxpage"></a>Creazione di`UserInformation.aspx`pagina

A questo punto siamo pronti implementare l'interfaccia utente in `UserInformation.aspx`. Aprire questa pagina e aggiungere i controlli Web seguenti:

- Controllare un collegamento ipertestuale che, quando si fa clic, l'amministratore restituisce il `ManageUsers.aspx` pagina.
- Controllo etichetta Web per visualizzare il nome dell'utente selezionato. Impostare questa etichetta `ID` al `UserNameLabel` e cancellare il `Text` proprietà.
- Una casella di controllo denominato `IsApproved`. Impostare relativi `AutoPostBack` proprietà `true`.
- Data dell'ultimo blocco un controllo etichetta per la visualizzazione all'utente. Denominare questa etichetta `LastLockedOutDateLabel` e cancellare il `Text` proprietà.
- Pulsante per sbloccare l'utente. Denominare il pulsante `UnlockUserButton` e impostare il `Text` proprietà su "Sblocca utente".
- Un controllo etichetta per la visualizzazione dei messaggi di stato, ad esempio, "stato di approvazione dell'utente è stato aggiornato". Denominare questo controllo `StatusMessage`, deselezionare le relative `Text` proprietà e set relativo `CssClass` proprietà `Important`. (Il `Important` classe CSS viene definito nel `Styles.css` file foglio di stile; Visualizza il testo corrispondente in un tipo di carattere di grandi dimensioni, di colore rosso.)

Dopo l'aggiunta di questi controlli, la visualizzazione di progettazione in Visual Studio dovrebbe essere simile allo screenshot nella figura 2.


[![Creare l'interfaccia utente per UserInformation.aspx](unlocking-and-approving-user-accounts-cs/_static/image5.png)](unlocking-and-approving-user-accounts-cs/_static/image4.png)

**Figura 2**: l'interfaccia utente per creare `UserInformation.aspx` ([fare clic per visualizzare l'immagine con dimensioni normali](unlocking-and-approving-user-accounts-cs/_static/image6.png))


Con l'interfaccia utente completa, l'attività successiva consiste nell'impostare il `IsApproved` casella di controllo e altri controlli in base alle informazioni dell'utente selezionato. Creare un gestore eventi per la pagina `Load` eventi e aggiungere il codice seguente:

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample1.cs)]

Il codice sopra riportato viene avviato, garantendo che questa è la prima visita alla pagina e non un postback successivi. Quindi legge il nome utente passato tramite la `user` campo stringa di query e recupera le informazioni relative all'account utente tramite la `Membership.GetUser(username)` (metodo). Se tramite la stringa di query è stato fornito alcun nome utente, oppure se non è stato possibile trovare l'utente specificato, l'amministratore viene inviato il `ManageUsers.aspx` pagina.

Il `MembershipUser` dell'oggetto `UserName` valore viene quindi visualizzato nel `UserNameLabel` e il `IsApproved` casella di controllo viene verificata in base il `IsApproved` valore della proprietà.

Il `MembershipUser` dell'oggetto [ `LastLockoutDate` proprietà](https://msdn.microsoft.com/library/system.web.security.membershipuser.lastlockoutdate.aspx) restituisce un `DateTime` valore che indica quando l'utente è l'ultimo viene bloccato. Se l'utente non è stato bloccato, il valore restituito dipende dal provider di appartenenze. Quando viene creato un nuovo account, il `SqlMembershipProvider` imposta la `aspnet_Membership` della tabella `LastLockoutDate` campo `1754-01-01 12:00:00 AM`. Il codice sopra riportato consente di visualizzare una stringa vuota nel `LastLockoutDateLabel` se il `LastLockoutDate` proprietà si verifica prima dell'anno 2000; in caso contrario, la parte relativa alla data del `LastLockoutDate` proprietà sia visualizzata nell'etichetta. Il `UnlockUserButton'` s `Enabled` viene impostata per l'utente bloccato sullo stato, vale a dire che questo pulsante viene abilitato solo se l'utente è bloccato.

Si consiglia di testare il `UserInformation.aspx` pagina tramite un browser. È, naturalmente, necessario per l'avvio al `ManageUsers.aspx` e selezionare un account utente da gestire. Al momento in arrivo presso `UserInformation.aspx`, si noti che il `IsApproved` casella di controllo viene verificata solo se l'utente è approvato. Se l'utente è mai stato bloccato, viene visualizzato l'ultimo bloccata di Data. Pulsante Sblocca utente è attivato solo se l'utente è attualmente bloccato. Selezionare o deselezionare il `IsApproved` casella di controllo o facendo clic sul pulsante Sblocca utente provoca un postback, ma non vengono eseguite modifiche all'account utente perché dobbiamo ancora creare gestori eventi per questi eventi.

Tornare a Visual Studio e creare i gestori eventi per il `IsApproved` della casella di controllo `CheckedChanged` evento e il `UnlockUser` del pulsante `Click` evento. Nel `CheckedChanged` gestore dell'evento, impostare l'utente `IsApproved` proprietà per il `Checked` proprietà della casella di controllo, quindi salvare le modifiche tramite una chiamata a `Membership.UpdateUser`. Nel `Click` gestore dell'evento, semplicemente chiamare la `MembershipUser` dell'oggetto `UnlockUser` (metodo). In entrambi i gestori eventi, visualizzare un messaggio appropriato nella `StatusMessage` etichetta.

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample2.cs)]

### <a name="testing-theuserinformationaspxpage"></a>Test di`UserInformation.aspx`pagina

In questi gestori eventi posto, visitare di nuovo la pagina e non approvato un utente. Come illustrato nella figura 3, verrà visualizzato una breve messaggio nella pagina indicante che l'utente `IsApproved` proprietà è stata modificata correttamente.


[![Chris è stato Unapproved](unlocking-and-approving-user-accounts-cs/_static/image8.png)](unlocking-and-approving-user-accounts-cs/_static/image7.png)

**Figura 3**: Chris è stato Unapproved ([fare clic per visualizzare l'immagine con dimensioni normali](unlocking-and-approving-user-accounts-cs/_static/image9.png))


Successivamente, disconnessione e provare a eseguire l'accesso come utente con un account è stato semplicemente non approvati. Poiché l'utente non viene approvata, essi non è possibile eseguire l'accesso. Per impostazione predefinita, il controllo Login consente di visualizzare lo stesso messaggio se l'utente non è possibile eseguire l'accesso, indipendentemente dal motivo. Ma nel <a id="Tutorial6"> </a> [ *convalida utente credenziali contro l'appartenenza utente Store* ](../membership/validating-user-credentials-against-the-membership-user-store-cs.md) esercitazione è stata esaminata migliorando il controllo di accesso per visualizzare un messaggio più appropriato. Come illustrato nella figura 4, Chris viene visualizzata un messaggio che spiega che egli non può eseguire l'accesso perché il suo account non è ancora approvati.


[![Chris non è possibile perché His Account di accesso è non approvato](unlocking-and-approving-user-accounts-cs/_static/image11.png)](unlocking-and-approving-user-accounts-cs/_static/image10.png)

**Figura 4**: Chris non è possibile perché His Account di accesso è non approvati ([fare clic per visualizzare l'immagine con dimensioni normali](unlocking-and-approving-user-accounts-cs/_static/image12.png))


Per testare la funzionalità di bloccata, provare a eseguire l'accesso come utente autorizzato, ma usare una password errata. Ripetere questo processo il numero di volte fino a quando non è stato bloccato l'account utente necessari. Il controllo di accesso è stato inoltre aggiornato per mostrare un personalizzato il messaggio se il tentativo di eseguire l'accesso da un account bloccato. Si sa che un account è stato bloccato dopo aver avviato il seguente messaggio viene visualizzato nella pagina di accesso: "l'account è stato bloccato a causa di troppi tentativi di accesso non è valido. Contattare l'amministratore per l'account sbloccato."

Tornare al `ManageUsers.aspx` pagina e fare clic sul collegamento Manage per l'utente bloccato. Come illustrato nella figura 5, si dovrebbe essere un valore nel `LastLockedOutDateLabel` pulsante Sblocca utente deve essere abilitato. Fare clic sul pulsante Sblocca utente per sbloccare l'account utente. Dopo aver sbloccato l'utente, saranno in grado di accedere di nuovo.


[![Dave è stato bloccato dal sistema](unlocking-and-approving-user-accounts-cs/_static/image14.png)](unlocking-and-approving-user-accounts-cs/_static/image13.png)

**Figura 5**: Dave è stata bloccata Out del sistema ([fare clic per visualizzare l'immagine con dimensioni normali](unlocking-and-approving-user-accounts-cs/_static/image15.png))


## <a name="step-2-specifying-new-users-approved-status"></a>Passaggio 2: Specificare nuovi utenti approvati stato

Lo stato approvato è utile in scenari in cui si vuole che un'azione da eseguire prima un nuovo utente è in grado di eseguire l'accesso e accedere alle funzionalità specifiche dell'utente del sito. Ad esempio, si potrebbe essere in esecuzione un sito Web privato in cui tutte le pagine, fatta eccezione per le pagine di accesso e l'iscrizione, sono accessibili solo agli utenti autenticati. Ma cosa accade se un estraneo raggiunge il sito Web, consente di trovare la pagina di iscrizione e crea un account? Per evitare questa situazione è possibile spostare la pagina di iscrizione per un `Administration` cartella e richiede che un amministratore crei manualmente ogni account. In alternativa, è possibile consentire tutti gli utenti per l'iscrizione, ma non consentono l'accesso al sito fino a quando un amministratore ha approvato l'account utente.

Per impostazione predefinita, il controllo CreateUserWizard Approva nuovi account. È possibile configurare questo comportamento usando il controllo [ `DisableCreatedUser` proprietà](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.createuserwizard.disablecreateduser.aspx). Impostare questa proprietà su `true` di non approvare i nuovi account utente.

> [!NOTE]
> Per impostazione predefinita il controllo CreateUserWizard accede automaticamente al nuovo account utente. Questo comportamento è determinato del controllo [ `LoginCreatedUser` proprietà](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.createuserwizard.logincreateduser.aspx). Poiché non approvati utenti non possono accedere al sito, quando `DisableCreatedUser` viene `true` il nuovo account utente non è connesso al sito, indipendentemente dal valore della `LoginCreatedUser` proprietà.


Se si crea a livello di codice nuovi account utente tramite il `Membership.CreateUser` metodi, per creare un account utente non approvate di usare uno degli overload che accettano il nuovo utente `IsApproved` valore della proprietà come un parametro di input.

## <a name="step-3-approving-users-by-verifying-their-email-address"></a>Passaggio 3: L'approvazione degli utenti tramite la verifica indirizzo di posta elettronica

Molti siti Web che supportano gli account utente non si approva i nuovi utenti fino a quando non si verifica l'indirizzo di posta elettronica che vengono forniti durante la registrazione. Questo processo di verifica viene comunemente utilizzato per contrastare Bot, gli spammer e altri ne'er-do-wells perché richiede un indirizzo di posta elettronica univoco e soggetti a verifica e aggiunge un ulteriore passaggio nel processo di iscrizione. Con questo modello, quando un nuovo utente effettua l'iscrizione vengono inviati un messaggio di posta elettronica che include un collegamento a una pagina di verifica. Visitando il collegamento utente ha dimostrato che ricevono il messaggio di posta elettronica e, pertanto, che l'indirizzo e-mail fornito sia valido. La pagina di verifica è responsabile per l'approvazione dell'utente. Ciò può verificarsi automaticamente, in tal modo l'approvazione di tutti gli utenti che raggiungono questa pagina, o solo dopo che l'utente fornisce informazioni aggiuntive, ad esempio un [CAPTCHA](http://en.wikipedia.org/wiki/Captcha).

Per ospitare il flusso di lavoro, è necessario innanzitutto aggiornare la pagina di creazione di account in modo che i nuovi utenti siano non approvati. Aprire il `EnhancedCreateUserWizard.aspx` nella pagina la `Membership` cartella e dell'impostare il controllo CreateUserWizard `DisableCreatedUser` proprietà `true`.

Successivamente, è necessario configurare il controllo CreateUserWizard per inviare un messaggio di posta elettronica al nuovo utente con istruzioni su come verificare il proprio account. In particolare, si includerà un collegamento nel messaggio di posta elettronica per il `Verification.aspx` pagina (che dobbiamo ancora creare), passando il nuovo utente `UserId` tramite la stringa di query. Il `Verification.aspx` pagina verrà cercare l'utente specificato e contrassegnarli approvati.

### <a name="sending-a-verification-email-to-new-users"></a>L'invio di un messaggio di verifica per i nuovi utenti

Per inviare un messaggio di posta elettronica dal controllo CreateUserWizard, configurare relativo `MailDefinition` proprietà in modo appropriato. Come descritto nel <a id="Tutorial13"> </a> [esercitazione precedente](recovering-and-changing-passwords-cs.md), i controlli ChangePassword e PassaggiwordRecovery includono una [ `MailDefinition` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.maildefinition.aspx) che funziona in modo analogo CreateUserWizard del controllo.

> [!NOTE]
> Usare la `MailDefinition` opzioni di proprietà è necessario specificare il recapito della posta in `Web.config`. Per altre informazioni, consultare [l'invio di posta elettronica in ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx).


Iniziare creando un nuovo modello di messaggio di posta elettronica denominato `CreateUserWizard.txt` nella `EmailTemplates` cartella. Per il modello, usare il testo seguente:

[!code-aspx[Main](unlocking-and-approving-user-accounts-cs/samples/sample3.aspx)]

Impostare il `MailDefinition'` s `BodyFileName` proprietà su "~ / EmailTemplates/CreateUserWizard.txt" e il relativo `Subject` proprietà su "Benvenuti nel sito Web. Attivare l'account."

Si noti che il `CreateUserWizard.txt` modello di messaggio di posta elettronica include un `<%VerificationUrl%>` segnaposto. Si tratta di dove l'URL per il `Verification.aspx` pagina verrà inserita. CreateUserWizard sostituisce automaticamente il `<%UserName%>` e `<%Password%>` segnaposto con il nuovo account nome utente e password, ma non esiste alcun incorporati `<%VerificationUrl%>` segnaposto. È necessario sostituire manualmente con l'URL di verifica appropriati.

A tale scopo, creare un gestore eventi per il CreateUserWizard [ `SendingMail` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.sendingmail.aspx) e aggiungere il codice seguente:

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample4.cs)]

Il `SendingMail` evento viene generato dopo il `CreatedUser` evento, vale a dire che nel momento in cui il gestore dell'evento precedente esegue il nuovo utente account sia già stato creato. È possibile accedere del nuovo utente `UserId` valore chiamando il `Membership.GetUser` metodo, passando il `UserName` immessi nel controllo CreateUserWizard. Successivamente, l'URL di verifica è nel formato corretto. L'istruzione `Request.Url.GetLeftPart(UriPartial.Authority)` restituisce il `http://yourserver.com` parte dell'URL; `Request.ApplicationPath` restituisce il percorso in cui è possibile eseguire l'applicazione. La verifica URL viene quindi definito come `Verification.aspx?ID=userId`. Questi due stringhe vengono quindi concatenate per comporre l'URL completo. Infine, il corpo del messaggio di posta elettronica (`e.Message.Body`) ha tutte le occorrenze di `<%VerificationUrl%>` sostituito con l'URL completo.

Il risultato finale è che i nuovi utenti siano non approvati, vale a dire che essi non possono accedere al sito. Inoltre, essi vengono inviati automaticamente un messaggio di posta elettronica con un collegamento all'URL di verifica (vedere la figura 6).


[![Il nuovo utente riceve un messaggio di posta elettronica con un collegamento all'URL di verifica](unlocking-and-approving-user-accounts-cs/_static/image17.png)](unlocking-and-approving-user-accounts-cs/_static/image16.png)

**Figura 6**: il nuovo utente riceve un messaggio di posta elettronica con un collegamento all'URL di verifica ([fare clic per visualizzare l'immagine con dimensioni normali](unlocking-and-approving-user-accounts-cs/_static/image18.png))


> [!NOTE]
> Passaggio CreateUserWizard predefinito del controllo CreateUserWizard consente di visualizzare un messaggio che informa l'utente il proprio account è stato creato e viene visualizzato un pulsante Continua. Facendo clic su questa indirizza l'utente all'URL specificato per il controllo `ContinueDestinationPageUrl` proprietà. CreateUserWizard nella `EnhancedCreateUserWizard.aspx` è configurato per l'invio di nuovi utenti per il `~/Membership/AdditionalUserInfo.aspx`, che richiede all'utente le città di residenza, URL della home page e firma. Poiché queste informazioni possono essere aggiunti solo da utenti connessi, è opportuno aggiornare questa proprietà per l'invio agli utenti di tornare alla home page del sito (`~/Default.aspx`). Inoltre, il `EnhancedCreateUserWizard.aspx` pagina o il passaggio CreateUserWizard deve essere ampliata in modo da informare l'utente che sono stati inviati un messaggio di verifica e l'account non verranno attivato fino a quando non seguono le istruzioni riportate in questo messaggio di posta elettronica. Queste modifiche lascia come esercizio per il lettore.


### <a name="creating-the-verification-page"></a>Creazione della pagina di verifica

L'attività finale consiste nel creare il `Verification.aspx` pagina. Aggiungere questa pagina per la cartella radice, associandolo al `Site.master` pagina master. Come illustrato con la maggior parte delle pagine di contenuto precedente aggiunte al sito, rimuovere il controllo contenuto che fa riferimento il `LoginContent` ContentPlaceHolder in modo che la pagina di contenuto Usa la pagina master di contenuto predefinito.

Aggiungere un controllo Web Label per il `Verification.aspx` pagina, impostare relativi `ID` a `StatusMessage` e cancellare la relativa proprietà text. A questo punto, creare il `Page_Load` gestore dell'evento e aggiungere il codice seguente:

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample5.cs)]

La maggior parte del codice precedente verifica che il `UserId` fornito tramite il parametro querystring esista, che è un valido `Guid` valore e che fa riferimento a un account utente esistente. Se tutti questi controlli di esito positivo, l'account utente è approvato; in caso contrario, viene visualizzato un messaggio di stato appropriato.

Figura 7 mostra il `Verification.aspx` pagina quando visitati tramite un browser.


[![Il Account utente nuovo viene ora approvata](unlocking-and-approving-user-accounts-cs/_static/image20.png)](unlocking-and-approving-user-accounts-cs/_static/image19.png)

**Figura 7**: Account utente nuovo il viene ora approvate ([fare clic per visualizzare l'immagine con dimensioni normali](unlocking-and-approving-user-accounts-cs/_static/image21.png))


## <a name="summary"></a>Riepilogo

Tutti gli account utente di appartenenza dispongano di due stati che determinano se l'utente può accedere al sito: `IsLockedOut` e `IsApproved`. Entrambe queste proprietà devono essere `true` per l'utente all'account di accesso.

L'utente del blocco dello stato viene usato come misura di sicurezza per ridurre la probabilità che un pirata informatico suddividendo in un sito tramite i metodi di attacco di forza bruta. In particolare, un utente viene bloccato se sono presenti un certo numero di tentativi di accesso non valido all'interno di un determinato intervallo di tempo. Questi limiti possono essere configurati tramite le impostazioni del provider di appartenenza in `Web.config`.

Lo stato approvato viene comunemente usato come mezzo per impedire l'accesso fino a quando non è verificata un'azione di nuovi utenti. Ad esempio il sito Web richiede che i nuovi account prima di tutto devono essere approvate dall'amministratore o, come mostrato nel passaggio 3, tramite la verifica indirizzo di posta elettronica.

Buona programmazione!

### <a name="about-the-author"></a>Informazioni sull'autore

Scott Mitchell, autore di più libri e fondatore di 4GuysFromRolla.com, ha lavorato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola  *[Sams Teach Yourself ASP.NET 2.0 in 24 ore](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. È possibile contattarlo Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) o il suo blog all'indirizzo [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Ringraziamenti speciali...

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami un messaggio nella [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](recovering-and-changing-passwords-cs.md)
> [Successivo](building-an-interface-to-select-one-user-account-from-many-vb.md)
