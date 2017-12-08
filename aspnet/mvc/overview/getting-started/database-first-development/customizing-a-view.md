---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: 'Database di Entity Framework prima con ASP.NET MVC: personalizzare una visualizzazione | Documenti Microsoft'
author: tfitzmac
description: "Usa lo Scaffolding di ASP.NET MVC ed Entity Framework, è possibile creare un'applicazione web che fornisce un'interfaccia a un database esistente. Questa esercitazione seri..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: af9609396cff18b08824732731ddb9c5cca578fa
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="ef-database-first-with-aspnet-mvc-customizing-a-view"></a><span data-ttu-id="96225-104">Database di Entity Framework prima con ASP.NET MVC: personalizzare una visualizzazione</span><span class="sxs-lookup"><span data-stu-id="96225-104">EF Database First with ASP.NET MVC: Customizing a View</span></span>
====================
<span data-ttu-id="96225-105">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="96225-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="96225-106">Usa lo Scaffolding di ASP.NET MVC ed Entity Framework, è possibile creare un'applicazione web che fornisce un'interfaccia a un database esistente.</span><span class="sxs-lookup"><span data-stu-id="96225-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="96225-107">Questa serie di esercitazioni viene illustrato come automaticamente generare il codice che consente agli utenti di visualizzare, modificare, creare ed eliminare dati che si trovano in una tabella di database.</span><span class="sxs-lookup"><span data-stu-id="96225-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="96225-108">Il codice generato corrisponde alle colonne nella tabella di database.</span><span class="sxs-lookup"><span data-stu-id="96225-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="96225-109">Questa parte della serie è incentrata sulla modifica delle viste generati automaticamente per migliorare la presentazione.</span><span class="sxs-lookup"><span data-stu-id="96225-109">This part of the series focuses on changing the automatically-generated views to enhance the presentation.</span></span>


## <a name="add-enrolled-courses-to-student-details"></a><span data-ttu-id="96225-110">Aggiungi corsi registrati dettagli per studenti</span><span class="sxs-lookup"><span data-stu-id="96225-110">Add enrolled courses to student details</span></span>

<span data-ttu-id="96225-111">Il codice generato fornisce un buon punto di partenza per l'applicazione ma non necessariamente fornisce tutte le funzionalità necessarie nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="96225-111">The generated code provides a good starting point for your application but it does not necessarily provide all of the functionality that you need in your application.</span></span> <span data-ttu-id="96225-112">È possibile personalizzare il codice per soddisfare i requisiti specifici dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="96225-112">You can customize the code to meet the particular requirements of your application.</span></span> <span data-ttu-id="96225-113">Attualmente, l'applicazione non vengono visualizzati i corsi registrati per gli studenti selezionato.</span><span class="sxs-lookup"><span data-stu-id="96225-113">Currently, your application does not display the enrolled courses for the selected student.</span></span> <span data-ttu-id="96225-114">In questa sezione si aggiungeranno i corsi registrati per ogni studente per il **dettagli** visualizzazione per gli studenti.</span><span class="sxs-lookup"><span data-stu-id="96225-114">In this section, you will add the enrolled courses for each student to the **Details** view for the student.</span></span>

<span data-ttu-id="96225-115">Aprire **Students/Details.cshtml**e sotto l'ultima &lt;/dl&gt; scheda, ma prima della chiusura &lt;/div&gt; tag, aggiungere il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="96225-115">Open **Students/Details.cshtml**, and below the last &lt;/dl&gt; tab, but before the closing &lt;/div&gt; tag, add the following code.</span></span>

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

<span data-ttu-id="96225-116">Questo codice crea una tabella che visualizza una riga per ogni record nella tabella di registrazione per gli studenti selezionato.</span><span class="sxs-lookup"><span data-stu-id="96225-116">This code creates a table that displays a row for each record in the Enrollment table for the selected student.</span></span> <span data-ttu-id="96225-117">Il **visualizzazione** metodo esegue il rendering HTML per l'oggetto (modelItem) che rappresenta l'espressione.</span><span class="sxs-lookup"><span data-stu-id="96225-117">The **Display** method renders HTML for the object (modelItem) that represents the expression.</span></span> <span data-ttu-id="96225-118">Utilizzare il metodo di visualizzazione (invece di incorporamento semplicemente il valore della proprietà nel codice) per assicurarsi che il valore viene formattato correttamente in base a tipo e il modello per quel tipo.</span><span class="sxs-lookup"><span data-stu-id="96225-118">You use the Display method (rather than simply embedding the property value in the code) to make sure the value is formatted correctly based on its type and the template for that type.</span></span> <span data-ttu-id="96225-119">In questo esempio, ogni espressione restituisce una singola proprietà del record corrente del ciclo e i valori sono i tipi primitivi che vengono visualizzati come testo.</span><span class="sxs-lookup"><span data-stu-id="96225-119">In this example, each expression returns a single property from the current record in the loop, and the values are primitive types which are rendered as text.</span></span>

<span data-ttu-id="96225-120">Ripetere la ricerca per la visualizzazione di studenti/indice e selezionare **dettagli** per uno degli studenti.</span><span class="sxs-lookup"><span data-stu-id="96225-120">Browse to the Students/Index view again and select **Details** for one of the students.</span></span> <span data-ttu-id="96225-121">Verranno visualizzati che i corsi registrati sono stati inclusi nella vista.</span><span class="sxs-lookup"><span data-stu-id="96225-121">You will see the enrolled courses have been included in the view.</span></span>

![studente con la registrazione](customizing-a-view/_static/image1.png)

>[!div class="step-by-step"]
<span data-ttu-id="96225-123">[Precedente](changing-the-database.md)
[Successivo](enhancing-data-validation.md)</span><span class="sxs-lookup"><span data-stu-id="96225-123">[Previous](changing-the-database.md)
[Next](enhancing-data-validation.md)</span></span>
