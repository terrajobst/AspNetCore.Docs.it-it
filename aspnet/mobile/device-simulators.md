---
uid: mobile/device-simulators
title: Simulare i dispositivi mobili più diffusi per il test | Microsoft Docs
author: rick-anderson
description: È possibile scaricare gli emulatori per più diffusi dispositivi mobili e browser, vedere questi collegamenti
ms.author: riande
ms.date: 10/11/2018
ms.assetid: bfb5612e-c3ec-4f28-b43b-63d781aa2272
msc.legacyurl: /mobile/device-simulators
msc.type: content
ms.openlocfilehash: 8299295154d23f8fc430676b2c8ad8efc98ad185
ms.sourcegitcommit: 6e6002de467cd135a69e5518d4ba9422d693132a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/16/2018
ms.locfileid: "49348377"
---
# <a name="simulate-popular-mobile-devices-for-testing"></a>Simulare più diffusi dispositivi mobili per i test

> È possibile scaricare gli emulatori per più diffusi dispositivi mobili e browser, vedere questi collegamenti.

| Dispositivo o Browser | Emulatore o simulatore |
| --- | --- |
| BrowserStack Hosted Virtualization Browser ![BrowserStack Hosted Virtualization Browser](device-simulators/_static/image1.png) | [Virtualizzazione Browser ospitato BrowserStack](http://browserstack.com) testare l'ambiente locale o di produzione in qualsiasi browser su qualsiasi piattaforma. È possibile creare un tunnel tra il computer e la rete BrowserStack nella propria macchina virtuale ospitata. Assicurarsi di ottenere il [estensione di Visual Studio BrowserStack](https://marketplace.visualstudio.com/items?itemName=browserstackcom.BrowserStack) per un'esperienza ancora più semplice. |
| iPhone / iPod / iPad | [Alimentazione elettrica Mobile Studio](http://www.electricplum.com/studio.aspx) iPhone e iPad i simulatori per Windows, nonché un reattivo dello strumento di progettazione. |
| Android | [Android Studio](https://developer.android.com/studio/) o [Visual Studio Emulator for Android](https://visualstudio.microsoft.com/vs/msft-android-emulator/) |
| Opera Mobile | [Emulatore classico Mobile di opera](https://www.opera.com/developer/mobile-emulator) |
| Windows Mobile 6.5.3 | [Kit di strumenti per sviluppatori per dispositivi mobili Windows 6.5.3](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=c0213f68-2e01-4e5c-a8b2-35e081dcf1ca&amp;displaylang=en) si noti che per concedere l'accesso alla rete telefonica, occorre anche la scheda di rete Virtual PC inclusi in [Virtual PC 2007](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=04d26402-3199-48a3-afa2-2dc0b40a73b6&amp;DisplayLang=en). Per la connessione Internet Explorer sul telefono per il server di sviluppo di Visual Studio, vedere [post di blog di Kiran Patil](http://kiranpatils.wordpress.com/2009/11/19/access-internetlocal-website-from-your-windows-mobile-device-emulators/). |

> [!NOTE]
> Se si desidera visualizzare l'applicazione in un dispositivo mobile, ovvero l'unica opzione per testare completamente iPhone o iPad, poiché non esiste alcun emulatore true per Windows, è necessario ospitare l'applicazione in IIS o IIS Express. È possibile usare con facilità server web integrato di Visual Studio a tale scopo, in quanto non risponderà alle richieste da altri computer.