---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-cs
title: L'autenticazione degli utenti con l'autenticazione di Windows (c#) | Microsoft Docs
author: microsoft
description: Informazioni su come usare l'autenticazione di Windows nel contesto di un'applicazione MVC. Descrive come abilitare l'autenticazione di Windows all'interno di co web dell'applicazione...
ms.author: aspnetcontent
ms.date: 01/27/2009
ms.assetid: 418bb07e-f369-4119-b4b0-08f890f7abb2
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-cs
msc.type: authoredcontent
ms.openlocfilehash: dd5cea6b23aad668307326a46db91d8ee5526dcb
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802075"
---
<a name="authenticating-users-with-windows-authentication-c"></a>L'autenticazione degli utenti con l'autenticazione di Windows (c#)
====================
da [Microsoft](https://github.com/microsoft)

> Informazioni su come usare l'autenticazione di Windows nel contesto di un'applicazione MVC. Descrive come abilitare l'autenticazione di Windows nel file di configurazione dell'applicazione web e su come configurare l'autenticazione con IIS. Infine, informazioni su come usare l'attributo [Authorize] per limitare l'accesso alle azioni del controller a gruppi o utenti specifici di Windows.


L'obiettivo di questa esercitazione è illustrare come è possibile sfruttare i vantaggi della sicurezza funzionalità incorporate in Internet Information Services password proteggere le viste nelle applicazioni MVC. Descrive come consentire o meno azioni del controller essere richiamata soltanto dagli utenti di Windows specifici o gli utenti che sono membri di determinati gruppi di Windows.

Usa l'autenticazione di Windows ha senso quando si compila un sito Web aziendale interno (un sito intranet) e si desidera che gli utenti siano in grado di utilizzare i relativi nomi utente di Windows standard e una password per l'accesso del sito Web. Se si sta compilando un verso l'esterno con connessione sito Web (un sito Web Internet) provare a utilizzare autenticazione basata su form.

#### <a name="enabling-windows-authentication"></a>Abilitazione dell'autenticazione di Windows

Quando si crea una nuova applicazione MVC ASP.NET, l'autenticazione di Windows non è abilitato per impostazione predefinita. Autenticazione basata su form è abilitato per le applicazioni MVC il tipo di autenticazione predefinito. È necessario abilitare l'autenticazione di Windows modificando i file di configurazione (Web. config) web dell'applicazione MVC. Trovare il &lt;autenticazione&gt; sezione e modificare in modo che utilizzi Windows anziché l'autenticazione basata su form simile al seguente:

[!code-xml[Main](authenticating-users-with-windows-authentication-cs/samples/sample1.xml)]

Quando si abilita l'autenticazione di Windows, il server web diventa responsabile per l'autenticazione degli utenti. In genere, esistono due tipi diversi di server web che utilizzano durante la creazione e distribuzione di un'applicazione ASP.NET MVC.

Durante lo sviluppo di un'applicazione MVC, è prima di tutto, usare il Server Web di sviluppo ASP.NET fornito con Visual Studio. Per impostazione predefinita, il Server Web di sviluppo ASP.NET esegue tutte le pagine nel contesto dell'account di Windows corrente (qualsiasi altro account utilizzato per l'accesso in Windows).

Il Server Web di sviluppo ASP.NET supporta anche l'autenticazione NTLM. È possibile abilitare l'autenticazione NTLM facendo clic sul nome del progetto in Esplora soluzioni e selezionando proprietà. Successivamente, selezionare la scheda del Web e selezionare la casella di controllo NTLM (vedere la figura 1).

**Figura 1: abilitazione autenticazione NTLM per il Server Web di sviluppo ASP.NET**

![clip_image002](authenticating-users-with-windows-authentication-cs/_static/image1.jpg)

Per un'applicazione web di produzione sull'icona della mano, è usare IIS come server web. IIS supporta vari tipi di autenticazione, tra cui:

- L'autenticazione di base, definito come parte del protocollo HTTP 1.0. Invia i nomi utente e password in testo non crittografato (con codificata Base64) tramite Internet. -Autenticazione del Digest: invia un hash di una password, anziché la password, attraverso la rete internet. -Autenticazione integrata di Windows (NTLM) – il tipo di autenticazione da usare in ambienti intranet usando windows più adatto. -Certificato di autenticazione: abilita l'autenticazione usando un certificato sul lato client. Il certificato viene mappato a un account utente di Windows.

> [!NOTE] 
> 
> Per una panoramica più dettagliata di questi diversi tipi di autenticazione, vedere [ https://msdn.microsoft.com/library/aa292114(VS.71).aspx ](https://msdn.microsoft.com/library/aa292114(VS.71).aspx).


È possibile utilizzare Gestione Internet Information Services per abilitare un particolare tipo di autenticazione. Tenere presente che tutti i tipi di autenticazione non sono disponibili nel caso di ogni sistema operativo. Inoltre, se si usa IIS 7.0 con Windows Vista, è necessario abilitare i diversi tipi di autenticazione di Windows prima che vengano visualizzati in Gestione Internet Information Services. Aprire **Pannello di controllo, applicazioni, programmi e funzionalità, o disattivazione delle funzionalità Windows attivare**, espandere il nodo Internet Information Services (vedere la figura 2).

**Figura 2: funzionalità di attivazione Windows IIS**

![clip_image004](authenticating-users-with-windows-authentication-cs/_static/image2.jpg)

Usa Internet Information Services, è possibile abilitare o disabilitare i diversi tipi di autenticazione. Ad esempio, la figura 3 illustra l'autenticazione anonima disabilitazione e abilitazione dell'autenticazione integrata di Windows (NTLM) quando si utilizza IIS 7.0.

**Figura 3: abilitazione dell'autenticazione integrata di Windows**

![clip_image006](authenticating-users-with-windows-authentication-cs/_static/image3.jpg)

#### <a name="authorizing-windows-users-and-groups"></a>Autorizzazione Windows utenti e gruppi

Dopo aver abilitato l'autenticazione di Windows, è possibile usare l'attributo [Authorize] per controllare l'accesso ai controller o azioni del controller. Questo attributo può essere applicato a un intero controller MVC o una determinata azione del controller.

Ad esempio, il controller Home nel listato 1 espone tre azioni denominate Index () e CompanySecrets() StephenSecrets(). Chiunque può richiamare l'azione Index (). Tuttavia, solo i membri del gruppo di gestione locale di Windows possono richiamare l'azione CompanySecrets(). Infine, solo l'utente di dominio Windows denominato Stephen (nel dominio di Redmond) può richiamare l'azione StephenSecrets().

**Listato 1 – controllers\homecontroller.cs.**

[!code-csharp[Main](authenticating-users-with-windows-authentication-cs/samples/sample2.cs)]

> [!NOTE] 
> 
> A causa di Windows controllo Account utente (UAC), quando si lavora con Windows Vista o Windows Server 2008, il gruppo Administrators locale si comporterà in modo diverso rispetto ad altri gruppi. L'attributo [Authorize] non riconosca correttamente un membro del gruppo Administrators locale a meno che non è modificare le impostazioni di controllo dell'account utente del computer.


Esattamente cosa accade quando si tenta di richiamare un'azione del controller se non le autorizzazioni appropriate dipende dal tipo di autenticazione abilitato. Per impostazione predefinita, quando si usa il Server di sviluppo ASP.NET, è sufficiente ottenere una pagina vuota. Viene visualizzata la pagina con un **401 non autorizzato** stato della risposta HTTP.

Se, d'altra parte, si utilizza IIS con l'autenticazione anonima disabilitata e l'autenticazione di base abilitata e quindi viene visualizzato un prompt di finestra di dialogo di accesso ogni volta che si richiede alla pagina protetta (vedere la figura 4).

**Figura 4 – finestra di dialogo account di accesso dell'autenticazione di base**

![clip_image008](authenticating-users-with-windows-authentication-cs/_static/image4.jpg)

#### <a name="summary"></a>Riepilogo

Questa esercitazione è spiegato come usare l'autenticazione di Windows nel contesto di un'applicazione ASP.NET MVC. Si è appreso come abilitare l'autenticazione di Windows nel file di configurazione dell'applicazione web e su come configurare l'autenticazione con IIS. Infine, si è appreso come usare l'attributo [Authorize] per limitare l'accesso alle azioni del controller a gruppi o utenti specifici di Windows.

> [!div class="step-by-step"]
> [Precedente](authenticating-users-with-forms-authentication-cs.md)
> [Successivo](preventing-javascript-injection-attacks-cs.md)
