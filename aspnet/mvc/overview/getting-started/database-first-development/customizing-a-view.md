---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: 'Database di Entity Framework prima di tutto con ASP.NET MVC: personalizzare una vista | Microsoft Docs'
author: tfitzmac
description: Usa MVC, Entity Framework e lo Scaffolding di ASP.NET, è possibile creare un'applicazione web che fornisce un'interfaccia a un database esistente. Questa esercitazione seri...
ms.author: aspnetcontent
ms.date: 10/01/2014
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: 7359b8daddc74e375675d73126d7d76b288e853d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817188"
---
<a name="ef-database-first-with-aspnet-mvc-customizing-a-view"></a><span data-ttu-id="6682a-104">Database di Entity Framework prima di tutto con ASP.NET MVC: personalizzare una vista</span><span class="sxs-lookup"><span data-stu-id="6682a-104">EF Database First with ASP.NET MVC: Customizing a View</span></span>
====================
<span data-ttu-id="6682a-105">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="6682a-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="6682a-106">Usa MVC, Entity Framework e lo Scaffolding di ASP.NET, è possibile creare un'applicazione web che fornisce un'interfaccia a un database esistente.</span><span class="sxs-lookup"><span data-stu-id="6682a-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="6682a-107">Questa serie di esercitazioni illustra come generare il codice che consente agli utenti di visualizzare, modificare, creare automaticamente ed eliminare dati che si trovano in una tabella di database.</span><span class="sxs-lookup"><span data-stu-id="6682a-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="6682a-108">Il codice generato corrispondente alle colonne nella tabella di database.</span><span class="sxs-lookup"><span data-stu-id="6682a-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="6682a-109">Questa parte della serie è incentrato su come modificare le visualizzazioni generate automaticamente per migliorare la presentazione.</span><span class="sxs-lookup"><span data-stu-id="6682a-109">This part of the series focuses on changing the automatically-generated views to enhance the presentation.</span></span>


## <a name="add-enrolled-courses-to-student-details"></a><span data-ttu-id="6682a-110">Aggiungere corsi registrati i dettagli per studenti</span><span class="sxs-lookup"><span data-stu-id="6682a-110">Add enrolled courses to student details</span></span>

<span data-ttu-id="6682a-111">Il codice generato fornisce un buon punto di partenza per l'applicazione, ma non necessariamente fornisce tutte le funzionalità necessarie nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="6682a-111">The generated code provides a good starting point for your application but it does not necessarily provide all of the functionality that you need in your application.</span></span> <span data-ttu-id="6682a-112">È possibile personalizzare il codice per soddisfare i requisiti specifici dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="6682a-112">You can customize the code to meet the particular requirements of your application.</span></span> <span data-ttu-id="6682a-113">Attualmente, l'applicazione non visualizza i corsi registrati dello studente selezionato.</span><span class="sxs-lookup"><span data-stu-id="6682a-113">Currently, your application does not display the enrolled courses for the selected student.</span></span> <span data-ttu-id="6682a-114">In questa sezione si aggiungerà i corsi registrati per ogni studente per il **dettagli** vista per gli studenti.</span><span class="sxs-lookup"><span data-stu-id="6682a-114">In this section, you will add the enrolled courses for each student to the **Details** view for the student.</span></span>

<span data-ttu-id="6682a-115">Aprire **Students/Details.cshtml**e sotto l'ultimo &lt;/dl&gt; scheda, ma prima della chiusura &lt;/div&gt; tag, aggiungere il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="6682a-115">Open **Students/Details.cshtml**, and below the last &lt;/dl&gt; tab, but before the closing &lt;/div&gt; tag, add the following code.</span></span>

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

<span data-ttu-id="6682a-116">Questo codice crea una tabella che visualizza una riga per ogni record della tabella Enrollment dello studente selezionato.</span><span class="sxs-lookup"><span data-stu-id="6682a-116">This code creates a table that displays a row for each record in the Enrollment table for the selected student.</span></span> <span data-ttu-id="6682a-117">Il **Display** metodo esegue il rendering HTML per l'oggetto (modelItem) che rappresenta l'espressione.</span><span class="sxs-lookup"><span data-stu-id="6682a-117">The **Display** method renders HTML for the object (modelItem) that represents the expression.</span></span> <span data-ttu-id="6682a-118">Si usa il metodo di visualizzazione (anziché semplicemente incorporare il valore della proprietà nel codice) per assicurarsi che il valore viene formattato correttamente in base del tipo e il modello per tale tipo.</span><span class="sxs-lookup"><span data-stu-id="6682a-118">You use the Display method (rather than simply embedding the property value in the code) to make sure the value is formatted correctly based on its type and the template for that type.</span></span> <span data-ttu-id="6682a-119">In questo esempio, ogni espressione restituisce una singola proprietà del record corrente nel ciclo e i valori sono i tipi primitivi che vengono visualizzati come testo.</span><span class="sxs-lookup"><span data-stu-id="6682a-119">In this example, each expression returns a single property from the current record in the loop, and the values are primitive types which are rendered as text.</span></span>

<span data-ttu-id="6682a-120">Ripetere la ricerca alla visualizzazione Students/Index e selezionare **dettagli** per uno degli studenti.</span><span class="sxs-lookup"><span data-stu-id="6682a-120">Browse to the Students/Index view again and select **Details** for one of the students.</span></span> <span data-ttu-id="6682a-121">Si noterà che i corsi registrati sono stati inclusi nella vista.</span><span class="sxs-lookup"><span data-stu-id="6682a-121">You will see the enrolled courses have been included in the view.</span></span>

![studente con la registrazione](customizing-a-view/_static/image1.png)

> [!div class="step-by-step"]
> <span data-ttu-id="6682a-123">[Precedente](changing-the-database.md)
> [Successivo](enhancing-data-validation.md)</span><span class="sxs-lookup"><span data-stu-id="6682a-123">[Previous](changing-the-database.md)
[Next](enhancing-data-validation.md)</span></span>
