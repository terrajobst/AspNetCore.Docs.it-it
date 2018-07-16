Di [Rick Anderson](https://twitter.com/RickAndMSFT)

In questa sezione si aggiungono alcune classi per la gestione di filmati in un database. Si usano queste classi con [Entity Framework Core](/ef/core) (EF Core) per usare un database. EF Core è un framework object-relational mapping (ORM) che semplifica il codice di accesso ai dati che è necessario scrivere.

Le classi di modello create sono dette classi POCO (da "plain-old CLR objects") perché non hanno alcuna dipendenza in EF Core. Definiscono le proprietà dei dati archiviati nel database.

In questa esercitazione si scrivono innanzitutto le classi di modello e EF Core crea il database. Un approccio alternativo non trattato in questo articolo consiste nel [generare classi del modello da un database esistente](/ef/core/get-started/aspnetcore/existing-db).

[Visualizzare o scaricare il campione ](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie).
