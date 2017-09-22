---
title: Sostituire < machineKey > in ASP.NET
author: rick-anderson
description: Sostituire < machineKey > in ASP.NET
keywords: ASP.NET Core, sicurezza, < machineKey >, machineKey
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 5ac13589-3837-4b4d-8abe-81f843942120
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/compatibility/replacing-machinekey
ms.openlocfilehash: 8c00c05a1120e65f503b70229466fcad561bc6a9
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/22/2017
---
# <a name="replacing-machinekey-in-aspnet"></a>Sostituzione di `<machineKey>` in ASP.NET

<a name=compatibility-replacing-machinekey></a>

L'implementazione del `<machineKey>` elemento ASP.NET [sostituibile](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/). In questo modo la maggior parte delle chiamate alle routine di crittografia di ASP.NET essere instradata attraverso un meccanismo di protezione dei dati di sostituzione, tra cui il nuovo sistema di protezione dati.

## <a name="package-installation"></a>Installazione del pacchetto

> [!NOTE]
> Il nuovo sistema di protezione dati può solo essere installato in un'applicazione ASP.NET esistente come destinazione .NET 4.5.1 o versione successiva. L'installazione verrà esito negativo se l'applicazione è destinata a .NET 4.5 o inferiore.

Per installare il nuovo sistema di protezione dati in un progetto 4.5.1+ ASP.NET esistente, installare il pacchetto Microsoft.AspNetCore.DataProtection.SystemWeb. Questo crea un'istanza di sistema di protezione dati mediante il [configurazione predefinita](../configuration/default-settings.md#data-protection-default-settings) impostazioni.

Quando si installa il pacchetto, inserita una riga in *Web. config* che indica ad ASP.NET di usarlo per [più operazioni di crittografia](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/), inclusi autenticazione basata su form, lo stato di visualizzazione e le chiamate a MachineKey.Protect. La riga di inserimento è simile alla seguente.

```xml
<machineKey compatibilityMode="Framework45" dataProtectorType="..." />
```

>[!TIP]
> È possibile stabilire se il nuovo sistema di protezione dati è attivo controllando campi quali `__VIEWSTATE`, che deve iniziare con "CfDJ8" come nell'esempio seguente. "CfDJ8" è la rappresentazione base64 dell'intestazione magic "09 F0 C9 F0" che identifica un payload protetto dal sistema di protezione dati.

```html
<input type="hidden" name="__VIEWSTATE" id="__VIEWSTATE" value="CfDJ8AWPr2EQPTBGs3L2GCZOpk..." />
```

## <a name="package-configuration"></a>Configurazione del pacchetto

Il sistema di protezione dati viene creato con una configurazione predefinita per l'installazione di zero. Tuttavia, poiché per impostazione predefinita le chiavi vengono rese persistenti nel file system locale, non funzionerà per le applicazioni che vengono distribuite in una farm. Per risolvere questo problema, è possibile specificare la configurazione tramite la creazione di un tipo che crea una sottoclasse DataProtectionStartup ed esegue l'override di metodo ConfigureServices.

Di seguito è riportato un esempio di un tipo di avvio di protezione dati personalizzati che è configurato sia in cui le chiavi sono persistenti e come essi sta crittografati a riposo. Esegue l'override anche i criteri di isolamento predefinito app fornendo il proprio nome dell'applicazione.

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
> È inoltre possibile utilizzare `<machineKey applicationName="my-app" ... />` al posto di una chiamata esplicita a SetApplicationName. Si tratta di un meccanismo utile per evitare di forzare lo sviluppatore di creare un tipo derivato da DataProtectionStartup se il nome dell'applicazione tutti volevano per configurare l'impostazione.

Per abilitare questa configurazione personalizzata, tornare al file Web. config e cercare il `<appSettings>` elemento che consentono di installare il pacchetto aggiunto al file di configurazione. Sarà simile al seguente il markup seguente:

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

Inserire il valore vuoto con il nome completo dell'assembly del tipo derivato DataProtectionStartup che appena creato. Se il nome dell'applicazione è DataProtectionDemo, questo sarà simile di seguito.

```xml
<add key="aspnet:dataProtectionStartupType"
     value="DataProtectionDemo.MyDataProtectionStartup, DataProtectionDemo" />
```

Il sistema di protezione dati appena configurato è ora pronto per l'utilizzo all'interno dell'applicazione.
