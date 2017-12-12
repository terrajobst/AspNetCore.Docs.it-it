---
uid: web-forms/overview/moving-to-aspnet-20/profiles-themes-and-web-parts
title: I profili, temi e Web part | Documenti Microsoft
author: microsoft
description: Sono disponibili le principali modifiche alla configurazione e strumentazione di ASP.NET 2.0. La nuova API di configurazione di ASP.NET consente di apportare modifiche di configurazione da apportare pr...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 92df4051-77c6-492c-bd34-23d24189cea4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/profiles-themes-and-web-parts
msc.type: authoredcontent
ms.openlocfilehash: c9fe97dbd5fe10cbde25b9daf5ddd35b2d7eaab5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="profiles-themes-and-web-parts"></a>I profili, temi e Web part
====================
da [Microsoft](https://github.com/microsoft)

> Sono disponibili le principali modifiche alla configurazione e strumentazione di ASP.NET 2.0. La nuova API di configurazione di ASP.NET consente di apportare modifiche di configurazione da eseguire a livello di codice. Inoltre, molte nuove impostazioni di configurazione esistano Consenti per le nuove configurazioni e la strumentazione.


ASP.NET 2.0 rappresenta un notevole miglioramento nell'area dei siti Web personalizzati. Oltre a weve di funzionalità di appartenenza già coperte, profili ASP.NET, temi e Web part aumentare notevolmente la personalizzazione in siti Web.

## <a name="aspnet-profiles"></a>Profili ASP.NET

I profili ASP.NET sono simili alle sessioni. La differenza è che un profilo è persistente, mentre una sessione viene persa quando il browser viene chiuso. Un'altra importante differenza tra le sessioni e i profili è che i profili sono fortemente tipizzati, pertanto fornire IntelliSense durante il processo di sviluppo.

Un profilo è definito nel file di configurazione del computer o il file Web. config per l'applicazione. (È possibile definire un profilo in un file Web. config le sottocartelle). Il codice seguente definisce un profilo per l'archiviazione prima di tutto i visitatori del sito Web e il cognome.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample1.xml)]

Il tipo di dati predefinito per una proprietà del profilo è System. String. Nell'esempio precedente, è stato specificato alcun tipo di dati. Di conseguenza le proprietà FirstName e LastName sono entrambi di tipo stringa. Come accennato in precedenza, profilo di proprietà sono fortemente tipizzati. Il codice seguente aggiunge una nuova proprietà per età che è di tipo Int32.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample2.xml)]

I profili sono in genere utilizzati con autenticazione basata su form ASP.NET. Quando utilizzato in combinazione con l'autenticazione basata su form, ogni utente dispone di un profilo distinto associato al proprio ID utente. Tuttavia, è anche possibile consentire l'uso di profili in un'applicazione anonimo utilizzando il &lt;anonymousIdentification&gt; elemento nel file di configurazione con il **allowAnonymous** attributo di seguito riportato:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample3.xml)]

Quando un utente anonimo accede al sito, in ASP.NET viene creata un'istanza di **ProfileCommon** per l'utente. Questo profilo viene utilizzato un ID univoco memorizzato in un cookie nel browser per identificare l'utente come un visitatore unico. In questo modo, è possibile archiviare le informazioni per gli utenti che esplorano in modo anonimo.

## <a name="profile-groups"></a>Gruppi di profili

È possibile per le proprietà di gruppo dei profili. Dalle proprietà di raggruppamento, è possibile simulare più profili per un'applicazione specifica.

La configurazione seguente consente di configurare una proprietà FirstName e LastName per due gruppi. Gli acquirenti e prospettive.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample4.xml)]

È quindi possibile impostare le proprietà su un gruppo specifico, come indicato di seguito:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample5.cs)]

## <a name="storing-complex-objects"></a>L'archiviazione di oggetti complesso

Finora, abbiamo trattato negli esempi sono archiviati i tipi di dati semplice in un profilo. È anche possibile archiviare i tipi di dati complessi in un profilo, specificando il metodo di serializzazione utilizzando il **serializeAs** attributo come indicato di seguito:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample6.xml)]

In questo caso, il tipo è PurchaseInvoice. La classe PurchaseInvoice deve essere contrassegnato come serializzabile e può contenere qualsiasi numero di proprietà. Ad esempio, se PurchaseInvoice ha una proprietà denominata **NumItemsPurchased**, è possibile fare riferimento a tale proprietà nel codice come segue:

[!code-css[Main](profiles-themes-and-web-parts/samples/sample7.css)]

## <a name="profile-inheritance"></a>Ereditarietà di profilo

È possibile creare un profilo per l'utilizzo in più applicazioni. Creando una classe che deriva da ProfileBase di un profilo, è possibile riutilizzare un profilo in diverse applicazioni utilizzando il **eredita** attributo come illustrato di seguito:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample8.xml)]

In questo caso, la classe **PurchasingProfile** si presenta come segue:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample9.cs)]

## <a name="profile-providers"></a>Provider di profili

I profili ASP.NET utilizzano il modello di provider. Il provider predefinito archivia le informazioni in un database di SQL Server Express nell'App\_cartella dati dell'applicazione Web utilizzando il provider SqlProfileProvider. Se il database non esiste, ASP.NET verrà creato automaticamente quando il profilo tenta di archiviare le informazioni.

In alcuni casi, tuttavia, può essere sviluppare un provider di profili. La funzionalità di profilo ASP.NET consente di utilizzare facilmente provider diversi.

Creare un provider di profilo personalizzato quando:

- È necessario archiviare le informazioni in un'origine dati, ad esempio in un database FoxPro o in un database Oracle, che non è supportato dai provider di profili inclusi in .NET Framework.
- È necessario gestire le informazioni sul profilo utilizzando uno schema di database che è diverso dallo schema del database utilizzato dai provider incluso in .NET Framework. Un esempio comune è che si desidera integrare le informazioni sul profilo con i dati utente in un database di SQL Server esistente.

### <a name="required-classes"></a>Classi necessarie

Per implementare un provider di profili, si crea una classe che eredita la classe astratta System.Web.Profile.ProfileProvider. Il **ProfileProvider** astratta a sua volta eredita la classe astratta System.Configuration.SettingsProvider, che eredita la classe astratta. ProviderBase. A causa di questa catena di ereditarietà, oltre ai membri obbligatori del **ProfileProvider** (classe), è necessario implementare i membri del **SettingsProvider** e  **ProviderBase** classi.

Le tabelle seguenti descrivono le proprietà e metodi che è necessario implementare dal **ProviderBase**, **SettingsProvider**, e **ProfileProvider** astratta classi.

### <a name="providerbase-members"></a>Membri di ProviderBase

| **Membro** | **Descrizione** |
| --- | --- |
| Initialize (metodo) | Accetta come input il nome dell'istanza del provider e NameValueCollection delle impostazioni di configurazione. Utilizzato per impostare le opzioni e i valori delle proprietà per l'istanza del provider, inclusi i valori specifici dell'implementazione e le opzioni specificate in configurazione del computer o il file Web. config. |

### <a name="settingsprovider-members"></a>Membri di SettingsProvider

