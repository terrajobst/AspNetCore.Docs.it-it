---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
title: Informazioni sulla localizzazione di ASP.NET AJAX | Documenti Microsoft
author: scottcate
description: Localizzazione è il processo di progettazione e l'integrazione di supporto per una lingua specifica e le impostazioni cultura in un'applicazione o un componente dell'applicazione. Il Mic...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/14/2008
ms.topic: article
ms.assetid: c1a35f18-bab9-41f7-8497-15530c37a09d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
msc.type: authoredcontent
ms.openlocfilehash: 565b0294f57b784bc592b286b3d8b28504110415
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="understanding-aspnet-ajax-localization"></a>Informazioni sulla localizzazione di AJAX ASP.NET
====================
da [Scott categorie](https://github.com/scottcate)

[Scarica il PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial04_Localization_cs.pdf)

> Localizzazione è il processo di progettazione e l'integrazione di supporto per una lingua specifica e le impostazioni cultura in un'applicazione o un componente dell'applicazione. La piattaforma Microsoft ASP.NET fornisce un supporto completo per la localizzazione delle applicazioni ASP.NET standard integrando dal modello di localizzazione .NET standard. Microsoft AJAX Framework utilizzano il modello integrato per supportare i diversi scenari in cui può essere eseguita la localizzazione.


## <a name="introduction"></a>Introduzione

Tecnologia Microsoft per ASP.NET offre un modello di programmazione orientata a oggetti e sugli eventi e si unisce i vantaggi del codice compilato. Tuttavia, il relativo modello di elaborazione sul lato server presenta numerosi svantaggi inerenti la tecnologia, molti dei quali è possibile risolvere le nuove funzionalità incluse nello spazio dei nomi Extensions, che incapsula i servizi di Microsoft AJAX in .NET Framework 3.5. Queste estensioni consentono a numerose funzionalità rich client, in precedenza disponibile come parte di ASP.NET 2.0 AJAX Extensions, ma ora parte della libreria di classi di Base di Framework. Controlli e funzionalità in questo spazio dei nomi include il rendering parziale di pagine senza richiedere un aggiornamento completo di pagina, la possibilità di accedere ai servizi Web tramite lo script client (inclusi l'API di profilatura di ASP.NET) e un'ampia API lato client è progettato per eseguire il mirroring molte delle gli schemi di controllo mostrati nel set di controllo sul lato server ASP.NET.

Questo white paper esamina la localizzazione alcune funzionalità presenti in Microsoft AJAX Framework e Script Microsoft AJAX Library, nel contesto di esigenze aziendali per il supporto di localizzazione e la revisione già integrato il supporto per la localizzazione in web applicazioni fornite da .NET Framework. La libreria Microsoft AJAX Script utilizza il formato di file resx già utilizzato da applicazioni .NET, che fornisce il supporto integrato di IDE e un tipo di risorsa condivisibile.

Questo white paper è basato sulla versione Beta 2 di Microsoft Visual Studio 2008. Questo white paper si presuppone inoltre che è necessario lavorare con Visual Studio 2008, non Visual Web Developer Express e verranno illustrate le procedure dettagliate in base all'interfaccia utente di Visual Studio. Alcuni esempi di codice utilizzano modelli di progetto potrebbero non essere disponibili in Visual Web Developer Express.

## <a name="the-need-for-localization"></a>*La necessità di localizzazione*

In particolare per gli sviluppatori di applicazioni aziendali e gli sviluppatori di componenti, la possibilità di creare strumenti che è possano tenere in considerazione le differenze tra le impostazioni cultura e lingue è diventano sempre più necessaria. La progettazione di componenti con la possibilità di adattarsi alle impostazioni locali del client aumenta la produttività degli sviluppatori e riduce la quantità di lavoro necessaria per l'adattamento di un componente per funzione a livello globale.

Localizzazione è il processo di progettazione e l'integrazione di supporto per una lingua specifica e le impostazioni cultura in un'applicazione o un componente dell'applicazione. La piattaforma Microsoft ASP.NET fornisce un supporto completo per la localizzazione delle applicazioni ASP.NET standard integrando dal modello di localizzazione .NET standard. Microsoft AJAX Framework utilizzano il modello integrato per supportare i diversi scenari in cui può essere eseguita la localizzazione. Con il Framework AJAX di Microsoft, gli script possono essere localizzati da distribuito in assembly satellite, o utilizzando una struttura di sistema di file statici.

## <a name="embedding-scripts-with-satellite-assemblies"></a>*Incorporamento di script con assembly Satellite*

È coerente con la strategia di localizzazione .NET Framework standard, le risorse possono essere inclusi in assembly satellite. Gli assembly satellite offrono numerosi vantaggi sull'inclusione di risorse tradizionale in file binari - qualsiasi dato localizzazione può essere aggiornati senza aggiornare l'immagine più grande, altri processi possono essere distribuiti semplicemente installando gli assembly satellite in la cartella di progetto e gli assembly satellite possono essere distribuito senza causare un ricaricamento dell'assembly del progetto principale. Particolarmente nei progetti ASP.NET, è utile poiché può ridurre notevolmente la quantità di risorse di sistema utilizzate per gli aggiornamenti incrementali e minima determina la chiusura dell'utilizzo del sito Web di produzione.

Gli script incorporati nell'assembly includendoli in gestite con estensione resx (o compilate con estensione resources), i file sono inclusi nell'assembly in fase di compilazione. Le relative risorse vengono quindi resi disponibili per l'applicazione di script AJAX generati dal runtime codice, tramite gli attributi a livello di assembly

*Convenzioni di denominazione per i file di Script incorporati*

La gestione di script Microsoft AJAX Framework supporta un'ampia gamma di opzioni per la distribuzione e test degli script e vengono fornite linee guida per semplificare queste opzioni.

*Per semplificare il debug:*

Gli script di versione (produzione) non devono includere il `.debug` qualificatore del nome di file. Script progettati per il debug deve includere `.debug` nel nome file.

*Per facilitare la localizzazione:*

Gli script delle impostazioni cultura neutre non devono includere un identificatore delle impostazioni cultura il nome del file. Per gli script che contengono risorse localizzate, il codice di lingua ISO deve essere specificato nel nome del file. Ad esempio, `es-CO` è l'acronimo per lo spagnolo, Columbia.

Nella tabella seguente vengono riepilogate le convenzioni di denominazione file con gli esempi:

| Nomefile | Significato |
| --- | --- |
| Script.js | Uno script di lingua della versione di rilascio. |
| Script.debug.js | Uno script di lingua della versione di debug. |
| Script.en-US.js | Versione in lingua inglese, Stati Uniti script in versione. |
| Script.debug.es-CO.js | Uno script di Columbia spagnola, versione di debug. |

## <a name="walkthrough-create-an-localized-embedded-script"></a>Procedura dettagliata: Creare uno Script incorporato, localizzato

*Nota: questa procedura dettagliata richiede l'utilizzo di Visual Studio 2008 come Visual Web Developer Express Edition non include un modello di progetto per i progetti libreria di classi.*

1. Creare un nuovo progetto di sito Web con ASP.NET AJAX Extensions integrato. Creare un altro progetto, un progetto libreria di classi all'interno della soluzione LocalizingResources chiamato.
2. Aggiungere un file Jscript denominato VerifyDeletion.js al progetto LocalizingResources, nonché i file di risorse con estensione resx denominati DeletionResources.resx e DeletionResources.es.resx. Il primo conterrà le risorse indipendenti dalla lingua. quest'ultimo conterrà le risorse di lingua spagnola.
3. Aggiungere il codice seguente per VerifyDeletion.js:

[!code-javascript[Main](understanding-asp-net-ajax-localization/samples/sample1.js)]

Per coloro che non si ha dimestichezza con sintassi JavaScript Regex, il testo all'interno delle barre singole (nell'esempio precedente, /FILENAME/ è riportato un esempio) indica un oggetto RegExp. MSDN Library contiene un riferimento a JavaScript completo e le risorse sugli oggetti nativi di JavaScript sono disponibili online.

1. Aggiungere le seguenti stringhe di risorse a DeletionResources.resx: 

    **VerifyDelete**: è certi che si desidera eliminare FILENAME?

    **Eliminato**: nome del file è stato eliminato.

1. Aggiungere le seguenti stringhe di risorse a DeletionResources.es.resx: 

    **VerifyDelete**: Est seguro que desee quitar FILENAME?

    **Eliminato**: FILENAME sa a disponibilità elevata quitado.
2. Aggiungere le righe di codice seguente al file AssemblyInfo:

[!code-csharp[Main](understanding-asp-net-ajax-localization/samples/sample2.cs)]

1. Aggiungere riferimenti a System. Web ed Extensions al progetto LocalizingResources.
2. Aggiungere un riferimento al progetto LocalizingResources dal progetto di sito Web.
3. In default.aspx, sotto il progetto di sito Web, aggiornare il controllo ScriptManager con il markup aggiuntivo seguente:

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample3.aspx)]

