---
uid: whitepapers/side-by-side-with-10
title: L'esecuzione di ASP.NET Side-by-Side di .NET Framework 1.0 e 1.1 | Microsoft Docs
author: rick-anderson
description: Questo white paper viene descritto come installare sia .NET 1.0 e 1.1 di .NET nel computer, consentendo un'applicazione Web ASP.NET per l'esecuzione in entrambe le versioni di con i fotogrammi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: bdea2003-e964-4db5-9092-d56cc7560616
ms.technology: ''
msc.legacyurl: /whitepapers/side-by-side-with-10
msc.type: content
ms.openlocfilehash: 1b8bcebd59a900e54c759509fd4fc5ad1f4f8576
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391160"
---
<a name="aspnet-side-by-side-execution-of-net-framework-10-and-11"></a>Esecuzione di ASP.NET Side-by-Side di .NET Framework 1.0 e 1.1
====================
> Questo white paper viene descritto come installare sia .NET 1.0 e 1.1 di .NET nel computer, consentendo un'applicazione Web ASP.NET per l'esecuzione in entrambe le versioni di framework.
> 
> Si applica a ASP.NET 1.0 e ASP.NET 1.1.


In ASP.NET, le applicazioni si parla di essere in esecuzione side-by-side quando vengono installati nello stesso computer, ma usare versioni diverse di .NET Framework. L'argomento seguente descrive come configurare le applicazioni ASP.NET per l'esecuzione side-by-side e fornisce istruzioni dettagliate per:

