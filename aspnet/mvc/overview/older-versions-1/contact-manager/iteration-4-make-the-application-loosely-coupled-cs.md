---
uid: mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-cs
title: "Iterazione #4: verificare l'applicazione ad accoppiamento debole (c#) | Documenti Microsoft"
author: microsoft
description: In questa terza iterazione, possiamo usufruire dei diversi modelli di progettazione di software per renderne più semplice gestire e modificare l'applicazione Gestione contatti. Per...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: 829f589f-e201-4f6e-9ae6-08ae84322065
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-cs
msc.type: authoredcontent
ms.openlocfilehash: 33221c6c3326c7034fe013f152579828e2fc8a3a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="iteration-4--make-the-application-loosely-coupled-c"></a>Iterazione #4: verificare l'applicazione ad accoppiamento debole (c#)
====================
by [Microsoft](https://github.com/microsoft)

[Scaricare il codice](iteration-4-make-the-application-loosely-coupled-cs/_static/contactmanager_4_cs1.zip)

> In questa terza iterazione, possiamo usufruire dei diversi modelli di progettazione di software per renderne più semplice gestire e modificare l'applicazione Gestione contatti. Ad esempio, si effettua il refactoring l'applicazione di utilizzare il modello di Repository e il modello di inserimento di dipendenze.


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Creazione di un'applicazione ASP.NET MVC di gestione dei contatti (c#)

In questa serie di esercitazioni, si compila un'intera applicazione di gestione dei contatti dall'inizio completamento. L'applicazione Contact Manager consente di archiviare le informazioni di contatto - nomi, i numeri di telefono e indirizzi di posta elettronica: per un elenco di persone.

È compilare l'applicazione più iterazioni. A ogni iterazione, gradualmente è migliorare l'applicazione. L'obiettivo di questo approccio iterazione più è che consentono di comprendere il motivo per ogni modifica.

- Iterazione #1 - creare l'applicazione. Nella prima iterazione, verranno create Contact Manager in modo più semplice possibile. Viene aggiunto il supporto per le operazioni di database basic: creazione, lettura, aggiornamento ed eliminazione (CRUD).

- Iterazione #2 - verificare l'applicazione l'aspetto. In questa iterazione, è migliorare l'aspetto dell'applicazione modificando il valore predefinito di pagina master di visualizzazione ASP.NET MVC e foglio di stile CSS.

- Iterazione #3 - aggiungere la convalida dei form. Nella terza iterazione, è aggiungere la convalida di form di base. È impedire agli utenti di inviare un modulo senza completare i campi modulo necessari. È inoltre possibile convalidare gli indirizzi di posta elettronica e numeri di telefono.

- Iterazione #4: verificare l'applicazione ad accoppiamento debole. In questa terza iterazione, possiamo usufruire dei diversi modelli di progettazione di software per renderne più semplice gestire e modificare l'applicazione Gestione contatti. Ad esempio, si effettua il refactoring l'applicazione di utilizzare il modello di Repository e il modello di inserimento di dipendenze.

- Iterazione #5 - creare unit test. Nella quinta iterazione, si rende l'applicazione di più facile da gestire e modificare tramite l'aggiunta di unit test. È simulare il nostro classi del modello di dati e generare unit test per i controller e logica di convalida.

- Iterazione 6 # - utilizzare sviluppo basato su test. In questa iterazione sesto è aggiungere nuove funzionalità per l'applicazione scrivendo unit test prima e la scrittura di codice per gli unit test. In questa iterazione, è aggiungere gruppi di contatti.

- Iterazione #7 - aggiunta di funzionalità Ajax. Nella settima iterazione, è migliorare la velocità di risposta e prestazioni dell'applicazione aggiunta del supporto per Ajax.

## <a name="this-iteration"></a>Questa iterazione

In questa iterazione quarto dell'applicazione Contact Manager, è effettuare il refactoring per rendere l'applicazione più strettamente collegato. Quando un'applicazione ad accoppiamento è ridotto, è possibile modificare il codice in un'unica parte dell'applicazione senza dover modificare il codice in altre parti dell'applicazione. Applicazioni loosely coupled sono molto più adattabile ai modificare.

Attualmente, tutta la logica di accesso e la convalida di dati utilizzata dall'applicazione Contact Manager è contenuto nelle classi controller. Si tratta di una buona idea. Ogni volta che si desidera modificare una parte dell'applicazione, si rischia di introdurre bug in un'altra parte dell'applicazione. Ad esempio, se si modifica la logica di convalida, si rischia di creare nuovi bug nella logica di accesso o controller di dati.

> [!NOTE] 
> 
> (Criteri di restrizione software), una classe non deve avere mai più di uno dei motivi per modificare. La combinazione di controller, convalida e logica del database è una violazione del principio di responsabilità singolo massa.


Esistono vari motivi che potrebbe essere necessario modificare l'applicazione. Potrebbe essere necessario aggiungere una nuova funzionalità a un'applicazione, potrebbe essere necessario correggere un bug nell'applicazione o potrebbe essere necessario modificare l'implementazione di una funzionalità dell'applicazione. Le applicazioni sono raramente statiche. Tendono a crescere e modificato nel tempo.

Si supponga, ad esempio, che si decide di modificare l'implementazione del livello di accesso ai dati. Diritto a questo punto, l'applicazione Gestione contatti Usa Microsoft Entity Framework per accedere al database. Tuttavia, è possibile decidere di eseguire la migrazione a una tecnologia di accesso ai dati nuovi o alternativo, ad esempio ADO.NET Data Services o NHibernate. Tuttavia, poiché il codice di accesso ai dati non è isolato dal codice di convalida e controller, è possibile modificare il codice di accesso ai dati nell'applicazione senza modificare altro codice che non è direttamente correlato all'accesso ai dati.

Quando un'applicazione è ad accoppiamento debole, d'altra parte, è possibile apportare modifiche a una parte di un'applicazione senza interessare le altre parti di un'applicazione. Ad esempio, è possibile passare tecnologie di accesso ai dati senza modificare la logica di convalida o controller.

In questa iterazione, possiamo usufruire dei diversi modelli di progettazione di software che consentono di effettuare il refactoring l'applicazione Contact Manager in un'applicazione più regime di controllo. Al termine, il responsabile del contatto vinto t operazioni che è non effettuare prima. Tuttavia, è possibile modificare l'applicazione più facilmente in futuro.

> [!NOTE] 
> 
> Refactoring è il processo di un'applicazione in modo che non perdono le funzionalità esistenti di riscrittura.


## <a name="using-the-repository-software-design-pattern"></a>Utilizzando il modello di progettazione del Software del Repository

La prima modifica è possa sfruttare i vantaggi di un modello di progettazione software denominato modello di Repository. Si userà il modello di Repository per isolare il codice di accesso ai dati dal resto dell'applicazione.

Implementazione del modello di Repository, è necessario per completare i due passaggi seguenti:

1. Creare un'interfaccia
2. Creare una classe concreta che implementa l'interfaccia

In primo luogo, è necessario creare un'interfaccia che descrive tutti i metodi di accesso ai dati che è necessario eseguire. L'interfaccia IContactManagerRepository è contenuto in elenco 1. Questa interfaccia vengono descritti i cinque metodi: CreateContact(), DeleteContact(), EditContact(), GetContact e ListContacts().

**Elenco 1 - Models\IContactManagerRepositiory.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample1.cs)]

