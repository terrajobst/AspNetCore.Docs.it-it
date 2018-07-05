---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework
title: Creazione di una Server Farm con Web Farm Framework | Microsoft Docs
author: jrjlee
description: In questo argomento viene descritto come utilizzare la Web Farm Framework (WFF) 2.0 per creare e configurare una farm di server web da una raccolta di server.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 656dd06d-806c-467c-863d-9fc45e5ba3ab
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework
msc.type: authoredcontent
ms.openlocfilehash: ad557306cb4d3c62932c640b598f82d692bc74cf
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388293"
---
<a name="creating-a-server-farm-with-the-web-farm-framework"></a>Creazione di una Server Farm con Web Farm Framework
====================
da [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene descritto come utilizzare la Web Farm Framework (WFF) 2.0 per creare e configurare una farm di server web da una raccolta di server.


WFF consente di sincronizzare i prodotti della piattaforma web e componenti, applicazioni web, siti Web e le impostazioni di configurazione tra più server web con bilanciamento del carico. Negli scenari in cui è necessario più di un server web, ad esempio gli ambienti di staging e produzione, ciò consente di semplificare notevolmente il processo di distribuzione e la configurazione. È possibile distribuire un'applicazione web in un unico server&#x2014;il *server primario*&#x2014;e WFF eseguirà automaticamente la replica di quell'applicazione web su tutti gli altri server web nella server farm.

## <a name="understanding-the-web-farm-framework"></a>La comprensione di Web Farm Framework

È possibile usare 2.0 WFF per effettuare il provisioning, gestire e distribuire il contenuto a un gruppo di server web. Una distribuzione WFF è costituito da tre ruoli server principali:

- Il *server del controller*. Si usa questo server per creare e configurare le server farm WFF. Il server del controller gestisce la sincronizzazione di componenti della piattaforma web, le impostazioni di configurazione e delle applicazioni tra i server web in una server farm. Si installa WFF 2.0 nel server di controller e il server del controller a sua volta verrà installato l'agente WFF in ogni server in una server farm. Il server del controller non appartiene a livello concettuale di alcuna server farm WFF e un server singolo controller può gestire più server farm. In questo scenario è usare un singolo server del controller WFF per creare e gestire la farm di server di gestione temporanea e la server farm di produzione.
- Il *server primario*. Ogni server farm WFF include un singolo server primario. Quando si installino i componenti della piattaforma web o si distribuiscono applicazioni per il server primario, di WFF Sincronizza le modifiche apportate a tutti gli altri server nella server farm.
- Il *server secondario*. Ogni server farm WFF include uno o più server secondari. Le modifiche apportate al server primario vengono replicate in ogni server secondario all'interno della farm di server.

Viene illustrato come questi ruoli del server correlati agli ambienti di staging e produzione di Fabrikam, Inc.:

![](creating-a-server-farm-with-the-web-farm-framework/_static/image1.png)

In questo scenario, l'ambiente di gestione temporanea e l'ambiente di produzione sono entrambi configurati come server farm di WFF. Un singolo server del controller WFF gestisce entrambe le farm. All'interno di ogni server farm, tutte le modifiche al server primario vengono replicate in ogni server secondario.

Prima di iniziare a configurare gli ambienti di staging e produzione, è consigliabile leggere questi articoli per acquisire familiarità con i concetti principali di WFF 2.0:

- [Panoramica di Web Farm Framework 2.0 per IIS 7](https://go.microsoft.com/?linkid=9805126)
- [Configurazione di una Server Farm con Web Farm Framework 2.0 per IIS 7](https://go.microsoft.com/?linkid=9805127)
- [Requisiti di sistema e della piattaforma per la Web Farm Framework 2.0 per IIS 7](https://go.microsoft.com/?linkid=9805128)

## <a name="task-overview"></a>Panoramica di Task

Per completare le attività e procedure dettagliate in questo argomento, è necessario almeno tre server&#x2014;un controller WFF, un server web primario per la server farm e uno o più server web secondaria per la server farm. È possibile aggiungere più server secondari a una server farm WFF in qualsiasi momento. A livello generale, per creare e configurare una server farm di WFF per l'ambiente di gestione temporanea o produzione, occorre:

- Creare un server del controller tramite l'installazione di Internet Information Services (IIS) 7.5 e WFF 2.0.
- Preparare i server primari e secondari, creare un account di amministratore comuni e configurare eccezioni del firewall.
- Configurare la farm di server tramite Gestione IIS sul server del controller.
- Configurare il bilanciamento del carico usando IIS Application Request Routing (ARR) o una tecnologia di bilanciamento del carico alternativa.

Le attività e procedure dettagliate in questo argomento presuppongono che si sta iniziando con le build server pulito che esegue Windows Server 2008 R2. Prima di iniziare, per ogni server, verificare quanto segue:

- Windows Server 2008 R2 Service Pack 1 e tutti gli aggiornamenti disponibili sono installati.
- Il server viene aggiunto al dominio.
- Il server ha un indirizzo IP statico.

> [!NOTE]
> Per ulteriori informazioni sull'aggiunta di computer a un dominio, vedere [aggiunta di computer al dominio ed effettuare l'accesso](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Per altre informazioni su come configurare gli indirizzi IP statici, vedere [configurare un indirizzo IP statico](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx).


## <a name="create-the-wff-controller-server"></a>Creare il Server del Controller WFF

Per creare un server del controller WFF, è necessario installare IIS 7 o versione successiva e WFF 2.0 o versione successiva. Dietro le quinte, WFF Usa lo strumento di distribuzione Web IIS (distribuzione Web) 2.x per sincronizzare i server nella farm. Se si usa l'installazione guidata piattaforma Web installare WFF, il programma di installazione verrà automaticamente scaricare e installare distribuzione Web per l'utente.

**Per creare il server del controller WFF**

1. Scaricare e installare il [instalace Webové Platformy](https://go.microsoft.com/?linkid=9739157).
2. In cima il **installazione guidata piattaforma Web 3.0** finestra, fare clic su **prodotti**.
3. Sul lato sinistro della finestra, nel riquadro di spostamento, fare clic su **Server**.
4. Nel **la configurazione consigliata di IIS 7** riga, fare clic su **Add**.
5. Nel <strong>Web Farm Framework 2.</strong> <em>x</em> riga, fare clic su <strong>Add</strong>.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image2.png)
6. Fare clic su **Installa**. Si noti che l'installazione guidata piattaforma Web è aggiunto lo strumento di distribuzione Web, insieme a diverse altre dipendenze, all'elenco di installazione.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image3.png)
7. Esaminare le condizioni di licenza e se l'utente accetti le condizioni, fare clic su **accetto**.
8. Una volta completata l'installazione, fare clic su **Finish**, quindi chiudere il **installazione guidata piattaforma Web 3.0** finestra.

## <a name="configure-the-primary-and-secondary-servers"></a>Configurare il server primario e secondario

Prima di creare una server farm WFF, è necessario completare alcune attività di preparazione sul server web che costituiranno la farm:

- Aggiungere le eccezioni del firewall per consentire la **Core Networking**, **amministrazione remota**, e **condivisione File e stampanti** funzionalità per comunicare con il server del controller WFF .
- Creare un account di dominio (ad esempio, **FABRIKAM\stagingfarm**) in Active Directory e aggiungerlo al gruppo administrators locale su ciascun server. Si userà questo account come account di amministratore della farm di server quando si crea la server farm.

Per altre informazioni su come configurare le eccezioni del firewall in Windows Firewall, vedere [requisiti di sistema e della piattaforma per la Web Farm Framework 2.0 per IIS 7](https://go.microsoft.com/?linkid=9805128). Per altri sistemi firewall, consultare la documentazione del prodotto.

È possibile utilizzare la procedura seguente per aggiungere un account di dominio al gruppo administrators locale in Windows Server 2008 R2. È consigliabile eseguire questa procedura in tutti i server che si desidera aggiungere alla server farm&#x2014;in altre parole, aggiungere lo stesso account di dominio al gruppo administrators locale nel server primario e in ogni server secondario.

**Per aggiungere un account di dominio al gruppo administrators locale**

1. Nel **avviare** dal menu **strumenti di amministrazione**, quindi fare clic su **Server Manager**.
2. Nel **Server Manager** finestra, nel riquadro di visualizzazione dell'albero, espandere **Configuration**, espandere **utenti e gruppi locali**, quindi fare clic su **gruppi**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image4.png)
3. Nel **gruppi** riquadro, fare doppio clic su **gli amministratori**.
4. Nel **proprietà Administrators** finestra di dialogo, fare clic su **Add**.
5. Nel **Seleziona utenti, computer, account del servizio o gruppi** finestra di dialogo digitare (o selezionare) all'account di dominio (ad esempio **FABRIKAM\stagingfarm**) e quindi fare clic su **OK**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image5.png)
6. Nel **proprietà Administrators** finestra di dialogo, fare clic su **OK**.

I server sono ora pronti per l'aggiunta a una server farm. Nel caso il server primario, è possibile configurare il server per soddisfare i requisiti dell'applicazione, prima o dopo aver creato la server farm&#x2014;in entrambi i casi di WFF sincronizzerà i server distribuendo i prodotti, componenti o configurazione stessa per i server secondari. Per ragioni di semplicità, questa esercitazione si presuppone che si configurerà il server primario al termine della creazione della server farm.

## <a name="create-the-wff-server-farm"></a>Creare la Farm di Server WFF

A questo punto, tutti i server sono pronti per l'aggiunta a una server farm WFF:

- Sono stati installati WFF sul server del controller.
- Sono state configurate eccezioni firewall sui server web primario e secondario.
- È stato aggiunto un account di dominio al gruppo administrators locale nei server web primario e secondario.

Il passaggio successivo consiste nel creare la server farm in WFF. È possibile farlo dalla gestione IIS sul server del controller WFF.

**Per creare una server farm WFF**

1. Nel server, controller WFF sul **avviare** dal menu **strumenti di amministrazione**e quindi fare clic su **Internet Information Services (IIS) Manager**.
2. Nel **connessioni** riquadro, espandere il nodo del server locale, fare doppio clic su **Server farm**, quindi fare clic su **crea Server Farm**.
3. Nel **crea Server Farm** finestra di dialogo, digitare un nome significativo per la server farm (ad esempio **Farm di gestione temporanea**) e quindi selezionare **Esegui provisioning server farm**.
4. Digitare il nome utente e la password dell'account di dominio che è stato aggiunto al gruppo administrators locale su ciascun server.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image6.png)
5. Scegliere **Avanti**.
6. Nel **Aggiungi server** pagina, digitare il nome di dominio completo (FQDN) del server primario, selezionare **Server primario**e quindi fare clic su **Add**.
7. A questo punto, WFF proverà a contattare il server primario utilizzando le credenziali specificate. Se la connessione ha esito positivo, il server primario verrà aggiunto alla tabella nel **Aggiungi server** pagina.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image7.png)

    > [!NOTE]
    > È possibile osservare **Server è disponibile per il bilanciamento del carico** è selezionata per impostazione predefinita. WFF Usa il modulo ARR IIS per implementare il bilanciamento del carico e quindi distribuire le richieste tra i server web nella server farm. Nella maggior parte degli scenari, potrebbe cancellare solo i **Server è disponibile per il bilanciamento del carico** opzione se si vuole usare un bilanciamento soluzione invece il carico di terze parti.
8. Nel **Aggiungi server** pagina, digitare il nome FQDN del server secondario prima e quindi fare clic su **Add**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image8.png)
9. Ripetere il passaggio 7 per tutti i server secondari aggiuntivi nella farm e quindi fare clic su **fine**.

La server farm WFF è ora in esecuzione. Eventuali prodotti della piattaforma web o componenti da installare in server primario e tutte le applicazioni web o contenuto da distribuire al server primario, viene possibile automaticamente il provisioning su tutti i server secondari.

WFF è un argomento vasto e complesso, e sono disponibili ulteriori informazioni sul [Microsoft Web Farm Framework 2.0 per IIS 7](https://go.microsoft.com/?linkid=9805129) sito Web. Per il momento, tuttavia, esistono due aree di funzionalità che è necessario essere a conoscenza di:

- *Il provisioning dell'applicazione* è il processo che consente di replicare il contenuto dal server primario, come le applicazioni web e le impostazioni di configurazione, tra tutti i server secondari nella server farm. Ad esempio, se si distribuisce la soluzione di esempio Contact Manager al server di gestione temporanea primario, il processo di provisioning dell'applicazione WFF distribuirà questa soluzione per tutti i server di gestione temporanea secondari. Per impostazione predefinita, il processo di provisioning dell'applicazione viene eseguito ogni 30 secondi.
- *Provisioning piattaforma* è il processo che sincronizza i prodotti della piattaforma web e i componenti dal server primario per tutti i server secondari nella server farm. Ad esempio, se si installa ASP.NET MVC 3 nel principale server di gestione temporanea, il processo di provisioning piattaforma utilizzerà l'installazione guidata piattaforma Web per installare ASP.NET MVC 3 in tutti i server di gestione temporanea secondari. Per impostazione predefinita, il processo di provisioning della piattaforma viene eseguito ogni cinque minuti.

È possibile gestire un'applicazione di base e le impostazioni di provisioning della piattaforma da Gestione IIS sul server del controller WFF.

**Esplorare applicazioni e provisioning delle impostazioni della piattaforma**

1. In Gestione IIS nel **connessioni** riquadro, selezionare la server farm.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image9.png)
2. Nel **Server Farm** riquadro, fare doppio clic su **Provisioning delle applicazioni**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image10.png)
3. Come può notare, la server farm è attualmente configurata per sincronizzare le impostazioni di configurazione e il contenuto web tra il server primario e i server secondari ogni 30 secondi.
4. Fare clic su **nuovamente**, quindi fare doppio clic su **Provisioning piattaforma**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image11.png)
5. Come può notare, la server farm è attualmente configurata per sincronizzare i prodotti della piattaforma web e componenti tra il server primario e i server secondari ogni cinque minuti.
6. Fare clic su **nuovamente**.
7. Per forzare la server farm per sincronizzare immediatamente, i prodotti della piattaforma web nel **azioni** riquadro, fare clic su **provisioning piattaforma**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image12.png)

    > [!NOTE]
    > Provisioning piattaforma potrebbe richiedere alcuni minuti. Il processo di installazione viene eseguito in background sui server secondari nella server farm.
