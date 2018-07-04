---
uid: web-forms/overview/older-versions-security/admin/recovering-and-changing-passwords-vb
title: Recupero e modifica delle password (VB) | Microsoft Docs
author: rick-anderson
description: ASP.NET include due controlli Web per il supporto tramite il recupero e modifica delle password. Il controllo PassaggiwordRecovery Abilita un visitatore ripristinare il suo indirizzo pa perso...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2008
ms.topic: article
ms.assetid: f9adcb5d-6d70-4885-a3bf-ed95efb4da1a
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-security/admin/recovering-and-changing-passwords-vb
msc.type: authoredcontent
ms.openlocfilehash: 68b70708904a46e31639331bbfd0dd31240ea421
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393996"
---
<a name="recovering-and-changing-passwords-vb"></a>Recupero e modifica delle password (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare il codice](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/VB.13.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial13_ChangingPasswords_vb.pdf)

> ASP.NET include due controlli Web per il supporto tramite il recupero e modifica delle password. Il controllo PassaggiwordRecovery Abilita un visitatore ripristinare la password persa. Il controllo ChangePassword consente all'utente di aggiornare la relativa password. Come gli altri controlli correlati all'accesso Web abbiamo visto in tutta questa serie di esercitazioni di PassaggiwordRecovery e ChangePassword controlla funzionano con il framework di appartenenza dietro le quinte di reimpostare o modificare le password degli utenti.


## <a name="introduction"></a>Introduzione

Tra i siti Web per la mia bank, azienda, compagnia telefonica, account di posta elettronica e i portali web personalizzata, come la maggior parte delle persone, ho decine di ricordare diverse password. Con così tante credenziali memorizzare questi giorni, non è raro che gli utenti dimentichino le password. Per questo motivo, siti Web che offrono gli account utente necessario includere un modo per un utente ripristinare la password. Questo processo implica in genere la generazione di una nuova password casuale e inviarlo all'indirizzo di posta elettronica dell'utente nel file. Dopo aver ricevuto la nuova password maggior parte degli utenti ritorna al sito e modificare la password da quello generato in modo casuale a uno più facili da ricordare.

ASP.NET include due controlli Web per il supporto tramite il recupero e modifica delle password. Il controllo PassaggiwordRecovery Abilita un visitatore ripristinare la password persa. Il controllo ChangePassword consente all'utente di aggiornare la relativa password. Come gli altri controlli correlati all'accesso Web abbiamo visto in tutta questa serie di esercitazioni di PassaggiwordRecovery e ChangePassword controlla funzionano con il framework di appartenenza dietro le quinte di reimpostare o modificare le password degli utenti.

In questa esercitazione verranno esaminate tramite questi due controlli. Si vedrà anche come a livello di codice, modificare e reimpostare la password dell'utente tramite il `MembershipUser` della classe `ChangePassword` e `ResetPassword` metodi.

## <a name="step-1-helping-users-recover-lost-passwords"></a>Passaggio 1: Aiutare gli utenti Recover perdita le password

Tutti i siti Web che supportano gli account utente necessario fornire agli utenti un meccanismo per ripristinare le password dimenticate. La buona notizia è che l'implementazione di queste funzionalità in ASP.NET è semplicissima, grazie al controllo Web PassaggiwordRecovery. Il controllo PassaggiwordRecovery esegue il rendering di un'interfaccia che richiede all'utente il nome utente e, se necessario, la risposta alla loro domanda di sicurezza. Quindi invia tramite posta elettronica all'utente la password.

> [!NOTE]
> Poiché i messaggi di posta elettronica vengono trasmessi in rete in testo normale sono i rischi di sicurezza con l'invio della password dell'utente tramite posta elettronica.


Il controllo PassaggiwordRecovery è costituito da tre visualizzazioni:

- **Nome utente** -chiede il visitatore per il nome utente. Si tratta della visualizzazione iniziale.
- **Domanda**-Visualizza domande di nome utente e la sicurezza dell'utente come testo, unitamente a una casella di testo per l'utente di immettere la risposta alla sua domanda di sicurezza.
- **Operazione riuscita**-viene visualizzato un messaggio che informa l'utente che la password è stata inviata.

Visualizzare le viste e le azioni eseguite dal controllo PassaggiwordRecovery dipendono le seguenti impostazioni di configurazione di appartenenza:

- `RequiresQuestionAndAnswer`
- `EnablePasswordRetrieval`
- `EnablePasswordReset`

