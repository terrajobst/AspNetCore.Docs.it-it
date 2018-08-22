---
uid: mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-vb
title: 'Iterazione #3-aggiungere la convalida dei form (VB) | Microsoft Docs'
author: microsoft
description: Nella terza iterazione, aggiungiamo la convalida dei form di base. Persone è impedire l'invio di un modulo senza completare i campi del modulo richiesto. È inoltre possibile convalidare emai...
ms.author: riande
ms.date: 02/20/2009
ms.assetid: 4805e75a-7911-46e3-b11b-229a6eed245e
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-vb
msc.type: authoredcontent
ms.openlocfilehash: b44aaab45f04f736e4171a43a8b24b71aaedca2f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830320"
---
<a name="iteration-3--add-form-validation-vb"></a>Iterazione #3-aggiungere la convalida dei form (VB)
====================
da [Microsoft](https://github.com/microsoft)

[Scaricare il codice](iteration-3-add-form-validation-vb/_static/contactmanager_3_vb1.zip)

> Nella terza iterazione, aggiungiamo la convalida dei form di base. Persone è impedire l'invio di un modulo senza completare i campi del modulo richiesto. È anche convalidare gli indirizzi di posta elettronica e numeri di telefono.


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

In questa seconda iterazione dell'applicazione Contact Manager, viene aggiunto la convalida dei form di base. Si evita che gli utenti dall'invio di un contatto senza dover immettere i valori per i campi modulo necessari. È inoltre possibile convalidare i numeri di telefono e indirizzi di posta elettronica (vedere la figura 1).


[![La finestra di dialogo Nuovo progetto](iteration-3-add-form-validation-vb/_static/image1.jpg)](iteration-3-add-form-validation-vb/_static/image1.png)

**Figura 01**: un modulo con la convalida ([fare clic per visualizzare l'immagine con dimensioni normali](iteration-3-add-form-validation-vb/_static/image2.png))


In questa iterazione, la logica di convalida viene aggiunto direttamente per le azioni del controller. In generale, non il metodo consigliato per aggiungere la convalida a un'applicazione ASP.NET MVC. Un approccio migliore consiste nell'inserire una logica di convalida s dell'applicazione in un oggetto separato [livello di servizio](http://martinfowler.com/eaaCatalog/serviceLayer.html). Nell'iterazione successiva, è eseguire il refactoring all'applicazione Contact Manager di eseguire l'applicazione più facilmente gestibile.

In questa iterazione, per semplicità, si scrive tutto il codice di convalida manualmente. Invece di scrivere il codice di convalida noi stessi, è possibile sfruttare i vantaggi di un framework di convalida. Ad esempio, è possibile utilizzare Microsoft Enterprise Library convalida Application Block (VAB) per implementare la logica di convalida per l'applicazione ASP.NET MVC. Per altre informazioni su del blocco applicazione per la convalida, vedere:

[*http://msdn.microsoft.com/library/dd203099.aspx*](https://msdn.microsoft.com/library/dd203099.aspx)

## <a name="adding-validation-to-the-create-view"></a>Aggiunta della convalida della visualizzazione di creazione

Let s iniziare aggiungendo la logica di convalida per la visualizzazione di creazione. Fortunatamente, perché è stato generato della visualizzazione di creazione con Visual Studio, la visualizzazione di creazione contiene già tutta la logica dell'interfaccia utente necessarie per visualizzare i messaggi di convalida. Visualizzazione di creazione è contenuta nel listato 1.

**Listato 1 - \Views\Contact\Create.aspx**

[!code-aspx[Main](iteration-3-add-form-validation-vb/samples/sample1.aspx)]

La classe di errore di convalida campo viene usata per definire lo stile dell'output sottoposto a rendering dall'helper Html.ValidationMessage(). La classe di errore di convalida input viene utilizzata per definire lo stile casella di testo (input) viene eseguito il rendering dall'helper Html.TextBox(). La classe di errori di riepilogo di convalida viene utilizzata per definire lo stile di elenco non ordinato viene eseguito il rendering dall'helper Html.ValidationSummary().

> [!NOTE] 
> 
> È possibile modificare le classi del foglio di stile descritte in questa sezione per personalizzare l'aspetto dei messaggi di errore di convalida.


## <a name="adding-validation-logic-to-the-create-action"></a>Aggiunta della logica di convalida l'azione di creazione

Al momento, la visualizzazione di creazione non visualizza mai i messaggi di errore di convalida perché non è stato scritto la logica per generare i messaggi. Per visualizzare i messaggi di errore di convalida, è necessario aggiungere i messaggi di errore per ModelState.

> [!NOTE] 
> 
> Il metodo UpdateModel() aggiunge messaggi di errore per ModelState automaticamente quando si verifica un errore assegnando il valore di un campo del form a una proprietà. Ad esempio, se si tenta di assegnare la stringa "apple" a una proprietà data di nascita che accetta i valori DateTime, il metodo UpdateModel() aggiunge un errore per ModelState.


Il metodo Create () modificato nel listato 2 contiene una nuova sezione che convalida le proprietà della classe contattare prima il nuovo contatto viene inserito nel database.

**Listato 2 - Controllers\ContactController.vb (Create con la convalida)**

[!code-vb[Main](iteration-3-add-form-validation-vb/samples/sample2.vb)]

La sezione Convalida applica quattro regole di convalida distinti:

- La proprietà FirstName deve avere una lunghezza maggiore di zero (e non può essere composto esclusivamente spazi)
- La proprietà LastName deve avere una lunghezza maggiore di zero (e non può essere composto esclusivamente spazi)
- Se la proprietà Phone ha un valore (ha una lunghezza maggiore di 0), quindi un'espressione regolare deve corrispondere la proprietà di telefono.
- Se la proprietà indirizzo di posta elettronica ha un valore (ha una lunghezza maggiore di 0), quindi un'espressione regolare deve corrispondere la proprietà di messaggio di posta elettronica.

Quando si verifica una violazione della regola di convalida, con l'aiuto del metodo AddModelError() ModelState viene aggiunto un messaggio di errore. Quando si aggiunge un messaggio a ModelState, si fornisce il nome di una proprietà e il testo di un messaggio di errore di convalida. Questo messaggio di errore viene visualizzato nella vista dai metodi helper Html.ValidationSummary() e Html.ValidationMessage().

Dopo che vengono eseguite le regole di convalida, viene verificata la proprietà IsValid di ModelState. La proprietà IsValid restituisce false quando gli eventuali messaggi di errore di convalida sono stati aggiunti per ModelState. Se la convalida non riesce, il modulo di creazione verrà visualizzata nuovamente con i messaggi di errore.

> [!NOTE] 
> 
> Ho le espressioni regolari per convalidare l'indirizzo di posta elettronica e numero di telefono dal repository dell'espressione regolare in [*http://regexlib.com*](http://regexlib.com)


## <a name="adding-validation-logic-to-the-edit-action"></a>Aggiunta di logica di convalida per l'operazione di modifica

L'azione di Edit () aggiorna un contatto. L'azione di Edit () deve seguire esattamente la stessa convalida l'azione create (). Anziché ripetere lo stesso codice di convalida, è necessario effettuare il refactoring il controller di contattare in modo che le azioni sia create () ed Edit () chiama lo stesso metodo di convalida.

La classe controller contatto modificata è contenuta nel listato 3. Questa classe ha un nuovo metodo ValidateContact() che viene chiamato all'interno di azioni sia al metodo Edit ().

**Listato 3 - Controllers\ContactController.vb**

[!code-vb[Main](iteration-3-add-form-validation-vb/samples/sample3.vb)]

## <a name="summary"></a>Riepilogo

Questa iterazione, abbiamo aggiunto la convalida dei form di base per l'applicazione Contact Manager. La logica di convalida impedisce agli utenti di inviare un nuovo contatto o la modifica di un contatto esistente senza fornire valori per le proprietà FirstName e LastName. Inoltre, gli utenti devono fornire gli indirizzi di posta elettronica e numeri di telefono valido.

Questa iterazione, abbiamo aggiunto la logica di convalida all'applicazione Contact Manager nel modo più semplice possibile. Tuttavia, combinare la logica di convalida nel nostro logica dei controller creerà i problemi per noi a lungo termine. L'applicazione sarà più difficile da gestire e modificare nel tempo.

Nell'iterazione successiva, è eseguire il refactoring la logica di convalida e logica di accesso ai database all'esterno ai controller. Daremo sfruttare alcuni principi di progettazione di software per abilitare la creazione di un'applicazione più strettamente collegata e più facilmente gestibile.

> [!div class="step-by-step"]
> [Precedente](iteration-2-make-the-application-look-nice-vb.md)
> [Successivo](iteration-4-make-the-application-loosely-coupled-vb.md)
