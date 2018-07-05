---
uid: mvc/overview/getting-started/database-first-development/creating-the-web-application
title: "Database di Entity Framework prima di tutto con ASP.NET MVC: creazione dell'applicazione Web e i modelli di dati | Microsoft Docs"
author: tfitzmac
description: Usa MVC, Entity Framework e lo Scaffolding di ASP.NET, è possibile creare un'applicazione web che fornisce un'interfaccia a un database esistente. Questa esercitazione seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: bc8f2bd5-ff57-4dcd-8418-a5bd517d8953
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/database-first-development/creating-the-web-application
msc.type: authoredcontent
ms.openlocfilehash: a1c4e3365d320ee54d378de33a77666558a854d2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37398245"
---
<a name="ef-database-first-with-aspnet-mvc-creating-the-web-application-and-data-models"></a>Database di Entity Framework prima di tutto con ASP.NET MVC: creazione dell'applicazione Web e i modelli di dati
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Usa MVC, Entity Framework e lo Scaffolding di ASP.NET, è possibile creare un'applicazione web che fornisce un'interfaccia a un database esistente. Questa serie di esercitazioni illustra come generare il codice che consente agli utenti di visualizzare, modificare, creare automaticamente ed eliminare dati che si trovano in una tabella di database. Il codice generato corrispondente alle colonne nella tabella di database.
> 
> Questa parte della serie è incentrato sulla creazione dell'applicazione web e la generazione di modelli di dati basati su tabelle di database.


## <a name="create-a-new-aspnet-web-application"></a>Creare una nuova applicazione Web ASP.NET

In una nuova soluzione o la stessa soluzione come progetto di database, creare un nuovo progetto in Visual Studio e selezionare il **applicazione Web ASP.NET** modello. Denominare il progetto **ContosoSite**.

![Crea progetto](creating-the-web-application/_static/image1.png)

Fare clic su **OK**.

Nella finestra Nuovo progetto ASP.NET, selezionare la **MVC** modello. È possibile cancellare il **ospita nel cloud** opzione per il momento, perché si distribuirà l'applicazione nel cloud in un secondo momento. Fare clic su **OK** per creare l'applicazione.

![Selezionare il modello mvc](creating-the-web-application/_static/image2.png)

Il progetto viene creato con i file predefiniti e le cartelle.

In questa esercitazione si userà Entity Framework 6. È possibile verificare la versione di Entity Framework nel progetto tramite la finestra Gestisci pacchetti NuGet. Se necessario, aggiornare la versione di Entity Framework.

![Mostra versione](creating-the-web-application/_static/image3.png)

## <a name="generate-the-models"></a>Genera i modelli

Ora si creerà i modelli di Entity Framework dalle tabelle del database. Questi modelli sono classi che si userà per lavorare con i dati. Ogni modello riflette una tabella nel database e contiene proprietà che corrispondono alle colonne nella tabella.

Fare doppio clic il **modelli** cartella e selezionare **Add** e **nuovo elemento**.

![Aggiungi nuovo elemento](creating-the-web-application/_static/image4.png)

Nella finestra Aggiungi nuovo elemento, selezionare **Data** nel riquadro sinistro e **ADO.NET Entity Data Model** tra le opzioni nel riquadro centrale. Denominare il nuovo file di modello **ContosoModel**.

![Crea modello](creating-the-web-application/_static/image5.png)

Fare clic su **Aggiungi**.

Nella procedura guidata Entity Data Model, selezionare **Entity Framework Designer da database**.

![Genera da database](creating-the-web-application/_static/image6.png)

Scegliere **Avanti**.

Se si dispone di connessioni di database definite all'interno dell'ambiente di sviluppo, si potrebbe vedere una di queste connessioni pre-selezionate. Tuttavia, si desidera creare una nuova connessione al database creato nella prima parte di questa esercitazione. Scegliere il **nuova connessione** pulsante.

![connettersi al database](creating-the-web-application/_static/image7.png)

Nella finestra proprietà di connessione, specificare il nome del server locale in cui è stato creato il database (in questo caso **\ProjectsV12 (localdb)**). Dopo aver specificato il nome del server, selezionare il ContosoUniversityData dai database di disponibilità.

![impostare le proprietà di connessione](creating-the-web-application/_static/image8.png)

Fare clic su **OK**.

Sono ora visualizzate le proprietà di connessione corretta. È possibile usare il nome predefinito per la connessione nel file Web. config

![impostazioni di connessione](creating-the-web-application/_static/image9.png)

Scegliere **Avanti**.

Selezionare **tabelle** per generare modelli per tutte le tre tabelle.

![Selezionare le tabelle](creating-the-web-application/_static/image10.png)

Scegliere **Fine**.

Se si riceve un avviso di sicurezza, selezionare **OK** per continuare l'esecuzione del modello.

I modelli vengono generati dalle tabelle del database e viene visualizzato un diagramma che mostra le proprietà e relazioni tra le tabelle.

![diagramma del modello](creating-the-web-application/_static/image11.png)

La cartella Models ora include molti nuovi file correlati ai modelli generati dal database.

![Mostra i nuovi file di modello](creating-the-web-application/_static/image12.png)

Il **ContosoModel.Context.cs** file contiene una classe che deriva dalle **DbContext** classe e fornisce una proprietà per ogni classe di modello che corrisponde a una tabella di database. Il **Course.cs**, **Enrollment.cs**, e **Student.cs** file contengono le classi del modello che rappresentano le tabelle di database. Si utilizzerà la classe del contesto sia le classi del modello quando si lavora con scaffolding.

Prima di procedere con questa esercitazione, compilare il progetto. Nella sezione successiva, si genererà codice basato su modelli di data, ma tale sezione non funzionerà se non è stato compilato il progetto.

> [!div class="step-by-step"]
> [Precedente](setting-up-database.md)
> [Successivo](generating-views.md)
