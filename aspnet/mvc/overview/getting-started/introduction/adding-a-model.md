---
uid: mvc/overview/getting-started/introduction/adding-a-model
title: Aggiunta di un modello | Documenti Microsoft
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 276552b5-f349-4fcf-8f40-6d042f7aa88e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: b3ef871c4d7627a03c8f0fd8cce9d3e97fc1a4ba
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30867450"
---
<a name="adding-a-model"></a>Aggiunta di un modello
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

In questa sezione si aggiungeranno alcune classi per la gestione di filmati in un database. Queste classi sarà il &quot;modello&quot; fa parte dell'applicazione ASP.NET MVC.

Si utilizzerà una tecnologia di accesso ai dati di .NET Framework nota come il [Entity Framework](https://docs.microsoft.com/ef/) per definire e utilizzare queste classi di modello. Entity Framework (noto anche come EF) supporta un paradigma di sviluppo denominato *Code First*. Codice, innanzitutto, consente di creare gli oggetti del modello mediante la scrittura di classi semplici. (Sono anche noti come classi POCO, da &quot;oggetti CLR normale precedente.&quot;) È il database creato al momento dalle classi, che consente a un flusso di lavoro di sviluppo molto pulito e rapido. Se è necessario creare innanzitutto il database, è comunque possibile eseguire questa esercitazione per apprendere lo sviluppo di app MVC ed Entity Framework. È quindi possibile seguire Tom Fizmakens [lo Scaffolding di ASP.NET](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) esercitazione che illustra il primo approccio di database.

## <a name="adding-model-classes"></a>Aggiunta di classi modello

In **Esplora soluzioni**, fare clic destro la *modelli* cartella, selezionare **Aggiungi**e quindi selezionare **classe**.

![](adding-a-model/_static/image1.png)

Immettere il *classe* nome &quot;film&quot;.

Aggiungere le seguenti cinque proprietà per il `Movie` classe:

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

Si userà la `Movie` classe per rappresentare filmati in un database. Ogni istanza di un `Movie` corrisponde a una riga all'interno di una tabella di database e ogni proprietà dell'oggetto di `Movie` classe verrà eseguito il mapping a una colonna nella tabella.

Nota: Per utilizzare System.Data.Entity e la classe correlata, è necessario installare il [pacchetto NuGet di Entity Framework](https://www.nuget.org/packages/EntityFramework/). Fare clic sul collegamento per ulteriori istruzioni.

Nello stesso file, aggiungere le seguenti `MovieDBContext` classe:

[!code-csharp[Main](adding-a-model/samples/sample2.cs?highlight=2,15-18)]

Il `MovieDBContext` classe rappresenta il contesto del database film Entity Framework, che gestisce il recupero, l'archiviazione e l'aggiornamento `Movie` classe istanze in un database. Il `MovieDBContext` deriva il `DbContext` classe forniti da Entity Framework di base.

Per poter fare riferimento a `DbContext` e `DbSet`, è necessario aggiungere il seguente `using` istruzione all'inizio del file:

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

È possibile farlo aggiungendo manualmente utilizzando l'istruzione oppure è possibile passare il mouse sulle righe ondulate rosse, fare clic su `Show potential fixes` e fare clic su `using System.Data.Entity;`

![](adding-a-model/_static/image2.png)

Nota: Alcuni inutilizzati `using` istruzioni sono state rimosse. Visual Studio Visualizza dipendenze non utilizzate in grigio. È possibile rimuovere le dipendenze non utilizzate passandovi sopra le dipendenze grigio, fare clic su `Show potential fixes` e fare clic su **Rimuovi using inutilizzate.**

![](adding-a-model/_static/image3.png)

Infine, è stato aggiunto un modello (M in MVC). Nella sezione successiva si utilizzerà la stringa di connessione di database.

> [!div class="step-by-step"]
> [Precedente](adding-a-view.md)
> [Successivo](creating-a-connection-string.md)
