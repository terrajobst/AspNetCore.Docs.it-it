---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing
title: Pubblicazione di distribuzione di un Server di Database di configurazione per il Web | Documenti Microsoft
author: jrjlee
description: "In questo argomento viene descritto come configurare un server di database di SQL Server 2008 R2 per il supporto di pubblicazione e distribuzione web. Le attività descritte in questo argomento sono co..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: e7c447f9-eddf-4bbe-9f18-3326d965d093
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing
msc.type: authoredcontent
ms.openlocfilehash: 98fd728f48f6fb64a61686bc58824b9fb3a28b13
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/15/2018
---
<a name="configuring-a-database-server-for-web-deploy-publishing"></a>Configurazione di un Server di Database per la pubblicazione di distribuzione Web
====================
da [Jason Lee](https://github.com/jrjlee)

[Scarica il PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene descritto come configurare un server di database di SQL Server 2008 R2 per il supporto di pubblicazione e distribuzione web.
> 
> Le attività descritte in questo argomento sono comuni a ogni scenario di distribuzione & #x 2014; non è importante se i server web sono configurati per usare il servizio agente remoto di strumento di distribuzione Web di IIS (distribuzione Web), il gestore di distribuzione Web o distribuzione non in linea o l'applicazione è in esecuzione in un singolo server web o una farm di server. Il modo in cui si distribuisce il database può variare in base ai requisiti di sicurezza e altre considerazioni. Ad esempio, è possibile distribuire il database con o senza dati di esempio e, è possibile distribuire i mapping dei ruoli utente o configurare manualmente dopo la distribuzione. Tuttavia, la modalità di configurazione il server di database rimane invariato.


Non è necessario installare altri prodotti o strumenti di configurazione di un server di database per supportare la distribuzione di web. Supponendo che il server di database e server web vengono eseguiti in computer diversi, è sufficiente:

- Consentire a SQL Server per comunicare tramite TCP/IP.
- Consentire il traffico di SQL Server da qualsiasi firewall in uso.
- Assegnare l'account del computer server web di un account di accesso di SQL Server.
- Eseguire il mapping di accesso dell'account computer a ruoli di database necessari.
- Assegnare all'account che eseguiranno la distribuzione di un account di accesso e database creator le autorizzazioni SQL Server.
- Per supportare le distribuzioni di ripetere l'operazione, eseguire il mapping di accesso dell'account di distribuzione per il **db\_proprietario** ruolo del database.

Questo argomento viene illustrato come eseguire ognuna di queste procedure. Le attività e procedure dettagliate in questo argomento si presuppongono che si sta iniziando con un'istanza predefinita di SQL Server 2008 R2 in esecuzione in Windows Server 2008 R2. Prima di continuare, verificare quanto segue:

- Windows Server 2008 R2 Service Pack 1 e tutti gli aggiornamenti disponibili vengono installati.
- Il server è aggiunto al dominio.
- Il server ha un indirizzo IP statico.
- Vengono installati SQL Server 2008 R2 Service Pack 1 e tutti gli aggiornamenti disponibili.

Solo l'istanza di SQL Server deve includere il **servizi motore di Database** ruolo, viene inclusa automaticamente in tutte le installazioni di SQL Server. Tuttavia, per facilitare la configurazione e manutenzione, è consigliabile includere il **strumenti di gestione-di base** e **strumenti di gestione-completa** i ruoli del server.

> [!NOTE]
> Per ulteriori informazioni sull'aggiunta di computer a un dominio, vedere [aggiunta di computer al dominio e registrazione](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Per ulteriori informazioni sulla configurazione di indirizzi IP statici, vedere [configurare un indirizzo IP statico](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx). Per ulteriori informazioni sull'installazione di SQL Server, vedere [l'installazione di SQL Server 2008 R2](https://technet.microsoft.com/library/bb500395.aspx).


## <a name="enable-remote-access-to-sql-server"></a>Abilitare l'accesso remoto a SQL Server

SQL Server utilizza TCP/IP per comunicare con i computer remoti. Se il server di database e server web si trovano in computer diversi, è necessario:

- Configurare le impostazioni di rete SQL Server per consentire la comunicazione su TCP/IP.
- Configurare eventuali firewall hardware o software per consentire il traffico TCP (e in alcuni casi il protocollo UDP (User Datagram) del traffico) sulle porte utilizzate dall'istanza di SQL Server.

Per abilitare SQL Server per comunicare su TCP/IP, utilizzare Gestione configurazione SQL Server per modificare la configurazione di rete per l'istanza di SQL Server.

**Per abilitare SQL Server comunicare tramite TCP/IP**

1. Nel **avviare** dal menu **tutti i programmi**, fare clic su **Microsoft SQL Server 2008 R2**, fare clic su **strumenti di configurazione**, quindi fare clic su **Gestione configurazione SQL Server**.
2. Nel riquadro di visualizzazione albero, espandere **configurazione di rete SQL Server**, quindi fare clic su **protocolli per MSSQLSERVER**.

    > [!NOTE]
    > Se più istanze di SQL Server installato, verrà visualizzato un **protocolli per * * * [nome istanza]* elemento per ogni istanza. È necessario configurare le impostazioni di rete in modo da-istanza.
3. Nel riquadro dei dettagli fare doppio clic su di **TCP/IP** riga e quindi fare clic su **abilitare**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image1.png)
4. Nel **avviso** la finestra di dialogo, fare clic su **OK**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image2.png)
5. È necessario riavviare il servizio MSSQLSERVER rendere effettiva la nuova configurazione di rete. È possibile farlo in un prompt dei comandi, dalla console di servizi o da SQL Server Management Studio. In questa procedura, userai SQL Server Management Studio.
6. Chiudere Gestione configurazione SQL Server.
7. Nel **avviare** dal menu **tutti i programmi**, fare clic su **Microsoft SQL Server 2008 R2**, quindi fare clic su **SQL Server Management Studio**.
8. Nel **Connetti al Server** della finestra di dialogo di **nome Server** casella, digitare il nome del server di database e quindi fare clic su **Connetti**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image3.png)
9. Nel **Esplora oggetti** riquadro destro del mouse il nodo del server padre (ad esempio, **TESTDB1**) e quindi fare clic su **riavviare**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image4.png)
10. Nel **Microsoft SQL Server Management Studio** la finestra di dialogo, fare clic su **Sì**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image5.png)
11. Dopo il riavvio del servizio, chiudere SQL Server Management Studio.