| **Membro** | **Descrizione** |
| --- | --- |
| Proprietà ApplicationName | Il nome dell'applicazione che viene archiviato con ogni profilo. Il provider di profilo utilizza il nome dell'applicazione per archiviare informazioni sul profilo separatamente per ogni applicazione. In questo modo più applicazioni ASP.NET di utilizzare la stessa origine dati senza conflitti, se lo stesso nome utente viene creato in applicazioni diverse. In alternativa, più applicazioni ASP.NET è possono condividere un'origine dati di profilo, specificando il nome dell'applicazione stessa. |
| Metodo GetPropertyValues | Accetta come input un SettingsContext e un oggetto SettingsPropertyCollection. Il **SettingsContext** fornisce informazioni sull'utente. È possibile utilizzare le informazioni come chiave primaria per recuperare informazioni sulle proprietà di profilo per l'utente. Utilizzare il **SettingsContext** oggetto per cui ottenere il nome utente e se l'utente è autenticato o anonimo. Il **SettingsPropertyCollection** contiene una raccolta di oggetti SettingsProperty. Ogni **SettingsProperty** oggetto fornisce il nome e il tipo di proprietà, nonché informazioni aggiuntive, ad esempio il valore predefinito per la proprietà e se la proprietà è di sola lettura. Il **GetPropertyValues** metodo popola un SettingsPropertyValueCollection con SettingsPropertyValue oggetti basati sul **SettingsProperty** oggetti forniti come input. I valori dall'origine dati per l'utente specificato vengono assegnati alle proprietà PropertyValue per ogni **SettingsPropertyValue** viene restituito l'oggetto e l'intera raccolta. La chiamata al metodo aggiorna anche il valore di LastActivityDate per il profilo utente specificato per la data e ora correnti. |
| Metodo SetPropertyValues | Accetta come input un **SettingsContext** e **SettingsPropertyValueCollection** oggetto. Il **SettingsContext** fornisce informazioni sull'utente. È possibile utilizzare le informazioni come chiave primaria per recuperare informazioni sulle proprietà di profilo per l'utente. Utilizzare il **SettingsContext** oggetto per cui ottenere il nome utente e se l'utente è autenticato o anonimo. Il **SettingsPropertyValueCollection** contiene una raccolta di **SettingsPropertyValue** oggetti. Ogni **SettingsPropertyValue** oggetto fornisce il nome, tipo e valore di proprietà, nonché informazioni aggiuntive, ad esempio il valore predefinito per la proprietà e se la proprietà è di sola lettura. Il **SetPropertyValues** metodo aggiorna i valori delle proprietà del profilo nell'origine dati per l'utente specificato. La chiamata al metodo anche gli aggiornamenti di **LastActivityDate** e LastUpdatedDate valori per il profilo utente specificato per la data e ora correnti. |

### <a name="profileprovider-members"></a>Membri di ProfileProvider

| **Membro** | **Descrizione** |
| --- | --- |
| Metodo DeleteProfiles | Accetta come input dell'utente, una matrice di stringhe di nomi ed Elimina dall'origine dati per i nomi di tutti i valori informazioni e proprietà del profilo in cui il nome dell'applicazione corrisponde la **ApplicationName** valore della proprietà. Se l'origine dati supporta le transazioni, si consiglia di includere tutte le operazioni delete in una transazione e che il rollback della transazione, generare un'eccezione se qualsiasi operazione di eliminazione ha esito negativo. |
| Metodo DeleteProfiles | Accetta come input una raccolta di ProfileInfo oggetti ed Elimina dall'origine dati per ogni profilo di tutti i valori proprietà e le informazioni del profilo in cui il nome dell'applicazione corrisponde la **ApplicationName** valore della proprietà. Se l'origine dati supporta le transazioni, è consigliabile includere tutte le operazioni delete in una transazione e il rollback della transazione e genera un'eccezione se qualsiasi operazione di eliminazione ha esito negativo. |
| Metodo DeleteInactiveProfiles | Accetta come input un valore ProfileAuthenticationOption e un oggetto DateTime e le eliminazioni dei dati di tutte le informazioni di origine e i valori delle proprietà in cui la data dell'ultima attività è minore o uguale alla data specificata e all'ora e il nome dell'applicazione corrisponde la **ApplicationName** valore della proprietà. Il **ProfileAuthenticationOption** parametro specifica se solo profili anonimi, i profili autenticati oppure tutti i profili da eliminare. Se l'origine dati supporta le transazioni, è consigliabile includere tutte le operazioni delete in una transazione e il rollback della transazione e genera un'eccezione se qualsiasi operazione di eliminazione ha esito negativo. |
| Metodo GetAllProfiles | Accetta come input un **ProfileAuthenticationOption** valore integer che specifica l'indice della pagina, un numero intero che specifica le dimensioni della pagina e un riferimento a un intero che imposterà il conteggio totale dei profili. Restituisce un ProfileInfoCollection contenente **ProfileInfo** oggetti per tutti i profili nell'origine dati in cui il nome dell'applicazione corrisponde la **ApplicationName** valore della proprietà. Il **ProfileAuthenticationOption** parametro specifica se solo profili anonimi, i profili autenticati o devono essere restituiti tutti i profili. I risultati restituiti dal **GetAllProfiles** metodo sono vincolati dai valori di dimensioni di pagina e indice della pagina. Il valore delle dimensioni pagina specifica il numero massimo di **ProfileInfo** oggetti per restituire il **ProfileInfoCollection**. Il valore di indice di pagina specifica la pagina di risultati da restituire, dove 1 indica la prima pagina. Il parametro dei record totali è un parametro out (è possibile utilizzare **ByRef** in Visual Basic) che è impostato sul numero totale dei profili. Se l'archivio dati contiene 13 profili per l'applicazione e il valore di indice di pagina è 2 con una dimensione di pagina pari a 5, ad esempio il **ProfileInfoCollection** restituito contiene dal sesto al decimo profilo. Quando il metodo restituisce, il valore totale dei record viene impostato su 13. |
| Metodo GetAllInactiveProfiles | Accetta come input un **ProfileAuthenticationOption** valore, un **DateTime** oggetto, un numero intero che specifica l'indice della pagina, un numero intero che specifica le dimensioni della pagina e un riferimento a un intero che verrà impostato conteggio totale dei profili. Restituisce un **ProfileInfoCollection** contenente **ProfileInfo** oggetti per tutti i profili nell'origine dati in cui la data dell'ultima attività è minore o uguale all'oggetto specificato **DateTime**  e il nome dell'applicazione corrisponde la **ApplicationName** valore della proprietà. Il **ProfileAuthenticationOption** parametro specifica se solo profili anonimi, i profili autenticati o devono essere restituiti tutti i profili. I risultati restituiti dal **GetAllInactiveProfiles** metodo sono vincolati dai valori di dimensioni di pagina e indice della pagina. Il valore delle dimensioni pagina specifica il numero massimo di **ProfileInfo** oggetti per restituire il **ProfileInfoCollection**. Il valore di indice di pagina specifica la pagina di risultati da restituire, dove 1 indica la prima pagina. Il parametro dei record totali è un parametro out (è possibile utilizzare **ByRef** in Visual Basic) che è impostato sul numero totale dei profili. Se l'archivio dati contiene 13 profili per l'applicazione e il valore di indice di pagina è 2 con una dimensione di pagina pari a 5, ad esempio il **ProfileInfoCollection** restituito contiene dal sesto al decimo profilo. Quando il metodo restituisce, il valore totale dei record viene impostato su 13. |
| Metodo FindProfilesByUserName | Accetta come input un **ProfileAuthenticationOption** valore, una stringa contenente un nome utente, un numero intero che specifica l'indice della pagina, un numero intero che specifica le dimensioni della pagina e un riferimento a un intero che verrà impostato per il numero totale di profili. Restituisce un **ProfileInfoCollection** contenente **ProfileInfo** oggetti per tutti i profili nei dati di origine in cui il nome utente corrisponde al nome utente specificato e il nome dell'applicazione corrisponde il **ApplicationName** valore della proprietà. Il **ProfileAuthenticationOption** parametro specifica se solo profili anonimi, i profili autenticati o devono essere restituiti tutti i profili. Se l'origine dati supporta la funzionalità di ricerca aggiuntive, ad esempio i caratteri jolly, è possibile fornire più ampie funzionalità di ricerca per i nomi utente. I risultati restituiti dal **FindProfilesByUserName** metodo sono vincolati dai valori di dimensioni di pagina e indice della pagina. Il valore delle dimensioni pagina specifica il numero massimo di **ProfileInfo** oggetti per restituire il **ProfileInfoCollection**. Il valore di indice di pagina specifica la pagina di risultati da restituire, dove 1 indica la prima pagina. Il parametro dei record totali è un parametro out (è possibile utilizzare **ByRef** in Visual Basic) che è impostato sul numero totale dei profili. Se l'archivio dati contiene 13 profili per l'applicazione e il valore di indice di pagina è 2 con una dimensione di pagina pari a 5, ad esempio il **ProfileInfoCollection** restituito contiene dal sesto al decimo profilo. Quando il metodo restituisce, il valore totale dei record viene impostato su 13. |
| Metodo FindInactiveProfilesByUserName | Accetta come input un **ProfileAuthenticationOption** valore, una stringa contenente un nome utente, un **DateTime** oggetto, un numero intero che specifica l'indice della pagina, un numero intero che specifica le dimensioni della pagina e un Fare riferimento a un intero che imposterà il conteggio totale dei profili. Restituisce un **ProfileInfoCollection** contenente **ProfileInfo** gli oggetti per tutti i profili nell'origine dati in cui il nome utente corrisponde al nome utente specificato, in cui la data dell'ultima attività è minore di o uguale all'oggetto specificato **DateTime**, e il nome dell'applicazione corrisponde la **ApplicationName** valore della proprietà. Il **ProfileAuthenticationOption** parametro specifica se solo profili anonimi, i profili autenticati o devono essere restituiti tutti i profili. Se l'origine dati supporta la funzionalità di ricerca aggiuntive, ad esempio i caratteri jolly, è possibile fornire più ampie funzionalità di ricerca per i nomi utente. I risultati restituiti dal **FindInactiveProfilesByUserName** metodo sono vincolati dai valori di dimensioni di pagina e indice della pagina. Il valore delle dimensioni pagina specifica il numero massimo di **ProfileInfo** oggetti per restituire il **ProfileInfoCollection**. Il valore di indice di pagina specifica la pagina di risultati da restituire, dove 1 indica la prima pagina. Il parametro dei record totali è un parametro out (è possibile utilizzare **ByRef** in Visual Basic) che è impostato sul numero totale dei profili. Se l'archivio dati contiene 13 profili per l'applicazione e il valore di indice di pagina è 2 con una dimensione di pagina pari a 5, ad esempio il **ProfileInfoCollection** restituito contiene dal sesto al decimo profilo. Quando il metodo restituisce, il valore totale dei record viene impostato su 13. |
| Metodo GetNumberOfInActiveProfiles | Accetta come input un **ProfileAuthenticationOption** valore e un **DateTime** specificato e restituisce un conteggio di tutti i profili nell'origine dati in cui la data dell'ultima attività è minore o uguale al specificato **DateTime** e il nome dell'applicazione corrisponde la **ApplicationName** valore della proprietà. Il **ProfileAuthenticationOption** parametro specifica se solo profili anonimi, i profili autenticati oppure tutti i profili da contare. |

