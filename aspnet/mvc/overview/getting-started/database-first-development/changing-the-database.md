---
uid: mvc/overview/getting-started/database-first-development/changing-the-database
title: 'Esercitazione: Modificare il database per Entity Framework Database First con app ASP.NET MVC'
description: Questa esercitazione è incentrata su rilasciato un aggiornamento per la struttura del database e propagare tale modifica in tutta l'applicazione web.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: cfd5c083-a319-482e-8f25-5b38caa93954
msc.legacyurl: /mvc/overview/getting-started/database-first-development/changing-the-database
msc.type: authoredcontent
ms.openlocfilehash: 52cad1120908cf0d4f85770f8e2690f9415c5f56
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667635"
---
# <a name="tutorial-change-the-database-for-ef-database-first-with-aspnet-mvc-app"></a>Esercitazione: Modificare il database per Entity Framework Database First con app ASP.NET MVC

Usa MVC, Entity Framework e lo Scaffolding di ASP.NET, è possibile creare un'applicazione web che fornisce un'interfaccia a un database esistente. Questa serie di esercitazioni illustra come generare il codice che consente agli utenti di visualizzare, modificare, creare automaticamente ed eliminare dati che si trovano in una tabella di database. Il codice generato corrispondente alle colonne nella tabella di database.

Questa esercitazione è incentrata su rilasciato un aggiornamento per la struttura del database e propagare tale modifica in tutta l'applicazione web.

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Aggiungere una colonna
> * Aggiungere la proprietà alle visualizzazioni

## <a name="prerequisites"></a>Prerequisiti

* [La generazione di visualizzazioni](generating-views.md)

## <a name="add-a-column"></a>Aggiungere una colonna

Se si aggiorna la struttura di una tabella nel database, è necessario assicurarsi che la modifica viene propagata per il modello di dati, viste e controller.

Per questa esercitazione, si aggiungerà una nuova colonna alla tabella Student per registrare il cognome dello studente. Per aggiungere questa colonna, aprire il progetto di database e aprire il file Student.sql. Tramite la finestra di progettazione o il codice T-SQL, aggiungere una colonna denominata **MiddleName** che è un nvarchar (50) e consente valori NULL.

Distribuire questa modifica nel database locale avviando il progetto di database (o F5). Il nuovo campo viene aggiunto alla tabella. Se non è presente in Esplora oggetti di SQL Server, fare clic sul pulsante Aggiorna nel riquadro.

![Mostra la nuova colonna](changing-the-database/_static/image2.png)

La nuova colonna esiste nella tabella di database, ma non esiste attualmente nella classe di modello di dati. È necessario aggiornare il modello per includere la nuova colonna. Nel **modelli** cartella, aprire il **ContosoModel.edmx** file per visualizzare il diagramma del modello. Si noti che il modello per studenti non contiene la proprietà MiddleName. Fare doppio clic su un punto qualsiasi nell'area di progettazione e selezionare **Aggiorna modello da Database**.

Nell'aggiornamento guidato, selezionare la **Refresh** scheda e quindi selezionare **tabelle** > **dbo** > **studente**. Scegliere **Fine**.

Una volta completato il processo di aggiornamento, il diagramma di database include le nuove **MiddleName** proprietà. Salvare il **ContosoModel.edmx** file. È necessario salvare questo file per la nuova proprietà di propagazione per la **Student.cs** classe. A questo punto è stato aggiornato il database e il modello.

Compilare la soluzione.

## <a name="add-the-property-to-the-views"></a>Aggiungere la proprietà alle visualizzazioni

Sfortunatamente, le viste ancora non contengono la nuova proprietà. Per aggiornare le viste sono disponibili due opzioni: è possibile nuovamente generare le visualizzazioni, ancora una volta aggiungere lo scaffolding per la classe Student, o è possibile aggiungere manualmente la nuova proprietà a visualizzazioni esistenti. In questa esercitazione si aggiungerà lo scaffolding nuovamente perché non si sono eseguite le modifiche personalizzate alle visualizzazioni generati automaticamente. È possibile aggiungere manualmente la proprietà quando si sono apportate modifiche alle visualizzazioni e non si desidera perdere tali modifiche.

Per garantire le viste vengono ricreate, eliminare il **studenti** cartella sotto **viste**ed eliminare i **StudentsController**. Quindi, fare doppio clic sui **controller** cartella e aggiungere lo scaffolding per il **studente** modello. Anche in questo caso, denominare il controller **StudentsController**. Selezionare **Aggiungi**.

Compilare di nuovo la soluzione. Le visualizzazioni contengono ora la proprietà MiddleName.

![mostrano middle name](changing-the-database/_static/image5.png)

## <a name="next-steps"></a>Passaggi successivi

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Aggiunta di una colonna
> * Aggiunta la proprietà alle visualizzazioni

Passare all'esercitazione successiva per informazioni su come personalizzare la visualizzazione per la visualizzazione dei dettagli di un record di studenti.
> [!div class="nextstepaction"]
> [Personalizzare una vista](customizing-a-view.md)