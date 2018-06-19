---
uid: mvc/overview/older-versions-1/overview/asp-net-mvc-overview
title: Panoramica di ASP.NET MVC | Documenti Microsoft
author: microsoft
description: Informazioni sulle differenze tra l'applicazione MVC ASP.NET e applicazioni Web Form ASP.NET. Informazioni su come stabilire quando compilare un'applicazione ASP.NET MVC.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 2dcb44a4-5cbf-4d62-b363-718104082d86
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/overview/asp-net-mvc-overview
msc.type: authoredcontent
ms.openlocfilehash: f44e6fb1e19d3c2384ebaeeca0ddea8239dd5a3c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
ms.locfileid: "26500870"
---
<a name="aspnet-mvc-overview"></a>Panoramica di ASP.NET MVC
====================
da [Microsoft](https://github.com/microsoft)

> Informazioni sulle differenze tra l'applicazione MVC ASP.NET e applicazioni Web Form ASP.NET. Informazioni su come stabilire quando compilare un'applicazione ASP.NET MVC.


Il modello architetturale Model-View-Controller (MVC) suddivide un'applicazione in tre componenti principali: il modello, la visualizzazione e il controller. Il framework ASP.NET MVC offre un'alternativa al modello Web Form ASP.NET per la creazione di applicazioni Web basate su MVC. Il framework ASP.NET MVC è un framework di presentazione leggero e facile da testare che (come con le applicazioni basate su Web Form) è integrato con le funzionalità ASP.NET esistenti, ad esempio pagine master e l'autenticazione basata sull'appartenenza. Il framework di MVC è definito nel **System** dello spazio dei nomi e fa parte di base, è supportata la **System. Web** dello spazio dei nomi.   
  
MVC è un modello di progettazione standard che molti sviluppatori hanno familiarità con. Alcuni tipi di applicazioni Web trarranno vantaggio dal framework MVC. Altri continueranno a utilizzare il modello ASP.NET tradizionale basato su Web Form e postback. Altri tipi di applicazioni Web verranno combinati i due approcci; Nessuno dei due esclude l'altro.   
  
Il framework di MVC include i componenti seguenti:


[![Richiamare un'azione del controller che prevede un valore di parametro](asp-net-mvc-overview/_static/image1.jpg)](asp-net-mvc-overview/_static/image1.png)

**Figura 01**: richiamare un'azione del controller che prevede un valore di parametro ([fare clic per visualizzare l'immagine ingrandita](asp-net-mvc-overview/_static/image2.png))


- **Modelli**. Gli oggetti modello rappresentano le parti dell'applicazione che implementano la logica per il dominio di dati applicazione s. Spesso, gli oggetti del modello recupero e archiviano lo stato del modello in un database. Ad esempio, un oggetto prodotto potrebbe recuperare informazioni da un database, operano su di esso e quindi scrivere informazioni aggiornate in una tabella dei prodotti in SQL Server.

Nelle applicazioni di piccole dimensioni, il modello è spesso una separazione concettuale anziché fisica. Ad esempio, se l'applicazione solo legge un set di dati e lo invia alla visualizzazione, l'applicazione non dispone di un livello di modello fisico e classi associate. In tal caso, il set di dati assume il ruolo di un oggetto model.

- **Viste**. Le visualizzazioni sono i componenti che consentono di visualizzare l'interfaccia utente di applicazione s (UI). In genere, questa interfaccia viene creata da dati del modello. Un esempio è rappresentato da una visualizzazione di modifica di una tabella di prodotti che consente di visualizzare le caselle di testo, elenchi a discesa e caselle di controllo in base allo stato corrente di un oggetto di prodotti.

- **Controller**. I controller sono i componenti che gestiscono l'interazione dell'utente, utilizzano il modello e selezionano infine una visualizzazione per il rendering dell'interfaccia utente. In un'applicazione MVC, Visualizza solo informazioni; il controller gestisce e risponde all'input dell'utente e l'interazione. Ad esempio, il controller gestisce i valori di stringa di query e passa questi valori per il modello, che a sua volta interroga il database utilizzando i valori.

Il modello MVC consente di creare applicazioni che separano i diversi aspetti dell'applicazione (logica di input, logica di business e logica dell'interfaccia utente), fornendo un accoppiamento libero tra questi elementi. Il modello specifica in cui ogni tipo di logica deve trovarsi nell'applicazione. La logica dell'interfaccia utente risiede nella vista. La logica di input risiede nel controller. La logica di business risiede nel modello. Questa separazione consente di gestire la complessità quando si compila un'applicazione, in quanto consente di concentrarsi su un aspetto dell'implementazione in una fase. Ad esempio, è possibile concentrarsi sulla visualizzazione senza in base alla logica di business.   
  
