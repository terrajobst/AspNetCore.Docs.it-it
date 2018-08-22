---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing
title: Configurare un Server di Database per il Web pubblicazione con distribuzione | Microsoft Docs
author: jrjlee
description: Questo argomento descrive come configurare un server di database di SQL Server 2008 R2 per supportare la pubblicazione e distribuzione web. Le attività descritte in questo argomento sono co...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: e7c447f9-eddf-4bbe-9f18-3326d965d093
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing
msc.type: authoredcontent
ms.openlocfilehash: 0f8639efcfcd02c9fdb65ce3ed25272965be8aa8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827078"
---
<a name="configuring-a-database-server-for-web-deploy-publishing"></a>Configurazione di un Server di Database per la pubblicazione con distribuzione Web
====================
da [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Questo argomento descrive come configurare un server di database di SQL Server 2008 R2 per supportare la pubblicazione e distribuzione web.
> 
> Le attività descritte in questo argomento sono comuni a tutti gli scenari di distribuzione&#x2014;non importa se i server web configurati per usare il servizio agente remoto di strumento di distribuzione Web IIS (distribuzione Web), il gestore di distribuzione Web o distribuzione non in linea con il applicazione è in esecuzione in un singolo server web o una farm di server. La modalità di distribuzione del database può cambiare in base ai requisiti di sicurezza e altre considerazioni. Ad esempio, è possibile distribuire il database con o senza dati di esempio e si potrebbe distribuire mapping dei ruoli utente o configurare manualmente dopo la distribuzione. Tuttavia, la modalità di configurazione il server di database rimane invariato.


Non è necessario installare altri prodotti o gli strumenti per la configurazione di un server di database per supportare la distribuzione di web. Supponendo che il server di database e server web eseguiti in computer diversi, è sufficiente:

- Consentire a SQL Server per comunicare tramite TCP/IP.
- Consentire il traffico di SQL Server da qualsiasi firewall.
- Assegnare l'account del computer server web di un account di accesso di SQL Server.
- Eseguire il mapping l'accesso dell'account computer per tutti i ruoli di database necessari.
- Assegnare all'account che verrà eseguita la distribuzione di un account di accesso e database creatore le autorizzazioni SQL Server.
- Per supportare distribuzioni ripetute, associare l'account di accesso di distribuzione per il **db\_proprietario** ruolo predefinito del database.

In questo argomento illustrerà come eseguire ognuna di queste procedure. Le attività e procedure dettagliate in questo argomento presuppongono che si sta iniziando con un'istanza predefinita di SQL Server 2008 R2 in esecuzione in Windows Server 2008 R2. Prima di continuare, verificare quanto segue:

- Windows Server 2008 R2 Service Pack 1 e tutti gli aggiornamenti disponibili sono installati.
- Il server viene aggiunto al dominio.
- Il server ha un indirizzo IP statico.
- Vengono installati SQL Server 2008 R2 Service Pack 1 e tutti gli aggiornamenti disponibili.

Istanza di SQL Server è sufficiente includere il **servizi motore di Database** ruolo, che è incluso automaticamente in tutte le installazioni di SQL Server. Tuttavia, per facilitare la configurazione e manutenzione, si consiglia di includere il **strumenti di gestione-di base** e **strumenti di gestione-completa** ruoli predefiniti del server.

> [!NOTE]
> Per ulteriori informazioni sull'aggiunta di computer a un dominio, vedere [aggiunta di computer al dominio ed effettuare l'accesso](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Per altre informazioni su come configurare gli indirizzi IP statici, vedere [configurare un indirizzo IP statico](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx). Per altre informazioni sull'installazione di SQL Server, vedere [installazione di SQL Server 2008 R2](https://technet.microsoft.com/library/bb500395.aspx).


## <a name="enable-remote-access-to-sql-server"></a>Abilitare l'accesso remoto a SQL Server

SQL Server utilizza TCP/IP per comunicare con computer remoti. Se il server di database e server web sono in computer diversi, è necessario:

- Configurare le impostazioni di rete SQL Server per consentire la comunicazione su TCP/IP.
- Configurare eventuali firewall hardware o software per consentire il traffico TCP (e in alcuni casi protocollo UDP (User Datagram) del traffico) sulle porte utilizzata dall'istanza di SQL Server.

Per abilitare SQL Server per comunicare su TCP/IP, utilizzare Gestione configurazione SQL Server per modificare la configurazione di rete per l'istanza di SQL Server.

**Per abilitare SQL Server per comunicare tramite TCP/IP**

1. Nel **avviare** dal menu **tutti i programmi**, fare clic su **Microsoft SQL Server 2008 R2**, fare clic su **gli strumenti di configurazione**e quindi fare clic su **Gestione configurazione SQL Server**.
2. Nel riquadro visualizzazione albero, espandere **configurazione di rete SQL Server**, quindi fare clic su **protocolli per MSSQLSERVER**.

   > [!NOTE]
   > Se è stato installato più istanze di SQL Server, si noterà una <strong>protocolli per</strong><em>[nome istanza]</em> elemento per ogni istanza. È necessario configurare le impostazioni di rete in una singola istanza-da-istanza.
3. Nel riquadro dei dettagli, fare doppio clic il **TCP/IP** riga e quindi fare clic su **abilitare**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image1.png)
4. Nel **avviso** finestra di dialogo, fare clic su **OK**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image2.png)
5. È necessario riavviare il servizio MSSQLSERVER per rendere effettive la nuova configurazione di rete. È possibile utilizzare un prompt dei comandi, dalla console dei servizi o da SQL Server Management Studio. In questa procedura si userà SQL Server Management Studio.
6. Chiudere Gestione configurazione SQL Server.
7. Nel **avviare** dal menu **tutti i programmi**, fare clic su **Microsoft SQL Server 2008 R2**, quindi fare clic su **SQL Server Management Studio**.
8. Nel **Connetti al Server** nella finestra di dialogo il **nome Server** casella, digitare il nome del server di database e quindi fare clic su **Connect**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image3.png)
9. Nel **Esplora oggetti** riquadro, fare clic sul nodo del server padre (ad esempio, **TESTDB1**) e quindi fare clic su **riavviare**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image4.png)
10. Nel **Microsoft SQL Server Management Studio** finestra di dialogo, fare clic su **Yes**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image5.png)
11. Quando il riavvio del servizio, chiudere SQL Server Management Studio.

