---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
title: Informazioni sulla localizzazione in ASP.NET AJAX | Microsoft Docs
author: scottcate
description: La localizzazione è il processo di progettazione e l'integrazione di supporto per una lingua specifica e le impostazioni cultura in un'applicazione o un componente dell'applicazione. Il Mic...
ms.author: aspnetcontent
ms.date: 03/14/2008
ms.assetid: c1a35f18-bab9-41f7-8497-15530c37a09d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
msc.type: authoredcontent
ms.openlocfilehash: ce6404ce4faa1018a4f8118f6167a4f93956abd3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37815007"
---
<a name="understanding-aspnet-ajax-localization"></a>Informazioni sulla localizzazione in ASP.NET AJAX
====================
da [Scott Cate](https://github.com/scottcate)

[Scaricare PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial04_Localization_cs.pdf)

> La localizzazione è il processo di progettazione e l'integrazione di supporto per una lingua specifica e le impostazioni cultura in un'applicazione o un componente dell'applicazione. La piattaforma Microsoft ASP.NET offre ampio supporto per la localizzazione per le applicazioni ASP.NET standard grazie all'integrazione dal modello di localizzazione .NET standard. Il Framework AJAX di Microsoft utilizzano il modello integrato per supportare i diversi scenari in cui può essere eseguita la localizzazione.


## <a name="introduction"></a>Introduzione

Tecnologia Microsoft per ASP.NET offre un modello di programmazione orientate a oggetti e guidati da eventi e si unisce i vantaggi del codice compilato. Tuttavia, il relativo modello di elaborazione sul lato server presenta numerosi svantaggi intrinseci della tecnologia, molti dei quali è possibile risolvere le nuove funzionalità incluse nello spazio dei nomi Extensions, che incapsula i servizi AJAX di Microsoft in .NET Framework 3.5. Queste estensioni consentono molte funzionalità rich client, in precedenza disponibili come parte di ASP.NET 2.0 AJAX Extensions, ma ora parte della libreria di classi Base Framework. Controlli e funzionalità in questo spazio dei nomi includono il rendering parziale delle pagine senza richiedere un aggiornamento completo della pagina, la possibilità di accedere ai servizi Web tramite lo script client (inclusa l'API di profilatura ASP.NET) e un'API lato client estesa progettati per eseguire il mirroring di molte delle gli schemi di controllo visualizzati nell'insieme di controlli lato server ASP.NET.

Questo white paper vengono esaminate le funzionalità di localizzazione presenti in Microsoft AJAX Framework e Script Microsoft AJAX Library, nel contesto di supporto di localizzazione e la revisione già integrato il supporto per la localizzazione in web in ambito aziendale applicazioni fornite da .NET Framework. La libreria di Script AJAX di Microsoft utilizza il formato di file resx già usato da applicazioni .NET, che fornisce supporto IDE integrato e un tipo di risorsa condivisibile.

Questo white paper si basa sulla versione di Microsoft Visual Studio 2008 Beta 2. Questo white paper si presuppone inoltre che si userà Visual Studio 2008, non Visual Web Developer Express e forniscono procedure dettagliate in base all'interfaccia utente di Visual Studio. Alcuni esempi di codice utilizzeranno i modelli di progetto che potrebbero non essere disponibili in Visual Web Developer Express.

## <a name="the-need-for-localization"></a>*La necessità di localizzazione*

In particolare per gli sviluppatori di applicazioni aziendali e gli sviluppatori di componenti, la possibilità di creare strumenti che possono essere consapevoli delle differenze tra lingue e culture è diventata sempre più necessaria. Progettazione di componenti con la possibilità di adattare le impostazioni locali del client aumenta la produttività degli sviluppatori e riduce la quantità di lavoro richiesto per l'adattamento di un componente di funzionare a livello globale.

La localizzazione è il processo di progettazione e l'integrazione di supporto per una lingua specifica e le impostazioni cultura in un'applicazione o un componente dell'applicazione. La piattaforma Microsoft ASP.NET offre ampio supporto per la localizzazione per le applicazioni ASP.NET standard grazie all'integrazione dal modello di localizzazione .NET standard. Il Framework AJAX di Microsoft utilizzano il modello integrato per supportare i diversi scenari in cui può essere eseguita la localizzazione. Con il Framework Microsoft AJAX, gli script possono essere localizzati da distribuire in assembly satellite, o utilizzando una struttura di sistema dei file statici.

## <a name="embedding-scripts-with-satellite-assemblies"></a>*Incorporamento di script con assembly Satellite*

Coerente con la strategia di localizzazione di .NET Framework standard, le risorse possono essere inclusi in assembly satellite. Gli assembly satellite offrono diversi vantaggi sull'inclusione di risorse tradizionali in file binari - qualsiasi localizzazione specificato può essere aggiornati senza aggiornare l'immagine più grande, localizzazioni aggiuntivi possono essere distribuiti installando semplicemente gli assembly satellite in la cartella del progetto e gli assembly satellite possono essere distribuiti senza causare un ricaricamento dell'assembly principale del progetto. In particolare nei progetti ASP.NET, ciò è utile poiché può ridurre notevolmente la quantità di risorse di sistema utilizzate per gli aggiornamenti incrementali e minima dovuta a scopi di sito Web di produzione.

Gli script vengono incorporati nell'assembly includendoli in gestite con estensione resx (o compilate con estensione resources), i file sono inclusi nell'assembly in fase di compilazione. Le relative risorse vengono quindi resi disponibili per l'applicazione di script tramite AJAX generati dal runtime codice, tramite gli attributi a livello di assembly

*Convenzioni di denominazione per i file di Script incorporati*

La gestione di script Microsoft AJAX Framework supporta un'ampia gamma di opzioni per l'utilizzo nella distribuzione e test degli script e vengono fornite indicazioni generali per agevolare queste opzioni.

*Per facilitare il debug:*

Gli script di rilascio (produzione) non devono includere il `.debug` il qualificatore del nome file. Gli script progettati per il debug deve includere `.debug` nel nome file.

*Per facilitare la localizzazione:*

Impostazioni cultura neutre script non deve includere un identificatore delle impostazioni cultura il nome del file. Per gli script che contengono risorse localizzate, è necessario specificare il codice di lingua ISO nel nome del file. Ad esempio, `es-CO` è l'acronimo per lo spagnolo, Columbia.

La tabella seguente riepiloga il file con esempi di convenzioni di denominazione:

| Nomefile | Significato |
| --- | --- |
| Script.js | Uno script indipendenti dalla lingua della versione di rilascio. |
| Script.debug.js | Uno script indipendenti dalla lingua della versione di debug. |
| Script.en-US.js | Versione in lingua inglese, Stati Uniti script in versione. |
| Script.debug.es-CO.js | Uno script di Columbia spagnola, versione di debug. |

## <a name="walkthrough-create-an-localized-embedded-script"></a>Procedura dettagliata: Creare uno Script incorporato, localizzato

*Nota: questa procedura dettagliata richiede l'uso di Visual Studio 2008, Visual Web Developer Express include un modello di progetto per i progetti libreria di classi.*

1. Creare un nuovo progetto di sito Web con ASP.NET AJAX Extensions integrato. Creare un altro progetto, un progetto libreria di classi, all'interno della soluzione denominata LocalizingResources.
2. Aggiungere un file Jscript denominato VerifyDeletion.js al progetto LocalizingResources, nonché i file di risorse RESX denominati DeletionResources.resx e DeletionResources.es.resx. Nel primo caso conterrà risorse indipendente dalle impostazioni cultura. quest'ultimo conterrà le risorse di lingua spagnola.
3. Aggiungere il codice seguente al VerifyDeletion.js:

[!code-javascript[Main](understanding-asp-net-ajax-localization/samples/sample1.js)]

Per chi ha familiarità con la sintassi JavaScript Regex, testo all'interno di singole barre (nell'esempio precedente, /FILENAME/ è riportato un esempio) indica un oggetto RegExp. MSDN Library contiene un riferimento di JavaScript completo e risorse per gli oggetti nativi di JavaScript sono disponibili online.

1. Aggiungere le seguenti stringhe di risorse a DeletionResources.resx: 

    **VerifyDelete**: si è sicuri di voler eliminare il nome del file?

    **Eliminato**: nome del file è stato eliminato.

1. Aggiungere le seguenti stringhe di risorse a DeletionResources.es.resx: 

    **VerifyDelete**: Est seguro que desee quitar FILENAME?

    **Eliminato**: FILENAME sa a disponibilità elevata quitado.
2. Aggiungere le righe di codice seguenti al file AssemblyInfo:

[!code-csharp[Main](understanding-asp-net-ajax-localization/samples/sample2.cs)]

1. Aggiungere riferimenti a System. Web ed Extensions al progetto LocalizingResources.
2. Aggiungere un riferimento al progetto LocalizingResources dal progetto di sito Web.
3. In default. aspx, sotto il progetto di sito Web, aggiornare il controllo ScriptManager con il markup aggiuntivo seguente:

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample3.aspx)]