Per consentire il traffico di SQL Server attraverso un firewall, è innanzitutto necessario conoscere quali porte utilizza l'istanza di SQL Server. Questo dipende come istanza di SQL Server è stato creato e configurato:

- Oggetto *istanza predefinita* di SQL Server è in ascolto per (e risponde a) le richieste sulla porta TCP 1433.
- Oggetto *istanza denominata* di SQL Server è in ascolto per (e risponde a) le richieste su una porta TCP assegnata dinamicamente.
- Se il servizio SQL Server Browser sia abilitato, i client possono eseguire una query il servizio sulla porta UDP 1434 per individuare la porta TCP da utilizzare per una particolare istanza di SQL Server. Tuttavia, il servizio viene disabilitato spesso per motivi di sicurezza.

Supponendo che si utilizza un'istanza predefinita di SQL Server, è necessario configurare il firewall per consentire il traffico.

| Direzione | Dalla porta | Alla porta | Tipo di porta |
| --- | --- | --- | --- |
| In ingresso | Qualsiasi | 1433 | TCP |
| In uscita | 1433 | Qualsiasi | TCP |
  

> [!NOTE]
> Tecnicamente, un computer client userà una porta TCP assegnata in modo casuale compreso tra 1024 e 5000 per comunicare con SQL Server e che è possibile limitare le regole del firewall, di conseguenza. Per ulteriori informazioni sulle porte di SQL Server e i firewall, vedere [i numeri di porta TCP/IP necessari per comunicare con SQL attraverso un firewall](https://go.microsoft.com/?linkid=9805125) e [procedura: configurare un Server per l'attesa su una porta TCP specifica (configurazione di SQL Server Gestione)](https://msdn.microsoft.com/library/ms177440.aspx).


Nella maggior parte degli ambienti Windows Server, sarà probabilmente necessario configurare Windows Firewall nel server di database. Per impostazione predefinita, Windows Firewall consente tutto il traffico in uscita, a meno che una regola impedisca esplicitamente. Per abilitare il server web raggiungere il database, è necessario configurare una regola in ingresso che consenta il traffico TCP sul numero di porta utilizzato dall'istanza di SQL Server. Se si utilizza un'istanza predefinita di SQL Server, è possibile utilizzare la procedura seguente per configurare questa regola.

**Per configurare Windows Firewall per consentire la comunicazione con un'istanza di SQL Server predefinita**

1. Nel server di database, nel **avviare** dal menu **strumenti di amministrazione**, quindi fare clic su **Windows Firewall con sicurezza avanzata**.
2. Nel riquadro di visualizzazione albero, fare clic su **regole connessioni in entrata**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image6.png)
3. Nel **azioni** riquadro, in **regole connessioni in entrata**, fare clic su **nuova regola**.
4. Nelle connessioni in entrata Creazione guidata nuova regola, nel **tipo di regola** selezionare **porta**e quindi fare clic su **Avanti**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image7.png)
5. Nel **protocollo e porte** pagina, assicurarsi che **TCP** è selezionata e il **porte locali specifiche** , digitare **1433**e quindi fare clic su **Avanti**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image8.png)
6. Nel **azione** , mantenere **Consenti la connessione** selezionata e fare clic su **Avanti**.
7. Nel **profilo** , mantenere **dominio** selezionata, deselezionare il **privata** e **pubblica** caselle di controllo e quindi fare clic su  **Avanti**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image9.png)
8. Nel **nome** pagina, assegnare un nome descrittivo adeguatamente la regola (ad esempio, **istanza predefinita di SQL Server: accesso alla rete**), quindi fare clic su **fine**.

