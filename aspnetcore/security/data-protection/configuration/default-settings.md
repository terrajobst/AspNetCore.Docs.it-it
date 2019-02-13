---
title: Gestione delle chiavi protezione dati e la durata in ASP.NET Core
author: rick-anderson
description: Informazioni sulla gestione delle chiavi la protezione dei dati e la durata in ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/configuration/default-settings
ms.openlocfilehash: 2f022a4c7519485fe629ce47c27d214c8c27d5bc
ms.sourcegitcommit: af8a6eb5375ef547a52ffae22465e265837aa82b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/12/2019
ms.locfileid: "56159211"
---
# <a name="data-protection-key-management-and-lifetime-in-aspnet-core"></a>Gestione delle chiavi protezione dati e la durata in ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="key-management"></a>Gestione delle chiavi

L'app prova a rilevare proprio ambiente operativo e gestire configurazione della chiave autonomamente.

1. Se l'app è ospitata [app di Azure](https://azure.microsoft.com/services/app-service/), le chiavi vengono mantenute per il *%HOME%\ASP.NET\DataProtection-Keys* cartella. La cartella è associata all'archiviazione di rete e sincronizzata in tutti i computer che ospitano l'app.
   * Le chiavi non vengono protette quando sono inattive.
   * Il *DataProtection-Keys* cartella offre il Keyring a tutte le istanze di un'app in un singolo slot di distribuzione.
   * Gli slot di distribuzione separati, ad esempio gli slot di gestione temporanea e di produzione, non condividono un KeyRing. Durante lo scambio tra gli slot di distribuzione, ad esempio lo scambio alla produzione oppure usando un test / B, qualsiasi app usando la protezione dei dati sarà in grado di decrittografare i dati archiviati usando il Keyring all'interno di slot precedente. Di conseguenza gli utenti registrati da un'app che usa l'autenticazione dei cookie ASP.NET Core standard, in quanto utilizza la protezione dei dati per proteggere i cookie. Se lo si desiderano anelli chiave indipendente dallo slot, usare un provider di Keyring esterno, ad esempio archiviazione di Blob di Azure, Azure Key Vault, un archivio, SQL o della cache Redis.

1. Se il profilo utente è disponibile, le chiavi vengono rese persistenti per il *%LOCALAPPDATA%\ASP.NET\DataProtection-Keys* cartella. Se il sistema operativo è Windows, le chiavi vengono crittografate a riposo tramite DPAPI.

   Il pool di applicazioni [setProfileEnvironment attributo](/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration) deve anche essere abilitato. Il valore predefinito di `setProfileEnvironment` è `true`. In alcuni scenari (ad esempio, Windows del sistema operativo), `setProfileEnvironment` è impostata su `false`. Se le chiavi non vengono archiviate nella directory del profilo utente come previsto:

   1. Passare il *%windir%/system32/inetsrv/config* cartella.
   1. Aprire il *applicationHost. config* file.
   1. Individuare l'elemento `<system.applicationHost><applicationPools><applicationPoolDefaults><processModel>` .
   1. Verificare che il `setProfileEnvironment` attributo non è presente, che per impostazione predefinita il valore per `true`, o impostare in modo esplicito il valore dell'attributo `true`.

1. Se l'app è ospitata in IIS, le chiavi vengono mantenute nel Registro di sistema HKLM in una chiave del Registro di sistema speciale che acled solo per l'account di processo di lavoro. Le chiavi vengono crittografate a riposo tramite la DPAPI.

1. Se nessuna di queste condizioni corrispondono, le chiavi non sono persistenti all'esterno del processo corrente. Quando il processo viene arrestato, tutti generate le chiavi vengono perse.

Lo sviluppatore è sempre il controllo completo e può eseguire l'override come e dove le chiavi vengono archiviate. Le prime tre opzioni riportate sopra devono fornire valori predefiniti idonei per la maggior parte delle App simili al modo in cui in ASP.NET  **\<machineKey >** routine di generazione automatica ha lavorato in passato. L'opzione di fallback, finale è l'unico scenario che richiede allo sviluppatore di specificare [configurazione](xref:security/data-protection/configuration/overview) iniziale se desiderano persistenza chiave, ma questo fallback si verifica solo in rare situazioni.

Quando si ospita in un contenitore Docker, le chiavi devono essere mantenute in una cartella che è un volume di Docker (un volume condiviso o un volume montato host che persiste oltre la durata del contenitore) o in un provider esterno, ad esempio [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) oppure [Redis](https://redis.io/). Un provider esterno è utile in scenari web farm anche se le app non possono accedere a un volume di rete condivisa (vedere [PersistKeysToFileSystem](xref:security/data-protection/configuration/overview#persistkeystofilesystem) per altre informazioni).

> [!WARNING]
> Se lo sviluppatore esegue l'override di regole illustrate in precedenza e punti nel sistema di protezione dei dati in un repository di chiave specifico, viene disabilitata la crittografia automatica delle chiavi a riposo. Protezione dei dati inattivi può essere riabilitata tramite [configurazione](xref:security/data-protection/configuration/overview).

## <a name="key-lifetime"></a>Durata delle chiavi

Le chiavi hanno una durata di 90 giorni per impostazione predefinita. Quando una chiave scade, l'app genera una nuova chiave automaticamente e imposta la nuova chiave come chiave attiva. Finché le chiavi ritirate rimangono nel sistema, l'app può decrittografare tutti i dati protetti con essi. Visualizzare [gestione delle chiavi](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling) per altre informazioni.

## <a name="default-algorithms"></a>Algoritmi predefiniti

L'algoritmo di protezione dati di payload predefinito utilizzato è AES-256-CBC per la riservatezza e HMACSHA256 per autenticità. Una chiave master di 512 bit, modificata ogni 90 giorni, viene usata per derivare le due chiavi secondarie utilizzate per questi algoritmi in base al payload. Visualizzare [sottochiave derivazione](xref:security/data-protection/implementation/subkeyderivation#additional-authenticated-data-and-subkey-derivation) per altre informazioni.

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:security/data-protection/extensibility/key-management>
* <xref:host-and-deploy/web-farm>
