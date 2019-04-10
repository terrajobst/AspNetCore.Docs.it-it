---
title: Sostituire l'elemento machineKey ASP.NET in ASP.NET Core
author: rick-anderson
description: Informazioni su come sostituire machineKey di ASP.NET per consentire l'uso di un sistema di protezione dati nuovi e più sicuro.
ms.author: riande
ms.date: 04/06/2019
uid: security/data-protection/compatibility/replacing-machinekey
ms.openlocfilehash: 2317cb50cfe63226baf336ebfc5d681d1cebe5c6
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/10/2019
ms.locfileid: "59468566"
---
# <a name="replace-the-aspnet-machinekey-in-aspnet-core"></a><span data-ttu-id="c3d21-103">Sostituire l'elemento machineKey ASP.NET in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c3d21-103">Replace the ASP.NET machineKey in ASP.NET Core</span></span>

<a name="compatibility-replacing-machinekey"></a>

<span data-ttu-id="c3d21-104">L'implementazione del `<machineKey>` elemento ASP.NET [sostituibile](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/).</span><span class="sxs-lookup"><span data-stu-id="c3d21-104">The implementation of the `<machineKey>` element in ASP.NET [is replaceable](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/).</span></span> <span data-ttu-id="c3d21-105">In questo modo la maggior parte delle chiamate alle routine di crittografia ASP.NET devono essere instradati attraverso un meccanismo di protezione dei dati di sostituzione, tra cui il nuovo sistema di protezione dati.</span><span class="sxs-lookup"><span data-stu-id="c3d21-105">This allows most calls to ASP.NET cryptographic routines to be routed through a replacement data protection mechanism, including the new data protection system.</span></span>

## <a name="package-installation"></a><span data-ttu-id="c3d21-106">Installazione del pacchetto</span><span class="sxs-lookup"><span data-stu-id="c3d21-106">Package installation</span></span>

> [!NOTE]
> <span data-ttu-id="c3d21-107">Il nuovo sistema di protezione dei dati può essere solo installato in un'applicazione ASP.NET esistente destinate a .NET 4.5.1 o versioni successive.</span><span class="sxs-lookup"><span data-stu-id="c3d21-107">The new data protection system can only be installed into an existing ASP.NET application targeting .NET 4.5.1 or later.</span></span> <span data-ttu-id="c3d21-108">Installazione verrà esito negativo se l'applicazione è destinata a .NET 4.5 o in basso.</span><span class="sxs-lookup"><span data-stu-id="c3d21-108">Installation will fail if the application targets .NET 4.5 or lower.</span></span>

<span data-ttu-id="c3d21-109">Per installare il nuovo sistema di protezione dei dati in un progetto di 4.5.1+ ASP.NET esistente, installare il pacchetto Microsoft.AspNetCore.DataProtection.SystemWeb.</span><span class="sxs-lookup"><span data-stu-id="c3d21-109">To install the new data protection system into an existing ASP.NET 4.5.1+ project, install the package Microsoft.AspNetCore.DataProtection.SystemWeb.</span></span> <span data-ttu-id="c3d21-110">Si creerà un'istanza del sistema di protezione dati i dati tramite il [configurazione predefinita](xref:security/data-protection/configuration/default-settings) impostazioni.</span><span class="sxs-lookup"><span data-stu-id="c3d21-110">This will instantiate the data protection system using the [default configuration](xref:security/data-protection/configuration/default-settings) settings.</span></span>

<span data-ttu-id="c3d21-111">Quando si installa il pacchetto, inserita una riga in *Web. config* ASP.NET da utilizzare per comunicare [più operazioni di crittografia](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/), tra cui l'autenticazione basata su form, lo stato di visualizzazione e le chiamate a MachineKey. Protect.</span><span class="sxs-lookup"><span data-stu-id="c3d21-111">When you install the package, it inserts a line into *Web.config* that tells ASP.NET to use it for [most cryptographic operations](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/), including forms authentication, view state, and calls to MachineKey.Protect.</span></span> <span data-ttu-id="c3d21-112">La riga inserita legge come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="c3d21-112">The line that's inserted reads as follows.</span></span>

```xml
<machineKey compatibilityMode="Framework45" dataProtectorType="..." />
```

>[!TIP]
> <span data-ttu-id="c3d21-113">È possibile indicare se il nuovo sistema di protezione dati è attivo controllando i campi, ad esempio `__VIEWSTATE`, che deve iniziare con "CfDJ8" come nell'esempio seguente.</span><span class="sxs-lookup"><span data-stu-id="c3d21-113">You can tell if the new data protection system is active by inspecting fields like `__VIEWSTATE`, which should begin with "CfDJ8" as in the example below.</span></span> <span data-ttu-id="c3d21-114">"CfDJ8" è la rappresentazione base64 dell'intestazione magic "09 F0 C9 F0" che identifica un payload protetto dal sistema di protezione dati.</span><span class="sxs-lookup"><span data-stu-id="c3d21-114">"CfDJ8" is the base64 representation of the magic "09 F0 C9 F0" header that identifies a payload protected by the data protection system.</span></span>

```html
<input type="hidden" name="__VIEWSTATE" id="__VIEWSTATE" value="CfDJ8AWPr2EQPTBGs3L2GCZOpk...">
```