Per consentire il traffico di SQL Server attraverso un firewall, è innanzitutto necessario conoscere quali porte Usa l'istanza di SQL Server. Ciò dipende come istanza di SQL Server è stata creata e configurata:

- Oggetto *istanza predefinita* di SQL Server resta in ascolto (e risponde a) le richieste sulla porta TCP 1433.
- Oggetto *istanza denominata* di SQL Server resta in ascolto (e risponde a) le richieste su una porta TCP assegnata dinamicamente.
- Se il servizio SQL Server Browser è abilitato, i client possono eseguire una query il servizio sulla porta UDP 1434 per individuare la porta TCP da utilizzare per una particolare istanza di SQL Server. Tuttavia, il servizio viene disabilitato spesso per motivi di sicurezza.

Supponendo che si usa un'istanza predefinita di SQL Server, è necessario configurare il firewall per consentire il traffico.

| Direzione | Dalla porta | Eseguire il porting | Tipo di porta |
| --- | --- | --- | --- |
| Connessioni in entrata | Qualsiasi | 1433 | TCP |
| In uscita | 1433 | Qualsiasi | TCP |
  

> [!NOTE]
> Tecnicamente, un computer client userà una porta TCP assegnata in modo casuale compreso tra 1024 e 5000 per comunicare con SQL Server e si possono limitare le regole del firewall di conseguenza. Per altre informazioni sulle porte di SQL Server e i firewall, vedere [numeri di porta TCP/IP necessari per comunicare con SQL su un firewall](https://go.microsoft.com/?linkid=9805125) e [procedura: configurare un Server per l'ascolto su una porta TCP specifica (configurazione di SQL Server Gestione)](https://msdn.microsoft.com/library/ms177440.aspx).


Nella maggior parte degli ambienti Windows Server, probabilmente è possibile configurare Windows Firewall nel server di database. Per impostazione predefinita, Windows Firewall consente tutto il traffico in uscita, a meno che una regola impedisce in modo specifico. Per consentire al server web di raggiungere il database, è necessario configurare una regola in ingresso che consenta il traffico TCP per il numero di porta utilizzato dall'istanza di SQL Server. Se si usa un'istanza predefinita di SQL Server, è possibile utilizzare la procedura successiva per configurare questa regola.

**Per configurare Windows Firewall per consentire la comunicazione con un'istanza di SQL Server predefinita**

1. Nel server di database, nel **avviare** dal menu **strumenti di amministrazione**, quindi fare clic su **Windows Firewall con sicurezza avanzata**.
2. Nel riquadro visualizzazione albero, fare clic su **regole connessioni in entrata**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image6.png)
3. Nel **azioni** riquadro, di sotto **regole connessioni in entrata**, fare clic su **nuova regola**.
4. Nelle connessioni in entrata Creazione guidata nuova regola, nelle **tipo di regola** pagina, selezionare **porta**e quindi fare clic su **Avanti**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image7.png)
5. Nel **protocollo e porte** pagina, assicurarsi che **TCP** è selezionata e nel **porte locali specifiche** digitare **1433**e quindi fare clic su **Successivo**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image8.png)
6. Nel **azione** pagina, lasciare **Consenti solo connessioni** selezionata e fare clic su **Avanti**.
7. Nel **profilo** pagina, lasciare **dominio** selezionata, deselezionare il **Private** e **pubblico** caselle di controllo e quindi fare clic su  **Avanti**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image9.png)
8. Nel **Name** pagina, assegnare alla regola un nome descrittivo adeguatamente (ad esempio, **istanza predefinita di SQL Server: l'accesso alla rete**) e quindi fare clic su **fine**.

Per altre informazioni su come configurare Windows Firewall per SQL Server, in particolare se è necessario per comunicare con SQL Server sulle porte non standard o dinamiche, vedere [procedura: configurare un Firewall di Windows per l'accesso al motore di Database](https://technet.microsoft.com/library/ms175043.aspx).

## <a name="configure-logins-and-database-permissions"></a>Configurare gli account di accesso e autorizzazioni di Database

Quando si distribuisce un'applicazione web per Internet Information Services (IIS), l'applicazione viene eseguita usando l'identità del pool di applicazioni. In un ambiente di dominio, le identità del pool di applicazioni utilizzano l'account del computer del server in cui vengono eseguite per accedere alle risorse di rete. Gli account Machine assumono la forma <em>[nome dominio]</em><strong>\</ strong ><em>[nome macchina]</em><strong>$</strong>&#x2014;, ad esempio, <strong>FABRIKAM\TESTWEB1$</strong>. Per consentire all'applicazione web accedere a un database attraverso la rete, è necessario:

- Aggiungere un account di accesso per l'account del computer server web per l'istanza di SQL Server.
- Eseguire il mapping per tutti i ruoli di database necessari l'accesso dell'account computer (in genere **db\_datareader** e **db\_datawriter**).

Se l'applicazione web è in esecuzione in una server farm, anziché un singolo server, è necessario ripetere queste procedure per tutti i server web nella server farm.

> [!NOTE]
> Per altre informazioni su identità del pool di applicazioni e accesso alle risorse di rete, vedere [Application Pool Identities](https://go.microsoft.com/?linkid=9805123).


È possibile eseguire queste attività in vari modi. Per creare l'account di accesso, è possibile:

- Creare manualmente l'account di accesso nel server di database utilizzando Transact-SQL o SQL Server Management Studio.
- Usare un progetto Server di SQL Server 2008 in Visual Studio per creare e distribuire l'account di accesso.

Un account di accesso di SQL Server è un oggetto a livello di server, anziché un oggetto a livello di database, pertanto non è dipendente nel database di cui che si vuole distribuire. Di conseguenza, è possibile creare l'account di accesso in qualsiasi momento e l'approccio più semplice è spesso per creare manualmente l'account di accesso nel server di database prima di iniziare la distribuzione di database. È possibile utilizzare la procedura seguente per creare un account di accesso in SQL Server Management Studio.

**Per creare un account di accesso di SQL Server per l'account del computer server web**

1. Nel server di database, nel **avviare** dal menu **tutti i programmi**, fare clic su **Microsoft SQL Server 2008 R2**, quindi fare clic su **SQL Server Management Studio** .
2. Nel **Connetti al Server** nella finestra di dialogo il **nome Server** casella, digitare il nome del server di database e quindi fare clic su **Connect**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image10.png)
3. Nel **Esplora oggetti** riquadro, fare doppio clic su **Security**, scegliere **New**e quindi fare clic su **Login**.
4. Nel **accesso-nuovo** nella finestra di dialogo il **nome account di accesso** , digitare il nome dell'account computer del server web (ad esempio, **FABRIKAM\TESTWEB1$**).

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image11.png)
5. Fare clic su **OK**.

A questo punto, il server di database è pronto per la pubblicazione di distribuzione Web. Tuttavia, eventuali soluzioni distribuite non funzioneranno fino a quando non si esegue il mapping l'accesso dell'account computer per i ruoli di database necessari. Mapping di account di accesso ai ruoli del database richiede molti più pensato, come si non è possibile mappa ruoli fino a dopo aver aver distribuiti il database. Per mappare l'accesso dell'account computer per i ruoli di database necessari, è possibile:

- Assegnare i ruoli del database all'account di accesso manualmente, dopo aver distribuito il database per la prima volta.
- Usare uno script di post-distribuzione per assegnare i ruoli del database all'account di accesso.

Per altre informazioni su come automatizzare la creazione di account di accesso e di mapping dei ruoli di database, vedere [distribuzione di Database le appartenenze ai ruoli per gli ambienti di Test](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). In alternativa, è possibile utilizzare la procedura successiva per il mapping manualmente l'accesso dell'account computer per i ruoli di database necessari. Tenere presente che è possibile eseguire questa procedura finché *dopo* è stato distribuito il database.

**Per eseguire il mapping dei ruoli di database per l'accesso dell'account computer server web**

1. Aprire SQL Server Management Studio come indicato in precedenza.
2. Nel **Esplora oggetti** riquadro, espandere il **sicurezza** nodo, espandere il **gli account di accesso** nodo e quindi fare doppio clic sull'accesso dell'account computer (ad esempio,  **$ FABRIKAM\TESTWEB1**).

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image12.png)
3. Nel **proprietà account di accesso** finestra di dialogo, fare clic su **Mapping utenti**.
4. Nel **utenti mappati all'account di accesso seguente** tabella, selezionare il nome del database (ad esempio, **ContactManager**).
5. Nel **Database di appartenenza al ruolo per:** *[nome database]* selezionare le autorizzazioni necessarie. Nel caso la soluzione di esempio Contact Manager, è necessario selezionare la **db\_datareader** e **db\_datawriter** ruoli.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image13.png)
6. Fare clic su **OK**.

