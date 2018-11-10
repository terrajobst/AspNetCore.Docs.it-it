---
title: Ospitare ASP.NET Core in un servizio Windows
author: guardrex
description: Informazioni su come ospitare un'app ASP.NET Core in un servizio Windows.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/30/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: 11913019bfe5d06c259b806fce9cc580a8280ad5
ms.sourcegitcommit: fc2486ddbeb15ab4969168d99b3fe0fbe91e8661
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/01/2018
ms.locfileid: "50758193"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="b6d97-103">Ospitare ASP.NET Core in un servizio Windows</span><span class="sxs-lookup"><span data-stu-id="b6d97-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="b6d97-104">Di [Luke Latham](https://github.com/guardrex) e [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="b6d97-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="b6d97-105">È possibile ospitare un'app ASP.NET Core in Windows senza usare IIS come [servizio Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="b6d97-105">An ASP.NET Core app can be hosted on Windows without using IIS as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="b6d97-106">Quando è ospitata come servizio Windows, l'app viene avviata automaticamente dopo ogni riavvio.</span><span class="sxs-lookup"><span data-stu-id="b6d97-106">When hosted as a Windows Service, the app automatically starts after reboots.</span></span>

<span data-ttu-id="b6d97-107">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b6d97-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="convert-a-project-into-a-windows-service"></a><span data-ttu-id="b6d97-108">Convertire un progetto in un servizio Windows</span><span class="sxs-lookup"><span data-stu-id="b6d97-108">Convert a project into a Windows Service</span></span>

<span data-ttu-id="b6d97-109">Sono necessarie le modifiche minime seguenti per configurare un progetto ASP.NET Core esistente da eseguire come servizio:</span><span class="sxs-lookup"><span data-stu-id="b6d97-109">The following minimum changes are required to set up an existing ASP.NET Core project to run as a service:</span></span>

