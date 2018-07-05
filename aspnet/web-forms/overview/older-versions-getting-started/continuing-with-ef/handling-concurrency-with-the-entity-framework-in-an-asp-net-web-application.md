---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application
title: Ošetření souběžnosti se sadou Entity Framework 4.0 v ASP.NET 4 webové aplikace | Dokumentace Microsoftu
author: tdykstra
description: V této sérii kurzů staví na Contoso University webovou aplikaci, která se vytvořila Začínáme s Entity Framework 4.0 série kurzů. I...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: a5aa22a6-fb7f-4f41-9c7f-addda151940b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application
msc.type: authoredcontent
ms.openlocfilehash: c22f06e68b15d3fbf13e9f2af0e19631abe076ec
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37392734"
---
<a name="handling-concurrency-with-the-entity-framework-40-in-an-aspnet-4-web-application"></a>Ošetření souběžnosti se sadou Entity Framework 4.0 v ASP.NET 4 webové aplikace
====================
podle [Petr Dykstra](https://github.com/tdykstra)

> V této sérii kurzů staví na Contoso University webovou aplikaci, která se vytvořila [Začínáme s Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) série kurzů. Pokud nebyla dokončena v předchozích kurzech, jako výchozí bod pro účely tohoto kurzu můžete [stáhnout aplikaci](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) , kterou by jste vytvořili. Můžete také [stáhnout aplikaci](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) , který vytvoří kompletní série kurzů. Pokud máte dotazy týkající se těchto kurzů, můžete je publikovat [fórum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).


V předchozím kurzu jste zjistili, jak řadit a filtrovat data s využitím `ObjectDataSource` ovládacího prvku a Entity Framework. Tento kurz ukazuje možnosti pro ošetření souběžnosti ve webové aplikaci ASP.NET, která používá Entity Framework. Vytvoří novou webovou stránku, který je vyhrazen pro aktualizaci přiřazení office instruktorem. Postaráme potíže se souběžností na této stránce a na stránce oddělení, který jste vytvořili dříve.

[![Image06](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image2.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image1.png)

[![Image01](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image4.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image3.png)

## <a name="concurrency-conflicts"></a>Konflikty souběžnosti

Když jeden uživatel upraví záznam a jiný uživatel upraví stejný záznam před první uživatel změn je zapsána do databáze dojde ke konfliktu souběžnosti. Pokud nenastavíte Entity Framework takové konflikty je zjistit, kdo aktualizuje databázi poslední přepíše změny dalších uživatelů. V mnoha aplikacích toto riziko je přijatelné a není nutné nakonfigurovat aplikaci pro zpracování konfliktů souběžnosti je to možné. (Pokud existuje několik uživatelů nebo několik aktualizací, nebo pokud není skutečně důležité, pokud některé změny budou přepsány, výhody vyváží nižší náklady na programování pro souběžnost.) Pokud není nutné se starat o konfliktů souběžnosti, můžete přeskočit tento kurz; Zbývající dva kurzy z této série nejsou závislé na všechno, co vytvoříte v tohoto objektu.

### <a name="pessimistic-concurrency-locking"></a>Pesimistická souběžnost (uzamčení)

Pokud vaše aplikace potřebuje se tak ztrátě dat ve scénářích souběžnosti, je to udělat jedním ze způsobů použití uzamčení databáze. Tento postup se nazývá *Pesimistická souběžnost*. Například předtím, než se pustíte do čtení řádku z databáze, můžete požádat o zámek pro jen pro čtení nebo pro přístup k aktualizaci. Pokud řádek pro aktualizaci přístup, žádné uživatelé můžou zamknout řádku, buď pro jen pro čtení nebo aktualizaci přístup, protože by dostanou kopii dat, která se právě mění. Pokud řádek pro přístup jen pro čtení, ostatní také zařízení Uzamknout pro přístup jen pro čtení, ale ne pro aktualizace.

Zámky pro správu obsahuje i určité nevýhody. Může být složité do programu. Vyžaduje významné databáze správy zdrojů, a to může způsobit problémy s výkonem jako počet uživatelů aplikace zvyšuje (to znamená, ho nemá uspokojivé škálování). Z těchto důvodů ne všechny systémy správy databáze nepodporují Pesimistická souběžnost. Entity Framework obsahuje předdefinovanou podporu pro ni a v tomto kurzu nezobrazí způsobu jeho implementace.

### <a name="optimistic-concurrency"></a>Optimistická souběžnost

Je alternativou k Pesimistická souběžnost *optimistického řízení souběžnosti*. Povolení konfliktů souběžnosti, která se provede a reaguje správně, pokud tomu znamená, že optimistického řízení souběžnosti. Například Jan spustí *Department.aspx* stránce, kliknutí **upravit** odkaz pro oddělení historie a snižuje **rozpočtu** částku od 1,000,000.00 $ $ 125,000.00. (Jan spravuje konkurenční oddělení a chce uvolnit tak peníze pro své oddělení.)

[![Image07](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image6.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image5.png)

Předtím, než Jan klikne **aktualizace**, Jana běží na stejné stránce, kliknutí **upravit** odkaz historie oddělení, a potom klepněte na změny **datum zahájení** pole z 1/10/2011 na 1/1 / 1999. (Jana spravuje oddělení historie a chce, aby se pro ni další služební.)

[![Image08](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image8.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image7.png)

Jan klikne **aktualizace** pak Jana nejprve klikne **aktualizace**. Jana prohlížeč teď seznamy **rozpočtu** objem $1,000,000.00, ale to je nesprávná, protože velikost změnila Jan na 125,000.00 $.

Některé akce, které si můžete v tomto scénáři patří:

- Můžete sledovat, které vlastnosti uživatele byl změněn a aktualizovat pouze odpovídající sloupce v databázi. V ukázkovém scénáři žádné by dojít ke ztrátě dat., protože různé vlastnosti byly aktualizovány dva uživatelé. Při příštím někdo prohlíží historii oddělení, zobrazí se 1/1/1999 a 125,000.00 $. 

    Toto je výchozí chování v Entity Framework a může podstatně snížit počet konfliktů, ke kterým může dojít ke ztrátě. Ale toto chování není nedošlo ke ztrátě dat. Pokud konkurenční změn na stejnou vlastnost entity. Kromě toho není toto chování vždycky možné; Při mapování uložené procedury pro typ entity, všechny vlastnosti entity jsou aktualizovány při jakékoli změny v entitě v databázi.
- Můžete umožnit změnu Jana John's změna přepsána. Po kliknutí Jana **aktualizace**, **rozpočtu** částka se vrátí do 1,000,000.00 $. Tento postup se nazývá *Wins, klient* nebo *poslední ve službě Wins* scénář. (Hodnoty klienta přednost co je v úložišti.)
- Jana změny můžete zabránit aktualizují v databázi. By obvykle zobrazí chybovou zprávu, zobrazit její aktuální stav dat a manažerovi zadejte znovu své změny, pokud chce je. Uložením svůj vstup a poskytne jí příležitost znovu bez nutnosti znovu zadat ho může dál automatizovat proces. Tento postup se nazývá *Store Wins* scénář. (Hodnoty úložiště dat přednost hodnoty odeslány klientem.)

### <a name="detecting-concurrency-conflicts"></a>Zjišťování konfliktů souběžnosti

V rozhraní Entity Framework lze vyřešit konflikty zpracování `OptimisticConcurrencyException` výjimky, které vyvolá rozhraní Entity Framework. Pokud chcete zjistit, kdy se má vyvolat tyto výjimky, musí být schopen rozpoznat konflikty Entity Framework. Proto je nutné nakonfigurovat databázi a datový model odpovídajícím způsobem. Některé možnosti aktivace zjišťování konfliktů, patří:

- V databázi zahrnují sloupce tabulky, který slouží k určení, kdy změnil řádek. Potom můžete nakonfigurovat rozhraní Entity Framework patří tento sloupec ve `Where` klauzule SQL `Update` nebo `Delete` příkazy.

    To je účel `Timestamp` sloupec v `OfficeAssignment` tabulky.

    [![Image09](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image10.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image9.png)

    Datový typ `Timestamp` sloupce se také nazývá `Timestamp`. Sloupec však skutečně neobsahuje hodnotu data a času. Místo toho je hodnota pořadové číslo, které se zvýší pokaždé, když se aktualizuje řádek. V `Update` nebo `Delete` příkazu `Where` klauzule obsahuje původní `Timestamp` hodnotu. Pokud se změnila řádek aktualizován jiným uživatelem, hodnota v `Timestamp` se liší od původní hodnoty, proto `Where` klauzule vrátí žádný řádek aktualizovat. Při Entity Framework zjistí, že byly aktualizovány žádné řádky aktuální `Update` nebo `Delete` příkazu (to znamená, když počet ovlivněných řádků je nula), který interpretuje jako ke konfliktu souběžnosti.
- Konfigurace rozhraní Entity Framework pro zahrnutí původní hodnota každý sloupec v tabulce `Where` klauzuli `Update` a `Delete` příkazy.

    Jako první možnost, pokud něco v řádku od řádku se nejdřív přečíst, změnila `Where` klauzule nevrátí řádek k aktualizaci, která nastavení interpretuje Entity Framework jako ke konfliktu souběžnosti. Tato metoda je co nejúčinnější pomocí `Timestamp` pole, ale může být neefektivní. Pro databázové tabulky, které mají mnoho sloupců, to může vést k velmi velké `Where` klauzule, a ve webové aplikaci může vyžadovat, že udržujete velké množství stavu. Správa velkého objemu stavu může ovlivnit výkon aplikace, protože ji vyžaduje prostředky serveru (například stav relace) nebo musí být součástí webové stránky (například stav zobrazení).

V tomto kurzu přidáte optimistického řízení souběžnosti konflikty pro entitu, která nemá vlastnost sledování zpracování chyb ( `Department` entity) a entity, které mají vlastnost sledování ( `OfficeAssignment` entity).

## <a name="handling-optimistic-concurrency-without-a-tracking-property"></a>Zpracování optimistického řízení souběžnosti bez vlastnosti sledování do

Implementace optimistického řízení souběžnosti pro `Department` entity, která nemá sleduje (`Timestamp`) vlastnost, se dokončí následující úkoly:

- Změnit datový model, který povolit sledování souběžnosti `Department` entity.
- V `SchoolRepository` třídy, zpracování výjimek souběžnosti v `SaveChanges` metody.
- V *Departments.aspx* stránky, zpracování výjimek souběžnosti zobrazením uživateli upozornění, že neúspěšné pokusy o změny byly úspěšné. Uživatel můžete zobrazit aktuální hodnoty a zkuste změny, pokud jsou stále potřeba.

### <a name="enabling-concurrency-tracking-in-the-data-model"></a>Povolení sledování v datovém modelu souběžnosti

V sadě Visual Studio otevřete webová aplikace Contoso University, kterým jste pracovali v předchozím kurzu této série.

Otevřít *SchoolModel.edmx*a v Návrháři modelů dat, klikněte pravým tlačítkem `Name` vlastnost v `Department` entity a pak klikněte na tlačítko **vlastnosti**. V **vlastnosti** okno Změnit `ConcurrencyMode` vlastnost `Fixed`.

[![Image16](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image12.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image11.png)

Provést totéž pro ostatní primární klíč Skalární vlastnosti (`Budget`, `StartDate`, a `Administrator`.) (Nelze to provést pro navigační vlastnosti.) Určuje, že vždy, když rozhraní Entity Framework generuje `Update` nebo `Delete` příkaz SQL pro aktualizaci `Department` entitu v databázi, tyto sloupce (s původní hodnoty) musí být součástí `Where` klauzuli. Pokud není nalezen žádný řádek, kdy `Update` nebo `Delete` příkaz spustí, Entity Framework vyvolá výjimku optimistického řízení souběžnosti.

Uložte a zavřete datový model.

### <a name="handling-concurrency-exceptions-in-the-dal"></a>Zpracování výjimky souběžnosti v vrstvy DAL

Otevřít *SchoolRepository.cs* a přidejte následující `using` příkaz `System.Data` obor názvů:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample1.cs)]

Přidejte následující novou `SaveChanges` metodu, která zpracovává výjimky optimistického řízení souběžnosti:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample2.cs)]

Pokud došlo k chybě souběžnosti nastane, pokud tato metoda je volána, hodnoty vlastností entity v paměti se nahradí hodnotami aktuálně v databázi. Výjimky souběžnosti je znovu vyvolána, takže ji můžete zpracovat webové stránky.

V `DeleteDepartment` a `UpdateDepartment` metody, nahraďte existující volání `context.SaveChanges()` voláním `SaveChanges()` k vyvolání nové metody.

### <a name="handling-concurrency-exceptions-in-the-presentation-layer"></a>Zpracování výjimky souběžnosti v prezentační vrstvě

Otevřít *Departments.aspx* a přidat `OnDeleted="DepartmentsObjectDataSource_Deleted"` atribut `DepartmentsObjectDataSource` ovládacího prvku. Počáteční značka pro ovládací prvek bude nyní vypadat podobně jako v následujícím příkladu.

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample3.aspx)]

V `DepartmentsGridView` řídit, zadejte možnost all sloupců tabulky podle `DataKeyNames` atributu, jak je znázorněno v následujícím příkladu. Všimněte si, že tím se vytvoří zobrazení velmi velkého pole stavu, což je jedním z důvodů důvod, proč pomocí sledování pole je obecně preferovaný způsob, jak sledovat konfliktů souběžnosti.

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample4.aspx)]

