---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application
title: Zpracování souběžnosti s rozhraní Entity Framework 4.0 4 webové aplikace ASP.NET | Microsoft Docs
author: tdykstra
description: Tento kurz řady staví na webové aplikace Contoso univerzity, který byl vytvořený Začínáme s řadou kurz Entity Framework 4.0. I...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: a5aa22a6-fb7f-4f41-9c7f-addda151940b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application
msc.type: authoredcontent
ms.openlocfilehash: f40695270006e4f8b0c9ad8e94049e5239f06e63
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30889859"
---
<a name="handling-concurrency-with-the-entity-framework-40-in-an-aspnet-4-web-application"></a>Zpracování souběžnosti s rozhraní Entity Framework 4.0 4 webové aplikace technologie ASP.NET
====================
Podle [tní Dykstra](https://github.com/tdykstra)

> Tento kurz řady staví na webové aplikace Contoso univerzity, který byl vytvořený [Začínáme s Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) kurz řady. Pokud nebyla dokončena starší kurzy, jako výchozí bod pro účely tohoto kurzu můžete [stažení aplikace](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) , kterou by jste vytvořili. Můžete také [stažení aplikace](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) vytvořené dokončení kurzu řady. Pokud máte dotazy týkající se kurzy, můžete je do příspěvku [ASP.NET Entity Framework fórum](https://forums.asp.net/1227.aspx).


V předchozích kurzu jste se dozvěděli, jak řadit a filtrování dat pomocí `ObjectDataSource` řízení a Entity Framework. Tento kurz ukazuje možnosti pro zpracování souběžnosti ve webové aplikaci ASP.NET, který používá rozhraní Entity Framework. Vytvoří novou webovou stránku, který je vyhrazen pro aktualizaci lektorem office přiřazení. Budete zpracovávat potíže se souběžností v této stránce a na stránce oddělení, kterou jste vytvořili dříve.

[![Image06](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image2.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image1.png)

[![Image01](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image4.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image3.png)

## <a name="concurrency-conflicts"></a>Konflikty souběžnosti

Concurrency dojde ke konfliktu jeden uživatel upravuje záznam a jiný uživatel upravuje stejný záznam před první uživatel změnu je zapsána do databáze. Pokud nemáte nastaven rozhraní Entity Framework, na zjištění takové konflikty, kdo aktualizuje databázi přepíše poslední změny jiného uživatele. V mnoha aplikacích toto riziko je přijatelné a nemusíte konfigurace aplikace pro zpracování konfliktů možné souběžnosti. (Pokud existuje několik uživatelů nebo několik aktualizací, nebo pokud není opravdu důležité, pokud jsou některé změny přepsány, náklady na programování pro concurrency vyváží výhodou.) Pokud potřebujete nemusíte si dělat starosti souběžnosti je v konfliktu, můžete přeskočit tento kurz; zbývající dvě kurzy v této sérii nemáte závisí na nic, co vytvoříte tohoto objektu.

### <a name="pessimistic-concurrency-locking"></a>Pesimistické souběžnosti (uzamčení)

Pokud aplikace třeba zabránit, aby před náhodnou ztrátou dat v concurrency scénáře, je jeden ze způsobů použití uzamčení databáze. To se označuje jako *pesimistické souběžnosti*. Například předtím, než se pustíte do čtení řádek z databáze, můžete požádat o zámku pro jen pro čtení nebo pro přístup k aktualizaci. Pokud zamknete řádek pro přístup k aktualizaci, je povoleno uzamčení řádek buď pro žádní jiní uživatelé jen pro čtení nebo aktualizace přístup, protože by získají kopii dat, která je právě probíhá změnit. Pokud zamknete řádek pro oprávnění jen pro čtení, ostatní také možné zamknout ho pro přístup jen pro čtení, ale není pro aktualizaci.

Správa zámky má některé nevýhody. Může být složité programu. Vyžaduje významné databáze správy prostředků, a jako počet uživatelů, aplikace, může to způsobit problémy s výkonem zvyšuje (to znamená, že se nemá uspokojivé škálování). Z těchto důvodů ne všechny databázové systémy podporovat pesimistické souběžnosti. Rozhraní Entity Framework poskytuje žádné integrovanou podporu pro něj, a v tomto kurzu není ukazují, jak ho implementovat.

### <a name="optimistic-concurrency"></a>Optimistickou metodu souběžného zpracování

Je alternativa k pesimistické souběžnosti *optimistickou metodu souběžného*. Optimistickou metodu souběžného znamená povolení konfliktů souběžnosti provést a reaktivní správně, pokud tomu tak je. Například Jan spustí *Department.aspx* stránce, kliknutí **upravit** odkaz pro oddělení historie a snižuje **nároky** množství od 1,000,000.00 $ $ 125,000.00. (Jan spravuje konkurenční oddělení a chce uvolněte peníze pro své vlastní oddělení.)

[![Image07](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image6.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image5.png)

Předtím, než Jan klikne **aktualizace**, Jana běží stejné stránce, klikne **upravit** odkaz historie oddělení, a potom klepněte na změny **počáteční datum** pole z 1/10/2011 1/1 / 1999. (Jana spravuje oddělení historie a chce umožnit další služební věk.)

[![Image08](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image8.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image7.png)

Jan klikne **aktualizace** nejprve, pak klikne Jana **aktualizace**. Jana prohlížeč teď seznamy **nároky** jako $1,000,000.00, ale tato velikost je nesprávný, protože velikost změnil Jan na 125,000.00 $.

Některé akce, které můžete provést v tomto scénáři patří:

- Můžete udržovat přehled o vlastností, které uživatel změnil a aktualizovat na odpovídající sloupce v databázi. V ukázkovém scénáři by byl žádná data ztratí, protože různé vlastnosti aktualizoval dva uživatele. Při příštím někdo umožňuje oddělení historie, zobrazí se 1/1/1999 a 125,000.00 $. 

    Toto je výchozí chování v rozhraní Entity Framework a může podstatně snížit počet konfliktům, ke kterým může dojít ke ztrátě dat. Toto chování však není vyhnuli ztrátě dat, pokud konkurenční změn stejnou vlastnost entity. Kromě toho není toto chování vždy možné; Pokud namapujete uložené procedury na typ entity, všechny vlastnosti entity jsou aktualizovány při provedení změny v entitě v databázi.
- Můžete je nechat Jana na změnu Jan pro změna přepsána. Po klikne Jana **aktualizace**, **nároky** velikost přejde zpět na 1,000,000.00 $. Tento postup se nazývá *klienta Wins* nebo *poslední ve službě Wins* scénář. (Hodnoty klienta přednost co je v úložišti.)
- Jana na změnu může zabránit aktualizaci v databázi. By obvykle, zobrazí se chybová zpráva, mu zobrazit aktuální stav dat a mu Pokud chce je provést znovu zadejte své změny. Může další automatizovat proces pomocí ukládání svůj vstup a jí poskytnete příležitost znovu bez nutnosti znovu je zadejte. Tento postup se nazývá *Wins úložiště* scénář. (Hodnoty úložiště dat mají přednost před odeslané klientem hodnoty.)

### <a name="detecting-concurrency-conflicts"></a>Zjišťování konfliktů souběžnosti

V rozhraní Entity Framework, můžete vyřešit konflikty zpracování `OptimisticConcurrencyException` výjimky, které vyvolá rozhraní Entity Framework. Chcete-li vědět, kdy má být vyvolána tyto výjimky, musí být schopna zjistit konflikty rozhraní Entity Framework. Proto musíte nakonfigurovat databázi a datový model správně. Některé možnosti aktivace zjišťování konfliktů, patří:

- V databázi zahrnují sloupci tabulky, který slouží k určení, kdy se změnil na řádek. Potom můžete nakonfigurovat rozhraní Entity Framework zahrnovat tento sloupec v `Where` klauzuli SQL `Update` nebo `Delete` příkazy.

    Je účelem `Timestamp` sloupec v `OfficeAssignment` tabulky.

    [![Image09](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image10.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image9.png)

    Datový typ `Timestamp` sloupec se také nazývá `Timestamp`. Sloupec, ale ve skutečnosti neobsahuje hodnotu datum nebo čas. Místo toho hodnota je pořadové číslo, které se zvýší pokaždé, když se aktualizuje na řádek. V `Update` nebo `Delete` příkaz, `Where` klauzule obsahuje původní `Timestamp` hodnota. Pokud se změnila řádek aktualizován jiným uživatelem, hodnota v `Timestamp` je jiná než původní hodnota, proto `Where` klauzule vrací žádný řádek, který chcete aktualizovat. Když rozhraní Entity Framework najde, aby byly aktualizovány žádné řádky aktuální `Update` nebo `Delete` příkazů (to znamená, když počet ovlivněných řádků je nulová), který ji interpretuje jako konflikt souběžnosti.
- Nakonfigurujte rozhraní Entity Framework zahrnují původní hodnoty každý sloupec v tabulce v `Where` klauzuli `Update` a `Delete` příkazy.

    Jako první možnost, pokud nic v řádku od řádek byl nejdřív přečíst, změnila `Where` klauzule nevrátí řádek aktualizace, které rozhraní Entity Framework interpretuje jako konflikt souběžnosti. Tato metoda je co nejúčinnější pomocí `Timestamp` pole, ale může být neefektivní. Pro databázové tabulky, které mají mnoho sloupců a může vést k velmi velké `Where` klauzule, a ve webové aplikaci může vyžadovat udržovat velkých objemů stavu. Správa velkých objemů stavu může ovlivnit výkon aplikace, protože buď vyžaduje prostředky serveru (například stav relace) nebo musí být součástí webové stránky (například stav zobrazení).

V tomto kurzu budete přidávat zpracování chyb pro optimistickou metodu souběžného konflikty pro entity, která nemá vlastnost sledování ( `Department` entity) a pro entitu, která nemá vlastnost sledování ( `OfficeAssignment` entity).

## <a name="handling-optimistic-concurrency-without-a-tracking-property"></a>Optimistickou metodu souběžného zpracování bez vlastnosti sledování

K implementaci optimistickou metodu souběžného pro `Department` entity, která nemá pro sledování (`Timestamp`) vlastnost, bude dokončit následující úlohy:

- Změnit datový model, který povolit sledování souběžnosti `Department` entity.
- V `SchoolRepository` třídy výjimky souběžnosti popisovač v `SaveChanges` metoda.
- V *Departments.aspx* stránky, obslužná rutina výjimky souběžnosti zobrazením uživateli upozornění, že pokus o změny se nezdařily. Uživatele můžete zobrazit aktuální hodnoty a opakujte změny, pokud jsou stále potřeba.

### <a name="enabling-concurrency-tracking-in-the-data-model"></a>Povolení sledování v datovém modelu souběžnosti

V sadě Visual Studio otevřete webovou aplikaci Contoso univerzity, kterou jste pracovali v předchozí kurzu této série.

Otevřete *SchoolModel.edmx*a v Návrháři modelu dat, klikněte pravým tlačítkem `Name` vlastnost `Department` entitu a pak klikněte na tlačítko **vlastnosti**. V **vlastnosti** změňte `ConcurrencyMode` vlastnost `Fixed`.

[![Image16](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image12.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image11.png)

Totéž pro ostatní primární klíč Skalární vlastnosti (`Budget`, `StartDate`, a `Administrator`.) (Nesmí to provést pro navigační vlastnosti.) Určuje, že vždy, když rozhraní Entity Framework generuje `Update` nebo `Delete` příkaz SQL pro aktualizaci `Department` entity v databázi, tyto sloupce (s původní hodnoty) musí být součástí `Where` klauzule. Pokud se nenajde žádný řádek, kdy `Update` nebo `Delete` příkaz provede, vyvolá výjimku optimistickou metodu souběžného rozhraní Entity Framework.

Uložte a zavřete datový model.

### <a name="handling-concurrency-exceptions-in-the-dal"></a>Zpracování výjimky souběžnosti v DAL

Otevřete *SchoolRepository.cs* a přidejte následující `using` údajů `System.Data` obor názvů:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample1.cs)]

Přidejte následující nové `SaveChanges` metoda, která zpracovává optimistickou metodu souběžného:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample2.cs)]

Pokud concurrency chyba nastane, když tato metoda je volána, hodnoty vlastností entity v paměti se nahradí hodnoty aktuálně v databázi. Výjimky souběžnosti je znovu vyvolány, aby ji může zpracovat webové stránky.

V `DeleteDepartment` a `UpdateDepartment` metody, nahraďte existující volání `context.SaveChanges()` pomocí volání `SaveChanges()` k vyvolání nové metody.

### <a name="handling-concurrency-exceptions-in-the-presentation-layer"></a>Zpracování výjimky souběžnosti v prezentační vrstvě

Otevřete *Departments.aspx* a přidejte `OnDeleted="DepartmentsObjectDataSource_Deleted"` atribut `DepartmentsObjectDataSource` ovládacího prvku. Počáteční značka pro ovládací prvek bude nyní vypadat podobně jako v následujícím příkladu.

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample3.aspx)]

V `DepartmentsGridView` řídit, zadejte všechny sloupce tabulky v `DataKeyNames` atributu, jak je znázorněno v následujícím příkladu. Všimněte si, že tím se vytvoří velký zobrazení pole stavu, který je jedním z důvodů proč pomocí sledování pole je obecně upřednostňovaný způsob sledování je v konfliktu souběžnosti.

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample4.aspx)]

