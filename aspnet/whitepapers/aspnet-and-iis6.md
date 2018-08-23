---
uid: whitepapers/aspnet-and-iis6
title: Esecuzione di ASP.NET 1.1 con IIS 6.0 | Microsoft Docs
author: rick-anderson
description: Mentre Windows Server 2003 include sia IIS 6.0 e ASP.NET 1.1, questi componenti sono disabilitati per impostazione predefinita. Questo white paper viene descritto come abilitare IIS 6.0 un...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: 5a5537bf-2aaa-49e7-839f-9e6522b829d8
msc.legacyurl: /whitepapers/aspnet-and-iis6
msc.type: content
ms.openlocfilehash: 38cd0abc1e9133b9b86cff6dd2759ce98ac5a115
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838969"
---
<a name="running-aspnet-11-with-iis-60"></a>Esecuzione di ASP.NET 1.1 con IIS 6.0
====================
> Mentre Windows Server 2003 include sia IIS 6.0 e ASP.NET 1.1, questi componenti sono disabilitati per impostazione predefinita. Questo white paper viene descritto come abilitare IIS 6.0 e ASP.NET 1.1 e consiglia di diverse impostazioni di configurazione per ottenere prestazioni ottimali da IIS e ASP.NET.
> 
> Si applica a IIS 6.0 e ASP.NET 1.1.


In ASP.NET 1.1 viene fornito con Windows Server 2003, che include anche la versione più recente di Internet Information Server (IIS) versione 6.0. IIS 6.0 e ASP.NET 1.1 sono progettati per integrarsi perfettamente e ASP.NET ora impostazioni predefinite per il nuovo modello di processo di lavoro di IIS 6.0.

## <a name="aspnet-11-is-not-installed-by-default"></a>In ASP.NET 1.1 non è installato per impostazione predefinita

Diversamente dalle versioni precedenti dei sistemi operativi server di Microsoft, Internet Information Server (IIS) non è abilitato per impostazione predefinita. né ASP.NET 1.1. Sono disponibili due opzioni per l'abilitazione di IIS:

### <a name="enabling-iis-option-1---configure-your-server-wizard"></a>Abilitazione della configurazione guidata Server IIS, #1 - opzione

Windows Server 2003 viene fornito un nuovo 'Configurazione guidata Server"che consentono di configurare correttamente il server in modalità desiderata.

Per avviare la procedura guidata: passare a nota, eseguire la procedura guidata è necessario essere l'accesso come amministratore:: Start | I programmi | Strumenti di amministrazione e selezionare "Configurazione guidata Server'.

Una volta selezionata visualizzata la schermata di apertura 'Configurazione guidata Server':

![](aspnet-and-iis6/_static/image1.jpg)

Fare clic su ' Avanti &gt;':

![](aspnet-and-iis6/_static/image2.jpg)

Fare clic su ' Avanti &gt;'

![](aspnet-and-iis6/_static/image3.jpg)

In questa schermata sarà necessario selezionare ' server di applicazioni (IIS, ASP.NET) come opzioni per la configurazione.

Fare clic su ' Avanti &gt;'.

![](aspnet-and-iis6/_static/image4.jpg)

Dopo aver selezionato per configurare il server come Server applicazioni, verrà visualizzata questa schermata che richiede le funzionalità aggiuntive devono essere installate. Nessuna delle due opzioni sono selezionata per impostazione predefinita. Per abilitare automaticamente ASP.NET, è necessario selezionare ' Abilita ASP. NET'.

Fare clic su ' Avanti &gt;'.

![](aspnet-and-iis6/_static/image5.jpg)

Questa schermata consente di visualizzare quali sono le opzioni di installazione.

Fare clic su ' Avanti &gt;'.

![](aspnet-and-iis6/_static/image6.jpg)

Le opzioni selezionate in corso l'installazione verrà visualizzato in questa schermata. È normale che altra finestra di dialogo verranno visualizzate finestre come servizi in corso l'installazione. Potrebbe inoltre essere richiesto per la posizione del CD di installazione di Windows 2003 Server.

Fare clic su ' Avanti &gt;' al termine.

![](aspnet-and-iis6/_static/image7.jpg)

Fare clic su 'Fine' - Windows Server 2003 è ora configurato per il supporto IIS 6.0 e ASP.NET 1.1.

### <a name="enabling-iis-option-2---manually-configuring-iis-and-aspnet"></a>L'attivazione di IIS, opzione #2 - configurare manualmente IIS e ASP.NET

Se non si desidera usare la "Configurazione guidata Server' è facoltativamente possibile installare IIS 6.0 e ASP.NET 1.1 tramite 'Installazione applicazioni' dal Pannello di controllo.

Prima di tutto, aprire il pannello di controllo:

![](aspnet-and-iis6/_static/image8.jpg)

Successivamente, fare clic su ' Aggiungi/Rimuovi Windows Components' che verrà aperto 'Windows Aggiunta guidata componenti di':

![](aspnet-and-iis6/_static/image9.jpg)

