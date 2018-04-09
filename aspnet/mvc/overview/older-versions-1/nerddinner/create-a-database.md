---
uid: mvc/overview/older-versions-1/nerddinner/create-a-database
title: Creare un Database | Documenti Microsoft
author: microsoft
description: Passaggio 2 mostra i passaggi per creare il database che contiene tutti i dinner e RSVP dati per l'applicazione NerdDinner.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 983f3ffa-08b8-4868-b8c9-aa34593fc683
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-database
msc.type: authoredcontent
ms.openlocfilehash: ba28d671bf13ec54b83b876462e2c23f90310037
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="create-a-database"></a>Creare un Database
====================
by [Microsoft](https://github.com/microsoft)

[Scarica il PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Fase 2 del processo di una liberazione [esercitazione applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che si interromperanno-through come compilare un piccolo, ma completa, un'applicazione web con ASP.NET MVC 1.
> 
> Passaggio 2 mostra i passaggi per creare il database che contiene tutti i dinner e RSVP dati per l'applicazione NerdDinner.
> 
> Se si utilizza ASP.NET MVC 3, è consigliabile seguire il [recupero avviato con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [negozio MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) esercitazioni.


## <a name="nerddinner-step-2-creating-the-database"></a>NerdDinner passaggio 2: Creazione del Database

Verrà usato un database per archiviare tutti i dati Dinner e RSVP per l'applicazione NerdDinner.

La procedura seguente illustra la creazione del database utilizzando l'edizione gratuita di SQL Server Express (che è possibile installare facilmente V2 di utilizzando il [installazione guidata piattaforma Web di Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)). Tutto il codice in cui verranno scritti funziona con SQL Server Express e SQL Server completo.

### <a name="creating-a-new-sql-server-express-database"></a>Creare un nuovo database di SQL Server Express

Verrà innanzitutto facendo clic sul progetto web e quindi selezionare il **Add -&gt;nuovo elemento** comando di menu:

![](create-a-database/_static/image1.png)

Verrà visualizzata finestra di dialogo "Aggiungi nuovo elemento" Visual Studio. Si sarà filtrare in base alla categoria "Dati" e selezionare il modello di elemento "Database di SQL Server":

![](create-a-database/_static/image2.png)

È possibile assegnare un nome il database SQL Server Express, di voler creare "NerdDinner.mdf" e fare clic su ok. Visual Studio verrà quindi chiedere se si vuole aggiungere il file per il nostro \App\_directory dei dati (che è già una directory di installazione con di lettura e scrittura sicurezza ACL):

![](create-a-database/_static/image3.png)

È possibile fare clic su "Yes" e il nuovo database verrà creato e aggiunto a questo Esplora soluzioni:

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a>Creazione di tabelle all'interno del Database

È ora disponibile un nuovo database vuoto. Aggiungere alcune tabelle a esso.

A tale scopo è passerà alla finestra della scheda "Esplora Server" all'interno di Visual Studio, che consente di gestire i database e server. Il database di SQL Server Express archiviati nel \App\_cartella dati dell'applicazione verrà visualizzate automaticamente all'interno di Esplora Server. È facoltativamente possibile utilizzare l'icona di "Connessione al Database" nella parte superiore della finestra "Esplora Server" per aggiungere ulteriori database di SQL Server (locali e remoti) oltre all'elenco:

![](create-a-database/_static/image5.png)

Il database NerdDinner: uno per archiviare il nostro Dinners e l'altro per tenere traccia delle accettazioni RSVP ad essi verranno aggiunti due tabelle. È possibile creare nuove tabelle facendo clic sulla cartella "Tables" all'interno del database e scegliendo il comando di menu "Aggiungi nuova tabella":

![](create-a-database/_static/image6.png)

Verrà aperta una finestra di progettazione di una tabella che consente di configurare lo schema della tabella. Per la tabella "Dinners" verranno aggiunti 10 colonne di dati:

![](create-a-database/_static/image7.png)

Si vuole la colonna "DinnerID" da una chiave primaria univoca per la tabella. È possibile configurare questa impostazione facendo clic sulla colonna "DinnerID" e scegliendo la voce di menu "Imposta chiave primaria":

![](create-a-database/_static/image8.png)

Oltre a rendere DinnerID una chiave primaria, è inoltre auspicabile che configura come una colonna "identity" il cui valore viene incrementato automaticamente quando vengono aggiunte nuove righe di dati alla tabella (vale a dire la prima riga Dinner inserita avrà un DinnerID pari a 1, il secondo inserito riga sarà necessario un DinnerID 2, e così via).

È possibile eseguire questa operazione selezionando la colonna "DinnerID" e quindi utilizzare l'editor di "Proprietà della colonna" per impostare la proprietà "(Is Identity)" nella colonna su "Sì". Si utilizzerà le impostazioni predefinite standard di identità (iniziano da 1 e incrementare di 1 in ogni nuova riga Dinner):

![](create-a-database/_static/image9.png)

Si verrà quindi salvare la tabella digitando Ctrl + S oppure utilizzando il **File -&gt;salvare** comando di menu. Questo richiederà di denominare la tabella. Sarà denominato "Dinners":

![](create-a-database/_static/image10.png)

La nuova tabella Dinners verrà quindi visualizzati all'interno di questo database in Esplora server.

È quindi possibile ripetere i passaggi precedenti e creare una tabella di "RSVP". Questa tabella con con 3 colonne. Si verrà RsvpID colonna come chiave primaria del programma di installazione e prendere inoltre una colonna identity:

![](create-a-database/_static/image11.png)

Viene salvarlo e assegnargli il nome "RSVP".

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a>Impostazione di una relazione di chiave esterna tra le tabelle

Sono ora presenti due tabelle all'interno del database. L'ultimo passaggio Progettazione schema sarà per impostare una relazione "uno-a-molti" tra le due tabelle: in modo che è possibile associare ogni riga Dinner con zero o più righe RSVP che si applicano a esso. Questo verranno eseguite tramite la configurazione di colonna della tabella RSVP "DinnerID" con una relazione di chiave esterna per la colonna "DinnerID" nella tabella "Dinners".

A tale scopo che il contenuto della tabella RSVP all'interno di progettazione tabelle verrà aperto facendo doppio clic in Esplora server. Verrà quindi selezionare la colonna "DinnerID" all'interno di esso, mouse e scegliere il "Relationshps..." comando di menu di scelta:

![](create-a-database/_static/image12.png)

Verrà visualizzata una finestra di dialogo che è possibile utilizzare per le relazioni di programma di installazione tra le tabelle:

![](create-a-database/_static/image13.png)

Si sarà fare clic sul pulsante "Aggiungi" per aggiungere una nuova relazione tra la finestra di dialogo. Dopo l'aggiunta di una relazione, si sarà espandere il nodo di visualizzazione ad albero "Specifica tabelle e colonne" all'interno della griglia di proprietà a destra della finestra di dialogo e quindi fare clic sul pulsante "…" a destra di esso:

![](create-a-database/_static/image14.png)

Fare clic sul pulsante "…" verrà visualizzata un'altra finestra di dialogo che consente di specificare quali tabelle e colonne sono coinvolte nella relazione, nonché consentono di assegnare un nome della relazione.

Si verrà modificare la tabella di chiave primaria per "Dinners" e selezionare la colonna "DinnerID" all'interno della tabella Dinners come chiave primaria. La tabella RSVP sarà la tabella di chiave esterna e il RSVP. Colonna DinnerID verrà associato come la chiave esterna:

![](create-a-database/_static/image15.png)

Ora ogni riga della tabella RSVP verrà associato a una riga nella tabella Dinner. SQL Server verrà mantenere l'integrità referenziale per noi e ci impedire l'aggiunta di una nuova riga RSVP se non punta a una riga Dinner valida. Anche impedirà ci eliminazione di una riga Dinner se non esistono ancora RSVP righe che fanno riferimento a esso.

### <a name="adding-data-to-our-tables"></a>Aggiunta di dati per le tabelle

Completare aggiungendo alcuni dati di esempio per la tabella Dinners. È possibile aggiungere dati a una tabella facendo clic su di esso in Esplora Server e scegliendo il comando "Mostra dati tabella":

![](create-a-database/_static/image16.png)

Si aggiungeranno alcune righe di dati Dinner che è possibile utilizzare in un secondo momento come iniziare a implementare l'applicazione:

![](create-a-database/_static/image17.png)

### <a name="next-step"></a>Passo successivo

È stata completata la creazione del database. Creare ora le classi di modello che è possibile utilizzare per eseguire una query e l'aggiornamento.

> [!div class="step-by-step"]
> [Precedente](create-a-new-aspnet-mvc-project.md)
> [Successivo](build-a-model-with-business-rule-validations.md)