### <a name="applicationname"></a>ApplicationName

Poiché i provider di profilo archiviano informazioni sul profilo separatamente per ogni applicazione, è necessario assicurarsi che lo schema di dati includa il nome dell'applicazione e che le query e aggiornamenti includano anche il nome dell'applicazione. Ad esempio, il comando seguente viene utilizzato per recuperare un valore della proprietà da un database in base al nome utente e se il profilo è anonimo e assicura che il **ApplicationName** valore è incluso nella query.

[!code-sql[Main](profiles-themes-and-web-parts/samples/sample10.sql)]

## <a name="aspnet-themes"></a>Temi ASP.NET

## <a name="what-are-aspnet-20-themes"></a>Quali sono i temi di ASP.NET 2.0?

Uno degli aspetti più importanti di un'applicazione Web è un aspetto coerente tra il sito. Gli sviluppatori di 1. x ASP.NET in genere utilizzare fogli CSS (Cascading Style) per implementare un aspetto coerente. ASP.NET 2.0 temi in modo significativo migliorano CSS poiché offrono lo sviluppatore ASP.NET la possibilità di definire l'aspetto dei controlli server ASP.NET, nonché gli elementi HTML. Per singoli controlli, una pagina Web specifica o un'intera applicazione Web, è possono utilizzare i temi ASP.NET. Temi utilizzano una combinazione di file CSS, un file di interfaccia facoltativa e una directory di immagini facoltativa, se sono necessarie le immagini. Il file di interfaccia controlla l'aspetto visivo dei controlli server ASP.NET.

## <a name="where-are-themes-stored"></a>Dove sono archiviati i temi?

Il percorso in cui sono archiviati i temi varia in base al proprio ambito. Temi che possono essere applicati a qualsiasi applicazione vengono archiviati nella cartella seguente:

`C:\WINDOWS\Microsoft.NET\Framework\v2.x.xxxxx\ASP.NETClientFiles\Themes\<Theme_Name>`

Un tema specifico per una particolare applicazione verrà archiviato in un'App\_temi\&lt; Tema\_nome&gt; directory nella radice del sito Web.

> [!NOTE]
> Un file di interfaccia debba modificare solo le proprietà di controllo server che influiscono sull'aspetto.

Un tema globale è un tema che può essere applicato a qualsiasi applicazione o un sito Web in esecuzione nel server Web. I temi vengono archiviati per impostazione predefinita nella directory ASP.NETClientfiles\Themes all'interno della directory v2.x.xxxxx. In alternativa, è possibile spostare i file di tema in aspnet\_sistema client/\_web / /Themes/ [versione] [tema\_nome] cartella nella radice del sito Web.

Temi specifici dell'applicazione è applicabile solo per l'applicazione in cui si trovano i file. Questi file vengono archiviati nell'App\_temi /&lt;tema\_nome&gt; directory nella radice del sito Web.

## <a name="the-components-of-a-theme"></a>I componenti di un tema

Un tema è costituito da uno o più file CSS, un file di interfaccia facoltativa e una cartella immagini facoltativa. I file CSS possono essere qualsiasi nome che si desidera (ad esempio default.css o theme.css, e così via) deve essere nella radice della cartella di temi. I file CSS vengono utilizzati per definire le classi normali di CSS e gli attributi per i selettori specifici. Per applicare una delle classi CSS a un elemento di pagina, il **CSSClass** viene utilizzata la proprietà.

Il file di interfaccia è un file XML che contiene le definizioni di proprietà per i controlli server ASP.NET. Il codice riportato di seguito è riportato un file di interfaccia.

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample11.aspx)]

**Figura 1** seguente mostra una pagina ASP.NET small esplorazione senza un tema. **Figura 2** Mostra lo stesso file con un tema. Il colore di sfondo e testo vengono configurate mediante un file CSS. L'aspetto del pulsante e casella di testo vengono configurati utilizzando il file di interfaccia elencato in precedenza.


![Nessun tema](profiles-themes-and-web-parts/_static/image1.gif)

**Figura 1**: Nessun tema


![Tema](profiles-themes-and-web-parts/_static/image2.gif)

**Figura 2**: tema


