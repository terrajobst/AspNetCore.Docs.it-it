---
uid: mvc/overview/getting-started/database-first-development/setting-up-database
title: Introduzione a Entity Framework 6 Database First MVC 5 con | Documenti Microsoft
author: tfitzmac
description: Usa lo Scaffolding di ASP.NET MVC ed Entity Framework, è possibile creare un'applicazione web che fornisce un'interfaccia a un database esistente. Questa esercitazione seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: 095abad4-3bfe-4f06-b092-ae6a735b7e49
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/setting-up-database
msc.type: authoredcontent
ms.openlocfilehash: ae60b5c808d2522c66dc17ccf7d16fefdc65d552
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879335"
---
<a name="getting-started-with-entity-framework-6-database-first-using-mvc-5"></a>Introduzione a Entity Framework 6 Database First con MVC 5
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Usa lo Scaffolding di ASP.NET MVC ed Entity Framework, è possibile creare un'applicazione web che fornisce un'interfaccia a un database esistente. Questa serie di esercitazioni viene illustrato come automaticamente generare il codice che consente agli utenti di visualizzare, modificare, creare ed eliminare dati che si trovano in una tabella di database. Il codice generato corrisponde alle colonne nella tabella di database. Nell'ultima parte della serie, si distribuirà il sito e il database in Azure.
> 
> Questa parte della serie è incentrata sulla creazione di un database e il suo popolamento con dati.
> 
> Questa serie è stato scritto con i contributi da Tom Dykstra e Rick Anderson. È stato migliorato in base ai suggerimenti degli utenti nella sezione dei commenti.


## <a name="introduction"></a>Introduzione

Questo argomento viene illustrato come iniziare con un oggetto esistente del database e creare rapidamente un'applicazione web che consente agli utenti di interagire con i dati. Per compilare l'applicazione web Usa il Entity Framework 6 e 5 per MVC. La funzionalità di Scaffolding di ASP.NET consente di generare automaticamente il codice per la visualizzazione, l'aggiornamento, la creazione e l'eliminazione dei dati. Utilizzando gli strumenti di pubblicazione all'interno di Visual Studio, è possibile distribuire facilmente il sito e il database in Azure.

Questo argomento illustra la situazione in cui si dispone di un database e si desidera generare il codice per un'applicazione web in base ai campi di tale database. Questo approccio viene definito lo sviluppo del Database First. Se si dispone già di un database esistente, è possibile utilizzare invece un approccio definito per lo sviluppo Code First che include la definizione di classi di dati e la generazione del database dalle proprietà della classe.

