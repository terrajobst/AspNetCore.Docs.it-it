---
uid: identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
title: "Identità di ASP.NET: Uso di archiviazione di MySQL con un Provider di MySQL EntityFramework (c#) | Documenti Microsoft"
author: maumar
description: In questa esercitazione viene illustrato come sostituire il meccanismo di archiviazione di dati predefinito per ASP.NET Identity EntityFramework (provider SQL client) con un essere ampliati o ridotti MySQL...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/10/2013
ms.topic: article
ms.assetid: 15253312-a92c-43ba-908e-b5dacd3d08b8
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
msc.type: authoredcontent
ms.openlocfilehash: ac254abcb756d048d159a9b67967a581f35ac871
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider-c"></a><span data-ttu-id="e9168-103">ASP.NET Identity: Uso di archiviazione di MySQL con un Provider di EntityFramework MySQL (c#)</span><span class="sxs-lookup"><span data-stu-id="e9168-103">ASP.NET Identity: Using MySQL Storage with an EntityFramework MySQL Provider (C#)</span></span>
====================
<span data-ttu-id="e9168-104">da [Maurycy Markowski](https://github.com/maumar), [Raquel Soares De Almeida](https://github.com/raquelsa), [Robert McMurray](https://github.com/rmcmurray)</span><span class="sxs-lookup"><span data-stu-id="e9168-104">by [Maurycy Markowski](https://github.com/maumar), [Raquel Soares De Almeida](https://github.com/raquelsa), [Robert McMurray](https://github.com/rmcmurray)</span></span>

> <span data-ttu-id="e9168-105">In questa esercitazione viene illustrato come sostituire il meccanismo di archiviazione di dati predefinito per [ **ASP.NET Identity** ](introduction-to-aspnet-identity.md) con EntityFramework (provider SQL client) con un provider MySQL.</span><span class="sxs-lookup"><span data-stu-id="e9168-105">This tutorial shows you how to replace the default data storage mechanism for [**ASP.NET Identity**](introduction-to-aspnet-identity.md) with EntityFramework (SQL client provider) with a MySQL provider.</span></span>


<span data-ttu-id="e9168-106">In questa esercitazione verranno trattati i seguenti argomenti:</span><span class="sxs-lookup"><span data-stu-id="e9168-106">The following topics will be covered in this tutorial:</span></span>

- <span data-ttu-id="e9168-107">Creazione di un database MySQL su Azure</span><span class="sxs-lookup"><span data-stu-id="e9168-107">Creating a MySQL database on Azure</span></span>
- <span data-ttu-id="e9168-108">Creazione di un'applicazione MVC utilizzando il modello MVC di Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="e9168-108">Creating an MVC application using Visual Studio 2013 MVC template</span></span>
- <span data-ttu-id="e9168-109">Configurazione EntityFramework per funzionare con un provider di database MySQL</span><span class="sxs-lookup"><span data-stu-id="e9168-109">Configuring EntityFramework to work with a MySQL database provider</span></span>
- <span data-ttu-id="e9168-110">Esecuzione dell'applicazione per verificare i risultati</span><span class="sxs-lookup"><span data-stu-id="e9168-110">Running the application to verify the results</span></span>

<span data-ttu-id="e9168-111">Al termine di questa esercitazione, è un'applicazione MVC con l'identità ASP.NET archiviare che utilizza un database MySQL in cui è ospitato in Azure.</span><span class="sxs-lookup"><span data-stu-id="e9168-111">At the end of this tutorial, you will have an MVC application with the ASP.NET Identity store that is using a MySQL database that is hosted in Azure.</span></span>

## <a name="creating-a-mysql-database-instance-on-azure"></a><span data-ttu-id="e9168-112">Creazione di un'istanza di database MySQL su Azure</span><span class="sxs-lookup"><span data-stu-id="e9168-112">Creating a MySQL database instance on Azure</span></span>

1. <span data-ttu-id="e9168-113">Accedere al [portale di Azure](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="e9168-113">Log in to the [Azure Portal](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409).</span></span>
2. <span data-ttu-id="e9168-114">Fare clic su **NEW** nella parte inferiore della pagina e quindi selezionare **archivio**:</span><span class="sxs-lookup"><span data-stu-id="e9168-114">Click **NEW** at the bottom of the page, and then select **STORE**:</span></span>  
  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.png)
3. <span data-ttu-id="e9168-115">Nel **scegliere e componente aggiuntivo** procedura guidata, selezionare **MySQL Database ClearDB**e quindi fare clic su di **successivo** freccia nella parte inferiore del riquadro:</span><span class="sxs-lookup"><span data-stu-id="e9168-115">In the **Choose and Add-on** wizard, select **ClearDB MySQL Database**, and then click the **Next** arrow at the bottom of the frame:</span></span>  
  
 <span data-ttu-id="e9168-116">[Fare clic sull'immagine seguente per espanderlo.</span><span class="sxs-lookup"><span data-stu-id="e9168-116">[Click the following image to expand it.</span></span> <span data-ttu-id="e9168-117">]</span><span class="sxs-lookup"><span data-stu-id="e9168-117">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.png)
4. <span data-ttu-id="e9168-118">Mantenere il valore predefinito **libero** pianificare, modificare il **nome** a **IdentityMySQLDatabase**, selezionare l'area che è più vicino è e quindi fare clic su di **Avanti** freccia nella parte inferiore del riquadro:</span><span class="sxs-lookup"><span data-stu-id="e9168-118">Keep the default **Free** plan, change the **NAME** to **IdentityMySQLDatabase**, select the region that is nearest to you, and then click the **Next** arrow at the bottom of the frame:</span></span>  
  
 <span data-ttu-id="e9168-119">[Fare clic sull'immagine seguente per espanderlo.</span><span class="sxs-lookup"><span data-stu-id="e9168-119">[Click the following image to expand it.</span></span> <span data-ttu-id="e9168-120">]</span><span class="sxs-lookup"><span data-stu-id="e9168-120">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.png)
5. <span data-ttu-id="e9168-121">Fare clic su di **acquisto** segno di spunta per completare la creazione del database.</span><span class="sxs-lookup"><span data-stu-id="e9168-121">Click the **PURCHASE** checkmark to complete the database creation.</span></span>  
  
 <span data-ttu-id="e9168-122">[Fare clic sull'immagine seguente per espanderlo.</span><span class="sxs-lookup"><span data-stu-id="e9168-122">[Click the following image to expand it.</span></span> <span data-ttu-id="e9168-123">]</span><span class="sxs-lookup"><span data-stu-id="e9168-123">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.png)
6. <span data-ttu-id="e9168-124">Dopo aver creato il database, è possibile gestirlo dal **componenti aggiuntivi** scheda nel portale di gestione.</span><span class="sxs-lookup"><span data-stu-id="e9168-124">After your database has been created, you can manage it from the **ADD-ONS** tab in the management portal.</span></span> <span data-ttu-id="e9168-125">Per recuperare le informazioni di connessione per il database, fare clic su **le informazioni di connessione** nella parte inferiore della pagina:</span><span class="sxs-lookup"><span data-stu-id="e9168-125">To retrieve the connection information for your database, click **CONNECTION INFO** at the bottom of the page:</span></span>  
  
 <span data-ttu-id="e9168-126">[Fare clic sull'immagine seguente per espanderlo.</span><span class="sxs-lookup"><span data-stu-id="e9168-126">[Click the following image to expand it.</span></span> <span data-ttu-id="e9168-127">]</span><span class="sxs-lookup"><span data-stu-id="e9168-127">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image10.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image9.png)
7. <span data-ttu-id="e9168-128">Copiare la stringa di connessione facendo clic sul pulsante Copia per il **CONNECTIONSTRING** campo e salvarlo, si utilizzerà queste informazioni più avanti in questa esercitazione per l'applicazione MVC:</span><span class="sxs-lookup"><span data-stu-id="e9168-128">Copy the connection string by clicking on the copy button by the **CONNECTIONSTRING** field and save it; you will use this information later in this tutorial for your MVC application:</span></span>  
  
 <span data-ttu-id="e9168-129">[Fare clic sull'immagine seguente per espanderlo.</span><span class="sxs-lookup"><span data-stu-id="e9168-129">[Click the following image to expand it.</span></span> <span data-ttu-id="e9168-130">]</span><span class="sxs-lookup"><span data-stu-id="e9168-130">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image12.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image11.png)

## <a name="creating-an-mvc-application-project"></a><span data-ttu-id="e9168-131">Creazione di un progetto di applicazione MVC</span><span class="sxs-lookup"><span data-stu-id="e9168-131">Creating an MVC application project</span></span>

<span data-ttu-id="e9168-132">Per completare i passaggi descritti in questa sezione dell'esercitazione, è necessario innanzitutto installare [Visual Studio Express 2013 per Web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="e9168-132">To complete the steps in this section of the tutorial, you will first need to install [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="e9168-133">Dopo aver installato Visual Studio, utilizzare la procedura seguente per creare un nuovo progetto di applicazione MVC:</span><span class="sxs-lookup"><span data-stu-id="e9168-133">Once Visual Studio has been installed, use the following steps to create a new MVC application project:</span></span>

1. <span data-ttu-id="e9168-134">Aprire Visual Studio 2103.</span><span class="sxs-lookup"><span data-stu-id="e9168-134">Open Visual Studio 2103.</span></span>
2. <span data-ttu-id="e9168-135">Fare clic su **nuovo progetto** dal **avviare** pagina oppure è possibile fare clic su di **File** menu e quindi **nuovo progetto**:</span><span class="sxs-lookup"><span data-stu-id="e9168-135">Click **New Project** from the **Start** page, or you can click the **File** menu and then **New Project**:</span></span>  
  
 <span data-ttu-id="e9168-136">[Fare clic sull'immagine seguente per espanderlo.</span><span class="sxs-lookup"><span data-stu-id="e9168-136">[Click the following image to expand it.</span></span> <span data-ttu-id="e9168-137">]</span><span class="sxs-lookup"><span data-stu-id="e9168-137">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.jpg)
3. <span data-ttu-id="e9168-138">Quando il **nuovo progetto** è visualizzata la finestra di dialogo, espandere **Visual c#** nell'elenco dei modelli, fare clic su **Web**e selezionare **applicazione Web ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="e9168-138">When the **New Project** dialog box is displayed, expand **Visual C#** in the list of templates, then click **Web**, and select **ASP.NET Web Application**.</span></span> <span data-ttu-id="e9168-139">Denominare il progetto **IdentityMySQLDemo** e quindi fare clic su **OK**:</span><span class="sxs-lookup"><span data-stu-id="e9168-139">Name your project **IdentityMySQLDemo** and then click **OK**:</span></span>  
  
 <span data-ttu-id="e9168-140">[Fare clic sull'immagine seguente per espanderlo.</span><span class="sxs-lookup"><span data-stu-id="e9168-140">[Click the following image to expand it.</span></span> <span data-ttu-id="e9168-141">]</span><span class="sxs-lookup"><span data-stu-id="e9168-141">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image14.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image13.png)
4. <span data-ttu-id="e9168-142">Nel **nuovo progetto ASP.NET** finestra di dialogo Seleziona il **MVC** templatewith le opzioni predefinite; in questo modo configurare **singoli account utente di** come metodo di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="e9168-142">In the **New ASP.NET Project** dialog, select the **MVC** templatewith the default options; this will configure **Individual User Accounts** as the authentication method.</span></span> <span data-ttu-id="e9168-143">Fare clic su **OK**:</span><span class="sxs-lookup"><span data-stu-id="e9168-143">Click **OK**:</span></span>  
  
 <span data-ttu-id="e9168-144">[Fare clic sull'immagine seguente per espanderlo.</span><span class="sxs-lookup"><span data-stu-id="e9168-144">[Click the following image to expand it.</span></span> <span data-ttu-id="e9168-145">]</span><span class="sxs-lookup"><span data-stu-id="e9168-145">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image16.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image15.png)

## <a name="configure-entityframework-to-work-with-a-mysql-database"></a><span data-ttu-id="e9168-146">Configurare EntityFramework per lavorare con un database MySQL</span><span class="sxs-lookup"><span data-stu-id="e9168-146">Configure EntityFramework to work with a MySQL database</span></span>

### <a name="update-the-entity-framework-assembly-for-your-project"></a><span data-ttu-id="e9168-147">Aggiorna l'assembly di Entity Framework per il progetto</span><span class="sxs-lookup"><span data-stu-id="e9168-147">Update the Entity Framework assembly for your project</span></span>

<span data-ttu-id="e9168-148">L'applicazione MVC che è stato creato dal modello di Visual Studio 2013 contiene un riferimento al [EntityFramework 6.0.0](http://www.nuget.org/packages/EntityFramework) del pacchetto, ma è stato aggiornamenti a tale assembly dopo il rilascio contenenti significativi miglioramenti delle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="e9168-148">The MVC application that was created from the Visual Studio 2013 template contains a reference to the [EntityFramework 6.0.0](http://www.nuget.org/packages/EntityFramework) package, but there have been updates to to that assembly since its release which contain significant performance improvements.</span></span> <span data-ttu-id="e9168-149">Per utilizzare questi ultimi aggiornamenti dell'applicazione, utilizzare la procedura seguente.</span><span class="sxs-lookup"><span data-stu-id="e9168-149">In order to use these latest updates in your application, use the following steps.</span></span>

1. <span data-ttu-id="e9168-150">Aprire il progetto MVC in Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="e9168-150">Open your MVC project in Visual Studio 2013.</span></span>
2. <span data-ttu-id="e9168-151">Fare clic su **strumenti**, quindi fare clic su **Gestione pacchetti libreria**, quindi fare clic su **Package Manager Console**:</span><span class="sxs-lookup"><span data-stu-id="e9168-151">Click **TOOLS**, then click **Library Package Manager**, and then click **Package Manager Console**:</span></span>  
  
 <span data-ttu-id="e9168-152">[Fare clic sull'immagine seguente per espanderlo.</span><span class="sxs-lookup"><span data-stu-id="e9168-152">[Click the following image to expand it.</span></span> <span data-ttu-id="e9168-153">]</span><span class="sxs-lookup"><span data-stu-id="e9168-153">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image18.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image17.png)
3. <span data-ttu-id="e9168-154">Il **Package Manager Console** verranno visualizzati nella parte inferiore di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e9168-154">The **Package Manager Console** will appear in the bottom section of Visual Studio.</span></span> <span data-ttu-id="e9168-155">Tipo &quot; **pacchetto di aggiornamento EntityFramework** &quot; e premere INVIO:</span><span class="sxs-lookup"><span data-stu-id="e9168-155">Type &quot;**Update-Package EntityFramework**&quot; and press Enter:</span></span>  
  
 <span data-ttu-id="e9168-156">[Fare clic sull'immagine seguente per espanderlo.</span><span class="sxs-lookup"><span data-stu-id="e9168-156">[Click the following image to expand it.</span></span> <span data-ttu-id="e9168-157">]</span><span class="sxs-lookup"><span data-stu-id="e9168-157">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image20.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image19.png)

### <a name="install-the-mysql-provider-for-entityframework"></a><span data-ttu-id="e9168-158">Installare il provider di MySQL per EntityFramework</span><span class="sxs-lookup"><span data-stu-id="e9168-158">Install the MySQL provider for EntityFramework</span></span>

<span data-ttu-id="e9168-159">Affinché EntityFramework per connettersi al database MySQL, è necessario installare un provider MySQL.</span><span class="sxs-lookup"><span data-stu-id="e9168-159">In order for EntityFramework to connect to MySQL database, you need to install a MySQL provider.</span></span> <span data-ttu-id="e9168-160">A tale scopo, aprire il **Package Manager Console** e tipo &quot; **MySql.Data.Entity Install-Package - Pre**&quot;, quindi premere INVIO.</span><span class="sxs-lookup"><span data-stu-id="e9168-160">To do so, open the **Package Manager Console** and type &quot;**Install-Package MySql.Data.Entity -Pre**&quot;, and then press Enter.</span></span>

> [!NOTE]
> <span data-ttu-id="e9168-161">Si tratta di una versione non definitiva dell'assembly e di conseguenza può contenere bug.</span><span class="sxs-lookup"><span data-stu-id="e9168-161">This is a pre-release version of the assembly, and as such it may contain bugs.</span></span> <span data-ttu-id="e9168-162">Utilizzare una versione non definitiva del provider non nell'ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="e9168-162">You should not use a pre-release version of the provider in production.</span></span>


<span data-ttu-id="e9168-163">[Fare clic nell'immagine seguente per espandere il nodo].</span><span class="sxs-lookup"><span data-stu-id="e9168-163">[Click the following image to expand it.]</span></span>  
  
[![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image22.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image21.png)

### <a name="making-project-configuration-changes-to-the-webconfig-file-for-your-application"></a><span data-ttu-id="e9168-164">Apportare modifiche di configurazione di progetto nel file Web. config dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="e9168-164">Making project configuration changes to the Web.config file for your application</span></span>

<span data-ttu-id="e9168-165">In questa sezione che si configurerà Entity Framework per utilizzare il provider di MySQL appena installata, registrare la factory del provider MySQL e aggiungere la stringa di connessione da Azure.</span><span class="sxs-lookup"><span data-stu-id="e9168-165">In this section you will configure the Entity Framework to use the MySQL provider that you just installed, register the MySQL provider factory, and add your connection string from Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="e9168-166">Nell'esempio seguente contengono una versione di assembly specifico per MySql.Data.dll.</span><span class="sxs-lookup"><span data-stu-id="e9168-166">The following examples contain a specific assembly version for MySql.Data.dll.</span></span> <span data-ttu-id="e9168-167">Se viene modificata la versione dell'assembly, è necessario modificare le impostazioni di configurazione appropriato con la versione corretta.</span><span class="sxs-lookup"><span data-stu-id="e9168-167">If the assembly version changes, you will need to modify the appropriate configuration settings with the correct version.</span></span>


1. <span data-ttu-id="e9168-168">Aprire il file Web. config per il progetto in Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="e9168-168">Open the Web.config file for your project in Visual Studio 2013.</span></span>
2. <span data-ttu-id="e9168-169">Individuare le seguenti impostazioni di configurazione, che definiscono il provider di database predefinito e la factory per Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="e9168-169">Locate the following configuration settings, which define the default database provider and factory for the Entity Framework:</span></span>

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample1.xml)]
3. <span data-ttu-id="e9168-170">Sostituire le impostazioni di configurazione con il codice seguente, che verrà configurato per utilizzare il provider di MySQL Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="e9168-170">Replace those configuration settings with the following, which will configure the Entity Framework to use the MySQL provider:</span></span> 

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample2.xml)]
4. <span data-ttu-id="e9168-171">Individuare il &lt;connectionStrings&gt; sezione e sostituirlo con il codice seguente, che verrà definita la stringa di connessione per il database di MySQL che è ospitato in Azure (si noti che il valore di providerName anche è stato modificato dal originale):</span><span class="sxs-lookup"><span data-stu-id="e9168-171">Locate the &lt;connectionStrings&gt; section and replace it with the following code, which will define the connection string for your MySQL database that is hosted on Azure (note that providerName value has also been changed from the original):</span></span>

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample3.xml?highlight=3-4)]

### <a name="adding-custom-migrationhistory-context"></a><span data-ttu-id="e9168-172">Aggiunta di contesto MigrationHistory personalizzato</span><span class="sxs-lookup"><span data-stu-id="e9168-172">Adding custom MigrationHistory context</span></span>

<span data-ttu-id="e9168-173">Code First di Entity Framework Usa un **MigrationHistory** tabella per tenere traccia delle modifiche apportate al modello e per garantire la coerenza tra lo schema di database e un schema concettuale.</span><span class="sxs-lookup"><span data-stu-id="e9168-173">Entity Framework Code First uses a **MigrationHistory** table to keep track of model changes and to ensure the consistency between the database schema and conceptual schema.</span></span> <span data-ttu-id="e9168-174">Tuttavia, questa tabella non funziona per MySQL per impostazione predefinita perché la chiave primaria è troppo grande.</span><span class="sxs-lookup"><span data-stu-id="e9168-174">However, this table does not work for MySQL by default because the primary key is too large.</span></span> <span data-ttu-id="e9168-175">Per risolvere questo problema, è necessario ridurre la dimensione della chiave per la tabella.</span><span class="sxs-lookup"><span data-stu-id="e9168-175">To remedy this situation, you will need to shrink the key size for that table.</span></span> <span data-ttu-id="e9168-176">A tale scopo, attenersi alla procedura seguente:</span><span class="sxs-lookup"><span data-stu-id="e9168-176">To do so, use the following steps:</span></span>

1. <span data-ttu-id="e9168-177">Le informazioni sullo schema per questa tabella viene acquisite un **HistoryContext**, che può essere modificato come qualsiasi altro **DbContext**.</span><span class="sxs-lookup"><span data-stu-id="e9168-177">The schema information for this table is captured in a **HistoryContext**, which can be modified as any other **DbContext**.</span></span> <span data-ttu-id="e9168-178">A tale scopo, aggiungere un nuovo file di classe denominato **MySqlHistoryContext.cs** al progetto e sostituirne il contenuto con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="e9168-178">To do so, add a new class file named **MySqlHistoryContext.cs** to the project, and replace its contents with the following code:</span></span>

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample4.cs)]
2. <span data-ttu-id="e9168-179">Successivamente sarà necessario configurare Entity Framework per l'utilizzo di modificati **HistoryContext**, anziché quella predefinita.</span><span class="sxs-lookup"><span data-stu-id="e9168-179">Next you will need to configure Entity Framework to use the modified **HistoryContext**, rather than default one.</span></span> <span data-ttu-id="e9168-180">Questa operazione può essere eseguita sfruttando le funzionalità di configurazione basato sul codice.</span><span class="sxs-lookup"><span data-stu-id="e9168-180">This can be done by leveraging code-based configuration features.</span></span> <span data-ttu-id="e9168-181">A tale scopo, aggiungere nuovi file di classe denominato **MySqlConfiguration.cs** al progetto e sostituirne il contenuto con:</span><span class="sxs-lookup"><span data-stu-id="e9168-181">To do so, add new class file named **MySqlConfiguration.cs** to your project and replace its contents with:</span></span>

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample5.cs)]

### <a name="creating-a-custom-entityframework-initializer-for-applicationdbcontext"></a><span data-ttu-id="e9168-182">Creazione di un inizializzatore EntityFramework personalizzato per ApplicationDbContext</span><span class="sxs-lookup"><span data-stu-id="e9168-182">Creating a custom EntityFramework initializer for ApplicationDbContext</span></span>

<span data-ttu-id="e9168-183">Il provider di MySQL è disponibile in questa esercitazione non supporta attualmente le migrazioni di Entity Framework, pertanto è necessario usare gli inizializzatori di modello per la connessione al database.</span><span class="sxs-lookup"><span data-stu-id="e9168-183">The MySQL provider that is featured in this tutorial does not currently support Entity Framework migrations, so you will need to use model initializers in order to connect to the database.</span></span> <span data-ttu-id="e9168-184">Poiché in questa esercitazione Usa un'istanza di MySQL in Azure, è necessario necessario creare un inizializzatore personalizzato in Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e9168-184">Because this tutorial is using a MySQL instance on Azure, you will need need to create a custom Entity Framework initializer.</span></span>

> [!NOTE]
> <span data-ttu-id="e9168-185">Questo passaggio non è obbligatorio se ci si connette a un'istanza di SQL Server in Azure o se si utilizza un database in cui è ospitato in locale.</span><span class="sxs-lookup"><span data-stu-id="e9168-185">This step is not required if you are connecting to a SQL Server instance on Azure or if you are using a database that is hosted on premises.</span></span>


<span data-ttu-id="e9168-186">Per creare un inizializzatore personalizzato in Entity Framework per MySQL, attenersi alla procedura seguente:</span><span class="sxs-lookup"><span data-stu-id="e9168-186">To create a custom Entity Framework initializer for MySQL, use the following steps:</span></span>

1. <span data-ttu-id="e9168-187">Aggiungere un nuovo file di classe denominato **MySqlInitializer.cs** è per il progetto, quindi sostituire il contenuto con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="e9168-187">Add a new class file named **MySqlInitializer.cs** to the project, and replace it's contents with the following code:</span></span> 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample6.cs?highlight=23)]
2. <span data-ttu-id="e9168-188">Aprire il **IdentityModels.cs** file per il progetto, che si trova nella **modelli** directory e sostituire il contenuto con quanto segue:</span><span class="sxs-lookup"><span data-stu-id="e9168-188">Open the **IdentityModels.cs** file for your project, which is located in the **Models** directory, and replace it's contents with the following:</span></span> 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample7.cs)]

## <a name="running-the-application-and-verifying-the-database"></a><span data-ttu-id="e9168-189">Esecuzione dell'applicazione e verifica del database</span><span class="sxs-lookup"><span data-stu-id="e9168-189">Running the application and verifying the database</span></span>

<span data-ttu-id="e9168-190">Dopo aver completato i passaggi descritti nelle sezioni precedenti, è necessario testare il database.</span><span class="sxs-lookup"><span data-stu-id="e9168-190">Once you have completed the steps in the preceding sections, you should test your database.</span></span> <span data-ttu-id="e9168-191">A tale scopo, attenersi alla procedura seguente:</span><span class="sxs-lookup"><span data-stu-id="e9168-191">To do so, use the following steps:</span></span>

1. <span data-ttu-id="e9168-192">Premere **Ctrl + F5** per compilare ed eseguire l'applicazione web.</span><span class="sxs-lookup"><span data-stu-id="e9168-192">Press **Ctrl + F5** to build and run the web application.</span></span>
2. <span data-ttu-id="e9168-193">Fare clic su di **registrare** scheda nella parte superiore della pagina:</span><span class="sxs-lookup"><span data-stu-id="e9168-193">Click the **Register** tab on the top of the page:</span></span>  
  
 <span data-ttu-id="e9168-194">[Fare clic sull'immagine seguente per espanderlo.</span><span class="sxs-lookup"><span data-stu-id="e9168-194">[Click the following image to expand it.</span></span> <span data-ttu-id="e9168-195">]</span><span class="sxs-lookup"><span data-stu-id="e9168-195">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.jpg)
3. <span data-ttu-id="e9168-196">Immettere un nuovo nome utente e una password e quindi fare clic su **registrare**:</span><span class="sxs-lookup"><span data-stu-id="e9168-196">Enter a new user name and password, and then click **Register**:</span></span>  
  
 <span data-ttu-id="e9168-197">[Fare clic sull'immagine seguente per espanderlo.</span><span class="sxs-lookup"><span data-stu-id="e9168-197">[Click the following image to expand it.</span></span> <span data-ttu-id="e9168-198">]</span><span class="sxs-lookup"><span data-stu-id="e9168-198">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image24.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image23.png)
4. <span data-ttu-id="e9168-199">A questo punto in cui vengono create le tabelle di ASP.NET Identity nel Database di MySQL e l'utente è registrato e connesso all'applicazione:</span><span class="sxs-lookup"><span data-stu-id="e9168-199">At this point the ASP.NET Identity tables are created on the MySQL Database, and the user is registered and logged into the application:</span></span>  
  
 <span data-ttu-id="e9168-200">[Fare clic sull'immagine seguente per espanderlo.</span><span class="sxs-lookup"><span data-stu-id="e9168-200">[Click the following image to expand it.</span></span> <span data-ttu-id="e9168-201">]</span><span class="sxs-lookup"><span data-stu-id="e9168-201">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.jpg)

### <a name="installing-mysql-workbench-tool-to-verify-the-data"></a><span data-ttu-id="e9168-202">Installazione dello strumento di MySQL Workbench per verificare i dati</span><span class="sxs-lookup"><span data-stu-id="e9168-202">Installing MySQL Workbench tool to verify the data</span></span>

1. <span data-ttu-id="e9168-203">Installare il **MySQL Workbench** dello strumento la [pagina di download di MySQL](http://dev.mysql.com/downloads/windows/installer/)</span><span class="sxs-lookup"><span data-stu-id="e9168-203">Install the **MySQL Workbench** tool from the [MySQL downloads page](http://dev.mysql.com/downloads/windows/installer/)</span></span>
2. <span data-ttu-id="e9168-204">Nell'installazione guidata: **Selezione funzionalità** , selezionare **MySQL Workbench** in **applicazioni** sezione.</span><span class="sxs-lookup"><span data-stu-id="e9168-204">In the installation wizard: **Feature Selection** tab, select **MySQL Workbench** under **applications** section.</span></span>
3. <span data-ttu-id="e9168-205">Avviare l'app e aggiungere una nuova connessione utilizzando i dati di stringa di connessione del database MySQL Azure creato al colmare di questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="e9168-205">Launch the app and add a new connection using the connection string data from the Azure MySQL database you created at the begging of this tutorial.</span></span>
4. <span data-ttu-id="e9168-206">Dopo avere stabilito la connessione, controllare il **ASP.NET Identity** le tabelle create nel **IdentityMySQLDatabase.**</span><span class="sxs-lookup"><span data-stu-id="e9168-206">After establishing the connection, inspect the **ASP.NET Identity** tables created on the **IdentityMySQLDatabase.**</span></span>
5. <span data-ttu-id="e9168-207">Si noterà che tutte le identità di ASP.NET necessari le tabelle vengono create come illustrato nell'immagine riportata di seguito:</span><span class="sxs-lookup"><span data-stu-id="e9168-207">You will see that all ASP.NET Identity required tables are created as shown in the image below:</span></span>  
  
 <span data-ttu-id="e9168-208">[Fare clic sull'immagine seguente per espanderlo.</span><span class="sxs-lookup"><span data-stu-id="e9168-208">[Click the following image to expand it.</span></span> <span data-ttu-id="e9168-209">]</span><span class="sxs-lookup"><span data-stu-id="e9168-209">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.jpg)
6. <span data-ttu-id="e9168-210">Controllare il **aspnetusers** per l'istanza di tabella per cercare le voci di registrazione di nuovi utenti.</span><span class="sxs-lookup"><span data-stu-id="e9168-210">Inspect the **aspnetusers** table for instance to check for the entries as you register new users.</span></span>  
  
 <span data-ttu-id="e9168-211">[Fare clic sull'immagine seguente per espanderlo.</span><span class="sxs-lookup"><span data-stu-id="e9168-211">[Click the following image to expand it.</span></span> <span data-ttu-id="e9168-212">]</span><span class="sxs-lookup"><span data-stu-id="e9168-212">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image26.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image25.png)
