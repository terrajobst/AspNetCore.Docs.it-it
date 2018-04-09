---
uid: mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-cs
title: 'Iterazione #3: aggiunta della convalida del form (c#) | Documenti Microsoft'
author: microsoft
description: Nella terza iterazione, è aggiungere la convalida di form di base. È impedire agli utenti di inviare un modulo senza completare i campi modulo necessari. È inoltre possibile convalidare emai...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: 51a0d175-913b-43d8-95e3-840fb96ad1a9
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-cs
msc.type: authoredcontent
ms.openlocfilehash: b9353c32b2839fd760513982c5742bb8f521e94a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="iteration-3--add-form-validation-c"></a>Iterazione #3: aggiunta della convalida del form (c#)
====================
by [Microsoft](https://github.com/microsoft)

[Scaricare il codice](iteration-3-add-form-validation-cs/_static/contactmanager_3_cs1.zip)

> Nella terza iterazione, è aggiungere la convalida di form di base. È impedire agli utenti di inviare un modulo senza completare i campi modulo necessari. È inoltre possibile convalidare gli indirizzi di posta elettronica e numeri di telefono.


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

In questa seconda iterazione dell'applicazione Contact Manager, è aggiungere la convalida di form di base. È impedire agli utenti di inviare un contatto senza l'immissione di valori per i campi modulo necessari. È inoltre possibile convalidare i numeri di telefono e indirizzi di posta elettronica (vedere la figura 1).


[![La finestra di dialogo Nuovo progetto](iteration-3-add-form-validation-cs/_static/image1.jpg)](iteration-3-add-form-validation-cs/_static/image1.png)

**Figura 01**: un form con convalida ([fare clic per visualizzare l'immagine ingrandita](iteration-3-add-form-validation-cs/_static/image2.png))


In questa iterazione, la logica di convalida è aggiungere direttamente per le azioni del controller. In generale, questo non è il modo consigliato per aggiungere la convalida a un'applicazione MVC ASP.NET. Un approccio migliore consiste nell'inserire la logica di convalida s un'applicazione in un apposito [livello di servizio](http://martinfowler.com/eaaCatalog/serviceLayer.html). Nell'iterazione successiva, è effettuare il refactoring l'applicazione di gestione di contatto per rendere più gestibile l'applicazione.

In questa iterazione, per semplicità, è scrivere tutto il codice di convalida manualmente. Invece di scrivere il codice di convalida effettuata, è possibile sfruttare i vantaggi di un framework di convalida. Ad esempio, è possibile utilizzare Microsoft Enterprise Library convalida applicazione blocco (VAB) per implementare la logica di convalida per l'applicazione ASP.NET MVC. Per ulteriori informazioni sul blocco di applicazione di convalida, vedere:

[*http://msdn.microsoft.com/library/dd203099.aspx*](https://msdn.microsoft.com/library/dd203099.aspx)

## <a name="adding-validation-to-the-create-view"></a>Aggiunta della convalida per la visualizzazione di creazione

Consente di iniziare, aggiungere la logica di convalida per la visualizzazione di creazione s. Fortunatamente, perché è stato generato della visualizzazione di creazione con Visual Studio, la visualizzazione di creazione contiene già tutta la logica dell'interfaccia utente necessari per visualizzare i messaggi di convalida. Visualizzazione di creazione è contenuta in elenco 1.

**Elenco 1 - \Views\Contact\Create.aspx**

[!code-aspx[Main](iteration-3-add-form-validation-cs/samples/sample1.aspx)]

Si noti la chiamata al metodo di supporto Html.ValidationSummary() che viene visualizzato immediatamente sopra il form HTML. Se sono presenti messaggi di errore di convalida, questo metodo visualizza i messaggi di convalida in un elenco puntato.

Si noti, inoltre, le chiamate a Html.ValidationMessage() visualizzati accanto a ogni campo del form. L'helper ValidationMessage() Visualizza un messaggio di errore di convalida singoli. Nel caso di listato 1, viene visualizzato un asterisco, quando si verifica un errore di convalida.

Infine, l'helper Html.TextBox() automaticamente esegue il rendering di una classe foglio di stile CSS quando si verifica un errore di convalida associato alla proprietà visualizzata per il supporto. L'helper Html.TextBox() esegue il rendering di una classe denominata **errore di convalida input**.

Quando si crea una nuova applicazione MVC ASP.NET, un foglio di stile denominato Site viene creato automaticamente nella cartella del contenuto. Questo foglio di stile contiene le definizioni per classi CSS correlate all'aspetto dei messaggi di errore seguenti:

[!code-css[Main](iteration-3-add-form-validation-cs/samples/sample2.css)]

La classe di errore di convalida campo viene utilizzata per definire lo stile dell'output sottoposto a rendering dall'helper della Html.ValidationMessage(). La classe di errore di convalida di input viene utilizzata per definire lo stile di casella di testo (input) eseguito il rendering dall'helper della Html.TextBox(). La classe di errori di riepilogo di convalida viene utilizzata per definire lo stile di elenco non ordinato dall'helper della Html.ValidationSummary() sottoposto a rendering.

> [!NOTE] 
> 
> È possibile modificare le classi del foglio di stile descritte in questa sezione per personalizzare l'aspetto dei messaggi di errore di convalida.


## <a name="adding-validation-logic-to-the-create-action"></a>Aggiunta della logica di convalida di creare azioni

Al momento, crea mai visualizzati messaggi di errore di convalida perché non è stato scritto la logica per generare i messaggi. Per visualizzare i messaggi di errore di convalida, è necessario aggiungere i messaggi di errore ModelState.

> [!NOTE] 
> 
> Il metodo UpdateModel() aggiunge messaggi di errore per ModelState automaticamente quando si verifica un errore di assegnazione del valore di un campo modulo a una proprietà. Ad esempio, se si tenta di assegnare la stringa "apple" a una proprietà di data di nascita che accetta valori di data/ora, il metodo UpdateModel() aggiunge un errore per ModelState.


Il metodo di metodo di creazione modificato nel listato 2 contiene una nuova sezione che convalida le proprietà della classe di contatto, prima che venga inserito il nuovo contatto nel database.

**Il listato 2 - Controllers\ContactController.cs (Create con la convalida)**

[!code-csharp[Main](iteration-3-add-form-validation-cs/samples/sample3.cs)]

La sezione Convalida applica quattro regole di convalida distinti:

- La proprietà FirstName deve avere una lunghezza maggiore di zero (e non può essere composti esclusivamente da spazi)
- La proprietà LastName deve avere una lunghezza maggiore di zero (e non può essere composti esclusivamente da spazi)
- Se la proprietà Phone ha un valore (ha una lunghezza maggiore di 0), quindi la proprietà Phone deve corrispondere a un'espressione regolare.
- Se la proprietà del messaggio di posta elettronica ha un valore (ha una lunghezza maggiore di 0), quindi la proprietà di messaggio di posta elettronica deve corrispondere a un'espressione regolare.

Quando si verifica una violazione delle regole di convalida, viene aggiunto un messaggio di errore per ModelState con l'aiuto del metodo AddModelError(). Quando si aggiunge un messaggio a ModelState, fornire il nome di una proprietà e il testo di un messaggio di errore di convalida. Questo messaggio di errore viene visualizzato nella vista dai metodi di supporto Html.ValidationSummary() e Html.ValidationMessage().

Dopo che vengono eseguite le regole di convalida, viene controllata la proprietà IsValid di ModelState. La proprietà IsValid restituisce false quando sono stati aggiunti gli eventuali messaggi di errore di convalida per ModelState. Se la convalida non riesce, viene visualizzata di nuovo il modulo Crea con i messaggi di errore.

> [!NOTE] 
> 
> Ho le espressioni regolari per convalidare l'indirizzo di posta elettronica e numero di telefono dal repository di espressione regolare in [*http://regexlib.com*](http://regexlib.com)


## <a name="adding-validation-logic-to-the-edit-action"></a>Aggiunta di logica di convalida per l'azione di modifica

L'azione Edit() aggiorna un contatto. L'azione Edit() deve eseguire la stessa convalida l'azione del metodo di creazione. Anziché ripetere lo stesso codice di convalida, si dovrebbe effettuare il refactoring di controller di contatto in modo che sia il metodo di creazione Edit() azioni chiamare lo stesso metodo di convalida.

La classe controller contatto modificata è contenuta nel listato 3. Questa classe è un nuovo metodo ValidateContact() che viene chiamato all'interno di azioni Edit() sia il metodo di creazione.

**Elenco di 3 - Controllers\ContactController.cs**

[!code-csharp[Main](iteration-3-add-form-validation-cs/samples/sample4.cs)]

## <a name="summary"></a>Riepilogo

In questa iterazione, abbiamo aggiunto la convalida dei form di base per l'applicazione di gestione di contatto. La logica di convalida impedisce l'invio di un nuovo contatto o la modifica di un contatto esistente senza fornire valori per le proprietà FirstName e LastName. Inoltre, gli utenti devono fornire gli indirizzi di posta elettronica e numeri di telefono valido.

In questa iterazione, la logica di convalida è aggiunto all'applicazione Gestione contatti nel modo più semplice possibile. Tuttavia, si unisce la logica di convalida la logica di controller creerà problemi per noi a lungo termine. L'applicazione sarà più difficile da gestire e modificare nel tempo.

Nell'iterazione successiva, è eseguire il refactoring la logica di convalida e la logica di accesso ai database da questo controller. Verrà usufruire dei principi di progettazione software diversi per consentirci di creare un'applicazione più regime di controllo e più gestibile.

> [!div class="step-by-step"]
> [Precedente](iteration-2-make-the-application-look-nice-cs.md)
> [Successivo](iteration-4-make-the-application-loosely-coupled-cs.md)
