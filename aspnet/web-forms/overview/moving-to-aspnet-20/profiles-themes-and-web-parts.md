---
uid: web-forms/overview/moving-to-aspnet-20/profiles-themes-and-web-parts
title: I profili, temi e Web part | Microsoft Docs
author: microsoft
description: Esistono importanti modifiche alla configurazione e la strumentazione in ASP.NET 2.0. La nuova API di configurazione di ASP.NET consente di apportare modifiche di configurazione da apportare delle richieste pull...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 92df4051-77c6-492c-bd34-23d24189cea4
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/profiles-themes-and-web-parts
msc.type: authoredcontent
ms.openlocfilehash: 98559c2a378c72bc5664faafe5436753050b574f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833413"
---
<a name="profiles-themes-and-web-parts"></a>I profili, temi e Web part
====================
da [Microsoft](https://github.com/microsoft)

> Esistono importanti modifiche alla configurazione e la strumentazione in ASP.NET 2.0. La nuova API di configurazione di ASP.NET consente modifiche di configurazione da apportare a livello di codice. Inoltre, esistono molte nuove impostazioni di configurazione consentono le nuove configurazioni e la strumentazione.


ASP.NET 2.0 rappresenta un miglioramento significativo nell'area dei siti Web personalizzati. Oltre le funzionalità appartenenza weve già illustrati, profili ASP.NET, temi e Web part in modo significativo la capacità di personalizzazione in siti Web.

## <a name="aspnet-profiles"></a>Profili ASP.NET

I profili ASP.NET sono simili alle sessioni. La differenza è che un profilo è persistente, mentre una sessione viene persa quando il browser viene chiuso. Un'altra grande differenza tra le sessioni e i profili è che i profili sono fortemente tipizzati, pertanto offrendo IntelliSense durante il processo di sviluppo.

Un profilo è definito nel file di configurazione del computer o il file Web. config per l'applicazione. (È possibile definire un profilo in un file Web. config nelle sottocartelle). Il codice seguente definisce un profilo per archiviare i visitatori del sito Web prima di tutto nome e cognome.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample1.xml)]

Il tipo di dati predefinito per una proprietà del profilo è System. String. Nell'esempio precedente, è stato specificato alcun tipo di dati. Di conseguenza le proprietà FirstName e LastName sono entrambi di tipo stringa. Come accennato in precedenza, profilo di proprietà fortemente tipizzate. Il codice seguente aggiunge una nuova proprietà per durata che è di tipo Int32.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample2.xml)]

I profili vengono in genere usati con autenticazione basata su form ASP.NET. Quando usato in combinazione con autenticazione basata su form, ogni utente dispone di un profilo separato associato al proprio ID utente. Tuttavia, è anche possibile consentire l'uso di profili in un'applicazione anonimo usando il &lt;anonymousIdentification&gt; elemento nel file di configurazione con la **allowAnonymous** dell'attributo come la seguente:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample3.xml)]

Quando un utente anonimo visita il sito, ASP.NET crea un'istanza di **ProfileCommon** per l'utente. Questo profilo Usa un ID univoco archiviato in un cookie nel browser per identificare l'utente come un visitatore univoco. In questo modo, è possibile archiviare informazioni sul profilo per gli utenti che cercano in modo anonimo.

## <a name="profile-groups"></a>Gruppi di profili

È possibile raggruppare le proprietà dei profili. Dalle proprietà di raggruppamento, è possibile simulare più profili per un'applicazione specifica.

La configurazione seguente consente di configurare una proprietà FirstName e LastName per due gruppi. Gli acquirenti e prospettive.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample4.xml)]

È quindi possibile impostare le proprietà in un determinato gruppo, come indicato di seguito:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample5.cs)]

## <a name="storing-complex-objects"></a>L'archiviazione di oggetti complesso

Gli esempi che appena trattati finora, sono archiviati tipi di dati semplici in un profilo. È anche possibile archiviare i tipi di dati complessi in un profilo, specificando il metodo di serializzazione utilizzando il **serializeAs** attributo come indicato di seguito:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample6.xml)]

In questo caso, il tipo è PurchaseInvoice. La classe PurchaseInvoice deve essere contrassegnato come serializzabile e può contenere un numero qualsiasi di proprietà. Ad esempio, se PurchaseInvoice ha una proprietà denominata **NumItemsPurchased**, è possibile fare riferimento a tale proprietà nel codice come indicato di seguito:

[!code-css[Main](profiles-themes-and-web-parts/samples/sample7.css)]

## <a name="profile-inheritance"></a>Ereditarietà di profilo

È possibile creare un profilo per l'uso in più applicazioni. Tramite la creazione di una classe di profilo che deriva da ProfileBase, è possibile riutilizzare un profilo in diverse applicazioni usando il **eredita** attributo come illustrato di seguito:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample8.xml)]

In questo caso, la classe **PurchasingProfile** apparirebbe come segue:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample9.cs)]

## <a name="profile-providers"></a>Provider di profili

I profili ASP.NET usano il modello di provider. Il provider predefinito archivia le informazioni in un database di SQL Server Express nell'App\_cartella dati dell'applicazione Web utilizzando il provider SqlProfileProvider. Se il database non esiste, ASP.NET verrà creato automaticamente quando il profilo tenta di archiviare informazioni.

In alcuni casi, tuttavia, è possibile sviluppare un provider di profili. La funzionalità di profilo ASP.NET ti permette di usare facilmente diversi provider.

Si crea un provider di profili personalizzato quando:

- È necessario archiviare le informazioni sul profilo in un'origine dati, ad esempio in un database di FoxPro o in un database Oracle, che non è supportato dal provider di profili disponibili con .NET Framework.
- È necessario gestire le informazioni di profilo utilizzando uno schema di database che è diverso dallo schema del database utilizzato dai provider incluso in .NET Framework. Un esempio comune è che si desidera integrare le informazioni sul profilo con i dati utente in un database di SQL Server esistente.

### <a name="required-classes"></a>Classi necessarie

Per implementare un provider di profili, si crea una classe che eredita la classe astratta System.Web.Profile.ProfileProvider. Il **ProfileProvider** classe astratta eredita a sua volta la classe astratta System.Configuration.SettingsProvider, che eredita la classe astratta System.Configuration.Provider.ProviderBase. A causa di questa catena di ereditarietà, oltre ai membri necessari del **ProfileProvider** (classe), è necessario implementare i membri necessari delle **SettingsProvider** e  **ProviderBase** classi.

Le tabelle seguenti descrivono le proprietà e metodi che è necessario implementare dal **ProviderBase**, **SettingsProvider**, e **ProfileProvider** astratta classi.

### <a name="providerbase-members"></a>ProviderBase membri

| **Membro** | **Descrizione** |
| --- | --- |
| Initialize (metodo) | Accetta come input il nome dell'istanza del provider e NameValueCollection delle impostazioni di configurazione. Utilizzato per impostare le opzioni e i valori di proprietà per l'istanza del provider, inclusi i valori specifici dell'implementazione e le opzioni specificate nel file Web. config o configurazione della macchina. |

### <a name="settingsprovider-members"></a>Membri SettingsProvider

