---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-vb
title: Differenze di configurazione comuni tra lo sviluppo e produzione (VB) | Documenti Microsoft
author: rick-anderson
description: Nelle esercitazioni precedenti sono stati distribuiti il sito Web copiando tutti i file rilevanti dall'ambiente di sviluppo all'ambiente di produzione. Tuttavia, si...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: 548e75f6-4d6c-4cb4-8da8-417915eb8393
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-vb
msc.type: authoredcontent
ms.openlocfilehash: 8de1acada8713abf5f92c1f13fa82a5d4ccc18be
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="common-configuration-differences-between-development-and-production-vb"></a>Differenze di configurazione comuni tra lo sviluppo e produzione (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica il PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial05_ConfigDifferences_vb.pdf)

> Nelle esercitazioni precedenti sono stati distribuiti il sito Web copiando tutti i file rilevanti dall'ambiente di sviluppo all'ambiente di produzione. Tuttavia, non è raro che sia le differenze di configurazione tra gli ambienti, che richiede che ogni ambiente è presente un file Web. config univoco disponibile. In questa esercitazione vengono esaminate le differenze di configurazione tipica ed esamina le strategie per mantenere le informazioni di configurazione separato.


## <a name="introduction"></a>Introduzione


Le ultime due esercitazioni esaminato in dettaglio la distribuzione di un'applicazione web semplice. Il [ *la distribuzione del sito utilizzando un FTP Client* ](deploying-your-site-using-an-ftp-client-vb.md) esercitazione viene illustrato come utilizzare un client FTP autonomo per copiare i file necessari da un ambiente di sviluppo di un massimo di produzione. L'esercitazione precedente, [ *la distribuzione del sito tramite Visual Studio*](deploying-your-site-using-visual-studio-vb.md), cercare in fase di distribuzione utilizzando l'opzione pubblica e strumento Copia sito Web di Visual Studio. In entrambe le esercitazioni tutti i file nell'ambiente di produzione è una copia di un file nell'ambiente di sviluppo. Tuttavia, non è insolito che i file di configurazione nell'ambiente di produzione sono diversi da quelli nell'ambiente di sviluppo. Configurazione di un'applicazione web viene archiviata nel `Web.config` file e in genere sono incluse informazioni sulle risorse esterne, ad esempio database, web e server di posta elettronica. Definiscono anche il comportamento dell'applicazione in determinate situazioni, ad esempio la linea di azione da intraprendere quando si verifica un'eccezione non gestita.

Quando si distribuisce un'applicazione web è importante che le informazioni di configurazione corretto finire nell'ambiente di produzione. Nella maggior parte dei casi il `Web.config` file nell'ambiente di sviluppo non può essere copiato nell'ambiente di produzione come-è. Al contrario, una versione personalizzata di `Web.config` devono essere caricati nell'ambiente di produzione. In questa esercitazione esamina brevemente alcune delle differenze di configurazione più comuni; Inoltre, riepiloga alcune tecniche per la gestione di informazioni di configurazione diverso tra gli ambienti.

## <a name="typical-configuration-differences-between-the-development-and-production-environments"></a>Configurazione tipica di differenze tra lo sviluppo e di ambienti di produzione

Il `Web.config` file include un'ampia gamma di informazioni di configurazione per un'applicazione ASP.NET. Alcune di queste informazioni di configurazione è lo stesso indipendentemente dall'ambiente. Ad esempio, le impostazioni di autenticazione e regole di autorizzazione URL dichiarato nel `Web.config` file `<authentication>` e `<authorization>` gli elementi sono in genere lo stesso indipendentemente dall'ambiente. Ma altre informazioni di configurazione, ad esempio informazioni su risorse esterne, in genere è diverso a seconda dell'ambiente.