Il framework di appartenenza `RequiresQuestionAndAnswer` impostazione indica se gli utenti devono specificare una domanda di sicurezza e una risposta durante la registrazione per un account. Come illustrato nella <a id="_msoanchor_1"> </a> [ *creazione degli account utente* ](../membership/creating-user-accounts-vb.md) tutorial, if `RequiresQuestionAndAnswer` è True (impostazione predefinita), interfaccia del CreateUserWizard include nella casella di testo controlli per la domanda di sicurezza del nuovo utente e fornire una risposta. Se `RequiresQuestionAndAnswer` è False, nessun tali informazioni vengono raccolte. Analogamente, se `RequiresQuestionAndAnswer` è True, quindi verrà visualizzato il controllo di PassaggiwordRecovery consente di visualizzare la domanda dopo che l'utente ha immesso il nome utente, viene ripristinata la password solo se l'utente immette la risposta di sicurezza corrette. Se `RequiresQuestionAndAnswer` è False, tuttavia, si sposta il controllo PassaggiwordRecovery direttamente dalla visualizzazione nome utente nella visualizzazione operazione riuscita.

Dopo che l'utente ha fornito il suo nome utente - o il suo nome utente e la sicurezza di risposta, se `RequiresQuestionAndAnswer` è True: il PassaggiwordRecovery invia tramite posta elettronica all'utente la relativa password. Se il `EnablePasswordRetrieval` opzione è impostata su True e quindi l'utente viene inviato tramite posta elettronica la password corrente. Se è impostata su False e `EnablePasswordReset` è impostata su True, il controllo PassaggiwordRecovery genera una nuova password casuale per l'utente e invia tramite posta elettronica questa nuova password ad essi. Se entrambe `EnablePasswordRetrieval` e `EnablePasswordReset` sono False, il controllo PassaggiwordRecovery genera un'eccezione.

> [!NOTE]
> Si tenga presente che il `SqlMembershipProvider` archivia le password degli utenti in uno dei tre formati: Clear, Hashed (predefinito) o crittografato. Il meccanismo di archiviazione usato dipende le impostazioni di configurazione di appartenenza; l'applicazione demo Usa il formato della password con hash. Quando si usa il formato della password con hash di `EnablePasswordRetrieval` opzione deve essere impostata su False perché il sistema non può determinare la password dell'utente effettivo della versione con hash archiviato nel database.


Figura 1 illustra come interfaccia e il comportamento di PassaggiwordRecovery è influenzato dalla configurazione di appartenenza.


[![Il RequiresQuestionAndAnswer EnablePasswordRetrieval ed EnablePasswordReset influenzare l'aspetto e il comportamento del controllo PassaggiwordRecovery](recovering-and-changing-passwords-vb/_static/image2.png)](recovering-and-changing-passwords-vb/_static/image1.png)

**Figura 1**: il `RequiresQuestionAndAnswer`, `EnablePasswordRetrieval`, e `EnablePasswordReset` influenzare l'aspetto e il comportamento del controllo PassaggiwordRecovery ([fare clic per visualizzare l'immagine con dimensioni normali](recovering-and-changing-passwords-vb/_static/image3.png))


> [!NOTE]
> Nel <a id="_msoanchor_2"> </a> [ *creazione dello Schema di appartenenza in SQL Server* ](../membership/creating-the-membership-schema-in-sql-server-vb.md) esercitazione è stato configurato il provider di appartenenza impostando `RequiresQuestionAndAnswer` su True, `EnablePasswordRetrieval` a False, e `EnablePasswordReset` su True.


### <a name="using-the-passwordrecovery-control"></a>Utilizzo del controllo PassaggiwordRecovery

Verrà ora esaminato tramite il controllo PassaggiwordRecovery in una pagina ASP.NET. Aprire `RecoverPassword.aspx` e trascinare e rilasciare un controllo PassaggiwordRecovery dalla casella degli strumenti nella finestra di progettazione; impostare relativi `ID` a `RecoverPwd`. Analogamente ai controlli CreateUserWizard Web e account di accesso, le visualizzazioni del controllo PassaggiwordRecovery eseguire il rendering di una ricca interfaccia composita che include le etichette, caselle di testo, pulsanti e i controlli di convalida. È possibile personalizzare l'aspetto delle visualizzazioni tramite proprietà di stile del controllo o convertendo le viste in modelli. Lasciare come esercizio per il lettore di interesse.

Quando un utente visita questa pagina Anna verrà immettere il proprio nome utente e fare clic sul pulsante Invia. Poiché è stato impostato il `RequiresQuestionAndAnswer` proprietà su True nelle impostazioni di configurazione appartenenza, il PassaggiwordRecovery controllare will quindi visualizzare la visualizzazione domanda. Dopo che l'utente immette la risposta di sicurezza corretto e fa clic su invio, il controllo PassaggiwordRecovery verrà aggiornare la password dell'utente a uno casuale e questa password per l'indirizzo di posta elettronica nel file di posta elettronica. Tutto questo è stato possibile senza dover scrivere una singola riga di codice!

