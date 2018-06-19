---
uid: web-forms/overview/moving-to-aspnet-20/membership
title: L'appartenenza | Documenti Microsoft
author: microsoft
description: Appartenenza ASP.NET si basa sul successo del modello di autenticazione form da ASP.NET 1. x. Autenticazione basata su form ASP.NET fornisce un modo pratico per incorp...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: f2339485-5d78-4c5e-8c0a-dc9b8a315345
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/membership
msc.type: authoredcontent
ms.openlocfilehash: 1a5a495845b60f9aac51c9776311af67f5dc8767
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
ms.locfileid: "28885578"
---
<a name="membership"></a>Appartenenza
====================
by [Microsoft](https://github.com/microsoft)

> Appartenenza ASP.NET si basa sul successo del modello di autenticazione form da ASP.NET 1. x. Autenticazione basata su form ASP.NET fornisce un modo pratico per incorporare un form di accesso nell'applicazione ASP.NET e convalidare gli utenti in un database o l'altro archivio dati.


Appartenenza ASP.NET si basa sul successo del modello di autenticazione form da ASP.NET 1. x. Autenticazione basata su form ASP.NET fornisce un modo pratico per incorporare un form di accesso nell'applicazione ASP.NET e convalidare gli utenti in un database o l'altro archivio dati. I membri della classe FormsAuthentication consentono di gestire i cookie per l'autenticazione, verificare la presenza di un account di accesso valido, accedere a un utente e così via. Tuttavia, l'implementazione di autenticazione basata su form in un'applicazione di 1. x ASP.NET può richiedere una notevole quantità di codice.

L'appartenenza di ASP.NET 2.0 è un miglioramento rispetto all'uso di autenticazione basata su form solo. (L'appartenenza è più affidabile quando è associato l'autenticazione basata su form, ma utilizzando l'autenticazione basata su form non è un requisito). Come non appena si vedrà, è possibile utilizzare i controlli di accesso e le appartenenze di ASP.NET in ASP.NET 2.0 per implementare un sistema di appartenenze potente senza scrivere molto codice affatto.

## <a name="implementing-membership-in-aspnet-20"></a>Implementazione di appartenenza in ASP.NET 2.0

L'appartenenza è implementata da quattro passaggi. Tenere presente che esistono molti passaggi secondari nonché configurazione facoltativa che può essere implementata anche coinvolti. Questi passaggi sono concepiti per illustrare il quadro generale della configurazione dell'appartenenza.

1. Creare il database di appartenenza (se SQL Server viene utilizzato come archivio di appartenenza.)
2. Specificare le opzioni di appartenenza nei file di configurazione di applicazioni. (L'appartenenza è attivata per impostazione predefinita).
3. Determinare il tipo di archivio di appartenenza che si desidera utilizzare. Le opzioni sono: 

    - Microsoft SQL Server (versione 7.0 o versioni successive)
    - Archivio di Active Directory
    - Provider di appartenenze personalizzato
4. Configurare l'applicazione per l'autenticazione basata su form ASP.NET. In questo caso, l'appartenenza è progettata per sfruttare i vantaggi dell'autenticazione basata su form, ma utilizzando l'autenticazione basata su form non è un requisito.
5. Definire gli account utente per l'appartenenza e se lo si desidera configurare i ruoli.

## <a name="creating-the-membership-database"></a>Creazione del Database di appartenenza

Se viene utilizzando SQL Server 7.0 o versioni successive come archivio di appartenenza, è possibile utilizzare aspnet\_utilità regsql (disponibile più facilmente da di Visual Studio .NET 2005 prompt dei comandi) per configurare il database. Aspnet\_regsql utilità può essere utilizzato come uno strumento del prompt dei comandi o tramite una procedura guidata grafica. Il metodo di creazione guidata è il modo più semplice per configurare il database. Per accedere alla procedura guidata, è sufficiente eseguire il comando seguente:

`aspnet_regsql W`

Quando si esegue il comando, verrà visualizzata con l'installazione guidata di ASP.NET SQL Server come illustrato di seguito.


![](membership/_static/image1.jpg)

**Figura 1**


L'installazione guidata di ASP.NET SQL Server crea il sito Web nell'istanza specificate nella procedura guidata. Tuttavia, ASP.NET utilizzerà la stringa di connessione nel file Machine. config per la connessione al database. Per impostazione predefinita, la stringa di connessione punterà a un'istanza di SQL Server 2005, pertanto se si utilizza un'istanza di SQL Server 2000 o SQL Server 7.0, sarà necessario modificare la stringa di connessione nel file Machine. config. Stringa di connessione può trovarsi qui:

[!code-xml[Main](membership/samples/sample1.xml)]

Sfortunatamente, se non si modifica la stringa di connessione, ASP.NET non fornirà un errore descrittivo. Solo continuerà a reclamano che informa che il database è ancora stato creato. Nel caso precedente, ho modificato la stringa di connessione per scegliere l'istanza locale di SQL Server 2000.

## <a name="specifying-configuration-and-adding-users-and-roles"></a>Specifica di configurazione e l'aggiunta di utenti e ruoli

Il passaggio successivo nella configurazione dell'appartenenza consiste nell'aggiungere le informazioni necessarie per il file Web. config dell'applicazione. In ASP.NET 1. x, modificando il file Web. config a volte è difficile a causa l'utilizzo di lowerCamelCase e la mancanza di Intellisense. Visual Studio .NET 2005 semplifica l'attività con Intellisense per i file di configurazione, ma ASP.NET 2.0 consente fornendo un'interfaccia Web per la modifica dei file di configurazione.

È possibile avviare l'interfaccia Web fare clic sul pulsante configurazione ASP.NET, sulla barra degli strumenti Esplora soluzioni, come illustrato di seguito. È anche possibile avviare l'interfaccia Web tramite i popup visualizzate quando si inseriscono controlli di accesso.


![](membership/_static/image2.jpg)

**Figura 2**


Verrà avviato lo strumento di amministrazione sito Web di ASP.NET illustrato di seguito. L'amministrazione sito Web ASP.NET è un'interfaccia di quattro-scheda che rende più semplice gestire le impostazioni dell'applicazione. Sono disponibili le schede seguenti:

- **Home Page**
- **Sicurezza** configurare l'accesso, ruoli e utenti.
- **Applicazione** Configura impostazioni dell'applicazione.
- **Provider** configurare e testare il provider di appartenenze di applicazioni.

Lo strumento Amministrazione sito Web consente facilmente creare nuovi utenti, creare nuovi ruoli e per gestire utenti e ruoli. Questa possibilità non è disponibile nell'interfaccia di Windows. L'interfaccia di Windows consente di definire con facilità le impostazioni di autorizzazione e di aggiungere, eliminare e gestire i provider, che non sono presenti nello strumento Amministrazione sito Web.

Per avviare l'interfaccia di Windows, aprire lo snap-in Internet Information Services, fare doppio clic sull'applicazione e scegliere Proprietà. Fare clic sulla scheda ASP.NET e quindi fare clic sul pulsante Modifica configurazione. (L'applicazione deve essere in esecuzione in ASP.NET 2.0 per il pulsante Modifica configurazione deve essere abilitata. È possibile configurare la versione di ASP.NET nella finestra di dialogo ASP.NET nonché.) La finestra di dialogo Impostazioni di configurazione ASP.NET viene visualizzato come illustrato di seguito.


![](membership/_static/image3.jpg)

**Figura 3**


Nella scheda Generale, sono elencate le stringhe di connessione e le impostazioni dell'applicazione. Tutte le impostazioni in corsivo sono definite in un file di configurazione padre (Machine. config o Web. config a un livello superiore) e le impostazioni non in corsivo sono dal file di configurazione di applicazioni. Se viene aggiunta un'impostazione, rimosso o modificato a livello di applicazione, ASP.NET verrà aggiungere, rimuovere o modificare l'impostazione di Web. config livelli applicazione invece di rimuovere l'impostazione dal file di configurazione da cui viene ereditato.

Di seguito è riportata la scheda autenticazione. Si tratta di dove si configureranno le impostazioni di appartenenza. Le impostazioni di autenticazione, il provider di appartenenze, form e i provider di ruoli possono essere configurati.


![](membership/_static/image4.jpg)

**Figura 4**


## <a name="implementing-membership-in-your-application"></a>Implementazione di appartenenza nell'applicazione

Il modo più semplice per implementare l'appartenenza di ASP.NET 2.0 in un'applicazione è di utilizzare i controlli di accesso specificati. Questo metodo consente di implementare le nozioni di base dell'appartenenza di ASP.NET 2.0 senza scrivere alcun codice affatto.

I seguenti controlli di accesso sono disponibili in ASP.NET 2.0:

## <a name="login-control"></a>Controllo di accesso

Il controllo di accesso fornisce un'interfaccia per un utente per l'accesso al sistema di appartenenza. Si usufruisce textboxt un nome utente e password e un pulsante di accesso. Molte altre funzionalità comuni, ad esempio un collegamento per eseguire la registrazione per gli utenti che non sono ancora stato fatto in modo, una casella di controllo che consente all'utente di accesso in modalità automatica nelle visite successive, un collegamento per ricordare la password e così via. Tutte le funzionalità di controllo di accesso sono personalizzabili tramite le proprietà del controllo.

In ASP.NET 1. x, gli sviluppatori era necessario scrivere una notevole quantità di codice per eseguire una ricerca quando si utilizza l'autenticazione basata su form. Con l'appartenenza di ASP.NET 2.0, è possibile convalidare gli utenti senza scrivere alcun codice affatto. ASP.NET eseguirà automaticamente la ricerca dell'utente per l'utente. (Se si utilizza il controllo di accesso senza mediante l'appartenenza ASP.NET, è possibile utilizzare il **OnAuthenticate** metodo per convalidare l'utente.)

## <a name="loginview-control"></a>Controllo LoginView

Il controllo LoginView è un controllo basato su modelli che sono disponibili due modelli per impostazione predefinita. il modello AnonymousTemplate e LoggedInTemplate. Il modello che viene visualizzato è o meno l'utente è connesso al sistema di appartenenza. Questo controllo è in genere utilizzato per visualizzare un controllo di accesso quando un utente non è connesso e un controllo LoginStatus e/o altri controlli di accesso quando l'utente ha effettuato l'accesso. Se si utilizza la gestione dei ruoli in un'applicazione ASP.NET, il controllo LoginView possibile visualizzare un modello specifico in base al ruolo degli utenti. (Più in ASP.NET la gestione dei ruoli verrà descritta più avanti.)

## <a name="passwordrecovery-control"></a>Controllo PasswordRecovery

Il controllo PasswordRecovery consente agli utenti di ricevere un messaggio di posta elettronica con la propria password corrente o reimpostare la propria password. Essere ripristinate e inviato tramite posta elettronica agli utenti come testo non crittografato e le password crittografate. Se viene eseguito l'hashing della password, non può essere recuperato. L'utente sarà invece necessario per eseguire una reimpostazione della password.

## <a name="loginstatus-control"></a>Controllo LoginStatus

Il controllo LoginStatus viene utilizzato per visualizzare un indicatore di accesso agli utenti che non sono connessi e un indicatore di disconnessione per gli utenti attualmente connessi. La proprietà Request.IsAuthenticated viene utilizzata per determinare quale indicatore da visualizzare. L'indicatore visualizzato dal controllo LoginStatus può essere un testo (implementata tramite il **LoginText** e **LogoutText** proprietà) o immagini (implementata tramite il **LoginImageUrl**e **LogoutImageUrl** proprietà.)

Quando un utente si disconnette tramite il controllo LoginStatus, egli viene reindirizzato all'URL specificato per il **LogoutPageUrl** proprietà. Se tale proprietà non è impostata, la pagina corrente viene aggiornata. Poiché il sito è probabilmente protetto dall'autenticazione basata su form, l'aggiornamento della pagina corrente reindirizzerà l'utente alla pagina di accesso per il sito.

## <a name="loginname-control"></a>Controllo LoginName

Il controllo LoginName Visualizza il nome utente dell'utente attualmente connesso al sito.

## <a name="createuserwizard-control"></a>Controllo CreateUserWizard

Il controllo CreateUserWizard fornisce agli utenti un modo pratico per eseguire la registrazione per il sistema di appartenenze. È possibile aggiungere passaggi (implementati come una raccolta di WizardSteps) tramite l'interfaccia illustrato di seguito.


![](membership/_static/image5.jpg)

**Figura 5**


CreateUserWizard è un controllo basato su modelli che deriva dalla classe di procedura guidata e fornisce i modelli seguenti:

- **HeaderTemplate** questo modello determina l'aspetto dell'intestazione della procedura guidata.
- **SidebarTemplate** questo modello determina l'aspetto della barra laterale della procedura guidata.
- **StartNavigationTemplate** controlli questo modello sono l'aspetto del riquadro di spostamento della procedura guidata in fase di avvio.
- **StepNavigationTemplate** questo modello determina l'aspetto del riquadro di spostamento quando non è nel passaggio di inizio o fine.
- **FinishNavigationTemplate** questo modello determina l'aspetto del riquadro di spostamento quando nel passaggio finale.

Inoltre, per ogni passaggio che aggiunta alla procedura guidata, ASP.NET creerà un modello personalizzato che contiene un ContentTemplate sia un CustomNavigationTemplate per tale passaggio. Per informazioni dettagliate sulla personalizzazione CreateUserWizard, vedere la documentazione di VS.NET 2005:

## <a name="changepassword-control"></a>Controllo ChangePassword

Il controllo ChangePassword consente agli utenti di modificare la propria password. Se la proprietà DisplayUserName è true (è false per impostazione predefinita), l'utente può modificare la propria password quando non vengono registrate. Se l'utente *è* ancora effettuato l'accesso e la proprietà DisplayUserName è true, l'utente sarà in grado di modificare la password di un altro utente che non è connesso fornendo si conosce l'ID utente di tale utente.

Tenere presente che se si desidera che gli utenti siano in grado di modificare le password senza dover accedere, è necessario assicurarsi che la pagina in cui viene visualizzato il controllo ChangePassword consente l'accesso anonimo. Ovviamente, sarà necessario fornire la vecchia password per cambiare la password.

## <a name="role-management"></a>Gestione dei ruoli

Gestione dei ruoli consente di assegnare utenti a un ruolo specifico e quindi limitare l'accesso a determinati file o cartelle in base a tale ruolo. Gestione dei ruoli fornisce inoltre un'API in modo che è a livello di codice può determinare il ruolo someones o determinare tutti gli utenti in un determinato ruolo e rispondere di conseguenza.

Gestione dei ruoli non è un requisito di appartenenza ASP.NET, né è un requisito per usare la gestione dei ruoli di appartenenza. Tuttavia, due integrare correttamente tra loro ed è probabile che gli sviluppatori utilizzeranno le in combinazione tra loro.

Per abilitare la gestione dei ruoli dell'applicazione, apportare la modifica seguente nel file Web. config:

[!code-xml[Main](membership/samples/sample2.xml)]

Quando il **cacheRolesInCookie** attributo è impostato su true, ASP.NET memorizza nella cache un appartenenza ai ruoli utenti in un cookie sul client. In questo modo si verifichino, senza le chiamate a RoleProvider ricerche ruolo. Quando si usa questo attributo, gli sviluppatori sono invitati a verificare che il **cookieProtection** attributo è impostato su tutto. (Questo è l'impostazione predefinita). Ciò garantisce che i dati del cookie vengono crittografati e consente di verificare che il contenuto del cookie non è stato alterato. I ruoli possono essere aggiunti utilizzando lo strumento Amministrazione sito Web. Consente di definire i ruoli, configurare l'accesso a parti del sito in base ai ruoli e assegnare utenti a ruoli.


![](membership/_static/image6.jpg)

**Figura 6**


Come illustrato in precedenza, i nuovi ruoli possono essere aggiunti semplicemente immettendo il nome del ruolo e quindi fare clic su Aggiungi ruolo. Ruoli esistenti possono essere gestiti o eliminati facendo clic sul collegamento appropriato nell'elenco dei ruoli esistenti.

Quando si gestisce un ruolo, è possibile aggiungere o rimuovere utenti, come illustrato di seguito.


![](membership/_static/image7.jpg)

**Figura 7**


Selezionando la casella di controllo del ruolo dell'utente, è possibile aggiungere facilmente un utente a un ruolo specifico. ASP.NET aggiornerà automaticamente il database di appartenenza con le voci appropriate. È anche possibile configurare le regole di accesso per l'applicazione. Gli sviluppatori di 1. x ASP.NET si ha familiari con questa operazione tramite il &lt;autorizzazione&gt; elemento nel file Web. config e che l'opzione è ancora disponibile in ASP.NET 2.0. Tuttavia, il più semplice gestire l'accesso delle regole utilizzando la strumento Amministrazione sito Web come mostrato di seguito.


![](membership/_static/image8.jpg)

**Figura 8**


In questo caso, la cartella di amministrazione è evidenziata (relativo difficile vedere perché lo strumento viene evidenziato in grigio chiaro) e il ruolo di amministratore è stato concesso l'accesso. Vengono negati tutti gli altri utenti. È possibile fare clic sull'icona head per selezionare una regola e quindi utilizzare i pulsanti Sposta su e Sposta giù per disporre le regole. Come con ASP.NET &lt;autorizzazione&gt; elemento, le regole vengono elaborate nell'ordine in cui appaiono. In altre parole, se l'ordine delle regole della ripresa precedente sono stata annullata, nessun altro avrebbe accesso alla cartella amministrazione perché la prima regola di ASP.NET riscontrerebbero sarebbe la regola che neghi a tutti gli utenti della cartella.

ASP.NET 2.0 aggiunge un file Web. config nella cartella per cui si specifica una regola di accesso. Le regole di accesso possono essere modificate tramite il file di configurazione o lo strumento Amministrazione sito Web. In altre parole, lo strumento Amministrazione sito Web è semplicemente un'interfaccia attraverso cui il file di configurazione può essere modificato nella finestra di un ambiente intuitivo.

## <a name="using-roles-in-code"></a>Utilizzo dei ruoli nel codice

L'API per la gestione dei ruoli non è stato modificato fin dalla versione 1. x. Il **IsInRole** metodo viene utilizzato per determinare se un utente in un determinato ruolo.

[!code-csharp[Main](membership/samples/sample3.cs)]

ASP.NET crea inoltre un'istanza RolePrincipal come membro del contesto corrente. L'oggetto RolePrincipal può essere utilizzato per ottenere tutti i ruoli a cui l'utente appartiene come indicato di seguito:

[!code-csharp[Main](membership/samples/sample4.cs)]

## <a name="using-rolegroups-with-the-loginview-control"></a>Utilizzo RoleGroups con il controllo LoginView

Ora che è necessario comprendere la gestione dei ruoli e appartenenze, consente di illustrare brevemente come il controllo LoginView sfrutta questa funzionalità in ASP.NET 2.0. Come illustrato in precedenza, il controllo LoginView è un controllo basato su modelli che contiene i due modelli per impostazione predefinita. il modello AnonymousTemplate e LoggedInTemplate. L'attività LoginView finestra di dialogo è un collegamento (mostrato sotto) che consente di modificare RoleGroups.


![](membership/_static/image9.jpg)

**Figura 9**


Ogni oggetto RoleGroup contiene una matrice di stringhe che definisce i ruoli che RoleGroup si applica a. Per aggiungere un nuovo RoleGroup al controllo LoginView, fare clic sul collegamento Modifica RoleGroups. Nell'immagine precedente, si noterà che ho aggiunto un nuovo RoleGroup per gli amministratori. Se si seleziona tale RoleGroup (RoleGroup[0]) nell'elenco a discesa di visualizzazioni, è possibile configurare un modello che verrà visualizzato solo ai membri del ruolo Administrators. Nell'immagine seguente, ho aggiunto un nuovo RoleGroup che si applica ai membri del ruolo Sales e il ruolo di distribuzione. Questo modo viene aggiunto un RoleGroup secondo elenco a discesa viste nella finestra di dialogo attività e qualsiasi elemento aggiunto a tale modello sarà visibile per tutti gli utenti di vendita o di distribuzione ruolo.


![](membership/_static/image10.jpg)

**Figura 10**


## <a name="overriding-the-existing-membership-provider"></a>Il Provider di appartenenze esistente viene sottoposto a override

Esistono un paio di modi per estendere la funzionalità di appartenenza ASP.NET. Prima di tutto, è ovviamente possibile modificare le funzionalità esistenti di classe SqlMembershipProvider che eredita da esso ed eseguendo l'override di metodi. Ad esempio, se si desidera implementare la funzionalità di creazione di utenti, è possibile creare la propria classe che eredita da SqlMembershipProvider come indicato di seguito:

[!code-csharp[Main](membership/samples/sample5.cs)]

Se, invece, si desidera creare un provider personalizzato (per memorizzare le informazioni di appartenenza in un database di Access, ad esempio), è possibile creare un provider personalizzato.

## <a name="creating-your-own-membership-provider"></a>Creazione di un Provider di appartenenza

Per creare un provider di appartenenze personalizzato, è necessario innanzitutto creare una classe che eredita dalla classe MembershipProvider. Se si utilizza Visual Basic.NET, Visual Studio 2005 aggiungerà gli stub per tutti i metodi che è necessario eseguire l'override. Se si utilizza c#, il relativo fino a è possibile aggiungere gli stub.

È necessario eseguire l'override di seguito:

- Proprietà ApplicationName
- ChangePassword (funzione)
- ChangePasswordQuestionAndAnswer (funzione)
- Funzione CreateUser
- DeleteUser (funzione)
- Proprietà EnablePasswordReset
- Proprietà EnablePasswordRetrieval
- Funzione FindUsersByEmail
- Funzione FindUsersByName
- GetAllUsers (funzione)
- GetNumberOfUsersOnline (funzione)
- Funzione GetPassword
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
- Funzione ResetPassword
- Unlock (funzione) utente
- Funzione UpdateUser
- ValidateUser (funzione)

Memorizzate piuttosto un elenco di implementare gli sviluppatori c#. Può risultare più semplice creare la classe in Visual Basic.NET senza alcuna implementazione e quindi usare Reflector .NET o uno strumento simile per convertire il codice in c#.

La stringa di connessione e le altre proprietà devono essere impostati sui valori predefiniti nel metodo Initialize. (Il metodo Initialize viene generato quando il provider viene caricato in fase di esecuzione). Il secondo parametro per il metodo Initialize è di tipo System.Collections.Specialized.NameValueCollection e un riferimento al &lt;aggiungere&gt; elemento associato con il provider personalizzato nel file Web. config. Tale voce è simile al seguente:

[!code-xml[Main](membership/samples/sample6.xml)]

Di seguito è riportato un esempio del metodo Initialize.

[!code-csharp[Main](membership/samples/sample7.cs)]

Per convalidare l'utente durante l'invio del form di accesso, è necessario utilizzare il metodo ValidateUser. Questo metodo viene generato quando l'utente fa clic sul pulsante di accesso nel controllo di accesso. Inserire il codice che effettua la ricerca dell'utente in questo metodo.

Come si può notare, la scrittura di un provider di appartenenza non è difficile e consente di estendere questa potente funzionalità di ASP.NET 2.0.