Oltre a gestire la complessità, il modello MVC rende più semplice testare le applicazioni rispetto a un'applicazione Web ASP.NET basata su Web Form di test. In un'applicazione Web ASP.NET basata su Web Form, ad esempio, una singola classe viene utilizzata per visualizzare l'output e per rispondere all'input dell'utente. Scrittura di test automatizzati per le applicazioni basate su Web Form ASP.NET può essere complessa, poiché per testare una singola pagina, è necessario creare un'istanza della classe di pagina, tutti i relativi controlli figlio e classi dipendenti aggiuntive nell'applicazione. Poiché molte classi vengono create istanze per eseguire la pagina, può essere difficile scrivere test indirizzati esclusivamente su singole parti dell'applicazione. Test per le applicazioni basate su Web Form ASP.NET può essere pertanto più difficili da implementare rispetto ai test in un'applicazione MVC. Inoltre, i test in un'applicazione basata su Web Form ASP.NET richiedono un server Web. Il framework MVC disaccoppia i componenti e utilizza in modo intensivo delle interfacce, che rende possibile il test di singoli componenti in maniera isolata dal resto del framework.   
  
L'accoppiamento debole tra i tre componenti principali di un'applicazione MVC Alza di livello anche lo sviluppo parallelo. Ad esempio, uno sviluppatore può utilizzare la visualizzazione, uno sviluppatore secondo gli strumenti per la logica di controller e un terzo può concentrarsi sulla logica di business nel modello.

## <a name="deciding-when-to-create-an-mvc-application"></a>Decidere quando creare un'applicazione MVC

È necessario considerare con attenzione se implementare un'applicazione Web tramite il framework ASP.NET MVC o il modello Web Form ASP.NET. Il framework di MVC non sostituisce il modello Web Form. è possibile utilizzare uno dei due framework per applicazioni Web. (Se si dispone di applicazioni esistenti basate su Web Form, queste continueranno a funzionare esattamente come in precedenza.)   
  
Prima di decidere di utilizzare il framework di MVC o il modello Web Form per un sito Web specifico, considerare i vantaggi di ogni approccio.

### <a name="advantages-of-an-mvc-based-web-application"></a>Vantaggi di un'applicazione Web basata su MVC

Il framework ASP.NET MVC offre i vantaggi seguenti:

- Rende più facile da gestire la complessità mediante la divisione di un'applicazione in del modello, la visualizzazione e il controller.
- Non utilizza lo stato di visualizzazione o form basati su server. In questo modo il framework di MVC ideale per gli sviluppatori che desiderano controllare il comportamento di un'applicazione completa.
- Utilizza un modello Front Controller che elabora le richieste dell'applicazione Web tramite un singolo controller. Ciò consente di progettare un'applicazione che supporta l'infrastruttura di routing avanzata. Per ulteriori informazioni, vedere [Front Controller](https://go.microsoft.com/fwlink/?LinkId=106357 "Front Controller") nel sito Web MSDN.
- Fornisce un supporto migliore per lo sviluppo basato su test (TDD).
- Funziona anche per le applicazioni Web supportate da grandi team di sviluppatori e Designer Web che necessitano di un livello elevato di controllare il comportamento dell'applicazione.