Otevřete *Departments.aspx.cs* a přidejte následující `using` údajů `System.Data` obor názvů:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample5.cs)]

Přidejte následující nové metodu, která bude volat z ovládacího prvku zdroje dat `Updated` a `Deleted` obslužných rutin událostí pro zpracování výjimky souběžnosti:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample6.cs)]

Tento kód ověří typ výjimky, a pokud je výjimky souběžnosti, kód dynamicky vytvoří `CustomValidator` ovládací prvek, který pak zobrazí zprávu v `ValidationSummary` ovládacího prvku.

Volejte metodu nové z `Updated` obslužné rutiny události, které jste přidali dříve. Kromě toho vytvořte nový `Deleted` obslužné rutiny události, která volá metodu stejné (ale není dělat žádné další kroky):

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample7.cs)]

### <a name="testing-optimistic-concurrency-in-the-departments-page"></a>Na stránce oddělení testování optimistickou metodu souběžného zpracování

Spustit *Departments.aspx* stránky.

[![Image17](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image14.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image13.png)

Klikněte na tlačítko **upravit** v řádku a změňte hodnotu v **nároky** sloupce. (Nezapomeňte, že můžete upravit pouze záznamy, které jste vytvořili v tomto kurzu, protože existující `School` záznamů databáze obsahovat některé neplatná data. Záznam pro oddělení ekonomické ukazatele je bezpečné jeden a experimentovat s).

[![Image18](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image16.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image15.png)

Otevřete nové okno prohlížeče a spusťte znovu stránka (kopie adresu URL z pole adresy první okno prohlížeče na druhý okno prohlížeče).

[![Image17](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image18.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image17.png)

Klikněte na tlačítko **upravit** ve stejné můžete upravit dříve řádek a změňte **nároky** hodnotu na něco jiného.

[![Image19](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image20.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image19.png)

V okně prohlížeče druhý klikněte na tlačítko **aktualizace**. **Nároky** tato nová hodnota se úspěšně změnila velikost.

[![Image20](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image22.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image21.png)

V prvním okně prohlížeče, klikněte na tlačítko **aktualizace**. Aktualizace se nezdaří. **Nároky** velikost se zobrazí znovu, používá hodnotu nastavenou v druhé okno prohlížeče a zobrazí chybovou zprávu.

[![Image21](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image24.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image23.png)

## <a name="handling-optimistic-concurrency-using-a-tracking-property"></a>Pomocí vlastnosti sledování optimistickou metodu souběžného zpracování

Pro zpracování optimistickou metodu souběžného pro entitu, která má vlastnost sledování, dokončí následující úkoly:

- Přidat uložené procedury do datového modelu spravovat `OfficeAssignment` entity. (Vlastnosti sledování a uložených procedur není nutné použít společně, jsou právě seskupeny dohromady sem pro ilustraci.)
- Přidejte metody CRUD DAL a BLL pro `OfficeAssignment` entity, včetně kódu zpracování výjimek optimistickou metodu souběžného v DAL.
- Vytvořte webovou stránku office přiřazení.
- Test optimistickou metodu souběžného nové webové stránce.

### <a name="adding-officeassignment-stored-procedures-to-the-data-model"></a>Přidání OfficeAssignment uložené procedury do datového modelu

Otevřete *SchoolModel.edmx* souboru v Návrháři model, klikněte pravým tlačítkem na návrhovou plochu a klikněte na tlačítko **aktualizovat Model z databáze**. V **přidat** kartě **zvolte si databázové objekty** dialogové okno, rozbalte seznam **uložené procedury** a vyberte tří `OfficeAssignment` uložené procedury (najdete v části Následující snímek obrazovky) a potom klikněte na **Dokončit**. (Tyto uložené procedury již v databázi v době nebyly jste stáhli nebo vytvořili pomocí skriptu.)

[![Image02](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image26.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image25.png)

Klikněte pravým tlačítkem myši `OfficeAssignment` entity a vyberte **uložené procedury mapování**.

[![Image03](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image28.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image27.png)

Nastavte **vložit**, **aktualizace**, a **odstranit** funkce na jejich odpovídající pomocí uložené procedury. Pro `OrigTimestamp` parametr `Update` fungovat, nastavte **vlastnost** k `Timestamp` a vyberte **použijte původní hodnotu** možnost.

[![Image04](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image30.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image29.png)

Při volání rozhraní Entity Framework `UpdateOfficeAssignment` uložené procedury, předá se původní hodnotu `Timestamp` sloupec v `OrigTimestamp` parametr. Uložená procedura používá tento parametr v jeho `Where` klauzule:

[!code-sql[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample8.sql)]

Uložená procedura také vybere novou hodnotu `Timestamp` sloupec po aktualizaci tak, aby rozhraní Entity Framework můžete ponechat `OfficeAssignment` entita, která je v paměti synchronizována s odpovídající řádek databáze.

(Všimněte si, že nemá uložené procedury pro odstranění office přiřazení `OrigTimestamp` parametr. Z toho důvodu rozhraní Entity Framework nemůže ověřit, že entita nezměnil se před odstraněním.)

Uložte a zavřete datový model.

### <a name="adding-officeassignment-methods-to-the-dal"></a>Přidání metody OfficeAssignment vrstvy Dal

Otevřete *ISchoolRepository.cs* a přidejte následující metody CRUD pro `OfficeAssignment` sady entit:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample9.cs)]

Přidejte následující nové metody pro *SchoolRepository.cs*. V `UpdateOfficeAssignment` metody volání místní `SaveChanges` metoda místo `context.SaveChanges`.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample10.cs)]

V k testovacímu projektu, otevřete *MockSchoolRepository.cs* a přidejte následující `OfficeAssignment` kolekce a CRUD metody k němu. (Imitované úložiště musí implementovat rozhraní úložiště, nebo nebude kompilace řešení.)

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample11.cs)]

### <a name="adding-officeassignment-methods-to-the-bll"></a>Přidání metody OfficeAssignment BLL

V hlavní projekt, otevřete *SchoolBL.cs* a přidejte následující metody CRUD pro `OfficeAssignment` sada entit k němu:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample12.cs)]

## <a name="creating-an-officeassignments-web-page"></a>Vytvoření webové stránky OfficeAssignments

Vytvořit novou webovou stránku, který používá *Site.Master* hlavní stránky a pojmenujte ji *OfficeAssignments.aspx*. Přidejte následující kód do `Content` ovládací prvek s názvem `Content2`:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample13.aspx)]

