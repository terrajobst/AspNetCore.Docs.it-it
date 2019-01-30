---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: 'Esercitazione: Personalizzare la visualizzazione per Entity Framework Database First con app ASP.NET MVC'
description: Questa esercitazione è incentrata su come modificare le visualizzazioni generate automaticamente per migliorare la presentazione.
author: Rick-Anderson
ms.author: riande
ms.date: 01/24/2019
ms.topic: tutorial
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: 89b8a0eb84b6e287c45bc141c68a2c76e63b0e41
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236497"
---
# <a name="tutorial-customize-view-for-ef-database-first-with-aspnet-mvc-app"></a><span data-ttu-id="13d16-103">Esercitazione: Personalizzare la visualizzazione per Entity Framework Database First con app ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="13d16-103">Tutorial: Customize view for EF Database First with ASP.NET MVC app</span></span>

<span data-ttu-id="13d16-104">Usa MVC, Entity Framework e lo Scaffolding di ASP.NET, è possibile creare un'applicazione web che fornisce un'interfaccia a un database esistente.</span><span class="sxs-lookup"><span data-stu-id="13d16-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="13d16-105">Questa serie di esercitazioni illustra come generare il codice che consente agli utenti di visualizzare, modificare, creare automaticamente ed eliminare dati che si trovano in una tabella di database.</span><span class="sxs-lookup"><span data-stu-id="13d16-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="13d16-106">Il codice generato corrispondente alle colonne nella tabella di database.</span><span class="sxs-lookup"><span data-stu-id="13d16-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="13d16-107">Questa esercitazione è incentrata su come modificare le visualizzazioni generate automaticamente per migliorare la presentazione.</span><span class="sxs-lookup"><span data-stu-id="13d16-107">This tutorial focuses on changing the automatically-generated views to enhance the presentation.</span></span>

<span data-ttu-id="13d16-108">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="13d16-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="13d16-109">Aggiungere corsi alla pagina di dettaglio per studenti</span><span class="sxs-lookup"><span data-stu-id="13d16-109">Add courses to the student detail page</span></span>
> * <span data-ttu-id="13d16-110">Verificare che i corsi vengono aggiunti alla pagina</span><span class="sxs-lookup"><span data-stu-id="13d16-110">Confirm that the courses are added to the page</span></span>

## <a name="prerequisites"></a><span data-ttu-id="13d16-111">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="13d16-111">Prerequisites</span></span>

* [<span data-ttu-id="13d16-112">Modificare il database</span><span class="sxs-lookup"><span data-stu-id="13d16-112">Change the database</span></span>](changing-the-database.md)

## <a name="add-courses-to-student-detail"></a><span data-ttu-id="13d16-113">Aggiungere corsi alla dettagli dello studente</span><span class="sxs-lookup"><span data-stu-id="13d16-113">Add courses to student detail</span></span>

<span data-ttu-id="13d16-114">Il codice generato fornisce un buon punto di partenza per l'applicazione, ma non necessariamente fornisce tutte le funzionalità necessarie nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="13d16-114">The generated code provides a good starting point for your application but it does not necessarily provide all of the functionality that you need in your application.</span></span> <span data-ttu-id="13d16-115">È possibile personalizzare il codice per soddisfare i requisiti specifici dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="13d16-115">You can customize the code to meet the particular requirements of your application.</span></span> <span data-ttu-id="13d16-116">Attualmente, l'applicazione non visualizza i corsi registrati dello studente selezionato.</span><span class="sxs-lookup"><span data-stu-id="13d16-116">Currently, your application does not display the enrolled courses for the selected student.</span></span> <span data-ttu-id="13d16-117">In questa sezione si aggiungerà i corsi registrati per ogni studente per il **dettagli** vista per gli studenti.</span><span class="sxs-lookup"><span data-stu-id="13d16-117">In this section, you will add the enrolled courses for each student to the **Details** view for the student.</span></span>

