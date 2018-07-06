---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent
title: Configurazione di un Server Web per il Web (agente remoto) pubblicazione con distribuzione | Microsoft Docs
author: jrjlee
description: Questo argomento descrive come configurare un server web Internet Information Services (IIS) per supportare la distribuzione usando la distribuzione Web IIS e la pubblicazione sul web...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 239c7aa8-d09a-4d02-9c0e-6bd52be5f0d5
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent
msc.type: authoredcontent
ms.openlocfilehash: 348c618fe6ec726e8087b4f3acb66cddbef1d225
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819643"
---
<a name="configuring-a-web-server-for-web-deploy-publishing-remote-agent"></a>Configurazione di un Server Web per la distribuzione Web pubblicazione (agente remoto)
====================
da [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Questo argomento descrive come configurare un server web Internet Information Services (IIS) per supportare la pubblicazione sul web e la distribuzione usando il servizio agente remoto di strumento di distribuzione Web IIS (distribuzione Web).
> 
> Quando si lavora con distribuzione Web 2.0 o versione successiva, esistono tre approcci principali, che è possibile usare per ottenere le applicazioni o i siti in un server web. È possibile:
> 
> - Usare la *assistenza remota dell'agente di distribuzione Web*. Questo approccio implica una configurazione del server web, ma è necessario fornire le credenziali dell'amministratore del server locale per distribuire qualsiasi elemento nel server.
> - Usare la *gestore distribuzione Web*. Questo approccio è molto più complesso e richiede più di impegno iniziale a configurare il server web. Tuttavia, quando si usa questo approccio, è possibile configurare IIS per consentire agli utenti senza privilegi di amministratore eseguire la distribuzione. Il gestore di distribuzione Web è disponibile solo in IIS 7 o versione successiva.
> - Uso *distribuzione offline*. Questo approccio richiede la minima configurazione del server web, ma un amministratore del server deve copiare il pacchetto web nel server e importarlo tramite Gestione IIS manualmente.
> 
> Per altre informazioni su funzionalità chiave, vantaggi e gli svantaggi di questi approcci, vedere [scelta dell'approccio a destra per la distribuzione Web](choosing-the-right-approach-to-web-deployment.md).


## <a name="is-the-web-deploy-remote-agent-the-right-approach-for-you"></a>È il Web distribuire agente remoto l'approccio giusto per te?

Sì, se l'utente che verrà distribuito il contenuto può fornire le credenziali di amministratore nel server di destinazione. Questo approccio è spesso utile in questi tipi di scenari:

- Ambienti di sviluppo o test, in cui lo sviluppatore ha il pieno controllo sul server di database e server web di destinazione.
- Organizzazioni più piccole in cui un singolo utente o un piccolo gruppo di utenti ha il controllo al ciclo di vita dell'intera applicazione.

In un numero elevato di organizzazioni più grandi e in particolare per gli ambienti di gestione temporanea o produzione, spesso non è realistico per concedere agli utenti diritti di amministratore nei server web. Nel caso dei server di hosting web, ciò è particolarmente improbabile che sia il caso. Inoltre, se si prevede di automatizzare la distribuzione da un server di compilazione, è non possibile utilizzare le credenziali di amministratore per il processo di distribuzione. In questi scenari, la configurazione dei server web per supportare la distribuzione usando il [gestore distribuzione Web](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md) può offrire una scelta più soddisfacente.

## <a name="task-overview"></a>Panoramica di Task

Questo argomento descrive come configurare un server web Internet Information Services (IIS) 7.5 per accettare e distribuire pacchetti web da un computer remoto usando l'approccio di Web Deploy Remote Agent. È necessario:

- Installare IIS 7.5 e la configurazione consigliata per IIS 7.
- Installare la funzionalità distribuzione Web 2.1 o versioni successive.
- Creare un sito Web IIS per ospitare il contenuto distribuito.
- Assicurarsi che il servizio agente di distribuzione Web sia in esecuzione.

Per ospitare in modo specifico la soluzione di esempio, è anche necessario:

- Installare .NET Framework 4.0.
- Installare ASP.NET MVC 3.

In questo argomento illustrerà come eseguire ognuna di queste procedure. Le attività e procedure dettagliate in questo argomento presuppongono che si intende iniziare con una compilazione pulita server che esegue Windows Server 2008 R2. Prima di continuare, verificare quanto segue:

- Windows Server 2008 R2 Service Pack 1 e tutti gli aggiornamenti disponibili sono installati.
- Il server viene aggiunto al dominio.
- Il server ha un indirizzo IP statico.

> [!NOTE]
> Per ulteriori informazioni sull'aggiunta di computer a un dominio, vedere [aggiunta di computer al dominio ed effettuare l'accesso](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Per altre informazioni su come configurare gli indirizzi IP statici, vedere [configurare un indirizzo IP statico](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx). Il servizio agente remoto è supportato da IIS 6 e versioni successive e non richiede di essere aggiunti a un dominio. Tuttavia, i passaggi descritti in questa esercitazione sono stati sviluppati e testati in IIS 7.5 e le procedure per le altre versioni possono variare.


## <a name="install-products-and-components"></a>Installare i prodotti e componenti

In questa sezione consentirà di installare i componenti e prodotti necessari nel server web. Prima di iniziare, è consigliabile eseguire Windows Update per verificare che il server sia completamente aggiornato.

In questo caso, è necessario installare quanto segue:

- **Configurazione consigliata per IIS 7**. In questo modo, il **Server Web (IIS)** ruolo sul server web e installa il set di moduli IIS e i componenti necessari per ospitare un'applicazione ASP.NET.
- **.NET Framework 4.0**. Ciò è necessario eseguire le applicazioni che sono state create in questa versione di .NET Framework.
- **Web Deployment Tool 2.1 o versioni successive**. Ciò consente di installare distribuzione Web (e il relativo file eseguibile sottostante, MSDeploy.exe) nel server. Come parte di questo processo, installa e avvia il servizio agente di distribuzione Web. Questo servizio ti permette di distribuire i pacchetti web da un computer remoto.
- **ASP.NET MVC 3**. Ciò consente di installare gli assembly che è necessario eseguire le applicazioni MVC 3.

> [!NOTE]
> Questa procedura dettagliata viene descritto l'utilizzo dell'installazione guidata piattaforma Web per installare e configurare i componenti necessari. Sebbene non sia necessario usare l'installazione guidata piattaforma Web, semplifica il processo di installazione automaticamente rilevando le dipendenze e assicurare che ottengano sempre le versioni di prodotto più recenti. Per altre informazioni, vedere [programma di installazione di piattaforma Web Microsoft 3.0](https://go.microsoft.com/?linkid=9805118).


**Per installare i componenti e prodotti necessari**

1. Scaricare e installare il [instalace Webové Platformy](https://go.microsoft.com/?linkid=9805118).
2. Durante l'installazione è stata completata, verrà avviata automaticamente dall'installazione guidata piattaforma Web.

    > [!NOTE]
    > È ora possibile avviare l'installazione guidata piattaforma Web in qualsiasi momento dal **avviare** menu. A tale scopo, scegliere il **avviare** menu, fare clic su **tutti i programmi**e quindi fare clic su **Microsoft Web Platform Installer**.
3. In cima il **installazione guidata piattaforma Web 3.0** finestra, fare clic su **prodotti**.
4. Sul lato sinistro della finestra, nel riquadro di spostamento, fare clic su **Frameworks**.
5. Nel **Microsoft .NET Framework 4** righe, se .NET Framework non è già installato, fare clic su **Add**.

    > [!NOTE]
    > Si potrebbe avere già installato .NET Framework 4.0 tramite Windows Update. Se è già installato un prodotto o componente, l'installazione guidata piattaforma Web indicherà questo sostituendo il **Add** sul pulsante con il testo **installato**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image1.png)
6. Nel **ASP.NET MVC 3 (Visual Studio 2010)** riga, fare clic su **Add**.
7. Nel riquadro di spostamento, fare clic su **Server**.
8. Nel **la configurazione consigliata di IIS 7** riga, fare clic su **Add**.
9. Nel **2.1 dello strumento di distribuzione Web** riga, fare clic su **Add**.
10. Fare clic su **Installa**. L'installazione guidata piattaforma Web verrà visualizzato un elenco di prodotti&#x2014;con le dipendenze associate&#x2014;per essere installata e verrà richiesto di accettare le condizioni di licenza.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image2.png)
11. Esaminare le condizioni di licenza e se l'utente accetti le condizioni, fare clic su **accetto**.
12. Una volta completata l'installazione, fare clic su **Finish**, quindi chiudere il **installazione guidata piattaforma Web 3.0** finestra.

Se è installato .NET Framework 4.0 prima dell'installazione di IIS, è necessario eseguire la [strumento di registrazione ASP.NET IIS](https://msdn.microsoft.com/library/k6h9cz8h(v=VS.100).aspx) (aspnet\_regiis.exe) per registrare la versione più recente di ASP.NET con IIS. Se si non esegue questa operazione, si scoprirà che IIS fornirà il contenuto statico (ad esempio file HTML) senza problemi, ma restituirà **404.0 Errore HTTP – non trovato** quando si tenta di passare al contenuto ASP.NET. È possibile utilizzare questa procedura per assicurarsi che sia registrato ASP.NET 4.0.

**Per registrare ASP.NET 4.0 con IIS**

1. Fare clic su **avviare**, quindi digitare **prompt dei comandi**.
2. Nei risultati della ricerca, fare doppio clic su **prompt dei comandi**, quindi fare clic su **Esegui come amministratore**.
3. Nella finestra del prompt dei comandi, passare al **%WINDIR%\Microsoft.NET\Framework\v4.0.30319** directory.
4. Digitare il comando seguente e quindi premere INVIO:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-remote-agent/samples/sample1.cmd)]
5. Se si prevede di ospitare applicazioni web a 64 bit in qualsiasi momento, è consigliabile registrare anche la versione a 64 bit di ASP.NET con IIS. A tale scopo, nella finestra del prompt dei comandi, passare al **%WINDIR%\Microsoft.NET\Framework64\v4.0.30319** directory.
6. Digitare il comando seguente e quindi premere INVIO:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-remote-agent/samples/sample2.cmd)]

