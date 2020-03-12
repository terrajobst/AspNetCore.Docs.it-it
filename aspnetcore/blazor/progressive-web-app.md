---
title: Creazione di applicazioni Web progressive con ASP.NET Core Blazor webassembly
author: guardrex
description: Informazioni su come creare applicazioni Web progressive basate su Blazor(PWA), app Web che usano le funzionalità moderne del browser per comportarsi come le app desktop.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/09/2020
no-loc:
- Blazor
- SignalR
uid: blazor/progressive-web-app
ms.openlocfilehash: f1c1ce50f20bf579e67e1d4eb02cc7d9d681e354
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083720"
---
# <a name="build-progressive-web-applications-with-aspnet-core-opno-locblazor-webassembly"></a>Creazione di applicazioni Web progressive con ASP.NET Core Blazor webassembly

Di [Steve Sanderson](https://github.com/SteveSandersonMS)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

Un'applicazione Web progressiva (PWA) è un'applicazione basata sul Web che usa le API e le funzionalità moderne del browser per comportarsi come un'applicazione desktop. Queste funzionalità possono includere:

* Lavorare offline e caricare sempre immediatamente, indipendentemente dalla velocità di rete
* Possibilità di esecuzione nella finestra dell'applicazione, non solo in una finestra del browser
* Viene avviato dal menu Start del sistema operativo host, ancorato o dalla schermata iniziale
* Ricezione di notifiche push da un server back-end, anche quando l'utente non usa l'applicazione
* Aggiornamento automatico in background

Un utente potrebbe prima di tutto individuare e usare l'applicazione all'interno del Web browser come qualsiasi altra applicazione a singola pagina (SPA), quindi procedere in seguito per installarla nel sistema operativo e abilitare le notifiche push. Ecco perché usiamo il termine *progressive*.

Blazor webassembly è una vera piattaforma applicativa lato client basata su standard, pertanto può usare qualsiasi API del browser, incluse le API di PWA necessarie per le funzionalità elencate in precedenza. Può funzionare offline esattamente come qualsiasi altra tecnologia Web sul lato client.

## <a name="pwa-template"></a>Modello di PWA

Quando si crea una nuova applicazione Blazor webassembly, viene offerta la possibilità di aggiungere le funzionalità di PWA. In Visual Studio, l'opzione viene specificata come casella di controllo nella finestra di dialogo di creazione del progetto:

![image](https://user-images.githubusercontent.com/1101362/76207411-a6b54200-61f5-11ea-9dfc-6acd87a91428.png)

Se si sta creando il progetto nella riga di comando, è possibile usare il flag `--pwa`. Ad esempio,

```dotnetcli
dotnet new blazorwasm --pwa -o MyNewProject
```

In entrambi i casi, è possibile combinare questa opzione con l'opzione "ASP.NET Core Hosted", se lo si desidera, ma non è necessario. Le funzionalità di PWA sono indipendenti dal modello di hosting.

## <a name="installation-and-app-manifest"></a>Installazione e manifesto dell'applicazione

Quando si visita un'applicazione creata usando l'opzione del modello di PWA, gli utenti hanno la possibilità di installare l'applicazione nel menu Start del sistema operativo, nel Dock o nella schermata iniziale.

Il modo in cui viene presentata questa opzione dipende dal browser dell'utente. Ad esempio, quando si usano browser basati su Chrome Desktop come Edge o Chrome, viene visualizzato un pulsante *Aggiungi* nella barra degli URL:

![image](https://user-images.githubusercontent.com/1101362/76208127-f7796a80-61f6-11ea-8aea-7fba704be787.png)

In iOS i visitatori possono installare il PWA usando il pulsante di *condivisione* di Safari e l'opzione *Aggiungi a homescreen* . In Chrome per Android gli utenti devono toccare il pulsante di *menu* nell'angolo in alto a destra, quindi scegliere *Aggiungi alla schermata iniziale*.

Una volta installato, l'applicazione viene visualizzata nella propria finestra, senza alcuna barra degli indirizzi.

![image](https://user-images.githubusercontent.com/1101362/76208588-e2e9a200-61f7-11ea-85e1-8c3fc849b252.png)

Per personalizzare il titolo della finestra, la combinazione di colori, l'icona o altri dettagli, vedere il file `manifest.json` nella directory *wwwroot* del progetto. Lo schema di questo file è definito dagli standard Web. Per la documentazione dettagliata, vedere https://developer.mozilla.org/docs/Web/Manifest.

## <a name="offline-support"></a>Supporto offline

Per impostazione predefinita, le applicazioni create utilizzando l'opzione del modello PWA dispongono del supporto per l'esecuzione offline. Un utente deve prima visitare l'applicazione mentre è online, il browser scaricherà automaticamente e memorizza nella cache tutte le risorse necessarie per il funzionamento offline.

> [!IMPORTANT]
> Il supporto offline è abilitato solo per le applicazioni *pubblicate* . Non è abilitata durante lo sviluppo. Ciò è dovuto al fatto che interferisce con il normale ciclo di sviluppo di apportare modifiche e testarle.

> [!WARNING]
> Se si intende distribuire un PWA abilitato offline, è necessario comprendere [alcuni avvisi e Avvertenze importanti](#caveats-for-offline-pwas) . Sono intrinseci ai PWA offline e non sono specifici per Blazor. Assicurarsi di leggere e comprendere questi avvertimenti prima di ipotizzare il funzionamento dell'applicazione abilitata per offline.

Per verificare il funzionamento del supporto offline, [pubblicare prima l'applicazione](https://docs.microsoft.com/aspnet/core/host-and-deploy/blazor/?view=aspnetcore-3.1&tabs=visual-studio#publish-the-app)e ospitarla in un server che supporta HTTPS. Quando si visita l'applicazione, dovrebbe essere possibile aprire gli strumenti di sviluppo del browser e verificare che un ruolo di *lavoro del servizio* sia registrato per l'host:

![image](https://user-images.githubusercontent.com/1101362/76210294-bd5e9780-61fb-11ea-9869-65c55c62803d.png)

Inoltre, se si ricarica la pagina, nella scheda *rete* si noterà che tutte le risorse necessarie per caricare la pagina vengono recuperate dal ruolo di lavoro del *servizio* o dalla *cache della memoria*:

![image](https://user-images.githubusercontent.com/1101362/76210472-1d553e00-61fc-11ea-84c6-291644df709e.png)

Ciò indica che il browser non dipende dall'accesso alla rete per caricare l'applicazione. Per verificarlo, è possibile arrestare il server Web o indicare al browser di simulare la modalità offline:

![image](https://user-images.githubusercontent.com/1101362/76210556-47a6fb80-61fc-11ea-9d12-20a8f6528744.png)

A questo punto, anche senza accedere al server Web, dovrebbe essere possibile ricaricare la pagina e verificare che l'applicazione venga caricata ed eseguita. Analogamente, anche se si simula una connessione di rete molto lenta, la pagina verrà caricata immediatamente perché viene caricata indipendentemente dalla rete.

### <a name="service-worker"></a>Ruolo di lavoro del servizio

Il supporto offline viene eseguito tramite un ruolo di lavoro del servizio. Si tratta di uno standard Web, non specifico per Blazor. Per la documentazione relativa ai service worker, vedere https://developer.mozilla.org/docs/Web/API/Service_Worker_API. Per ulteriori informazioni sui modelli di utilizzo comuni per i ruoli di lavoro del servizio, vedere l'articolo eccellente ciclo di vita [del servizio](https://developers.google.com/web/fundamentals/primers/service-workers/lifecycle).

il modello PWA di Blazorproduce due file del ruolo di lavoro del servizio:

* *wwwroot/Service-Worker. js*, usato durante lo sviluppo
* *wwwroot/Service-Worker. Published. js*, che viene usato una volta pubblicata l'applicazione

Se si vuole condividere la logica tra questi due file, è consigliabile aggiungere un terzo file JavaScript per includere la logica comune e usare [`self.importScripts`](https://developer.mozilla.org/docs/Web/API/WorkerGlobalScope/importScripts) per caricare la logica in entrambi i file.

#### <a name="cache-first-fetch-strategy"></a>Strategia di recupero cache-First

Il ruolo di lavoro del servizio *service-worker. Published. js* incorporato risolve le richieste usando una strategia di *cache-First* . Ciò significa che è sempre preferibile restituire il contenuto memorizzato nella cache, se disponibile, indipendentemente dal fatto che l'utente abbia accesso alla rete o che il contenuto più recente sia disponibile nel server.

Questo è utile per due motivi:

* **Garantisce l'affidabilità.** L'accesso alla rete non è uno stato booleano. Un utente non è semplicemente "online" o "offline". In realtà, il dispositivo dell'utente può ritenere che sia online, ma la rete potrebbe essere così lenta da risultare poco pratica da attendere. In alternativa, la rete potrebbe restituire risultati non validi per determinati URL, ad esempio quando è presente un portale Wi-Fi captive che sta attualmente bloccando o reindirizzando determinate richieste. Questo è il motivo per cui l'API `navigator.onLine` del browser non è affidabile e non dipende da.
* **Garantisce la correttezza.** Quando si compila una cache di risorse offline, il ruolo di lavoro del servizio utilizza l'hashing del contenuto per garantire che sia stato recuperato uno snapshot completo e coerente delle risorse in un singolo momento. Questa cache viene quindi utilizzata come unità atomica. Dato questo, non ci sono punti che richiedono la rete per le risorse più recenti, perché le uniche versioni desiderate sono quelle già memorizzate nella cache. Altri rischiano incoerenza e incompatibilità (ad esempio, il tentativo di usare le versioni degli assembly .NET che non sono stati compilati insieme).

#### <a name="background-updates"></a>Aggiornamenti in background

Come modello mentale, è possibile pensare a un'app per dispositivi mobili che può essere installata in modalità offline. Viene sempre avviato immediatamente indipendentemente dalla connettività di rete, ma la logica dell'applicazione installata deriva da uno snapshot temporizzato che potrebbe non essere la versione più recente.

Il modello di Blazor PWA produce applicazioni che tentano automaticamente di aggiornarsi in background ogni volta che l'utente visita e ha una connessione di rete funzionante. Il funzionamento di questo approccio è il seguente:

* Durante la compilazione, il progetto genera un manifesto delle risorse del ruolo di *lavoro del servizio*. Per impostazione predefinita, questo nome è denominato *service-worker-assets. js*. Vengono elencate tutte le risorse statiche di cui l'applicazione deve eseguire il funzionamento offline, ad esempio assembly .NET, file JavaScript, CSS e così via, inclusi gli hash del contenuto. Questo elenco viene caricato dal ruolo di lavoro del servizio in modo da sapere quali risorse memorizzare nella cache.
* Ogni volta che l'utente visita l'applicazione, il browser richiede nuovamente *service-worker. js* e *service-worker-assets. js* in background. Se il server restituisce il contenuto modificato per uno di questi file (a confronto byte per byte con il ruolo di lavoro del servizio installato esistente), il ruolo di lavoro del servizio tenterà di installare una nuova versione di se stessa.
* Quando si installa una nuova versione di se stessa, il ruolo di lavoro del servizio crea una nuova cache separata per le risorse offline e inizia a popolarla con le risorse elencate in *service-worker-assets. js*. Questa logica viene implementata nella funzione `onInstall` all'interno di *service-worker. Published. js*.
* Se il processo viene completato correttamente (ad esempio, tutte le risorse vengono caricate senza errori e tutti gli hash del contenuto corrispondono), il nuovo thread di lavoro entra in uno stato "in attesa di attivazione". Non appena l'utente chiude l'applicazione (ad esempio, non sono presenti schede o finestre rimanenti che visualizzano l'applicazione), il nuovo servizio di lavoro diventa "attivo" e verrà usato per le visite successive all'applicazione. Il servizio di lavoro precedente e la relativa cache vengono eliminati.
* Se il processo non viene completato correttamente, la nuova istanza del ruolo di lavoro del servizio viene eliminata. Il processo di aggiornamento verrà nuovamente ritentato alla visita successiva dell'utente, quando si spera che dispongano di una connessione di rete migliore in grado di completare le richieste.

È possibile personalizzare qualsiasi aspetto di questo processo modificando la logica del ruolo di lavoro del servizio. Nessuno dei precedenti è specifico per Blazor, ma è semplicemente un suggerimento fornito dall'opzione del modello PWA. Per ulteriori informazioni, vedere la [documentazione del servizio Worker](https://developer.mozilla.org/docs/Web/API/Service_Worker_API.) .

#### <a name="how-requests-are-resolved"></a>Modalità di risoluzione delle richieste

Come descritto in precedenza, il ruolo di lavoro predefinito del servizio utilizza una strategia di *cache-First* , ovvero tenta di gestire il contenuto memorizzato nella cache, se disponibile. Se non sono presenti contenuti memorizzati nella cache per un determinato URL, ad esempio quando si richiedono dati da un'API back-end, il ruolo di lavoro del servizio esegue il fallback su una normale richiesta di rete, che può avere esito positivo solo se il server è raggiungibile. Questa logica viene implementata all'interno `onFetch` all'interno di *service-worker. Published. js*.

Se i componenti Blazor si basano sulla richiesta di dati dalle API back-end e si desidera fornire un'esperienza utente intuitiva nel caso in cui tali richieste abbiano esito negativo a causa della mancata disponibilità della rete, è necessario implementare la logica all'interno dei componenti. Ad esempio, usare `try/catch` circa `HttpClient` richieste.

#### <a name="support-server-rendered-pages"></a>Pagine di supporto per il rendering del server

Si consideri cosa accade quando l'utente accede per la prima volta a un URL, ad esempio `/counter` o qualsiasi altro collegamento profondo nell'applicazione. In questi casi, non si vuole restituire contenuto memorizzato nella cache come `/counter`, ma è necessario che il browser carichi il contenuto memorizzato nella cache come `/index.html` per avviare l'applicazione di Blazor webassembly. Queste richieste iniziali sono note come richieste di *navigazione* (in contrapposizione alle richieste di *sottorisorse* per immagini/CSS e così via o richieste di *recupero/XHR* per i dati dell'API).

Il ruolo di lavoro del servizio predefinito contiene la logica speciale per le richieste di navigazione. Vengono risolti restituendo il contenuto memorizzato nella cache per `/index.html`, indipendentemente dall'URL richiesto. Questa logica viene implementata nella funzione `onFetch` all'interno di *service-worker. Published. js*.

Se l'applicazione dispone di determinati URL che devono restituire il codice HTML sottoposto a rendering del server (senza `/index.html` dalla cache), è necessario modificare la logica nel ruolo di lavoro del servizio. Se, ad esempio, tutti gli URL contenenti `/Identity/` devono essere gestiti come richieste normali solo in linea al server, modificare la logica di `onFetch` *service-worker. Published. js* . Individuare il codice riportato di seguito:

```javascript
const shouldServeIndexHtml = event.request.mode === 'navigate';
```

Modificare il codice nel modo seguente:

```javascript
const shouldServeIndexHtml = event.request.mode === 'navigate'
    && !event.request.url.includes('/Identity/');
```

Se non si esegue questa operazione, indipendentemente dalla connettività di rete, il ruolo di lavoro del servizio intercetta le richieste per tali URL e le risolve utilizzando `/index.html`.

#### <a name="control-asset-caching"></a>Controllare la memorizzazione nella cache degli asset

Se il progetto definisce una proprietà MSBuild denominata `ServiceWorkerAssetsManifest`, gli strumenti di compilazione di Blazorgenereranno un manifesto delle risorse del ruolo di lavoro del servizio con il nome specificato. Il modello predefinito di PWA produce un file di progetto contenente gli elementi seguenti:

```xml
<ServiceWorkerAssetsManifest>service-worker-assets.js</ServiceWorkerAssetsManifest>
```

Il file si trova nella directory di output *wwwroot* , quindi il browser può recuperare il file richiedendo `/service-worker-assets.js`. Per visualizzare il contenuto, aprire *YourProject\bin\Debug\netstandard2.1\wwwroot\service-Worker-assets.js* in un editor di testo. Tuttavia, non modificare il file, perché verrà rigenerato in ogni compilazione.

Per impostazione predefinita, questo manifesto elenca:

* Tutte le risorse gestite da Blazor, ad esempio gli assembly .NET e i file di runtime di .NET webassembly, necessari per il funzionamento offline
* Tutte le risorse che verranno pubblicate nella directory *wwwroot* , ad esempio immagini, file CSS e file JavaScript. Sono inclusi gli asset Web statici forniti da progetti esterni e pacchetti NuGet.

È possibile controllare quali di queste risorse verranno recuperate e memorizzate nella cache dal ruolo di lavoro del servizio modificando la logica in `onInstall` in *service-worker. Published. js*. Per impostazione predefinita, i file vengono recuperati e memorizzati nella cache corrispondenti ad estensioni di file Web tipiche, ad esempio *HTML*, *CSS*, *JS*, *WASM*e altri, oltre ai tipi di file specifici per Blazor webassembly (con estensione*dll*, *PDB*).

Se si desidera includere risorse aggiuntive che non sono presenti nella directory *wwwroot* , è possibile eseguire questa operazione definendo voci ItemGroup MSBuild aggiuntive. Nel file di progetto, ad esempio, aggiungere:

```xml
<ItemGroup>
    <ServiceWorkerAssetsManifestItem
        Include="MyDirectory\AnotherFile.json"
        RelativePath="MyDirectory\AnotherFile.json"
        AssetUrl="files/AnotherFile.json" />
</ItemGroup>
```

I metadati di `AssetUrl` specificano l'URL relativo di base che il browser deve usare durante il recupero della risorsa nella cache. Questo può essere indipendente dal nome del file di origine originale sul disco.

> [!IMPORTANT]
> L'aggiunta di un `ServiceWorkerAssetsManifestItem` non determina la pubblicazione del file nella directory *wwwroot* Consente di controllare l'output di pubblicazione separatamente. Il `ServiceWorkerAssetsManifestItem` causa solo la visualizzazione di una voce aggiuntiva nel manifesto delle risorse del ruolo di lavoro del servizio.

## <a name="push-notifications"></a>Notifiche push

Analogamente a qualsiasi altro PWA, un Blazor webassembly PWA può ricevere notifiche push da un server back-end. Il server può inviare questi elementi in qualsiasi momento, anche quando l'utente non usa attivamente l'applicazione (ad esempio, quando un altro utente esegue un'azione che può essere pertinente).

Il meccanismo per l'invio di una notifica push è interamente indipendente da Blazor webassembly, dal momento che è implementato dal server back-end che può usare qualsiasi tecnologia. Se si desidera inviare notifiche push da un server di ASP.NET Core, è consigliabile [utilizzare una tecnica simile a quella del workshop di pizza ardente](https://github.com/dotnet-presentations/blazor-workshop/blob/master/docs/09-progressive-web-app.md#sending-push-notifications).

Il meccanismo per la ricezione e la visualizzazione di una notifica push sul client è anche indipendente da Blazor webassembly, dal momento che è implementato nel servizio Worker, che è un file JavaScript. Ad esempio, è possibile vedere di nuovo [l'approccio usato nell'officina della pizza ardente](https://github.com/dotnet-presentations/blazor-workshop/blob/master/docs/09-progressive-web-app.md#displaying-notifications).

## <a name="caveats-for-offline-pwas"></a>Avvertenze per PWA offline

Non tutte le applicazioni devono provare a supportare l'uso offline. Viene aggiunta una complessità significativa, sebbene non sempre pertinente.

Il supporto offline è in genere rilevante solo:

* Se l'archivio dati primario è locale nel browser. Ad esempio, quando si compila un'interfaccia utente per [un dispositivo Internet](https://en.wikipedia.org/wiki/Internet_of_things) delle cose che archivia i dati in `localStorage` o [IndexedDB](https://developer.mozilla.org/docs/Web/API/IndexedDB_API).

* Se si esegue un lavoro significativo per recuperare e memorizzare nella cache i dati dell'API back-end rilevanti per ogni utente, in modo da poterli esplorare offline. Se si supporta la modifica, sarà anche necessario compilare un sistema per tenere traccia delle modifiche e sincronizzarle con il back-end.

* Se l'obiettivo è garantire il caricamento immediato dell'applicazione indipendentemente dalle condizioni della rete. Sarà quindi necessario implementare un'esperienza utente adatta alle richieste dell'API back-end per mostrare lo stato di avanzamento delle richieste e comportarsi in modo normale quando hanno esito negativo a causa della mancata disponibilità della rete.

Inoltre, le PWA con supporto offline devono gestire una serie di complicazioni aggiuntive. Gli sviluppatori devono acquisire familiarità con attenzione con le avvertenze seguenti.

### <a name="offline-support-only-when-published"></a>Supporto offline solo dopo la pubblicazione

il modello PWA di Blazorconsente il supporto offline solo dopo la pubblicazione. Il motivo è che, durante lo sviluppo, in genere si desidera visualizzare ogni modifica riflessa immediatamente nel browser, senza passare attraverso un processo di aggiornamento in background.

Pertanto, quando si compila un'applicazione che supporta offline, non è sufficiente testare l'applicazione in modalità di sviluppo. È necessario testare l'applicazione nello stato pubblicato per comprendere come risponderà a condizioni di rete diverse.

### <a name="update-completion-after-user-navigation-away-from-app"></a>Completamento dell'aggiornamento dopo la navigazione dell'utente dall'app

Gli aggiornamenti non vengono completati fino a quando l'utente non si è spostato dall'applicazione in tutte le schede. Come illustrato negli [aggiornamenti in background](#background-updates), dopo la distribuzione di un aggiornamento nell'applicazione, il browser recupererà i file di lavoro del servizio aggiornati e avvierà un processo di aggiornamento.

Quali sono le sorprese che molti sviluppatori, anche al termine dell'aggiornamento, diventano effettive fino a quando l'utente **non** si è spostato in tutte le schede. **Non** è sufficiente aggiornare la scheda visualizzando l'applicazione, anche se è l'unica scheda che Visualizza l'applicazione. Finché l'applicazione non viene chiusa completamente, il nuovo ruolo di lavoro del servizio rimarrà in stato "in attesa di attivazione". **Questo non è specifico per Blazor, ma è piuttosto un comportamento standard della piattaforma Web.**

Questo problema si verifica in genere dagli sviluppatori che tentano di testare gli aggiornamenti al ruolo di lavoro del servizio o alle risorse nella cache offline. Se si archiviano gli strumenti di sviluppo del browser, è possibile che venga visualizzato un risultato simile al seguente:

![image](https://user-images.githubusercontent.com/1101362/76226394-b93f7380-6215-11ea-8572-7d52afee2dd8.png)

Per il periodo di tempo in cui l'elenco dei "client" (ovvero le schede o le finestre che visualizzano l'applicazione) non è vuoto, il thread di lavoro continuerà ad attendere. Il motivo per cui i dipendenti del servizio eseguono questa operazione è garantire la coerenza, ovvero, che tutte le risorse vengono recuperate dalla stessa cache atomica.

Quando si verificano le modifiche, può essere utile fare clic sul collegamento "skipWaiting", come illustrato nella schermata precedente, quindi ricaricare la pagina. Se lo si desidera, è possibile automatizzare questa operazione per tutti gli utenti codificando il ruolo di lavoro del servizio in modo da [ignorare la fase di attesa e attivare immediatamente l'aggiornamento](https://developers.google.com/web/fundamentals/primers/service-workers/lifecycle#skip_the_waiting_phase). Tuttavia, se si esegue questa operazione, si cede la garanzia che le risorse vengano sempre recuperate in modo coerente dalla stessa istanza della cache.

### <a name="users-may-run-any-historical-version-of-the-app"></a>Gli utenti possono eseguire qualsiasi versione cronologica dell'app

Gli sviluppatori Web si aspettano abitualmente che gli utenti eseguano solo la versione distribuita più recente della propria applicazione Web, dal momento che si tratta di una situazione normale nel modello di distribuzione Web tradizionale. Tuttavia, un PWA per la prima volta offline è più simile a un'app per dispositivi mobili nativa, in cui gli utenti non eseguono necessariamente la versione più recente.

Come spiegato negli [aggiornamenti in background](#background-updates), dopo la distribuzione di un aggiornamento nell'applicazione, **ogni utente esistente continuerà a usare una versione precedente per almeno un'altra visita** (poiché l'aggiornamento viene eseguito in background e non viene attivato fino a quando l'utente non si sposta). Inoltre, la versione precedente utilizzata non è necessariamente quella precedente distribuita. può trattarsi di *qualsiasi* versione cronologica, a seconda del momento in cui l'utente ha completato un aggiornamento.

Questo può essere un problema se le parti front-end e back-end dell'applicazione richiedono un accordo sullo schema per le richieste API. Non è necessario distribuire le modifiche allo schema API non compatibili con le versioni precedenti fino a quando non si è certi che tutti gli utenti abbiano eseguito l'aggiornamento o che almeno gli utenti non possano usare versioni precedenti dell'app incompatibili. Questo è proprio come un'app per dispositivi mobili nativa. Se si distribuisce una modifica di rilievo nelle API del server, l'app client verrà interrotta per gli utenti che non hanno ancora eseguito l'aggiornamento.

Se possibile, non distribuire le modifiche di rilievo alle API back-end. Tuttavia, se è necessario eseguire questa operazione, è consigliabile utilizzare le [API di lavoro del servizio standard, ad esempio `ServiceWorkerRegistration`](https://developer.mozilla.org/docs/Web/API/ServiceWorkerRegistration) per determinare se l'applicazione è aggiornata e, in caso contrario, per impedire l'utilizzo.

### <a name="interference-with-server-rendered-pages"></a>Interferenze con le pagine sottoposte a rendering server

[Come descritto in precedenza](#support-server-rendered-pages), se si desidera ignorare il comportamento del ruolo di lavoro del servizio per restituire `/index.html` contenuto per tutte le richieste di navigazione, è necessario modificare la logica nel ruolo di lavoro del servizio.

### <a name="all-service-worker-asset-manifest-contents-are-cached-by-default"></a>Tutti i contenuti del manifesto dell'asset del servizio di lavoro vengono memorizzati nella cache

[Come descritto in precedenza](#control-asset-caching), il file *service-worker-assets. js* viene generato durante la compilazione ed elenca tutti gli asset che devono essere recuperati e memorizzati nella cache dal ruolo di lavoro del servizio.

Poiché per impostazione predefinita questo elenco include tutti gli elementi emessi in *wwwroot* (incluso il contenuto fornito da pacchetti e progetti esterni), è necessario prestare attenzione a non inserire troppi contenuti. Se ad esempio la directory *wwwroot* contiene milioni di immagini, il ruolo di lavoro del servizio tenterà di recuperarli e memorizzarli nella cache, consumando una larghezza di banda eccessiva e probabilmente non verrà completata correttamente.

È possibile implementare una logica arbitraria per controllare quale subset del contenuto del manifesto deve essere recuperato e memorizzato nella cache modificando la funzione `onInstall` in *service-worker. Published. js*.

### <a name="interaction-with-authentication"></a>Interazione con l'autenticazione

È possibile utilizzare l'opzione del modello PWA insieme alle opzioni di autenticazione. Un PWA con supporto offline può inoltre supportare l'autenticazione quando l'utente dispone di connettività di rete.

Tuttavia, quando un utente non dispone di connettività di rete, non sarà in grado di autenticare o ottenere i token di accesso. Se si tenta di visitare la pagina "login", per impostazione predefinita viene visualizzato un messaggio che indica "errore di rete".

Di conseguenza, è il processo di progettazione di un flusso dell'interfaccia utente che consente all'utente di eseguire operazioni utili in modalità offline senza provare ad autenticare o ottenere i token di accesso o almeno in caso di errore in modo normale. Se l'applicazione non è possibile, è possibile che non si desideri abilitare il supporto offline.
