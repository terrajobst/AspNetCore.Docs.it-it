---
uid: web-forms/what-is-web-forms
title: Novità di Web Form | Documenti Microsoft
author: rick-anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/21/2014
ms.topic: article
ms.assetid: 5fa1daf9-1161-4cfa-bd4c-658f48b2c229
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/what-is-web-forms
msc.type: content
ms.openlocfilehash: dcaba60a8640ce83f73460a5c8ee457257b9397e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
ms.locfileid: "26528610"
---
<a name="what-is-web-forms"></a>Informazioni su Web Form
====================
Web Form ASP.NET è una parte del framework dell'applicazione web ASP.NET e viene incluso [Visual Studio](https://www.asp.net/downloads). Uno dei quattro modelli di programmazione che è possibile utilizzare per creare applicazioni web ASP.NET, gli altri sono applicazioni a pagina singola ASP.NET MVC ASP.NET e ASP.NET Web Pages.

Web Form sono le pagine che gli utenti richiedono l'utilizzo del browser. Queste pagine possono essere scritte utilizzando una combinazione di HTML, lo script client, i controlli server e il codice lato server. Quando gli utenti richiedono una pagina, viene compilato ed eseguito nel server dal framework e quindi il framework genera il markup HTML che è possibile eseguire il rendering nel browser. Una pagina Web Form ASP.NET presenta le informazioni utente in qualsiasi browser o un dispositivo client.

Se si utilizza Visual Studio, è possibile creare Web Form ASP.NET. IDE di Visual Studio Integrated Development ambiente () consente di trascinare i controlli server per il layout della pagina Web Form. Si può facilmente impostare proprietà, metodi ed eventi per i controlli della pagina o per controllarne la pagina. Queste proprietà, metodi ed eventi utilizzati per definire il comportamento della pagina web, l'aspetto e così via. Per scrivere il codice server per gestire la logica per la pagina, è possibile utilizzare un linguaggio .NET come c# o Visual Basic.

> [!NOTE] 
> 
> Documentazione di ASP.NET e Visual Studio si estende su più versioni. Gli argomenti che illustrano le funzionalità da versioni precedenti possono essere utili per le attività correnti e scenari con le versioni più recenti.


**Web Form ASP.NET sono:** 

- Basato sulla tecnologia Microsoft ASP.NET, in cui il codice eseguito sul server in modo dinamico genera output delle pagine Web nel dispositivo client o browser.
- Compatibile con qualsiasi browser o un dispositivo mobile. Una pagina Web ASP.NET esegue automaticamente il rendering dell'HTML conforme al browser corretto per funzionalità quali gli stili, layout e così via.
- Compatibile con qualsiasi linguaggio supportato da .NET common language runtime, ad esempio Microsoft Visual Basic e Microsoft Visual c#.
- Basato su Microsoft .NET Framework. In questo modo tutti i vantaggi del framework, inclusi un ambiente gestito, indipendenza dai tipi e l'ereditarietà.
- Flessibile perché è possibile aggiungere creati dall'utente e i controlli di terze parti a loro.

**Web Form ASP.NET offrono:** 

- Separazione di HTML e altro codice dell'interfaccia utente dalla logica dell'applicazione.
- Un gruppo completo di controlli server per le attività comuni, tra cui l'accesso ai dati.
- Potenti associazione dati, con supporto dello strumento ideale.
- Supporto per lo scripting sul lato client che viene eseguita nel browser.
- Supporto per un'ampia gamma di altre funzionalità, tra cui routing, protezione, prestazioni, internazionalizzazione, test, debug, la gestione degli errori e la gestione dello stato.

## <a name="aspnet-web-forms-helps-you-overcome-challenges"></a>Web Form ASP.NET consente di superare le difficoltà di

Programmazione di applicazioni Web presenta problemi che si verificano in genere durante la programmazione di applicazioni client tradizionali. Alcuni di questi problemi sono:

- **Implementazione di un'interfaccia utente Web completa** : può essere difficile e difficile da progettare e implementare un utente interfaccia utilizzando funzionalità HTML di base, soprattutto se la pagina ha un layout complesso, una grande quantità di contenuto dinamico e completa oggetti utente interattivo.
- **Separazione di client e server** -applicazione Web In un client (browser) e server sono diversi programmi in esecuzione in computer diversi (e anche in sistemi operativi diversi). Di conseguenza, le due metà dell'applicazione condividono poche informazioni; essi possono comunicare, ma in genere si scambiano solo piccoli blocchi di dati semplici.
- **Esecuzione senza stato** : quando un server Web riceve una richiesta per una pagina, individua la pagina, viene elaborato, viene inviata al browser e quindi lo ignora tutte le informazioni di pagina. Se l'utente richiede nuovamente la stessa pagina, il server si ripete l'intera sequenza, la rielaborazione da zero. In altre parole, un server non ha memoria delle pagine in cui è stato elaborato, sono senza stato. Pertanto, se un'applicazione deve gestire le informazioni relative a una pagina, l'assenza di tali può diventare un problema.
- **Funzionalità del client sconosciuto** -In molti casi, le applicazioni Web sono accessibili a molti utenti utilizzano browser differenti. Browser hanno funzionalità diverse, rendendo difficile creare un'applicazione che verrà eseguito altrettanto correttamente in tutte.
- **Problemi con l'accesso ai dati** -la lettura da e scrittura a un'origine dati nelle applicazioni Web tradizionali può risultare complicata ed elevato utilizzo di risorse.
- **Problemi relativi alla scalabilità** : In molti casi, le applicazioni Web progettate con metodi esistenti in grado di soddisfare gli obiettivi di scalabilità a causa della mancanza di compatibilità tra i vari componenti dell'applicazione. Si tratta spesso di un punto di errore comuni per le applicazioni in un ciclo di crescita elevato.

Rispondere queste sfide per le applicazioni Web può richiedere tempo e impegno. Web Form ASP.NET e framework di ASP.NET risolve queste problematiche nei modi seguenti:

- **Modello a oggetti intuitivo e coerente** -framework di pagine ASP.NET presenta un modello a oggetti che consente di considerare i form come un'unità, non come parti separate, client e server. In questo modello, è possibile programmare la pagina in modo più intuitivo rispetto alle applicazioni Web tradizionali, inclusa la possibilità di impostare le proprietà degli elementi di pagina e rispondere agli eventi. Inoltre, i controlli server ASP.NET sono un'astrazione del contenuto di una pagina HTML fisico e l'interazione diretta tra browser e server. In generale, è possibile utilizzare i controlli server modo è possibile utilizzare i controlli in un'applicazione client e non devono preoccuparsi di come creare il codice HTML per presentare ed elaborare i controlli e il relativo contenuto.
- **Modello di programmazione basato su eventi** -Web Form ASP.NET portare il noto modello di scrittura di gestori eventi per eventi che si verificano sul client o server applicazioni Web. Il framework della pagina ASP.NET astrae questo modello in modo che il meccanismo di acquisizione di un evento nel client, la trasmissione di tale server e la chiamata al metodo appropriato sia automatico e invisibile all'utente. Il risultato è una struttura di codice scritta in modo semplice e chiaro che supporta lo sviluppo basato su eventi.
- **Gestione dello stato intuitiva** : framework di pagine ASP.NET gestisce automaticamente le attività di mantenere lo stato della pagina e i relativi controlli e consente di mantenere lo stato di informazioni specifiche dell'applicazione in modo esplicito. Questa operazione viene eseguita senza un uso massiccio di risorse del server e può essere implementata con o senza inviare cookie nel browser.
- **Le applicazioni indipendenti dal browser** -framework della pagina ASP.NET consente di creare tutta la logica dell'applicazione nel server, eliminando la necessità di scrivere codice per le differenze nel browser. Tuttavia, ancora consente di sfruttare le funzionalità specifiche del browser scrivendo codice sul lato client per offrire un'esperienza più completa di client e prestazioni migliorate.
- **Supporto di .NET framework common language runtime** -framework della pagina ASP.NET si basa su .NET Framework, all'intero framework è disponibile per qualsiasi applicazione ASP.NET. Le applicazioni possono essere scritte in qualsiasi linguaggio compatibile con il runtime. Inoltre, l'accesso ai dati viene semplificato utilizzando l'infrastruttura di accesso ai dati fornito da .NET Framework, incluso ADO.NET.
- **Le prestazioni del server di scalabilità di .NET framework** -framework della pagina ASP.NET consente di ridimensionare l'applicazione Web da un computer con un singolo processore per una Web farm con più computer correttamente e senza apportare modifiche complesse per l'applicazione logica.