Come buona pratica, utilizzare Windows Update nuovamente a questo punto per scaricare e installare eventuali aggiornamenti disponibili per i nuovi prodotti e componenti installati.

## <a name="configure-the-iis-website"></a>Configurare il sito Web IIS

Prima di poter distribuire il contenuto web al server, è necessario creare e configurare un sito Web IIS per ospitare il contenuto. La funzionalità distribuzione Web è possibile distribuire solo pacchetti web a un sito Web IIS esistente; Impossibile creare il sito Web per l'utente. A livello generale, è necessario completare queste attività:

- Creare una cartella nel file system per ospitare il contenuto.
- Creare un sito Web IIS per rendere disponibile il contenuto e associarlo a una cartella locale.
- Concessione di autorizzazioni di lettura per l'identità del pool di applicazioni per la cartella locale.

Anche se nulla che vieti di distribuzione del contenuto al sito Web predefinito in IIS, questo approccio non è consigliato per qualsiasi elemento diverso da scenari di test o dimostrazione. Per simulare un ambiente di produzione, è necessario creare un nuovo sito Web IIS con impostazioni specifiche per i requisiti dell'applicazione.

**Per creare e configurare un sito Web IIS**

1. Nel file system locale, creare una cartella per archiviare il contenuto (ad esempio, **C:\DemoSite**).
2. Nel **avviare** dal menu **strumenti di amministrazione**, quindi fare clic su **Gestione Internet Information Services (IIS)**.
3. In Gestione IIS nel **connessioni** riquadro, espandere il nodo del server (ad esempio, **TESTWEB1**).

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image3.png)
4. Fare doppio clic il **siti** nodo e quindi fare clic su **Aggiungi sito Web**.
5. Nel **nomesito** , digitare un nome per il sito Web IIS (ad esempio, **DemoSite**).
6. Nel **percorso fisico** digitare (o passare a) il percorso alla cartella locale (ad esempio **C:\DemoSite**).
7. Nel **porta** , digitare il numero di porta su cui si vuole ospitare il sito Web (ad esempio **85**).

    > [!NOTE]
    > I numeri di porta standard sono 80 per HTTP e 443 per HTTPS. Tuttavia, se si ospita il sito Web sulla porta 80, è necessario arrestare il sito Web predefinito prima che si possano accedere al sito.
