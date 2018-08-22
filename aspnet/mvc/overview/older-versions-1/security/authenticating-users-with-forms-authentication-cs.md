---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-cs
title: Autenticazione degli utenti con form di autenticazione (c#) | Microsoft Docs
author: microsoft
description: Informazioni su come usare l'attributo [Authorize] per proteggere una password specifiche pagine nell'applicazione MVC. Informazioni su come usare il sito Web Amministrazione troppo...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 239fd3ca-5630-4b8d-bc4b-2f906b1d3504
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-cs
msc.type: authoredcontent
ms.openlocfilehash: d69ae977b3e6a323d1dff1443f09ac40e8f9a449
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829217"
---
<a name="authenticating-users-with-forms-authentication-c"></a>Autenticazione degli utenti con autenticazione basata su form (c#)
====================
da [Microsoft](https://github.com/microsoft)

> Informazioni su come usare l'attributo [Authorize] per proteggere una password specifiche pagine nell'applicazione MVC. Descrive come usare lo strumento Amministrazione sito Web per creare e gestire utenti e ruoli. Anche informazioni su come configurare l'archiviazione delle informazioni sui ruoli e account di utente.


L'obiettivo di questa esercitazione è illustrare come è possibile utilizzare forme autenticazione password proteggere le viste nelle applicazioni ASP.NET MVC. Descrive come usare lo strumento Amministrazione sito Web per creare utenti e ruoli. Anche informazioni su come impedire agli utenti non autorizzati di richiamare le azioni del controller. Infine, informazioni su come configurare dove vengono archiviate i nomi utente e password.

#### <a name="using-the-web-site-administration-tool"></a>Utilizzando lo strumento Amministrazione sito Web

Prima di procedere qualsiasi altro elemento, da dove cominciare creando alcuni utenti e ruoli. Il modo più semplice per creare nuovi utenti e ruoli è per poter sfruttare lo strumento Amministrazione sito Web di Visual Studio 2008. È possibile avviare questo strumento selezionando l'opzione di menu **progetto, configurazione di ASP.NET**. In alternativa, è possibile avviare lo strumento Amministrazione sito Web facendo clic sull'icona (in qualche modo scary) di hammer raggiungendo il mondo che viene visualizzato nella parte superiore della finestra Esplora soluzioni (vedere la figura 1).

**Figura 1: avvio dello strumento Amministrazione sito Web**

![clip_image002](authenticating-users-with-forms-authentication-cs/_static/image1.jpg)

Nello strumento di amministrazione sito Web, selezionando la scheda di sicurezza è creare nuovi utenti e ruoli. Scegliere il **creare un utente** collegamento per creare un nuovo utente denominato Stephen (vedere la figura 2). Fornire all'utente di Stephen qualsiasi password desiderata (ad esempio, *segreto*).

**Figura 2: creazione di un nuovo utente**

![clip_image004](authenticating-users-with-forms-authentication-cs/_static/image2.jpg)

Creare nuovi ruoli è innanzitutto necessario abilitare i ruoli e la definizione di uno o più ruoli. Abilitare i ruoli facendo il **abilitare i ruoli** collegamento. Successivamente, creare un ruolo denominato *gli amministratori* facendo le **crea o Gestisci ruoli** link (vedere la figura 3).

**Figura 3: creazione di un nuovo ruolo**

![clip_image006](authenticating-users-with-forms-authentication-cs/_static/image3.jpg)

Infine, creare un nuovo utente denominato Sara e associare Sara con il ruolo di amministratore facendo clic sul collegamento Create User e selezionando gli amministratori durante la creazione di Sara (vedere la figura 4).

**Figura 4: aggiunta di un utente a un ruolo**

![clip_image008](authenticating-users-with-forms-authentication-cs/_static/image4.jpg)

Quando tutti alla fine, è necessario disporre di due nuovi utenti denominati Stephen e Sara. È necessario anche un nuovo ruolo denominato Administrators. Sara è un membro del ruolo amministratori di Stephen non.

#### <a name="requiring-authorization"></a>Richiesta di autorizzazione

È possibile richiedere all'utente di essere autenticati prima che l'utente richiama un'azione del controller, aggiungere l'attributo [Authorize] all'azione. È possibile applicare l'attributo [Authorize] a un'azione del controller singoli oppure è possibile applicare questo attributo a una classe intero controller.

Ad esempio, il controller nel listato 1 espone un'azione denominata CompanySecrets(). Poiché questa azione è decorata con l'attributo [Authorize], questa azione non è possibile richiamare, a meno che un utente viene autenticato.

**Listato 1 – controllers\homecontroller.cs.**

[!code-csharp[Main](authenticating-users-with-forms-authentication-cs/samples/sample1.cs)]

Se si richiama l'azione CompanySecrets() immettendo l'URL avremo/CompanySecrets nella barra degli indirizzi del browser e non si tratta di un utente autenticato, quindi si verrà reindirizzati alla visualizzazione account di accesso automaticamente (vedere la figura 5).

**Figura 5: la visualizzazione di accesso**

![clip_image010](authenticating-users-with-forms-authentication-cs/_static/image5.jpg)

È possibile usare la visualizzazione di accesso immettere il nome utente e password. Se non si è un utente registrato, è possibile scegliere il **registrare** collegamento per passare al registro (vedere la figura 6). È possibile utilizzare la vista di registro per creare un nuovo account utente.

**Figura 6-visualizzazione Register**

![clip_image012](authenticating-users-with-forms-authentication-cs/_static/image6.jpg)

Dopo aver eseguito correttamente, è possibile visualizzare il CompanySecrets (vedere la figura 7). Per impostazione predefinita, si continuerà a essere registrate nei finché non si chiude la finestra del browser.

**Figura 7: la vista CompanySecrets**

![clip_image014](authenticating-users-with-forms-authentication-cs/_static/image7.jpg)

#### <a name="authorizing-by-user-name-or-user-role"></a>Autorizzazione in base al nome utente o ruolo utente

È possibile usare l'attributo [Authorize] limitare l'accesso a un'azione del controller per un particolare set di utenti o un particolare set di ruoli utente. Ad esempio, il controller Home modificato nel listato 2 contiene due nuove azioni denominate StephenSecrets() e AdministratorSecrets().

**Listato 2 – controllers\homecontroller.cs.**

[!code-csharp[Main](authenticating-users-with-forms-authentication-cs/samples/sample2.cs)]

Solo un utente con il nome utente Stephen può richiamare l'azione StephenSecrets(). Tutti gli altri utenti venire reindirizzate alla visualizzazione account di accesso. La proprietà di utenti accetta un elenco delimitato da virgole di nomi di account utente.

Solo gli utenti nel ruolo Administrators possono richiamare l'azione AdministratorSecrets(). Ad esempio, poiché Sara è un membro del gruppo Administrators, Anna può richiamare l'azione AdministratorSecrets(). Tutti gli altri utenti venire reindirizzate alla visualizzazione account di accesso. La proprietà ruoli accetta un elenco delimitato da virgole di nomi di ruolo.

#### <a name="configuring-authentication"></a>Configurazione dell'autenticazione

A questo punto è lecito chiedersi posizioni di archiviazione di informazioni di account e il ruolo utente. Per impostazione predefinita, le informazioni vengono archiviate in un database SQL (RANU) Express denominato aspnetdb. mdf situato nell'App dell'applicazione MVC\_cartella dati. Questo database viene generato automaticamente dal framework ASP.NET quando si inizia a usare l'appartenenza.

Per visualizzare il database aspnetdb. mdf nella finestra Esplora soluzioni, è necessario innanzitutto selezionare l'opzione di menu progetto, Mostra tutti i file.

Utilizzo del database di SQL Express predefinito è bene quando si sviluppa un'applicazione. Molto probabilmente, tuttavia, non vuoi usare database aspnetdb. mdf predefinito per un'applicazione di produzione. In tal caso, è possibile modificare archiviazione informazioni sull'account utente, completare i due passaggi seguenti:

1. Aggiungere gli oggetti di database di servizi delle applicazioni al database di produzione - modificare la stringa di connessione dell'applicazione in modo che punti al database di produzione

Il primo passaggio consiste nell'aggiungere tutti gli oggetti di database necessari (tabelle e stored procedure) al database di produzione. Il modo più semplice per aggiungere questi oggetti in un nuovo database sia per sfruttare i vantaggi della configurazione guidata Server SQL di ASP.NET (vedere la figura 8). È possibile avviare questo strumento, aprire il Prompt dei comandi di Visual Studio 2008 dal gruppo di programmi Microsoft Visual Studio 2008 e l'esecuzione del comando seguente dal prompt dei comandi:

aspnet\_regsql

**Figura 8: l'installazione guidata Server SQL ASP.NET**

![clip_image016](authenticating-users-with-forms-authentication-cs/_static/image8.jpg)

L'installazione guidata di ASP.NET SQL Server consente di selezionare un database di SQL Server nella rete e installare tutti gli oggetti di database necessari per i servizi delle applicazioni ASP.NET. Il server di database non deve trovarsi nel computer locale.

> [!NOTE] 
> 
> Se non si desidera utilizzare l'installazione guidata di ASP.NET SQL Server, è possibile trovare gli script SQL per aggiungere gli oggetti di database dell'applicazione servizi nella cartella seguente:
> 
> > C:\Windows\Microsoft.NET\Framework\v2.0.50727


Dopo aver creato gli oggetti di database necessari, è necessario modificare la connessione al database usata dall'applicazione MVC. Modificare la stringa di connessione ApplicationServices nel file di configurazione (Web. config) web in modo che punti al database di produzione. Ad esempio, la connessione modificata nel listato 3 punta a un database denominato MyProductionDB (la stringa di connessione ApplicationServices originale è stata impostata come commento).

**Listato 3 – Web. config**

[!code-xml[Main](authenticating-users-with-forms-authentication-cs/samples/sample3.xml)]

#### <a name="configuring-database-permissions"></a>Configurazione delle autorizzazioni di Database

Se si usa la sicurezza integrata per connettersi al database sarà necessario aggiungere l'account utente Windows corretto come un account di accesso al database. L'account corretto varia a seconda se si usa il Server di sviluppo ASP.NET o Internet Information Services come server web. L'account utente corretto dipende anche dal sistema operativo.

Se si usa il Server di sviluppo ASP.NET (il server web predefinito usato da Visual Studio) l'applicazione viene eseguita all'interno del contesto dell'account utente di Windows. In tal caso, è necessario aggiungere l'account utente di Windows come un accesso al server di database.

In alternativa, se si usa Internet Information Services quindi è necessario aggiungere l'account ASPNET o NT AUTHORITY/account servizio di rete come un accesso al server di database. Se si utilizza Windows XP, aggiungere l'account ASPNET come account di accesso al database. Se si usa un sistema operativo più recente, ad esempio Windows Vista o Windows Server 2008, quindi aggiungere l'account NT AUTHORITY/NETWORK SERVICE l'account di accesso del database.

È possibile aggiungere un nuovo account utente al database utilizzando Microsoft SQL Server Management Studio (vedere la figura 9).

**Figura 9: creare un nuovo account di accesso di Microsoft SQL Server**

![clip_image018](authenticating-users-with-forms-authentication-cs/_static/image9.jpg)

Dopo aver creato l'account di accesso necessarie, è necessario eseguire il mapping di account di accesso a un utente di database con i ruoli del database a destra. Fare doppio clic su account di accesso e selezionare la scheda Mapping utente. Selezionare uno o più ruoli di database di servizi dell'applicazione. Ad esempio, per poter autenticare gli utenti, è necessario abilitare aspnet\_appartenenza\_BasicAccess: ruolo predefinito del database. Per creare nuovi utenti, è necessario abilitare aspnet\_appartenenza\_FullAccess ruolo predefinito del database (vedere la figura 10).

**Figura 10: aggiunta di ruoli di database di servizi delle applicazioni**

![clip_image020](authenticating-users-with-forms-authentication-cs/_static/image10.jpg)

#### <a name="summary"></a>Riepilogo

In questa esercitazione è stato descritto come usare l'autenticazione form durante la creazione di un'applicazione ASP.NET MVC. In primo luogo, è stato descritto come creare nuovi utenti e ruoli, sfruttando i vantaggi dello strumento Amministrazione sito Web. Successivamente, si è appreso come usare l'attributo [Authorize] per impedire agli utenti non autorizzati di richiamare le azioni del controller. Infine, si è appreso come configurare l'applicazione MVC per archiviare informazioni sui ruoli e utenti in un database di produzione.

> [!div class="step-by-step"]
> [avanti](authenticating-users-with-windows-authentication-cs.md)
