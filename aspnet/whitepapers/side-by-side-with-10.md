---
uid: whitepapers/side-by-side-with-10
title: Esecuzione di ASP.NET Side-by-Side di .NET Framework 1.0 e 1.1 | Documenti Microsoft
author: rick-anderson
description: Questo white paper viene descritto come installare sia .NET 1.0 e 1.1 di .NET nel computer, consentendo di un'applicazione Web ASP.NET per l'esecuzione su entrambe le versioni di con i fotogrammi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: bdea2003-e964-4db5-9092-d56cc7560616
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/side-by-side-with-10
msc.type: content
ms.openlocfilehash: 939b04d440955b184bf5f4c40a2ef8175641edfa
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
ms.locfileid: "26530180"
---
<a name="aspnet-side-by-side-execution-of-net-framework-10-and-11"></a>Esecuzione di ASP.NET Side-by-Side di .NET Framework 1.0 e 1.1
====================
> Questo white paper viene descritto come installare sia .NET 1.0 e 1.1 di .NET nel computer, consentendo di un'applicazione Web ASP.NET per l'esecuzione su entrambe le versioni di framework.
> 
> Si applica a ASP.NET 1.0 e 1.1 di ASP.NET.


In ASP.NET, applicazioni si parla di essere in esecuzione side-by-side quando vengono installati nello stesso computer, ma utilizzare versioni diverse di .NET Framework. L'argomento seguente viene descritto come configurare le applicazioni ASP.NET per l'esecuzione side-by-side e vengono fornite procedure dettagliate per:

- [Gestire i mapping dell'applicazione Web durante l'installazione di .NET Framework versione 1.0](#1)
- [Eseguire il mapping di un'applicazione Web in una versione specifica di .NET Framework](#2)
- [Trovare la versione di .NET Framework che utilizza un sito Web](#3)

In genere, quando un componente o un'applicazione viene aggiornata in un computer, la versione precedente è rimossa e sostituita con la versione più recente. Se la nuova versione non è compatibile con la versione precedente, viene in genere interrotta altre applicazioni che utilizzano il componente o applicazione. .NET Framework fornisce il supporto per l'esecuzione side-by-side, che consente più versioni di un assembly o applicazione venga installata nello stesso computer contemporaneamente. Poiché è possibile installare più versioni contemporaneamente, le applicazioni gestite possono selezionare la versione da utilizzare senza influire sulle applicazioni che utilizzano una versione diversa.

Per impostazione predefinita, durante l'installazione di .NET Framework versione 1.1, tutte le applicazioni ASP.NET esistenti vengono riconfigurate automaticamente per usare la versione più recente di .NET Framework. Se si preferisce non nelle applicazioni ASP.NET in .NET Framework 1.1 per impostazione predefinita, fare clic su [qui](#1) per informazioni su come evitare questo problema durante l'installazione.

Se si aggiorna il server Web a .NET Framework 1.1 e si desidera una o più applicazioni Web per l'esecuzione di .NET Framework 1.0, è necessario aggiornare il mapping di Script di Internet Information Services (IIS). Il mapping di script è il meccanismo per eseguire il mapping di estensione di file con estensione aspx per un'applicazione Web specifica a una versione di .NET Framework. Fare clic su [qui](#2) per informazioni su come eseguire il mapping di un'applicazione Web in una versione specifica di .NET Framework.

È possibile utilizzare la gestione di informazioni Internet o lo strumento di registrazione IIS ASP.NET (Aspnet\_regiis.exe) per trovare la versione di .NET Framework in esecuzione una particolare applicazione Web. Fare clic su [qui](#3) per informazioni su come trovare la versione di .NET Framework che utilizza un sito Web.

Una considerazione di importazione per la migrazione a .NET Framework 1.1 è che ogni versione di .NET Framework Usa il proprio file Machine. config. Di conseguenza, se un amministratore Web ha apportato modifiche al file Machine. config, tali modifiche devono essere migrate nel file Machine. config di .NET Framework 1.1.

<a id="1"></a>

## <a name="maintaining-your-web-applications-mapping-to-net-framework-10-during-installation"></a>Gestione di mapping dell'applicazione Web durante l'installazione di .NET Framework 1.0

Per impostazione predefinita, tutte le applicazioni ASP.NET esistenti vengono automaticamente riconfigurate durante l'installazione di utilizzare la versione più recente di .NET Framework. Usa la versione più recente di .NET Framework, le applicazioni possono usufruire dei miglioramenti e nuove funzionalità incluse nella nuova versione. Allo stesso tempo, l'amministratore Web, che potrebbe essere necessario un controllo granulare delle applicazioni che vengono aggiornati, può impedire la riassociazione automatica di tutte le applicazioni ASP.NET esistenti durante l'installazione di .NET Framework.

Per evitare la riassociazione automatica dell'intera applicazione ASP.NET per la versione più recente di .NET Framework, l'amministratore Web è possibile utilizzare l'opzione della riga di comando /noaspupgrade con il programma di installazione Dotnetfx.exe.

**Per impedire la modifica del totale dell'applicazione ASP.NET alla versione più recente**

1. Passare a **avviare**.
2. Fare clic su **eseguire**.
3. Digitare **cmd**.
4. Fare clic su **OK**.  
  
    ![](side-by-side-with-10/_static/image1.gif)
5. Dal prompt dei comandi, digitare quanto segue per avviare l'installazione di .NET Framework: **/c: Dotnetfx.exe "installare /noaspupgrade?**.  
  
    ![](side-by-side-with-10/_static/image2.gif)
6. Fare clic su **Sì** nell'installazione di Microsoft .NET Framework 1.1. Questo verrà avviato il processo di installazione di .NET Framework 1.1.  
  
    ![](side-by-side-with-10/_static/image3.gif)

<a id="2"></a>

## <a name="map-a-web-application-to-a-specific-version-of-the-net-framework"></a>Eseguire il mapping di un'applicazione Web in una versione specifica di .NET Framework

Ogni versione di .NET Framework include una versione dello strumento di registrazione IIS ASP.NET (Aspnet\_regiis.exe). Questo strumento consente agli amministratori di specificare che un'applicazione Web deve essere eseguito in una determinata versione di .NET Framework. Questo viene considerato il mapping di un'applicazione Web a una versione di .NET Framework. Gli amministratori devono selezionare Aspnet\_regiis.exe che corrisponde alla versione di .NET Framework che verrà associato all'applicazione Web. Ad esempio, un amministratore che si desidera specificare un sito Web viene utilizzato .NET Framework 1.1 debba utilizzare Aspnet\_regiis.exe fornito con .NET Framework 1.1.

Aspnet\_regiis.exe per la versione 1.0 si trova in:

- C:\WINDOWS\Microsoft.NET\Framework\**v 1.0.3705** \aspnet\_regiis

Aspnet\_regiis.exe per versione 1,1 si trova in:

- C:\WINDOWS\Microsoft.NET\Framework\**v 1.1.4322** \aspnet\_regiis

Aspnet\_regiis.exe sono disponibili due opzioni per il mapping di un'applicazione Web di script:

- **-s** imposta il mapping di script nel percorso e il relativo elemento figlio directory.
- **sn -** imposta il mapping di script solo del percorso.

Il percorso di definire il percorso dell'applicazione Web IIS metadati, che è definito sotto forma di W3SVC/ROOT / {WebSiteNumber} / {applicazione\_nome}. Per un'applicazione Web, definita portale si trova nel sito Web predefinito, ad esempio, il percorso della metabase è W3SVC/1/ROOT/portale.

![](side-by-side-with-10/_static/image4.gif)

Si noti che inoltre, è possibile utilizzare uno strumento denominato Editor Metabase per ottenere il percorso della metabase. È possibile scaricare questo strumento dal sito Microsoft Support [https://support.microsoft.com/default.aspx?scid=kb;en-us;232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)

- Eseguire Aspnet\_regiis.exe -s W3SVC/1/ROOT/portale per aggiornare il portale IIS script mappa e il relativo subapplication.  
  
    ![](side-by-side-with-10/_static/image5.gif)

- Eseguire Aspnet\_regiis.exe sn - W3SVC/1/ROOT/portale per aggiornare lo script IIS portale esegue il mapping, senza influire sulle applicazioni nel portale? sottodirectory s.  
  
    ![](side-by-side-with-10/_static/image6.gif)

<a id="3"></a>

## <a name="find-the-net-framework-version-that-a-web-application-is-using"></a>Trovare la versione di .NET Framework che utilizza un'applicazione Web

Un amministratore può utilizzare Gestione servizio Internet per trovare la versione di .NET Framework viene eseguito un sito Web. Versioni diverse del sistema operativo avvia Gestione servizi Internet in modo diverso. Per avviare il gestore del servizio, attenersi alla procedura riportata di seguito.

**Avviare Internet Service Manager**

1. Passare a **avviare**.
2. Fare clic su **eseguire**.
3. Tipo **inetmgr**.  
  
    ![](side-by-side-with-10/_static/image7.gif)
4. Da Gestione servizi Internet, selezionare l'applicazione Web, la cui versione di .NET Framework che si desidera conoscere.  
  
    ![](side-by-side-with-10/_static/image8.gif)
5. Pulsante destro del mouse sull'applicazione Web e fare clic su **proprietà.**  
  
    ![](side-by-side-with-10/_static/image9.gif)
6. Dalla finestra Proprietà selezionare **configurazione.**  
  
    ![](side-by-side-with-10/_static/image10.gif)
7. La tabella di mapping dell'applicazione, selezionare **aspx**, fare clic su **modifica**.  
  
    ![](side-by-side-with-10/_static/image11.gif)
8. Dal **eseguibile** casella di testo, esaminare la directory della versione mediante lo scorrimento. Se la directory della versione è v.1.1.4322, l'applicazione è mappata a .NET Framework 1.1. Viceversa, se la directory versione v 1.0.3705, l'applicazione è mappata a .NET Framework 1.0.  
  
    ![](side-by-side-with-10/_static/image12.gif)
