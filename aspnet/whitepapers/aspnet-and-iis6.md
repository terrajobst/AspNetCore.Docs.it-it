---
uid: whitepapers/aspnet-and-iis6
title: Esecuzione di ASP.NET 1.1 con IIS 6.0 | Documenti Microsoft
author: rick-anderson
description: Sebbene Windows Server 2003 include sia IIS 6.0 e ASP.NET 1.1, questi componenti sono disabilitati per impostazione predefinita. Questo white paper descrive come abilitare IIS 6.0 un...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 5a5537bf-2aaa-49e7-839f-9e6522b829d8
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-and-iis6
msc.type: content
ms.openlocfilehash: 1fcac7b8bc295ccf4e36189295b6bc2e4d328623
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="running-aspnet-11-with-iis-60"></a>Esecuzione di ASP.NET 1.1 con IIS 6.0
====================
> Sebbene Windows Server 2003 include sia IIS 6.0 e ASP.NET 1.1, questi componenti sono disabilitati per impostazione predefinita. Questo white paper viene descritto come abilitare IIS 6.0 e ASP.NET 1.1 e consiglia diverse impostazioni di configurazione per ottenere prestazioni ottimali da IIS e ASP.NET.
> 
> Si applica a ASP.NET 1.1 e IIS 6.0.


ASP.NET 1.1 viene fornito con Windows Server 2003, che include anche la versione più recente di Internet Information Server (IIS) versione 6.0. IIS 6.0 e ASP.NET 1.1 sono progettati per integrarsi perfettamente e ASP.NET viene ora impostato per il nuovo modello di processo di lavoro di IIS 6.0.

## <a name="aspnet-11-is-not-installed-by-default"></a>ASP.NET 1.1 non è installato per impostazione predefinita

Diversamente dalle versioni precedenti di sistemi operativi Microsoft, Internet Information Server (IIS) non è abilitato per impostazione predefinita. non è ASP.NET 1.1. Sono disponibili due opzioni per l'abilitazione di IIS:

### <a name="enabling-iis-option-1---configure-your-server-wizard"></a>L'abilitazione di IIS, l'opzione #1 - Configurazione guidata Server

Windows Server 2003 è disponibile un nuovo 'Configurazione guidata Server' che consentono di configurare correttamente il server in modalità desiderata.

Per avviare la procedura guidata - nota, eseguire la procedura guidata è necessario eseguire l'accesso come amministratore, passare a: Start | I programmi | Strumenti di amministrazione e selezionare 'configurazione Server".

Una volta selezionata verrà visualizzata la schermata di apertura 'Configurazione guidata Server':

![](aspnet-and-iis6/_static/image1.jpg)

Fare clic su ' Avanti &gt;':

![](aspnet-and-iis6/_static/image2.jpg)

Fare clic su ' Avanti &gt;'

![](aspnet-and-iis6/_static/image3.jpg)

In questa schermata sarà necessario selezionare "il server applicazioni (IIS, ASP.NET) come opzioni di configurazione.

Fare clic su ' Avanti &gt;'.

![](aspnet-and-iis6/_static/image4.jpg)

Dopo aver selezionato per configurare il server come Server applicazioni, verrà visualizzata questa schermata che richiede le funzionalità aggiuntive devono essere installate. Nessuna opzione è selezionata per impostazione predefinita. Per abilitare ASP.NET automaticamente, è necessario selezionare ' abilitare ASP. NET'.

Fare clic su ' Avanti &gt;'.

![](aspnet-and-iis6/_static/image5.jpg)

Questa schermata consente di visualizzare che cosa sono le opzioni da installare.

Fare clic su ' Avanti &gt;'.

![](aspnet-and-iis6/_static/image6.jpg)

Verrà visualizzata questa schermata durante l'installazione le opzioni selezionate. È normale per visualizzare altri finestra di dialogo verranno visualizzate finestre di come vengono installati i servizi. Potrebbe inoltre essere richiesto per la posizione del CD di installazione di Windows Server 2003.

Fare clic su ' Avanti &gt;' al termine.

![](aspnet-and-iis6/_static/image7.jpg)

Fare clic su 'Fine' - Windows Server 2003 è ora configurato per il supporto di IIS 6.0 e ASP.NET 1.1.

### <a name="enabling-iis-option-2---manually-configuring-iis-and-aspnet"></a>L'attivazione di IIS, l'opzione #2: configurare manualmente IIS e ASP.NET

Se non si desidera utilizzare il "Configurazione guidata Server' è facoltativamente possibile installare IIS 6.0 e ASP.NET 1.1 tramite 'Installazione applicazioni' nel Pannello di controllo.

Innanzitutto, aprire il pannello di controllo:

![](aspnet-and-iis6/_static/image8.jpg)

Successivamente, fare clic su ' Aggiungi/Rimuovi componenti di Windows' verrà aperta 'Guidata componenti di Windows':

![](aspnet-and-iis6/_static/image9.jpg)