Evidenziare e selezionare "Server applicazione" e quindi fare clic su 'Dettagli'? Pulsante:

![](aspnet-and-iis6/_static/image10.jpg)

Per installare ASP.NET, selezionare ' ASP. NET'.

Fare clic su 'OK' per tornare all'aggiunta guidata componenti di Windows. Fare clic su ' Avanti &gt;' dall'Aggiunta guidata componenti di Windows per iniziare l'installazione:

![](aspnet-and-iis6/_static/image11.jpg)

È normale che altra finestra di dialogo verranno visualizzate finestre come servizi in corso l'installazione. Potrebbe inoltre essere richiesto per la posizione del CD di installazione di Windows 2003 Server.

Al termine dell'installazione verrà visualizzato l'ultima schermata di aggiunta guidata componenti di Windows:

![](aspnet-and-iis6/_static/image12.jpg)

IIS 6.0 e ASP.NET 1.1 sono ora configurati e disponibili.

## <a name="recommended-settings"></a>Impostazioni consigliate

Durante l'esecuzione di ASP.NET 1.1 con IIS 6.0 sono disponibili diverse impostazioni di configurazione consigliati per ottenere prestazioni ottimali da ASP.NET:

- Configurazione dei limiti di memoria processo di lavoro
- Configurazione riciclo dei processi di lavoro

### <a name="configuring-worker-process-memory-limits"></a>Configurazione dei limiti di memoria processo di lavoro

Per impostazione predefinita, IIS 6.0 non impostare un limite sulla quantità di memoria che è possibile utilizzare IIS. ASP. Funzionalità della Cache della rete si basa su un limite di memoria in modo che la Cache può rimuovere in modo proattivo gli elementi inusati dalla memoria.

È consigliabile configurare la memoria il riciclo delle funzionalità di IIS 6.0. Per configurare questo aprire Gestione Internet Information Services (Start | I programmi | Strumenti di amministrazione | Internet Information Services). Una volta aperto, espandere la cartella 'Pool di applicazioni':

Per ogni pool di applicazioni:

![](aspnet-and-iis6/_static/image13.jpg)

1. Fare clic sul pool di applicazioni, ad esempio "DefaultAppPool" e selezionare "proprietà":

![](aspnet-and-iis6/_static/image14.jpg)

2. Successivamente, abilitare il riciclo memoria facendo clic su uno ' quantità massima di memoria utilizzata (in megabyte):'. Il valore non deve essere superiore rispetto alla quantità di memoria fisica (non virtuale) nel server, una buona approssimazione è 60% della memoria fisica, ad esempio per un server con 512MB di memoria fisica, selezionare 310. È inoltre consigliabile che il valore massimo non superino 800MB quando si usa uno spazio degli indirizzi di 2GB. Se lo spazio degli indirizzi di memoria del server è 3GB, il limite di memoria massima per il processo di lavoro può essere fino alto 1, 800MB:

![](aspnet-and-iis6/_static/image15.jpg)

Fare clic su 'Applica' e 'OK' per uscire dalla finestra di dialogo proprietà. Ripetere questo passaggio per tutti i pool di applicazioni disponibili.

### <a name="configuring-worker-recycling"></a>Configurazione riciclo del ruolo di lavoro

Per impostazione predefinita IIS 6.0 è configurato per riciclare il processo di lavoro ogni 29 ore. Questo è un po' aggressivo per un'applicazione in esecuzione ASP.NET ed è consigliabile che il riciclo dei processi di lavoro automatico è disabilitato.

Per disabilitare il riciclo dei processi di lavoro automatico, aprire innanzitutto Gestione Internet Information Services (Start | I programmi | Strumenti di amministrazione | Internet Information Services). Una volta aperto, espandere la cartella 'Pool di applicazioni':

![](aspnet-and-iis6/_static/image16.jpg)

Per ogni pool di applicazioni:

1. Fare clic sul pool di applicazioni, ad esempio "DefaultAppPool" e selezionare "proprietà":

![](aspnet-and-iis6/_static/image17.jpg)

2. Deselezionare l'opzione ' Ocesso di lavoro (in minuti):':

![](aspnet-and-iis6/_static/image18.jpg)

Fare clic su 'Applica' e 'OK' per uscire dalla finestra di dialogo proprietà. Ripetere questo passaggio per tutti i pool di applicazioni disponibili.

## <a name="granting-write-access-to-the-file-system"></a>Concessione dell'accesso in scrittura nel file System

Se l'applicazione richiede l'accesso in scrittura nel file System e si utilizza è necessario modificare l'elenco di controllo un accesso (ACL) per il file o cartella per concedere l'accesso ASP.NET per NTFS.

Ad esempio, per concedere ASP.NET accesso in scrittura per il c:\inetpub\wwwroot innanzitutto aprire Esplora e passare alla directory:

![](aspnet-and-iis6/_static/image19.jpg)

