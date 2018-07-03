---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
title: Archivio Blob non strutturati (creazione di App Cloud funzionanti con Azure) | Microsoft Docs
author: MikeWasson
description: La creazione Real World di App Cloud con e-book Azure si basa su una presentazione sviluppata da Scott Guthrie. Viene spiegato 13 modelli e procedure consigliate che egli può...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/30/2015
ms.topic: article
ms.assetid: 9f05ccb1-2004-4661-ad8b-c370e6c09c8e
ms.technology: ''
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
msc.type: authoredcontent
ms.openlocfilehash: 1840bea3b4183838ffdfe710e987b864a05d53fb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37364184"
---
<a name="unstructured-blob-storage-building-real-world-cloud-apps-with-azure"></a>Archivio Blob non strutturati (creazione di App Cloud funzionanti con Azure)
====================
dal [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download risolverlo Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [Scarica l'E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Il **creazione Real World di App Cloud con Azure** eBook si basa su una presentazione sviluppata da Scott Guthrie. Viene illustrato 13 modelli e procedure consigliate che consentono di avere esito positivo lo sviluppo di App web per il cloud. Per informazioni sull'e-book, vedere [capitolo prima](introduction.md).


Nel capitolo precedente abbiamo esaminato gli schemi di partizionamento e illustrato come l'app Fix It archivia le immagini nel servizio Blob di archiviazione di Azure e altri dati contenuti nel Database SQL di Azure. In questo capitolo si approfondiscono il servizio Blob e Mostra come viene implementato nel codice di progetto di Fix It.

## <a name="what-is-blob-storage"></a>Che cos'è l'archiviazione Blob?

Il servizio Blob di archiviazione di Azure fornisce un modo per archiviare i file nel cloud. Il servizio Blob include una serie di vantaggi rispetto all'archiviazione file in un file system di rete locale:

- È altamente scalabile. Un singolo account di archiviazione può archiviare [centinaia di terabyte](https://msdn.microsoft.com/library/windowsazure/dn249410.aspx), ed è possibile avere più account di archiviazione. Alcuni dei principali clienti Azure archiviano centinaia di petabyte. Microsoft SkyDrive Usa l'archiviazione blob.
- È durevole. Ogni file che è archiviare nel servizio Blob viene automaticamente eseguito il backup.
- Fornisce la disponibilità elevata. Il [contratto di servizio per l'archiviazione](https://go.microsoft.com/fwlink/p/?linkid=159705&amp;clcid=0x409) promesse del 99,99% o 99,9% il tempo di attività, a seconda di quale opzione di ridondanza geografica scelta.
- È una funzionalità di platform-as-a-service (PaaS) di Azure, pertanto è sufficiente archiviare e recuperare i file, pagando solo per la quantità effettiva di archiviazione usate, e Azure si occupa automaticamente di dover configurare e gestire tutte le macchine virtuali e le unità disco richiesto per il servizio.
- È possibile accedere al servizio Blob usando un'API REST o tramite un linguaggio di programmazione, API. Sono disponibili SDK per .NET, Java, Ruby e altri utenti.
- Quando si archivia un file nel servizio Blob, è possibile facilmente renderlo disponibile pubblicamente su Internet.
- È possibile proteggere i file nel Blob di accedere al servizio in modo da poter solo dagli utenti autorizzati, oppure è possibile fornire i token di accesso temporaneo che li rende disponibili per un utente solo per un periodo di tempo limitato.

Ogni volta che si sta creando un'app per Azure e si desidera memorizzare una grande quantità di dati in un ambiente on-premises vengono inseriti nei file, ad esempio immagini, video, documenti PDF, fogli di calcolo e così via, prendere in considerazione il servizio Blob.

## <a name="creating-a-storage-account"></a>Creazione di un account di archiviazione

Per iniziare a usare il servizio Blob creare un account di archiviazione in Azure. Nel portale, fare clic su **New** -- **Data Services** -- **archiviazione** -- **creazione rapida**, e quindi immettere un URL e una posizione del data center. Posizione del data center deve essere quello utilizzato per l'app web.

![Creare un account Archiv.](unstructured-blob-storage/_static/image1.png)

Si seleziona l'area primaria in cui si desidera archiviare il contenuto e se si sceglie la [replica geografica](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx#_Geo_Redundant_Storage) opzione, Azure crea le repliche di tutti i dati in un altro data center in un'altra area del paese. Ad esempio, se si sceglie data center degli Stati Uniti occidentali, quando si archivia un file che diventa anche al data center degli Stati Uniti occidentali, ma in background Azure lo copia a uno degli altri Stati Uniti data center. Se si verifica un'emergenza in un'area del paese, i dati sono comunque al sicuro.

Azure non replica i dati attraverso i limiti geopolitica: se la posizione primaria è negli Stati Uniti, i file vengono replicati solo in un'altra area negli Stati Uniti; Se la posizione primaria è Australia, i file vengono replicati solo a un altro data center in Australia.

Naturalmente, è anche possibile creare un account di archiviazione eseguendo i comandi da uno script, come abbiamo visto in precedenza. Ecco un comando di Windows PowerShell per creare un account di archiviazione:

[!code-powershell[Main](unstructured-blob-storage/samples/sample1.ps1)]

Dopo aver creato un account di archiviazione, è possibile avviare immediatamente l'archiviazione dei file nel servizio Blob.

## <a name="using-blob-storage-in-the-fix-it-app"></a>Uso dell'archiviazione Blob nell'app Fix It

L'app Fix It consente di caricare foto.

![Creare un'attività Fix It](unstructured-blob-storage/_static/image2.png)

Quando fa clic su **creare i FixIt**, l'applicazione carica il file di immagine specificati e lo archivia nel servizio Blob.

### <a name="set-up-the-blob-container"></a>Configurare il contenitore Blob

Per poter archiviare un file nel servizio Blob di cui è necessario un *contenitore* in cui salvarlo. Un contenitore Blob del servizio corrisponde a una cartella del file system. Gli script di creazione ambiente che abbiamo esaminato nel [automatizzare tutto capitolo](automate-everything.md) creare l'account di archiviazione, ma non creano un contenitore. Pertanto, lo scopo del `CreateAndConfigure` metodo del `PhotoService` classe consiste nel creare un contenitore se non esiste già. Questo metodo viene chiamato dal `Application_Start` nel metodo *Global. asax*.

[!code-csharp[Main](unstructured-blob-storage/samples/sample2.cs)]

La chiave di accesso e il nome account archiviazione vengono archiviate nel `appSettings` raccolta del *Web. config* del file e del codice nel `StorageUtils.StorageAccount` metodo Usa tali valori per compilare una stringa di connessione e stabilire una connessione:

[!code-csharp[Main](unstructured-blob-storage/samples/sample3.cs)]

Il `CreateAndConfigureAsync` crea quindi un oggetto che rappresenta il servizio Blob e un oggetto che rappresenta un contenitore (cartella) denominato "immagini" nel servizio Blob:

[!code-csharp[Main](unstructured-blob-storage/samples/sample4.cs)]

Se un contenitore denominato "immagini" non esiste ancora, che verrà attivato la prima volta che si esegue l'app su un nuovo account di archiviazione, il codice crea il contenitore e imposta le autorizzazioni per renderlo disponibile pubblicamente. (Per impostazione predefinita, nuovi contenitori blob privati e sono accessibili solo agli utenti che dispongono dell'autorizzazione per accedere all'account di archiviazione.)

[!code-csharp[Main](unstructured-blob-storage/samples/sample5.cs)]

### <a name="store-the-uploaded-photo-in-blob-storage"></a>Store la foto caricata nell'archivio Blob

Per caricare e salvare il file di immagine, l'app Usa un' `IPhotoService` interfaccia e un'implementazione dell'interfaccia nel `PhotoService` classe. Il *PhotoService.cs* file contiene tutto il codice nel Fix It app che comunica con il servizio Blob.

Il metodo del controller MVC seguente viene chiamato quando l'utente sceglie **creare i FixIt**. In questo codice `photoService` fa riferimento a un'istanza del `PhotoService` (classe), e `fixittask` fa riferimento a un'istanza del `FixItTask` classe di entità che archivia i dati per una nuova attività.

[!code-csharp[Main](unstructured-blob-storage/samples/sample6.cs?highlight=8)]

Il `UploadPhotoAsync` metodo nel `PhotoService` classe archivia il file caricato nel servizio Blob e restituisce un URL che punta al nuovo blob.

[!code-csharp[Main](unstructured-blob-storage/samples/sample7.cs)]

Come nel `CreateAndConfigure` metodo, il codice si connette all'account di archiviazione e crea un oggetto che rappresenta il contenitore blob "immagini", ad eccezione del fatto di seguito presuppone che il contenitore esiste già.

Quindi crea un identificatore univoco per l'immagine da caricare, mediante la concatenazione di un nuovo valore GUID con l'estensione del file:

[!code-csharp[Main](unstructured-blob-storage/samples/sample8.cs)]

Il codice quindi utilizza l'oggetto contenitore blob e il nuovo identificatore univoco per creare un oggetto blob, imposta un attributo in tale oggetto che indica quale tipo di file è e quindi utilizza l'oggetto blob per archiviare il file nell'archiviazione blob.

[!code-csharp[Main](unstructured-blob-storage/samples/sample9.cs)]

Infine, ottiene un URL che fa riferimento il blob. Questo URL verrà archiviato nel database e può essere utilizzato nelle pagine web Fix It per visualizzare l'immagine caricata.

[!code-csharp[Main](unstructured-blob-storage/samples/sample10.cs)]

Questo URL viene archiviato nel database come una delle colonne della tabella FixItTask.

[!code-csharp[Main](unstructured-blob-storage/samples/sample11.cs?highlight=10)]

Con solo l'URL nel database e le immagini nell'archiviazione Blob, l'app Fix It mantiene il database piccoli, scalabile ed economica, mentre le immagini vengono archiviate in cui l'archiviazione è economica e in grado di gestire terabyte o petabyte. Un account di archiviazione può archiviare centinaia di terabyte di foto di Fix It e si paga solo per ciò che usi. Pertanto, è possibile iniziare piccoli centesimi 9 delle versioni a pagamento per il primo GB e aggiungere altre immagini per centesimi per ogni GB aggiuntivo.

### <a name="display-the-uploaded-file"></a>Visualizzare il file caricato

L'applicazione Fix It consente di visualizzare il file di immagine caricata quando visualizza i dettagli per un'attività.

![Risolverlo dettagli attività con foto](unstructured-blob-storage/_static/image3.png)

Per visualizzare l'immagine, la visualizzazione MVC ha a che fare è includono il `PhotoUrl` valore nel codice HTML inviato al browser. Il server web e il database non utilizza i cicli per visualizzare l'immagine, queste operino solo pochi byte per l'URL dell'immagine. Nel codice Razor seguente `Model` fa riferimento a un'istanza del `FixItTask` classe di entità.

[!code-cshtml[Main](unstructured-blob-storage/samples/sample12.cshtml?highlight=11)]

Se si esamina il codice HTML della pagina che consente di visualizzare, viene visualizzato l'URL che punta direttamente all'immagine nell'archivio blob, simile al seguente:

[!code-cshtml[Main](unstructured-blob-storage/samples/sample13.cshtml?highlight=11)]

## <a name="summary"></a>Riepilogo

Si è visto come l'app Fix It archivia le immagini nel servizio Blob e solo gli URL di immagini nel database SQL. Uso del servizio Blob mantiene il database SQL di molto inferiore a quello in caso contrario, sarebbe, in modo da scalabilità fino a un numero quasi illimitato di attività e può essere eseguita senza scrivere una grande quantità di codice.

È possibile avere centinaia di terabyte in un account di archiviazione e il costo di archiviazione è molto meno costoso rispetto all'archiviazione di Database SQL, iniziando in corrispondenza di circa 3 centesimi per GB al mese più un addebito di transazione di piccole dimensioni. E tenere presente che non sta effettuando il pagamento per la capacità massima, ma solo per la quantità che effettivamente archiviati in modo che l'app è pronta per la scalabilità ma non si sta effettuando il pagamento per aumentare la capacità.

Nel [capitolo successivo](design-to-survive-failures.md) parleremo l'importanza di rendere un'app cloud in grado di gestire normalmente gli errori.

## <a name="resources"></a>Risorse

Per altre informazioni vedere le risorse seguenti:

- [Introduzione all'archiviazione BLOB di Azure](https://www.simple-talk.com/cloud/cloud-data/an-introduction-to-windows-azure-blob-storage-/). Blog di Mike Wood.
- [Come usare il servizio di archiviazione Blob di Azure in .NET](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs). Documentazione ufficiale nel sito MicrosoftAzure.com. Una breve introduzione all'archiviazione seguita da esempi di codice che illustra come connettersi all'archiviazione blob, blob creare contenitori, caricare e scaricare i BLOB e così via.
- [Operatore alternativo: Creazione di servizi Cloud scalabili, resilienti](https://channel9.msdn.com/Series/FailSafe). Serie video in nove parti Marc Mercuri, Ulrich Homann e Mark Simms. Presenta i concetti generali e principi architetturali in modo estremamente accessibile e interessano, con storie ricavate dall'esperienza di Microsoft Customer Advisory Team (CAT) con i clienti effettivi. Per una descrizione del servizio di archiviazione di Azure e BLOB, vedere episodio 5 partire 35:13.
- [Microsoft Patterns and Practices - informazioni aggiuntive su Azure](https://msdn.microsoft.com/library/dn568099.aspx). Vedere Valet Key pattern.

> [!div class="step-by-step"]
> [Precedente](data-partitioning-strategies.md)
> [Successivo](design-to-survive-failures.md)