## <a name="features-of-aspnet-web-forms"></a>Funzionalità di Web Form ASP.NET

- **I controlli server**-controlli server Web ASP.NET sono gli oggetti nelle pagine Web ASP.NET che vengono eseguiti quando viene richiesta la pagina e per eseguire il rendering dei tag nel browser. Molti controlli server Web sono simili a elementi HTML noti, ad esempio i pulsanti e caselle di testo. Altri controlli presentano comportamenti complessi, ad esempio un calendario di controlli e controlli che è possibile utilizzare per connettersi alle origini dati e visualizzare i dati.
- **Pagine master**-pagine master ASP.NET consentono di creare un layout coerente per le pagine dell'applicazione. Una singola pagina master definisce l'aspetto e il comportamento standard che desidera che per tutte le pagine (o un gruppo di pagine) nell'applicazione. È quindi possibile creare singole pagine di contenuto che si desidera visualizzare il contengono. Quando gli utenti richiedono le pagine di contenuto, vengono unite con la pagina master per produrre l'output che combina il layout della pagina master con il contenuto della pagina di contenuto.
- **Utilizzo dei dati**-ASP.NET fornisce numerose opzioni per l'archiviazione, il recupero e visualizzazione dei dati. In un'applicazione Web Form ASP.NET, utilizzare i controlli associati a dati per automatizzare la presentazione o un input di dati in una pagina web di elementi dell'interfaccia utente, ad esempio tabelle e le caselle di testo e gli elenchi a discesa.
- **L'appartenenza**-ASP.NET Identity archivia le credenziali degli utenti in un database creato dall'applicazione. Quando l'accesso, l'applicazione convalida le credenziali per la lettura del database. Il progetto *Account* cartella contenente i file che implementano le varie parti di appartenenza: la registrazione, accesso, la modifica di una password e autorizzare l'accesso. Inoltre, Web Form ASP.NET supporta OAuth e OpenID. Questi miglioramenti di autenticazione consentono agli utenti di accedere al sito utilizzando le credenziali esistenti, tali account come Facebook, Twitter, Windows Live e Google. Per impostazione predefinita, il modello crea un database delle appartenenze utilizzando un nome predefinito del database in un'istanza di SQL Server Express LocalDB, il server di database di sviluppo che viene fornito con Visual Studio Express 2013 per Web.
- **Lo Script client e altri Framework Client**-è possibile migliorare le funzionalità basate su server di ASP.NET, includendo funzionalità di script client nelle pagine Web Form ASP.NET. È possibile utilizzare lo script client per fornire un'interfaccia utente più dettagliata e reattiva agli utenti. È inoltre possibile utilizzare lo script client per effettuare chiamate asincrone al server Web durante l'esecuzione di una pagina nel browser.
- **Routing**-routing degli URL consente di configurare un'applicazione per accettare richieste di URL che non fanno riferimento a file fisici. Un URL della richiesta è semplicemente l'URL di un utente immette nel browser per trovare una pagina nel sito web. Utilizzare il routing per definire gli URL che sono semanticamente significativi per gli utenti e che possono aiutare a con l'ottimizzazione motore di ricerca (SEO).
- **Gestione stato**-Web Form ASP.NET include diverse opzioni che consentono di mantenere i dati in una pagina specifica sia una livello di applicazione di base.
- **Sicurezza**-una parte importante dello sviluppo di un'applicazione più sicura consiste nel comprendere le minacce a esso. Microsoft ha sviluppato un modo per classificare i pericoli: Spoofing, manomissione, ripudio, diffusione di informazioni, Denial of service, l'elevazione dei privilegi (STRIDE). Web Form ASP.NET, è possibile aggiungere punti di estendibilità e opzioni di configurazione che consentono di personalizzare i vari comportamenti di sicurezza in Web Form ASP.NET.
- **Prestazioni**-prestazioni possono rappresentare un fattore chiave in un progetto o un sito Web ha esito positivo. Web Form ASP.NET consente di modificare le prestazioni correlate alla pagina ed elaborazioni nel server di controllo, la gestione dello stato, l'accesso ai dati, configurazione dell'applicazione e il caricamento ed efficiente di codifica consigliate.
- **Internazionalizzazione**- Web Form ASP.NET consente di creare pagine web che è possibile ottenere il contenuto e altri dati in base all'impostazione della lingua preferita per il browser o in base alla scelta dell'utente esplicito del linguaggio. Contenuto e altri dati vengono definiti come risorse e tali dati possono essere archiviati in file di risorse o altre origini. In una pagina Web Form ASP.NET, configurare i controlli per ottenere i valori di proprietà dalle risorse. In fase di esecuzione, le espressioni di risorsa vengono sostituite dalle risorse dal file di risorse localizzate appropriate.
- **Debug e gestione degli errori**-ASP.NET include funzionalità che consentono di diagnosticare i problemi che potrebbero verificarsi nell'applicazione Web Form. Debug e la gestione degli errori sono supportati anche nei Web Form ASP.NET in modo che le applicazioni di compilare ed eseguire in modo efficace.
- **Distribuzione e Hosting**-IIS, ASP.NET, Azure e Visual Studio forniscono strumenti che consentono il processo di distribuzione e che ospita l'applicazione Web Form.