Per ulteriori informazioni su come configurare Windows Firewall per SQL Server, in particolare se è necessario per comunicare con SQL Server tramite le porte non standard o dinamiche, vedere [procedura: configurazione di Windows Firewall per l'accesso al motore di Database](https://technet.microsoft.com/library/ms175043.aspx).

## <a name="configure-logins-and-database-permissions"></a>Configurare account di accesso e autorizzazioni di Database

Quando si distribuisce un'applicazione web di Internet Information Services (IIS), l'applicazione viene eseguita utilizzando l'identità del pool di applicazioni. In un ambiente di dominio, le identità del pool di applicazioni utilizzano l'account computer del server in cui vengono eseguite per accedere alle risorse di rete. Gli account computer assumono la forma * [nome dominio]***\** * [nome macchina]***$ * * & #x 2014; ad esempio, **FABRIKAM\TESTWEB1$**. Per consentire all'applicazione web accedere a un database attraverso la rete, è necessario:

- Aggiungere un account di accesso per l'account del computer server web per l'istanza di SQL Server.
- Eseguire il mapping di accesso dell'account computer a ruoli di database necessari (in genere **db\_datareader** e **db\_datawriter**).

Se l'applicazione web è in esecuzione in una server farm, anziché un singolo server, è necessario ripetere queste procedure per ogni server web nella server farm.

> [!NOTE]
> Per ulteriori informazioni sull'identità del pool di applicazioni e accesso alle risorse di rete, vedere [le identità del Pool di applicazioni](https://go.microsoft.com/?linkid=9805123).


È possibile eseguire queste attività in vari modi. Per creare l'account di accesso, è possibile:

- Creare manualmente l'account di accesso nel server di database, utilizzando Transact-SQL o SQL Server Management Studio.
- Utilizzare un progetto Server di SQL Server 2008 in Visual Studio per creare e distribuire l'account di accesso.

Un account di accesso di SQL Server è un oggetto a livello di server, anziché un oggetto a livello di database, pertanto non dipendente dal database di cui che si desidera distribuire. Di conseguenza, è possibile creare l'account di accesso in qualsiasi momento e l'approccio più semplice è spesso per creare manualmente l'account di accesso nel server di database prima di iniziare la distribuzione di database. È possibile utilizzare la procedura seguente per creare un account di accesso in SQL Server Management Studio.

**Per creare un account di accesso di SQL Server per l'account del computer server web**

1. Nel server di database, nel **avviare** dal menu **tutti i programmi**, fare clic su **Microsoft SQL Server 2008 R2**, quindi fare clic su **SQL Server Management Studio** .
2. Nel **Connetti al Server** della finestra di dialogo di **nome Server** casella, digitare il nome del server di database e quindi fare clic su **Connetti**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image10.png)
3. Nel **Esplora oggetti** riquadro destro **sicurezza**, scegliere **New**e quindi fare clic su **accesso**.
4. Nel **nuovo account di accesso** della finestra di dialogo di **nome account di accesso** , digitare il nome dell'account di computer server web (ad esempio, **FABRIKAM\TESTWEB1$**).

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image11.png)
5. Fare clic su **OK**.

