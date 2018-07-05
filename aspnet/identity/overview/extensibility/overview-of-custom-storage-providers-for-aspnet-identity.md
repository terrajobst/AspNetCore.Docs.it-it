---
uid: identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity
title: Panoramica del provider di archiviazione personalizzati per ASP.NET Identity | Microsoft Docs
author: tfitzmac
description: ASP.NET Identity è un sistema estendibile che consente di creare un proprio provider di archiviazione e di inserirli direttamente nell'applicazione senza utilizzare nuovamente il appli...
ms.author: aspnetcontent
ms.date: 10/13/2014
ms.assetid: 681a9204-462e-4260-9a0b-19f0644d6ad7
msc.legacyurl: /identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 35c8986024279bf66f239dbd969fba5ca02f3d01
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37831738"
---
<a name="overview-of-custom-storage-providers-for-aspnet-identity"></a>Panoramica del provider di archiviazione personalizzati per ASP.NET Identity
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> ASP.NET Identity è un sistema estendibile che consente di creare un proprio provider di archiviazione e di inserirli direttamente nell'applicazione senza utilizzare nuovamente l'applicazione. Questo argomento descrive come creare un provider di archiviazione personalizzati per ASP.NET Identity. Viene descritto come i concetti importanti per la creazione di un proprio provider di archiviazione, ma non è una procedura dettagliata dell'implementazione di un provider di archiviazione personalizzati.
> 
> Per un esempio di implementazione di un provider di archiviazione personalizzati, vedere [implementazione di un Provider di archiviazione identità di ASP.NET MySQL personalizzato](implementing-a-custom-mysql-aspnet-identity-storage-provider.md).
> 
> In questo argomento è stato aggiornato per identità di ASP.NET 2.0.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - Visual Studio 2013 con Update 2
> - Identità ASP.NET 2


## <a name="introduction"></a>Introduzione

Per impostazione predefinita, il sistema di ASP.NET Identity archivia informazioni utente in un database di SQL Server e utilizza Code First di Entity Framework per creare il database. Per molte applicazioni, questo approccio funziona bene. Tuttavia, è preferibile utilizzare un diverso tipo di meccanismo di persistenza, ad esempio archiviazione tabelle di Azure, o tabelle di database con una struttura molto diversa da quella predefinita è già presente. In entrambi i casi, è possibile scrivere un provider personalizzato per il meccanismo di archiviazione e collegarsi all'applicazione che il provider.

