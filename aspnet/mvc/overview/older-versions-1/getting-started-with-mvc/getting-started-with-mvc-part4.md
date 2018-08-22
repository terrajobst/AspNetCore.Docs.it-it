---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
title: Creazione di un Database | Microsoft Docs
author: shanselman
description: Si tratta di un'esercitazione per principianti che introduce i concetti di base di ASP.NET MVC. Creare un'applicazione web semplice che legge e scrive da un database.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: 742df67f-484d-4ef3-af6b-8c791e556b43
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
msc.type: authoredcontent
ms.openlocfilehash: 596a491b4152da341a7779236dab17967a6de670
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823727"
---
<a name="creating-a-database"></a>Creazione di un Database
====================
da [Scott Hanselman](https://github.com/shanselman)

> Si tratta di un'esercitazione per principianti che introduce i concetti di base di ASP.NET MVC. Si creerà una semplice applicazione web che legge e scrive da un database. Visitare il [centro di formazione di ASP.NET MVC](../../../index.md) per trovare altri ASP.NET MVC, esercitazioni ed esempi.


In questa sezione si intende creare un nuovo database di SQL Express che verranno utilizzate per archiviare e recuperare i dati dei film. IDE di Visual Web Developer, selezionare Visualizza | Esplora server. Fare clic con il pulsante destro su connessioni di dati e fare clic su Aggiungi connessione...

![AddConnection](getting-started-with-mvc-part4/_static/image1.png)

Nella finestra di dialogo Scegli origine dati, selezionare Microsoft SQL Server e selezionare continua.

![](getting-started-with-mvc-part4/_static/image2.png)

Nella finestra di dialogo Aggiungi connessione, immettere ". \SQLEXPRESS" per il nome del Server, quindi immettere "Movies" come nome per il nuovo database.

[![Aggiungi finestra di dialogo di connessione](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)

Fare clic su OK e viene chiesto se si desidera creare tale database. Selezionare Sì.

[![Creazione di film?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)

A questo punto è disponibile un database vuoto in Esplora Server.

![Aggiungi nuova tabella](getting-started-with-mvc-part4/_static/image7.png)

Fare clic con il pulsante destro su tabelle e fare clic su Aggiungi tabella. Verrà visualizzata la finestra di progettazione di tabella. Aggiungere colonne per Id, Title, ReleaseDate, Genre e prezzo. Fare clic con il pulsante destro sulla colonna ID e fare clic su Imposta chiave primaria. Ecco quali my aree di progettazione dell'aspetto.

[![Editor di tabella di database](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)

Inoltre, selezionare la colonna Id e nella proprietà della colonna seguito modificare "Specifica Identity" su "Sì".

[![IsIdentity - proprietà colonna](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)

Quando è stato fatto, fare clic sull'icona Salva nella barra degli strumenti o scegliere File | Salva dal menu di scelta e assegnare un nome di tabella "**film**" (singolare). Abbiamo un database e una tabella.

[![Scegli nome](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)

Tornare a Esplora Server e fare clic con il pulsante destro tabella Movie, quindi selezionare "Mostra dati tabella". Immettere alcuni filmati in modo che il database dispone di alcuni dati.

[![Modifica delle tabelle di database](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)

## <a name="creating-a-model"></a>Creazione di un modello

A questo punto, tornare a Esplora soluzioni sul lato destro dell'IDE e del mouse sulla cartella Models e scegliere Aggiungi | Nuovo elemento.

[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)

Dobbiamo creare un modello di entità dal nuovo database. Si aggiungerà un set di classi per il progetto che rende più semplice per la query e modificare i dati all'interno di database. Selezionare il nodo di dati sul lato sinistro della finestra di dialogo e quindi selezionare il modello di elemento ADO.NET Entity Data Model. Denominarla Movies.edmx.

[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)

Fare clic sul pulsante "Aggiungi". Verrà quindi avviata "Entity Data Model Wizard".

Nella finestra di dialogo Nuovo che viene visualizzata, selezionare Generate dal Database. Poiché abbiamo appena creato un database, è necessario solo in merito a Entity Framework il nuovo database e la relativa tabella. Scegliere Avanti per salvare la connessione al database nella configurazione dell'applicazione web. A questo punto, controllare le tabelle e i film e fare clic su Fine.

[![Procedura guidata Entity Data Model](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)

A questo punto è possibile visualizzare la nuova tabella di film in Entity Framework Designer e accedervi dal codice.

[![Film - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)

Nell'area di progettazione è possibile visualizzare una classe "Movie". Questa classe esegue il mapping alla tabella "Movie" nel nostro database, e ogni proprietà all'interno di esso viene eseguito il mapping a una colonna con la tabella. Ogni istanza di una classe "Movie" corrisponderà a una riga all'interno della tabella "Movie".

Se non sono quelli di denominazione predefinito e le convenzioni usate da Entity Framework di mapping, è possibile utilizzare la finestra di progettazione di Entity Framework per modificare o personalizzarli. Per questa applicazione verrà usata le impostazioni predefinite e salvare solo il file come-è.

A questo punto, è possibile usare alcuni dati reali.

> [!div class="step-by-step"]
> [Precedente](getting-started-with-mvc-part3.md)
> [Successivo](getting-started-with-mvc-part5.md)