A questo punto, il server di database è pronto per la pubblicazione di distribuzione Web. Tuttavia, le soluzioni di distribuire non funzioneranno fino a quando non si esegue il mapping di accesso dell'account computer per i ruoli del database necessari. Mapping dell'account di accesso ai ruoli del database richiede molto più considerato, come si non mappa ruoli fino a dopo aver aver distribuiti il database. Per eseguire il mapping di accesso dell'account computer per i ruoli di database richiesti, è possibile:

- Assegnare i ruoli del database all'account di accesso manualmente, dopo aver distribuito il database per la prima volta.
- Utilizzare uno script di post-distribuzione per assegnare i ruoli del database all'account di accesso.

Per ulteriori informazioni su come automatizzare la creazione di account di accesso e i mapping dei ruoli di database, vedere [distribuzione appartenenze al ruolo per gli ambienti di Test](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). In alternativa, è possibile utilizzare la procedura seguente per eseguire il mapping di accesso dell'account computer per i ruoli di database richiesti manualmente. Tenere presente che è possibile eseguire questa procedura finché *dopo* aver distribuito il database.

**Per eseguire il mapping dei ruoli di database per l'accesso di account computer server web**

1. Aprire SQL Server Management Studio, come in precedenza.
2. Nel **Esplora oggetti** riquadro espandere il **sicurezza** nodo, espandere il **gli account di accesso** nodo e quindi fare doppio clic su account di accesso di account computer (ad esempio,  **$ FABRIKAM\TESTWEB1**).

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image12.png)
3. Nel **proprietà account di accesso** la finestra di dialogo, fare clic su **Mapping utente**.
4. Nel **utenti mappati all'account di accesso** tabella, selezionare il nome del database (ad esempio, **ContactManager**).
5. Nel **l'appartenenza al ruolo di Database:** *[nome database]* selezionare le autorizzazioni necessarie. Nel caso la soluzione di esempio Contact Manager, è necessario selezionare il **db\_datareader** e **db\_datawriter** ruoli.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image13.png)
6. Fare clic su **OK**.

Sebbene mapping manualmente i ruoli del database è spesso più che sufficiente per gli ambienti di test, è meno utile per le distribuzioni automatica o con un clic agli ambienti di produzione o gestione temporanea. È possibile trovare ulteriori informazioni su come automatizzare questo tipo di attività con gli script di post-distribuzione in [distribuzione appartenenze al ruolo per gli ambienti di Test](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md).