Successivamente, è necessario creare una classe concreta che implementa l'interfaccia IContactManagerRepository. Poiché si sta usando Microsoft Entity Framework per accedere al database, si creerà una nuova classe denominata EntityContactManagerRepository. Questa classe è contenuta nel listato 2.

**Il listato 2 - Models\EntityContactManagerRepository.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample2.cs)]

Si noti che la classe EntityContactManagerRepository implementa l'interfaccia IContactManagerRepository. La classe implementa tutti e cinque i metodi descritti da quell'interfaccia.

Ci si potrebbe chiedere perché è necessario preoccuparsi di un'interfaccia. Motivo per cui è necessario creare un'interfaccia sia una classe che lo implementa

Con un'unica eccezione, il resto dell'applicazione interagisce con l'interfaccia e non la classe concreta. Anziché chiamare i metodi esposti dalla classe EntityContactManagerRepository, verrà chiamato i metodi esposti dall'interfaccia IContactManagerRepository.

In questo modo, è possibile implementare l'interfaccia con una nuova classe senza dover modificare il resto dell'applicazione. In seguito, ad esempio, potrebbe essere opportuno implementare una classe DataServicesContactManagerRepository che implementa l'interfaccia IContactManagerRepository. La classe DataServicesContactManagerRepository è possibile utilizzare ADO.NET Data Services per accedere a un database anziché Microsoft Entity Framework.

