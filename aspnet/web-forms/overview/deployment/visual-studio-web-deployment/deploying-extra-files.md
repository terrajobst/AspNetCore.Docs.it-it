---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
title: 'Distribuzione Web ASP.NET utilizzando Visual Studio: distribuire file aggiuntivi | Documenti Microsoft'
author: tdykstra
description: Questa serie di esercitazioni viene illustrato come distribuire un ASP.NET (pubblica) per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web da utilizza...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/23/2015
ms.topic: article
ms.assetid: 1cd91055-84bc-42c6-9d80-646f41429d4d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
msc.type: authoredcontent
ms.openlocfilehash: 46e18ba81c3db8bb04c5cb997bcc1607e4e38bae
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-extra-files"></a>Distribuzione Web ASP.NET utilizzando Visual Studio: distribuire file aggiuntivi
====================
Da [Tom Dykstra](https://github.com/tdykstra)

[Scaricare il progetto di avvio](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Questa serie di esercitazioni viene illustrato come distribuire un ASP.NET (pubblica) per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web utilizzando Visual Studio 2012 o Visual Studio 2010. Per informazioni sulla serie, vedere [la prima esercitazione di serie](introduction.md).


## <a name="overview"></a>Panoramica

In questa esercitazione viene illustrato come estendere il sito web-pubblicazione di Visual Studio pipeline per eseguire un'attività aggiuntiva durante la distribuzione. L'attività consiste nel copiare i file aggiuntivi che non sono presenti nella cartella del progetto per il sito web di destinazione.

Per questa esercitazione si copierà un file extra: *robots*. Si desidera distribuire il file di gestione temporanea ma non di produzione. In [la distribuzione nell'ambiente di produzione](deploying-to-production.md) dell'esercitazione, questo file è stato aggiunto al progetto e di produzione configurati profilo se non si desidera pubblicare. In questa esercitazione verrà visualizzato un metodo alternativo per gestire questa situazione, uno che risulteranno utili per tutti i file che si desidera distribuire, ma non si desidera includere nel progetto.

## <a name="move-the-robotstxt-file"></a>Spostare il file robots

Per preparare un altro metodo di gestione *robots*, in questa sezione dell'esercitazione si sposta il file in una cartella che non è incluso nel progetto e si elimina *robots* dal server di prova ambiente. È necessario eliminare il file dalla gestione temporanea in modo da poter verificare che funzioni correttamente, il nuovo metodo di distribuzione del file in tale ambiente.

1. In **Esplora**, fare doppio clic su di *robots* file e fare clic su **Escludi dal progetto**.
2. Tramite Esplora File, creare una nuova cartella nella cartella della soluzione e denominarlo *ExtraFiles*.
3. Spostare il *robots* file dal *ContosoUniversity* cartella del progetto per il *ExtraFiles* cartella.

    ![Cartella ExtraFiles](deploying-extra-files/_static/image1.png)
4. Utilizzando lo strumento FTP, eliminare il *robots* file dal sito web di gestione temporanea.

    In alternativa, è possibile selezionare **Rimuovi file aggiuntivi nella destinazione** in **Opzioni pubblicazione File** sul **impostazioni** scheda del profilo di pubblicazione di gestione temporanea, e ripubblicare di gestione temporanea.

## <a name="update-the-publish-profile-file"></a>Aggiornare il file del profilo di pubblicazione

È necessario solo *robots* in gestione temporanea, in modo il profilo di pubblicazione solo è necessario aggiornare per distribuirla è di gestione temporanea.

1. In Visual Studio, aprire *Staging.pubxml*.
2. Alla fine del file, prima della chiusura `</Project>` tag, aggiungere il markup seguente:

    [!code-xml[Main](deploying-extra-files/samples/sample1.xml)]

    Questo codice crea un nuovo *destinazione* che raccoglierà i file aggiuntivi da distribuire. Una destinazione è costituita da uno o più attività MSBuild verranno eseguite in base alle condizioni specificate.

    Il `Include` attributo specifica che la cartella in cui trovare i file è *ExtraFiles*, che si trova allo stesso livello della cartella di progetto. MSBuild raccoglierà tutti i file da tale cartella e in modo ricorsivo da tutte le sottocartelle (l'asterisco double consente di specificare le sottocartelle ricorsive). Con questo codice è possibile inserire più file e nelle sottocartelle all'interno di *ExtraFiles* cartella e tutti i verrà distribuito.

    Il `DestinationRelativePath` elemento specifica che le cartelle e file devono essere copiati nella cartella radice del sito web di destinazione, nella stessa struttura di file e cartelle come si trovano nel *ExtraFiles* cartella. Se si desidera copiare il *ExtraFiles* cartella stessa, la `DestinationRelativePath` valore sarebbe *ExtraFiles\%(RecursiveDir)%(Filename)%(Extension)*.
3. Alla fine del file, prima della chiusura `</Project>` tag, aggiungere il markup seguente che specifica quando è necessario eseguire la nuova destinazione.

    [!code-xml[Main](deploying-extra-files/samples/sample2.xml)]

    Questo codice genera il nuovo `CustomCollectFiles` destinazione deve essere eseguito ogni volta che viene eseguita la destinazione in cui vengono copiati i file nella cartella di destinazione. È presente una destinazione separata per la pubblicazione e creazione del pacchetto di distribuzione e la nuova destinazione viene inserita in entrambe le destinazioni nel caso in cui si decide di distribuire usando un pacchetto di distribuzione anziché la pubblicazione.

    Il *pubxml* file avrà ora un aspetto analogo al seguente:

    [!code-xml[Main](deploying-extra-files/samples/sample3.xml?highlight=53-71)]
4. Salvare e chiudere il *Staging.pubxml* file.

## <a name="publish-to-staging"></a>Pubblicazione di gestione temporanea

Con un clic pubblicare o la riga di comando, pubblicare l'applicazione utilizzando il profilo di gestione temporanea.

Se si utilizza un solo clic di pubblicazione, è possibile verificare il **anteprima** finestra che *robots* verranno copiati. In caso contrario, utilizzare lo strumento FTP per verificare che il *robots* file si trova nella cartella radice del sito web dopo la distribuzione.

## <a name="summary"></a>Riepilogo

In questo passaggio si completa questa serie di esercitazioni sulla distribuzione di un'applicazione web ASP.NET in un provider di hosting di terze parti. Per ulteriori informazioni sugli argomenti trattati in queste esercitazioni, vedere il [mappa del contenuto di distribuzione di ASP.NET](https://go.microsoft.com/fwlink/p/?LinkId=282413).

## <a name="more-information"></a>Altre informazioni

Se si conosce l'utilizzo del file di MSBuild, è possibile automatizzare molte altre attività di distribuzione mediante la scrittura di codice *pubxml* file (per attività specifiche di profilo) o il progetto *. wpp.targets* file (per le attività che si applica a tutti i profili). Per ulteriori informazioni su *pubxml* e *. wpp.targets* file, vedere [come: modificare le impostazioni di distribuzione nei file di profilo di pubblicazione (con estensione pubxml) e. wpp.targets File nel sito Web di Visual Studio Progetti](https://msdn.microsoft.com/library/ff398069). Per un'introduzione al codice di MSBuild, vedere **l'anatomia di un File di progetto** in [serie distribuzione aziendale: informazioni sui File di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Per informazioni su come utilizzare i file di MSBuild per eseguire attività per i propri scenari, vedere il manuale: [all'interno di Microsoft Build Engine: uso di MSBuild e Team Foundation Build](http://msbuildbook.com) Sayed Ibraham Hashimi e William Bartholomew.

## <a name="acknowledgements"></a>Riconoscimenti

Come è possibile grazie seguenti persone che hanno apportato contributi significativi per il contenuto di questa serie di esercitazioni:

- [Alberto Poblacion, MVP &amp; MCT, (Spagna)](https://mvp.microsoft.com/mvp/Alberto%20Poblacion%20Bolano-36772)
- Stati Uniti Jarod Ferguson, MVP di sviluppo della piattaforma di dati,
- Harsh Mittal, Microsoft
- [Jon Galloway](https://weblogs.asp.net/jgalloway) (twitter: [ @jongalloway ](http://twitter.com/jongalloway))
- [Kristina Olson, Microsoft](https://blogs.iis.net/krolson/default.aspx)
- [Mike Pope, Microsoft](http://www.mikepope.com/blog/DisplayBlog.aspx)
- Mohit Srivastava, Microsoft
- [Raffaele Rialdi (Italia)](http://www.iamraf.net/)
- [Rick Anderson, Microsoft](https://blogs.msdn.com/b/rickandy/)
- [Hashimi sayed, Microsoft](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))
- [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))
- [Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))
- [Srđan Božović, Serbia](http://msforge.net/blogs/zmajcek/)
- [Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))

>[!div class="step-by-step"]
[Precedente](command-line-deployment.md)
[Successivo](troubleshooting.md)