| **Membro** | **Descrizione** |
| --- | --- |
| Proprietà ApplicationName | Il nome dell'applicazione che viene archiviato con ogni profilo. Il provider del profilo Usa il nome dell'applicazione per archiviare informazioni sul profilo separatamente per ogni applicazione. Ciò consente a più applicazioni ASP.NET usare la stessa origine dati senza conflitti, se lo stesso nome utente viene creato in applicazioni diverse. In alternativa, più applicazioni ASP.NET possono condividere un'origine dati di profilo specificando il nome dell'applicazione stessa. |
| Metodo GetPropertyValues | Accetta come input un SettingsContext e un oggetto SettingsPropertyCollection. Il **SettingsContext** vengono fornite informazioni sull'utente. È possibile usare le informazioni come una chiave primaria per recuperare informazioni sulle proprietà di profilo per l'utente. Usare la **SettingsContext** oggetto per cui ottenere il nome utente e se l'utente è autenticato o anonimo. Il **SettingsPropertyCollection** contiene una raccolta di oggetti SettingsProperty. Ciascuna **SettingsProperty** oggetto fornisce il nome e il tipo di proprietà, nonché informazioni aggiuntive, ad esempio il valore predefinito per la proprietà e indica se la proprietà è di sola lettura. Il **GetPropertyValues** metodo popola un SettingsPropertyValueCollection con oggetti SettingsPropertyValue sulla base di **SettingsProperty** gli oggetti forniti come input. I valori dall'origine dati per l'utente specificato vengono assegnati alle proprietà PropertyValue per ognuno **SettingsPropertyValue** viene restituito l'oggetto e l'intera raccolta. Chiamare il metodo aggiorna anche il valore LastActivityDate per il profilo utente specificato per la data e ora correnti. |
| Metodo SetPropertyValues | Accetta come input un **SettingsContext** e una **SettingsPropertyValueCollection** oggetto. Il **SettingsContext** vengono fornite informazioni sull'utente. È possibile usare le informazioni come una chiave primaria per recuperare informazioni sulle proprietà di profilo per l'utente. Usare la **SettingsContext** oggetto per cui ottenere il nome utente e se l'utente è autenticato o anonimo. Il **SettingsPropertyValueCollection** contiene una raccolta di **SettingsPropertyValue** oggetti. Ciascuna **SettingsPropertyValue** oggetto fornisce il nome, tipo e valore della proprietà, nonché informazioni aggiuntive, ad esempio il valore predefinito per la proprietà e indica se la proprietà è di sola lettura. Il **SetPropertyValues** metodo aggiorna i valori delle proprietà del profilo nell'origine dati per l'utente specificato. La chiamata al metodo anche gli aggiornamenti di **LastActivityDate** e LastUpdatedDate valori per il profilo utente specificato per la data e ora correnti. |

### <a name="profileprovider-members"></a>Membri ProfileProvider

| **Membro** | **Descrizione** |
| --- | --- |
| Metodo DeleteProfiles | Accetta come input una matrice di stringhe dell'utente nomi ed Elimina dall'origine dati per i nomi di tutti i valori informazioni e proprietà del profilo in cui il nome dell'applicazione corrisponde la **ApplicationName** valore della proprietà. Se l'origine dati supporta le transazioni, si consiglia di includere tutte le operazioni delete in una transazione e che il rollback della transazione e genera un'eccezione se qualsiasi operazione di eliminazione ha esito negativo. |
| Metodo DeleteProfiles | Accetta come input una raccolta di ProfileInfo oggetti ed Elimina dall'origine dati per ogni profilo di tutti i valori proprietà e le informazioni del profilo in cui il nome dell'applicazione corrisponde la **ApplicationName** valore della proprietà. Se l'origine dati supporta le transazioni, è consigliabile includere tutte le operazioni delete in una transazione e il rollback della transazione e genera un'eccezione se qualsiasi operazione di eliminazione ha esito negativo. |
| Metodo DeleteInactiveProfiles | Accetta come input un valore ProfileAuthenticationOption e un oggetto DateTime e le eliminazioni dei dati di tutte le informazioni di origine e i valori delle proprietà in cui la data dell'ultima attività è minore o uguale alla data specificata e all'ora e il nome dell'applicazione corrisponde alla **ApplicationName** valore della proprietà. Il **ProfileAuthenticationOption** parametro specifica se solo nei profili anonimi, solo nei profili autenticati, o tutti i profili devono essere eliminate. Se l'origine dati supporta le transazioni, è consigliabile includere tutte le operazioni delete in una transazione e il rollback della transazione e genera un'eccezione se qualsiasi operazione di eliminazione ha esito negativo. |
| Metodo GetAllProfiles | Accetta come input un **ProfileAuthenticationOption** valore, un numero intero che specifica l'indice della pagina, un numero intero che specifica le dimensioni della pagina e un riferimento a un integer che verrà impostato per il conteggio totale dei profili. Restituisce un ProfileInfoCollection contenente **ProfileInfo** gli oggetti per tutti i profili nell'origine dei dati in cui il nome dell'applicazione corrisponde la **ApplicationName** valore della proprietà. Il **ProfileAuthenticationOption** parametro specifica se solo nei profili anonimi, solo nei profili autenticati, o tutti i profili devono essere restituiti. I risultati restituiti dai **GetAllProfiles** metodo sono vincolati dai valori di dimensioni di pagina e indice della pagina. Il valore delle dimensioni di pagina consente di specificare il numero massimo di **ProfileInfo** oggetti da restituire nella **ProfileInfoCollection**. Il valore di indice di pagina specifica la pagina di risultati da restituire, dove 1 indica la prima pagina. Il parametro per totale di record è un parametro out (è possibile usare **ByRef** in Visual Basic) che è impostato per il numero totale di profili. Ad esempio, se l'archivio dati contiene 13 profili per l'applicazione e il valore di indice di pagina è 2 con una dimensione di pagina pari a 5, il **ProfileInfoCollection** restituiti dal sesto al decimo profilo contiene. Il valore totale di record viene impostato su 13 quando restituito dal metodo. |
| Metodo GetAllInactiveProfiles | Accetta come input un **ProfileAuthenticationOption** valore, un **DateTime** oggetto, un numero intero che specifica l'indice della pagina, un numero intero che specifica le dimensioni della pagina e un riferimento a un integer che verrà impostato per il conteggio totale dei profili. Restituisce un **ProfileInfoCollection** che contiene **ProfileInfo** oggetti per tutti i profili presenti nell'origine dati in cui la data dell'ultima attività è minore o uguale all'oggetto specificato **DateTime**  e in cui il nome dell'applicazione corrisponda a quello di **ApplicationName** valore della proprietà. Il **ProfileAuthenticationOption** parametro specifica se solo nei profili anonimi, solo nei profili autenticati, o tutti i profili devono essere restituiti. I risultati restituiti dai **GetAllInactiveProfiles** metodo sono vincolati dai valori di dimensioni di pagina e indice della pagina. Il valore delle dimensioni di pagina consente di specificare il numero massimo di **ProfileInfo** oggetti da restituire nella **ProfileInfoCollection**. Il valore di indice di pagina specifica la pagina di risultati da restituire, dove 1 indica la prima pagina. Il parametro per totale di record è un parametro out (è possibile usare **ByRef** in Visual Basic) che è impostato per il numero totale di profili. Ad esempio, se l'archivio dati contiene 13 profili per l'applicazione e il valore di indice di pagina è 2 con una dimensione di pagina pari a 5, il **ProfileInfoCollection** restituiti dal sesto al decimo profilo contiene. Il valore totale di record viene impostato su 13 quando restituito dal metodo. |
| Metodo FindProfilesByUserName | Accetta come input un **ProfileAuthenticationOption** valore, una stringa contenente un nome utente, un numero intero che specifica l'indice della pagina, un numero intero che specifica le dimensioni della pagina e un riferimento a un integer che verrà impostato per il numero totale di profili. Restituisce un **ProfileInfoCollection** che contiene **ProfileInfo** oggetti per tutti i profili nei dati di origine in cui il nome utente corrisponde al nome utente specificato e il nome dell'applicazione corrisponde il **ApplicationName** valore della proprietà. Il **ProfileAuthenticationOption** parametro specifica se solo nei profili anonimi, solo nei profili autenticati, o tutti i profili devono essere restituiti. Se l'origine dati supporta la funzionalità di ricerca aggiuntive, ad esempio i caratteri jolly, è possibile fornire le funzionalità di ricerca più estese per i nomi utente. I risultati restituiti dai **FindProfilesByUserName** metodo sono vincolati dai valori di dimensioni di pagina e indice della pagina. Il valore delle dimensioni di pagina consente di specificare il numero massimo di **ProfileInfo** oggetti da restituire nella **ProfileInfoCollection**. Il valore di indice di pagina specifica la pagina di risultati da restituire, dove 1 indica la prima pagina. Il parametro per totale di record è un parametro out (è possibile usare **ByRef** in Visual Basic) che è impostato per il numero totale di profili. Ad esempio, se l'archivio dati contiene 13 profili per l'applicazione e il valore di indice di pagina è 2 con una dimensione di pagina pari a 5, il **ProfileInfoCollection** restituiti dal sesto al decimo profilo contiene. Il valore totale di record viene impostato su 13 quando restituito dal metodo. |
| Metodo FindInactiveProfilesByUserName | Accetta come input un **ProfileAuthenticationOption** value, una stringa contenente il nome utente, un **DateTime** oggetto, un numero intero che specifica l'indice della pagina, un numero intero che specifica le dimensioni della pagina e un oggetto Fare riferimento a un integer che verrà impostato per il conteggio totale dei profili. Restituisce un **ProfileInfoCollection** che contiene **ProfileInfo** oggetti per tutti i profili nell'origine dati in cui il nome utente corrisponde al nome utente specificato, in cui la data dell'ultima attività è minore di o uguale all'oggetto specificato **data/ora**, e in cui il nome dell'applicazione corrisponda a quello di **ApplicationName** valore della proprietà. Il **ProfileAuthenticationOption** parametro specifica se solo nei profili anonimi, solo nei profili autenticati, o tutti i profili devono essere restituiti. Se l'origine dati supporta la funzionalità di ricerca aggiuntive, ad esempio i caratteri jolly, è possibile fornire le funzionalità di ricerca più estese per i nomi utente. I risultati restituiti dai **FindInactiveProfilesByUserName** metodo sono vincolati dai valori di dimensioni di pagina e indice della pagina. Il valore delle dimensioni di pagina consente di specificare il numero massimo di **ProfileInfo** oggetti da restituire nella **ProfileInfoCollection**. Il valore di indice di pagina specifica la pagina di risultati da restituire, dove 1 indica la prima pagina. Il parametro per totale di record è un parametro out (è possibile usare **ByRef** in Visual Basic) che è impostato per il numero totale di profili. Ad esempio, se l'archivio dati contiene 13 profili per l'applicazione e il valore di indice di pagina è 2 con una dimensione di pagina pari a 5, il **ProfileInfoCollection** restituiti dal sesto al decimo profilo contiene. Il valore totale di record viene impostato su 13 quando restituito dal metodo. |
| Metodo GetNumberOfInActiveProfiles | Accetta come input un **ProfileAuthenticationOption** valore e una **DateTime** specificato e restituisce un conteggio di tutti i profili nell'origine dati in cui la data dell'ultima attività è minore o uguale al specificato **Data/ora** e in cui il nome dell'applicazione corrisponda a quello di **ApplicationName** valore della proprietà. Il **ProfileAuthenticationOption** parametro specifica se solo nei profili anonimi, solo nei profili autenticati, o tutti i profili devono essere contati. |

