---
uid: identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure
title: Procedure consigliate per la distribuzione delle password e altri dati sensibili in ASP.NET e servizio App di Azure | Microsoft Docs
author: Rick-Anderson
description: Questa esercitazione illustra come il codice è possibile archiviare e accedere alle informazioni protette in modo sicuro. Il punto più importante è che evitare di archiviare le password o altri servizi...
ms.author: aspnetcontent
ms.date: 05/21/2015
ms.assetid: 97902c66-cb61-4d11-be52-73f962f2db0a
msc.legacyurl: /identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure
msc.type: authoredcontent
ms.openlocfilehash: 445e7c32baf4316273b0a5901a776684a6c5d73f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832455"
---
<a name="best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure-app-service"></a>Le procedure consigliate per la distribuzione delle password e altri dati sensibili in ASP.NET e servizio App di Azure
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

> Questa esercitazione illustra come il codice è possibile archiviare e accedere alle informazioni protette in modo sicuro. Il punto più importante è evitare di archiviare le password o altri dati sensibili nel codice sorgente e non è consigliabile utilizzare i segreti di produzione in modalità di sviluppo e test.
> 
> Il codice di esempio è una semplice app console processo Web e un'app ASP.NET MVC che deve accedere a un database connection string password, Twilio, Google e SendGrid le chiavi di sicurezza.
> 
> In locale le impostazioni e PHP viene menzionata anche.