8. Lasciare il **nome Host** casella vuota, a meno che non si desidera configurare un record di sistema DNS (Domain Name) per il sito Web e quindi fare clic su **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image4.png)

    > [!NOTE]
    > In un ambiente di produzione probabilmente vorrai ospitare il sito Web sulla porta 80 e configurare un'intestazione host, insieme ai record DNS corrispondenti. Per altre informazioni su come configurare le intestazioni host in IIS 7, vedere [configurare un'intestazione Host per un sito Web (IIS 7)](https://technet.microsoft.com/library/cc753195(WS.10).aspx). Per altre informazioni sul ruolo Server DNS in Windows Server 2008 R2, vedere [Panoramica di Server DNS](https://technet.microsoft.com/en-gb/library/cc770392.aspx) e [Server DNS](https://technet.microsoft.com/windowsserver/dd448607).
9. Nel riquadro **Azioni** sotto **Modifica sito**, fare clic su **Binding**.
10. Nel **binding sito** finestra di dialogo, fare clic su **Aggiungi**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image5.png)
11. Nel **Aggiungi Binding sito** finestra di dialogo, impostare il **indirizzo IP** e **porta** in modo che corrisponda alla configurazione del sito esistente.
12. Nel **nome Host** , digitare il nome del server web (ad esempio, **TESTWEB1**), quindi fare clic su **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image6.png)

    > [!NOTE]
    > Il primo binding del sito consente di accedere al sito in locale usando l'indirizzo IP e la porta o `http://localhost:85`. L'associazione del sito secondo consente di accedere al sito da altri computer appartenenti al dominio usando il nome del computer (ad esempio, http://testweb1:85).
13. Nel **binding sito** finestra di dialogo, fare clic su **Chiudi**.
14. Nel **connessioni** riquadro, fare clic su **pool di applicazioni**.
15. Nel **pool di applicazioni** riquadro, fare doppio clic il nome del pool di applicazioni e quindi fare clic su **impostazioni di base**. Per impostazione predefinita, il nome del pool di applicazioni corrisponderà al nome del sito Web (ad esempio, **DemoSite**).
16. Nel **versione di .NET Framework** elenco, selezionare **.NET Framework v4.0.30319**, quindi fare clic su **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image7.png)

    > [!NOTE]
    > La soluzione di esempio richiede .NET Framework 4.0. Questo non è un requisito per la distribuzione Web in generale.

