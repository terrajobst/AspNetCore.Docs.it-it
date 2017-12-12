---
uid: whitepapers/aspnet4/breaking-changes
title: Modifiche di rilievo 4 ASP.NET | Documenti Microsoft
author: rick-anderson
description: Questo documento vengono descritte le modifiche apportate per la versione di .NET Framework versione 4 che potrebbe influire sulle applicazioni che sono state create con...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: d601c540-f86b-4feb-890c-20c806b3da6c
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet4/breaking-changes
msc.type: content
ms.openlocfilehash: a0f25ed3c996b73e362177b196539c6f2b143739
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-4-breaking-changes"></a>Modifiche di rilievo 4 ASP.NET
====================
> Questo documento descrive le modifiche apportate per la versione di .NET Framework versione 4 che potrebbe influire sulle applicazioni che sono state create con versioni precedenti, comprese le versioni 1 di ASP.NET 4 Beta e Beta 2.
> 
> [Scaricare il white paper](https://download.microsoft.com/download/7/1/A/71A105A9-89D6-4201-9CC5-AD6A3B7E2F22/ASP_NET_4_Breaking_Changes.pdf)


<a id="0.1__Toc256768952"></a><a id="0.1__Toc256770056"></a>

## <a name="contents"></a>Contenuto

[L'impostazione ControlRenderingCompatibilityVersion nel File Web. config](#0.1__Toc256770141 "_Toc256770141")  
[Le modifiche ClientIDMode](#0.1__Toc256770142 "_Toc256770142")  
[HtmlEncode e UrlEncode ora codificare virgolette](#0.1__Toc256770143 "_Toc256770143")  
[Pagina ASP.NET (aspx) Parser è Stricter](#0.1__Toc256770144 "_Toc256770144")  
[File di definizione del browser aggiornati](#0.1__Toc256770145 "_Toc256770145")  
[System.Web.Mobile.dll rimosso dal File di configurazione Web radice](#0.1__Toc256770146 "_Toc256770146")  
[Convalida della richiesta ASP.NET](#0.1__Toc256770147 "_Toc256770147")  
[Algoritmo hash predefinito è ora HMACSHA256](#0.1__Toc256770148 "_Toc256770148")  
[Errori di configurazione correlati per nuova configurazione di ASP.NET 4 radice](#0.1__Toc256770149 "_Toc256770149")  
[Le applicazioni ASP.NET 4 figlio in grado di avvio in caso di ASP.NET 2.0 o ASP.NET 3.5 applicazioni](#0.1__Toc256770150 "_Toc256770150")  
[Siti Web ASP.NET 4 non venga avviato nei computer in cui è installato SharePoint](#0.1__Toc256770151 "_Toc256770151")  
[La proprietà HttpRequest.FilePath non include più valori PathInfo](#0.1__Toc256770152 "_Toc256770152")  
[ASP.NET 2.0 applicazioni potrebbero generare errori HttpException che fanno riferimento a eurl. axd](#0.1__Toc256770153 "_Toc256770153")  
[I gestori eventi non potrebbero essere generati non in un documento predefinito in IIS 7 o IIS 7.5 in modalità integrata](#0.1__Toc256770154 "_Toc256770154")  
[Modifiche all'implementazione di sicurezza dall'accesso di codice ASP.NET](#0.1__Toc256770155 "_Toc256770155")  
[Sono stati spostati MembershipUser e altri tipi nel Namespace System.Web.Security](#0.1__Toc256770156 "_Toc256770156")  
[Memorizzazione nella cache le modifiche per variare l'output \* intestazione HTTP](#0.1__Toc256770157 "_Toc256770157")  
[Tipi di System.Web.Security per Passport sono Obsolete](#0.1__Toc256770158 "_Toc256770158")  
[La proprietà MenuItem.PopOutImageUrl non riesce a eseguire il rendering di un'immagine in ASP.NET 4](#0.1__Toc256770159 "_Toc256770159")  
[Menu.StaticPopOutImageUrl e non è riuscita per il rendering delle immagini quando i percorsi contenere barre rovesciate Menu.DynamicPopOutImageUrl](#0.1__Toc256770160 "_Toc256770160")  
[Dichiarazione di non responsabilità](#0.1__Toc256770161 "_Toc256770161")

<a id="0.1__ControlRenderingCompatibilityVersio"></a><a id="0.1__Toc245724853"></a><a id="0.1__Toc255587630"></a><a id="0.1__Toc256770141"></a>

## <a name="controlrenderingcompatibilityversion-setting-in-the-webconfig-file"></a>Impostazione ControlRenderingCompatibilityVersion nel File Web. config

I controlli ASP.NET sono stati modificati in .NET Framework versione 4 per consentono di specificare in modo più preciso il rendering markup. Nelle versioni precedenti di .NET Framework, alcuni controlli generavano markup che era possibile disabilitare. Per impostazione predefinita, questo tipo di markup di ASP.NET 4 non viene più generata.

Se si utilizza Visual Studio 2010 per aggiornare l'applicazione da ASP.NET 2.0 o ASP.NET 3.5, lo strumento aggiunge automaticamente un'impostazione per il `Web.config` file che mantiene legacy per il rendering. Se tuttavia si aggiorna un'applicazione modificando il pool di applicazioni in IIS per impostare come destinazione .NET Framework 4, ASP.NET usa la nuova modalità di rendering per impostazione predefinita. Per disattivare la nuova modalità di rendering, aggiungere la seguente impostazione nel `Web.config` file:

[!code-xml[Main](breaking-changes/samples/sample1.xml)]

Come indicato di seguito sono riportate le modifiche principali per il rendering che mette a disposizione il nuovo comportamento:

- Il **immagine** e **ImageButton** controlli eseguono il rendering non è più un `border="0"` attributo.
- Il **BaseValidator** classe e convalida i controlli che derivano da essa non è più il rendering del testo di colore rosso per impostazione predefinita.
- Il **HtmlForm** non eseguire il rendering di un **nome** attributo.
- Il **tabella** rendering del controllo non è più un `border="0"` attributo.
- I controlli che non sono progettati per l'input dell'utente (ad esempio, il **etichetta** controllo) non eseguire il rendering il `disabled="disabled"` attributo se loro **abilitato** è impostata su **false**(o se questa impostazione ereditare un controllo contenitore).

<a id="0.1__Toc245724854"></a><a id="0.1__Toc255587631"></a><a id="0.1__Toc256770142"></a>

## <a name="clientidmode-changes"></a>Modifiche ClientIDMode

Il **ClientIDMode** impostazione in ASP.NET 4 consente di specificare la modalità di ASP.NET genera il **id** attributo per gli elementi HTML. Nelle versioni precedenti di ASP.NET, il comportamento predefinito era equivalente al **AutoID** l'impostazione di **ClientIDMode**. Tuttavia, l'impostazione predefinita è ora **stimabile**.

Se si utilizza Visual Studio 2010 per aggiornare l'applicazione da ASP.NET 2.0 o ASP.NET 3.5, lo strumento aggiunge automaticamente un'impostazione per il `Web.config` file che consente di mantenere il comportamento delle versioni precedenti di .NET Framework. Se tuttavia si aggiorna un'applicazione modificando il pool di applicazioni in IIS per impostare come destinazione .NET Framework 4, ASP.NET usa la nuova modalità per impostazione predefinita. Per disattivare la nuova modalità di ID client, aggiungere la seguente impostazione nel `Web.config` file:

[!code-xml[Main](breaking-changes/samples/sample2.xml)]

<a id="0.1__Toc245724855"></a><a id="0.1__Toc255587632"></a><a id="0.1__Toc256770143"></a>

## <a name="htmlencode-and-urlencode-now-encode-single-quotation-marks"></a>HtmlEncode e UrlEncode ora codificare tra virgolette singole

In ASP.NET 4, il **HtmlEncode** e **UrlEncode** metodi il **HttpUtility** e **HttpServerUtility** classi sono state aggiornate per codifica il carattere di virgoletta singola (') come indicato di seguito:

- Il **HtmlEncode** metodo codifica le istanze della virgoletta singola come.
- Il **UrlEncode** metodo codifica le istanze della virgoletta singola come % 27.

<a id="0.1__Toc255587633"></a><a id="0.1__Toc256770144"></a><a id="0.1__Toc245724856"></a>

## <a name="aspnet-page-aspx-parser-is-stricter"></a>Pagina ASP.NET (aspx) Parser è Stricter

Il parser della pagina per le pagine ASP.NET (`.aspx` file) e controlli utente (`.ascx` file) più restrittivo in ASP.NET 4 e rifiuterà più istanze di markup non valido. Ad esempio, i seguenti due frammenti di codice sarebbe analizzare correttamente nelle versioni precedenti di ASP.NET, ma ora genererà errori del parser in ASP.NET 4.

[!code-aspx[Main](breaking-changes/samples/sample3.aspx)]

Il punto e virgola non valido alla fine di osservare il **HiddenField** tag.

[!code-aspx[Main](breaking-changes/samples/sample4.aspx)]

Si noti il chiuso **stile** attributo che viene eseguito nel **CssClass** attributo.

<a id="0.1__Toc255587634"></a><a id="0.1__Toc256770145"></a>

## <a name="browser-definition-files-updated"></a>File di definizione del browser aggiornati

I file di definizione del browser sono stati aggiornati per includere informazioni su browser e dispositivi nuovi e aggiornati. Sono stati rimossi browser e dispositivi precedenti come Netscape Navigator e sono stati aggiunti browser e dispositivi più recenti come Google Chrome e Apple iPhone.

Se l'applicazione in uso contiene definizioni del browser personalizzate che derivano da una delle definizioni del browser rimosse, viene visualizzato un errore. Ad esempio, se il `App_Browsers` cartella contiene una definizione del browser che eredita dalla definizione del browser IE2, si riceverà il messaggio di errore di configurazione seguenti:

- Impossibile trovare l'elemento browser o gateway con ID 'IE2'.

> [!NOTE]
> Il **HttpBrowserCapabilities** oggetto (che viene esposta nella pagina **Request** proprietà) è determinato dai file di definizioni del browser. Pertanto, le informazioni restituite da accesso a una proprietà di questo oggetto in ASP.NET 4 potrebbero essere diverse le informazioni restituite in una versione precedente di ASP.NET.


È possibile ripristinare i file di definizione del browser precedenti copiando i file di definizione del browser dalla seguente cartella:

[!code-console[Main](breaking-changes/samples/sample5.cmd)]

Copiare i file nella corrispondente `\CONFIG\Browsers` cartella per ASP.NET 4. Dopo aver copiato i file, eseguire Aspnet\_strumento da riga di comando regbrowsers.exe.

<a id="0.1__Toc255587635"></a><a id="0.1__Toc256770146"></a>

## <a name="systemwebmobiledll-removed-from-root-web-configuration-file"></a>System.Web.Mobile.dll rimossa dal File di configurazione Web radice

Nelle versioni precedenti di ASP.NET, un riferimento all'assembly System.Web.Mobile.dll è stato incluso nella directory principale `Web.config` file nel **assembly** sezione in. Per migliorare le prestazioni, il riferimento a questo assembly è stato rimosso.

L'assembly System.Web.Mobile.dll è inclusa in ASP.NET 4, ma è deprecato. Se si desidera utilizzare i tipi dall'assembly System, è possibile aggiungere un riferimento all'assembly alla radice della `Web.config` file o a un'applicazione `Web.config` file. Ad esempio, se si desidera utilizzare uno dei controlli per dispositivi mobili ASP.NET (obsoleti), è necessario aggiungere un riferimento all'assembly System per il `Web.config` file.

<a id="0.1__Toc245724857"></a><a id="0.1__Toc255587636"></a><a id="0.1__Toc256770147"></a>

## <a name="aspnet-request-validation"></a>Convalida della richiesta ASP.NET

La funzionalità di convalida richiesta ASP.NET fornisce un certo livello di protezione predefinita da attacchi di cross-site scripting (XSS). Nelle versioni precedenti di ASP.NET, la convalida della richiesta è stata abilitata per impostazione predefinita. Tuttavia, applicato solo alle pagine ASP.NET (`.aspx` file e i file di classe) e solo quando tali pagine sono in esecuzione.

In ASP.NET 4, per impostazione predefinita, la convalida delle richieste è abilitata per tutte le richieste, perché è abilitata prima di **BeginRequest** fase di una richiesta HTTP. Di conseguenza, la convalida della richiesta si applica alle richieste per tutte le risorse ASP.NET, non solo le richieste di pagine aspx. Questo include le richieste, ad esempio chiamate al servizio Web e i gestori HTTP personalizzati. Convalida della richiesta è attiva anche se i moduli HTTP personalizzati legge il contenuto di una richiesta HTTP.

Di conseguenza, ora possono verificarsi errori di convalida di richiesta per le richieste che in precedenza non aver generato gli errori. Per ripristinare il comportamento della funzionalità di convalida richiesta di ASP.NET 2.0, aggiungere la seguente impostazione nel `Web.config` file:

[!code-xml[Main](breaking-changes/samples/sample6.xml)]

Tuttavia, si consiglia di analizzare gli errori di convalida richiesta per determinare se i gestori esistenti, i moduli o altro codice personalizzato potenzialmente accede input HTTP non sicuri che potrebbe essere XSS attacchi vettori.

<a id="0.1__Toc245724858"></a><a id="0.1__Toc255587637"></a><a id="0.1__Toc256770148"></a>

## <a name="default-hashing-algorithm-is-now-hmacsha256"></a>Algoritmo hash predefinito è ora HMACSHA256

Per la protezione di dati come i cookie di autenticazione modulo e lo stato di visualizzazione, ASP.NET usa sia la crittografia che gli algoritmi hash. Per impostazione predefinita, ASP.NET 4 utilizza l'algoritmo HMACSHA256 ora per le operazioni di hash su cookie e lo stato di visualizzazione. Le versioni precedenti di ASP.NET è utilizzato l'algoritmo HMACSHA1 precedente.

Le applicazioni possono essere influenzate se si esegue misto 2.0/ASP.NET ASP.NET 4 ambienti in cui i dati, ad esempio i cookie di autenticazione form devono essere utilizzati across.NET Framework versioni. Per configurare un'applicazione Web ASP.NET 4 per utilizzare l'algoritmo HMACSHA1 precedente, aggiungere la seguente impostazione nel `Web.config` file:

[!code-xml[Main](breaking-changes/samples/sample7.xml)]

<a id="0.1__Toc245724859"></a><a id="0.1__Toc255587638"></a><a id="0.1__Toc256770149"></a>

## <a name="configuration-errors-related-to-new-aspnet-4-root-configuration"></a>Errori di configurazione relativi alla nuova configurazione di ASP.NET 4 radice

I file di configurazione radice (il `machine.config` file e directory principale `Web.config` file) per .NET Framework 4 (e pertanto ASP.NET 4) sono stati aggiornati per includere la maggior parte delle informazioni di configurazione standard che in ASP.NET 3.5 è state trovate di applicazione `Web.config` file. A causa della complessità dei sistemi configurazione IIS 7 e IIS 7.5 gestiti, esecuzione di applicazioni ASP.NET 3.5 in ASP.NET 4 e in IIS 7 e IIS 7.5 può comportare ASP.NET o IIS errori di configurazione.

È consigliabile aggiornare le applicazioni ASP.NET 3.5 per ASP.NET 4 tramite gli strumenti di aggiornamento del progetto in Visual Studio 2010, se possibile. Visual Studio 2010 modifica automaticamente un'applicazione ASP.NET 3.5 `Web.config` file contiene le impostazioni appropriate per ASP.NET 4.

Tuttavia, è uno scenario supportato per l'esecuzione di applicazioni ASP.NET 3.5 utilizzando .NET Framework 4 senza ricompilazione. In tal caso, potrebbe essere necessario modificare manualmente l'applicazione `Web.config` file prima di eseguire l'applicazione in .NET Framework 4 e in IIS 7 o IIS 7.5.

Le prossime due sezioni descrivono le modifiche che potrebbe essere necessario apportare per diverse combinazioni di software.

**Windows Vista SP1 o Windows Server 2008 SP1, in cui sono installati hotfix KB958854 né SP2.** In questa configurazione, il sistema di configurazione di IIS 7 in modo non corretto unisce configurazione di un'applicazione gestita tramite il confronto a livello di applicazione `Web.config` file a ASP.NET 2.0 `machine.config` file. A causa di questo, a livello di applicazione `Web.config` da .NET Framework 3.5 o versioni successive i file devono avere un **Extensions** definizione di sezione di configurazione (elemento) in modo da non causare un errore di convalida IIS 7.

Tuttavia, modificare manualmente a livello di applicazione `Web.config` le voci di file che non corrispondono esattamente le definizioni di tipo sezione di configurazione boilerplate originale che sono stati introdotti con Visual Studio 2008 causerà errori di configurazione ASP.NET. (Le voci di configurazione predefinite che vengono generate da Visual Studio 2008 funzionano correttamente). Un problema comune è che modificato manualmente `Web.config` file tralasciare il **allowDefinition** e **requirePermission** gli attributi di configurazione che si trovano nella sezione di configurazione diversi definizioni. In questo modo una mancata corrispondenza tra la sezione di configurazione abbreviato in a livello di applicazione `Web.config` file e la definizione completa in ASP.NET 4 `machine.config` file. Di conseguenza, in fase di esecuzione, il sistema di configurazione di ASP.NET 4 genera un errore di configurazione.

**Windows Vista SP2, Windows Server 2008 SP2, Windows 7, Windows Server 2008 R2 e Windows Vista SP1 e Windows Server 2008 SP1 in cui è installato l'hotfix KB958854.**

In questo scenario, il sistema di configurazione nativo di IIS 7 e IIS 7.5 restituisce un errore di configurazione perché consente di eseguire un confronto del testo sul **tipo** attributo definito per un gestore della sezione di configurazione gestiti. Poiché tutti `Web.config` i file che vengono generati da Visual Studio 2008 e Visual Studio 2008 SP1 hanno "3.5" nella stringa di tipo per il **Extensions** (e correlate) i gestori di sezione di configurazione e poiché ASP.NET 4 `machine.config` file è "4.0" **tipo** attributo per i gestori di sezione di configurazione stesso, le applicazioni che vengono generati in Visual Studio 2008 o Visual Studio 2008 SP1 sempre esito negativo della convalida della configurazione in IIS 7 e IIS 7.5.

<a id="0.1__Toc251910248"></a>

### <a name="resolving-these-issues"></a>Risoluzione di questi problemi

La soluzione alternativa per il primo scenario consiste nell'aggiornare il livello di applicazione `Web.config` file includendo il testo di configurazione standard da un `Web.config` file che è stato generato automaticamente da Visual Studio 2008.

Una soluzione alternativa per il primo scenario è per l'installazione di Service Pack 2 per la Vista o Windows Server 2008 nel computer o installare l'hotfix KB958854 ([https://support.microsoft.com/kb/958854](https://support.microsoft.com/kb/958854)) correggere non corretto comportamento di unione di configurazione del sistema di configurazione IIS. Tuttavia, dopo aver eseguito una di queste azioni, l'applicazione probabilmente verificherà un errore di configurazione a causa del problema descritto per il secondo scenario.

La soluzione per il secondo scenario consiste nell'eliminare o impostare come commento tutti i **Extensions** definizioni di sezione di configurazione e sezione di configurazione del gruppo di definizioni a livello di applicazione `Web.config` file. Queste definizioni sono in genere nella parte superiore del livello di applicazione `Web.config` file e può essere identificato tramite il **configSections** elemento e i relativi elementi figlio.

Per entrambi gli scenari, è consigliabile eliminare manualmente il **System. CodeDom** sezione, anche se non è obbligatorio.

<a id="0.1__Toc252995490"></a><a id="0.1__Toc255587639"></a><a id="0.1__Toc256770150"></a><a id="0.1__Toc245724860"></a>

## <a name="aspnet-4-child-applications-fail-to-start-when-under-aspnet-20-or-aspnet-35-applications"></a>Le applicazioni ASP.NET 4 figlio in grado di avvio in caso di ASP.NET 2.0 o ASP.NET 3.5 applicazioni

Le applicazioni ASP.NET 4 configurate come elementi figlio di applicazioni che eseguono versioni precedenti di ASP.NET potrebbero non avviarsi a causa di errori di compilazione o di configurazione. Nell'esempio seguente viene illustrata una struttura di directory per un'applicazione interessata.

`/parentwebapp`(configurato per utilizzare ASP.NET 2.0 o ASP.NET 3.5)  
`/childwebapp`(configurato per l'utilizzo di ASP.NET 4)

L'applicazione nel `childwebapp` cartella non riuscirà ad avviare in IIS 7 o IIS 7.5 e genererà un errore di configurazione. Il testo di errore includeranno un messaggio simile al seguente:

- `The requested page cannot be accessed because the related configuration data for the page is invalid.`
  

- `The configuration section 'configSections' cannot be read because it is missing a section declaration.`

IIS 6, l'applicazione nel `childwebapp` cartella sarà anche possibile avviare, ma segnala un errore diverso. Ad esempio, il testo dell'errore potrebbe indicare quanto segue:

- `The value for the 'compilerVersion' attribute in the provider options must be 'v4.0' or later if you are compiling for version 4.0 or later of the .NET Framework. To compile this Web application for version 3.5 or earlier of the .NET Framework, remove the 'targetFramework' attribute from the element of the Web.config file`

Questi scenari si verificano perché le informazioni di configurazione dell'applicazione principale nel `parentwebapp` cartella fa parte della gerarchia delle informazioni di configurazione che determina le impostazioni di configurazione unita finale utilizzati da web figlio applicazione di `childwebapp` cartella. A seconda se l'applicazione Web ASP.NET 4 è in esecuzione in IIS 7 (o IIS 7.5) o IIS 6, il sistema di configurazione di IIS o nel sistema di compilazione ASP.NET 4 verrà restituito un errore.

I passaggi da seguire per risolvere il problema e ottenere l'elemento figlio di funzionamento dell'applicazione ASP.NET 4 dipendono se l'applicazione ASP.NET 4 viene eseguito in IIS 6 o IIS 7 (o IIS 7.5).

### <a name="step-1-iis-7-or-iis-75-only"></a>Passaggio 1 (IIS 7 o IIS 7.5)

Questo passaggio è necessario solo in sistemi operativi che eseguono IIS 7 o IIS 7.5, che include Windows Vista, Windows Server 2008, Windows 7 e Windows Server 2008 R2.

Spostare il **configSections** definizione di `Web.config` file dell'applicazione padre (l'applicazione che viene eseguito ASP.NET 2.0 o ASP.NET 3.5) nella directory principale del `Web.config` file per.NET Framework 2.0. Il sistema di configurazione nativo di IIS 7 e IIS 7.5 viene analizzato il **configSections** elemento quando esegue il merge della gerarchia di file di configurazione. Lo spostamento di **configSections** definizione dall'applicazione Web padre `Web.config` file nella radice `Web.config` file nasconde in modo efficace l'elemento dal processo di configurazione di tipo merge che si verifica per l'elemento figlio di ASP.NET 4 applicazione.

In un sistema operativo a 32 bit o per i pool di applicazioni a 32 bit, la radice `Web.config` file per ASP.NET 2.0 e 3.5 di ASP.NET in genere si trova nella cartella seguente:

`C:\Windows\Microsoft.NET\Framework\v2.0.50727\CONFIG`

In un sistema operativo a 64 bit o 64 bit pool di applicazioni, la radice `Web.config` file per ASP.NET 2.0 e 3.5 di ASP.NET in genere si trova nella cartella seguente:

`C:\Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG`

Se si eseguono applicazioni di Web sia a 32 bit e a 64 bit in un computer a 64 bit, è necessario spostare il **configSections** elemento backup nella directory principale `Web.config` file per i sistemi a 64 bit e 32 bit.

Quando si inserisce il **configSections** elemento nella radice `Web.config` file, incollare la sezione immediatamente dopo il **configurazione** elemento. L'esempio seguente mostra quali nella parte superiore della radice `Web.config` file dovrebbe essere simile al termine del spostando gli elementi.

> [!NOTE]
> Nell'esempio seguente, le righe sono state integrate per migliorare la leggibilità.


[!code-xml[Main](breaking-changes/samples/sample8.xml)]

### <a name="step-2-all-versions-of-iis"></a>Passaggio 2 (tutte le versioni di IIS)

Questo passaggio è obbligatorio se l'elemento figlio di ASP.NET 4 applicazione Web è in esecuzione su IIS 6 o IIS 7 (o IIS 7.5).

Nel `Web.config` file dell'elemento padre dell'applicazione Web che esegue 2 ASP.NET o ASP.NET 3.5, aggiungere un **percorso** tag che specifica in modo esplicito (per i sistemi di configurazione di IIS e ASP.NET) che solo le voci di configurazione si applicano all'applicazione Web padre. Nell'esempio seguente mostra la sintassi del **percorso** elemento da aggiungere:

[!code-xml[Main](breaking-changes/samples/sample9.xml)]

L'esempio seguente viene illustrato il modo in **percorso** tag viene utilizzato per eseguire il wrapping di tutte le sezioni di configurazione a partire dal **appSettings** sezione e terminando con **System. webServer**sezione.

[!code-xml[Main](breaking-changes/samples/sample10.xml)]

Quando sono stati completati i passaggi 1 e 2, le applicazioni Web ASP.NET 4 figlio verranno avviato senza errori.

<a id="0.1__Toc252995491"></a><a id="0.1__Toc255587640"></a><a id="0.1__Toc256770151"></a>

## <a name="aspnet-4-web-sites-fail-to-start-on-computers-where-sharepoint-is-installed"></a>Siti Web ASP.NET 4 non venga avviato nei computer in cui è installato SharePoint

Server Web che eseguono SharePoint dispongono di un `Web.config` file che viene distribuito nella radice di un sito Web di SharePoint (ad esempio, `c:\inetpub\wwwroot\web.config` per sito Web predefinito). In questo `Web.config` file SharePoint livello è impostato un di attendibilità parziale personalizzato denominato WSS\_minimo.

Se si tenta di eseguire un sito Web ASP.NET 4 che viene distribuito come figlio di questo tipo di sito Web di SharePoint, si visualizzerà l'errore seguente:

`Could not find permission set named 'ASP.NET'.`

Questo errore si verifica perché l'infrastruttura di sicurezza dall'accesso di codice ASP.NET 4 esegue la ricerca di un set di autorizzazioni denominato ASP.NET. Tuttavia, parziale considerare attendibile il file di configurazione che fa riferimento WSS\_minima non contiene alcun set di autorizzazioni con lo stesso nome.

Non è attualmente presente una versione di SharePoint che è compatibile con ASP.NET. Di conseguenza, non tentare di eseguire un sito Web di ASP.NET 4 come sito figlio di sotto di Web di SharePoint i siti.

<a id="0.1__Toc255587641"></a><a id="0.1__Toc256770152"></a>

## <a name="the-httprequestfilepath-property-no-longer-includes-pathinfo-values"></a>La proprietà HttpRequest.FilePath non include più valori PathInfo

Le versioni precedenti di ASP.NET incluso un **PathInfo** valore nel valore restituito da vari file relativo al percorso proprietà, tra cui **HttpRequest.FilePath**,  **HttpRequest.AppRelativeCurrentExecutionFilePath**, e **HttpRequest.CurrentExecutionFilePath**. In ASP.NET 4 non include più il **PathInfo** valore nei valori restituiti da queste proprietà. Al contrario, il **PathInfo** informazioni sono disponibili in **HttpRequest.PathInfo**. Considerare ad esempio il frammento di URL seguente:

`/testapp/Action.mvc/SomeAction`

Nelle versioni precedenti di ASP.NET, **HttpRequest** proprietà valori sono i seguenti:

**HttpRequest.FilePath**:`/testapp/Action.mvc/SomeAction`

**HttpRequest.PathInfo**: (vuoto)

In ASP.NET 4, **HttpRequest** proprietà invece valori sono i seguenti:

**HttpRequest.FilePath**:`/testapp/Action.mvc`

**HttpRequest.PathInfo**:`SomeAction`

<a id="0.1__Toc252995493"></a><a id="0.1__Toc255587642"></a><a id="0.1__Toc256770153"></a><a id="0.1__Toc245724861"></a>

## <a name="aspnet-20-applications-might-generate-httpexception-errors-that-reference-eurlaxd"></a>ASP.NET 2.0 applicazioni potrebbero generare errori HttpException che fanno riferimento a eurl. axd

Dopo l'abilitazione di ASP.NET 4 IIS 6, applicazioni ASP.NET 2.0 in esecuzione su IIS 6 (in Windows Server 2003 o Windows Server 2003 R2) potrebbero generare errori, ad esempio le operazioni seguenti:

`System.Web.HttpException: Path '/[yourApplicationRoot]/eurl.axd/[Value]' was not found.`

Questo errore si verifica perché quando ASP.NET rileva che un sito Web sia configurato per l'utilizzo di ASP.NET 4, un componente nativo di ASP.NET 4 passa un URL senza estensione per la parte gestita di ASP.NET per un'ulteriore elaborazione. Tuttavia, se le directory virtuali che sono di sotto di un sito Web di ASP.NET 4 sono configurate per utilizzare ASP.NET 2.0, elaborazione dell'URL senza estensione nei risultati modo in un URL modificato che contiene la stringa "eurl. axd". Questo URL modificato viene quindi inviato all'applicazione ASP.NET 2.0. ASP.NET 2.0 non è in grado di riconoscere il formato "eurl. axd". Pertanto, ASP.NET 2.0 tenta di trovare un file denominato `eurl.axd` ed eseguirlo. Poiché tale file non esiste, la richiesta ha esito negativo con un **HttpException** eccezione.

È possibile risolvere questo problema utilizzando una delle opzioni seguenti.

### <a name="option-1"></a>opzione 1

Se non è richiesto per eseguire il sito Web ASP.NET 4, modificare il mapping del sito per l'utilizzo con ASP.NET 2.0.

### <a name="option-2"></a>Opzione 2

Se ASP.NET 4 è necessario per eseguire il sito Web, è possibile spostare le directory virtuali di ASP.NET 2.0 figlio a un sito Web diverso che viene eseguito il mapping a ASP.NET 2.0.

### <a name="option-3"></a>Opzione 3

Se non è possibile modificare il mapping del sito Web per ASP.NET 2.0 o per modificare il percorso di una directory virtuale, è necessario disabilitare in modo esplicito l'elaborazione in ASP.NET 4 di URL senza estensione. Utilizzare la procedura seguente:

1. Nel Registro di sistema Windows, aprire il nodo seguente:

`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ASP.NET\4.0.30319.0`

1. Creare un nuovo **DWORD** valore denominato **EnableExtensionlessUrls**.
2. Impostare **EnableExtensionlessUrls** su 0. Disabilita il comportamento degli URL.
3. Il valore del Registro di sistema di salvare e chiudere l'editor del Registro di sistema.
4. Eseguire il **iisreset** strumento della riga di comando, che viene avviata leggere il nuovo valore del Registro di sistema.

> [!NOTE]
> Impostazione **EnableExtensionlessUrls** su 1 abilita il comportamento degli URL. Questo è l'impostazione predefinita se è specificato alcun valore.


<a id="0.1__Toc252995494"></a><a id="0.1__Toc255587643"></a><a id="0.1__Toc256770154"></a><a id="0.1__Toc245724862"></a>

## <a name="event-handlers-might-not-be-not-raised-in-a-default-document-in-iis-7-or-iis-75-integrated-mode"></a>I gestori eventi non potrebbero essere generati non in un documento predefinito in IIS 7 o IIS 7.5 in modalità integrata

ASP.NET 4 include modifiche che modificano il modo in **azione** attributo del codice HTML **modulo** elemento sottoposto a rendering quando risolve un URL senza estensione a un documento predefinito. Un esempio di URL senza estensione risolvere in un documento predefinito può essere [http://contoso.com/](http://contoso.com/), risultante in una richiesta di [http://contoso.com/Default.aspx](http://contoso.com/Default.aspx).

In ASP.NET 4 ora esegue il rendering HTML **modulo** dell'elemento **azione** valore dell'attributo come una stringa vuota quando viene effettuata una richiesta a un URL senza estensione a cui è mappato a esso un documento predefinito. Ad esempio, nelle versioni precedenti di ASP.NET, una richiesta di [http://contoso.com](http://contoso.com) darà origine a una richiesta di `Default.aspx`. In tale documento, l'apertura **modulo** tag verrebbe eseguito come nell'esempio seguente:

`<form action="Default.aspx" />`

In ASP.NET 4, una richiesta di [http://contoso.com](http://contoso.com) anche i risultati in una richiesta di `Default.aspx`. Tuttavia, ASP.NET ora esegue il rendering dell'apertura HTML **modulo** tag come nell'esempio seguente:

`<form action="" />`

Questa differenza nel modo in cui il **azione** attributo sottoposto a rendering può causare lievi modifiche in modalità di elaborazione di un post del form da IIS e ASP.NET. Quando il **azione** attributo è una stringa vuota, IIS **DefaultDocumentModule** oggetto verrà creata una richiesta figlio per `Default.aspx`. Nella maggior parte dei casi, questa richiesta figlio è trasparente al codice dell'applicazione e `Default.aspx` pagina viene eseguito normalmente.

Tuttavia una potenziale interazione tra il codice gestito e la modalità integrata di IIS 7 o IIS 7.5 può interrompere il funzionamento corretto delle pagine gestite con estensione aspx durante la richiesta figlio. Se le condizioni seguenti si verificano, la richiesta figlio a un `Default.aspx` documento verrà generato un errore o un comportamento imprevisto:

1. Una pagina aspx viene inviata al browser con il **modulo** dell'elemento **azione** attributo impostato su "".
2. Il modulo viene eseguito il postback ASP.NET.
3. Un modulo HTTP gestito legge una parte del corpo dell'entità. Ad esempio, un modulo legge **Request. Form** o **Request**. Di conseguenza il corpo dell'entità della richiesta POST viene letto nella memoria gestita. Pertanto il corpo dell'entità non è più disponibile per i moduli di codice nativo che sono in esecuzione in modalità integrata IIS 7 o IIS 7.5.
4. IIS **DefaultDocumentModule** oggetto alla fine viene eseguito e crea una richiesta figlio per il `Default.aspx` documento. Tuttavia poiché il corpo dell'entità è già stato letto da un frammento di codice gestito, non è disponibile alcun corpo di entità da inviare alla richiesta figlio.
5. Quando la pipeline HTTP viene eseguito per la richiesta figlio, il gestore per `.aspx` file viene eseguito durante la fase di esecuzione gestore.
6. Poiché non esiste alcun corpo di entità, esistono non variabili dei form e non lo stato di visualizzazione e pertanto non sono disponibili informazioni per il gestore di pagina aspx determinare quale evento (se presente) deve essere generato. Pertanto non viene eseguito nessun gestore dell'evento di postback per la pagina con estensione aspx interessata.

È possibile risolvere questo problema nei modi seguenti:

- Identificare il modulo HTTP che accede corpo entità della richiesta durante le richieste di documenti predefiniti e determinare se può essere configurato per l'esecuzione solo per le richieste gestite. In modalità integrata per IIS 7 e IIS 7.5, moduli HTTP possono essere contrassegnati per l'esecuzione solo per le richieste gestite aggiungendo l'attributo seguente al modulo **WebServer/Modules** voce:

- `precondition="managedHandler"`

- Disabilita l'impostazione del modulo per le richieste di IIS 7 e IIS 7.5 determinare come non gestite le richieste. Per le richieste di documento predefinito, la prima richiesta è per un URL senza estensione. Pertanto, IIS non esegue tutti i moduli gestiti contrassegnati con una precondizione del gestore gestito durante l'elaborazione della richiesta iniziale. Di conseguenza, i moduli gestiti non verranno lette accidentalmente il corpo dell'entità e pertanto il corpo dell'entità è ancora disponibile e viene passato per la richiesta figlio e per il documento predefinito.

- Se è necessario eseguire per le richieste di tutti i moduli HTTP problematici (per i file statici, per URL senza estensione che si risolvono nel **DefaultDocumentModule** oggetto, per le richieste gestite e così via), modificare in modo esplicito le pagine aspx interessato da l'impostazione di **azione** la pagina proprietà **System.Web.UI.HtmlControls.HtmlForm** controllo in una stringa non vuota. Ad esempio, se il documento predefinito è `Default.aspx`, modificare il codice della pagina per impostare in modo esplicito il **HtmlForm** del controllo **azione** proprietà su "Default.aspx".

<a id="0.1__Toc255587644"></a><a id="0.1__Toc256770155"></a>

## <a name="changes-to-the-aspnet-code-access-security-cas-implementation"></a>Modifiche all'implementazione di sicurezza dall'accesso di codice ASP.NET

ASP.NET 2.0, e dall'estensione per le funzionalità ASP.NET che sono stati aggiunti in 3.5, utilizzare il .NET Framework 1.1 e un modello di sicurezza dall'accesso di codice 2.0. Tuttavia l'implementazione di CAS in ASP.NET 4 è stata modificata in modo sostanziale. Di conseguenza, le applicazioni ASP.NET parzialmente attendibile che si basano su codice attendibile in esecuzione nella global assembly cache (GAC) potrebbero non riuscire con varie eccezioni di sicurezza. Applicazioni parzialmente attendibili che si basano su modifiche estese ai criteri di sicurezza del computer potrebbero inoltre non riuscire con eccezioni di sicurezza.

È possibile ripristinare applicazioni ASP.NET 4 con attendibilità parziale per il comportamento di ASP.NET 1.1 e 2.0 utilizzando il nuovo **legacyCasModel** attributo la **trust** elemento di configurazione, come illustrato nell'esempio seguente :

`<trust level= "Medium" legacyCasModel="true" />`

Quando si ripristina il modello legacy di autorità di certificazione, sono abilitati i seguenti comportamenti di autorità di certificazione precedenti:

- Criteri di sicurezza per macchina viene rispettata.
- Sono consentiti più set di autorizzazioni diverse in un singolo dominio applicazione.
- Le asserzioni di autorizzazioni esplicite non sono necessari per gli assembly nella GAC che vengono richiamati quando solo ASP.NET o altro codice di .NET Framework è nello stack.

Uno scenario non può essere annullato in .NET Framework 4: applicazioni con attendibilità parziale non Web non è più possono chiamare alcune API in System.Web.dll e System.Web.Extensions.dll. Nelle versioni precedenti di .NET Framework, è possibile per applicazioni con attendibilità parziale non Web a cui viene concesso in modo esplicito **AspNetHostingPermission** autorizzazioni. Quindi è possibile utilizzare queste applicazioni **System.Web.HttpUtility**, digita il **System.Web.ClientServices.\***  spazi dei nomi e tipi correlati all'appartenenza, ruoli e profili. Chiamata di questi tipi da applicazioni con attendibilità parziale non Web non è più supportata in .NET Framework 4.

> [!NOTE]
> Il **HtmlEncode** e **HtmlDecode** funzionalità del **System.Web.HttpUtility** classe sia stata spostata di nuovo .NET Framework 4  **System.Net.WebUtility** classe. Se la funzionalità ASP.NET sola che veniva utilizzata, modificare il codice dell'applicazione per utilizzare la nuova **WebUtility** classe.


Di seguito è riportato un riepilogo generale delle modifiche per l'implementazione di autorità di certificazione predefinita in ASP.NET 4:

- Domini dell'applicazione ASP.NET sono ora domini dell'applicazione omogenei. Solo i set di autorizzazioni di attendibilità parziale e attendibilità totale sono disponibili in un dominio applicazione.
- Set di autorizzazioni di attendibilità parziale ASP.NET sono indipendenti da qualsiasi criterio di CA a livello aziendale, a livello di computer o a livello di utente.
- Assembly ASP.NET forniti in 3.5 e 3.5 SP1 sono stati convertiti per utilizzare il modello di trasparenza di .NET Framework 4.
- Utilizzo di ASP.NET **AspNetHostingPermission** attributo è state ridotte. La maggior parte delle istanze di questo attributo sono state rimosse le APIs ASP.NET pubbliche.
- Assembly compilato dinamicamente creati dai provider di compilazione ASP.NET sono stati aggiornati per contrassegnare in modo esplicito gli assembly come trasparente.
- Tutti gli assembly ASP.NET sono ora contrassegnati in modo che l'attributo APTCA viene rispettato solo in ambienti di hosting Web. Ambienti di hosting Web non parzialmente attendibili come ClickOnce non sarà in grado di eseguire chiamate nell'assembly ASP.NET.

Per ulteriori informazioni sul nuovo modello di sicurezza dall'accesso codice di ASP.NET 4, vedere [utilizzo della protezione di accesso di codice nelle applicazioni ASP.NET](https://msdn.microsoft.com/en-us/library/dd984947%28VS.100%29.aspx) nel sito Web MSDN.

<a id="0.1__Toc256770156"></a><a id="0.1__Toc245724863"></a><a id="0.1__Toc252995496"></a><a id="0.1__Toc255587645"></a><a id="0.1__Toc245724864"></a>

## <a name="membershipuser-and-other-types-in-the-systemwebsecurity-namespace-have-been-moved"></a>Sono stati spostati MembershipUser e altri tipi nel Namespace System.Web.Security

Alcuni tipi utilizzati nel sistema di appartenenze ASP.NET sono stati spostati dal `System.Web.dll` al nuovo assembly System. I tipi sono stati spostati per risolvere dipendenze dei livelli di architettura tra i tipi nel client e negli SKU estesi di .NET Framework.

Progetti di sito Web non dispone dei problemi in seguito lo spostamento di questi tipi, perché System è stato aggiunto all'elenco di assembly di riferimento utilizzato per impostazione predefinita dal sistema di compilazione ASP.NET. Se si aggiorna un progetto di sito Web che è stato creato utilizzando una versione precedente di ASP.NET ad ASP.NET 4, aprirlo in Visual Studio 2010, il progetto verrà compilato senza errori.

Analogamente, se si aggiorna un progetto di applicazione Web che è stato creato in una versione precedente di ASP.NET ad ASP.NET 4, aprirlo in Visual Studio 2010, il processo di aggiornamento aggiunge un riferimento a System per il progetto. Di conseguenza, aggiornare Web progetti di applicazioni verranno inoltre compilato senza errori.

File (binari) compilati creati con versioni precedenti di ASP.NET vengono eseguiti anche senza errori in ASP.NET 4, anche se sono stati spostati i tipi di appartenenza a un assembly diverso. Tipo di informazioni di inoltro è stato aggiunto alla versione di ASP.NET 4 `System.Web.dll` che distribuisce automaticamente i riferimenti in fase di esecuzione per questi tipi per il nuovo percorso per i tipi.

Tuttavia, le librerie di classi che utilizzano tipi di appartenenza specifico e che sono stati aggiornati da versioni precedenti di ASP.NET avrà esito negativo per la compilazione quando utilizzato in un progetto ASP.NET 4. Ad esempio, un progetto libreria di classi potrebbe non riuscire compilare e segnalare un errore analogo al seguente:

- `The type 'System.Web.Security.MembershipUser' is defined in an assembly that is not referenced. You must add a reference to assembly 'System.Web.ApplicationServices, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
  

- `The type name 'MembershipUser' could not be found. This type has been forwarded to assembly 'System.Web.ApplicationServices, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'. Consider adding a reference to that assembly.`

È possibile risolvere il problema aggiungendo un riferimento nel progetto libreria di classi System.

Nell'elenco seguente viene illustrata la *System.Web.Security* tipi che sono stati spostati dal `System.Web.dll` da System:

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

Nella versione 1.0 di ASP.NET, un bug causato pagine memorizzate nella cache specificati `Location="ServerAndClient"` come un'impostazione della cache di output per generare un `Vary:*` intestazione HTTP nella risposta. Questa intestazione comunicava ai browser client di non memorizzare mai la pagina nella cache in locale.

In ASP.NET 1.1, il **System.Web.HttpCachePolicy.SetOmitVaryStar** metodo è stato aggiunto, che è possibile chiamare per eliminare il `Vary:*` intestazione. Questo metodo è stato scelto perché modifica l'intestazione HTTP generato è stata considerata potenzialmente importanti modifiche al momento. Tuttavia, gli sviluppatori hanno stato confuso da parte del comportamento in ASP.NET e i report di bug suggeriscono che gli sviluppatori sono a conoscenza dell'oggetto esistente **SetOmitVaryStar** comportamento.

In ASP.NET 4, la decisione è stata effettuata per risolvere il problema principale. Il `Vary:*` intestazione HTTP non viene generata dalle risposte che specificano la direttiva seguente:

`<%@OutputCache Location="ServerAndClient" %>`

Di conseguenza, **SetOmitVaryStar** non è più necessario per eliminare il `Vary:*` intestazione.

Nelle applicazioni che specificano `Location="ServerAndClient"` nel **@ OutputCache** direttiva in una pagina, verrà visualizzato il comportamento implicito nel nome del **percorso** il valore dell'attributo, ovvero, pagine saranno memorizzabile nella cache nel browser senza che si chiama il **SetOmitVaryStar** metodo.

Se è necessario che le pagine dell'applicazione `Vary:*`, chiamare il **AppendHeader** (metodo), come nell'esempio seguente:

`HttpResponse.AppendHeader("Vary","*");`

In alternativa, è possibile modificare il valore della memorizzazione nella cache di output **percorso** attributo "Server".

<a id="0.1__Toc255587646"></a><a id="0.1__Toc256770158"></a>

## <a name="systemwebsecurity-types-for-passport-are-obsolete"></a>Tipi di System.Web.Security per Passport sono Obsolete

Il supporto Passport incorporato in ASP.NET 2.0 è stato obsoleto e non supportati da alcuni anni a causa di modifiche a Passport (ora LiveID). Di conseguenza, i cinque tipi correlati a Passport in **System.Web.Security** sono ora contrassegnati con il **ObsoleteAttribute** attributo.

<a id="0.1__The_MenuItem.PopOutImageUrl_Propert"></a><a id="0.1__Toc256770159"></a>

## <a name="the-menuitempopoutimageurl-property-fails-to-render-an-image-in-aspnet-4"></a>La proprietà MenuItem.PopOutImageUrl non riesce a eseguire il rendering di un'immagine in ASP.NET 4

In ASP.NET 3.5, il *MenuItem.PopOutImageUrl* proprietà consente di specificare l'URL per un'immagine che viene visualizzata in una voce di menu per indicare che la voce di menu dispone di un sottomenu dinamico. Nell'esempio seguente viene illustrato come specificare questa proprietà nel markup in ASP.NET 3.5.

[!code-aspx[Main](breaking-changes/samples/sample11.aspx)]

In seguito a una modifica della progettazione in ASP.NET 4, non viene restituito alcun output per il *PopOutImageUrl* se la proprietà è impostata per il *MenuItem* classe. In alternativa, è necessario specificare l'URL di un'immagine direttamente nel *Menu* controllo utilizzando il *StaticPopOutImageUrl* proprietà o *DynamicPopOutImageUrl* proprietà. Quando si lavora con un menu statico, il *Menu.StaticPopOutImageUrl* proprietà specifica l'URL per un'immagine visualizzata per indicare che la voce di menu statico dispone di un sottomenu, come illustrato nell'esempio seguente:

[!code-aspx[Main](breaking-changes/samples/sample12.aspx)]

Se si lavora con un menu dinamico, utilizzare il *Menu.DynamicPopOutImageUrl* proprietà per specificare l'URL per un'immagine che indica che una voce di menu dinamico dispone di un sottomenu. Nell'esempio seguente è simile a quella precedente, ma viene illustrato come impostare il *DynamicPopOutImageUrl* proprietà per un menu dinamico.

[!code-aspx[Main](breaking-changes/samples/sample13.aspx)]

Se il *Menu.DynamicPopOutImageUrl* non è impostata e *Menu.DynamicEnableDefaultPopOutImage* è impostata su *false*, viene visualizzata alcuna immagine. Analogamente, se il *StaticPopOutImageUrl* non è impostata e *StaticEnableDefaultPopOutImage* è impostata su *false*, viene visualizzata alcuna immagine.

Quando si impostano i percorsi di queste proprietà, utilizzare una barra (/) anziché una barra rovesciata (\). Per ulteriori informazioni, vedere [Menu.StaticPopOutImageUrl e non è riuscita a eseguire il rendering di immagini quando i percorsi contenere barre rovesciate Menu.DynamicPopOutImageUrl](#0.1__Menu.StaticPopOutImageUrl_and_Menu. "_Menu.StaticPopOutImageUrl_and_Menu.") in questo documento.

<a id="0.1__Menu.StaticPopOutImageUrl_and_Menu."></a><a id="0.1__Toc256770160"></a>

## <a name="menustaticpopoutimageurl-and-menudynamicpopoutimageurl-fail-to-render-images-when-paths-contain-backslashes"></a>Menu.StaticPopOutImageUrl e Menu.DynamicPopOutImageUrl non riescono a eseguire il rendering delle immagini quando i percorsi contenere barre rovesciate

In ASP.NET 4, le immagini specificato mediante il *Menu.StaticPopOutImageUrl* e *Menu.DynamicPopOutImageUrl* proprietà non verranno eseguito il rendering se il percorso contiene backlashes (\). Si tratta di una modifica da versioni precedenti di ASP.NET.

Nell'esempio seguente di *Menu* controllare markup mostrato il *StaticPopOutImageUrl* impostata mediante un percorso che contiene una barra rovesciata. In ASP.NET 4, non sarà possibile l'immagine specificata nella proprietà.

[!code-aspx[Main](breaking-changes/samples/sample14.aspx)]

Per risolvere questo problema, modificare i valori di percorso che vengono specificati nel *StaticPopOutImageUrl* e *DynamicPopOutImageUrl* proprietà da utilizzare le barre (/). Nell'esempio seguente viene illustrata questa modifica:

[!code-aspx[Main](breaking-changes/samples/sample15.aspx)]

Si noti che le applicazioni che sono state migrate da versioni precedenti di ASP.NET ad ASP.NET 4 potrebbero anche essere interessato, poiché il *MenuItem.PopOutImageUrl* è stata modificata. Per ulteriori informazioni, vedere [il MenuItem.PopOutImageUrl proprietà ha esito negativo per il rendering di un'immagine in ASP.NET 4](#0.1__The_MenuItem.PopOutImageUrl_Propert "_The_MenuItem.PopOutImageUrl_Propert") altrove in questo documento.

<a id="0.1__Toc224729061"></a><a id="0.1__Toc255587647"></a><a id="0.1__Toc256770161"></a>

## <a name="disclaimer"></a>Dichiarazione di non responsabilità

Il presente documento è una versione preliminare e può essere modificato in modo sostanziale prima della versione finale commerciale del software qui descritto.

Le informazioni contenute nel presente documento rappresentano l'attuale opinione di Microsoft Corporation circa le problematiche discusse alla data della pubblicazione. Poiché Microsoft deve rispondere ai cambiamenti delle condizioni di mercato, il presente documento non deve essere interpretato quale un impegno da parte di Microsoft e Microsoft non può garantire l'accuratezza delle informazioni presentate dopo la data di pubblicazione.

Il presente white paper è fornito solo a scopi informativi. MICROSOFT NON RILASCIA ALCUNA GARANZIA, ESPRESSA, IMPLICITA O LEGALE, IN MERITO ALLE INFORMAZIONI DEL PRESENTE DOCUMENTO.

Il rispetto di tutte le leggi applicabili in materia di copyright è a esclusivo carico dell'utente. Senza limitare i diritti sanciti dal copyright, nessuna parte di questo documento può essere riprodotta, memorizzata o inserita in un sistema di ricerca o trasmessa in qualsiasi forma o mezzo (elettronico o meccanico, mediante fotocopia, registrazione o altro), per alcuno scopo, senza l'autorizzazione scritta di Microsoft Corporation.

Microsoft può essere titolare di brevetti, domande di brevetto, marchi, copyright o altri diritti di proprietà intellettuale relativi all'oggetto del presente documento. Salvo quanto espressamente previsto in un contratto scritto di licenza Microsoft, la consegna del presente documento non implica la concessione di alcuna licenza su tali brevetti, marchi, copyright o altra proprietà intellettuale.

Se non specificato diversamente, ogni riferimento a società, organizzazioni, prodotti, nomi di domini, indirizzi di posta elettronica, logo, persone, luoghi ed eventi citati nel presente documento è puramente casuale e ha il solo scopo di illustrare l'uso del prodotto Microsoft.

© 2010 Microsoft Corporation. Tutti i diritti sono riservati.

Microsoft e Windows sono marchi registrati o marchi di Microsoft Corporation negli Stati Uniti e/o in altri paesi.

I nomi di società e prodotti reali citati nel presente documento possono essere marchi dei rispettivi proprietari.