Il file di interfaccia elencato in precedenza definisce un'interfaccia predefinita per tutti i controlli casella di testo e i controlli Button. Ciò significa che ogni casella di testo e il controllo pulsante inserito in una pagina avrà questo aspetto. È inoltre possibile definire un'interfaccia che può essere applicata a istanze specifiche di questi controlli utilizzando il **SkinID** proprietà del controllo.

Il codice seguente definisce un'interfaccia per un controllo pulsante. Controlla solo pulsante con un **SkinID** proprietà di **goButton** avrà l'aspetto dell'interfaccia.

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample12.aspx)]

È possibile avere solo un'interfaccia predefinita per ogni tipo di controllo server. Se si richiedono le interfacce aggiuntive, è necessario utilizzare la proprietà SkinID.

## <a name="applying-themes-to-pages"></a>L'applicazione di temi alle pagine

Un tema può essere applicato tramite uno dei metodi seguenti:

- Nel &lt;pagine&gt; elemento del file Web. config
- Nel @Page di una pagina
- A livello di codice

## <a name="applying-a-theme-in-the-configuration-file"></a>Applicare un tema nel File di configurazione

Per applicare un tema nel file di configurazione di applicazioni, utilizzare la sintassi seguente:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample13.xml)]

Il nome del tema specificato qui deve corrispondere al nome della cartella di temi. Questa cartella può esistere in uno dei percorsi indicato in precedenza in questo corso. Se si tenta di applicare un tema che non esiste, si verificherà un errore di configurazione.

## <a name="applying-a-theme-in-the-page-directive"></a>Applicare un tema nella direttiva della pagina

È inoltre possibile applicare un tema nella direttiva @ Page. Questo metodo consente di utilizzare un tema per una pagina specifica.

Per applicare un tema nel @Page direttiva, utilizzare la sintassi seguente:

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample14.aspx)]

In questo caso, il tema specificato qui deve corrispondere la cartella tema come indicato in precedenza. Se si tenta di applicare un tema che non esiste, si verificherà un errore di compilazione. Visual Studio verrà inoltre evidenziare l'attributo e inviare una notifica che questo tema non esiste.

## <a name="applying-a-theme-programmatically"></a>Applicare un tema a livello di codice

Per applicare un tema a livello di codice, è necessario specificare il **tema** proprietà per la pagina di **pagina\_PreInit** metodo.

Per applicare un tema a livello di codice, utilizzare la sintassi seguente:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample15.cs)]

È necessario applicare il tema nel metodo PreInit a causa della sua durata. Se lo si applica da quel punto, il tema di pagine verrà sono già stato applicato dal runtime e una modifica a questo punto è troppo tardi nel ciclo di vita. Se si applica un tema che non esiste un **HttpException** si verifica. Quando un tema viene applicato a livello di codice, verrà generato un avviso di compilazione se tutti i controlli server dispongono di una proprietà SkinID specificato. Questo avviso è informare l'utente che nessun tema viene applicato in modo dichiarativo e può essere ignorato.

## <a name="exercise-1--applying-a-theme"></a>Esercizio 1: Applicare un tema

In questo esercizio, verrà applicato un tema ASP.NET a un sito Web.

> [!IMPORTANT]
> Se si utilizza Microsoft Word per immettere le informazioni in un file di interfaccia, assicurarsi che si siano sostituendo non regolari tra virgolette con virgolette inglesi. Le virgolette inglesi si verificheranno problemi con i file di interfaccia.

1. Creare un nuovo sito Web ASP.NET.
2. Pulsante destro del mouse sul progetto in Esplora soluzioni e scegliere Aggiungi nuovo elemento.
3. Scegliere i File di configurazione Web dall'elenco di file e fare clic su Aggiungi.
4. Pulsante destro del mouse sul progetto in Esplora soluzioni e scegliere Aggiungi nuovo elemento.
5. Scegliere i File di interfaccia e fare clic su Aggiungi.
6. Fare clic su Sì quando viene chiesto se desidera inserire il file all'interno dell'App youd\_cartella temi.
7. Pulsante destro del mouse sulla cartella SkinFile all'interno dell'App\_cartella temi in Esplora soluzioni e scegliere Aggiungi nuovo elemento.
8. Scegliere il foglio di stile dall'elenco di file e fare clic su Aggiungi. È ora tutti i file necessari per implementare il nuovo tema. Tuttavia, Visual Studio è denominata la cartella di temi SkinFile. Fare clic su tale cartella e modificare il nome in CoolTheme.
9. Aprire il file SkinFile.skin e aggiungere il codice seguente la fine del file: 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample16.aspx)]
10. Salvare il file SkinFile.skin.
11. Aprire lo StyleSheet. CSS.
12. Sostituire tutto il testo in essa contenuti con gli elementi seguenti: 

    [!code-css[Main](profiles-themes-and-web-parts/samples/sample17.css)]
13. Salvare il file StyleSheet. CSS.
14. Aprire la pagina Default.aspx.
15. Aggiungere un controllo casella di testo e un controllo pulsante.
16. Salvare la pagina. Cercare la pagina Default.aspx. Viene visualizzato come normale Web form.
17. Aprire il file Web. config.
18. Aggiungere il seguente direttamente sotto l'apertura `<system.web>` tag: 

    [!code-xml[Main](profiles-themes-and-web-parts/samples/sample18.xml)]
19. Salvare il file Web. config. Cercare la pagina Default.aspx. Viene visualizzato con il tema.
20. Se non è già aperto, aprire la pagina aspx in Visual Studio.
21. Selezionare il pulsante.
22. Modifica il **SkinID** proprietà goButton. Si noti che Visual Studio fornisce un elenco a discesa con SkinID i valori validi per un controllo pulsante.
23. Salvare la pagina. Ora Visualizza di nuovo la pagina nel browser. Il pulsante dovrebbe ora essere visualizzato "go" e deve essere più ampio di aspetto.

Utilizzo di **SkinID** proprietà, è possibile configurare facilmente le interfacce diverse per istanze diverse di un determinato tipo di controllo del server.

## <a name="the-stylesheettheme-property"></a>La proprietà StyleSheetTheme

Finora, abbiamo parlato solo l'applicazione di temi mediante la proprietà del tema. Quando si utilizza la proprietà del tema, il file di interfaccia sostituiranno le impostazioni per i controlli server dichiarative. Ad esempio, nell'esercizio 1 è stato specificato un SkinID del "goButton" per il controllo Button e che è modificato il testo del pulsante per "go". Si può notare che la proprietà di testo del pulsante nella finestra di progettazione è stata impostata su "Button", ma il tema overrode che. Il tema sempre sostituiranno le impostazioni delle proprietà nella finestra di progettazione.

Se si desidera essere in grado di eseguire l'override di proprietà definite nel file di interfaccia del tema con le proprietà specificate nella finestra di progettazione, è possibile utilizzare il **StyleSheetTheme** proprietà anziché la proprietà del tema. La proprietà StyleSheetTheme è identica alla proprietà di tema ad eccezione del fatto che non esegue l'override di tutte le impostazioni di proprietà espliciti come la proprietà del tema.

Per vedere un esempio pratico, aprire il file Web. config dal progetto nell'esercizio 1 e modificare il &lt;pagine&gt; elemento per le operazioni seguenti:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample19.xml)]

Cercare la pagina Default.aspx e si noterà che il controllo pulsante ha una proprietà di testo di "Pulsante" nuovo. Ciò avviene perché l'impostazione della proprietà esplicita nella finestra di progettazione esegue l'override delle proprietà Text goButton SkinID l'impostazione.

## <a name="overriding-themes"></a>Si esegue l'override di temi