Všimněte si, že v `DataKeyNames` atribut, určuje kód `Timestamp` vlastnost, jakož i klíč záznamu (`InstructorID`). Zadání vlastnosti v `DataKeyNames` atribut způsobuje ovládacího prvku o jejich uložení do stavu ovládacích prvků (což je podobná zobrazení stavu), aby původní hodnoty jsou k dispozici při zpracování zpětného volání.

Pokud jste neprovedli `Timestamp` hodnota, rozhraní Entity Framework nebude mít ho `Where` klauzuli SQL `Update` příkaz. Nic z tohoto důvodu by nalézt pro aktualizaci. V důsledku toho by rozhraní Entity Framework vyvolat výjimku optimistickou metodu souběžného pokaždé, když `OfficeAssignment` aktualizaci entity.

Otevřete *OfficeAssignments.aspx.cs* a přidejte následující `using` příkaz pro vrstvu přístupu k datům:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample14.cs)]

Přidejte následující `Page_Init` metoda, která umožňuje funkce Dynamická Data. Také přidejte následující obslužnou rutinu pro `ObjectDataSource` ovládacího prvku `Updated` události chcete-li zkontrolovat souběžnosti chyby:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample15.cs)]

### <a name="testing-optimistic-concurrency-in-the-officeassignments-page"></a>Na stránce OfficeAssignments testování optimistickou metodu souběžného zpracování