### <a name="applicationname"></a>ApplicationName

Poiché i provider di profili consentono di archiviare informazioni sul profilo separatamente per ogni applicazione, è necessario assicurarsi che lo schema dei dati include il nome dell'applicazione e che le query e gli aggiornamenti includono anche il nome dell'applicazione. Ad esempio, il comando seguente consente di recuperare un valore della proprietà da un database in base al nome utente e se il profilo è anonimo e assicura che il **ApplicationName** valore è incluso nella query.

[!code-sql[Main](profiles-themes-and-web-parts/samples/sample10.sql)]

## <a name="aspnet-themes"></a>Temi ASP.NET

## <a name="what-are-aspnet-20-themes"></a>Quali sono i temi di ASP.NET 2.0?

Uno degli aspetti più importanti di un'applicazione Web è un aspetto uniforme e coerente tra il sito. Gli sviluppatori ASP.NET 1.x usano in genere Cascading Style Sheets (CSS) per implementare un aspetto uniforme e coerente. ASP.NET 2.0 i temi in modo significativo migliorano CSS poiché offrono lo sviluppatore ASP.NET la possibilità di definire l'aspetto dei controlli server ASP.NET, nonché gli elementi HTML. I temi ASP.NET è applicabile a singoli controlli, una pagina Web o un'intera applicazione Web. I temi di usano una combinazione di file CSS, un file di interfaccia facoltativa e una directory Images facoltativa se sono necessarie immagini. Il file di interfaccia controlla l'aspetto visivo dei controlli server ASP.NET.

## <a name="where-are-themes-stored"></a>Dove sono archiviati i temi?

Il percorso in cui sono archiviati i temi varia in base al relativo ambito. Temi che possono essere applicati a tutte le applicazioni vengono archiviati nella cartella seguente:

`C:\WINDOWS\Microsoft.NET\Framework\v2.x.xxxxx\ASP.NETClientFiles\Themes\<Theme_Name>`

Un tema specifico per una determinata applicazione verrà archiviato in un `App\_Themes\<Theme\_Name>` directory nella radice del sito Web.

> [!NOTE]
> Un file di interfaccia deve solo modificare proprietà dei controlli server che influiscono sull'aspetto.

Un tema globale è un tema che può essere applicato a qualsiasi applicazione o sito Web in esecuzione sul server Web. Per impostazione predefinita nella directory ASP.NETClientfiles\Themes all'interno della directory v2.x.xxxxx sono archiviati questi temi. In alternativa, è possibile spostare i file di tema in aspnet\_client/system\_web / /Themes/ [versione] [tema\_name] cartella nella radice del sito Web.

I temi di specifiche dell'applicazione è applicabile solo all'applicazione in cui si trovano i file. Questi file vengono archiviati nel `App\_Themes/<theme\_name>` directory nella radice del sito Web.

## <a name="the-components-of-a-theme"></a>I componenti di un tema

Un tema è costituito da uno o più file CSS, un file di interfaccia facoltativa e una cartella immagini facoltativa. I file CSS possono essere qualsiasi nome desidera (ad esempio default. CSS o theme. CSS e così via) e deve trovarsi nella radice della cartella dei temi. I file CSS vengono utilizzati per definire le classi CSS normali e gli attributi per i selettori specifici. Per applicare una delle classi CSS a un elemento di pagina, il **CSSClass** proprietà viene utilizzata.

Il file di interfaccia è un file XML che contiene le definizioni di proprietà per i controlli server ASP.NET. Il codice riportato di seguito è riportato un esempio di file dell'interfaccia personalizzata.

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample11.aspx)]

**Figura 1** seguente mostra una pagina ASP.NET small esplorato senza un tema applicato. **Figura 2** Mostra lo stesso file con un tema applicato. Il colore di sfondo e testo vengono configurate mediante un file CSS. L'aspetto del pulsante e casella di testo vengono configurati tramite il file di interfaccia elencato in precedenza.


![Nessuna tema](profiles-themes-and-web-parts/_static/image1.gif)

**Figura 1**: alcun tema


![Tema applicato](profiles-themes-and-web-parts/_static/image2.gif)

**Figura 2**: tema applicato


