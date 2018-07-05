---
uid: mvc/overview/getting-started/introduction/adding-a-model
title: Aggiunta di un modello | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 05/28/2015
ms.assetid: 276552b5-f349-4fcf-8f40-6d042f7aa88e
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 2d99a9c553ba79922cb0c5e852709ab97ea2e22b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818463"
---
<a name="adding-a-model"></a>Aggiunta di un modello
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

In questa sezione si aggiungeranno alcune classi per la gestione di film in un database. Queste classi saranno la &quot;modello&quot; fa parte dell'app ASP.NET MVC.

Si userà una tecnologia di accesso ai dati di .NET Framework nota come il [Entity Framework](https://docs.microsoft.com/ef/) per definire e usare queste classi del modello. Entity Framework (noto anche come Entity Framework) supporta un paradigma di sviluppo denominato *Code First*. Prima di tutto i codice consente di creare gli oggetti del modello mediante la scrittura di classi semplici. (Queste sono anche note come classi POCO, dalla &quot;plain-old CLR Object.&quot;) È quindi possibile avere il database creato in tempo reale dalle classi, che consente a un flusso di lavoro molto pulito e rapida evoluzione. Se viene richiesto di creare il database prima di tutto, è comunque possibile seguire questa esercitazione per informazioni sullo sviluppo di app MVC ed Entity Framework. È quindi possibile seguire Tom Fizmakens [Scaffolding di ASP.NET](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) esercitazione che illustra il primo approccio di database.

## <a name="adding-model-classes"></a>Aggiunta di classi di modello

Nella **Esplora soluzioni**, fare clic il *modelli* cartella, selezionare **Add**e quindi selezionare **classe**.

![](adding-a-model/_static/image1.png)

Immettere il *classe* name &quot;film&quot;.

Aggiungere le seguenti cinque proprietà per il `Movie` classe:

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

Si userà il `Movie` classe per rappresentare i film in un database. Ogni istanza di un `Movie` oggetto corrisponderà a una riga all'interno di una tabella di database e ogni proprietà del `Movie` classe verrà eseguito il mapping a una colonna nella tabella.

Nota: Per usare Data. Entity e la classe correlata, è necessario installare il [pacchetto NuGet di Entity Framework](https://www.nuget.org/packages/EntityFramework/). Fare clic sul collegamento per ulteriori istruzioni.

Nello stesso file, aggiungere il codice seguente `MovieDBContext` classe:

[!code-csharp[Main](adding-a-model/samples/sample2.cs?highlight=2,15-18)]

Il `MovieDBContext` classe rappresenta il contesto di database di film Entity Framework, che gestisce il recupero, l'archiviazione e aggiornando `Movie` classe istanze in un database. Il `MovieDBContext` deriva dal `DbContext` classe fornita da Entity Framework di base.

Per poter fare riferimento a `DbContext` e `DbSet`, è necessario aggiungere il codice seguente `using` informativa nella parte superiore del file:

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

Tale scopo, è possibile aggiungere manualmente l'utilizzo istruzione oppure è possibile passare il mouse sulle righe ondulate rosse, fare clic su `Show potential fixes` e fare clic su `using System.Data.Entity;`

![](adding-a-model/_static/image2.png)

Nota: Alcuni inutilizzati `using` istruzioni sono state rimosse. Visual Studio mostrerà le dipendenze non usate in grigio. È possibile rimuovere le dipendenze inutilizzate passandovi sopra le dipendenze grigio, fare clic su `Show potential fixes` e fare clic su **Rimuovi using inutilizzate.**

![](adding-a-model/_static/image3.png)

È stato infine aggiunto un modello (il valore M in MVC). Nella sezione successiva si utilizzerà la stringa di connessione di database.

> [!div class="step-by-step"]
> [Precedente](adding-a-view.md)
> [Successivo](creating-a-connection-string.md)