Per un esempio introduttivo dello sviluppo Code First, vedere [Introduzione a ASP.NET MVC 5](../introduction/getting-started.md). Per un esempio più avanzato, vedere [creazione di un modello di dati di Entity Framework per applicazioni ASP.NET MVC 4](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

Per istruzioni sulla selezione l'approccio Entity Framework da utilizzare, vedere [approcci allo sviluppo con Entity Framework](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).

## <a name="prerequisites"></a>Prerequisiti

Visual Studio 2013 o Visual Studio Express 2013 per Web

## <a name="set-up-the-database"></a>Configurare il database

Per simulare l'ambiente di disporre di un database esistente, è prima di creare un database con alcuni dati precompilati e quindi creare l'applicazione web che si connette al database.

In questa esercitazione è stata sviluppata con LocalDB con Visual Studio 2013 o Visual Studio Express 2013 per Web. È possibile utilizzare un server di database esistente anziché LocalDB, ma a seconda della versione di Visual Studio e il tipo di database, tutti gli strumenti dati in Visual Studio potrebbe non essere supportati. Se gli strumenti non sono disponibili per il database, si potrebbe essere necessario eseguire alcuni passaggi specifici del database all'interno del gruppo di gestione per il database.

Se si verifica un problema con gli strumenti di database nella versione di Visual Studio, assicurarsi che è stata installata la versione più recente degli strumenti di database. Per informazioni sull'aggiornamento o l'installazione di strumenti di database, vedere [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/hh297027).

Avviare Visual Studio e creare un **il progetto di Database di SQL Server**. Denominare il progetto **ContosoUniversityData**.

![creare il progetto di database](setting-up-database/_static/image1.png)

È ora disponibile un progetto di database vuoto. Si distribuirà questo database in Azure più avanti in questa esercitazione, quindi è necessario impostare i Database SQL di Azure come piattaforma di destinazione per il progetto. Impostazione della piattaforma di destinazione non viene effettivamente distribuito del database. significa solo che il progetto di database verrà verificato che il database è compatibile con la piattaforma di destinazione. Per impostare la piattaforma di destinazione, aprire il **proprietà** per il progetto e selezionare **Database SQL di Microsoft Azure** per la piattaforma di destinazione.

![piattaforma di destinazione set](setting-up-database/_static/image2.png)

È possibile creare le tabelle necessarie per questa esercitazione aggiungendo gli script SQL che definiscono le tabelle. Mouse sul progetto e aggiungere un nuovo elemento.

![Aggiungi nuovo elemento](setting-up-database/_static/image3.png)

Aggiungere una nuova tabella denominata Student.

![aggiungere una tabella di studenti](setting-up-database/_static/image4.png)

Nel file di tabella, sostituire il comando T-SQL con il codice seguente per creare la tabella.

[!code-sql[Main](setting-up-database/samples/sample1.sql)]

Si noti che la finestra di progettazione si sincronizza automaticamente con il codice. Per lavorare con il codice o la finestra di progettazione.

![visualizzare codice e del progetto](setting-up-database/_static/image5.png)

Aggiungere un'altra tabella. Questa volta denominarlo corso e utilizzare il comando T-SQL seguente.

[!code-sql[Main](setting-up-database/samples/sample2.sql)]

E ripetere ancora una volta per creare una tabella denominata registrazione.

[!code-sql[Main](setting-up-database/samples/sample3.sql)]

È possibile popolare il database con i dati tramite uno script che viene eseguito dopo che il database viene distribuito. Aggiungere uno Script di post-distribuzione per il progetto. È possibile utilizzare il nome predefinito.

![aggiungere uno script di post-distribuzione](setting-up-database/_static/image6.png)

Aggiungere il codice T-SQL seguente allo script di post-distribuzione. Quando non viene trovato alcun record corrispondente, questo script aggiunge semplicemente dati al database. Non sovrascrivere o eliminare i dati che immessi nel database.

[!code-sql[Main](setting-up-database/samples/sample4.sql)]

È importante notare che lo script di post-distribuzione viene eseguito ogni volta che si distribuisce il progetto di database. Pertanto, è necessario considerare con attenzione i requisiti durante la scrittura di script. In alcuni casi, è preferibile iniziare da un set di dati noto ogni volta che viene distribuito il progetto. In altri casi, non è di modificare i dati esistenti in alcun modo. In base alle esigenze, è possibile decidere se è necessario uno script di post-distribuzione o si desidera includere nello script. Per ulteriori informazioni sul popolamento del database con uno script di post-distribuzione, vedere [dati inclusi in un progetto di Database di SQL Server](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx).

È ora possibile 4 file di script SQL, ma non le tabelle effettive. Si è pronti per distribuire il progetto di database in localdb. In Visual Studio, fare clic sul pulsante Start (o F5) per compilare e distribuire il progetto di database. Controllare la scheda di Output per verificare che la compilazione e distribuzione ha avuto esito positivo.

![Mostra output](setting-up-database/_static/image7.png)

Per verificare che il nuovo database è stato creato, aprire **Esplora oggetti di SQL Server** e cercare il nome del progetto nel server di database locale corretto (in questo caso **(localdb) \ProjectsV12**).

![Mostra i nuovi database](setting-up-database/_static/image8.png)

Per verificare che le tabelle vengono popolate con dati, una tabella e scegliere **Visualizza dati**.

![Mostra dati tabella](setting-up-database/_static/image9.png)

Viene visualizzata una visualizzazione modificabile dei dati della tabella.

![Mostra i risultati di dati tabella](setting-up-database/_static/image10.png)

Il database è ora configurato e popolato con dati. Nella prossima esercitazione, si creerà un'applicazione web per il database.

> [!div class="step-by-step"]
> [avanti](creating-the-web-application.md)
