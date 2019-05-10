---
title: Gerarchia di scopi e multi-tenancy in ASP.NET Core
author: rick-anderson
description: Informazioni sulla gerarchia di stringhe di scopi e multi-tenancy in relazione a ASP.NET Core Data Protection API.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/purpose-strings-multitenancy
ms.openlocfilehash: 1133d40e7b325d58b3f70e7387494dae36ff8ac9
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/27/2019
ms.locfileid: "64896798"
---
# <a name="purpose-hierarchy-and-multi-tenancy-in-aspnet-core"></a>Gerarchia di scopi e multi-tenancy in ASP.NET Core

Poiché un' `IDataProtector` è inoltre in modo implicito un `IDataProtectionProvider`, a scopo può essere concatenato. In questo senso `provider.CreateProtector([ "purpose1", "purpose2" ])` equivale a `provider.CreateProtector("purpose1").CreateProtector("purpose2")`.

In questo modo per alcune relazioni gerarchiche interessanti tramite il sistema di protezione dati. Nell'esempio precedente di [Contoso.Messaging.SecureMessage](xref:security/data-protection/consumer-apis/purpose-strings#data-protection-contoso-purpose), può chiamare il componente SecureMessage `provider.CreateProtector("Contoso.Messaging.SecureMessage")` iniziale una sola volta e memorizzare nella cache il risultato in una privata `_myProvider` campo. Sarà quindi possibile creare protezioni futuri tramite chiamate a `_myProvider.CreateProtector("User: username")`, e queste protezioni vengono usate per la protezione dei singoli messaggi.

Ciò può anche essere invertita. Prendere in considerazione una singola applicazione logica che ospita più tenant (un sistema CMS sembra ragionevole) e ogni tenant può essere configurato con un proprio sistema di gestione dello stato e l'autenticazione. L'applicazione generico dispone di un singolo provider master e chiama `provider.CreateProtector("Tenant 1")` e `provider.CreateProtector("Tenant 2")` per fornire la propria sezione isolata del sistema di protezione dati di ogni tenant. I tenant è stato possibile derivare le proprie singoli programmi di protezione in base alle proprie esigenze, ma, indipendentemente dal livello provano non può creare protezioni con cui sono in conflitto con un altro tenant nel sistema. Graficamente, ciò è rappresentato come indicato di seguito.

![A scopo di tenancy più](purpose-strings-multitenancy/_static/purposes-multi-tenancy.png)

>[!WARNING]
> Ciò presuppone l'ombrello controlli delle applicazioni quali API sono disponibili a singoli tenant e che i tenant non è possibile eseguire codice arbitrario nel server. Se un tenant può eseguire codice arbitrario, è stato possibile eseguire la reflection privata per interrompere le garanzie di isolamento oppure potrebbe semplicemente leggere direttamente il materiale della chiave master e derivano tutte le sottochiavi hanno l'esigenza di.

Il sistema di protezione dei dati utilizza effettivamente una sorta di multi-tenancy nella configurazione predefinita della finestra. Per impostazione predefinita materiale della chiave master viene archiviato nella cartella del profilo utente dell'account del processo di lavoro (o il Registro di sistema, per le identità pool di applicazioni IIS). Ma è in realtà è abbastanza comune usare un singolo account per eseguire più applicazioni e pertanto tutte le applicazioni finirebbe materiale per le chiavi master di condivisione. Per risolvere questo problema, il sistema di protezione dati inserisce automaticamente un identificatore univoco per ogni applicazione come primo elemento nella catena di scopo generale. Che consente di funzione implicita [isolare le singole applicazioni](xref:security/data-protection/configuration/overview#per-application-isolation) uno da altro, in modo efficace considerando ogni applicazione come un tenant all'interno del sistema e il processo di creazione di protezione univoco è identico all'immagine sopra.