Un tema globale può essere sottoposto a override applicando un tema con lo stesso nome nell'App\_cartella temi dell'applicazione. Tuttavia, non verrà applicato il tema in uno scenario di sostituzione true. Se il runtime incontra il file di tema nell'App\_cartella temi, applicherà il tema di utilizzo dei file e ignorerà il tema globale.

La proprietà StyleSheetTheme è sottoponibile a override e può essere sottoposto a override nel codice come segue:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample20.cs)]

## <a name="web-parts"></a>Web part

Web part ASP.NET è un set integrato di controlli per la creazione di siti Web che consentono agli utenti finali di modificare il contenuto, l'aspetto e il comportamento delle pagine Web direttamente da un browser. Le modifiche possono essere applicate a tutti gli utenti nel sito o a singoli utenti. Quando gli utenti modificano pagine e controlli, le impostazioni possono essere salvate per mantenere le preferenze personali dell'utente tra le sessioni future del browser, una funzionalità di personalizzazione. Le funzionalità Web part significa che gli sviluppatori che consentono agli utenti finali di personalizzare un'applicazione Web in modo dinamico, senza l'intervento di sviluppatore o amministratore.

Utilizza il set di controlli Web part, gli sviluppatori consentono agli utenti finali di:

- Personalizzare il contenuto della pagina. Gli utenti possono aggiungere nuovi controlli Web part a una pagina, rimuoverli, nasconderle o ridurle come normale di windows.
- Personalizzare il layout di pagina. Gli utenti possono trascinare un controllo Web part a un'altra zona su una pagina o modificarne l'aspetto, proprietà e comportamento.
- Esportazione e importazione di controlli. Gli utenti possono importare o esportare le Web part controllo le impostazioni per l'uso in altre pagine o siti, mantenendo le proprietà, l'aspetto e anche i dati nei controlli. In questo modo si riduce richieste di configurazione e immissione dati agli utenti finali.
- Creazione di connessioni. Gli utenti possono creare connessioni tra i controlli in modo che, ad esempio, un controllo chart potrebbe visualizzare un grafico per i dati in un controllo di ticker per le azioni. Gli utenti è possano personalizzare non solo la connessione, ma l'aspetto e i dettagli della modalità di visualizzazione dei dati nel controllo chart.
- Gestire e personalizzare le impostazioni a livello di sito. Gli utenti autorizzati possono configurare le impostazioni a livello di sito, determinare chi può accedere a un sito o una pagina, impostare l'accesso basato sui ruoli per i controlli e così via. Ad esempio, un utente in un ruolo amministrativo potrebbe impostare un controllo Web part deve essere condiviso da tutti gli utenti e impedire agli utenti non amministratori la personalizzazione del controllo condiviso.

In genere funzionerà con le Web part in uno dei tre modi: creazione di pagine che utilizzano i controlli Web part, la creazione di singoli controlli Web part o creazione di applicazioni Web personalizzabile, completate, ad esempio un portale.

## <a name="page-development"></a>Sviluppo della pagina

Gli sviluppatori di pagine è possono utilizzare gli strumenti di progettazione visiva, ad esempio Microsoft Visual Studio 2005 per creare pagine che utilizzano le Web part. Un vantaggio di uno strumento come Visual Studio è l'insieme di controlli Web part fornisce funzionalità per la creazione di trascinamento e rilascio e configurazione dei controlli Web part in una finestra di progettazione. Ad esempio, è possibile utilizzare la finestra di progettazione per trascinare una zona Web part o un controllo editor di Web part, nell'area di progettazione e quindi configurare il controllo a destra nella finestra di progettazione utilizzando l'interfaccia utente fornita dalle Web part di set di controlli. Questo può velocizzare lo sviluppo di applicazioni Web part e ridurre la quantità di codice da scrivere.

## <a name="control-development"></a>Sviluppo di controlli

È possibile utilizzare qualsiasi controllo ASP.NET esistente come un controllo di Web part, inclusi i controlli server Web standard, i controlli server personalizzati e controlli utente. Per ottimizzare il controllo a livello di codice dell'ambiente, è anche possibile creare controlli Web part personalizzati che derivano dalla classe di Web part. Per singoli sviluppo di controlli Web part, si sarà in genere creare un controllo utente e utilizzarlo come un controllo Web part o sviluppare un controllo personalizzato di Web part.

Ad esempio di sviluppo di un controllo personalizzato di Web part, è possibile creare un controllo per fornire le funzionalità fornite da altri controlli server ASP.NET che potrebbero essere utili per pacchetto come un controllo Web part personalizzabile: calendari, elenchi, informazioni finanziarie, notizie, calcolatori, i controlli di testo RTF per l'aggiornamento contenute modificabile griglie che si connettono ai database, i grafici in modo dinamico loro consente di visualizzare, aggiornare o meteorologiche e informazioni di viaggio. Se si fornisce una finestra di progettazione con il controllo, quindi gli sviluppatori di pagina utilizzando Visual Studio possono semplicemente trascinare il controllo in una zona Web part e configurarlo in fase di progettazione senza dover scrivere codice aggiuntivo.

La personalizzazione è la base della funzionalità Web part. Consente agli utenti di modificare, o personalizzare, il layout, l'aspetto e comportamento dei controlli Web part in una pagina. Le impostazioni personalizzate sono di lunga durate: vengono rese persistenti non solo durante la sessione del browser corrente (come avviene con lo stato di visualizzazione), ma anche nell'archiviazione a lungo termine, in modo che le impostazioni dell'utente vengono salvate anche sessioni future del browser. La personalizzazione è attivata per impostazione predefinita per le pagine Web part.

I componenti strutturali di interfaccia utente si basano sulla personalizzazione e forniscono la struttura di base e i servizi richiesti da tutti i controlli Web part. Un componente strutturale di interfaccia utente richiesto in ogni pagina Web part è il controllo WebPartManager. Anche se non è visibile, il controllo non dispone dell'attività critica di coordinamento di tutti i controlli Web part in una pagina. Ad esempio, tiene traccia di tutti i singoli controlli Web part. Gestisce le zone Web part (aree che contengono i controlli Web part in una pagina), e che sono controlli alle varie zone. Inoltre, tiene traccia e di visualizzazione diverse modalità di una pagina in, ad esempio come individuare, connettersi, modificare o catalogo e se le modifiche di personalizzazione si applicano a tutti gli utenti o a singoli utenti. Infine, avvia e tiene traccia delle connessioni e la comunicazione tra i controlli Web part.