Il file di interfaccia elencato in precedenza definisce un'interfaccia predefinita per tutti i controlli TextBox e i controlli pulsante. Ciò significa che ogni controllo TextBox e Button inserito in una pagina avrà questo aspetto. È anche possibile definire un'interfaccia che può essere applicata a istanze specifiche di questi controlli usando il **SkinID** proprietà del controllo.

Il codice seguente definisce un'interfaccia per un controllo pulsante. Solo pulsante Controlla con un **SkinID** proprietà di **goButton** avrà l'aspetto dell'interfaccia personalizzata.

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample12.aspx)]

È possibile avere solo un'interfaccia predefinita per ogni tipo di controllo server. Se sono necessarie le interfacce aggiuntive, è consigliabile utilizzare la proprietà SkinID.

## <a name="applying-themes-to-pages"></a>Applicazione di temi per le pagine

Un tema può essere applicato usando uno dei metodi seguenti:

- Nel &lt;pagine&gt; elemento del file Web. config
- Nel @Page direttiva della pagina
- A livello di codice

## <a name="applying-a-theme-in-the-configuration-file"></a>Il tema predefinito nel File di configurazione

Per applicare un tema nel file di configurazione dell'applicazione, usare la sintassi seguente:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample13.xml)]

Il nome del tema specificato qui deve corrispondere al nome della cartella dei temi. Questa cartella può esistere in uno dei percorsi indicato in precedenza in questo corso. Se si prova ad applicare un tema che non esiste, si verificherà un errore di configurazione.

## <a name="applying-a-theme-in-the-page-directive"></a>Se si applica un tema nella direttiva di pagina

È anche possibile applicare un tema nella direttiva @ Page. Questo metodo consente di utilizzare un tema per una pagina specifica.

Per applicare un tema nel @Page direttiva, usare la sintassi seguente:

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample14.aspx)]

Ancora una volta, il tema specificato qui deve corrispondere la cartella dei temi come indicato in precedenza. Se si prova ad applicare un tema che non esiste, si verificherà un errore di compilazione. Visual Studio anche evidenziare l'attributo e ricevere una notifica che tale tema non esista.

## <a name="applying-a-theme-programmatically"></a>Se si applica un tema a livello di codice

Per applicare un tema a livello di codice, è necessario specificare il **tema** proprietà per la pagina nel **pagina\_PreInit** (metodo).

Per applicare un tema a livello di codice, usare la sintassi seguente:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample15.cs)]

È necessario applicare il tema nel metodo PreInit a causa della sua durata. Se lo si applica questo punto, il tema di pagine verrà siano già stato applicato dal runtime e una modifica a questo punto è troppo tardi nel ciclo di vita. Se si applica un tema che non esiste, un' **HttpException** si verifica. Quando un tema viene applicato a livello di codice, verrà generato un avviso di compilazione se tutti i controlli server dispongono di una proprietà SkinID specificato. Questo avviso è destinato per informare l'utente che non tema viene applicato in modo dichiarativo e può essere ignorato.

## <a name="exercise-1--applying-a-theme"></a>Esercizio 1: Applicazione di un tema

In questo esercizio, verrà applicato un tema ASP.NET a un sito Web.

> [!IMPORTANT]
> Se si usa Microsoft Word per immettere le informazioni in un file di interfaccia, assicurarsi che si siano sostituendo non regolari tra virgolette con virgolette inglesi. Virgolette inglesi si verificheranno problemi con i file di interfaccia.

1. Creare un nuovo sito Web ASP.NET.
2. Pulsante destro del mouse sul progetto in Esplora soluzioni e scegliere Aggiungi nuovo elemento.
3. Scegliere i File di configurazione Web dall'elenco di file e fare clic su Aggiungi.
4. Pulsante destro del mouse sul progetto in Esplora soluzioni e scegliere Aggiungi nuovo elemento.
5. Soubor Skinu scegliere e fare clic su Aggiungi.
6. Fare clic su Sì, quando viene richiesto se avessero come inserire il file all'interno dell'App\_cartella dei temi.
7. Pulsante destro del mouse sulla cartella SkinFile all'interno dell'App\_cartella dei temi in Esplora soluzioni e scegliere Aggiungi nuovo elemento.
8. Scegliere il foglio di stile dall'elenco di file e fare clic su Aggiungi. Sono ora tutti i file necessari per implementare il nuovo tema. Tuttavia, Visual Studio è denominata la cartella dei temi SkinFile. Fare clic su tale cartella e modificare il nome in CoolTheme.
9. Aprire il file SkinFile.skin e aggiungere il codice seguente la fine del file: 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample16.aspx)]
10. Salvare il file SkinFile.skin.
11. Aprire lo StyleSheet. CSS.
12. Sostituire tutto il testo in essa con il codice seguente: 

    [!code-css[Main](profiles-themes-and-web-parts/samples/sample17.css)]
13. Salvare il file StyleSheet. CSS.
14. Aprire la pagina default. aspx.
15. Aggiungere un controllo TextBox e Button.
16. Salvare la pagina. Ora esplorare la pagina default. aspx. Dovrà essere indicato come normali Web form.
17. Aprire il file Web. config.
18. Aggiungere il codice seguente direttamente sotto l'apertura `<system.web>` tag: 

    [!code-xml[Main](profiles-themes-and-web-parts/samples/sample18.xml)]
19. Salvare il file Web. config. Ora esplorare la pagina default. aspx. Viene visualizzato con il tema applicato.
20. Se non è già aperto, aprire la pagina default. aspx in Visual Studio.
21. Selezionare il pulsante.
22. Modifica il **SkinID** proprietà goButton. Si noti che Visual Studio offre un elenco a discesa con i valori SkinID validi per un controllo pulsante.
23. Salvare la pagina. A questo punto visualizzare in anteprima la pagina nel browser anche in questo caso. Il pulsante sarà: "Vai" e deve essere più larghi nell'aspetto.

Usando il **SkinID** proprietà, è possibile configurare con facilità interfacce diverse per istanze diverse di un particolare tipo di controllo del server.

## <a name="the-stylesheettheme-property"></a>La proprietà StyleSheetTheme

Finora, abbiamo solo sull'applicazione di temi usando la proprietà del tema. Quando si usa la proprietà del tema, il file di interfaccia sostituiranno le impostazioni per i controlli server dichiarative. Nell'esercizio 1, ad esempio, è stato specificato un SkinID di "goButton" per il controllo Button e che è modificato il testo del pulsante "Vai". Si è notato che la proprietà di testo del pulsante nella finestra di progettazione è stata impostata su "Pulsante", ma il tema che ha eseguito l'override. Il tema verrà sempre eseguire l'override di eventuali impostazioni delle proprietà nella finestra di progettazione.

Se si desidera essere in grado di eseguire l'override di proprietà definite nel file di interfaccia del tema con le proprietà specificate nella finestra di progettazione, è possibile usare la **StyleSheetTheme** proprietà anziché la proprietà del tema. La proprietà StyleSheetTheme è quello utilizzato per la proprietà Theme ad eccezione del fatto che non esegue l'override di tutte le impostazioni di proprietà espliciti come la proprietà del tema.

Per visualizzare questa azione, aprire il file Web. config nel progetto in esercizio 1 e modificare il `<pages>` elemento al seguente:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample19.xml)]

A questo punto esaminare la pagina default. aspx e si noterà che il controllo pulsante ha una proprietà di testo di "Pulsante" nuovo. Ciò avviene perché l'impostazione della proprietà esplicite nella finestra di progettazione sta eseguendo l'override da goButton SkinID impostando la proprietà Text.

## <a name="overriding-themes"></a>Si esegue l'override dei temi

