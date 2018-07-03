---
uid: whitepapers/aspnet4/breaking-changes
title: ASP.NET 4 Breaking Changes | Microsoft Docs
author: rick-anderson
description: Questo documento vengono descritte le modifiche che sono state apportate per la versione di .NET Framework versione 4 che può potenzialmente influire sulle applicazioni che sono state create usando...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: d601c540-f86b-4feb-890c-20c806b3da6c
ms.technology: ''
msc.legacyurl: /whitepapers/aspnet4/breaking-changes
msc.type: content
ms.openlocfilehash: bf9968c49e904c064338d7123405cb6578a915f2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378650"
---
<a name="aspnet-4-breaking-changes"></a>ASP.NET 4 Breaking Changes
====================
> Questo documento vengono descritte le modifiche che sono state apportate per la versione di .NET Framework versione 4 che può potenzialmente influire sulle applicazioni create utilizzando versioni precedenti, tra cui le versioni Beta 2 e 4 Beta 1 di ASP.NET.
> 
> [Scarica questo white paper](https://download.microsoft.com/download/7/1/A/71A105A9-89D6-4201-9CC5-AD6A3B7E2F22/ASP_NET_4_Breaking_Changes.pdf)


<a id="0.1__Toc256768952"></a><a id="0.1__Toc256770056"></a>

## <a name="contents"></a>Sommario

[Impostazione ControlRenderingCompatibilityVersion nel File Web. config](#0.1__Toc256770141 "_Toc256770141")  
[Le modifiche ClientIDMode](#0.1__Toc256770142 "_Toc256770142")  
[HtmlEncode e UrlEncode ora tra virgolette singole di codificare](#0.1__Toc256770143 "_Toc256770143")  
[Pagina ASP.NET (aspx) del Parser è Stricter](#0.1__Toc256770144 "_Toc256770144")  
[File di definizione del browser aggiornati](#0.1__Toc256770145 "_Toc256770145")  
[DLL rimosso dal File di configurazione Web radice](#0.1__Toc256770146 "_Toc256770146")  
[Convalida delle richieste ASP.NET](#0.1__Toc256770147 "_Toc256770147")  
[Algoritmo hash predefinito è ora HMACSHA256](#0.1__Toc256770148 "_Toc256770148")  
[Configurazione errori correlati al nuovo ASP.NET 4 Root Configuration](#0.1__Toc256770149 "_Toc256770149")  
[Le applicazioni ASP.NET 4 figlio esito negativo a Start in caso di ASP.NET 2.0 o ASP.NET 3.5 Applications](#0.1__Toc256770150 "_Toc256770150")  
[Siti Web ASP.NET 4 non venga avviato nei computer in cui è installato SharePoint](#0.1__Toc256770151 "_Toc256770151")  
[La proprietà HttpRequest.FilePath non include più valori PathInfo](#0.1__Toc256770152 "_Toc256770152")  
[ASP.NET 2.0 Applications possono generare errori HttpException che fanno riferimento a eurl. axd](#0.1__Toc256770153 "_Toc256770153")  
[Gestori di eventi non vengano generati non in un documento predefinito in IIS 7.5 o IIS 7 in modalità integrata](#0.1__Toc256770154 "_Toc256770154")  
[Modifiche all'implementazione di sicurezza dall'accesso di codice ASP.NET](#0.1__Toc256770155 "_Toc256770155")  
[Sono stati spostati MembershipUser e altri tipi all'interno di Namespace dotare](#0.1__Toc256770156 "_Toc256770156")  
[Memorizzazione nella cache le modifiche per variare l'output \* intestazione HTTP](#0.1__Toc256770157 "_Toc256770157")  
[Tipi di dotare per Passport sono Obsolete](#0.1__Toc256770158 "_Toc256770158")  
[La proprietà MenuItem.PopOutImageUrl non riesce a eseguire il rendering di un'immagine in ASP.NET 4](#0.1__Toc256770159 "_Toc256770159")  
[Menu.StaticPopOutImageUrl senza riuscirci Menu.DynamicPopOutImageUrl per il rendering delle immagini quando i percorsi di contenere barre rovesciate](#0.1__Toc256770160 "_Toc256770160")  
[Disclaimer](#0.1__Toc256770161 "_Toc256770161")

<a id="0.1__ControlRenderingCompatibilityVersio"></a><a id="0.1__Toc245724853"></a><a id="0.1__Toc255587630"></a><a id="0.1__Toc256770141"></a>

## <a name="controlrenderingcompatibilityversion-setting-in-the-webconfig-file"></a>Impostazione ControlRenderingCompatibilityVersion nel File Web. config

I controlli ASP.NET sono stati modificati in .NET Framework versione 4 per consentono di specificare in modo più preciso la modalità il rendering del markup. Nelle versioni precedenti di .NET Framework, alcuni controlli generavano markup che non era possibile disabilitare. Per impostazione predefinita, ASP.NET 4, questo tipo di markup non vengono generati.

Se si usa Visual Studio 2010 per aggiornare l'applicazione da ASP.NET 2.0 o ASP.NET 3.5, lo strumento aggiunge automaticamente un'impostazione per il `Web.config` file tale da mantenere per il rendering legacy. Se tuttavia si aggiorna un'applicazione modificando il pool di applicazioni in IIS per impostare come destinazione .NET Framework 4, ASP.NET usa la nuova modalità di rendering per impostazione predefinita. Per disabilitare la nuova modalità di rendering, aggiungere la seguente impostazione nel `Web.config` file:

[!code-xml[Main](breaking-changes/samples/sample1.xml)]

Le modifiche principali per il rendering che introduce il nuovo comportamento sono i seguenti:

- Il **immagine** e **ImageButton** controlli di rendering non è più un `border="0"` attributo.
- Il **BaseValidator** classe e convalida i controlli che derivano da quest'ultimo non è più il rendering di testo di colore rosso per impostazione predefinita.
- Il **HtmlForm** non eseguire il rendering di un **nome** attributo.
- Il **tabella** rendering del controllo non è più un `border="0"` attributo.
- I controlli che non sono progettati per l'input utente (ad esempio, il **etichetta** controllo) non è più il rendering il `disabled="disabled"` attributo se loro **abilitato** è impostata su **false**(o se ereditano questa impostazione di un controllo contenitore).

<a id="0.1__Toc245724854"></a><a id="0.1__Toc255587631"></a><a id="0.1__Toc256770142"></a>

## <a name="clientidmode-changes"></a>Modifiche ClientIDMode

Il **ClientIDMode** impostazione in ASP.NET 4 consente di specificare la modalità ASP.NET genera il **id** attributo per gli elementi HTML. Nelle versioni precedenti di ASP.NET, il comportamento predefinito era equivalente per la **AutoID** impostazione **ClientIDMode**. Tuttavia, l'impostazione predefinita è ora **stimabile**.

Se si usa Visual Studio 2010 per aggiornare l'applicazione da ASP.NET 2.0 o ASP.NET 3.5, lo strumento aggiunge automaticamente un'impostazione per il `Web.config` file che mantiene il comportamento delle versioni precedenti di .NET Framework. Se tuttavia si aggiorna un'applicazione modificando il pool di applicazioni in IIS per impostare come destinazione .NET Framework 4, ASP.NET usa la nuova modalità per impostazione predefinita. Per disabilitare la nuova modalità ID client, aggiungere la seguente impostazione nel `Web.config` file:

[!code-xml[Main](breaking-changes/samples/sample2.xml)]

<a id="0.1__Toc245724855"></a><a id="0.1__Toc255587632"></a><a id="0.1__Toc256770143"></a>

## <a name="htmlencode-and-urlencode-now-encode-single-quotation-marks"></a>HtmlEncode e UrlEncode ora codificare tra virgolette singole

In ASP.NET 4 il **HtmlEncode** e **UrlEncode** metodi del **HttpUtility** e **HttpServerUtility** classi sono state aggiornate per codificare il carattere di virgoletta singola (') come indicato di seguito:

- Il **HtmlEncode** metodo codifica le istanze della virgoletta singola come.
- Il **UrlEncode** metodo codifica le istanze della virgoletta singola come % 27.

<a id="0.1__Toc255587633"></a><a id="0.1__Toc256770144"></a><a id="0.1__Toc245724856"></a>

## <a name="aspnet-page-aspx-parser-is-stricter"></a>Pagina ASP.NET (aspx) del Parser è Stricter

Il parser della pagina per le pagine ASP.NET (`.aspx` file) e i controlli utente (`.ascx` file) è più restrittivo in ASP.NET 4 e rifiuta le altre istanze di markup non valido. Ad esempio, due frammenti di codice seguenti potrebbe analizzare correttamente nelle versioni precedenti di ASP.NET, ma ora genera errori del parser in ASP.NET 4.

[!code-aspx[Main](breaking-changes/samples/sample3.aspx)]

Si noti che il punto e virgola non è valido in fondo il **HiddenField** tag.

[!code-aspx[Main](breaking-changes/samples/sample4.aspx)]

Si noti che il non chiusa **stile** attributo che si raggiunge il **CssClass** attributo.

<a id="0.1__Toc255587634"></a><a id="0.1__Toc256770145"></a>

## <a name="browser-definition-files-updated"></a>File di definizione del browser aggiornati

I file di definizione del browser sono stati aggiornati per includere informazioni su browser e dispositivi nuovi e aggiornati. Sono stati rimossi browser e dispositivi precedenti come Netscape Navigator e sono stati aggiunti browser e dispositivi più recenti come Google Chrome e Apple iPhone.

Se l'applicazione in uso contiene definizioni del browser personalizzate che derivano da una delle definizioni del browser rimosse, viene visualizzato un errore. Ad esempio, se il `App_Browsers` cartella contiene una definizione del browser che eredita dalla definizione del browser IE2, si riceverà il messaggio di errore di configurazione seguenti:

- Impossibile trovare l'elemento del browser o un gateway con ID 'IE2'.

> [!NOTE]
> Il **HttpBrowserCapabilities** oggetto (esposta nella pagina **Request** proprietà) dipende dai file di definizioni del browser. Di conseguenza, le informazioni restituite mediante accesso a una proprietà di questo oggetto in ASP.NET 4 potrebbero essere diverse rispetto alle informazioni restituite in una versione precedente di ASP.NET.


È possibile ripristinare i file di definizione browser precedenti copiando i file di definizione browser dalla seguente cartella:

[!code-console[Main](breaking-changes/samples/sample5.cmd)]

Copiare i file nella corrispondente `\CONFIG\Browsers` cartella per ASP.NET 4. Dopo aver copiato i file, eseguire Aspnet\_regbrowsers.exe lo strumento da riga di comando.

<a id="0.1__Toc255587635"></a><a id="0.1__Toc256770146"></a>

## <a name="systemwebmobiledll-removed-from-root-web-configuration-file"></a>DLL rimosso dal File di configurazione Web radice

Nelle versioni precedenti di ASP.NET, è stato incluso un riferimento all'assembly dll nella directory radice `Web.config` del file nei **assembly** sezione in. Per migliorare le prestazioni, è stato rimosso il riferimento a questo assembly.

L'assembly dll è incluso in ASP.NET 4, ma è deprecato. Se si desidera utilizzare i tipi dall'assembly System, aggiungere un riferimento a questo assembly a una radice `Web.config` file o a un'applicazione `Web.config` file. Ad esempio, se si desidera usare uno qualsiasi dei controlli mobili ASP.NET (obsoleti), è necessario aggiungere un riferimento all'assembly DLL per il `Web.config` file.

<a id="0.1__Toc245724857"></a><a id="0.1__Toc255587636"></a><a id="0.1__Toc256770147"></a>

## <a name="aspnet-request-validation"></a>Convalida delle richieste ASP.NET

La funzionalità di convalida delle richieste in ASP.NET fornisce un certo livello di protezione predefinita da attacchi di cross-site scripting (XSS). Nelle versioni precedenti di ASP.NET, la convalida della richiesta è stata abilitata per impostazione predefinita. Tuttavia, applicato solo alle pagine ASP.NET (`.aspx` file e i file di classe) e solo quando queste pagine sono stati in esecuzione.

In ASP.NET 4, per impostazione predefinita, la convalida delle richieste è abilitata per tutte le richieste, perché è abilitato prima la **BeginRequest** fase di una richiesta HTTP. Di conseguenza, la convalida della richiesta si applica alle richieste per tutte le risorse ASP.NET, non solo le richieste di pagine. aspx. Questo include le richieste, ad esempio chiamate a servizi Web e gestori HTTP personalizzati. Convalida delle richieste è attiva anche quando i moduli HTTP personalizzati legge il contenuto di una richiesta HTTP.

Di conseguenza, ora possono verificarsi errori di convalida richiesta per le richieste che in precedenza non è stata attiva gli errori. Per ripristinare il comportamento della funzionalità di convalida richiesta di ASP.NET 2.0, aggiungere la seguente impostazione nel `Web.config` file:

[!code-xml[Main](breaking-changes/samples/sample6.xml)]

Tuttavia, è consigliabile analizzare gli errori di convalida richiesta per determinare se i gestori esistenti, i moduli o altro codice personalizzato accede potenzialmente non sicuri input HTTP che possono essere XSS vettori di attacco.

<a id="0.1__Toc245724858"></a><a id="0.1__Toc255587637"></a><a id="0.1__Toc256770148"></a>

## <a name="default-hashing-algorithm-is-now-hmacsha256"></a>Algoritmo hash predefinito è ora HMACSHA256

Per la protezione di dati come i cookie di autenticazione modulo e lo stato di visualizzazione, ASP.NET usa sia la crittografia che gli algoritmi hash. Per impostazione predefinita, ASP.NET 4 Usa ora l'algoritmo HMACSHA256 per le operazioni di hash sui cookie e lo stato di visualizzazione. Le versioni precedenti di ASP.NET utilizzano l'algoritmo di validazione="HMACSHA1 meno recente.

Le applicazioni interessate se si esegue misto 2.0/ASP.NET ASP.NET 4 ambienti in cui i dati, ad esempio i cookie di autenticazione moduli devono funzionare across.NET le versioni di Framework. Per configurare un'applicazione Web ASP.NET 4 per utilizzare l'algoritmo di validazione="HMACSHA1 precedente, aggiungere la seguente impostazione nel `Web.config` file:

[!code-xml[Main](breaking-changes/samples/sample7.xml)]

<a id="0.1__Toc245724859"></a><a id="0.1__Toc255587638"></a><a id="0.1__Toc256770149"></a>

## <a name="configuration-errors-related-to-new-aspnet-4-root-configuration"></a>Configurazione errori correlati al nuovo ASP.NET 4 Root Configuration

I file di configurazione radice (il `machine.config` file e la radice `Web.config` file) per .NET Framework 4 (e pertanto ASP.NET 4) sono stati aggiornati per includere la maggior parte delle informazioni di configurazione boilerplate che in ASP.NET 3.5 è state trovate di applicazione `Web.config` file. A causa della complessità dei sistemi di configurazione gestiti IIS 7 e IIS 7.5, esecuzione di applicazioni ASP.NET 3.5 in ASP.NET 4 e in IIS 7 e IIS 7.5 può comportare ASP.NET o IIS errori di configurazione.

È consigliabile eseguire l'aggiornamento delle applicazioni ASP.NET 3.5 ad ASP.NET 4 con gli strumenti di aggiornamento progetto in Visual Studio 2010, se è più pratico. Visual Studio 2010 modifica automaticamente l'applicazione di ASP.NET 3.5 `Web.config` file per contenere le impostazioni appropriate per ASP.NET 4.

Tuttavia, è uno scenario supportato per l'esecuzione di applicazioni ASP.NET 3.5 usando .NET Framework 4 senza ricompilazione. In tal caso, potrebbe essere necessario modificare manualmente l'applicazione `Web.config` file prima di eseguire l'applicazione in .NET Framework 4 e in IIS 7 o IIS 7.5.

Due sezioni successive descrivono le modifiche che potrebbe essere necessario apportare per diverse combinazioni di software.

**Windows Vista SP1 o Windows Server 2008 SP1, in cui sono installati hotfix KB958854 né SP2.** In questa configurazione, il sistema di configurazione di IIS 7 in modo non corretto unisce una configurazione dell'applicazione gestita tramite il confronto a livello di applicazione `Web.config` file per ASP.NET 2.0 `machine.config` file. A causa di questo, a livello di applicazione `Web.config` nei file da .NET Framework 3.5 o versione successiva deve essere un **Extensions** definizioni di sezione di configurazione (elemento) per non causare un errore di convalida di IIS 7.

Tuttavia, modificato manualmente a livello di applicazione `Web.config` voci del file che non corrispondono esattamente le definizioni di tipo sezione di configurazione boilerplate originale che sono state introdotte con Visual Studio 2008 causerà errori di configurazione ASP.NET. (Le voci di configurazione predefiniti che vengono generate da Visual Studio 2008 funzionano correttamente.) Un problema comune è che modificato manualmente `Web.config` tralasciare i file di **allowDefinition** e **requirePermission** attributi di configurazione presenti nella sezione di configurazione diverse definizioni. In questo modo, una mancata corrispondenza tra la sezione di configurazione abbreviato in a livello di applicazione `Web.config` file e la definizione completa in ASP.NET 4 `machine.config` file. Di conseguenza, in fase di esecuzione, il sistema di configurazione di ASP.NET 4 genera un errore di configurazione.

**Windows Vista SP2, Windows Server 2008 SP2, Windows 7, Windows Server 2008 R2 e anche Windows Vista SP1 e Windows Server 2008 SP1 in cui è installato l'hotfix KB958854.**

In questo scenario, il sistema di configurazione native IIS 7.5 e IIS 7 restituisce un errore di configurazione perché consente di eseguire un confronto di testo nel **tipo** attributo definito per un gestore della sezione di configurazione gestita. Poiché tutti i `Web.config` i file che vengono generati da Visual Studio 2008 e Visual Studio 2008 SP1 hanno "3.5" nella stringa di tipo per il **Extensions** (e correlate) gestori delle sezioni di configurazione e poiché in ASP.NET 4 `machine.config` file è "4.0" **tipo** attributo per i gestori delle sezioni di configurazione stesso, le applicazioni che vengono generati sempre in Visual Studio 2008 SP1 o Visual Studio 2008 non superano la convalida di configurazione in IIS 7 e IIS 7.5.

<a id="0.1__Toc251910248"></a>

### <a name="resolving-these-issues"></a>Risoluzione di questi problemi

La soluzione alternativa per il primo scenario consiste nell'aggiornare il livello di applicazione `Web.config` file includendo il testo di configurazione boilerplate da un `Web.config` file generato automaticamente da Visual Studio 2008.

Una soluzione alternativa per il primo scenario consiste nell'installare Service Pack 2 per la Vista o Windows Server 2008 nel computer o installare l'hotfix KB958854 ([https://support.microsoft.com/kb/958854](https://support.microsoft.com/kb/958854)) per correggere il comportamento di configurazione non corretta-unione del Sistema di configurazione di IIS. Tuttavia, dopo aver eseguito una di queste azioni, l'applicazione si verifica un errore di configurazione a causa del problema descritto per il secondo scenario.

La soluzione alternativa per il secondo scenario consiste nell'eliminare o commento tutte le **Extensions** le definizioni di sezione di configurazione e sezione di configurazione del gruppo di definizioni del livello di applicazione `Web.config` file. Queste definizioni sono in genere all'inizio del livello di applicazione `Web.config` file e può essere identificato tramite il **configSections** elemento e i relativi figli.

Per entrambi gli scenari, si consiglia di eliminare manualmente il **System. CodeDom** sezione, anche se non è obbligatorio.

<a id="0.1__Toc252995490"></a><a id="0.1__Toc255587639"></a><a id="0.1__Toc256770150"></a><a id="0.1__Toc245724860"></a>

## <a name="aspnet-4-child-applications-fail-to-start-when-under-aspnet-20-or-aspnet-35-applications"></a>Le applicazioni ASP.NET 4 figlio esito negativo a Start in caso di ASP.NET 2.0 o ASP.NET 3.5 Applications

Le applicazioni ASP.NET 4 configurate come elementi figlio di applicazioni che eseguono versioni precedenti di ASP.NET potrebbero non avviarsi a causa di errori di compilazione o di configurazione. Nell'esempio seguente viene mostrata una struttura di directory per un'applicazione interessata.

`/parentwebapp` (configurato per utilizzare ASP.NET 2.0 o ASP.NET 3.5)  
`/childwebapp` (configurato per l'utilizzo di ASP.NET 4)

L'applicazione nel `childwebapp` cartella riuscirà ad avviare in IIS 7 o IIS 7.5 e segnalerà un errore di configurazione. Il testo dell'errore includerà un messaggio simile al seguente:

- `The requested page cannot be accessed because the related configuration data for the page is invalid.`
  

- `The configuration section 'configSections' cannot be read because it is missing a section declaration.`

IIS 6, l'applicazione nel `childwebapp` cartella sarà anche possibile avviare, ma segnalerà un errore diverso. Ad esempio il testo dell'errore potrebbe essere stato quanto segue:

- `The value for the 'compilerVersion' attribute in the provider options must be 'v4.0' or later if you are compiling for version 4.0 or later of the .NET Framework. To compile this Web application for version 3.5 or earlier of the .NET Framework, remove the 'targetFramework' attribute from the element of the Web.config file`

Questi scenari si verificano perché le informazioni di configurazione dell'applicazione padre nel `parentwebapp` cartella fa parte della gerarchia di informazioni di configurazione che determinano le impostazioni di configurazione unita finale usati da web figlio applicazione di `childwebapp` cartella. A seconda del fatto che l'applicazione Web ASP.NET 4 è in esecuzione in IIS 7 (o IIS 7.5) o IIS 6, il sistema di configurazione di IIS o nel sistema di compilazione ASP.NET 4 restituirà un errore.

I passaggi da seguire per risolvere questo problema e ottenere l'elemento figlio funzionamento dell'applicazione ASP.NET 4 variano a seconda se ASP.NET 4 in esecuzione l'applicazione in IIS 6 o IIS 7 (o IIS 7.5).

### <a name="step-1-iis-7-or-iis-75-only"></a>Passaggio 1 (IIS 7 o IIS 7.5)

Questo passaggio è necessario solo in sistemi operativi che eseguono IIS 7 o IIS 7.5, che include Windows Vista, Windows Server 2008, Windows 7 e Windows Server 2008 R2.

Spostare il **configSections** definizione nel `Web.config` file dell'applicazione padre, l'applicazione che esegue ASP.NET 2.0 o ASP.NET 3.5, nella radice `Web.config` file per.NET Framework 2.0. Il sistema di configurazione nativo di IIS 7 e IIS 7.5 viene analizzato il **configSections** elemento quando unisce la gerarchia dei file di configurazione. Spostare il **configSections** definizione da applicazione Web padre `Web.config` file alla radice `Web.config` file nasconde in modo efficace l'elemento dal processo di configurazione di tipo merge che si verifica per l'elemento figlio ASP.NET 4 applicazione.

In un sistema operativo a 32 bit o per i pool di applicazioni a 32 bit, la radice `Web.config` file per ASP.NET 2.0 e ASP.NET 3.5 in genere si trova nella cartella seguente:

`C:\Windows\Microsoft.NET\Framework\v2.0.50727\CONFIG`

In un sistema operativo a 64 bit o per i pool di applicazioni a 64 bit, la radice `Web.config` file per ASP.NET 2.0 e ASP.NET 3.5 in genere si trova nella cartella seguente:

`C:\Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG`

Se si eseguono le applicazioni Web sia a 32 e a 64 bit in un computer a 64 bit, è necessario spostare il **configSections** elemento superiore nella radice `Web.config` file per i 32 bit e i sistemi a 64 bit.

Quando si inserisce il **configSections** elemento nella directory radice `Web.config` file, incollare la sezione immediatamente dopo il **configurazione** elemento. L'esempio seguente mostra che la parte superiore della radice `Web.config` file dovrebbe essere, ad esempio quando si have completato lo spostamento di elementi.

> [!NOTE]
> Nell'esempio seguente, le righe sono state integrate per migliorare la leggibilità.


[!code-xml[Main](breaking-changes/samples/sample8.xml)]

### <a name="step-2-all-versions-of-iis"></a>Passaggio 2 (tutte le versioni di IIS)

Questo passaggio è obbligatorio se l'elemento figlio ASP.NET 4 dell'applicazione Web è in esecuzione su IIS 6 o IIS 7 (o IIS 7.5).

Nel `Web.config` file dell'elemento padre dell'applicazione Web che esegue 2 ASP.NET o ASP.NET 3.5, aggiungere un' **percorso** tag che specifica in modo esplicito (per i sistemi di configurazione di IIS e ASP.NET) che solo le voci di configurazione si applicano all'applicazione Web padre. Nell'esempio seguente mostra la sintassi del **posizione** elemento da aggiungere:

[!code-xml[Main](breaking-changes/samples/sample9.xml)]

L'esempio seguente illustra come la **posizione** tag viene utilizzato per eseguire il wrapping di tutte le sezioni di configurazione a partire dal **appSettings** sezione e terminando con **System. webServer**sezione.

[!code-xml[Main](breaking-changes/samples/sample10.xml)]

Dopo avere completato i passaggi 1 e 2, le applicazioni Web ASP.NET 4 figlio verranno avviato senza errori.

<a id="0.1__Toc252995491"></a><a id="0.1__Toc255587640"></a><a id="0.1__Toc256770151"></a>

## <a name="aspnet-4-web-sites-fail-to-start-on-computers-where-sharepoint-is-installed"></a>Siti Web ASP.NET 4 non venga avviato nei computer in cui è installato SharePoint

I server Web che eseguono SharePoint con un `Web.config` file, che viene distribuito a livello di radice di un sito Web di SharePoint (ad esempio, `c:\inetpub\wwwroot\web.config` per sito Web predefinito). In questo `Web.config` file di SharePoint imposta un'attendibilità parziale personalizzato a livello denominato WSS\_minimo.

Se si prova a eseguire un sito Web ASP.NET 4 distribuito come un elemento figlio di questo tipo di sito Web di SharePoint, si verrà visualizzato l'errore seguente:

`Could not find permission set named 'ASP.NET'.`

Questo errore si verifica perché l'infrastruttura di sicurezza dall'accesso di codice ASP.NET 4 Cerca un set di autorizzazioni denominato ASP.NET. Tuttavia, parziale trust file di configurazione che fa riferimento WSS\_minimo non contiene alcun set di autorizzazioni con lo stesso nome.

Attualmente non esiste una versione di SharePoint compatibile con ASP.NET. Di conseguenza, non tentare di eseguire un sito Web ASP.NET 4 come un sito figlio di sotto di Web di SharePoint siti.

<a id="0.1__Toc255587641"></a><a id="0.1__Toc256770152"></a>

## <a name="the-httprequestfilepath-property-no-longer-includes-pathinfo-values"></a>La proprietà HttpRequest.FilePath non include più valori PathInfo

Le versioni precedenti di ASP.NET inclusa una **PathInfo** valore nel valore restituito da diversi file correlati ai percorsi proprietà, tra cui **HttpRequest.FilePath**,  **HttpRequest.AppRelativeCurrentExecutionFilePath**, e **HttpRequest.CurrentExecutionFilePath**. ASP.NET 4 non sono più inclusi i **PathInfo** valore nei valori restituiti da queste proprietà. Al contrario, il **PathInfo** informazioni sono disponibili nei **HttpRequest.PathInfo**. Considerare ad esempio il frammento di URL seguente:

`/testapp/Action.mvc/SomeAction`

Nelle versioni precedenti di ASP.NET **HttpRequest** proprietà hanno valori seguenti:

**HttpRequest.FilePath**: `/testapp/Action.mvc/SomeAction`

**HttpRequest.PathInfo**: (vuoto)

In ASP.NET 4 **HttpRequest** proprietà hanno invece i valori seguenti:

**HttpRequest.FilePath**: `/testapp/Action.mvc`

**HttpRequest.PathInfo**: `SomeAction`

<a id="0.1__Toc252995493"></a><a id="0.1__Toc255587642"></a><a id="0.1__Toc256770153"></a><a id="0.1__Toc245724861"></a>

## <a name="aspnet-20-applications-might-generate-httpexception-errors-that-reference-eurlaxd"></a>ASP.NET 2.0 Applications possono generare errori HttpException che fanno riferimento a eurl. axd

Dopo l'abilitazione di ASP.NET 4 in IIS 6, le applicazioni ASP.NET 2.0 in esecuzione su IIS 6 (in Windows Server 2003 o Windows Server 2003 R2) possono generare errori come il seguente:

`System.Web.HttpException: Path '/[yourApplicationRoot]/eurl.axd/[Value]' was not found.`

Questo errore si verifica perché quando ASP.NET rileva che un sito Web sia configurato per utilizzare ASP.NET 4, un componente nativo di ASP.NET 4 passa un URL senza estensione per la parte gestita di ASP.NET per un'ulteriore elaborazione. Tuttavia, se le directory virtuali che sono di sotto di un sito Web ASP.NET 4 sono configurate per utilizzare ASP.NET 2.0, l'elaborazione dell'URL senza estensione nei risultati in questo modo in un URL modificato che contiene la stringa "eurl. axd". Questo URL modificato viene quindi inviato all'applicazione ASP.NET 2.0. ASP.NET 2.0 non è in grado di riconoscere il formato "eurl. axd". Pertanto, ASP.NET 2.0 tenta di trovare un file denominato `eurl.axd` ed eseguirlo. Poiché il file non esiste, la richiesta ha esito negativo con un **HttpException** eccezione.

È possibile risolvere questo problema utilizzando una delle opzioni seguenti.

### <a name="option-1"></a>Opzione 1

Se ASP.NET 4 non è necessaria per eseguire il sito Web, modificare il mapping del sito per l'uso di ASP.NET 2.0.

### <a name="option-2"></a>Opzione 2

Se ASP.NET 4 è necessaria per eseguire il sito Web, spostare directory virtuali figlio ASP.NET 2.0 per un altro sito Web che viene eseguito il mapping ad ASP.NET 2.0.

### <a name="option-3"></a>Opzione 3

Se non è possibile modificare il mapping del sito Web per ASP.NET 2.0 o per modificare il percorso di una directory virtuale, disabilitare in modo esplicito l'URL senza estensione di elaborazione in ASP.NET 4. Utilizzare la procedura seguente:

1. Nel Registro di sistema Windows, aprire il nodo seguente:

`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ASP.NET\4.0.30319.0`

1. Creare una nuova **DWORD** valore denominato **EnableExtensionlessUrls**.
2. Impostare **EnableExtensionlessUrls** su 0. In questo modo viene disabilitato il comportamento degli URL.
3. Salvare il valore del Registro di sistema e chiudere l'editor del Registro di sistema.
4. Eseguire la **iisreset** strumento da riga di comando, che fa in modo che IIS leggere il nuovo valore del Registro di sistema.

> [!NOTE]
> L'impostazione **EnableExtensionlessUrls** su 1 abilita il comportamento degli URL. Questo è l'impostazione predefinita se viene specificato alcun valore.


<a id="0.1__Toc252995494"></a><a id="0.1__Toc255587643"></a><a id="0.1__Toc256770154"></a><a id="0.1__Toc245724862"></a>

## <a name="event-handlers-might-not-be-not-raised-in-a-default-document-in-iis-7-or-iis-75-integrated-mode"></a>Gestori di eventi non vengano generati non in un documento predefinito in IIS 7.5 o IIS 7 in modalità integrata

ASP.NET 4 include modifiche che modificano il **azione** attributo HTML **form** elemento sottoposto a rendering quando un URL senza estensione viene risolta in un documento predefinito. Un esempio di URL senza estensione la risoluzione in un documento predefinito sarebbe [ http://contoso.com/ ](http://contoso.com/), risultante in una richiesta al [ http://contoso.com/Default.aspx ](http://contoso.com/Default.aspx).

ASP.NET 4 ora esegue il rendering HTML **form** dell'elemento **azione** valore dell'attributo come una stringa vuota quando viene effettuata una richiesta a un URL senza estensione a cui è mappato ad esso un documento predefinito. Ad esempio, nelle versioni precedenti di ASP.NET, una richiesta al [ http://contoso.com ](http://contoso.com) comporterebbe una richiesta a `Default.aspx`. In tale documento, l'apertura **form** tag veniva restituito come nell'esempio seguente:

`<form action="Default.aspx" />`

In ASP.NET 4 una richiesta al [ http://contoso.com ](http://contoso.com) comporta anche una richiesta a `Default.aspx`. Tuttavia, ASP.NET ora esegue il rendering di apertura HTML **form** tag come illustrato di seguito:

`<form action="" />`

Questa differenza nel modo in cui il **azione** attributo sottoposto a rendering può causare lievi modifiche in modalità di elaborazione di un post del form da IIS e ASP.NET. Quando la **azione** attributo è una stringa vuota, IIS **DefaultDocumentModule** oggetto verrà creata una richiesta figlio a `Default.aspx`. Nella maggior parte dei casi, la richiesta figlio è trasparente al codice dell'applicazione e il `Default.aspx` pagina viene eseguito normalmente.

Tuttavia una potenziale interazione tra il codice gestito e la modalità integrata di IIS 7 o IIS 7.5 può interrompere il funzionamento corretto delle pagine gestite con estensione aspx durante la richiesta figlio. Se le condizioni seguenti si verificano, la richiesta figlio a un `Default.aspx` documento verrà generato un errore o un comportamento imprevisto:

1. Una pagina aspx viene inviata al browser con il **form** dell'elemento **azione** attributo impostato su "".
2. Il modulo viene reinserito in ASP.NET.
3. Un modulo HTTP gestito legge una parte del corpo dell'entità. Ad esempio, un modulo legge **Request. Form** oppure **Request. params**. Di conseguenza il corpo dell'entità della richiesta POST viene letto nella memoria gestita. Pertanto il corpo dell'entità non è più disponibile per i moduli di codice nativo che sono in esecuzione in modalità integrata IIS 7 o IIS 7.5.
4. IIS **DefaultDocumentModule** oggetto alla fine viene eseguito e crea una richiesta figlio per il `Default.aspx` documento. Tuttavia poiché il corpo dell'entità è già stato letto da un frammento di codice gestito, non è disponibile alcun corpo di entità da inviare alla richiesta figlio.
5. Quando la pipeline HTTP viene eseguita per la richiesta figlio, il gestore per `.aspx` file viene eseguito durante la fase handler-Execute.
6. Poiché non è presente alcun corpo di entità, esistono nessuno stato di visualizzazione e nessuna variabile di form, e pertanto non sono disponibili informazioni per il gestore della pagina aspx determinare quale evento (se presente) dovrebbe essere generato. Pertanto non viene eseguito nessun gestore dell'evento di postback per la pagina con estensione aspx interessata.

È possibile risolvere il problema nei modi seguenti:

- Identificare il modulo HTTP che accede a corpo entità della richiesta durante le richieste di documenti predefiniti e determinare se può essere configurato per l'esecuzione solo per le richieste gestite. In modalità integrata per IIS 7 e IIS 7.5, i moduli HTTP possono essere contrassegnati per l'esecuzione solo per le richieste gestite aggiungendo l'attributo seguente al modulo **WebServer/Modules** voce:

- `precondition="managedHandler"`

- Disabilita questa impostazione per il modulo richiede che IIS 7 e IIS 7.5 di determinare come non gestite le richieste. Per richieste di documenti predefiniti, la prima richiesta è un URL senza estensione. Pertanto, IIS non viene eseguito tutti i moduli gestiti contrassegnati con una precondizione di gestore gestito durante l'elaborazione della richiesta iniziale. Di conseguenza, i moduli gestiti non verranno lette accidentalmente il corpo dell'entità e pertanto il corpo dell'entità è ancora disponibile e viene passato per la richiesta figlio e il documento predefinito.

- La presenza di moduli HTTP problematici per l'esecuzione per tutte le richieste (per i file statici, per URL senza estensione a cui si risolvono i **DefaultDocumentModule** oggetto, per le richieste gestite e così via), modificare in modo esplicito le pagine con estensione aspx interessata da impostando il **azione** proprietà della pagina **System.Web.UI.HtmlControls.HtmlForm** controllo in una stringa non vuota. Ad esempio, se il documento predefinito è `Default.aspx`, modificare il codice della pagina per impostare esplicitamente le **HtmlForm** del controllo **azione** proprietà su "Default. aspx".

<a id="0.1__Toc255587644"></a><a id="0.1__Toc256770155"></a>

## <a name="changes-to-the-aspnet-code-access-security-cas-implementation"></a>Modifiche apportate all'implementazione di sicurezza dall'accesso di codice ASP.NET

ASP.NET 2.0, e dall'estensione per usare le funzionalità di ASP.NET che sono stati aggiunti 3.5, .NET Framework 1.1 e modello di sicurezza dall'accesso di codice 2.0. Tuttavia l'implementazione di CAS in ASP.NET 4 è stata modificata in modo sostanziale. Di conseguenza, le applicazioni ASP.NET parzialmente attendibile che si basano su codice attendibile in esecuzione nella global assembly cache (GAC) potrebbero non riuscire con diverse eccezioni di sicurezza. Applicazioni con attendibilità parziale che si basano su modifiche estese ai criteri CAS del computer potrebbero anche non riuscire con eccezioni di sicurezza.

È possibile ripristinare le applicazioni ASP.NET 4 con attendibilità parziale per il comportamento di ASP.NET 1.1 e 2.0 usando le nuove **legacyCasModel** attributo il **trust** elemento di configurazione, come illustrato nell'esempio seguente :

`<trust level= "Medium" legacyCasModel="true" />`

Quando si ripristina il modello CAS legacy, sono abilitati i seguenti comportamenti di autorità di certificazione precedenti:

- Criteri CAS del computer viene rispettato.
- Sono consentiti più set di autorizzazioni diverso in un solo dominio applicazione.
- Le asserzioni di autorizzazioni esplicite non sono necessarie per gli assembly nella Global Assembly Cache che vengono richiamati quando solo ASP.NET o altro codice di .NET Framework è nello stack.

Uno scenario non può essere annullato in .NET Framework 4: applicazioni con attendibilità parziale non Web non è più possono chiamare alcune API nella DLL e System.Web.Extensions.dll. Nelle versioni precedenti di .NET Framework, è possibile per le applicazioni con attendibilità parziale non Web essere concessi esplicitamente <strong>AspNetHostingPermission</strong> autorizzazioni. Sarebbe quindi possibile usare queste applicazioni <strong>System.Web.HttpUtility</strong>, digita il <strong>System.Web.ClientServices.\< / strong > * gli spazi dei nomi e i tipi correlati a appartenenza, ruoli e profili. La chiamata di questi tipi di applicazioni con attendibilità parziale non Web non è più supportata in .NET Framework 4.

> [!NOTE]
> Il **HtmlEncode** e **HtmlDecode** funzionalità del **System.Web.HttpUtility** classe è stata spostata in .NET Framework 4 nuovi  **System.Net.WebUtility** classe. Se era l'unica funzionalità ASP.NET che veniva utilizzato, modificare il codice dell'applicazione per usare le nuove **WebUtility** classe.


Di seguito è riportato un riepilogo dettagliato delle modifiche per l'implementazione di autorità di certificazione predefinita in ASP.NET 4:

- Domini delle applicazioni ASP.NET sono ora i domini applicazione omogenei. Solo i set di concessione di attendibilità e attendibilità parziale sono disponibili in un dominio applicazione.
- Set di concessioni di attendibilità parziale ASP.NET sono indipendenti da qualsiasi criterio di autorità di certificazione a livello aziendale, a livello di computer o a livello di utente.
- ASP.NET assembly forniti con 3.5 e 3.5 SP1 sono stati convertiti per utilizzare il modello di trasparenza di .NET Framework 4.
- Utilizzo di ASP.NET **AspNetHostingPermission** attributo è stato notevolmente ridotto. La maggior parte delle istanze di questo attributo sono state rimosse dalle APIs ASP.NET pubblico.
- Compilata in modo dinamico gli assembly che vengono creati dal provider di compilazione ASP.NET sono stati aggiornati per contrassegnare in modo esplicito gli assembly come trasparente.
- Tutti gli assembly ASP.NET sono ora contrassegnati in modo che l'attributo APTCA viene rispettato solo in ambienti di hosting Web. Ambienti di hosting Web non parzialmente attendibili, ad esempio ClickOnce non sarà in grado di eseguire chiamate nell'assembly ASP.NET.

Per altre informazioni sul nuovo modello di sicurezza dall'accesso codice di ASP.NET 4, vedere [Using Code Access Security nelle applicazioni ASP.NET](https://msdn.microsoft.com/library/dd984947%28VS.100%29.aspx) sul sito Web MSDN.

<a id="0.1__Toc256770156"></a><a id="0.1__Toc245724863"></a><a id="0.1__Toc252995496"></a><a id="0.1__Toc255587645"></a><a id="0.1__Toc245724864"></a>

## <a name="membershipuser-and-other-types-in-the-systemwebsecurity-namespace-have-been-moved"></a>Sono stati spostati MembershipUser e altri tipi all'interno di Namespace dotare

Alcuni tipi vengono usati nell'appartenenza ASP.NET sono state spostate da `System.Web.dll` al nuovo assembly ApplicationServices. I tipi sono stati spostati per risolvere dipendenze dei livelli di architettura tra i tipi nel client e negli SKU estesi di .NET Framework.

Progetti di siti Web non è problemi come risultato lo spostamento di questi tipi, poiché ApplicationServices è stata aggiunta all'elenco di assembly di riferimento che viene usato per impostazione predefinita per il sistema di compilazione ASP.NET. Se si aggiorna un progetto sito Web creato utilizzando una versione precedente di ASP.NET ad ASP.NET 4 aprendolo in Visual Studio 2010, il progetto verrà compilato senza errori.

Analogamente, se si aggiorna un progetto di applicazione Web che è stato creato in una versione precedente di ASP.NET ad ASP.NET 4 aprendolo in Visual Studio 2010, il processo di aggiornamento aggiunge un riferimento a ApplicationServices al progetto. Pertanto, aggiornato Web i progetti di applicazioni verranno inoltre compilato senza errori.

I file (binari) compilati che sono stati creati usando versioni precedenti di ASP.NET verranno inoltre eseguita senza errori in ASP.NET 4, anche se sono stati spostati i tipi di appartenenza a un assembly diverso. Tipo di informazioni di inoltro è stato aggiunto alla versione di ASP.NET 4 `System.Web.dll` che indirizza automaticamente i riferimenti in fase di esecuzione per questi tipi nella nuova posizione per i tipi.

Tuttavia, le librerie di classi che utilizzano tipi di appartenenza specifico e che sono stati aggiornati da versioni precedenti di ASP.NET non verranno compilato quando utilizzato in un progetto ASP.NET 4. Ad esempio, un progetto di libreria di classi potrebbe non riuscire a compilare e segnalano un errore simile al seguente:

- `The type 'System.Web.Security.MembershipUser' is defined in an assembly that is not referenced. You must add a reference to assembly 'System.Web.ApplicationServices, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
  

- `The type name 'MembershipUser' could not be found. This type has been forwarded to assembly 'System.Web.ApplicationServices, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'. Consider adding a reference to that assembly.`

È possibile risolvere questo problema aggiungendo un riferimento nel progetto libreria di classi a ApplicationServices.

Nell'elenco seguente viene illustrata la *dotare* tipi che sono stati spostati dal `System.Web.dll` a ApplicationServices:

- *System.Web.Security.MembershipCreateStatus*
- *System.Web.Security.Membership.CreateUserException*
- *MembershipPasswordException*
- *System.Web.Security.MembershipPasswordFormat*
- *System.Web.Security.MembershipProvider*
- *System.Web.Security.MembershipProviderCollection*
- *System.Web.Security.MembershipUser*
- *System.Web.Security.MembershipUserCollection*
- *System.Web.Security.MembershipValidatePasswordEventHandler*
- *System.Web.Security.ValidatePasswordEventArgs*
- *System.Web.Security.RoleProvider*
- <a id="0.1_a"></a>*System.Web.Configuration.MembershipPasswordCompatibilityMode*

<a id="0.1__Toc256770157"></a>

## <a name="output-caching-changes-to-vary--http-header"></a>Memorizzazione nella cache le modifiche per variare l'output \* intestazione HTTP

In ASP.NET 1.0, un bug causato memorizzate nella cache le pagine specificate `Location="ServerAndClient"` come impostazione cache di output – per generare un `Vary:*` intestazione HTTP nella risposta. Questa intestazione comunicava ai browser client di non memorizzare mai la pagina nella cache in locale.

In ASP.NET 1.1, il **System.Web.HttpCachePolicy.SetOmitVaryStar** metodo è stato aggiunto, che è possibile chiamare per eliminare il `Vary:*` intestazione. Questo metodo è stato scelto perché l'intestazione HTTP generato la modifica è stata considerata una modifica potenziale modifica al momento. Tuttavia, gli sviluppatori sono trovati disorientati dal comportamento di ASP.NET e segnalazioni di bug suggeriscono che gli sviluppatori sono a conoscenza dell'oggetto esistente **SetOmitVaryStar** comportamento.

In ASP.NET 4, la decisione è stata effettuata per risolvere il problema radice. Il `Vary:*` intestazione HTTP non viene più generata dalle risposte che specificano la direttiva seguente:

`<%@OutputCache Location="ServerAndClient" %>`

Ne consegue **SetOmitVaryStar** non è più necessario per eliminare il `Vary:*` intestazione.

Nelle applicazioni che specificano `Location="ServerAndClient"` nella **@ OutputCache** direttiva in una pagina, verrà ora visualizzato il comportamento implicito nel nome del **percorso** valore dell'attributo, ovvero, pagine saranno inseribili nella cache nel browser senza che si chiama il **SetOmitVaryStar** (metodo).

Se le pagine dell'applicazione devono emettere `Vary:*`, chiamare il **AppendHeader** (metodo), come nell'esempio seguente:

`HttpResponse.AppendHeader("Vary","*");`

In alternativa, è possibile modificare il valore della memorizzazione nella cache di output **posizione** dell'attributo "Server".

<a id="0.1__Toc255587646"></a><a id="0.1__Toc256770158"></a>

## <a name="systemwebsecurity-types-for-passport-are-obsolete"></a>Tipi di dotare per Passport sono Obsolete

Il supporto per Passport incorporato in ASP.NET 2.0 è stato obsoleto e non supportati da alcuni anni a causa di modifiche in Passport (ora Windows Live ID). Di conseguenza, i cinque tipi associati a Passport nel **dotare** sono ora contrassegnate con la **ObsoleteAttribute** attributo.

<a id="0.1__The_MenuItem.PopOutImageUrl_Propert"></a><a id="0.1__Toc256770159"></a>

## <a name="the-menuitempopoutimageurl-property-fails-to-render-an-image-in-aspnet-4"></a>La proprietà MenuItem.PopOutImageUrl non riesce a eseguire il rendering di un'immagine in ASP.NET 4

In ASP.NET 3.5, il *MenuItem.PopOutImageUrl* proprietà consente di specificare l'URL per un'immagine che viene visualizzata in una voce di menu per indicare che la voce di menu dispone di un sottomenu dinamico. Nell'esempio seguente viene illustrato come specificare questa proprietà nel markup in ASP.NET 3.5.

[!code-aspx[Main](breaking-changes/samples/sample11.aspx)]

In seguito a una modifica di progettazione in ASP.NET 4, non viene restituito alcun output per il *PopOutImageUrl* se la proprietà è impostata per il *MenuItem* classe. In alternativa, è necessario specificare un URL immagine direttamente nel *dal Menu* controllare tramite il *StaticPopOutImageUrl* proprietà o il *DynamicPopOutImageUrl* proprietà. Quando si lavora con un menu statico, il *Menu.StaticPopOutImageUrl* proprietà consente di specificare l'URL per un'immagine visualizzata per indicare che la voce di menu statico dispone di un sottomenu, come illustrato nell'esempio seguente:

[!code-aspx[Main](breaking-changes/samples/sample12.aspx)]

Se si lavora con un menu dinamico, usare il *Menu.DynamicPopOutImageUrl* proprietà per specificare l'URL per un'immagine che indica che una voce di menu dinamico dispone di un sottomenu. Nell'esempio seguente è simile a quello precedente, ma viene illustrato come impostare il *DynamicPopOutImageUrl* proprietà per un menu dinamico.

[!code-aspx[Main](breaking-changes/samples/sample13.aspx)]

Se il *Menu.DynamicPopOutImageUrl* non è impostata e il *Menu.DynamicEnableDefaultPopOutImage* viene impostata su *false*, viene visualizzata alcuna immagine. In modo analogo, se il *StaticPopOutImageUrl* non è impostata e il *StaticEnableDefaultPopOutImage* viene impostata su *false*, viene visualizzata alcuna immagine.

Quando si impostano i percorsi per queste proprietà, utilizzare una barra (/) anziché una barra rovesciata (\). Per altre informazioni, vedere [Menu.StaticPopOutImageUrl e Menu.DynamicPopOutImageUrl senza successo a eseguire il rendering di immagini quando i percorsi di contenere barre rovesciate](#0.1__Menu.StaticPopOutImageUrl_and_Menu. "_Menu.StaticPopOutImageUrl_and_Menu.") in altre posizioni in questo documento.

<a id="0.1__Menu.StaticPopOutImageUrl_and_Menu."></a><a id="0.1__Toc256770160"></a>

## <a name="menustaticpopoutimageurl-and-menudynamicpopoutimageurl-fail-to-render-images-when-paths-contain-backslashes"></a>Menu.StaticPopOutImageUrl e Menu.DynamicPopOutImageUrl esito negativo per il rendering delle immagini quando i percorsi di contenere barre rovesciate

In ASP.NET 4, le immagini specificato utilizzando il *Menu.StaticPopOutImageUrl* e *Menu.DynamicPopOutImageUrl* delle proprietà non verranno eseguito il rendering se il percorso contiene backlashes (\). Si tratta di una modifica rispetto alle versioni precedenti di ASP.NET.

L'esempio seguente di *dal Menu* controllare markup mostrato il *StaticPopOutImageUrl* proprietà impostata utilizzando un percorso che contiene una barra rovesciata. In ASP.NET 4 l'immagine specificata nella proprietà non eseguirà il rendering.

[!code-aspx[Main](breaking-changes/samples/sample14.aspx)]

Per risolvere questo problema, modificare i valori di percorso specificate nel *StaticPopOutImageUrl* e *DynamicPopOutImageUrl* le proprietà da usare barre (/). L'esempio seguente illustra questa modifica:

[!code-aspx[Main](breaking-changes/samples/sample15.aspx)]

Si noti che le applicazioni che sono state migrate da versioni precedenti di ASP.NET ad ASP.NET 4 potrebbero anche essere interessato, poiché il *MenuItem.PopOutImageUrl* proprietà è stata modificata. Per altre informazioni, vedere [The MenuItem.PopOutImageUrl proprietà non riesce a eseguire il rendering di un'immagine in ASP.NET 4](#0.1__The_MenuItem.PopOutImageUrl_Propert "_The_MenuItem.PopOutImageUrl_Propert") altrove in questo documento.

<a id="0.1__Toc224729061"></a><a id="0.1__Toc255587647"></a><a id="0.1__Toc256770161"></a>

## <a name="disclaimer"></a>Dichiarazione di non responsabilità

Il presente documento è una versione preliminare e può essere modificato in modo sostanziale prima della versione finale commerciale del software qui descritto.

Le informazioni contenute nel presente documento rappresentano l'attuale opinione di Microsoft Corporation circa le problematiche discusse alla data della pubblicazione. Poiché Microsoft deve rispondere ai cambiamenti delle condizioni di mercato, il presente documento non deve essere interpretato quale un impegno da parte di Microsoft e Microsoft non può garantire l'accuratezza delle informazioni presentate dopo la data di pubblicazione.

Il presente white paper è fornito solo a scopi informativi. MICROSOFT NON RILASCIA ALCUNA GARANZIA, ESPRESSA, IMPLICITA O LEGALE, IN MERITO ALLE INFORMAZIONI DEL PRESENTE DOCUMENTO.

Il rispetto di tutte le leggi applicabili in materia di copyright è a esclusivo carico dell'utente. Senza limitare i diritti sanciti dal copyright, nessuna parte di questo documento può essere riprodotta, memorizzata o inserita in un sistema di ricerca o trasmessa in qualsiasi forma o mezzo (elettronico o meccanico, mediante fotocopia, registrazione o altro), per alcuno scopo, senza l'autorizzazione scritta di Microsoft Corporation.

Microsoft può essere titolare di brevetti, domande di brevetto, marchi, copyright o altri diritti di proprietà intellettuale relativi all'oggetto del presente documento. Salvo quanto espressamente previsto in un contratto scritto di licenza Microsoft, la consegna del presente documento non implica la concessione di alcuna licenza su tali brevetti, marchi, copyright o altra proprietà intellettuale.

Se non diversamente specificato, la società, organizzazioni, prodotti, nomi di dominio, indirizzi di posta elettronica, logo, persone, luoghi ed eventi citati in questo documento sono fittizio e nessuna associazione con qualsiasi società, organizzazione, prodotto, nome di dominio, indirizzo di posta elettronica indirizzo, logo, persona, luogo o evento è intenzionale o può essere presupposta.

© 2010 Microsoft Corporation. Tutti i diritti sono riservati.

Microsoft e Windows sono marchi registrati o marchi di Microsoft Corporation negli Stati Uniti e/o in altri paesi.

I nomi di società e prodotti reali citati nel presente documento possono essere marchi dei rispettivi proprietari.
