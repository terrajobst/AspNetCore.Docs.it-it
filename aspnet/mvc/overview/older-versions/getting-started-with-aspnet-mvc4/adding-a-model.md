---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
title: Aggiunta di un modello | Microsoft Docs
author: Rick-Anderson
description: 'Nota: Una versione aggiornata di questa esercitazione è disponibile qui che usa ASP.NET MVC 5 e Visual Studio 2013. È più sicuro e molto più semplice da seguire e demo...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 53db72da-e0b9-44d9-b60b-6e6988c00b28
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 5308d2d1d11f954db8a4502adb42223f69e0c675
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578510"
---
<a name="adding-a-model"></a>Aggiunta di un modello
====================
da [Rick Anderson]((https://twitter.com/RickAndMSFT))

> > [!NOTE]
> > È disponibile una versione aggiornata di questa esercitazione [qui](../../getting-started/introduction/getting-started.md) che usa ASP.NET MVC 5 e Visual Studio 2013. È più sicuro, molto più semplice da seguire e vengono illustrate altre funzionalità.


In questa sezione si aggiungeranno alcune classi per la gestione di film in un database. Queste classi saranno la &quot;modello&quot; fa parte dell'applicazione ASP.NET MVC.

Si userà una tecnologia di accesso ai dati di .NET Framework nota come il [Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx) per definire e usare queste classi del modello. Entity Framework (noto anche come Entity Framework) supporta un paradigma di sviluppo denominato *Code First*. Prima di tutto i codice consente di creare gli oggetti del modello mediante la scrittura di classi semplici. (Queste sono anche note come classi POCO, dalla &quot;plain-old CLR Object.&quot;) È quindi possibile avere il database creato in tempo reale dalle classi, che consente a un flusso di lavoro molto pulito e rapida evoluzione.

## <a name="adding-model-classes"></a>Aggiunta di classi di modello

Nella **Esplora soluzioni**, fare clic il *modelli* cartella, selezionare **Add**e quindi selezionare **classe**.

![](adding-a-model/_static/image1.png)

Immettere il *classe* name &quot;film&quot;.

Aggiungere le seguenti cinque proprietà per il `Movie` classe:

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

Si userà il `Movie` classe per rappresentare i film in un database. Ogni istanza di un `Movie` oggetto corrisponderà a una riga all'interno di una tabella di database e ogni proprietà del `Movie` classe verrà eseguito il mapping a una colonna nella tabella.

Nello stesso file, aggiungere il codice seguente `MovieDBContext` classe:

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

Il `MovieDBContext` classe rappresenta il contesto di database di film Entity Framework, che gestisce il recupero, l'archiviazione e aggiornando `Movie` classe istanze in un database. Il `MovieDBContext` deriva dal `DbContext` classe fornita da Entity Framework di base.

Per poter fare riferimento a `DbContext` e `DbSet`, è necessario aggiungere il codice seguente `using` informativa nella parte superiore del file:

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

L'intero *Movie.cs* file è illustrato di seguito. (Diverse istruzioni using che non sono necessarie sono state rimosse.)

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Creazione di una stringa di connessione e l'utilizzo di LocalDB di SQL Server

Il `MovieDBContext` classe creata gestisce le attività di mapping e la connessione al database `Movie` oggetti per i record del database. Una domanda che ci si potrebbe chiedere, tuttavia, è illustrato come specificare i database a cui si connette. È necessario usare l'aggiunta di informazioni di connessione nel *Web. config* file dell'applicazione.

Aprire la radice dell'applicazione *Web. config* file. (Non il *Web. config* del file nei *viste* cartella.) Aprire il *Web. config* file evidenziate in rosso.

![](adding-a-model/_static/image2.png)

Aggiungere la seguente stringa di connessione per il `<connectionStrings>` elemento il *Web. config* file.

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

L'esempio seguente mostra una parte i *Web. config* file con la nuova stringa di connessione aggiunta:

[!code-xml[Main](adding-a-model/samples/sample6.xml?highlight=6-9)]

Questa piccola quantità di codice e XML è tutto ciò che occorre per scrivere allo scopo di rappresentare e archiviare i dati dei film in un database.

Successivamente, si creerà un nuovo `MoviesController` classe che è possibile usare per visualizzare i dati dei film e consentire agli utenti di creare nuove voci di filmato.

> [!div class="step-by-step"]
> [Precedente](adding-a-view.md)
> [Successivo](accessing-your-models-data-from-a-controller.md)