Un tema globale può essere sostituito da applicare un tema con lo stesso nome nell'App\_cartella temi dell'applicazione. Tuttavia, il tema non viene applicato in uno scenario di sostituzione true. Se il runtime incontra i file di tema nell'App\_cartella temi, verrà applicato il tema con i file e ignorerà il tema globale.

La proprietà StyleSheetTheme è sottoponibile a override e può essere sottoposto a override nel codice come indicato di seguito:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample20.cs)]

## <a name="web-parts"></a>Web part

Web part ASP.NET è un set integrato di controlli per la creazione di siti Web che consentono agli utenti finali di modificare il contenuto, l'aspetto e comportamento delle pagine Web direttamente da un browser. Le modifiche possono essere applicate a tutti gli utenti nel sito o a singoli utenti. Quando gli utenti modificano le pagine e controlli, le impostazioni possono essere salvate per mantenere le preferenze personali dell'utente tra le sessioni future del browser, una funzionalità denominata sulla personalizzazione. Queste funzionalità di Web part significa che gli sviluppatori che consentono agli utenti finali di personalizzare un'applicazione Web in modo dinamico, senza l'intervento di sviluppatore o amministratore.

Usa il set di controlli Web part, gli sviluppatori possono abilitare agli utenti finali di:

- Personalizzare il contenuto della pagina. Gli utenti possono aggiungere nuovi controlli Web part a una pagina, rimuoverli, nasconderle o ridurle a icona, ad esempio windows ordinari.
- Personalizzare il layout di pagina. Gli utenti possono trascinare un controllo Web part in un'area diversa in una pagina o modificarne l'aspetto, proprietà e comportamento.
- Esportare e importare i controlli. Gli utenti possono importare o esportare le Web part controllo le impostazioni per l'uso in altre pagine o siti, mantenendo le proprietà, l'aspetto e anche i dati nei controlli. Ciò riduce le richieste di immissione e la configurazione dei dati agli utenti finali.
- Creazione di connessioni. Gli utenti possono stabilire connessioni tra i controlli in modo che, ad esempio, un controllo chart potrebbe visualizzare un grafico per i dati in un controllo di quotazioni di borsa. Gli utenti è stato possibile personalizzare non solo la connessione, ma l'aspetto e i dettagli della modalità di visualizzazione dei dati nel controllo chart.
- Gestire e personalizzare le impostazioni a livello di sito. Gli utenti autorizzati possono configurare le impostazioni a livello di sito, determinare chi può accedere a un sito o una pagina, impostare l'accesso basato sui ruoli ai controlli e così via. Ad esempio, un utente in un ruolo amministrativo è stato possibile impostare un controllo Web part deve essere condiviso da tutti gli utenti e impedire agli utenti non amministratori la personalizzazione del controllo condiviso.

In genere funzionerà con le Web part in uno dei tre modi: creazione di pagine che utilizzano i controlli Web part, la creazione di singoli controlli Web part o creazione di applicazioni Web complete e personalizzabili, ad esempio, un portale.

## <a name="page-development"></a>Sviluppo di pagine

Gli sviluppatori di pagina possono usare gli strumenti di progettazione visiva, ad esempio Microsoft Visual Studio 2005 per creare pagine che utilizzano le Web part. Uno dei vantaggi usando uno strumento, ad esempio Visual Studio è che un set di controlli Web part fornisce funzionalità per la creazione di trascinamento e rilascio e configurazione dei controlli Web part in una finestra di progettazione. Ad esempio, è possibile usare la finestra di progettazione trascinare una zona Web part, o un controllo editor di Web part, nell'area di progettazione e quindi configurare il controllo a destra nella finestra di progettazione tramite l'interfaccia utente fornita dalle Web part di set di controlli. Ciò può accelerare lo sviluppo di applicazioni Web part e ridurre la quantità di codice da scrivere.

## <a name="control-development"></a>Sviluppo di controlli

È possibile usare qualsiasi controllo ASP.NET esistente come un controllo Web part, tra cui controlli server Web standard, i controlli server personalizzati e controlli utente. Per ottimizzare il controllo a livello di codice del proprio ambiente, è anche possibile creare controlli Web part personalizzati che derivano dalla classe WebPart. Per lo sviluppo di controlli Web part singoli, si sarà in genere creare un controllo utente e usarlo come un controllo Web part o sviluppare un controllo Web part personalizzato.

Ad esempio di sviluppo di un controllo Web part personalizzato, è possibile creare un controllo per fornire le funzionalità fornite da altri controlli server ASP.NET che potrebbero essere utili per pacchetto come un controllo Web part personalizzabile: calendari, elenchi, le informazioni finanziarie, notizie, calcolatori, i controlli testo RTF per l'aggiornamento di griglie modificabili, contenute che si connettono ai database, i grafici che lo schermo, aggiornare o meteo e informazioni di viaggio in modo dinamico. Se si fornisce una finestra di progettazione visiva con il controllo, quindi qualsiasi sviluppatore di pagine tramite Visual Studio può semplicemente trascinare il controllo in una zona Web part e configurarlo in fase di progettazione senza dover scrivere codice aggiuntivo.

La personalizzazione è alla base della funzionalità di Web part. Consente agli utenti di modificare, o personalizzare: il layout, l'aspetto e comportamento dei controlli Web part in una pagina. Le impostazioni personalizzate sono di lunga durate: vengono resi persistenti non solo durante la sessione del browser corrente (come lo stato di visualizzazione), ma anche nell'archiviazione a lungo termine, in modo che le impostazioni dell'utente vengono salvate anche le sessioni future del browser. Personalizzazione è abilitata per impostazione predefinita per le pagine Web part.

I componenti strutturali dell'interfaccia utente si basano sulla personalizzazione e forniscono la struttura di base e i servizi necessari per tutti i controlli Web part. Un componente dell'interfaccia utente strutturale necessario in ogni pagina Web part è il controllo WebPartManager. Anche se è mai visibile, questo controllo ha il compito fondamentale di coordinamento tutti i controlli Web part in una pagina. Ad esempio, tiene traccia di tutti i singoli controlli Web part. Gestisce le aree Web part (aree che contengono controlli Web part in una pagina), e che sono controlli alle varie zone. Inoltre, tiene traccia e controlla le modalità di visualizzazione diversi che una pagina può essere in, ad esempio Sfoglia, connettere, modificare o modalità del catalogo e se le modifiche di personalizzazione applicano a tutti gli utenti o ai singoli utenti. Infine, avvia e tiene traccia delle connessioni e la comunicazione tra i controlli Web part.

