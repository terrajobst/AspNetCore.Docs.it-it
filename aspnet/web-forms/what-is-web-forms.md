---
uid: web-forms/what-is-web-forms
title: What ' s Web Form | Microsoft Docs
author: rick-anderson
description: ''
ms.author: riande
ms.date: 02/21/2014
ms.assetid: 5fa1daf9-1161-4cfa-bd4c-658f48b2c229
msc.legacyurl: /web-forms/what-is-web-forms
msc.type: content
ms.openlocfilehash: ccb0e6096b0281e5ef8af63bfd84b172e7f209b9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835644"
---
<a name="what-is-web-forms"></a>What ' s Web Forms
====================
Web Form ASP.NET è una parte del framework dell'applicazione web ASP.NET ed è incluso con [Visual Studio](https://www.asp.net/downloads). È uno dei quattro modelli di programmazione che è possibile usare per creare applicazioni web ASP.NET, gli altri sono applicazioni a pagina singola ASP.NET, ASP.NET Web Pages e ASP.NET MVC.

Web Form sono le pagine che gli utenti richiedono utilizzando il proprio browser. Queste pagine possono essere scritte utilizzando una combinazione di HTML, script client, i controlli server e il codice lato server. Quando gli utenti richiedono una pagina, viene compilata ed eseguita nel server dal framework, e quindi il framework genera il markup HTML che può eseguire il rendering nel browser. Una pagina Web Form ASP.NET presenta le informazioni dell'utente in qualsiasi browser o un dispositivo client.

Usa Visual Studio, è possibile creare Web Form ASP.NET. L'IDE Visual Studio Integrated Development ambiente () consente di trascinare e rilasciare controlli server per il layout della pagina Web Form. È possibile quindi è impostare facilmente le proprietà, metodi ed eventi per i controlli nella pagina o per la pagina desiderata. Queste proprietà, metodi ed eventi vengono usati per definire il comportamento della pagina web, aspetto e così via. Per scrivere il codice server per gestire la logica per la pagina, è possibile usare un linguaggio .NET, ad esempio Visual Basic o c#.

> [!NOTE] 
> 
> Documentazione di ASP.NET e Visual Studio si estende su più versioni. Gli argomenti che illustrano le funzionalità di versioni precedenti possono essere utili per le attività correnti e scenari con le versioni più recenti.


**Web Form ASP.NET sono:** 

- Basata sulla tecnologia di Microsoft ASP.NET, in cui il codice che viene eseguito nel server in modo dinamico genera output delle pagine Web nel dispositivo client o browser.
- È compatibile con qualsiasi browser o un dispositivo mobile. Una pagina Web ASP.NET esegue automaticamente il rendering dell'HTML browser conforme corretto per le funzionalità, ad esempio stili, layout e così via.
- È compatibile con qualsiasi linguaggio supportato da .NET common language runtime, ad esempio Microsoft Visual Basic e Microsoft Visual c#.
- Basata su Microsoft .NET Framework. In questo modo tutti i vantaggi di framework, inclusi un ambiente gestito, indipendenza dai tipi e l'ereditarietà.
- Flessibile perché è possibile aggiungere creati dall'utente e controlli di terzi a essi.

**Offerta di Web Form ASP.NET:** 

- Separazione del codice HTML e altro codice dell'interfaccia utente dalla logica dell'applicazione.
- Una suite completa di controlli server per attività comuni, tra cui l'accesso ai dati.
- Dati avanzato, con il supporto di straordinario strumento di associazione.
- Supporto per la creazione di script lato client che viene eseguita nel browser.
- Supporto per un'ampia gamma di altre funzionalità, tra cui routing, sicurezza, prestazioni, internazionalizzazione, test, debug, la gestione degli errori e la gestione dello stato.

## <a name="aspnet-web-forms-helps-you-overcome-challenges"></a>Web Form ASP.NET consente di superare le difficoltà

Programmazione di applicazioni Web presenta sfide che non si verificano in genere durante la programmazione di applicazioni basate su client tradizionale. Alcuni di questi problemi sono:

- **Implementazione di una ricca interfaccia utente Web** : può essere difficile e noioso progettare e implementare un utente dell'interfaccia usando funzionalità HTML di base, soprattutto se la pagina ha un layout complesso, una grande quantità di contenuto dinamico e completa oggetti utente interattivo.
- **La separazione di client e server** -applicazione Web In un client (browser) e server sono diversi programmi in esecuzione in computer diversi (e anche in sistemi operativi diversi). Di conseguenza, le due metà dell'applicazione condividono poche informazioni; Questi possono comunicare, ma in genere solo di piccoli blocchi di semplici informazioni di exchange.
- **L'esecuzione senza stato** : quando un server Web riceve una richiesta per una pagina, consente di trovare la pagina, li elabora, lo invia al browser e quindi rimuove tutte le informazioni sulla pagina. Se l'utente richiede la stessa pagina anche in questo caso, il server si ripete l'intera sequenza, la rielaborazione da zero. In altre parole, un server non ha memoria delle pagine in cui è stato elaborato, pagina sono senza stato. Pertanto, se un'applicazione deve conservare informazioni su una pagina, la sua natura senza stato può diventare un problema.
- **Funzionalità del client sconosciuto** -In molti casi, le applicazioni Web sono accessibili a molti utenti con browser diversi. Browser hanno funzionalità diverse, è difficile creare un'applicazione che verrà eseguito altrettanto correttamente in tutti gli elementi.
- **Problemi con l'accesso ai dati** -lettura dal disco e la scrittura di un'origine dati nelle applicazioni Web tradizionali possono risultare complessi e a elevato utilizzo di risorse.
- **Problemi relativi alla scalabilità** : In molti casi le applicazioni Web progettate con metodi esistenti non soddisfano gli obiettivi di scalabilità a causa della mancanza di compatibilità tra i vari componenti dell'applicazione. Si tratta spesso di un punto di errore comuni per le applicazioni in un ciclo di crescita elevato.

Risolvere queste difficoltà per le applicazioni Web può richiedere tempo e impegno. Web Form ASP.NET e framework di ASP.NET risolvere queste problematiche nei modi seguenti:

- **Modello a oggetti intuitiva e coerente** -framework di pagine di ASP.NET presenta un modello a oggetti che consente di considerare il form come una singola unità, non come parti separate, client e server. In questo modello, è possibile programmare la pagina in modo più intuitivo rispetto alle applicazioni Web tradizionali, inclusa la possibilità di impostare le proprietà per gli elementi della pagina e rispondere agli eventi. Inoltre, i controlli server ASP.NET sono un'astrazione del contenuto di una pagina HTML fisico e l'interazione diretta tra browser e server. In generale, è possibile usare i controlli server il modo è possibile utilizzare i controlli in un'applicazione client e non devono preoccuparsi di come creare il codice HTML per presentare ed elaborare i controlli e i relativi contenuti.
- **Modello di programmazione guidata dagli eventi** -ASP.NET Web Form offre alle applicazioni Web il noto modello di scrittura di gestori eventi per eventi che si verificano nel server o client. Framework della pagina ASP.NET consente di astrarre il modello in modo che il meccanismo sottostante di un evento nel client di acquisizione, la trasmissione di tale server e la chiamata al metodo appropriato è tutto automatico e invisibile all'utente. Il risultato è una struttura di codice scritta in modo semplice e chiaro che supporta lo sviluppo basato su eventi.
- **Gestione dello stato intuitiva** : framework di pagine ASP.NET gestisce automaticamente l'attività di gestione dello stato della pagina e i relativi controlli, e consente di mantenere lo stato di informazioni specifiche dell'applicazione in modo esplicito. Questa operazione viene eseguita senza un uso massiccio di risorse del server e può essere implementata con o senza inviare cookie nel browser.
- **Le applicazioni indipendenti dal browser** -framework della pagina ASP.NET consente di creare tutta la logica dell'applicazione nel server, eliminando la necessità di scrivere codice per le differenze nel browser. Tuttavia, ancora consente di sfruttare le funzionalità specifiche del browser mediante la scrittura di codice lato client per offrire un'esperienza client avanzata e prestazioni migliorate.
- **Supporto di .NET framework common language runtime** -framework della pagina ASP.NET viene compilato in .NET Framework, pertanto è disponibile per qualsiasi applicazione ASP.NET all'intero framework. Le applicazioni possono essere scritte in qualsiasi linguaggio che è compatibile con il runtime. Inoltre, l'accesso ai dati viene semplificata usando l'infrastruttura di accesso dati fornita da .NET Framework, tra cui ADO.NET.
- **Le prestazioni del server di scalabilità di .NET framework** -framework della pagina ASP.NET consente di ridimensionare l'applicazione Web da un computer con un singolo processore per una Web farm con più computer correttamente e senza apportare modifiche complesse dell'applicazione per la logica.

