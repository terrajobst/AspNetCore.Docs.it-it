---
uid: mvc/overview/getting-started/database-first-development/setting-up-database
title: Guida introduttiva a 6 Database First di Entity Framework con MVC 5 | Microsoft Docs
author: tfitzmac
description: Usa MVC, Entity Framework e lo Scaffolding di ASP.NET, è possibile creare un'applicazione web che fornisce un'interfaccia a un database esistente. Questa esercitazione seri...
ms.author: aspnetcontent
ms.date: 10/01/2014
ms.assetid: 095abad4-3bfe-4f06-b092-ae6a735b7e49
msc.legacyurl: /mvc/overview/getting-started/database-first-development/setting-up-database
msc.type: authoredcontent
ms.openlocfilehash: 20e590ad1b3a59f93d1ba48a2564ddc1a5cbb315
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836120"
---
<a name="getting-started-with-entity-framework-6-database-first-using-mvc-5"></a>Guida introduttiva a 6 Database First di Entity Framework con MVC 5
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Usa MVC, Entity Framework e lo Scaffolding di ASP.NET, è possibile creare un'applicazione web che fornisce un'interfaccia a un database esistente. Questa serie di esercitazioni illustra come generare il codice che consente agli utenti di visualizzare, modificare, creare automaticamente ed eliminare dati che si trovano in una tabella di database. Il codice generato corrispondente alle colonne nella tabella di database. Nell'ultima parte della serie, si distribuirà il sito e il database di Azure.
> 
> Questa parte della serie è incentrato sulla creazione di un database e popolarlo con i dati.
> 
> Questa serie è stato scritto con contributi Tom Dykstra e Rick Anderson. È stato migliorato in base al feedback degli utenti nella sezione dei commenti.


## <a name="introduction"></a>Introduzione

Questo argomento illustra come iniziare con un oggetto esistente del database e creare rapidamente un'applicazione web che consente agli utenti di interagire con i dati. Per compilare l'applicazione web Usa l'Entity Framework 6 e MVC 5. La funzionalità di Scaffolding di ASP.NET consente di generare automaticamente codice per la visualizzazione, l'aggiornamento, la creazione e l'eliminazione dei dati. Usa gli strumenti di pubblicazione all'interno di Visual Studio, è possibile distribuire facilmente il sito e il database in Azure.

Questo argomento illustra la situazione in cui si dispone di un database e si desidera generare il codice per un'applicazione web in base ai campi di tale database. Questo approccio è definito lo sviluppo di Database First. Se si dispone già di un database esistente, è possibile usare invece un approccio definito lo sviluppo Code First che prevede la definizione delle classi di dati e generazione del database dalle proprietà della classe.

