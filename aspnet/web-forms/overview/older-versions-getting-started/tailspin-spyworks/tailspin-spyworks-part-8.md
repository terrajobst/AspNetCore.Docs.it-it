---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
title: 'Parte 8: Pagine finali, gestione delle eccezioni e conclusione | Microsoft Docs'
author: JoeStagner
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks. Parte 8 aggiunge una pagina di contatto, sulla pagina e l'eccezione...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 5aeadf8f-39f3-4f07-a78f-1c310c64fb23
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
msc.type: authoredcontent
ms.openlocfilehash: 3f1d855e157f6a58995d301a793e660925767fb4
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37380253"
---
<a name="part-8-final-pages-exception-handling-and-conclusion"></a><span data-ttu-id="e3fbe-104">Parte 8: Pagine finali, gestione delle eccezioni e conclusione</span><span class="sxs-lookup"><span data-stu-id="e3fbe-104">Part 8: Final Pages, Exception Handling, and Conclusion</span></span>
====================
<span data-ttu-id="e3fbe-105">da [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="e3fbe-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="e3fbe-106">Tailspin Spyworks viene illustrato come saliente è davvero semplice per creare applicazioni potenti e scalabili per la piattaforma .NET.</span><span class="sxs-lookup"><span data-stu-id="e3fbe-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="e3fbe-107">Illustra come usare le nuove funzionalità in ASP.NET 4 per creare un negozio online, tra cui acquisti, estrazione e l'amministrazione.</span><span class="sxs-lookup"><span data-stu-id="e3fbe-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="e3fbe-108">Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="e3fbe-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="e3fbe-109">Parte 8 aggiunge una pagina di contatto, sulla pagina e la gestione delle eccezioni.</span><span class="sxs-lookup"><span data-stu-id="e3fbe-109">Part 8 adds a contact page, about page, and exception handling.</span></span> <span data-ttu-id="e3fbe-110">Si tratta la conclusione della serie.</span><span class="sxs-lookup"><span data-stu-id="e3fbe-110">This is the conclusion of the series.</span></span>


## <a id="_Toc260221680"></a>  <span data-ttu-id="e3fbe-111">Contattare pagina (invio messaggio di posta elettronica da ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="e3fbe-111">Contact Page (Sending email from ASP.NET)</span></span>

<span data-ttu-id="e3fbe-112">Creare una nuova pagina denominata ContactUs.aspx</span><span class="sxs-lookup"><span data-stu-id="e3fbe-112">Create a new page named ContactUs.aspx</span></span>

<span data-ttu-id="e3fbe-113">Usando la finestra di progettazione, creare il formato seguente, preso nota speciale per includere il ToolkitScriptManager e il controllo dell'Editor dal AjaxdControlToolkit.</span><span class="sxs-lookup"><span data-stu-id="e3fbe-113">Using the designer, create the following form taking special note to include the ToolkitScriptManager and the Editor control from the AjaxdControlToolkit.</span></span> <span data-ttu-id="e3fbe-114">.</span><span class="sxs-lookup"><span data-stu-id="e3fbe-114">.</span></span>

![](tailspin-spyworks-part-8/_static/image1.jpg)

<span data-ttu-id="e3fbe-115">Fare doppio clic sul pulsante "Invia" per generare un gestore eventi click nel file dietro il codice e implementare un metodo per inviare le informazioni di contatto come messaggio di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="e3fbe-115">Double click on the "Submit" button to generate a click event handler in the code behind file and implement a method to send the contact information as an email.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample1.cs)]

<span data-ttu-id="e3fbe-116">Questo codice si presuppone che il file Web. config contenga una voce nella sezione di configurazione che specifica il server SMTP da utilizzare per l'invio di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="e3fbe-116">This code requires that your web.config file contain an entry in the configuration section that specifies the SMTP server to use for sending mail.</span></span>

[!code-xml[Main](tailspin-spyworks-part-8/samples/sample2.xml)]

## <a id="_Toc260221681"></a>  <span data-ttu-id="e3fbe-117">Informazioni sulla pagina</span><span class="sxs-lookup"><span data-stu-id="e3fbe-117">About Page</span></span>

