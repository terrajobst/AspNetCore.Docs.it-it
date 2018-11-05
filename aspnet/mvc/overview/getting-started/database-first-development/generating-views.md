---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: 'Database di Entity Framework prima di tutto con ASP.NET MVC: generazione di visualizzazioni | Microsoft Docs'
author: Rick-Anderson
description: Usa MVC, Entity Framework e lo Scaffolding di ASP.NET, è possibile creare un'applicazione web che fornisce un'interfaccia a un database esistente. Questa esercitazione seri...
ms.author: riande
ms.date: 12/29/2014
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: 7d925573dd4cdf5c1a36e51f312e18093bd35043
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021089"
---
<a name="ef-database-first-with-aspnet-mvc-generating-views"></a>Database di Entity Framework prima di tutto con ASP.NET MVC: generazione di visualizzazioni
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Usa MVC, Entity Framework e lo Scaffolding di ASP.NET, è possibile creare un'applicazione web che fornisce un'interfaccia a un database esistente. Questa serie di esercitazioni illustra come generare il codice che consente agli utenti di visualizzare, modificare, creare automaticamente ed eliminare dati che si trovano in una tabella di database. Il codice generato corrispondente alle colonne nella tabella di database.
> 
> Questa parte della serie è incentrato sull'uso di Scaffolding di ASP.NET per generare il controller e visualizzazioni.


## <a name="add-scaffold"></a>Aggiungi scaffolding

Si è pronti per generare il codice che fornisce operazioni di dati standard per le classi del modello. Aggiungere il codice aggiungendo un elemento di scaffolding. Sono disponibili molte opzioni per il tipo di scaffolding che è possibile aggiungere; in questa esercitazione, lo scaffold includerà un controller e visualizzazioni che corrispondono ai modelli per studenti e di registrazione che è stato creato nella sezione precedente.

Per mantenere la coerenza del progetto, si aggiungerà il nuovo controller esistente **controller** cartella. Fare doppio clic il **controller** cartella e selezionare **Add** – **nuovo elemento di scaffolding**.

![Aggiungi scaffolding](generating-views/_static/image1.png)

Selezionare il **Controller MVC 5 con visualizzazioni, mediante Entity Framework** opzione. Questa opzione genererà il controller e visualizzazioni per l'aggiornamento, eliminazione, la creazione e visualizzazione dei dati nel modello.

![Aggiungi controller mvc](generating-views/_static/image2.png)

Selezionare **studente** per la classe di modello e selezionare il **ContosoUniversityEntities** per la classe del contesto. Mantenere il nome del controller come **StudentsController**,

![specificare controller](generating-views/_static/image3.png)

Fare clic su **Aggiungi**.

Se si riceve un errore, è possibile che il progetto nella sezione precedente non è stata compilata. In questo caso, provare a compilare il progetto e quindi aggiungere di nuovo l'elemento di scaffolding.

Al termine il processo di generazione di codice, si noterà un nuovo controller e visualizzazioni nel progetto.

![Mostra visualizzazioni](generating-views/_static/image4.png)

Eseguire di nuovo gli stessi passaggi, ma aggiungere lo scaffolding per la classe di registrazione. Al termine, si avrà un' **EnrollmentsController.cs** file e una cartella sotto **viste** denominato **registrazioni** con Create, Delete, i dettagli, modifica e indice Visualizzazioni.

![Mostra visualizzazioni](generating-views/_static/image5.png)

## <a name="add-links-to-new-views"></a>Aggiungere collegamenti per le nuove visualizzazioni

Per renderne più semplice per passare alle nuove visualizzazioni, è possibile aggiungere un paio di collegamenti ipertestuali alle viste di indice per gli studenti e le registrazioni. Aprire il file dal **Views/Home/Index.cshtml**, che è la home page del sito. Aggiungere il codice seguente sotto il jumbotron.

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

Per il metodo ActionLink, il primo parametro è il testo da visualizzare nel collegamento. Il secondo parametro è l'azione e il terzo parametro è il nome del controller. Ad esempio, il primo collegamento punta all'azione Index in StudentsController. Il collegamento effettivo viene costruito da questi valori. Il primo collegamento in definitiva richiede agli utenti delle **index. cshtml** all'interno del file il **viste/Students** cartella.

## <a name="display-student-views"></a>Visualizzare le visualizzazioni per studenti

Si verificherà che il codice aggiunto correttamente al progetto viene visualizzato un elenco degli studenti e consente agli utenti di modificare, creare o eliminare i record di studenti nel database.

Fare doppio clic il **Views/Home/Index.cshtml** e selezionare **Visualizza nel Browser**. In questa pagina, fare clic sul collegamento per l'elenco degli studenti.

![](generating-views/_static/image6.png)

In questa pagina viene visualizzato l'elenco degli studenti e i collegamenti per modificare i dati.

![elenco degli studenti](generating-views/_static/image7.png)

Scegliere il **Crea nuovo** collegare e fornire valori per un nuovo studente.

![creare nuovo studente](generating-views/_static/image8.png)

Fare clic su **Create**e notare che il nuovo studente viene aggiunto all'elenco.

![elenco con nuovo studente](generating-views/_static/image9.png)

Selezionare il **modifica** collegamento e modificare alcuni dei valori di uno studente.

![Modifica studente](generating-views/_static/image10.png)

Fare clic su **salvare**e notare che è stato modificato il record degli studenti.

Infine, selezionare il **eliminare** collegare e confermare che si desidera eliminare i record facendo il **eliminare** pulsante.

![eliminazione degli studenti](generating-views/_static/image11.png)

Senza scrivere alcun codice, si sono aggiunte le viste che eseguono operazioni comuni sui dati nella tabella studenti.

Si è notato che l'etichetta di testo per un campo è basata sulla proprietà database (ad esempio **LastName**) che non è necessariamente ciò che si desidera visualizzare nella pagina web. Ad esempio, è preferibile all'etichetta **cognome**. Si correggerà questo problema di visualizzazione in un secondo momento nell'esercitazione.

## <a name="display-enrollment-views"></a>Visualizzare le visualizzazioni di registrazione

Il database include una relazione uno-a-molti tra le tabelle Student e registrazione e una relazione uno-a-molti tra le tabelle Course ed Enrollment esiste. Le viste per la registrazione di gestiscono correttamente tali relazioni. Passare alla home page per il sito e selezionare il **elenco di registrazioni** collegamento e quindi la **Crea nuovo** collegamento. Nella visualizzazione di un form per la creazione di un nuovo record di registrazione. In particolare, si noti che il form contiene due elenchi a discesa vengono popolati con i valori dalle tabelle correlate.

![creazione di registrazione](generating-views/_static/image12.png)

Inoltre, convalida dei valori forniti viene applicata automaticamente in base sul tipo di dati del campo. Livello richiede un numero, pertanto viene visualizzato un messaggio di errore se si tenta di fornire un valore incompatibile.

![messaggio di convalida](generating-views/_static/image13.png)

Si è verificato che le visualizzazioni generate automaticamente consentono agli utenti di lavorare con i dati nel database. Nella prossima esercitazione della serie, si sarà l'aggiornamento del database e apportare le modifiche corrispondenti nell'applicazione web.

> [!div class="step-by-step"]
> [Precedente](creating-the-web-application.md)
> [Successivo](changing-the-database.md)