- [Utilizzo delle password nell'ambiente di sviluppo](#pwd)
- [Uso delle stringhe di connessione nell'ambiente di sviluppo](#con)
- [App console di WebJobs](#wj)
- [Distribuzione dei segreti in Azure](#da)
- [Note per On-Premise e PHP](#not)
- [Risorse aggiuntive](#addRes)

<a id="pwd"></a>
## <a name="working-with-passwords-in-the-development-environment"></a>Utilizzo delle password nell'ambiente di sviluppo

Le esercitazioni illustrano spesso i dati sensibili nel codice sorgente, si spera con un'avvertenza che è non necessario archiviare mai dati sensibili nel codice sorgente. Ad esempio, my [app ASP.NET MVC 5 con SMS e posta elettronica 2FA](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md) esercitazione illustra le opzioni seguenti nella *Web. config* file:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample1.xml)]

Il *Web. config* file è il codice sorgente, in modo che questi segreti non devono mai essere archiviati in tale file. Per fortuna, il `<appSettings>` elemento ha un `file` attributo che consente di specificare un file esterno che contiene le impostazioni di configurazione app sensibili. È possibile spostare tutti i segreti in un file esterno, purché il file esterno non è selezionato nell'albero di origine. Ad esempio, nel markup seguente, il file *AppSettingsSecrets.config* contiene tutti i segreti dell'app:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample2.xml)]

Il markup nel file esterno (*AppSettingsSecrets.config* in questo esempio), lo stesso markup si trova nel *Web. config* file:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample3.xml)]

Il runtime ASP.NET unisce il contenuto del file esterno con il markup nel &lt;appSettings&gt; elemento. Il runtime ignora l'attributo del file se non viene trovato il file specificato.

> [!WARNING]
> Security - non aggiungere il *segreti con estensione config* file al progetto o archiviarlo nel controllo del codice sorgente. Per impostazione predefinita, Visual Studio imposta la `Build Action` a `Content`, ovvero il file viene distribuito. Per altre informazioni vedere [perché non tutti i file nella cartella del progetto vengono distribuiti?](https://msdn.microsoft.com/library/ee942158(v=vs.110).aspx#can_i_exclude_specific_files_or_folders_from_deployment) Sebbene sia possibile utilizzare qualsiasi estensione per il *segreti con estensione config* file, è consigliabile mantenere *config*, come i file di configurazione non vengono serviti da IIS. Si noti inoltre che il *AppSettingsSecrets.config* file sia due livelli di directory backup dalle *Web. config* file, in modo che rientra completamente la directory della soluzione. Spostando il file dalla directory della soluzione, &quot;git aggiungere \* &quot; non aggiungerlo al repository.


<a id="con"></a>
## <a name="working-with-connection-strings-in-the-development-environment"></a>Uso delle stringhe di connessione nell'ambiente di sviluppo

Visual Studio crea nuovi progetti ASP.NET che utilizzano [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx). Local DB è stato creato appositamente per l'ambiente di sviluppo. Non richiede una password, pertanto non è necessario eseguire alcuna operazione per evitare che i segreti controllato nel codice sorgente. Alcuni team di sviluppo utilizzare le versioni complete di SQL Server (o altri DBMS) che richiedono una password.

È possibile usare la `configSource` attributo per sostituire l'intero `<connectionStrings>` markup. A differenza di `<appSettings>` `file` attributo che unisce il markup, il `configSource` attributo sostituisce il markup. Il markup seguente mostra le `configSource` attributo la *Web. config* file:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample4.xml?highlight=1)]

> [!NOTE]
> Se si usa il `configSource` attributo come illustrato in precedenza per spostare le stringhe di connessione a un file esterno e dispone di Visual Studio creare un nuovo sito web, non sarà in grado di rilevare si usa un database e non si otterrà l'opzione di configurazione del database quando si pu bblica su Azure da Visual Studio. Se si usa il `configSource` attributo, è possibile usare PowerShell per creare e distribuire il sito web e database, oppure è possibile creare il sito web e il database nel portale, prima della pubblicazione. Il [New-AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) script creerà un nuovo sito web e database.


> [!WARNING]
> Security - a differenza di *AppSettingsSecrets.config* file, il file di stringhe di connessione esterna deve essere nella stessa directory radice *Web. config* file, in modo che è possibile adottare delle precauzioni per assicurarsi di non archiviarlo nel repository del codice sorgente.


> [!NOTE]
> **Avviso di sicurezza nel file dei segreti:** si consiglia di non usare i segreti di produzione nel test e sviluppo. Utilizzo delle password di produzione in sviluppo o test di perdite di questi segreti.


<a id="wj"></a>
## <a name="webjobs-console-apps"></a>App console di WebJobs

Il *app. config* file utilizzato da un'app console non supporta i percorsi relativi, ma supporta percorsi assoluti. È possibile usare un percorso assoluto per spostare i segreti all'esterno delle directory del progetto. Il markup seguente mostra i segreti nel *C:\secrets\AppSettingsSecrets.config* file e i dati non sensibili nel *app. config* file.

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample5.xml?highlight=2)]

<a id="da"></a>
## <a name="deploying-secrets-to-azure"></a>Distribuzione dei segreti in Azure

Quando si distribuisce l'app web in Azure, il *AppSettingsSecrets.config* file non verrà distribuito (che è risultato desiderato). È possibile seguire per il [portale di gestione di Azure](https://azure.microsoft.com/services/management-portal/) e impostarli manualmente, eseguire questa operazione:

1. Passare a [ https://portal.azure.com ](https://portal.azure.com)e accedere con le credenziali di Azure.
2. Fare clic su **esplorare &gt; App Web**, quindi fare clic sul nome dell'app web.
3. Fare clic su **tutte le impostazioni &gt; impostazioni applicazione**.

Il **le impostazioni dell'app** e **stringa di connessione** valori eseguono l'override le impostazioni del *Web. config* file. In questo esempio, Microsoft non sono stati distribuiti queste impostazioni in Azure, ma se queste chiavi sono state nel *Web. config* file, le impostazioni presenti nel portale del sarebbero hanno la precedenza.

Una procedura consigliata consiste nel seguire un [flusso di lavoro DevOps](../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything.md) e usare [Azure PowerShell](https://azure.microsoft.com/documentation/articles/install-configure-powershell/) (o un altro framework, ad esempio [Chef](http://www.opscode.com/chef/) oppure [Puppet](http://puppetlabs.com/puppet/what-is-puppet)) per automatizza l'impostazione di questi valori in Azure. Il seguente script di PowerShell Usa [Export-CliXml](http://www.powershellcookbook.com/recipe/PukO/securely-store-credentials-on-disk) per esportare i segreti crittografati su disco:

[!code-powershell[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample6.ps1)]

Nello script precedente, 'Name' è il nome della chiave privata, ad esempio '&quot;FB\_AppSecret&quot; o "TwitterSecret". È possibile visualizzare il file ".credential" creato dallo script nel browser. Il frammento di codice seguente verifica ognuno dei file di credenziali e imposta i segreti per l'app web denominati:

[!code-powershell[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample7.ps1)]

> [!WARNING]
> Security - non includere le password o altri segreti nello script di PowerShell, eseguire in questo caso vanifica lo scopo dell'uso di uno script di PowerShell per distribuire i dati sensibili. Il [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) cmdlet offre un meccanismo protetto per ottenere una password. Usando un prompt dei comandi dell'interfaccia utente può impedire la perdita di una password.


### <a name="deploying-db-connection-strings"></a>Le stringhe di connessione di database di distribuzione

Le stringhe di connessione di database vengono gestite in modo analogo alle impostazioni dell'app. Se si distribuisce l'app web da Visual Studio, la stringa di connessione verrà configurata automaticamente. È possibile verificarlo nel portale. È consigliabile impostare la stringa di connessione con PowerShell. Per un esempio di uno script di PowerShell di crea un sito Web e database e imposta la stringa di connessione nel sito Web di download [New-AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) dal [libreria di Script di Azure](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&amp;f%5B0%5D.Value=WindowsAzure).

<a id="not"></a>
## <a name="notes-for-php"></a>Note per PHP

Poiché le coppie chiave-valore per entrambi **le impostazioni dell'app** e **stringhe di connessione** vengono archiviati nelle variabili di ambiente nel servizio App di Azure, gli sviluppatori che usano qualsiasi can Framework (ad esempio PHP) di app web con facilità recuperare questi valori. Vedere di Stefan Schackow [siti Web di Azure: How Application Strings and Connection Strings Work](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/) post di blog che mostra un frammento di codice PHP a leggere le impostazioni dell'app e le stringhe di connessione.

## <a name="notes-for-on-premises-servers"></a>Note per i server locali

Se si distribuiscono i server web in locale, è possibile consentire proteggere segreti dal [crittografare le sezioni di configurazione dei file di configurazione](https://msdn.microsoft.com/library/ff647398.aspx). In alternativa, è possibile usare lo stesso approccio consigliato per siti Web di Azure: mantenere le impostazioni di sviluppo nei file di configurazione e usare i valori di variabile di ambiente per le impostazioni di ambiente di produzione. In questo caso, tuttavia, è necessario scrivere il codice dell'applicazione per la funzionalità è automatica in siti Web di Azure: recuperare le impostazioni dalle variabili di ambiente e utilizzare tali valori al posto delle impostazioni del file di configurazione o utilizzare le impostazioni di file di configurazione quando le variabili di ambiente non vengono trovate.

<a id="addRes"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

Per un esempio di un PowerShell script che crea un'app web e database, imposta la stringa di connessione e le impostazioni dell'app, download [New-AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) dalle [libreria di Script di Azure](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&amp;f%5B0%5D.Value=WindowsAzure). 

Vedere di Stefan Schackow [siti Web di Azure: come le stringhe applicazione e connessione funzionamento delle stringhe](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/)


Ringraziamenti Barry Dorrans speciali ( [ @blowdart ](https://twitter.com/blowdart) ) e Farre Carlos per la revisione.