> [!NOTE]
> Per ulteriori informazioni sui progetti di server e progetti di database, vedere [progetti di Database di Visual Studio 2010 SQL Server](https://msdn.microsoft.com/library/ff678491.aspx).


## <a name="configure-permissions-for-the-deployment-account"></a>Configurare le autorizzazioni per l'Account di distribuzione

Se l'account che verrà utilizzato per eseguire la distribuzione non è un amministratore di SQL Server, è necessario anche creare un account di accesso per questo account. Per creare il database, l'account deve essere un membro del **dbcreator** ruolo del server o disporre di autorizzazioni equivalenti.

> [!NOTE]
> Quando si utilizza distribuzione Web o VSDBCMD per distribuire un database, è possibile utilizzare le credenziali di Windows o credenziali di SQL Server (se l'istanza di SQL Server è configurato per supportare l'autenticazione modalità mista). La procedura seguente si presuppone che si desidera utilizzare le credenziali di Windows, ma non è necessario eseguire l'arresto è specificare un nome utente di SQL Server e una password nella stringa di connessione quando si configura la distribuzione.


**Per impostare le autorizzazioni per l'account di distribuzione**

1. Aprire SQL Server Management Studio, come in precedenza.
2. Nel **Esplora oggetti** riquadro destro **sicurezza**, scegliere **New**e quindi fare clic su **accesso**.
3. Nel **nuovo account di accesso** della finestra di dialogo di **nome account di accesso** , digitare il nome dell'account di distribuzione (ad esempio, **FABRIKAM\matt**).
4. Nel **selezione pagina** riquadro, fare clic su **i ruoli del Server**.
5. Selezionare **dbcreator**, quindi fare clic su **OK**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image14.png)

Per supportare le distribuzioni successive, è inoltre necessario aggiungere l'account di distribuzione per il **db\_proprietario** ruolo nel database dopo la prima distribuzione. In questo modo nelle distribuzioni successive si sta la modifica dello schema di un database esistente, anziché creare un nuovo database. Come descritto nella sezione precedente, è possibile aggiungere un utente a un ruolo del database fino a creare il database, per motivi di ovvi.

**Per eseguire il mapping di account di accesso di distribuzione al database\_ruolo proprietario del database**

1. Aprire SQL Server Management Studio, come in precedenza.
2. Nel **Esplora oggetti** finestra, espandere il **sicurezza** nodo, espandere il **gli account di accesso** nodo e quindi fare doppio clic su account di accesso di account computer (ad esempio,  **FABRIKAM\matt**).
3. Nel **proprietà account di accesso** la finestra di dialogo, fare clic su **Mapping utente**.
4. Nel **utenti mappati all'account di accesso** tabella, selezionare il nome del database (ad esempio, **ContactManager**).
5. Nel **l'appartenenza al ruolo di Database:** *[nome database]* elenco, selezionare il **db\_proprietario** ruolo.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image15.png)
6. Fare clic su **OK**.

## <a name="conclusion"></a>Conclusione

Il server di database dovrebbe ora essere pronto per accettare le distribuzioni di database remoto e per consentire ai server web IIS remoto accedere ai database. Prima di tentare di distribuire e utilizzare i database, si consiglia di verificare i seguenti punti chiave:

- Sono state configurate SQL Server per accettare le connessioni TCP/IP remote?
- È stato configurato alcun firewall per consentire il traffico di SQL Server?
- È stato creato un account computer per ogni server web che accederà a SQL Server?
- La distribuzione di database include uno script per creare i mapping dei ruoli utente, o è necessario crearle manualmente dopo aver distribuito il database per la prima volta?
- Un account di accesso per l'account di distribuzione creato e aggiunto al **dbcreator** ruolo del server?

## <a name="further-reading"></a>Ulteriori informazioni

Per istruzioni sulla distribuzione di progetti di database, vedere [progetti di Database di distribuzione](../web-deployment-in-the-enterprise/deploying-database-projects.md). Per istruzioni sulla creazione di appartenenza ai ruoli di database eseguendo uno script di post-distribuzione, vedere [distribuzione appartenenze al ruolo per gli ambienti di Test](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Per istruzioni su come soddisfare le esigenze di distribuzione univoca che implicano i database di appartenenza, vedere [la distribuzione di database di appartenenza per ambienti aziendali](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

>[!div class="step-by-step"]
[Precedente](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
[Successivo](creating-a-server-farm-with-the-web-farm-framework.md)