<span data-ttu-id="13d16-118">Aprire **viste** > **studenti** > *Details. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="13d16-118">Open **Views** > **Students** > *Details.cshtml*.</span></span> <span data-ttu-id="13d16-119">Sotto l'ultima &lt;/dl&gt; tag, ma prima della chiusura &lt;/div&gt; tag, aggiungere il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="13d16-119">Below the last &lt;/dl&gt; tag, but before the closing &lt;/div&gt; tag, add the following code.</span></span>

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

<span data-ttu-id="13d16-120">Questo codice crea una tabella che visualizza una riga per ogni record della tabella Enrollment dello studente selezionato.</span><span class="sxs-lookup"><span data-stu-id="13d16-120">This code creates a table that displays a row for each record in the Enrollment table for the selected student.</span></span> <span data-ttu-id="13d16-121">Il **Display** metodo esegue il rendering HTML per l'oggetto (modelItem) che rappresenta l'espressione.</span><span class="sxs-lookup"><span data-stu-id="13d16-121">The **Display** method renders HTML for the object (modelItem) that represents the expression.</span></span> <span data-ttu-id="13d16-122">Si usa il metodo di visualizzazione (anziché semplicemente incorporare il valore della proprietà nel codice) per assicurarsi che il valore viene formattato correttamente in base del tipo e il modello per tale tipo.</span><span class="sxs-lookup"><span data-stu-id="13d16-122">You use the Display method (rather than simply embedding the property value in the code) to make sure the value is formatted correctly based on its type and the template for that type.</span></span> <span data-ttu-id="13d16-123">In questo esempio, ogni espressione restituisce una singola proprietà del record corrente nel ciclo e i valori sono i tipi primitivi che vengono visualizzati come testo.</span><span class="sxs-lookup"><span data-stu-id="13d16-123">In this example, each expression returns a single property from the current record in the loop, and the values are primitive types which are rendered as text.</span></span>

## <a name="confirm-courses-are-added"></a><span data-ttu-id="13d16-124">Confermare i corsi vengono aggiunti</span><span class="sxs-lookup"><span data-stu-id="13d16-124">Confirm courses are added</span></span>

<span data-ttu-id="13d16-125">Eseguire la soluzione.</span><span class="sxs-lookup"><span data-stu-id="13d16-125">Run the solution.</span></span> <span data-ttu-id="13d16-126">Fare clic su **elenco degli studenti** e selezionare **dettagli** per uno degli studenti.</span><span class="sxs-lookup"><span data-stu-id="13d16-126">Click **List of students** and select **Details** for one of the students.</span></span> <span data-ttu-id="13d16-127">Si noterà che i corsi registrati sono stati inclusi nella vista.</span><span class="sxs-lookup"><span data-stu-id="13d16-127">You will see the enrolled courses have been included in the view.</span></span>

![studente con la registrazione](customizing-a-view/_static/image1.png)

## <a name="next-steps"></a><span data-ttu-id="13d16-129">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="13d16-129">Next steps</span></span>
<span data-ttu-id="13d16-130">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="13d16-130">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="13d16-131">Corsi aggiunti alla pagina di dettaglio per studenti</span><span class="sxs-lookup"><span data-stu-id="13d16-131">Added courses to the student detail page</span></span>
> * <span data-ttu-id="13d16-132">Conferma che i corsi vengono aggiunti alla pagina</span><span class="sxs-lookup"><span data-stu-id="13d16-132">Confirmed that the courses are added to the page</span></span>

<span data-ttu-id="13d16-133">Passare all'esercitazione successiva per informazioni su come aggiungere le annotazioni dei dati per specificare i requisiti della convalida e formattazione di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="13d16-133">Advance to the next tutorial to learn how to add data annotations to specify validation requirements and display formatting.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="13d16-134">Migliorare la convalida dei dati</span><span class="sxs-lookup"><span data-stu-id="13d16-134">Enhance data validation</span></span>](enhancing-data-validation.md)