ASP.NET Identity è incluso per impostazione predefinita in molti dei modelli di Visual Studio 2013. È possibile ottenere gli aggiornamenti ad ASP.NET Identity attraverso [pacchetto EntityFramework NuGet identità di Microsoft AspNet](http://www.nuget.org/packages/Microsoft.AspNet.Identity.EntityFramework/).

Questo argomento include le sezioni seguenti:

- [Comprendere l'architettura](#architecture)
- [Comprendere i dati archiviati](#data)
- [Creare il livello di accesso ai dati](#dal)
- [Personalizzare la classe utente](#user)
- [Personalizzare l'archivio dell'utente](#userstore)
- [Personalizzare la classe di ruoli](#role)
- [Personalizzare l'archivio di ruolo](#rolestore)
- [Riconfigurare l'applicazione per l'uso di nuovi provider di archiviazione](#reconfigure)
- [Altre implementazioni del provider di archiviazione personalizzati](#other)

<a id="architecture"></a>
## <a name="understand-the-architecture"></a>Comprendere l'architettura

ASP.NET Identity è costituito da classi responsabili e gli archivi. I responsabili sono classi ad alto livello che uno sviluppatore di applicazioni viene utilizzato per eseguire operazioni, ad esempio la creazione di un utente, nel sistema di identità di ASP.NET. Gli archivi sono classi di basso livello che specificano come entità, ad esempio utenti e ruoli, vengono resi persistenti. Gli archivi sono strettamente associati con il meccanismo di persistenza, ma i responsabili sono separati dagli archivi che significa che è possibile sostituire il meccanismo di persistenza senza interrompere l'intera applicazione.

Il diagramma seguente mostra come l'applicazione web interagisce con i gestori e archivi di interagiscono con il livello di accesso ai dati.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image1.png)

Per creare un provider di archiviazione personalizzati per ASP.NET Identity, è necessario creare l'origine dati, il livello di accesso ai dati e le classi di archiviazione che interagiscono con questo livello di accesso ai dati. È possibile continuare con la stessa API di gestione per eseguire operazioni sui dati per l'utente, ma ora che i dati vengono salvati in un sistema di archiviazione diverso.

Non è necessario personalizzare le classi di gestione perché quando si crea una nuova istanza della classe UserManager o RoleManager è specificare il tipo della classe utente e passare un'istanza della classe store come argomento. Questo approccio consente di inserire le classi personalizzate nella struttura esistente. Si vedrà come creare un'istanza della classe UserManager e RoleManager con le classi di archivio personalizzato nella sezione [riconfigurare l'applicazione di usare provider di archiviazione nuovo](#reconfigure).

<a id="data"></a>
## <a name="understand-the-data-that-is-stored"></a>Comprendere i dati archiviati

Per implementare un provider di archiviazione personalizzati, è necessario conoscere i tipi di dati usati con ASP.NET Identity e decidere quali funzionalità sono rilevanti per l'applicazione.

| Dati | Descrizione |
| --- | --- |
| Utenti | Utenti registrati del sito web. Include il nome utente e Id utente. Potrebbe includere una password con hash se gli utenti accedono con le credenziali specifiche per il sito, anziché con le credenziali di un sito esterno, ad esempio Facebook, e indicatore di sicurezza per indicare se qualsiasi elemento è stato modificato nelle credenziali dell'utente. Potrebbe anche includere indirizzo di posta elettronica, numero di telefono, se è abilitata l'autenticazione a due fattori, il numero corrente di accessi, non riusciti e indica se un account è stato bloccato. |
| Attestazioni utente | Un set di istruzioni (o attestazioni) sull'utente che rappresentano l'identità dell'utente. Possibile abilitare l'espressione più grande dell'identità dell'utente rispetto a quello ottenibile tramite i ruoli. |
| Account di accesso utente | Informazioni sul provider di autenticazione esterni (come Facebook) da usare durante la registrazione di un utente. |
| Ruoli | Gruppi di autorizzazione per il sito. Include il nome di Id e il ruolo del ruolo (ad esempio, "Admin" o "Employee"). |

<a id="dal"></a>
## <a name="create-the-data-access-layer"></a>Creare il livello di accesso ai dati

In questo argomento si presuppone che si ha familiarità con il meccanismo di persistenza che si intende usare e come creare le entità per questo meccanismo. In questo argomento non fornisce informazioni dettagliate su come creare il repository o classi di accesso ai dati; al contrario, vengono forniti alcuni suggerimenti sulle decisioni di progettazione da prendere quando si lavora con ASP.NET Identity.

Si dispone di molta libertà quando si progetta il repository di codice per un oggetto personalizzato provider dell'archivio. È sufficiente creare i repository per le funzionalità che si intendono utilizzare nell'applicazione. Ad esempio, se non si usa i ruoli dell'applicazione, è necessario creare l'archivio per i ruoli o i ruoli utente. L'infrastruttura esistente e la tecnologia può richiedere una struttura che è molto diversa da un'implementazione predefinita di ASP.NET Identity. Nel livello di accesso ai dati, è fornire la logica da utilizzare con la struttura del repository.

Per un'implementazione di MySQL dei repository di dati per ASP.NET Identity 2.0, vedere [MySQLIdentity.sql](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLIdentity.sql).

Nel livello di accesso ai dati, è fornire la logica per salvare i dati di ASP.NET Identity all'origine dati. Il livello di accesso ai dati per il provider di archiviazione personalizzato potrebbe includere le classi seguenti per archiviare le informazioni utente e il ruolo.

| Classe | Descrizione | Esempio |
| --- | --- | --- |
| Contesto | Incapsula le informazioni per connettersi al meccanismo di persistenza ed eseguire query. Questa classe è fondamentale per il livello di accesso ai dati. Le altre classi di dati richiederà un'istanza di questa classe per eseguire le relative operazioni. Le classi di archivio con un'istanza di questa classe verrà inoltre inizializzato. | [MySQLDatabase](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLDatabase.cs) |
| Archiviazione utente | Archivia e recupera le informazioni utente (ad esempio hash della password e nome utente). | [UserTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserTable.cs) |
| Archiviazione di ruolo | Archivia e recupera le informazioni sui ruoli (ad esempio, il nome del ruolo). | [RoleTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleTable.cs) |
| Archiviazione UserClaims | Archivia e recupera informazioni sulle attestazioni di utente (ad esempio il tipo di attestazione e il valore). | [UserClaimsTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserClaimsTable.cs) |
| Accessi utente archiviazione | Archivia e recupera le informazioni di accesso utente (ad esempio un provider di autenticazione esterni). | [UserLoginsTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserLoginsTable.cs) |
| Archiviazione UserRole | Archivia e recupera i ruoli che un utente è assegnato a. | [UserRoleTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserRoleTable.cs) |

Anche in questo caso, è sufficiente implementare le classi che si intendono utilizzare nell'applicazione.

Nelle classi di accesso dati, è fornire codice per eseguire operazioni di dati per il meccanismo di persistenza specifico. Ad esempio, all'interno dell'implementazione di MySQL, la classe UserTable contiene un metodo per inserire un nuovo record nella tabella di database agli utenti. La variabile denominata `_database` è un'istanza della classe MySQLDatabase.

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample1.cs)]

Dopo aver creato le classi di accesso ai dati, è necessario creare classi di archivio che chiamano i metodi specifici del livello di accesso dati.

<a id="user"></a>
## <a name="customize-the-user-class"></a>Personalizzare la classe utente

Quando si implementa un proprio provider di archiviazione, è necessario creare una classe di utenti che corrispondono al [IdentityUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityuser(v=vs.108).aspx) classe la [Microsoft.ASP.NET.Identity.EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) dello spazio dei nomi:

Il diagramma seguente illustra la classe IdentityUser che è necessario creare e l'interfaccia da implementare in questa classe.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image2.png)

Il [IUser&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613291(v=vs.108).aspx) interfaccia definisce le proprietà che la classe UserManager tenta di chiamare quando l'esecuzione di operazioni richieste. L'interfaccia contiene due proprietà - Id e nome utente. Il [IUser&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613291(v=vs.108).aspx) interfaccia consente di specificare il tipo della chiave per l'utente tramite il modello generico **TKey** parametro. Il tipo della proprietà Id corrisponde al valore del parametro TKey.

Il framework di identità fornisce anche il [IUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.iuser(v=vs.108).aspx) interfaccia (senza il parametro generico) quando si desidera utilizzare un valore stringa per la chiave.

La classe IdentityUser implementa IUser e contiene proprietà aggiuntive o i costruttori per gli utenti nel proprio sito web. Nell'esempio seguente viene illustrata una classe di IdentityUser che usa un numero intero per la chiave. Il campo Id viene impostato su **int** in base al valore del parametro generico. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample2.cs)]

 Per un'implementazione completa, vedere [IdentityUser (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityUser.cs). 

<a id="userstore"></a>
## <a name="customize-the-user-store"></a>Personalizzare l'archivio dell'utente

È anche possibile creare una classe nello UserStore che fornisce i metodi per tutte le operazioni di dati per l'utente. Questa classe è equivalente al [nello UserStore&lt;TUser&gt; ](https://msdn.microsoft.com/library/dn315446(v=vs.108).aspx) classe la [Microsoft.ASP.NET.Identity.EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) dello spazio dei nomi. Nella classe nello UserStore, si implementa il [IUserStore&lt;TKey, TUser&gt; ](https://msdn.microsoft.com/library/dn613276(v=vs.108).aspx) e in tutte le interfacce facoltative. Si seleziona quali interfacce facoltative per implementare basati sulle funzionalità che si desidera fornire nell'applicazione.

L'immagine seguente illustra la classe oggetto UserStore che è necessario creare e le relative interfacce.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image3.png)

Il modello di progetto predefinito in Visual Studio contiene codice che presuppone che molte delle interfacce facoltative sono state implementate nell'archivio dell'utente. Se si usa il modello predefinito con un archivio utente personalizzato, è necessario implementare interfacce facoltative nell'archivio utenti o modificare il codice del modello per non chiamare metodi nelle interfacce che non è stato implementato.

 Nell'esempio seguente viene illustrata una classe di archivio utente semplice. Il **TUser** parametro generico accetta il tipo della classe utente che in genere è la classe IdentityUser è definita. Il **TKey** parametro generico accetta il tipo della chiave utente. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample3.cs)]

 In questo esempio, il costruttore che accetta un parametro denominato *database* typu ExampleDatabase è solo un'illustrazione di come passare nella classe accesso dati. Ad esempio, nell'implementazione di MySQL, il costruttore accetta un parametro di tipo MySQLDatabase. 

All'interno della classe, nello UserStore è usare le classi di accesso di dati creato per eseguire operazioni. Ad esempio, nell'implementazione di MySQL, la classe oggetto UserStore include il metodo CreateAsync che usa un'istanza di UserTable per inserire un nuovo record. Il **inserire** metodo sulle **userTable** oggetti sono lo stesso metodo descritto nella sezione precedente. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample4.cs)]

### <a name="interfaces-to-implement-when-customizing-user-store"></a>Interfacce da implementare quando si personalizza l'archivio dell'utente

L'immagine successiva Mostra altri dettagli sulle funzionalità definite in ogni interfaccia. Tutte le interfacce facoltative ereditare IUserStore.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image4.png)

- **IUserStore**  
  Il [IUserStore&lt;TKey, TUser&gt; ](https://msdn.microsoft.com/library/dn613278(v=vs.108).aspx) è l'unica interfaccia è necessario implementare nell'archivio utente. Definisce i metodi per la creazione, aggiornamento, eliminazione e recupero degli utenti.
- **IUserClaimStore**  
  Il [IUserClaimStore&lt;TKey, TUser&gt; ](https://msdn.microsoft.com/library/dn613265(v=vs.108).aspx) interfaccia definisce i metodi è necessario implementare nell'archivio utente per abilitare attestazioni utente. Contiene metodi o l'aggiunta, rimozione e recupera le attestazioni utente.
- **IUserLoginStore**  
  Il [IUserLoginStore&lt;TKey, TUser&gt; ](https://msdn.microsoft.com/library/dn613272(v=vs.108).aspx) definisce i metodi è necessario implementare nell'archivio utente per consentire ai provider di autenticazione esterni. Contiene i metodi per l'aggiunta, rimozione e il recupero di account di accesso utente e un metodo per il recupero di un utente in base a informazioni di accesso.
- **IUserRoleStore**  
  Il [IUserRoleStore&lt;TKey, TUser&gt; ](https://msdn.microsoft.com/library/dn613276(v=vs.108).aspx) interfaccia definisce i metodi è necessario implementare nell'archivio utenti di eseguire il mapping di un utente a un ruolo. Contiene i metodi per aggiungere, rimuovere e recuperare i ruoli dell'utente e un metodo per verificare se un utente è assegnato a un ruolo.
- **IUserPasswordStore**  
  Il [IUserPasswordStore&lt;TKey, TUser&gt; ](https://msdn.microsoft.com/library/dn613273(v=vs.108).aspx) interfaccia definisce i metodi è necessario implementare nell'archivio utente in modo permanente l'hashing delle password. Contiene i metodi per ottenere e impostare la password con hash e un metodo che indica se l'utente ha impostato una password.
- **IUserSecurityStampStore**  
  Il [IUserSecurityStampStore&lt;TKey, TUser&gt; ](https://msdn.microsoft.com/library/dn613277(v=vs.108).aspx) interfaccia definisce i metodi è necessario implementare nell'archivio utenti di utilizzare un indicatore di sicurezza per indicare se le informazioni dell'utente account sono stato modificato . Questo indicatore viene aggiornato quando un utente modifica la password, o aggiunge o rimuove gli account di accesso. Contiene i metodi per ottenere e impostare l'indicatore di sicurezza.
- **IUserTwoFactorStore**  
  Il [IUserTwoFactorStore&lt;TKey, TUser&gt; ](https://msdn.microsoft.com/library/dn613279(v=vs.108).aspx) interfaccia definisce i metodi è necessario implementare per implementare autenticazione a due fattori. Contiene i metodi per ottenere e impostare se per un utente è abilitata l'autenticazione a due fattori.
- **IUserPhoneNumberStore**  
  Il [IUserPhoneNumberStore&lt;TKey, TUser&gt; ](https://msdn.microsoft.com/library/dn613275(v=vs.108).aspx) interfaccia definisce i metodi è necessario implementare per archiviare i numeri di telefono degli utenti. Contiene i metodi per ottenere e impostare il numero di telefono e indica se il numero di telefono è confermato.
- **IUserEmailStore**  
  Il [IUserEmailStore&lt;TKey, TUser&gt; ](https://msdn.microsoft.com/library/dn613143(v=vs.108).aspx) interfaccia definisce i metodi è necessario implementare per archiviare gli indirizzi di posta elettronica utente. Contiene i metodi per ottenere e impostare l'indirizzo di posta elettronica e indica se il messaggio di posta elettronica è confermato.
- **IUserLockoutStore**  
  Il [IUserLockoutStore&lt;TKey, TUser&gt; ](https://msdn.microsoft.com/library/dn613271(v=vs.108).aspx) interfaccia definisce i metodi è necessario implementare per archiviare le informazioni sul blocco di un account. Contiene i metodi per ottenere il numero corrente di tentativi di accesso non riuscito, recupero e impostazione se l'account può essere bloccato, ottenere e impostare la data di fine del blocco, incrementare il numero di tentativi non riusciti e reimpostazione del numero di tentativi non riusciti.
- **IQueryableUserStore**  
  Il [IQueryableUserStore&lt;TKey, TUser&gt; ](https://msdn.microsoft.com/library/dn613267(v=vs.108).aspx) interfaccia definisce i membri è necessario implementare per fornire un archivio utente disponibile per query. Contiene una proprietà che contiene gli utenti di essere sottoposta a query.

  Implementare le interfacce che sono necessari nell'applicazione; ad esempio, di tipo IUserClaimStore, IUserLoginStore, IUserRoleStore, IUserPasswordStore e IUserSecurityStampStore interfacce come illustrato di seguito. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample5.cs)]

Per un'implementazione completa (incluse tutte le interfacce), vedere [nello UserStore (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserStore.cs).

### <a name="identityuserclaim-identityuserlogin-and-identityuserrole"></a>IdentityUserClaim IdentityUserLogin e IdentityUserRole

Lo spazio dei nomi EntityFramework contiene le implementazioni del [IdentityUserClaim](https://msdn.microsoft.com/library/dn613250(v=vs.108).aspx), [IdentityUserLogin](https://msdn.microsoft.com/library/dn613251(v=vs.108).aspx), e [IdentityUserRole](https://msdn.microsoft.com/library/dn613252(v=vs.108).aspx) classi. Se si usano queste funzionalità, è possibile creare le versioni personalizzate di queste classi e definire le proprietà per l'applicazione. Tuttavia, a volte è più efficiente carica tali entità in memoria durante l'esecuzione di operazioni di base (ad esempio aggiungendo o rimuovendo l'attestazione dell'utente). Al contrario, le classi di archiviazione back-end possono eseguire queste operazioni direttamente sull'origine dati. Ad esempio, il metodo UserStore.GetClaimsAsync() può chiamare il userClaimTable.FindByUserId(user. Metodo di ID) per eseguire una query su tabella direttamente e restituiscono un elenco di attestazioni.

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample6.cs)]

<a id="role"></a>
## <a name="customize-the-role-class"></a>Personalizzare la classe di ruoli

Quando si implementa un proprio provider di archiviazione, è necessario creare una classe role che equivale al [IdentityRole](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityrole(v=vs.108).aspx) classe la [Microsoft.ASP.NET.Identity.EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) dello spazio dei nomi:

Il diagramma seguente illustra la classe IdentityRole che è necessario creare e l'interfaccia da implementare in questa classe.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image5.png)

Il [IRole&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613268(v=vs.108).aspx) interfaccia definisce le proprietà che RoleManager tenta di chiamare quando l'esecuzione di operazioni richieste. L'interfaccia contiene due proprietà - Id e nome. Il [IRole&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613268(v=vs.108).aspx) interfaccia consente di specificare il tipo della chiave per il ruolo tramite il modello generico **TKey** parametro. Il tipo della proprietà Id corrisponde al valore del parametro TKey.

Il framework di identità fornisce anche il [IRole](https://msdn.microsoft.com/library/microsoft.aspnet.identity.irole(v=vs.108).aspx) interfaccia (senza il parametro generico) quando si desidera utilizzare un valore stringa per la chiave.

Nell'esempio seguente viene illustrata una classe di IdentityRole che usa un numero intero per la chiave. Il campo Id viene impostato a int in base al valore del parametro generico. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample7.cs)]

 Per un'implementazione completa, vedere [IdentityRole (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityRole.cs). 

<a id="rolestore"></a>
## <a name="customize-the-role-store"></a>Personalizzare l'archivio di ruolo

È anche possibile creare una classe di oggetto RoleStore che fornisce i metodi per tutte le operazioni di dati sui ruoli. Questa classe è equivalente al [oggetto RoleStore&lt;TRole&gt; ](https://msdn.microsoft.com/library/dn468181(v=vs.108).aspx) classe nello spazio dei nomi Microsoft.ASP.NET.Identity.EntityFramework. Nella classe oggetto RoleStore, si implementa il [IRoleStore&lt;TKey, TRole&gt; ](https://msdn.microsoft.com/library/dn613266(v=vs.108).aspx) e, facoltativamente, il [IQueryableRoleStore&lt;TRole, TKey&gt; ](https://msdn.microsoft.com/library/dn613262(v=vs.108).aspx) interfaccia.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image6.png)

Nell'esempio seguente viene illustrata una classe di archivio ruoli. Il parametro generico TRole accetta il tipo della classe del ruolo che in genere è la classe IdentityRole che è definita. Il parametro generico TKey accetta il tipo della chiave del ruolo. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample8.cs)]

- **IRoleStore&lt;TRole&gt;**  
  Il [IRoleStore](https://msdn.microsoft.com/library/dn468195.aspx) interfaccia definisce i metodi da implementare nella classe archivio ruoli. Contiene i metodi per la creazione, aggiornamento, eliminazione e recupero dei ruoli.
- **RoleStore&lt;TRole&gt;**  
  Per personalizzare l'oggetto RoleStore, creare una classe che implementa l'interfaccia IRoleStore. È sufficiente implementare questa classe se desidera utilizzare i ruoli del sistema. Il costruttore che accetta un parametro denominato *database* typu ExampleDatabase è solo un'illustrazione di come passare nella classe accesso dati. Ad esempio, nell'implementazione di MySQL, il costruttore accetta un parametro di tipo MySQLDatabase.  
  
  Per un'implementazione completa, vedere [oggetto RoleStore (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleStore.cs) .

<a id="reconfigure"></a>
## <a name="reconfigure-application-to-use-new-storage-provider"></a>Riconfigurare l'applicazione per l'uso di nuovi provider di archiviazione

È stato implementato il nuovo provider di archiviazione. A questo punto, è necessario configurare l'applicazione per usare questo provider di archiviazione. Se il provider di archiviazione predefinito è stato incluso nel progetto, è necessario rimuovere il provider predefinito e sostituirlo con il provider.

### <a name="replace-default-storage-provider-in-mvc-project"></a>Sostituire il provider di archiviazione predefinito nel progetto MVC

1. Nel **Gestisci pacchetti NuGet** finestra, disinstallare le **Microsoft ASP.NET Identity EntityFramework** pacchetto. È possibile trovare il pacchetto eseguendo una ricerca nei pacchetti installati per Identity.EntityFramework.  
    ![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image7.png) Verrà richiesto se si desidera disinstallare anche Entity Framework. Se non è necessario in altre parti dell'applicazione, è possibile disinstallarlo.
2. Nel file IdentityModels.cs nella cartella Models, eliminare o impostare come commento il **ApplicationUser** e **ApplicationDbContext** classi. In un'applicazione MVC, è possibile eliminare l'intero file IdentityModels.cs. In un'applicazione Web Form, eliminare le due classi, ma accertarsi di che mantenere la classe helper che si trova anche nel file IdentityModels.cs.
3. Se il provider di archiviazione si trova in un progetto separato, aggiungere un riferimento a esso nell'applicazione web.
4. Sostituire tutti i riferimenti a `using Microsoft.AspNet.Identity.EntityFramework;` con un'istruzione per lo spazio dei nomi del provider di archiviazione.
5. Nel **Startup.Auth.cs** classe, modificare il **ConfigureAuth** metodo da usare una singola istanza di contesto appropriata. 

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample9.cs?highlight=3)]
6. Nell'App\_cartella di avvio, aprire **IdentityConfig.cs**. Nella classe ApplicationUserManager, modificare il **Create** per restituire una gestione utente che usa l'archivio utente personalizzato. 

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample10.cs?highlight=3)]
7. Sostituire tutti i riferimenti a **ApplicationUser** con **IdentityUser**.
8. Il progetto predefinito include alcuni membri nella classe utente che non sono definiti nell'interfaccia IUser. ad esempio indirizzo di posta elettronica, PasswordHash e GenerateUserIdentityAsync. Se la classe utente non dispone di questi membri, è necessario implementarle o modificare il codice che usa questi membri.
9. Se è stato creato tutte le istanze di RoleManager, modificare il codice per usare la nuova classe di oggetto RoleStore.  

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample11.cs)]
10. Il progetto predefinito è progettato per una classe utente che ha un valore stringa per la chiave. Se la classe utente presenta un tipo diverso per la chiave (ad esempio, un numero intero), è necessario modificare il progetto per l'uso con il tipo. Visualizzare [modificare la chiave primaria per gli utenti in ASP.NET Identity](change-primary-key-for-users-in-aspnet-identity.md).
11. Se necessario, aggiungere la stringa di connessione nel file Web. config.

<a id="other"></a>
## <a name="other-resources"></a>Altre risorse

- Blog: [implementazione di ASP.NET Identity](http://odetocode.com/blogs/scott/archive/2014/01/20/implementing-asp-net-identity.aspx)
- Codice di esercitazione e GIT: [Simple.Data Provider di identità Asp.Net](http://designcoderelease.blogspot.co.uk/2015/03/simpledata-aspnet-identity-provider.html)
- Esercitazione:[configurare gli account di identità di base e che punta a un database esterno](http://typecastexception.com/post/2013/10/27/Configuring-Db-Connection-and-Code-First-Migration-for-Identity-Accounts-in-ASPNET-MVC-5-and-Visual-Studio-2013.aspx). Dal [ @xivSolutions ](https://twitter.com/xivSolutions).
- Esercitazione[: implementazione di un Provider di archiviazione MySQL personalizzato ASP.NET Identity](implementing-a-custom-mysql-aspnet-identity-storage-provider.md)
- [Le entità CodeFluent](http://blog.codefluententities.com/2014/04/30/asp-net-identity-v2-and-codefluent-entities/) da [SoftFluent](http://www.softfluent.com/)
- [Archiviazione tabelle di Azure](https://www.nuget.org/packages/accidentalfish.aspnet.identity.azure/) Randall James.
- Archiviazione tabelle di Azure: [AspNet.Identity.TableStorage](https://github.com/stuartleeks/leeksnet.AspNet.Identity.TableStorage) dal [ @stuartleeks ](https://twitter.com/stuartleeks).
- [CouchDB / Cloudant da Daniel Wertheim.](https://github.com/danielwertheim/mycouch.aspnet.identity)
- Elastico Uire[h:%m identità elastico](https://github.com/bmbsqd/elastic-identity) da Bombsquad AB.
- [MongoDB](http://www.nuget.org/packages/MongoDB.AspNet.Identity/) da Jonathan Sheely Jonathan Sheely.
- [NHibernate.AspNet.Identity](https://github.com/milesibastos/NHibernate.AspNet.Identity) da Antônio Milesi Bastos.
- [RavenDB](http://www.nuget.org/packages/AspNet.Identity.RavenDB/1.0.0) dal [ @tourismgeek ](https://twitter.com/tourismgeek).
- [RavenDB.AspNet.Identity](https://github.com/ILMServices/RavenDB.AspNet.Identity) dal [ILMServices](http://www.ilmservice.com/).
- Redis: [Redis.AspNet.Identity](https://github.com/aminjam/Redis.AspNet.Identity)
- Modelli T4 per generare Entity Framework di codice per un archivio utente "del database prima di tutto": [AspNet.Identity.EntityFramework](https://github.com/cbfrank/AspNet.Identity.EntityFramework)