Otevřít *Departments.aspx.cs* a přidejte následující `using` příkaz `System.Data` obor názvů:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample5.cs)]

Přidejte následující novou metodu, která bude volat z ovládacího prvku zdroje dat `Updated` a `Deleted` obslužné rutiny událostí pro zpracování výjimky souběžnosti:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample6.cs)]

Tento kód ověří typ výjimky, a pokud se jedná výjimky souběžnosti, kód dynamicky vytvoří `CustomValidator` ovládací prvek, který se pak zobrazí zprávu v `ValidationSummary` ovládacího prvku.

Zavolat novou metodu z `Updated` obslužná rutina události, které jste přidali dříve. Kromě toho, vytvořte nový `Deleted` obslužná rutina události, která volá metodu stejné (ale nebude dělat nic jiného):

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample7.cs)]

### <a name="testing-optimistic-concurrency-in-the-departments-page"></a>Na stránce oddělení testování optimistického řízení souběžnosti

Spustit *Departments.aspx* stránky.

[![Image17](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image14.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image13.png)

Klikněte na tlačítko **upravit** v řádku a změňte hodnotu v **rozpočtu** sloupce. (Mějte na paměti, že lze upravit pouze záznamy, které jste vytvořili pro účely tohoto kurzu, protože stávající `School` záznamy v databázi obsahují neplatná data. Záznam pro oddělení hospodárnost je bezpečné můžete experimentovat.)

[![Image18](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image16.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image15.png)

Otevřete nové okno prohlížeče a spusťte znovu stránky (zkopírovat adresu URL z pole adresy první okno prohlížeče na druhé okno prohlížeče).

[![Image17](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image18.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image17.png)

Klikněte na tlačítko **upravit** ve stejném řádku jste upravili v předchozích krocích a změnit **rozpočtu** hodnotu na něco jiného.

[![Image19](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image20.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image19.png)

V druhém okně prohlížeče, klikněte na tlačítko **aktualizace**. **Rozpočtu** částka se úspěšně změnil na této nové hodnoty.

[![Image20](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image22.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image21.png)

V prvním okně prohlížeče, klikněte na tlačítko **aktualizace**. Aktualizace se nezdaří. **Rozpočtu** částka se zobrazí znovu pomocí hodnotu nastavenou v druhém okně prohlížeče a zobrazí chybovou zprávu.

[![Image21](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image24.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image23.png)

## <a name="handling-optimistic-concurrency-using-a-tracking-property"></a>Zpracování pomocí vlastnosti sledování do optimistického řízení souběžnosti

Pro zpracování optimistického řízení souběžnosti, který má vlastnosti sledování do entity, se dokončí následující úkoly:

- Přidat uložené procedury do datového modelu pro správu `OfficeAssignment` entity. (Vlastnosti sledování a uložené procedury nemusí být použity současně, jsou právě seskupeny dohromady zde ukázky)
- Přidejte metody CRUD vrstvy DAL a BLL pro `OfficeAssignment` entitami, včetně kód pro zpracování výjimek optimistického řízení souběžnosti v vrstvy DAL.
- Vytvořte webovou stránku office přiřazení.
- Otestujte optimistického řízení souběžnosti v nové webové stránky.

### <a name="adding-officeassignment-stored-procedures-to-the-data-model"></a>Přidání OfficeAssignment uložené procedury do datového modelu

Otevřít *SchoolModel.edmx* soubor v Návrháři modelů klikněte pravým tlačítkem na návrhové ploše a klikněte na tlačítko **aktualizace modelů z databáze**. V **přidat** karty **zvolte vaše databázové objekty** dialogového okna rozbalte **uložené procedury** a vyberte tři `OfficeAssignment` uložených procedur komponentami TableAdapter (naleznete v tématu Následující snímek obrazovky) a potom klikněte na tlačítko **Dokončit**. (Tyto uložené procedury bylo již v databázi při jste stáhli nebo vytvořili pomocí skriptu.)

[![Image02](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image26.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image25.png)

Klikněte pravým tlačítkem myši `OfficeAssignment` entity a vyberte **mapování uložené procedury**.

[![Image03](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image28.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image27.png)

Nastavte **vložit**, **aktualizace**, a **odstranit** funkce použít odpovídající uložené procedury. Pro `OrigTimestamp` parametr `Update` fungovat, nastavte **vlastnost** k `Timestamp` a vyberte **původní hodnotu použití** možnost.

[![Image04](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image30.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image29.png)

Když volá rozhraní Entity Framework `UpdateOfficeAssignment` uložené procedury, předá se původní hodnoty `Timestamp` sloupec v `OrigTimestamp` parametr. Uložená procedura použije tento parametr v jeho `Where` klauzule:

[!code-sql[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample8.sql)]

Uložená procedura také vybere novou hodnotu `Timestamp` sloupec po aktualizaci tak, aby rozhraní Entity Framework můžete ponechat `OfficeAssignment` entity, která je v paměti synchronizované s odpovídající řádek.

(Všimněte si, že nemá uložené procedury pro odstranění přiřazení kanceláře `OrigTimestamp` parametru. Z tohoto důvodu Entity Framework nelze ověřit, že je před jejím odstraněním beze změny entity.)

Uložte a zavřete datový model.

### <a name="adding-officeassignment-methods-to-the-dal"></a>Přidání metody OfficeAssignment vrstvy Dal

Otevřít *ISchoolRepository.cs* a přidejte následující metody CRUD pro `OfficeAssignment` sady entit:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample9.cs)]

Přidejte následující nové metody, které *SchoolRepository.cs*. V `UpdateOfficeAssignment` metodu zavoláte místní `SaveChanges` metoda místo `context.SaveChanges`.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample10.cs)]

V projektu testů otevřete *MockSchoolRepository.cs* a přidejte následující `OfficeAssignment` kolekce a CRUD metody k němu. (Mock úložiště musí implementovat rozhraní úložiště nebo řešení nebude kompilovat.)

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample11.cs)]

### <a name="adding-officeassignment-methods-to-the-bll"></a>Přidání metody OfficeAssignment BLL

V hlavním projektu otevřete *SchoolBL.cs* a přidejte následující metody CRUD pro `OfficeAssignment` sada entit k ní:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample12.cs)]

## <a name="creating-an-officeassignments-web-page"></a>Vytvoření webové stránky OfficeAssignments

Vytvořit novou webovou stránku, která používá *Site.Master* stránku předlohy a pojmenujte ho *OfficeAssignments.aspx*. Přidejte následující kód k `Content` ovládací prvek s názvem `Content2`:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample13.aspx)]

