---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: 'Esercitazione: Generare viste per Entity Framework Database First con app ASP.NET MVC'
description: Questa esercitazione è incentrata sull'uso di Scaffolding di ASP.NET per generare il controller e visualizzazioni.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: 7a56c0f9197a99427bcde6103ebc69d245e8ce63
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236419"
---
# <a name="tutorial-generate-views-for-ef-database-first-with-aspnet-mvc-app"></a>Esercitazione: Generare viste per Entity Framework Database First con app ASP.NET MVC

Usa MVC, Entity Framework e lo Scaffolding di ASP.NET, è possibile creare un'applicazione web che fornisce un'interfaccia a un database esistente. Questa serie di esercitazioni illustra come generare il codice che consente agli utenti di visualizzare, modificare, creare automaticamente ed eliminare dati che si trovano in una tabella di database. Il codice generato corrispondente alle colonne nella tabella di database.

Questa esercitazione è incentrata sull'uso di Scaffolding di ASP.NET per generare il controller e visualizzazioni.

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Aggiungi scaffolding
> * Aggiungere collegamenti per le nuove visualizzazioni
> * Visualizzare le visualizzazioni per studenti
> * Visualizzare le visualizzazioni di registrazione

## <a name="prerequisite"></a>Prerequisito

* [Creare modelli di dati e applicazioni web](creating-the-web-application.md)

## <a name="add-scaffold"></a>Aggiungi scaffolding

Si è pronti per generare il codice che fornisce operazioni di dati standard per le classi del modello. Aggiungere il codice aggiungendo un elemento di scaffolding. Sono disponibili molte opzioni per il tipo di scaffolding che è possibile aggiungere; in questa esercitazione, lo scaffold includerà un controller e visualizzazioni che corrispondono ai modelli per studenti e di registrazione che è stato creato nella sezione precedente.

Per mantenere la coerenza del progetto, si aggiungerà il nuovo controller esistente **controller** cartella. Fare doppio clic il **controller** cartella e selezionare **Add** > **nuovo elemento di scaffolding**.

Selezionare il **Controller MVC 5 con visualizzazioni, mediante Entity Framework** opzione. Questa opzione genererà il controller e visualizzazioni per l'aggiornamento, eliminazione, la creazione e visualizzazione dei dati nel modello.

![Aggiungi controller mvc](generating-views/_static/image2.png)

Selezionare **Student (ContosoSite.Models)** per la classe di modello e selezionare il **ContosoUniversityDataEntities (ContosoSite.Models)** per la classe del contesto. Mantenere il nome del controller come **StudentsController**.

Fare clic su **Aggiungi**.

Se si riceve un errore, è possibile che il progetto nella sezione precedente non è stata compilata. In questo caso, provare a compilare il progetto e quindi aggiungere di nuovo l'elemento di scaffolding.

Al termine il processo di generazione di codice, verrà visualizzato un nuovo controller e visualizzazioni del progetto **controller** e **viste** > **studenti** cartelle .


Eseguire di nuovo gli stessi passaggi, ma aggiungere lo scaffolding per il **registrazione** classe. Al termine, disponibile un' **EnrollmentsController.cs** file e una cartella sotto **viste** denominato **registrazioni** con le visualizzazioni Create, Delete, i dettagli, modifica e indice.

## <a name="add-links-to-new-views"></a>Aggiungere collegamenti per le nuove visualizzazioni

Per renderne più semplice per passare alle nuove visualizzazioni, è possibile aggiungere un paio di collegamenti ipertestuali alle viste di indice per gli studenti e le registrazioni. Aprire il file dal **viste** > **Home** > *index. cshtml*, che è la home page del sito. Aggiungere il codice seguente sotto il jumbotron.

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

Per il metodo ActionLink, il primo parametro è il testo da visualizzare nel collegamento. Il secondo parametro è l'azione e il terzo parametro è il nome del controller. Ad esempio, il primo collegamento punta all'azione Index in StudentsController. Il collegamento effettivo viene costruito da questi valori. Il primo collegamento in definitiva richiede agli utenti delle **index. cshtml** all'interno del file il **viste/Students** cartella.

## <a name="display-student-views"></a>Visualizzare le visualizzazioni per studenti

Si verificherà che il codice aggiunto correttamente al progetto viene visualizzato un elenco degli studenti e consente agli utenti di modificare, creare o eliminare i record di studenti nel database.

Fare doppio clic il **viste** > **Home** > *index. cshtml* file e scegliere **Visualizza nel Browser**. Nella home page dell'applicazione, selezionare **elenco degli studenti**.

![](generating-views/_static/image6.png)

Nel **indice** pagina, viene visualizzato l'elenco degli studenti e i collegamenti per modificare i dati. Selezionare il **Crea nuovo** collegare e fornire valori per un nuovo studente. Fare clic su **Create**e notare che il nuovo studente viene aggiunto all'elenco.

Nella **indice** pagina, selezionare la **modificare** collegamento e modificare alcuni dei valori di uno studente. Fare clic su **salvare**e notare che è stato modificato il record degli studenti.

Infine, selezionare il **eliminare** collegare e confermare che si desidera eliminare i record facendo il **eliminare** pulsante.

Senza scrivere alcun codice, si sono aggiunte le viste che eseguono operazioni comuni sui dati nella tabella studenti.

Si è notato che l'etichetta di testo per un campo è basata sulla proprietà database (ad esempio **LastName**) che non è necessariamente ciò che si desidera visualizzare nella pagina web. Ad esempio, è preferibile all'etichetta **cognome**. Si correggerà questo problema di visualizzazione in un secondo momento nell'esercitazione.

## <a name="display-enrollment-views"></a>Visualizzare le visualizzazioni di registrazione

Il database include una relazione uno-a-molti tra le tabelle Student e registrazione e una relazione uno-a-molti tra le tabelle Course ed Enrollment esiste. Le viste per la registrazione di gestiscono correttamente tali relazioni. Passare alla home page per il sito e selezionare il **elenco di registrazioni** collegamento e quindi la **Crea nuovo** collegamento.

Nella visualizzazione di un form per la creazione di un nuovo record di registrazione. In particolare, si noti che il modulo contiene un **CourseID** elenco a discesa elenco e una **StudentID** elenco a discesa. Entrambi vengono popolate con i valori dalle tabelle correlate.

Inoltre, convalida dei valori forniti viene applicata automaticamente in base sul tipo di dati del campo. **Livello** richiede un numero, pertanto viene visualizzato un messaggio di errore se si tenta di fornire un valore incompatibile: *Il livello di campo deve essere un numero.*

Si è verificato che le visualizzazioni generate automaticamente consentono agli utenti di lavorare con i dati nel database. Nella prossima esercitazione della serie, si sarà l'aggiornamento del database e apportare le modifiche corrispondenti nell'applicazione web.

## <a name="next-steps"></a>Passaggi successivi

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Aggiunta di scaffolding
> * Sono stati aggiunti collegamenti per le nuove visualizzazioni
> * Viste visualizzate per studenti
> * Viste di registrazione visualizzata

Passare all'esercitazione successiva per informazioni su come modificare il database.
> [!div class="nextstepaction"]
> [Modificare il database](changing-the-database.md)