Stringhe di connessione di database sono un esempio di informazioni di configurazione diverso in base all'ambiente. Quando un'applicazione web comunica con un server di database, innanzitutto necessario stabilire una connessione e questa operazione viene eseguita tramite un [stringa di connessione](http://www.connectionstrings.com/Articles/Show/what-is-a-connection-string). Sebbene sia possibile codificare la stringa di connessione di database direttamente in pagine web o il codice che si connette al database, è consigliabile inserirlo `Web.config`del [ `<connectionStrings>` elemento](https://msdn.microsoft.com/library/bf7sd233.aspx) in modo che la stringa di connessione informazioni sono in una singola posizione centralizzata. Spesso viene utilizzato un database diverso durante lo sviluppo rispetto a quella utilizzata in produzione. di conseguenza, le informazioni sulla stringa di connessione deve essere univoci per ogni ambiente.

> [!NOTE]
> Nelle esercitazioni successive esplorare la distribuzione di applicazioni guidate dai dati, a quel punto verranno esaminate le specifiche come stringhe di connessione di database vengono archiviati nel file di configurazione.


Il comportamento previsto di ambienti di sviluppo e produzione è sostanzialmente diverso. Un'applicazione web nell'ambiente di sviluppo vengono creata, testato e sottoposto a debug da un piccolo gruppo di sviluppatori. Nell'ambiente di produzione che stessa applicazione viene visitato da molti utenti simultanei diversi. ASP.NET include numerose funzionalità che consentono agli sviluppatori di test e debug di un'applicazione, ma queste funzionalità devono essere disabilitate per motivi di prestazioni e sicurezza quando nell'ambiente di produzione. Ecco alcuni tali impostazioni di configurazione.

### <a name="configuration-settings-that-impact-performance"></a>Impostazioni di configurazione che influiscono sulle prestazioni

Quando viene visitata una pagina ASP.NET per la prima volta (o la prima volta dopo che è stato modificato), il markup dichiarativo deve essere convertito in una classe e questa classe deve essere compilata. Se l'applicazione web utilizza la compilazione automatica classe code-behind della pagina deve essere compilato, troppo. È possibile configurare un'ampia gamma di opzioni di compilazione tramite la `Web.config` file [ `<compilation>` elemento](https://msdn.microsoft.com/library/s10awwz0.aspx).

L'attributo di debug è uno degli attributi più importanti nel `<compilation>` elemento. Se il `debug` attributo è impostato su "true", assembly compilati includere i simboli di debug, che sono necessari durante il debug di un'applicazione in Visual Studio. Tuttavia, i simboli di debug aumentare le dimensioni dell'assembly e impongano i requisiti di memoria aggiuntivi quando si esegue il codice. Inoltre, quando il `debug` attributo è impostato su "true" qualsiasi contenuto restituito dalla `WebResource.axd` non nella cache, vale a dire che ogni volta che un utente visita una pagina, dovranno scaricare nuovamente il contenuto statico restituito da `WebResource.axd`.

> [!NOTE]
> `WebResource.axd`è un gestore HTTP predefinito introdotto in ASP.NET 2.0 usato dai controlli server per recuperare le risorse incorporate, ad esempio i file di script, immagini, file CSS e altro contenuto. Per ulteriori informazioni su come `WebResource.axd` works e come è possibile usarlo per accedere alle risorse incorporate dai controlli server personalizzati, vedere [accesso incorporato risorse tramite un URL mediante `WebResource.axd` ](http://aspnet.4guysfromrolla.com/articles/080906-1.aspx).


Il `<compilation>` dell'elemento `debug` attributo viene in genere impostato su "true" nell'ambiente di sviluppo. In effetti, questo attributo deve essere impostato su "true" per eseguire il debug di un'applicazione web. Se si tenta di eseguire il debug di un'applicazione ASP.NET da Visual Studio e `debug` attributo è impostato su "false", Visual Studio verrà visualizzato un messaggio che spiega che è Impossibile eseguire il debug dell'applicazione fino a quando il `debug` attributo è impostato su "true" e verrà offerta per apportare questa modifica per l'utente.

È necessario **mai** hanno il `debug` attributo impostato su "true" in un ambiente di produzione a causa dell'impatto relativo sulle prestazioni. Per una discussione più approfondita su questo argomento, fare riferimento a [Scott Guthrie](https://weblogs.asp.net/scottgu/)del post di blog, [non eseguire produzione ASP.NET applicazioni con `debug="true"` abilitato](https://weblogs.asp.net/scottgu/442448).

### <a name="custom-errors-and-tracing"></a>Traccia e gli errori personalizzati

Quando si verifica un'eccezione non gestita in un'applicazione ASP.NET propagato il runtime a quel punto si verifica una delle tre operazioni:

- Viene visualizzato un messaggio di errore di runtime generico. Questa pagina informa l'utente che si è verificato un errore di runtime, ma non fornisce i dettagli sull'errore.
- Viene visualizzato un messaggio di eccezione dettagli, che include informazioni sull'eccezione generata solo.
- Verrà visualizzata una pagina di errore personalizzati, che è una pagina ASP.NET che crei che consente di visualizzare tutti i messaggi desiderati.

Ciò che accade in caso di un'eccezione non gestita dipende il `Web.config` file [ `<customErrors>` sezione](https://msdn.microsoft.com/library/h0hfz6fc.aspx).

Per sviluppare e testare un'applicazione è utile per visualizzare i dettagli di qualsiasi eccezione nel browser. Tuttavia, visualizzare i dettagli dell'eccezione in un'applicazione in produzione è un potenziale rischio di sicurezza. Inoltre, è unflattering e rende il sito Web un aspetto poco professionale. In teoria, in caso di un'eccezione non gestita un'applicazione web nell'ambiente di sviluppo per visualizzare i dettagli dell'eccezione durante la stessa applicazione nell'ambiente di produzione verrà visualizzati una pagina di errore personalizzato.

> [!NOTE]
> Il valore predefinito `<customErrors>` impostazione della sezione mostra i dettagli dell'eccezione dei messaggi solo quando la pagina visitata tramite localhost e viene mostrata la pagina di errore di runtime generico in caso contrario. Questo non è ideale, ma è idoneo per sapere che il comportamento predefinito non rivela i dettagli dell'eccezione per i visitatori non locali. Un'esercitazione future esamina il `<customErrors>` sezione in modo più dettagliato e viene illustrato come utilizzare una pagina di errore personalizzati visualizzata quando si verifica un errore nell'ambiente di produzione.


Un'altra funzionalità ASP.NET che è utile durante lo sviluppo è la traccia. La traccia, se abilitato, registra le informazioni relative a ogni richiesta in ingresso e fornisce una pagina web speciale, `Trace.axd`, per la visualizzazione dei dettagli della richiesta di recente. È possibile attivare e configurare la traccia tramite il [ `<trace>` elemento](https://msdn.microsoft.com/library/6915t83k.aspx) in `Web.config`.

Se si abilita analisi assicurarsi che non è disabilitato nell'ambiente di produzione. Poiché le informazioni di traccia sono inclusi i cookie, i dati della sessione e altre informazioni riservate, è importante disabilitare la traccia nell'ambiente di produzione. Buone notizie sono che, per impostazione predefinita, la traccia è disabilitata e `Trace.axd` file è accessibile solo tramite localhost. Se si modificano queste impostazioni predefinite in fase di sviluppo assicurarsi che sono state disattivate nuovamente nell'ambiente di produzione.

## <a name="techniques-for-maintaining-separate-configuration-information"></a>Tecniche per mantenere le informazioni di configurazione separato

La presenza di impostazioni di configurazione diverse in ambienti di sviluppo e produzione complica il processo di distribuzione. Nelle due esercitazioni precedenti il processo di distribuzione copiando tutti i file necessari dallo sviluppo alla produzione, ma tale approccio funziona solo se le informazioni di configurazione sono uguale in entrambi gli ambienti. Esistono diverse tecniche per la distribuzione di un'applicazione con informazioni di configurazione diversi. Di seguito catalogo alcune di queste opzioni per le applicazioni web ospitati.

### <a name="manually-deploying-the-production-environment-configuration-file"></a>Distribuzione manuale del File di configurazione dell'ambiente di produzione

L'approccio più semplice consiste nel gestire due versioni del `Web.config` file: uno per l'ambiente di sviluppo e uno per l'ambiente di produzione. Distribuzione di un sito di produzione vengono copiati tutti i file al server di produzione nell'ambiente di sviluppo *tranne* per il `Web.config` file. Al contrario, le specifiche dell'ambiente di produzione `Web.config` file verrà copiato nell'ambiente di produzione.

Questo approccio non è molto complesso, ma è facile da implementare perché le informazioni di configurazione vengono modificati raramente. Adatto per le applicazioni con un team di sviluppo di piccole dimensioni che sono ospitati in un singolo server web e le cui informazioni di configurazione raramente viene modificati. È più tenable quando si distribuisce manualmente i file dell'applicazione utilizzando un client FTP autonomo. Quando si utilizza l'opzione pubblica o strumento Copia sito Web di Visual Studio, devi innanzitutto lo swapping su specifiche della distribuzione `Web.config` file con quello di produzione specifico prima di distribuire e quindi sostituirli nuovamente dopo il completamento di distribuzione.

### <a name="change-the-configuration-during-the-build-or-deployment-process"></a>Modificare la configurazione durante la compilazione o di un processo di distribuzione

Discussioni finora sono si presuppone che un processo di compilazione e distribuzione di ad hoc. Molti progetti software di grandi dimensioni hanno formalizzato più processi che rendono l'utilizzo di open source, impegnativi, o strumenti di terze parti. Per i progetti è probabilmente possibile personalizzare il processo di compilazione o di distribuzione per modificare in modo appropriato le informazioni di configurazione prima di esso viene inserito nell'ambiente di produzione. Se si compila l'applicazione web utilizzando [MSBuild](http://en.wikipedia.org/wiki/MSBuild), [NAnt](http://nant.sourceforge.net/), o un altro strumento di compilazione, probabilmente è possibile aggiungere un'istruzione di compilazione per modificare il `Web.config` file da includere le impostazioni specifiche di produzione. O il flusso di lavoro di distribuzione a livello di codice Impossibile connettersi al server di controllo di origine e recuperare appropriata `Web.config` file.

L'approccio per ottenere le informazioni di configurazione appropriato nell'ambiente di produzione effettivo varia notevolmente a seconda gli strumenti e del flusso di lavoro. Di conseguenza, si verrà analizzano non ulteriormente in questo argomento. Se si utilizza uno strumento di compilazione più diffusi come MSBuild o NAnt è possibile trovare le esercitazioni e articoli sulla distribuzione specifiche per questi strumenti tramite una ricerca sul web.

### <a name="managing-configuration-differences-via-the-web-deployment-project-add-in"></a>La gestione delle differenze di configurazione tramite la distribuzione Web progetto componente aggiuntivo

2006 Microsoft ha rilasciato il Web sviluppo progetto di componente aggiuntivo per Visual Studio 2005. Un componente aggiuntivo per Visual Studio 2008 è stato rilasciato nel 2008. Questo componente aggiuntivo consente agli sviluppatori ASP.NET per creare un progetto di distribuzione Web separato con il progetto di applicazione web che, quando compilato, in modo esplicito viene compilata l'applicazione web e copia i file da distribuire in una directory di output locale. Il progetto di applicazione Web utilizza MSBuild dietro le quinte.

Per del impostazione predefinita, l'ambiente di sviluppo `Web.config` file viene copiato nella directory di output, ma è possibile configurare il progetto di distribuzione Web per personalizzare il

informazioni di configurazione che viene copiate in questa directory nei modi seguenti:

- Tramite `Web.config` sostituzione di sezione, in cui si specifica la sezione da sostituire e un file XML che contiene il testo di sostituzione del file.
- Fornendo un percorso di file di origine di configurazione esterno. Con questa opzione è selezionata, il progetto di distribuzione Web consente di copiare un particolare `Web.config` file nella directory di output (anziché `Web.config` file utilizzato nell'ambiente di sviluppo).
- Tramite l'aggiunta di regole personalizzate per il file MSBuild utilizzato dal progetto di distribuzione Web.

Per distribuire la compilazione di applicazione web il progetto di distribuzione Web e quindi copiare i file dalla cartella di output del progetto nell'ambiente di produzione.

Per ulteriori informazioni sull'utilizzo di progetto di distribuzione Web estrarre [questo articolo i progetti di distribuzione Web](https://msdn.microsoft.com/magazine/cc163448.aspx) dal numero di aprile 2007 di [MSDN Magazine](https://msdn.microsoft.com/magazine/default.aspx), o i collegamenti nella sezione a ulteriori informazioni, consultare il fine dell'esercitazione.

> [!NOTE]
> Poiché il progetto di distribuzione Web viene implementato come un Visual Studio aggiuntivo e Visual Studio Express Edition (inclusi Visual Web Developer) non supportano i componenti aggiuntivi, è possibile utilizzare il progetto di distribuzione Web con Visual Web Developer.


## <a name="summary"></a>Riepilogo

Le risorse esterne e il comportamento di un'applicazione web in fase di sviluppo sono in genere diversi rispetto a quando la stessa applicazione è in fase di produzione. Ad esempio, le stringhe di connessione di database, le opzioni di compilazione e il comportamento quando un'eccezione non gestita si verifica in genere differiscono tra ambienti. Il processo di distribuzione è necessario gestire queste differenze. Come descritto in questa esercitazione, l'approccio più semplice consiste nel copiare manualmente un file di configurazione alternativa nell'ambiente di produzione. Sono possibili soluzioni più efficaci quando si usa il Web distribuzione progetto di componente aggiuntivo o con un processo di compilazione o distribuzione formalizzato più in grado di soddisfare tali personalizzazioni.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Stringhe di connessione illustrate](http://www.connectionstrings.com/Articles/Show/what-is-a-connection-string)
- [@ ConnectionStrings.com le stringhe di connessione di database](http://www.connectionstrings.com/)
- [Non eseguire le applicazioni di produzione ASP.NET con `debug="true"` abilitato](https://weblogs.asp.net/scottgu/Don_1920_t-run-production-ASP.NET-Applications-with-debug_3D001D20_true_1D20_-enabled)
- [Risponde correttamente alle eccezioni non gestite - visualizzazione di pagine di errore descrittivi](http://aspnet.4guysfromrolla.com/articles/090606-1.aspx)
- [Procedura: utilizzo un progetto di distribuzione Web di Visual Studio 2008?](../../../videos/how-do-i/how-do-i-use-a-visual-studio-2008-web-deployment-project.md)
- [Impostazioni di configurazione della chiave durante la distribuzione di un Database](http://aspnet.4guysfromrolla.com/articles/121008-1.aspx)
- [Download di progetti di Visual Studio 2008 Web distribuzione](https://www.microsoft.com/downloads/details.aspx?FamilyId=0AA30AE8-C73B-4BDD-BB1B-FE697256C459&amp;displaylang=en) | [Download di progetti di Visual Studio 2005 Web distribuzione](https://download.microsoft.com/download/9/4/9/9496adc4-574e-4043-bb70-bc841e27f13c/WebDeploymentSetup.msi)
- [Progetti di distribuzione Web di Visual Studio 2008](https://weblogs.asp.net/scottgu/archive/2005/11/06/429723.aspx) | [Visual Studio 2008 Web progetto supporto per la distribuzione rilasciato](https://weblogs.asp.net/scottgu/archive/2008/01/28/vs-2008-web-deployment-project-support-released.aspx)
- [Progetti di distribuzione Web](https://msdn.microsoft.com/magazine/cc163448.aspx)

>[!div class="step-by-step"]
[Precedente](deploying-your-site-using-visual-studio-vb.md)
[Successivo](core-differences-between-iis-and-the-asp-net-development-server-vb.md)
