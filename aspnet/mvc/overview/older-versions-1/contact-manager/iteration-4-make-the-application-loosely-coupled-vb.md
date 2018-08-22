---
uid: mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-vb
title: "Iterazione #4-rendere l'applicazione regime di controllo (VB) | Microsoft Docs"
author: microsoft
description: In questo quarta iterazione, possiamo usufruire dei diversi modelli di progettazione di software per renderne più semplice da gestire e modificare l'applicazione Contact Manager. Per...
ms.author: riande
ms.date: 02/20/2009
ms.assetid: 92c70297-4430-4e4e-919a-9c2333a8d09a
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-vb
msc.type: authoredcontent
ms.openlocfilehash: be6ddbdfbe8da33871355c2a7917a7ce7008d81b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826285"
---
<a name="iteration-4--make-the-application-loosely-coupled-vb"></a>Iterazione #4-rendere l'applicazione regime di controllo (VB)
====================
da [Microsoft](https://github.com/microsoft)

[Scaricare il codice](iteration-4-make-the-application-loosely-coupled-vb/_static/contactmanager_4_vb1.zip)

> In questo quarta iterazione, possiamo usufruire dei diversi modelli di progettazione di software per renderne più semplice da gestire e modificare l'applicazione Contact Manager. Ad esempio, è eseguire il refactoring l'applicazione per usare il modello di Repository e il modello di inserimento delle dipendenze.


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Creazione di un'applicazione di gestione dei contatti ASP.NET MVC (VB)

In questa serie di esercitazioni, creiamo un'intera applicazione di gestione dei contatti dall'inizio alla fine. L'applicazione Contact Manager consente di archiviare le informazioni di contatto - nomi, i numeri di telefono e indirizzi di posta elettronica - per un elenco di persone.

Si compilerà l'applicazione a varie iterazioni. A ogni iterazione, abbiamo migliorare gradualmente l'applicazione. L'obiettivo di questo approccio iterazione più è che consentono di comprendere il motivo per ogni modifica.

- Iterazione #1 - creare l'applicazione. Nella prima iterazione, verranno create Contact Manager nel modo più semplice possibile. Viene aggiunto il supporto per le operazioni di base dei database: crea, lettura, aggiornamento ed eliminazione (CRUD).

- Iterazione #2 - migliorare l'applicazione aspetto interessante. In questa iterazione è migliorare l'aspetto dell'applicazione modificando il valore predefinito di pagina master visualizzazione MVC ASP.NET e foglio di stile CSS.

- Iterazione #3 - aggiungere la convalida dei form. Nella terza iterazione, aggiungiamo la convalida dei form di base. Persone è impedire l'invio di un modulo senza completare i campi del modulo richiesto. È anche convalidare gli indirizzi di posta elettronica e numeri di telefono.

- Iterazione #4 - rendere l'applicazione a regime. In questo quarta iterazione, possiamo usufruire dei diversi modelli di progettazione di software per renderne più semplice da gestire e modificare l'applicazione Contact Manager. Ad esempio, è eseguire il refactoring l'applicazione per usare il modello di Repository e il modello di inserimento delle dipendenze.

- Iterazione #5 - creare unit test. Nell'iterazione del quinto, si semplifica l'applicazione di manutenzione e la modifica mediante l'aggiunta di unit test. È simulare le classi di modello di dati e generare unit test per i controller e logica di convalida.

- Iterazione #6 - usare lo sviluppo basato su test. In questa iterazione sesta, viene aggiunto nuove funzionalità all'applicazione scrivendo unit test prima e scrivere codice sull'unit test. In questa iterazione, viene aggiunto gruppi di contatti.

- Iterazione #7 - aggiungere funzionalità Ajax. Nell'iterazione del settimo, aggiungendo il supporto per Ajax è migliorare la velocità di risposta e le prestazioni dell'applicazione.

## <a name="this-iteration"></a>Questa iterazione

In questo quarta iterazione dell'applicazione Contact Manager, è effettuare il refactoring per rendere l'applicazione più regime di controllo. Quando un'applicazione è regime di controllo, è possibile modificare il codice in una parte dell'applicazione senza la necessità di modificare il codice in altre parti dell'applicazione. Applicazioni loosely coupled sono più resiliente a cambiare.

Attualmente, tutta la logica di accesso e la convalida dei dati utilizzata dall'applicazione Contact Manager è contenuto nelle classi controller. Questa è una buona idea. Ogni volta che è necessario modificare una parte dell'applicazione, si rischia di introdurre bug in un'altra parte dell'applicazione. Ad esempio, se si modifica la logica di convalida, si rischia di creare nuovi bug nella logica di accesso o un controller di dati.

> [!NOTE] 
> 
> (SRP), una classe non deve avere mai più di un motivo per cambiare. La combinazione di controller, convalida e logica del database è una violazione del principio di singola responsabilità enorme.


Esistono diversi motivi per cui potrebbe essere necessario modificare l'applicazione. Potrebbe essere necessario aggiungere una nuova funzionalità all'applicazione, potrebbe essere necessario correggere un bug nell'applicazione o potrebbe essere necessario modificare la modalità in cui viene implementata una funzionalità dell'applicazione. Le applicazioni sono raramente statiche. Tendono a crescere e modificato nel corso del tempo.

Si supponga, ad esempio, che si decide di cambiare modalità di implementazione del livello di accesso ai dati. Diritto a questo punto, l'applicazione Contact Manager utilizza Microsoft Entity Framework per accedere al database. Tuttavia, è possibile decidere di eseguire la migrazione a una tecnologia di accesso ai dati nuovi o alternativo, ad esempio ADO.NET Data Services o NHibernate. Tuttavia, poiché il codice di accesso ai dati non è isolato dal codice di convalida e il controller, non è alcun modo possibile modificare il codice di accesso ai dati nell'applicazione senza modificare l'altro codice che non è direttamente correlato all'accesso ai dati.

Quando un'applicazione è regime di controllo, d'altra parte, è possibile apportare modifiche a una parte di un'applicazione senza modificare altre parti di un'applicazione. Ad esempio, è possibile passare tecnologie di accesso ai dati senza modificare la logica di convalida o un controller.

In questa iterazione, possiamo usufruire dei diversi modelli di progettazione software che consentono di effettuare il refactoring applicazione Contact Manager in un'applicazione più regime. Quando è stata completata, ha vinto t di Contact Manager eseguire alcuna operazione che è t non prima. Tuttavia, sarà possibile modificare l'applicazione più facilmente in futuro.

> [!NOTE] 
> 
> Il refactoring è il processo di riscrittura di un'applicazione in modo che non perdere le funzionalità esistenti.


## <a name="using-the-repository-software-design-pattern"></a>Usando lo schema progettuale Repository Software

La prima modifica consiste nel poter sfruttare un modello di progettazione software denominato modello di Repository. Si userà il modello di Repository per isolare il codice di accesso ai dati dal resto della nostra applicazione.

Implementazione del pattern di Repository richiede di completare i due passaggi seguenti:

1. Creare un'interfaccia
2. Creare una classe concreta che implementa l'interfaccia

In primo luogo, è necessario creare un'interfaccia che descrive tutti i metodi di accesso di dati che è necessario eseguire. L'interfaccia IContactManagerRepository è contenuta nel listato 1. Questa interfaccia vengono descritti i cinque metodi: CreateContact() DeleteContact(), EditContact(), GetContact e ListContacts().

**Listato 1 - Models\IContactManagerRepository.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample1.vb)]

