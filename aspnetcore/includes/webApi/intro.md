## <a name="overview"></a>Přehled

Tady je rozhraní API, které vytvoříte:

|rozhraní API | Popis    | Text žádosti    | Text odpovědi   |
|--- | ---- | ---- | ---- |
|ZÍSKAT /api/todo  | Načtení všech položek seznamu úkolů | Žádné | Pole položkami seznamu úkolů|
|ZÍSKAT /api/todo / {id}  | Získání položky podle ID | Žádné | Položky seznamu úkolů|
|POST/api/todo | Přidat novou položku | Položky seznamu úkolů  | Položky seznamu úkolů |
|UVEĎTE /api/todo / {id} | Aktualizace stávající položku&nbsp;  | Položky seznamu úkolů |  Žádné |
|Odstranit /api/todo / {id}&nbsp;  &nbsp; | Odstranit položku&nbsp;  &nbsp;  | Žádné  | Žádné|

<br>

Následující diagram znázorňuje základní návrhu aplikace.

![Klient je reprezentována pole na levé straně a odešle žádost a obdrží odpověď z aplikace, pole vykreslovat na pravé straně. V dialogovém okně aplikace představují tři pole kontroleru, model a vrstva přístupu k datům. Žádost přichází do aplikace řadiče a operace čtení a zápisu dochází ke mezi řadičem a vrstva přístupu k datům. Model je serializované a vrátí klientovi v odpovědi.](../../tutorials/first-web-api/_static/architecture.png)

* Klient je, ať využívá webové rozhraní API (mobilní aplikace, prohlížeč atd.). V tomto kurzu jsme nejsou zápisu klienta. Použijeme [Postman](https://www.getpostman.com/) nebo [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html) k testování aplikace.

* A *modelu* je objekt, který představuje data ve vaší aplikaci. V takovém případě je pouze model položku seznamu úkolů. Modely jsou reprezentovány jako třídy jazyka C#, také znát jako **P**rostý **O**ld **C**# **O**bject (POCOs).

* A *řadič* je objekt, který zpracovává HTTP požadavků a vytvoří odpověď HTTP. Tato aplikace bude mít jediného kontroleru.

* Pro zjednodušení tento kurz, nepoužívá aplikaci trvalé databáze. Ukázková aplikace ukládá položky úkolů v databázi v paměti.