Il secondo tipo di componente strutturale dell'interfaccia utente è la zona. Le aree funzionano come i gestori di layout in una pagina Web part. Essi contengono e organizzare i controlli che derivano dalla classe parte (parte controlli) e offrono la possibilità di eseguire operazioni di layout di pagina modulari con orientamento orizzontale o verticale. Le zone offrono anche elementi dell'interfaccia utente comuni e coerenti (ad esempio lo stile dell'intestazione e piè di pagina, titolo, lo stile del bordo, pulsanti di azione e così via) per ogni controllo che contengono; Questi elementi comuni sono noti come il colore di un controllo. Nelle modalità di visualizzazione diversi e con vari controlli, vengono usati diversi tipi specializzati di zone. Nella sezione controlli Web part essenziale riportato di seguito sono descritti i diversi tipi di zone.

I controlli di interfaccia utente Web part, ognuno dei quali derivano dal **parte** di classi, costituiscono l'interfaccia utente principale in una pagina Web part. Il set di controlli Web part è flessibile e inclusi nelle opzioni di ti per la creazione di controlli Web part. Oltre alla creazione di controlli Web part personalizzati, è possibile anche usare i controlli server ASP.NET esistenti, i controlli utente o controlli server personalizzati come controlli Web part. I controlli essenziali utilizzati più frequentemente per la creazione di pagine Web part sono descritti nella sezione successiva.

## <a name="web-parts-essential-controls"></a>Essential controlli Web part

Il set di controlli Web part è vasto, ma alcuni controlli sono essenziali, poiché sono necessari per le Web part lavorare o perché sono i controlli usati più frequentemente nelle pagine Web part. Come iniziare a utilizzare le Web part e crea le pagine Web part, è consigliabile acquisire familiarità con i controlli Web part essenziali descritti nella tabella seguente.

| **Controllo Web part** | **Descrizione** |
| --- | --- |
| WebPartManager | Gestisce tutti i controlli Web part in una pagina. (Uno solo) **WebPartManager** controllo è obbligatorio per ogni pagina Web part. |
| CatalogZone | Contiene controlli CatalogPart. Utilizzare quest'area per creare un catalogo di controlli Web part da cui gli utenti possono selezionare i controlli da aggiungere a una pagina. |
| EditorZone | Contiene controlli EditorPart. Utilizzare quest'area per consentire agli utenti di modificare e personalizzare i controlli Web part in una pagina. |
| WebPartZone | Contiene e fornisce il layout complessivo per i controlli Web part che costituiscono l'interfaccia utente principale di una pagina. Utilizzare quest'area ogni volta che si creano pagine con i controlli Web part. Le pagine possono contenere una o più zone. |
| ConnectionsZone | Contiene controlli WebPartConnection e fornisce un'interfaccia utente per la gestione delle connessioni. |
| Web part (GenericWebPart) | Esegue il rendering di interfaccia utente primaria. la maggior parte dei controlli di interfaccia utente Web part rientrano in questa categoria. Per ottimizzare il controllo a livello di codice, è possibile creare controlli Web part personalizzati che derivano dalla base **WebPart** controllo. È anche possibile usare i controlli server esistenti, i controlli utente o controlli personalizzati come controlli Web part. Ogni volta che uno di questi controlli vengono posizionato in una zona, il **WebPartManager** controllo automaticamente ne esegue il wrapping con **GenericWebPart** controlli in fase di esecuzione in modo che sia possibile usarle con funzionalità di Web part. |
| CatalogPart | Contiene un elenco di controlli Web part disponibili che gli utenti possono aggiungere alla pagina. |
| WebPartConnection | Crea una connessione tra due controlli Web part in una pagina. La connessione definisce uno dei controlli Web part come un provider (dei dati) e l'altro come un consumer. |
| EditorPart | Funge da classe base per i controlli di editor specializzato. |
| Controlli EditorPart (AppearanceEditorPart LayoutEditorPart, BehaviorEditorPart e PropertyGridEditorPart) | Consentire agli utenti di personalizzare diversi aspetti dei controlli di interfaccia utente Web part in una pagina |

## <a name="lab-create-a-web-part-page"></a>Esercitazione: Creare una pagina Web Part

In questo laboratorio si creerà una pagina Web part che rimarranno persistenti le informazioni tramite i profili ASP.NET.

### <a name="creating-a-simple-page-with-web-parts"></a>Creazione di una semplice pagina con Web part

In questa parte della procedura dettagliata, si crea una pagina che usa i controlli Web part per visualizzare il contenuto statico. Il primo passaggio nell'utilizzo delle Web part consiste nel creare una pagina con due elementi strutturali richiesti. In primo luogo, una pagina Web part richiede un controllo WebPartManager per tenere traccia e coordinare tutti i controlli Web part. In secondo luogo, una pagina Web part richiede uno o più aree, quali sono i controlli composti che contengono Web part o altri controlli server e occupano una determinata area di una pagina.

> [!NOTE]
> Non è necessario eseguire alcuna operazione per abilitare la personalizzazione di Web part; si è abilitato per impostazione predefinita per il set di controlli Web part. Quando si esegue innanzitutto una pagina Web part in un sito, ASP.NET imposta un provider di personalizzazioni predefinito per archiviare le impostazioni di personalizzazione utente. Per altre informazioni sulla personalizzazione, vedere Panoramica sulla personalizzazione delle parti Web.


### <a name="to-create-a-page-for-containing-web-parts-controls"></a>Per creare una pagina destinata a contenere i controlli Web part

1. Chiudere la pagina predefinita e aggiungere una nuova pagina per il sito denominato WebPartsDemo.
2. Passare a **progettazione** visualizzazione.
3. Dal **vista** menu, assicurarsi che il **controlli Non visivi** e **dettagli** sono selezionate le opzioni in modo che è possibile visualizzare i tag di layout e i controlli che non è un'interfaccia utente.
4. Posizionare il cursore prima il `<div>` i tag di area di progettazione e premere INVIO per aggiungere una nuova riga. Posizionare il cursore prima il carattere di nuova riga, fare clic sui **formato blocco** elenco a discesa elenco di controllo nel menu e selezionare il **titolo 1** opzione. Nell'intestazione, aggiungere il testo **pagina di Web part dimostrazione**.
5. Dal **WebParts** della casella degli strumenti, trascinare un **WebPartManager** controllo nella pagina di, posizionarla subito dopo il carattere di nuova riga e prima di `<div>`tag.   
  
   Il **WebPartManager** controllo non esegue il rendering qualsiasi output, viene visualizzato come una casella grigia nell'area di progettazione.
6. Posizionare il cursore all'interno di `<div>` tag.
7. Nel **Layout** menu, fare clic su **Inserisci tabella**e creare una nuova tabella con una riga e tre colonne. Fare clic sui **Cell Properties** pulsante, seleziona **top** dal **allineamento verticale** elenco a discesa, fare clic su **OK**e fare clic su **OK** nuovamente per creare la tabella.
8. Trascinare un controllo WebPartZone nella colonna della tabella a sinistra. Fare doppio clic il **WebPartZone** controllare, scegliere **proprietà**e impostare le proprietà seguenti:   
  
   ID: SidebarZone   
  
   HeaderText: nella barra laterale
9. Trascinare una seconda **WebPartZone** controllare nella colonna della tabella centrale e impostare le proprietà seguenti:   
  
   ID: MainZone   
  
   HeaderText: principale
10. Salvare il file.

La pagina ora presenta due zone distinte che possono essere controllati separatamente. Tuttavia, nessuno dei due zona comprende qualsiasi contenuto, in modo che la creazione di contenuto è il passaggio successivo. Per questa procedura dettagliata, si utilizzano i controlli Web part che consentono di visualizzare solo il contenuto statico.

Il layout di una zona Web part specificato da un &lt;zonetemplate&gt; elemento. All'interno del modello di zona, è possibile aggiungere qualsiasi controllo ASP.NET, se è un controllo Web part personalizzato, un controllo utente o un controllo server esistente. Si noti che qui si usa il controllo etichetta e che si sta aggiungendo semplicemente un testo statico. Quando si inserisce un controllo del server regolare in una **WebPartZone** zona, ASP.NET gestisce il controllo come un controllo Web part in fase di esecuzione, che abilita le funzionalità del controllo Web part.

**Per creare contenuto per la zona principale**

1. Nella **progettazione** visualizzazione, trascinare un **etichetta** controllare dal **Standard** scheda della casella degli strumenti nell'area di contenuto dell'area di cui **ID** proprietà è impostato su MainZone.
2. Passare a **origine** visualizzazione. Si noti che un &lt;zonetemplate&gt; elemento è stato aggiunto per eseguire il wrapping il **etichetta** controllo in MainZone.
3. Aggiungere un attributo denominato **title** per il &lt;asp: label&gt; elemento e impostarne il valore al contenuto. Rimuovere il testo = attributo "Label" di &lt;asp: label&gt; elemento. Tra i tag di apertura e chiusura del &lt;asp: label&gt; elemento, aggiungere testo, ad esempio **benvenuto alla mia Home Page** all'interno di una coppia di &lt;h2&gt; tag dell'elemento. Il codice dovrebbe essere come indicato di seguito. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample21.aspx)]
4. Salvare il file.