Il secondo tipo di componente strutturale dell'interfaccia utente è la zona. Le aree funzionano come gestori di layout in una pagina Web part. Essi e organizzare i controlli che derivano dalla classe della parte (parte controlla) e offrono la possibilità di eseguire il layout di pagina modulare con orientamento orizzontale o verticale. Le zone offrono anche coerenti e comuni di elementi dell'interfaccia utente (ad esempio stile dell'intestazione e piè di pagina, titolo, lo stile del bordo, pulsanti di azione e così via) per ogni controllo che contengono; Questi elementi comuni sono noti come il colore di un controllo. Diversi tipi di area specializzati vengono usati nelle modalità di visualizzazione diversi e con vari controlli. Nella sezione controlli Web part essenziali seguente sono descritti i diversi tipi di area.

L'interfaccia utente Web part, i controlli che derivano dal **parte** classe, che costituiscono l'interfaccia utente principale in una pagina Web part. Il set di controlli Web part è flessibile e numerose opzioni offre per la creazione di controlli Web part. Oltre alla creazione di controlli Web part personalizzati, è possibile inoltre utilizzare i controlli server ASP.NET esistenti, i controlli utente o controlli personalizzati di server come controlli Web part. I controlli essenziali utilizzati più frequentemente per la creazione di pagine Web part sono descritti nella sezione successiva.

## <a name="web-parts-essential-controls"></a>Essenziali controlli Web part

Il set di controlli Web part è vasto, ma alcuni controlli sono essenziali perché sono necessari per le Web part funzionino o perché sono i controlli utilizzati più di frequente nelle pagine Web part. Come iniziare a utilizzare le Web part e crea le pagine Web part, è utile acquisire familiarità con i controlli Web part essenziali descritti nella tabella seguente.

| **Controllo Web part** | **Descrizione** |
| --- | --- |
| WebPartManager | Gestisce tutti i controlli Web part in una pagina. (Una sola) **WebPartManager** controllo è obbligatorio per ogni pagina Web part. |
| CatalogZone | Contiene controlli CatalogPart. Utilizzare questa area per creare un catalogo di controlli Web part da cui gli utenti possono selezionare i controlli per aggiungere a una pagina. |
| EditorZone | Contiene controlli EditorPart. Utilizzare questa area per consentire agli utenti di modificare e personalizzare i controlli Web part in una pagina. |
| WebPartZone | Contiene e fornisce il layout complessivo per i controlli Web part che costituiscono l'interfaccia utente principale di una pagina. Utilizzare questa area ogni volta che si creano pagine con i controlli Web part. Le pagine possono contenere una o più zone. |
| ConnectionsZone | Contiene controlli WebPartConnection e fornisce un'interfaccia utente per la gestione delle connessioni. |
| Web part (GenericWebPart) | Esegue il rendering di interfaccia utente primario. la maggior parte dei controlli di interfaccia utente Web part rientrano in questa categoria. Per ottimizzare il controllo a livello di codice, è possibile creare controlli Web part personalizzati che derivano dalla base **WebPart** controllo. È inoltre possibile utilizzare i controlli server esistenti, i controlli utente o controlli personalizzati come controlli Web part. Ogni volta che uno di questi controlli vengono inserito in una zona, la **WebPartManager** controllo automaticamente ne esegue il wrapping con **GenericWebPart** controlli in fase di esecuzione in modo che è possibile utilizzare le funzionalità Web part. |
| CatalogPart | Contiene un elenco di controlli Web part disponibili che gli utenti possono aggiungere alla pagina. |
| WebPartConnection | Crea una connessione tra due controlli Web part in una pagina. La connessione definisce uno dei controlli Web part come un provider (dati) e l'altro come un consumer. |
| EditorPart | Funge da classe base per i controlli di editor specializzato. |
| Controlli EditorPart (AppearanceEditorPart LayoutEditorPart, BehaviorEditorPart e PropertyGridEditorPart) | Consentire agli utenti di personalizzare i vari aspetti di controlli di interfaccia utente Web part in una pagina |

## <a name="lab-create-a-web-part-page"></a>Esercitazione: Creare una pagina Web Part

In questo laboratorio si creerà una pagina Web part che verrà mantenute le informazioni tramite i profili ASP.NET.

### <a name="creating-a-simple-page-with-web-parts"></a>Creazione di una semplice pagina con Web part

In questa parte della procedura dettagliata, si crea una pagina che utilizza i controlli Web part per visualizzare il contenuto statico. Il primo passaggio nell'utilizzo di Web part consiste nel creare una pagina con due elementi strutturali richiesti. In primo luogo, una pagina Web part richiede un controllo WebPartManager per tenere traccia e coordinare tutti i controlli Web part. In secondo luogo, una pagina Web part richiede uno o più aree, che sono controlli compositi che contengono Web part o altri controlli server e occupano una determinata area di una pagina.

> [!NOTE]
> Non è necessario eseguire alcuna operazione per abilitare la personalizzazione delle Web part; viene abilitato per impostazione predefinita per il set di controlli Web part. Quando si esegue innanzitutto una pagina Web part in un sito, ASP.NET imposta un provider di personalizzazioni predefinito per archiviare le impostazioni di personalizzazione utente. Per ulteriori informazioni sulla personalizzazione, vedere la panoramica personalizzazione di Web part.


### <a name="to-create-a-page-for-containing-web-parts-controls"></a>Per creare una pagina contenente i controlli Web part

1. Chiudere la pagina predefinita e aggiungere una nuova pagina per il sito denominato WebPartsDemo.
2. Passare a **progettazione** visualizzazione.
3. Dal **vista** menu, verificare che il **controlli Non visivi** e **dettagli** le opzioni sono selezionate in modo da visualizzare il tag di layout e i controlli che non dispongono di un'interfaccia utente.
4. Posizionare il cursore prima di  **&lt;div&gt;**  tag nella superficie di progettazione e premere INVIO per aggiungere una nuova riga. Posizionare il cursore prima del carattere di nuova riga, fare clic su di **formato blocco** dal menu di controllo elenco a discesa e selezionare il **titolo 1** opzione. Nell'intestazione, aggiungere il testo **pagina Web part dimostrazione**.
5. Dal **WebParts** della casella degli strumenti, trascinare un **WebPartManager** controllo nella pagina, posizionarla subito dopo il carattere di nuova riga e prima di  **&lt;div&gt;**  tag.   
  
 Il **WebPartManager** controllo non eseguire il rendering di alcun output, viene visualizzato come casella grigia nell'area di progettazione.
6. Posizionare il cursore all'interno di  **&lt;div&gt;**  tag.
7. Nel **Layout** menu, fare clic su **Inserisci tabella**e creare una nuova tabella con una riga e tre colonne. Fare clic su di **proprietà delle celle** pulsante, selezionare **top** dal **allineamento verticale** elenco a discesa, fare clic su **OK**, fare clic su **OK** nuovamente per creare la tabella.
8. Trascinare un controllo WebPartZone nella colonna della tabella a sinistra. Fare doppio clic su di **WebPartZone** di controllo, scegliere **proprietà**e impostare le proprietà seguenti:   
  
 ID: SidebarZone   
  
 HeaderText: barra laterale
9. Trascinare una seconda **WebPartZone** controllare nella colonna della tabella centrale e impostare le proprietà seguenti:   
  
 ID: MainZone   
  
 HeaderText: principale
10. Salvare il file.

La pagina presenta ora due aree distinte che è possibile controllare separatamente. Tuttavia, nessuna delle due nella zona. qualsiasi contenuto, pertanto la creazione di contenuto è il passaggio successivo Questa procedura dettagliata, si utilizzano i controlli Web part che visualizzano solo contenuto statico.

Il layout di una zona Web part è specificata da un  **&lt;zonetemplate&gt;**  elemento. All'interno del modello di area, è possibile aggiungere qualsiasi controllo ASP.NET, se si tratta di un controllo personalizzato di Web part, un controllo utente o un controllo server esistente. Si noti che si sta utilizzando il controllo etichetta, e che si sta aggiungendo semplicemente un testo statico. Quando si inserisce un controllo server regolare in un **WebPartZone** zona, ASP.NET considera il controllo come controllo Web part in fase di esecuzione che abilita le funzionalità Web part nel controllo.

**Per creare contenuto per l'area principale**

1. In **progettazione** visualizzare, trascinare un **etichetta** controllo il **Standard** della casella degli strumenti nell'area di contenuto dell'area di cui **ID** proprietà è impostato su MainZone.
2. Passare a **origine** visualizzazione. Si noti che un  **&lt;zonetemplate&gt;**  elemento è stato aggiunto per eseguire il wrapping di **etichetta** controllo in MainZone.
3. Aggiungere un attributo denominato **titolo** per il  **&lt;asp: label&gt;**  elemento e impostarne il valore al contenuto. Rimuovere il testo = attributo "Label" di  **&lt;asp: label&gt;**  elemento. Tra i tag di apertura e chiusura del  **&lt;asp: label&gt;**  elemento, aggiungere un testo, ad esempio **iniziale per la Home Page** all'interno di una coppia di  **&lt;h2 &gt;**  tag dell'elemento. Il codice dovrebbe apparire come segue. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample21.aspx)]
4. Salvare il file.

