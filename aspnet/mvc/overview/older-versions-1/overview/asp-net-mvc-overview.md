---
uid: mvc/overview/older-versions-1/overview/asp-net-mvc-overview
title: Panoramica di ASP.NET MVC | Microsoft Docs
author: microsoft
description: Informazioni sulle differenze tra l'applicazione MVC ASP.NET e applicazioni Web Form ASP.NET. Informazioni su come stabilire quando compila un'applicazione ASP.NET MVC.
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 2dcb44a4-5cbf-4d62-b363-718104082d86
msc.legacyurl: /mvc/overview/older-versions-1/overview/asp-net-mvc-overview
msc.type: authoredcontent
ms.openlocfilehash: 61a7841ee238ec365b7d1909221bbe3d834faf84
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836769"
---
<a name="aspnet-mvc-overview"></a>Panoramica di ASP.NET MVC
====================
da [Microsoft](https://github.com/microsoft)

> Informazioni sulle differenze tra l'applicazione MVC ASP.NET e applicazioni Web Form ASP.NET. Informazioni su come stabilire quando compila un'applicazione ASP.NET MVC.


Il modello architetturale Model-View-Controller (MVC) suddivide un'applicazione in tre componenti principali: il modello, la visualizzazione e il controller. Il framework ASP.NET MVC offre un'alternativa al modello Web Form ASP.NET per la creazione di applicazioni Web basate su MVC. Il framework ASP.NET MVC è un framework di presentazione leggero e facile da testare che (come con le applicazioni basate su Web Form) è integrato con funzionalità ASP.NET esistenti, ad esempio pagine master e l'autenticazione basata sull'appartenenza. Il framework di MVC è definito nel **System** dello spazio dei nomi ed è una parte fondamentale, supportata delle **System. Web** dello spazio dei nomi.   
  
MVC è uno schema progettuale standard che hanno familiari con molti sviluppatori. Alcuni tipi di applicazioni Web trarranno vantaggio dal framework di MVC. Gli altri continueranno a usare il modello ASP.NET tradizionale basato su Web Form e postback. Altri tipi di applicazioni Web combineranno i due approcci; Nessuno dei due esclude l'altro.   
  
Il framework di MVC include i componenti seguenti:


[![Richiamo di un'azione del controller che prevede un valore di parametro](asp-net-mvc-overview/_static/image1.jpg)](asp-net-mvc-overview/_static/image1.png)

**Figura 01**: richiamare un'azione del controller che prevede un valore di parametro ([fare clic per visualizzare l'immagine con dimensioni normali](asp-net-mvc-overview/_static/image2.png))


- **I modelli**. Gli oggetti modello sono le parti dell'applicazione che implementano la logica per il dominio di applicazione s dei dati. Spesso, gli oggetti modello recupero e archiviano lo stato del modello in un database. Ad esempio, un oggetto Product potrebbe recuperare informazioni da un database, operare su di esso e quindi scrivere informazioni aggiornate in una tabella Products in SQL Server.

Nelle applicazioni di piccole dimensioni, il modello è spesso una separazione concettuale anziché fisica. Ad esempio, se l'applicazione solo legge un set di dati e lo invia alla visualizzazione, l'applicazione non è un livello di modello fisico e classi associate. In tal caso, il set di dati assume il ruolo di un oggetto model.

- **Viste**. Le visualizzazioni sono i componenti che consentono di visualizzare l'interfaccia utente di applicazione s (UI). In genere, questa interfaccia utente viene creata da dati del modello. Un esempio sarebbe una visualizzazione di modifica di una tabella di prodotti che vengono visualizzate le caselle di testo, elenchi a discesa e caselle di controllo basate sullo stato corrente di un oggetto di prodotti.

- **I controller**. I controller sono i componenti che gestiscono l'interazione dell'utente, usare il modello e selezionano infine una visualizzazione per il rendering dell'interfaccia utente. In un'applicazione MVC la visualizzazione presenta solo le informazioni, mentre il controller gestisce e risponde all'input e all'interazione dell'utente. Ad esempio, il controller gestisce i valori di stringa di query e passa questi valori per il modello, che a sua volta esegue query nel database utilizzando i valori.

Il pattern MVC consente di creare applicazioni che separano i diversi aspetti dell'applicazione (logica di input, logica di business e logica dell'interfaccia utente), fornendo un accoppiamento libero tra questi elementi. Lo schema specifica in cui ogni tipo di logica deve trovarsi nell'applicazione. La logica dell'interfaccia utente risiede nella vista. La logica di input risiede nel controller. La logica di business risiede nel modello. Questa separazione consente di gestire la complessità quando si compila un'applicazione, perché ti consente di concentrarsi su un aspetto dell'implementazione alla volta. Ad esempio, è possibile concentrarsi sulla visualizzazione senza a seconda della logica di business.   
  