Successivamente, fare doppio clic sulla directory, ad esempio 'wwwroot' e scegliere Proprietà. Quando si apre la finestra di dialogo proprietà, selezionare la scheda 'Sicurezza':

![](aspnet-and-iis6/_static/image20.jpg)

La directory c:\inetpub\wwwroot\ è una directory speciale in cui il gruppo speciale di IIS 6.0 ' IIS\_WPG' è già stata concessa lettura &amp; le autorizzazioni di esecuzione, visualizzazione contenuto cartella e lettura. Tuttavia, per concedere l'autorizzazione di scrittura, è necessario selezionare la casella di controllo Consenti per la scrittura:

![](aspnet-and-iis6/_static/image21.jpg)

IIS 6.0 è ora l'autorizzazione di scrittura per questa cartella. Per concedere le autorizzazioni di scrittura in altre cartelle, seguire questa procedura: tenere presente, potrebbe essere necessario aggiungere IIS\_gruppo WPG se non esiste già.

> [!CAUTION]
> Concessione dell'autorizzazione di scrittura per IIS\_WPG consentiranno di scrivere nella directory qualsiasi applicazione ASP.NET.

## <a name="supporting-integrated-authentication-with-sql-server"></a>Che supportano l'autenticazione integrata con SQL Server

L'autenticazione integrata consente per SQL Server sfruttare l'autenticazione di Windows NT per convalidare gli account di accesso di SQL Server. Ciò consente all'utente di ignorare il processo di accesso di SQL Server standard. Con questo approccio, un utente di rete possa accedere a un database di SQL Server senza specificare una password o un ID di accesso separato in quanto SQL Server Ottiene le informazioni sull'utente e password dal processo di sicurezza di rete di Windows NT.

Scelta dell'autenticazione integrata per le applicazioni ASP.NET è una buona scelta perché le credenziali non vengono mai archiviate all'interno della stringa di connessione per l'applicazione. Anziché la stringa di connessione utilizzata per connettersi a SQL apparirà come segue:

`"server=localhost; database=Northwind;Trusted_Connection=true"`

Questa stringa di connessione a SQL Server di usare le credenziali di Windows dell'applicazione tenta di accedere a SQL Server. Nel caso di ASP.NET/IIS 6 questo sarebbe un account in IIS\_gruppo WPG.

Per abilitare l'autenticazione integrata tra ASP.NET e SQL Server, è necessario innanzitutto verificare che SQL Server sia configurato per l'autenticazione integrata oppure l'autenticazione modalità mista: verificare con l'amministratore del database per determinare questo. Se SQL Server si trova in uno di questi due modalità, è possibile usare l'autenticazione integrata.

Aprire SQL Server Enterprise Manager (Start | I programmi | Microsoft SQL Server | Enterprise Manager), selezionare il server appropriato ed espandere la cartella sicurezza:

![](aspnet-and-iis6/_static/image22.jpg)

Se ' BUILTINT\IIS\_WPG' gruppo non è elencato, fare clic su account di accesso e selezionare 'Nuovo account di accesso':

![](aspnet-and-iis6/_static/image23.jpg)

Nel ' nome:' nella casella di testo immettere ' [nome Server/dominio] \IIS\_WPG' o fare clic sul pulsante dei puntini di sospensione per aprire la selezione di utenti o gruppi di Windows NT:

![](aspnet-and-iis6/_static/image24.jpg)

Seleziona IIS corrente del computer\_gruppo WPG e fare clic su 'Add' e su OK per chiudere lo strumento di selezione.

È quindi necessario impostare anche il database predefinito e le autorizzazioni per accedere al database. Per impostare il database predefinito scegliere dall'elenco a discesa sia selezionato, ad esempio Northwind riportato di seguito:

![](aspnet-and-iis6/_static/image25.jpg)

Fare quindi clic sulla scheda di accesso al Database:

![](aspnet-and-iis6/_static/image26.jpg)

Fare clic sulla casella di controllo Consenti per ogni database che si vuole consentire l'accesso a. È necessario anche per ruoli del database, selezionare il controllo db\_proprietario garantirà l'account di accesso dispone di tutte le autorizzazioni necessarie per gestire e utilizzare il database selezionato.

Fare clic su OK per chiudere la finestra di dialogo proprietà. L'applicazione ASP.NET è ora configurato per supportare l'autenticazione integrata di SQL Server.

## <a name="dont-run-aspnet-10-in-iis-60-native-mode"></a>Non eseguire ASP.NET 1.0 in modalità nativa di IIS 6.0

ASP.NET 1.0 in IIS 6.0 è supportato solo in modalità di compatibilità di IIS 5.

Per configurare ASP.NET 1.0 per l'esecuzione in modalità compatibilità IIS 5.0, aprire Gestione servizi Internet e fare clic con il pulsante destro siti Web e selezionare le proprietà:

![](aspnet-and-iis6/_static/image27.jpg)

Passare alla scheda del servizio e controllare? Eseguire il servizio WWW in modalità isolamento IIS 5.0?:

![](aspnet-and-iis6/_static/image28.jpg)