Evidenziare e selezionare 'Server di applicazioni' e quindi fare clic su "Dettagli"? pulsante:

![](aspnet-and-iis6/_static/image10.jpg)

Per installare ASP.NET, selezionare ' ASP. NET'.

Fare clic su 'OK' per tornare alla finestra di aggiunta guidata componenti di Windows. Fare clic su ' Avanti &gt;' dall'Aggiunta guidata componenti di Windows per avviare l'installazione:

![](aspnet-and-iis6/_static/image11.jpg)

È normale per visualizzare altri finestra di dialogo verranno visualizzate finestre di come vengono installati i servizi. Potrebbe inoltre essere richiesto per la posizione del CD di installazione di Windows Server 2003.

Al termine dell'installazione verrà visualizzata la schermata ultimo dell'Aggiunta guidata componenti di Windows:

![](aspnet-and-iis6/_static/image12.jpg)

IIS 6.0 e ASP.NET 1.1 siano configurati e disponibili.

## <a name="recommended-settings"></a>Impostazioni consigliate

Durante l'esecuzione di ASP.NET 1.1 con IIS 6.0 sono disponibili diverse impostazioni di configurazione consigliati per ottenere prestazioni ottimali da ASP.NET:

- Configurazione dei limiti di memoria processo di lavoro
- Il riciclo del processo di configurazione

### <a name="configuring-worker-process-memory-limits"></a>Configurazione dei limiti di memoria processo di lavoro

Per impostazione predefinita, IIS 6.0 non impostare un limite sulla quantità di memoria che IIS è possibile utilizzare. ASP. Funzionalità della Cache della rete si basa su un limite di memoria Cache potrà così in modo proattivo rimuovere elementi inutilizzati dalla memoria.

Si consiglia di configurare la memoria, il riciclo delle funzionalità di IIS 6.0. Per configurare questo aprire Gestione Internet Information Services (Start | I programmi | Strumenti di amministrazione | Internet Information Services). Una volta aperto, espandere la cartella 'Pool di applicazioni':

Per ogni pool di applicazioni:

![](aspnet-and-iis6/_static/image13.jpg)

1. Pulsante destro del mouse sul pool di applicazioni, ad esempio 'DefaultAppPool' e 'Properties' selezionare:

![](aspnet-and-iis6/_static/image14.jpg)

2. Successivamente, abilitare il riciclo memoria facendo clic su uno ' quantità massima di memoria utilizzata (in megabyte):'. Il valore non deve essere maggiore di memoria fisica (non virtuale) nel server, una buona approssimazione è 60% della memoria fisica, ad esempio per un server con 512MB di memoria fisica, selezionare 310. È inoltre consigliabile che il valore massimo non superi 800MB quando si utilizza uno spazio degli indirizzi di 2GB. Se lo spazio degli indirizzi di memoria del server è di 3GB, il limite di memoria massima per il processo di lavoro può essere a un massimo di 1, 800MB:

![](aspnet-and-iis6/_static/image15.jpg)

Fare clic su 'Applica' e 'OK' per uscire dalla finestra di dialogo proprietà. Ripetere questo passaggio per tutti i pool di applicazioni disponibili.

### <a name="configuring-worker-recycling"></a>Configurazione riciclo di lavoro

Per impostazione predefinita IIS 6.0 è configurato per riciclare il processo di lavoro 29 ore. Si tratta di un bit aggressiva per un'applicazione in esecuzione ASP.NET e, è consigliabile che il riciclo dei processi di lavoro automatico è disabilitato.

Per disabilitare il riciclo dei processi di lavoro automatico, innanzitutto aprire Gestione Internet Information Services (Start | I programmi | Strumenti di amministrazione | Internet Information Services). Una volta aperto, espandere la cartella 'Pool di applicazioni':

![](aspnet-and-iis6/_static/image16.jpg)

Per ogni pool di applicazioni:

1. Pulsante destro del mouse sul pool di applicazioni, ad esempio 'DefaultAppPool' e 'Properties' selezionare:

![](aspnet-and-iis6/_static/image17.jpg)

2. Deselezionare l'opzione ' Ricicla il processo (in minuti):':

![](aspnet-and-iis6/_static/image18.jpg)

Fare clic su 'Applica' e 'OK' per uscire dalla finestra di dialogo proprietà. Ripetere questo passaggio per tutti i pool di applicazioni disponibili.

## <a name="granting-write-access-to-the-file-system"></a>Concessione dell'accesso in scrittura nel file System

Se si utilizza NTFS, è necessario modificare l'elenco di controllo un accesso (ACL) per il file o una cartella per concedere l'accesso ASP.NET per l'applicazione richiede l'accesso in scrittura nel file System.

Ad esempio, per concedere ASP.NET accesso in scrittura per il c:\inetpub\wwwroot innanzitutto aprire Esplora e passare alla directory:

![](aspnet-and-iis6/_static/image19.jpg)

Successivamente, fare doppio clic sulla directory, ad esempio 'wwwroot' e scegliere Proprietà. Una volta aperta la finestra di dialogo proprietà, selezionare la scheda 'Security':