Oltre a gestire la complessità, il modello MVC rende più semplice testare le applicazioni rispetto al test di un'applicazione Web ASP.NET basata su Web Form. In un'applicazione Web ASP.NET basata su Web Form, ad esempio, viene utilizzata un'unica classe per visualizzare l'output e per rispondere all'input dell'utente. La scrittura di test automatizzati per le applicazioni basate su Web Form ASP.NET può risultare complessa, perché per testare una singola pagina, è necessario creare un'istanza della classe di pagina, tutti i relativi controlli figlio e classi dipendenti aggiuntive nell'applicazione. Poiché in questo numero di classi viene create istanze per l'esecuzione della pagina, può essere difficile scrivere test indirizzati esclusivamente su singole parti dell'applicazione. Test per le applicazioni basate su Web Form ASP.NET può pertanto essere più difficili da implementare rispetto ai test in un'applicazione MVC. Inoltre, i test in un'applicazione basata su Web Form ASP.NET richiedono un server Web. Il framework MVC disaccoppia i componenti e utilizza in modo massiccio di interfacce, che consente di testare singoli componenti in modo isolato dal resto del framework.   
  
L'accoppiamento libero tra i tre componenti principali di un'applicazione MVC favorisce inoltre lo sviluppo parallelo. Ad esempio, uno sviluppatore può lavorare sulla visualizzazione, uno sviluppatore secondo gli strumenti per la logica del controller e un terzo può concentrarsi sulla logica di business nel modello.

## <a name="deciding-when-to-create-an-mvc-application"></a>Decidere quando creare un'applicazione MVC

È necessario considerare con attenzione se implementare un'applicazione Web usando il framework ASP.NET MVC o il modello Web Form ASP.NET. Il framework MVC non sostituisce il modello Web Form; è possibile usare uno dei due framework per applicazioni Web. (Se hai applicazioni esistenti basate su Web Form, queste continueranno a funzionare esattamente come in precedenza.)   
  
Prima di decidere di utilizzare il framework di MVC o il modello Web Form per un sito Web specifico, considerare i vantaggi offerti da questi due approcci.

### <a name="advantages-of-an-mvc-based-web-application"></a>Vantaggi di un'applicazione Web basata su MVC

Il framework ASP.NET MVC offre i vantaggi seguenti:

- Rende più facile da gestire la complessità mediante la suddivisione di un'applicazione nel modello, la visualizzazione e il controller.
- Non usa lo stato di visualizzazione o form basati su server. Ciò rende il framework di MVC ideale per gli sviluppatori che intendono controllo completo sul comportamento di un'applicazione.
- Usa un modello Front Controller che elabora le richieste dell'applicazione Web tramite un singolo controller. In questo modo è possibile progettare un'applicazione che supporta l'infrastruttura di routing avanzata. Per altre informazioni, vedere [Front Controller](https://go.microsoft.com/fwlink/?LinkId=106357 "Front Controller") sul sito Web MSDN.
- Fornisce un supporto migliore per lo sviluppo basato su test (TDD).
- Funziona anche per le applicazioni Web che sono supportate dal team di sviluppatori e Designer Web che richiedono un elevato grado di controllare il comportamento dell'applicazione di grandi dimensioni.