Sebbene manualmente il mapping di ruoli predefiniti del database è spesso più che sufficiente per gli ambienti di test, è meno consigliato per distribuzioni automatizzate o con un clic agli ambienti di gestione temporanea o produzione. È possibile trovare altre informazioni su come automatizzare questo tipo di attività usando script di post-distribuzione in [distribuzione di Database le appartenenze ai ruoli per gli ambienti di Test](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md).

> [!NOTE]
> Per altre informazioni sui progetti server e progetti di database, vedere [progetti di Visual Studio 2010 SQL Server Database](https://msdn.microsoft.com/library/ff678491.aspx).


## <a name="configure-permissions-for-the-deployment-account"></a>Configurare le autorizzazioni per l'Account di distribuzione

Se l'account che si userà per eseguire la distribuzione non è un amministratore di SQL Server, è necessario anche creare un account di accesso per questo account. Per creare il database, l'account deve essere un membro del **dbcreator** ruolo del server o avere autorizzazioni equivalenti.

> [!NOTE]
> Quando si usa distribuzione Web o VSDBCMD per distribuire un database, è possibile usare le credenziali di Windows o le credenziali di SQL Server (se l'istanza di SQL Server è configurata per supportare l'autenticazione modalità mista). La procedura seguente si presuppone che si desidera utilizzare le credenziali di Windows, ma non c'è nulla che vieti di specificare un nome utente di SQL Server e una password nella stringa di connessione quando si configura la distribuzione.


**Per impostare le autorizzazioni per l'account di distribuzione**

1. Aprire SQL Server Management Studio come indicato in precedenza.
2. Nel **Esplora oggetti** riquadro, fare doppio clic su **Security**, scegliere **New**e quindi fare clic su **Login**.
3. Nel **accesso-nuovo** nella finestra di dialogo il **nome account di accesso** , digitare il nome dell'account di distribuzione (ad esempio, **FABRIKAM\matt**).
4. Nel **selezione pagina** riquadro, fare clic su **ruoli Server**.
5. Selezionare **dbcreator**, quindi fare clic su **OK**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image14.png)

