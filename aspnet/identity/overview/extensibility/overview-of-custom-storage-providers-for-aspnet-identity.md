---
uid: identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity
title: "Panoramica dei provider di archiviazione personalizzato per l'identità ASP.NET | Documenti Microsoft"
author: tfitzmac
description: "Identità di ASP.NET è un sistema estendibile che consente di creare un provider di archiviazione e collegarlo all'applicazione senza utilizzare nuovamente il appli..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/13/2014
ms.topic: article
ms.assetid: 681a9204-462e-4260-9a0b-19f0644d6ad7
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 1ea779cb10661512690e3fec16ae73be0f40d15a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="overview-of-custom-storage-providers-for-aspnet-identity"></a>Panoramica dei provider di archiviazione personalizzato per l'identità ASP.NET
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Identità di ASP.NET è un sistema estendibile che consente di creare un provider di archiviazione e collegarlo all'applicazione senza utilizzare nuovamente l'applicazione. In questo argomento viene descritto come creare un provider di archiviazione personalizzato per l'identità ASP.NET. Illustra i concetti importanti per la creazione di un provider di archiviazione, ma non è una procedura dettagliata dell'implementazione di un provider di archiviazione personalizzato.
> 
> Per un esempio di implementazione di un provider di archiviazione personalizzati, vedere [implementa un Provider di archiviazione di MySQL ASP.NET Identity personalizzato](implementing-a-custom-mysql-aspnet-identity-storage-provider.md).
> 
> In questo argomento è stato aggiornato per identità di ASP.NET 2.0.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - Visual Studio 2013 con Update 2
> - Identità di ASP.NET 2


## <a name="introduction"></a>Introduzione

Per impostazione predefinita, il sistema di identità ASP.NET memorizza informazioni utente in un database di SQL Server e utilizza Code First di Entity Framework per creare il database. Per molte applicazioni, questo approccio funziona bene. Tuttavia, è preferibile utilizzare un diverso tipo di meccanismo di persistenza, ad esempio archiviazione tabelle di Azure, o si disponga già di tabelle di database con una struttura molto diversa da quella predefinita. In entrambi i casi, è possibile scrivere un provider personalizzato per il meccanismo di archiviazione e collegare l'applicazione di tale provider.