1. <span data-ttu-id="b6d97-110">Nel file di progetto:</span><span class="sxs-lookup"><span data-stu-id="b6d97-110">In the project file:</span></span>

   * <span data-ttu-id="b6d97-111">Verificare la presenza di un [identificatore di runtime](/dotnet/core/rid-catalog) di Windows o aggiungerlo al `<PropertyGroup>` che contiene il framework di destinazione:</span><span class="sxs-lookup"><span data-stu-id="b6d97-111">Confirm the presence of a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) or add it to the `<PropertyGroup>` that contains the target framework:</span></span>

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.2</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      <span data-ttu-id="b6d97-112">Per eseguire la pubblicazione per più identificatori di runtime:</span><span class="sxs-lookup"><span data-stu-id="b6d97-112">To publish for multiple RIDs:</span></span>

      * <span data-ttu-id="b6d97-113">Specificare gli identificatori di runtime in un elenco delimitato da punto e virgola.</span><span class="sxs-lookup"><span data-stu-id="b6d97-113">Provide the RIDs in a semicolon-delimited list.</span></span>
      * <span data-ttu-id="b6d97-114">Usare il nome di proprietà `<RuntimeIdentifiers>` (plurale).</span><span class="sxs-lookup"><span data-stu-id="b6d97-114">Use the property name `<RuntimeIdentifiers>` (plural).</span></span>

      <span data-ttu-id="b6d97-115">Per altre informazioni, vedere il [Catalogo RID di .NET Core](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="b6d97-115">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

   * <span data-ttu-id="b6d97-116">Aggiungere un riferimento al pacchetto per [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span><span class="sxs-lookup"><span data-stu-id="b6d97-116">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span></span>

1. <span data-ttu-id="b6d97-117">Modificare `Program.Main` nel modo seguente:</span><span class="sxs-lookup"><span data-stu-id="b6d97-117">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="b6d97-118">Chiamare [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) invece di `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="b6d97-118">Call [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) instead of `host.Run`.</span></span>

   * <span data-ttu-id="b6d97-119">Chiamare [UseContentRoot](xref:fundamentals/host/web-host#content-root) e usare un percorso di pubblicazione dell'app invece di `Directory.GetCurrentDirectory()`.</span><span class="sxs-lookup"><span data-stu-id="b6d97-119">Call [UseContentRoot](xref:fundamentals/host/web-host#content-root) and use a path to the app's published location instead of `Directory.GetCurrentDirectory()`.</span></span>

     [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=8-9,16)]

1. <span data-ttu-id="b6d97-120">Pubblicare l'app usando [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), un [profilo di pubblicazione di Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) o Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="b6d97-120">Publish the app using [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles), or Visual Studio Code.</span></span> <span data-ttu-id="b6d97-121">Quando si usa Visual Studio, selezionare **FolderProfile** e configurare il **Percorso di destinazione** prima di selezionare il pulsante **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="b6d97-121">When using Visual Studio, select the **FolderProfile** and configure the **Target Location** before selecting the **Publish** button.</span></span>

   <span data-ttu-id="b6d97-122">Per pubblicare l'app di esempio usando gli strumenti dell'interfaccia della riga di comando, eseguire il comando [dotnet publish](/dotnet/core/tools/dotnet-publish) da un prompt dei comandi nella cartella del progetto.</span><span class="sxs-lookup"><span data-stu-id="b6d97-122">To publish the sample app using command-line interface (CLI) tools, run the [dotnet publish](/dotnet/core/tools/dotnet-publish) command at a command prompt from the project folder.</span></span> <span data-ttu-id="b6d97-123">L'identificatore di runtime deve essere specificato nella proprietà `<RuntimeIdenfifier>` (o `<RuntimeIdentifiers>`) del file di progetto.</span><span class="sxs-lookup"><span data-stu-id="b6d97-123">The RID must be specified in the `<RuntimeIdenfifier>` (or `<RuntimeIdentifiers>`) property of the project file.</span></span> <span data-ttu-id="b6d97-124">Nell'esempio seguente, l'app viene pubblicata nella configurazione per il rilascio del runtime `win7-x64` in una cartella creata in *c:\\svc*:</span><span class="sxs-lookup"><span data-stu-id="b6d97-124">In the following example, the app is published in Release configuration for the `win7-x64` runtime to a folder created at *c:\\svc*:</span></span>

   ```console
   dotnet publish --configuration Release --runtime win7-x64 --output c:\svc
   ```

1. <span data-ttu-id="b6d97-125">Creare un account utente per il servizio con il comando `net user`:</span><span class="sxs-lookup"><span data-stu-id="b6d97-125">Create a user account for the service using the `net user` command:</span></span>

   ```console
   net user {USER ACCOUNT} {PASSWORD} /add
   ```

   <span data-ttu-id="b6d97-126">Per l'app di esempio, creare un account utente con il nome `ServiceUser` e una password.</span><span class="sxs-lookup"><span data-stu-id="b6d97-126">For the sample app, create a user account with the name `ServiceUser` and a password.</span></span> <span data-ttu-id="b6d97-127">Nel comando seguente sostituire `{PASSWORD}` con una [password complessa](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).</span><span class="sxs-lookup"><span data-stu-id="b6d97-127">In the following command, replace `{PASSWORD}` with a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).</span></span>

   ```console
   net user ServiceUser {PASSWORD} /add
   ```

   <span data-ttu-id="b6d97-128">Se è necessario aggiungere l'utente a un gruppo, usare il comando `net localgroup`, dove `{GROUP}` è il nome del gruppo:</span><span class="sxs-lookup"><span data-stu-id="b6d97-128">If you need to add the user to a group, use the `net localgroup` command, where `{GROUP}` is the name of the group:</span></span>

   ```console
   net localgroup {GROUP} {USER ACCOUNT} /add
   ```

   <span data-ttu-id="b6d97-129">Per altre informazioni, vedere [Service User Accounts](/windows/desktop/services/service-user-accounts) (Account utente di servizio).</span><span class="sxs-lookup"><span data-stu-id="b6d97-129">For more information, see [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

1. <span data-ttu-id="b6d97-130">Concedere l'accesso per scrittura/lettura/esecuzione alla cartella dell'app usando il comando [icacls](/windows-server/administration/windows-commands/icacls):</span><span class="sxs-lookup"><span data-stu-id="b6d97-130">Grant write/read/execute access to the app's folder using the [icacls](/windows-server/administration/windows-commands/icacls) command:</span></span>

   ```console
   icacls "{PATH}" /grant {USER ACCOUNT}:(OI)(CI){PERMISSION FLAGS} /t
   ```

   * <span data-ttu-id="b6d97-131">`{PATH}` &ndash; Percorso della cartella dell'app.</span><span class="sxs-lookup"><span data-stu-id="b6d97-131">`{PATH}` &ndash; Path to the app's folder.</span></span>
   * <span data-ttu-id="b6d97-132">`{USER ACCOUNT}` &ndash; Account utente (SID).</span><span class="sxs-lookup"><span data-stu-id="b6d97-132">`{USER ACCOUNT}` &ndash; The user account (SID).</span></span>
   * <span data-ttu-id="b6d97-133">`(OI)` &ndash; Il flag Eredità oggetto propaga le autorizzazioni ai file subordinati.</span><span class="sxs-lookup"><span data-stu-id="b6d97-133">`(OI)` &ndash; The Object Inherit flag propagates permissions to subordinate files.</span></span>
   * <span data-ttu-id="b6d97-134">`(CI)` &ndash; Il flag Eredità contenitore propaga le autorizzazioni alle cartelle subordinate.</span><span class="sxs-lookup"><span data-stu-id="b6d97-134">`(CI)` &ndash; The Container Inherit flag propagates permissions to subordinate folders.</span></span>
   * <span data-ttu-id="b6d97-135">`{PERMISSION FLAGS}` &ndash; Imposta le autorizzazioni di accesso dell'app.</span><span class="sxs-lookup"><span data-stu-id="b6d97-135">`{PERMISSION FLAGS}` &ndash; Sets the app's access permissions.</span></span>
     * <span data-ttu-id="b6d97-136">Scrittura (`W`)</span><span class="sxs-lookup"><span data-stu-id="b6d97-136">Write (`W`)</span></span>
     * <span data-ttu-id="b6d97-137">Lettura (`R`)</span><span class="sxs-lookup"><span data-stu-id="b6d97-137">Read (`R`)</span></span>
     * <span data-ttu-id="b6d97-138">Esecuzione (`X`)</span><span class="sxs-lookup"><span data-stu-id="b6d97-138">Execute (`X`)</span></span>
     * <span data-ttu-id="b6d97-139">Completo (`F`)</span><span class="sxs-lookup"><span data-stu-id="b6d97-139">Full (`F`)</span></span>
     * <span data-ttu-id="b6d97-140">Modifica (`M`)</span><span class="sxs-lookup"><span data-stu-id="b6d97-140">Modify (`M`)</span></span>
   * <span data-ttu-id="b6d97-141">`/t` &ndash; Applicare in modo ricorsivo alle cartelle e ai file subordinati esistenti.</span><span class="sxs-lookup"><span data-stu-id="b6d97-141">`/t` &ndash; Apply recursively to existing subordinate folders and files.</span></span>

   <span data-ttu-id="b6d97-142">Per l'app di esempio pubblicata nella cartella *c:\\svc* e l'account `ServiceUser` con autorizzazioni di scrittura/lettura/esecuzione, usare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="b6d97-142">For the sample app published to the *c:\\svc* folder and the `ServiceUser` account with write/read/execute permissions, use the following command:</span></span>

   ```console
   icacls "c:\svc" /grant ServiceUser:(OI)(CI)WRX /t
   ```

   <span data-ttu-id="b6d97-143">Per altre informazioni, vedere [icacls](/windows-server/administration/windows-commands/icacls).</span><span class="sxs-lookup"><span data-stu-id="b6d97-143">For more information, see [icacls](/windows-server/administration/windows-commands/icacls).</span></span>

1. <span data-ttu-id="b6d97-144">Usare lo strumento da riga di comando [sc.exe](https://technet.microsoft.com/library/bb490995) per creare il servizio.</span><span class="sxs-lookup"><span data-stu-id="b6d97-144">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service.</span></span> <span data-ttu-id="b6d97-145">Il valore `binPath` rappresenta il percorso dell'eseguibile dell'app, che include il nome del file eseguibile.</span><span class="sxs-lookup"><span data-stu-id="b6d97-145">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="b6d97-146">**Lo spazio tra il segno di uguale e il carattere di virgoletta di ogni parametro e valore è obbligatorio.**</span><span class="sxs-lookup"><span data-stu-id="b6d97-146">**The space between the equal sign and the quote character of each parameter and value is required.**</span></span>

   ```console
   sc create {SERVICE NAME} binPath= "{PATH}" obj= "{DOMAIN}\{USER ACCOUNT}" password= "{PASSWORD}"
   ```

   * <span data-ttu-id="b6d97-147">`{SERVICE NAME}` &ndash; Nome da assegnare al servizio in [Gestione controllo servizi](/windows/desktop/services/service-control-manager).</span><span class="sxs-lookup"><span data-stu-id="b6d97-147">`{SERVICE NAME}` &ndash; The name to assign to the service in [Service Control Manager](/windows/desktop/services/service-control-manager).</span></span>
   * <span data-ttu-id="b6d97-148">`{PATH}` &ndash; Percorso dell'eseguibile del servizio.</span><span class="sxs-lookup"><span data-stu-id="b6d97-148">`{PATH}` &ndash; The path to the service executable.</span></span>
   * <span data-ttu-id="b6d97-149">`{DOMAIN}` (o se il computer non è aggiunto a un dominio, il nome del computer locale) e `{USER ACCOUNT}` &ndash; il dominio (o nome del computer locale) e account utente con cui viene eseguito il servizio.</span><span class="sxs-lookup"><span data-stu-id="b6d97-149">`{DOMAIN}` (or if the machine isn't domain joined, the local machine name) and `{USER ACCOUNT}` &ndash; The domain (or local machine name) and user account under which the service runs.</span></span> <span data-ttu-id="b6d97-150">**Non** omettere il parametro `obj`.</span><span class="sxs-lookup"><span data-stu-id="b6d97-150">Do **not** omit the `obj` parameter.</span></span> <span data-ttu-id="b6d97-151">Il valore predefinito per `obj` è l'account [LocalSystem](/windows/desktop/services/localsystem-account).</span><span class="sxs-lookup"><span data-stu-id="b6d97-151">The default value for `obj` is the [LocalSystem account](/windows/desktop/services/localsystem-account) account.</span></span> <span data-ttu-id="b6d97-152">L'esecuzione di un servizio nel contesto dell'account `LocalSystem` presenta rischi significativi per la sicurezza.</span><span class="sxs-lookup"><span data-stu-id="b6d97-152">Running a service under the `LocalSystem` account presents a significant security risk.</span></span> <span data-ttu-id="b6d97-153">Eseguire sempre un servizio con un account utente con privilegi limitati nel server.</span><span class="sxs-lookup"><span data-stu-id="b6d97-153">Always run a service under a user account with restricted privileges on the server.</span></span>
   * <span data-ttu-id="b6d97-154">`{PASSWORD}` &ndash; Password dell'account utente.</span><span class="sxs-lookup"><span data-stu-id="b6d97-154">`{PASSWORD}` &ndash; The user account password.</span></span>

   <span data-ttu-id="b6d97-155">Nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="b6d97-155">In the following example:</span></span>

   * <span data-ttu-id="b6d97-156">Il servizio è denominato **MyService**.</span><span class="sxs-lookup"><span data-stu-id="b6d97-156">The service is named **MyService**.</span></span>
   * <span data-ttu-id="b6d97-157">Il servizio pubblicato risiede nella cartella *c:\\svc*.</span><span class="sxs-lookup"><span data-stu-id="b6d97-157">The published service resides in the *c:\\svc* folder.</span></span> <span data-ttu-id="b6d97-158">L'eseguibile dell'app è denominato *AspNetCoreService.exe*.</span><span class="sxs-lookup"><span data-stu-id="b6d97-158">The app executable is named *AspNetCoreService.exe*.</span></span> <span data-ttu-id="b6d97-159">Il valore `binPath` è racchiuso tra virgolette semplici (").</span><span class="sxs-lookup"><span data-stu-id="b6d97-159">The `binPath` value is enclosed in straight quotation marks (").</span></span>
   * <span data-ttu-id="b6d97-160">Il servizio viene eseguito nel contesto dell'account `ServiceUser`.</span><span class="sxs-lookup"><span data-stu-id="b6d97-160">The service runs under the `ServiceUser` account.</span></span> <span data-ttu-id="b6d97-161">Sostituire `{DOMAIN}` con il dominio dell'account utente o il nome del computer locale.</span><span class="sxs-lookup"><span data-stu-id="b6d97-161">Replace `{DOMAIN}` with the user account's domain or local machine name.</span></span> <span data-ttu-id="b6d97-162">Racchiudere il valore `obj` tra virgolette semplici (").</span><span class="sxs-lookup"><span data-stu-id="b6d97-162">Enclose the `obj` value in straight quotation marks (").</span></span> <span data-ttu-id="b6d97-163">Esempio: se il sistema host è un computer locale denominato `MairaPC`, impostare `obj` su `"MairaPC\ServiceUser"`.</span><span class="sxs-lookup"><span data-stu-id="b6d97-163">Example: If the hosting system is a local machine named `MairaPC`, set `obj` to `"MairaPC\ServiceUser"`.</span></span>
   * <span data-ttu-id="b6d97-164">Sostituire `{PASSWORD}` con la password dell'account utente.</span><span class="sxs-lookup"><span data-stu-id="b6d97-164">Replace `{PASSWORD}` with the user account's password.</span></span> <span data-ttu-id="b6d97-165">Il valore `password` è racchiuso tra virgolette semplici (").</span><span class="sxs-lookup"><span data-stu-id="b6d97-165">The `password` value is enclosed in straight quotation marks (").</span></span>

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe" obj= "{DOMAIN}\ServiceUser" password= "{PASSWORD}"
   ```

   > [!IMPORTANT]
   > <span data-ttu-id="b6d97-166">Assicurarsi che siano presenti gli spazi tra i segni di uguale e i valori dei parametri.</span><span class="sxs-lookup"><span data-stu-id="b6d97-166">Make sure that the spaces between the parameters' equal signs and the parameters' values are present.</span></span>

1. <span data-ttu-id="b6d97-167">Avviare il servizio con il comando `sc start {SERVICE NAME}`.</span><span class="sxs-lookup"><span data-stu-id="b6d97-167">Start the service with the `sc start {SERVICE NAME}` command.</span></span>

   <span data-ttu-id="b6d97-168">Per avviare il servizio app di esempio, usare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="b6d97-168">To start the sample app service, use the following command:</span></span>

   ```console
   sc start MyService
   ```

   <span data-ttu-id="b6d97-169">L'avvio del servizio richiede alcuni secondi.</span><span class="sxs-lookup"><span data-stu-id="b6d97-169">The command takes a few seconds to start the service.</span></span>

1. <span data-ttu-id="b6d97-170">Per verificare lo stato del servizio, usare il comando `sc query {SERVICE NAME}`.</span><span class="sxs-lookup"><span data-stu-id="b6d97-170">To check the status of the service, use the `sc query {SERVICE NAME}` command.</span></span> <span data-ttu-id="b6d97-171">Lo stato viene indicato con uno dei valori seguenti:</span><span class="sxs-lookup"><span data-stu-id="b6d97-171">The status is reported as one of the following values:</span></span>

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   <span data-ttu-id="b6d97-172">Usare il comando seguente per verificare lo stato del servizio app di esempio:</span><span class="sxs-lookup"><span data-stu-id="b6d97-172">Use the following command to check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

1. <span data-ttu-id="b6d97-173">Quando il servizio è nello stato `RUNNING` e se il servizio è un'app Web, selezionare l'app dal suo percorso (per impostazione predefinita, `http://localhost:5000`, che reindirizza a `https://localhost:5001` quando si utilizza [il middleware di reindirizzamento HTTPS](xref:security/enforcing-ssl)).</span><span class="sxs-lookup"><span data-stu-id="b6d97-173">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

   <span data-ttu-id="b6d97-174">Per il servizio app di esempio, selezionare l'app in `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="b6d97-174">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

1. <span data-ttu-id="b6d97-175">Arrestare il servizio con il comando `sc stop {SERVICE NAME}`.</span><span class="sxs-lookup"><span data-stu-id="b6d97-175">Stop the service with the `sc stop {SERVICE NAME}` command.</span></span>

   <span data-ttu-id="b6d97-176">Per arrestare il servizio app di esempio, usare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="b6d97-176">The following command stops the sample app service:</span></span>

   ```console
   sc stop MyService
   ```

1. <span data-ttu-id="b6d97-177">Dopo il breve lasso di tempo necessario per arrestare il servizio, disinstallare il servizio con il comando `sc delete {SERVICE NAME}`.</span><span class="sxs-lookup"><span data-stu-id="b6d97-177">After a short delay to stop a service, uninstall the service with the `sc delete {SERVICE NAME}` command.</span></span>

   <span data-ttu-id="b6d97-178">Verificare lo stato del servizio app di esempio:</span><span class="sxs-lookup"><span data-stu-id="b6d97-178">Check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

   <span data-ttu-id="b6d97-179">Quando il servizio app di esempio è nello stato `STOPPED`, usare il comando seguente per disinstallare il servizio app di esempio:</span><span class="sxs-lookup"><span data-stu-id="b6d97-179">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

   ```console
   sc delete MyService
   ```

## <a name="run-the-app-outside-of-a-service"></a><span data-ttu-id="b6d97-180">Eseguire l'app fuori da un servizio</span><span class="sxs-lookup"><span data-stu-id="b6d97-180">Run the app outside of a service</span></span>

<span data-ttu-id="b6d97-181">È più semplice testare ed eseguire il debug durante l'esecuzione all'esterno di un servizio, pertanto può essere utile aggiungere codice che chiama `RunAsService` solo in determinate condizioni.</span><span class="sxs-lookup"><span data-stu-id="b6d97-181">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="b6d97-182">Ad esempio, l'app può essere eseguita come un'app console con un argomento della riga di comando `--console` o se il debugger è collegato:</span><span class="sxs-lookup"><span data-stu-id="b6d97-182">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

<span data-ttu-id="b6d97-183">Poiché la configurazione di ASP.NET Core richiede coppie nome-valore per gli argomenti della riga di comando, l'opzione `--console` viene rimossa prima che gli argomenti vengono passati a [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span><span class="sxs-lookup"><span data-stu-id="b6d97-183">Because ASP.NET Core configuration requires name-value pairs for command-line arguments, the `--console` switch is removed before the arguments are passed to [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

> [!NOTE]
> <span data-ttu-id="b6d97-184">`isService` non viene passato da `Main` in `CreateWebHostBuilder` perché la firma di `CreateWebHostBuilder` deve essere `CreateWebHostBuilder(string[])` per garantire il corretto funzionamento dei [test di integrazione](xref:test/integration-tests).</span><span class="sxs-lookup"><span data-stu-id="b6d97-184">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="b6d97-185">Gestire gli eventi di arresto e avvio</span><span class="sxs-lookup"><span data-stu-id="b6d97-185">Handle stopping and starting events</span></span>

<span data-ttu-id="b6d97-186">Per gestire gli eventi [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted) e [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping), apportare le modifiche aggiuntive seguenti:</span><span class="sxs-lookup"><span data-stu-id="b6d97-186">To handle [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), and [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) events, make the following additional changes:</span></span>

1. <span data-ttu-id="b6d97-187">Creare una classe che derivi dalla classe [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span><span class="sxs-lookup"><span data-stu-id="b6d97-187">Create a class that derives from [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="b6d97-188">Creare un metodo di estensione per [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) che passi l'oggetto `WebHostService` personalizzato a [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span><span class="sxs-lookup"><span data-stu-id="b6d97-188">Create an extension method for [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) that passes the custom `WebHostService` to [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="b6d97-189">In `Program.Main` chiamare il nuovo metodo di estensione, `RunAsCustomService`, anziché [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span><span class="sxs-lookup"><span data-stu-id="b6d97-189">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=17)]

   > [!NOTE]
   > <span data-ttu-id="b6d97-190">`isService` non viene passato da `Main` in `CreateWebHostBuilder` perché la firma di `CreateWebHostBuilder` deve essere `CreateWebHostBuilder(string[])` per garantire il corretto funzionamento dei [test di integrazione](xref:test/integration-tests).</span><span class="sxs-lookup"><span data-stu-id="b6d97-190">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

<span data-ttu-id="b6d97-191">Se il codice `WebHostService` personalizzato richiede un servizio dall'inserimento di dipendenze, ad esempio un logger, ottenerlo dalla proprietà [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services):</span><span class="sxs-lookup"><span data-stu-id="b6d97-191">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) property:</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=Logging&highlight=7-8)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="b6d97-192">Scenari con server proxy e servizi di bilanciamento del carico</span><span class="sxs-lookup"><span data-stu-id="b6d97-192">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="b6d97-193">I servizi che interagiscono con le richieste da Internet o da una rete aziendale e si trovano dietro un proxy o un servizio di bilanciamento del carico potrebbero richiedere una configurazione aggiuntiva.</span><span class="sxs-lookup"><span data-stu-id="b6d97-193">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="b6d97-194">Per ulteriori informazioni, vedere <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="b6d97-194">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-https"></a><span data-ttu-id="b6d97-195">Configurare HTTPS</span><span class="sxs-lookup"><span data-stu-id="b6d97-195">Configure HTTPS</span></span>

<span data-ttu-id="b6d97-196">Per configurare il servizio con un endpoint sicuro:</span><span class="sxs-lookup"><span data-stu-id="b6d97-196">To configure the service with a secure endpoint:</span></span>

1. <span data-ttu-id="b6d97-197">Creare un certificato X.509 per il sistema di hosting usando i meccanismi di acquisizione e distribuzione dei certificati della piattaforma.</span><span class="sxs-lookup"><span data-stu-id="b6d97-197">Create an X.509 certificate for the hosting system using your platform's certificate acquisition and deployment mechanisms.</span></span>
1. <span data-ttu-id="b6d97-198">Specificare una [configurazione dell'endpoint HTTPS del server Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) per usare il certificato.</span><span class="sxs-lookup"><span data-stu-id="b6d97-198">Specify a [Kestrel server HTTPS endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) to use the certificate.</span></span>

<span data-ttu-id="b6d97-199">L'uso del certificato di sviluppo ASP.NET Core HTTPS per proteggere un endpoint del servizio non è supportato.</span><span class="sxs-lookup"><span data-stu-id="b6d97-199">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="b6d97-200">Directory corrente e radice del contenuto</span><span class="sxs-lookup"><span data-stu-id="b6d97-200">Current directory and content root</span></span>

<span data-ttu-id="b6d97-201">La directory di lavoro corrente restituita chiamando `Directory.GetCurrentDirectory()` per un servizio Windows è la cartella *C:\\WINDOWS\\system32*.</span><span class="sxs-lookup"><span data-stu-id="b6d97-201">The current working directory returned by calling `Directory.GetCurrentDirectory()` for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="b6d97-202">La cartella *system32* non è un percorso appropriato per archiviare i file di un servizio, ad esempio i file di impostazioni.</span><span class="sxs-lookup"><span data-stu-id="b6d97-202">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="b6d97-203">Usare uno degli approcci seguenti per gestire e accedere alle risorse e ai file di impostazioni di un servizio con [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) quando si usa un'interfaccia [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder):</span><span class="sxs-lookup"><span data-stu-id="b6d97-203">Use one of the following approaches to maintain and access a service's assets and settings files with [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) when using an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder):</span></span>

* <span data-ttu-id="b6d97-204">Usare il percorso radice del contenuto.</span><span class="sxs-lookup"><span data-stu-id="b6d97-204">Use the content root path.</span></span> <span data-ttu-id="b6d97-205">L'elemento `IHostingEnvironment.ContentRootPath` è lo stesso percorso fornito all'argomento `binPath` durante la creazione del servizio.</span><span class="sxs-lookup"><span data-stu-id="b6d97-205">The `IHostingEnvironment.ContentRootPath` is the same path provided to the `binPath` argument when the service is created.</span></span> <span data-ttu-id="b6d97-206">Invece di usare `Directory.GetCurrentDirectory()` per creare i percorsi dei file di impostazioni, usare il percorso radice del contenuto e mantenere i file nella radice del contenuto dell'app.</span><span class="sxs-lookup"><span data-stu-id="b6d97-206">Instead of using `Directory.GetCurrentDirectory()` to create paths to settings files, use the content root path and maintain the files in the app's content root.</span></span>
* <span data-ttu-id="b6d97-207">Archiviare i file in un percorso appropriato nel disco.</span><span class="sxs-lookup"><span data-stu-id="b6d97-207">Store the files in a suitable location on disk.</span></span> <span data-ttu-id="b6d97-208">Specificare un percorso assoluto con `SetBasePath` alla cartella contenente i file.</span><span class="sxs-lookup"><span data-stu-id="b6d97-208">Specify an absolute path with `SetBasePath` to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b6d97-209">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="b6d97-209">Additional resources</span></span>

* <span data-ttu-id="b6d97-210">[Configurazione dell'endpoint Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (include la configurazione HTTPS e il supporto SNI)</span><span class="sxs-lookup"><span data-stu-id="b6d97-210">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
