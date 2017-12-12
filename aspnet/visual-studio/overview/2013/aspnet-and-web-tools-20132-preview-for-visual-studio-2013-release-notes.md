---
uid: visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
title: Web ASP.NET e strumenti 2013.2 delle note sulla versione di Visual Studio 2013 | Documenti Microsoft
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/06/2014
ms.topic: article
ms.assetid: 7ef5f73c-ca60-43c1-bdb2-702800347e7e
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
msc.type: authoredcontent
ms.openlocfilehash: d3a8183fecaf830b2ee1211acd56da86454b4437
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-and-web-tools-20132--for-visual-studio-2013-release-notes"></a>ASP.NET e strumenti Web 2013.2 delle note sulla versione di Visual Studio 2013
====================
da [Microsoft](https://github.com/microsoft)

## <a name="installation-notes"></a>Note sull'installazione

ASP.NET e strumenti Web per Visual Studio 2013.2 sono inclusi nel programma di installazione principale e può essere scaricati come parte di [Visual Studio 2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521).

## <a name="documentation"></a>Documentazione

Sono disponibili esercitazioni e altre informazioni su ASP.NET e strumenti Web per Visual Studio 2013.2 il [sito web ASP.NET](https://www.asp.net/).

## <a name="software-requirements"></a>Requisiti software

ASP.NET e strumenti Web per Visual Studio 2013.2 richiede Visual Studio 2013.

## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-20132"></a>Nuove funzionalità di ASP.NET e Web Tools per Visual Studio 2013.2

Nelle sezioni seguenti vengono descritte le funzionalità che sono state introdotte nella versione.

- [Modelli di progetto ASP.NET](#oneaspnet)
- [Supporta SSL durante l'avvio di applicazioni Web in IIS Express](#ssl)
- [Miglioramenti di Editor di Visual Studio Web](#vswebeditor)
- [Collegamento del browser](#browserlink)
- [Supporto per le app Web di servizio App di Azure in Visual Studio](#waws)
- [Creare risorse di Azure remote quando si crea un nuovo progetto Web](#AzureResources)
- [Miglioramenti di pubblicazione sul Web](#webpublish)
- [Scaffolding di ASP.NET](#scaffolding)
- [NuGet 2.8.1](#nuget)
- [Web Form ASP.NET](#webforms)
- [ASP.NET MVC 5.1.2](#mvc)
- [ASP.NET Web API 2.1.2](#webapi)
- [3.1.2 le pagine Web ASP.NET](#webpages)
- [Entity Framework 6.1](#ef)
- [Identità ASP.NET 2.0.0](#identity)
- [Componenti Microsoft OWIN](#owin)
- [ASP.NET SignalR 2.0.2](#signalr)

<a id="oneaspnet"></a>
### <a name="one-aspnet-project-templates"></a>Modelli di progetto ASP.NET

- Aggiornamenti ai modelli di progetto ASP.NET per supportare la conferma dell'Account e la reimpostazione della Password.
- Aggiornare il modello API Web ASP.NET per supportare l'autenticazione usando in locale gli account aziendali.
- Il modello ASP.NET SPA contiene ora l'autenticazione basata sulle viste lato MVC e server. Il modello dispone di un controller WebAPI accessibili solo dagli utenti autenticati.

<a id="ssl"></a>
### <a name="support-ssl-when-launching-web-applications-on-iis-express"></a>Supporta SSL durante l'avvio di applicazioni Web in IIS Express

Per eliminare l'avviso di sicurezza durante l'esplorazione e debug HTTPS su localhost, è stata aggiunta una finestra di dialogo per consentire di Internet Explorer e Chrome per considerare attendibile autofirmato IIS express certificato SSL.

Ad esempio, è possibile impostare una proprietà del progetto web per utilizzare SSL. Fare clic su F4 per aprire la finestra di dialogo proprietà. Modifica **SSL abilitato** su true. Copiare l'URL SSL.

![Proprietà Enabled di SSL](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image1.png)

Impostare la scheda web project proprietà pagina web per utilizzare il protocollo HTTPS base URL (URL SSL sarà `https://localhost:44300/` a meno che non è già stato creato di siti Web SSL.)

![Impostare l'URL di progetto (HTTPS)](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image2.png)

Premere CTRL+F5 per eseguire l'applicazione. Seguire le istruzioni per considerare attendibile il certificato autofirmato che ha generato IIS Express.

![Avviso SSL](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image3.png)

Lettura di **avviso di sicurezza** finestra di dialogo e quindi fare clic su **Sì** se si desidera installare il certificato che rappresenta localhost.

![Avviso di sicurezza](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image4.png)

Verrà visualizzato il sito in Internet Explorer o Chrome senza avviso inerente il certificato nel browser.

![Pagina HTTPS senza avvisi](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image5.png)

Firefox Usa il proprio archivio certificati, quindi verrà visualizzato un avviso.

<a id="vswebeditor"></a>
### <a name="visual-studio-web-editor-enhancements"></a>Miglioramenti di Editor di Visual Studio Web

- **Nuovo elemento di progetto JSON ed editor**: È stato aggiunto un elemento di progetto JSON e l'editor di Visual Studio. Funzionalità dell'editor JSON corrente includono colorazione, la convalida della sintassi, completamento parentesi graffa, struttura, impostazione dell'opzione di strumenti e altro ancora.

    ![Editor JSON](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image6.png)

    Supporta ora IntelliSense [Schema JSON](http://json-schema.org/) v3 e v4. È una casella combinata di schema per scegliere gli schemi esistenti, modificare il percorso di schema locali o trascinare semplicemente eliminare un file JSON di progetto in modo da ottenere il percorso relativo.

    ![Intellisense JSON](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image7.png)    ![Editor schemi di JSON](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image8.png)
- **Nuovo editor Sass (SCSS)**: È stato aggiunto minore in VS2013 RTM ed è ora disponibile un editor e un elemento di progetto Sass. Gli strumenti dell'editor di sass funzionalità sono confrontabili con l'editor di meno e includono colorazione, la variabile e Mixins IntelliSense, rimuovere il commento/commento, informazioni rapide, formattazione, la convalida della sintassi, struttura, Vai a definizione, alla selezione dei colori, opzione impostazione e così via.

    ![Aggiungi nuovo elemento: Foglio di stile SCSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image9.png)    ![Editor di foglio di stile](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image10.png)
- **Nuova selezione URL in formato HTML, Razor, CSS, minore e documenti Sass:** VS 2013 fornita con nessuna selezione URL di fuori delle pagine Web Form. La nuova selezione URL per HTML, Razor, CSS, LESS e Sass editor è un controllo di selezione digitando fluent, privo di finestra di dialogo che riconosce '... ' e file dei filtri vengono elencati in modo appropriato per i collegamenti e tag img.

    ![Selezione di URL per il tag di immagine](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image11.png)    ![Selezione di URL per le viste](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image12.png)    ![Selezione di URL per CSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image13.png)
- **Aggiornamenti a editor LESS aggiungendo altre funzionalità**
- **Aggiornamento di Intellisense Knockout**: è stata aggiunta una sintassi non standard di ritaglio per intelliSense di Visual Studio, "ko-vs-editor viewModel:" sintassi. E può essere utilizzato da associare alla visualizzazione di più modelli in una pagina utilizzando i commenti nel formato:

    ![Knockout Intellisense](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image14.png)

    È anche necessario aggiunto il supporto per ViewModel IntelliSense nidificati, pertanto può eseguire il drill-oggetti eccessivamente annidati di ViewModel.

    `<div data-bind="text: foo.bar.baz.etc" />`

    Il IntelilSense visualizzato è IntelliSense completa dell'oggetto JavaScript.

    ![Oggetto di JavaScript IntelliSense che mostra completo](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image15.png)
- **Nuova selezione URL in formato HTML, Razor, CSS, minore e documenti Sass**: Visual Studio 2013 fornita con nessuna selezione URL di fuori delle pagine Web Form. La nuova selezione URL per HTML, Razor, CSS, LESS e Sass editor è un controllo di selezione digitando fluent, privo di finestra di dialogo che riconosce '... ' e file dei filtri vengono elencati in modo appropriato per i collegamenti e tag img.

    ![Selezione di URL per il tag di immagine](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image16.png)    ![Selezione di URL per le viste](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image17.png)    ![Selezione di URL per CSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image18.png)

<a id="browserlink"></a>
### <a name="browser-link"></a>Collegamento del browser

- Collegamento del browser ora supporta le connessioni HTTPS ed elencherà che nel Dashboard con le altre connessioni fino a quando il certificato è considerato attendibile dal browser.
- Mapping di origine HTML statico
- SPA supporta per i dati di mapping
- Dati di mapping di aggiornamento automatico

<a id="waws"></a>
### <a name="support-for-azure-app-service-web-apps-in-visual-studio"></a>Supporto per le app Web di servizio App di Azure in Visual Studio

- **Supporto Azure l'accesso.**
- **Vista remota per le applicazioni web e il debug remoto**: È ora supportato [il debug remoto per le app web in Azure App Service](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio) e la visualizzazione dei file di contenuto dell'app web in Esplora server remota.

<a id="AzureResources"></a>
### <a name="create-remote-azure-resources-when-creating-a-new-web-project"></a>Creare risorse di Azure remote quando si crea un nuovo progetto Web

È stato aggiunto un Azure ["Crea risorse Remote"](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet) casella di controllo nella finestra dell'applicazione web nuova. Selezionando questa opzione sarà in grado di integrare l'esperienza di creazione di una nuova applicazione web, configurare il sito di pubblicazione di Azure per test e la creazione del profilo di pubblicazione in pochi semplici passaggi.

![Nuovo progetto con risorse di Azure](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image19.png)![Pubblicazione in Azure](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image20.png)

<a id="webpublish"></a>
### <a name="web-publish-enhancements"></a>Miglioramenti di pubblicazione sul Web

- Migliorare l'esperienza utente per la pubblicazione.

<a id="scaffolding"></a>
### <a name="aspnet-scaffolding"></a>Scaffolding di ASP.NET

- **Supporto di enum:** se il modello utilizza le enumerazioni, quindi il Scaffolder MVC genererà elenco a discesa per l'enumerazione. In MVC utilizza gli helper di enumerazione.
- **Supporto di avvio automatico**: aggiornare i modelli di EditorFor lo Scaffolding di MVC in modo che utilizzino le classi di Bootstrap.
- **Pacchetto di supporto**: MVC e Web API Scaffolders verranno aggiunti i 5.1 pacchetti per MVC e Web API

Le schermate seguenti mostrano i modelli di scaffolding.

- Codice del modello:

     ![Codice del modello](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image21.png)
- Compilare il codice del modello di pulsante destro del mouse e selezionare **Aggiungi**, **nuovo elemento di scaffolding**.

     ![Aggiungi nuovo elemento di scaffolding](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image22.png)
- Scegliere **Controller MVC5 con visualizzazioni, mediante Entity Framework**:

     ![Aggiungi nuova MVC5 controller con visualizzazioni](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image23.png)
- **Aggiungi Controller** utilizzando il modello:

    ![](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image24.png)
- Controllare il codice generato, ad esempio Views/WeekdayModels/Edit.cshtml contiene `@Html.EnumDropDownListFor`: ![visualizzare contenente EnumDropDownListFor](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image25.png)
- Eseguire la pagina per visualizzare la casella combinata enum generata, si noti che, se un valore può essere null, è possibile scegliere una stringa vuota per la casella combinata. Ad esempio, il **crea** pagina Mostra le operazioni seguenti:

    ![Casella combinata, consentendo una stringa vuota](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image26.png)

<a id="nuget"></a>
### <a name="nuget-281"></a>NuGet 2.8.1

NuGet 2.8.1 che verrà rilasciata nel mese di aprile 2014 RTM. Di seguito sono i punti principali dalle note sulla versione, ma, controllare il [note sulla versione completa](http://docs.nuget.org/docs/release-notes/nuget-2.8) per ulteriori informazioni su queste modifiche.

- **Applicazioni di Windows Phone 8.1 come destinazione**: NuGet 2.8.1 ora supporta la destinazione delle applicazioni di Windows Phone 8.1 usando il moniker di framework di destinazione 'WindowsPhoneApp', 'WPA', 'WindowsPhoneApp81' e 'WPA81'.
- **Patch di risoluzione delle dipendenze**: durante la risoluzione di dipendenze dei pacchetti NuGet in passato ha implementato una strategia di selezionando la versione del pacchetto principale e secondario più bassa che soddisfa le dipendenze del pacchetto. A differenza delle versioni principale e secondaria, tuttavia, la versione di patch è stato risolto sempre la versione più recente. Il comportamento è stato malintenzionato, creata una mancanza di determinismo per l'installazione di pacchetti con dipendenze.
- **Commutatore DependencyVersion**: NuGet 2.8 anche se cambia il *predefinito* comportamento per la risoluzione delle dipendenze, aggiunge anche il processo di risoluzione dipendenza tramite l'opzione - DependencyVersion nel controllo più preciso il console di gestione pacchetti. L'opzione consente di risolvere le dipendenze per la versione minima di possibili (comportamento predefinito), la versione più recente, o il minore più alto o la versione di patch. Questa opzione funziona solo per il pacchetto di installazione del comando di powershell.
- **Attributo DependencyVersion**: oltre l'opzione - DependencyVersion sopra descritti, NuGet è inoltre consentito per la possibilità di impostare un nuovo attributo nel file NuGet. config che definisce che cos'è il valore predefinito, se DependencyVersion - opzione non è specificato in una chiamata di pacchetto di installazione. Questo valore verrà rispettato anche dalla finestra di dialogo Gestione pacchetti NuGet per tutte le operazioni di pacchetto di installazione. Per impostare questo valore, aggiungere l'attributo di sotto del file NuGet. config:

    `<config> <add key="dependencyversion" value="Highest" /> </config>`
- **Visualizzare in anteprima le operazioni con NuGet - whatif**: pacchetti NuGet alcune possono avere grafici delle dipendenze complete e di conseguenza, può essere utile durante un'installazione, disinstallare o operazione visualizzare prima di tutto ciò che accadrà di aggiornamento. NuGet 2.8 aggiunge PowerShell standard: che cosa accade se passare i comandi di pacchetto di installazione, disinstallare-package e pacchetto di aggiornamento per abilitare la chiusura di pacchetti a cui applicare il comando intera visualizzazione.
- **Effettuare il downgrade del pacchetto**: non è insolito per installare una versione non definitiva di un pacchetto per analizzare nuove funzionalità e si decide di ripristinare l'ultima versione stabile. Prima di NuGet 2.8, questo è un processo in più passaggi di disinstallare il pacchetto non definitiva e le relative dipendenze e quindi installare la versione precedente. Con NuGet 2.8, tuttavia, il pacchetto di aggiornamento verrà ora eseguito il rollback della chiusura dell'intero pacchetto (ad esempio struttura delle dipendenze del pacchetto) alla versione precedente.
- **Dipendenze di sviluppo**: molti tipi diversi di funzionalità possono essere forniti come pacchetti NuGet - tra cui gli strumenti utilizzati per ottimizzare il processo di sviluppo. Questi componenti, anche se possono essere strumentale lo sviluppo di un nuovo pacchetto, non devono essere considerati una dipendenza del nuovo pacchetto quando più recente pubblicata. NuGet 2.8 consente a un pacchetto per identificarsi con il file. nuspec come un developmentDependency. Durante l'installazione, verranno aggiunto anche i metadati per il file Packages config del progetto in cui è stato installato il pacchetto. Quando tale file Packages config viene analizzato in un secondo momento per le dipendenze di NuGet durante nuget.exe pack, verrà escluso tali interdipendenze contrassegnati come dipendenze di sviluppo.
- **File di singoli file Packages. config per le diverse piattaforme**: quando si sviluppano applicazioni per più piattaforme di destinazione, è comune disporre di file di progetto diversi per ognuno degli ambienti di compilazione corrispondente. È inoltre comune per utilizzare i pacchetti NuGet diversi nei file di progetto diverso, come pacchetti con vari livelli di supporto per le diverse piattaforme. NuGet 2.8 fornisce un supporto migliorato per questo scenario tramite la creazione di file diversi file Packages. config per i file di progetto specifico della piattaforma diverso.
- **Fallback alla Cache locale**: pacchetti NuGet anche se vengono utilizzati in genere da una raccolta remota, ad esempio il [raccolta NuGet](http://www.nuget.org) utilizzando una connessione di rete, esistono molti scenari in cui il client non è connesso. Senza una connessione di rete, il client NuGet non è in grado di installare correttamente i pacchetti, anche quando i pacchetti sono stati già nel computer client nella cache locale NuGet. NuGet 2.8 aggiunge automatica della cache fallback nella console di gestione del pacchetto.

    La funzionalità di fallback della cache non richiedono alcun argomento di comando specifico. Inoltre, cache fallback attualmente funziona solo nella console di gestione di pacchetti: il comportamento non funziona nella finestra di dialogo Gestione pacchetti.
- **Correzioni di bug**: una delle principali correzioni di bug apportate stato miglioramento delle prestazioni nel pacchetto di aggiornamento-comando.

    Oltre a queste funzionalità e risolvere il problema di prestazioni menzionati in precedenza, questa versione di NuGet include anche molte altre correzioni di bug. Si sono verificati problemi di totali 181 risolti nella versione. Per un elenco completo delle operazioni elementi fissa NuGet 2.8,. visualizzazione di [NuGet Issue Tracker](https://nuget.codeplex.com/workitem/list/advanced?release=NuGet%202.8&status=all) per questa versione.

<a id="webforms"></a>
### <a name="aspnet-web-forms"></a>Web Form ASP.NET

- I modelli Web Form ora mostrano come eseguire la conferma dell'Account e la reimpostazione della Password per l'identità ASP.NET.
- Il controllo origine dati di entità e il Provider di dati dinamica per Entity Framework 6. Per ulteriori informazioni, vedere il blog MSDN seguente: [provider di dati dinamici e controllo EntityDataSource per Entity Framework 6](https://blogs.msdn.com/b/webdev/archive/2014/01/30/announcing-preview-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx).

<a id="mvc"></a>
### <a name="aspnet-mvc-512"></a>ASP.NET MVC 5.1.2

- [Attributo di miglioramenti di Routing](../../../mvc/overview/releases/mvc51-release-notes.md#AttributeRouting)
- [Bootstrap supporto per i modelli di editor](../../../mvc/overview/releases/mvc51-release-notes.md#Bootstrap)
- [Supporto di enum nelle viste](../../../mvc/overview/releases/mvc51-release-notes.md#Enum)
- [Supporto Unobstrusive per MinLength / MaxLength attributi](../../../mvc/overview/releases/mvc51-release-notes.md#Unobtrusive)
- [Supporto di contesto 'this' in non intrusivo Ajax](../../../mvc/overview/releases/mvc51-release-notes.md#thisContext)
- Vari [correzioni di bug](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=MVC&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webapi"></a>
### <a name="aspnet-web-api-212"></a>ASP.NET Web API 2.1.2

- [Gestione degli errori globale](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#global-error)
- [Attributo di miglioramenti di routing](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#attribute-routing)
- [Miglioramenti di pagina della Guida](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#help-page)
- [Supporto IgnoreRoute](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#ignoreroute)
- [Formattatore di media type BSON](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#bson)
- [Supporto migliorato per i filtri di async](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#async-filters)
- [Analisi per il client di libreria di formattazione della query](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#query-parsing)
- Vari [correzioni di bug](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20API%7cWeb%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webpages"></a>
### <a name="aspnet-web-pages-312"></a>3.1.2 le pagine Web ASP.NET

- Vari [correzioni di bug](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20Pages/Razor&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="ef"></a>
### <a name="entity-framework-61"></a>Entity Framework 6.1

Entity Framework è stato aggiornato alla versione 6.1 runtime e gli strumenti per. 6.1 Entity Framework (EF) è un aggiornamento secondario di Entity Framework 6 e include un numero di nuove funzionalità e correzioni di bug. Per informazioni dettagliate su EF6.1, inclusi i collegamenti alla documentazione per le nuove funzionalità, vedere [cronologia delle versioni di Entity Framework](https://msdn.microsoft.com/en-US/data/jj574253). Le nuove funzionalità in questa versione includono:

- **Gli strumenti di consolidamento** consente di creare un nuovo modello di Entity Framework in modo coerenza. Questa funzionalità estende la procedura guidata di ADO.NET Entity Data Model per supportare la creazione di modelli Code First, tra cui la decompilazione da un database esistente. Queste funzionalità in precedenza erano disponibili in qualità di Beta negli strumenti Power di Entity Framework.
- **La gestione degli errori di commit delle transazioni** fornisce la nuova [System.Data.Entity.Infrastructure.CommitFailureHandler](https://msdn.microsoft.com/en-us/library/system.data.entity.infrastructure.commitfailurehandler(v=vs.113).aspx) che usano il possibilità appena introdotte per intercettare le operazioni di transazioni. Il **CommitFailureHandler** consente il recupero automatico da errori di connessione durante l'esecuzione del commit di una transazione.
- **IndexAttribute** consente indici specificare inserendo un attributo in una proprietà (o proprietà) nel modello Code First. Codice innanzitutto quindi creare un indice corrispondente nel database.
- **L'API di mapping pubblica** fornisce l'accesso alle informazioni di Entity Framework è in modalità di mapping di proprietà e tipi a colonne e tabelle nel database. Nelle versioni precedenti questa API è interna.
- **Possibilità di configurare gli intercettori tramite il file App/Web.config**(consentire gli intercettori da aggiungere senza ricompilare l'applicazione).
- **DatabaseLogger** è un nuovo intercettore che rende più semplice registrare tutte le operazioni di database in un file. In combinazione con la funzionalità precedente, ciò permette di cambiare facilmente la registrazione di operazioni di database per un'applicazione distribuita, senza dover ricompilare.
- **Il rilevamento delle modifiche del modello migrazioni** è stata migliorata in modo che le migrazioni scaffolding sono più accurate; prestazioni del processo di rilevamento modifiche è stata notevolmente migliorata.
- **Miglioramenti delle prestazioni** incluse operazioni di database ridotto durante l'inizializzazione, le ottimizzazioni per il confronto di uguaglianza null nelle query LINQ, visualizzare generazione (creazione del modello) più velocemente in più scenari e più efficiente materializzazione di entità rilevate con più associazioni.

<a id="identity"></a>
### <a name="aspnet-identity-200"></a>Identità ASP.NET 2.0.0

- **Autenticazione a due fattori**: identità ASP.NET supporta ora l'autenticazione a due fattori. Autenticazione a due fattori fornisce un ulteriore livello di sicurezza per gli account utente nel caso in cui la password viene compromesso. È inoltre disponibile la protezione per gli attacchi di forza bruta contro i codici di autenticazione a due fattori.
- **Il blocco degli account:** fornisce un modo per bloccare l'utente se l'utente immette la password o codici a due fattori in modo non corretto. Il numero di tentativi non validi e l'intervallo di tempo per gli utenti vengono bloccati può essere configurato. Uno sviluppatore può facoltativamente disattivare il blocco degli Account per determinati account utente in caso di.
- **La conferma dell'account:** il sistema di identità di ASP.NET supporta ora la conferma dell'Account. Si tratta di uno scenario abbastanza comune nella maggior parte dei siti Web di oggi, in cui, quando si registra per un nuovo account nel sito Web, viene richiesto di confermare la posta elettronica prima di eseguire qualsiasi operazione nel sito Web. Conferma tramite posta elettronica è utile perché impedisce account fittizio di creazione. Ciò è estremamente utile se si utilizza un messaggio di posta elettronica come un metodo di comunicazione con gli utenti del sito Web, ad esempio siti, servizi bancari, e-commerce o siti web di social networking Forum.
- **La reimpostazione della password:** la reimpostazione della Password è una funzionalità in cui l'utente può reimpostare la password se si hanno dimenticato la password.
- **Indicatore di sicurezza (accesso ovunque):** supporta un modo per rigenerare il Token di sicurezza per l'utente in casi quando l'utente modifica la password o qualsiasi sicurezza di altre informazioni, ad esempio la rimozione di un account di accesso associati (ad esempio Facebook, Google, Account Microsoft e così via). Questo è necessario per garantire che tutti i token generati con la vecchia password vengono invalidati. Nel progetto di esempio, se si modifica la password dell'utente quindi un nuovo token viene generato per l'utente e tutti i token precedenti vengono invalidati. Questa funzionalità fornisce un ulteriore livello di sicurezza per l'applicazione poiché quando si modifica la password, verrà registrato ovunque (tutti gli altri browser) in cui si è connessi questa applicazione.
- **Rendere il tipo di chiave primaria di essere estendibile per utenti e ruoli**: In ASP.NET Identity 1.0, il tipo di chiave primaria della tabella utenti e ruoli è stringhe. Ciò significa che quando il sistema di identità di ASP.NET è stata resa persistente in SQL Server tramite Entity Framework, utilizzato nvarchar. Si sono verificati dibattito attorno a questa implementazione predefinita su Stack Overflow e in base ai commenti in ingresso. Microsoft fornisce un hook di estensibilità in cui è possibile specificare quale deve essere la chiave primaria della tabella di utenti e ruoli. Questo hook di estensibilità è particolarmente utile che se si esegue la migrazione dell'applicazione e l'applicazione è stata l'archiviazione degli ID utente sono GUID o int.
- **Supporta l'oggetto IQueryable su utenti e ruoli**: aggiunta del supporto per IQueryable UsersStore e RolesStore, è possibile ottenere facilmente l'elenco di utenti e ruoli.
- **Supporta l'operazione di eliminazione tramite la classe UserManager**
- **L'indicizzazione in nome utente**: implementazione di ASP.NET Identity Entity Framework, è stato aggiunto il un indice univoco al nome utente utilizzando il nuovo IndexAttribute in EF 6.1.0. Ciò assicura che i nomi utente sono sempre univoci e si è verificato alcun situazione in cui è possibile che con i nomi utente duplicati.
- **Convalida della Password avanzato:** il validator password forniti in ASP.NET Identity 1.0 è stato un validator di password abbastanza semplice che è stato convalida solo la lunghezza minima. È un nuovo validator di password che garantisce un maggiore controllo sulla complessità della password. Si noti che anche se si attiva tutte le impostazioni della password, si consiglia di abilitare l'autenticazione a due fattori per gli account utente.
- **IdentityFactory Middleware / CreatePerOwinContext**:

    - **Gestione utenti**: È possibile utilizzare l'implementazione della Factory per ottenere un'istanza della classe UserManager dal contesto OWIN. Questo modello è simile a ciò che verrà usato per il recupero AuthenticationManager dal contesto OWIN per l'accesso e disconnessione. Si tratta di un metodo consigliato per ottenere un'istanza della classe UserManager per ogni richiesta per l'applicazione.
    - **DbContextFactory**: identità ASP.NET Usa Entity Framework per mantenere il sistema di identità in SQL Server. A tale scopo il sistema di identità è un riferimento di ApplicationDbContext. Il DbContextFactory Middleware restituisce un'istanza di ApplicationDbContext per ogni richiesta che è possibile utilizzare nell'applicazione.
- **Pacchetto NuGet di esempi di identità ASP.NET**: questo pacchetto esempi può rendere più semplice installare e di eseguire gli esempi per ASP.NET Identity e seguire le procedure consigliate. Questo è un esempio di applicazione MVC ASP.NET. Modificare il codice in base all'applicazione prima di distribuire nell'ambiente di produzione. L'esempio deve essere installato in un'applicazione ASP.NET vuota. Per ulteriori informazioni sul pacchetto, vedere il post di blog: [annuncio RTM di ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx)

<a id="owin"></a>
### <a name="microsoft-owin-components"></a>Componenti Microsoft OWIN

Non vi sono molti bug corretti in questa versione. Vedere il [note sulla versione per il 2.1.0 versione](https://katanaproject.codeplex.com/releases/view/113281) per informazioni più dettagliate.

<a id="signalr"></a>
### <a name="aspnet-signalr-202"></a>ASP.NET SignalR 2.0.2

Non vi sono molti bug corretti in questa versione. Vedere il [note sulla versione per il 2.0.2 versione](https://github.com/SignalR/SignalR/releases/tag/2.0.2) per informazioni più dettagliate.