## <a name="features-of-aspnet-web-forms"></a>Funzionalità di Web Form ASP.NET

- **I controlli server**-controlli server Web ASP.NET sono oggetti in ASP.NET Web pages che vengono eseguiti quando viene richiesta la pagina e tale markup rendering nel browser. Molti controlli server Web sono simili agli elementi HTML familiari, quali pulsanti e caselle di testo. Altri controlli presentano comportamenti complessi, ad esempio un calendario di controlli e controlli che è possibile usare per connettersi alle origini dati e visualizzare i dati.
- **Pagine master**-pagine master ASP.NET consentono di creare un layout coerente per le pagine nell'applicazione. Una singola pagina master definisce l'aspetto e il comportamento standard che desidera che per tutte le pagine (o un gruppo di pagine) nell'applicazione. È quindi possibile creare singole pagine di contenuto che includono il contenuto che si desidera visualizzare. Quando gli utenti richiedono le pagine di contenuto, queste vengono unite alla pagina master per generare l'output che combina il layout della pagina master con il contenuto della pagina di contenuto.
- **Utilizzo dei dati**-ASP.NET offre molte opzioni per l'archiviazione, il recupero e visualizzazione dei dati. In un'applicazione Web Form ASP.NET, si usano controlli associati a dati per automatizzare la presentazione o l'input di dati in elementi dell'interfaccia utente della pagina web, ad esempio tabelle e le caselle di testo ed elenchi di riepilogo a discesa.
- **Appartenenza**-ASP.NET Identity archivia le credenziali degli utenti in un database creato dall'applicazione. Quando l'accesso, l'applicazione convalida le proprie credenziali per la lettura del database. Il progetto *Account* cartella contiene i file che implementano le varie parti di appartenenza: la registrazione, accesso, modifica di una password e l'autorizzazione dell'accesso. Inoltre, Web Form ASP.NET supporta OAuth e OpenID. Questi miglioramenti di autenticazione consentono agli utenti di accedere al sito usando le credenziali esistenti, da tali account come Facebook, Twitter, Windows Live e Google. Per impostazione predefinita, il modello crea un database di appartenenza usando un nome predefinito del database in un'istanza di SQL Server Express LocalDB, il server di database di sviluppo fornita con Visual Studio Express 2013 per Web.
- **Lo Script client e i Framework Client**-è possibile migliorare le funzionalità basate su server di ASP.NET, includendo la funzionalità di script client nelle pagine Web Form ASP.NET. È possibile usare lo script client per fornire un'interfaccia utente più reattiva e più completa agli utenti. È anche possibile usare lo script client per effettuare chiamate asincrone al server Web durante l'esecuzione di una pagina nel browser.
- **Routing**-routing degli URL consente di configurare un'applicazione di accettare richieste di URL che non sono mappati a file fisici. URL della richiesta è semplicemente l'URL di un utente immette il browser per trovare una pagina nel sito web. Usare il routing per definire l'URL che sono significativi semanticamente per gli utenti e che può aiutare con l'ottimizzazione motore di ricerca (SEO).
- **Gestione dello stato**-Web Form ASP.NET include numerose opzioni che consentono di mantenere i dati in una pagina specifica sia un'applicazione a livello.
- **Sicurezza**-una parte importante dello sviluppo di un'applicazione più sicura consiste nel comprendere le minacce ad esso. Microsoft ha sviluppato un modo per classificare le minacce: lo Spoofing, manomissioni, ripudio, diffusione di informazioni, Denial of service, l'elevazione dei privilegi (STRIDE). In Web Form ASP.NET, è possibile aggiungere punti di estendibilità e le opzioni di configurazione che consentono di personalizzare diversi comportamenti di sicurezza in Web Form ASP.NET.
- **Prestazioni**-prestazioni possono essere un fattore chiave in un progetto o sito Web ha esito positivo. Web Form ASP.NET consente di modificare le prestazioni correlate alla pagina e l'elaborazione di controllo server, gestione dello stato, l'accesso ai dati, configurazione dell'applicazione e il caricamento ed efficiente di codifica consigliate.
- **Internazionalizzazione**: Web Form ASP.NET consente di creare pagine web che è possibile ottenere il contenuto e altri dati in base all'impostazione di lingua preferita per il browser o basato su scelta esplicita dell'utente del linguaggio. Contenuto e altri dati vengono chiamati come risorse e tali dati possono essere archiviati in file di risorse o altre origini. In una pagina Web Form ASP.NET, si configura i controlli per ottenere i valori delle proprietà delle risorse. In fase di esecuzione, le espressioni di risorse vengono sostituite dalle risorse dal file di risorse localizzate appropriate.
- **Debug e la gestione degli errori**-ASP.NET include funzionalità che consentono di diagnosticare i problemi che potrebbero verificarsi nell'applicazione Web Form. Debug e la gestione degli errori sono supportati anche nei Web Form ASP.NET in modo che le applicazioni, compilare ed eseguire in modo efficace.
- **Distribuzione e Hosting**-Visual Studio, ASP.NET, Azure e IIS forniscono strumenti che consentono il processo di distribuzione e che ospita l'applicazione Web Form.

