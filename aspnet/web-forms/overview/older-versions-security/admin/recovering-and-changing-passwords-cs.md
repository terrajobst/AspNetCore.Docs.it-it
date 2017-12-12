---
uid: web-forms/overview/older-versions-security/admin/recovering-and-changing-passwords-cs
title: Recupero e modifica delle password (c#) | Documenti Microsoft
author: rick-anderson
description: ASP.NET include due controlli Web per facilitare il ripristino e la modifica delle password. Il controllo PasswordRecovery consente un visitatore ripristinare il suo pa perso...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2008
ms.topic: article
ms.assetid: 19c4d042-4e34-4b44-9f1d-6bf2253ba366
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/admin/recovering-and-changing-passwords-cs
msc.type: authoredcontent
ms.openlocfilehash: ef10d5140073d28589c0be80a3a3bb4d3a554e35
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="recovering-and-changing-passwords-c"></a>Recupero e modifica delle password (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare codice](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/CS.13.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial13_ChangingPasswords_cs.pdf)

> ASP.NET include due controlli Web per facilitare il ripristino e la modifica delle password. Il controllo PasswordRecovery consente un visitatore ripristinare la password persa. Il controllo ChangePassword consente all'utente di aggiornare la password. Come gli altri controlli Web relative all'accesso abbiamo visto in tutta questa serie di esercitazioni, PasswordRecovery e ChangePassword controlli vengono utilizzati con il framework di appartenenza in background di reimpostare o modificare le password degli utenti.


## <a name="introduction"></a>Introduzione

Tra i siti Web per my bank, società, telefonata, account di posta elettronica e i portali web personalizzati, come la maggior parte delle persone, ho decine di password diverse da ricordare. Con così tante credenziali per memorizzare questi giorni, non è raro che gli utenti dimenticano la password. Per evitare ciò, è necessario includere un modo per un utente di ripristinare la password siti Web che offrono gli account utente. In genere, questo processo comporta la generazione di una nuova password casuale e inviarlo all'indirizzo di posta elettronica dell'utente nel file. Dopo aver ricevuto la nuova password maggior parte degli utenti ritorna al sito e cambiare la password da quello generato in modo casuale a uno più facili da ricordare.

ASP.NET include due controlli Web per facilitare il ripristino e la modifica delle password. Il controllo PasswordRecovery consente un visitatore ripristinare la password persa. Il controllo ChangePassword consente all'utente di aggiornare la password. Come gli altri controlli Web relative all'accesso abbiamo visto in tutta questa serie di esercitazioni, PasswordRecovery e ChangePassword controlli vengono utilizzati con il framework di appartenenza in background di reimpostare o modificare le password degli utenti.

In questa esercitazione verrà esaminato tramite questi due controlli. Verrà inoltre spiegato come modificare a livello di programmazione e reimpostare la password dell'utente tramite il `MembershipUser` della classe `ChangePassword` e `ResetPassword` metodi.

## <a name="step-1-helping-users-recover-lost-passwords"></a>Passaggio 1: Aiutare gli utenti recupero password perdita

Tutti i siti Web che supportano gli account utente necessario fornire agli utenti un meccanismo per il recupero delle password dimenticata. Buone notizie sono che l'implementazione di queste funzionalità in ASP.NET è molto semplice grazie a controllo PasswordRecovery Web. Il controllo PasswordRecovery esegue il rendering di un'interfaccia che richiede all'utente per il proprio nome utente e, se necessario, la risposta alla domanda di sicurezza relativi. Quindi messaggi di posta elettronica all'utente la password.

> [!NOTE]
> Poiché i messaggi di posta elettronica vengono trasmessi nella rete in testo normale esistono rischi di sicurezza necessarie per l'invio di una password tramite posta elettronica.


Il controllo PasswordRecovery è costituito da tre visualizzazioni:

- **Nome utente** -richiede l'inserimento per il proprio nome utente. Si tratta della visualizzazione iniziale.
- **Domanda**-Visualizza domande di nome utente e la sicurezza dell'utente come testo, insieme a una casella di testo per l'utente di immettere la risposta alla sua domanda di sicurezza.
- **Esito positivo**-viene visualizzato un messaggio che informa l'utente che la password è stata inviata.

Visualizzare le viste e le azioni eseguite dal controllo PasswordRecovery dipendono le seguenti impostazioni di configurazione di appartenenza:

- `RequiresQuestionAndAnswer`
- `EnablePasswordRetrieval`
- `EnablePasswordReset`

Il framework di appartenenza `RequiresQuestionAndAnswer` impostazione indica se gli utenti devono specificare una domanda di sicurezza e una risposta durante la registrazione di un account. Come accennato nel <a id="_msoanchor_1"> </a> [ *creazione degli account utente* ](../membership/creating-user-accounts-cs.md) dell'esercitazione, se `RequiresQuestionAndAnswer` è True (impostazione predefinita), l'interfaccia del CreateUserWizard includerà casella di testo controlli per la domanda di sicurezza e la risposta; il nuovo utente Se `RequiresQuestionAndAnswer` è False, tali informazioni non vengono raccolti. Analogamente, se `RequiresQuestionAndAnswer` è True, il controllo PasswordRecovery Visualizza la domanda visualizzare dopo che l'utente ha immesso il proprio nome utente; la password viene recuperata solo se l'utente immette la risposta di sicurezza corrette. Se `RequiresQuestionAndAnswer` è False, tuttavia, si sposta il controllo PasswordRecovery direttamente dalla visualizzazione di nome utente nella visualizzazione di esito positivo.

Dopo che l'utente ha fornito il suo nome utente - o una risposta il suo nome utente e la sicurezza, se `RequiresQuestionAndAnswer` è True - PasswordRecovery messaggi di posta elettronica all'utente la password. Se il `EnablePasswordRetrieval` opzione è impostata su True, quindi l'utente viene inviato tramite posta elettronica la password corrente. Se è impostato su False e `EnablePasswordReset` è impostata su True, il controllo PasswordRecovery genera una nuova password casuale per l'utente e la nuova password a tali messaggi di posta elettronica. Se entrambi `EnablePasswordRetrieval` e `EnablePasswordReset` sono False, il controllo PasswordRecovery genera un'eccezione.

> [!NOTE]
> Tenere presente che il `SqlMembershipProvider` archivia le password degli utenti in uno dei tre formati: Clear, Hashed (impostazione predefinita) o crittografato. Il meccanismo di archiviazione utilizzato dipende dalle impostazioni di configurazione di appartenenza; l'applicazione demo utilizza il formato della password Hashed. Quando si utilizza il formato della password Hashed il `EnablePasswordRetrieval` opzione deve essere impostata su False, perché il sistema non può determinare la password dell'utente effettivo della versione hash archiviato nel database.


La figura 1 illustra come interfaccia e il comportamento di PasswordRecovery varia a seconda della configurazione di appartenenza.


[![Il RequiresQuestionAndAnswer EnablePasswordRetrieval ed EnablePasswordReset influenzare l'aspetto e il comportamento del controllo PasswordRecovery](recovering-and-changing-passwords-cs/_static/image2.png)](recovering-and-changing-passwords-cs/_static/image1.png)

**Figura 1**: il `RequiresQuestionAndAnswer`, `EnablePasswordRetrieval`, e `EnablePasswordReset` influenzare l'aspetto e il comportamento del controllo PasswordRecovery ([fare clic per visualizzare l'immagine ingrandita](recovering-and-changing-passwords-cs/_static/image3.png))


> [!NOTE]
> Nel <a id="_msoanchor_2"> </a> [ *creazione dello Schema di appartenenza in SQL Server* ](../membership/creating-the-membership-schema-in-sql-server-cs.md) esercitazione è stato configurato il provider di appartenenze impostando `RequiresQuestionAndAnswer` su True, `EnablePasswordRetrieval` per False, e `EnablePasswordReset` su True.


### <a name="using-the-passwordrecovery-control"></a>Tramite il controllo PasswordRecovery

Esaminiamo il controllo PasswordRecovery utilizzato in una pagina ASP.NET. Aprire `RecoverPassword.aspx` e trascinare e rilasciare un controllo PasswordRecovery dalla casella degli strumenti nella finestra di progettazione, impostare il relativo `ID` a `RecoverPwd`. Ad esempio i controlli di accesso e CreateUserWizard Web, le visualizzazioni del controllo PasswordRecovery eseguire il rendering di un'interfaccia composta completa che include le etichette, caselle di testo, pulsanti e i controlli di convalida. È possibile personalizzare l'aspetto delle viste tramite le proprietà di stile del controllo o convertendo le viste in modelli. Lasciare questo campo come esercizio per il lettore di interesse.

Quando un utente visita questa pagina e verrà immettere il proprio nome utente e fare clic sul pulsante Invia. Poiché è stata impostata la `RequiresQuestionAndAnswer` proprietà su True nelle impostazioni di configurazione appartenenza, PasswordRecovery controllo, quindi visualizzare la visualizzazione della domanda. Dopo che l'utente immette la risposta di sicurezza corrette e fa clic su Invia, il controllo PasswordRecovery aggiornare la password dell'utente con uno generato casualmente e la password per l'indirizzo di posta elettronica nel file di posta elettronica. Tutto questo è stato possibile senza la necessità di scrivere una singola riga di codice!

Prima di testare questa pagina, è una parte finale della configurazione da tendono a: è necessario specificare le impostazioni di recapito di posta elettronica in `Web.config`. Il controllo PasswordRecovery si basa su queste impostazioni per l'invio di posta elettronica.

La configurazione di recapito di posta elettronica viene specificata tramite il [ `<system.net>` elemento](https://msdn.microsoft.com/en-us/library/6484zdc1.aspx)del [ `<mailSettings>` elemento](https://msdn.microsoft.com/en-us/library/w355a94k.aspx). Utilizzare il [ `<smtp>` elemento](https://msdn.microsoft.com/en-us/library/ms164240.aspx) per indicare il metodo di recapito e il valore predefinito dall'indirizzo. Il markup seguente configura le impostazioni di posta elettronica per l'utilizzo di un server SMTP di rete denominato `smtp.example.com` sulla porta 25 e con le credenziali di nome utente e password del nome utente e password.

> [!NOTE]
> `<system.net>`è un elemento figlio della radice `<configuration>` elemento e pari livello `<system.web>`. Pertanto, non inserire il `<system.net>` elemento all'interno di `<system.web>` elemento; invece inserirlo allo stesso livello.


[!code-xml[Main](recovering-and-changing-passwords-cs/samples/sample1.xml)]

Oltre a utilizzare un server SMTP nella rete, è possibile specificare una directory di prelievo in cui devono essere depositati messaggi di posta elettronica da inviare.

Dopo aver configurato le impostazioni SMTP, visitare il `RecoverPassword.aspx` pagina tramite un browser. Provare a immettere un nome utente che non esiste nell'archivio dell'utente. Come illustrato nella figura 2, il controllo PasswordRecovery Visualizza un messaggio che indica che le informazioni utente non è accessibile. Il testo del messaggio può essere personalizzato tramite il controllo [ `UserNameFailureText` proprietà](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.passwordrecovery.usernamefailuretext.aspx).


[![Viene visualizzato un messaggio di errore se viene immesso un nome utente non valido](recovering-and-changing-passwords-cs/_static/image5.png)](recovering-and-changing-passwords-cs/_static/image4.png)

**Figura 2**: viene visualizzato un messaggio di errore se viene immesso un nome utente non valido ([fare clic per visualizzare l'immagine ingrandita](recovering-and-changing-passwords-cs/_static/image6.png))


Immettere un nome utente. Utilizzare il nome utente di un account nel sistema con un indirizzo di posta elettronica che è possibile accedere e rispondere a cui sicurezza è noto. Dopo aver immesso il nome utente e facendo clic su Invia, il controllo PasswordRecovery consente di visualizzare la visualizzazione della domanda. Come con la visualizzazione del nome utente, se si immette un'implementazione non corretta risponde il controllo PasswordRecovery Visualizza un messaggio di errore (vedere la figura 3). Utilizzare il [ `QuestionFailureText` proprietà](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.passwordrecovery.questionfailuretext.aspx) per personalizzare questo messaggio di errore.


[![Se l'utente immette una risposta di sicurezza non valida, viene visualizzato un messaggio di errore](recovering-and-changing-passwords-cs/_static/image8.png)](recovering-and-changing-passwords-cs/_static/image7.png)

**Figura 3**: viene visualizzato un messaggio di errore se l'utente immette una risposta di sicurezza non valida ([fare clic per visualizzare l'immagine ingrandita](recovering-and-changing-passwords-cs/_static/image9.png))


Infine, immettere la risposta di sicurezza corrette e fare clic su Invia. Dietro le quinte, il controllo PasswordRecovery genera una password casuale, viene assegnato all'account utente, invia un messaggio di posta elettronica per informare l'utente la nuova password (vedere la figura 4) e quindi Visualizza l'esito positivo.


[![L'utente viene inviato un messaggio di posta elettronica con His nuova Password](recovering-and-changing-passwords-cs/_static/image11.png)](recovering-and-changing-passwords-cs/_static/image10.png)

**Figura 4**: l'utente viene inviato un messaggio di posta elettronica con una nuova Password His ([fare clic per visualizzare l'immagine ingrandita](recovering-and-changing-passwords-cs/_static/image12.png))


### <a name="customizing-the-email"></a>Personalizzazione di messaggio di posta elettronica

Il messaggio di posta elettronica predefinito inviato dal controllo PasswordRecovery è piuttosto monotono (vedere la figura 4). Il messaggio viene inviato dall'account specificato nella `<smtp>` dell'elemento `from` attributo con l'oggetto Password e il corpo di testo normale:

Tornare al sito e accedere con le informazioni seguenti.

Nome utente: *nome utente*

password: *password*

Questo messaggio può essere personalizzato a livello di codice tramite un gestore eventi per il controllo PasswordRecovery [ `SendingMail` evento](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.passwordrecovery.sendingmail.aspx), o in modo dichiarativo mediante la [ `MailDefinition` proprietà](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.passwordrecovery.maildefinition.aspx). Vediamo entrambe queste opzioni.

Il `SendingMail` evento viene generato immediatamente prima il messaggio di posta elettronica viene inviato ed è l'ultima possibilità di modificare a livello di codice il messaggio di posta elettronica. Quando viene generato questo evento, il gestore dell'evento viene passato un oggetto di tipo [ `MailMessageEventArgs` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.mailmessageeventargs.aspx), il cui `Message` proprietà contiene un riferimento al messaggio di posta elettronica per essere inviato.

Creare un gestore eventi per il `SendingMail` eventi e aggiungere il codice seguente, che a livello di codice aggiunge `webmaster@example.com` all'elenco di CC.

[!code-csharp[Main](recovering-and-changing-passwords-cs/samples/sample2.cs)]

Messaggio di posta elettronica può inoltre essere configurato tramite dichiarativi. Il PasswordRecovery `MailDefinition` proprietà è un oggetto di tipo [ `MailDefinition` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.maildefinition.aspx). Il `MailDefinition` classe offre una serie di proprietà correlate al messaggio di posta elettronica, tra cui `From`, `CC`, `Priority`, `Subject`, `IsBodyHtml`, `BodyFileName`e altri. Per iniziare, impostare il [ `Subject` proprietà](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.maildefinition.subject.aspx) per renderlo più descrittivo rispetto a quello utilizzato per impostazione predefinita (Password), ad esempio la password è stata reimpostata...

Per personalizzare il corpo del messaggio di posta elettronica, che è necessario creare un file di modello di posta elettronica separato che il contenuto del corpo. Iniziare creando una nuova cartella nel sito Web denominato `EmailTemplates`. Successivamente, aggiungere un nuovo file di testo in questa cartella denominata `PasswordRecovery.txt` e aggiungere il seguente contenuto:

[!code-aspx[Main](recovering-and-changing-passwords-cs/samples/sample3.aspx)]

Si noti l'utilizzo dei segnaposto `<%UserName%>` e `<%Password%>`. Il controllo PasswordRecovery sostituisce automaticamente questi due segnaposto con nome utente e password recuperate prima dell'invio messaggio di posta elettronica dell'utente.

Infine, scegliere il `MailDefinition`del [ `BodyFileName` proprietà](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.maildefinition.bodyfilename.aspx) al modello di posta elettronica appena creato (`~/EmailTemplates/PasswordRecovery.txt`).

Dopo avere apportare queste modifiche rivedere il `RecoverPassword.aspx` pagina e immettere la risposta di nome utente e la sicurezza. Viene visualizzato un messaggio di posta elettronica simile a quello in figura 5 deve. Si noti che `webmaster@example.com` è stata CC sarebbe e che l'oggetto e corpo sono state aggiornate.


[![Elenco CC, oggetto e corpo sono stati aggiornati.](recovering-and-changing-passwords-cs/_static/image14.png)](recovering-and-changing-passwords-cs/_static/image13.png)

**Figura 5**: del soggetto, corpo e CC elenco sono state aggiornate ([fare clic per visualizzare l'immagine ingrandita](recovering-and-changing-passwords-cs/_static/image15.png))


Per inviare un messaggio di posta elettronica in formato HTML impostare [ `IsBodyHtml` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.maildefinition.isbodyhtml.aspx) su True (il valore predefinito è False) e il modello di posta elettronica di aggiornamento per includere HTML.

Il `MailDefinition` proprietà non è univoca per la classe PasswordRecovery. Come verrà illustrato nel passaggio 2, il controllo ChangePassword offre inoltre un `MailDefinition` proprietà. Inoltre, il controllo CreateUserWizard include tale proprietà che è possibile configurare per l'invio automatico di un messaggio di posta elettronica di benvenuto ai nuovi utenti.

> [!NOTE]
> Attualmente non esistono collegamenti nel riquadro di spostamento a sinistra per raggiungere il `RecoverPassword.aspx` pagina. Un utente potrebbe essere interessato solo a visitare questa pagina, se l'utente è stato in grado di accedere al sito. Di conseguenza, aggiornare il `Login.aspx` pagina per includere un collegamento per il `RecoverPassword.aspx` pagina.


### <a name="programmatically-resetting-a-users-password"></a>A livello di codice di reimpostazione Password di un utente

Reimpostazione password di un utente PasswordRecovery quando il controllo di chiamate di `MembershipUser` dell'oggetto [ `ResetPassword` metodo](https://msdn.microsoft.com/en-us/library/system.web.security.membershipuser.resetpassword.aspx). Questo metodo dispone di due overload:

- **[`ResetPassword`](https://msdn.microsoft.com/en-us/library/d94bdzz2.aspx)**-Reimposta una password. Utilizzare questo overload se `RequiresQuestionAndAnswer` è False.
- **[`ResetPassword(securityAnswer)`](https://msdn.microsoft.com/en-us/library/d90zte4w.aspx)**-Reimposta fornito se solo la password di un utente *securityAnswer* sia corretto. Utilizzare questo overload se `RequiresQuestionAndAnswer` è True.

Entrambi gli overload restituiscono la nuova password generata casualmente.

Come con gli altri metodi di Framework, l'appartenenza di `ResetPassword` delegati del metodo per il provider configurato. Il `SqlMembershipProvider` richiama il `aspnet_Membership_ResetPassword` stored procedure, passando il nome dell'utente, la nuova password e la risposta per la password fornita, tra gli altri campi. La stored procedure assicura che la risposta per la password corrisponda e che quindi aggiorna la password dell'utente.

Un paio di note di implementazione di basso livello:

- Un utente bloccato non può reimpostare la password. Tuttavia, un utente non approvato può. Si parlerà di bloccato e approvato stati in dettaglio nel <a id="_msoanchor_3"> </a> [ *Unlocking e approvazione utente* ](unlocking-and-approving-user-accounts-cs.md) esercitazione account.
- Se la risposta segreta non è corretta, viene incrementato numero tentativi di risposta segreta non riusciti dell'utente. In caso di un numero specificato di tentativi di risposta di sicurezza non valido all'interno di un intervallo di tempo specificato, l'utente è bloccato.

### <a name="a-word-on-how-the-random-passwords-are-generated"></a>Una parola in modo la password casuali generati

Le password generata casualmente visualizzate nei messaggi di posta elettronica nelle figure 4 e 5 vengono create la classe di appartenenza [ `GeneratePassword` metodo](https://msdn.microsoft.com/en-us/library/system.web.security.membership.generatepassword.aspx). Questo metodo accetta due parametri di input di integer - *lunghezza* e *numberOfNonAlphanumericCharacters* - e restituisce una stringa almeno *lunghezza* alla long con caratteri almeno *numberOfNonAlphanumericCharacters* numero di caratteri non alfanumerici. Quando questo metodo viene chiamato da classi di appartenenza o controlli Web relative all'accesso, i valori per questi due parametri vengono determinati tramite la configurazione di appartenenza `MinRequiredPasswordLength` e `MinRequiredNonalphanumericCharacters` le proprietà che è impostati su 7 e 1, rispettivamente.

Il `GeneratePassword` metodo utilizza un generatore di numeri casuali di crittografia avanzato per verificare che non sia presente alcun distorsione in quali caratteri casuali vengono selezionate. Inoltre, `GeneratePassword` è `public`, vale a dire che è possibile utilizzarlo direttamente dall'applicazione ASP.NET se è necessario generare stringhe casuali o password.

> [!NOTE]
> Il `SqlMembershipProvider` classe genera sempre una password casuale almeno 14 caratteri, pertanto se `MinRequiredPasswordLength` è inferiore a 14, il relativo valore viene ignorato.


## <a name="step-2-changing-passwords"></a>Passaggio 2: Modifica delle password

Le password generata casualmente sono difficili da ricordare. Prendere in considerazione la password illustrata nella figura 4: `WWGUZv(f2yM:Bd`. Provare a eseguire il commit che alla memoria! Inutile a dirsi, dopo che un utente viene inviato una password generata casualmente di questo tipo, e sarà possibile modificare la password in modo che più facili da ricordare.

Utilizzare il controllo ChangePassword per creare un'interfaccia per un utente di modificare la password. Molto simile al controllo PasswordRecovery, controllo ChangePassword costituito da due visualizzazioni: modifica della Password e il completamento. La visualizzazione Cambia Password richiede all'utente per le password precedenti e nuove. Al momento di fornire la vecchia password corretta e una nuova password che soddisfi i requisiti di carattere non alfanumerico e la lunghezza minima, il controllo ChangePassword Aggiorna la password dell'utente e la visualizzazione di esito positivo.

> [!NOTE]
> Il controllo ChangePassword modifica la password dell'utente richiamando il `MembershipUser` dell'oggetto [ `ChangePassword` metodo](https://msdn.microsoft.com/en-us/library/system.web.security.membershipuser.changepassword.aspx). Il metodo ChangePassword accetta due `string` - parametri di input *oldPassword* e *newPassword*- e aggiorna l'account dell'utente con il *newPassword*, Supponendo che il parametro fornito *oldPassword* sia corretto.


Aprire il `ChangePassword.aspx` pagina e aggiungere un controllo ChangePassword alla pagina, denominarla `ChangePwd`. A questo punto, l'area di progettazione deve visualizzare la modifica di Password (vedere la figura 6). Ad esempio con il controllo PasswordRecovery, è possibile passare tra le visualizzazioni tramite Smart Tag del controllo. Inoltre, gli aspetti di queste visualizzazioni sono personalizzabili tramite le proprietà di stile diverse o convertendole in un modello di.


[![Aggiungere un controllo ChangePassword alla pagina](recovering-and-changing-passwords-cs/_static/image17.png)](recovering-and-changing-passwords-cs/_static/image16.png)

**Figura 6**: aggiungere un controllo ChangePassword ([fare clic per visualizzare l'immagine ingrandita](recovering-and-changing-passwords-cs/_static/image18.png))


Il controllo ChangePassword può aggiornare la password dell'utente attualmente connesso *o* la password di un altro utente specificato. Come illustrato nella figura 6, la visualizzazione di modifica della Password predefinita esegue il rendering solo tre controlli TextBox: uno per la vecchia password e due per la nuova password. Questa interfaccia predefinita viene utilizzata per aggiornare la password dell'utente attualmente connesso.

Per utilizzare il controllo ChangePassword per aggiornare la password dell'utente, impostare il controllo [ `DisplayUserName` proprietà](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.changepassword.displayusername.aspx) su True. Questa operazione aggiunge una quarta casella di testo alla pagina, richiedere il nome utente dell'utente di cui modificare la password.

Impostazione `DisplayUserName` a True è utile se si desidera che un utente connesso out di modificare la password senza dover accedere. Personalmente, ritengo che non c'è niente di problemi che richiedono un utente all'account di accesso prima di consentire di modificare la password. Pertanto, lasciare `DisplayUserName` impostato su False (predefinito). Prendere tale decisione, tuttavia, è essenzialmente stiamo impedendo agli utenti anonimi di accedere a questa pagina. Aggiornare le regole di autorizzazione URL del sito in modo da negare agli utenti anonimi da visitare `ChangePassword.aspx`. Se è necessario aggiornare la memoria nella sintassi delle regole di autorizzazione URL, fare riferimento in futuro il <a id="_msoanchor_4"> </a> [ *autorizzazione basata sull'utente* ](../membership/user-based-authorization-cs.md) esercitazione.

> [!NOTE]
> Potrebbe sembrare che il `DisplayUserName` proprietà è utile per consentire agli amministratori di modificare le password degli altri utenti. Tuttavia, anche quando `DisplayUserName` è impostata su True, la vecchia password corretta deve essere nota e immessi. Verranno presentati le tecniche per consentire agli amministratori di modificare le password degli utenti nel passaggio 3.


Visitare il `ChangePassword.aspx` pagina tramite un browser e cambia la password. Si noti che viene visualizzato un messaggio di errore se si immette una nuova password che non riesce a soddisfare i requisiti di carattere non alfanumerico specificati nella configurazione di appartenenza e di lunghezza della password (vedere la figura 7).


[![Aggiungere un controllo ChangePassword alla pagina](recovering-and-changing-passwords-cs/_static/image20.png)](recovering-and-changing-passwords-cs/_static/image19.png)

**Figura 7**: aggiungere un controllo ChangePassword ([fare clic per visualizzare l'immagine ingrandita](recovering-and-changing-passwords-cs/_static/image21.png))


Al momento immettendo la vecchia password corretta e una password di nuovo valida, usato per l'accesso dell'utente viene modificata la password e la visualizzazione di esito positivo.

### <a name="sending-a-confirmation-email"></a>L'invio di un messaggio di posta elettronica di conferma

Per impostazione predefinita, il controllo ChangePassword non invia un messaggio di posta elettronica all'utente la cui password è stata appena aggiornata. Se si desidera inviare un messaggio di posta elettronica, semplicemente configurare il controllo `MailDefinition` proprietà. Configurazione del controllo ChangePassword in modo che l'utente viene inviato un messaggio in formato HTML che contiene la nuova password.

Iniziare creando un nuovo file di `EmailTemplates` cartella denominata `ChangePassword.htm`. Aggiungere il markup seguente:

[!code-html[Main](recovering-and-changing-passwords-cs/samples/sample4.html)]

Successivamente, impostare il controllo di ChangePassword `MailDefinition` della proprietà `BodyFileName`, `IsBodyHtml`, e `Subject` proprietà ~ / EmailTemplates/ChangePassword.htm, True e la password è stata modificata!, rispettivamente.

Dopo aver apportato queste modifiche, rivedere la pagina e modificare di nuovo la password. Questa volta, il controllo ChangePassword invia un messaggio di posta elettronica personalizzato, in formato HTML all'indirizzo di posta elettronica dell'utente nel file (vedere la figura 8).


[![Un messaggio di posta elettronica indica la Password l'utente che è stato modificato](recovering-and-changing-passwords-cs/_static/image23.png)](recovering-and-changing-passwords-cs/_static/image22.png)

**Figura 8**: un messaggio di posta elettronica informa l'utente che Their Password è stata modificata ([fare clic per visualizzare l'immagine ingrandita](recovering-and-changing-passwords-cs/_static/image24.png))


## <a name="step-3-allowing-administrators-to-change-users-passwords"></a>Passaggio 3: Consentendo agli amministratori di modificare le password degli utenti

Una funzionalità comune in applicazioni che supportano gli account utente è la possibilità per un utente amministratore modificare le password degli altri utenti. A volte questa funzionalità è necessaria perché il sistema non consente agli utenti di modificare la propria password. In tal caso, per l'amministratore di assegnare una nuova password può essere l'unico modo per un utente di recuperare la password dimenticata. Con i controlli PasswordRecovery e ChangePassword, tuttavia, gli utenti amministratori necessitano non occupato autonomamente con la modifica delle password degli utenti, come gli utenti sono in grado di eseguire questo autonomamente.

Ma cosa accade se il client prevede che gli utenti amministratori devono essere in grado di modificare le password degli altri utenti? Purtroppo, l'aggiunta di questa funzionalità può essere un po' di lavoro. Per modificare una password, è necessario specificare entrambe le password per il `MembershipUser` dell'oggetto `ChangePassword` (metodo), ma un amministratore non deve avere conoscere password la per modificarlo.

Una soluzione alternativa consiste innanzitutto reimpostare la password dell'utente e quindi modificarlo con la nuova password utilizzando codice simile al seguente:

[!code-aspx[Main](recovering-and-changing-passwords-cs/samples/sample5.aspx)]

Inizia recuperando le informazioni su questo codice *username*, ovvero l'utente cui l'amministratore desidera modificare la password. Successivamente, il `ResetPassword` metodo viene richiamato, quale assegna e l'utente di una nuova password casuale. Questa password generata casualmente viene restituita dal metodo e archiviata nella variabile `resetPwd`. Ora che si conosce la password dell'utente, è possibile modificare tramite una chiamata a `ChangePassword`.

Il problema è che il codice funziona solo se la configurazione di sistema di appartenenza è impostata in modo che `RequiresQuestionAndAnswer` è False. Se `RequiresQuestionAndAnswer` è True, come nel caso di applicazione, il `ResetPassword` (metodo) deve essere passato la risposta segreta, in caso contrario verrà generata un'eccezione.

Se il framework di appartenenze è configurato per richiedere una domanda di sicurezza e una risposta e ancora il client prevede che gli amministratori in grado di modificare le password degli utenti, sono disponibili tre opzioni:

- Generare pratiche in modalità wireless e indicare i client che si tratta di un solo elemento che non può essere eseguito.
- Impostare `RequiresQuestionAndAnswer` su False. Il risultato di un'applicazione di una soluzione meno sicura. Si supponga che un utente nefandi ha ottenuto l'accesso alla posta in arrivo di un altro utente tramite posta elettronica. Ad esempio l'utente compromesso ha lasciato scrivania per andare a pranzo e non blocca le workstation oppure potrebbe essere accedere alla posta elettronica da un terminale pubblico e non è stato disconnesso. In entrambi i casi, l'utente nefandi è possibile visitare il `RecoverPassword.aspx` pagina e immettere il nome dell'utente. Il sistema invierà quindi la password recuperata senza chiedere conferma per la risposta segreta.
- Ignorare il livello di astrazione creato dal framework di appartenenza e lavoro direttamente con il database di SQL Server. Lo schema di appartenenza include una stored procedure denominata `aspnet_Membership_SetPassword` che imposta la password dell'utente e non richiede la risposta segreta o la password precedente per eseguire l'attività.

Nessuna di queste opzioni sono particolarmente interessante, ma che è come il ciclo di vita di uno sviluppatore viene a volte.

Si procede e implementato il terzo approccio, scrivere codice che consente di ignorare il `Membership` e `MembershipUser` classi e direttamente su cui opera il `SecurityTutorials` database.

> [!NOTE]
> Quando si lavora direttamente con il database, l'incapsulamento fornito dal framework di appartenenza è esploso. Questa decisione legata per il `SqlMembershipProvider`, rendendo il nostro codice meno portabile. Inoltre, questo codice potrebbe non funzionare come previsto nelle future versioni di ASP.NET, se viene modificato lo schema di appartenenza. Questo approccio è una soluzione alternativa e, come la maggior parte delle soluzioni alternative, non è un esempio di procedure consigliate.


Il codice presenta alcuni bit della indesiderati ed è piuttosto lungo. Pertanto, non desidero creare confusione in questa esercitazione con un'analisi approfondita di esso. Se si desidera ottenere ulteriori, scaricare il codice per questa esercitazione e visitare il `~/Administration/ManageUsers.aspx` pagina. Questa pagina, che è stati creati nel <a id="_msoanchor_5"> </a> [esercitazione precedente](building-an-interface-to-select-one-user-account-from-many-cs.md), elenca ogni utente. È già stato aggiornato per includere un collegamento a GridView il `UserInformation.aspx` pagina, passando il nome utente dell'utente selezionato tramite la stringa di query. Il `UserInformation.aspx` pagina vengono visualizzate informazioni sull'utente selezionato e caselle di testo per cambiare la propria password (vedere Figura 9).

Dopo aver immettere la nuova password, si verifica nella seconda casella di testo e fare clic sul pulsante aggiornamento utente, viene utilizzata un postback e `aspnet_Membership_SetPassword` stored procedure viene richiamata, aggiornare la password dell'utente. Ti suggeriamo coloro che interessati a questa funzionalità per acquisire maggiore familiarità con il codice e provare a estendere la funzionalità per includere l'invio di un messaggio di posta elettronica all'utente la cui password è stata modificata.


[![Un amministratore può modificare la Password dell'utente](recovering-and-changing-passwords-cs/_static/image26.png)](recovering-and-changing-passwords-cs/_static/image25.png)

**Figura 9**: un amministratore può modificare la Password dell'utente ([fare clic per visualizzare l'immagine ingrandita](recovering-and-changing-passwords-cs/_static/image27.png))


> [!NOTE]
> Il `UserInformation.aspx` pagina attualmente funziona solo se il framework di appartenenze è configurato per archiviare le password in formato Clear o Hashed. Manca il codice per crittografare la nuova password, anche se viene visualizzato un messaggio per aggiungere questa funzionalità. Il modo in cui è consigliabile aggiungere il codice necessario consiste nell'utilizzare un decompilatore come [Reflector](http://www.aisto.com/roeder/dotnet/) per esaminare il codice sorgente per i metodi in .NET Framework; iniziare esaminando il `SqlMembershipProvider` della classe `ChangePassword` metodo. Questa è la tecnica che consentono di scrivere il codice per la creazione di un hash della password.


## <a name="summary"></a>Riepilogo

In ASP.NET sono disponibili due controlli per consentire agli utenti di gestire la propria password. Il controllo PasswordRecovery è utile per gli utenti che hanno dimenticato la propria password. A seconda della configurazione del framework di appartenenza, l'utente sia viene inviato tramite posta elettronica la password esistente o una nuova password generata casualmente. Il controllo ChangePassword consente a un utente di aggiornare la password.

Ad esempio i controlli di accesso e CreateUserWizard, i controlli PasswordRecovery e ChangePassword eseguire il rendering di un'interfaccia utente avanzata senza dover scrivere un trovarsi prima del markup dichiarativo o riga di codice. Se l'interfaccia utente predefinita non soddisfa le proprie esigenze, è possibile personalizzare tramite un'ampia gamma di proprietà di stile. In alternativa, le interfacce dei controlli possono essere convertite in modelli, per un livello di controllo ancora più preciso. Dietro le quinte, questi controlli utilizzano l'API di appartenenza, richiamare il `MembershipUser` dell'oggetto `ResetPassword` e `ChangePassword` metodi.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Guida introduttiva di controllo ChangePassword](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/changepassword.aspx)
- [Guida introduttiva di controllo PasswordRecovery](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/passwordrecovery.aspx)
- [L'invio di posta elettronica in ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)
- [`System.Net.Mail`Domande frequenti](http://www.systemnetmail.com/)

### <a name="about-the-author"></a>Informazioni sull'autore

Scott Mitchell, autore di più libri e fondatore di 4GuysFromRolla, ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è  *[SAM insegna manualmente ASP.NET 2.0 nelle 24 ore](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott può essere raggiunto al [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) o tramite il suo blog all'indirizzo [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Lead revisori per questa esercitazione includono Michael Emmings e Suchi Banerjee. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Precedente](building-an-interface-to-select-one-user-account-from-many-cs.md)
[Successivo](unlocking-and-approving-user-accounts-cs.md)