### <a name="advantages-of-a-web-forms-based-web-application"></a>Vantaggi di un'applicazione Web basata su moduli Web

Il framework basato su Web Form offre i vantaggi seguenti:

- Supporta un modello di eventi che mantiene lo stato su HTTP, con conseguenti vantaggi per lo sviluppo di applicazioni Web line-of-business. L'applicazione basata su Web Form fornisce decine di eventi che sono supportati in centinaia di controlli server.
- Usa un modello Page Controller che aggiunge funzionalità a singole pagine. Per altre informazioni, vedere [Page Controller](https://go.microsoft.com/fwlink/?LinkId=106359 "Page Controller") sul sito Web MSDN.
- Usa lo stato di visualizzazione o form basati su server, che possono semplificarne la gestione delle informazioni sullo stato.
- Funziona anche per piccoli team di sviluppatori e progettisti che vogliono sfruttare i vantaggi del numero elevato di componenti disponibili per lo sviluppo rapido di applicazioni.
- In generale, è meno complesso per lo sviluppo di applicazioni, poiché i componenti (il **pagina** classe, controlli e così via) sono strettamente integrati e necessitano in genere meno codice rispetto al modello MVC.

## <a name="features-of-the-aspnet-mvc-framework"></a>Funzionalità del Framework ASP.NET MVC

Il framework ASP.NET MVC offre le funzionalità seguenti:

- Separazione delle attività dell'applicazione (logica di input, logica di business e logica dell'interfaccia utente), testabilità e sviluppo basato su test (TDD) per impostazione predefinita. Tutti i contratti principali nel framework di MVC sono basati sull'interfaccia e possono essere testati tramite oggetti fittizi, ovvero oggetti simulati che imitano il comportamento degli oggetti effettivi nell'applicazione. È possibile unit test dell'applicazione senza dover eseguire il controller in un processo ASP.NET, rendendo gli unit test veloci e flessibili. È possibile usare qualsiasi framework di unit test compatibile con .NET Framework.
- Un framework estendibile e collegabile. I componenti del framework ASP.NET MVC sono progettati in modo che possano essere facilmente sostituiti o personalizzati. È possibile collegare il proprio motore di visualizzazione, criteri di routing di URL, la serializzazione dei parametri del metodo di azione e altri componenti. Il framework ASP.NET MVC supporta inoltre l'utilizzo dei modelli contenitore Dependency Injection (DI) e Inversion of Control (IOC). L'inserimento delle dipendenze consente di inserire oggetti in una classe, anziché basarsi sulla classe per creare l'oggetto stesso. Il modello IOC specifica che se un oggetto richiede un altro oggetto, il primo deve ottenere il secondo oggetto da un'origine esterna, ad esempio un file di configurazione. In questo modo le operazioni di testing.
- Un componente di mapping di URL potenti che ti permette di creare applicazioni con URL comprensibili e disponibili per la ricerca. URL non è necessario includere estensioni di file e sono progettati per supportare modelli di denominazione di URL che funzionano bene per la ricerca engine optimization (SEO) e REST representational state transfer () addressing.
- Supporto per l'utilizzo del markup nella pagina ASP.NET esistente (file con estensione aspx), il controllo utente (file con estensione ascx) e file di markup (file con estensione master) della pagina master come modelli di visualizzazione. È possibile usare le funzionalità ASP.NET esistenti con il framework ASP.NET MVC, ad esempio pagine master annidate, espressioni inline (&lt;% = %&gt;), controlli server dichiarativi, modelli, associazione dati, localizzazione e così via.
- Supporto per le funzionalità ASP.NET esistenti. ASP.NET MVC consente di usare funzionalità quali l'autenticazione basata su form e l'autenticazione di Windows, autorizzazione URL, l'appartenenza e ruoli, output e la memorizzazione nella cache di dati, gestione dello stato sessione e profilo, il monitoraggio dell'integrità, il sistema di configurazione e il provider architettura.
