---
title: Aggiunta di un modello a un'app di pagine Razor in ASP.NET Core
author: rick-anderson
description: Aggiunta di un modello a un'app di pagine Razor in ASP.NET Core
keywords: ASP.NET Core, pagine Razor, Razor, MVC
ms.author: riande
manager: wpickett
ms.date: 07/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/modelz
ms.openlocfilehash: 8e370decfd81e62022478b0ab695ff876e5e0a10
ms.sourcegitcommit: 74a8ad9c1ba5c155d7c4303e67632a0922c38e86
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/20/2017
---
# <a name="adding-a-model-to-a-razor-pages-app"></a>Aggiunta di un modello a un'app di pagine Razor

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Aggiungere un modello di dati

In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto **RazorPagesMovie** > **Aggiungi** > **Nuova cartella**. Assegnare il nome *Modelli* alla cartella.

Fare clic con il pulsante destro del mouse sulla cartella *Models* > **Aggiungi** > **Classe**. Assegnare un nome alla classe **Movie** e aggiungere le proprietà seguenti:

[!INCLUDE[model 2](../../includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a>Aggiungere una stringa di connessione del database

Aggiungere una stringa di connessione al file *appsettings.json*.

[!code-json[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a>Registrare il contesto del database

Registrare il contesto del database con il contenitore [Dependency Injection](xref:fundamentals/dependency-injection) (inserimento delle dipendenze) nel file *Startup.cs*.

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-6)]

Compilare il progetto per verificare che non ci siano errori.

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a>Aggiungere gli strumenti di scaffolding ed eseguire la migrazione iniziale

In questa sezione viene usata la Console di Gestione pacchetti per:

* Aggiungere il pacchetto di generazione codice Web di Visual Studio. Questo pacchetto è necessario per eseguire il motore di scaffolding.
* Aggiungere una migrazione iniziale.
* Aggiornare il database con la migrazione iniziale.

Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet > Console di Gestione pacchetti**.

  ![Menu della Console di Gestione pacchetti](../first-mvc-app/adding-model/_static/pmc.png)

In PMC, immettere i comandi seguenti:

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.0
Add-Migration Initial
Update-Database
```

Il comando `Install-Package` installa gli strumenti necessari a eseguire il motore di scaffolding.

Il comando `Add-Migration` genera un codice per creare lo schema del database iniziale. Lo schema è basato sul modello specificato in `DbContext` (nel file *Models/MovieContext.cs*). L'argomento `Initial` viene usato per denominare le migrazioni. È possibile usare qualunque nome, ma per convenzione si sceglie un nome che descrive la migrazione. Per altre informazioni vedere [Introduzione alle migrazioni](xref:data/ef-mvc/migrations#introduction-to-migrations).

Il comando `Update-Database` esegue il metodo `Up` nel file *Migrations/\<time-stamp>_InitialCreate.cs*, che crea il database.

[!INCLUDE[model 4windows](../../includes/RP/model4Win.md)]

[!INCLUDE[model 4](../../includes/RP/model4.md)]

L'esercitazione successiva illustra i file creati dallo scaffolding.

>[!div class="step-by-step"]
[Indietro: Introduzione](xref:tutorials/razor-pages/razor-pages-start)
[Avanti: pagine Razor create tramite scaffolding](xref:tutorials/razor-pages/page)    