Všimněte si, že v `DataKeyNames` atribut, určuje kód `Timestamp` vlastnosti, jakož i klíč záznamu (`InstructorID`). Zadání vlastnosti `DataKeyNames` atribut způsobí, že ovládací prvek k jejich uložení v stav ovládacího prvku (které je podobné zobrazení stavu) tak, že původní hodnoty jsou k dispozici během zpracování zpětného volání.

Pokud jste neprovedli `Timestamp` hodnotu, Entity Framework nemusí ho `Where` klauzule SQL `Update` příkazu. V důsledku nic bude nalezen aktualizovat. V důsledku toho by Entity Framework vyvolat výjimku optimistického řízení souběžnosti pokaždé, když `OfficeAssignment` se aktualizuje entitu.

Otevřít *OfficeAssignments.aspx.cs* a přidejte následující `using` příkaz pro vrstvy přístupu k datům:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample14.cs)]

Přidejte následující `Page_Init` metodu, která povoluje funkce Dynamická Data. Také přidejte následující obslužnou rutinu pro `ObjectDataSource` ovládacího prvku `Updated` událostí za účelem ověření chyby souběžnosti:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample15.cs)]

### <a name="testing-optimistic-concurrency-in-the-officeassignments-page"></a>Na stránce OfficeAssignments testování optimistického řízení souběžnosti