1. In default. aspx, in un punto qualsiasi della pagina, includere questo markup:

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample4.aspx)]

1. Premere F5. Se richiesto, abilita il debug. Quando la pagina viene caricata, premere il pulsante di eliminazione. Si noti che richiesta in lingua inglese (a meno che il computer è impostato per preferire le risorse di lingua spagnola per impostazione predefinita) di conferma.
2. Chiudere la finestra del browser e tornare a default. aspx. Nel @Page direttiva di intestazione, sostituire auto per Culture e UICulture con es-ES. Premere nuovamente F5 per avviare nuovamente l'applicazione web nel browser. Questa volta, si noti che viene richiesto di eliminare il file in spagnolo:


[![](understanding-asp-net-ajax-localization/_static/image2.png)](understanding-asp-net-ajax-localization/_static/image1.png)

([Fare clic per visualizzare l'immagine con dimensioni normali](understanding-asp-net-ajax-localization/_static/image3.png))


[![](understanding-asp-net-ajax-localization/_static/image5.png)](understanding-asp-net-ajax-localization/_static/image4.png)

([Fare clic per visualizzare l'immagine con dimensioni normali](understanding-asp-net-ajax-localization/_static/image6.png))


Si noti che esistono diverse varianti per questa procedura dettagliata. Ad esempio, gli script è stato possibile registrare con il controllo ScriptManager a livello di codice durante il caricamento della pagina.

## <a name="including-a-static-script-file-structure"></a>*Tra cui una struttura di File di Script statici*

Quando si usano file di script statici per la distribuzione, si perde alcuni dei vantaggi dell'uso lo schema di localizzazione .NET inerente. È principalmente visibile che si perdita gli automatica del tipo generata dall'inclusione di file di risorse di script; Nella procedura dettagliata precedente, ad esempio, le risorse sono state esposte da un tipo generato automaticamente e chiamato messaggio dal controllo ScriptManager.

Esistono tuttavia alcuni vantaggi dell'uso di una struttura di file di script statici. Gli aggiornamenti possono essere eseguiti senza ricompilare e ridistribuire gli assembly satellite e l'uso di una struttura di file statici può essere eseguita anche per eseguire l'override dello script incorporato, per integrare una parte minore di funzionalità che potrebbe non essere stata fornita con un componente.

Microsoft consiglia di evitare un problema di controllo della versione generando automaticamente le risorse di script durante la compilazione del progetto. Quando si gestisce un codice di script ampia base, può diventare sempre più difficile garantire che le modifiche al codice vengono riflesse in ogni script localizzati. In alternativa, è possibile gestire semplicemente uno script per la logica e più script di localizzazione, unione dei file durante la compilazione del progetto.

Perché non sono presenti risorse da includere in modo dichiarativo, script statici devono essere file di cui viene fatto riferimento tramite l'aggiunta `<asp:ScriptElement>` elementi come elemento figlio di `<Scripts>` tag del controllo ScriptManager, o a livello di codice aggiungere `ScriptReference` oggetti per il `Scripts` proprietà del `ScriptManager` controllo nella pagina in fase di esecuzione.

## <a name="the-scriptmanager-and-its-role-in-localization"></a>*ScriptManager e il suo ruolo nell'ambito della localizzazione*

ScriptManager consente diversi comportamenti automatici per le applicazioni localizzate:

- Individua automaticamente i file di script basati sulle impostazioni e le convenzioni di denominazione; ad esempio, carica attivato il debug degli script in modalità di debug e caricamenti localizzato script basati sulla selezione dell'interfaccia utente del browser.
- Consente la definizione delle impostazioni cultura, tra cui impostazioni cultura personalizzate.
- Consente la compressione dei file di script tramite HTTP.
- Memorizza nella cache gli script per gestire in modo efficiente un numero di richieste.
- Aggiunge un livello di riferimento indiretto agli script inviando loro tramite un URL crittografato.

Riferimenti a script possono essere aggiunti al controllo ScriptManager in a livello di codice o markup dichiarativo. Markup dichiarativo è particolarmente utile quando si lavora con gli script incorporati nell'assembly diverso dal progetto sito web stesso, come il nome dello script verrà probabilmente non modificato come le revisioni vengono passate.

## <a name="summary"></a>Riepilogo

Man mano che aumentano le applicazioni web per raggiungere un pubblico più ampio, la necessità di essere in grado di raggiungere le impostazioni cultura e le community più ampi diventa principale di un modello di business. le applicazioni web di e-commerce devono essere in grado di gestire con valute, sistemi di gestione dei contenuti devono essere presenti in grado non solo il relativo contenuto, ma anche loro hint per la navigazione e i campi del modulo in altre lingue e le aziende devono sapere che questa esigenza accessibile.

.NET Framework supporta intrinsecamente un framework di localizzazione completa, che usano gli assembly satellite e file XML (resx) per presentare un metodo uniforme per cercare immagini e stringhe di risorse. ASP.NET AJAX Extensions, tra cui il Framework AJAX di Microsoft e Script Microsoft AJAX Library, forniscono supporto per questo modello di programmazione nel codice del lato client, abilitare le ricerche di stringhe di risorse semplice. Gli assembly satellite supportano l'inserimento automatico delle risorse di script (file con estensione js effettivo) tramite ScriptResource. axd, purché i nomi di file seguono uno schema di denominazione specifico. Con questo supporto, ASP.NET AJAX Extensions semplifica la localizzazione degli script e la globalizzazione di applicazioni.

## <a name="bio"></a>*Bio*

Scott Cate ha collaborato con tecnologie Web di Microsoft dal 1997 ed è il presidente di myKB.com ([www.myKB.com](http://www.myKB.com)) ed è specializzato nella scrittura di ASP.NET basato su applicazioni incentrate su soluzioni Software di Knowledge Base. È possibile contattare Scott tramite posta elettronica all'indirizzo [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) o il suo blog all'indirizzo [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Precedente](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
> [Successivo](understanding-asp-net-ajax-web-services.md)
