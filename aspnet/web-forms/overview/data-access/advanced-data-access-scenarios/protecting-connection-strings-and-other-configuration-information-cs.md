---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/protecting-connection-strings-and-other-configuration-information-cs
title: Proteggere le stringhe di connessione e altre informazioni di configurazione (c#) | Documenti Microsoft
author: rick-anderson
description: Un'applicazione ASP.NET, le informazioni di configurazione vengono in genere archiviati in un file Web. config. Alcune di queste informazioni riservate e garantisce la protezione. Da def....
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: ad8dd396-30f7-4abe-ac02-a0b84422e5be
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/protecting-connection-strings-and-other-configuration-information-cs
msc.type: authoredcontent
ms.openlocfilehash: 20a18a36cb5d1621b0b718f87c05eb3175110143
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="protecting-connection-strings-and-other-configuration-information-c"></a>Proteggere le stringhe di connessione e altre informazioni di configurazione (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare codice](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_73_CS.zip) o [Scarica il PDF](protecting-connection-strings-and-other-configuration-information-cs/_static/datatutorial73cs1.pdf)

> Un'applicazione ASP.NET, le informazioni di configurazione vengono in genere archiviati in un file Web. config. Alcune di queste informazioni riservate e garantisce la protezione. Per impostazione predefinita questo file non verrà utilizzato per un visitatore di un sito Web, ma un amministratore o un utente malintenzionato può accedere al file system del server Web e visualizzare il contenuto del file. In questa esercitazione viene illustrato che ASP.NET 2.0 consente di proteggere le informazioni riservate tramite la crittografia delle sezioni del file Web. config.


## <a name="introduction"></a>Introduzione

Informazioni di configurazione per le applicazioni ASP.NET sono solitamente archiviate in un file XML denominato `Web.config`. Nel corso di queste esercitazioni sono state aggiornate le `Web.config` un numero limitato di volte. Durante la creazione il `Northwind` DataSet tipizzato nel [prima esercitazione](../introduction/creating-a-data-access-layer-cs.md), ad esempio, informazioni sulla stringa di connessione è stato automaticamente aggiunto a `Web.config` nel `<connectionStrings>` sezione. Più avanti nel [pagine Master e esplorazione del sito](../introduction/master-pages-and-site-navigation-cs.md) esercitazione è aggiornato manualmente `Web.config`, aggiungendo un `<pages>` elemento che indica che tutte le pagine ASP.NET nel progetto deve utilizzare il `DataWebControls` tema.

Poiché `Web.config` potrebbe contenere dati riservati, ad esempio le stringhe di connessione, è importante che il contenuto di `Web.config` rimanere nascosto agli utenti non autorizzati e protetto. Per impostazione predefinita, qualsiasi HTTP richiesta in un file con il `.config` viene gestita dal motore di ASP.NET, che restituisce il *questo tipo di pagina non è servito* messaggio illustrato nella figura 1. Ciò significa che non è possibile visualizzare i visitatori del `Web.config` s contenuto del file immettendo semplicemente http://www.YourServer.com/Web.config nella barra degli indirizzi del browser s.


[![Visita di Web. config tramite un Browser restituisce un questo tipo di pagina non è servito messaggio](protecting-connection-strings-and-other-configuration-information-cs/_static/image2.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image1.png)

**Figura 1**: visitare `Web.config` tramite un Browser restituisce un questo tipo di pagina non è servita messaggio ([fare clic per visualizzare l'immagine ingrandita](protecting-connection-strings-and-other-configuration-information-cs/_static/image3.png))


Ma cosa accade se un utente malintenzionato è in grado di trovare alcuni altri exploit che consente di visualizzare il `Web.config` s contenuto del file? Cosa può fare con queste informazioni un utente malintenzionato e le misure può essere eseguite per proteggere ulteriormente le informazioni sensibili all'interno di `Web.config`? Fortunatamente, la maggior parte delle sezioni in `Web.config` non contengono informazioni riservate. I danni è un utente malintenzionato selettivo se si conosce il nome del tema usato da pagine ASP.NET predefinito?

Determinati `Web.config` sezioni, tuttavia, contengono informazioni riservate che possono includere stringhe di connessione, i nomi utente, password, i nomi dei server, le chiavi di crittografia e così via. Queste informazioni si trova in genere nell'esempio seguente `Web.config` sezioni:

- `<appSettings>`
- `<connectionStrings>`
- `<identity>`
- `<sessionState>`

In questa esercitazione verranno esaminati tecniche per proteggere tali informazioni di configurazione sensibili. Come si vedrà, .NET Framework versione 2.0 include un sistema di configurazioni protetto che rende molto semplice di sezioni di configurazione selezionato a livello di codice con crittografia e decrittografia.

> [!NOTE]
> Questa esercitazione si conclude con suggerimenti di Microsoft s per la connessione a un database da un'applicazione ASP.NET. Oltre a crittografare le stringhe di connessione, ti consente di finalizzare il sistema, verificare che ci si connette al database in modo sicuro.


## <a name="step-1-exploring-aspnet-20-s-protected-configuration-options"></a>Passaggio 1: Esplorazione di ASP.NET 2.0 s protetto le opzioni di configurazione

ASP.NET 2.0 include un sistema di configurazione protetta per crittografare e decrittografare le informazioni di configurazione. Questo include i metodi in .NET Framework che può essere utilizzato a livello di programmazione crittografare o decrittografare le informazioni di configurazione. Il sistema di configurazione protetta utilizza il [modello provider](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), che consente agli sviluppatori di scegliere quale implementazione di crittografia viene utilizzata.

.NET Framework sono disponibili due provider di configurazione protetta:

- [`RSAProtectedConfigurationProvider`](https://msdn.microsoft.com/library/system.configuration.rsaprotectedconfigurationprovider.aspx) -Usa l'asimmetrica [algoritmo RSA](http://en.wikipedia.org/wiki/Rsa) per crittografia e decrittografia.
- [`DPAPIProtectedConfigurationProvider`](https://msdn.microsoft.com/system.configuration.dpapiprotectedconfigurationprovider.aspx) -Usa Windows [Data Protection API (DPAPI)](https://msdn.microsoft.com/library/ms995355.aspx) per crittografia e decrittografia.

Poiché il sistema di configurazione protetto implementa il modello di progettazione del provider, è possibile creare un provider di configurazione protetto e inserirlo nell'applicazione. Vedere [implementando un Provider di configurazione protetta](https://msdn.microsoft.com/library/wfc2t3az(VS.80).aspx) per ulteriori informazioni su questo processo.

I provider RSA e DPAPI Usa chiavi per le routine di crittografia e decrittografia, e queste chiavi possono essere archiviate al livello computer o utente. Chiavi a livello di computer sono ideali per gli scenari in cui l'applicazione web viene eseguito in un server dedicato o se sono presenti più applicazioni in un server che devono condividere informazioni crittografate. Chiavi a livello di utente sono un'opzione più sicura in ambienti di hosting condivisi in altre applicazioni sullo stesso server non devono essere in grado di decrittografare le sezioni di configurazione applicazione s protetto.

In questa esercitazione si utilizzerà negli esempi di provider DPAPI e chiavi a livello di computer. In particolare, verranno esaminati la crittografia di `<connectionStrings>` sezione `Web.config`, anche se il sistema di configurazione protetta può essere utilizzato per crittografare la maggior parte delle eventuali `Web.config` sezione. Per informazioni sull'uso di chiavi a livello di utente o tramite il provider RSA, consultare le risorse nella sezione ulteriori letture alla fine di questa esercitazione.

> [!NOTE]
> Il `RSAProtectedConfigurationProvider` e `DPAPIProtectedConfigurationProvider` provider sono registrati nel `machine.config` file con i nomi di provider `RsaProtectedConfigurationProvider` e `DataProtectionConfigurationProvider`, rispettivamente. Durante la crittografia o decrittografia di informazioni di configurazione, è necessario specificare il nome del provider appropriato (`RsaProtectedConfigurationProvider` o `DataProtectionConfigurationProvider`) anziché il nome effettivo del tipo (`RSAProtectedConfigurationProvider` e `DPAPIProtectedConfigurationProvider`). È possibile trovare il `machine.config` file nel `$WINDOWS$\Microsoft.NET\Framework\version\CONFIG` cartella.


## <a name="step-2-programmatically-encrypting-and-decrypting-configuration-sections"></a>Passaggio 2: La crittografia a livello di codice e la decrittografia delle sezioni di configurazione

Con poche righe di codice è possibile crittografare o decrittografare una sezione di configurazione specifico utilizzando un provider specificato. Il codice, come verrà illustrato più avanti, deve semplicemente a livello di codice fa riferimento la sezione di configurazione appropriata, chiamare il relativo `ProtectSection` o `UnprotectSection` (metodo) e quindi chiamare il `Save` metodo per rendere permanenti le modifiche. Inoltre, .NET Framework include un'utilità della riga di comando utili che è possibile crittografare e decrittografare le informazioni di configurazione. Esamineremo questa utilità della riga di comando nel passaggio 3.

Per illustrare le informazioni sulla configurazione di protezione a livello di codice, s consentono di creare una pagina ASP.NET che include i pulsanti per crittografare e decrittografare il `<connectionStrings>` sezione `Web.config`.

Aprire il `EncryptingConfigSections.aspx` nella pagina di `AdvancedDAL` cartella. Trascinare un controllo casella di testo dalla casella degli strumenti nella finestra di progettazione, l'impostazione relativa `ID` proprietà `WebConfigContents`, l'oggetto `TextMode` proprietà `MultiLine`e il relativo `Width` e `Rows` proprietà 95% e 15, rispettivamente. Il controllo casella di testo verrà visualizzato il contenuto di `Web.config` che consente di visualizzare rapidamente se il contenuto crittografato o non. Naturalmente, in un'applicazione reale si mai desidera visualizzare il contenuto di `Web.config`.

Sotto la casella di testo, aggiungere due controlli Button denominati `EncryptConnStrings` e `DecryptConnStrings`. Impostare le proprietà di testo per le stringhe di connessione di crittografare e decrittografare le stringhe di connessione.

A questo punto la schermata dovrebbe essere simile alla figura 2.


[![Aggiungere una casella di testo e due controlli Web pulsante alla pagina](protecting-connection-strings-and-other-configuration-information-cs/_static/image5.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image4.png)

**Figura 2**: aggiungere una casella di testo e due controlli Web di pulsante ([fare clic per visualizzare l'immagine ingrandita](protecting-connection-strings-and-other-configuration-information-cs/_static/image6.png))


Successivamente, è necessario scrivere codice che carica e visualizza il contenuto di `Web.config` nel `WebConfigContents` casella di testo quando la pagina viene caricata. Aggiungere il codice seguente per la classe code-behind s di pagina. Questo codice aggiunge un metodo denominato `DisplayWebConfig` e viene chiamato dal `Page_Load` gestore dell'evento quando `Page.IsPostBack` è `false`:


[!code-csharp[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample1.cs)]

Il `DisplayWebConfig` metodo utilizza il [ `File` classe](https://msdn.microsoft.com/library/system.io.file.aspx) per aprire l'applicazione s `Web.config` file, il [ `StreamReader` classe](https://msdn.microsoft.com/library/system.io.streamreader.aspx) per leggere il contenuto in una stringa e il [ `Path` classe](https://msdn.microsoft.com/library/system.io.path.aspx) per generare il percorso fisico di `Web.config` file. Queste tre classi sono tutti contenute nel [ `System.IO` dello spazio dei nomi](https://msdn.microsoft.com/library/system.io.aspx). Di conseguenza, sarà necessario aggiungere un `using` `System.IO` istruzione per l'inizio della classe code-behind o, in alternativa, queste classi di nomi con prefisso `System.IO.` .

Successivamente, è necessario aggiungere gestori eventi per i due controlli pulsante `Click` gli eventi e aggiungere il codice necessario per crittografare e decrittografare il `<connectionStrings>` sezione usando una chiave a livello di computer con il provider DPAPI. Dalla finestra di progettazione, fare doppio clic su ciascun pulsante per aggiungere un `Click` gestore nel code-behind classe e quindi aggiungere il codice seguente:


[!code-csharp[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample2.cs)]

Il codice utilizzato nei gestori di due eventi è quasi identico. Entrambi vengono avviati tramite il recupero delle informazioni relative a s l'applicazione corrente `Web.config` file tramite il [ `WebConfigurationManager` classe](https://msdn.microsoft.com/library/system.web.configuration.webconfigurationmanager.aspx) s [ `OpenWebConfiguration` metodo](https://msdn.microsoft.com/library/system.web.configuration.webconfigurationmanager.openwebconfiguration.aspx). Questo metodo restituisce il file di configurazione web per il percorso virtuale specificato. Successivamente, il `Web.config` file s `<connectionStrings>` sezione è possibile accedere tramite il [ `Configuration` classe](https://msdn.microsoft.com/library/system.configuration.configuration.aspx) s [ `GetSection(sectionName)` metodo](https://msdn.microsoft.com/library/system.configuration.configuration.getsection.aspx), che restituisce un [ `ConfigurationSection` ](https://msdn.microsoft.com/library/system.configuration.configurationsection.aspx) oggetto.

Il `ConfigurationSection` oggetto include un [ `SectionInformation` proprietà](https://msdn.microsoft.com/library/system.configuration.configurationsection.sectioninformation.aspx) che fornisce informazioni aggiuntive e le funzionalità relative alla sezione di configurazione. Come il codice precedente, è possibile determinare se la sezione di configurazione è crittografata controllando il `SectionInformation` proprietà s `IsProtected` proprietà. Inoltre, può essere crittografata o decrittografata mediante la sezione di `SectionInformation` proprietà s `ProtectSection(provider)` e `UnprotectSection` metodi.

Il `ProtectSection(provider)` metodo accetta come input una stringa che specifica il nome del provider di configurazione protetta da utilizzare durante la crittografia. Nel `EncryptConnString` passiamo DataProtectionConfigurationProvider nel gestore eventi del pulsante s il `ProtectSection(provider)` metodo in modo che viene utilizzato il provider DPAPI. Il `UnprotectSection` metodo consente di determinare il provider utilizzato per crittografare la sezione di configurazione e pertanto non richiede i parametri di input.

Dopo la chiamata di `ProtectSection(provider)` o `UnprotectSection` (metodo), è necessario chiamare il `Configuration` oggetto s [ `Save` metodo](https://msdn.microsoft.com/library/system.configuration.configuration.save.aspx) per rendere permanenti le modifiche. Una volta le informazioni di configurazione crittografate o decrittografate e le modifiche salvate, chiamare `DisplayWebConfig` caricare aggiornato `Web.config` contenuto nel controllo TextBox.

Dopo aver immesso il codice sopra riportato, testarlo visitando il `EncryptingConfigSections.aspx` pagina tramite un browser. Inizialmente verrà visualizzata una pagina che elenca il contenuto della `Web.config` con il `<connectionStrings>` sezione visualizzati in testo normale (vedere la figura 3).


[![Aggiungere una casella di testo e due controlli Web pulsante alla pagina](protecting-connection-strings-and-other-configuration-information-cs/_static/image8.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image7.png)

**Figura 3**: aggiungere una casella di testo e due controlli Web di pulsante ([fare clic per visualizzare l'immagine ingrandita](protecting-connection-strings-and-other-configuration-information-cs/_static/image9.png))


Fare clic sul pulsante di crittografare le stringhe di connessione. Se la convalida delle richieste è abilitata, il codice eseguito il postback dal `WebConfigContents` TextBox produrrà un `HttpRequestValidationException`, che viene visualizzato il messaggio, potenzialmente pericoloso `Request.Form` valore rilevato dal client. La convalida delle richieste, che è abilitata per impostazione predefinita in ASP.NET 2.0, impedisce i postback che includono non codificato in formato HTML ed è progettata per aiutare a evitare attacchi script injection. È possibile disabilitare questo controllo alla pagina - o a livello di applicazione. Per disattivare l'opzione per questa pagina, impostare il `ValidateRequest` impostando su `false` nel `@Page` direttiva. Il `@Page` direttiva si trova nella parte superiore del markup dichiarativo pagina s.


[!code-aspx[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample3.aspx)]

Per ulteriori informazioni sulla convalida della richiesta, lo scopo, come disabilitarla nella pagina - e a livello di applicazione, come anche come codificare markup HTML, vedere [la convalida della richiesta - impedisce attacchi Script](../../../../whitepapers/request-validation.md).

Dopo aver disabilitato la convalida delle richieste per la pagina, provare a fare di nuovo clic sul pulsante crittografare stringhe di connessione. Durante il postback, si accederà il file di configurazione e il relativo `<connectionStrings>` sezione crittografati utilizzando il provider DPAPI. La casella di testo viene quindi aggiornato per visualizzare il nuovo `Web.config` contenuto. Come illustrato nella figura 4, il `<connectionStrings>` informazioni ora sono crittografate.


[![Scegliere di crittografare connessione stringhe pulsante Crittografa il &lt;connectionString&gt; sezione](protecting-connection-strings-and-other-configuration-information-cs/_static/image11.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image10.png)

**Figura 4**: scegliere di crittografare connessione stringhe pulsante Crittografa il `<connectionString>` sezione ([fare clic per visualizzare l'immagine ingrandita](protecting-connection-strings-and-other-configuration-information-cs/_static/image12.png))


Crittografata `<connectionStrings>` segue sezione generati nel computer, anche se alcuni contenuti nel `<CipherData>` elemento è stato rimosso per ragioni di brevità:


[!code-xml[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample4.xml)]

> [!NOTE]
> Il `<connectionStrings>` elemento specifica il provider utilizzato per eseguire la crittografia (`DataProtectionConfigurationProvider`). Queste informazioni vengono utilizzate per la `UnprotectSection` metodo quando si fa clic sul pulsante decrittografare le stringhe di connessione.


Quando le informazioni sulla stringa di connessione si accede da `Web.config` - tramite codice è scritto da un controllo SqlDataSource, o il codice generato automaticamente dagli oggetti TableAdapter nei dataset tipizzati - è stata decrittografata automaticamente. In breve, non è necessario aggiungere il codice aggiuntivo o la logica per decrittografare crittografata `<connectionString>` sezione. Per dimostrare questo concetto, visitare una delle esercitazioni precedenti a questo punto, ad esempio l'esercitazione di visualizzazione semplice dalla sezione Reporting di base (`~/BasicReporting/SimpleDisplay.aspx`). Come illustrato nella figura 5, l'esercitazione funziona esattamente come previsto, che indica che le informazioni sulla stringa di connessione crittografata viene automaticamente decrittografate dalla pagina ASP.NET.


[![Livello di accesso ai dati decrittografa automaticamente le informazioni sulla stringa di connessione](protecting-connection-strings-and-other-configuration-information-cs/_static/image14.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image13.png)

**Figura 5**: livello di accesso ai dati decrittografa automaticamente le informazioni sulla stringa di connessione ([fare clic per visualizzare l'immagine ingrandita](protecting-connection-strings-and-other-configuration-information-cs/_static/image15.png))


Per ripristinare il `<connectionStrings>` sezione torna nella relativa rappresentazione di testo normale, fare clic sul pulsante di decrittografare le stringhe di connessione. Durante il postback dovrebbe essere stringhe di connessione `Web.config` in testo normale. A questo punto la schermata dovrebbe essere analoga a quella durante la prima visita questa pagina (vedere Figura 3).

## <a name="step-3-encrypting-configuration-sections-usingaspnetregiisexe"></a>Passaggio 3: Crittografare sezioni di configurazione tramite`aspnet_regiis.exe`

.NET Framework include una varietà di strumenti da riga di comando nel `$WINDOWS$\Microsoft.NET\Framework\version\` cartella. Nel [utilizzando le dipendenze della Cache di SQL](../caching-data/using-sql-cache-dependencies-cs.md) esercitazione, ad esempio, è descritto l'utilizzo di `aspnet_regsql.exe` strumento da riga di comando per aggiungere l'infrastruttura necessaria per le dipendenze della cache SQL. Un altro strumento da riga di comando utili in questa cartella è la [strumento di registrazione IIS ASP.NET (`aspnet_regiis.exe`)](https://msdn.microsoft.com/library/k6h9cz8h(VS.80).aspx). Come suggerisce il nome, lo strumento di registrazione di ASP.NET in IIS viene utilizzato principalmente per registrare un'applicazione ASP.NET 2.0 con server di Web di Microsoft s professionali, IIS. Oltre la funzionalità correlate a IIS, lo strumento di registrazione di ASP.NET in IIS è possibile usare anche per crittografare o decrittografare le sezioni di configurazione specificato `Web.config`.

L'istruzione seguente illustra la sintassi generale utilizzata per crittografare una sezione di configurazione con il `aspnet_regiis.exe` strumento da riga di comando:


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample5.cmd)]

*sezione* è la sezione di configurazione per la crittografia (ad esempio connectionStrings), il *fisico\_directory* è il percorso fisico completo alla directory radice dell'applicazione s web, e *provider*  è il nome del provider di configurazione protetta da utilizzare (ad esempio DataProtectionConfigurationProvider). In alternativa, se l'applicazione web è registrato in IIS è possibile immettere il percorso virtuale anziché il percorso fisico utilizzando la sintassi seguente:


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample6.cmd)]

Nell'esempio `aspnet_regiis.exe` esempio crittografa il `<connectionStrings>` sezione Utilizzo del provider DPAPI con una chiave a livello di computer:


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample7.cmd)]

Analogamente, il `aspnet_regiis.exe` strumento da riga di comando può essere utilizzato per decrittografare le sezioni di configurazione. Anziché utilizzare il `-pef` interruttore, utilizzare `-pdf` (o instead of `-pe`, utilizzare `-pd`). Si noti inoltre che il nome del provider non è necessario durante la decrittografia.


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample8.cmd)]

> [!NOTE]
> Poiché si sta utilizzando il provider DPAPI, che usa chiavi specifiche per il computer, è necessario eseguire `aspnet_regiis.exe` nello stesso computer da cui vengono gestite le pagine web. Ad esempio, se si esegue il programma della riga di comando dal computer di sviluppo locale e quindi carica il file Web. config crittografato al server di produzione, il server di produzione non sarà in grado di decrittografare le informazioni sulla stringa di connessione, poiché è stato crittografato uso di chiavi specifiche per il computer di sviluppo. Il provider RSA dispone di questa limitazione è possibile esportare le chiavi RSA in un altro computer.


## <a name="understanding-database-authentication-options"></a>Informazioni sulle opzioni di autenticazione Database

Prima di qualsiasi applicazione può eseguire `SELECT`, `INSERT`, `UPDATE`, o `DELETE` query a un database di Microsoft SQL Server, il database prima di tutto necessario identificare il richiedente. Questo processo è noto come *autenticazione* e SQL Server sono disponibili due metodi di autenticazione:

- **L'autenticazione di Windows** -il processo in cui viene eseguita l'applicazione viene utilizzato per comunicare con il database. Quando si esegue un'applicazione ASP.NET tramite Visual Studio 2005 s Server di sviluppo ASP.NET, l'applicazione ASP.NET si presuppone che l'identità dell'utente attualmente connesso. Per le applicazioni ASP.NET in Microsoft Internet Information Server (IIS), applicazioni ASP.NET è in genere assumere l'identità di `domainName``\MachineName` o `domainName``\NETWORK SERVICE`, anche se può essere personalizzata.
- **L'autenticazione di SQL** -vengono forniti i valori di ID e password un utente come credenziali per l'autenticazione. Con l'autenticazione SQL, l'ID utente e password vengono forniti nella stringa di connessione.

L'autenticazione di Windows è preferibile l'autenticazione di SQL perché è più sicuro. Con l'autenticazione di Windows è disponibile da un nome utente e una password della stringa di connessione e se il server web e il server di database si trovano in due computer diversi, le credenziali non vengono inviate in rete in testo normale. Con l'autenticazione SQL, tuttavia, le credenziali di autenticazione sono hardcoded nella stringa di connessione e vengono trasmessi dal server web al server di database in testo normale.

Queste esercitazioni utilizzata l'autenticazione Windows. È possibile stabilire la modalità di autenticazione è utilizzata controllando la stringa di connessione. La stringa di connessione in `Web.config` per le esercitazioni è stato:

`Data Source=.\SQLEXPRESS; AttachDbFilename=|DataDirectory|\NORTHWND.MDF; Integrated Security=True; User Instance=True`

La sicurezza integrata = True e la mancanza di un nome utente e password indicano che viene utilizzata l'autenticazione di Windows. In una connessione stringhe il termine Trusted Connection = Yes o la sicurezza integrata = SSPI viene utilizzato invece della sicurezza integrata = True, ma tutti e tre indicano l'utilizzo dell'autenticazione di Windows.

Nell'esempio seguente viene illustrata una stringa di connessione che utilizza l'autenticazione SQL. Tenere presente le credenziali incorporate all'interno della stringa di connessione:

`Server=serverName; Database=Northwind; uid=userID; pwd=password`

Si supponga che un utente malintenzionato sia in grado di visualizzare l'applicazione s `Web.config` file. Se si utilizza l'autenticazione di SQL per connettersi a un database che è accessibile tramite Internet, l'autore dell'attacco è possibile utilizzare la stringa di connessione per connettersi al database tramite SQL Management Studio o da pagine ASP.NET nel proprio sito Web. Per attenuare questa minaccia, crittografare le informazioni sulla stringa di connessione in `Web.config` utilizzando il sistema di configurazione protetta.

> [!NOTE]
> Per ulteriori informazioni sui diversi tipi di autenticazione disponibile in SQL Server, vedere [creazione di applicazioni ASP.NET sicure: autenticazione, autorizzazione e comunicazioni protette](https://msdn.microsoft.com/library/aa302392.aspx). Per ulteriori esempi stringhe di connessione che illustrano le differenze tra la sintassi dell'autenticazione di Windows e SQL, vedere [ConnectionStrings.com](http://www.connectionstrings.com/).


## <a name="summary"></a>Riepilogo

Per impostazione predefinita, i file con un `.config` estensione in un'applicazione ASP.NET non è accessibile tramite un browser. Questi tipi di file non vengono restituiti perché essi possono contenere informazioni riservate, ad esempio le stringhe di connessione di database, nomi utente e password e così via. Il sistema di configurazione protetta in .NET 2.0 consente di proteggere ulteriormente le informazioni riservate, consentendo di sezioni di configurazione specificato da crittografare. Sono disponibili due provider di configurazione protetta predefiniti: uno che utilizza l'algoritmo RSA e uno che utilizza Windows Data Protection API (DPAPI).

In questa esercitazione è stato esaminato come crittografare e decrittografare le impostazioni di configurazione utilizzando il provider di DPAPI. Questa operazione può essere eseguita sia a livello di programmazione, come illustrato nel passaggio 2, nonché mediante il `aspnet_regiis.exe` strumento da riga di comando, che è stata descritta nel passaggio 3. Per ulteriori informazioni sull'uso di chiavi a livello di utente o tramite provider RSA, vedere le risorse nella sezione di approfondimento.

Buona programmazione!

## <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Compilazione applicazione ASP.NET sicure: Autenticazione, autorizzazione e comunicazioni protette](https://msdn.microsoft.com/library/aa302392.aspx)
- [La crittografia delle informazioni di configurazione in ASP.NET 2.0 applicazioni](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)
- [La crittografia `Web.config` valori in ASP.NET 2.0](https://weblogs.asp.net/scottgu/archive/2006/01/09/434893.aspx)
- [Procedura: Crittografare sezioni di configurazione in ASP.NET 2.0 con DPAPI](https://msdn.microsoft.com/library/ms998280.aspx)
- [Procedura: Crittografare sezioni di configurazione in ASP.NET 2.0 tramite RSA](https://msdn.microsoft.com/library/ms998283.aspx)
- [L'API di configurazione di .NET 2.0](http://www.odetocode.com/Articles/418.aspx)
- [Protezione dei dati di Windows](https://msdn.microsoft.com/library/ms995355.aspx)

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Lead revisori per questa esercitazione sono stati Teresa Murphy e Randy Schmidt. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs.md)
> [Successivo](debugging-stored-procedures-cs.md)