Successivamente, è necessario creare una classe concreta che implementa l'interfaccia IContactManagerRepository. Poiché si sta usando Microsoft Entity Framework per accedere al database, si creerà una nuova classe denominata EntityContactManagerRepository. Questa classe è contenuta nel listato 2.

**Listato 2 - Models\EntityContactManagerRepository.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample2.vb)]

Si noti che la classe EntityContactManagerRepository implementi l'interfaccia IContactManagerRepository. La classe implementa tutti e cinque i metodi descritti da quell'interfaccia.

Ci si potrebbe chiedere perché è necessario preoccuparsi di un'interfaccia. Il motivo per cui è necessario creare un'interfaccia e una classe che lo implementa

Con un'unica eccezione, il resto della nostra applicazione interagisce con l'interfaccia e non sulla classe concreta. Invece di chiamare i metodi esposti dalla classe EntityContactManagerRepository, verrà chiamato i metodi esposti dall'interfaccia IContactManagerRepository.

In questo modo, è possibile implementare l'interfaccia con una nuova classe senza dover modificare il resto dell'applicazione. In alcuni data futura, ad esempio, si potrebbe essere opportuno implementare una classe DataServicesContactManagerRepository che implementa l'interfaccia IContactManagerRepository. La classe DataServicesContactManagerRepository potrebbe utilizzare ADO.NET Data Services per accedere a un database invece di Microsoft Entity Framework.

