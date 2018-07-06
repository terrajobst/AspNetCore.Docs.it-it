---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-cs
title: Differenze di configurazione più comuni tra sviluppo e produzione (c#) | Microsoft Docs
author: rick-anderson
description: Nelle esercitazioni precedenti è stato distribuito il nostro sito Web tramite la copia di tutti i file pertinenti dall'ambiente di sviluppo all'ambiente di produzione. Tuttavia, ho...
ms.author: aspnetcontent
ms.date: 04/01/2009
ms.assetid: 721a5c37-7e21-48e0-832e-535c6351dcae
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-cs
msc.type: authoredcontent
ms.openlocfilehash: 5b3476cdb25a0fc87ce233d7e26de094a0b7b17c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808908"
---
<a name="common-configuration-differences-between-development-and-production-c"></a>Differenze di configurazione più comuni tra sviluppo e produzione (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial05_ConfigDifferences_cs.pdf)

> Nelle esercitazioni precedenti è stato distribuito il nostro sito Web tramite la copia di tutti i file pertinenti dall'ambiente di sviluppo all'ambiente di produzione. Non è, tuttavia, non comune per essere differenze di configurazione tra gli ambienti, che richiede che ogni ambiente dispone di un file Web. config univoco. Questa esercitazione esamina le differenze di configurazione tipici ed esamina le strategie per la gestione delle informazioni di configurazione separato.


## <a name="introduction"></a>Introduzione


Le ultime due esercitazioni esaminato la distribuzione di una semplice applicazione web. Il [ *distribuzione del sito tramite un FTP Client* ](deploying-your-site-using-an-ftp-client-cs.md) esercitazione ha illustrato come usare un client FTP autonoma per copiare i file necessari dall'ambiente di sviluppo fino a ambiente di produzione. L'esercitazione precedente [ *distribuzione del sito tramite Visual Studio*](deploying-your-site-using-visual-studio-cs.md), cercare in fase di distribuzione usando l'opzione di pubblicazione e lo strumento Copia sito Web di Visual Studio. In entrambe le esercitazioni ogni file nell'ambiente di produzione è una copia di un file nell'ambiente di sviluppo. Non è, tuttavia, non comune per i file di configurazione nell'ambiente di produzione in modo diverso rispetto a quelli nell'ambiente di sviluppo. Configurazione di un'applicazione web viene archiviata nel `Web.config` file e in genere sono incluse informazioni sulle risorse esterne, ad esempio database, web e server di posta elettronica. Inoltre illustra in dettaglio il comportamento dell'applicazione in determinate situazioni, ad esempio la linea di azione da intraprendere quando si verifica un'eccezione non gestita.

Quando si distribuisce un'applicazione web è importante che le informazioni di configurazione corretto finire nell'ambiente di produzione. Nella maggior parte dei casi il `Web.config` non è possibile copiare file nell'ambiente di sviluppo all'ambiente di produzione come-è. Al contrario, una versione personalizzata di `Web.config` dovrà essere caricato nell'ambiente di produzione. Questa esercitazione esamina brevemente alcune delle differenze di configurazione più comuni; Inoltre, riepiloga alcune tecniche per la gestione delle informazioni di configurazione diverso tra gli ambienti.

## <a name="typical-configuration-differences-between-the-development-and-production-environments"></a>Configurazione tipica differenze tra lo sviluppo e ambienti di produzione

Il `Web.config` file include un'ampia gamma di informazioni di configurazione per un'applicazione ASP.NET. Alcune di queste informazioni di configurazione è lo stesso indipendentemente dall'ambiente. Ad esempio, le impostazioni di autenticazione e regole di autorizzazione URL esplicitate il `Web.config` del file `<authentication>` e `<authorization>` elementi sono in genere gli stessi indipendentemente dall'ambiente. Ma altre informazioni di configurazione - ad esempio informazioni sulle risorse esterne, in genere variano a seconda dell'ambiente.

Stringhe di connessione del database sono un ottimo esempio delle informazioni di configurazione diverso in base all'ambiente. Quando un'applicazione web comunica con un server di database prima di tutto è necessario stabilire una connessione e questa operazione viene eseguita tramite un [stringa di connessione](http://www.connectionstrings.com/Articles/Show/what-is-a-connection-string). Sebbene sia possibile codificare la stringa di connessione di database direttamente in pagine web o il codice che si connette al database, è consigliabile inserirlo `Web.config`del [ `<connectionStrings>` elemento](https://msdn.microsoft.com/library/bf7sd233.aspx) in modo che la stringa di connessione informazioni sono in un'unica posizione centralizzata. Spesso durante lo sviluppo viene utilizzato un database diverso rispetto a quello utilizzato nell'ambiente di produzione; di conseguenza, le informazioni sulla stringa di connessione deve essere univoci per ogni ambiente.

> [!NOTE]
> Nelle esercitazioni successive esplorare la distribuzione di applicazioni guidate dai dati, a questo punto verranno esaminate le specifiche come stringhe di connessione di database vengono archiviati nel file di configurazione.


Il comportamento previsto di ambienti di sviluppo e produzione differisce notevolmente. Un'applicazione web nell'ambiente di sviluppo viene creata, testati e sottoposto a debug da un piccolo gruppo di sviluppatori. Nell'ambiente di produzione stessa applicazione viene visitato da numerosi utenti simultanei. ASP.NET include numerose funzionalità che consentono agli sviluppatori di test e debug di un'applicazione, ma queste funzionalità devono essere disabilitate per motivi di prestazioni e sicurezza quando nell'ambiente di produzione. Esaminiamo alcuni tali impostazioni di configurazione.

### <a name="configuration-settings-that-impact-performance"></a>Impostazioni di configurazione che influiscono sulle prestazioni

Quando viene visitata una pagina ASP.NET per la prima volta (o la prima volta dopo la modifica), il relativo markup dichiarativo devono essere convertite in una classe e questa classe deve essere compilata. Se l'applicazione web Usa la compilazione automatica classe code-behind della pagina deve quindi essere compilato, troppo. È possibile configurare un'ampia gamma di opzioni di compilazione tramite il `Web.config` del file [ `<compilation>` elemento](https://msdn.microsoft.com/library/s10awwz0.aspx).

L'attributo di debug è uno degli attributi più importanti nel `<compilation>` elemento. Se il `debug` attributo è impostato su "true", assembly compilati includono i simboli di debug, che sono necessari durante il debug di un'applicazione in Visual Studio. Tuttavia, i simboli di debug aumentare le dimensioni dell'assembly e impongano requisiti di memoria aggiuntiva durante l'esecuzione di codice. Inoltre, quando la `debug` attributo è impostato su "true" qualsiasi contenuto restituito da `WebResource.axd` non viene memorizzato nella cache, di vale a dire che ogni volta che un utente visita una pagina dovranno scaricare nuovamente il contenuto statico restituito da `WebResource.axd`.

> [!NOTE]
> `WebResource.axd` è un gestore HTTP predefinito introdotto in ASP.NET 2.0 utilizzato dai controlli server per recuperare le risorse incorporate, ad esempio i file di script, immagini, file CSS e altro contenuto. Per altre informazioni su come `WebResource.axd` funziona e come è possibile usarlo per accedere alle risorse incorporate dai controlli server personalizzati, vedere [l'accesso a incorporate risorse tramite un URL usando `WebResource.axd` ](http://aspnet.4guysfromrolla.com/articles/080906-1.aspx).


Il `<compilation>` dell'elemento `debug` attributo viene in genere impostato su "true" nell'ambiente di sviluppo. In effetti, questo attributo deve essere impostato su "true" per eseguire il debug di un'applicazione web. Se si prova a eseguire il debug di un'applicazione ASP.NET da Visual Studio e il `debug` attributo è impostato su "false", Visual Studio verrà visualizzato un messaggio che spiega che l'applicazione non è possibile eseguire il debug finché il `debug` attributo è impostato su "true" e verrà l'offerta per apportare questa modifica per l'utente.

È consigliabile **mai** dispone i `debug` attributo impostato su "true" in un ambiente di produzione a causa dell'impatto relativo sulle prestazioni. Per una discussione più approfondita su questo argomento, consultare [Scott Guthrie](https://weblogs.asp.net/scottgu/)del post di blog [non eseguire produzione ASP.NET le applicazioni con `debug="true"` Enabled](https://weblogs.asp.net/scottgu/442448).

### <a name="custom-errors-and-tracing"></a>Traccia e gli errori personalizzati

Quando si verifica un'eccezione non gestita in un'applicazione ASP.NET propagato al runtime a questo punto si verifica una delle tre operazioni:

- Viene visualizzato un messaggio di errore di runtime generico. Questa pagina informa l'utente che si è verificato un errore di runtime, ma non fornisce i dettagli sull'errore.
- Viene visualizzato un messaggio di dettagli di eccezione, che include informazioni sull'eccezione appena generata.
- Viene visualizzata una pagina di errore personalizzato, ovvero una pagina ASP.NET creato che consente di visualizzare tutti i messaggi desiderati.

Ciò che accade in caso di un'eccezione non gestita dipende il `Web.config` del file [ `<customErrors>` sezione](https://msdn.microsoft.com/library/h0hfz6fc.aspx).

Durante lo sviluppo e test di un'applicazione che è possibile per visualizzare i dettagli di qualsiasi eccezione nel browser. Tuttavia, che mostra i dettagli dell'eccezione in un'applicazione in produzione è un potenziale rischio di sicurezza. Inoltre, è unflattering e conferisce un aspetto poco professionale il sito Web. Idealmente, in caso di un'eccezione non gestita un'applicazione web nell'ambiente di sviluppo per visualizzare i dettagli dell'eccezione durante la stessa applicazione nell'ambiente di produzione mostrerà una pagina di errore personalizzato.

> [!NOTE]
> Il valore predefinito `<customErrors>` impostazione sezione mostra i dettagli dell'eccezione dei messaggi solo quando la pagina viene visitata tramite localhost e viene illustrata la pagina di errore di runtime generico in caso contrario. Questo comportamento non è ideale, ma è in modo da assicurare per sapere che il comportamento predefinito non rivela i dettagli dell'eccezione per i visitatori non locali. Un'esercitazione futura esamina il `<customErrors>` sezione in modo più dettagliato e viene mostrato come configurare una pagina di errore personalizzati visualizzata quando si verifica un errore nell'ambiente di produzione.


Un'altra funzionalità ASP.NET che risulta utile durante lo sviluppo è la traccia. Traccia, se abilitata, registra le informazioni relative a ogni richiesta in ingresso e fornisce una pagina web speciale, `Trace.axd`, per la visualizzazione dei dettagli delle richieste recenti. È possibile attivare e configurare la traccia tramite il [ `<trace>` elemento](https://msdn.microsoft.com/library/6915t83k.aspx) in `Web.config`.

Se si abilita traccia assicurarsi che l'it è disabilitato nell'ambiente di produzione. Poiché le informazioni di traccia sono inclusi i cookie, i dati della sessione e altre informazioni potenzialmente riservate, è importante disabilitare la traccia nell'ambiente di produzione. La buona notizia è che, per impostazione predefinita, viene disabilitata la traccia e il `Trace.axd` file è accessibile solo tramite localhost. Se si modificano queste impostazioni predefinite in fase di sviluppo assicurarsi che sono spenti nuovamente nell'ambiente di produzione.

## <a name="techniques-for-maintaining-separate-configuration-information"></a>Tecniche per la gestione delle informazioni di configurazione separato

La presenza di impostazioni di configurazione diverse in ambienti di sviluppo e produzione complica il processo di distribuzione. Nelle due esercitazioni precedenti il processo di distribuzione coinvolta la copia di tutti i file necessari dallo sviluppo alla produzione, ma tale approccio funziona solo se le informazioni di configurazione sono lo stesso in entrambi gli ambienti. Esistono varie tecniche per la distribuzione di un'applicazione con informazioni di configurazione diverse. Verrà ora catalogo alcune di queste opzioni per le applicazioni web ospitate.

### <a name="manually-deploying-the-production-environment-configuration-file"></a>Distribuzione manuale di File di configurazione dell'ambiente di produzione

L'approccio più semplice consiste nel mantenere due versioni del `Web.config` file: uno per l'ambiente di sviluppo e uno per l'ambiente di produzione. Distribuire un sito di produzione vengono copiati tutti i file al server di produzione nell'ambiente di sviluppo *eccetto* per il `Web.config` file. Al contrario, le specifiche dell'ambiente produzione `Web.config` file verrà copiato nell'ambiente di produzione.

Questo approccio non sia molto sofisticato, ma è facile da implementare poiché le informazioni di configurazione non vengono modificati spesso. Funziona al meglio per le applicazioni con un team di sviluppo di piccole dimensioni che sono ospitati in un singolo server web e le cui informazioni di configurazione viene modificati raramente. È più tenable quando si distribuisce manualmente i file dell'applicazione tramite un client FTP autonome. Quando si usa l'opzione pubblica o lo strumento Copia sito Web di Visual Studio, devi innanzitutto lo swapping delle specifiche della distribuzione `Web.config` con quello specifico ambiente di produzione prima della distribuzione di file e quindi sostituirli nuovamente dopo aver completato la distribuzione.

### <a name="change-the-configuration-during-the-build-or-deployment-process"></a>Modificare la configurazione durante la compilazione o di un processo di distribuzione

Le discussioni finora hanno si presuppone che un processo di compilazione e distribuzione ad hoc. Molti progetti software di grandi dimensioni sono formalizzati più processi che fanno uso di open source, personalizzati o gli strumenti di terze parti. Per questi progetti è probabilmente possibile personalizzare il processo di compilazione o di distribuzione per modificare in modo appropriato le informazioni di configurazione prima di essere inviato all'ambiente di produzione. Se si compila l'applicazione web usando [MSBuild](http://en.wikipedia.org/wiki/MSBuild), [NAnt](http://nant.sourceforge.net/), o un altro strumento di compilazione, probabilmente è possibile aggiungere un passaggio di compilazione per modificare il `Web.config` file da includere le impostazioni specifiche dell'ambiente di produzione. O il flusso di lavoro di distribuzione a livello di codice è stato possibile connettersi al server di controllo di origine e recuperare appropriato `Web.config` file.

L'approccio per ottenere le informazioni di configurazione appropriato nell'ambiente di produzione effettivo dipende ampiamente i tuoi strumenti e flusso di lavoro. Di conseguenza, saranno non affrontare ulteriormente in questo argomento. Se si usa uno strumento di compilazione più diffusi come MSBuild o NAnt è possibile trovare le esercitazioni e articoli sulla distribuzione specifiche per questi strumenti tramite una ricerca sul web.

### <a name="managing-configuration-differences-via-the-web-deployment-project-add-in"></a>La gestione delle differenze di configurazione tramite la distribuzione Web del progetto componente aggiuntivo

Nel 2006 Microsoft ha rilasciato lo sviluppo progetto di componente aggiuntivo Web per Visual Studio 2005. Un componente aggiuntivo per Visual Studio 2008 è stato rilasciato nel 2008. Questo componente aggiuntivo consente agli sviluppatori ASP.NET per creare un progetto di distribuzione Web separato insieme il progetto di applicazione web che, quando compilato, in modo esplicito viene compilato l'applicazione web e copia i file da distribuire in una directory di output locale. Il progetto di applicazione Web Usa MSBuild dietro le quinte.

Per del impostazione predefinita, l'ambiente di sviluppo `Web.config` file viene copiato nella directory di output, ma è possibile configurare il progetto di distribuzione Web per personalizzare il

informazioni di configurazione che viene copiate in questa directory nei modi seguenti:

- Tramite `Web.config` file un file XML che contiene il testo di sostituzione e sostituzione di sezione, in cui si specifica la sezione da sostituire.
- Fornendo un percorso di un file di origine di configurazione esterno. Questa opzione è selezionata, il progetto di distribuzione Web consente di copiare un determinato `Web.config` file nella directory di output (anziché la `Web.config` file usato nell'ambiente di sviluppo).
- Tramite l'aggiunta di regole personalizzate per il file di MSBuild usato dal progetto di distribuzione Web.

Per distribuire la compilazione dell'applicazione web il progetto di distribuzione Web e quindi copiare i file dalla cartella di output del progetto nell'ambiente di produzione.

Per altre informazioni sull'uso di progetto di distribuzione Web estrarre [questo articolo di progetti di distribuzione Web](https://msdn.microsoft.com/magazine/cc163448.aspx) il numero di aprile 2007 di [MSDN Magazine](https://msdn.microsoft.com/magazine/default.aspx), o consultare i collegamenti nella sezione delle letture di approfondimento di termine di questa esercitazione.

> [!NOTE]
> È possibile usare il progetto di distribuzione Web con Visual Web Developer perché il progetto di distribuzione Web viene implementato come un Visual Studio componente aggiuntivo e le edizioni Visual Studio Express (incluso in Visual Web Developer) non supportano l'Add-Ins.


## <a name="summary"></a>Riepilogo

Il comportamento di un'applicazione web in fase di sviluppo e alle risorse esterne sono in genere diversi rispetto a quando la stessa applicazione è in fase di produzione. Ad esempio, le stringhe di connessione di database, le opzioni di compilazione e il comportamento quando si verifica un'eccezione non gestita in genere differiscono tra ambienti. Il processo di distribuzione deve supportare queste differenze. Come descritto in questa esercitazione, l'approccio più semplice consiste nel copiare manualmente un file di configurazione alternativa nell'ambiente di produzione. Sono possibili soluzioni più efficaci quando si usa la distribuzione di progetto di componente aggiuntivo Web o con un processo di compilazione o distribuzione formalizzato più in grado di soddisfare tali personalizzazioni.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Stringhe di connessione illustrate](http://www.connectionstrings.com/Articles/Show/what-is-a-connection-string)
- [@ ConnectionStrings.com le stringhe di connessione al database](http://www.connectionstrings.com/)
- [Non eseguire le applicazioni ASP.NET di produzione con `debug="true"` abilitata](https://weblogs.asp.net/scottgu/Don_1920_t-run-production-ASP.NET-Applications-with-debug_3D001D20_true_1D20_-enabled)
- [Risponde correttamente alle eccezioni non gestite - visualizzazione di pagine di errore descrittivo](http://aspnet.4guysfromrolla.com/articles/090606-1.aspx)
- [Procedura: usare un progetto di distribuzione Web di Visual Studio 2008?](../../../videos/how-do-i/how-do-i-use-a-visual-studio-2008-web-deployment-project.md)
- [Impostazioni di configurazione della chiave durante la distribuzione di un Database](http://aspnet.4guysfromrolla.com/articles/121008-1.aspx)
- [Download di progetti di Visual Studio 2008 Web Deployment](https://www.microsoft.com/downloads/details.aspx?FamilyId=0AA30AE8-C73B-4BDD-BB1B-FE697256C459&amp;displaylang=en) | [Download di progetti di Visual Studio 2005 Web distribuzione](https://download.microsoft.com/download/9/4/9/9496adc4-574e-4043-bb70-bc841e27f13c/WebDeploymentSetup.msi)
- [Progetti di distribuzione Web di Visual Studio 2008](https://weblogs.asp.net/scottgu/archive/2005/11/06/429723.aspx) | [rilasciato Visual Studio 2008 Web distribuzione progetto supporto](https://weblogs.asp.net/scottgu/archive/2008/01/28/vs-2008-web-deployment-project-support-released.aspx)
- [Progetti di distribuzione Web](https://msdn.microsoft.com/magazine/cc163448.aspx)

> [!div class="step-by-step"]
> [Precedente](deploying-your-site-using-visual-studio-cs.md)
> [Successivo](core-differences-between-iis-and-the-asp-net-development-server-cs.md)