Prima di testare questa pagina, vi è un'ultima informazione di configurazione da tendono a: è necessario specificare le impostazioni di recapito di posta elettronica in `Web.config`. Il controllo PassaggiwordRecovery si basa su queste impostazioni per l'invio di posta elettronica.

La configurazione di recapito di posta elettronica viene specificata tramite il [ `<system.net>` elemento](https://msdn.microsoft.com/library/6484zdc1.aspx)del [ `<mailSettings>` elemento](https://msdn.microsoft.com/library/w355a94k.aspx). Usare la [ `<smtp>` elemento](https://msdn.microsoft.com/library/ms164240.aspx) per indicare il metodo di recapito e il valore predefinito dall'indirizzo. Il markup seguente configura le impostazioni di posta elettronica per l'uso di un server SMTP di rete denominato `smtp.example.com` sulla porta 25 e con le credenziali nome utente/password del nome utente e password.

> [!NOTE]
> `<system.net>` è un elemento figlio della radice `<configuration>` elemento e pari livello `<system.web>`. Pertanto, non inserire il `<system.net>` elemento all'interno di `<system.web>` elemento; invece inserirlo allo stesso livello.


[!code-xml[Main](recovering-and-changing-passwords-vb/samples/sample1.xml)]

Oltre a usare un server SMTP nella rete, è possibile specificare una directory di prelievo in cui devono essere depositati messaggi di posta elettronica da inviare.

Dopo aver configurato le impostazioni SMTP, visitare il `RecoverPassword.aspx` pagina tramite un browser. Innanzitutto provare a immettere un nome utente che non esiste nell'archivio dell'utente. Come illustrato nella figura 2, il controllo PassaggiwordRecovery Visualizza un messaggio che indica che le informazioni dell'utente non sono accessibile. Il testo del messaggio può essere personalizzato tramite il controllo [ `UserNameFailureText` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.usernamefailuretext.aspx).


[![Viene visualizzato un messaggio di errore se viene immesso un nome utente non valido](recovering-and-changing-passwords-vb/_static/image5.png)](recovering-and-changing-passwords-vb/_static/image4.png)

**Figura 2**: viene visualizzato un messaggio di errore se viene immesso un nome utente non valido ([fare clic per visualizzare l'immagine con dimensioni normali](recovering-and-changing-passwords-vb/_static/image6.png))


A questo punto immettere un nome utente. Usare il nome utente di un account nel sistema con un indirizzo di posta elettronica che è possibile accedere e rispondere a cui sicurezza è conoscere. Dopo aver immesso il nome utente e facendo clic su Invia, il controllo PassaggiwordRecovery consente di visualizzare la relativa visualizzazione domanda. Come con la visualizzazione nome utente, se si immette un'implementazione non corretta di rispondere il controllo PassaggiwordRecovery Visualizza un messaggio di errore (vedere la figura 3). Usare la [ `QuestionFailureText` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.questionfailuretext.aspx) per personalizzare questo messaggio di errore.


[![Viene visualizzato un messaggio di errore se l'utente immette una risposta di sicurezza non valido](recovering-and-changing-passwords-vb/_static/image8.png)](recovering-and-changing-passwords-vb/_static/image7.png)

**Figura 3**: viene visualizzato un messaggio di errore se l'utente immette una risposta di sicurezza non valido ([fare clic per visualizzare l'immagine con dimensioni normali](recovering-and-changing-passwords-vb/_static/image9.png))


Infine, immettere la risposta di sicurezza corrette e fare clic su Invia. Dietro le quinte, il controllo PassaggiwordRecovery genera una password casuale, viene assegnato all'account utente, invia un messaggio di posta elettronica per informare l'utente della nuova password (vedere la figura 4) e quindi Visualizza la visualizzazione operazione riuscita.


[![L'utente viene inviato un messaggio di posta elettronica con la nuova Password](recovering-and-changing-passwords-vb/_static/image11.png)](recovering-and-changing-passwords-vb/_static/image10.png)

**Figura 4**: l'utente viene inviato un messaggio di posta elettronica con la nuova Password ([fare clic per visualizzare l'immagine con dimensioni normali](recovering-and-changing-passwords-vb/_static/image12.png))


### <a name="customizing-the-email"></a>Personalizzare il messaggio di posta elettronica

Il messaggio di posta elettronica predefinito inviato dal controllo PassaggiwordRecovery è piuttosto pesante (vedere la figura 4). Il messaggio viene inviato dall'account specificato nella `<smtp>` dell'elemento `from` attributo il cui oggetto Password e il corpo di testo normale:

Tornare al sito e accedere con le informazioni seguenti.

Nome utente: *username*

Password: *password*

Questo messaggio può essere personalizzato a livello di codice tramite un gestore eventi per il controllo di PassaggiwordRecovery [ `SendingMail` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.sendingmail.aspx), o in modo dichiarativo mediante la [ `MailDefinition` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.maildefinition.aspx). Esaminiamo queste due opzioni.

Il `SendingMail` evento viene generato appena prima che il messaggio di posta elettronica viene inviato ed è la tua ultima occasione per modificare a livello di codice il messaggio di posta elettronica. Quando questo evento viene generato, il gestore dell'evento viene passato un oggetto di tipo [ `MailMessageEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.mailmessageeventargs.aspx), la cui `Message` proprietà contiene un riferimento al messaggio di posta elettronica in fase di invio.

Creare un gestore eventi per il `SendingMail` eventi e aggiungere il codice seguente, che a livello di codice aggiunge `webmaster@example.com` all'elenco di CC.

[!code-vb[Main](recovering-and-changing-passwords-vb/samples/sample2.vb)]

Messaggio di posta elettronica può essere configurato anche tramite metodi dichiarativi. Il PassaggiwordRecovery `MailDefinition` proprietà è un oggetto di tipo [ `MailDefinition` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.aspx). Il `MailDefinition` classe offre una vasta gamma di proprietà correlate al messaggio di posta elettronica, inclusi `From`, `CC`, `Priority`, `Subject`, `IsBodyHtml`, `BodyFileName`e così via. Per i principianti, impostare il [ `Subject` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.subject.aspx) per renderlo più descrittivo rispetto a quello utilizzato per impostazione predefinita (Password), ad esempio la password è stata reimpostata...

Per personalizzare il corpo del messaggio di posta elettronica che è necessario creare un file di modello di posta elettronica diverso che contiene il contenuto del corpo. Iniziare creando una nuova cartella nel sito Web denominato `EmailTemplates`. Successivamente, aggiungere un nuovo file di testo in questa cartella denominata `PasswordRecovery.txt` e aggiungere il contenuto seguente:

[!code-aspx[Main](recovering-and-changing-passwords-vb/samples/sample3.aspx)]

Si noti l'uso dei segnaposto `<%UserName%>` e `<%Password%>`. Il controllo PassaggiwordRecovery sostituisce automaticamente questi due segnaposto con nome utente e la password recuperata prima dell'invio messaggio di posta elettronica dell'utente.

Infine, scegliere il `MailDefinition`del [ `BodyFileName` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.bodyfilename.aspx) al modello di messaggio di posta elettronica abbiamo appena creato (`~/EmailTemplates/PasswordRecovery.txt`).

Dopo aver apportare modifiche a rivedere il `RecoverPassword.aspx` pagina e immettere la risposta di nome utente e la sicurezza. Si riceve un messaggio di posta elettronica simile a quello in figura 5 deve. Si noti che `webmaster@example.com` è stata CC sarebbe e che l'oggetto e corpo sono stati aggiornati.


[![L'oggetto, corpo e CC elenco sono stati aggiornati](recovering-and-changing-passwords-vb/_static/image14.png)](recovering-and-changing-passwords-vb/_static/image13.png)

**Figura 5**: il soggetto, corpo e CC elenco sono state aggiornate ([fare clic per visualizzare l'immagine con dimensioni normali](recovering-and-changing-passwords-vb/_static/image15.png))


Per inviare un messaggio di posta elettronica in formato HTML impostata [ `IsBodyHtml` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.isbodyhtml.aspx) su True (impostazione predefinita è False) e aggiornare il modello di messaggio di posta elettronica da includere HTML.

Il `MailDefinition` proprietà non è univoca per la classe PassaggiwordRecovery. Come si vedrà in passaggio 2, il controllo ChangePassword offre inoltre un `MailDefinition` proprietà. Inoltre, il controllo CreateUserWizard contiene questo tipo di proprietà che è possibile configurare per l'invio automatico di un messaggio di posta elettronica di benvenuto ai nuovi utenti.

> [!NOTE]
> Attualmente non sono presenti collegamenti nel riquadro di spostamento a sinistra per raggiungere il `RecoverPassword.aspx` pagina. Un utente potrebbe essere interessato solo a visitare questa pagina se Anna non è riuscita ad accedere correttamente al sito. Di conseguenza, aggiornare il `Login.aspx` per includere un collegamento alla pagina di `RecoverPassword.aspx` pagina.


### <a name="programmatically-resetting-a-users-password"></a>A livello di codice la reimpostazione Password di un utente

Quando la reimpostazione password di un utente di PassaggiwordRecovery le chiamate di controllo di `MembershipUser` dell'oggetto [ `ResetPassword` metodo](https://msdn.microsoft.com/library/system.web.security.membershipuser.resetpassword.aspx). Questo metodo presenta due overload:

- **[`ResetPassword`](https://msdn.microsoft.com/library/d94bdzz2.aspx)** -Consente di reimpostare la password dell'utente. Utilizzare questo overload se `RequiresQuestionAndAnswer` è False.
- **[`ResetPassword(securityAnswer)`](https://msdn.microsoft.com/library/d90zte4w.aspx)** -Consente di reimpostare solo se la password dell'utente fornito *securityAnswer* sia corretto. Utilizzare questo overload se `RequiresQuestionAndAnswer` è True.

Entrambi gli overload restituiscono la nuova password generata casualmente.

Come con gli altri metodi nel framework di appartenenza di `ResetPassword` delegati del metodo al provider configurato. Il `SqlMembershipProvider` richiama il `aspnet_Membership_ResetPassword` stored procedure, passando il nome dell'utente, la nuova password e la risposta per la password fornita, tra gli altri campi. La stored procedure assicura che la risposta segreta corrispondente e quindi aggiorna la password dell'utente.

Alcune note di implementazione di basso livello:

- Un utente bloccato non può reimpostare password. Tuttavia, può un utente non approvato. Si illustrerà la bloccato e approvato gli stati più dettagliatamente nella <a id="_msoanchor_3"> </a> [ *Unlocking e l'approvazione dell'utente* ](unlocking-and-approving-user-accounts-vb.md) esercitazione account.
- Se la risposta segreta non è corretta, viene incrementato numero tentativi di risposta segreta non riusciti dell'utente. Se un numero specificato di tentativi di risposta di sicurezza non valido si verifica all'interno di un intervallo di tempo specificato, l'utente è bloccato.

### <a name="a-word-on-how-the-random-passwords-are-generated"></a>Una parola nel modo in cui la password casuali generati

Le password generata casualmente mostrate nei messaggi di posta elettronica nelle figure 4 e 5 vengono create la classe di appartenenza [ `GeneratePassword` metodo](https://msdn.microsoft.com/library/system.web.security.membership.generatepassword.aspx). Questo metodo accetta due parametri di input di integer - *lunghezza* e *numberOfNonAlphanumericCharacters* - e restituisce una stringa almeno *lunghezza* alla durata con caratteri almeno *numberOfNonAlphanumericCharacters* numero di caratteri non alfanumerici. Quando questo metodo viene chiamato dall'interno di classi di appartenenza o controlli correlati all'accesso Web, vengono determinati i valori per questi due parametri nella configurazione di appartenenza `MinRequiredPasswordLength` e `MinRequiredNonalphanumericCharacters` proprietà che è impostati su 7 e 1, rispettivamente.

Il `GeneratePassword` metodo Usa un generatore di numeri casuali crittograficamente sicuro per garantire che non c'è alcuna differenza nelle quali caratteri casuali vengono selezionate. Inoltre, `GeneratePassword` è `Public`, vale a dire che è possibile usarlo direttamente dall'applicazione ASP.NET se è necessario generare stringhe casuali o le password.

> [!NOTE]
> Il `SqlMembershipProvider` classe genera sempre una password casuale lunga almeno 14 caratteri, pertanto se `MinRequiredPasswordLength` è inferiore a 14, il relativo valore viene ignorato.


## <a name="step-2-changing-passwords"></a>Passaggio 2: Modifica delle password

Le password generata casualmente sono difficili da ricordare. Prendere in considerazione la password illustrata nella figura 4: `WWGUZv(f2yM:Bd`. Provare a eseguire il commit che per la memoria. Inutile a dirsi, dopo che un utente viene inviato una password generata casualmente di questo tipo, è opportuno modificare la password in modo che più facili da ricordare.

Per creare un'interfaccia per un utente di modificare la password, usare il controllo ChangePassword. Molto, come il controllo PassaggiwordRecovery, il controllo ChangePassword costituito da due visualizzazioni: modifica della Password e il completamento. La visualizzazione di modifica della Password richiede all'utente le proprie password vecchi e nuovi. Al momento di fornire la vecchia password corrette e una nuova password che soddisfi i requisiti di carattere non alfanumerico e la lunghezza minima, il controllo ChangePassword Aggiorna la password dell'utente e visualizza l'esito positivo.

> [!NOTE]
> Controllo ChangePassword modifica la password dell'utente chiamando il `MembershipUser` dell'oggetto [ `ChangePassword` metodo](https://msdn.microsoft.com/library/system.web.security.membershipuser.changepassword.aspx). Il metodo ChangePassword accetta due `String` - parametri di input *oldPassword* e *newPassword*- e aggiorna l'account dell'utente con il *newPassword*, Supponendo che il parametro fornito *oldPassword* sia corretto.


Aprire il `ChangePassword.aspx` pagina e aggiungere un controllo ChangePassword alla pagina, denominarlo `ChangePwd`. A questo punto, la visualizzazione della struttura deve mostrare al cambiamento della Password (vedere la figura 6). Ad esempio con il controllo PassaggiwordRecovery, è possibile passare tra le visualizzazioni tramite Smart Tag del controllo. Inoltre, aspetti di queste visualizzazioni sono personalizzabili tramite le proprietà di stile vengono formattati diversi o convertendole in un modello.


[![Aggiungere un controllo ChangePassword alla pagina](recovering-and-changing-passwords-vb/_static/image17.png)](recovering-and-changing-passwords-vb/_static/image16.png)

**Figura 6**: aggiungere un controllo ChangePassword alla pagina ([fare clic per visualizzare l'immagine con dimensioni normali](recovering-and-changing-passwords-vb/_static/image18.png))


Controllo ChangePassword possibile aggiornare la password dell'utente attualmente connesso *o* la password dell'utente specificato, un altro. Come illustrato nella figura 6, la visualizzazione di modifica della Password predefinita esegue il rendering solo tre controlli TextBox: uno per la vecchia password e due per la nuova password. Questa interfaccia predefinita viene utilizzata per aggiornare la password dell'utente attualmente connesso.

Per usare il controllo ChangePassword per aggiornare la password di un altro utente, impostare il controllo [ `DisplayUserName` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.changepassword.displayusername.aspx) su True. Questa operazione aggiunge una quarta casella di testo alla pagina, richiesta di conferma per il nome utente dell'utente la cui password da modificare.

Impostazione `DisplayUserName` a True è utile se si desidera consentire agli utenti registrati out di modificare la password senza dover effettuare l'accesso. Personalmente, ritengo che non vi sono problemi con richiedere che l'utente all'account di accesso prima che consente di modificare la password. Pertanto, lasciare `DisplayUserName` impostato su False (predefinito). Nel prendere questa decisione, tuttavia, è essenzialmente stiamo ad eccezione agli utenti anonimi di raggiungere questa pagina. Aggiornare le regole di autorizzazione URL del sito in modo da impedire agli utenti anonimi di visitare `ChangePassword.aspx`. Se è necessario aggiornare la memoria nella sintassi delle regole di autorizzazione URL, vedere la <a id="_msoanchor_4"> </a> [ *autorizzazione basata su utente* ](../membership/user-based-authorization-vb.md) esercitazione.

> [!NOTE]
> Può sembrare che il `DisplayUserName` proprietà è utile per consentire agli amministratori di cambiare le password degli altri utenti. Tuttavia, anche quando `DisplayUserName` è impostata su True la vecchia password corretta deve essere nota e immesso. Si discuterà tecniche per consentire agli amministratori di cambiare le password degli utenti nel passaggio 3.


Visita il `ChangePassword.aspx` pagina tramite un browser e cambia la password. Si noti che se si immette una nuova password che non riesce a soddisfare i requisiti di carattere non alfanumerico specificati nella configurazione di appartenenza e la lunghezza della password viene visualizzato un messaggio di errore (vedere la figura 7).


[![Aggiungere un controllo ChangePassword alla pagina](recovering-and-changing-passwords-vb/_static/image20.png)](recovering-and-changing-passwords-vb/_static/image19.png)

**Figura 7**: aggiungere un controllo ChangePassword alla pagina ([fare clic per visualizzare l'immagine con dimensioni normali](recovering-and-changing-passwords-vb/_static/image21.png))


Al momento immettendo la vecchia password corrette e una valida nuova password, usato per l'accesso dell'utente di modifica della password e visualizzato visualizzazione operazione riuscita.

### <a name="sending-a-confirmation-email"></a>L'invio di un messaggio di posta elettronica di conferma

Per impostazione predefinita, il controllo ChangePassword non invia un messaggio di posta elettronica all'utente la cui password è stata appena aggiornata. Se si desidera inviare un messaggio di posta elettronica, è sufficiente configurare il controllo `MailDefinition` proprietà. È possibile configurare il controllo ChangePassword in modo che l'utente viene inviata un'e-mail in formato HTML che contiene la nuova password.

Iniziare creando un nuovo file nei `EmailTemplates` cartella denominata `ChangePassword.htm`. Aggiungere il markup seguente:

[!code-html[Main](recovering-and-changing-passwords-vb/samples/sample4.html)]

Successivamente, impostare il controllo di ChangePassword `MailDefinition` della proprietà `BodyFileName`, `IsBodyHtml`, e `Subject` proprietà da ~ / EmailTemplates/ChangePassword.htm True e la password è stato modificato!, rispettivamente.

Dopo aver apportato queste modifiche, visitare di nuovo la pagina e modificare nuovamente la password. Questa volta, il controllo ChangePassword invia un messaggio di posta elettronica personalizzato, in formato HTML all'indirizzo di posta elettronica dell'utente nel file (vedere la figura 8).


[![Un messaggio di posta elettronica per informare l'utente che Their Password è stata modificata](recovering-and-changing-passwords-vb/_static/image23.png)](recovering-and-changing-passwords-vb/_static/image22.png)

**Figura 8**: un messaggio di posta elettronica per informare l'utente che Their Password è stata modificata ([fare clic per visualizzare l'immagine con dimensioni normali](recovering-and-changing-passwords-vb/_static/image24.png))


## <a name="step-3-allowing-administrators-to-change-users-passwords"></a>Passaggio 3: Consentendo agli amministratori di modificare le password degli utenti

Una funzionalità comune in applicazioni che supportano gli account utente è la possibilità per un utente amministratore modificare le password di altri utenti. A volte questa funzionalità è necessaria perché il sistema non consente agli utenti di modificare la propria password. In tal caso, l'unico modo per un utente di ripristinare la password dimenticata sarà l'amministratore di assegnare loro una nuova password. Con i controlli PassaggiwordRecovery e ChangePassword, tuttavia, gli utenti amministratori necessitano non occupato autonomamente con la modifica delle password degli utenti, come gli utenti sono in grado di soddisfare questa autonomamente.

Ma cosa accade se il client prevede che gli utenti amministratori devono essere in grado di modificare le password degli altri utenti? Sfortunatamente, l'aggiunta di questa funzionalità può essere un po' di lavoro. Per modificare la password dell'utente, è necessario specificare la password vecchia e nuova per il `MembershipUser` dell'oggetto `ChangePassword` (metodo), ma un amministratore non deve avere conoscere la password dell'utente per modificarlo.

Una soluzione alternativa consiste innanzitutto reimpostare la password dell'utente e quindi modificarlo con la nuova password usando codice simile al seguente:

[!code-vb[Main](recovering-and-changing-passwords-vb/samples/sample5.vb)]

Questo codice inizia recuperando le informazioni sulle *username*, ovvero l'utente cui l'amministratore voglia modificare la password. Successivamente, il `ResetPassword` metodo viene richiamato, che assegna e l'utente di una nuova password casuale. Questa password generata casualmente restituita dal metodo e archiviata nella variabile `resetPwd`. Ora che si conosce la password dell'utente, è possibile modificarlo tramite una chiamata a `ChangePassword`.

Il problema è che questo codice funziona solo se la configurazione di sistema di appartenenza viene impostata in modo che `RequiresQuestionAndAnswer` è False. Se `RequiresQuestionAndAnswer` è True, come nel caso di applicazione, quindi il `ResetPassword` metodo è necessario passare la risposta di sicurezza, in caso contrario verrà generata un'eccezione.

Se il framework di appartenenza è configurato per richiedere una domanda di sicurezza e una risposta e ancora il client prevede che gli amministratori in grado di modificare le password degli utenti, sono disponibili tre opzioni:

- Generare le mani in aria e indicare i client che si tratta di un solo aspetto che non può essere eseguito.
- Impostare `RequiresQuestionAndAnswer` su False. Di conseguenza un'applicazione meno protetta. Si supponga che un utente nefandi ha ottenuto l'accesso alla posta in arrivo messaggio di posta elettronica di un altro utente. Probabilmente l'utente compromessi uscito dalla propria scrivania per andare a pranzo e non blocca le workstation, oppure forse accedere alla posta elettronica da un terminale pubblico e non è stato disconnesso. In entrambi i casi, l'utente dannosa può visitare la `RecoverPassword.aspx` pagina e immettere il nome dell'utente. Il sistema invierà quindi la password recuperata senza chiedere conferma per la risposta di sicurezza.
- Ignorare il livello di astrazione creato dal framework di appartenenza e funzionano direttamente con il database di SQL Server. Lo schema di appartenenza include una stored procedure denominata `aspnet_Membership_SetPassword` che imposta la password dell'utente e non richiede la risposta di sicurezza o la password precedente per eseguire l'attività.

Nessuna di queste opzioni sono particolarmente interessante, ma che è come il ciclo di vita di uno sviluppatore è a volte.

E lo seleziono implementato il terzo approccio, la scrittura di codice che consente di ignorare il `Membership` e `MembershipUser` classi e direttamente su cui opera il `SecurityTutorials` database.

> [!NOTE]
> La possibilità di lavorare direttamente con il database, l'incapsulamento fornito dal framework di appartenenza è esplosi. Consente di associare a questa decisione di `SqlMembershipProvider`, rendendo il nostro codice meno portabile. Inoltre, questo codice potrebbe non funzionare come previsto nelle future versioni di ASP.NET, se viene modificato lo schema di appartenenza. Questo approccio è una soluzione alternativa e, come la maggior parte delle soluzioni alternative, non è un esempio di procedure consigliate.


Il codice presenta alcuni bit graficamente meno elegante ed è abbastanza lungo. Pertanto, non desidero creare confusione in questa esercitazione con un'analisi approfondita di esso. Se è interessati a ulteriori informazioni, scaricare il codice per questa esercitazione e visita il `~/Administration/ManageUsers.aspx` pagina. Questa pagina, che è stato creato nel <a id="_msoanchor_5"> </a> [esercitazione precedente](building-an-interface-to-select-one-user-account-from-many-vb.md), elenco degli utenti. È già stato aggiornato il controllo GridView per includere un collegamento per il `UserInformation.aspx` pagina, passando il nome utente dell'utente selezionato tramite la stringa di query. Il `UserInformation.aspx` pagina vengono visualizzate informazioni sull'utente selezionato e caselle di testo per modificare la password (vedere la figura 9).

Dopo aver immesso la nuova password, conferma nella seconda casella di testo e facendo clic sul pulsante di aggiornamento utente, un postback previsioni e `aspnet_Membership_SetPassword` stored procedure viene richiamata, aggiornare la password dell'utente. Ti suggeriamo coloro che interessati a questa funzionalità per acquisire familiarità con il codice e provare a estendere la funzionalità per includere l'invio di un messaggio di posta elettronica all'utente la cui password è stata modificata.


[![Un amministratore può modificare la Password dell'utente](recovering-and-changing-passwords-vb/_static/image26.png)](recovering-and-changing-passwords-vb/_static/image25.png)

**Figura 9**: un amministratore può modificare Password la ([fare clic per visualizzare l'immagine con dimensioni normali](recovering-and-changing-passwords-vb/_static/image27.png))


> [!NOTE]
> Il `UserInformation.aspx` pagina attualmente funziona solo se il framework di appartenenza è configurato per archiviare le password in formato chiaro o Hashed. Manca il codice per crittografare la nuova password, anche se viene visualizzato un messaggio per aggiungere questa funzionalità. Il modo in cui è consigliabile aggiungere il codice necessario consiste nell'usare un decompilatore, ad esempio [Reflector](http://www.aisto.com/roeder/dotnet/) per esaminare il codice sorgente per i metodi in .NET Framework; iniziare esaminando le `SqlMembershipProvider` della classe `ChangePassword` (metodo). Si tratta della tecnica che consente di scrivere il codice per la creazione di un hash della password.


## <a name="summary"></a>Riepilogo

ASP.NET offre due controlli per consentire agli utenti di gestire la propria password. Il controllo PassaggiwordRecovery è utile per coloro che hanno dimenticato la propria password. A seconda della configurazione del framework di appartenenza, l'utente che viene inviato tramite posta elettronica la password esistente o una nuova password generata casualmente. Controllo ChangePassword consente a un utente di aggiornare la password.

Analogamente ai controlli di accesso e CreateUserWizard, i controlli PassaggiwordRecovery e ChangePassword eseguire il rendering di un'interfaccia utente avanzata senza dover scrivere il Benché minimo frammento di markup dichiarativo o della riga di codice. Se l'interfaccia utente predefinita non soddisfa le proprie esigenze, è possibile personalizzare tramite un'ampia gamma di proprietà di stile. In alternativa, le interfacce dei controlli possono essere convertite in modelli, per un livello ancora più preciso di controllo. Dietro le quinte questi controlli usano l'API di appartenenza, richiama il `MembershipUser` dell'oggetto `ResetPassword` e `ChangePassword` metodi.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Controllo ChangePassword guide introduttive](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/changepassword.aspx)
- [Guide introduttive PassaggiwordRecovery controllo](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/passwordrecovery.aspx)
- [L'invio di posta elettronica in ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)
- [`System.Net.Mail` Domande frequenti](http://www.systemnetmail.com/)

### <a name="about-the-author"></a>Informazioni sull'autore

Scott Mitchell, autore di più libri e fondatore di 4GuysFromRolla.com, ha lavorato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola  *[Sams Teach Yourself ASP.NET 2.0 in 24 ore](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. È possibile contattarlo Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) o il suo blog all'indirizzo [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. I revisori per questa esercitazione includono Michael Emmings e Suchi Banerjee. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami un messaggio nella [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](building-an-interface-to-select-one-user-account-from-many-vb.md)
> [Successivo](unlocking-and-approving-user-accounts-vb.md)
