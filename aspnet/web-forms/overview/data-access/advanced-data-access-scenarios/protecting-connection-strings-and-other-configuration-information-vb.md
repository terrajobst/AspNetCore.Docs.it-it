---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/protecting-connection-strings-and-other-configuration-information-vb
title: Protezione delle stringhe di connessione e altre informazioni di configurazione (VB) | Microsoft Docs
author: rick-anderson
description: Un'applicazione ASP.NET in genere archivia le informazioni di configurazione in un file Web. config. Alcune di queste informazioni è sensibile e garantisce la protezione dati. Da def....
ms.author: riande
ms.date: 08/03/2007
ms.assetid: cd17dbe1-c5e1-4be8-ad3d-57233d52cef1
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/protecting-connection-strings-and-other-configuration-information-vb
msc.type: authoredcontent
ms.openlocfilehash: 9713bbd983c4e922273a23356cbbb3848a8b7c50
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838369"
---
<a name="protecting-connection-strings-and-other-configuration-information-vb"></a>Protezione delle stringhe di connessione e altre informazioni di configurazione (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare il codice](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_73_VB.zip) o [Scarica il PDF](protecting-connection-strings-and-other-configuration-information-vb/_static/datatutorial73vb1.pdf)

> Un'applicazione ASP.NET in genere archivia le informazioni di configurazione in un file Web. config. Alcune di queste informazioni è sensibile e garantisce la protezione dati. Per impostazione predefinita questo file non verrà servito da un visitatore di un sito Web, ma un amministratore o un utente malintenzionato potrebbe ottenere l'accesso al file system del server Web e visualizzare il contenuto del file. In questa esercitazione si apprenderà che ASP.NET 2.0 consente di proteggere informazioni sensibili tramite la crittografia delle sezioni del file Web. config.


## <a name="introduction"></a>Introduzione

Le informazioni di configurazione per le applicazioni ASP.NET vengono in genere archiviate in un file XML denominato `Web.config`. Nel corso delle esercitazioni seguenti sono state aggiornate le `Web.config` un numero limitato di volte. Durante la creazione il `Northwind` DataSet tipizzata nel [prima esercitazione](../introduction/creating-a-data-access-layer-vb.md), ad esempio, informazioni sulla stringa di connessione aggiunto automaticamente al `Web.config` nel `<connectionStrings>` sezione. Più avanti, nelle [pagine Master e spostamento nel sito](../introduction/master-pages-and-site-navigation-vb.md) esercitazione abbiamo aggiornato manualmente `Web.config`, l'aggiunta un `<pages>` elemento che indica che tutte le pagine ASP.NET nel nostro progetto deve utilizzare il `DataWebControls` tema.

Poiché `Web.config` potrebbe contenere dati riservati, ad esempio le stringhe di connessione, è importante che il contenuto di `Web.config` rimanere sicuri e nascosta dai visualizzatori autorizzati. Per impostazione predefinita, qualsiasi HTTP richiesta in un file con il `.config` estensione viene gestita dal motore di ASP.NET, che restituisce il *questo tipo di pagina non è servito* messaggio illustrato nella figura 1. Di conseguenza, i visitatori non è possibile visualizzare le `Web.config` s contenuto dei file immettendo semplicemente http://www.YourServer.com/Web.config nella barra degli indirizzi del browser s.