Per supportare le distribuzioni successive, è anche necessario aggiungere l'account di distribuzione per il **db\_proprietario** ruolo nel database dopo la prima distribuzione. Questo avviene perché nelle successive distribuzioni si sta la modifica dello schema di un database esistente, anziché creare un nuovo database. Come descritto nella sezione precedente, è possibile aggiungere un utente a un ruolo del database fino a quando non è stato creato il database, per ovvie ragioni.

**Per eseguire il mapping di account di accesso di distribuzione al database\_ruolo del database proprietario**

1. Aprire SQL Server Management Studio come indicato in precedenza.
2. Nel **Esplora oggetti** finestra, espandere il **sicurezza** nodo, espandere il **gli account di accesso** nodo e quindi fare doppio clic sull'accesso dell'account computer (ad esempio,  **FABRIKAM\matt**).
3. Nel **proprietà account di accesso** finestra di dialogo, fare clic su **Mapping utenti**.
4. Nel **utenti mappati all'account di accesso seguente** tabella, selezionare il nome del database (ad esempio, **ContactManager**).
5. Nel **Database di appartenenza al ruolo per:** *[nome database]* elenco, selezionare il **db\_proprietario** ruolo.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image15.png)
6. Fare clic su **OK**.

## <a name="conclusion"></a>Conclusione

