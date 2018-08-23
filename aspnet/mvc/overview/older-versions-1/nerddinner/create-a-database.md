---
uid: mvc/overview/older-versions-1/nerddinner/create-a-database
title: Creare un Database | Microsoft Docs
author: microsoft
description: Passaggio 2 mostra i passaggi per creare il database che contengono tutte le la cena e conferma la partecipazione dei dati per la nostra applicazione NerdDinner.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 983f3ffa-08b8-4868-b8c9-aa34593fc683
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-database
msc.type: authoredcontent
ms.openlocfilehash: b6ab0740f251889f0fa0561809cac2bbe79bcb3a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834455"
---
<a name="create-a-database"></a><span data-ttu-id="a9bc8-103">Creare un Database</span><span class="sxs-lookup"><span data-stu-id="a9bc8-103">Create a Database</span></span>
====================
<span data-ttu-id="a9bc8-104">da [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="a9bc8-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="a9bc8-105">Scaricare PDF</span><span class="sxs-lookup"><span data-stu-id="a9bc8-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="a9bc8-106">Si tratta di passaggio 2 di una liberazione [esercitazione sull'applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che si interromperanno-dettaglio come compilare una piccola, ma completa, applicazione web con ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="a9bc8-106">This is step 2 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="a9bc8-107">Passaggio 2 mostra i passaggi per creare il database che contengono tutte le la cena e conferma la partecipazione dei dati per la nostra applicazione NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="a9bc8-107">Step 2 shows the steps to create the database holding all of the dinner and RSVP data for our NerdDinner application.</span></span>
> 
> <span data-ttu-id="a9bc8-108">Se si usa ASP.NET MVC 3, è consigliabile seguire le [Guida introduttiva con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) oppure [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) esercitazioni.</span><span class="sxs-lookup"><span data-stu-id="a9bc8-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-2-creating-the-database"></a><span data-ttu-id="a9bc8-109">NerdDinner passaggio 2: Creazione del Database</span><span class="sxs-lookup"><span data-stu-id="a9bc8-109">NerdDinner Step 2: Creating the Database</span></span>

<span data-ttu-id="a9bc8-110">Verrà usato un database per archiviare tutti i dati Dinner e RSVP per la nostra applicazione NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="a9bc8-110">We'll be using a database to store all of the Dinner and RSVP data for our NerdDinner application.</span></span>

<span data-ttu-id="a9bc8-111">La procedura seguente illustra la creazione del database usando l'edizione gratuita di SQL Server Express (che è possibile installare facilmente tramite V2 del [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)).</span><span class="sxs-lookup"><span data-stu-id="a9bc8-111">The steps below show creating the database using the free SQL Server Express edition (which you can easily install using V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)).</span></span> <span data-ttu-id="a9bc8-112">Tutto il codice verrà scritta funziona con SQL Server Express e la versione completa di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="a9bc8-112">All of the code we'll write works with both SQL Server Express and the full SQL Server.</span></span>

### <a name="creating-a-new-sql-server-express-database"></a><span data-ttu-id="a9bc8-113">Crea un nuovo database di SQL Server Express</span><span class="sxs-lookup"><span data-stu-id="a9bc8-113">Creating a new SQL Server Express database</span></span>

<span data-ttu-id="a9bc8-114">Si sarà iniziare facendo clic con il progetto web e quindi selezionare il **Add -&gt;nuovo elemento** comando di menu:</span><span class="sxs-lookup"><span data-stu-id="a9bc8-114">We'll begin by right-clicking on our web project, and then select the **Add-&gt;New Item** menu command:</span></span>

![](create-a-database/_static/image1.png)

<span data-ttu-id="a9bc8-115">Verrà visualizzata una finestra di dialogo "Aggiungi nuovo elemento" Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a9bc8-115">This will bring up Visual Studio's "Add New Item" dialog.</span></span> <span data-ttu-id="a9bc8-116">Verrà filtrare in base alla categoria "Dati" e selezionare il modello di elemento "Database di SQL Server":</span><span class="sxs-lookup"><span data-stu-id="a9bc8-116">We'll filter by the "Data" category and select the "SQL Server Database" item template:</span></span>

![](create-a-database/_static/image2.png)

<span data-ttu-id="a9bc8-117">Denominiamo il database di SQL Server Express che vogliamo creare "NerdDinner.mdf" e fare clic su ok.</span><span class="sxs-lookup"><span data-stu-id="a9bc8-117">We'll name the SQL Server Express database we want to create "NerdDinner.mdf" and hit ok.</span></span> <span data-ttu-id="a9bc8-118">Visual Studio richiederà quindi noi se si vuole aggiungere il file al nostro \App\_directory dei dati (che è già una directory di installazione con sia in lettura e scrittura ACL di sicurezza):</span><span class="sxs-lookup"><span data-stu-id="a9bc8-118">Visual Studio will then ask us if we want to add this file to our \App\_Data directory (which is a directory already setup with both read and write security ACLs):</span></span>

![](create-a-database/_static/image3.png)

<span data-ttu-id="a9bc8-119">Facciamo clic su "Sì" e il nuovo database verrà creato e aggiunto al nostro Esplora soluzioni:</span><span class="sxs-lookup"><span data-stu-id="a9bc8-119">We'll click "Yes" and our new database will be created and added to our Solution Explorer:</span></span>

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a><span data-ttu-id="a9bc8-120">Creazione di tabelle all'interno di Database</span><span class="sxs-lookup"><span data-stu-id="a9bc8-120">Creating Tables within our Database</span></span>

<span data-ttu-id="a9bc8-121">È ora disponibile un nuovo database vuoto.</span><span class="sxs-lookup"><span data-stu-id="a9bc8-121">We now have a new empty database.</span></span> <span data-ttu-id="a9bc8-122">È possibile aggiungervi alcune tabelle.</span><span class="sxs-lookup"><span data-stu-id="a9bc8-122">Let's add some tables to it.</span></span>

<span data-ttu-id="a9bc8-123">A tale scopo è visualizzata finestra scheda "Esplora Server" all'interno di Visual Studio, che consente di gestire server e database.</span><span class="sxs-lookup"><span data-stu-id="a9bc8-123">To do this we'll navigate to the "Server Explorer" tab window within Visual Studio, which enables us to manage databases and servers.</span></span> <span data-ttu-id="a9bc8-124">Database di SQL Server Express archiviati nel \App\_cartella dati dell'applicazione verrà visualizzato automaticamente all'interno di Esplora Server.</span><span class="sxs-lookup"><span data-stu-id="a9bc8-124">SQL Server Express databases stored in the \App\_Data folder of our application will automatically show up within the Server Explorer.</span></span> <span data-ttu-id="a9bc8-125">È facoltativamente possibile usare l'icona "Connettersi al Database" nella parte superiore della finestra "Esplora Server" per aggiungere altri database di SQL Server (locali e remoti) oltre all'elenco:</span><span class="sxs-lookup"><span data-stu-id="a9bc8-125">We can optionally use the "Connect to Database" icon on the top of the "Server Explorer" window to add additional SQL Server databases (both local and remote) to the list as well:</span></span>

![](create-a-database/_static/image5.png)

<span data-ttu-id="a9bc8-126">Due tabelle verrà aggiunto al database NerdDinner: uno per archiviare i nostri Dinners e l'altro per tenere traccia delle accettazioni RSVP ad essi.</span><span class="sxs-lookup"><span data-stu-id="a9bc8-126">We will add two tables to our NerdDinner database – one to store our Dinners, and the other to track RSVP acceptances to them.</span></span> <span data-ttu-id="a9bc8-127">È possibile creare nuove tabelle facendo clic sulla cartella "Tabelle" all'interno di database e scegliendo il comando di menu "Aggiungi nuova tabella":</span><span class="sxs-lookup"><span data-stu-id="a9bc8-127">We can create new tables by right-clicking on the "Tables" folder within our database and choosing the "Add New Table" menu command:</span></span>

![](create-a-database/_static/image6.png)

<span data-ttu-id="a9bc8-128">Verrà aperta una finestra di progettazione di una tabella che consente di configurare lo schema della tabella.</span><span class="sxs-lookup"><span data-stu-id="a9bc8-128">This will open up a table designer that allows us to configure the schema of our table.</span></span> <span data-ttu-id="a9bc8-129">Per la tabella "Dinners" si aggiungerà 10 colonne di dati:</span><span class="sxs-lookup"><span data-stu-id="a9bc8-129">For our "Dinners" table we will add 10 columns of data:</span></span>

![](create-a-database/_static/image7.png)

<span data-ttu-id="a9bc8-130">Si vuole la colonna "DinnerID" sia una chiave primaria univoca per la tabella.</span><span class="sxs-lookup"><span data-stu-id="a9bc8-130">We want the "DinnerID" column to be a unique primary key for the table.</span></span> <span data-ttu-id="a9bc8-131">È possibile configurare questa impostazione facendo clic sulla colonna "DinnerID" e scegliendo la voce di menu "Imposta chiave primaria":</span><span class="sxs-lookup"><span data-stu-id="a9bc8-131">We can configure this by right-clicking on the "DinnerID" column and choosing the "Set Primary Key" menu item:</span></span>

![](create-a-database/_static/image8.png)

<span data-ttu-id="a9bc8-132">Oltre a rendere DinnerID una chiave primaria, è necessario anche lo configura come una colonna di "identity" il cui valore viene incrementato automaticamente man mano che nuove righe di dati vengono aggiunti alla tabella (vale a dire la prima riga Dinner inserita avrà un DinnerID pari a 1, il secondo inserito riga sarà necessario un DinnerID 2, e così via).</span><span class="sxs-lookup"><span data-stu-id="a9bc8-132">In addition to making DinnerID a primary key, we also want configure it as an "identity" column whose value is automatically incremented as new rows of data are added to the table (meaning the first inserted Dinner row will have a DinnerID of 1, the second inserted row will have a DinnerID of 2, etc).</span></span>

<span data-ttu-id="a9bc8-133">È possibile farlo, selezionare la colonna "DinnerID" e quindi usare l'editor "Proprietà delle colonne" per impostare la proprietà "(Is Identity)" nella colonna su "Sì".</span><span class="sxs-lookup"><span data-stu-id="a9bc8-133">We can do this by selecting the "DinnerID" column and then use the "Column Properties" editor to set the "(Is Identity)" property on the column to "Yes".</span></span> <span data-ttu-id="a9bc8-134">Si userà le impostazioni predefinite standard di identità (partono da 1 e incrementare di 1 in ogni nuova riga Dinner):</span><span class="sxs-lookup"><span data-stu-id="a9bc8-134">We will use the standard identity defaults (start at 1 and increment 1 on each new Dinner row):</span></span>

![](create-a-database/_static/image9.png)

<span data-ttu-id="a9bc8-135">La tabella verranno quindi salvate, digitando Ctrl + S oppure usando il **File -&gt;salvare** comando di menu.</span><span class="sxs-lookup"><span data-stu-id="a9bc8-135">We'll then save our table by typing Ctrl-S or by using the **File-&gt;Save** menu command.</span></span> <span data-ttu-id="a9bc8-136">Verrà richiesto di assegnare un nome di tabella.</span><span class="sxs-lookup"><span data-stu-id="a9bc8-136">This will prompt us to name the table.</span></span> <span data-ttu-id="a9bc8-137">Denominiamo "Dinners":</span><span class="sxs-lookup"><span data-stu-id="a9bc8-137">We'll name it "Dinners":</span></span>

![](create-a-database/_static/image10.png)

<span data-ttu-id="a9bc8-138">La nuova tabella Dinners verrà quindi visualizzato entro il database in Esplora server.</span><span class="sxs-lookup"><span data-stu-id="a9bc8-138">Our new Dinners table will then show up within our database in the server explorer.</span></span>

<span data-ttu-id="a9bc8-139">È quindi possibile ripetere i passaggi precedenti e creare una tabella "RSVP".</span><span class="sxs-lookup"><span data-stu-id="a9bc8-139">We'll then repeat the above steps and create a "RSVP" table.</span></span> <span data-ttu-id="a9bc8-140">La tabella con include 3 colonne.</span><span class="sxs-lookup"><span data-stu-id="a9bc8-140">This table with have 3 columns.</span></span> <span data-ttu-id="a9bc8-141">Verranno della colonna RsvpID come chiave primaria di installazione e rendono inoltre una colonna identity:</span><span class="sxs-lookup"><span data-stu-id="a9bc8-141">We will setup the RsvpID column as the primary key, and also make it an identity column:</span></span>

![](create-a-database/_static/image11.png)

<span data-ttu-id="a9bc8-142">È possibile salvarlo e assegnare il nome "RSVP".</span><span class="sxs-lookup"><span data-stu-id="a9bc8-142">We'll save it and give it the name "RSVP".</span></span>

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a><span data-ttu-id="a9bc8-143">Impostazione di una relazione di chiave esterna tra le tabelle</span><span class="sxs-lookup"><span data-stu-id="a9bc8-143">Setting up a Foreign Key Relationship between Tables</span></span>

<span data-ttu-id="a9bc8-144">Ora abbiamo due tabelle all'interno di database.</span><span class="sxs-lookup"><span data-stu-id="a9bc8-144">We now have two tables within our database.</span></span> <span data-ttu-id="a9bc8-145">L'ultimo passaggio della progettazione dello schema sarà per impostare una relazione "uno-a-molti" tra queste due tabelle, in modo che è possibile associare ogni riga Dinner con zero o più righe RSVP che si applicano a esso.</span><span class="sxs-lookup"><span data-stu-id="a9bc8-145">Our last schema design step will be to setup a "one-to-many" relationship between these two tables – so that we can associate each Dinner row with zero or more RSVP rows that apply to it.</span></span> <span data-ttu-id="a9bc8-146">Faremo configurando colonna "DinnerID" della tabella RSVP per avere una relazione di chiave esterna nella colonna "DinnerID" nella tabella "Dinners".</span><span class="sxs-lookup"><span data-stu-id="a9bc8-146">We will do this by configuring the RSVP table's "DinnerID" column to have a foreign-key relationship to the "DinnerID" column in the "Dinners" table.</span></span>

<span data-ttu-id="a9bc8-147">A tale scopo che verrà aperto il contenuto della tabella RSVP all'interno di progettazione tabelle facendovi doppio clic in Esplora server.</span><span class="sxs-lookup"><span data-stu-id="a9bc8-147">To do this we'll open up the RSVP table within the table designer by double-clicking it in the server explorer.</span></span> <span data-ttu-id="a9bc8-148">La colonna "DinnerID" all'interno di esso, quindi selezioniamo destro del mouse e scegliere il comando di menu di scelta rapida "Relationshps in corso":</span><span class="sxs-lookup"><span data-stu-id="a9bc8-148">We'll then select the "DinnerID" column within it, right-click, and choose the "Relationshps…" context menu command:</span></span>

![](create-a-database/_static/image12.png)

<span data-ttu-id="a9bc8-149">Verrà visualizzata una finestra di dialogo che è possibile usare per le relazioni di programma di installazione tra le tabelle:</span><span class="sxs-lookup"><span data-stu-id="a9bc8-149">This will bring up a dialog that we can use to setup relationships between tables:</span></span>

![](create-a-database/_static/image13.png)

<span data-ttu-id="a9bc8-150">Si sarà fare clic sul pulsante "Aggiungi" per aggiungere una nuova relazione nella finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="a9bc8-150">We'll click the "Add" button to add a new relationship to the dialog.</span></span> <span data-ttu-id="a9bc8-151">Dopo aver aggiunto una relazione, si sarà espandere il nodo di visualizzazione ad albero "Specifica di tabelle e colonne" all'interno della griglia delle proprietà a destra della finestra di dialogo e quindi fare clic sul pulsante "..." a destra:</span><span class="sxs-lookup"><span data-stu-id="a9bc8-151">Once a relationship has been added, we'll expand the "Tables and Column Specification" tree-view node within the property grid to the right of the dialog, and then click the "…" button to the right of it:</span></span>

![](create-a-database/_static/image14.png)

<span data-ttu-id="a9bc8-152">Fare clic sul pulsante "...", verrà visualizzata un'altra finestra di dialogo che consente di specificare quali tabelle e colonne sono coinvolte nella relazione, nonché consentono di assegnare un nome della relazione.</span><span class="sxs-lookup"><span data-stu-id="a9bc8-152">Clicking the "…" button will bring up another dialog that allows us to specify which tables and columns are involved in the relationship, as well as allow us to name the relationship.</span></span>

<span data-ttu-id="a9bc8-153">Si verrà modificare la tabella di chiave primaria affinché sia "Dinners" e selezionare la colonna "DinnerID" all'interno della tabella Dinners come chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="a9bc8-153">We will change the Primary Key Table to be "Dinners", and select the "DinnerID" column within the Dinners table as the primary key.</span></span> <span data-ttu-id="a9bc8-154">La tabella RSVP sarà la tabella di chiave esterna e il RSVP. Colonna DinnerID sarà associato come la chiave esterna:</span><span class="sxs-lookup"><span data-stu-id="a9bc8-154">Our RSVP table will be the foreign-key table, and the RSVP.DinnerID column will be associated as the foreign-key:</span></span>

![](create-a-database/_static/image15.png)

<span data-ttu-id="a9bc8-155">Ora ogni riga della tabella RSVP verrà associato a una riga nella tabella cena.</span><span class="sxs-lookup"><span data-stu-id="a9bc8-155">Now each row in the RSVP table will be associated with a row in the Dinner table.</span></span> <span data-ttu-id="a9bc8-156">SQL Server verrà mantenere l'integrità referenziale per noi – e impediscono di aggiunta di una nuova riga RSVP se non fa riferimento a una riga Dinner valida.</span><span class="sxs-lookup"><span data-stu-id="a9bc8-156">SQL Server will maintain referential integrity for us – and prevent us from adding a new RSVP row if it does not point to a valid Dinner row.</span></span> <span data-ttu-id="a9bc8-157">Anche impedirà noi l'eliminazione di una riga Dinner se non esistono ancora RSVP righe che fanno riferimento a esso.</span><span class="sxs-lookup"><span data-stu-id="a9bc8-157">It will also prevent us from deleting a Dinner row if there are still RSVP rows referring to it.</span></span>

### <a name="adding-data-to-our-tables"></a><span data-ttu-id="a9bc8-158">Aggiunta di dati per le tabelle</span><span class="sxs-lookup"><span data-stu-id="a9bc8-158">Adding Data to our Tables</span></span>

<span data-ttu-id="a9bc8-159">È possibile completare aggiungendo alcuni dati di esempio alla nostra tabella Dinners.</span><span class="sxs-lookup"><span data-stu-id="a9bc8-159">Let's finish by adding some sample data to our Dinners table.</span></span> <span data-ttu-id="a9bc8-160">È possibile aggiungere dati a una tabella facendo clic su di esso in Esplora Server e scegliendo il comando "Mostra dati tabella":</span><span class="sxs-lookup"><span data-stu-id="a9bc8-160">We can add data to a table by right-clicking on it within the Server Explorer and choosing the "Show Table Data" command:</span></span>

![](create-a-database/_static/image16.png)

<span data-ttu-id="a9bc8-161">Si aggiungerà poche righe di dati Dinner che è possibile usare in un secondo momento come iniziare a implementare l'applicazione:</span><span class="sxs-lookup"><span data-stu-id="a9bc8-161">We'll add a few rows of Dinner data that we can use later as we start implementing the application:</span></span>

![](create-a-database/_static/image17.png)

### <a name="next-step"></a><span data-ttu-id="a9bc8-162">Passo successivo</span><span class="sxs-lookup"><span data-stu-id="a9bc8-162">Next Step</span></span>

<span data-ttu-id="a9bc8-163">Abbiamo completato la creazione di database.</span><span class="sxs-lookup"><span data-stu-id="a9bc8-163">We've finished creating our database.</span></span> <span data-ttu-id="a9bc8-164">Creare ora le classi di modello che è possibile usare per eseguire una query e aggiornarlo.</span><span class="sxs-lookup"><span data-stu-id="a9bc8-164">Let's now create model classes that we can use to query and update it.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a9bc8-165">[Precedente](create-a-new-aspnet-mvc-project.md)
> [Successivo](build-a-model-with-business-rule-validations.md)</span><span class="sxs-lookup"><span data-stu-id="a9bc8-165">[Previous](create-a-new-aspnet-mvc-project.md)
[Next](build-a-model-with-business-rule-validations.md)</span></span>