1. In default.aspx, in qualsiasi punto della pagina, includere questo codice:

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample4.aspx)]

1. Premere F5. Se richiesto, attivare il debug. Quando viene caricata la pagina, fare clic sul pulsante Elimina. Si noti che richiesto in inglese (a meno che il computer è quella di preferire le risorse di lingua spagnola per impostazione predefinita) per conferma.
2. Chiudere la finestra del browser e tornare a default.aspx. Nel @Page direttiva di intestazione, sostituzione automatica per Culture e UICulture con es-ES. Premere F5 per avviare nuovamente l'applicazione web nel browser. Questa volta, si noti che viene chiesto di eliminare il file in spagnolo:


[![](understanding-asp-net-ajax-localization/_static/image2.png)](understanding-asp-net-ajax-localization/_static/image1.png)

([Fare clic per visualizzare l'immagine ingrandita](understanding-asp-net-ajax-localization/_static/image3.png))


[![](understanding-asp-net-ajax-localization/_static/image5.png)](understanding-asp-net-ajax-localization/_static/image4.png)

([Fare clic per visualizzare l'immagine ingrandita](understanding-asp-net-ajax-localization/_static/image6.png))


Si noti che esistono diverse varianti per questa procedura dettagliata. Ad esempio, gli script potrebbero essere registrati con il controllo ScriptManager a livello di codice durante il caricamento di pagina.

## <a name="including-a-static-script-file-structure"></a>*Tra cui una struttura di File Script statico*

Quando si utilizza file di script statici per la distribuzione, si perdono alcuni dei vantaggi dell'utilizzo lo schema di localizzazione .NET inerente. È principalmente visibile che si perde il tipo automatico generato, compresi file di risorse di script. Nella procedura dettagliata precedente, ad esempio, le risorse sono stati esposti da un tipo generato automaticamente chiamato messaggio dal controllo ScriptManager.

Esistono tuttavia alcuni vantaggi derivanti dall'utilizzo di una struttura di file di script statico. Gli aggiornamenti possono essere eseguiti senza ricompilare e ridistribuire gli assembly satellite e l'utilizzo di una struttura di file statici può essere eseguita anche per eseguire l'override di script incorporati, per integrare una parte minore di funzionalità che potrebbe non essere stata fornita con un componente.

Si consiglia di evitare un problema di controllo della versione generando automaticamente le risorse di script durante la compilazione del progetto. Quando si gestisce un codice di script completa base, può diventare sempre più difficile da assicurare che le modifiche al codice vengono riflesse in ogni script localizzati. In alternativa, si consente semplicemente di mantenere uno script di logica e più script di localizzazione, unione dei file durante la compilazione del progetto.

Poiché non sono disponibili risorse da includere in modo dichiarativo, script statico file devono essere fatto riferimento tramite l'aggiunta di `<asp:ScriptElement>` elementi come elemento figlio di `<Scripts>` tag del controllo ScriptManager, o a livello di codice aggiungere `ScriptReference` oggetti per il `Scripts` proprietà del `ScriptManager` controllo nella pagina in fase di esecuzione.

## <a name="the-scriptmanager-and-its-role-in-localization"></a>*ScriptManager e il relativo ruolo nella localizzazione*

ScriptManager consente diverse funzionalità automatiche per le applicazioni localizzate:

- Individua automaticamente i file di script in base alle impostazioni e le convenzioni di denominazione; ad esempio, carica attivato il debug di script in modalità debug e carica localizzata script in base alla selezione di interfaccia utente del browser.
- Consente la definizione delle impostazioni cultura, tra cui impostazioni cultura personalizzate.
- Consente la compressione dei file di script tramite HTTP.
- Memorizza nella cache di script per gestire in modo efficiente un numero di richieste.
- Aggiunge un livello di riferimento indiretto script eseguendo il piping di loro tramite un URL crittografato.

Riferimenti a script può essere aggiunto al controllo ScriptManager a livello di codice o da codice dichiarativo. Markup dichiarativo è particolarmente utile quando si utilizzano script incorporato nell'assembly diverso dal progetto di sito web stesso, come il nome dello script probabilmente non cambierà quando vengono inviate revisioni.

## <a name="summary"></a>Riepilogo

Applicazioni web con l'aumentare per raggiungere un numero elevato di destinatari, la necessità di essere in grado di raggiungere più ampia di impostazioni cultura e le community diventa principale di un modello di business. le applicazioni web di e-commerce devono essere in grado di gestire le valute, sistemi di gestione dei contenuti devono essere presenti in grado non solo il contenuto ma i relativi parametri di spostamento e i campi del form in altri linguaggi e le aziende devono sapere che questa esigenza accessibile.

.NET Framework supporta intrinsecamente un framework di localizzazione avanzata che utilizzano gli assembly satellite e file di risorse (resx) XML per presentare un metodo uniforme per cercare le immagini e stringhe di risorse. Estensioni AJAX di ASP.NET, tra cui Microsoft AJAX Framework e Script Microsoft AJAX Library, forniscono supporto per questo modello di programmazione nel codice sul lato client, abilitare le ricerche di stringhe di risorse semplice. Gli assembly satellite supportano l'inclusione automatica delle risorse di script (file con estensione js effettivo) attraverso ScriptResource, purché i nomi di file seguono uno schema di denominazione specificato. Con questo supporto, ASP.NET AJAX Extensions semplificano la localizzazione di script e globalizzazione di applicazioni.

## <a name="bio"></a>*Bio*

Categoria Scott lavora con tecnologie Web di Microsoft dal 1997 ed è il vicepresidente myKB.com ([www.myKB.com](http://www.myKB.com)) in cui si è specializzato nella scrittura ASP.NET basato su applicazioni con stato attivo sulle soluzioni Software Knowledge Base. Scott possano essere contattati tramite posta elettronica al [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) o il suo blog all'indirizzo [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Precedente](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
> [Successivo](understanding-asp-net-ajax-web-services.md)