Spustit *OfficeAssignments.aspx* stránky.

[![Image10](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image32.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image31.png)

Klikněte na tlačítko **upravit** v řádku a změňte hodnotu v **umístění** sloupce.

[![Image11](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image34.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image33.png)

Otevřete nové okno prohlížeče a spusťte znovu stránka (kopie adresu URL z první okno prohlížeče na druhý okno prohlížeče).

[![Image10](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image36.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image35.png)

Klikněte na tlačítko **upravit** ve stejné můžete upravit dříve řádek a změňte **umístění** hodnotu na něco jiného.

[![Image12](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image38.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image37.png)

V okně prohlížeče druhý klikněte na tlačítko **aktualizace**.

[![Image13](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image40.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image39.png)

Přepnout na první okno prohlížeče a klikněte na tlačítko **aktualizace**.

[![Image15](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image42.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image41.png)

Zobrazí chybovou zprávu a **umístění** hodnota je aktualizovaná tak, aby zobrazit hodnotu změnit jeho ve druhém okně prohlížeče.

## <a name="handling-concurrency-with-the-entitydatasource-control"></a>Zpracování souběžnosti pomocí ovládacího prvku EntityDataSource

`EntityDataSource` Ovládací prvek obsahuje vestavěnou logiku, který rozpoznává nastavení souběžnosti v datovém modelu a obslužné rutiny aktualizace a operace odstranění odpovídajícím způsobem. Ale stejně jako u všech výjimek, je nutné zpracovat `OptimisticConcurrencyException` výjimky sami, chcete-li poskytovat uživatelsky přívětivý chybová zpráva.

Dále je nutné nakonfigurovat *Courses.aspx* stránky (které používá `EntityDataSource` ovládací prvek) povolit aktualizaci a operace odstranění a zobrazí se chybová zpráva, pokud dojde ke konfliktu souběžnosti. `Course` Entity nemá souběžných sledování sloupce, takže budete používat stejným způsobem, jaký jste to udělali s `Department` entity: sledování hodnoty všech vlastností jiný klíč.

Otevřete *SchoolModel.edmx* souboru. Pro vlastnosti neklíčový `Course` entity (`Title`, `Credits`, a `DepartmentID`), můžete nastavit **souběžný režim** vlastnost `Fixed`. Potom uložte a zavřete datový model.

Otevřete *Courses.aspx* stránky a proveďte následující změny:

- V `CoursesEntityDataSource` řídit, přidejte `EnableUpdate="true"` a `EnableDelete="true"` atributy. Počáteční značka pro ovládací prvek nyní podobá následujícímu příkladu:

    [!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample16.aspx)]
- V `CoursesGridView` řídit, změňte `DataKeyNames` atribut hodnotu `"CourseID,Title,Credits,DepartmentID"`. Pak přidejte `CommandField` elementu, který chcete `Columns` element, který ukazuje **upravit** a **odstranit** tlačítka (`<asp:CommandField ShowEditButton="True" ShowDeleteButton="True" />`). `GridView` Řízení nyní podobá následujícímu příkladu:

    [!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample17.aspx)]

Spuštění stránky a vytvořit situaci konflikt, který jste použili před na stránce oddělení. Spuštění stránky v dvě okna prohlížeče, klikněte na tlačítko **upravit** ve stejném řádku v každé okno a proveďte další změny v každé z nich. Klikněte na tlačítko **aktualizace** v jednom okně a pak klikněte na tlačítko **aktualizace** v dalším okně. Když kliknete na tlačítko **aktualizace** podruhé, zobrazí se chybová stránka, která je výsledkem výjimky neošetřené souběžnosti.

[![Image22](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image44.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image43.png)

Zpracování této chyby způsobem, který se velmi podobná způsob pro zpracování `ObjectDataSource` ovládacího prvku. Otevřete *Courses.aspx* stránky a v `CoursesEntityDataSource` řízení, zadejte obslužné rutiny pro `Deleted` a `Updated` události. Počáteční značka ovládacího prvku nyní podobá následujícímu příkladu:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample18.aspx)]

Před `CoursesGridView` řídit, přidejte následující `ValidationSummary` ovládacího prvku:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample19.aspx)]

V *Courses.aspx.cs*, přidejte `using` příkaz pro `System.Data` obor názvů, přidejte metodu, která kontroluje pro výjimky souběžnosti a přidejte obslužné rutiny pro `EntityDataSource` ovládacího prvku `Updated` a `Deleted`obslužné rutiny. Kód bude vypadat takto:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample20.cs)]

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample21.cs)]

Jediným rozdílem mezi tento kód a nebyla `ObjectDataSource` ovládací prvek je, že v tomto případě výjimky souběžnosti není v `Exception` vlastnost objektu argumenty události, a nikoli v této výjimky `InnerException` vlastnost.

Spuštění stránky a znovu vytvořte konflikt souběžnosti. Tentokrát uvidíte chybovou zprávu:

[![Image23](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image46.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image45.png)

Tím dokončíte Úvod pro zpracování konfliktů souběžnosti. V dalším kurzu se poskytují pokyny o tom, jak zlepšit výkon v webové aplikace, která používá rozhraní Entity Framework.

> [!div class="step-by-step"]
> [Předchozí](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)
> [další](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)