Se il codice dell'applicazione è programmato all'interfaccia IContactManagerRepository anziché la classe concreta EntityContactManagerRepository senza modificare la parte del codice è possibile passare le classi concrete. Ad esempio, è possibile passare dalla classe EntityContactManagerRepository alla classe DataServicesContactManagerRepository senza modificare la logica di convalida o l'accesso ai dati.

Programmazione in base a interfacce (astrazioni) anziché le classi concrete rende più flessibile per modificare l'applicazione.

> [!NOTE] 
> 
> È possibile creare rapidamente un'interfaccia da una classe concreta all'interno di Visual Studio selezionando l'opzione di menu eseguire il refactoring Estrai interfaccia. Ad esempio, è possibile creare la classe EntityContactManagerRepository prima e quindi utilizzare Estrai interfaccia per generare automaticamente l'interfaccia IContactManagerRepository.


## <a name="using-the-dependency-injection-software-design-pattern"></a>Utilizzando il modello di progettazione Software inserimento di dipendenza

Ora che il codice di accesso ai dati di stato migrazione in una classe separata di Repository, è necessario modificare il controller di contatto per l'utilizzo di questa classe. Ci si avvale di un modello di progettazione software denominato inserimento di dipendenze per utilizzare la classe di Repository in questo controller.

Il controller di contatto modificato è contenuto nel listato 3.

**Elenco di 3 - Controllers\ContactController.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample3.cs)]

Si noti che il controller di contatto nel listato 3 dispone di due costruttori. Il primo costruttore passa un'istanza concreta dell'interfaccia IContactManagerRepository per il secondo costruttore. La classe utilizza controller contatto *costruttore Dependency Injection*.

Il metodo e solo sul posto che viene usata la classe di EntityContactManagerRepository è il primo costruttore. Il resto della classe utilizza l'interfaccia IContactManagerRepository anziché la classe EntityContactManagerRepository concreta.

Questo semplifica passare le implementazioni della classe IContactManagerRepository in futuro. Se si desidera utilizzare la classe DataServicesContactRepository anziché la classe EntityContactManagerRepository, modificare solo il primo costruttore.

Inserimento di dipendenze di costruttore rende inoltre la classe controller contatto molto testabile. Negli unit test, è possibile creare il controller di contatto passando un'implementazione della classe IContactManagerRepository fittizia. Questa funzionalità di, Dependency Injection sarà molto importante per noi nell'iterazione successiva quando si compila unit test per l'applicazione Gestione contatti.

> [!NOTE] 
> 
> Se si desidera separare completamente la classe di controller di contatto da una particolare implementazione dell'interfaccia IContactManagerRepository quindi è possibile sfruttare un framework che supporta l'inserimento di dipendenze, ad esempio StructureMap o Microsoft Entity Framework (MEF). L'utilizzo di un framework di inserimento di dipendenze, è non necessario mai fare riferimento a una classe concreta nel codice.


## <a name="creating-a-service-layer"></a>Creazione di un livello di servizio

È possibile notare che la logica di convalida è ancora confuse con la logica del controller nella classe controller modificato nel listato 3. Per lo stesso motivo che è buona norma isolare la logica di accesso ai dati, è buona norma isolare la logica di convalida.

Per risolvere questo problema, è possibile creare un apposito [ *livello di servizio*](http://martinfowler.com/eaaCatalog/serviceLayer.html). Il livello di servizio è un livello separato che è possibile inserire tra il controller e classi del repository. Il livello di servizio contiene la logica di business inclusi tutta la logica di convalida.

Il ContactManagerService è contenuta nel listato 4. Contiene la logica di convalida dalla classe controller contatto.

**Listato 4 - Models\ContactManagerService.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample4.cs)]

Si noti che il costruttore per il ContactManagerService richiede un ValidationDictionary. Il livello di servizio comunica con il livello di controller tramite questo ValidationDictionary. Quando si prende in esame il modello di espressione Decorator prende in esame ValidationDictionary in dettaglio nella sezione seguente.

Si noti inoltre che il ContactManagerService implementa l'interfaccia IContactManagerService. È sempre opportuno programmare con interfacce anziché classi concrete. Altre classi nell'applicazione Gestione contatti non interagiscono direttamente con la classe ContactManagerService. In alternativa, con un'unica eccezione, il resto dell'applicazione Contact Manager programmato all'interfaccia IContactManagerService.

L'interfaccia IContactManagerService è contenuta nel listato 5.

