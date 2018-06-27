# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a>Přidat model do aplikace ASP.NET MVC jádra

Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [tní Dykstra](https://github.com/tdykstra)

V této části přidáte některé třídy pro správu filmy v databázi. Tyto třídy bude "**M**odelu" součástí **M**VC aplikace.

Použití těchto tříd s [Entity Framework Core](/ef/core) (EF jader) pro práci s databází. Základní EF je představuje rozhraní objektu relační mapování (ORM), které zjednodušuje data přístupový kód, který máte k zápisu. [Jádro EF podporuje mnoho databázové stroje](/ef/core/providers/).

Třídy modelu, který budete vytvářet se označují jako třídy objektů POCO (z "prostý starý CLR objektů"), protože nemají některé závislé na základní EF. Právě definují vlastnosti data, která bude uložená v databázi.

V tomto kurzu napíšete třídy modelu nejprve a databáze se vytvoří EF jádra. Alternativní přístup nejsou zahrnuté v tomto poli se generují třídy modelu v databázi již existuje. Informace o tohoto přístupu najdete v tématu [ASP.NET Core - existující databázi](/ef/core/get-started/aspnetcore/existing-db).

## <a name="add-a-data-model-class"></a>Přidejte třídu modelu dat
