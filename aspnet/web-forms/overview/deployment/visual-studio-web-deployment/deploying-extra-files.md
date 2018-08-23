---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
title: 'Distribuzione Web ASP.NET tramite Visual Studio: distribuzione Extra Files | Microsoft Docs'
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire, pubblicare, ASP.NET per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web da utilizza...
ms.author: riande
ms.date: 03/23/2015
ms.assetid: 1cd91055-84bc-42c6-9d80-646f41429d4d
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
msc.type: authoredcontent
ms.openlocfilehash: 42b9089ba0662cf0912d2b637edec98f59c69927
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834015"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-extra-files"></a>Distribuzione Web ASP.NET tramite Visual Studio: distribuzione di file aggiuntivi
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Download progetto iniziale](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Questa serie di esercitazioni illustra come distribuire, pubblicare, ASP.NET per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web usando Visual Studio 2012 o Visual Studio 2010. Per informazioni sulla serie, vedere [la prima esercitazione della serie](introduction.md).


## <a name="overview"></a>Panoramica

Questa esercitazione illustra come estendere il sito web-pubblicazione di Visual Studio pipeline per eseguire un'attività aggiuntiva durante la distribuzione. L'attività consiste nel copiare i file aggiuntivi non presenti nella cartella del progetto al sito web di destinazione.

Per questa esercitazione si copierà un file aggiuntivo: *robots*. Si desidera distribuire questo file in gestione temporanea, ma non nell'ambiente di produzione. Nelle [la distribuzione nell'ambiente di produzione](deploying-to-production.md) dell'esercitazione, è questo file viene aggiunto al progetto e configurato l'ambiente di produzione per essere esclusa profilo di pubblicazione. In questa esercitazione si noterà un metodo alternativo per gestire questa situazione, uno che risulteranno utili per tutti i file che si desidera distribuire, ma non si desidera includere nel progetto.

## <a name="move-the-robotstxt-file"></a>Spostare il file robots. txt

Per preparare un altro metodo di gestione *robots*, in questa sezione dell'esercitazione spostare il file in una cartella in cui non è incluso nel progetto e si elimina *robots* dal server di prova ambiente. È necessario eliminare il file dalla gestione temporanea in modo che sia possibile verificare che funzioni correttamente, il nuovo metodo di distribuzione del file in tale ambiente.

1. Nella **Esplora soluzioni**, fare doppio clic il *robots* del file e fare clic su **Escludi dal progetto**.
2. Usando Esplora File di Windows, creare una nuova cartella nella cartella della soluzione e denominarlo *ExtraFiles*.
3. Spostare il *robots* del file dal *ContosoUniversity* cartella del progetto per il *ExtraFiles* cartella.

    ![Cartella ExtraFiles](deploying-extra-files/_static/image1.png)
4. Usando lo strumento FTP, eliminare il *robots* file dal sito web di staging.

    In alternativa, è possibile selezionare **Rimuovi file aggiuntivi nella destinazione** sotto **Opzioni pubblicazione File** sul **impostazioni** scheda del profilo di pubblicazione di gestione temporanea, e pubblicare di nuovo alla gestione temporanea.

## <a name="update-the-publish-profile-file"></a>Aggiornare il file di profilo di pubblicazione

È sufficiente *robots* in gestione temporanea, in modo che il profilo di pubblicazione solo è necessario Aggiorna in modo da distribuirlo è di gestione temporanea.

