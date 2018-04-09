---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-partitioning-strategies
title: Dati (creazione di applicazioni con Azure Cloud del mondo reale) strategie di partizionamento | Documenti Microsoft
author: MikeWasson
description: Le App per Cloud mondo reale compilazione con e-book Azure si basa su una presentazione sviluppata da Scott Guthrie. Viene spiegato 13 modelli e procedure che è possibile...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 513837a7-cfea-4568-a4e9-1f5901245d24
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-partitioning-strategies
msc.type: authoredcontent
ms.openlocfilehash: 9ff7f37a03d8d3dfab50e8007a6645bb0d88f453
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="data-partitioning-strategies-building-real-world-cloud-apps-with-azure"></a>Dati (creazione di applicazioni con Azure Cloud del mondo reale) strategie di partizionamento
====================
dal [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download correggerlo progetto](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [scaricare E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Il **predefiniti reale World Cloud App con Azure** e-book è basato su una presentazione sviluppata da Scott Guthrie. Vengono descritte le 13 modelli e procedure consigliate che consentono di avere esito negativo con lo sviluppo di App web per il cloud. Per informazioni sulla serie, vedere [primo capitolo](introduction.md).


In precedenza è stato illustrato come è facile per scalare il livello web di un'applicazione cloud, aggiungendo e rimuovendo i server web. Ma se sono tutti riscontrano lo stesso archivio dati, collo di bottiglia dell'applicazione viene spostato da front-end al back-end e il livello dati è più difficili da scalare. In questo capitolo verranno esaminate come effettuare il livello dati scalabile per il partizionamento dei dati in più database relazionali o combinando l'archiviazione del database relazionale con altre opzioni di archiviazione di dati.

Configurazione di uno schema di partizionamento è consigliabile completato in anticipo per lo stesso motivo indicato in precedenza: è molto difficile modificare la strategia di archiviazione di dati dopo che è un'app nell'ambiente di produzione. Se si pensa rigido anticipo diversi approcci, è possibile evitare "Twitter qualche" quando l'app si blocca o diventa inattiva per lungo tempo mentre è riorganizzare i dati e codice di accesso ai dati dell'applicazione.

## <a name="the-three-vs-of-data-storage"></a>Visual tre Studio di archiviazione dei dati

Per determinare se è necessario una strategia di partizionamento e quale deve essere, prendere in considerazione tre domande sui dati:

- Volume: la quantità di dati verrà infine li memorizzerà? Due gigabyte? Gigabyte un paio di centinaia? Terabyte? Petabyte?
- Velocità: che cos'è la frequenza aumenteranno i dati? Si tratta di un'applicazione interna che non è la generazione di una grande quantità di dati? Un'applicazione esterna che i clienti verranno caricando le immagini e video in?
- Archiviare diversi: il tipo di dati verrà? Relazionali, immagini, coppie chiave-valore, sociali grafici?

Se si ritiene che si intende un volume, velocità o varietà di numerosi, è necessario considerare con attenzione il tipo di schema di partizionamento verrà meglio abilitare un'app per la scalabilità in modo efficiente ed efficace, dimensioni e per verificare che non vengono eseguiti in eventuali colli di bottiglia.

Esistono tre approcci di partizionamento:

- Partizionamento verticale
- il partizionamento orizzontale
- Partizionamento ibrido

## <a name="vertical-partitioning"></a>Partizionamento verticale

Porzioni verticale è simile a suddividere una tabella in base a colonne: un set di colonne entra in un archivio dati, e un altro set di colonne passa in un archivio dati diverso.

Si supponga, ad esempio, che l'applicazione archivia i dati sulle persone, incluse le immagini:

![Tabella di dati](data-partitioning-strategies/_static/image1.png)

Quando si rappresentare tali dati come una tabella e osservare il varia a seconda dei dati, si può verificare che le tre colonne a sinistra sono associati dati di tipo stringa che possono essere archiviati in modo efficiente da un database relazionale, mentre le colonne di due a destra sono essenzialmente le matrici di byte da c abitazione dai file di immagine. È possibile ai dati di file di immagine di archiviazione in un database relazionale e molti utenti eseguire questa operazione perché non si desidera salvare i dati nel file System. Potrebbero non disporre di un file system in grado di archiviare i necessari volumi di dati o potrebbe non desiderano gestire separato copie di backup e ripristino di sistema. Questo approccio funziona bene per database locali e per piccole quantità di dati nel database cloud. Nell'ambiente locale, potrebbe essere più semplice consentire solo l'amministratore di database di risolvere.

Ma in un database cloud, archiviazione è relativamente costosa, e un numero elevato di immagini è possibile apportare le dimensioni del database di crescere oltre i limiti in cui può funzionare in modo efficiente. È possibile risolvere questi problemi tramite il partizionamento verticale, i dati che indica che si sceglie l'archivio di dati più appropriato per ogni colonna nella tabella di dati. Ciò che potrebbe essere più adatti per questo esempio consiste nell'inserire i dati di tipo stringa in un database relazionale e le immagini nell'archiviazione Blob.

![Tabella di dati partizionata verticalmente](data-partitioning-strategies/_static/image2.png)

L'archiviazione di immagini nell'archiviazione Blob anziché un database è più pratico nel cloud di in un ambiente locale, poiché non è necessario preoccuparsi configurare file server o la gestione di backup e ripristino dei dati archiviati all'esterno del database relazionale: tutti che viene eseguita automaticamente dal servizio di archiviazione Blob.

Si tratta dell'approccio di partizionamento è implementato nell'app Correggi e verrà esaminato il codice per cui nel [capitolo di archiviazione Blob](unstructured-blob-storage.md). Senza questo schema di partizionamento e presupponendo una dimensione media immagine di 3 MB, l'app Correggi solo sarebbe in grado di archiviare circa 40.000 attività prima di raggiungere le dimensioni massime del database di 150 GB. Dopo aver rimosso le immagini, il database è possibile archiviare 10 volte superiore all'attività. è possibile passare più prima di poter considerare l'implementazione di uno schema di partizionamento orizzontale. E se l'app viene ridimensionata, le spese allunga di conseguenza più lentamente perché la maggior parte delle esigenze di archiviazione sta passando a basso costo di archiviazione Blob.

## <a name="horizontal-partitioning-sharding"></a>(Partizionamento orizzontale) di partizionamento orizzontale

Porzioni orizzontale è come suddividere una tabella per le righe: un set di righe entra in un archivio dati, e un altro set di righe passa in un archivio dati diverso.

Dato lo stesso set di dati, un'altra opzione sarebbe per archiviare diversi intervalli di nomi di clienti in database diversi.

![Tabella di dati partizionata orizzontalmente](data-partitioning-strategies/_static/image3.png)

Si desidera prestare molta attenzione per assicurarsi che i dati vengono distribuiti uniformemente per evitare sensibili lo schema di partizionamento orizzontale. Questo semplice esempio utilizzando la prima lettera del cognome non soddisfa questo requisito, perché molti utenti hanno i cognomi che iniziano con determinate lettere comuni. Prima di quanto previsto perché alcuni database potrebbe ottenere dimensioni molto grandi durante la maggior parte rimarrebbero small si raggiungeva limitazioni relative alle dimensioni di tabella.

Un lato di partizionamento orizzontale è che potrebbe essere difficile eseguire query su tutti i dati. In questo esempio, una query sarebbe necessario disegnare da database diversi fino a 26 per ottenere tutti i dati archiviati dall'app.

## <a name="hybrid-partitioning"></a>Partizionamento ibrido

È possibile combinare il partizionamento orizzontale e verticale. Ad esempio, nei dati di esempio, è possibile archiviare le immagini nell'archiviazione Blob e partizionare orizzontalmente i dati di tipo stringa.

![Ibrida di tabella di dati partizionato](data-partitioning-strategies/_static/image4.png)

## <a name="partitioning-a-production-application"></a>Partizionamento di un'applicazione di produzione

Concettualmente è facile vedere funzionamento uno schema di partizionamento, ma qualsiasi schema di partizionamento di aumentare la complessità del codice e presenta molti complicazioni nuovo che è necessario affrontare. Se si sta spostando le immagini nell'archiviazione BLOB, in che modo? quando il servizio di archiviazione è inattivo Come gestire la sicurezza blob? Cosa accade se il database e l'archiviazione blob Ottiene sincronizzato? Se si partizionamento orizzontale, come verranno gestiti query tra tutti i database?

Le complicazioni sono gestibile, purché la pianificazione della relativa prima di passare alla produzione. Molte persone che non si che desidera era già presente in un secondo momento. In Media il nostro team Team (CAT, Customer Advisory) Ottiene panicked telefonate circa una volta al mese dai clienti il cui le app vengono in modo molto grande e non avviene questa pianificazione. E si dice simile al seguente: "Guida in linea. Inserire tutti gli elementi in un unico archivio dati e 45 giorni verrà spazio insufficiente su di esso." E, se si ha una grande quantità di logica di business in modalità di accesso nell'archivio dati e si dispone di clienti che utilizzano l'app, è consigliabile passare verso il basso per un giorno, mentre esegue la migrazione. Si finisce passando herculean sforzi per la partizione del cliente per i propri dati in tempo reale con alcun periodo di inattività. È molto appassionante e molto ardua, e che non si desidera essere coinvolto nel se non è possibile evitare! Pensa questo inizio viene quindi integrato nell'app per facilitare la vita molto se l'app si estende in un secondo momento.

## <a name="summary"></a>Riepilogo

Uno schema di partizionamento effettivo è possibile abilitare l'applicazione cloud scalabilità petabyte di dati nel cloud senza colli di bottiglia. E non è necessario pagare inizio macchine massive o un'infrastruttura completa come si farebbe se è stata usata l'app in un data center locale. Nel cloud è possibile aggiungere capacità in modo incrementale come necessario, e si sta solo a pagamento per come si usa quando viene usato.

Nel [capitolo successivo](unstructured-blob-storage.md) vedremo come app Correggi implementa il partizionamento verticale tramite l'archiviazione di immagini nell'archiviazione Blob.

## <a name="resources"></a>Risorse

Per ulteriori informazioni sulle strategie di partizionamento, vedere le risorse seguenti.

Documentazione:

- [Procedure consigliate per la progettazione di servizi su larga scala nei servizi Cloud Windows Azure](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). White paper Mark Simms e Michael Thomassy.
- [Microsoft Patterns and Practices - modelli di progettazione del Cloud](https://msdn.microsoft.com/library/dn568099.aspx). Consultare le informazioni disponibili partizionamento dei dati, il modello di partizionamento orizzontale.

Video

- [Operatore alternativo: Compilazione di servizi Cloud scalabili e resilienti](https://channel9.msdn.com/Series/FailSafe). Serie di nove parti Ulrich Homann, Marc Mercuri e Mark Simms. Presenta i concetti generali e i principi architetturali in modo molto accessibile e interessano con storie ricavate dall'esperienza di Microsoft Team (CAT, Customer Advisory) con i clienti effettivi. Vedere la discussione di partizionamento in episodio 7.
- [Compilazione Big: Insegnamenti appresi dai clienti di Microsoft Azure - parte I](https://channel9.msdn.com/Events/Build/2012/3-029). Mark Simms vengono descritti gli schemi di partizionamento, strategie di partizionamento orizzontale, come l'implementazione di partizionamento orizzontale e le federazioni di Database SQL, a partire da 19:49. Simile alla serie di operatore alternativo ma consente di spostarsi in dettaglio procedure.

Codice di esempio:

- [Sviluppo di servizi in Microsoft Azure cloud](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Applicazione di esempio che include un database partizionato. Per una descrizione dello schema di partizionamento orizzontale implementato, vedere [DAL: partizionamento orizzontale di RDBMS](https://blogs.msdn.com/b/windowsazure/archive/2013/09/05/dal-sharding-of-rdbms.aspx) nel blog di Windows Azure.

> [!div class="step-by-step"]
> [Precedente](data-storage-options.md)
> [Successivo](unstructured-blob-storage.md)
