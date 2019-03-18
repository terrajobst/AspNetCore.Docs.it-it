---
ms.openlocfilehash: 1f8d3913c83aaf5fe6ec2cec482a30f0f066c16b
ms.sourcegitcommit: 34bf9fc6ea814c039401fca174642f0acb14be3c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/14/2019
ms.locfileid: "57841693"
---

> [!NOTE]
> Per questa esercitazione si userà la funzionalità *migrazioni* di Entity Framework Core laddove possibile. Le migrazioni aggiornano lo schema del database in base alle modifiche nel modello di dati. Tuttavia, le migrazioni possono eseguire solo i tipi di modifiche supportate dal provider EF Core e le funzionalità del provider SQLite sono limitate. Ad esempio l'aggiunta di una colonna è supportata, ma la rimozione o la modifica di una colonna non è supportata. Se si crea una migrazione per rimuovere o modificare una colonna, il comando `ef migrations add` ha esito positivo, ma il comando `ef database update` ha esito negativo. A causa di queste limitazioni, in questa esercitazione non vengono usate le migrazioni per le modifiche dello schema di SQLite. In caso di modifiche dello schema, il database viene invece eliminato e ricreato.
>
>La soluzione per ovviare alle limitazioni di SQLite consiste nello scrivere manualmente il codice delle migrazioni per eseguire una ricompilazione della tabella in caso di modifiche nella tabella. La ricompilazione della tabella comporta:
>
>* Ridenominazione della tabella esistente.
>* Creazione di una nuova tabella.
>* Copia dei dati dalla vecchia tabella alla nuova tabella.
>* Eliminazione della tabella precedente.
>
>Per altre informazioni, vedere le seguenti risorse:
>
> * [Limitazioni del provider di database SQLite per EF Core](/ef/core/providers/sqlite/limitations)
> * [Personalizzare il codice di migrazione](/ef/core/managing-schemas/migrations/#customize-migration-code)
> * [Seeding dei dati](/ef/core/modeling/data-seeding)