1. In Visual Studio, aprire *Staging.pubxml*.
2. Alla fine del file, prima della chiusura `</Project>` tag, aggiungere il markup seguente:

    [!code-xml[Main](deploying-extra-files/samples/sample1.xml)]

    Questo codice crea un nuovo *destinazione* che raccoglierà i file aggiuntivi da distribuire. Una destinazione è costituita da uno o più attività che MSBuild verrà eseguito in base alle condizioni specificate.

    Il `Include` attributo specifica che la cartella in cui trovare i file sia *ExtraFiles*, che si trova sullo stesso livello della cartella di progetto. MSBuild raccoglierà tutti i file da tale cartella e in modo ricorsivo da eventuali sottocartelle (l'asterisco double consente di specificare le sottocartelle ricorsive). Con questo codice è possibile inserire più file e i file nelle sottocartelle all'interno di *ExtraFiles* cartella e tutto verrà distribuito.

    Il `DestinationRelativePath` elemento specifica che le cartelle e file devono essere copiati nella cartella radice del sito web di destinazione, nella stessa struttura di file e cartelle quando vengono individuate nel *ExtraFiles* cartella. Se si desidera copiare le *ExtraFiles* cartella stessa, il `DestinationRelativePath` valore sarebbe *ExtraFiles\%(RecursiveDir)%(Filename)%(Extension)*.
3. Alla fine del file, prima della chiusura `</Project>` tag, aggiungere il markup seguente che specifica quando è necessario eseguire la nuova destinazione.

    [!code-xml[Main](deploying-extra-files/samples/sample2.xml)]

    Questo codice fa in modo che il nuovo `CustomCollectFiles` destinazione deve essere eseguito ogni volta che viene eseguita la destinazione in cui vengono copiati i file nella cartella di destinazione. È presente una destinazione separata per pubblicare e creazione di pacchetti di distribuzione e la nuova destinazione viene inserita in entrambe le destinazioni nel caso in cui si decide di distribuire usando un pacchetto di distribuzione anziché la pubblicazione.

    Il *pubxml* file avrà ora un aspetto simile al seguente:

    [!code-xml[Main](deploying-extra-files/samples/sample3.xml?highlight=53-71)]
4. Salvare e chiudere il *Staging.pubxml* file.

## <a name="publish-to-staging"></a>Pubblicare in gestione temporanea

Un solo clic tramite la pubblicazione o la riga di comando, pubblicare l'applicazione tramite il profilo di gestione temporanea.

Se si usa un solo clic pubblica, è possibile verificare nel **Preview** finestra che *robots* verranno copiati. In caso contrario, utilizzare lo strumento FTP per verificare che il *robots* file si trova nella cartella radice del sito web dopo la distribuzione.

## <a name="summary"></a>Riepilogo

In questo passaggio si completa questa serie di esercitazioni sulla distribuzione di un'applicazione web ASP.NET in un provider di hosting di terze parti. Per altre informazioni su uno qualsiasi degli argomenti trattati in queste esercitazioni, vedere la [mappa del contenuto ASP.NET distribuzione](https://go.microsoft.com/fwlink/p/?LinkId=282413).

## <a name="more-information"></a>Altre informazioni

Se si sa come lavorare con i file di MSBuild, è possibile automatizzare molte altre attività di distribuzione mediante la scrittura di codice *pubxml* i file (per attività specifiche di profilo) o il progetto *. WPP* file (per le attività che si applicano a tutti i profili). Per altre informazioni sulle *pubxml* e *. WPP* i file, vedere [come: modificare le impostazioni di distribuzione nei file di profilo di pubblicazione (con estensione pubxml) e il. WPP File nel Web in Visual Studio I progetti](https://msdn.microsoft.com/library/ff398069). Per un'introduzione al codice di MSBuild, vedere **Anatomia di un File di progetto** nelle [serie sulla distribuzione aziendale: informazioni sul File di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Per informazioni su come lavorare con i file di MSBuild per eseguire attività per i propri scenari, vedere questo libro: [all'interno di Microsoft Build Engine: uso di MSBuild e Team Foundation Build](http://msbuildbook.com) Sayed Ibraham Hashimi e William Bartholomew.

## <a name="acknowledgements"></a>Riconoscimenti

Vorrei ringraziare i seguenti persone che hanno apportato contributi significativi per il contenuto di questa serie di esercitazioni:

- [Alberto Poblacion, MVP &amp; MCT, Spagna](https://mvp.microsoft.com/mvp/Alberto%20Poblacion%20Bolano-36772)
- Jarod Ferguson, sviluppo della piattaforma dati MVP, Stati Uniti
- Harsh Mittal, Microsoft
- [Jon Galloway](https://weblogs.asp.net/jgalloway) (twitter: [ @jongalloway ](http://twitter.com/jongalloway))
- [Kristina Olson, Microsoft](https://blogs.iis.net/krolson/default.aspx)
- [Mike Pope, Microsoft](http://www.mikepope.com/blog/DisplayBlog.aspx)
- Mohit Srivastava, Microsoft
- [Raffaele Rialdi (Italia)](http://www.iamraf.net/)
- [Rick Anderson, Microsoft](https://blogs.msdn.com/b/rickandy/)
- [Microsoft, sayed Hashimi](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))
- [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))
- [Microsoft, Scott Hunter](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))
- [Srđan Božović, Serbia e Montenegro](http://msforge.net/blogs/zmajcek/)
- [Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))

> [!div class="step-by-step"]
> [Precedente](command-line-deployment.md)
> [Successivo](troubleshooting.md)
