---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
title: Aggiunta di un modello (VB) | Documenti Microsoft
author: Rick-Anderson
description: In questa esercitazione verrà fornite le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, ovvero...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: b3aa7720-5c78-4ca2-baef-9a52234fb7ce
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: e9a271c64347b4004d5cc5d9d91085c4e642e95d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868139"
---
<a name="adding-a-model-vb"></a>Aggiunta di un modello (VB)
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

> In questa esercitazione verrà fornite le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, che è una versione gratuita di Microsoft Visual Studio. Prima di iniziare, assicurarsi di che aver installato i prerequisiti elencati di seguito. È possibile installare tutti gli elementi facendo clic sul seguente collegamento: [installazione guidata piattaforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). In alternativa, è possibile installare singolarmente i prerequisiti utilizzando i collegamenti seguenti:
> 
> - [Prerequisiti di Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(supportano runtime + tools)
> 
> Se si utilizza Visual Studio 2010 anziché Visual Web Developer 2010, installare i prerequisiti, fare clic sul seguente collegamento: [prerequisiti di Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un progetto di Visual Web Developer con codice sorgente VB.NET complemento è disponibile in questo argomento. [Scaricare la versione di Visual Basic.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se si preferisce c#, passare il [c# versione](../cs/adding-a-model.md) di questa esercitazione.


## <a name="adding-a-model"></a>Aggiunta di un modello

In questa sezione si aggiungeranno alcune classi per la gestione di filmati in un database. Queste classi sarà la parte "modello" dell'applicazione ASP.NET MVC.

Utilizzare una tecnologia di accesso ai dati di .NET Framework nota come Entity Framework per definire e utilizzare queste classi di modello. Entity Framework (noto anche come EF) supporta un paradigma di sviluppo denominato *Code First*. Codice, innanzitutto, consente di creare gli oggetti del modello mediante la scrittura di classi semplici. (Sono anche noti come classi POCO, da "oggetti CLR plain-old.") È il database creato al momento dalle classi, che consente a un flusso di lavoro di sviluppo molto pulito e rapido.

## <a name="adding-model-classes"></a>Aggiunta di classi modello

In **Esplora soluzioni**, fare clic destro la *modelli* cartella, selezionare **Aggiungi**e quindi selezionare **classe**.

![](adding-a-model/_static/image1.png)

Nome della classe "Filmato".

Aggiungere le seguenti cinque proprietà per il `Movie` classe:

[!code-vb[Main](adding-a-model/samples/sample1.vb)]

Si userà la `Movie` classe per rappresentare filmati in un database. Ogni istanza di un `Movie` corrisponde a una riga all'interno di una tabella di database e ogni proprietà dell'oggetto di `Movie` classe verrà eseguito il mapping a una colonna nella tabella.

Nello stesso file, aggiungere le seguenti `MovieDBContext` classe:

[!code-vb[Main](adding-a-model/samples/sample2.vb)]

Il `MovieDBContext` classe rappresenta il contesto del database film Entity Framework, che gestisce il recupero, l'archiviazione e l'aggiornamento `Movie` classe istanze in un database. Il `MovieDBContext` deriva il `DbContext` classe forniti da Entity Framework di base. Per ulteriori informazioni su `DbContext` e `DbSet`, vedere [miglioramenti per la produttività per Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).

Per poter fare riferimento a `DbContext` e `DbSet`, è necessario aggiungere il seguente `imports` istruzione all'inizio del file:

[!code-vb[Main](adding-a-model/samples/sample3.vb)]

L'intero *Movie.vb* file è illustrato di seguito.

[!code-vb[Main](adding-a-model/samples/sample4.vb)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a>Creazione di una stringa di connessione e l'utilizzo di SQL Server Compact

Il `MovieDBContext` classe creata gestisce le attività di connessione al database e di mapping `Movie` oggetti per i record del database. Una domanda, che si potrebbe chiedere, tuttavia, viene illustrato come specificare il database si connetterà. Operazione dovrà essere effettuata mediante l'aggiunta di informazioni di connessione nel *Web. config* file dell'applicazione.

Aprire la radice dell'applicazione *Web. config* file. (Non il *Web. config* file nel *viste* cartella.) Nell'immagine seguente mostra entrambi *Web. config* file; aprire il *Web. config* file racchiuse in un cerchio rosso.

![](adding-a-model/_static/image2.png)

## 

Aggiungere la seguente stringa di connessione per il `<connectionStrings>` elemento il *Web. config* file.

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

Nell'esempio seguente viene mostrata una parte di *Web. config* file con la nuova stringa di connessione aggiunta:

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

Questa piccola quantità di codice e XML è tutto ciò che occorre per scrivere per rappresentare e archiviare i dati dei film in un database.

Successivamente, si creerà un nuovo `MoviesController` classe che è possibile utilizzare per visualizzare i dati dei film e consentire agli utenti di creare nuove voci di film.

> [!div class="step-by-step"]
> [Precedente](adding-a-view.md)
> [Successivo](accessing-your-models-data-from-a-controller.md)