Successivamente, creare un controllo utente che è anche possibile aggiungere alla pagina un controllo Web part.

### <a name="to-create-a-user-control"></a>Per creare un controllo utente

1. Aggiungere un nuovo controllo utente Web al sito come un controllo di ricerca. Deselezionare l'opzione per **inserire codice sorgente in un file separato**. Aggiungere nella stessa directory WebPartsDemo e denominarlo SearchUserControl.   
  
    > [!NOTE]
    > Il controllo utente per questa procedura dettagliata non implementa la funzionalità di ricerca effettiva. utilizzato solo per illustrare le funzionalità Web part.
2. Passare a **progettazione** visualizzazione. Dal **Standard** scheda della casella degli strumenti, trascinare un controllo casella di testo nella pagina.
3. Posizionare il cursore dopo la casella di testo che appena aggiunto e premere INVIO per aggiungere una nuova riga.
4. Trascinare un controllo pulsante nella pagina in una nuova riga sotto la casella di testo che appena aggiunto.
5. Passare a **origine** visualizzazione. Verificare che il codice sorgente per il controllo utente è simile alla seguente. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample22.aspx)]
6. Salvare e chiudere il file.

È ora possibile aggiungere i controlli Web part a tale area. Si aggiungono due controlli all'area di intestazione laterale, uno contenente un elenco di collegamenti e l'altro che rappresenta il controllo utente è stato creato nella procedura precedente. I collegamenti vengono aggiunti come standard **etichetta** controllo server, simile al modo in cui è stato creato il testo statico per l'area principale. Tuttavia, anche se i singoli controlli server contenuti in controllo utente potrebbe essere contenuto direttamente nella zona (ad esempio, il controllo etichetta), in questo caso non lo sono. In alternativa, fanno parte del controllo utente che è stato creato nella procedura precedente. Viene descritto un modo comune per un pacchetto con controlli e funzionalità aggiuntive che si desidera un controllo utente e quindi fare riferimento a tale controllo in un'area come un controllo Web part.

In fase di esecuzione, il set di controlli Web part esegue il wrapping di entrambi i controlli con controlli GenericWebPart. Quando un **GenericWebPart** controllo esegue il wrapping di un controllo server Web e la parte generica è il controllo padre, è possibile accedere al controllo server tramite la proprietà ChildControl del controllo padre. Questo utilizzo di controlli Web part generica consente i controlli server Web standard per gli attributi e dello stesso comportamento di base come controlli Web part che derivano dal **WebPart** classe.

### <a name="to-add-web-parts-controls-to-the-sidebar-zone"></a>Per aggiungere i controlli Web part alla zona barra laterale

1. Aprire la pagina WebPartsDemo.
2. Passare a **progettazione** visualizzazione.
3. Trascinare la pagina controllo utente è stato creato, SearchUserControl, da **Esplora** nell'area di cui **ID** proprietà è impostata su SidebarZone e rilascio non esiste.
4. Salvare la pagina WebPartsDemo.
5. Passare a **origine** visualizzazione.
6. All'interno di  **&lt;asp: webpartzone&gt;**  elemento per SidebarZone, appena sopra il riferimento al controllo utente, aggiungere un  **&lt;asp: label&gt;**  elemento con i collegamenti contenuti, come illustrato nell'esempio seguente. Inoltre, aggiungere un **titolo** tag del controllo utente, con un valore di attributo **ricerca**, come illustrato. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample23.aspx)]
7. Salvare e chiudere il file.

Ora è possibile testare la pagina per ricercare nel browser. Nella pagina vengono visualizzati due zone. La schermata seguente mostra la pagina.

**Pagina dimostrativa Web part con due zone**


![Schermata procedura dettagliata 1 di Web part](profiles-themes-and-web-parts/_static/image3.gif)

**Figura 3**: Web part schermata della procedura dettagliata 1


Nel titolo della barra di ogni controllo è una freccia verso il basso che fornisce l'accesso a un menu dei verbi di azioni disponibili, che è possibile eseguire su un controllo. Fare clic sul menu dei verbi per uno dei controlli, quindi scegliere il **Riduci a icona** verbo e notare che il controllo è ridotta a icona. Dal menu, fare clic su **ripristinare**, e restituisce il controllo alle dimensioni normali.

### <a name="enabling-users-to-edit-pages-and-change-layout"></a>Consentire agli utenti di modificare le pagine e modificare il Layout

Web part consente agli utenti di modificare il layout dei controlli Web part trascinandoli da un'area a altra. Oltre a consentire agli utenti di spostare **WebPart** controlli da una zona in un altro, è possibile consentire agli utenti di modificare diverse caratteristiche dei controlli, inclusi i relativi aspetto, layout e il comportamento. Il set di controlli Web part forniscono funzionalità di modifica per **WebPart** controlli. Sebbene non verrà effettuato in questa procedura dettagliata, è anche possibile creare controlli editor personalizzati che consentono agli utenti di modificare le funzionalità di **WebPart** controlli. Come con la modifica del percorso di un **WebPart** controllo, la modifica delle proprietà di un controllo si basa sulla personalizzazione di ASP.NET per salvare le modifiche apportate dall'utente.

In questa parte della procedura dettagliata, aggiungere la possibilità agli utenti di modificare le caratteristiche di qualsiasi **WebPart** controllo nella pagina. Per abilitare queste funzionalità, aggiungere un altro controllo utente personalizzato per la pagina, insieme a un  **&lt;asp: editorzone&gt;**  elemento e due controlli di modifica.

### <a name="to-create-a-user-control-that-enables-changing-page-layout"></a>Per creare un controllo utente che consente la modifica di layout di pagina

1. In Visual Studio, sul **File** menu, seleziona il **nuovo** sottomenu e fare clic su di **File** opzione.
2. Nel **Aggiungi nuovo elemento** finestra di dialogo Seleziona **controllo utente Web**. Nome del nuovo file DisplayModeMenu. Deselezionare l'opzione per **inserire codice sorgente in file separato**.
3. Fare clic su Aggiungi per creare il nuovo controllo.
4. Passare a **origine** visualizzazione.
5. Rimuovere tutto il codice esistente nel nuovo file, quindi incollare nel codice seguente. Questo codice di controllo utente utilizza le funzionalità dell'insieme di controlli Web part che consentono di modificare la modalità di visualizzazione della pagina e consente inoltre di modificare l'aspetto fisico e layout della pagina quando si trovano in determinate modalità di visualizzazione. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample24.aspx)]
6. Salvare il file, fare clic su Salva icona sulla barra degli strumenti oppure selezionando **salvare** sul **File** menu.

### <a name="to-enable-users-to-change-the-layout"></a>Per consentire agli utenti di modificare il layout

