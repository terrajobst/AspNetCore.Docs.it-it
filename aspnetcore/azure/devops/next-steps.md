---
title: DevOps con ASP.NET Core e Azure | Passaggi successivi
author: CamSoper
description: Questa guida include informazioni complete sulla creazione di una pipeline DevOps per un'app ASP.NET Core ospitata in Azure.
ms.author: casoper
ms.date: 08/07/2018
uid: azure/devops/next-steps
ms.openlocfilehash: 7a0f1b1b56a33b1870e0657d8ba465adb84f5a02
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "42909670"
---
# <a name="next-steps"></a>Passaggi successivi

In questa guida è stata creata una pipeline di DevOps per un'app di esempio ASP.NET Core. La procedura è stata completata. Ci auguriamo che sia stata soddisfacente di apprendimento pubblicare le app web ASP.NET Core in servizio App di Azure e automatizzare l'integrazione continua delle modifiche.

Oltre a DevOps e hosting web, Azure offre un'ampia gamma di servizi di Platform-as-a-Service (PaaS) utili agli sviluppatori di ASP.NET Core. Questa sezione viene fornita una breve panoramica di alcuni dei servizi più usati.

## <a name="storage-and-databases"></a>Archiviazione e database

[Cache redis](https://docs.microsoft.com/azure/redis-cache/) è memorizzazione disponibile come servizio di dati e velocità effettiva elevata e bassa latenza. Può essere utilizzato per la memorizzazione nella cache dell'output delle pagine, riducendo le richieste del database e che fornisce lo stato della sessione ASP.NET Core in più istanze di un'app.

[Archiviazione di Azure](https://docs.microsoft.com/azure/storage/) è un'archiviazione cloud altamente scalabile di Azure. Gli sviluppatori possono sfruttare [archiviazione di Accodamento](https://docs.microsoft.com/azure/storage/queues/storage-queues-introduction) per l'accodamento dei messaggi affidabile, e [archiviazione tabelle](https://docs.microsoft.com/azure/storage/tables/table-storage-overview) è un archivio chiave-valore NoSQL progettato per lo sviluppo rapido con set di dati di grandi dimensioni, semi-strutturati.

[Database SQL di Azure](https://docs.microsoft.com/azure/sql-database/) fornisce funzionalità di database relazionale intuitiva di un servizio usando il motore Microsoft SQL Server.

[COSMOS DB](https://docs.microsoft.com/azure/cosmos-db/) servizio di database NoSQL multimodello, distribuito a livello globale. Più API sono disponibili, tra cui MongoDB, Cassandra e API SQL (in precedenza denominato DocumentDB).

## <a name="identity"></a>identità

[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) e [Azure Active Directory B2C](https://docs.microsoft.com/azure/active-directory-b2c/) sono entrambi servizi di identità. Azure Active Directory è progettato per gli scenari aziendali e consente di collaborazione di Azure AD B2B (business-to-business), mentre Azure Active Directory B2C è previsti scenari business to consumer, tra cui accedi social network.

## <a name="mobile"></a>Cellulare

[Hub di notifica](https://docs.microsoft.com/azure/notification-hubs/) è un motore di notifiche push multipiattaforma e scalabile per inviare rapidamente milioni di messaggi alle applicazioni eseguite in diversi tipi di dispositivi.

## <a name="web-infrastructure"></a>Infrastruttura Web

[Servizio contenitore di Azure](https://docs.microsoft.com/azure/aks/) gestisce l'ambiente Kubernetes ospitato, rendendo veloce e facile da distribuire e gestire le App in contenitori senza competenze nell'orchestrazione di contenitori.

[Ricerca di Azure](https://docs.microsoft.com/azure/search/) viene usato per creare una soluzione di ricerca aziendale su contenuto privato eterogeneo.

[Service Fabric](https://docs.microsoft.com/azure/service-fabric/) è una piattaforma di sistemi distribuiti che consente di creare un pacchetto, distribuire e gestire facilmente scalabile e affidabile microservizi e contenitori.
