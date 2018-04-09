---
uid: mvc/overview/getting-started/database-first-development/enhancing-data-validation
title: 'Database di Entity Framework prima con ASP.NET MVC: miglioramento della convalida dei dati | Documenti Microsoft'
author: tfitzmac
description: Usa lo Scaffolding di ASP.NET MVC ed Entity Framework, è possibile creare un'applicazione web che fornisce un'interfaccia a un database esistente. Questa esercitazione seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/29/2014
ms.topic: article
ms.assetid: 0ed5e67a-34c0-4b57-84a6-802b0fb3cd00
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/enhancing-data-validation
msc.type: authoredcontent
ms.openlocfilehash: 8ea2e94db7956b76c5ccf0a139ac024e38910b49
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="ef-database-first-with-aspnet-mvc-enhancing-data-validation"></a>Database di Entity Framework prima con ASP.NET MVC: miglioramento della convalida dei dati
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Usa lo Scaffolding di ASP.NET MVC ed Entity Framework, è possibile creare un'applicazione web che fornisce un'interfaccia a un database esistente. Questa serie di esercitazioni viene illustrato come automaticamente generare il codice che consente agli utenti di visualizzare, modificare, creare ed eliminare dati che si trovano in una tabella di database. Il codice generato corrisponde alle colonne nella tabella di database.
> 
> Questa parte della serie è incentrata sull'aggiunta di annotazioni di dati al modello di dati per specificare i requisiti della convalida e formattazione di visualizzazione. È stato migliorato in base ai suggerimenti degli utenti nella sezione dei commenti.


## <a name="add-data-annotations"></a>Aggiungere le annotazioni dei dati

Come osservato nell'argomento precedente, vengono applicate automaticamente alcune regole di convalida dei dati per l'input dell'utente. Ad esempio, è possibile fornire solo un numero per la proprietà livello. Per specificare più regole di convalida di dati, è possibile aggiungere le annotazioni dei dati per la classe di modello. Queste annotazioni vengono applicate in tutta l'applicazione web per la proprietà specificata. È inoltre possibile applicare gli attributi di formattazione che modificano la modalità vengono visualizzate le proprietà. ad esempio, la modifica del valore utilizzato per le etichette di testo.

In questa esercitazione si aggiungerà le annotazioni dei dati per limitare la lunghezza dei valori forniti per le proprietà di FirstName, LastName e MiddleName. Nel database, questi valori sono limitati a 50 caratteri. Tuttavia, nell'applicazione web tale limite di caratteri attualmente non viene applicata. Se un utente fornisce più di 50 caratteri per uno di questi valori, la pagina verrà arrestato in modo anomalo durante il tentativo di salvare il valore per il database. Livello si limiterà anche per i valori compresi tra 0 e 4.

Aprire il **Student.cs** file nel **modelli** cartella. Aggiungere il codice evidenziato seguente alla classe.

[!code-csharp[Main](enhancing-data-validation/samples/sample1.cs?highlight=5,15,17,20)]

In Enrollment.cs, aggiungere il codice evidenziato di seguito.

[!code-csharp[Main](enhancing-data-validation/samples/sample2.cs?highlight=5,10)]

Compilare la soluzione.

Passare a una pagina per la modifica o la creazione di uno studente. Se si tenta di immettere più di 50 caratteri, viene visualizzato un messaggio di errore.

![Mostra messaggio di errore](enhancing-data-validation/_static/image1.png)

Passare alla pagina per modificare le registrazioni e tenta di fornire un livello sopra 4.

![Errore di intervallo di livello](enhancing-data-validation/_static/image2.png)

Per un elenco completo delle annotazioni di convalida dei dati è possibile applicare alle proprietà e le classi, vedere [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).

## <a name="add-metadata-classes"></a>Aggiungere le classi di metadati

L'aggiunta degli attributi di convalida direttamente alla classe di modello funziona se non si prevede il database da modificare; Tuttavia, se il database viene modificato ed è necessario rigenerare la classe modello, si perderanno tutti gli attributi che sono stati applicati alla classe di modello. Questo approccio può essere molto inefficiente e soggetto a perdere le regole di convalida importanti.

Per evitare questo problema, è possibile aggiungere una classe di metadati che contiene gli attributi. Quando si associa la classe di modello per la classe di metadati, tali attributi vengono applicati al modello. In questo caso, la classe modello può essere rigenerata senza perdere tutti gli attributi che sono stati applicati alla classe di metadati.

Nel **modelli** cartella, aggiungere una classe denominata **Metadata.cs**.

![aggiungere una classe di metadati](enhancing-data-validation/_static/image3.png)

Sostituire il codice in Metadata.cs con il codice seguente.

[!code-csharp[Main](enhancing-data-validation/samples/sample3.cs)]

Queste classi di metadati contengano tutti gli attributi di convalida che è stato applicato precedentemente a classi del modello. Il **visualizzazione** attributo viene utilizzato per modificare il valore utilizzato per le etichette di testo.

A questo punto, è necessario associare le classi di modello con le classi dei metadati.

Nel **modelli** cartella, aggiungere una classe denominata **PartialClasses.cs**.

Sostituire il contenuto del file con il codice seguente.

[!code-csharp[Main](enhancing-data-validation/samples/sample4.cs)]

Si noti che ogni classe è contrassegnata come un `partial` classe e ogni lo stesso nome e spazio dei nomi della classe generata automaticamente. Applicando l'attributo dei metadati per la classe parziale, assicurarsi che gli attributi di convalida di dati verranno applicati alla classe generata automaticamente. Questi attributi non andranno persi quando si rigenerano le classi di modello, poiché viene applicato l'attributo di metadati in classi parziali che non vengono rigenerate.

Per rigenerare le classi generate automaticamente, aprire il file ContosoModel.edmx. In questo caso, fare clic su area di progettazione e seleziona **il modello di aggiornamento dal Database**. Anche se non è stato modificato il database, questo processo verrà rigenerata le classi. Nel **aggiornamento** , selezionare **tabelle** e **fine**.

![aggiornare le tabelle](enhancing-data-validation/_static/image4.png)

Salvare il file ContosoModel.edmx per applicare le modifiche.

Aprire il file Student.cs o il file Enrollment.cs e si noti che gli attributi di convalida dei dati applicate in precedenza non sono più nel file. Tuttavia, eseguire l'applicazione e si noti che quando si immettono dati ancora vengono applicate le regole di convalida.

> [!div class="step-by-step"]
> [Precedente](customizing-a-view.md)
> [Successivo](publish-to-azure.md)