Successivamente, creare un controllo utente che può essere aggiunti anche alla pagina un controllo Web part.

### <a name="to-create-a-user-control"></a>Per creare un controllo utente

1. Aggiungere un nuovo controllo utente Web al sito come un controllo di ricerca. Deselezionare l'opzione **inserire il codice sorgente in un file separato**. Aggiungerlo nella stessa directory WebPartsDemo e denominarlo SearchUserControl.   
  
    > [!NOTE]
    > Il controllo utente per questa procedura dettagliata non implementa la funzionalità di ricerca effettivo; utilizzato solo per illustrare le funzionalità di Web part.
2. Passare a **progettazione** visualizzazione. Dal **Standard** scheda casella degli strumenti, trascinare un controllo TextBox nella pagina.
3. Posizionare il cursore dopo la casella di testo che appena aggiunto e premere INVIO per aggiungere una nuova riga.
4. Trascinare un controllo pulsante nella pagina in una nuova riga sotto la casella di testo che appena aggiunto.
5. Passare a **origine** visualizzazione. Assicurarsi che il codice sorgente per il controllo utente è simile al seguente. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample22.aspx)]
6. Salvare e chiudere il file.

A questo punto è possibile aggiungere i controlli Web part a tale area. A tale area si vengono aggiunti due controlli, uno che contiene un elenco di collegamenti e l'altro che è il controllo utente è stato creato nella procedura precedente. I collegamenti vengono aggiunti come standard **etichetta** controllo server, simile al modo in cui è stato creato il testo statico dell'area principale. Tuttavia, sebbene i singoli controlli server contenuti nel controllo utente potrebbe essere contenuto direttamente nella zona (ad esempio, il controllo etichetta), in questo caso non lo sono. Al contrario, fanno parte del controllo utente che è stato creato nella procedura precedente. Ciò viene illustrato un modo comune per creare un pacchetto con controlli e funzionalità aggiuntive che si desidera un controllo utente e quindi fare riferimento a tale controllo in una zona di un controllo Web part.

In fase di esecuzione, il set di controlli Web part esegue il wrapping di entrambi i controlli con i controlli di GenericWebPart. Quando un **GenericWebPart** controllo esegue il wrapping di un controllo server Web, il controllo Web part generico è il controllo padre e il controllo del server è possibile accedere tramite la proprietà ChildControl del controllo padre. Questo uso di controlli Web part generico consente ai controlli server Web standard avere lo stesso comportamento di base e gli attributi dei controlli Web part che derivano dal **WebPart** classe.

### <a name="to-add-web-parts-controls-to-the-sidebar-zone"></a>Per aggiungere i controlli Web part all'area dell'intestazione laterale

1. Aprire la pagina WebPartsDemo.
2. Passare a **progettazione** visualizzazione.
3. Trascinare la pagina del controllo utente è stato creato, SearchUserControl, dalla **Esplora soluzioni** all'area di cui **ID** proprietà è impostata su SidebarZone e rilasciarla non esiste.
4. Salvare la pagina WebPartsDemo.
5. Passare a **origine** visualizzazione.
6. All'interno di &lt;asp: webpartzone&gt; (elemento) per SidebarZone, appena sopra il riferimento al controllo utente, aggiungere un' &lt;asp: label&gt; elemento con contenuto collegamenti, come illustrato nell'esempio seguente. Aggiungere anche un **Title** tag del controllo utente, con il valore dell'attributo **ricerca**, come illustrato. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample23.aspx)]
7. Salvare e chiudere il file.

È ora possibile testare la pagina, esplorandola nel browser. La pagina Visualizza le due aree. Lo screenshot seguente mostra la pagina.

**Pagina di prova di parti Web con due zone**


![Schermata procedura dettagliata 1 di Web part Visual Studio](profiles-themes-and-web-parts/_static/image3.gif)

**Figura 3**: Web part VS schermata della procedura dettagliata 1


Nel titolo della barra di ogni controllo è una freccia verso il basso che fornisce l'accesso a un menu dei verbi di azioni disponibili, che è possibile eseguire su un controllo. Scegliere il menu dei verbi per uno dei controlli, quindi scegliere il **Riduci a icona** verbo e notare che il controllo è ridotto. Scegliere dal menu dei verbi **ripristinare**, e il controllo viene restituito per le dimensioni normali.

### <a name="enabling-users-to-edit-pages-and-change-layout"></a>Consentendo agli utenti di modificare il Layout e pagine di modifica

Web part consente agli utenti di modificare il layout dei controlli Web part trascinandoli da un'area a altra. Oltre a consentire agli utenti di spostare **WebPart** controlli da una zona a altra, è possibile consentire agli utenti di modificare diverse caratteristiche dei controlli, tra cui loro aspetto, il layout e comportamento. Il set di controlli Web part fornisce funzionalità di modifica di base per **WebPart** controlli. Sebbene non verrà effettuato in questa procedura dettagliata, è possibile anche creare controlli di editor personalizzati che consentono agli utenti di modificare le funzionalità di **WebPart** controlli. Come con la modifica del percorso di un **WebPart** (controllo), la modifica delle proprietà di un controllo si basa sulla personalizzazione di ASP.NET per salvare le modifiche apportate dagli utenti.

In questa parte della procedura dettagliata, si aggiungere la possibilità agli utenti di modificare le caratteristiche di base di qualsiasi **WebPart** controllo nella pagina. Per abilitare queste funzionalità, aggiungere un altro controllo utente personalizzato la pagina, insieme a un &lt;asp: editorzone&gt; elemento e due controlli di modifica.

### <a name="to-create-a-user-control-that-enables-changing-page-layout"></a>Per creare un controllo utente che consente la modifica il layout di pagina

1. In Visual Studio, sul **File** menu, seleziona il **New** sottomenu e fare clic sul **File** opzione.
2. Nel **Aggiungi nuovo elemento** finestra di dialogo, seleziona **controllo utente Web**. Denominare il nuovo file DisplayModeMenu. Deselezionare l'opzione **inserire il codice sorgente in file separati**.
3. Fare clic su Aggiungi per creare il nuovo controllo.
4. Passare a **origine** visualizzazione.
5. Rimuovere tutto il codice esistente nel nuovo file e incollare il codice seguente. Questo codice di controllo utente vengono utilizzate caratteristiche che consentono di modificare la modalità di visualizzazione della pagina dell'insieme di controlli Web part e consente inoltre di modificare l'aspetto fisico e layout di pagina mentre si trovano in determinate modalità di visualizzazione. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample24.aspx)]
6. Salvare il file facendo clic su Salva icona sulla barra degli strumenti o selezionando **salvare** nel **File** menu.

### <a name="to-enable-users-to-change-the-layout"></a>Consentire agli utenti di modificare il layout