<span data-ttu-id="e3fbe-118">Creare una pagina denominata AboutUs. aspx e aggiungere il contenuto desiderato.</span><span class="sxs-lookup"><span data-stu-id="e3fbe-118">Create a page named AboutUs.aspx and add whatever content you like.</span></span>

## <a id="_Toc260221682"></a>  <span data-ttu-id="e3fbe-119">Gestore eccezioni globale</span><span class="sxs-lookup"><span data-stu-id="e3fbe-119">Global Exception Handler</span></span>

<span data-ttu-id="e3fbe-120">Infine, in tutta l'applicazione è stato generato l'eccezione e vi sono circostanze impreviste che a freddo anche le eccezioni non gestita causa nella nostra applicazione web.</span><span class="sxs-lookup"><span data-stu-id="e3fbe-120">Lastly, throughout the application we have thrown exceptions and there are unforeseen circumstances that cold also cause unhandled exceptions in our web application.</span></span>

<span data-ttu-id="e3fbe-121">Non vogliamo mai un'eccezione non gestita da visualizzare per un visitatore di un sito web.</span><span class="sxs-lookup"><span data-stu-id="e3fbe-121">We never want an unhandled exception to be displayed to a web site visitor.</span></span>

![](tailspin-spyworks-part-8/_static/image2.jpg)

<span data-ttu-id="e3fbe-122">Oltre ad avere un'esperienza utente terribili le eccezioni non gestite possono anche essere un problema di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="e3fbe-122">Apart from being a terrible user experience unhandled exceptions can also be a security problem.</span></span>

<span data-ttu-id="e3fbe-123">Per risolvere questo problema verrà implementato un gestore eccezioni globale.</span><span class="sxs-lookup"><span data-stu-id="e3fbe-123">To solve this problem we will implement a global exception handler.</span></span>

<span data-ttu-id="e3fbe-124">A tale scopo, aprire il file Global. asax e notare il seguente gestore eventi generati in precedenza.</span><span class="sxs-lookup"><span data-stu-id="e3fbe-124">To do this, open the Global.asax file and note the following pre-generated event handler.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample3.cs)]

<span data-ttu-id="e3fbe-125">Aggiungere il codice per implementare l'applicazione\_gestore errori come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="e3fbe-125">Add code to implement the Application\_Error handler as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample4.cs)]

<span data-ttu-id="e3fbe-126">Quindi aggiungere una pagina denominata aspx alla soluzione e aggiungere questo frammento di markup.</span><span class="sxs-lookup"><span data-stu-id="e3fbe-126">Then add a page named Error.aspx to the solution and add this markup snippet.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-8/samples/sample5.aspx)]

<span data-ttu-id="e3fbe-127">Ora nella pagina\_caricare extract di gestore eventi i messaggi di errore dall'oggetto Request.</span><span class="sxs-lookup"><span data-stu-id="e3fbe-127">Now in the Page\_Load event handler extract the error messages from the Request Object.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample6.cs)]

## <a id="_Toc260221683"></a>  <span data-ttu-id="e3fbe-128">Conclusioni</span><span class="sxs-lookup"><span data-stu-id="e3fbe-128">Conclusion</span></span>

<span data-ttu-id="e3fbe-129">Abbiamo visto che Web Form ASP.NET rende più semplice per creare un sito Web sofisticate con accesso al database, l'appartenenza, AJAX, e così via.</span><span class="sxs-lookup"><span data-stu-id="e3fbe-129">We've seen that ASP.NET WebForms makes it easy to create a sophisticated website with database access, membership, AJAX, etc.</span></span> <span data-ttu-id="e3fbe-130">abbastanza rapidamente.</span><span class="sxs-lookup"><span data-stu-id="e3fbe-130">pretty quickly.</span></span>

<span data-ttu-id="e3fbe-131">Si spera in questa esercitazione è stata fornita gli strumenti che necessari per iniziare a compilare applicazioni proprie Web Form ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e3fbe-131">Hopefully this tutorial has given you the tools you need to get started building your own ASP.NET WebForms applications!</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="e3fbe-132">Precedente</span><span class="sxs-lookup"><span data-stu-id="e3fbe-132">Previous</span></span>](tailspin-spyworks-part-7.md)