Il server di database dovrebbe ora essere pronto per accettare le distribuzioni di database remoto e per consentire remoto ai server web IIS accedere ai database. Prima di provare a distribuire e usare i database, è possibile controllare questi punti importanti:

- È stato configurato SQL Server per accettare le connessioni TCP/IP remote?
- Sono state configurate eventuali firewall per consentire il traffico di SQL Server?
- È stato creato un account di accesso account computer per tutti i server web che accederà a SQL Server?
- La distribuzione di database include uno script per creare mapping del ruolo utente, o è necessario crearle manualmente dopo aver distribuito il database per la prima volta?
- Avere creato un account di accesso per l'account di distribuzione e aggiunto per il **dbcreator** ruolo del server?

## <a name="further-reading"></a>Ulteriori informazioni

Per indicazioni sulla distribuzione di progetti di database, vedere [distribuire progetti di Database](../web-deployment-in-the-enterprise/deploying-database-projects.md). Per istruzioni sulla creazione di database le appartenenze ai ruoli eseguendo uno script di post-distribuzione, vedere [distribuzione di Database le appartenenze ai ruoli per gli ambienti di Test](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Per indicazioni su come soddisfare le esigenze di distribuzione univoco che implicano i database di appartenenza, vedere [la distribuzione dei database di appartenenza negli ambienti aziendali](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

> [!div class="step-by-step"]
> [Precedente](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
> [Successivo](creating-a-server-farm-with-the-web-farm-framework.md)