Se il codice dell'applicazione è programmato in base all'interfaccia IContactManagerRepository anziché la classe EntityContactManagerRepository concreta è possibile passare le classi concrete senza modificare la parte del codice. Ad esempio, è possibile passare dalla classe EntityContactManagerRepository alla classe DataServicesContactManagerRepository senza modificare la logica di accesso o la convalida dei dati.

La programmazione rispetto a interfacce (astrazioni) anziché le classi concrete rende più resiliente per modificare l'applicazione.

> [!NOTE] 
> 
> È possibile creare rapidamente un'interfaccia da una classe concreta all'interno di Visual Studio selezionando l'opzione di menu eseguire il refactoring Estrai interfaccia. Ad esempio, è possibile creare la classe EntityContactManagerRepository prima e quindi usare Estrai interfaccia per generare l'interfaccia IContactManagerRepository automaticamente.


## <a name="using-the-dependency-injection-software-design-pattern"></a>Usando il modello di progettazione Software di inserimento delle dipendenze

Ora che è stata eseguita la migrazione di nostro codice di accesso ai dati a una classe di Repository separata, è necessario modificare il controller di contatto per l'uso di questa classe. Si sarà possibile avvalersi di un modello di progettazione software denominato inserimento delle dipendenze per usare la classe di Repository nel controller.

Il controller di contatto modificato è contenuto nel listato 3.

**Listato 3 - Controllers\ContactController.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample3.vb)]

Si noti che il controller di contatto nel listato 3 ha due costruttori. Il primo costruttore passa un'istanza concreta dell'interfaccia IContactManagerRepository per il secondo costruttore. La classe Usa controller contattare *inserimento delle dipendenze del costruttore*.

Solo sul posto che viene usata la classe EntityContactManagerRepository e quello è il primo costruttore. Il resto della classe Usa l'interfaccia IContactManagerRepository anziché la classe EntityContactManagerRepository concreta.

Questo rende facile passare le implementazioni della classe IContactManagerRepository in futuro. Se si desidera usare la classe DataServicesContactRepository anziché la classe EntityContactManagerRepository, modificare solo il primo costruttore.

Inserimento delle dipendenze del costruttore rende inoltre alla classe controller Contact molto testabile. Negli unit test, è possibile creare un'istanza del controller di contatto passando un'implementazione fittizia della classe IContactManagerRepository. Questa funzionalità di inserimento delle dipendenze è molto importante per noi nell'iterazione successiva quando si compilano gli unit test per l'applicazione Contact Manager.

> [!NOTE] 
> 
> Se si vuole separare completamente la classe di controller di contatto da una particolare implementazione dell'interfaccia IContactManagerRepository quindi è possibile sfruttare un framework che supporta l'inserimento di dipendenze, ad esempio StructureMap o Microsoft Entity Framework (MEF). Grazie all'uso di un framework di inserimento delle dipendenze, è non necessario mai fare riferimento a una classe concreta nel codice.


## <a name="creating-a-service-layer"></a>Creazione di un livello di servizio

Si potrebbe aver notato che la logica di convalida è ancora confuse tra la logica del controller nella classe controller modificato nel listato 3. Per lo stesso motivo che è una buona idea per isolare la logica di accesso ai dati, è buona norma isolare la logica di convalida.

Per risolvere questo problema, è possibile creare un oggetto separato [livello di servizio](http://martinfowler.com/eaaCatalog/serviceLayer.html). Il livello di servizio è un livello separato che possiamo inserire tra i controller e le classi di repository. Il livello di servizio contiene la logica di business tra cui tutta la logica di convalida.

Il ContactManagerService è contenuta nel listato 4. Contiene la logica di convalida dalla classe di controller di contatto.

**Listato 4 - Models\ContactManagerService.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample4.vb)]

Si noti che il costruttore per la ContactManagerService richiede un ValidationDictionary. Il livello di servizio comunica con il livello di controller tramite questo ValidationDictionary. Illustreremo ValidationDictionary in dettaglio nella sezione seguente quando viene illustrato il pattern Decorator.

Si noti inoltre che il ContactManagerService implementi l'interfaccia IContactManagerService. È sempre consigliabile cercare di programmare sulla base di interfacce anziché classi concrete. Altre classi nell'applicazione Contact Manager non interagiscono direttamente con la classe ContactManagerService. In alternativa, con un'eccezione, il resto dell'applicazione Contact Manager programmato in base all'interfaccia IContactManagerService.

L'interfaccia IContactManagerService è contenuta nel listato 5.

**Listato 5 - Models\IContactManagerService.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample5.vb)]

