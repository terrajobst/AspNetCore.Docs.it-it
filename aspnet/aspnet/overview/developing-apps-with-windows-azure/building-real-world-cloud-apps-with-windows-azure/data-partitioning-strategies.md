---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-partitioning-strategies
title: I dati (creazione di App Cloud funzionanti con Azure) di strategie di partizionamento | Microsoft Docs
author: MikeWasson
description: La creazione Real World di App Cloud con e-book Azure si basa su una presentazione sviluppata da Scott Guthrie. Viene spiegato 13 modelli e procedure consigliate che egli può...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 513837a7-cfea-4568-a4e9-1f5901245d24
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-partitioning-strategies
msc.type: authoredcontent
ms.openlocfilehash: 07a80767ca2def26c0252037a3b8b990abcbe1b3
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578484"
---
<a name="data-partitioning-strategies-building-real-world-cloud-apps-with-azure"></a>Dati (creazione di App Cloud funzionanti con Azure) di strategie di partizionamento
====================
dal [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Download risolverlo Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [Scarica l'E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Il **creazione Real World di App Cloud con Azure** eBook si basa su una presentazione sviluppata da Scott Guthrie. Viene illustrato 13 modelli e procedure consigliate che consentono di avere esito positivo lo sviluppo di App web per il cloud. Per informazioni sulla serie, vedere [capitolo prima](introduction.md).


In precedenza è stato illustrato come è facile scalare il livello web di un'applicazione cloud, aggiungendo e rimuovendo i server web. Ma se sono tutti verificano lo stesso archivio dati, collo di bottiglia dell'applicazione viene spostato dal front-end al back-end e il livello dati è più difficile da ridimensionare. In questo capitolo illustra come è possibile apportare al livello dati scalabili tramite il partizionamento dei dati in più database relazionali o combinando l'archiviazione del database relazionale con altre opzioni di archiviazione dei dati.

La configurazione di uno schema di partizionamento è migliore completato in anticipo per lo stesso motivo indicato in precedenza: è molto difficile modificarla strategia di archiviazione dei dati dopo che un'app è in fase di produzione. Se si pensa rigido fin dall'inizio approcci diversi, è possibile evitare "Twitter qualche" quando l'app si blocca o diventa inattiva per lungo tempo mentre si riorganizza i dati e codice di accesso ai dati dell'app.

## <a name="the-three-vs-of-data-storage"></a>Visual tre Studio dell'archivio dati

Per determinare se è necessario una strategia di partizionamento e cosa dovrebbe esserlo, prendere in considerazione tre domande sui dati:

- Volume: la quantità di dati sarà necessario infine archiviare? Due gigabyte? Due centinaia gigabyte? Terabyte? Petabyte?
- Velocità: che cos'è la frequenza con cui i dati aumenterà? Si tratta di un'app interna che non è la generazione di una grande quantità di dati? Un'applicazione esterna che ai clienti verranno caricata immagini e video in?
- Diversi: il tipo di dati verranno archiviati? Relazionali, immagini, coppie chiave-valore, grafici basati su social network?

Se si ritiene che si intende presenta un elevato numero di volume, velocità o diverse, è necessario valutare con attenzione il tipo di schema di partizionamento ottimale permetterà all'app di scalare in modo efficiente ed efficace quando si espande, e per assicurarsi che non vengono eseguiti in eventuali colli di bottiglia.

Esistono sostanzialmente tre approcci di partizionamento:

- Partizionamento verticale
- Il partizionamento orizzontale
- Partizionamento ibrido

## <a name="vertical-partitioning"></a>Partizionamento verticale

Porzioni verticale è, ad esempio suddividendo una tabella da colonne: un set di colonne entra in un archivio dati, e un altro set di colonne entra in un archivio dati diverso.

Si supponga, ad esempio, che l'app archivia i dati sulle persone, incluse le immagini:

![Tabella di dati](data-partitioning-strategies/_static/image1.png)

Quando rappresentare tali dati sotto forma di tabella ed esaminare i diversi tipi diversi di dati, si noterà che le tre colonne a sinistra sono dati di tipo stringa che possono essere archiviati in modo efficiente da un database relazionale, anche se le due colonne a destra sono essenzialmente le matrici di byte che c orte dai file di immagine. È possibile ai dati di file di immagine di archiviazione in un database relazionale e molti utenti eseguire l'operazione perché non si vuole salvare i dati nel file System. Potrebbero non avere un file system in grado di archiviare i necessari volumi di dati o potrebbe non desiderano gestire separato il backup e ripristino di sistema. Questo approccio funziona bene per i database locali e per piccole quantità di dati in database cloud. Nell'ambiente locale, potrebbe essere più semplice consentire solo l'amministratore di database risolvere il problema.

Ma in un database cloud, è relativamente costosa archiviazione e un volume elevato di immagini è stato possibile verificare le dimensioni del database di crescere oltre i limiti in corrispondenza del quale può funzionare in modo efficiente. Si possono risolvere questi problemi tramite il partizionamento dei dati in verticale, ovvero che scelto l'archivio di dati più appropriato per ogni colonna della tabella di dati. Ciò che potrebbe essere più adatti per questo esempio consiste nell'inserire i dati della stringa in un database relazionale e le immagini nell'archiviazione Blob.

![Tabella di dati partizionata verticalmente](data-partitioning-strategies/_static/image2.png)

L'archiviazione delle immagini nell'archiviazione Blob anziché un database è più pratico nel cloud rispetto a in un ambiente on-premises perché non è necessario preoccuparsi della configurazione di file server o la gestione di backup e ripristino dei dati archiviati all'esterno del database relazionale: tutti che viene eseguita automaticamente dal servizio di archiviazione Blob.

Si tratta dell'approccio di partizionamento è implementata nell'app Fix It e si cercheranno il codice che nel [capitolo archiviazione Blob](unstructured-blob-storage.md). Senza questo schema di partizionamento e presupponendo una dimensione di immagine medio del 3 MB, l'app Fix It solo sarebbe in grado di archiviare circa 40.000 attività prima di raggiungere le dimensioni massime del database di 150 GB. Dopo aver rimosso le immagini, il database può archiviare 10 volte superiore all'attività. è possibile andare molto più tempo prima è necessario preoccuparsi di implementare uno schema di partizionamento orizzontale. E se l'app viene scalata, le spese allunga di conseguenza più lento perché la maggior parte delle esigenze di archiviazione verranno nell'archiviazione Blob molto conveniente.

## <a name="horizontal-partitioning-sharding"></a>Partizionamento orizzontale (sharding)

Porzioni orizzontali è, ad esempio suddividendo una tabella dalle righe: un set di righe entra in un archivio dati, e un altro set di righe entra in un archivio dati diverso.

Dato lo stesso set di dati, un'altra opzione, è possibile archiviare diversi intervalli di nomi di clienti in database diversi.

![Tabella di dati partizionata orizzontalmente](data-partitioning-strategies/_static/image3.png)

Si desidera prestare molta attenzione lo schema di partizionamento orizzontale per assicurarsi che i dati sono distribuiti uniformemente per evitare le aree sensibili. Questo semplice esempio utilizzando la prima lettera del cognome non soddisfa questo requisito, perché molte persone hanno i cognomi che iniziano con determinate lettere comuni. Si raggiungeva prima del previsto perché alcuni database otterrebbe grandi mentre la maggior parte rimarrebbe piccole limitazioni relative alle dimensioni di tabella.

Un lato negativo di partizionamento orizzontale è che potrebbe essere difficile eseguire query su tutti i dati. In questo esempio, una query sarebbe necessario disegnare a partire dal 26 fino a diversi database per ottenere tutti i dati archiviati dall'app.

## <a name="hybrid-partitioning"></a>Partizionamento ibrido

È possibile combinare il partizionamento orizzontale e verticale. Ad esempio, nei dati di esempio è stato possibile archiviare le immagini nell'archiviazione Blob e partizionare orizzontalmente i dati della stringa.

![Ibrido di dati tabella partizionata](data-partitioning-strategies/_static/image4.png)

## <a name="partitioning-a-production-application"></a>Il partizionamento di un'applicazione di produzione

Concettualmente è facile vedere funzionamento uno schema di partizionamento, ma qualsiasi schema di partizionamento di aumentare la complessità del codice e introduce molte nuove complicazioni che devi affrontare. Se stai spostando le immagini nell'archiviazione BLOB, cosa fare quando il servizio di archiviazione è inattivo? Come è possibile gestire la sicurezza blob? Cosa accade se lo spazio di archiviazione blob e database non venga sincronizzato. Se si ha di partizionamento orizzontale, come verranno gestiti eseguono query su tutti i database?

Le complicazioni sono semplici da gestire, purché si intende per loro prima di passare all'ambiente di produzione. Molte persone che non desiderano che avevano in un secondo momento. In Media il nostro team di Customer Advisory Team (CAT) Ottiene telefonate in panico su una volta al mese da parte dei clienti di App sono prendendo piede in modo significativo e operazioni non eseguite questa pianificazione. Scopro immancabilmente simile a: "Guida in linea. Tutto ciò che è stato inserito in un unico archivio dati e di 45 giorni verrà esaurito lo spazio su tale!" E se si ha una grande quantità di logica di business incorporata in modalità di accesso all'archivio dati e per i clienti che usano l'app, non sono previsti tempi buona per spostarsi in basso per un giorno, mentre esegue la migrazione. Si finisce passare attraverso herculean sforzi per la partizione dei clienti i propri dati in tempo reale con senza tempi di inattività. È molto appassionante e spaventosa e non si desidera essere coinvolto nel se è possibile evitarlo. Valutando ciò fin dall'inizio e relativa integrazione in app per facilitare la vita molto se l'app cresce in un secondo momento.

## <a name="summary"></a>Riepilogo

Un efficace schema di partizionamento è possibile abilitare le app cloud per la scalabilità fino a petabyte di dati nel cloud senza i colli di bottiglia. E non devi pagare fin dall'inizio per le macchine di grandi dimensioni o un'infrastruttura completa come si farebbe se è stata usata l'app in un data center locale. Nel cloud è possibile è possibile aggiungere capacità in modo incrementale quando ti serve, e sta pagando solo per quanto si usa quando si usa lo.

Nel [capitolo successivo](unstructured-blob-storage.md) vedremo come l'app Fix It implementa il partizionamento verticale archiviando le immagini nell'archiviazione Blob.

## <a name="resources"></a>Risorse

Per altre informazioni sulle strategie di partizionamento, vedere le risorse seguenti.

Documentazione:

- [Procedure consigliate per la progettazione di servizi su larga scala nei servizi Cloud Windows Azure](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). White paper Mark Simms e Michael Thomassy.
- [Microsoft Patterns and Practices - Cloud Design Patterns](https://msdn.microsoft.com/library/dn568099.aspx). Vedere le indicazioni partizionamento dei dati, modello di partizionamento orizzontale.

Video

- [Operatore alternativo: Creazione di servizi Cloud scalabili, resilienti](https://channel9.msdn.com/Series/FailSafe). Serie in nove parti Marc Mercuri, Ulrich Homann e Mark Simms. Presenta i concetti generali e principi architetturali in modo estremamente accessibile e interessano, con storie ricavate dall'esperienza di Microsoft Customer Advisory Team (CAT) con i clienti effettivi. Vedere la discussione partizionamento episodio 7.
- [Big predefiniti: Lezioni apprese dai clienti di Azure - parte I](https://channel9.msdn.com/Events/Build/2012/3-029). Mark Simms illustra gli schemi di partizionamento, strategie di partizionamento orizzontale, come l'implementazione di partizionamento orizzontale e le federazioni di Database SQL, iniziando in corrispondenza di 19:49. Simile alla serie di tipo operatore alternativo, ma passa ad analizzare altri dettagli sulle procedure.

Codice di esempio:

- [Sviluppo di servizi in Windows Azure cloud](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Applicazione di esempio che include un database partizionato. Per una descrizione dello schema di partizionamento orizzontale implementato, vedere [DAL: partizionamento orizzontale di RDBMS](https://blogs.msdn.com/b/windowsazure/archive/2013/09/05/dal-sharding-of-rdbms.aspx) sul blog di Windows Azure.

> [!div class="step-by-step"]
> [Precedente](data-storage-options.md)
> [Successivo](unstructured-blob-storage.md)
