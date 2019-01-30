---
uid: mvc/overview/getting-started/database-first-development/enhancing-data-validation
title: 'Esercitazione: Migliorare la convalida dei dati per Entity Framework Database First con app ASP.NET MVC'
description: Questa esercitazione è incentrata sull'aggiunta di annotazioni dei dati al modello di dati per specificare i requisiti della convalida e formattazione di visualizzazione.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: 0ed5e67a-34c0-4b57-84a6-802b0fb3cd00
msc.legacyurl: /mvc/overview/getting-started/database-first-development/enhancing-data-validation
msc.type: authoredcontent
ms.openlocfilehash: 85299d70c6cba52c1d40a42edfd429c96318134a
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236484"
---
# <a name="tutorial-enhance-data-validation-for-ef-database-first-with-aspnet-mvc-app"></a>Esercitazione: Migliorare la convalida dei dati per Entity Framework Database First con app ASP.NET MVC

Usa MVC, Entity Framework e lo Scaffolding di ASP.NET, è possibile creare un'applicazione web che fornisce un'interfaccia a un database esistente. Questa serie di esercitazioni illustra come generare il codice che consente agli utenti di visualizzare, modificare, creare automaticamente ed eliminare dati che si trovano in una tabella di database. Il codice generato corrispondente alle colonne nella tabella di database.

Questa esercitazione è incentrata sull'aggiunta di annotazioni dei dati al modello di dati per specificare i requisiti della convalida e formattazione di visualizzazione. È stato migliorato in base al feedback degli utenti nella sezione dei commenti.

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Aggiungere annotazioni dei dati
> * Aggiungere classi di metadati

## <a name="prerequisites"></a>Prerequisiti

* [Personalizzare una vista](customizing-a-view.md)

## <a name="add-data-annotations"></a>Aggiungere annotazioni dei dati

Come illustrato in un argomento precedente, alcune regole di convalida dei dati vengono automaticamente applicate agli input dell'utente. Ad esempio, è possibile fornire solo un numero per la proprietà di livello. Per specificare più regole di convalida dei dati, è possibile aggiungere annotazioni dei dati per la classe di modello. Queste annotazioni vengono applicate in tutta l'applicazione web per la proprietà specificata. È inoltre possibile applicare gli attributi di formattazione che modificano la modalità di visualizzazione di proprietà; ad esempio, la modifica del valore usato per le etichette di testo.

In questa esercitazione si aggiungerà le annotazioni dei dati per limitare la lunghezza dei valori forniti per le proprietà FirstName, LastName e MiddleName. Nel database, questi valori sono limitati a 50 caratteri. Tuttavia, nell'applicazione web tale limite di caratteri attualmente non viene applicata. Se si specificano più di 50 caratteri per uno di questi valori, la pagina viene interrotta quando prova a salvare il valore per il database. Livello si limiterà anche in valori compresi tra 0 e 4.

Selezionare **modelli** > **ContosoModel.edmx** > **ContosoModel.tt** e aprire il *Student.cs* file. Aggiungere il codice evidenziato seguente alla classe.

[!code-csharp[Main](enhancing-data-validation/samples/sample1.cs?highlight=5,15,17,20)]

Aprire *Enrollment.cs* e aggiungere il codice evidenziato seguente.

[!code-csharp[Main](enhancing-data-validation/samples/sample2.cs?highlight=5,10)]

Compilare la soluzione.

Fare clic su **elenco degli studenti** e selezionare **modificare**. Se si tenta di immettere più di 50 caratteri, viene visualizzato un messaggio di errore.

![Mostra messaggio di errore](enhancing-data-validation/_static/image1.png)

Tornare alla home page. Fare clic su **elenco di registrazioni** e selezionare **modificare**. Tentativo di fornire un livello superiori a 4. Si riceverà questo errore: *Il campo livello deve essere compreso tra 0 e 4.*

## <a name="add-metadata-classes"></a>Aggiungere classi di metadati

L'aggiunta di attributi di convalida direttamente alla classe di modello funziona quando non si prevede il database da modificare; Tuttavia, se il database viene modificato ed è necessario rigenerare la classe del modello, si perderanno tutti gli attributi applicati alla classe di modello. Questo approccio può essere molto inefficiente e soggetto a perdere le regole di convalida importanti.

Per evitare questo problema, è possibile aggiungere una classe di metadati che contiene gli attributi. Quando si associa la classe del modello alla classe di metadati, tali attributi vengono applicati al modello. Questo approccio, la classe modello può essere rigenerata senza perdita di tutti gli attributi che sono stati applicati alla classe di metadati.

Nel **modelli** cartella, aggiungere una classe denominata *Metadata.cs*.

Sostituire il codice nel *Metadata.cs* con il codice seguente.

[!code-csharp[Main](enhancing-data-validation/samples/sample3.cs)]

Queste classi di metadati contengono tutti gli attributi di convalida applicati in precedenza per le classi del modello. Il **Display** attributo viene usato per modificare il valore usato per le etichette di testo.

A questo punto, è necessario associare le classi del modello con le classi di metadati.

Nel **modelli** cartella, aggiungere una classe denominata *PartialClasses.cs*.

Sostituire il contenuto del file con il codice seguente.

[!code-csharp[Main](enhancing-data-validation/samples/sample4.cs)]

Si noti che ogni classe è contrassegnata come un `partial` classe e ogni corrisponde al nome e lo spazio dei nomi della classe generata automaticamente. Applicando l'attributo di metadati alla classe parziale, assicurarsi che gli attributi di convalida dei dati saranno essere applicati alla classe generata automaticamente. Questi attributi non andranno persi quando si rigenerano le classi del modello perché viene applicato l'attributo di metadati nelle classi parziali che non vengono rigenerate.

Per rigenerare le classi generate automaticamente, aprire il *ContosoModel.edmx* file. Ancora una volta, fare clic su area di progettazione e seleziona **Aggiorna modello da Database**. Anche se non sono stati modificati del database, questo processo verrà rigenerare le classi. Nel **Refresh** scheda, seleziona **tabelle** e **fine**.

Salvare il *ContosoModel.edmx* file per applicare le modifiche.

Aprire il *Student.cs* file o la *Enrollment.cs* file e notare che gli attributi di convalida dei dati è stato applicato in precedenza non sono più nel file. Tuttavia, eseguire l'applicazione e si noti che le regole di convalida vengono comunque applicate quando si immettono dati.

## <a name="additional-resources"></a>Risorse aggiuntive

Per un elenco completo delle annotazioni di convalida dei dati è possibile applicare alle proprietà e classi, vedere [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).

## <a name="next-steps"></a>Passaggi successivi

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Annotazioni di dati aggiunti
> * Classi di metadati aggiunti

Passare all'esercitazione successiva per informazioni su come pubblicare l'app web e database in Azure.
> [!div class="nextstepaction"]
> [Pubblicare in Azure](publish-to-azure.md)