Spustit *OfficeAssignments.aspx* stránky.

[![Image10](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image32.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image31.png)

Klikněte na tlačítko **upravit** v řádku a změňte hodnotu v **umístění** sloupce.

[![Image11](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image34.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image33.png)

Otevřete nové okno prohlížeče a spusťte znovu stránku (zkopírovat adresu URL z první okno prohlížeče na druhé okno prohlížeče).

[![Image10](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image36.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image35.png)

Klikněte na tlačítko **upravit** ve stejném řádku jste upravili v předchozích krocích a změnit **umístění** hodnotu na něco jiného.

[![Image12](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image38.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image37.png)

V druhém okně prohlížeče, klikněte na tlačítko **aktualizace**.

[![Image13](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image40.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image39.png)

Přepněte na první okno prohlížeče a klikněte na tlačítko **aktualizace**.

[![Image15](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image42.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image41.png)

Zobrazí chybová zpráva a **umístění** hodnota se aktualizovala na Zobrazit hodnotu ho na změně v druhém okně prohlížeče.

## <a name="handling-concurrency-with-the-entitydatasource-control"></a>Ošetření souběžnosti s ovládacím prvkem EntityDataSource

`EntityDataSource` Ovládací prvek obsahuje integrovanou logikou, která rozpozná nastavení souběžnosti v datovém modelu a popisovače aktualizační a odstraňovací operace odpovídajícím způsobem. Nicméně, stejně jako u všech výjimek, musí zpracovat `OptimisticConcurrencyException` výjimky sami negace popisnou chybovou zprávu.

Dále je nutné nakonfigurovat *Courses.aspx* stránky (který používá `EntityDataSource` ovládací prvek) Chcete-li povolit aktualizaci a odstraňování a se zobrazí chybová zpráva, pokud dojde ke konfliktu souběžnosti. `Course` Entita nemá souběžnosti sledování sloupců, což vám umožní používat stejné metody, které jste prováděli pomocí `Department` entity: sledování hodnoty všech vlastností neklíčovým.

Otevřít *SchoolModel.edmx* souboru. Pro vlastnosti neklíčovým `Course` entity (`Title`, `Credits`, a `DepartmentID`), můžete nastavit **souběžný režim** vlastnost `Fixed`. Potom uložte a zavřete datový model.

Otevřít *Courses.aspx* stránce a proveďte následující změny:

- V `CoursesEntityDataSource` řídit, přidejte `EnableUpdate="true"` a `EnableDelete="true"` atributy. Počáteční značka pro tento ovládací prvek teď vypadá podobně jako v následujícím příkladu:

    [!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample16.aspx)]
- V `CoursesGridView` řídit, změnit `DataKeyNames` atribut hodnotu `"CourseID,Title,Credits,DepartmentID"`. Pak přidejte `CommandField` element `Columns` element, který ukazuje **upravit** a **odstranit** tlačítka (`<asp:CommandField ShowEditButton="True" ShowDeleteButton="True" />`). `GridView` Ovládací prvek teď vypadá podobně jako v následujícím příkladu:

    [!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample17.aspx)]

Spuštění stránky a vytvořte konflikt situaci jako předtím na stránce oddělení. Spuštění stránky v dvě okna prohlížeče, klikněte na tlačítko **upravit** ve stejném řádku v každé okno a různých změnu provést v každém z nich. Klikněte na tlačítko **aktualizace** v jednom okně a pak klikněte na tlačítko **aktualizace** v druhém okně. Po kliknutí na **aktualizace** podruhé, zobrazit chybovou stránku, která je výsledkem výjimka neošetřená souběžnosti.

[![Image22](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image44.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image43.png)

Zpracovat tuto chybu způsobem, který je velmi podobný způsob pro zpracování `ObjectDataSource` ovládacího prvku. Otevřít *Courses.aspx* stránky a `CoursesEntityDataSource` ovládacího prvku, určují obslužné rutiny pro `Deleted` a `Updated` události. Počáteční značka ovládacího prvku teď vypadá podobně jako v následujícím příkladu:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample18.aspx)]

Před `CoursesGridView` řídit, přidejte následující `ValidationSummary` ovládacího prvku:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample19.aspx)]

V *Courses.aspx.cs*, přidejte `using` příkaz pro `System.Data` obor názvů, přidejte metodu, která kontroluje výjimky souběžnosti a přidejte obslužné rutiny pro `EntityDataSource` ovládacího prvku `Updated` a `Deleted`obslužné rutiny. Kód bude vypadat nějak takto:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample20.cs)]

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample21.cs)]

Jediným rozdílem mezi tímto kódem a jaké kroky jste provedli `ObjectDataSource` ovládací prvek je, že v tomto případě výjimky souběžnosti v `Exception` vlastnost objektu argumenty události, nikoli v tuto výjimku `InnerException` vlastnost.

Spuštění stránky a znovu vytvořit ke konfliktu souběžnosti. Tentokrát se zobrazí chybová zpráva:

[![image23](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image46.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image45.png)

Dokončení tohoto postupu Úvod ke zpracování konfliktů souběžnosti. V dalším kurzu najdete pokyny k vylepšení výkonu ve webové aplikaci, která používá Entity Framework.

> [!div class="step-by-step"]
> [Předchozí](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)
> [další](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)