**Nel listato 5 - Models\IContactManagerService.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample5.cs)]

La classe controller contatto modificata è contenuta nel listato 6. Si noti che il controller di contatto non interagisce con il repository ContactManager. Al contrario, il controller di contatto interagisce con il servizio di ContactManager. Ogni livello è isolata al massimo da altri livelli.

**Elenco 6 - Controllers\ContactController.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample6.cs)]

L'applicazione non viene eseguito afoul dell'unica responsabilità principio (criteri di restrizione software). Il controller di contatto nel listato 6 è stata rimossa ogni responsabilità diverso dal controllo del flusso di esecuzione dell'applicazione. Tutta la logica di convalida è stata rimossa dal controller di contatto e inserita nel livello di servizio. Tutta la logica del database è stato inserito nel livello di repository.

## <a name="using-the-decorator-pattern"></a>Usa il modello di espressione Decorator

Si vuole essere in grado di separare completamente il livello di servizio di livello il controller. In sostanza, si dovrebbe essere in grado di compilare il livello di servizio in un assembly separato dal livello controller senza la necessità di aggiungere un riferimento all'applicazione MVC.

Tuttavia, il livello di servizio deve essere in grado di passare i messaggi di errore di convalida al livello di controller. Come è possibile abilitare il livello di servizio comunicare i messaggi di errore di convalida senza l'accoppiamento tra il controller e il livello di servizio? È possibile sfruttare un modello di progettazione software denominato il [modello di espressione Decorator](http://en.wikipedia.org/wiki/Decorator_pattern).

Un controller di Usa un ModelStateDictionary denominato ModelState per rappresentare gli errori di convalida. Pertanto, si potrebbe essere tentati di passare ModelState dal livello dei controller a livello di servizio. Tuttavia, utilizzano ModelState nel livello del servizio renderebbe il livello di servizio dipendente da una funzionalità del framework di MVC ASP.NET. Questo potrebbe essere non valido perché, a un giorno, è possibile utilizzare il livello di servizio con un'applicazione WPF anziché un'applicazione MVC ASP.NET. In tal caso, sarebbe t a cui fare riferimento il framework ASP.NET MVC per utilizzare la classe ModelStateDictionary.

Il modello di espressione Decorator consente di eseguire il wrapping di una classe esistente in una nuova classe per implementare un'interfaccia. Del progetto Contact Manager include la classe ModelStateWrapper contenuta nel listato 7. La classe ModelStateWrapper implementa l'interfaccia nel listato 8.

**Elenco 7 - Models\Validation\ModelStateWrapper.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample7.cs)]

**Elenco 8 - Models\Validation\IValidationDictionary.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample8.cs)]

Se si consente di esaminare listato 5 si potrà osservare che il livello di servizio ContactManager utilizza l'interfaccia IValidationDictionary in modo esclusivo. Il servizio ContactManager non dipende dalla classe ModelStateDictionary. Quando il controller di contattare il servizio ContactManager viene creato, il controller esegue il wrapping relativo ModelState simile al seguente:

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample9.cs)]

## <a name="summary"></a>Riepilogo

In questa iterazione, si non aggiungere alcuna nuova funzionalità per l'applicazione Gestione contatti. L'obiettivo di questa iterazione è stata per effettuare il refactoring dell'applicazione di gestione di contatto in modo che sia più facile da gestire e modificare.

È implementato per primo, il modello di progettazione del software del Repository. La migrazione di tutto il codice di accesso ai dati in una classe di repository ContactManager separata.

La logica di convalida è inoltre isolata dalla logica controller. È stato creato un livello di servizio distinto che contiene tutto il codice di convalida. Il livello di controller interagisce con il livello di servizio e il livello di servizio interagisce con il livello di repository.

Quando abbiamo creato il livello di servizio, abbiamo sfruttato il modello di espressione Decorator per isolare ModelState da questo livello di servizio. Il livello di servizio, è programmato all'interfaccia IValidationDictionary anziché ModelState.

Infine, abbiamo sfruttato un modello di progettazione software denominato il modello di inserimento di dipendenze. Questo modello consente di programmare interfacce (astrazioni) anziché le classi concrete. Implementazione dello schema progettuale Dependency Injection inoltre rende il codice più verificabili. Nell'iterazione successiva, è aggiungere unit test al progetto.

> [!div class="step-by-step"]
> [Precedente](iteration-3-add-form-validation-cs.md)
> [Successivo](iteration-5-create-unit-tests-cs.md)