![](aspnet-and-iis6/_static/image20.jpg)

La directory c:\inetpub\wwwroot\ è una directory speciale in cui il gruppo di IIS 6.0 speciale ' IIS\_WPG' è già stata concessa lettura &amp; le autorizzazioni di esecuzione, visualizzazione contenuto cartella e lettura. Tuttavia, per concedere l'autorizzazione di scrittura, è necessario selezionare la casella di controllo Consenti per la scrittura:

![](aspnet-and-iis6/_static/image21.jpg)

IIS 6.0 include l'autorizzazione di scrittura per questa cartella. Per concedere autorizzazioni di scrittura in altre cartelle, eseguire la procedura, si noti che potrebbe essere necessario aggiungere IIS\_gruppo WPG se non esiste già.

> [!CAUTION]
> Concessione dell'autorizzazione di scrittura a IIS\_WPG consentirà di qualsiasi applicazione ASP.NET scrivere in questa directory.

## <a name="supporting-integrated-authentication-with-sql-server"></a>Supportano l'autenticazione integrata con SQL Server

L'autenticazione integrata consente di sfruttare l'autenticazione di Windows NT di SQL Server per convalidare gli account di accesso di SQL Server. Ciò consente all'utente di ignorare il processo di accesso di SQL Server standard. Con questo approccio, un utente di rete possa accedere a un database di SQL Server senza fornire un ID di accesso separato o una password, in quanto SQL Server Ottiene le informazioni sull'utente e password dal processo di sicurezza di rete di Windows NT.

Scelta dell'autenticazione integrata per le applicazioni ASP.NET è una buona scelta poiché credenziali non vengono mai archiviate all'interno della stringa di connessione per l'applicazione. Anziché la stringa di connessione utilizzata per connettersi a SQL apparirà come segue:

`"server=localhost; database=Northwind;Trusted_Connection=true"`

La stringa di connessione al Server di SQL per utilizzare le credenziali di Windows dell'applicazione, il tentativo di accedere a SQL Server. Nel caso di ASP.NET/IIS 6 questo sarebbe un account in IIS\_gruppo WPG.

Per abilitare l'autenticazione integrata tra ASP.NET e SQL Server, è necessario innanzitutto verificare che SQL Server sia configurato per l'autenticazione integrata oppure l'autenticazione in modalità mista, verificare con l'amministratore del database a tale scopo. Se SQL Server è in uno di questi due modalità, è possibile utilizzare l'autenticazione integrata.

Aprire SQL Server Enterprise Manager (Start | I programmi | Microsoft SQL Server | Enterprise Manager), selezionare il server appropriato ed espandere la cartella sicurezza:

![](aspnet-and-iis6/_static/image22.jpg)

Se ' BUILTINT\IIS\_WPG' gruppo non è elencato, fare clic su account di accesso e selezionare 'Nuovo account di accesso':

![](aspnet-and-iis6/_static/image23.jpg)

Nel ' nome:' casella di testo immettere ' [nome Server o dominio] \IIS\_WPG' o fare clic sul pulsante dei puntini di sospensione per aprire il selettore di utente/gruppo di Windows NT:

![](aspnet-and-iis6/_static/image24.jpg)

Selezionare IIS corrente del computer\_gruppo WPG e fare clic su 'Add' e su OK per chiudere il selettore.

È quindi necessario impostare anche il database predefinito e le autorizzazioni per accedere al database. Per impostare il database predefinito scegliere dall'elenco a discesa è selezionata, ad esempio Northwind riportato di seguito:

![](aspnet-and-iis6/_static/image25.jpg)

Successivamente, fare clic sulla scheda di accesso al Database:

![](aspnet-and-iis6/_static/image26.jpg)

Fare clic sulla casella di controllo Consenti per ogni database che si desidera consentire l'accesso a. È inoltre necessario selezionare i ruoli del database, controllo db\_proprietario garantisce l'account di accesso ha tutte le autorizzazioni necessarie per gestire e utilizzare il database selezionato.

Fare clic su OK per chiudere la finestra di dialogo proprietà. L'applicazione ASP.NET è ora configurato per supportare l'autenticazione integrata di SQL Server.

## <a name="dont-run-aspnet-10-in-iis-60-native-mode"></a>Versione 1.0 di ASP.NET non vengono eseguiti in modalità nativa di IIS 6.0

1.0 di ASP.NET in IIS 6.0 è supportata solo in modalità di compatibilità di IIS 5.

Per configurare ASP.NET 1.0 per l'esecuzione in modalità compatibilità IIS 5.0, aprire Gestione servizi Internet e fare clic con il pulsante destro siti Web e selezionare le proprietà:

![](aspnet-and-iis6/_static/image27.jpg)

Passare alla scheda servizio e controllare? Esegui il servizio WWW in modalità isolamento IIS 5.0:

![](aspnet-and-iis6/_static/image28.jpg)
