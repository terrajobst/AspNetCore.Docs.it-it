---
title: Gestione della chiave di protezione dati e la durata in ASP.NET Core
author: rick-anderson
description: Informazioni sulla gestione delle chiavi protezione dei dati e la durata in ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/configuration/default-settings
ms.openlocfilehash: b43c14af015d5e03f46200c51a1218a581b1de0c
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
ms.locfileid: "28887290"
---
# <a name="data-protection-key-management-and-lifetime-in-aspnet-core"></a>Gestione della chiave di protezione dati e la durata in ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="key-management"></a>Gestione delle chiavi

L'applicazione tenta di rilevare l'ambiente operativo e gestire chiavi configurazione nel proprio.

1. Se l'applicazione è ospitata in [App Azure](https://azure.microsoft.com/services/app-service/), le chiavi vengono rese persistenti il *%HOME%\ASP.NET\DataProtection-Keys* cartella. Questa cartella viene supportata dall'archiviazione di rete e viene sincronizzata in tutti i computer che ospita l'app.
   * Le chiavi non sono protette inattivi.
   * Il *DataProtection chiavi* cartella fornisce la gestione delle chiavi a tutte le istanze di un'app in uno slot di distribuzione singolo.
   * Gli slot di distribuzione separato, ad esempio gestione temporanea e produzione, non condividono un anello di chiave. Durante lo scambio tra gli slot di distribuzione, ad esempio lo scambio di gestione temporanea nell'ambiente di produzione o utilizzando A / B test, qualsiasi app utilizzando la protezione dei dati non saranno in grado di decrittografare i dati archiviati tramite la gestione delle chiavi all'interno di slot precedente. Questo porta agli utenti viene registrati da un'app che utilizza l'autenticazione dei cookie ASP.NET Core standard, in quanto utilizza la protezione dei dati per proteggere i propri cookie. Se si desiderano anelli chiave indipendente di slot, usare un provider di chiavi esterne, ad esempio l'archiviazione di Blob di Azure, insieme di credenziali chiave di Azure, un archivio SQL, oppure cache Redis.

1. Se il profilo utente è disponibile, le chiavi vengono rese persistenti il *%LOCALAPPDATA%\ASP.NET\DataProtection-Keys* cartella. Se il sistema operativo è Windows, le chiavi vengono crittografate a riposo usando la DPAPI.

1. Se l'applicazione è ospitata in IIS, le chiavi vengono mantenute nel Registro di sistema HKLM in una chiave del Registro di sistema speciale solo per l'account del processo di lavoro. Le chiavi vengono crittografate a riposo tramite la DPAPI.

1. Se nessuna di queste condizioni corrispondono, le chiavi non sono persistenti all'esterno del processo corrente. Quando il processo viene arrestato, tutti generati le chiavi vengono perse.

Lo sviluppatore è sempre il controllo completo e può eseguire l'override di come e dove sono memorizzate chiavi. Le prime tre opzioni sopra devono fornire buoni valori predefiniti per la maggior parte delle applicazioni allo stesso modo in ASP.NET  **\<machineKey >** routine la generazione automatica funzionavano in passato. L'opzione di fallback, finale è l'unico scenario che richiede allo sviluppatore di specificare [configurazione](xref:security/data-protection/configuration/overview) iniziale se desiderano persistenza chiave, ma questo fallback si verifica solo in rare situazioni.

Durante l'hosting in un contenitore Docker, chiavi devono essere mantenute in una cartella che è un volume di Docker (un volume condiviso o un volume montato host che viene mantenuto oltre la durata del contenitore) o in un provider esterno, ad esempio [insieme credenziali chiavi Azure](https://azure.microsoft.com/services/key-vault/) o [Redis](https://redis.io/). Un provider esterno è utile in scenari web farm anche se le app non è possibile accedere a un volume di rete condivisa (vedere [PersistKeysToFileSystem](xref:security/data-protection/configuration/overview#persistkeystofilesystem) per altre informazioni).

> [!WARNING]
> Se lo sviluppatore sostituisce le regole descritte in precedenza e indica al sistema di protezione dei dati in un repository di chiave specifico, la crittografia automatica delle chiavi di resto è disabilitata. Nella protezione dei dati può essere riabilitata tramite [configurazione](xref:security/data-protection/configuration/overview).

## <a name="key-lifetime"></a>Durata della chiave

Per impostazione predefinita, le chiavi hanno una durata di 90 giorni. Quando una chiave scade, l'applicazione genera una nuova chiave automaticamente e imposta la nuova chiave come chiave attiva. Fino a quando le chiavi ritirate restano nel sistema, l'app può decrittografare i dati protetti con essi. Vedere [gestione delle chiavi](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling) per ulteriori informazioni.

## <a name="default-algorithms"></a>Algoritmi predefiniti

L'algoritmo di protezione dati di payload predefinito utilizzato è AES-256-CBC la riservatezza e HMACSHA256 l'autenticità. Una chiave master di 512 bit, cambiata ogni 90 giorni, viene usata per derivare le due chiavi secondario utilizzate per questi algoritmi in base al payload. Vedere [sottochiave derivazione](xref:security/data-protection/implementation/subkeyderivation#additional-authenticated-data-and-subkey-derivation) per ulteriori informazioni.

## <a name="see-also"></a>Vedere anche

* [Estendibilità della gestione delle chiavi](xref:security/data-protection/extensibility/key-management)
