---
title: Gestire i pacchetti lato client con Bower in ASP.NET Core
author: rick-anderson
description: La gestione dei pacchetti lato client con Bower.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 08/09/2018
uid: client-side/bower
ms.openlocfilehash: 8606c21596a5d9d6ada9c60b55b2f54da21c601b
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/22/2018
ms.locfileid: "41902719"
---
# <a name="manage-client-side-packages-with-bower-in-aspnet-core"></a>Gestire i pacchetti lato client con Bower in ASP.NET Core

Dal [Rick Anderson](https://twitter.com/RickAndMSFT), [Noel riso](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/), e [Scott Addie](https://scottaddie.com)

> [!IMPORTANT]
> Mentre viene mantenuta Bower, i relativi gestori consigliabile usare una soluzione diversa. [Gestione librerie](https://blogs.msdn.microsoft.com/webdev/2018/04/18/what-happened-to-bower/) (LibMan per brevità) è nuova libreria lato client acquisizione degli strumenti Visual Studio (Visual Studio 15,8 o versione successiva). Per altre informazioni, vedere <xref:client-side/libman/index>. Bower è supportata in Visual Studio fino alla versione 15.5.
>
> Yarn con Webpack è una diffusa alternativa per il quale [istruzioni relative alla migrazione](https://bower.io/blog/2017/how-to-migrate-away-from-bower/) sono disponibili.

[Bower](https://bower.io/) chiama se stessa "Gestione pacchetti per il web". All'interno dell'ecosistema .NET, riempie il vuoto a sinistra di impossibilità di NuGet per recapitare i file di contenuto statici. Per i progetti ASP.NET Core, sono inerenti alle librerie lato client, ad esempio i file statici [jQuery](http://jquery.com/) e [Bootstrap](http://getbootstrap.com/). Per le librerie .NET, è comunque usare [NuGet](https://www.nuget.org/) Gestione pacchetti.

Processo di compilazione di nuovi progetti creati con i modelli di progetto ASP.NET Core configurare lato client. [jQuery](http://jquery.com/) e [Bootstrap](http://getbootstrap.com/) sono installati, e Bower è supportata.

Vengono elencati i pacchetti lato client nel *bower. JSON* file. Consente di configurare i modelli di progetto ASP.NET Core *bower. JSON* con jQuery, la convalida di jQuery e Bootstrap.

In questa esercitazione, verrà aggiunto il supporto per [Font Awesome](http://fontawesome.io). I pacchetti bower possono essere installati con il **Gestisci pacchetti Bower** dell'interfaccia utente o manualmente nel *bower. JSON* file.

### <a name="installation-via-manage-bower-packages-ui"></a>Installazione tramite pacchetti Bower Gestisci dell'interfaccia utente

* Creare una nuova app Web ASP.NET Core con il **applicazione Web ASP.NET Core (.NET Core)** modello. Selezionare **applicazione Web** e **Nessuna autenticazione**.

* Fare clic sul progetto in Esplora soluzioni e selezionare **Gestisci pacchetti Bower** (in alternativa dal menu principale **Project** > **Gestisci pacchetti Bower**).

* Nel **Bower: \<nome progetto\>**  finestra, fare clic sulla scheda "Sfoglia" e quindi filtrare l'elenco di pacchetti tramite l'immissione di `font-awesome` nella casella di ricerca:

  ![Gestisci pacchetti bower](bower/_static/manage-bower-packages.png)

* Verificare che il "salvare le modifiche *bower. JSON*" sia selezionata la casella di controllo. Selezionare una versione dall'elenco a discesa elenco e scegliere il **installare** pulsante. Il **Output** finestra Mostra i dettagli di installazione.

### <a name="manual-installation-in-bowerjson"></a>Installazione manuale in bower. JSON

Aprire il *bower. JSON* file e aggiungere "font awesome" alle dipendenze. IntelliSense mostra i pacchetti disponibili. Quando viene selezionato un pacchetto, vengono visualizzate le versioni disponibili. Le immagini seguenti sono meno recenti e non corrisponderanno a ciò che viene visualizzato.

![IntelliSense di Esplora pacchetti bower](bower/_static/add-package.png)

![versione bower IntelliSense](bower/_static/version-intelliSense.png)

Viene utilizzato per bower [versionamento semantico](http://semver.org/) per organizzare le dipendenze. Versionamento semantico, noto anche come SemVer, identifica i pacchetti con lo schema di numerazione \<principale >.\< secondaria >. \<patch >. IntelliSense semplifica versionamento semantico mostrando solo alcune opzioni comuni. Il primo elemento nell'elenco di IntelliSense (4.6.3 nell'esempio precedente) viene considerato la versione stabile più recente del pacchetto. Il simbolo di accento circonflesso (^) corrisponde alla versione principale più recente e la tilde (~) corrisponde alla versione secondaria più recente.

Salvare il *bower. JSON* file. Visual Studio controlla le *bower. JSON* file per le modifiche. Al momento del salvataggio, il *bower install* comando viene eseguito. Vedere la finestra di Output **npm/Bower** visualizzazione per il comando esatto eseguito.

Aprire il *. bowerrc* del file sotto *bower. JSON*. Il `directory` è impostata su *wwwroot/lib* che indica la posizione Bower installerà gli asset di pacchetti.

```json
{
 "directory": "wwwroot/lib"
}
```

Per individuare e visualizzare il pacchetto font awesome, è possibile utilizzare la casella di ricerca in Esplora soluzioni.

Aprire il *Views\Shared\_layout. cshtml* file e aggiungere il file CSS font awesome all'ambiente [Helper Tag](xref:mvc/views/tag-helpers/intro) per `Development`. Da Esplora soluzioni, trascinare e rilasciare *font awesome.css* all'interno di `<environment names="Development">` elemento.

[!code-html[](bower/sample/_Layout.cshtml?highlight=4&range=9-13)]

In un'app di produzione aggiungerebbe *font awesome.min.css* per l'helper tag di ambiente per `Staging,Production`.

Sostituire il contenuto del *Views\Home\About.cshtml* file Razor con il markup seguente:

[!code-html[](bower/sample/About.cshtml)]

Eseguire l'app e passare alla visualizzazione About per verificare il funzionamento del pacchetto font awesome.

## <a name="exploring-the-client-side-build-process"></a>Esplorare il processo di compilazione dal lato client

La maggior parte dei modelli di progetto ASP.NET Core sono già configurati per usare Bower. Questa procedura dettagliata successiva inizia con un progetto ASP.NET Core vuoto e aggiunge ogni pezzo manualmente, in modo che è possibile avere un'idea di come Bower viene utilizzato in un progetto. È possibile visualizzare cosa accade alla struttura di progetto e il runtime perché viene effettuata ogni modifica della configurazione di output.

I passaggi generali per usare il processo di compilazione dal lato client con Bower sono:

* Definire pacchetti usati nel progetto. <!-- once defined, you don't need to download them, VS does -->
* Pacchetti di riferimento dalle pagine web.

### <a name="define-packages"></a>Definire pacchetti

Quando si elencano i pacchetti nel *bower. JSON* file, Visual Studio li scaricheranno. L'esempio seguente usa Bower caricare jQuery e Bootstrap per i *wwwroot* cartella.

* Creare una nuova app Web ASP.NET Core con il **applicazione Web ASP.NET Core (.NET Core)** modello. Selezionare il **vuote** modello di progetto e fare clic su **OK**.

* In Esplora soluzioni fare clic sul progetto > **Aggiungi nuovo elemento** e selezionare **File di configurazione Bower**. Nota: Una *. bowerrc* verrà inoltre aggiunto file.

* Aprire *bower. JSON*, quindi aggiungere jquery e bootstrap per il `dependencies` sezione. L'oggetto risultante *bower. JSON* file avrà un aspetto simile all'esempio seguente. Le versioni cambieranno nel corso del tempo e potrebbero non corrispondere a quella riportata di seguito.

[!code-json[](bower/sample/bower.json?highlight=5,6)]

* Salvare il *bower. JSON* file.

  Verificare che il progetto includa il *bootstrap* e *jQuery* directory nel *wwwroot/lib*. Bower Usa il *. bowerrc* per installare gli asset di file *wwwroot/lib*.

  Nota: L'interfaccia utente "Gestisci pacchetti Bower" fornisce un'alternativa alla modifica del file manuale.

### <a name="enable-static-files"></a>Abilitare i file statici

* Aggiungere il `Microsoft.AspNetCore.StaticFiles` pacchetto NuGet al progetto.
* Abilitare i file statici essere serviti con la [middleware dei file statici](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions). Aggiungere una chiamata a [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions) per il `Configure` metodo `Startup`.

[!code-csharp[](bower/sample/Startup.cs?highlight=9)]

### <a name="reference-packages"></a>Pacchetti di riferimento

In questa sezione si creerà una pagina HTML per verificare che possa accedere i pacchetti distribuiti.

* Aggiungere una nuova pagina HTML denominata *index. HTML* per il *wwwroot* cartella. Nota: È necessario aggiungere il file HTML per il *wwwroot* cartella. Per impostazione predefinita, il contenuto statico non può essere visualizzato di fuori *wwwroot*. Visualizzare [i file statici](xref:fundamentals/static-files) per altre informazioni.

  Sostituire il contenuto del *index. HTML* con il markup seguente:

[!code-html[](bower/sample/Index.html)]

* Eseguire l'app e passare a `http://localhost:<port>/Index.html`. In alternativa, con *index. HTML* aperta, premere `Ctrl+Shift+W`. Verificare che l'applicazione di stili jumbotron venga applicato, il codice jQuery risponde quando si fa clic sul pulsante e che il pulsante Bootstrap viene modificato lo stato.

  ![stile Jumbotron applicato](bower/_static/jumbotron.png)
