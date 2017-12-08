---
uid: mvc/overview/getting-started/database-first-development/changing-the-database
title: 'Database di Entity Framework prima con ASP.NET MVC: modifica del Database | Documenti Microsoft'
author: tfitzmac
description: "Usa lo Scaffolding di ASP.NET MVC ed Entity Framework, è possibile creare un'applicazione web che fornisce un'interfaccia a un database esistente. Questa esercitazione seri..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: cfd5c083-a319-482e-8f25-5b38caa93954
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/changing-the-database
msc.type: authoredcontent
ms.openlocfilehash: 1ffe753812e5eef817f03ab488a28ae5fcefd41e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="ef-database-first-with-aspnet-mvc-changing-the-database"></a>Database di Entity Framework prima con ASP.NET MVC: modifica del Database
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Usa lo Scaffolding di ASP.NET MVC ed Entity Framework, è possibile creare un'applicazione web che fornisce un'interfaccia a un database esistente. Questa serie di esercitazioni viene illustrato come automaticamente generare il codice che consente agli utenti di visualizzare, modificare, creare ed eliminare dati che si trovano in una tabella di database. Il codice generato corrisponde alle colonne nella tabella di database.
> 
> Questa parte della serie è incentrata su effettua un aggiornamento per la struttura del database e propagare la modifica dell'applicazione web.


## <a name="add-a-column"></a>Aggiungere una colonna

Se si aggiorna la struttura di una tabella nel database, è necessario assicurarsi che la modifica viene propagata per il modello di dati, visualizzazioni e controller.

Per questa esercitazione si aggiungerà una nuova colonna alla tabella di studenti per registrare il cognome dello studente. Per aggiungere questa colonna, aprire il progetto di database, aprire il file Student.sql. Tramite la finestra di progettazione o il codice T-SQL, aggiungere una colonna denominata **MiddleName** che è un nvarchar (50) e consente valori NULL.

![aggiungere secondo nome](changing-the-database/_static/image1.png)

Distribuire questa modifica nel database locale avviando il progetto di database (o F5). Il nuovo campo viene aggiunto alla tabella. Se non è presente in Esplora oggetti di SQL Server, fare clic sul pulsante Aggiorna nel riquadro.

![Mostra nuova colonna](changing-the-database/_static/image2.png)

La nuova colonna esiste nella tabella di database, ma non è attualmente esiste nella classe di modello di dati. È necessario aggiornare il modello per includere la nuova colonna. Nel **modelli** cartella, aprire il **ContosoModel.edmx** file per la visualizzazione del diagramma del modello. Si noti che il modello di Student non contiene la proprietà MiddleName. In un punto qualsiasi nell'area di progettazione e scegliere **il modello di aggiornamento dal Database**.

![modello di aggiornamento](changing-the-database/_static/image3.png)

Nell'aggiornamento guidato, selezionare il **aggiornamento** scheda e **Student** tabella.

![aggiornamento guidato](changing-the-database/_static/image4.png)

Scegliere **Fine**.

Al termine del processo di aggiornamento, il diagramma di database include il nuovo **MiddleName** proprietà. Salvare il **ContosoModel.edmx** file. È necessario salvare questo file per la nuova proprietà di propagazione per il **Student.cs** classe. A questo punto, è stato aggiornato il database e il modello.

Compilare la soluzione.

Purtroppo, le viste ancora non contengono la nuova proprietà. Per aggiornare le viste sono disponibili due opzioni: è possibile nuovamente generare le visualizzazioni aggiungendo ancora una volta lo scaffolding per la classe Student o è possibile aggiungere manualmente la nuova proprietà per le visualizzazioni esistenti. In questa esercitazione si aggiungeranno lo scaffolding nuovamente perché non sono state modifiche personalizzate per le visualizzazioni generate automaticamente. È possibile aggiungere manualmente la proprietà quando si sono apportate modifiche alle viste e non si desidera perdere tali modifiche.

Per assicurarsi che le viste vengono ricreate, eliminare il **studenti** cartella **viste**ed eliminare il **StudentsController**. Quindi, fare doppio clic sul **controller** cartella e aggiungere lo scaffolding per il **Student** modello. Nuovamente, denominare il controller **StudentsController**. Scegliere **OK**.

Le viste contengono ora la proprietà MiddleName.

![visualizzare cognome](changing-the-database/_static/image5.png)

Nella sezione successiva, si aggiungerà codice per personalizzare la visualizzazione per la visualizzazione dei dettagli di un record di studenti.

>[!div class="step-by-step"]
[Precedente](generating-views.md)
[Successivo](customizing-a-view.md)