## <a name="package-configuration"></a><span data-ttu-id="c3d21-115">configurazione del pacchetto</span><span class="sxs-lookup"><span data-stu-id="c3d21-115">Package configuration</span></span>

<span data-ttu-id="c3d21-116">Il sistema di protezione dati viene creata un'istanza con una configurazione di zero-programma di installazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="c3d21-116">The data protection system is instantiated with a default zero-setup configuration.</span></span> <span data-ttu-id="c3d21-117">Tuttavia, poiché per impostazione predefinita le chiavi vengono rese persistenti nel file system locale, questo non funzionerà per le applicazioni che vengono distribuite in una farm.</span><span class="sxs-lookup"><span data-stu-id="c3d21-117">However, since by default keys are persisted to the local file system, this won't work for applications which are deployed in a farm.</span></span> <span data-ttu-id="c3d21-118">Per risolvere questo problema, è possibile fornire configurazione creando un tipo che crea una sottoclasse DataProtectionStartup ed esegue l'override relativo metodo ConfigureServices.</span><span class="sxs-lookup"><span data-stu-id="c3d21-118">To resolve this, you can provide configuration by creating a type which subclasses DataProtectionStartup and overrides its ConfigureServices method.</span></span>

<span data-ttu-id="c3d21-119">Di seguito è riportato un esempio di un tipo di avvio di protezione dati personalizzati che configurato sia in cui vengono salvati in modo permanente le chiavi e modo in cui si sta crittografati a riposo.</span><span class="sxs-lookup"><span data-stu-id="c3d21-119">Below is an example of a custom data protection startup type which configured both where keys are persisted and how they're encrypted at rest.</span></span> <span data-ttu-id="c3d21-120">Esegue l'override anche i criteri di isolamento di app predefinito, fornendo il proprio nome dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c3d21-120">It also overrides the default app isolation policy by providing its own application name.</span></span>

```csharp
using System;
using System.IO;
using Microsoft.AspNetCore.DataProtection;
using Microsoft.AspNetCore.DataProtection.SystemWeb;
using Microsoft.Extensions.DependencyInjection;

namespace DataProtectionDemo
{
    public class MyDataProtectionStartup : DataProtectionStartup
    {
        public override void ConfigureServices(IServiceCollection services)
        {
            services.AddDataProtection()
                .SetApplicationName("my-app")
                .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\myapp-keys\"))
                .ProtectKeysWithCertificate("thumbprint");
        }
    }
}
```

>[!TIP]
> <span data-ttu-id="c3d21-121">È anche possibile usare `<machineKey applicationName="my-app" ... />` al posto di una chiamata esplicita a SetApplicationName.</span><span class="sxs-lookup"><span data-stu-id="c3d21-121">You can also use `<machineKey applicationName="my-app" ... />` in place of an explicit call to SetApplicationName.</span></span> <span data-ttu-id="c3d21-122">Si tratta di un meccanismo utile per evitare di forzare lo sviluppatore a creare un tipo derivato DataProtectionStartup se tutto è quindi necessario configurare l'impostazione del nome dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c3d21-122">This is a convenience mechanism to avoid forcing the developer to create a DataProtectionStartup-derived type if all they wanted to configure was setting the application name.</span></span>

<span data-ttu-id="c3d21-123">Per abilitare questa configurazione personalizzata, tornare al file Web. config e cercare il `<appSettings>` elemento che installano il pacchetto aggiunto al file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="c3d21-123">To enable this custom configuration, go back to Web.config and look for the `<appSettings>` element that the package install added to the config file.</span></span> <span data-ttu-id="c3d21-124">Sarà simile al markup seguente:</span><span class="sxs-lookup"><span data-stu-id="c3d21-124">It will look like the following markup:</span></span>

```xml
<appSettings>
  <!--
  If you want to customize the behavior of the ASP.NET Core Data Protection stack, set the
  "aspnet:dataProtectionStartupType" switch below to be the fully-qualified name of a
  type which subclasses Microsoft.AspNetCore.DataProtection.SystemWeb.DataProtectionStartup.
  -->
  <add key="aspnet:dataProtectionStartupType" value="" />
</appSettings>
```

<span data-ttu-id="c3d21-125">Immettere il valore vuoto con il nome qualificato dall'assembly del tipo derivato DataProtectionStartup che appena creato.</span><span class="sxs-lookup"><span data-stu-id="c3d21-125">Fill in the blank value with the assembly-qualified name of the DataProtectionStartup-derived type you just created.</span></span> <span data-ttu-id="c3d21-126">Se il nome dell'applicazione è DataProtectionDemo, riportato di seguito.</span><span class="sxs-lookup"><span data-stu-id="c3d21-126">If the name of the application is DataProtectionDemo, this would look like the below.</span></span>

```xml
<add key="aspnet:dataProtectionStartupType"
     value="DataProtectionDemo.MyDataProtectionStartup, DataProtectionDemo" />
```

<span data-ttu-id="c3d21-127">Il sistema di protezione dati appena configurato è ora pronto per l'utilizzo all'interno dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c3d21-127">The newly-configured data protection system is now ready for use inside the application.</span></span>