- [Gestire mapping dell'applicazione Web a .NET Framework versione 1.0 durante l'installazione](#1)
- [Eseguire il mapping di un'applicazione Web a una versione specifica di .NET Framework](#2)
- [Trovare la versione di .NET Framework che utilizza un sito Web](#3)

In genere, quando un componente o un'applicazione viene aggiornata in un computer, la versione meno recente viene rimosso e sostituita con la versione più recente. Se la nuova versione non è compatibile con la versione precedente, questa operazione interrompe in genere altre applicazioni che usano il componente o applicazione. .NET Framework fornisce supporto per l'esecuzione side-by-side, che consente a più versioni di un assembly o applicazione da installare nello stesso computer contemporaneamente. Poiché è possibile installare più versioni contemporaneamente, le applicazioni gestite possono selezionare la versione da usare senza influire sulle applicazioni che usano una versione diversa.

Per impostazione predefinita, durante l'installazione di .NET Framework versione 1.1, tutte le applicazioni ASP.NET esistenti vengono riconfigurate automaticamente per usare la versione più recente di .NET Framework. Se si preferisce che non le applicazioni ASP.NET per .NET Framework 1.1 per impostazione predefinita, fare clic su [qui](#1) per informazioni su come evitare questo problema durante l'installazione.

Se si aggiorna il server Web per .NET Framework 1.1 e si desidera che uno o più applicazioni Web per l'esecuzione di .NET Framework 1.0, è necessario aggiornare il mapping di Script di Internet Information Services (IIS). Il mapping di script è il meccanismo per eseguire il mapping di estensione di file con estensione aspx per un'applicazione Web specifica a una versione di .NET Framework. Fare clic su [qui](#2) per imparare a eseguire il mapping di un'applicazione Web a una versione specifica di .NET Framework.

È possibile usare la gestione di informazioni Internet o lo strumento di registrazione ASP.NET IIS (Aspnet\_regiis.exe) per trovare la versione di .NET Framework in esecuzione una particolare applicazione Web. Fare clic su [qui](#3) per imparare a trovare la versione di .NET Framework che utilizza un sito Web.

Un'importazione considerazione durante la migrazione a .NET Framework 1.1 è che ogni versione di .NET Framework Usa un proprio file Machine. config. Di conseguenza, se un amministratore IT ha apportato modifiche al file Machine. config, tali modifiche devono essere migrate nel file Machine. config di .NET Framework 1.1.

<a id="1"></a>

## <a name="maintaining-your-web-applications-mapping-to-net-framework-10-during-installation"></a>Gestione di mapping dell'applicazione Web per .NET Framework 1.0 durante l'installazione

Per impostazione predefinita, tutte le applicazioni ASP.NET esistenti vengono automaticamente riconfigurate durante l'installazione per utilizzare la versione più recente di .NET Framework. Usa la versione più recente di .NET Framework, le applicazioni possono usufruire dei miglioramenti e nuove funzionalità incluse nella nuova versione. Allo stesso tempo, l'amministratore Web, che potrebbe essere necessario un controllo granulare delle applicazioni che vengono aggiornati, può impedire la riassociazione automatica di tutte le applicazioni ASP.NET esistenti durante l'installazione di .NET Framework.

Per evitare la riassociazione automatica dell'intera applicazione ASP.NET per la versione più recente di .NET Framework, l'amministratore Web può usare l'opzione della riga di comando /noaspupgrade Dotnetfx.exe programma di installazione.

**Per impedire la modifica del totale dell'applicazione di ASP.NET alla versione più recente**

1. Passare a **avviare**.
2. Fare clic su **eseguiti**.
3. Digitare **cmd**.
4. Fare clic su **OK**.  
  
    ![](side-by-side-with-10/_static/image1.gif)
5. Dal prompt dei comandi, digitare la riga seguente per avviare l'installazione di .NET Framework: **Dotnetfx.exe /c: "installare /noaspupgrade?**.  
  
    ![](side-by-side-with-10/_static/image2.gif)
6. Fare clic su **Sì** nell'installazione di Microsoft .NET Framework 1.1. Questo verrà avviato il processo di installazione di .NET Framework 1.1.  
  
    ![](side-by-side-with-10/_static/image3.gif)

<a id="2"></a>

## <a name="map-a-web-application-to-a-specific-version-of-the-net-framework"></a>Eseguire il mapping di un'applicazione Web a una versione specifica di .NET Framework

Ogni versione di .NET Framework include una versione dello strumento di registrazione ASP.NET IIS (Aspnet\_regiis.exe). Questo strumento consente agli amministratori di specificare che un'applicazione Web deve essere eseguito in una particolare versione di .NET Framework. Ciò viene detto mapping di un'applicazione Web a una versione di .NET Framework. Gli amministratori devono selezionare Aspnet\_regiis.exe che corrisponde alla versione di .NET Framework che verrà associato all'applicazione Web. Ad esempio, un amministratore che si desidera specificare che un sito Web usare .NET Framework 1.1 debba usare Aspnet\_regiis.exe fornito con .NET Framework 1.1.

Aspnet\_regiis.exe per la versione 1.0 si trova in:

- C:\WINDOWS\Microsoft.NET\Framework\**v1.0.3705**\aspnet\_regiis

Aspnet\_regiis.exe per versione 1,1 si trova in:

- C:\WINDOWS\Microsoft.NET\Framework\**v1.1.4322**\aspnet\_regiis

Aspnet\_regiis.exe fornisce due opzioni per il mapping di un'applicazione Web di script:

- **-s** imposta il mapping di script nel percorso e nel relativo elemento figlio le directory.
- **-sn** imposta il mapping di script solo del percorso.

Il percorso definisce il percorso dell'applicazione Web IIS metadati, che è definito sotto forma di W3SVC/ROOT / {WebSiteNumber} / {applicazione\_nome}. Ad esempio, per un'applicazione Web denominata portale che si trova nel sito Web predefinito, il percorso della metabase è W3SVC/1/ROOT/il portale.

![](side-by-side-with-10/_static/image4.gif)

Tenere presente che è anche possibile usare uno strumento denominato Editor Metabase per ottenere il percorso della metabase. È possibile scaricare questo strumento dal sito di supporto tecnico Microsoft all'indirizzo [ https://support.microsoft.com/default.aspx?scid=kb; en-us; 232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)

- Eseguire Aspnet\_regiis.exe -s W3SVC/1/ROOT/il portale per aggiornare il portale IIS mappa e relativo subapplication script.  
  
    ![](side-by-side-with-10/_static/image5.gif)

- Eseguire Aspnet\_regiis.exe sn - W3SVC/1/ROOT/il portale per aggiornare lo script IIS portale esegue il mapping, senza influire sulle applicazioni nel portale? le sottodirectory s.  
  
    ![](side-by-side-with-10/_static/image6.gif)

<a id="3"></a>

## <a name="find-the-net-framework-version-that-a-web-application-is-using"></a>Trovare la versione di .NET Framework che usa un'applicazione Web

Un amministratore può utilizzare Gestione servizi Internet per trovare la versione di .NET Framework esegue un sito Web. Versioni diverse del sistema operativo avviare Gestione servizi Internet in modo diverso. Per avviare il gestore del servizio, seguire i passaggi elencati di seguito.

**Avviare Internet Service Manager**

1. Passare a **avviare**.
2. Fare clic su **eseguiti**.
3. Tipo di **inetmgr**.  
  
    ![](side-by-side-with-10/_static/image7.gif)
4. Da Gestione servizi Internet, selezionare l'applicazione Web la cui versione di .NET Framework che si desidera conoscere.  
  
    ![](side-by-side-with-10/_static/image8.gif)
5. Fare doppio clic sull'applicazione Web e fare clic su **proprietà.**  
  
    ![](side-by-side-with-10/_static/image9.gif)
6. Nella finestra Proprietà, selezionare **configurazione.**  
  
    ![](side-by-side-with-10/_static/image10.gif)
7. La tabella di mapping dell'applicazione, selezionare **. aspx**, fare clic su **modificare**.  
  
    ![](side-by-side-with-10/_static/image11.gif)
8. Dal **eseguibile** casella di testo, esaminare la directory della versione mediante lo scorrimento. Se la directory della versione è v.1.1.4322, l'applicazione è mappata a .NET Framework 1.1. Viceversa, se la directory della versione è v1.0.3705, l'applicazione è mappata a .NET Framework 1.0.  
  
    ![](side-by-side-with-10/_static/image12.gif)
