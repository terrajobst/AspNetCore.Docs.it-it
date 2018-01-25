---
uid: whitepapers/mvc3-release-notes
title: ASP.NET MVC 3 | Microsoft Docs
author: rick-anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/06/2010
ms.topic: article
ms.assetid: f44c166e-7e91-48a0-a6f8-d9285f3594e5
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/mvc3-release-notes
msc.type: content
ms.openlocfilehash: c1fa5d31f68b44bfdfda61c870a6825eeba18647
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-mvc-3"></a>ASP.NET MVC 3
====================
- [Panoramica](#overview)
- [Note sull'installazione](#installation-notes)
- [Requisiti software](#software-requirements)
- [Documentazione](#documentation)
- [Supporto](#support)
- [Aggiornamento di un progetto ASP.NET MVC 2 ad ASP.NET MVC 3 Tools Update](#upgrading)
- [ASP.NET MVC 3 Tools Update (12 aprile 2011)](#tu-changes)

    - [Finestra di dialogo "Aggiungi Controller" può ora lo scaffolding controller con codice di accesso ai dati e viste](#tu-AddControllerDialog)
    - [Miglioramenti per la "ASP.NET MVC 3 nuovo progetto" finestra di dialogo](#tu-ImprovementsNewDialogBox)
    - [Modelli di progetto includono ora Modernizr 1.7](#tu-Modernizr)
    - [Modelli di progetto includono le versioni aggiornate di jQuery, jQuery UI e jQuery convalida](#tu-UpdatedJQuery)
    - [Modelli di progetto includono ora ADO.NET Entity Framework 4.1 come pacchetto NuGet preinstallato](#tu-EF)
    - [Modelli di progetto includono le librerie JavaScript come pacchetti NuGet preinstallati](#tu-JavaScriptLibsNuget)
    - [Problemi noti](#tu-KI)
- [ASP.NET MVC 3 RTM (13 gennaio 2011)](#MVC3RTM)

    - [Modifica: Aggiornata la versione di jQuery UI per 1.8.7](#RTM-1)
    - [Modifica: Modificare il valore predefinito ModelMetadataProvider al DataAnnotationsModelMetadataProvider](#RTM-2)
    - [Problema risolto: Incollare parte di un'espressione Razor che contiene gli spazi vuoti risultati inversione](#RTM-3)
    - [Problema risolto: Ridenominazione di un file Razor che viene aperto nell'editor disabilita la colorazione della sintassi e IntelliSense](#RTM-4)
    - [Problemi noti](#RTM-KI)
    - [Modifiche di rilievo](#RTM-BC)
- [ASP.NET MVC 3 Release Candidate 2 (10 dicembre 2010)](#_Toc2)

    - [Modelli di progetto modificati per includere jQuery 1.4.4 1.7 convalida jQuery e jQuery UI 1.8.6y UI 1.8.6](#_Toc2_1)
    - [Classe aggiunto "AdditionalMetadataAttribute"](#_Toc2_2)
    - [Scaffolding di una migliore visualizzazione](#_Toc2_3)
    - [Metodo Html.Raw aggiunto](#_Toc2_3)
    - [Proprietà rinominato "Controller.ViewModel" e "Visualizzazione" a "ViewBag"](#_Toc2_4)
    - [Classe rinominato "ControllerSessionStateAttribute" a "SessionStateAttribute"](#_Toc2_5)
    - [RemoteAttribute rinominato "Fields" proprietà "AdditionalFields"](#_Toc2_6)
    - [Rinominato "SkipRequestValidationAttribute" a "AllowHtmlAttribute"](#_Toc2_7)
    - [Metodo modificate "Html.ValidationMessage" per visualizzare il primo messaggio di errore utile](#_Toc2_8)
    - [Predefinito @model dichiarazione di non aggiungere spazi vuoti nel documento](#_Toc2_9)
    - [Proprietà aggiunto "FileExtensions" per i motori di visualizzazione per supportare i nomi di File specifici del motore](#_Toc2_10)
    - [Supporto predefinito "LabelFor" per generare il valore corretto per l'attributo "For"](#_Toc2_11)
    - [Metodo predefinito "RenderAction" per fornire valori espliciti precedenza durante l'associazione del modello](#_Toc2_12)
    - [Modifiche di rilievo](#_Toc2_BC)
    - [Problemi noti](#_Toc2_KI)
- [ASP.NET MVC 3 Release Candidate (9 novembre 2010)](#TOC_ASP_NET_3_RC)

    - [Nuove funzionalità in ASP.NET MVC 3 RC](#_Toc276711785)
    - [Gestione pacchetti NuGet](#_Toc276711786)
    - ["Nuovo progetto" migliorata la finestra di dialogo](#_Toc276711787)
    - [Controller di sessione](#_Toc276711788)
    - [Nuovi attributi di convalida](#_Toc276711789)
    - [Nuovi overload per i metodi di "LabelForModel" e "LabelFor"](#_Toc276711790)
    - [Azione figlio la memorizzazione nella cache di Output](#_Toc276711791)
    - [Miglioramenti casella della finestra di dialogo "Aggiungi visualizzazione"](#_Toc276711792)
    - [Convalida della richiesta granulari](#_Toc276711793)
    - [Modifiche di rilievo](#_Toc276711794)
    - [Problemi noti](#_Toc276711795)
- [ASP. Note sulla versione Beta MVC 3 (6 ottobre 2010)](#TOC_ASP_NET_3_Beta)

    - [Nuove funzionalità nella versione Beta di ASP.NET MVC 3](#0.1__Toc274034215)
    - [NuPack Package Manager](#0.1__Toc274034216)
    - [Finestra di dialogo Nuovo progetto migliorato](#0.1__Toc274034217)
    - [Un modo semplice per specificare fortemente tipizzato modelli nelle visualizzazioni Razor](#0.1__Toc274034218)
    - [Supporto per nuovi metodi di supporto di pagine Web ASP.NET](#0.1__Toc274034219)
    - [Supporto dell'attacco intrusivo nel codice delle dipendenze aggiuntive](#0.1__Toc274034220)
    - [Nuovo supporto per Ajax basate su jQuery non intrusiva](#0.1__Toc274034221)
    - [Nuovo supporto per la convalida di jQuery non intrusiva](#0.1__Toc274034222)
    - [Nuovo flag a livello di applicazione per la convalida del Client e di JavaScript non intrusivo](#0.1__Toc274034223)
    - [Nuovo supporto per il codice che viene eseguito prima dell'esecuzione di viste](#0.1__Toc274034224)
    - [Nuovo supporto per la sintassi Razor VBHTML](#0.1__Toc274034225)
    - [Controllo più granulare ValidateInputAttribute](#0.1__Toc274034226)
    - [Helper per convertire i caratteri di sottolineatura in trattini per i nomi di attributo HTML specificati l'utilizzo di oggetti anonimi](#0.1__Toc274034227)
    - [Correzioni di bug](#0.1__Toc274034228)
    - [Modifiche di rilievo](#0.1__Toc274034229)
    - [Problemi noti](#0.1__Toc274034230)
- [Dichiarazione di non responsabilità](#0.1__Toc274034231)

<a id="overview"></a>
## <a name="overview"></a>Panoramica

Questo documento descrive la versione di ASP.NET MVC 3 RTM per Visual Studio 2010. ASP.NET MVC è un framework per lo sviluppo di applicazioni Web che utilizza il modello Model-View-Controller (MVC). Il programma di installazione di ASP.NET MVC 3 include i componenti seguenti:

- Componenti di runtime di ASP.NET MVC 3
- Strumenti di ASP.NET MVC 3 Visual Studio 2010
- Componenti di run-time di pagine Web ASP.NET
- Strumenti di Visual Studio 2010 di pagine Web ASP.NET
- Gestione pacchetti Microsoft per .NET (NuGet)
- Un aggiornamento per Visual Studio 2010 che consente il supporto per la sintassi Razor. (Per informazioni dettagliate, vedere articolo della Knowledge Base 2483190).

Il set completo di note sulla versione per ogni versione non definitiva di ASP.NET MVC 3 è reperibile nel sito Web ASP.NET all'URL seguente:

https://www.asp.net/learn/whitepapers/mvc3-release-notes

<a id="installation-notes"></a>
## <a name="installation-notes"></a>Note sull'installazione

Per installare ASP.NET MVC 3 RTM con l'installazione guidata piattaforma Web (PI Web), visitare la pagina seguente:

[https://www.microsoft.com/web/gallery/install.aspx?appid=MVC3](https://www.microsoft.com/web/gallery/install.aspx?appid=MVC3)

In alternativa, è possibile scaricare il programma di installazione per ASP.NET MVC 3 RTM per Visual Studio 2010 dalla pagina seguente:

https://go.microsoft.com/fwlink/?LinkID=208140

ASP.NET MVC 3 può essere installato ed è possibile eseguire side-by-side con ASP.NET MVC 2.

<a id="software-requirements"></a>
## <a name="software-requirements"></a>Requisiti software

I componenti di runtime di ASP.NET MVC 3 richiedono il software seguente:

- .NET framework versione 4. 

    ASP.NET MVC 3 Visual Studio 2010 tools richiedono il software seguente:
- Visual Studio 2010 o Visual Web Developer 2010 Express.

<a id="documentation"></a>
## <a name="documentation"></a>Documentazione

Documentazione per ASP.NET MVC è disponibile nel sito Web MSDN all'URL seguente:

[https://go.microsoft.com/fwlink/?LinkId=205717](https://go.microsoft.com/fwlink/?LinkId=205717)

Esercitazioni e altre informazioni su ASP.NET MVC sono disponibili nella pagina MVC del sito Web ASP.NET all'URL seguente:

[https://www.asp.net/mvc/](../mvc/index.md)

<a id="support"></a>
## <a name="support"></a>Supporto

Si tratta di una versione completamente supportata. Informazioni su come ottenere supporto tecnico, visitare il [sito Web del supporto Microsoft](https://support.microsoft.com/).

È inoltre possibile pubblicare domande relative a questa versione il forum di ASP.NET MVC, in cui i membri della community ASP.NET sono spesso in grado di fornire supporto informale:

[https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)

<a id="upgrading"></a>
## <a name="upgrading-an-aspnet-mvc-2-project-to-aspnet-mvc-3-tools-update"></a>Aggiornamento di un progetto ASP.NET MVC 2 ad ASP.NET MVC 3 Tools Update

ASP.NET MVC 3 può essere installato in modo affiancato con ASP.NET MVC 2 nello stesso computer, che offre una flessibilità nella scelta del momento aggiornare un'applicazione ASP.NET MVC 2 ad ASP.NET MVC 3.

Per aggiornare manualmente un'applicazione ASP.NET MVC 2 esistente alla versione 3, effettuare le operazioni seguenti:

1. Creare un nuovo progetto ASP.NET MVC 3 vuoto nel computer in uso. Questo progetto conterrà alcuni file necessari per l'aggiornamento.
2. Copiare i file seguenti dal progetto ASP.NET MVC 3 nel percorso corrispondente del progetto ASP.NET MVC 2. È necessario aggiornare tutti i riferimenti alla libreria jQuery per tenere conto per il nuovo nome del file (jQuery-1.5.1.js): 

    - /Views/Web.config
    - /packages.config
    - /scripts/\*.js
    - /Contenuto/temi/\*.\*
3. Copia il *pacchetti* cartella nella radice della soluzione di progetto ASP.NET MVC 3 vuota nella radice della soluzione, ovvero nella directory in cui si trova il file con estensione sln della soluzione.
4. Se il progetto ASP.NET MVC 2 contiene tutte le aree, copiare il file /Views/Web.config il *viste* cartella relativa a ogni area.
5. In entrambi i file Web. config nel progetto ASP.NET MVC 2, a livello globale cercare e sostituire la versione di ASP.NET MVC. Individuare la stringa seguente: 

    [!code-console[Main](mvc3-release-notes/samples/sample1.cmd)]

    Sostituirlo con gli elementi seguenti:

    [!code-console[Main](mvc3-release-notes/samples/sample2.cmd)]
6. In Esplora soluzioni, eliminare il riferimento a *System* (che punta alla DLL dalla versione 2), quindi aggiungere un riferimento a *System* (v 3.0.0.0).
7. Aggiungere un riferimento a System.Web.WebPages.dll e System.Web.Helpers.dll. Questi assembly si trovano nelle cartelle seguenti: 

    - %ProgramFiles%\ Microsoft ASP.NET\ASP.NET MVC 3\Assemblies
    - %ProgramFiles%\ Microsoft ASP.NET\ASP.NET Web Pages\v1.0\Assemblies
8. In Esplora soluzioni fare doppio clic sul nome del progetto e scegliere Scarica progetto. Quindi fare di nuovo il nome del progetto e scegliere Modifica *ProjectName*con estensione csproj.
9. Individuare il *ProjectTypeGuids* elemento e sostituire {F85E285D-A4E0-4152-9332-AB1D724D3325} con {E53F8FEA-EAE0-44A6-8774-FFD645390401}.
10. Salvare le modifiche, fare clic sul progetto e quindi scegliere Ricarica progetto.
11. Nel file Web. config radice dell'applicazione aggiungere le seguenti impostazioni per il *assembly* sezione. 

    [!code-xml[Main](mvc3-release-notes/samples/sample3.xml)]
12. Se il progetto fa riferimento a tutte le librerie di terze parti che vengono compilate utilizzando ASP.NET MVC 2, aggiungere il seguente evidenziato *bindingRedirect* elemento nel file Web. config nella radice dell'applicazione sotto il  *configurazione* sezione: 

    [!code-xml[Main](mvc3-release-notes/samples/sample4.xml)]

<a id="tu-changes"></a>
## <a name="changes-in-aspnet-mvc-3-tools-update"></a>Modifiche apportate in ASP.NET MVC 3 Tools Update

In questa sezione descrive le modifiche apportate nella versione di ASP.NET MVC 3 Tools Update fin dalla versione di ASP.NET MVC 3 RTM.

<a id="tu-AddControllerDialog"></a>
### <a name="add-controller-dialog-box-can-now-scaffold-controllers-with-views-and-data-access-code"></a>Finestra di dialogo "Aggiungi Controller" può ora lo scaffolding controller con codice di accesso ai dati e viste

Lo scaffolding consente di generare rapidamente un controller e visualizzazioni per l'applicazione. Dopo avere generato il codice, è possibile modificarlo per soddisfare i requisiti del progetto.

Per avviare il *Aggiungi Controller* destro del mouse nella finestra di dialogo ASP.NET MVC 3, il *controller* cartella *Esplora soluzioni*, fare clic su *Aggiungi*, quindi fare clic su *Controller*. Nella finestra di dialogo è stata migliorata per offrire ulteriori opzioni di scaffolding.

![](mvc3-release-notes/_static/image1.png)

Per impostazione predefinita sono disponibili tre modelli di scaffolding.

#### <a name="empty-controller"></a>Controller vuoto

Questo modello genera un file del controller vuoto. Questo modello è equivale a non *azioni per la creazione, modifica e i dettagli di aggiungere, eliminare gli scenari di* nelle versioni precedenti di ASP.NET MVC. Se si sceglie questa opzione, non le ulteriori opzioni disponibili.

#### <a name="controller-with-empty-readwrite-actions"></a>Controller con azioni di lettura/scrittura vuote

Questo modello genera un file del controller che dispone di tutti i metodi di azione richiesta, ma nessun codice di implementazione nei metodi. Questo modello equivale a selezionare *azioni per la creazione, modifica e i dettagli di aggiungere, eliminare gli scenari di* nelle versioni precedenti di ASP.NET MVC. Se si sceglie questa opzione, non le ulteriori opzioni disponibili.

#### <a name="controller-with-readwrite-actions-and-views-using-entity-framework"></a>Controller con azioni di lettura/scrittura e visualizzazioni, mediante Entity Framework

Questo modello consente di creare rapidamente un'interfaccia utente per l'immissione di dati di lavoro. Genera il codice che gestisce una gamma di comuni requisiti e scenari, ad esempio le operazioni seguenti:

- *Accesso ai dati*. Il codice generato legge e scrive le entità in un database. Funziona con l'approccio Entity Framework Code First se si sceglie una classe di contesto dati esistente o se si consente al modello di generare un nuovo *DbContext* classe. Funziona anche con l'approccio Entity Framework Database First o Model First se si sceglie un'esistente *ObjectContext* classe.
- *Convalida*. Il codice generato utilizza l'associazione di modelli ASP.NET MVC e funzionalità dei metadati in modo che invii di form vengono convalidati in base alle regole dichiarate nella classe di modello. Ciò comprende le regole di convalida incorporate, ad esempio il *necessari* e *StringLength* gli attributi e regole di convalida personalizzate.
- *Le relazioni uno-a-molti*. Se si definiscono relazioni di chiave esterna uno-a-molti tra le classi di modello, il codice generato produrrà elenchi a discesa per selezionare le entità correlate. Ad esempio, è possibile definire le seguenti classi di modello seguenti convenzioni Entity Framework Code First: 

    [!code-csharp[Main](mvc3-release-notes/samples/sample5.cs)]

    Quando si quindi lo scaffolding di un controller per il *prodotto* (classe), relative visualizzazioni consentiranno agli utenti di scegliere un *categoria* per ciascun *prodotto* istanza.

    Questo modello consente opzioni aggiuntive di *Aggiungi Controller* la finestra di dialogo. Per *classe modello*, è possibile scegliere qualsiasi classe di modello nella soluzione, che determina il tipo di dati che gli utenti saranno in grado di creare o modificare:
- Se si desidera utilizzare Code First di Entity Framework, è possibile scegliere qualsiasi classe di modello.
- Se si utilizza Entity Framework Database First o Model First di Entity Framework, assicurarsi di scegliere una classe di entità definita nel modello concettuale.

Per *classe contesto dati*, è possibile selezionare queste opzioni:

- Se si desidera utilizzare Code First e non hanno nessun contesto dati esistente, classe, scegliere  *&lt;nuovo contesto dati... &gt;*". Una classe del contesto dati verrà quindi generata automaticamente.
- Se si desidera utilizzare Code First e dispone di una classe di contesto dati esistente, selezionarlo qui. Verrà eseguito un aggiornamento per rendere persistente la classe di modello selezionato.
- Se si utilizza Database First o Model First, scegliere la classe di contesto di oggetto.

Per le viste, scegliere il motore di visualizzazione che si desidera utilizzare, o scegliere Nessuno se non si desidera eseguire lo scaffolding di visualizzazioni.

È possibile selezionare Avanzate Optionsto specificare ulteriori opzioni per le visualizzazioni generate. Ad esempio, è possibile scegliere il layout o la pagina master da utilizzare.

<a id="tu-ImprovementsNewDialogBox"></a>
### <a name="improvements-to-the-aspnet-mvc-3-new-project-dialog-box"></a>Miglioramenti per la "ASP.NET MVC 3 nuovo progetto" finestra di dialogo

La finestra di dialogo che consente di creare nuovi progetti ASP.NET MVC 3 include più miglioramenti, come indicato di seguito.

![](mvc3-release-notes/_static/image2.png)

#### <a name="new-intranet-project-template"></a>Nuovo modello di "Progetto Intranet"

L'elenco modello di progetto include un nuovo modello applicazione Intranet. Questo modello contiene le impostazioni per la creazione di un'applicazione web tramite l'autenticazione di Windows anziché l'autenticazione basata su form. Poiché un'applicazione intranet richiede alcune impostazioni di IIS che non possono essere incapsulate in un modello di progetto, il modello include un file readme con istruzioni su come rendere il modello di progetto di lavoro in IIS. Documentazione per il modello applicazione Intranet è disponibile nel sito Web MSDN all'URL seguente:

[https://msdn.microsoft.com/library/gg703322(VS.98).aspx](https://msdn.microsoft.com/library/gg703322(VS.98).aspx)

#### <a name="project-templates-are-now-html5-enabled"></a>Modelli di progetto sono ora abilitati per HTML5

Nella finestra di dialogo Nuovo progetto contiene ora un'opzione per aggiungere funzionalità specifiche di HTML5 per i modelli di progetto. Se si seleziona l'opzione, le visualizzazioni generate che contengono la nuova HTML5  *&lt;intestazione&gt;*,  *&lt;piè di pagina&gt;*, e  *&lt;navigazione&gt;*  elementi.

Si noti che le versioni precedenti dei browser non supportano i tag specifici di HTML5. Per risolvere questa limitazione, i modelli di progetto HTML5 includono un riferimento alla libreria Modernizr. (Vedere la sezione successiva).

<a id="tu-Modernizr"></a>
### <a name="project-templates-now-include-modernizr-17"></a>Modelli di progetto includono ora Modernizr 1.7

Modernizr è una libreria JavaScript che abilita il supporto di CSS 3 e HTML5 nei browser che non supportano ancora tali funzionalità. Questa libreria è disponibile come pacchetto NuGet preinstallato nei modelli per progetti ASP.NET MVC 3. Per ulteriori informazioni su Modernizr, vedere [http://www.modernizr.com/](http://www.modernizr.com/).

<a id="tu-UpdatedJQuery"></a>
### <a name="project-templates-include-updated-versions-of-jquery-jquery-ui-and-jquery-validation"></a>Modelli di progetto includono le versioni aggiornate di jQuery, jQuery UI e jQuery convalida

I modelli di progetto includono ora le versioni seguenti degli script jQuery:

- 1.5.1 jQuery
- 1.8 convalida jQuery
- jQuery UI 1.8.11

Queste librerie vengono incluse come pacchetti NuGet preinstallati.

<a id="tu-EF"></a>
### <a name="project-templates-now-include-adonet-entity-framework-41-as-a-pre-installed-nuget-package"></a>Modelli di progetto includono ora ADO.NET Entity Framework 4.1 come pacchetto NuGet preinstallato

ADO.NET Entity Framework 4.1 include la funzionalità Code First. Code First è un nuovo modello di sviluppo per ADO.NET Entity Framework che offre un'alternativa ai modelli Database First e Model First esistenti.

Code First è incentrato sulla definizione del modello mediante le classi POCO ("plain old CLR Object") scritte in Visual Basic o c#. Queste classi possono quindi essere mappate a un database esistente o essere utilizzate per generare uno schema di database. È possibile fornire un'ulteriore configurazione utilizzando *DataAnnotations* attributi o le fluent API.

Documentazione per l'utilizzo di codice Firstwith ASP.NET MVC è disponibile nel sito Web ASP.NET gli URL seguenti:

[https://www.ASP.NET/MVC/tutorials/Getting-Started-with-mvc3-part1-cs](../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) [https://www.asp.net/entity-framework/tutorials/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)

<a id="tu-JavaScriptLibsNuget"></a>
### <a name="project-templates-include-javascript-libraries-as-pre-installed-nuget-packages"></a>Modelli di progetto includono le librerie JavaScript come pacchetti NuGet preinstallati

Quando si crea un nuovo progetto ASP.NET MVC 3, il progetto include i file JavaScript citati in precedenza (ad esempio, la libreria Modernizr) installandoli mediante NuGet anziché aggiungendo direttamente gli script alla cartella degli script nel modello di progetto contenuto. In questo modo è possibile utilizzare NuGet per aggiornare gli script per la versione più recente quando vengono rilasciate nuove versioni degli script.

Ad esempio, data la frequenza di nuove versioni di jQuery, la versione di jQuery inclusa nel modello di progetto a un certo punto verrà aggiornata. Tuttavia, poiché jQuery è incluso come pacchetto NuGet installato, verrà informato mediante la finestra di dialogo NuGet quando sono disponibili versioni più recenti di jQuery.

Poiché jQuery include il numero di versione nel nome del file, Aggiorna jQuery alla versione più recente sarà necessario aggiornare anche il  *&lt;script&gt;*  tag che fa riferimento al file jQuery per utilizzare il nuovo nome file. Altre librerie di script incluse non includono il numero di versione nel nome dello script, pertanto possono essere aggiornate più facilmente alle versioni più recenti.

<a id="tu-KI"></a>
## <a name="known-issues"></a>Problemi noti

- In alcuni casi, l'installazione potrebbe non riuscire con l'errore messaggio "installazione non riuscita con codice di errore: (0x80070643)". Per informazioni su come risolvere questo problema, vedere [articolo della KnowledgeBase 2531566](https://support.microsoft.com/kb/2531566).
- Lo scaffolding per l'aggiunta di un controller non lo scaffolding di entità che sfruttano i vantaggi del supporto dell'ereditarietà entità all'interno di Entity Framework. Ad esempio, data una base *persona* classe ereditata da un *Student* classe lo scaffolding di *Student* classe comporterà il codice generato che non viene compilato.
- Creazione di un nuovo progetto ASP.NET MVC 3 all'interno di una cartella della soluzione cause un *NullReferenceException* errore. La soluzione alternativa consiste nel creare il progetto ASP.NET MVC 3 nella radice della soluzione e quindi spostare il file nella cartella della soluzione.
- IntelliSense per la sintassi Razor non funziona quando è installato ReSharper. Se è installato ReSharper e si desidera sfruttare il supporto IntelliSense per Razor in ASP.NET MVC 3, vedere la voce [Razor Intellisense and ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) sul blog di hadi, che illustra come usarli insieme oggi.
- Durante l'installazione, la finestra di dialogo di accettazione EULA Visualizza le condizioni di licenza in una finestra che è inferiore rispetto a quello previsto.
- Quando si modifica una visualizzazione Razor (cshtml o. *vbhtml* file), le visualizzazioni. ASP.NET MVC 3 non sono inclusi frammenti per le visualizzazioni Razor... aspxselecting un frammento di codice per ASP.NET MVC vengono visualizzati i frammenti per
- Se si installa ASP.NET MVC 3 per Visual Web Developer Express in un computer in cui non è installato Visual Studio e quindi installa in un secondo momento Visual Studio, è necessario reinstallare ASP.NET MVC 3. Visual Studio e Visual Web Developer Express condividono componenti che vengono aggiornati dal programma di installazione di ASP.NET MVC 3. Lo stesso problema si applica se si installa ASP.NET MVC 3 per Visual Studio in un computer che non hanno Visual Web Developer Express e quindi installare in un secondo momento Visual Web Developer Express.

<a id="MVC3RTM"></a>
## <a name="changes-in-aspnet-mvc-3-rtm"></a>Modifiche in ASP.NET MVC 3 RTM

In questa sezione vengono descritte le modifiche e le correzioni apportate nella versione RTM di ASP.NET MVC 3. la versione RC2.

<a id="RTM-1"></a>
### <a name="change-updated-the-version-of-jquery-ui-to-187"></a>Modifica: Aggiornata la versione di jQuery UI per 1.8.7

I modelli di progetto ASP.NET MVC per Visual Studio sono stati aggiornati per includere la versione più recente della libreria jQuery UI. I modelli includono inoltre il set minimo di file di risorse necessari per jQuery UI, ad esempio il CSS associati e i file di immagine.

<a id="RTM-2"></a>
### <a name="change-changed-the-default-modelmetadataprovider-back-to-dataannotationsmodelmetadataprovider"></a>Modifica: Modificare il valore predefinito ModelMetadataProvider al DataAnnotationsModelMetadataProvider

La versione RC2 di ASP.NET MVC 3 è stato introdotto un *CachedDataAnnotationsMetadataProvider* classe che ha fornito la memorizzazione nella cache su esistente *DataAnnotationsModelMetadataProvider* classe come un miglioramento delle prestazioni. Tuttavia, alcuni bug sono stati segnalati con questa implementazione, pertanto la modifica è stata annullata e spostata nel progetto MVC future, disponibile all'indirizzo [WebStack ASP.NET](https://github.com/aspnet/AspNetWebStack).

<a id="RTM-3"></a>
### <a name="fixed-pasting-part-of-a-razor-expression-that-contains-whitespace-results-in-it-being-reversed"></a>Problema risolto: Incollare parte di un'espressione Razor che contiene gli spazi vuoti risultati inversione

Nelle versioni definitive di ASP.NET MVC 3, quando si incolla una parte di un'espressione Razor che contiene spazi vuoti in un file Razor, l'espressione risultante viene invertita. Si consideri ad esempio il blocco di codice Razor seguente:

[!code-cshtml[Main](mvc3-release-notes/samples/sample6.cshtml)]

Se si seleziona il testo "primo param" nel primo metodo e incollare il secondo metodo come argomento, il risultato è come segue:

[!code-cshtml[Main](mvc3-release-notes/samples/sample7.cshtml)]

Il comportamento corretto è che l'operazione Incolla deve risultare in quanto segue:

[!code-cshtml[Main](mvc3-release-notes/samples/sample8.cshtml)]

Questo problema è stato risolto nella versione RTM, in modo che l'espressione correttamente verrà conservato durante l'operazione Incolla.

<a id="RTM-4"></a>
### <a name="fixed-renaming-a-razor-file-that-is-opened-in-the-editor-disables-syntax-colorization-and-intellisense"></a>Problema risolto: Ridenominazione di un file Razor che viene aperto nell'editor disabilita la colorazione della sintassi e IntelliSense

Ridenominazione di un file Razor utilizzando Esplora soluzioni, mentre il file viene aperto nella finestra dell'editor determina l'evidenziazione della sintassi e IntelliSense per smettere di funzionare per tale file. Questo problema è stato risolto in modo che l'evidenziazione e IntelliSense vengono mantenute dopo un'operazione di ridenominazione.

<a id="RTM-KI"></a>
## <a name="known-issues"></a>Problemi noti

- Se si chiude Visual Studio 2010 SP1 Beta mentre è aperta la Console di gestione pacchetti NuGet, Visual Studio si blocca e tenta di riavviare. Questo problema verrà risolto nella versione RTM di Visual Studio 2010 SP1.
- Il programma di installazione di ASP.NET MVC 3 è in grado di installare una versione iniziale di gestione pacchetti NuGet. Dopo aver installato la versione iniziale, NuGet può essere installato e aggiornato utilizzando Gestione estensioni di Visual Studio. Se si dispone già di NuGet installato, passare a una raccolta di estensioni di Visual Studio per l'aggiornamento alla versione più recente di NuGet.
- Creazione di un nuovo progetto ASP.NET MVC 3 all'interno di una cartella della soluzione cause un *NullReferenceException* errore. La soluzione alternativa consiste nel creare il progetto ASP.NET MVC 3 nella radice della soluzione e quindi spostare il file nella cartella della soluzione.
- Il programma di installazione potrebbe richiedere molto più tempo rispetto alle versioni precedenti di ASP.NET MVC per il completamento. In questo modo viene aggiornato di componenti di Visual Studio 2010.
- IntelliSense per la sintassi Razor non funziona quando è installato ReSharper. Se è installato ReSharper e si desidera sfruttare il supporto IntelliSense per Razor in ASP.NET MVC 3, vedere la voce [Razor Intellisense and ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) sul blog di hadi, che illustra come usarli insieme oggi.
- CCSHTML e VBHTML viste create con la versione Beta di ASP.NET MVC 3 non sono impostato correttamente l'azione di compilazione, in modo che visualizzare questi tipi vengono omessi quando viene pubblicato il progetto. Il valore di azione di compilazione per questi file deve essere impostato su "Contenuto". ASP.NET MVC 3 RTM consente di correggere questo problema per i nuovi file, ma non l'impostazione corretta per i file esistenti per un progetto creato con le versioni non definitive.
- ![](mvc3-release-notes/_static/image3.png)
- Durante l'installazione, la finestra di dialogo di accettazione contratto consente di visualizzare le condizioni di licenza in una finestra di dimensioni inferiori a quelle prevista. o li&gt;
- Quando si modifica una visualizzazione Razor (cshtml file), la voce di menu Vai al Controller in Visual Studio non sarà disponibile ed non esistono alcun frammenti di codice.
- Se si installa ASP.NET MVC 3 per Visual Web Developer Express in un computer in cui non è installato Visual Studio e quindi installa in un secondo momento Visual Studio, è necessario reinstallare ASP.NET MVC 3. Visual Studio e Visual Web Developer Express condividono componenti che vengono aggiornati dal programma di installazione di ASP.NET MVC 3. Lo stesso problema si applica se si installa ASP.NET MVC 3 per Visual Studio in un computer che non hanno Visual Web Developer Express e quindi installare in un secondo momento Visual Web Developer Express.

<a id="RTM-BC"></a>
## <a name="breaking-changes"></a>Modifiche di interruzione

- Nelle versioni precedenti di ASP.NET MVC sono filtri azione creare per ogni richiesta, tranne in alcuni casi. Questo comportamento è stato mai un comportamento garantito ma semplicemente un dettaglio di implementazione e il contratto per i filtri è stato quindi considerare senza stato. In ASP.NET MVC 3, i filtri vengono memorizzati nella cache in modo più aggressivo. Pertanto, qualsiasi filtro azione personalizzata in modo non corretto di archiviare lo stato dell'istanza potrebbe essere interrotta.
- L'ordine di esecuzione per i filtri eccezioni è stato modificato per i filtri eccezioni con lo stesso *ordine* valore. In ASP.NET MVC 2 e versioni precedenti, i filtri eccezioni il controller con lo stesso *ordine* valore come quelli di un metodo di azione vengono eseguiti prima dei filtri eccezioni del metodo di azione. Questo potrebbe essere in genere quando i filtri eccezioni vengono applicati senza un oggetto specificato *ordine* valore. In ASP.NET MVC 3 questo ordine è stato invertito in modo che venga eseguito per primo il gestore di eccezioni più specifico. Come nelle versioni precedenti, se il *ordine* proprietà viene specificata in modo esplicito, i filtri vengono eseguiti nell'ordine specificato.
- Una nuova proprietà denominata *FileExtensions* è stato aggiunto per il *VirtualPathProviderViewEngine* classe di base. Quando ASP.NET cerca una visualizzazione dal percorso (non per nome), vengono considerate solo le viste con un'estensione di file contenuti nell'elenco specificato da questa nuova proprietà. Si tratta di una modifica nelle applicazioni in cui viene registrato un provider di compilazione personalizzata per abilitare un'estensione di file personalizzato per le visualizzazioni di Web Form e il provider fa riferimento a tali viste utilizzando un percorso completo anziché un nome. La soluzione alternativa consiste nel modificare il valore di *FileExtensions* proprietà da includere l'estensione di file personalizzato.
- Implementazione della factory del controller personalizzato che implementano direttamente il *IControllerFactory* interfaccia deve fornire un'implementazione del nuovo *GetControllerSessionBehavior* metodo aggiungere l'interfaccia in questa versione. In generale, si consiglia di non implementare questa interfaccia direttamente e invece derivare la classe da *DefaultControllerFactory*.

<a id="_Toc2"></a>
## <a name="changes-in-aspnet-mvc-3-rc2"></a>Modifiche in ASP.NET MVC 3 RC2

In questa sezione descrive le modifiche (nuove funzionalità e correzioni di bug) apportate nella versione di ASP.NET MVC 3 RC2 fin dalla versione RC.

<a id="_Toc2_1"></a>
### <a name="project-templates-changed-to-include-jquery-144-jquery-validation-17-and-jquery-ui-186"></a>Modelli di progetto modificato per includere jQuery 1.4.4 1.7 convalida jQuery e jQuery UI 1.8.6

I modelli di progetto ASP.NET MVC 3 includono ora le versioni più recenti di jQuery, jQuery e jQuery convalida dell'interfaccia utente. jQuery UI è una novità per i modelli di progetto e fornisce i widget dell'interfaccia utente utile. Per ulteriori informazioni su jQuery UI, visitare la home page: [http://jqueryui.com/](http://jqueryui.com/).

<a id="_Toc2_2"></a>
### <a name="added-additionalmetadataattribute-class"></a>Classe aggiunto "AdditionalMetadataAttribute"

È possibile utilizzare il *AdditionalMetadataAttribute* classe per popolare il *ModelMetadata.AdditionalValues* dizionario per una proprietà del modello.

Si supponga, ad esempio, che un modello di visualizzazione dispone di proprietà che deve essere visualizzata solo all'amministratore. Tale modello può essere annotato con il nuovo attributo utilizzando AdminOnly come chiave e true come valore, come nell'esempio seguente:

[!code-csharp[Main](mvc3-release-notes/samples/sample9.cs)]

Questi metadati viene resa disponibili a qualsiasi modello di visualizzazione o editor quando viene eseguito il rendering di un modello di visualizzazione del prodotto. È responsabilità dell'utente come sviluppatore di applicazioni di interpretare le informazioni sui metadati.

<a id="_Toc2_3"></a>
### <a name="improved-view-scaffolding"></a>Scaffolding di una migliore visualizzazione

I modelli T4 utilizzati per le visualizzazioni di scaffolding ora generano chiamate ai metodi di supporto di modello come *EditorFor* anziché helper, ad esempio *TextBoxFor*. Questa modifica migliora il supporto per i metadati del modello nella forma di attributi di annotazione dei dati quando la finestra di dialogo Aggiungi visualizzazione genera una visualizzazione.

Lo scaffolding Aggiungi visualizzazione include inoltre migliorata di rilevamento e l'utilizzo di informazioni sulla chiave primarie nel modello, basato sulla convenzione. Ad esempio, la finestra di dialogo Aggiungi visualizzazione utilizza queste informazioni per verificare che il valore della chiave primario non è scaffolding come un campo modulo modificabile.

La modifica predefinito e creare modelli includono riferimenti agli script jQuery necessari per la convalida del client.

<a id="_Toc2_4"></a>
### <a name="added-htmlraw-method"></a>Metodo Html.Raw aggiunto

Per impostazione predefinita, motore di codifica in HTML Razor visualizzare tutti i valori. Ad esempio, il frammento di codice seguente consente di codificare il codice HTML interno la variabile di saluto in modo che venga visualizzata nella pagina come &amp;lt; sicuro&amp;gt; Salve, mondo! &amp;lt; / strong&amp;gt;.

[!code-cshtml[Main](mvc3-release-notes/samples/sample10.cshtml)]

Il nuovo *Html.Raw* metodo fornisce un modo semplice per visualizzare HTML non codificato, quando il contenuto è conosciuto sicura. L'esempio seguente mostra la stessa stringa, ma la stringa viene eseguito il rendering come markup:

[!code-cshtml[Main](mvc3-release-notes/samples/sample11.cshtml)]

<a id="_Toc2_5"></a>
### <a name="renamed-controllerviewmodel-property-and-the-view-property-to-viewbag"></a>Proprietà rinominato "Controller.ViewModel" e "Visualizzazione" a "ViewBag"

In precedenza, il *ViewModel* proprietà di *Controller* corrispondeva al *vista* proprietà di una vista. Entrambe queste proprietà forniscono un modo per accedere ai valori del *ViewDataDictionary* utilizzando la sintassi di accesso dinamico proprietà dell'oggetto. Entrambe le proprietà sono state rinominate per essere lo stesso per evitare confusione e più coerenti.

<a id="_Toc2_6"></a>
### <a name="renamed-controllersessionstateattribute-class-to-sessionstateattribute"></a>Classe rinominato "ControllerSessionStateAttribute" a "SessionStateAttribute"

Il *ControllerSessionStateAttribute* classe è stata introdotta nella versione RC di ASP.NET MVC 3. La proprietà è stata rinominata da più conciso.

<a id="_Toc2_7"></a>
### <a name="renamed-remoteattribute-fields-property-to-additionalfields"></a>RemoteAttribute rinominato "Fields" proprietà "AdditionalFields"

Il *RemoteAttribute* della classe *campi* proprietà causa confusione tra gli utenti. Ridenominazione di questa proprietà su *AdditionalFields* illustra lo scopo.

<a id="_Toc2_8"></a>
### <a name="renamed-skiprequestvalidationattribute-to-allowhtmlattribute"></a>Rinominato "SkipRequestValidationAttribute" a "AllowHtmlAttribute"

Il *SkipRequestValidationAttribute* attributo è stato rinominato in *AllowHtmlAttribute* per rappresentare meglio l'utilizzo previsto.

<a id="_Toc2_9"></a>
### <a name="changed-htmlvalidationmessage-method-to-display-the-first-useful-error-message"></a>Metodo modificate "Html.ValidationMessage" per visualizzare il primo messaggio di errore utile

Il *Html.ValidationMessage* metodo è stato corretto per visualizzare il primo messaggio di errore utile anziché semplicemente visualizzare il primo errore.

Durante l'associazione del modello, il *ModelState* dizionario può essere popolato da più origini con messaggi di errore sulla proprietà, incluso il modello stesso (se implementa *IValidatableObject* ), dagli attributi di convalida applicati alla proprietà e le eccezioni generate durante l'accesso alla proprietà viene.

Quando il *Html.ValidationMessage* metodo visualizza un messaggio di convalida, ignora le voci di stato del modello che includono un'eccezione, poiché questi sono in genere previsti per l'utente finale. Al contrario, il metodo cerca il primo messaggio di convalida non è associata a un'eccezione e viene visualizzato il messaggio. Se non viene trovato alcun messaggio di questo tipo, per impostazione predefinita un messaggio di errore generico che è associato con la prima eccezione.

<a id="_Toc2_10"></a>
### <a name="fixed-model-declaration-to-not-add-whitespace-to-the-document"></a>Predefinito @model dichiarazione di non aggiungere spazi vuoti nel documento

Nelle versioni precedenti, il  *@model*  dichiarazione nella parte superiore di una vista aggiunta una riga vuota per l'output del rendering HTML. È stato risolto in modo che la dichiarazione non introduce gli spazi vuoti.

<a id="_Toc2_11"></a>
### <a name="added-fileextensions-property-to-view-engines-to-support-engine-specific-file-names"></a>Proprietà aggiunto "FileExtensions" per i motori di visualizzazione per supportare i nomi di File specifici del motore

Un motore di visualizzazione può restituire una vista utilizzando un percorso di visualizzazione esplicita come nell'esempio seguente:

[!code-csharp[Main](mvc3-release-notes/samples/sample12.cs)]

Il motore di visualizzazione prima tenta sempre di eseguire il rendering della visualizzazione. Per impostazione predefinita, il motore di visualizzazione Web Form è il primo motore di visualizzazione; Poiché il motore di Web Form non è possibile eseguire il rendering di una visualizzazione Razor, si verifica un errore. Motori di visualizzazione è ora dispongono un *FileExtensions* proprietà utilizzata per specificare le estensioni di file supportano. Questa proprietà viene verificata quando ASP.NET determina se un motore di visualizzazione può eseguire il rendering di un file. Si tratta di una modifica di rilievo e per ulteriori informazioni, vedere il [modifiche di rilievo](#_Toc2_BC) sezione di questo documento.

<a id="_Toc2_12"></a>
### <a name="fixed-labelfor-helper-to-emit-the-correct-value-for-the-for-attribute"></a>Supporto predefinito "LabelFor" per generare il valore corretto per l'attributo "For"

Posizione in cui è stato corretto un bug di *LabelFor* metodo sottoposto a rendering un *per* attributo corrispondente il *input* dell'elemento *nome* attributo il relativo ID. In base alle W3C, il *per* attributo deve corrispondere il *input* ID. elemento

<a id="_Toc2_13"></a>
### <a name="fixed-renderaction-method-to-give-explicit-values-precedence-during-model-binding"></a>Metodo predefinito "RenderAction" per fornire valori espliciti precedenza durante l'associazione del modello

Nelle versioni precedenti, valori espliciti che sono stati passati al *RenderAction* metodo sono stati viene ignorato e verrà usata i valori del form corrente durante l'associazione del modello all'interno di un'azione figlio. La correzione assicura che i valori espliciti hanno la precedenza durante l'associazione del modello.

<a id="_Toc2_BC"></a>
## <a name="breaking-changes"></a>Modifiche di interruzione

- Nelle versioni precedenti di ASP.NET MVC, i filtri azione sono stati creati per ogni richiesta, tranne in alcuni casi. Questo comportamento è stato mai un comportamento garantito ma semplicemente un dettaglio di implementazione e il contratto per i filtri è stato quindi considerare senza stato. In ASP.NET MVC 3, i filtri vengono memorizzati nella cache in modo più aggressivo. Pertanto, qualsiasi filtro azione personalizzata in modo non corretto di archiviare lo stato dell'istanza potrebbe essere interrotta.
- L'ordine di esecuzione per i filtri eccezioni è stato modificato per i filtri eccezioni con lo stesso *ordine* valore. In ASP.NET MVC 2 e versioni precedenti, i filtri eccezioni il controller che ha lo stesso *ordine* valore come quelli di un metodo di azione venivano eseguiti prima dei filtri eccezioni del metodo di azione. Questo potrebbe essere in genere quando i filtri eccezioni vengono applicati senza un oggetto specificato *ordine* valore. In ASP.NET MVC 3 questo ordine è stato invertito in modo che venga eseguito per primo il gestore di eccezioni più specifico. Come nelle versioni precedenti, se il *ordine* proprietà viene specificata in modo esplicito, i filtri vengono eseguiti nell'ordine specificato.
- Una nuova proprietà denominata *FileExtensions* è stato aggiunto per il *VirtualPathProviderViewEngine* classe di base. Quando ASP.NET cerca una visualizzazione dal percorso (non per nome), vengono considerate solo le viste con un'estensione di file contenuti nell'elenco specificato da questa nuova proprietà. Si tratta di una modifica nelle applicazioni in cui viene registrato un provider di compilazione personalizzata per abilitare un'estensione di file personalizzato per le visualizzazioni di Web Form e il provider fa riferimento a tali viste utilizzando un percorso completo anziché un nome. La soluzione alternativa consiste nel modificare il valore di *FileExtensions* proprietà da includere l'estensione di file personalizzato.
- Implementazione della factory del controller personalizzato che implementano direttamente il *IControllerFactory* interfaccia deve fornire un'implementazione del nuovo *GetControllerSessionBehavior * * metodo aggiunto al interfaccia in questa versione*. In generale, si consiglia di non implementare questa interfaccia direttamente e invece derivare la classe da *DefaultControllerFactory*.

<a id="_Toc2_KI"></a>
## <a name="known-issues"></a>Problemi noti

- Il programma di installazione di ASP.NET MVC 3 è in grado di installare una versione iniziale di gestione pacchetti NuGet. Dopo aver installato la versione iniziale, NuGet può essere installato e aggiornato utilizzando Gestione estensioni di Visual Studio. Se si dispone già di NuGet installato, passare a una raccolta di estensioni di Visual Studio per l'aggiornamento alla versione più recente di NuGet.
- Creazione di un nuovo progetto ASP.NET MVC 3 all'interno di una cartella della soluzione cause un *NullReferenceException* errore. La soluzione alternativa consiste nel creare il progetto ASP.NET MVC 3 nella radice della soluzione e quindi spostare il file nella cartella della soluzione.
- Il programma di installazione potrebbe richiedere molto più tempo rispetto alle versioni precedenti di ASP.NET MVC per il completamento. In questo modo viene aggiornato di componenti di Visual Studio 2010.
- IntelliSense per la sintassi Razor non funziona quando è installato ReSharper. Se è installato ReSharper e si desidera sfruttare il supporto IntelliSense per Razor in ASP.NET MVC 3 RC2, vedere la voce [Razor Intellisense and ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) sul blog di hadi, che illustra come usarli insieme oggi.
- CSHTML e VBHTML viste create con la versione Beta di ASP.NET MVC 3 non sono impostato correttamente l'azione di compilazione, in modo che visualizzare questi tipi vengono omessi quando viene pubblicato il progetto. Il *azione di compilazione* valore per questi file devono essere impostati su contenuto ". ASP.NET MVC 3 RC2 consente di correggere questo problema per i nuovi file, ma non l'impostazione corretta per i file esistenti per un progetto creato con la versione Beta.![](mvc3-release-notes/_static/image4.png)
- Durante l'installazione, la finestra di dialogo di accettazione EULA Visualizza le condizioni di licenza in una finestra che è inferiore rispetto a quello previsto.
- Quando si modifica una visualizzazione Razor (cshtml file), la voce di menu Vai al Controller in Visual Studio non sarà disponibile ed non esistono alcun frammenti di codice.
- Se si installa ASP.NET MVC 3 per Visual Web Developer Express in un computer in cui non è installato Visual Studio e quindi installa in un secondo momento Visual Studio, è necessario reinstallare ASP.NET MVC 3. Visual Studio e Visual Web Developer Express condividono componenti che vengono aggiornati dal programma di installazione di ASP.NET MVC 3. Lo stesso problema si applica se si installa ASP.NET MVC 3 per Visual Studio in un computer che non hanno Visual Web Developer Express e quindi installare in un secondo momento Visual Web Developer Express.
- L'installazione di ASP.NET MVC 3 RC 2 non aggiornare NuGet, se già installato. Per aggiornare NuGet, passare a Visual Studio Extension manager e vengono visualizzati gli aggiornamenti disponibili. È possibile aggiornare NuGet alla versione più recente da qui.

<a id="TOC_ASP_NET_3_RC"></a>
## <a name="aspnet-mvc-3-release-candidate"></a>Versione finale candidata di ASP.NET MVC 3

ASP.NET MVC versione finale candidata è stata rilasciata il 9 novembre 2010.

<a id="_Toc276711785"></a>
## <a name="new-features-in-aspnet-mvc-3-rc"></a>Nuove funzionalità in ASP.NET MVC 3 RC

Questa sezione vengono descritte le funzionalità che sono state introdotte nella versione di ASP.NET MVC 3 RC fin dalla versione Beta.

<a id="_Toc276711786"></a>
### <a name="nuget-package-manager"></a>Gestione pacchetti NuGet

ASP.NET MVC 3 include Gestione pacchetti NuGet (precedentemente noto come NuPack), che è uno strumento di gestione integrata del pacchetto per l'aggiunta di strumenti e librerie ai progetti di Visual Studio. Questo strumento consente di automatizzare i passaggi che gli sviluppatori di eseguire oggi per ottenere una raccolta nel loro struttura ad albero di origine.

Per lavorare con NuGet come uno strumento da riga di comando, come una finestra della console integrata all'interno di Visual Studio 2010, dal menu di scelta rapida di Visual Studio e come un set di cmdlet di PowerShell.

Per ulteriori informazioni su NuGet, leggere la [documentazione di Nuget](https://docs.microsoft.com/nuget/).

<a id="_Toc276711787"></a>
### <a name="improved-new-project-dialog-box"></a>"Nuovo progetto" migliorata la finestra di dialogo

Quando si crea un nuovo progetto, la finestra di dialogo Nuovo progetto ora consente di specificare il motore di visualizzazione, nonché un tipo di progetto ASP.NET MVC.

![](mvc3-release-notes/_static/image5.png)

Supporto per la modifica dell'elenco di modelli e motori elencata nella finestra di dialogo è incluso in questa versione.

I modelli predefiniti sono i seguenti:

Vuoto. Contiene un set minimo di file per un progetto ASP.NET MVC, tra cui la struttura di directory predefinito per i progetti ASP.NET MVC, un file Site.css che contiene il valore predefinito di che ASP.NET MVC gli stili e una directory di script che contiene i file di JavaScript predefiniti.

Applicazioni Internet. Contiene una funzionalità di esempio che illustra come usare il provider di appartenenze con ASP.NET MVC.

Viene specificato l'elenco dei modelli di progetto che viene visualizzato nella finestra di dialogo del Registro di sistema.

<a id="_Toc276711788"></a>
### <a name="sessionless-controllers"></a>Controller di sessione

Il nuovo *ControllerSessionStateAttribute* garantisce un maggiore controllo sul comportamento di sessione per i controller specificando un [System.Web.SessionState.SessionStateBehavior](https://msdn.microsoft.com/library/system.web.sessionstate.sessionstatebehavior.aspx) valore di enumerazione.

Nell'esempio seguente viene illustrato come disattivare lo stato della sessione per tutte le richieste a un controller.

[!code-csharp[Main](mvc3-release-notes/samples/sample13.cs)]

Nell'esempio seguente viene illustrato come impostare lo stato della sessione di sola lettura per tutte le richieste a un controller.

[!code-csharp[Main](mvc3-release-notes/samples/sample14.cs)]

<a id="_Toc276711789"></a>
### <a name="new-validation-attributes"></a>Nuovi attributi di convalida

#### <a name="compareattribute"></a>CompareAttribute

Il nuovo *CompareAttribute* attributo di convalida consente di confrontare i valori di due diverse proprietà di un modello. Nell'esempio seguente, il *ComparePassword* proprietà deve corrispondere il *Password* campo per essere valide.

[!code-csharp[Main](mvc3-release-notes/samples/sample15.cs)]

#### <a name="remoteattribute"></a>RemoteAttribute

Il nuovo *RemoteAttribute* attributo di convalida sfrutta validator di jQuery convalida del plug-in remoto, abilita la convalida lato client chiamare un metodo nel server che esegue la logica di convalida effettiva.

Nell'esempio seguente, il *UserName* proprietà ha il *RemoteAttribute* applicato. Quando si modifica questa proprietà in una visualizzazione di modifica, la convalida del client chiamerà un'azione denominata *UserNameAvailable* sul *UsersController* classe per convalidare questo campo.

[!code-csharp[Main](mvc3-release-notes/samples/sample16.cs)]

Nell'esempio seguente viene illustrato il corrispondente controller.

[!code-csharp[Main](mvc3-release-notes/samples/sample17.cs)]

Per impostazione predefinita, il nome della proprietà che viene applicato l'attributo viene inviato al metodo di azione come parametro della stringa di query.

<a id="_Toc276711790"></a>
### <a name="new-overloads-for-labelfor-and-labelformodel-methods"></a>Nuovi overload per i metodi di "LabelForModel" e "LabelFor"

Sono stati aggiunti nuovi overload per il *LabelFor* e *LabelForModel* metodi che consentono di specificano il testo dell'etichetta. Nell'esempio seguente viene illustrato come utilizzare questi overload.

[!code-cshtml[Main](mvc3-release-notes/samples/sample18.cshtml)]

<a id="_Toc276711791"></a>
### <a name="child-action-output-caching"></a>Azione figlio la memorizzazione nella cache di Output

Il *OutputCacheAttribute* supporta la memorizzazione nella cache di output delle azioni figlio che vengono chiamate utilizzando la *Html.RenderAction* o *HTML. Action* metodi di supporto. Nell'esempio seguente viene illustrata una visualizzazione che chiama un'altra azione.

[!code-cshtml[Main](mvc3-release-notes/samples/sample19.cshtml)]

Il *GetDate* azione viene annotata con il *OutputCacheAttribute*:

[!code-csharp[Main](mvc3-release-notes/samples/sample20.cs)]

Quando si esegue questo codice, viene memorizzato nella cache il risultato della chiamata a Html.Action("GetDate") per 100 secondi.

<a id="_Toc276711792"></a>
### <a name="add-view-dialog-box-improvements"></a>Miglioramenti casella della finestra di dialogo "Aggiungi visualizzazione"

Quando si aggiunge una visualizzazione fortemente tipizzata, la finestra di dialogo Aggiungi visualizzazione Filtra ora più tipi non è applicabile nelle versioni precedenti, ad esempio molti tipi di .NET Framework core. Inoltre, l'elenco viene ora ordinato dal nome della classe e non tramite il nome completo del tipo, che rende più semplice individuare i tipi. Ad esempio, il nome del tipo viene ora visualizzato come nell'esempio seguente:

ClassName (spazio dei nomi)

Nelle versioni precedenti, questo sarebbe stato stato visualizzato come segue:

Namespace.ClassName

<a id="_Toc276711793"></a>
### <a name="granular-request-validation"></a>Convalida della richiesta granulari

Il *escludere* proprietà di *ValidateInputAttribute* non esiste più. Per la convalida della richiesta ignorata per le proprietà specifiche di un modello durante l'associazione del modello, utilizzare invece il nuovo *SkipRequestValidationAttribute*.

Si supponga, ad esempio, che un metodo di azione consente di modificare un post di blog:

[!code-csharp[Main](mvc3-release-notes/samples/sample21.cs)]

Nell'esempio seguente viene illustrato il modello di visualizzazione per un post di blog.

[!code-csharp[Main](mvc3-release-notes/samples/sample22.cs)]

Quando un utente invia del markup per la proprietà di descrizione, l'associazione di modelli avrà esito negativo a causa di convalida della richiesta. Per disabilitare la convalida della richiesta durante l'associazione del modello per il post di blog, descrizione, applicare il *SkipRequpestValidationAttribute* alla proprietà, come illustrato in questo esempio:.

[!code-csharp[Main](mvc3-release-notes/samples/sample23.cs)]

Per disattivare la convalida della richiesta per tutte le proprietà del modello, in alternativa, è consigliabile applicare *ValidateInputAttribute* con un valore di *false* al metodo di azione:

[!code-csharp[Main](mvc3-release-notes/samples/sample24.cs)]

<a id="_Toc276711794"></a>
## <a name="breaking-changes"></a>Modifiche di interruzione

- L'ordine di esecuzione per i filtri eccezioni è stato modificato per i filtri eccezioni con lo stesso *ordine* valore. In ASP.NET MVC 2 e versioni precedenti, i filtri eccezioni il controller che ha lo stesso *ordine* come quelli di un metodo di azione venivano eseguiti prima dei filtri eccezioni del metodo di azione. Questo potrebbe essere in genere quando i filtri eccezioni vengono applicati senza un oggetto specificato *ordine* valore. In ASP.NET MVC 3 questo ordine è stato invertito in modo che venga eseguito per primo il gestore di eccezioni più specifico. Come nelle versioni precedenti, se il *ordine* proprietà viene specificata in modo esplicito, i filtri vengono eseguiti nell'ordine specificato.
- Aggiungere una nuova proprietà denominata *FileExtensions* per il *VirtualPathProviderViewEngine* classe di base. Durante la ricerca di una vista dal percorso (e non per nome), solo le viste con un'estensione di file contenuti in viene considerato l'elenco specificato da questa nuova proprietà. Si tratta di una modifica di rilievo per coloro che registra un oggetto personalizzato compilare provider per abilitare un'estensione di file personalizzato per le visualizzazioni di web form ed e fanno riferimento a tali viste utilizzando un percorso completo anziché un nome. La soluzione alternativa consiste nel modificare il valore di *FileExtensions* proprietà da includere l'estensione di file personalizzato.

<a id="_Toc276711795"></a>
## <a name="known-issues"></a>Problemi noti

- Il programma di installazione potrebbe richiedere molto più tempo rispetto alle versioni precedenti di ASP.NET MVC, perché aggiornano i componenti di Visual Studio 2010.
- Lo scaffolding di Aggiungi visualizzazione quando si seleziona astrongly tipizzati strutture visualizzare le proprietà in sola scrittura. Questi devono essere ignorati sempre dallo scaffolding. La finestra di dialogo Aggiungi visualizzazione anche delle strutture delle proprietà di sola lettura durante la generazione di una visualizzazione "Modifica" o "Creazione". Proprietà di sola lettura deve essere scaffolding solo per le visualizzazioni elenco e di visualizzazione.
- Debug non funziona quando ASP.NET MVC 3 viene installato insieme a Async CTP. ASP.NET MVC 3 non può essere installato side-by-side con Async CTP. Disinstallare Async CTP per il ripristino di debug. Per altre informazioni, leggere [questo post di blog](http://drew-prog.blogspot.com/2010/11/how-to-uninstall-microsoft-aspnet-mvc-3.html) sulla disinstallazione di tutte le parti di ASP.NET MVC 3 RC.
- Intellisense per Razor non funziona quando è installato Resharper. Se è installato ReSharper e si desidera sfruttare il supporto di intellisense Razor in ASP.NET MVC 3 RC, leggere [questo post di blog](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) da JetBrains che illustra come usarli insieme oggi.
- CSHTML e VBHTML viste create con una versione Beta di ASP.NET MVC 3 non sono l'azione di compilazione correttamente che consente di omettere la pubblicazione. Il *azione di compilazione* per questi file devono essere impostati su "Contenuto". ASP.NET MVC 3 RC consente di correggere questo problema per i nuovi file, ma non l'impostazione corretta per i file esistenti per un progetto creato con la versione Beta.
- Il programma di installazione potrebbe richiedere molto più tempo rispetto alle versioni precedenti di ASP.NET MVC, perché aggiornano i componenti di Visual Studio 2010.
- Lo scaffolding di Aggiungi visualizzazione quando selezionando "Modifica" fortemente tipizzata delle strutture visualizzazione proprietà in sola lettura. Analogamente, le proprietà di sola scrittura vengono scaffolding per "visualizzazioni".
- Durante l'installazione, la finestra di dialogo di accettazione EULA Visualizza le condizioni di licenza in una finestra che è inferiore rispetto a quello previsto.
- L'installazione di [Async CTP di Visual Studio](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=18712f38-fcd2-4e9f-9028-8373dc5732b2&amp;displaylang=en) provoca un conflitto con la versione Razor che è inclusa come parte di ASP.NET MVC 3, gli strumenti di installazione. Assicurarsi che non tenti di installare Visual Studio Async CTP sia la versione Razor nello stesso computer.
- Quando si modifica una visualizzazione Razor (cshtml file), la voce di menu Vai al Controller in Visual Studio non sarà disponibile ed non esistono alcun frammenti di codice.

<a id="TOC_ASP_NET_3_Beta"></a>
## <a name="aspnet-mvc-3-beta"></a>Versione Beta di ASP.NET MVC 3

ASP.NET MVC 3 Beta è stata rilasciata il 6 ottobre 2010. Le note seguenti sono specifiche per la versione Beta e sono soggetti a eventuali aggiornamenti o modifiche a cui fa riferimento nella sezione precedente ASP.NET MVC 3 Release Candidate.

## <a id="0.1__Toc274034215"></a>Nuova versione Beta di ASP.NET MVC 3 Featuresin

<a id="0.1__Default_validation_system"></a>Questa sezione vengono descritte le funzionalità che sono state introdotte nella versione Beta di ASP.NET MVC 3.

### <a id="0.1__Toc274034216"></a>Gestione pacchetti NuGet

ASP.NET MVC 3 include Gestione pacchetti NuGet, che è uno strumento di gestione integrata pacchetto per l'aggiunta di librerie e strumenti per i progetti di Visual Studio. La maggior parte, che consente di automatizzare i passaggi che gli sviluppatori di eseguire oggi per ottenere una raccolta nel loro struttura ad albero di origine.

Per lavorare con NuGet come uno strumento da riga di comando, come una finestra della console integrata all'interno di Visual Studio 2010, dal menu di scelta rapida di Visual Studio e come set di cmdlet di PowerShell.

Per ulteriori informazioni su NuGet, leggere la [documentazione di NuGet](https://docs.microsoft.com/nuget/).

### <a id="0.1__Toc274034217"></a>Finestra di dialogo Nuovo progetto migliorato

Quando si crea un nuovo progetto, la finestra di dialogo Nuovo progetto ora consente di specificare il motore di visualizzazione, nonché un tipo di progetto ASP.NET MVC.

![](mvc3-release-notes/_static/image6.png)

Supporto per la modifica dell'elenco di modelli e motori elencata nella finestra di dialogo non è incluso in questa versione.

I modelli predefiniti sono i seguenti:

Vuoto. Contiene un set minimo di file per un progetto ASP.NET MVC, tra cui la struttura di directory predefinito per i progetti ASP.NET MVC, un piccolo file Site.css che contiene il valore predefinito di che ASP.NET MVC gli stili e una directory di script che contiene i file di JavaScript predefiniti.

Applicazioni Internet. Contiene una funzionalità di esempio che illustra come usare il provider di appartenenza in ASP.NET MVC.

### <a id="0.1__Toc274034218"></a>Un modo semplice per specificare fortemente tipizzato modelli nelle visualizzazioni Razor

Il modo per specificare il tipo di modello per le visualizzazioni Razor fortemente tipizzate è stato semplificato utilizzando il nuovo @model direttiva per le viste CSHTML e @ModelType direttiva per le viste VBHTML. Nelle versioni precedenti di ASP.NET MVC, è necessario specificare che un modello fortemente tipizzato per Razor viste in questo modo:

[!code-cshtml[Main](mvc3-release-notes/samples/sample25.cshtml)]

In questa versione, è possibile utilizzare la sintassi seguente:

[!code-cshtml[Main](mvc3-release-notes/samples/sample26.cshtml)]

### <a id="0.1__Toc274034219"></a>Supporto per nuovi metodi di supporto di pagine Web ASP.NET

La nuova tecnologia di pagine Web ASP.NET include un set di metodi di supporto sono utili per l'aggiunta di funzionalità utilizzate comunemente per le visualizzazioni e controller. ASP.NET MVC 3 supporta l'utilizzo di questi metodi di supporto all'interno di controller e visualizzazioni (se appropriato). Questi metodi sono contenuti nell'assembly System.Web.Helpers. Nella tabella seguente sono elencati alcuni dei metodi di supporto di ASP.NET Web Pages.

| **Helper** | **Descrizione** |
| --- | --- |
| Grafico | Esegue il rendering di un grafico all'interno di una vista. Contiene i metodi, ad esempio Chart.ToWebImage Chart.Save e Chart.Write. |
| Livello di crittografia | Utilizza algoritmi per creare correttamente hash salt e hash delle password. |
| WebGrid | Esegue il rendering di una raccolta di oggetti (in genere, i dati da un database) come una griglia. Supporta l'ordinamento e paging. |
| WebImage | Esegue il rendering di un'immagine. |
| WebMail | Invia un messaggio di posta elettronica. |

Un argomento di riferimento rapido che elenca le funzioni di supporto e sintassi di base è disponibile come parte della documentazione di sintassi Razor di ASP.NET all'URL seguente:

[https://www.asp.net/webmatrix/tutorials/asp-net-web-pages-api-reference](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)

### <a id="0.1__Toc274034220"></a>Supporto dell'attacco intrusivo nel codice delle dipendenze aggiuntive

Compila la versione di anteprima 1 di ASP.NET MVC 3, la versione corrente include aggiunto il supporto per due nuovi servizi e quattro i servizi esistenti e supporto migliorato per la risoluzione delle dipendenze e il localizzatore di servizi comune.

#### <a name="new-icontrolleractivator-interface-for-fine-grained-controller-instantiation"></a>Nuova interfaccia IControllerActivator per la creazione di istanze di un Controller con granularità fine

La nuova interfaccia IControllerActivator fornisce un controllo più accurato sul modo controller vengono creata un'istanza tramite l'inserimento di dipendenze. Nell'esempio seguente viene illustrata l'interfaccia:

[!code-csharp[Main](mvc3-release-notes/samples/sample27.cs)]

Per ovviare a questo ruolo della factory del controller. Una factory del controller è un'implementazione dell'interfaccia IControllerFactory, che è responsabile per l'individuazione di un tipo di controller e per creare un'istanza di tale tipo di controller.

Attivatori sono responsabili solo un'istanza di un tipo di controller. Non viene eseguita la ricerca di tipo di controller. Dopo aver individuato un tipo appropriato di controller, controller factory devono delegare a un'istanza di IControllerActivator per gestire la creazione dell'istanza effettivo del controller.

La classe DefaultControllerFactory dispone di un nuovo costruttore che accetta un'istanza di IControllerFactory. È possibile applicare l'inserimento di dipendenze per gestire questo aspetto della creazione del controller senza la necessità di eseguire l'override del comportamento di ricerca predefinito-tipo di controller.

#### <a name="iservicelocator-interface-replaced-with-idependencyresolver"></a>Interfaccia IServiceLocator sostituita con resolver di dipendenza

In base ai suggerimenti della community, la versione Beta di ASP.NET MVC 3 è sostituito l'utilizzo dell'interfaccia IServiceLocator con un'interfaccia di resolver di dipendenza ideata discesa specifica per le esigenze di ASP.NET MVC. Nell'esempio seguente viene illustrata la nuova interfaccia:

[!code-csharp[Main](mvc3-release-notes/samples/sample28.cs)]

Come parte di questa modifica, la classe ServiceLocator anche è stata sostituita con la classe DependencyResolver. Registrazione di un resolver di dipendenza è simile alle versioni precedenti di ASP.NET MVC:

[!code-csharp[Main](mvc3-release-notes/samples/sample29.cs)]

Le implementazioni di questa interfaccia devono delegare semplicemente il contenitore dell'inserimento di dipendenza sottostante per fornire il servizio registrato per il tipo richiesto.

Quando non sono presenti servizi registrati del tipo richiesto, ASP.NET MVC prevede che le implementazioni di questa interfaccia per restituire un valore null GetService e per restituire una raccolta vuota da GetServices.

La nuova classe DependencyResolver consente di registrare le classi che implementano la nuova interfaccia di resolver di dipendenza o l'interfaccia del localizzatore di servizi comune (IServiceLocator). Per ulteriori informazioni sul localizzatore di servizi comune, vedere [CommonServiceLocator su GitHub](https://github.com/unitycontainer/commonservicelocator).

<a id="0.1__Breaking_Changes"></a>

#### <a name="new-iviewactivator-interface-for-fine-grained-view-page-instantiation"></a>Nuova interfaccia IViewActivator per la creazione di istanze di granulari per le visualizzazione pagina

La nuova interfaccia IViewPageActivator fornisce un controllo più accurato su come visualizzare le pagine vengono create istanze tramite l'inserimento di dipendenze. Questo vale per le istanze di WebFormView e RazorView. Nell'esempio seguente viene illustrata la nuova interfaccia:

[!code-csharp[Main](mvc3-release-notes/samples/sample30.cs)]

Queste classi ora accettano un argomento del costruttore IViewPageActivator, che consente di utilizzare inserimento di dipendenze per controllare la modalità con cui vengono creata un'istanza di tipi ViewPage, ViewUserControl e WebViewPage.

#### <a name="new-dependency-resolver-support-for-existing-services"></a>Nuovo supporto per il Resolver di dipendenza per i servizi esistenti

La nuova versione include il supporto di risoluzione dipendenza per i servizi seguenti:

- Provider di convalida del modello. Le classi che implementano ModelValidatorProvider possono essere registrate nel sistema di risoluzione dipendenza e il sistema li userà per supportare la convalida lato client e server.
- Provider di metadati del modello. Un'unica classe che implementa ModelMetadataProvider può essere registrata nel sistema di risoluzione dipendenza e il sistema verrà utilizzato per fornire i metadati per i sistemi di convalida e i modelli.
- Provider di valori. Le classi che implementano ValueProviderFactory possono essere registrate nel sistema di risoluzione dipendenza e il sistema verrà utilizzati per creare provider di valori che vengono utilizzate per il controller e durante l'associazione del modello.
- Raccoglitori di modelli. Le classi che implementano IModelBinderProvider possono essere registrate nel sistema di risoluzione dipendenza e il sistema verrà utilizzati per creare gestori di associazione del modello utilizzati dal sistema di associazione del modello.

### <a id="0.1__Toc274034221"></a>Nuovo supporto per Ajax basate su jQuery non intrusiva

MVC ASP.NET include metodi helper Ajax, ad esempio le operazioni seguenti:

- Ajax.ActionLink
- Ajax.RouteLink
- Ajax.BeginForm
- Ajax.BeginRouteForm

Questi metodi usano JavaScript per richiamare un metodo di azione nel server anziché un postback completo. Questa funzionalità è stata aggiornata per sfruttare i vantaggi di jQuery in modo non intrusivo. Anziché intrusivo la creazione di script client in linea, questi metodi di supporto separano il comportamento dal markup mediante la creazione di attributi HTML5 utilizzando il *dati ajax* prefisso. Il comportamento viene quindi applicato al markup facendo riferimento ai file JavaScript appropriati. Assicurarsi che fanno riferimento i seguenti file JavaScript:

- jquery-1.4.1.js
- jquery.unobtrusive.ajax.js

Questa funzionalità è abilitata per impostazione predefinita nel file Web. config di ASP.NET MVC 3 nuovi modelli di progetto, ma è disabilitata per impostazione predefinita per i progetti esistenti. Per ulteriori informazioni, vedere [aggiungere flag a livello di applicazione per la convalida del client e di JavaScript non intrusivo](#0.1_AddedApplicationWideFlagsForClientValida) più avanti in questo documento.

### <a id="0.1__Toc274034222"></a>Nuovo supporto per la convalida di jQuery non intrusiva

Per impostazione predefinita, ASP.NET MVC 3 Beta Usa la convalida jQuery in modo non intrusivo per eseguire la convalida lato client. Per abilitare la convalida del client non intrusiva, effettuare una chiamata simile al seguente all'interno di una vista:

[!code-csharp[Main](mvc3-release-notes/samples/sample31.cs)]

Questa operazione richiede che ViewContext.UnobtrusiveJavaScriptEnabled sia impostata su true, è possibile eseguire la chiamata seguente:

[!code-csharp[Main](mvc3-release-notes/samples/sample32.cs)]

Assicurarsi inoltre che fanno riferimento i seguenti file JavaScript.

- jquery-1.4.1.js
- jquery.validate.js
- jquery.validate.unobtrusive.js

Questa funzionalità è attivata per impostazione predefinita nel file Web. config di ASP.NET MVC 3 nuovi modelli di progetto, ma è disabilitata per impostazione predefinita per i progetti esistenti. Per ulteriori informazioni, vedere [nuovo flag a livello di applicazione per la convalida del client e di JavaScript non intrusivo](#0.1_AddedApplicationWideFlagsForClientValida) più avanti in questo documento.

<a id="0.1__Toc274034223"></a>

### <a id="0.1_AddedApplicationWideFlagsForClientValida"></a>Nuovo flag a livello di applicazione per la convalida del Client e di JavaScript non intrusivo

È possibile abilitare o disabilitare la convalida del client e JavaScript non intrusivo a livello globale tramite i membri statici della classe, come nell'esempio seguente:

[!code-csharp[Main](mvc3-release-notes/samples/sample33.cs)]

Per impostazione predefinita, i modelli di progetto predefiniti abilitare JavaScript non intrusivo. È inoltre possibile abilitare o disabilitare queste funzionalità nel file Web. config radice dell'applicazione usando le impostazioni seguenti:

[!code-xml[Main](mvc3-release-notes/samples/sample34.xml)]

Poiché per impostazione predefinita, è possibile abilitare queste funzionalità, sono stati introdotti nuovi overload per la classe HtmlHelper che consentono di eseguire l'override le impostazioni predefinite, come illustrato negli esempi seguenti:

[!code-csharp[Main](mvc3-release-notes/samples/sample35.cs)]

Per garantire la compatibilità con le versioni precedenti, entrambe queste funzionalità sono disabilitate per impostazione predefinita.

### <a id="0.1__Toc274034224"></a>Nuovo supporto per il codice che viene eseguito prima dell'esecuzione di viste

È possibile inserire un file denominato \_viewstart.cshtml (o \_viewstart.vbhtml) nella directory delle visualizzazioni e aggiungere codice che verrà condiviso tra più viste in tale directory e nelle relative sottodirectory. Ad esempio, inserire il codice seguente nel \_viewstart.cshtml pagina nella cartella ~/Views:

[!code-cshtml[Main](mvc3-release-notes/samples/sample36.cshtml)]

Consente di impostare la pagina di layout per tutte le visualizzazioni all'interno della cartella di viste e delle relative sottocartelle in modo ricorsivo tutti. Quando una vista viene eseguito il rendering, il codice di \_viewstart.cshtml file viene eseguito prima che venga eseguito il codice di visualizzazione. Il \_viewstart.cshtml codice si applica a tutte le visualizzazioni in tale cartella.

Per impostazione predefinita, il codice di \_viewstart.cshtml file si applica anche alle viste in qualsiasi sottocartella. Singole sottocartelle possono tuttavia avere una propria versione di \_viewstart.cshtml file; in caso, la versione locale ha la precedenza. Ad esempio, per eseguire il codice che è comune a tutte le viste per la classe HomeController, inserire un \_viewstart.cshtml file nella cartella ~/Views/Home.

### <a id="0.1__Toc274034225"></a>Nuovo supporto per la sintassi Razor VBHTML

L'anteprima di ASP.NET MVC precedente è incluso il supporto per le visualizzazioni con sintassi Razor basata su c#. Queste viste utilizzano l'estensione di file. cshtml. Come parte del lavoro in corso per supportare Razor, la versione Beta di ASP.NET MVC 3 viene introdotto il supporto per la sintassi Razor in Visual Basic, che utilizza l'estensione di file. vbhtml.

Per un'introduzione all'uso di sintassi di Visual Basic nelle pagine VBHTML, vedere l'esercitazione all'URL seguente:

[https://www.asp.net/webmatrix/tutorials/asp-net-web-pages-visual-basic](../web-pages/overview/getting-started/introducing-razor-syntax-vb.md)

### <a id="0.1__Toc274034226"></a>Controllo più granulare ValidateInputAttribute

ASP.NET MVC è sempre incluso la classe ValidateInputAttribute, che richiama l'infrastruttura di convalida della richiesta ASP.NET core per assicurarsi che la richiesta in ingresso non contiene input potenzialmente dannosi. Per impostazione predefinita, la convalida dell'input è abilitato. È possibile disabilitare la convalida della richiesta utilizzando l'attributo ValidateInputAttribute, come nell'esempio seguente:

[!code-csharp[Main](mvc3-release-notes/samples/sample37.cs)]

Tuttavia, molte applicazioni web sono singoli campi modulo necessari per consentire di HTML, mentre i campi rimanenti non dovrebbero. La classe ValidateInputAttribute ora consente di specificare un elenco di campi che non deve essere incluso nella convalida della richiesta.

Ad esempio, se si sviluppa un motore di blog, è consigliabile consentire il markup, i campi di riepilogo e corpo. Questi campi potrebbero essere rappresentati da due elementi di input, ognuno con un nome di attributo corrispondente al nome della proprietà ("Corpo" e "Riepilogo"). Per disabilitare la richiesta di convalida per questi campi solo di specificare i nomi (delimitato da virgole) nella proprietà esclusione della classe ValidateInput, come nell'esempio seguente:

[!code-csharp[Main](mvc3-release-notes/samples/sample38.cs)]

### <a id="0.1__Toc274034227"></a>Helper per convertire i caratteri di sottolineatura in trattini per i nomi di attributo HTML specificati l'utilizzo di oggetti anonimi

Metodi helper consentono di specificare coppie nome/valore di attributo con un oggetto anonimo, come nell'esempio seguente:

[!code-csharp[Main](mvc3-release-notes/samples/sample39.cs)]

Questo approccio non è possibile utilizzare i trattini nel nome attributo, poiché un trattino non può essere utilizzato per il nome di una proprietà in ASP.NET. Tuttavia, i trattini sono importanti per gli attributi personalizzati di HTML5; ad esempio HTML5 utilizza il prefisso "data-".

Allo stesso tempo, i caratteri di sottolineatura non può essere utilizzati per i nomi di attributo in formato HTML, ma sono valide all'interno dei nomi di proprietà. Pertanto, se si specificano gli attributi con un oggetto anonimo e se i nomi di attributo includono un carattere di sottolineatura, metodi di supporto convertirà i caratteri di sottolineatura in trattini. Ad esempio, la sintassi di supporto seguente utilizza un carattere di sottolineatura:

[!code-csharp[Main](mvc3-release-notes/samples/sample40.cs)]

Nell'esempio precedente viene eseguito il rendering il markup seguente quando viene eseguito l'helper:

[!code-html[Main](mvc3-release-notes/samples/sample41.html)]

## <a id="0.1__Toc274034228"></a>Correzioni di bug

Il modello di oggetto predefinito per gli helper di modello EditorFor e DisplayFor supporta ora l'ordine specificato nella proprietà DisplayAttribute.Order. (Nelle versioni precedenti, l'impostazione dell'ordine non è stato utilizzato.)

A questo punto la convalida del client supporta la convalida di proprietà sottoposto a override con gli attributi di convalida applicati.

JsonValueProviderFactory è stato registrato per impostazione predefinita.

## <a id="0.1__Toc274034229"></a>Modifiche di rilievo

L'ordine di esecuzione per i filtri eccezioni è stato modificato per i filtri eccezioni con lo stesso valore di ordine. In ASP.NET MVC 2 e versioni precedenti, come quelli di un metodo di azione venivano eseguiti prima dei filtri eccezioni del metodo di azione nel controller con lo stesso ordine i filtri eccezioni. Nel caso potrebbe essere in genere quando i filtri eccezioni vengono applicati senza un valore nell'ordine specificato. In ASP.NET MVC 3 questo ordine è stato invertito in modo che venga eseguito per primo il gestore di eccezioni più specifico. Come nelle versioni precedenti, se è specificata in modo esplicito la proprietà Order, i filtri vengono eseguiti nell'ordine specificato.

## <a id="0.1__Toc274034230"></a>Problemi noti

Durante l'installazione, la finestra di dialogo di accettazione EULA Visualizza le condizioni di licenza in una finestra che è inferiore rispetto a quello previsto.

Visualizzazioni Razor non è necessario il supporto IntelliSense né l'evidenziazione della sintassi. Si prevede che il supporto per la sintassi Razor in Visual Studio sarà incluso come parte di una versione successiva.

Quando si modifica una vista di Razor (CSHTML file), il <a id="0.1__Toc224729061"> </a> <a id="0.1__Toc238051347"> </a> voce di menu Vai al Controller in Visual Studio non sarà disponibile, e non esistono alcun frammenti di codice.

Quando si utilizza il @model consente di visualizzare la sintassi per specificare un CSHTML fortemente tipizzata, tasti di scelta rapida specifici della lingua per i tipi non sono riconosciute. Ad esempio, @model int non funzionerà, ma @model funzionerà Int32. La soluzione alternativa per questo bug consiste nell'utilizzare il nome effettivo del tipo quando si specifica il tipo di modello.

Quando si utilizza il @model sintassi per specificare una visualizzazione fortemente tipizzata di CSHTML (o @ModelType per specificare una visualizzazione fortemente tipizzata di VBHTML), tipi nullable e dichiarazioni di matrice non sono supportate. Ad esempio, @model int? non è supportata. Utilizzare invece @model Nullable&lt;Int32&gt;. La sintassi @model string [] non è supportato neanche; utilizzare invece @model IList&lt;stringa&gt;.

Quando si aggiorna un progetto ASP.NET MVC 2 a ASP.NET MVC 3, assicurarsi di aggiungere il seguente alla sezione appSettings del file Web. config:

[!code-xml[Main](mvc3-release-notes/samples/sample42.xml)]

Si verifica un problema noto che causa l'autenticazione basata su form sempre reindirizzare gli utenti non autenticati a ~/Account/account di accesso, ignorando l'impostazione di autenticazione form utilizzata in Web. config. La soluzione alternativa consiste nell'aggiungere la seguente impostazione dell'applicazione.

[!code-xml[Main](mvc3-release-notes/samples/sample43.xml)]

## <a id="0.1__Toc274034231"></a>  Disclaimer

© 2011 Microsoft Corporation. Tutti i diritti sono riservati. Questo documento viene fornito "come-viene." Informazioni e le opinioni espresse nel presente documento, inclusi URL e altri riferimenti a siti Web possono cambiare senza preavviso. L'utente le utilizza a proprio rischio.

Questo documento non fornisce alcun diritto legale su qualsiasi proprietà intellettuale di un qualsiasi prodotto Microsoft. È possibile copiare e utilizzare questo documento per scopi personali o come riferimento.
