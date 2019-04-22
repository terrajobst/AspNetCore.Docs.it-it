---
title: Sostituire l'elemento machineKey ASP.NET in ASP.NET Core
author: rick-anderson
description: Informazioni su come sostituire machineKey di ASP.NET per consentire l'uso di un sistema di protezione dati nuovi e più sicuro.
ms.author: riande
ms.date: 04/06/2019
uid: security/data-protection/compatibility/replacing-machinekey
ms.openlocfilehash: 2317cb50cfe63226baf336ebfc5d681d1cebe5c6
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59705549"
---
# <a name="replace-the-aspnet-machinekey-in-aspnet-core"></a>Sostituire l'elemento machineKey ASP.NET in ASP.NET Core

<a name="compatibility-replacing-machinekey"></a>

L'implementazione del `<machineKey>` elemento ASP.NET [sostituibile](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/). In questo modo la maggior parte delle chiamate alle routine di crittografia ASP.NET devono essere instradati attraverso un meccanismo di protezione dei dati di sostituzione, tra cui il nuovo sistema di protezione dati.

## <a name="package-installation"></a>Installazione del pacchetto

> [!NOTE]
> Il nuovo sistema di protezione dei dati può essere solo installato in un'applicazione ASP.NET esistente destinate a .NET 4.5.1 o versioni successive. Installazione verrà esito negativo se l'applicazione è destinata a .NET 4.5 o in basso.

Per installare il nuovo sistema di protezione dei dati in un progetto di 4.5.1+ ASP.NET esistente, installare il pacchetto Microsoft.AspNetCore.DataProtection.SystemWeb. Si creerà un'istanza del sistema di protezione dati i dati tramite il [configurazione predefinita](xref:security/data-protection/configuration/default-settings) impostazioni.

Quando si installa il pacchetto, inserita una riga in *Web. config* ASP.NET da utilizzare per comunicare [più operazioni di crittografia](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/), tra cui l'autenticazione basata su form, lo stato di visualizzazione e le chiamate a MachineKey. Protect. La riga inserita legge come indicato di seguito.

```xml
<machineKey compatibilityMode="Framework45" dataProtectorType="..." />
```

>[!TIP]
> È possibile indicare se il nuovo sistema di protezione dati è attivo controllando i campi, ad esempio `__VIEWSTATE`, che deve iniziare con "CfDJ8" come nell'esempio seguente. "CfDJ8" è la rappresentazione base64 dell'intestazione magic "09 F0 C9 F0" che identifica un payload protetto dal sistema di protezione dati.

```html
<input type="hidden" name="__VIEWSTATE" id="__VIEWSTATE" value="CfDJ8AWPr2EQPTBGs3L2GCZOpk...">
```

## <a name="package-configuration"></a>configurazione del pacchetto

Il sistema di protezione dati viene creata un'istanza con una configurazione di zero-programma di installazione predefinita. Tuttavia, poiché per impostazione predefinita le chiavi vengono rese persistenti nel file system locale, questo non funzionerà per le applicazioni che vengono distribuite in una farm. Per risolvere questo problema, è possibile fornire configurazione creando un tipo che crea una sottoclasse DataProtectionStartup ed esegue l'override relativo metodo ConfigureServices.

Di seguito è riportato un esempio di un tipo di avvio di protezione dati personalizzati che configurato sia in cui vengono salvati in modo permanente le chiavi e modo in cui si sta crittografati a riposo. Esegue l'override anche i criteri di isolamento di app predefinito, fornendo il proprio nome dell'applicazione.

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
> È anche possibile usare `<machineKey applicationName="my-app" ... />` al posto di una chiamata esplicita a SetApplicationName. Si tratta di un meccanismo utile per evitare di forzare lo sviluppatore a creare un tipo derivato DataProtectionStartup se tutto è quindi necessario configurare l'impostazione del nome dell'applicazione.

Per abilitare questa configurazione personalizzata, tornare al file Web. config e cercare il `<appSettings>` elemento che installano il pacchetto aggiunto al file di configurazione. Sarà simile al markup seguente:

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

Immettere il valore vuoto con il nome qualificato dall'assembly del tipo derivato DataProtectionStartup che appena creato. Se il nome dell'applicazione è DataProtectionDemo, riportato di seguito.

```xml
<add key="aspnet:dataProtectionStartupType"
     value="DataProtectionDemo.MyDataProtectionStartup, DataProtectionDemo" />
```

Il sistema di protezione dati appena configurato è ora pronto per l'utilizzo all'interno dell'applicazione.