Per un esempio introduttivo di sviluppo con Code First, vedere [Introduzione a ASP.NET MVC 5](../introduction/getting-started.md). Per un esempio più avanzato, vedere [creazione di un modello di dati di Entity Framework per un'App ASP.NET MVC 4](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

Per indicazioni sulla selezione l'approccio Entity Framework da utilizzare, vedere [approcci di sviluppo di Entity Framework](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).

## <a name="prerequisites"></a>Prerequisiti

Visual Studio 2013 o Visual Studio Express 2013 per Web

## <a name="set-up-the-database"></a>Configurare il database

Per simulare l'ambiente di disporre di un database esistente, è prima di tutto creare un database con alcuni dati pre-popolata e quindi creare l'applicazione web che si connette al database.

Questa esercitazione è stata sviluppata l'utilizzo di LocalDB con Visual Studio 2013 o Visual Studio Express 2013 per Web. È possibile usare un server di database esistente anziché LocalDB, ma a seconda della versione di Visual Studio e il tipo di database, tutti gli strumenti dati in Visual Studio potrebbe non essere supportati. Se gli strumenti non sono disponibili per il database, si potrebbe essere necessario eseguire alcuni passaggi specifici del database all'interno della suite di gestione per il database.

Se si dispone di un problema con gli strumenti di database nella versione di Visual Studio, assicurarsi che sia installata la versione più recente degli strumenti di database. Per informazioni sull'aggiornamento o installazione degli strumenti di database, vedere [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/hh297027).

Avviare Visual Studio e creare un **progetto di Database di SQL Server**. Denominare il progetto **ContosoUniversityData**.

![creare il progetto di database](setting-up-database/_static/image1.png)

È ora disponibile un progetto di database vuoto. Si distribuirà questo database di Azure più avanti in questa esercitazione, quindi è necessario impostare il Database SQL di Azure come piattaforma di destinazione per il progetto. Impostare la piattaforma di destinazione non distribuisce effettivamente il database. Ciò significa semplicemente che il progetto di database verrà verificato che la progettazione del database è compatibile con la piattaforma di destinazione. Per impostare la piattaforma di destinazione, aprire il **delle proprietà** per il progetto e selezionare **Database SQL di Microsoft Azure** per la piattaforma di destinazione.

![impostare la destinazione piattaforma](setting-up-database/_static/image2.png)

È possibile creare le tabelle necessarie per questa esercitazione aggiungendo gli script SQL che definiscono le tabelle. Mouse sul progetto e aggiungere un nuovo elemento.

![Aggiungi nuovo elemento](setting-up-database/_static/image3.png)

Aggiungere una nuova tabella denominata studente.

![Aggiungi tabella student](setting-up-database/_static/image4.png)

Nel file della tabella, sostituire il comando T-SQL con il codice seguente per creare la tabella.

[!code-sql[Main](setting-up-database/samples/sample1.sql)]

Si noti che la finestra di progettazione si sincronizza automaticamente con il codice. È possibile lavorare con il codice o la finestra di progettazione.

![Mostra codice e progettazione](setting-up-database/_static/image5.png)

Aggiungere un'altra tabella. Questa volta denominarlo Course e usare il comando T-SQL seguente.

[!code-sql[Main](setting-up-database/samples/sample2.sql)]

E, ancora una volta per creare una tabella denominata registrazione ripetere.

[!code-sql[Main](setting-up-database/samples/sample3.sql)]

È possibile popolare il database con i dati tramite uno script che viene eseguito dopo che il database viene distribuito. Aggiungere uno Script di post-distribuzione per il progetto. È possibile usare il nome predefinito.

![aggiungere uno script di post-distribuzione](setting-up-database/_static/image6.png)

Aggiungere il codice T-SQL seguente allo script di post-distribuzione. Questo script aggiunge semplicemente i dati al database quando non viene trovato alcun record corrispondente. Non sovrascrivere o eliminare i dati che immessi nel database.

[!code-sql[Main](setting-up-database/samples/sample4.sql)]

È importante notare che lo script di post-distribuzione viene eseguito ogni volta che si distribuisce il progetto di database. Pertanto, è necessario considerare attentamente i requisiti durante la scrittura di questo script. In alcuni casi, si desidera iniziare da capo da un set di dati noto ogni volta che viene distribuito il progetto. In altri casi, è possibile non modificare i dati esistenti in alcun modo. In base alle esigenze, è possibile decidere se è necessario uno script di post-distribuzione o quello che devi includere nello script. Per altre informazioni sul popolamento del database con uno script di post-distribuzione, vedere [tra cui i dati in un progetto di Database di SQL Server](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx).

È ora possibile 4 file script SQL, ma non le tabelle effettive. Si è pronti per distribuire il progetto di database in localdb. In Visual Studio, fare clic sul pulsante Start (o F5) per compilare e distribuire il progetto di database. Controllare la scheda di Output per verificare che la compilazione e la distribuzione ha avuto esito positivo.

![Mostra output](setting-up-database/_static/image7.png)

Per verificare che il nuovo database è stato creato, aprire **Esplora oggetti di SQL Server** e cercare il nome del progetto nel server di database locale corretto (in questo caso **\ProjectsV12 (localdb)**).

![visualizzare di nuovo database](setting-up-database/_static/image8.png)

Per verificare che le tabelle vengono popolate con i dati, fare doppio clic su una tabella e selezionare **dati della visualizzazione**.

![Mostra dati tabella](setting-up-database/_static/image9.png)

Viene mostrata una visualizzazione modificabile dei dati della tabella.

![visualizzare i risultati dei dati di tabella](setting-up-database/_static/image10.png)

Il database è ora configurato e popolato con i dati. Nella prossima esercitazione, si creerà un'applicazione web per il database.

> [!div class="step-by-step"]
> [avanti](creating-the-web-application.md)
