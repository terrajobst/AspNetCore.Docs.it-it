---
uid: mvc/overview/getting-started/database-first-development/enhancing-data-validation
title: 'Database di Entity Framework prima di tutto con ASP.NET MVC: miglioramento della convalida dei dati | Microsoft Docs'
author: Rick-Anderson
description: Usa MVC, Entity Framework e lo Scaffolding di ASP.NET, è possibile creare un'applicazione web che fornisce un'interfaccia a un database esistente. Questa esercitazione seri...
ms.author: riande
ms.date: 12/29/2014
ms.assetid: 0ed5e67a-34c0-4b57-84a6-802b0fb3cd00
msc.legacyurl: /mvc/overview/getting-started/database-first-development/enhancing-data-validation
msc.type: authoredcontent
ms.openlocfilehash: df2cd99619f097c9f392e8fe7352c1ce3a69c8df
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021664"
---
<a name="ef-database-first-with-aspnet-mvc-enhancing-data-validation"></a>Database di Entity Framework prima di tutto con ASP.NET MVC: miglioramento della convalida dei dati
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Usa MVC, Entity Framework e lo Scaffolding di ASP.NET, è possibile creare un'applicazione web che fornisce un'interfaccia a un database esistente. Questa serie di esercitazioni illustra come generare il codice che consente agli utenti di visualizzare, modificare, creare automaticamente ed eliminare dati che si trovano in una tabella di database. Il codice generato corrispondente alle colonne nella tabella di database.
> 
> Questa parte della serie si concentra sull'aggiunta di annotazioni dei dati al modello di dati per specificare i requisiti della convalida e formattazione di visualizzazione. È stato migliorato in base al feedback degli utenti nella sezione dei commenti.


## <a name="add-data-annotations"></a>Aggiungere annotazioni dei dati

Come illustrato in un argomento precedente, alcune regole di convalida dei dati vengono automaticamente applicate agli input dell'utente. Ad esempio, è possibile fornire solo un numero per la proprietà di livello. Per specificare più regole di convalida dei dati, è possibile aggiungere annotazioni dei dati per la classe di modello. Queste annotazioni vengono applicate in tutta l'applicazione web per la proprietà specificata. È inoltre possibile applicare gli attributi di formattazione che modificano la modalità di visualizzazione di proprietà; ad esempio, la modifica del valore usato per le etichette di testo.

In questa esercitazione si aggiungerà le annotazioni dei dati per limitare la lunghezza dei valori forniti per le proprietà FirstName, LastName e MiddleName. Nel database, questi valori sono limitati a 50 caratteri. Tuttavia, nell'applicazione web tale limite di caratteri attualmente non viene applicata. Se si specificano più di 50 caratteri per uno di questi valori, la pagina viene interrotta quando prova a salvare il valore per il database. Livello si limiterà anche in valori compresi tra 0 e 4.

Aprire il **Student.cs** del file nei **modelli** cartella. Aggiungere il codice evidenziato seguente alla classe.

[!code-csharp[Main](enhancing-data-validation/samples/sample1.cs?highlight=5,15,17,20)]

In Enrollment.cs, aggiungere il codice evidenziato seguente.

[!code-csharp[Main](enhancing-data-validation/samples/sample2.cs?highlight=5,10)]

Compilare la soluzione.

Passare a una pagina per la modifica o la creazione di uno studente. Se si tenta di immettere più di 50 caratteri, viene visualizzato un messaggio di errore.

![Mostra messaggio di errore](enhancing-data-validation/_static/image1.png)

Passare alla pagina per la modifica delle registrazioni e tenta di fornire un livello superiori a 4.

![Errore di intervallo di livello](enhancing-data-validation/_static/image2.png)

Per un elenco completo delle annotazioni di convalida dei dati è possibile applicare alle proprietà e classi, vedere [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).

## <a name="add-metadata-classes"></a>Aggiungere classi di metadati

L'aggiunta di attributi di convalida direttamente alla classe di modello funziona quando non si prevede il database da modificare; Tuttavia, se il database viene modificato ed è necessario rigenerare la classe del modello, si perderanno tutti gli attributi applicati alla classe di modello. Questo approccio può essere molto inefficiente e soggetto a perdere le regole di convalida importanti.

Per evitare questo problema, è possibile aggiungere una classe di metadati che contiene gli attributi. Quando si associa la classe del modello alla classe di metadati, tali attributi vengono applicati al modello. Questo approccio, la classe modello può essere rigenerata senza perdita di tutti gli attributi che sono stati applicati alla classe di metadati.

Nel **modelli** cartella, aggiungere una classe denominata **Metadata.cs**.

![aggiungere classe di metadati](enhancing-data-validation/_static/image3.png)

Sostituire il codice in Metadata.cs con il codice seguente.

[!code-csharp[Main](enhancing-data-validation/samples/sample3.cs)]

Queste classi di metadati contengono tutti gli attributi di convalida applicati in precedenza per le classi del modello. Il **Display** attributo viene usato per modificare il valore usato per le etichette di testo.

A questo punto, è necessario associare le classi del modello con le classi di metadati.

Nel **modelli** cartella, aggiungere una classe denominata **PartialClasses.cs**.

Sostituire il contenuto del file con il codice seguente.

[!code-csharp[Main](enhancing-data-validation/samples/sample4.cs)]

Si noti che ogni classe è contrassegnata come un `partial` classe e ogni corrisponde al nome e lo spazio dei nomi della classe generata automaticamente. Applicando l'attributo di metadati alla classe parziale, assicurarsi che gli attributi di convalida dei dati saranno essere applicati alla classe generata automaticamente. Questi attributi non andranno persi quando si rigenerano le classi del modello perché viene applicato l'attributo di metadati nelle classi parziali che non vengono rigenerate.

Per rigenerare le classi generate automaticamente, aprire il file ContosoModel.edmx. Ancora una volta, fare clic su area di progettazione e seleziona **Aggiorna modello da Database**. Anche se non sono stati modificati del database, questo processo verrà rigenerare le classi. Nel **Refresh** scheda, seleziona **tabelle** e **fine**.

![Aggiorna tabelle](enhancing-data-validation/_static/image4.png)

Salvare il file ContosoModel.edmx per applicare le modifiche.

Aprire il file Student.cs o il file Enrollment.cs e notare che gli attributi di convalida dei dati che è stato applicato in precedenza non sono più nel file. Tuttavia, eseguire l'applicazione e si noti che le regole di convalida vengono comunque applicate quando si immettono dati.

> [!div class="step-by-step"]
> [Precedente](customizing-a-view.md)
> [Successivo](publish-to-azure.md)