### <a name="advantages-of-a-web-forms-based-web-application"></a>Vantaggi di un'applicazione Web basata su form Web

Il framework basato su Web Form offre i vantaggi seguenti:

- Supporta un modello di eventi che mantiene lo stato su HTTP, che lo sviluppo di applicazioni Web line-of-business. L'applicazione basata su Web Form fornisce decine di eventi che sono supportati in centinaia di controlli server.
- Utilizza un modello Page Controller che aggiunge funzionalità a singole pagine. Per ulteriori informazioni, vedere [Page Controller](https://go.microsoft.com/fwlink/?LinkId=106359 "Page Controller") nel sito Web MSDN.
- Usa lo stato di visualizzazione o form basati su server, che può rendere più semplice la gestione delle informazioni sullo stato.
- Funziona anche per i piccoli team di sviluppatori e Designer che vogliono sfruttare le numerose componenti disponibili per lo sviluppo rapido di applicazioni Web.
- In generale, è meno complesso per lo sviluppo di applicazioni, poiché i componenti (il **pagina** classe, controlli e così via) sono strettamente integrati e necessitano in genere meno codice rispetto al modello MVC.

## <a name="features-of-the-aspnet-mvc-framework"></a>Funzionalità del Framework di MVC ASP.NET

Il framework ASP.NET MVC offre le funzionalità seguenti:

- Separazione delle attività dell'applicazione (logica di input, logica di business e logica dell'interfaccia utente), testabilità e sviluppo basato su test (TDD) per impostazione predefinita. Tutti i contratti principali nel framework di MVC sono basati sull'interfaccia e possono essere testati tramite oggetti fittizi, ovvero oggetti simulati che imitano il comportamento degli oggetti effettivi nell'applicazione. È possibile unit test dell'applicazione senza dover eseguire il controller in un processo ASP.NET, rendendo gli unit test veloci e flessibili. È possibile utilizzare qualsiasi framework di unit test compatibile con .NET Framework.
- Un framework estendibile e collegabile. I componenti del framework ASP.NET MVC sono progettati in modo che possano essere facilmente sostituiti o personalizzati. È possibile collegare il proprio motore di visualizzazione, i criteri di routing URL, la serializzazione dei parametri del metodo di azione e altri componenti. Il framework ASP.NET MVC supporta inoltre l'utilizzo dei modelli contenitore Dependency Injection (DI) e Inversion of Control (IOC). SEN consente di inserire oggetti in una classe, invece di basarsi sulla classe per creare l'oggetto stesso. Il modello IOC specifica che se un oggetto richiede un altro oggetto, il primo deve ottenere il secondo oggetto da un'origine esterna, ad esempio un file di configurazione. In questo modo le operazioni di testing.
- Un componente di mapping di URL potente che consente di creare applicazioni con URL comprensibili e disponibile per la ricerca. URL non è necessario includere le estensioni di nome file e sono progettati per supportare modelli di denominazione degli URL che funzionano anche per la ricerca engine optimization (SEO) e REST representational state transfer () addressing.
- Supporto per l'utilizzo del markup nella pagina ASP.NET esistente (file con estensione aspx), il controllo utente (ascx) e file di markup (file con estensione master) della pagina master come modelli di visualizzazione. È possibile utilizzare le funzionalità ASP.NET esistenti con il framework di MVC ASP.NET, ad esempio pagine master annidate, espressioni inline (&lt;% = %&gt;), controlli server dichiarativi, modelli, associazione dati, localizzazione e così via.
- Supporto per le funzionalità ASP.NET esistenti. ASP.NET MVC consente di usare funzionalità quali l'autenticazione basata su form e l'autenticazione di Windows, autorizzazione URL, appartenenza e ruoli, output e la memorizzazione nella cache di dati, la gestione dello stato di sessione e profilo, il monitoraggio dello stato, il sistema di configurazione e il provider architettura.
