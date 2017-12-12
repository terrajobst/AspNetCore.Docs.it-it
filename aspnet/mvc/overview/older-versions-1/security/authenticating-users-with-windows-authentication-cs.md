---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-cs
title: L'autenticazione degli utenti con l'autenticazione di Windows (c#) | Documenti Microsoft
author: microsoft
description: Informazioni su come utilizzare l'autenticazione di Windows nel contesto di un'applicazione MVC. Viene descritto come abilitare l'autenticazione di Windows all'interno di co web dell'applicazione...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 418bb07e-f369-4119-b4b0-08f890f7abb2
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-cs
msc.type: authoredcontent
ms.openlocfilehash: 575fb382cc758efb101485bd5aece461bf995bfc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="authenticating-users-with-windows-authentication-c"></a>L'autenticazione degli utenti con l'autenticazione di Windows (c#)
====================
da [Microsoft](https://github.com/microsoft)

> Informazioni su come utilizzare l'autenticazione di Windows nel contesto di un'applicazione MVC. Informazioni su come abilitare l'autenticazione di Windows all'interno di file di configurazione dell'applicazione web e come configurare l'autenticazione con IIS. Infine, è illustrato come utilizzare l'attributo [Authorize] per limitare l'accesso alle azioni del controller a gruppi o utenti specifici di Windows.


L'obiettivo di questa esercitazione viene illustrato come è possibile usufruire di sicurezza funzionalità incorporate in Internet Information Services password proteggere le viste nelle applicazioni MVC. Informazioni su come consentire azioni del controller da richiamare solo da utenti specifici di Windows o gli utenti che sono membri di determinati gruppi di Windows.

Utilizzando l'autenticazione di Windows ha senso quando si compila un sito Web interno aziendale (un sito intranet) e si desidera che gli utenti siano in grado di utilizzare i relativi nomi utente di Windows standard e una password per l'accesso del sito Web. Se si sta compilando un verso l'esterno affiancate sito Web (un sito Web Internet) provare a utilizzare autenticazione basata su form.

#### <a name="enabling-windows-authentication"></a>Abilitazione dell'autenticazione di Windows

Quando si crea una nuova applicazione MVC ASP.NET, l'autenticazione di Windows non è abilitata per impostazione predefinita. Autenticazione basata su form è il tipo di autenticazione predefinito è abilitato per applicazioni MVC. Modificando i file di configurazione (Web. config) web dell'applicazione MVC, è necessario abilitare l'autenticazione di Windows. Trovare il &lt;autenticazione&gt; sezione e modificarlo per utilizzare Windows anziché l'autenticazione basata su form simile al seguente:

[!code-xml[Main](authenticating-users-with-windows-authentication-cs/samples/sample1.xml)]

Quando si abilita l'autenticazione di Windows, server web diventa responsabile per l'autenticazione degli utenti. In genere, esistono due tipi diversi di server web che utilizzano durante la creazione e distribuzione di un'applicazione ASP.NET MVC.

In primo luogo, durante lo sviluppo di un'applicazione MVC, utilizzare il Server Web di sviluppo ASP.NET incluso in Visual Studio. Per impostazione predefinita, il Server Web di sviluppo ASP.NET esegue tutte le pagine nel contesto dell'account di Windows corrente (qualsiasi altro account utilizzato per l'accesso in Windows).

Il Server Web di sviluppo ASP.NET supporta anche l'autenticazione NTLM. È possibile abilitare l'autenticazione NTLM facendo clic il nome del progetto nella finestra Esplora soluzioni e scegliendo proprietà. Successivamente, selezionare la scheda Web e selezionare la casella di controllo NTLM (vedere la figura 1).

**Figura 1: abilitare l'autenticazione NTLM per il Server Web di sviluppo ASP.NET**

![clip_image002](authenticating-users-with-windows-authentication-cs/_static/image1.jpg)

Per un'applicazione web di produzione, l'icona della mano, si utilizza IIS come server web. IIS supporta vari tipi di autenticazione tra cui:

- L'autenticazione di base, definito come parte del protocollo HTTP 1.0. Invia i nomi utente e password in testo non crittografato (codificata Base64) tramite Internet. -L'autenticazione del Digest: invia un hash di una password, anziché la password, in internet. -Autenticazione integrata di Windows (NTLM) – il tipo di autenticazione da utilizzare in ambienti intranet mediante windows. -Certificati di autenticazione: abilita l'autenticazione utilizzando un certificato sul lato client. Il certificato viene mappato a un account utente di Windows.

> [!NOTE] 
> 
> Per una panoramica più dettagliata di questi diversi tipi di autenticazione, vedere [https://msdn.microsoft.com/en-us/library/aa292114(VS.71).aspx](https://msdn.microsoft.com/en-us/library/aa292114(VS.71).aspx).


Per abilitare un particolare tipo di autenticazione, è possibile utilizzare Gestione Internet Information Services. Tenere presente che tutti i tipi di autenticazione non sono disponibili nel caso di ogni sistema operativo. Inoltre, se si utilizza IIS 7.0 con Windows Vista, è necessario abilitare i diversi tipi di autenticazione di Windows prima che vengano visualizzati in Gestione Internet Information Services. Aprire **Pannello di controllo, programmi, programmi e funzionalità, delle funzionalità Windows attivare o disattivare**, espandere il nodo di Internet Information Services (vedere la figura 2).

**Figura 2: funzionalità di attivazione Windows IIS**

![clip_image004](authenticating-users-with-windows-authentication-cs/_static/image2.jpg)

Se si utilizza Internet Information Services, è possibile abilitare o disabilitare diversi tipi di autenticazione. Ad esempio, la figura 3 illustra l'autenticazione anonima disabilitazione e abilitazione dell'autenticazione integrata di Windows (NTLM) quando si utilizza IIS 7.0.

**Figura 3: abilitazione dell'autenticazione integrata di Windows**

![clip_image006](authenticating-users-with-windows-authentication-cs/_static/image3.jpg)

#### <a name="authorizing-windows-users-and-groups"></a>Autorizzazione Windows utenti e gruppi

Dopo aver abilitato l'autenticazione di Windows, è possibile utilizzare l'attributo [Authorize] per controllare l'accesso ai controller o le azioni del controller. Questo attributo può essere applicato a un controller MVC intero o un'azione del controller specifico.

Ad esempio, il controller Home nel listato 1 espone tre azioni denominate Index (), CompanySecrets() e StephenSecrets(). Chiunque può richiamare l'azione Index (). Tuttavia, solo i membri del gruppo di responsabili locale di Windows possono richiamare l'azione di CompanySecrets(). Infine, solo l'utente di dominio Windows denominato Stephen (nel dominio Redmond) è possibile richiamare l'azione StephenSecrets().

**Elenco 1 – controllers\homecontroller.cs.**

[!code-csharp[Main](authenticating-users-with-windows-authentication-cs/samples/sample2.cs)]

> [!NOTE] 
> 
> A causa di Windows controllo Account utente (UAC), quando si utilizza Windows Vista o Windows Server 2008, il gruppo Administrators locale si comporterà in modo diverso rispetto ad altri gruppi. L'attributo [Authorize] non riconosce un membro del gruppo Administrators locale a meno che non è modificare le impostazioni di controllo dell'account utente del computer.


Esattamente cosa accade quando si tenta di richiamare un'azione del controller senza le autorizzazioni appropriate dipende dal tipo di autenticazione abilitato. Per impostazione predefinita, quando si utilizza il Server di sviluppo ASP.NET, è sufficiente ottenere una pagina vuota. Viene visualizzata la pagina con un **401 non autorizzato** stato della risposta HTTP.

Se, invece, si utilizza IIS con l'autenticazione anonima sia disabilitata e abilitata l'autenticazione di base, quindi consente di accedere a un prompt della finestra di dialogo account di accesso ogni volta che si richiede la pagina protetta (vedere la figura 4).

**Figura 4-finestra di dialogo account di accesso autenticazione di base**

![clip_image008](authenticating-users-with-windows-authentication-cs/_static/image4.jpg)

#### <a name="summary"></a>Riepilogo

In questa esercitazione viene illustrato come è possibile utilizzare l'autenticazione di Windows nel contesto di un'applicazione MVC ASP.NET. È stato descritto come abilitare l'autenticazione di Windows all'interno di file di configurazione dell'applicazione web e come configurare l'autenticazione con IIS. Infine, è stato descritto come utilizzare l'attributo [Authorize] per limitare l'accesso alle azioni del controller a gruppi o utenti specifici di Windows.

>[!div class="step-by-step"]
[Precedente](authenticating-users-with-forms-authentication-cs.md)
[Successivo](preventing-javascript-injection-attacks-cs.md)