[![Visita di Web. config tramite un Browser restituisce un questo tipo di pagina non è servito messaggio](protecting-connection-strings-and-other-configuration-information-vb/_static/image2.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image1.png)

**Figura 1**: visita `Web.config` tramite un Browser restituisce un questo tipo di pagina non è servita messaggio ([fare clic per visualizzare l'immagine con dimensioni normali](protecting-connection-strings-and-other-configuration-information-vb/_static/image3.png))


Ma cosa succede se un utente malintenzionato è in grado di trovare alcuni altri exploit che consenta di visualizzare il `Web.config` s contenuto dei file? Che cosa può fare con queste informazioni un utente malintenzionato e le operazioni possono essere eseguite per proteggere ulteriormente le informazioni sensibili all'interno di `Web.config`? Fortunatamente, la maggior parte delle sezioni in `Web.config` non contengono informazioni riservate. Il danno può un utente malintenzionato selettivo se si conosce il nome del tema usato da pagine ASP.NET predefinito?

Determinati `Web.config` sezioni, tuttavia, contengono informazioni riservate che possono includere stringhe di connessione, i nomi utente, password, i nomi dei server, le chiavi di crittografia e così via. Queste informazioni si trova in genere nell'esempio seguente `Web.config` sezioni:

- `<appSettings>`
- `<connectionStrings>`
- `<identity>`
- `<sessionState>`

In questa esercitazione si esaminerà le tecniche per la protezione di tali informazioni di configurazione sensibili. Come si vedrà, .NET Framework versione 2.0 include un sistema di configurazioni protetto che rende le sezioni di configurazione selezionate a livello di codice la crittografia e la decrittografia di un gioco da ragazzi.

> [!NOTE]
> Questa esercitazione si conclude con uno sguardo alle raccomandazioni di Microsoft s per la connessione a un database da un'applicazione ASP.NET. Oltre a crittografare le stringhe di connessione, è possibile consentire la protezione avanzata del sistema, garantendo che ci si connette al database in modo sicuro.


## <a name="step-1-exploring-aspnet-20-s-protected-configuration-options"></a>Passaggio 1: Esplorazione di ASP.NET 2.0 s protette le opzioni di configurazione

ASP.NET 2.0 include un sistema di configurazione protetta per crittografare e decrittografare le informazioni di configurazione. I metodi inclusi in .NET Framework che può essere utilizzato per crittografare o decrittografare le informazioni di configurazione a livello di codice. Il sistema di configurazione protetta Usa la [modello di provider](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), che consente agli sviluppatori di scegliere quale implementazione di crittografia viene utilizzata.

.NET Framework viene fornito con due provider di configurazione protetta:

- [`RSAProtectedConfigurationProvider`](https://msdn.microsoft.com/library/system.configuration.rsaprotectedconfigurationprovider.aspx) -Usa l'asimmetriche [algoritmo RSA](http://en.wikipedia.org/wiki/Rsa) per la crittografia e decrittografia.
- [`DPAPIProtectedConfigurationProvider`](https://msdn.microsoft.com/system.configuration.dpapiprotectedconfigurationprovider.aspx) -Usa i Windows [Data Protection API (DPAPI)](https://msdn.microsoft.com/library/ms995355.aspx) per la crittografia e decrittografia.

Poiché il sistema di configurazione protetto implementa provider design pattern, è possibile creare il proprio provider di configurazione protetto e inserirlo nell'applicazione. Visualizzare [implementazione di un Provider di configurazione protetta](https://msdn.microsoft.com/library/wfc2t3az(VS.80).aspx) per altre informazioni su questo processo.

I provider di DPAPI e RSA usano le chiavi per le routine di crittografia e decrittografia, e queste chiavi possono essere archiviate a livello computer o utente. Le chiavi a livello di computer sono ideali per scenari in cui l'applicazione web viene eseguita sul proprio server dedicato o se sono presenti più applicazioni in un server che devono condividere informazioni crittografate. Le chiavi a livello di utente sono un'opzione più sicura in ambienti di hosting condivisi in cui altre applicazioni sullo stesso server non devono essere in grado di decrittografare le sezioni di configurazione dell'applicazione s protetti.

In questa esercitazione si userà questi esempi il provider di DPAPI e le chiavi a livello di computer. In particolare, si esaminerà la crittografia di `<connectionStrings>` nella sezione `Web.config`, anche se il sistema di configurazione protetta può essere utilizzato per crittografare la maggior parte delle eventuali `Web.config` sezione. Per informazioni sull'uso di chiavi a livello di utente o tramite il provider RSA, consultare le risorse nella sezione ulteriori letture alla fine di questa esercitazione.

> [!NOTE]
> Il `RSAProtectedConfigurationProvider` e `DPAPIProtectedConfigurationProvider` i provider vengono registrati nel `machine.config` file con i nomi di provider `RsaProtectedConfigurationProvider` e `DataProtectionConfigurationProvider`, rispettivamente. Quando la crittografia o decrittografia le informazioni di configurazione è necessario specificare il nome del provider appropriato (`RsaProtectedConfigurationProvider` oppure `DataProtectionConfigurationProvider`) anziché il nome di tipo effettivo (`RSAProtectedConfigurationProvider` e `DPAPIProtectedConfigurationProvider`). È possibile trovare il `machine.config` del file nei `$WINDOWS$\Microsoft.NET\Framework\version\CONFIG` cartella.


## <a name="step-2-programmatically-encrypting-and-decrypting-configuration-sections"></a>Passaggio 2: A livello di programmazione crittografare e decrittografare le sezioni di configurazione

Con poche righe di codice è possibile crittografare o decrittografare una sezione di configurazione specifico utilizzando un provider specificato. Il codice, come vedremo tra breve, deve semplicemente fare riferimento a livello di codice la sezione di configurazione appropriato chiamare relativo `ProtectSection` oppure `UnprotectSection` (metodo) e quindi chiamare il `Save` metodo per rendere permanenti le modifiche. Inoltre, .NET Framework include un'utilità della riga di comando utili che è possibile crittografare e decrittografare le informazioni di configurazione. Esamineremo questa utilità della riga di comando nel passaggio 3.

Per illustrare la protezione a livello di codice le informazioni di configurazione, s ti permettono di creare una pagina ASP.NET che include i pulsanti per crittografare e decrittografare i `<connectionStrings>` sezione `Web.config`.

Iniziare aprendo il `EncryptingConfigSections.aspx` nella pagina di `AdvancedDAL` cartella. Trascinare un controllo casella di testo dalla casella degli strumenti nella finestra di progettazione, l'impostazione relativa `ID` proprietà `WebConfigContents`, la relativa `TextMode` proprietà `MultiLine`e il relativo `Width` e `Rows` proprietà al 95% e 15, rispettivamente. Questo controllo casella di testo verrà visualizzato il contenuto di `Web.config` che consente di visualizzare rapidamente se il contenuto viene crittografato o No. Naturalmente, in un'applicazione reale non sarebbe mai desiderato visualizzare il contenuto di `Web.config`.

Sotto la casella di testo, aggiungere due controlli Button denominati `EncryptConnStrings` e `DecryptConnStrings`. Impostare le relative proprietà di testo per le stringhe di connessione di crittografare e decrittografare le stringhe di connessione.

A questo punto la schermata dovrebbe essere simile alla figura 2.


[![Aggiungere una casella di testo e due controlli Web pulsante alla pagina](protecting-connection-strings-and-other-configuration-information-vb/_static/image5.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image4.png)

**Figura 2**: aggiungere una casella di testo e due controlli Web pulsante alla pagina ([fare clic per visualizzare l'immagine con dimensioni normali](protecting-connection-strings-and-other-configuration-information-vb/_static/image6.png))


Successivamente, è necessario scrivere codice che carica e visualizza il contenuto del `Web.config` nella `WebConfigContents` nella casella di testo quando la pagina viene caricata. Aggiungere il codice seguente alla classe code-behind s di pagina. Questo codice aggiunge un metodo denominato `DisplayWebConfig` e lo chiama dal `Page_Load` gestore dell'evento quando `Page.IsPostBack` è `False`:


[!code-vb[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample1.vb)]

Il `DisplayWebConfig` metodo viene utilizzato il [ `File` classe](https://msdn.microsoft.com/library/system.io.file.aspx) per aprire l'applicazione s `Web.config` file, il [ `StreamReader` classe](https://msdn.microsoft.com/library/system.io.streamreader.aspx) leggerne il contenuto in una stringa e il [ `Path` classe](https://msdn.microsoft.com/library/system.io.path.aspx) per generare il percorso fisico di `Web.config` file. Queste tre classi tutti reperibili nel [ `System.IO` dello spazio dei nomi](https://msdn.microsoft.com/library/system.io.aspx). Di conseguenza, è necessario aggiungere un `Imports``System.IO` istruzione per l'inizio della classe code-behind o, in alternativa, queste classi nomi con prefisso `System.IO.`

Successivamente, è necessario aggiungere i gestori eventi per i due controlli Button `Click` gli eventi e aggiungere il codice necessario per crittografare e decrittografare il `<connectionStrings>` sezione usando una chiave a livello di computer con il provider DPAPI. Dalla finestra di progettazione, fare doppio clic su ognuno dei pulsanti per aggiungere un `Click` gestore nel code-behind in classe e quindi aggiungere il codice seguente:


[!code-vb[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample2.vb)]

Il codice usato in due gestori eventi è quasi identico. Entrambi per iniziare, recuperare informazioni sui dispositivi dell'applicazione corrente `Web.config` del file tramite il [ `WebConfigurationManager` classe](https://msdn.microsoft.com/library/system.web.configuration.webconfigurationmanager.aspx) s [ `OpenWebConfiguration` metodo](https://msdn.microsoft.com/library/system.web.configuration.webconfigurationmanager.openwebconfiguration.aspx). Questo metodo restituisce il file di configurazione web per il percorso virtuale specificato. Successivamente, il `Web.config` file s `<connectionStrings>` sezione è possibile accedere tramite il [ `Configuration` classe](https://msdn.microsoft.com/library/system.configuration.configuration.aspx) s [ `GetSection(sectionName)` metodo](https://msdn.microsoft.com/library/system.configuration.configuration.getsection.aspx), che restituisce un [ `ConfigurationSection` ](https://msdn.microsoft.com/library/system.configuration.configurationsection.aspx) oggetti.

Il `ConfigurationSection` oggetto include un [ `SectionInformation` proprietà](https://msdn.microsoft.com/library/system.configuration.configurationsection.sectioninformation.aspx) che fornisce informazioni aggiuntive e le funzionalità riguardanti la sezione di configurazione. Come illustrato nel codice precedente, è possibile determinare se la sezione di configurazione è crittografata controllando il `SectionInformation` proprietà s `IsProtected` proprietà. Inoltre, la sezione può essere crittografata o decrittografata mediante la `SectionInformation` proprietà s `ProtectSection(provider)` e `UnprotectSection` metodi.

Il `ProtectSection(provider)` metodo accetta come input una stringa che specifica il nome del provider di configurazione protetta da utilizzare durante la crittografia. Nel `EncryptConnString` passiamo DataProtectionConfigurationProvider nel gestore eventi del pulsante s il `ProtectSection(provider)` metodo in modo che viene utilizzato il provider DPAPI. Il `UnprotectSection` metodo consente di determinare il provider che è stato usato per crittografare la sezione di configurazione e pertanto non richiede eventuali parametri di input.

Dopo la chiamata di `ProtectSection(provider)` o `UnprotectSection` metodo, è necessario chiamare il `Configuration` oggetto s [ `Save` metodo](https://msdn.microsoft.com/library/system.configuration.configuration.save.aspx) per rendere permanenti le modifiche. Una volta le informazioni di configurazione sono state crittografate o decrittografate e le modifiche salvate, chiamiamo `DisplayWebConfig` caricare aggiornato `Web.config` contenuto nel controllo casella di testo.

Dopo aver immesso il codice sopra riportato, testarla visitando il `EncryptingConfigSections.aspx` pagina tramite un browser. Inizialmente viene visualizzata una pagina che elenca il contenuto della `Web.config` con il `<connectionStrings>` sezione visualizzati in testo normale (vedere la figura 3).


[![Aggiungere una casella di testo e due controlli Web pulsante alla pagina](protecting-connection-strings-and-other-configuration-information-vb/_static/image8.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image7.png)

**Figura 3**: aggiungere una casella di testo e due controlli Web pulsante alla pagina ([fare clic per visualizzare l'immagine con dimensioni normali](protecting-connection-strings-and-other-configuration-information-vb/_static/image9.png))


Fare clic sul pulsante di crittografare le stringhe di connessione. Se la convalida delle richieste è abilitata, il markup restituita dal `WebConfigContents` casella di testo produrrà un' `HttpRequestValidationException`, che viene visualizzato il messaggio, potenzialmente pericoloso `Request.Form` valore rilevato dal client. Convalida delle richieste, che è abilitata per impostazione predefinita in ASP.NET 2.0, non consente i postback che includono HTML non codificato ed è progettata per aiutare a evitare attacchi script injection. È possibile disabilitare questo controllo alla pagina - o a livello di applicazione. Per disattivarla per questa pagina, impostare il `ValidateRequest` se si imposta su `False` nel `@Page` direttiva. Il `@Page` direttiva si trova nella parte superiore di markup dichiarativo pagina s.


[!code-aspx[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample3.aspx)]

Per ulteriori informazioni sulla convalida della richiesta, il suo scopo, come disabilitarla nella pagina - e a livello di applicazione, come e su come codificare markup in formato HTML, vedere [richiesta di convalida - impedisce attacchi Script](../../../../whitepapers/request-validation.md).

Dopo aver disabilitato la convalida delle richieste per la pagina, provare a fare nuovamente clic sul pulsante di crittografare le stringhe di connessione. Durante il postback, si accederà il file di configurazione e la relativa `<connectionStrings>` sezione crittografata con il provider DPAPI. La casella di testo viene quindi aggiornato per visualizzare il nuovo `Web.config` contenuto. Come illustrato nella figura 4, il `<connectionStrings>` informazioni sono ora crittografate.


[![Facendo clic la Crittografa connessione stringhe pulsante Crittografa il &lt;connectionString&gt; sezione](protecting-connection-strings-and-other-configuration-information-vb/_static/image11.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image10.png)

**Figura 4**: facendo clic la Crittografa connessione stringhe pulsante Crittografa il `<connectionString>` sezione ([fare clic per visualizzare l'immagine con dimensioni normali](protecting-connection-strings-and-other-configuration-information-vb/_static/image12.png))


Crittografata `<connectionStrings>` segue sezione generati sul mio computer, sebbene alcuni contenuti nel `<CipherData>` elemento è stato rimosso per motivi di brevità:


[!code-xml[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample4.xml)]

> [!NOTE]
> Il `<connectionStrings>` elemento specifica il provider utilizzato per eseguire la crittografia (`DataProtectionConfigurationProvider`). Queste informazioni vengono usate per il `UnprotectSection` metodo quando si fa clic sul pulsante decrittografare le stringhe di connessione.


Quando le informazioni sulla stringa di connessione si accede da `Web.config` - in base al codice che abbiamo scritto, da un controllo SqlDataSource, o il codice generato automaticamente dagli oggetti TableAdapter in set di dati tipizzato - verrà decrittografato automaticamente. In breve, non dobbiamo aggiungere alcun codice aggiuntivo o per la logica per decrittografare il crittografato `<connectionString>` sezione. Per dimostrare questo concetto, visitare una delle esercitazioni precedenti in questa fase, ad esempio l'esercitazione di visualizzazione semplice nella sezione Basic Reporting (`~/BasicReporting/SimpleDisplay.aspx`). Come illustrato nella figura 5, l'esercitazione funziona esattamente come si può immaginare, che indica che le informazioni sulla stringa di connessione crittografata viene automaticamente decrittografate dalla pagina ASP.NET.


[![Il livello di accesso ai dati decrittografa automaticamente le informazioni sulla stringa di connessione](protecting-connection-strings-and-other-configuration-information-vb/_static/image14.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image13.png)

**Figura 5**: livello di accesso ai dati decrittografa automaticamente le informazioni sulla stringa di connessione ([fare clic per visualizzare l'immagine con dimensioni normali](protecting-connection-strings-and-other-configuration-information-vb/_static/image15.png))


Per ripristinare il `<connectionStrings>` sezione alla relativa rappresentazione di testo normale, fare clic sul pulsante di decrittografare le stringhe di connessione. Durante il postback dovrebbe le stringhe di connessione in `Web.config` in testo normale. A questo punto la schermata dovrebbe essere simile a quella prima di tutto gli utenti in visita questa pagina (vedere la figura 3).

## <a name="step-3-encrypting-configuration-sections-usingaspnetregiisexe"></a>Passaggio 3: Crittografare sezioni di configurazione usando`aspnet_regiis.exe`

.NET Framework include una vasta gamma di strumenti da riga di comando nel `$WINDOWS$\Microsoft.NET\Framework\version\` cartella. Nel [usando le dipendenze della Cache di SQL](../caching-data/using-sql-cache-dependencies-vb.md) esercitazione, ad esempio, è stata esaminata tramite il `aspnet_regsql.exe` strumento da riga di comando per aggiungere l'infrastruttura necessaria per la dipendenze della cache SQL. Un altro strumento utile riga di comando in questa cartella è il [dello strumento di registrazione di ASP.NET in IIS (`aspnet_regiis.exe`)](https://msdn.microsoft.com/library/k6h9cz8h(VS.80).aspx). Come suggerisce il nome, lo strumento di registrazione di ASP.NET in IIS viene utilizzato principalmente per registrare un'applicazione ASP.NET 2.0 con server di Web di Microsoft s livello professionale, IIS. Oltre alle relative funzionalità correlate a IIS, lo strumento di registrazione di ASP.NET in IIS è anche utilizzabile per crittografare o decrittografare le sezioni di configurazione specificato `Web.config`.

L'istruzione seguente viene illustrata la sintassi generale utilizzata per crittografare una sezione di configurazione con il `aspnet_regiis.exe` strumento da riga di comando:


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample5.cmd)]

*sezione* è la sezione di configurazione per la crittografia (ad esempio connectionStrings), il *fisico\_directory* è il percorso fisico completo alla directory radice dell'applicazione s, web e *provider*  è il nome del provider di configurazione protetta da usare (ad esempio DataProtectionConfigurationProvider). In alternativa, se è registrata l'applicazione web in IIS è possibile immettere il percorso virtuale anziché il percorso fisico usando la sintassi seguente:


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample6.cmd)]

Quanto segue `aspnet_regiis.exe` nell'esempio seguente il `<connectionStrings>` sezione Utilizzo del provider DPAPI con una chiave a livello di computer:


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample7.cmd)]

Analogamente, il `aspnet_regiis.exe` strumento della riga di comando può essere usato per decrittografare le sezioni di configurazione. Invece di usare la `-pef` interruttore, utilizzare `-pdf` (o instead of `-pe`, usare `-pd`). Si noti inoltre che il nome del provider non è necessario durante la decrittografia.


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample8.cmd)]

> [!NOTE]
> Poiché si sta usando il provider di DPAPI che usa chiavi specifiche per il computer, è necessario eseguire `aspnet_regiis.exe` dallo stesso computer da cui vengono gestite le pagine web. Ad esempio, se si esegue il programma della riga di comando dal computer di sviluppo locale e quindi carica il file Web. config crittografato al server di produzione, il server di produzione non sarà in grado di decrittografare le informazioni sulla stringa di connessione perché è stato crittografato uso delle chiavi specifiche di computer di sviluppo. Il provider RSA è privo di questa limitazione come è possibile esportare le chiavi RSA a un'altra macchina.


## <a name="understanding-database-authentication-options"></a>Informazioni sulle opzioni di autenticazione Database

Prima di qualsiasi applicazione può inviare `SELECT`, `INSERT`, `UPDATE`, o `DELETE` query a un database di Microsoft SQL Server, il database prima di tutto necessario identificare il richiedente. Questo processo è noto come *autenticazione* e SQL Server sono disponibili due metodi di autenticazione:

- **L'autenticazione di Windows** -il processo con cui viene eseguita l'applicazione viene utilizzato per comunicare con il database. Quando si esegue un'applicazione ASP.NET tramite Visual Studio 2005 s ASP.NET Development Server, l'applicazione ASP.NET assume l'identità dell'utente attualmente connesso. Per le applicazioni ASP.NET in Microsoft Internet Information Server (IIS), le applicazioni ASP.NET è in genere assumere l'identità del `domainName``\MachineName` o `domainName``\NETWORK SERVICE`, anche se può essere personalizzato.
- **L'autenticazione di SQL** -vengono forniti i valori di ID e la password un utente come credenziali per l'autenticazione. Con l'autenticazione SQL e vengono forniti l'ID utente e password nella stringa di connessione.

L'autenticazione di Windows è preferibile rispetto all'autenticazione di SQL perché è più sicuro. Con l'autenticazione di Windows la stringa di connessione sia priva di nome utente e password e se il server web e server di database si trovano in due computer diversi, le credenziali non vengono inviate attraverso la rete in testo normale. Con l'autenticazione SQL, tuttavia, le credenziali di autenticazione sono impostati come hardcoded nella stringa di connessione e vengono trasmessi dal server web al server di database in testo normale.

Queste esercitazioni usare l'autenticazione di Windows. È possibile indicare le modalità di autenticazione utilizzata controllando la stringa di connessione. La stringa di connessione in `Web.config` per le esercitazioni è stata:

`Data Source=.\SQLEXPRESS; AttachDbFilename=|DataDirectory|\NORTHWND.MDF; Integrated Security=True; User Instance=True`

La sicurezza integrata = True e la mancanza di un nome utente e una password indica che viene viene utilizzata l'autenticazione di Windows. In alcuni connessione stringhe di connessione Trusted il termine = Yes o la sicurezza integrata = SSPI viene usato invece Integrated Security = True, ma tutti e tre indicano l'uso dell'autenticazione di Windows.

Nell'esempio seguente mostra una stringa di connessione che usa l'autenticazione SQL. Tenere presente le credenziali incorporate all'interno della stringa di connessione:

`Server=serverName; Database=Northwind; uid=userID; pwd=password`

Si supponga che un utente malintenzionato è in grado di visualizzare la s applicazione `Web.config` file. Se si usa l'autenticazione di SQL per connettersi a un database che è accessibile tramite Internet, l'utente malintenzionato può usare questa stringa di connessione per connettersi al database tramite SQL Management Studio o dalle pagine di ASP.NET nel proprio sito Web. Per mitigare questa minaccia, crittografare le informazioni sulla stringa di connessione in `Web.config` utilizzando il sistema di configurazione protetta.

> [!NOTE]
> Per altre informazioni sui diversi tipi di autenticazione disponibile in SQL Server, vedere [Building Secure ASP.NET Applications: autenticazione, autorizzazione e comunicazioni protette](https://msdn.microsoft.com/library/aa302392.aspx). Per ulteriori esempi stringhe di connessione che illustra le differenze tra la sintassi di autenticazione di Windows e SQL, consultare [ConnectionStrings.com](http://www.connectionstrings.com/).


## <a name="summary"></a>Riepilogo

Per impostazione predefinita, i file con un `.config` estensione in un'applicazione ASP.NET non sono accessibili attraverso un browser. Questi tipi di file non vengono restituiti perché potrebbe contenere informazioni riservate, ad esempio le stringhe di connessione di database, i nomi utente e password e così via. Il sistema di configurazione protetta in .NET 2.0 consente di proteggere ulteriormente le informazioni riservate, consentendo a sezioni di configurazione specificato deve essere crittografato. Sono disponibili due provider di configurazione protetta predefiniti: uno che utilizza l'algoritmo RSA e uno che utilizza Windows Data Protection API (DPAPI).

In questa esercitazione è stato illustrato come crittografare e decrittografare le impostazioni di configurazione utilizzando il provider DPAPI. Questa operazione può essere eseguita sia a livello di programmazione, come mostrato nel passaggio 2, nonché mediante il `aspnet_regiis.exe` strumento da riga di comando, che è stato analizzato nel passaggio 3. Per altre informazioni su utilizzo di chiavi a livello di utente o usando invece il provider RSA, vedere le risorse nella sezione di letture di approfondimento.

Buona programmazione!

## <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Applicazioni ASP.NET protette predefiniti: Autenticazione, autorizzazione e comunicazioni protette](https://msdn.microsoft.com/library/aa302392.aspx)
- [La crittografia delle informazioni di configurazione in ASP.NET 2.0 Applications](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)
- [La crittografia `Web.config` valori in ASP.NET 2.0](https://weblogs.asp.net/scottgu/archive/2006/01/09/434893.aspx)
- [Procedura: Crittografare sezioni di configurazione in ASP.NET 2.0 usando DPAPI](https://msdn.microsoft.com/library/ms998280.aspx)
- [Procedura: Crittografare sezioni di configurazione in ASP.NET 2.0 utilizzando RSA](https://msdn.microsoft.com/library/ms998283.aspx)
- [L'API di configurazione in .NET 2.0](http://www.odetocode.com/Articles/418.aspx)
- [Protezione dei dati di Windows](https://msdn.microsoft.com/library/ms995355.aspx)

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. I revisori per questa esercitazione sono state Teresa Murphy e Randy Schmidt. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb.md)
> [Successivo](debugging-stored-procedures-vb.md)
