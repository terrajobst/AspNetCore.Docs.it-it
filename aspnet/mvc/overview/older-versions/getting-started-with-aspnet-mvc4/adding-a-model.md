---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
title: Aggiunta di un modello | Documenti Microsoft
author: Rick-Anderson
description: "Nota: Una versione aggiornata di questa esercitazione è disponibile qui che utilizza ASP.NET MVC 5 e Visual Studio 2013. È più sicuro, molto più semplice seguire e demo..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 53db72da-e0b9-44d9-b60b-6e6988c00b28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 1d066e4bab866a2195647f43aa886279fee941db
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-model"></a>Aggiunta di un modello
====================
Da [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > È disponibile una versione aggiornata di questa esercitazione [qui](../../getting-started/introduction/getting-started.md) che utilizza ASP.NET MVC 5 e Visual Studio 2013. È molto più semplice da seguire, più sicuro e vengono illustrate altre funzionalità.


In questa sezione si aggiungeranno alcune classi per la gestione di filmati in un database. Queste classi sarà il &quot;modello&quot; parte dell'applicazione ASP.NET MVC.

Si utilizzerà una tecnologia di accesso ai dati di .NET Framework nota come il [Entity Framework](https://msdn.microsoft.com/en-us/library/bb399572(VS.110).aspx) per definire e utilizzare queste classi di modello. Entity Framework (noto anche come EF) supporta un paradigma di sviluppo denominato *Code First*. Codice, innanzitutto, consente di creare gli oggetti del modello mediante la scrittura di classi semplici. (Sono anche noti come classi POCO, da &quot;oggetti CLR normale precedente.&quot;) È il database creato al momento dalle classi, che consente a un flusso di lavoro di sviluppo molto pulito e rapido.

## <a name="adding-model-classes"></a>Aggiunta di classi modello

In **Esplora soluzioni**, fare clic destro la *modelli* cartella, selezionare **Aggiungi**e quindi selezionare **classe**.

![](adding-a-model/_static/image1.png)

Immettere il *classe* nome &quot;film&quot;.

Aggiungere le seguenti cinque proprietà per il `Movie` classe:

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

Si userà la `Movie` classe per rappresentare filmati in un database. Ogni istanza di un `Movie` corrisponde a una riga all'interno di una tabella di database e ogni proprietà dell'oggetto di `Movie` classe verrà eseguito il mapping a una colonna nella tabella.

Nello stesso file, aggiungere le seguenti `MovieDBContext` classe:

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

Il `MovieDBContext` classe rappresenta il contesto del database film Entity Framework, che gestisce il recupero, l'archiviazione e l'aggiornamento `Movie` classe istanze in un database. Il `MovieDBContext` deriva il `DbContext` classe forniti da Entity Framework di base.

Per poter fare riferimento a `DbContext` e `DbSet`, è necessario aggiungere il seguente `using` istruzione all'inizio del file:

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

L'intero *Movie.cs* file è illustrato di seguito. (Più utilizzando le istruzioni che non sono necessarie sono state rimosse.)

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Creazione di una stringa di connessione e l'utilizzo di SQL Server LocalDB

Il `MovieDBContext` classe creata gestisce le attività di connessione al database e di mapping `Movie` oggetti per i record del database. Una domanda, che si potrebbe chiedere, tuttavia, viene illustrato come specificare il database si connetterà. Operazione dovrà essere effettuata mediante l'aggiunta di informazioni di connessione nel *Web. config* file dell'applicazione.

Aprire la radice dell'applicazione *Web. config* file. (Non il *Web. config* file nel *viste* cartella.) Aprire il *Web. config* file indicati in rosso.

![](adding-a-model/_static/image2.png)

Aggiungere la seguente stringa di connessione per il `<connectionStrings>` elemento il *Web. config* file.

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

Nell'esempio seguente viene mostrata una parte di *Web. config* file con la nuova stringa di connessione aggiunta:

[!code-xml[Main](adding-a-model/samples/sample6.xml?highlight=6-9)]

Questa piccola quantità di codice e XML è tutto ciò che occorre per scrivere per rappresentare e archiviare i dati dei film in un database.

Successivamente, si creerà un nuovo `MoviesController` classe che è possibile utilizzare per visualizzare i dati dei film e consentire agli utenti di creare nuove voci di film.

>[!div class="step-by-step"]
[Precedente](adding-a-view.md)
[Successivo](accessing-your-models-data-from-a-controller.md)