8. Dopo aver autorizzato tempo sufficiente per completare il processo di provisioning, è possibile verificare che i prodotti e i componenti che è stato aggiunto al server primario ora siano stati replicati nei server secondari. Ad esempio, è possibile accedere a un server secondario e usare la **Server Manager** per verificare che sia stato installato il ruolo server web.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image13.png)
9. È anche possibile controllare l'elenco di programmi installati per verificare che siano stati aggiunti vari componenti della piattaforma web.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image14.png)

## <a name="configure-load-balancing"></a>Configurare il bilanciamento del carico

Quando si crea una web farm, è necessario configurare una forma di bilanciamento del carico per distribuire le richieste HTTP tra i server web. Potrebbe trattarsi di bilanciamento del carico, ARR IIS, carico di rete di Windows Server 2008 o di terze parti basati su hardware o software di bilanciamento del carico soluzione.

WFF è progettato per integrare strettamente con IIS ARR. Per sfruttare i vantaggi di questa integrazione, è necessario installare il modulo ARR nel server del controller WFF. È quindi indirizzare tutto il traffico web al server del controller, in genere configurando i record di sistema DNS (Domain Name). Il server del controller distribuirà quindi le richieste in ingresso tra i server nella farm, basati sulla disponibilità del server e diversi altri criteri.