1. Aprire la pagina WebPartsDemo e passare a **progettazione** visualizzazione.
2. Posizionare il cursore nel **Design** visualizzare subito dopo la **WebPartManager** controllo aggiunti in precedenza. Aggiungere un disco rigido restituito dopo il testo in modo che sia presente una riga vuota dopo il **WebPartManager** controllo. Posizionare il punto di inserimento sulla riga vuota.
3. Trascinare il controllo utente appena creato (il file è denominato DisplayModeMenu) nel WebPartsDemo pagina e rilasciarla sulla riga vuota.
4. Trascinare un controllo EditorZone dal **WebParts** sezione della casella degli strumenti per la cella Apri tabella rimanenti nella pagina WebPartsDemo.
5. Dal **WebParts** sezione della casella degli strumenti, trascinare un AppearanceEditorPart (controllo) e un controllo LayoutEditorPart nel **EditorZone** controllo.
6. Passare a **origine** visualizzazione. Il codice risultante nella cella della tabella dovrebbe essere simile al codice seguente. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample25.aspx)]
7. Salvare il file WebPartsDemo. È stato creato un controllo utente che consente di modificare le modalità di visualizzazione e il layout di pagina e si è fatto riferimento il controllo della pagina Web primario.

È ora possibile testare la possibilità di modificare le pagine e il layout.

### <a name="to-test-layout-changes"></a>Per testare le modifiche del layout

1. Caricare la pagina in un browser.
2. Fare clic sui **modalità di visualizzazione** dal menu a discesa e selezionare **modificare**. Vengono visualizzati i titoli di zona.
3. Trascinare il **collegamenti personali** controllo dalla barra del titolo da tale area nella parte inferiore dell'area principale. La pagina dovrebbe essere, ad esempio l'immagine riportata di seguito.

### <a name="web-parts-demo-page-with-my-links-control-moved"></a>Pagina di prova di parti Web con il controllo collegamenti personali spostato


![Schermata procedura dettagliata 2 di Web part Visual Studio](profiles-themes-and-web-parts/_static/image4.gif)

**Figura 4**: Web part VS schermata della procedura dettagliata 2


1. Fare clic sui **modalità di visualizzazione** dal menu a discesa e selezionare **Sfoglia**. La pagina viene aggiornata, non vengono più visualizzati i nomi delle aree e il **collegamenti personali** controllo rimane nella stessa posizione.
2. Per illustrare il funzionamento della personalizzazione, chiudere il browser e quindi caricare di nuovo la pagina. Le modifiche apportate vengono salvate per le sessioni future del browser.
3. Dal **modalità di visualizzazione** dal menu **modificare**.   
  
   Ogni controllo nella pagina viene visualizzato con una freccia verso il basso nella barra del titolo, che contiene l'elenco a discesa dei verbi.
4. Fare clic sulla freccia per visualizzare il menu dei verbi nel **collegamenti personali** controllo. Scegliere il **modifica** verbo.   
  
   Il **EditorZone** controllo viene visualizzato, la visualizzazione di EditorPart controlli aggiunto in precedenza.
5. Nel **aspetto** sezione del controllo di modifica, modifica il **titolo** Preferiti, usare il **tipo riquadro** elenco a discesa per selezionare **solo Title**, quindi fare clic su **applica**. Lo screenshot seguente mostra la pagina in modalità di modifica.

### <a name="web-parts-demo-page-in-edit-mode"></a>Pagina di prova di parti Web in modalità di modifica


![Schermata procedura dettagliata 3 parti VS per Web](profiles-themes-and-web-parts/_static/image5.gif)

**Figura 5**: Web schermata della procedura dettagliata 3 parti di Visual Studio


1. Scegliere il **modalità di visualizzazione** menu e selezionare **Sfoglia** per tornare alla modalità browse.
2. Il controllo ha ora un titolo aggiornato e nessun bordo, come illustrato nella schermata riportata di seguito.

### <a name="edited-web-parts-demo-page"></a>Pagina Web part Demo modificata


![Schermata procedura dettagliata 4 parti VS per Web](profiles-themes-and-web-parts/_static/image6.gif)

**Figura 4**: Web schermata della procedura dettagliata 4 parti Visual Studio


### <a name="adding-web-parts-at-run-time"></a>Aggiunta di Web part in fase di esecuzione

È anche possibile consentire agli utenti di aggiungere i controlli Web part alla rispettiva pagina in fase di esecuzione. A tale scopo, configurare la pagina con un catalogo di Web part, che contiene un elenco di controlli Web part che si desidera rendere disponibili agli utenti.

**Per consentire agli utenti di aggiungere le Web part in fase di esecuzione**

1. Aprire la pagina WebPartsDemo e passare a **progettazione** visualizzazione.
2. Dal **WebParts** della scheda della casella degli strumenti, trascinare un controllo CatalogZone nella colonna a destra della tabella, sotto il **EditorZone** controllo.   
  
   Entrambi i controlli possono essere nella stessa cella di tabella perché è non vengano visualizzate nello stesso momento.
3. Nel riquadro proprietà, la stringa assegnata **Aggiungi Web part** alla proprietà HeaderText delle **CatalogZone** controllo.
4. Dal **WebParts** sezione della casella degli strumenti, trascinare un controllo DeclarativeCatalogPart nell'area del contenuto delle **CatalogZone** controllo.
5. Fare clic sulla freccia nell'angolo superiore destro del **DeclarativeCatalogPart** per esporre il relativo menu attività di controllo e quindi selezionare **modifica modelli**.
6. Dal **Standard** sezione della casella degli strumenti, trascinare un **FileUpload** controllo e un **calendario** controllano nel **WebPartsTemplate** sezione del **DeclarativeCatalogPart** controllo.
7. Passare a **origine** visualizzazione. Esaminare il codice sorgente del &lt;asp: catalogzone&gt; elemento. Si noti che il **DeclarativeCatalogPart** controllo contiene un &lt;webpartstemplate&gt; elemento con i due controlli server racchiusi che sarà possibile aggiungere a una pagina dal catalogo.
8. Aggiungere un **titolo** proprietà per ognuno dei controlli aggiunti al catalogo, utilizzando il valore stringa visualizzato per ogni titolo nell'esempio di codice riportato di seguito. Anche se il titolo non è una proprietà in genere è possibile impostare su questi due controlli server in fase di progettazione, quando un utente aggiunge questi controlli a un **WebPartZone** zona dal catalogo in fase di esecuzione, essi vengono ciascuno racchiuso tra un  **GenericWebPart** controllo. Ciò consente loro di fungere da controlli Web part, che saranno in grado di visualizzare i titoli.   
  
   Il codice per i due controlli contenuti nel **DeclarativeCatalogPart** controllo dovrebbe apparire come segue. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample26.aspx)]
9. Salvare la pagina.

È ora possibile testare il catalogo.

### <a name="to-test-the-web-parts-catalog"></a>Per testare il catalogo di Web part

1. Caricare la pagina in un browser.
2. Fare clic sui **modalità di visualizzazione** dal menu a discesa e selezionare **catalogo**.   
  
   Il catalogo intitolato **Aggiungi Web part** viene visualizzato.
3. Trascinare il **Preferiti** controllare dalla zona principale torna all'inizio dell'area di intestazione laterale, quindi rilasciarla non esiste.
4. Nel **Aggiungi Web part** catalogo, selezionare entrambe le caselle di controllo e quindi selezionare **Main** nell'elenco a discesa che contiene le zone di disponibilità.
5. Fare clic su **Add** nel catalogo. I controlli vengono aggiunti all'area principale. Se si desidera, è possibile aggiungere più istanze di controlli dal catalogo per la pagina.   
  
   Lo screenshot seguente mostra la pagina con il controllo di caricamento di file e il calendario nella zona principale. 

![Controlli aggiunti all'area principale dal catalogo](profiles-themes-and-web-parts/_static/image7.gif)

    **Figure 5**: Controls added to Main zone from the catalog
6. Fare clic sui **modalità di visualizzazione** dal menu a discesa e selezionare **Sfoglia**. Il catalogo viene rimosso e la pagina viene aggiornata.
7. Chiudere il browser. Caricare di nuovo la pagina. Le modifiche apportate salvare in modo permanente.