In ordine per il sito Web rendere disponibile il contenuto, l'identità del pool di applicazioni deve avere autorizzazioni lettura per la cartella locale che archivia il contenuto. In IIS 7.5, eseguire i pool di applicazioni con un'identità di pool di applicazioni univoche per impostazione predefinita (a differenza delle versioni precedenti di IIS, in cui i pool di applicazioni vengono in genere eseguito usando l'account del servizio di rete). L'identità del pool non è un account utente reale e non viene visualizzato in qualsiasi elenco di utenti o gruppi&#x2014;viene invece creata in modo dinamico quando viene avviato il pool di applicazioni. Ogni identità del pool di applicazioni viene aggiunto a locale **IIS\_IUSRS** gruppi di sicurezza come un elemento nascosto.

Per concedere autorizzazioni a un'identità del pool di applicazioni in un file o una cartella, sono disponibili due opzioni:

- Assegnare le autorizzazioni all'identità del pool di applicazioni direttamente, usando il formato <strong>IIS AppPool\</ strong ><em>[nome pool di applicazioni]</em>(ad esempio, <strong>IIS AppPool\DemoSite</strong>).
- Assegnare autorizzazioni per il **IIS\_IUSRS** gruppo.

L'approccio più comune consiste nell'assegnare autorizzazioni a locale **IIS\_IUSRS** perché questo approccio consente di modificare i pool di applicazioni senza dover riconfigurare le autorizzazioni del file di gruppo. La procedura successiva Usa questo approccio basato su gruppo.

> [!NOTE]
> Per ulteriori informazioni sull'identità del pool di applicazioni in IIS 7.5, vedere [Application Pool Identities](https://go.microsoft.com/?linkid=9805123).


**Configurare le autorizzazioni di cartella per un sito Web IIS**

1. In Windows Explorer passare al percorso della cartella locale.
2. Pulsante destro del mouse nella cartella e quindi fare clic su **proprietà**.
3. Nel **sicurezza** scheda, fare clic su **Edit**e quindi fare clic su **Add**.
4. Fare clic su **posizioni**. Nel **posizioni** finestra di dialogo, selezionare il server locale e quindi fare clic su **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image8.png)
5. Nel **Seleziona utenti o gruppi** finestra di dialogo, digitare **IIS\_IUSRS**, fare clic su **Controlla nomi**, quindi fare clic su **OK**.
6. Nel <strong>le autorizzazioni per</strong><em>[NomeCartella]</em>la finestra di dialogo, si noti che il nuovo gruppo è stato assegnato il <strong>lettura &amp; eseguire</strong>, <strong>visualizzazione contenuto cartella contenuto</strong>, e <strong>lettura</strong> le autorizzazioni per impostazione predefinita. Lasciare invariata e fare clic su <strong>OK</strong>.
7. Fare clic su <strong>OK</strong> per chiudere la <em>[NomeCartella]</em><strong>proprietà</strong> nella finestra di dialogo.