## <a name="deciding-when-to-create-a-web-forms-application"></a>Decidere quando creare un'applicazione Web Form

È necessario considerare con attenzione se implementare un'applicazione Web utilizzando ASP.NET Web Forms modello o un altro, ad esempio il framework di MVC ASP.NET. Il framework di MVC non sostituisce il modello Web Form. è possibile utilizzare uno dei due framework per applicazioni Web. Prima di decidere di utilizzare il modello Web Form o il framework MVC per un sito Web specifico, considerare i vantaggi di ogni approccio.

### <a name="advantages-of-a-web-forms-based-web-application"></a>Vantaggi di un'applicazione Web basata su form Web

Il framework basato su Web Form offre i vantaggi seguenti:

- Supporta un modello di eventi che mantiene lo stato su HTTP, che lo sviluppo di applicazioni Web line-of-business. L'applicazione basata su Web Form fornisce decine di eventi che sono supportati in centinaia di controlli server.
- Utilizza un modello Page Controller che aggiunge funzionalità a singole pagine. Per ulteriori informazioni, vedere [Page Controller](https://go.microsoft.com/fwlink/?LinkId=106359 "Page Controller") nel sito Web MSDN.
- Usa lo stato di visualizzazione o form basati su server, che può rendere più semplice la gestione delle informazioni sullo stato.
- Funziona anche per i piccoli team di sviluppatori e Designer che vogliono sfruttare le numerose componenti disponibili per lo sviluppo rapido di applicazioni Web.
- In generale, è meno complesso per lo sviluppo di applicazioni, poiché i componenti (il **pagina** classe, controlli e così via) sono strettamente integrati e necessitano in genere meno codice rispetto al modello MVC.

### <a name="advantages-of-an-mvc-based-web-application"></a>Vantaggi di un'applicazione Web basata su MVC

Il framework ASP.NET MVC offre i vantaggi seguenti:

- Rende più facile da gestire la complessità mediante la divisione di un'applicazione in del modello, la visualizzazione e il controller.
- Non utilizza lo stato di visualizzazione o form basati su server. In questo modo il framework di MVC ideale per gli sviluppatori che desiderano controllare il comportamento di un'applicazione completa.
- Utilizza un modello Front Controller che elabora le richieste dell'applicazione Web tramite un singolo controller. Ciò consente di progettare un'applicazione che supporta l'infrastruttura di routing avanzata. Per ulteriori informazioni, vedere [Front Controller](https://go.microsoft.com/fwlink/?LinkId=106357 "Front Controller") nel sito Web MSDN.
- Fornisce un supporto migliore per lo sviluppo basato su test (TDD).
- Funziona anche per le applicazioni Web supportate da grandi team di sviluppatori e Designer Web che necessitano di un livello elevato di controllare il comportamento dell'applicazione.
