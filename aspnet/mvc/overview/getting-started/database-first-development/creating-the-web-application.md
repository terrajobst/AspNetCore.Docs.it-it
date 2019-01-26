---
uid: mvc/overview/getting-started/database-first-development/creating-the-web-application
title: "Esercitazione: Creare il l'applicazione Web e i modelli di dati per Entity Framework Database First con ASP.NET MVC"
description: Questo articolo è incentrato sulla creazione dell'applicazione web e la generazione di modelli di data basati delle tabelle di database.
author: Rick-Anderson
ms.author: riande
ms.date: 01/23/2019
ms.topic: tutorial
ms.assetid: bc8f2bd5-ff57-4dcd-8418-a5bd517d8953
msc.legacyurl: /mvc/overview/getting-started/database-first-development/creating-the-web-application
msc.type: authoredcontent
ms.openlocfilehash: 095d355866c9ab8fba3759f3e05e2a521992f3d6
ms.sourcegitcommit: d5223cf6a2cf80b4f5dc54169b0e376d493d2d3a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2019
ms.locfileid: "54889769"
---
# <a name="tutorial-create-the-the-web-application-and-data-models-for-ef-database-first-with-aspnet-mvc"></a>Esercitazione: Creare il l'applicazione Web e i modelli di dati per Entity Framework Database First con ASP.NET MVC

 Usa MVC, Entity Framework e lo Scaffolding di ASP.NET, è possibile creare un'applicazione web che fornisce un'interfaccia a un database esistente. Questa serie di esercitazioni illustra come generare il codice che consente agli utenti di visualizzare, modificare, creare automaticamente ed eliminare dati che si trovano in una tabella di database. Il codice generato corrispondente alle colonne nella tabella di database.

Questo articolo è incentrato sulla creazione dell'applicazione web e la generazione di modelli di data basati delle tabelle di database.

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Creare un'app Web ASP.NET
> * Genera i modelli

## <a name="prerequisites"></a>Prerequisiti

* [Guida introduttiva a 6 Database First di Entity Framework con MVC 5](setting-up-database.md)

## <a name="create-an-aspnet-web-app"></a>Creare un'app Web ASP.NET

In una nuova soluzione o la stessa soluzione come progetto di database, creare un nuovo progetto in Visual Studio e selezionare il **applicazione Web ASP.NET** modello. Denominare il progetto **ContosoSite**.

![Crea progetto](creating-the-web-application/_static/image1.png)

Fare clic su **OK**.

Nella finestra Nuovo progetto ASP.NET, selezionare la **MVC** modello. È possibile cancellare il **ospita nel cloud** opzione per il momento, perché si distribuirà l'applicazione nel cloud in un secondo momento. Fare clic su **OK** per creare l'applicazione.

Il progetto viene creato con i file predefiniti e le cartelle.

In questa esercitazione si userà Entity Framework 6. È possibile verificare la versione di Entity Framework nel progetto tramite la finestra Gestisci pacchetti NuGet. Se necessario, aggiornare la versione di Entity Framework.

![Mostra versione](creating-the-web-application/_static/image3.png)

## <a name="generate-the-models"></a>Genera i modelli

Ora si creerà i modelli di Entity Framework dalle tabelle del database. Questi modelli sono classi che si userà per lavorare con i dati. Ogni modello riflette una tabella nel database e contiene proprietà che corrispondono alle colonne nella tabella.

Fare doppio clic il **modelli** cartella e selezionare **Add** e **nuovo elemento**.

Nella finestra Aggiungi nuovo elemento, selezionare **Data** nel riquadro sinistro e **ADO.NET Entity Data Model** tra le opzioni nel riquadro centrale. Denominare il nuovo file di modello **ContosoModel**.

Fare clic su **Aggiungi**.

Nella procedura guidata Entity Data Model, selezionare **Entity Framework Designer da database**.

Scegliere **Avanti**.

Se si dispone di connessioni di database definite all'interno dell'ambiente di sviluppo, si potrebbe vedere una di queste connessioni pre-selezionate. Tuttavia, si desidera creare una nuova connessione al database creato nella prima parte di questa esercitazione. Scegliere il **nuova connessione** pulsante.

Nella finestra proprietà di connessione, specificare il nome del server locale in cui è stato creato il database (in questo caso **\Projects13 (localdb)**). Dopo aver specificato il nome del server, selezionare il ContosoUniversityData dai database di disponibilità.

![impostare le proprietà di connessione](creating-the-web-application/_static/image8.png)

Fare clic su **OK**.

Sono ora visualizzate le proprietà di connessione corretta. È possibile usare il nome predefinito per la connessione nel file Web. config.

Scegliere **Avanti**.

Selezionare la versione più recente di Entity Framework.

Scegliere **Avanti**.

Selezionare **tabelle** per generare modelli per tutte le tre tabelle.

Scegliere **Fine**.

Se si riceve un avviso di sicurezza, selezionare **OK** per continuare l'esecuzione del modello.

I modelli vengono generati dalle tabelle del database e viene visualizzato un diagramma che mostra le proprietà e relazioni tra le tabelle.

![diagramma del modello](creating-the-web-application/_static/image11.png)

La cartella Models ora include molti nuovi file correlati ai modelli generati dal database.

Il **ContosoModel.Context.cs** file contiene una classe che deriva dalle **DbContext** classe e fornisce una proprietà per ogni classe di modello che corrisponde a una tabella di database. Il **Course.cs**, **Enrollment.cs**, e **Student.cs** file contengono le classi del modello che rappresentano le tabelle di database. Si utilizzerà la classe del contesto sia le classi del modello quando si lavora con scaffolding.

Prima di procedere con questa esercitazione, compilare il progetto. Nella sezione successiva, si genererà codice basato su modelli di data, ma tale sezione non funzionerà se non è stato compilato il progetto.

## <a name="next-steps"></a>Passaggi successivi

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Creazione di un'app web ASP.NET
> * I modelli generati

Passare all'articolo successivo per informazioni su come creare generano codice basato sui modelli di dati.
> [!div class="nextstepaction"]
> [La generazione di visualizzazioni](generating-views.md)