> [!NOTE]
> Non è necessario usare ARR con WFF; è possibile configurare WFF per lavorare con soluzioni di bilanciamento del carico di terze parti. Per altre informazioni, vedere [Panoramica di Web Farm Framework 2.0 per IIS 7](https://go.microsoft.com/?linkid=9805126).


Bilanciamento del carico con ARR è un argomento complesso, più che esula dall'ambito di questa esercitazione. Tuttavia, è possibile utilizzare la procedura seguente per installare il modulo ARR e iniziare a usare il bilanciamento del carico.

**Per configurare il bilanciamento del carico sul server del controller WFF**

1. Nel server di controller WFF, avviare l'installazione guidata piattaforma Web.
2. In cima il **installazione guidata piattaforma Web 3.0** finestra, fare clic su **prodotti**.
3. Sul lato sinistro della finestra, nel riquadro di spostamento, fare clic su **Server**.
4. Nel **Application Request Routing 2.5** riga, fare clic su **Add**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image15.png)
5. Fare clic su **installare**, quindi seguire le istruzioni riportate nel **installazione guidata piattaforma Web** finestra.
6. Una volta completata l'installazione, avviare Gestione IIS e il **connessioni** riquadro, fare clic sul nodo della farm di server. Si noti che sono state aggiunte diverse nuove icone per il **Server Farm** riquadro.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image16.png)
7. Nel **Server Farm** riquadro, fare doppio clic su **bilanciamento del carico**.
8. Nel **Load Balance** riquadro, selezionare un carico bilanciare algoritmo (ad esempio, **richiesta corrente meno**).

    > [!NOTE]
    > Per altre informazioni su algoritmi e altre impostazioni di configurazione di bilanciamento del carico, vedere [Application Request Routing modulo](https://go.microsoft.com/?linkid=9805130).

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image17.png)
9. Nel **azioni** riquadro, fare clic su **applica**.