1. Aprire la pagina WebPartsDemo e passare a **progettazione** visualizzazione.
2. Posizionare il cursore nel **progettazione** visualizzare subito dopo il **WebPartManager** controllo aggiunto in precedenza. Aggiungere un disco rigido restituito dopo il testo in modo che sia presente una riga vuota dopo il **WebPartManager** controllo. Posizionare il cursore sulla riga vuota.
3. Trascinare il controllo utente appena creato (il file è denominato DisplayModeMenu) nei WebPartsDemo pagina e rilasciarlo sulla riga vuota.
4. Trascinare un controllo EditorZone il **WebParts** sezione della casella degli strumenti per la cella di tabella aperta rimanenti nella pagina WebPartsDemo.
5. Dal **WebParts** sezione della casella degli strumenti, trascinare un controllo AppearanceEditorPart e un controllo LayoutEditorPart nel **EditorZone** controllo.
6. Passare a **origine** visualizzazione. Il codice risultante nella cella della tabella dovrebbe essere simile al codice seguente. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample25.aspx)]
7. Salvare il file WebPartsDemo. È stato creato un controllo utente che consente di modificare le modalità di visualizzazione e il layout di pagina e si è fatto riferimento il controllo della pagina Web primario.

È ora possibile testare la possibilità di modificare le pagine e il layout.

### <a name="to-test-layout-changes"></a>Per testare le modifiche di layout

1. Caricare la pagina in un browser.
2. Fare clic su di **modalità di visualizzazione** dal menu a discesa e selezionare **modifica**. Vengono visualizzati i titoli di zona.
3. Trascinare il **collegamenti personali** controllo barra del titolo da tale area nella parte inferiore dell'area principale. La pagina dovrebbe essere simile cattura di schermata seguente.

### <a name="web-parts-demo-page-with-my-links-control-moved"></a>Pagina dimostrativa Web part con controllo My Links spostato


![Schermata procedura dettagliata 2 di Web part](profiles-themes-and-web-parts/_static/image4.gif)

**Figura 4**: Web part schermata della procedura dettagliata 2


1. Fare clic su di **modalità di visualizzazione** dal menu a discesa e selezionare **Sfoglia**. La pagina viene aggiornata, i nomi delle aree scompaiono e **collegamenti personali** controllo rimane nella stessa posizione.
2. Per verificare che la personalizzazione viene eseguita, chiudere il browser e quindi caricare di nuovo la pagina. Le modifiche apportate vengono salvate per future sessioni del browser.
3. Dal **modalità di visualizzazione** dal menu **modifica**.   
  
 Ogni controllo nella pagina viene visualizzato con una freccia verso il basso nella barra del titolo, che contiene l'elenco a discesa dei verbi.
4. Fare clic sulla freccia per visualizzare il menu dei verbi di **collegamenti personali** controllo. Fare clic su di **modifica** verbo.   
  
 Il **EditorZone** verrà visualizzato un controllo, la visualizzazione di EditorPart i controlli aggiunti.
5. Nel **aspetto** sezione del controllo di modifica, modifica il **titolo** ai Preferiti, utilizzare il **tipo riquadro** elenco a discesa per selezionare **solo titolo**, quindi fare clic su **applica**. La schermata seguente mostra la pagina in modalità di modifica.

### <a name="web-parts-demo-page-in-edit-mode"></a>Pagina dimostrativa Web part in modalità di modifica


![Schermata procedura dettagliata 3 di Web part](profiles-themes-and-web-parts/_static/image5.gif)

**Figura 5**: Web part schermata della procedura dettagliata 3


1. Fare clic su di **modalità di visualizzazione** dal menu **Sfoglia** per tornare alla modalità browse.
2. Il controllo ha ora un titolo aggiornato e nessun bordo, come illustrato nella schermata seguente.

### <a name="edited-web-parts-demo-page"></a>Pagina Web part Demo modificata


![Schermata procedura dettagliata 4 parti VS per Web](profiles-themes-and-web-parts/_static/image6.gif)

**Figura 4**: Web part schermata della procedura dettagliata 4


### <a name="adding-web-parts-at-run-time"></a>Aggiunta di Web part in fase di esecuzione

È inoltre possibile consentire agli utenti di aggiungere i controlli Web part alla pagina in fase di esecuzione. A tale scopo, configurare la pagina con un catalogo Web part, che contiene un elenco di controlli Web part che si desidera rendere disponibili agli utenti.

**Per consentire agli utenti di aggiungere Web part in fase di esecuzione**

1. Aprire la pagina WebPartsDemo e passare a **progettazione** visualizzazione.
2. Dal **WebParts** scheda della casella degli strumenti trascinare un controllo CatalogZone nella colonna di destra della tabella, sotto il **EditorZone** controllo.   
  
 Entrambi i controlli possono essere nella stessa cella di tabella perché non verrà visualizzati allo stesso tempo.
3. Nel riquadro proprietà, assegnare la stringa **Aggiungi Web part** alla proprietà HeaderText del **CatalogZone** controllo.
4. Dal **WebParts** sezione della casella degli strumenti, trascinare un controllo DeclarativeCatalogPart nell'area di contenuto di **CatalogZone** controllo.
5. Fare clic sulla freccia nell'angolo superiore destro del **DeclarativeCatalogPart** per esporre il relativo menu attività di controllo e quindi selezionare **modifica modelli**.
6. Dal **Standard** sezione della casella degli strumenti, trascinare un **FileUpload** controllo e un **calendario** controllare nel **WebPartsTemplate** sezione di **DeclarativeCatalogPart** controllo.
7. Passare a **origine** visualizzazione. Esaminare il codice sorgente del  **&lt;asp: catalogzone&gt;**  elemento. Si noti che il **DeclarativeCatalogPart** controllo contiene un  **&lt;webpartstemplate&gt;**  elemento con i due controlli server racchiusi che sarà in grado di aggiungere alla pagina dal catalogo.
8. Aggiungere un **titolo** proprietà per ognuno dei controlli aggiunti al catalogo, utilizzando il valore stringa visualizzato per ogni titolo nell'esempio di codice riportato di seguito. Anche se il titolo non è una proprietà in genere è possibile impostare su questi due controlli server in fase di progettazione, quando un utente aggiunge questi controlli a un **WebPartZone** zona dal catalogo in fase di esecuzione, questi sono ognuno incluso con un  **GenericWebPart** controllo. Ciò consente loro di agire come controlli Web part, pertanto saranno in grado di visualizzare i titoli.   
  
 Il codice per i due controlli contenuti nel **DeclarativeCatalogPart** controllo dovrebbe apparire come segue. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample26.aspx)]
9. Salvare la pagina.

È ora possibile testare il catalogo.

### <a name="to-test-the-web-parts-catalog"></a>Per testare il catalogo Web part

1. Caricare la pagina in un browser.
2. Fare clic su di **modalità di visualizzazione** dal menu a discesa e selezionare **catalogo**.   
  
 Il catalogo denominato **Aggiungi Web part** viene visualizzato.
3. Trascinare il **Preferiti** da dell'area principale torna all'inizio dell'area di intestazione laterale e rilasciarlo non esiste.
4. Nel **Aggiungi Web part** del catalogo, selezionare entrambe le caselle di controllo, quindi **Main** nell'elenco a discesa che contiene le zone disponibili.
5. Fare clic su **Aggiungi** nel catalogo. I controlli vengono aggiunti all'area principale. Se si desidera, è possibile aggiungere più istanze di controlli dal catalogo alla pagina.   
  
 La schermata seguente mostra la pagina con il controllo caricamento file e il calendario nell'area principale. 

![Controlli aggiunti all'area Main dal catalogo](profiles-themes-and-web-parts/_static/image7.gif)

    **Figure 5**: Controls added to Main zone from the catalog
6. Fare clic su di **modalità di visualizzazione** dal menu a discesa e selezionare **Sfoglia**. Viene rimosso il catalogo e aggiornamento della pagina.
7. Chiudere il browser. Caricare di nuovo la pagina. Le modifiche apportate vengono mantenute.