La classe controller contatto modificata è contenuta nel listato 6. Si noti che il controller di contatto non sono più interagisce con il repository ContactManager. Al contrario, il controller di contatto interagisce con il servizio ContactManager. Ogni livello isolato in quanto più possibile da altri livelli.

**Listato 6 - Controllers\ContactController.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample6.vb)]

Non è più l'applicazione viene eseguita afoul del principio singola responsabilità (SRP). Il controller di contatto nel listato 6 estratto di ogni responsabilità diverse da controllo dei flussi di esecuzione dell'applicazione. Tutta la logica di convalida ha stato rimossa dal controller di contatto e il push nel livello di servizio. Tutta la logica di database è stato sottoposto a push nel livello di repository.

## <a name="using-the-decorator-pattern"></a>Utilizzo del modello Decorator

Si vuole essere in grado di separare completamente il livello di servizio dal nostro livello di controller. In teoria, abbiamo dovremmo essere in grado di compilare il livello di servizio in un assembly separato dal nostro livello di controller senza la necessità di aggiungere un riferimento all'applicazione MVC.

Tuttavia, il livello di servizio deve essere in grado di passare i messaggi di errore di convalida al livello di controller. Come è possibile abilitare il livello di servizio comunicare i messaggi di errore di convalida senza l'accoppiamento tra i livelli di servizio e il controller? Possiamo usufruire dei vantaggi di un modello di progettazione software denominato il [pattern Decorator](http://en.wikipedia.org/wiki/Decorator_pattern).

Un controller Usa un ModelStateDictionary denominato ModelState per rappresentare errori di convalida. Pertanto, si potrebbe essere tentati di passare ModelState dal livello di controller per il livello di servizio. Tuttavia, utilizzando ModelState nel livello di servizio renderebbe il livello di servizio dipendente da una funzionalità del framework ASP.NET MVC. Sarebbe un grosso problema in quanto, un giorno, si potrebbe voler usare il livello di servizio con un'applicazione WPF invece di un'applicazione ASP.NET MVC. In tal caso, si sarebbe t desidera fare riferimento a framework di MVC ASP.NET per usare la classe ModelStateDictionary.

Il pattern Decorator consente di eseguire il wrapping di una classe esistente in una nuova classe per implementare un'interfaccia. Il progetto Contact Manager include la classe ModelStateWrapper contenuta nel listato 7. La classe ModelStateWrapper implementa l'interfaccia nel listato 8.

**Listato 7 - Models\Validation\ModelStateWrapper.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample7.vb)]

**Listato 8 - Models\Validation\IValidationDictionary.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample8.vb)]

Se esaminiamo Chiudi listato 5 quindi noterete che il livello di servizio ContactManager utilizza l'interfaccia IValidationDictionary in modo esclusivo. Il servizio ContactManager non dipende dalla classe ModelStateDictionary. Quando il controller di contattare il servizio ContactManager viene creato, il controller esegue il wrapping relativo ModelState simile al seguente:

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample9.vb)]

## <a name="summary"></a>Riepilogo

In questa iterazione, non è aggiungere alcuna nuova funzionalità per l'applicazione Contact Manager. L'obiettivo di questa iterazione è stata per effettuare il refactoring dell'applicazione Contact Manager in modo che sia più facile da gestire e modificare.

Abbiamo implementato per primo, lo schema progettuale Repository software. Abbiamo eseguito la migrazione di tutto il codice di accesso dati in una classe di repository ContactManager separata.

La logica di convalida è inoltre isolate dal nostro logica dei controller. È stato creato un livello di servizio separato che contiene tutto il codice di convalida. Il livello di controller interagisce con il livello di servizio e il livello di servizio interagisce con il livello di repository.

Quando abbiamo creato il livello di servizio, abbiamo sfruttato il pattern Decorator per isolare ModelState dal nostro livello di servizio. Il livello di servizio, sono programmate in base all'interfaccia IValidationDictionary anziché ModelState.

Infine, abbiamo sfruttato un modello di progettazione software denominato il modello di inserimento delle dipendenze. Questo modello consente di programmare sulla base di interfacce (astrazioni) anziché le classi concrete. Implementazione del pattern di progettazione di inserimento delle dipendenze rende inoltre il codice più testabili. Nell'iterazione successiva, aggiungiamo gli unit test per il progetto.

> [!div class="step-by-step"]
> [Precedente](iteration-3-add-form-validation-vb.md)
> [Successivo](iteration-5-create-unit-tests-vb.md)
