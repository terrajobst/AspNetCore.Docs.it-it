---
uid: web-forms/overview/older-versions-security/admin/unlocking-and-approving-user-accounts-cs
title: Lo sblocco e approvare gli account utente (c#) | Documenti Microsoft
author: rick-anderson
description: In questa esercitazione viene illustrato come compilare una pagina web per gli amministratori per la gestione degli utenti bloccati e stati approvati. Si verifica anche come approvare o di nuovi utenti...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2008
ms.topic: article
ms.assetid: 5346aab1-9974-489f-a065-ae3883b8a350
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/admin/unlocking-and-approving-user-accounts-cs
msc.type: authoredcontent
ms.openlocfilehash: 65d32309cbd8bed6decbba4c5027d8e10a558ae8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="unlocking-and-approving-user-accounts-c"></a>Approvazione e lo sblocco degli account utente (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare codice](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/CS.14.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial14_UnlockAndApprove_cs.pdf)

> In questa esercitazione viene illustrato come compilare una pagina web per gli amministratori per la gestione degli utenti bloccati e stati approvati. Si verifica anche come approvare nuovi utenti solo dopo che è accertato l'indirizzo di posta elettronica.


## <a name="introduction"></a>Introduzione

Con un nome utente, password e posta elettronica, ogni account utente dispone di due campi di stato che indicano se l'utente può accedere al sito: bloccati e approvati. Un utente venga bloccato automaticamente se credenziali non valide fornisce un numero specificato di volte all'interno di un numero specificato di minuti (le impostazioni predefinite bloccano un utente dopo 5 tentativi di accesso non valido all'interno di 10 minuti). Lo stato approvato è utile negli scenari in cui un'azione deve trascorrere prima che un nuovo utente è in grado di accedere al sito. Ad esempio, un utente potrebbe essere necessario verificare innanzitutto l'indirizzo di posta elettronica o essere approvate da un amministratore prima di essere in grado di account di accesso.

Poiché un bloccato out o non approvati utente non è possibile eseguire l'accesso, è naturale solo a chiedersi come questi stati possono essere reimpostati. ASP.NET non include una funzionalità incorporata o controlli Web per la gestione degli utenti bloccati e approvato gli stati, in parte perché tali decisioni devono essere gestiti in modo da sito a sito. Alcuni siti potrebbero approvare automaticamente tutti i nuovi account utente (il comportamento predefinito). Gli altri sono un amministratore di approvare i nuovi account o non approvare utenti fino a quando visitano un collegamento inviato all'indirizzo di posta elettronica fornito quando si è effettuato l'iscrizione. Analogamente, alcuni siti potrebbero bloccare gli utenti fino a quando un amministratore reimposta il relativo stato, mentre altri siti inviano un messaggio di posta elettronica all'utente di blocco con un URL possono accedere per sbloccare l'account.

In questa esercitazione viene illustrato come compilare una pagina web per gli amministratori per la gestione degli utenti bloccati e stati approvati. Si verifica anche come approvare nuovi utenti solo dopo che è accertato l'indirizzo di posta elettronica.

## <a name="step-1-managing-users-locked-out-and-approved-statuses"></a>Passaggio 1: Gestione degli utenti bloccati e gli stati di approvazione

Nel <a id="Tutorial12"> </a> [ *creazione di un'interfaccia per selezionare un Account utente da molti* ](building-an-interface-to-select-one-user-account-from-many-cs.md) esercitazione è costruito una pagina in cui è elencato ogni account utente in un paging filtrati GridView. La griglia elenca ogni nome utente e posta elettronica, i relativi stati approvati e bloccati, se si è attualmente online ed eventuali commenti sull'utente. Per la gestione degli utenti approvati e gli stati di blocco, è possibile stabilire questa griglia modificabile. Per modificare lo stato di approvazione dell'utente, l'amministratore innanzitutto individuare l'account utente e quindi modificare la riga corrispondente di GridView, selezionando o deselezionando la casella di controllo approvato. In alternativa, è possibile gestire gli stati approvati e bloccati tramite una pagina ASP.NET.

Per questa esercitazione si userà due pagine ASP.NET: `ManageUsers.aspx` e `UserInformation.aspx`. L'idea è che `ManageUsers.aspx` Elenca gli account utente nel sistema, mentre `UserInformation.aspx` consente all'amministratore di gestire gli stati approvati e bloccati per un utente specifico. Il primo è per potenziare GridView in `ManageUsers.aspx` per includere un HyperLinkField, che esegue il rendering come una colonna di collegamenti. Si desidera che ogni collegamento per fare riferimento a `UserInformation.aspx?user=UserName`, dove *UserName* è il nome dell'utente da modificare.

> [!NOTE]
> Se è stato scaricato il codice per il <a id="Tutorial13"> </a> [ *Recovering e modifica delle password* ](recovering-and-changing-passwords-cs.md) esercitazione si può notare che il `ManageUsers.aspx` pagina contiene già un set di " Gestire"collegamenti e `UserInformation.aspx` pagina fornisce un'interfaccia per la modifica delle password dell'utente selezionato. Ho deciso di non replicare tale funzionalità in codice associato a questa esercitazione perché funzionante evitando di utilizzare l'API di appartenenza e operare direttamente con il database di SQL Server per modificare la password dell'utente. In questa esercitazione inizia da zero con la `UserInformation.aspx` pagina.


### <a name="adding-manage-links-to-theuseraccountsgridview"></a>Aggiunta di "Gestisci" collegamenti per il`UserAccounts`GridView

Aprire il `ManageUsers.aspx` pagina e aggiungere un HyperLinkField per il `UserAccounts` GridView. Impostare il HyperLinkField `Text` proprietà su "Manage" e il relativo `DataNavigateUrlFields` e `DataNavigateUrlFormatString` proprietà `UserName` e "UserInformation.aspx?user={0}", rispettivamente. Queste impostazioni configurano il HyperLinkField in modo che tutti i collegamenti ipertestuali visualizzare il testo "Manage", ma ogni collegamento passa appropriata *UserName* valore nella stringa di query.

Dopo aver aggiunto il HyperLinkField a GridView, dedicare alcuni minuti per visualizzare il `ManageUsers.aspx` pagina tramite un browser. Come illustrato nella figura 1, ogni riga GridView ora include un collegamento "Gestisci". Il collegamento "Gestisci" per Bruce punta a `UserInformation.aspx?user=Bruce`, mentre il collegamento "Gestisci" per Dave punta a `UserInformation.aspx?user=Dave`.


[![Aggiunge il HyperLinkField un](unlocking-and-approving-user-accounts-cs/_static/image2.png)](unlocking-and-approving-user-accounts-cs/_static/image1.png)

**Figura 1**: il HyperLinkField aggiunge un collegamento "Gestisci" per ogni Account utente ([fare clic per visualizzare l'immagine ingrandita](unlocking-and-approving-user-accounts-cs/_static/image3.png))


Verrà creato l'interfaccia utente e codice per il `UserInformation.aspx` pagina in un momento, ma prima si parlare su come modificare a livello di codice un utente bloccato e gli stati approvati. Il [ `MembershipUser` classe](https://msdn.microsoft.com/en-us/library/system.web.security.membershipuser.aspx) è [ `IsLockedOut` ](https://msdn.microsoft.com/en-us/library/system.web.security.membershipuser.islockedout.aspx) e [ `IsApproved` proprietà](https://msdn.microsoft.com/en-us/library/system.web.security.membershipuser.isapproved.aspx). Il `IsLockedOut` proprietà è di sola lettura. Non è disponibile alcun meccanismo di blocco a livello di codice un utente. Per sbloccare un utente, utilizzare il `MembershipUser` della classe [ `UnlockUser` metodo](https://msdn.microsoft.com/en-us/library/system.web.security.membershipuser.unlockuser.aspx). Il `IsApproved` proprietà è leggibile e scrivibile. Per salvare le modifiche a questa proprietà, è necessario chiamare il `Membership` della classe [ `UpdateUser` metodo](https://msdn.microsoft.com/en-us/library/system.web.security.membership.updateuser.aspx), passando il modificato `MembershipUser` oggetto.

Poiché il `IsApproved` proprietà è leggibile e scrivibile, un controllo casella di controllo è probabile che l'elemento dell'interfaccia utente migliore per la configurazione di questa proprietà. Tuttavia, una casella di controllo verrà non funzionano per i `IsLockedOut` proprietà perché non è possibile bloccare un utente amministratore, può solo sbloccare un utente. Un'interfaccia utente appropriato per il `IsLockedOut` proprietà è un pulsante che, quando si fa clic, sblocca l'account utente. Questo pulsante deve essere abilitato solo se l'utente è bloccato.

### <a name="creating-theuserinformationaspxpage"></a>Creazione di`UserInformation.aspx`pagina

A questo punto si è pronti implementare l'interfaccia utente in `UserInformation.aspx`. Aprire questa pagina e aggiungere i controlli Web seguenti:

- Controllare un collegamento ipertestuale che, quando si fa clic, restituisce l'amministratore per il `ManageUsers.aspx` pagina.
- Un controllo etichetta Web per visualizzare il nome dell'utente selezionato. Impostare questa etichetta `ID` a `UserNameLabel` e cancellare la `Text` proprietà.
- Un controllo casella di controllo denominato `IsApproved`. Impostare il relativo `AutoPostBack` proprietà `true`.
- Data dell'ultimo blocco un controllo etichetta per la visualizzazione dell'utente. Questa etichetta nome `LastLockedOutDateLabel` e cancellare la `Text` proprietà.
- Pulsante per lo sblocco dell'utente. Nome di questo pulsante `UnlockUserButton` e impostare il relativo `Text` proprietà su "Sblocca utente".
- Un controllo etichetta per la visualizzazione dei messaggi di stato, ad esempio, "stato di approvazione dell'utente è stato aggiornato". Nome di questo controllo `StatusMessage`, deselezionare le relative `Text` proprietà e set relativo `CssClass` proprietà `Important`. (Il `Important` classe CSS è definita nel `Styles.css` file di foglio di stile; Visualizza il testo corrispondente in un tipo di carattere di grandi dimensioni, di colore rosso.)

Dopo l'aggiunta di questi controlli, la visualizzazione di progettazione in Visual Studio dovrebbe essere simile alla schermata nella figura 2.


[![Creare l'interfaccia utente per UserInformation.aspx](unlocking-and-approving-user-accounts-cs/_static/image5.png)](unlocking-and-approving-user-accounts-cs/_static/image4.png)

**Figura 2**: creare l'interfaccia utente per `UserInformation.aspx` ([fare clic per visualizzare l'immagine ingrandita](unlocking-and-approving-user-accounts-cs/_static/image6.png))


Con l'interfaccia utente completa, l'attività successiva consiste nell'impostare il `IsApproved` casella di controllo e altri controlli in base alle informazioni dell'utente selezionato. Creare un gestore eventi per la pagina `Load` eventi e aggiungere il codice seguente:

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample1.cs)]

Il codice sopra riportato avvia, verificare che questa è la prima visita la pagina e non un postback successivi. Quindi legge il nome utente passato tramite la `user` campo querystring e recupera le informazioni relative all'account utente tramite il `Membership.GetUser(username)` metodo. Se è stato fornito alcun nome utente tramite la stringa di query o se non è stato possibile trovare l'utente specificato, l'amministratore viene inviato il `ManageUsers.aspx` pagina.

Il `MembershipUser` dell'oggetto `UserName` valore viene quindi visualizzato nel `UserNameLabel` e `IsApproved` casella di controllo è selezionata in base il `IsApproved` valore della proprietà.

Il `MembershipUser` dell'oggetto [ `LastLockoutDate` proprietà](https://msdn.microsoft.com/en-us/library/system.web.security.membershipuser.lastlockoutdate.aspx) restituisce un `DateTime` valore che indica quando l'utente è l'ultimo bloccato. Se l'utente non è stato bloccato, il valore restituito dipende dal provider di appartenenze. Quando viene creato un nuovo account, il `SqlMembershipProvider` imposta il `aspnet_Membership` della tabella `LastLockoutDate` campo `1754-01-01 12:00:00 AM`. Il codice precedente consente di visualizzare una stringa vuota nel `LastLockoutDateLabel` se il `LastLockoutDate` proprietà si verifica prima dell'anno 2000; in caso contrario, la parte relativa alla data del `LastLockoutDate` proprietà viene visualizzata nell'etichetta. Il `UnlockUserButton'` s `Enabled` proprietà è impostata per l'utente bloccato lo stato, vale a dire che questo pulsante viene abilitato solo se l'utente è bloccato.

È opportuno testare il `UserInformation.aspx` pagina tramite un browser. Naturalmente, occorre iniziare in corrispondenza di `ManageUsers.aspx` e selezionare un account utente da gestire. Al momento in arrivo presso `UserInformation.aspx`, si noti che il `IsApproved` casella di controllo è selezionata solo se l'utente è approvato. Se l'utente è mai stato bloccato, la loro ultima bloccato Data. Pulsante Sblocca utente è abilitato solo se l'utente è attualmente bloccato. Selezionare o deselezionare il `IsApproved` casella di controllo o facendo clic sul pulsante Sblocca utente provoca un postback, ma non vengono eseguite modifiche all'account utente perché è stata ancora per creare i gestori eventi per questi eventi.

Tornare a Visual Studio e creare gestori eventi per il `IsApproved` della casella di controllo `CheckedChanged` evento e `UnlockUser` del pulsante `Click` evento. Nel `CheckedChanged` gestore dell'evento, impostare l'utente `IsApproved` proprietà per il `Checked` proprietà della casella di controllo e quindi salvare le modifiche tramite una chiamata a `Membership.UpdateUser`. Nel `Click` gestore dell'evento, semplicemente chiamare la `MembershipUser` dell'oggetto `UnlockUser` metodo. In entrambi i gestori eventi, viene visualizzato un messaggio appropriato nella `StatusMessage` etichetta.

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample2.cs)]

### <a name="testing-theuserinformationaspxpage"></a>Test di`UserInformation.aspx`pagina

Con questi gestori eventi sul posto, rivedere la pagina e non approvati un utente. Come mostrato nella figura 3, verrà visualizzato una breve messaggio nella pagina che indica che l'utente `IsApproved` proprietà è stata modificata correttamente.


[![Chris è stato non approvato](unlocking-and-approving-user-accounts-cs/_static/image8.png)](unlocking-and-approving-user-accounts-cs/_static/image7.png)

**Figura 3**: Chris è stato Unapproved ([fare clic per visualizzare l'immagine ingrandita](unlocking-and-approving-user-accounts-cs/_static/image9.png))


Successivamente, disconnettersi e provare a eseguire l'accesso come utente il cui account è stato appena non approvati. Perché l'utente non viene approvata, essi non è possibile eseguire l'accesso. Per impostazione predefinita, il controllo di accesso consente di visualizzare lo stesso messaggio se l'utente non è possibile eseguire l'accesso, a prescindere dal motivo. Ma nel <a id="Tutorial6"> </a> [ *convalida utente contro l'appartenenza utente archivio credenziali* ](../membership/validating-user-credentials-against-the-membership-user-store-cs.md) esercitazione è stato esaminato migliorare il controllo di accesso per visualizzare un messaggio più appropriato. Come illustrato nella figura 4, Chris viene visualizzata un messaggio che spiega che ha non è possibile eseguire l'accesso perché il suo account non è ancora approvati.


[![Chris non è possibile perché His Account di accesso è non approvato](unlocking-and-approving-user-accounts-cs/_static/image11.png)](unlocking-and-approving-user-accounts-cs/_static/image10.png)

**Figura 4**: Chris non è possibile perché His Account di accesso è non approvati ([fare clic per visualizzare l'immagine ingrandita](unlocking-and-approving-user-accounts-cs/_static/image12.png))


Per testare la funzionalità di bloccata, tentare di eseguire l'accesso come utente autorizzato, ma utilizzare una password errata. Ripetere questo processo, il numero di volte fino a quando non è stato bloccato l'account utente necessari. Il controllo di accesso è stato inoltre aggiornato per mostrare un oggetto personalizzato dei messaggi se il tentativo di accesso da un account bloccato. Si è certi che un account è stato bloccato quando si inizia a visualizzare il messaggio seguente nella pagina di accesso: "l'account è stato bloccato a causa di troppi tentativi di accesso non valido. Contattare l'amministratore per l'account sbloccato."

Restituito per il `ManageUsers.aspx` pagina e fare clic sul collegamento di gestione per l'utente bloccato. Come illustrato nella figura 5, si dovrebbe essere un valore di `LastLockedOutDateLabel` pulsante Sblocca utente deve essere abilitato. Fare clic sul pulsante Sblocca utente per sbloccare l'account utente. Dopo aver sbloccato l'utente, saranno in grado di nuovo l'accesso.


[![Dave è stato bloccato dal sistema](unlocking-and-approving-user-accounts-cs/_static/image14.png)](unlocking-and-approving-user-accounts-cs/_static/image13.png)

**Figura 5**: Dave è stato bloccato Out del sistema ([fare clic per visualizzare l'immagine ingrandita](unlocking-and-approving-user-accounts-cs/_static/image15.png))


## <a name="step-2-specifying-new-users-approved-status"></a>Passaggio 2: Specificare nuovi utenti approvati stato

Lo stato approvato è utile negli scenari in cui si desidera un'azione da eseguire prima un nuovo utente è in grado di connettersi e accedere alle funzionalità specifiche dell'utente del sito. Ad esempio, si potrebbe essere in esecuzione un sito Web privato in cui tutte le pagine, ad eccezione delle pagine di accesso e l'iscrizione, sono accessibili solo agli utenti autenticati. Ma cosa accade se un estraneo raggiunge il sito Web, consente di trovare la pagina di iscrizione e crea un account? Per evitare questa situazione è possibile spostare la pagina di iscrizione per un `Administration` cartella e richiede che un amministratore di creare manualmente ogni account. In alternativa, è possibile consentono a chiunque di iscrizione, ma non consentire l'accesso del sito fino a quando non approvate da un amministratore dell'account utente.

Per impostazione predefinita, il controllo CreateUserWizard Approva nuovi account. È possibile configurare questo comportamento utilizzando il controllo [ `DisableCreatedUser` proprietà](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.createuserwizard.disablecreateduser.aspx). Impostare questa proprietà su `true` non approvare nuovi account utente.

> [!NOTE]
> Per impostazione predefinita il controllo CreateUserWizard accede automaticamente al nuovo account utente. Questo comportamento è determinato del controllo [ `LoginCreatedUser` proprietà](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.createuserwizard.logincreateduser.aspx). Poiché gli utenti non approvati impossibilità di accedere al sito, quando `DisableCreatedUser` è `true` il nuovo account utente non è connesso al sito, indipendentemente dal valore della `LoginCreatedUser` proprietà.


Se si crea a livello di codice nuovi account utente tramite il `Membership.CreateUser` metodo, per creare un account utente non approvati, usare uno degli overload che accettano il nuovo utente `IsApproved` valore della proprietà come parametro di input.

## <a name="step-3-approving-users-by-verifying-their-email-address"></a>Passaggio 3: Approvazione utenti verificando l'indirizzo di posta elettronica

Molti siti Web che supportano gli account utente non approvare nuovi utenti fino a quando non si verifica l'indirizzo di posta elettronica che sono forniti durante la registrazione. Il processo di verifica in genere viene utilizzato per contrastare robot, invio di posta indesiderata e altri ne'er-do-wells perché richiede un indirizzo di posta elettronica univoco, verifica e aggiunge un ulteriore passaggio nel processo di iscrizione. Con questo modello, quando un nuovo utente effettua l'iscrizione viene effettuato un messaggio di posta elettronica che include un collegamento a una pagina di verifica. Facendo clic sul link l'utente ha dimostrato che ricevono il messaggio di posta elettronica e, pertanto che l'indirizzo e-mail fornito è valido. La pagina di verifica è responsabile dell'approvazione dell'utente. Questo può essere eseguita automaticamente, in tal modo approvazione tutti gli utenti che raggiungono questa pagina, o solo dopo che l'utente fornisce informazioni aggiuntive, ad esempio un [CAPTCHA](http://en.wikipedia.org/wiki/Captcha).

Per gestire il flusso di lavoro, è necessario innanzitutto aggiornare la pagina di creazione di account in modo che i nuovi utenti sono non approvati. Aprire il `EnhancedCreateUserWizard.aspx` nella pagina di `Membership` cartella e il controllo CreateUserWizard insieme `DisableCreatedUser` proprietà `true`.

Successivamente, è necessario configurare il controllo CreateUserWizard per inviare un messaggio di posta elettronica per il nuovo utente con istruzioni su come verificare il proprio account. In particolare, verrà incluso un collegamento nel messaggio di posta elettronica per il `Verification.aspx` pagina (che sono state ancora da creare), passando il nuovo utente `UserId` tramite la stringa di query. Il `Verification.aspx` pagina cercare l'utente specificato e contrassegnarli approvato.

### <a name="sending-a-verification-email-to-new-users"></a>L'invio di un messaggio di verifica ai nuovi utenti

Per inviare un messaggio di posta elettronica dal controllo CreateUserWizard, configurare il relativo `MailDefinition` proprietà in modo appropriato. Come descritto nel <a id="Tutorial13"> </a> [esercitazione precedente](recovering-and-changing-passwords-cs.md), i controlli ChangePassword e PasswordRecovery includono un [ `MailDefinition` proprietà](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.maildefinition.aspx) che funziona in modo analogo CreateUserWizard del controllo.

> [!NOTE]
> Utilizzare il `MailDefinition` opzioni di proprietà, è necessario specificare il recapito della posta in `Web.config`. Per ulteriori informazioni, vedere [l'invio di posta elettronica in ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx).


Iniziare creando un nuovo modello di posta elettronica denominato `CreateUserWizard.txt` nel `EmailTemplates` cartella. Utilizzare il seguente testo per il modello:

[!code-aspx[Main](unlocking-and-approving-user-accounts-cs/samples/sample3.aspx)]

Impostare il `MailDefinition'` s `BodyFileName` proprietà su "~ / EmailTemplates/CreateUserWizard.txt" e il relativo `Subject` proprietà su "Benvenuti al mio sito Web Attivare l'account."

Si noti che il `CreateUserWizard.txt` modello di posta elettronica include un `<%VerificationUrl%>` segnaposto. Questa opzione è quando l'URL per il `Verification.aspx` pagina verrà inserita. CreateUserWizard sostituisce automaticamente il `<%UserName%>` e `<%Password%>` segnaposto con il nuovo account nome utente e password, ma non esiste alcun incorporato `<%VerificationUrl%>` segnaposto. È necessario sostituire manualmente con l'URL di verifica appropriati.

A tale scopo, creare un gestore eventi per il CreateUserWizard [ `SendingMail` evento](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.sendingmail.aspx) e aggiungere il codice seguente:

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample4.cs)]

Il `SendingMail` evento viene generato dopo il `CreatedUser` evento, vale a dire che nel momento in cui il gestore dell'evento precedente viene eseguito il nuovo utente account è già stato creato. È possibile accedere al nuovo utente `UserId` valore chiamando il `Membership.GetUser` metodo passando la `UserName` immesso nel controllo CreateUserWizard. Successivamente, l'URL di verifica è corretto. L'istruzione `Request.Url.GetLeftPart(UriPartial.Authority)` restituisce il `http://yourserver.com` parte dell'URL; `Request.ApplicationPath` restituisce percorso in cui è possibile eseguire l'applicazione. La verifica URL viene quindi definito come `Verification.aspx?ID=userId`. Questi due stringhe vengono quindi concatenate per formare l'URL completo. Infine, il corpo del messaggio di posta elettronica (`e.Message.Body`) dispone di tutte le occorrenze di `<%VerificationUrl%>` sostituito con l'URL completo.

L'effetto finale è che i nuovi utenti sono non approvati, vale a dire che non accedono al sito. Inoltre, essi vengono inviati automaticamente un messaggio di posta elettronica con un collegamento all'URL di verifica (vedere Figura 6).


[![Il nuovo utente riceve un messaggio di posta elettronica con un collegamento all'URL di verifica](unlocking-and-approving-user-accounts-cs/_static/image17.png)](unlocking-and-approving-user-accounts-cs/_static/image16.png)

**Figura 6**: il nuovo utente riceve un messaggio di posta elettronica con un collegamento all'URL di verifica ([fare clic per visualizzare l'immagine ingrandita](unlocking-and-approving-user-accounts-cs/_static/image18.png))


> [!NOTE]
> Passaggio di CreateUserWizard predefinito del controllo CreateUserWizard Visualizza un messaggio che informa l'utente con il proprio account è stato creato e viene visualizzato un pulsante Continua. Facendo clic su questa richiede l'utente all'URL specificato per il controllo `ContinueDestinationPageUrl` proprietà. CreateUserWizard in `EnhancedCreateUserWizard.aspx` è configurato per inviare ai nuovi utenti del `~/Membership/AdditionalUserInfo.aspx`, che richiede l'immissione di loro hometown URL home page e firma. Poiché queste informazioni possono essere aggiunti solo da utenti con accesso, è opportuno aggiornare questa proprietà per l'invio di utenti alla home page del sito (`~/Default.aspx`). Inoltre, il `EnhancedCreateUserWizard.aspx` pagina o il passaggio CreateUserWizard deve essere ampliato per informare l'utente che sono stati inviati un messaggio di verifica e il proprio account non verrà attivato fino a quando non seguono le istruzioni riportate in questo messaggio di posta elettronica. Un esercizio lasciare queste modifiche per il lettore.


### <a name="creating-the-verification-page"></a>Creazione della pagina di verifica

L'attività finale consiste nel creare il `Verification.aspx` pagina. Aggiungere questa pagina per la cartella radice, associarla al `Site.master` pagina master. Come che abbiamo realizzato con la maggior parte delle pagine precedenti contenute aggiunte al sito, rimuovere il controllo contenuto che fa riferimento il `LoginContent` ContentPlaceHolder in modo che la pagina di contenuto utilizza la pagina master di contenuto predefinito.

Aggiungere un controllo etichetta Web per il `Verification.aspx` pagina, impostare il relativo `ID` a `StatusMessage` e cancellare la proprietà text. Successivamente, creare il `Page_Load` gestore dell'evento e aggiungere il codice seguente:

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample5.cs)]

La maggior parte del codice precedente verifica che il `UserId` fornito tramite il parametro querystring esista, che è un valido `Guid` valore e che fa riferimento a un account utente esistente. Se tutti questi controlli di esito positivo, l'account utente è approvato; in caso contrario, viene visualizzato un messaggio di stato appropriato.

Figura 7 illustra il `Verification.aspx` pagina quando visitato tramite un browser.


[![Il Account utente nuovo è approvato ora](unlocking-and-approving-user-accounts-cs/_static/image20.png)](unlocking-and-approving-user-accounts-cs/_static/image19.png)

**Figura 7**: Account utente nuovo il è ora Approved ([fare clic per visualizzare l'immagine ingrandita](unlocking-and-approving-user-accounts-cs/_static/image21.png))


## <a name="summary"></a>Riepilogo

Tutti gli account utente di appartenenza dispongano di due stati che determinano se l'utente può accedere al sito: `IsLockedOut` e `IsApproved`. Entrambe queste proprietà devono essere `true` per l'utente all'account di accesso.

L'utente del bloccato lo stato viene utilizzato come misura di sicurezza per ridurre la probabilità che un pirata informatico la suddivisione in un sito tramite i metodi di attacchi di forza bruta. In particolare, un utente viene bloccato se sono presenti un certo numero di tentativi di accesso non valido all'interno di un determinato intervallo di tempo. Questi limiti sono configurabili tramite le impostazioni del provider di appartenenze in `Web.config`.

Lo stato approvato in genere viene utilizzato come mezzo per impedire l'accesso fino a quando non è trascorso di un'azione di nuovi utenti. Ad esempio il sito Web richiede che i nuovi account essere prima approvata dall'amministratore o, come illustrato nel passaggio 3, verificando l'indirizzo di posta elettronica.

Buona programmazione!

### <a name="about-the-author"></a>Informazioni sull'autore

Scott Mitchell, autore di più libri e fondatore di 4GuysFromRolla, ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è  *[SAM insegna manualmente ASP.NET 2.0 nelle 24 ore](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott può essere raggiunto al [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) o tramite il suo blog all'indirizzo [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Ringraziamenti speciali...

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Precedente](recovering-and-changing-passwords-cs.md)
[Successivo](building-an-interface-to-select-one-user-account-from-many-vb.md)
