---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-cs
title: L'autenticazione degli utenti con autenticazione (c#) basata su form | Documenti Microsoft
author: microsoft
description: Informazioni su come utilizzare l'attributo [Authorize] per proteggere una password pagine specifiche nell'applicazione MVC. Informazioni su come utilizzare il sito Web Amministrazione troppo...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 239fd3ca-5630-4b8d-bc4b-2f906b1d3504
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-cs
msc.type: authoredcontent
ms.openlocfilehash: e1def84bbf48847339e89b239b026d053640b935
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="authenticating-users-with-forms-authentication-c"></a>L'autenticazione degli utenti con autenticazione basata su form (c#)
====================
by [Microsoft](https://github.com/microsoft)

> Informazioni su come utilizzare l'attributo [Authorize] per proteggere una password pagine specifiche nell'applicazione MVC. Informazioni su come utilizzare lo strumento Amministrazione sito Web per creare e gestire utenti e ruoli. Inoltre informazioni su come configurare l'archiviazione delle informazioni sui ruoli e account di utente.


L'obiettivo di questa esercitazione viene illustrato come è possibile utilizzare forme autenticazione password proteggere le viste nelle applicazioni ASP.NET MVC. Informazioni su come utilizzare lo strumento Amministrazione sito Web per creare utenti e ruoli. Inoltre informazioni su come impedire agli utenti non autorizzati di richiamare le azioni del controller. Infine, è illustrato come configurare dove vengono archiviate i nomi utente e password.

#### <a name="using-the-web-site-administration-tool"></a>Utilizzando lo strumento Amministrazione sito Web

Prima di qualsiasi altra attività, deve iniziare creando alcuni utenti e ruoli. Il modo più semplice per creare nuovi utenti e ruoli è per poter sfruttare lo strumento Amministrazione sito Web di Visual Studio 2008. È possibile avviare questo strumento selezionando l'opzione di menu **progetto, configurazione di ASP.NET**. In alternativa, è possibile avviare lo strumento Amministrazione sito Web facendo clic sull'icona del hammer raggiunge il mondo che viene visualizzato nella parte superiore della finestra Esplora soluzioni (in qualche modo sconvolgere) (vedere la figura 1).

**Figura 1: avviare lo strumento Amministrazione sito Web**

![clip_image002](authenticating-users-with-forms-authentication-cs/_static/image1.jpg)

All'interno dello strumento Amministrazione sito Web, è creare nuovi utenti e ruoli, selezionare la scheda sicurezza. Fare clic su di **Create user** collegamento per creare un nuovo utente denominato Stephen (vedere la figura 2). Fornire all'utente di Stephen qualsiasi password che si desidera (ad esempio, *secret*).

**Figura 2: creazione di un nuovo utente**

![clip_image004](authenticating-users-with-forms-authentication-cs/_static/image2.jpg)

Per creare nuovi ruoli, prima di abilitare i ruoli e la definizione di uno o più ruoli. Abilitare i ruoli facendo il **abilitare i ruoli** collegamento. Successivamente, creare un ruolo denominato *amministratori* facendo il **crea o Gestisci ruoli** collegamento (vedere la figura 3).

**Figura 3: creare un nuovo ruolo**

![clip_image006](authenticating-users-with-forms-authentication-cs/_static/image3.jpg)

Infine, creare un nuovo utente denominato Sally e associare Sally il ruolo di amministratore facendo clic sul collegamento Crea utente e selezionando gli amministratori durante la creazione di Sara (vedere la figura 4).

**Figura 4: aggiunta di un utente a un ruolo**

![clip_image008](authenticating-users-with-forms-authentication-cs/_static/image4.jpg)

Quando tutti alla fine, è necessario disporre di due nuovi utenti denominati Stephen e Sally. È inoltre necessario avere un nuovo ruolo denominato Administrators. Sally è un membro del ruolo amministratori di Stephen non.

#### <a name="requiring-authorization"></a>Richiede l'autorizzazione

È possibile richiedere all'utente di essere autenticati prima che l'utente richiama un'azione del controller, aggiungere l'attributo [Authorize] per l'azione. È possibile applicare l'attributo [Authorize] a un'azione di singoli controller o è possibile applicare questo attributo a una classe controller intero.

Ad esempio, il controller nel listato 1 espone un'azione denominata CompanySecrets(). Perché questa azione è decorata con l'attributo [Authorize], a meno che un utente è autenticato Impossibile richiamare l'azione.

**Elenco 1 – controllers\homecontroller.cs.**

[!code-csharp[Main](authenticating-users-with-forms-authentication-cs/samples/sample1.cs)]

Se si richiama l'azione CompanySecrets() immettendo l'URL /Home/CompanySecrets nella barra degli indirizzi del browser, non si è un utente autenticato, quindi si verrà reindirizzati alla visualizzazione account di accesso automaticamente (vedere Figura 5).

**Figura 5 – la visualizzazione di account di accesso**

![clip_image010](authenticating-users-with-forms-authentication-cs/_static/image5.jpg)

Immettere il nome utente e password, è possibile utilizzare la visualizzazione di account di accesso. Se non si è un utente registrato, è possibile scegliere di **registrare** collegamento per passare al registro (vedere la figura 6). È possibile utilizzare la visualizzazione del registro per creare un nuovo account utente.

**Figura 6: la visualizzazione di registro**

![clip_image012](authenticating-users-with-forms-authentication-cs/_static/image6.jpg)

Dopo aver eseguito correttamente, è possibile visualizzare il CompanySecrets (vedere la figura 7). Per impostazione predefinita, si continuerà a eseguire l'accesso fino a quando non si chiude la finestra del browser.

**Figura 7: la vista CompanySecrets**

![clip_image014](authenticating-users-with-forms-authentication-cs/_static/image7.jpg)

#### <a name="authorizing-by-user-name-or-user-role"></a>Autorizzazione di nome utente o ruolo utente

Limitare l'accesso a un'azione del controller per un particolare set di utenti o di un particolare set di ruoli utente, è possibile utilizzare l'attributo [Authorize]. Ad esempio, il controller Home modificato nel listato 2 contiene due nuove azioni denominate StephenSecrets() e AdministratorSecrets().

**Elenco 2 – controllers\homecontroller.cs.**

[!code-csharp[Main](authenticating-users-with-forms-authentication-cs/samples/sample2.cs)]

Solo un utente con il nome utente Stephen può richiamare l'azione StephenSecrets(). Tutti gli altri utenti viene reindirizzati alla visualizzazione account di accesso. La proprietà di utenti accetta un elenco separato da virgole di nomi di account utente.

Solo gli utenti al ruolo Administrators possono richiamare l'azione di AdministratorSecrets(). Ad esempio, poiché Sally è un membro del gruppo Administrators, lei può richiamare l'azione AdministratorSecrets(). Tutti gli altri utenti viene reindirizzati alla visualizzazione account di accesso. La proprietà ruoli accetta un elenco separato da virgole di nomi di ruolo.

#### <a name="configuring-authentication"></a>Configurazione dell'autenticazione

A questo punto, è possibile chiedersi memorizzazione le informazioni di account e il ruolo utente. Per impostazione predefinita, le informazioni vengono archiviate in un database SQL (RANU) Express denominato ASPNETDB.mdf si trova nell'App dell'applicazione MVC\_cartella dati. Questo database viene generato automaticamente dal framework di ASP.NET quando si inizia a usare l'appartenenza.

Per visualizzare il database ASPNETDB.mdf nella finestra Esplora soluzioni, è necessario innanzitutto selezionare l'opzione di menu progetto, Mostra tutti i file.

Utilizzo del database di SQL Express predefinito è appropriato quando si sviluppa un'applicazione. Molto probabilmente, tuttavia, non vuoi usare ASPNETDB.mdf database predefinito per un'applicazione di produzione. In tal caso, è possibile modificare l'archiviazione di informazioni sull'account utente, completare i due passaggi seguenti:

1. Aggiungere gli oggetti di database di servizi delle applicazioni al database di produzione: modificare la stringa di connessione dell'applicazione in modo che punti al database di produzione

Il primo passaggio consiste nel database di produzione per aggiungere tutti gli oggetti di database necessari (tabelle e stored procedure). È il modo più semplice per aggiungere questi oggetti in un nuovo database in modo da sfruttare l'installazione guidata di ASP.NET SQL Server (vedere la figura 8). È possibile avviare questo strumento, aprire il Prompt dei comandi di Visual Studio 2008 dal gruppo di programmi Microsoft Visual Studio 2008 e di eseguire il comando seguente dal prompt dei comandi:

aspnet\_regsql

**Figura 8: l'installazione guidata di ASP.NET SQL Server**

![clip_image016](authenticating-users-with-forms-authentication-cs/_static/image8.jpg)

L'installazione guidata di ASP.NET SQL Server consente di selezionare un database di SQL Server nella rete e installare tutti gli oggetti di database necessari per i servizi delle applicazioni ASP.NET. Il server di database non è necessario trovarsi nel computer locale.

> [!NOTE] 
> 
> Se non si desidera utilizzare l'installazione guidata di ASP.NET SQL Server, è possibile trovare gli script SQL per l'aggiunta di oggetti di database dell'applicazione servizi nella cartella seguente:
> 
> > C:\Windows\Microsoft.NET\Framework\v2.0.50727


Dopo aver creato gli oggetti di database necessari, è necessario modificare la connessione al database utilizzata dall'applicazione MVC. Modificare la stringa di connessione ApplicationServices nel file di configurazione (Web. config) web in modo che punti al database di produzione. Ad esempio, la connessione modificata nel listato 3 punta a un database denominato MyProductionDB (la stringa di connessione ApplicationServices originale è stata impostata come commento).

**Elenco di 3: Web. config**

[!code-xml[Main](authenticating-users-with-forms-authentication-cs/samples/sample3.xml)]

#### <a name="configuring-database-permissions"></a>Configurazione delle autorizzazioni di Database

Se si utilizza la sicurezza integrata per connettersi al database sarà necessario aggiungere l'account utente di Windows corretto come account di accesso al database. L'account corretto varia a seconda che si stia usando il Server di sviluppo ASP.NET o Internet Information Services come server web. L'account utente corretto dipende inoltre dal sistema operativo.

Se si utilizza il Server di sviluppo ASP.NET (il server web predefinito utilizzato da Visual Studio) l'applicazione viene eseguita all'interno del contesto dell'account utente di Windows. In tal caso, è necessario aggiungere l'account utente di Windows come un accesso al server database.

In alternativa, se si utilizza Internet Information Services quindi è necessario aggiungere l'account ASPNET o l'account del servizio di rete autorità NT come account di accesso server database. Se si utilizza Windows XP, aggiunta l'account ASPNET come account di accesso al database. Se si utilizza un sistema operativo più recente, ad esempio Windows Vista o Windows Server 2008, aggiungere l'account del servizio di rete autorità NT come accesso al database.

È possibile aggiungere un nuovo account utente al database utilizzando Microsoft SQL Server Management Studio (vedere Figura 9).

**Figura 9: creazione di un nuovo account di accesso di Microsoft SQL Server**

![clip_image018](authenticating-users-with-forms-authentication-cs/_static/image9.jpg)

Dopo aver creato l'account di accesso necessarie, è necessario eseguire il mapping dell'account di accesso a un utente del database con i ruoli del database appropriato. Fare doppio clic su account di accesso e selezionare la scheda Mapping utente. Selezionare uno o più ruoli di database di servizi dell'applicazione. Ad esempio, per autenticare gli utenti, è necessario abilitare aspnet\_appartenenza\_BasicAccess: ruolo del database. Per creare nuovi utenti, è necessario abilitare aspnet\_appartenenza\_FullAccess ruolo del database (vedere la figura 10).

**Figura 10: aggiunta di ruoli di database di servizi delle applicazioni**

![clip_image020](authenticating-users-with-forms-authentication-cs/_static/image10.jpg)

#### <a name="summary"></a>Riepilogo

In questa esercitazione è stato descritto come utilizzare l'autenticazione basata su form quando si compila un'applicazione MVC ASP.NET. In primo luogo, è stato descritto come creare nuovi utenti e ruoli che possono sfruttare lo strumento Amministrazione sito Web. Successivamente, è stato descritto come utilizzare l'attributo [Authorize] per impedire agli utenti non autorizzati di richiamare le azioni del controller. Infine, è stato descritto come configurare l'applicazione MVC per archiviare informazioni su utenti e ruoli in un database di produzione.

> [!div class="step-by-step"]
> [avanti](authenticating-users-with-windows-authentication-cs.md)
