---
title: Passaggi successivi - DevOps con ASP.NET Core e Azure
author: CamSoper
description: Altri percorsi di apprendimento per DevOps con ASP.NET Core e Azure.
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: azure/devops/next-steps
ms.openlocfilehash: a775dc42551a43bcce72b5f9ca364ed00b1dc4e6
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78659474"
---
# <a name="next-steps"></a>Passaggi successivi

In questa guida è stata creata una pipeline di DevOps per un'app di esempio ASP.NET Core. Congratulazioni! Ci auguriamo che sia stata soddisfacente di apprendimento pubblicare le app web ASP.NET Core in servizio App di Azure e automatizzare l'integrazione continua delle modifiche.

Oltre a DevOps e hosting web, Azure offre un'ampia gamma di servizi di Platform-as-a-Service (PaaS) utili agli sviluppatori di ASP.NET Core. Questa sezione viene fornita una breve panoramica di alcuni dei servizi più usati.

## <a name="storage-and-databases"></a>Archiviazione e database

[Cache Redis](/azure/redis-cache/) è un'elevata velocità effettiva e la memorizzazione nella cache dei dati a bassa latenza disponibile come servizio. Può essere utilizzato per la memorizzazione nella cache dell'output delle pagine, riducendo le richieste del database e che fornisce lo stato della sessione ASP.NET Core in più istanze di un'app.

[Archiviazione di Azure](/azure/storage/) è una risorsa di archiviazione cloud estremamente scalabile di Azure. Gli sviluppatori possono sfruttare i vantaggi dell' [archiviazione code](/azure/storage/queues/storage-queues-introduction) per l'accodamento dei messaggi affidabile e l' [archiviazione tabelle](/azure/storage/tables/table-storage-overview) è un archivio chiave-valore NoSQL progettato per lo sviluppo rapido usando set di dati di grandi dimensioni e semi-strutturati.

Il [database SQL di Azure](/azure/sql-database/) offre funzionalità di database relazionali familiari come un servizio che usa il motore di Microsoft SQL Server.

[Cosmos DB](/azure/cosmos-db/) servizio di database NoSQL multimodello distribuito a livello globale. Più API sono disponibili, tra cui MongoDB, Cassandra e API SQL (in precedenza denominato DocumentDB).

## <a name="identity"></a>Identità

[Azure Active Directory](/azure/active-directory/) e [Azure Active Directory B2C](/azure/active-directory-b2c/) sono entrambi servizi di identità. Azure Active Directory è progettato per gli scenari aziendali e consente di collaborazione di Azure AD B2B (business-to-business), mentre Azure Active Directory B2C è previsti scenari business to consumer, tra cui accedi social network.

## <a name="mobile"></a>Mobile

[Hub di notifica](/azure/notification-hubs/) è un motore di notifiche push multipiattaforma e scalabile che consente di inviare rapidamente milioni di messaggi alle app in esecuzione su diversi tipi di dispositivi.

## <a name="web-infrastructure"></a>Infrastruttura Web

Il [servizio contenitore di Azure](/azure/aks/) gestisce l'ambiente Kubernetes ospitato, semplificando e semplificando la distribuzione e la gestione delle app incluse in contenitori senza competenze nell'orchestrazione di contenitori.

[Ricerca di Azure](/azure/search/) viene usato per creare una soluzione di ricerca aziendale sul contenuto privato eterogeneo.

[Service Fabric](/azure/service-fabric/) è una piattaforma di sistemi distribuiti che semplifica la disposizione di pacchetti, la distribuzione e la gestione di microservizi e contenitori scalabili e affidabili.