Come attività finale prima di tentare di distribuire tutti i pacchetti web al server, è necessario assicurarsi che il servizio agente di distribuzione Web sia in esecuzione. Quando si distribuisce un pacchetto da un computer remoto, il servizio agente di distribuzione Web è responsabile per l'estrazione e il contenuto del pacchetto di installazione. Il servizio viene avviato per impostazione predefinita quando si installa lo strumento di distribuzione Web e viene eseguito utilizzando l'identità del servizio di rete.

È possibile controllare se un servizio è in esecuzione in più modi diversi, usando diverse utilità della riga di comando o i cmdlet di Windows PowerShell. Questa procedura viene descritto un approccio basato su interfaccia utente semplice.

**Per verificare che il servizio agente di distribuzione Web sia in esecuzione**

1. Scegliere **Start** , **Strumenti di amministrazione**, quindi scegliere **Servizi**.
2. Individuare il **servizio agente distribuzione Web** riga e verificare che il **stato** è impostata su **avviato**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image9.png)
3. Se il servizio non è già avviato, fare clic su **avviare**.

## <a name="configure-firewall-exceptions"></a>Configurare eccezioni del Firewall

Per impostazione predefinita, il servizio agente remoto è in ascolto sulla porta TCP 80, in questo URL:

<http://servername.com/MSDEPLOYAGENTSERVICE>

Nella maggior parte dei casi, non sarà necessario configurare tutte le regole aggiuntive del firewall per il servizio agente remoto, perché i server web in genere in ascolto delle richieste HTTP sulla porta 80. Se si personalizza l'installazione per l'ascolto su una porta non standard, è necessario configurare eccezioni del firewall come richiesto.

## <a name="conclusion"></a>Conclusione

A questo punto, il server web è pronto per accettare e installare i pacchetti web da un computer remoto. Prima di tentare di distribuire un'applicazione web al server, è possibile controllare questi punti importanti:

- È stato registrato ASP.NET 4.0 con IIS?
- L'identità del pool ha accesso in lettura alla cartella di origine per il sito Web?
- Il servizio agente di distribuzione Web è in esecuzione?

## <a name="further-reading"></a>Ulteriori informazioni

Per indicazioni su come configurare i file di progetto di Microsoft Build Engine (MSBuild) personalizzati per distribuire i pacchetti web per il servizio agente remoto, vedere [configurare le proprietà di distribuzione per un ambiente di destinazione](configuring-deployment-properties-for-a-target-environment.md).

> [!div class="step-by-step"]
> [Precedente](scenario-configuring-a-production-environment-for-web-deployment.md)
> [Successivo](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)
