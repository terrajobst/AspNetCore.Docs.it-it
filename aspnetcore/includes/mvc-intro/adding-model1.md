# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a>Aggiungere un modello a un'app ASP.NET Core MVC

Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Tom Dykstra](https://github.com/tdykstra)

In questa sezione si aggiungeranno alcune classi per la gestione di film in un database. Queste classi saranno la parte "**M**odel" dell'app **M**VC.

Si usano queste classi con [Entity Framework Core](/ef/core) (EF Core) per usare un database. EF Core è un framework object-relational mapping (ORM) che semplifica il codice di accesso ai dati che è necessario scrivere. [EF Core supporta molti motori di database](/ef/core/providers/).

Le classi del modello che verranno create sono note come classi POCO (da "plain-old CLR Object") perché non contengono dipendenze da Entity Framework Core. Definiscono semplicemente le proprietà dei dati che verranno archiviati nel database.

In questa esercitazione si scrivono innanzitutto le classi di modello ed Entity Framework Core creerà il database. Un approccio alternativo non trattato in questo articolo consiste nel generare classi del modello da un database già esistente. Per informazioni su questo approccio, vedere [ASP.NET Core - Existing Database](/ef/core/get-started/aspnetcore/existing-db) (Introduzione a EF Core su ASP.NET Core con un database esistente).

## <a name="add-a-data-model-class"></a>Aggiungere una classe del modello di dati
