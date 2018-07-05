---
uid: web-forms/overview/moving-to-aspnet-20/membership
title: Appartenenza | Microsoft Docs
author: microsoft
description: Sistema di appartenenze ASP.NET si basa sul successo del modello di autenticazione form da ASP.NET 1.x. Autenticazione basata su form ASP.NET fornisce un modo pratico per incorp...
ms.author: aspnetcontent
ms.date: 02/20/2005
ms.assetid: f2339485-5d78-4c5e-8c0a-dc9b8a315345
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/membership
msc.type: authoredcontent
ms.openlocfilehash: f776ed628e206c06543589767ba364af3c76ae16
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818217"
---
<a name="membership"></a>Appartenenza
====================
da [Microsoft](https://github.com/microsoft)

> Sistema di appartenenze ASP.NET si basa sul successo del modello di autenticazione form da ASP.NET 1.x. Autenticazione basata su form ASP.NET fornisce un modo pratico per incorporare un modulo di accesso nell'applicazione ASP.NET e convalidare gli utenti in un database o un altro archivio dati.


Sistema di appartenenze ASP.NET si basa sul successo del modello di autenticazione form da ASP.NET 1.x. Autenticazione basata su form ASP.NET fornisce un modo pratico per incorporare un modulo di accesso nell'applicazione ASP.NET e convalidare gli utenti in un database o un altro archivio dati. I membri della classe FormsAuthentication rendono possibile la gestione dei cookie per l'autenticazione, verificare la presenza di un account di accesso valido, accedere a un utente e così via. Tuttavia, l'implementazione di autenticazione basata su form in un'applicazione di ASP.NET 1.x possono richiedere una notevole quantità di codice.

L'appartenenza di ASP.NET 2.0 è un miglioramento importante rispetto all'uso di autenticazione basata su form da solo. (L'appartenenza è più efficace quando è associata l'autenticazione basata su form, ma utilizzando l'autenticazione basata su form non è un requisito). Come si vedrà a breve, è possibile utilizzare i controlli di accesso e le appartenenze di ASP.NET in ASP.NET 2.0 per implementare un sistema di appartenenze potente senza scrivere molto codice affatto.

## <a name="implementing-membership-in-aspnet-20"></a>Implementazione di appartenenza in ASP.NET 2.0

L'appartenenza viene implementato seguendo i quattro passaggi. Tenere presente che esistono molti passaggi secondari che sono coinvolti, nonché configurazione facoltativa che può essere implementato anche. Questi passaggi sono concepiti per illustrare il quadro generale della configurazione di appartenenza.

1. Creare il database di appartenenza (se SQL Server viene usato come archivio di appartenenza.)
2. Specificare le opzioni di appartenenza nei file di configurazione delle applicazioni. (L'appartenenza è abilitata per impostazione predefinita).
3. Determinare il tipo di archivio di appartenenza che si desidera usare. Le opzioni sono: 

    - Microsoft SQL Server (versione 7.0 o versioni successive)
    - Active Directory Store
    - Provider di appartenenze personalizzato
4. Configurare l'applicazione per l'autenticazione basata su form ASP.NET. Ancora una volta, l'appartenenza è progettato per sfruttare i vantaggi dell'autenticazione basata su form, ma utilizzando l'autenticazione basata su form non è un requisito.
5. Definire gli account utente per l'appartenenza e configurare i ruoli se lo si desidera.

## <a name="creating-the-membership-database"></a>Creazione del Database di appartenenza

Se si utilizza SQL Server 7.0 o versione successiva come archivio di appartenenza, è possibile usare aspnet\_utilità regsql (disponibile più facilmente da di Visual Studio .NET 2005 prompt dei comandi) per configurare il database. Aspnet\_regsql utilità può essere usato come uno strumento del prompt dei comandi o tramite una procedura guidata GUI. Il metodo di procedura guidata è il modo più semplice per configurare il database. Per accedere alla procedura guidata, è sufficiente eseguire il comando seguente:

`aspnet_regsql W`

Quando si esegue il comando, verrà visualizzata con l'installazione guidata di ASP.NET SQL Server come illustrato di seguito.


![](membership/_static/image1.jpg)

**Figura 1**


L'installazione guidata di ASP.NET SQL Server crea il sito Web dell'istanza che è specificare nella procedura guidata. Tuttavia, ASP.NET utilizzerà la stringa di connessione nel file Machine. config per connettersi al database. Per impostazione predefinita, questa stringa di connessione faranno riferimento a un'istanza di SQL Server 2005, pertanto se si usa un'istanza di SQL Server 2000 o SQL Server 7.0, è necessario modificare la stringa di connessione nel file Machine. config. Stringa di connessione può essere disponibile qui:

[!code-xml[Main](membership/samples/sample1.xml)]

Sfortunatamente, se non si modifica la stringa di connessione, ASP.NET non fornirà un errore descrittivo. Solo continuerà a ricevere lamentele da parte per segnalare che il database è ancora stato creato. Nel caso precedente, ho modificato la stringa di connessione in modo da puntare alla mia istanza di SQL Server 2000 locale.

## <a name="specifying-configuration-and-adding-users-and-roles"></a>Specifica di configurazione e l'aggiunta di utenti e ruoli

Il passaggio successivo nella configurazione appartenenza consiste nell'aggiungere le informazioni necessarie al file Web. config dell'applicazione. In ASP.NET 1.x, modificando il file Web. config talvolta era difficile a causa della mancanza di Intellisense e l'uso di lowerCamelCase. Visual Studio .NET 2005 rende molto più semplice con Intellisense per i file di configurazione dell'attività, ma ASP.NET 2.0 fa un ulteriore passo fornendo un'interfaccia Web per la modifica dei file di configurazione.

È possibile avviare l'interfaccia Web facendo clic sul pulsante di configurazione ASP.NET sulla barra degli strumenti Esplora soluzioni, come illustrato di seguito. È anche possibile avviare l'interfaccia Web tramite i popup visualizzate quando vengono inseriti i controlli di accesso.


![](membership/_static/image2.jpg)

**Figura 2**


Verrà avviato lo strumento Amministrazione sito Web di ASP.NET illustrato di seguito. L'amministrazione del sito Web ASP.NET è un'interfaccia con quattro schede che rende più semplice gestire le impostazioni dell'applicazione. Sono disponibili le schede seguenti:

- **Home**
- **Sicurezza** configurare l'accesso, ruoli e utenti.
- **Applicazione** Configura impostazioni dell'applicazione.
- **Provider** configurare e testare il provider di appartenenze delle applicazioni.

Lo strumento Amministrazione sito Web consente facilmente creare nuovi utenti, creare nuovi ruoli e per gestire utenti e ruoli. Questa possibilità non è disponibile nell'interfaccia di Windows. L'interfaccia di Windows consente di definire in modo semplice le impostazioni di autorizzazione e aggiungere, eliminare e gestire i provider, che non sono presenti nello strumento Amministrazione sito Web.

Per avviare l'interfaccia di Windows, aprire lo snap-in Internet Information Services, fare doppio clic sull'applicazione e scegliere Proprietà. Fare clic sulla scheda ASP.NET e quindi fare clic sul pulsante Modifica configurazione. (L'applicazione deve essere eseguito con ASP.NET 2.0 per il pulsante di modifica configurazione deve essere abilitata. È possibile configurare la versione di ASP.NET in anche la finestra di dialogo ASP.NET.) La finestra di dialogo Impostazioni di configurazione ASP.NET viene visualizzato come illustrato di seguito.


![](membership/_static/image3.jpg)

**Figura 3**


Nella scheda Generale, sono elencate le stringhe di connessione e le impostazioni dell'applicazione. Tutte le impostazioni in corsivo vengono definite in un file di configurazione padre (Machine. config o Web. config a un livello superiore) e le impostazioni non in corsivo sono dal file di configurazione delle applicazioni. Se un'impostazione viene aggiunto, rimosso o modificato a livello di applicazione, ASP.NET verrà aggiungere, rimuovere o modificare l'impostazione in Web. config livelli applicazione invece di rimuovere l'impostazione dal file di configurazione da cui viene ereditato.

Di seguito è illustrata la scheda di autenticazione. Si tratta in cui si configureranno le impostazioni di appartenenza. Form di impostazioni di autenticazione provider di appartenenze, e i provider di ruoli possono essere configurati qui.


![](membership/_static/image4.jpg)

**Figura 4**


## <a name="implementing-membership-in-your-application"></a>Implementazione di appartenenza nell'applicazione

Il modo più semplice per implementare l'appartenenza di ASP.NET 2.0 nell'applicazione è usare i controlli di accesso specificati. Questo metodo consente di implementare le nozioni di base dell'appartenenza di ASP.NET 2.0 senza scrivere alcun codice affatto.

I seguenti controlli di accesso sono disponibili in ASP.NET 2.0:

## <a name="login-control"></a>Controllo degli accessi

Il controllo di accesso fornisce un'interfaccia per un utente per accedere al sistema di appartenenza. Vengono presentati textboxt un nome utente e password e un pulsante di accesso. Molte altre funzionalità comuni, ad esempio un collegamento per la registrazione per le persone che non hanno ancora stato fatto in modo che, una casella di controllo che consente all'utente di accesso in modalità automatica durante le successive visite, un collegamento per un promemoria della password e così via. Tutte le funzionalità di controllo di accesso sono personalizzabili mediante le proprietà del controllo.

In ASP.NET 1.x, gli sviluppatori dovevano scrivere una notevole quantità di codice per eseguire una ricerca quando si usa l'autenticazione basata su form. Con l'appartenenza di ASP.NET 2.0, è possibile convalidare gli utenti senza scrivere alcun codice affatto. ASP.NET eseguirà automaticamente la ricerca dell'utente per l'utente. (Se si usa il controllo di accesso senza l'utilizzo di appartenenza ASP.NET, è possibile usare la **OnAuthenticate** metodo per convalidare l'utente.)

## <a name="loginview-control"></a>Controllo LoginView

Il controllo LoginView è un controllo basato su modelli che sono disponibili due modelli per impostazione predefinita. il modello AnonymousTemplate e LoggedInTemplate. Il modello che viene visualizzato è determinato dal o meno l'utente è connesso al sistema di appartenenza. Questo controllo è in genere usato per visualizzare un controllo di accesso quando un utente non ha ancora effettuato e un controllo LoginStatus e/o altri controlli di accesso quando l'utente ha effettuato l'accesso. Se si usa la gestione dei ruoli in un'applicazione ASP.NET, il controllo LoginView può visualizzare un modello specifico in base ai ruoli degli utenti. (Più in ASP.NET gestione dei ruoli verrà trattata in un secondo momento.)

## <a name="passwordrecovery-control"></a>Controllo PassaggiwordRecovery

Il controllo PassaggiwordRecovery consente agli utenti di ricevere un messaggio di posta elettronica con la propria password corrente o reimpostare la propria password. Come testo non crittografato e le password crittografate possono essere ripristinate e inviato tramite posta elettronica agli utenti. Se viene eseguito l'hashing della password, non può essere recuperato. L'utente sarà invece necessario eseguire la reimpostazione della password.

## <a name="loginstatus-control"></a>Controllo LoginStatus

Il controllo LoginStatus consente di visualizzare un indicatore di accesso agli utenti che non sono connessi e un indicatore di disconnessione per gli utenti sono attualmente connessi. La proprietà Request.IsAuthenticated viene utilizzata per determinare quali indicatori da visualizzare. L'indicatore visualizzato dal controllo LoginStatus può essere un testo (implementata tramite il **LoginText** e **LogoutText** delle proprietà) o immagini (implementata tramite il **LoginImageUrl**e **LogoutImageUrl** proprietà.)

Quando si esegue la disconnessione tramite il controllo LoginStatus, egli viene reindirizzato all'URL specificato per il **LogoutPageUrl** proprietà. Se tale proprietà non è impostata, viene aggiornata la pagina corrente. Poiché il sito è probabilmente protetto dall'autenticazione basata su form, l'aggiornamento della pagina corrente reindirizzerà l'utente alla pagina di accesso per il sito.

## <a name="loginname-control"></a>Controllo LoginName

Il controllo LoginName Visualizza il nome utente dell'utente attualmente connesso al sito.

## <a name="createuserwizard-control"></a>Controllo CreateUserWizard

Il controllo CreateUserWizard offre agli utenti un modo pratico per registrarsi per il sistema di appartenenza. È possibile aggiungere passaggi (implementati come una raccolta di WizardStep) tramite l'interfaccia illustrato di seguito.


![](membership/_static/image5.jpg)

**Figura 5**


CreateUserWizard è un controllo basato su modelli che deriva dalla classe di procedura guidata e fornisce i modelli seguenti:

- **Modello HeaderTemplate** questo modello consente di controllare l'aspetto dell'intestazione della procedura guidata.
- **SidebarTemplate** questo modello consente di controllare l'aspetto della barra laterale della procedura guidata.
- **StartNavigationTemplate** controlli questo modello sono l'aspetto del riquadro di spostamento della procedura guidata in fase di avvio.
- **StepNavigationTemplate** questo modello consente di controllare l'aspetto dell'area di navigazione quando non è nel passaggio di inizio o fine.
- **FinishNavigationTemplate** questo modello consente di controllare l'aspetto del riquadro di spostamento quando nel passaggio finale.

Inoltre, per ogni passaggio che aggiunta alla procedura guidata, ASP.NET creerà un modello personalizzato che contiene un elemento ContentTemplate sia un CustomNavigationTemplate per tale passaggio. Per informazioni dettagliate sulla personalizzazione CreateUserWizard, vedere la documentazione di VS.NET 2005:

## <a name="changepassword-control"></a>Controllo ChangePassword

Il controllo ChangePassword consente agli utenti di modificare la propria password. Se la proprietà DisplayUserName è true (è false per impostazione predefinita), l'utente può modificare la password quando non si è connessi. Se l'utente *è* ancora effettuato l'accesso e la proprietà DisplayUserName è true, l'utente sarà in grado di modificare la password di un altro utente che non viene registrato nella fornitura di essi conosce l'ID utente dell'utente.

Tenere presente che se si desidera che gli utenti siano in grado di modificare le password senza dover effettuare l'accesso, è necessario assicurarsi che la pagina in cui viene visualizzato il controllo ChangePassword consenta l'accesso anonimo. Ovviamente, gli utenti dovranno fornire la vecchia password per poter modificare la password.

## <a name="role-management"></a>Gestione dei ruoli

Gestione dei ruoli consente di assegnare gli utenti a un ruolo specifico e quindi limitare l'accesso a determinati file o cartelle in base a tale ruolo. Gestione dei ruoli fornisce anche un'API in modo da poter a livello di codice individuare ruolo apportate o determinare tutti gli utenti in un determinato ruolo e rispondere di conseguenza.

Gestione dei ruoli non è un requisito di appartenenza ASP.NET, né è un requisito per poter utilizzare la gestione dei ruoli di appartenenza. Tuttavia, due integrano tra loro senza problemi ed è probabile che gli sviluppatori verranno usati in combinazione tra loro.

Per abilitare la gestione dei ruoli all'interno dell'applicazione, apportare la modifica seguente nel file Web. config:

[!code-xml[Main](membership/samples/sample2.xml)]

Quando la **cacheRolesInCookie** attributo è impostato su true, memorizzazione nella cache ASP.NET un'appartenenza a ruoli utenti in un cookie sul client. In questo modo le ricerche di ruolo essere eseguita senza le chiamate a RoleProvider. Quando si usa questo attributo, gli sviluppatori sono invitati a verificare che il **cookieProtection** attributo è impostato su tutte. (Questo è l'impostazione predefinita). Ciò garantisce che i dati del cookie vengono crittografati e consente di garantire che il contenuto dei cookie non è stato alterato. È possibile aggiungere i ruoli usando lo strumento Amministrazione sito Web. Consente di definire i ruoli, configurare l'accesso alle parti del sito in base ai ruoli e assegnare utenti ai ruoli.


![](membership/_static/image6.jpg)

**Figura 6**


Come illustrato in precedenza, è sufficiente immettere il nome del ruolo e quindi facendo clic su Aggiungi ruolo è possibile aggiungere nuovi ruoli. Ruoli esistenti possono essere gestiti o eliminati facendo clic sul collegamento appropriato nell'elenco dei ruoli esistenti.

Quando si gestisce un ruolo, è possibile aggiungere o rimuovere gli utenti, come illustrato di seguito.


![](membership/_static/image7.jpg)

**Figura 7**


Selezionando la casella di controllo è nel ruolo utente, è possibile aggiungere facilmente un utente a un ruolo specifico. ASP.NET aggiornerà automaticamente il database di appartenenza con le voci appropriate. È anche possibile configurare le regole di accesso per l'applicazione. ASP.NET 1.x sviluppatori ha familiarità con questa operazione tramite il &lt;autorizzazione&gt; elemento nel file Web. config e che l'opzione è ancora disponibile in ASP.NET 2.0. Tuttavia, più semplice gestire l'accesso delle regole utilizzando la strumento Amministrazione sito Web come mostrato di seguito.


![](membership/_static/image8.jpg)

**Figura 8**


In questo caso, viene evidenziata la cartella di amministrazione (difficile vedere perché lo strumento viene evidenziato in grigio chiaro) e il ruolo di amministratore è stato concesso l'accesso. Vengono negati tutti gli altri utenti. È possibile fare clic sull'icona di head per selezionare una regola e quindi usare i pulsanti Sposta su e Sposta giù per organizzare le regole. Come con ASP.NET &lt;autorizzazione&gt; elemento, le regole vengono elaborate nell'ordine in cui vengono visualizzati. In altre parole, se l'ordine delle regole della ripresa precedente sono stata annullata, nessuno avrebbe accesso alla cartella Administration perché la prima regola che ASP.NET incontrerebbe sarebbe la regola che neghi a tutti gli utenti della cartella.

ASP.NET 2.0 aggiunge un file Web. config nella cartella per cui si specifica una regola di accesso. Le regole di accesso possono essere modificate tramite il file di configurazione o tramite lo strumento Amministrazione sito Web. In altre parole, lo strumento Amministrazione sito Web è semplicemente un'interfaccia attraverso cui il file di configurazione può essere modificato in un ambiente intuitivo.

## <a name="using-roles-in-code"></a>Utilizzo dei ruoli nel codice

L'API per la gestione dei ruoli non è stato modificato dopo la versione 1.x. Il **IsInRole** metodo viene utilizzato per determinare se un utente è incluso un particolare ruolo.

[!code-csharp[Main](membership/samples/sample3.cs)]

ASP.NET crea anche un'istanza RolePrincipal come membro del contesto corrente. L'oggetto RolePrincipal può essere utilizzato per ottenere tutti i ruoli a cui l'utente appartiene come indicato di seguito:

[!code-csharp[Main](membership/samples/sample4.cs)]

## <a name="using-rolegroups-with-the-loginview-control"></a>Utilizzo RoleGroup con il controllo LoginView

Ora che avete la comprensione della gestione dei ruoli e appartenenza, consente di illustrare brevemente come il controllo LoginView sfrutta i vantaggi di questa funzionalità in ASP.NET 2.0. Come indicato in precedenza, il controllo LoginView è un controllo basato su modelli che contiene due modelli per impostazione predefinita. il modello AnonymousTemplate e LoggedInTemplate. All'interno di LoginView attività di finestra di dialogo è un collegamento (mostrato sotto) che consente di eseguire upravit kolekci RoleGroups.


![](membership/_static/image9.jpg)

**Figura 9**


Ogni oggetto RoleGroup contiene una matrice di stringhe che definisce i ruoli che RoleGroup si applica a. Per aggiungere un nuovo RoleGroup per il controllo LoginView, fare clic sul collegamento di modifica RoleGroup. Nell'immagine precedente, si noterà che è stato aggiunto un nuovo RoleGroup per gli amministratori. Selezionando tale RoleGroup (RoleGroup[0]) dall'elenco a discesa di visualizzazioni, è possibile configurare un modello che verrà visualizzato solo ai membri del ruolo amministratori di. Nell'immagine seguente, è stato aggiunto un nuovo RoleGroup che si applica ai membri del ruolo relativo alle vendite e il ruolo di distribuzione. Questo modo viene aggiunta un secondario RoleGroup l'elenco a discesa di viste nella finestra di dialogo Attività LoginView e qualsiasi elemento aggiunto a tale modello sarà visibile per tutti gli utenti di vendita o di distribuzione ruolo.


![](membership/_static/image10.jpg)

**Figura 10**


## <a name="overriding-the-existing-membership-provider"></a>Si esegue l'override del Provider di appartenenza esistente

Esistono un paio di modi che è possibile estendere le funzionalità di appartenenza ASP.NET. Prima di tutto, è ovviamente possibile modificare la funzionalità esistente della classe SqlMembershipProvider ereditino da esso ed eseguendo l'override di metodi. Ad esempio, se si desidera implementare la propria funzionalità quando gli utenti vengono creati, è possibile creare la propria classe che eredita da SqlMembershipProvider come indicato di seguito:

[!code-csharp[Main](membership/samples/sample5.cs)]

Se, d'altra parte, si desidera creare un proprio provider (per archiviare le informazioni di appartenenza in un database di Access, ad esempio), è possibile creare un provider personalizzato.

## <a name="creating-your-own-membership-provider"></a>Creazione di un proprio Provider di appartenenza

Per creare il proprio provider di appartenenze, è necessario innanzitutto creare una classe che eredita dalla classe MembershipProvider. Se si usa Visual Basic.NET, Visual Studio 2005 aggiungerà gli stub per tutti i metodi che è necessario eseguire l'override. Se si usa c#, dipende dalle nostre scelte è quindi necessario aggiungere gli stub.

È necessario eseguire l'override di seguito:

- Proprietà ApplicationName
- ChangePassword (funzione)
- ChangePasswordQuestionAndAnswer (funzione)
- CreateUser (funzione)
- DeleteUser (funzione)
- Proprietà EnablePasswordReset
- Proprietà EnablePasswordRetrieval
- FindUsersByEmail (funzione)
- FindUsersByName (funzione)
- GetAllUsers (funzione)
- GetNumberOfUsersOnline (funzione)
- GetPassword (funzione)
- GetUser (funzione)
- GetUserNameByEmail (funzione)
- Proprietà MaxInvalidPasswordAttempts
- Proprietà MinRequiredNonAlphanumericCharacters
- Proprietà MinRequiredPasswordLength
- Proprietà PasswordAttemptWindow
- Proprietà PasswordFormat
- Proprietà PasswordStrengthRegularExpression
- Proprietà RequiresQuestionAndAnswer
- Proprietà RequiresUniqueEmail
- ResetPassword (funzione)
- Unlock (funzione) utente
- UpdateUser (funzione)
- ValidateUser (funzione)

Memorizzate piuttosto un elenco per implementare gli sviluppatori c#. Può risultare più semplice creare la classe in Visual Basic.NET senza alcuna implementazione e quindi usare .NET Reflector o uno strumento simile per Converti il codice c#.

La stringa di connessione e altre proprietà devono essere impostati sui valori predefiniti nel metodo Initialize. (Il metodo Initialize viene generato quando il provider viene caricato in fase di esecuzione). Il secondo parametro per il metodo Initialize JE typu NameValueCollection ed è un riferimento per la &lt;aggiungere&gt; elemento associato con il provider personalizzato nel file Web. config. Tale voce è simile al seguente:

[!code-xml[Main](membership/samples/sample6.xml)]

Di seguito è riportato un esempio del metodo Initialize.

[!code-csharp[Main](membership/samples/sample7.cs)]

Per convalidare l'utente durante l'invio di form di accesso, è necessario usare il metodo ValidateUser. Questo metodo viene attivata quando l'utente fa clic sul pulsante di accesso nel controllo di accesso. Si inserirà il codice che effettua la ricerca dell'utente in questo metodo.

Come può notare, scrivere il proprio provider di appartenenze non è difficile e permette di estendere questa potente funzionalità di ASP.NET 2.0.