È stato configurato per i server nella farm di server di bilanciamento del carico di base. Se si indirizzano tutto il traffico per la farm di web al server del controller, le richieste verranno distribuite tra i server nella farm in base alla disponibilità e l'algoritmo di bilanciamento del carico che è stato selezionato.

Per altre informazioni su come configurare il bilanciamento del carico con ARR, vedere [Application Request Routing modulo](https://go.microsoft.com/?linkid=9805130).

## <a name="monitor-the-server-farm"></a>Monitorare la Server Farm

È possibile monitorare l'integrità della server farm in qualsiasi momento tramite Gestione IIS sul server del controller. Nel **connessioni** riquadro, espandere la farm di server e quindi fare clic su **server**. Il riquadro centrale mostrerà un riepilogo di ogni server nella farm insieme a un log di traccia delle attività recenti.

![](creating-a-server-farm-with-the-web-farm-framework/_static/image18.png)

## <a name="conclusion"></a>Conclusione

La server farm WFF dovrebbe ora essere in esecuzione. È possibile configurare il server primario per supportare indipendentemente dall'approccio di distribuzione si preferisce&#x2014;vedere la sezione di letture di approfondimento per informazioni dettagliate&#x2014;e la configurazione verrà replicata in ogni server secondario nella server farm.

## <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni su tutti gli aspetti della configurazione e l'utilizzo di WFF, vedere la [Microsoft Web Farm Framework 2.0 per IIS 7](https://go.microsoft.com/?linkid=9805129) sito Web.

> [!div class="step-by-step"]
> [Precedente](configuring-a-database-server-for-web-deploy-publishing.md)
> [Successivo](configuring-deployment-properties-for-a-target-environment.md)