Identità di ASP.NET è incluso per impostazione predefinita in molti dei modelli di Visual Studio 2013. È possibile ottenere gli aggiornamenti di ASP.NET Identity tramite [pacchetto NuGet di Microsoft ASP.NET Identity EntityFramework](http://www.nuget.org/packages/Microsoft.AspNet.Identity.EntityFramework/).

Questo argomento include le sezioni seguenti:

- [Comprendere l'architettura](#architecture)
- [Comprendere i dati archiviati](#data)
- [Creare il livello di accesso ai dati](#dal)
- [Personalizzare la classe utente](#user)
- [Personalizzare l'archivio dell'utente](#userstore)
- [Personalizzare la classe di ruoli](#role)
- [Personalizzare l'archivio di ruolo](#rolestore)
- [Riconfigurare l'applicazione per l'uso di nuovi provider di archiviazione](#reconfigure)
- [Altre implementazioni del provider di archiviazione personalizzate](#other)

<a id="architecture"></a>
## <a name="understand-the-architecture"></a>Comprendere l'architettura

ASP.NET Identity è costituito da classi, denominato responsabili e archivi. Gestori sono classi di alto livello che uno sviluppatore di applicazioni utilizzato per eseguire operazioni, ad esempio la creazione di un utente, nel sistema di identità di ASP.NET. Gli archivi sono classi di livello inferiore che specificano come entità, ad esempio utenti e ruoli, sono persistenti. Archivi sono strettamente collegati con il meccanismo di persistenza, ma Gestioni vengono disaccoppiate da archivi che significa che è possibile sostituire il meccanismo di persistenza senza interrompere l'intera applicazione.

Il diagramma seguente mostra come applicazione web interagisce con i gestori e archivi di interagiscono con il livello di accesso ai dati.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image1.png)

Per creare un provider di archiviazione personalizzato per ASP.NET Identity, è necessario creare l'origine dati, il livello di accesso ai dati e le classi di archivio che interagiscono con il livello di accesso ai dati. È possibile continuare con la stessa API di gestione per eseguire operazioni sui dati dell'utente, ma ora che i dati vengono salvati in un sistema di archiviazione diverso.

Non è necessario personalizzare le classi di gestione perché quando si crea una nuova istanza della classe UserManager o RoleManager è stato specificato il tipo della classe utente e passare un'istanza della classe di archiviazione come argomento. Questo approccio consente di inserire le classi personalizzate nella struttura esistente. Verrà visualizzato come creare un'istanza di classe UserManager e RoleManager con le classi di archivio personalizzato nella sezione [riconfigurare l'applicazione per l'utilizzo di provider di archiviazione nuovo](#reconfigure).

<a id="data"></a>
## <a name="understand-the-data-that-is-stored"></a>Comprendere i dati archiviati

Per implementare un provider di archiviazione personalizzata, è necessario conoscere i tipi di dati utilizzati con ASP.NET Identity e decidere quali funzionalità sono rilevanti per l'applicazione.

| Dati | Descrizione |
| --- | --- |
| Utenti | Utenti registrati del sito web. Include il nome utente e Id utente. Può includere una password con hash se gli utenti l'accesso con credenziali specifiche per il sito (anziché utilizzare le credenziali da un sito esterno, ad esempio Facebook) e l'indicatore di sicurezza per indicare se le credenziali utente è stata modificata alcuna operazione. Potrebbe includere anche l'indirizzo di posta elettronica, il numero di telefono, se è abilitata l'autenticazione a due fattori, il numero corrente di non gli account di accesso e se un account è stato bloccato. |
| Attestazioni utente | Un set di istruzioni (o attestazioni) sull'utente che rappresentano l'identità dell'utente. Consente l'espressione più grande dell'identità dell'utente rispetto a quello ottenibile tramite i ruoli. |
| Account di accesso utente | Informazioni sul provider di autenticazione esterno (ad esempio Facebook) da utilizzare durante l'accesso a un utente. |
| Ruoli | Gruppi di autorizzazioni per il sito. Include il nome di ruolo e l'Id del ruolo (ad esempio, "Admin" o "Employee"). |

<a id="dal"></a>
## <a name="create-the-data-access-layer"></a>Creare il livello di accesso ai dati

In questo argomento si presuppone che si ha familiarità con il meccanismo di persistenza che si desidera utilizzare e come creare le entità per tale meccanismo. In questo argomento non vengono forniti dettagli su come creare il repository o classi di accesso ai dati. al contrario, fornisce alcuni suggerimenti sulle decisioni di progettazione che è necessario apportare quando si lavora con ASP.NET Identity.

Sono presenti molti di libertà quando si progetta il repository per un oggetto personalizzato di provider dell'archivio. È sufficiente creare repository per le funzionalità che si intendono utilizzare nell'applicazione. Ad esempio, se non si utilizzano ruoli nell'applicazione, non è necessario creare l'archivio per i ruoli o i ruoli utente. La tecnologia e l'infrastruttura esistente potrebbe richiedere una struttura che è molto diversa dall'implementazione predefinita di ASP.NET Identity. Nel livello di accesso ai dati è fornire la logica per funzionare con la struttura dei repository.

Per un'implementazione di MySQL degli archivi di dati per l'identità di ASP.NET 2.0, vedere [MySQLIdentity.sql](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLIdentity.sql).

Nel livello di accesso ai dati, fornire la logica per salvare i dati di ASP.NET Identity all'origine dati. Il livello di accesso ai dati per il provider di archiviazione personalizzato potrebbe includere le seguenti classi per archiviare le informazioni utente e il ruolo.

| Classe | Descrizione | Esempio |
| --- | --- | --- |
| Contesto | Incapsula le informazioni per connettersi al meccanismo di persistenza ed eseguire query. Questa classe è fondamentale per il livello di accesso ai dati. Le altre classi di dati richiede un'istanza di questa classe per eseguire le operazioni. Si inizializzerà anche le classi di archivio con un'istanza di questa classe. | [MySQLDatabase](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLDatabase.cs) |
| Archiviazione utente | Archivia e recupera le informazioni utente (ad esempio, l'hash di nome e una password utente). | [UserTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserTable.cs) |
| Archiviazione di ruolo | Archivia e recupera le informazioni sui ruoli (ad esempio il nome del ruolo). | [RoleTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleTable.cs) |
| Archiviazione di attestazioni utente | Archivia e recupera le informazioni sulle attestazioni utente (ad esempio il tipo di attestazione e il valore). | [UserClaimsTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserClaimsTable.cs) |
| Archiviazione di accessi utente | Archivia e recupera informazioni sull'accesso utente (ad esempio un provider di autenticazione esterno). | [UserLoginsTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserLoginsTable.cs) |
| Archiviazione UserRole | Archivia e recupera i ruoli che un utente è assegnato. | [UserRoleTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserRoleTable.cs) |

Nuovamente, è necessario solo implementare le classi che si intendono utilizzare nell'applicazione.

Nelle classi di accesso ai dati, fornire il codice per eseguire operazioni sui dati per il meccanismo di persistenza specifico. Ad esempio, all'interno dell'implementazione di MySQL, la classe UserTable contiene un metodo per inserire un nuovo record nella tabella di database di utenti. La variabile denominata `_database` è un'istanza della classe MySQLDatabase.

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample1.cs)]

Dopo aver creato le classi di accesso ai dati, è necessario creare classi di archivio che chiamano i metodi specifici nel livello di accesso ai dati.

<a id="user"></a>
## <a name="customize-the-user-class"></a>Personalizzare la classe utente

Quando si implementa un provider di archiviazione, è necessario creare una classe utente, che equivale al [IdentityUser](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.identity.entityframework.identityuser(v=vs.108).aspx) classe il [Microsoft.ASP.NET.Identity.EntityFramework](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) dello spazio dei nomi:

Il diagramma seguente mostra la classe IdentityUser che è necessario creare e l'interfaccia per implementare questa classe.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image2.png)

Il [IUser&lt;TKey&gt; ](https://msdn.microsoft.com/en-us/library/dn613291(v=vs.108).aspx) interfaccia definisce le proprietà che la classe UserManager tenta di chiamare quando l'esecuzione di operazioni richieste. L'interfaccia contiene due proprietà - Id e nome utente. Il [IUser&lt;TKey&gt; ](https://msdn.microsoft.com/en-us/library/dn613291(v=vs.108).aspx) interfaccia consente di specificare il tipo della chiave per l'utente mediante il generico **TKey** parametro. Il tipo della proprietà Id corrisponde al valore del parametro TKey.

Fornisce inoltre il framework di identità di [IUser](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.identity.iuser(v=vs.108).aspx) interfaccia (senza il parametro generico) quando si desidera utilizzare un valore stringa per la chiave.

La classe IdentityUser implementa IUser e contiene proprietà aggiuntive o costruttori per gli utenti sul sito web. Nell'esempio seguente viene illustrata una classe di IdentityUser che utilizza un valore integer per la chiave. Il campo Id è impostato su **int** corrisponda al valore del parametro generico. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample2.cs)]

 Per un'implementazione completa, vedere [IdentityUser (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityUser.cs). 

<a id="userstore"></a>
## <a name="customize-the-user-store"></a>Personalizzare l'archivio dell'utente

Anche possibile creare una classe di oggetto UserStore che fornisce i metodi per tutte le operazioni di dati per l'utente. Questa classe è equivalente al [nello UserStore&lt;TUser&gt; ](https://msdn.microsoft.com/en-us/library/dn315446(v=vs.108).aspx) classe il [Microsoft.ASP.NET.Identity.EntityFramework](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) dello spazio dei nomi. Nella classe nello UserStore, si implementa il [IUserStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/en-us/library/dn613276(v=vs.108).aspx) e una delle interfacce facoltative. Selezionare facoltative interfacce da implementare in base alle funzionalità che si desidera fornire nell'applicazione.

L'immagine seguente mostra la classe oggetto UserStore che è necessario creare e le relative interfacce.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image3.png)

Il modello di progetto predefiniti in Visual Studio contiene codice che si presuppone che molte delle interfacce facoltative sono stati implementati nell'archivio dell'utente. Se si utilizza il modello predefinito con un archivio utente personalizzato, è necessario implementare interfacce facoltative nell'archivio utente o modificare il codice del modello per non chiamare metodi nelle interfacce che non è stato implementato.

 Nell'esempio seguente viene illustrata una classe di archivio utente semplice. Il **TUser** parametro generico accetta il tipo della classe utente è in genere la classe IdentityUser definita. Il **TKey** parametro generico accetta il tipo della chiave utente. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample3.cs)]

 In questo esempio, il costruttore che accetta un parametro denominato *database* di tipo ExampleDatabase è solo un'illustrazione di come passare nella classe di accesso ai dati. Nell'implementazione di MySQL, ad esempio, il costruttore accetta un parametro di tipo MySQLDatabase. 

All'interno della classe di oggetto UserStore, utilizzare le classi di accesso ai dati creata per eseguire operazioni. Ad esempio, nell'implementazione di MySQL, la classe oggetto UserStore è il metodo CreateAsync che utilizza un'istanza di UserTable per inserire un nuovo record. Il **inserire** metodo il **userTable** oggetto è lo stesso metodo che è stato illustrato nella sezione precedente. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample4.cs)]

### <a name="interfaces-to-implement-when-customizing-user-store"></a>Interfacce da implementare per la personalizzazione archivio utente

L'immagine successiva Mostra ulteriori informazioni sulle funzionalità definite in ogni interfaccia. Tutte le interfacce facoltative ereditare da IUserStore.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image4.png)

- **IUserStore**  
 Il [IUserStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/en-us/library/dn613278(v=vs.108).aspx) è l'unica interfaccia è necessario implementare nell'archivio utente. Definisce metodi per la creazione, aggiornamento, eliminazione e recupero degli utenti.
- **IUserClaimStore**  
 Il [IUserClaimStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/en-us/library/dn613265(v=vs.108).aspx) interfaccia definisce i metodi è necessario implementare nell'archivio utente per abilitare le attestazioni utente. Contiene metodi o aggiunta, rimozione e recupera le attestazioni utente.
- **IUserLoginStore**  
 Il [IUserLoginStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/en-us/library/dn613272(v=vs.108).aspx) definisce i metodi è necessario implementare nell'archivio utente per consentire ai provider di autenticazione esterno. Contiene metodi per l'aggiunta, rimozione e il recupero degli account di accesso utente e un metodo per il recupero di un utente in base alle informazioni di accesso.
- **IUserRoleStore**  
 Il [IUserRoleStore&lt;TKey, TUser&gt; ](https://msdn.microsoft.com/en-us/library/dn613276(v=vs.108).aspx) interfaccia definisce i metodi è necessario implementare nell'archivio utente per mappare un utente a un ruolo. Contiene metodi per aggiungere, rimuovere e recuperare i ruoli dell'utente e un metodo per verificare se un utente è assegnato a un ruolo.
- **IUserPasswordStore**  
 Il [IUserPasswordStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/en-us/library/dn613273(v=vs.108).aspx) interfaccia definisce i metodi è necessario implementare nell'archivio utente in modo permanente l'hashing delle password. Contiene metodi per ottenere e impostare la password con hash e un metodo che indica se l'utente ha impostato una password.
- **IUserSecurityStampStore**  
 Il [IUserSecurityStampStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/en-us/library/dn613277(v=vs.108).aspx) interfaccia definisce i metodi è necessario implementare nell'archivio utente da utilizzare un indicatore di sicurezza per che indica se le informazioni sull'account è stato modificato. . Questo indicatore viene aggiornato quando un utente modifica la password, o aggiunge o rimuove gli account di accesso. Contiene metodi per ottenere e impostare l'indicatore di sicurezza.
- **IUserTwoFactorStore**  
 Il [IUserTwoFactorStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/en-us/library/dn613279(v=vs.108).aspx) interfaccia definisce i metodi è necessario implementare per implementare l'autenticazione a due fattori. Contiene metodi per ottenere e impostare se per un utente è abilitata l'autenticazione a due fattori.
- **IUserPhoneNumberStore**  
 Il [IUserPhoneNumberStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/en-us/library/dn613275(v=vs.108).aspx) interfaccia definisce i metodi è necessario implementare per archiviare i numeri di telefono utente. Contiene metodi per ottenere e impostare il numero di telefono e indica se il numero di telefono è confermato.
- **IUserEmailStore**  
 Il [IUserEmailStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/en-us/library/dn613143(v=vs.108).aspx) interfaccia definisce i metodi è necessario implementare per archiviare gli indirizzi di posta elettronica utente. Contiene metodi per ottenere e impostare l'indirizzo di posta elettronica e se il messaggio di posta elettronica è confermato.
- **IUserLockoutStore**  
 Il [IUserLockoutStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/en-us/library/dn613271(v=vs.108).aspx) interfaccia definisce i metodi è necessario implementare per archiviare le informazioni sul blocco di un account. Contiene metodi per ottenere il numero corrente di tentativi di accesso non riuscito, recupero e impostazione dell'account può essere bloccato, ottenere e impostare la data di fine del blocco, incremento del numero di tentativi non riusciti e reimpostazione del numero di tentativi non riusciti.
- **IQueryableUserStore**  
 Il [IQueryableUserStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/en-us/library/dn613267(v=vs.108).aspx) interfaccia definisce i membri che è necessario implementare per fornire un archivio utente queryable. Contiene una proprietà che contiene gli utenti queryable.

 Implementare le interfacce che sono necessari nell'applicazione; ad esempio, di tipo IUserClaimStore, IUserLoginStore, IUserRoleStore, IUserPasswordStore e IUserSecurityStampStore interfacce come illustrato di seguito. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample5.cs)]

Per un'implementazione completa (incluse tutte le interfacce), vedere [nello UserStore (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserStore.cs).

### <a name="identityuserclaim-identityuserlogin-and-identityuserrole"></a>IdentityUserClaim IdentityUserLogin e IdentityUserRole

Lo spazio dei nomi EntityFramework contiene le implementazioni del [IdentityUserClaim](https://msdn.microsoft.com/en-us/library/dn613250(v=vs.108).aspx), [IdentityUserLogin](https://msdn.microsoft.com/en-us/library/dn613251(v=vs.108).aspx), e [IdentityUserRole](https://msdn.microsoft.com/en-us/library/dn613252(v=vs.108).aspx) classi. Se si utilizzano queste funzionalità, si desidera creare la propria versioni di queste classi e definire le proprietà per l'applicazione. Tuttavia, a volte risulta più efficiente per non caricare queste entità in memoria durante l'esecuzione di operazioni di base (ad esempio aggiunta o rimozione di attestazione dell'utente). Al contrario, le classi di archiviazione back-end possono eseguire queste operazioni direttamente sull'origine dati. Ad esempio, il metodo UserStore.GetClaimsAsync() può chiamare il userClaimTable.FindByUserId(user. Metodo ID) per eseguire una query su tabella direttamente e restituire un elenco di attestazioni.

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample6.cs)]

<a id="role"></a>
## <a name="customize-the-role-class"></a>Personalizzare la classe di ruoli

Quando si implementa un provider di archiviazione, è necessario creare una classe role che equivale al [IdentityRole](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.identity.entityframework.identityrole(v=vs.108).aspx) classe il [Microsoft.ASP.NET.Identity.EntityFramework](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) dello spazio dei nomi:

Il diagramma seguente mostra la classe IdentityRole che è necessario creare e l'interfaccia per implementare questa classe.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image5.png)

Il [IRole&lt;TKey&gt; ](https://msdn.microsoft.com/en-us/library/dn613268(v=vs.108).aspx) interfaccia definisce le proprietà che RoleManager tenta di chiamare quando l'esecuzione di operazioni richieste. L'interfaccia contiene due proprietà - Id e nome. Il [IRole&lt;TKey&gt; ](https://msdn.microsoft.com/en-us/library/dn613268(v=vs.108).aspx) interfaccia consente di specificare il tipo della chiave per il ruolo tramite il modello generico **TKey** parametro. Il tipo della proprietà Id corrisponde al valore del parametro TKey.

Fornisce inoltre il framework di identità di [IRole](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.identity.irole(v=vs.108).aspx) interfaccia (senza il parametro generico) quando si desidera utilizzare un valore stringa per la chiave.

Nell'esempio seguente viene illustrata una classe di IdentityRole che utilizza un valore integer per la chiave. Il campo Id è impostato su int per corrispondere al valore del parametro generico. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample7.cs)]

 Per un'implementazione completa, vedere [IdentityRole (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityRole.cs). 

<a id="rolestore"></a>
## <a name="customize-the-role-store"></a>Personalizzare l'archivio di ruolo

Anche possibile creare una classe di oggetto RoleStore che fornisce i metodi per tutte le operazioni di dati sui ruoli. Questa classe è equivalente al [oggetto RoleStore&lt;TRole&gt; ](https://msdn.microsoft.com/en-us/library/dn468181(v=vs.108).aspx) classe nello spazio dei nomi Microsoft.ASP.NET.Identity.EntityFramework. Nella classe oggetto RoleStore, si implementa il [IRoleStore&lt;TRole, TKey&gt; ](https://msdn.microsoft.com/en-us/library/dn613266(v=vs.108).aspx) e facoltativamente la [IQueryableRoleStore&lt;TRole, TKey&gt; ](https://msdn.microsoft.com/en-us/library/dn613262(v=vs.108).aspx) interfaccia.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image6.png)

Nell'esempio seguente viene illustrata una classe di archivio di ruolo. Il parametro generico TRole accetta il tipo della classe ruolo che in genere è la classe IdentityRole definita. Il parametro generico TKey accetta il tipo della chiave del ruolo. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample8.cs)]

- **Interfaccia IRoleStore&lt;TRole&gt;**  
 Il [IRoleStore](https://msdn.microsoft.com/en-us/library/dn468195.aspx) interfaccia definisce i metodi da implementare nella classe archivio ruoli. Contiene metodi per la creazione, aggiornamento, eliminazione e il recupero dei ruoli.
- **Oggetto RoleStore&lt;TRole&gt;**  
 Per personalizzare l'oggetto RoleStore, creare una classe che implementa l'interfaccia IRoleStore. È necessario implementare questa classe se desidera utilizzare i ruoli del sistema. Il costruttore che accetta un parametro denominato *database* di tipo ExampleDatabase è solo un'illustrazione di come passare nella classe di accesso ai dati. Nell'implementazione di MySQL, ad esempio, il costruttore accetta un parametro di tipo MySQLDatabase.  
  
 Per un'implementazione completa, vedere [oggetto RoleStore (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleStore.cs) .

<a id="reconfigure"></a>
## <a name="reconfigure-application-to-use-new-storage-provider"></a>Riconfigurare l'applicazione per l'uso di nuovi provider di archiviazione

È stato implementato il nuovo provider di archiviazione. A questo punto, è necessario configurare l'applicazione per utilizzare questo provider di archiviazione. Se il provider di archiviazione predefinito è stato incluso nel progetto, è necessario rimuovere il provider predefinito e sostituirlo con il provider.

### <a name="replace-default-storage-provider-in-mvc-project"></a>Sostituire il provider di archiviazione predefinito nel progetto MVC

1. Nel **Gestisci pacchetti NuGet** finestra, disinstallare il **Microsoft ASP.NET Identity EntityFramework** pacchetto. È possibile trovare il pacchetto eseguendo una ricerca nei pacchetti di installazione per Identity.EntityFramework.  
    ![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image7.png)Verrà richiesto se si desidera disinstallare anche Entity Framework. Se non è necessaria in altre parti dell'applicazione, è possibile disinstallarlo.
2. Nel file IdentityModels.cs nella cartella Models, eliminare o impostare come commento il **ApplicationUser** e **ApplicationDbContext** classi. In un'applicazione MVC, è possibile eliminare l'intero file IdentityModels.cs. In un'applicazione Web Form, eliminare le due classi, ma accertarsi di mantenere una classe helper che è disponibile anche nel file IdentityModels.cs.
3. Se il provider di archiviazione si trova in un progetto separato, è possibile aggiungere un riferimento a essa nell'applicazione web.
4. Sostituire tutti i riferimenti a `using Microsoft.AspNet.Identity.EntityFramework;` con un'istruzione per lo spazio dei nomi del provider di archiviazione.
5. Nel **Startup.Auth.cs** classe, modificare il **ConfigureAuth** metodo da usare una singola istanza del contesto appropriato. 

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample9.cs?highlight=3)]
6. Nell'App\_cartella di avvio, aprire **IdentityConfig.cs**. Nella classe ApplicationUserManager, modificare il **crea** per restituire una gestione utente che utilizza l'archivio utente personalizzato. 

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample10.cs?highlight=3)]
7. Sostituire tutti i riferimenti a **ApplicationUser** con **IdentityUser**.
8. Il progetto predefinito include alcuni membri nella classe utente che non sono definiti nell'interfaccia IUser. ad esempio posta elettronica, PasswordHash e GenerateUserIdentityAsync. Se la classe utente non dispone di questi membri, è necessario implementarle o modificare il codice che utilizza questi membri.
9. Se è stato creato tutte le istanze di RoleManager, modificare il codice per utilizzare la nuova classe di oggetto RoleStore.  

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample11.cs)]
10. Il progetto predefinito è progettato per una classe utente che ha un valore stringa per la chiave. Se la classe utente presenta un tipo diverso per la chiave (ad esempio un numero intero), è necessario modificare il progetto per l'uso con il tipo. Vedere [modificare chiave primaria per gli utenti in ASP.NET Identity](change-primary-key-for-users-in-aspnet-identity.md).
11. Se necessario, aggiungere la stringa di connessione nel file Web. config.

<a id="other"></a>
## <a name="other-resources"></a>Altre risorse

- Blog: [implementazione di ASP.NET Identity](http://odetocode.com/blogs/scott/archive/2014/01/20/implementing-asp-net-identity.aspx)
- Codice di esercitazione e GIT: [Simple.Data Provider di identità di Asp.Net](http://designcoderelease.blogspot.co.uk/2015/03/simpledata-aspnet-identity-provider.html)
- Esercitazione:[impostazione degli account di identità base e li verso un database esterno](http://typecastexception.com/post/2013/10/27/Configuring-Db-Connection-and-Code-First-Migration-for-Identity-Accounts-in-ASPNET-MVC-5-and-Visual-Studio-2013.aspx). Da [ @xivSolutions ](https://twitter.com/xivSolutions).
- Esercitazione[: implementazione di un Provider di archiviazione di ASP.NET Identity MySQL personalizzato](implementing-a-custom-mysql-aspnet-identity-storage-provider.md)
- [Entità CodeFluent](http://blog.codefluententities.com/2014/04/30/asp-net-identity-v2-and-codefluent-entities/) da [SoftFluent](http://www.softfluent.com/)
- [Archiviazione tabelle di Azure](https://www.nuget.org/packages/accidentalfish.aspnet.identity.azure/) da James Randall.
- Archiviazione tabelle di Azure: [AspNet.Identity.TableStorage](https://github.com/stuartleeks/leeksnet.AspNet.Identity.TableStorage) da [ @stuartleeks ](https://twitter.com/stuartleeks).
- [CouchDB / Cloudant da Daniel Wertheim.](https://github.com/danielwertheim/mycouch.aspnet.identity)
- Eseg elastico[h:%m identità elastico](https://github.com/bmbsqd/elastic-identity) da Bombsquad ab
- [MongoDB](http://www.nuget.org/packages/MongoDB.AspNet.Identity/) da Sheely Lorenzo Sheely Lorenzo.
- [NHibernate.AspNet.Identity](https://github.com/milesibastos/NHibernate.AspNet.Identity) da Antônio Milesi Bastos.
- [RavenDB](http://www.nuget.org/packages/AspNet.Identity.RavenDB/1.0.0) da [ @tourismgeek ](https://twitter.com/tourismgeek).
- [RavenDB.AspNet.Identity](https://github.com/ILMServices/RavenDB.AspNet.Identity) da [ILMServices](http://www.ilmservice.com/).
- Redis: [Redis.AspNet.Identity](https://github.com/aminjam/Redis.AspNet.Identity)
- Modelli T4 per generare EF code per un archivio utente "del database prima di tutto": [AspNet.Identity.EntityFramework](https://github.com/cbfrank/AspNet.Identity.EntityFramework)
