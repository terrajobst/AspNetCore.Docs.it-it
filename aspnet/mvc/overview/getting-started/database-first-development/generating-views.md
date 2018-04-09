---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: 'Database di Entity Framework prima con ASP.NET MVC: generazione di visualizzazioni | Documenti Microsoft'
author: tfitzmac
description: Usa lo Scaffolding di ASP.NET MVC ed Entity Framework, è possibile creare un'applicazione web che fornisce un'interfaccia a un database esistente. Questa esercitazione seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/29/2014
ms.topic: article
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: b60e89a187a879255eb051dc87241714cef6fa63
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="ef-database-first-with-aspnet-mvc-generating-views"></a>Database di Entity Framework prima con ASP.NET MVC: generazione di visualizzazioni
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Usa lo Scaffolding di ASP.NET MVC ed Entity Framework, è possibile creare un'applicazione web che fornisce un'interfaccia a un database esistente. Questa serie di esercitazioni viene illustrato come automaticamente generare il codice che consente agli utenti di visualizzare, modificare, creare ed eliminare dati che si trovano in una tabella di database. Il codice generato corrisponde alle colonne nella tabella di database.
> 
> Questa parte della serie si concentra sull'uso di Scaffolding di ASP.NET per generare il controller e visualizzazioni.


## <a name="add-scaffold"></a>aggiungere lo scaffolding

Si è pronti per generare il codice che fornisce le operazioni di dati standard per le classi di modello. Aggiungere il codice aggiungendo un elemento lo scaffolding. Sono disponibili molte opzioni per il tipo di scaffolding che è possibile aggiungere; in questa esercitazione il scaffold includerà un controller e visualizzazioni che corrispondono ai modelli di studenti e la registrazione che è stato creato nella sezione precedente.

Per mantenere la coerenza del progetto, si aggiungerà il nuovo controller esistente **controller** cartella. Fare doppio clic su di **controller** cartella e selezionare **Aggiungi** – **nuovo elemento di scaffolding**.

![aggiungere lo scaffolding](generating-views/_static/image1.png)

Selezionare il **Controller MVC 5 con visualizzazioni, mediante Entity Framework** opzione. Questa opzione genererà il controller e visualizzazioni per l'aggiornamento, eliminazione, la creazione e visualizzazione dei dati nel modello.

![Aggiungi controller mvc](generating-views/_static/image2.png)

Selezionare **Student** per la classe di modello e selezionare il **ContosoUniversityEntities** per la classe di contesto. Mantenere il nome del controller come **StudentsController**,

![specificare controller](generating-views/_static/image3.png)

Fare clic su **Aggiungi**.

Se si riceve un errore, è possibile che si non compila il progetto nella sezione precedente. In questo caso, provare a compilare il progetto e quindi aggiungere di nuovo l'elemento di scaffolding.

Al termine del processo di generazione del codice, si noterà un nuovo controller e visualizzazioni nel progetto.

![Mostra visualizzazioni](generating-views/_static/image4.png)

Eseguire di nuovo gli stessi passaggi, ma è aggiungere lo scaffolding per la classe di registrazione. Al termine, è necessario un **EnrollmentsController.cs** file e una cartella sotto **viste** denominato **le registrazioni** con Create, Delete, i dettagli, modifica e indice Visualizzazioni.

![Mostra visualizzazioni](generating-views/_static/image5.png)

## <a name="add-links-to-new-views"></a>Aggiungere collegamenti alle nuove visualizzazioni

Per rendere più semplice per passare alle nuove visualizzazioni, è possibile aggiungere un paio di collegamenti ipertestuali alle viste di indice per studenti e le registrazioni. Aprire il file in **Views/Home/Index.cshtml**, che è la home page del sito. Aggiungere il codice seguente sotto il jumbotron.

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

Per il metodo ActionLink, il primo parametro è il testo da visualizzare nel collegamento. Il secondo parametro è l'azione e il terzo parametro è il nome del controller. Ad esempio, il primo collegamento punta all'azione indice StudentsController. Il collegamento effettivo viene costruito da questi valori. Il primo collegamento accetta infine agli utenti del **cshtml** file all'interno di **viste e gli studenti** cartella.

## <a name="display-student-views"></a>Visualizzare le visualizzazioni di student

Si verificherà che il codice aggiunto al progetto correttamente Visualizza un elenco di studenti e consente agli utenti di modificare, creare o eliminare i record di studente nel database.

Fare doppio clic su di **Views/Home/Index.cshtml** file e selezionare **Visualizza nel Browser**. In questa pagina, fare clic sul collegamento per l'elenco di studenti.

![](generating-views/_static/image6.png)

In questa pagina viene visualizzato l'elenco di studenti e collegamenti per modificare questi dati.

![elenco di studenti](generating-views/_static/image7.png)

Fare clic su di **Crea nuovo** collegare e fornire valori per un nuovo studente.

![creare nuovo studente](generating-views/_static/image8.png)

Fare clic su **crea**e il nuovo studente viene aggiunto all'elenco.

![elenco con nuovo studente](generating-views/_static/image9.png)

Selezionare il **modifica** collegamento e modificare alcuni dei valori per uno studente.

![modifica di student](generating-views/_static/image10.png)

Fare clic su **salvare**e il record studente è stato modificato.

Infine, selezionare il **eliminare** collegare e confermare che si desidera eliminare il record, fare clic su di **eliminare** pulsante.

![eliminare student](generating-views/_static/image11.png)

Senza scrivere alcun codice, sono state aggiunte le viste che eseguono operazioni comuni sui dati nella tabella di studenti.

Si può notare che l'etichetta di testo per un campo è in base alla proprietà di database (ad esempio **LastName**) che non è necessariamente ciò che si desidera visualizzare nella pagina web. Ad esempio, è preferibile all'etichetta **cognome**. Per correggere il problema di visualizzazione è più avanti nell'esercitazione.

## <a name="display-enrollment-views"></a>Visualizzare le visualizzazioni di registrazione

Il database include una relazione uno-a-molti tra le tabelle Student e registrazione e una relazione uno-a-molti tra le tabelle in corso e la registrazione. Le viste per la registrazione di gestire correttamente tali relazioni. Passare alla home page per il sito e selezionare il **elenco le registrazioni** collegamento e quindi la **Crea nuovo** collegamento. Nella visualizzazione di un form per la creazione di un nuovo record di registrazione. In particolare, si noti che il form contiene due elenchi a discesa vengono popolati con i valori dalle tabelle correlate.

![creare una registrazione](generating-views/_static/image12.png)

Inoltre, convalida dei valori forniti viene applicata automaticamente in base al tipo di dati del campo. Livello richiede un numero, pertanto viene visualizzato un messaggio di errore se si tenta di fornire un valore non compatibile.

![messaggio di convalida](generating-views/_static/image13.png)

Significa che le visualizzazioni generate automaticamente consentono agli utenti di lavorare con i dati nel database. Nella prossima esercitazione di questa serie, si aggiorna il database e apportare le modifiche corrispondenti nell'applicazione web.

> [!div class="step-by-step"]
> [Precedente](creating-the-web-application.md)
> [Successivo](changing-the-database.md)