## <a name="deciding-when-to-create-a-web-forms-application"></a>Decidere quando creare un'applicazione Web Form

È necessario considerare con attenzione se implementare un'applicazione Web utilizzando ASP.NET Web Forms modello o un altro, ad esempio il framework ASP.NET MVC. Il framework MVC non sostituisce il modello Web Form; è possibile usare uno dei due framework per applicazioni Web. Prima di decidere di usare il modello Web Form o il framework MVC per un sito Web specifico, considerare i vantaggi offerti da questi due approcci.

### <a name="advantages-of-a-web-forms-based-web-application"></a>Vantaggi di un'applicazione Web basata su moduli Web

Il framework basato su Web Form offre i vantaggi seguenti:

- Supporta un modello di eventi che mantiene lo stato su HTTP, con conseguenti vantaggi per lo sviluppo di applicazioni Web line-of-business. L'applicazione basata su Web Form fornisce decine di eventi che sono supportati in centinaia di controlli server.
- Usa un modello Page Controller che aggiunge funzionalità a singole pagine. Per altre informazioni, vedere [Page Controller](https://go.microsoft.com/fwlink/?LinkId=106359 "Page Controller") sul sito Web MSDN.
- Usa lo stato di visualizzazione o form basati su server, che possono semplificarne la gestione delle informazioni sullo stato.
- Funziona anche per piccoli team di sviluppatori e progettisti che vogliono sfruttare i vantaggi del numero elevato di componenti disponibili per lo sviluppo rapido di applicazioni.
- In generale, è meno complesso per lo sviluppo di applicazioni, poiché i componenti (il **pagina** classe, controlli e così via) sono strettamente integrati e necessitano in genere meno codice rispetto al modello MVC.

### <a name="advantages-of-an-mvc-based-web-application"></a>Vantaggi di un'applicazione Web basata su MVC

Il framework ASP.NET MVC offre i vantaggi seguenti:

- Rende più facile da gestire la complessità mediante la suddivisione di un'applicazione nel modello, la visualizzazione e il controller.
- Non usa lo stato di visualizzazione o form basati su server. Ciò rende il framework di MVC ideale per gli sviluppatori che intendono controllo completo sul comportamento di un'applicazione.
- Usa un modello Front Controller che elabora le richieste dell'applicazione Web tramite un singolo controller. In questo modo è possibile progettare un'applicazione che supporta l'infrastruttura di routing avanzata. Per altre informazioni, vedere [Front Controller](https://go.microsoft.com/fwlink/?LinkId=106357 "Front Controller") sul sito Web MSDN.
- Fornisce un supporto migliore per lo sviluppo basato su test (TDD).
- Funziona anche per le applicazioni Web che sono supportate dal team di sviluppatori e Designer Web che richiedono un elevato grado di controllare il comportamento dell'applicazione di grandi dimensioni.
