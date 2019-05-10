---
title: Panoramica sulla sicurezza di ASP.NET Core
author: tdykstra
description: Informazioni sui concetti di base di autenticazione, autorizzazione e sicurezza in ASP.NET Core.
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/24/2018
uid: security/index
ms.openlocfilehash: 933501411169d89c4b24edda743c47591aa7a87a
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/27/2019
ms.locfileid: "64881886"
---
# <a name="overview-of-aspnet-core-security"></a>Panoramica sulla sicurezza di ASP.NET Core

ASP.NET Core consente agli sviluppatori di configurare e gestire facilmente la sicurezza per le proprie app. ASP.NET Core contiene funzionalità per gestire l'autenticazione, l'autorizzazione, la protezione dei dati, l'applicazione di HTTPS, i segreti dell'app, la protezione dalla falsificazione delle richieste e la gestione di CORS. Queste funzionalità di sicurezza consentono di compilare app ASP.NET Core solide e sicure.

## <a name="aspnet-core-security-features"></a>Funzionalità di sicurezza di ASP.NET Core

ASP.NET Core fornisce numerosi strumenti e librerie per proteggere le app, inclusi i provider di identità predefiniti, ma è anche possibile usare servizi di identità di terze parti, ad esempio Facebook, Twitter e LinkedIn. Con ASP.NET Core, è possibile gestire facilmente i segreti dell'app, che consentono di archiviare e usare informazioni riservate senza la necessità di esporle nel codice.

## <a name="authentication-vs-authorization"></a>Autenticazione e Autorizzazione

L'autenticazione è un processo in cui un utente fornisce le credenziali che vengono quindi confrontate con quelle archiviate in un sistema operativo, un database, un'applicazione o una risorsa. Se corrispondono, gli utenti vengono autenticati correttamente e quindi possono eseguire azioni per cui sono autorizzati, durante un processo di autorizzazione. L'autorizzazione è il processo che determina quali operazioni può eseguire un utente.

È possibile considerare l'autenticazione come un modo per entrare in uno spazio, ad esempio un server, un database, un'app o una risorsa, mentre l'autorizzazione definisce le azioni che l'utente può eseguire e su quali oggetti all'interno di tale spazio (server, database o app).

## <a name="common-vulnerabilities-in-software"></a>Vulnerabilità comuni nel software

ASP.NET Core ed Entity Framework contengono funzionalità che consentono di proteggere le app e impedire violazioni della sicurezza. L'elenco di collegamenti seguente offre documentazione in cui sono descritte le tecniche per evitare le vulnerabilità della sicurezza più comuni nelle app Web:

* [Attacchi di tipo cross-site scripting (XSS)](xref:security/cross-site-scripting)
* [Attacchi SQL injection](/ef/core/querying/raw-sql)
* [Richieste intersito false](xref:security/anti-request-forgery)
* [Attacchi di reindirizzamento aperti](xref:security/preventing-open-redirects)

Esistono altre vulnerabilità di cui è necessario essere consapevoli. Per altre informazioni, vedere gli altri articoli della sezione **Sicurezza e identità** del sommario.
