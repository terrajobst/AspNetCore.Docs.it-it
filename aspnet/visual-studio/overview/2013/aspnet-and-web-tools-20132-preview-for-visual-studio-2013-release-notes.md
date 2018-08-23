---
uid: visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
title: ASP.NET e Web Tools 2013.2 per Visual Studio 2013 Release Notes | Microsoft Docs
author: microsoft
description: ''
ms.author: riande
ms.date: 03/06/2014
ms.assetid: 7ef5f73c-ca60-43c1-bdb2-702800347e7e
msc.legacyurl: /visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
msc.type: authoredcontent
ms.openlocfilehash: 2a22c5b686cb8e02054f421f78a8fc910af7ce28
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833381"
---
<a name="aspnet-and-web-tools-20132--for-visual-studio-2013-release-notes"></a>ASP.NET and Web Tools 2013.2 per Visual Studio 2013 Release Notes
====================
da [Microsoft](https://github.com/microsoft)

## <a name="installation-notes"></a>Note sull'installazione

ASP.NET and Web Tools per Visual Studio 2013.2 sono inclusi nel programma di installazione principale e possono essere scaricati come parte della [Visual Studio 2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521).

## <a name="documentation"></a>Documentazione

Esercitazioni e altre informazioni su ASP.NET and Web Tools per Visual Studio 2013.2 sono disponibili i [sito web ASP.NET](https://www.asp.net/).

## <a name="software-requirements"></a>Requisiti software

ASP.NET and Web Tools per Visual Studio 2013.2 richiede Visual Studio 2013.

## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-20132"></a>Nuove funzionalità di ASP.NET and Web Tools per Visual Studio 2013.2

Le sezioni seguenti descrivono le funzionalità che sono state introdotte nella versione.

- [Modelli di progetto ASP.NET](#oneaspnet)
- [Supporta SSL durante l'avvio di applicazioni Web in IIS Express](#ssl)
- [Miglioramenti dell'Editor Web di Visual Studio](#vswebeditor)
- [Browser Link](#browserlink)
- [Supporto per le app Web di servizio App di Azure in Visual Studio](#waws)
- [Creare le risorse di Azure remote quando si crea un nuovo progetto Web](#AzureResources)
- [Miglioramenti di pubblicazione sul Web](#webpublish)
- [Scaffolding di ASP.NET](#scaffolding)
- [NuGet 2.8.1](#nuget)
- [Web Form ASP.NET](#webforms)
- [ASP.NET MVC 5.1.2](#mvc)
- [2.1.2 l'API Web ASP.NET](#webapi)
- [3.1.2 delle pagine Web ASP.NET](#webpages)
- [Entity Framework 6.1](#ef)
- [ASP.NET Identity 2.0.0](#identity)
- [Componenti Microsoft OWIN](#owin)
- [ASP.NET SignalR 2.0.2](#signalr)

<a id="oneaspnet"></a>
### <a name="one-aspnet-project-templates"></a>Modelli di progetto ASP.NET

- Aggiornamenti ai modelli di progetto ASP.NET per supportare la conferma dell'Account e la reimpostazione della Password.
- Aggiornare il modello API Web ASP.NET per supportare l'autenticazione usando in locale gli account aziendali.
- Il modello di applicazione a singola pagina ASP.NET contiene ora l'autenticazione basata su MVC e il server le viste lato. Il modello ha un controller API Web che sono accessibili solo dagli utenti autenticati.

<a id="ssl"></a>
### <a name="support-ssl-when-launching-web-applications-on-iis-express"></a>Supporta SSL durante l'avvio di applicazioni Web in IIS Express

Per eliminare l'avviso di sicurezza durante l'esplorazione e il debug HTTPS su localhost, è stata aggiunta una finestra di dialogo per consentire di Internet Explorer e Chrome per considerare attendibile autofirmato IIS express certificato SSL.

Ad esempio, è possibile impostare una proprietà del progetto web per usare SSL. Premere F4 per visualizzare la finestra di dialogo delle proprietà. Change **SSL abilitato** su true. Copiare l'URL SSL.

![La proprietà abilitata per SSL](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image1.png)

Impostare la scheda web progetto proprietà pagina web per l'uso di HTTPS base URL (URL SSL sarà `https://localhost:44300/` a meno che non è stato creato in precedenza siti Web SSL.)

![Impostare l'URL di progetto (HTTPS)](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image2.png)

Premere CTRL+F5 per eseguire l'applicazione. Seguire le istruzioni per considerare attendibile il certificato autofirmato IIS Express è generato.

![Avviso SSL](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image3.png)

Leggere il **avviso di sicurezza** finestra di dialogo e quindi fare clic su **Yes** se si desidera installare il certificato che rappresenta localhost.

![Avviso di sicurezza](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image4.png)

Il sito verrà visualizzato in Internet Explorer o Chrome senza avviso inerente il certificato nel browser.

![Pagina HTTPS senza avvisi](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image5.png)

Firefox Usa un proprio archivio certificati, in modo che verrà visualizzato un avviso.

<a id="vswebeditor"></a>
### <a name="visual-studio-web-editor-enhancements"></a>Miglioramenti dell'Editor Web di Visual Studio

- **Nuovo elemento di progetto JSON ed editor**: È stato aggiunto un elemento del progetto JSON ed editor di Visual Studio. Funzionalità dell'editor JSON corrente includono colorazione, la convalida della sintassi, completamento parentesi graffa, struttura, impostazione dell'opzione strumenti e altro ancora.

    ![Editor JSON](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image6.png)

    Supporta ora IntelliSense [dello Schema JSON](http://json-schema.org/) v3 e v4. È una casella combinata di schema per scegliere gli schemi esistenti, modificare il percorso locale dello schema, o semplicemente di trascinamento della selezione un file JSON di progetto per ottenere il percorso relativo.

    ![Intellisense JSON](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image7.png)    ![Editor schemi JSON](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image8.png)
- **Nuovo editor Sass (SCSS)**: abbiamo aggiunto il minore in VS2013 RTM ed è ora disponibile un elemento del progetto Sass ed editor. Gli strumenti dell'editor di sass funzionalità sono confrontabili con l'editor LESS e includere la colorazione, la variabile e Mixins IntelliSense, aggiungere/rimuovere il commento, informazioni rapide, formattazione, convalida della sintassi, la struttura, Vai a definizione, selezione colori, impostazione di opzione e così via.

    ![Aggiungi nuovo elemento: Foglio di stile SCSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image9.png)    ![Editor di foglio di stile](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image10.png)
- **Nuova selezione URL in formato HTML, Razor, CSS, LESS e Sass documenti:** VS 2013 forniti con nessuna selezione URL di fuori di pagine Web Form. La nuova selezione di URL per HTML, Razor, CSS, LESS e Sass editor è un controllo di selezione tipizzazione privi di finestra di dialogo, fluent che comprende '.. ' e file dei filtri sono elencati in modo appropriato per i collegamenti e i tag img.

    ![Selezione di URL per il tag di immagine](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image11.png)    ![Selezione di URL per le visualizzazioni](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image12.png)    ![Selezione di URL per CSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image13.png)
- **Aggiornamenti a editor LESS mediante l'aggiunta di altre funzionalità**
- **Aggiornamento di Intellisense Knockout**: È stata aggiunta una sintassi KnockOut non standard per intelliSense di Visual Studio, "viewModel ko-vs-editor:" sintassi. Può essere utilizzato da associare alla visualizzazione di più modelli in una pagina con i commenti nel formato:

    ![Intellisense Knockout](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image14.png)

    È anche aggiunto il supporto per ViewModel IntelliSense annidato, in modo che possono eseguire il drill-in oggetti eccessivamente annidati nel ViewModel.

    `<div data-bind="text: foo.bar.baz.etc" />`

    Il IntelilSense visualizzato è il supporto IntelliSense completo dell'oggetto JavaScript.

    ![Oggetto JavaScript completo di IntelliSense che mostra](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image15.png)
- **Nuova selezione URL in formato HTML, Razor, CSS, LESS e Sass documenti**: Visual Studio 2013 forniti con nessuna selezione URL di fuori di pagine Web Form. La nuova selezione di URL per HTML, Razor, CSS, LESS e Sass editor è un controllo di selezione tipizzazione privi di finestra di dialogo, fluent che comprende '.. ' e file dei filtri sono elencati in modo appropriato per i collegamenti e i tag img.

    ![Selezione di URL per il tag di immagine](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image16.png)    ![Selezione di URL per le visualizzazioni](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image17.png)    ![Selezione di URL per CSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image18.png)

<a id="browserlink"></a>
### <a name="browser-link"></a>Collegamento del browser

- Collegamento del browser ora supporta le connessioni HTTPS ed elencherà che nel Dashboard con altre connessioni, purché il certificato è considerato attendibile dal browser.
- Mapping di origine HTML statico
- SPA supporta per i dati di mapping
- Dati di mapping di aggiornamento automatico

<a id="waws"></a>
### <a name="support-for-azure-app-service-web-apps-in-visual-studio"></a>Supporto per le app Web di servizio App di Azure in Visual Studio

- **Supporto tecnico Azure l'accesso.**
- **Debug remoto e visualizzazione remota per le app web**: È ora supportata [debug remoto per le app web nel servizio App di Azure](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio) e visualizzazione remota di file di contenuto app web in Esplora server.

<a id="AzureResources"></a>
### <a name="create-remote-azure-resources-when-creating-a-new-web-project"></a>Creare le risorse di Azure remote quando si crea un nuovo progetto Web

È stata aggiunta un'istanza di Azure ["Crea risorse Remote"](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet) casella di controllo la nuova finestra di dialogo dell'applicazione web. Selezionando questa opzione sarà in grado di integrare l'esperienza di creazione di una nuova applicazione web, configurare il sito di pubblicazione di Azure per test e la creazione di profilo di pubblicazione in pochi semplici passaggi.

![Nuovo progetto con risorse di Azure](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image19.png)![Pubblicazione in Azure](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image20.png)

<a id="webpublish"></a>
### <a name="web-publish-enhancements"></a>Miglioramenti di pubblicazione sul Web

- Migliorare l'esperienza utente per la pubblicazione.

<a id="scaffolding"></a>
### <a name="aspnet-scaffolding"></a>Scaffolding di ASP.NET

- **Supporto di enumerazione:** se il modello utilizza le enumerazioni, quindi l'utilità di scaffolding di MVC genera elenco a discesa per l'enumerazione. Usa gli helper di enumerazione in MVC.
- **Eseguire il bootstrap supporto**: aggiornati i modelli di EditorFor lo Scaffolding di MVC in modo che usino le classi di Bootstrap.
- **Supporto del pacchetto**: gli API Web e MVC verranno aggiunti i 5.1 pacchetti per MVC e API Web

Gli screenshot seguenti illustrano i modelli di scaffolding.

- Codice del modello:

     ![Codice del modello](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image21.png)
- Compilare il codice del modello, pulsante destro del mouse e scegliere **Add**, **nuovo elemento di scaffolding**.

     ![Aggiungi nuovo elemento di scaffolding](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image22.png)
- Scegli **Controller MVC 5 con visualizzazioni, mediante Entity Framework**:

     ![Aggiungere nuovi controller MVC 5 con visualizzazioni](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image23.png)
- **Aggiungere Controller** usando il modello:

    ![](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image24.png)
- Controllare il codice generato, ad esempio contiene Views/WeekdayModels/Edit.cshtml `@Html.EnumDropDownListFor`: ![vista contenente EnumDropDownListFor](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image25.png)
- Eseguire la pagina per visualizzare la casella combinata enum generata, si noti che se un valore può essere null, una stringa vuota può essere scelta per la casella combinata. Ad esempio, il **Create** pagina mostra quanto segue:

    ![Casella combinata che consente una stringa vuota](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image26.png)

<a id="nuget"></a>
### <a name="nuget-281"></a>NuGet 2.8.1

NuGet 2.8.1 che RTM verrà rilasciata ad aprile 2014. Ecco i punti principali dalle note sulla versione, ma verificare i [note sulla versione complete](http://docs.nuget.org/docs/release-notes/nuget-2.8) per altre informazioni su queste modifiche.

- **Le applicazioni di Windows Phone 8.1 come destinazione**: NuGet 2.8.1 ora supporta la destinazione delle applicazioni di Windows Phone 8.1 usando il moniker del framework di destinazione 'WindowsPhoneApp', 'WPA', 'WindowsPhoneApp81' e 'WPA81'.
- **Risoluzione delle patch per le dipendenze**: durante la risoluzione delle dipendenze dei pacchetti, NuGet in passato ha implementato una strategia di selezionando la versione principale e secondaria del pacchetto più bassa che soddisfa le dipendenze del pacchetto. A differenza delle versioni principale e secondaria, tuttavia, la versione di patch è stato risolto sempre per la versione più recente. Il comportamento è stato malintenzionato, creata una mancanza di determinismo per l'installazione di pacchetti con dipendenze.
- **Opzione DependencyVersion**: viene modificato anche se NuGet 2.8 il *predefinito* comportamento per la risoluzione delle dipendenze, aggiunge anche maggiore controllo sul processo di risoluzione delle dipendenze mediante l'opzione DependencyVersion - in di console di gestione pacchetti. L'opzione Abilita la risoluzione delle dipendenze per la versione più bassa possibile (comportamento predefinito), la versione più recente, o il minore più alto o versione patch. Questa opzione funziona solo per install-package nel comando di powershell.
- **Attributo DependencyVersion**: oltre all'opzione DependencyVersion - come indicato in precedenza, NuGet ha consentito anche la possibilità di impostare un nuovo attributo nel file NuGet. config che definisce ciò che è il valore predefinito, se l'opzione DependencyVersion - non è specificato in una chiamata di install-package. Questo valore verrà rispettato anche dalla finestra di dialogo Gestione pacchetti NuGet per qualsiasi operazione di installazione del pacchetto. Per impostare questo valore, aggiungere l'attributo seguente nel file NuGet. config:

    `<config> <add key="dependencyversion" value="Highest" /> </config>`
- **Visualizzare in anteprima le operazioni con NuGet - whatif**: i pacchetti NuGet Some possono avere grafici di dipendenza completa e di conseguenza, possono risultare utili durante un'installazione, disinstallare o aggiornare innanzitutto vedere cosa succede dell'operazione. NuGet 2.8 aggiunge il comando di PowerShell standard: cosa accade se passare ai comandi per abilitare visualizzazione l'intera closure di pacchetti a cui verrà applicato il comando update-package, disinstallare-package e install-package.
- **Effettuare il downgrade del pacchetto**: non è insolito per installare una versione preliminare di un pacchetto e per studiare le nuove funzionalità e quindi decidere se eseguire il rollback all'ultima versione stabile. Prima di NuGet 2.8, questo era un processo in più passaggi di disinstallazione il pacchetto versione non definitivo e le relative dipendenze e quindi installare la versione precedente. Con NuGet 2.8, tuttavia, il pacchetto di aggiornamento verrà ora eseguito il rollback alla chiusura dell'intero pacchetto (ad esempio, l'albero delle dipendenze del pacchetto) alla versione precedente.
- **Le dipendenze dello sviluppo**: molti tipi diversi di funzionalità possono essere distribuiti come pacchetti NuGet - tra cui strumenti utilizzati per ottimizzare il processo di sviluppo. Questi componenti, anche se possono essere strumentale nello sviluppo di un nuovo pacchetto, non devono essere considerati una dipendenza del pacchetto di nuovo quando più recente pubblicata. NuGet 2.8 consente a un pacchetto per identificarsi nel file con estensione nuspec come un developmentDependency. Durante l'installazione, questa verranno anche aggiunti i metadati del file Packages. config del progetto in cui è stato installato il pacchetto. Quando tale file Packages. config viene analizzato in un secondo momento le dipendenze NuGet durante nuget.exe pack, vengono escluse le dipendenze contrassegnati come dipendenze di sviluppo.
- **File Packages. config singoli per diverse piattaforme**: quando si sviluppano applicazioni per più piattaforme di destinazione, è comune disporre di file di progetto diversi per ognuno degli ambienti di compilazione corrispondente. È anche comune per utilizzare diversi pacchetti NuGet nei file di progetto diverso, come i pacchetti hanno vari livelli di supporto per piattaforme diverse. NuGet 2.8 offre un supporto migliorato per questo scenario, creando file Packages. config diverso per i file di progetto specifico della piattaforma differente.
- **Fallback alla Cache locale**: i pacchetti NuGet anche se vengono utilizzati in genere da una raccolta remota, ad esempio il [NuGet gallery](http://www.nuget.org) Usa una connessione di rete, sono disponibili molti scenari in cui il client non è connesso. Senza una connessione di rete, il client di NuGet non è in grado di installare correttamente i pacchetti, anche quando tali pacchetti erano già presenti nel computer del client nella cache NuGet locale. NuGet 2.8 aggiunge fallback automatico nella cache alla console di gestione pacchetti.

    La funzionalità di fallback della cache non richiedono alcun argomento di comando specifico. Inoltre, fallback nella cache attualmente funziona solo nella console di gestione pacchetti: il comportamento non funziona nella finestra di dialogo Gestione pacchetti.
- **Correzioni di bug**: uno delle principali correzioni di bug apportate era il miglioramento delle prestazioni nel pacchetto di aggiornamento-reinstallare comando.

    Oltre a queste funzionalità e la correzione delle prestazioni indicati in precedenza, questa versione di NuGet include anche molte altre correzioni di bug. Si sono verificati 181 totali problemi risolti nella versione. Per un elenco completo delle operazioni elementi risolti in NuGet 2.8, vista la [NuGet Issue Tracker](https://nuget.codeplex.com/workitem/list/advanced?release=NuGet%202.8&status=all) per questa versione.

<a id="webforms"></a>
### <a name="aspnet-web-forms"></a>Web Form ASP.NET

- I modelli Web Form ora mostrano come eseguire la conferma dell'Account e la reimpostazione della Password per ASP.NET Identity.
- Il controllo origine dati di entità e Provider di dati dinamici per Entity Framework 6. Per altre informazioni, vedere il blog MSDN seguente: [provider di dati dinamici e il controllo EntityDataSource per Entity Framework 6](https://blogs.msdn.com/b/webdev/archive/2014/01/30/announcing-preview-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx).

<a id="mvc"></a>
### <a name="aspnet-mvc-512"></a>ASP.NET MVC 5.1.2

- [Miglioramenti di Routing dell'attributo](../../../mvc/overview/releases/mvc51-release-notes.md#AttributeRouting)
- [Supporto di bootstrap per i modelli di editor](../../../mvc/overview/releases/mvc51-release-notes.md#Bootstrap)
- [Supporto di enumerazione in visualizzazioni](../../../mvc/overview/releases/mvc51-release-notes.md#Enum)
- [Supporto Unobstrusive per MinLength / MaxLength attributi](../../../mvc/overview/releases/mvc51-release-notes.md#Unobtrusive)
- [Supporto di contesto 'this' in Ajax Unobtrusive](../../../mvc/overview/releases/mvc51-release-notes.md#thisContext)
- Vari [correzioni di bug](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=MVC&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webapi"></a>
### <a name="aspnet-web-api-212"></a>2.1.2 l'API Web ASP.NET

- [Gestione degli errori globali](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#global-error)
- [Miglioramenti di routing dell'attributo](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#attribute-routing)
- [Miglioramenti alla pagina della Guida](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#help-page)
- [Supporto IgnoreRoute](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#ignoreroute)
- [Formattatore di media type BSON](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#bson)
- [Supporto migliorato per i filtri asincroni](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#async-filters)
- [Eseguire una query di analisi per il client di libreria di formattazione](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#query-parsing)
- Vari [correzioni di bug](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20API%7cWeb%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webpages"></a>
### <a name="aspnet-web-pages-312"></a>3.1.2 delle pagine Web ASP.NET

- Vari [correzioni di bug](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20Pages/Razor&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="ef"></a>
### <a name="entity-framework-61"></a>Entity Framework 6.1

Entity Framework è stato aggiornato alla versione 6.1 per entrambi i runtime e degli strumenti. Entity Framework (EF) 6.1 è un aggiornamento secondario a Entity Framework 6 e include una serie di nuove funzionalità e correzioni di bug. Per informazioni dettagliate su EF6.1, inclusi collegamenti alla documentazione per le nuove funzionalità, vedere [cronologia delle versioni di Entity Framework](https://msdn.microsoft.com/data/jj574253). Le nuove funzionalità in questa versione includono:

- **Gli strumenti di consolidamento** fornisce un modo coerente per creare un nuovo modello di Entity Framework. Questa funzionalità consente di estendere la procedura guidata ADO.NET Entity Data Model per supportare la creazione di modelli di Code First, tra cui reverse engineering da un database esistente. Queste funzionalità in precedenza erano disponibili in qualità di Beta negli strumenti avanzati di Entity Framework.
- **La gestione di errori di commit delle transazioni** fornisce la nuova [System.Data.Entity.Infrastructure.CommitFailureHandler](https://msdn.microsoft.com/library/system.data.entity.infrastructure.commitfailurehandler(v=vs.113).aspx) che usa la funzionalità appena introdotta di intercettare le operazioni di transazioni. Il **CommitFailureHandler** consente il recupero automatico da errori di connessione durante il commit di una transazione.
- **IndexAttribute** sono consentiti indici specificare inserendo un attributo in una proprietà (o proprietà) nel modello Code First. Codice prima di tutto quindi creerà un indice corrispondente nel database.
- **L'API di mapping pubblico** fornisce l'accesso alle informazioni di Entity Framework è in modalità di mapping dei tipi e proprietà a colonne e tabelle nel database. Nelle versioni precedenti questa API è interna.
- **Possibilità di configurare gli intercettori tramite il file App/Web.config**(consentendo intercettori essere aggiunti senza dover ricompilare l'applicazione).
- **DatabaseLogger** è disponibile un nuovo intercettore che rende più semplice registrare tutte le operazioni di database in un file. In combinazione con la funzionalità precedente, ciò consente di cambiare facilmente la registrazione di operazioni di database per un'applicazione distribuita, senza dover ricompilare.
- **Rilevamento delle modifiche del modello migrazioni** è stata migliorata in modo che le migrazioni con scaffolding sono più accurate; le prestazioni del processo di rilevamento modifiche è stata notevolmente migliorata.
- **Miglioramenti delle prestazioni** tra cui operazioni di database ridotto durante l'inizializzazione, le ottimizzazioni per il confronto di uguaglianza null nelle query LINQ, visualizzazione generazione (la creazione del modello) più velocemente in più scenari e più efficiente materializzazione di entità rilevate con più associazioni.

<a id="identity"></a>
### <a name="aspnet-identity-200"></a>Identità ASP.NET 2.0.0

- **L'autenticazione a due fattori**: ASP.NET Identity ora supporta l'autenticazione a due fattori. L'autenticazione a due fattori fornisce un ulteriore livello di sicurezza per gli account utente nel caso in cui viene compromesso la password. È inoltre disponibile la protezione per gli attacchi di forza bruta contro i codici di autenticazione a due fattori.
- **Blocco degli account:** fornisce un modo per bloccare l'utente se l'utente immette la propria password o i codici a due fattori in modo non corretto. Il numero di tentativi non validi e l'intervallo di tempo per gli utenti vengono bloccati può essere configurato. Uno sviluppatore può facoltativamente disattivare il blocco degli Account per determinati account utente in caso di.
- **Conferma dell'account:** il sistema di identità ASP.NET supporta ora la conferma dell'Account. Si tratta di uno scenario piuttosto comune nella maggior parte dei siti Web di oggi in cui, quando si registra per un nuovo account nel sito Web, è necessario confermare l'indirizzo di posta elettronica prima di eseguire qualsiasi operazione nel sito Web. Conferma tramite posta elettronica è utile perché impedisce gli account fittizi di creazione. Ciò è particolarmente utile se si usa l'indirizzo di posta elettronica come metodo di comunicazione con gli utenti del sito Web, ad esempio siti di Forum, banking, e-commerce o siti web basati su social network.
- **La reimpostazione della password:** la reimpostazione della Password è una funzionalità in cui l'utente può reimpostare la password se si hanno dimenticato la password.
- **Indicatore di sicurezza (accesso ovunque):** supporta un modo per rigenerare il Token di sicurezza per l'utente in casi quando l'utente modifica la password o altri titoli correlati informazioni quali la rimozione di un account di accesso associato (ad esempio Facebook, Google, Account Microsoft e così via). Ciò è necessario per garantire che tutti i token generati con la vecchia password non sono più validi. Nel progetto di esempio, se si modifica la password dell'utente quindi un nuovo token viene generato per l'utente e non invalida i token precedenti. Questa funzionalità offre un ulteriore livello di sicurezza per l'applicazione poiché quando si modifica la password, verrà disconnesso da qualsiasi posizione (tutti gli altri browser) in cui si sono connessi a questa applicazione.
- **Rendere il tipo di chiave primaria di essere estensibile per utenti e ruoli**: In ASP.NET Identity 1.0, il tipo di chiave primaria per la tabella che gli utenti e ruoli era stringhe. Ciò significa che quando il sistema di identità di ASP.NET è stata resa persistente in SQL Server usando Entity Framework, usavamo nvarchar. Si sono verificati molte discussioni questa implementazione predefinita in Stack Overflow e in base ai commenti in ingresso. Microsoft ha fornito un hook di estensibilità dove è possibile specificare cosa deve essere la chiave primaria della tabella di utenti e ruoli. Questo hook di estensibilità è particolarmente utile che se si esegue la migrazione dell'applicazione e l'applicazione è stata l'archiviazione degli ID utente sono GUID o tipi int.
- **Supporto IQueryable su utenti e ruoli**: aggiunta del supporto per l'oggetto IQueryable su UsersStore e RolesStore, è possibile ottenere facilmente l'elenco di utenti e ruoli.
- **Operazione di eliminazione di supporto tramite la classe UserManager**
- **L'indicizzazione su nome utente**: implementazione di ASP.NET Identity Entity Framework, è stato aggiunto il un indice univoco su nome utente utilizzando il nuovo IndexAttribute in EF 6.1.0. Ciò assicura che i nomi utente sono sempre univoci e si è verificato alcuna condizione di race condition in cui si finiva con i nomi utente duplicati.
- **Migliorata convalida della Password:** al validator della password che è stato fornito in ASP.NET Identity 1.0 è stato un validator della password abbastanza semplice che è stata solo la convalida la lunghezza minima. È disponibile un nuovo validator della password che ti offre maggiore controllo sulla complessità della password. Si noti che anche se si attiva tutte le impostazioni in questa password, si consiglia di abilitare l'autenticazione a due fattori per gli account utente.
- **IdentityFactory Middleware / CreatePerOwinContext**:

    - **Responsabile utente**: È possibile usare l'implementazione della Factory per ottenere un'istanza della classe UserManager dal contesto OWIN. Questo modello è simile a ciò che viene usato per ottenere AuthenticationManager dal contesto OWIN per l'accesso e disconnessione. Questo è un modo consigliato per ottenere un'istanza della classe UserManager per ogni richiesta per l'applicazione.
    - **DbContextFactory**: ASP.NET Identity Usa Entity Framework per rendere persistente il sistema di identità in SQL Server. A tale scopo il sistema di identità contiene un riferimento al ApplicationDbContext. Il DbContextFactory Middleware restituisce un'istanza di ApplicationDbContext per ogni richiesta che è possibile usare nell'applicazione.
- **Pacchetto NuGet di esempi di identità ASP.NET**: questo pacchetto esempi rendono più semplice installare ed eseguire gli esempi per ASP.NET Identity e seguire le procedure consigliate. Si tratta di un'applicazione ASP.NET MVC di esempio. . Modificare il codice in base all'applicazione prima di distribuire nell'ambiente di produzione. L'esempio deve essere installato in un'applicazione ASP.NET vuota. Per ulteriori informazioni sul pacchetto, vedere il post di blog seguente: [annuncio della versione RTM di ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx)

<a id="owin"></a>
### <a name="microsoft-owin-components"></a>Componenti Microsoft OWIN

Non vi sono molti i bug risolti in questa versione. Vedere le [note sulla versione per il 2.1.0 versione](https://katanaproject.codeplex.com/releases/view/113281) per informazioni più dettagliate.

<a id="signalr"></a>
### <a name="aspnet-signalr-202"></a>ASP.NET SignalR 2.0.2

Non vi sono molti i bug risolti in questa versione. Vedere le [note sulla versione per il 2.0.2 versione](https://github.com/SignalR/SignalR/releases/tag/2.0.2) per informazioni più dettagliate.
